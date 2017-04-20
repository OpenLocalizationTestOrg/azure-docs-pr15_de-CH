<properties 
    pageTitle="Service Bus verbunden Namespaces | Microsoft Azure"
    description="Implementierungsdetails gepaarten Namespace und Kosten"
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Implementierungsdetails Namespace kombiniert und Kosten auswirkt

Die [PairNamespaceAsync][] -Methode mit einer [SendAvailabilityPairedNamespaceOptions][] -Instanz führt sichtbare Aufgaben in Ihrem Auftrag. Da es beim Verwenden des Features Kostenfaktoren sind, ist es sinnvoll, Vorgänge zu verstehen, das Verhalten erwarten, wenn es passiert. Die API greift das folgende automatische Verhalten in Ihrem Auftrag:

-   Erstellung von Backlog-Warteschlangen.
-   Ein [MessageSender][] -Objekt, das mit Warteschlangen oder Themen erstellen.
-   Beim messaging Entität nicht verfügbar ist, ping sendet Nachrichten an die Einheit versucht zu erkennen, wenn die Entität wieder verfügbar ist.
-   Optional eine Reihe von "Nachricht Pumpen" erstellt, die Nachrichten aus den Warteschlangen Rückstand primären Warteschlangen verschieben.
-   Schließen/fehlerhafte Instanzen [Fall][] primäre und sekundäre koordiniert.

Auf einer hohen Ebene das funktioniert wie folgt: Wenn die primäre Entität fehlerfrei ist, keine Verhalten Änderungen. Wenn die [FailoverInterval][] Dauer abläuft und der primären Entität sieht nicht erfolgreich sendet nicht flüchtigen [MessagingException][] oder eine [TimeoutException][]tritt folgendes Verhalten auf:

1.  Senden Sie Vorgänge mit der primären Entität deaktiviert und das System fragt der primären Entität bis Ping erfolgreich zugestellt werden können.

2.  Eine zufällige Backlog-Warteschlange ausgewählt ist.

3.  [BrokeredMessage][] Objekte werden an der gewählten Backlogwarteschlange weitergeleitet.

1.  Ausfall ein Sendevorgangs ausgewählten Backlogwarteschlange die Warteschlange aus der Rotation abgerufen und eine neue Warteschlange ausgewählt. Alle Absender [Fall][] Instanz enthält des Fehlers.

Die folgenden Zahlen zeigen die Sequenz. Zunächst sendet der Absender Nachrichten.

![Paarweise Namespaces][0]

Bei einem Ausfall der primären Warteschlange senden beginnt der Absender senden von Nachrichten an eine zufällig Backlogwarteschlange. Gleichzeitig wird eine pingaufgabe gestartet.

![Paarweise Namespaces][1]

An dieser Stelle Nachrichten werden weiterhin in der sekundären Warteschlange und nicht an die primäre Warteschlange übermittelt. Nachdem die primäre Warteschlange wieder fehlerfrei ist, läuft mindestens einen Prozess den Siphon. Der Siphon übermittelt die Nachrichten an die richtigen Zielentitäten (Warteschlangen und Themen) aus den verschiedenen Backlog-Warteschlangen.

![Paarweise Namespaces][2]

Dieses Thema behandelt die Einzelheiten der Funktionsweise dieser Teile.

## <a name="creation-of-backlog-queues"></a>Erstellung der Backlog-Warteschlangen

[PairNamespaceAsync][] -Methode übergebene [SendAvailabilityPairedNamespaceOptions][] -Objekt gibt die Anzahl der Backlog-Warteschlangen zu verwenden. Jede Backlogwarteschlange entsteht mit den folgenden Eigenschaften explizit festlegen (alle Werte sind [QueueDescription][] als Standard festgelegt):

| Pfad                                   | [primäre Namespace] / X-Servicebus-Transfer / [index] wobei [Index] Wert [0, BacklogQueueCount) |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | int. MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| LockDuration                           | 1 minute                                                                                             |
| EnableDeadLetteringOnMessageExpiration | true                                                                                                 |
| EnableBatchedOperations                | true                                                                                                 |

Beispielsweise der erste Backlogwarteschlange erstellt, mit dem Namen Namespace **Contoso** `contoso/x-servicebus-transfer/0`.

Beim Erstellen der Warteschlangen überprüft der Code zuerst, ob eine Warteschlange vorhanden ist. Wenn die Warteschlange nicht vorhanden ist, wird die Warteschlange erstellt. Der Code nicht bereinigen "extra" Backlog-Warteschlangen. Wenn die Anwendung mit primären Namespace **Contoso** fünf Auftragsbestand Warteschlangen aber eine Backlog-Warteschlange mit dem Pfad `contoso/x-servicebus-transfer/7` vorhanden ist, diese zusätzliche Backlog-Warteschlange noch vorhanden, aber nicht verwendet. Explizit kann zusätzliche Backlog-Warteschlangen vorhanden sein, die nicht verwendet werden. Als Besitzer des Namespace sind Sie verantwortlich für die Bereinigung Rückstand nicht verwendete/unerwünschte Warteschlangen. Der Grund dafür ist, dass Service Bus nicht bekannt ist, welche Zwecke für alle Warteschlangen im Namespace vorhanden. Außerdem, wenn eine Warteschlange mit dem angegebenen Namen existiert aber angenommenen [QueueDescription][]nicht entsprechen, sind Ihre Gründe eigene für das Standardverhalten ändern. Keine Änderungen an den Backlog-Warteschlangen vom Code Garantien werden. Achten Sie darauf, dass Ihre Änderung ausgiebig testen.

## <a name="custom-messagesender"></a>Benutzerdefinierte MessageSender

Beim Senden von gehen alle Nachrichten über ein internes [MessageSender][] -Objekt, das normalerweise verhält, wenn alles funktioniert und leitet den Backlog-Warteschlangen Dinge "aufheben". Einen nicht vorübergehender Fehler eingeht, wird ein Zeitgeber. Nach einer [Zeitspanne][] aus der [FailoverInterval][] -Eigenschaft der keine erfolgreichen Nachrichten gesendet werden, ist das Failover tätig. Zu diesem Zeitpunkt geschieht Folgendes für jede Entität:

- Ping-Vorgang führt alle [PingPrimaryInterval][] überprüfen, ob die Entität verfügbar ist. Wenn dieser Vorgang erfolgreich ist, startet alle Clientcode, der die Entität unmittelbar verwendet senden Nachrichten an den primären Namespace.

- Zukünftige Anfragen an die [BrokeredMessage][] geändert werden an sich in der Backlogwarteschlange führt dieselbe Entität von anderen Absendern. Die Änderung entfernt einige Eigenschaften des [BrokeredMessage][] -Objekts und an anderer Stelle gespeichert. Die folgenden Eigenschaften werden gelöscht und einen neuen Alias ermöglicht Service Bus und das SDK einheitlich zu Nachrichten hinzugefügt:

| Alter Eigenschaftsname       | Neuen Eigenschaftennamen |
|-------------------------|-------------------|
| SessionId               | X-ms-sessionid    |
| TimeToLive              | X-ms-timetolive   |
| ScheduledEnqueueTimeUtc | X-ms-Pfad         |

Ursprüngliche Zielpfad ist auch als eine Eigenschaft mit dem Namen X-ms-Pfad in der Nachricht gespeichert. Dadurch kann Nachrichten für viele Entitäten in einer einzigen Backlogwarteschlange koexistieren. Die Eigenschaften werden durch den Siphon übersetzt.

Das benutzerdefinierte Objekt [MessageSender][] kann Probleme auftreten, wenn Nachrichten 256 KB-Limit Ansatz und Failover ist. Das benutzerdefinierte [MessageSender][] -Objekt speichert Nachrichten für alle Warteschlangen und Themen zusammen in den Backlog-Warteschlangen. Dieses Objekt wird Nachrichten von vielen Grundfarben in den Backlog-Warteschlangen. Behandeln Sie Lastenausgleich zwischen viele Clients, die nicht kennen wählt das SDK zufällig eine Backlogwarteschlange für jeden [QueueClient][] oder [TopicClient][] im Code erstellen.

## <a name="pings"></a>Pings

Eine ist eine leere [BrokeredMessage][] die [ContentType][] -Eigenschaft auf Application/vnd.ms-Servicebus-Ping und [TimeToLive][] Wert 1 Sekunde. Dieser Ping ist eine Besonderheit im Service Bus: sendet der Server nie einen Ping, wenn ein Aufrufer [BrokeredMessage][]anfordert. So müssen Sie nie erfahren Sie empfangen und diese Nachrichten zu ignorieren. Jede Entität (eindeutige Warteschlange oder Thema) pro [Fall][] pro Client wird ein Pingsignal gesendet werden, wenn sie nicht gelten. Standardmäßig geschieht dies einmal pro Minute. Ping-Nachrichten gelten als normale Service Bus Nachrichten und Kosten für Bandbreite und Nachrichten führen. Als Kunden erkennen, dass das System verfügbar ist, beenden die Nachrichten.

## <a name="the-syphon"></a>Der Siphon

Mindestens ein Programm in der Anwendung läuft aktiv den Siphon. Der Siphon führt eine lange Abfrage erhalten, die 15 Minuten dauert. Wenn alle Elemente verfügbar sind und 10 Backlog-Warteschlangen, ruft die Anwendung, die den Siphon hostet den Empfangsvorgang 40 Mal pro Stunde, 960 Mal pro Tag und 28800 Mal in 30 Tagen. Der Siphon aktiv Nachrichten aus der primären Warteschlange verschoben wird, kommt jede Nachricht folgenden Gebühren (standard für die Nachrichtengröße und Bandbreite Gebühr in allen Phasen):

1.  Senden Sie zum Rückstand.

2.  Erhalten von den Rückstand.

3.  Senden Sie an die primäre.

4.  Der primäre erhalten.

## <a name="closefault-behavior"></a>Schließen/Fehler Verhalten

Innerhalb einer Anwendung, die hostet Siphon einmal primäre oder sekundäre [Fall][] Fehler oder ohne geschlossen erkennt auch fehlerhafte/geschlossen und der Siphon wird dieser Zustand Siphon fungiert. Wenn andere [Fall][] innerhalb von 5 Sekunden nicht geschlossen ist, wird der Siphon geöffnete [Fall][]Fehler.

## <a name="next-steps"></a>Nächste Schritte

Eine ausführliche Beschreibung des Service Bus asynchrones messaging finden Sie unter [asynchrones messaging Muster und hohe Verfügbarkeit][] . 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [Fall]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [ContentType]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asynchrones messaging Muster und hohe Verfügbarkeit]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png
