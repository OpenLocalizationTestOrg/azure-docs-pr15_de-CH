<properties 
    pageTitle="Service Bus asynchrones messaging | Microsoft Azure"
    description="Beschreibung des Service Bus asynchrones messaging."
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

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchrones messaging Muster und hohe Verfügbarkeit

Asynchrones messaging kann an verschiedene Arten implementiert werden. Warteschlangen, Themen und Abonnements unterstützt Azure Service Bus Asynchronie über einen Speicher und einen vorwärts-Mechanismus. Im Normalbetrieb (synchrone) Nachrichten an Warteschlangen und Nachrichten aus Warteschlangen und Abonnements. Programme, die Sie schreiben, hängt diese Entitäten ständig verfügbar. Ändert der Zustand der Entität aufgrund einer Reihe von Fällen benötigen Sie eine Möglichkeit zu einer reduzierten Fähigkeit Entität, die die Bedürfnisse erfüllen können.

Programme verwenden typischerweise asynchrone messaging Mustern Kommunikation Szenarien aktivieren. Sie können Clientanwendungen erstellen, in denen Clients Nachrichten an Dienste senden können, auch wenn der Dienst nicht ausgeführt wird. Applikationen, Bursts Kommunikation eine Warteschlange können, Erfahrung Ebene die Last mit einer Stelle Puffer Kommunikation. Schließlich erhalten Sie eine einfache, aber effektive Lastenausgleich Nachrichten auf mehrere Computer verteilt.

Um die Verfügbarkeit eines dieser Entitäten berücksichtigen Sie verschiedener Arten dieser Entitäten für ein dauerhaftes messaging-System nicht verfügbar erscheinen können. Im Allgemeinen sehen wir die Entität für Clientanwendungen verfügbar schreiben wir auf folgenden Arten:

- Keine Nachrichten senden.

- Keine Nachrichten empfangen.

- Nicht Entitäten verwalten (erstellen, abrufen, aktualisieren oder Löschen von Entitäten).

- Konnte nicht an den Dienst.

Für jeden Fehler vorhanden verschiedene Fehlermodi, die Anwendung weiterhin auf einer bestimmten Ebene reduzierten Fähigkeit Aufgaben ermöglichen. Beispielsweise ein System, das Nachrichten senden jedoch nicht empfangen kann Aufträge von Kunden erhalten jedoch kann diese Aufträge nicht verarbeiten. Dieses Thema behandelt Probleme, die auftreten können, und wie diese Probleme verringert werden. Service Bus hat mehrere schadensbegrenzende Strategien der in entscheiden müssen, und in diesem Thema erörtert die Modalitäten der Verwendung dieser Abschwächung anmelden.

## <a name="reliability-in-service-bus"></a>Zuverlässigkeit in Servicebus

Stehen mehrere Nachrichten und Entität Probleme und Richtlinien für die entsprechende Verwendung dieser Schutzmaßnahmen. Um die Richtlinien zu verstehen, müssen Sie zuerst verstehen, was in Service Bus nicht. Aufgrund der Azure-Systemen sind alle diese Probleme kurzlebig. Auf einer hohen Ebene die Ursachen von Ausfällen wie folgt angezeigt:

-   Einschränkung von einem externen System Service Bus hängt. Drosselung erfolgt von Interaktionen mit Speicher- und Compute-Ressourcen.

-   Problem bei einem System der Service Bus abhängig. Beispielsweise kann ein Teil des Speicher Probleme auftreten.

-   Fehler bei Service Bus auf einzelne Subsystem. In diesem Fall kann Compute-Knoten in einem inkonsistenten Zustand und muss neu gestartet werden, verursachen alle Entitäten zum Lastenausgleich mit anderen Knoten dient. Dies kann wiederum kurzer langsame Verarbeitung führen.

-   Fehler bei Service Bus in Azure-Rechenzentrum. Dies ist ein "Schwerwiegender Fehler" in dem System für viele Minuten oder einige Stunden nicht erreichbar ist.

> [AZURE.NOTE] Langfristige **Speicherung** bedeuten Azure Storage und SQL Azure.

Service Bus enthält eine Reihe von Schutzmaßnahmen für diese Probleme. Den folgenden Abschnitten werden die einzelnen Probleme und die entsprechenden Gegenmaßnahmen.

### <a name="throttling"></a>Drosselung

Mit Service Bus ermöglicht Drosselung kooperativen ratenverwaltung. Einzelnen Service Bus Knoten beinhaltet viele Entitäten. Jede dieser Entitäten stellt Anforderungen des Systems in Bezug auf CPU, Arbeitsspeicher, Speicher und andere Facets. Erkennt alle Facetten Verwendung Schwellenwerte überschreitet, Service Bus können eine bestimmte Anforderung verweigern. Der Aufrufer empfängt [ServerBusyException][] und nach 10 Sekunden wiederholt.

Als eine Minderung der Code den Fehler lesen und Wiederholungsversuche der Nachricht für 10 Sekunden angehalten. Da der Fehler auf der kundenanwendung möglich, erwartet jedes einzeln die Wiederholungslogik führt. Code kann die Wahrscheinlichkeit aktivieren auf eine Warteschlange oder ein Thema Partitionierung eingeschränkt verringern.

### <a name="issue-for-an-azure-dependency"></a>Problem bei Azure Abhängigkeit

Andere Komponenten in Azure können gelegentlich Probleme. Beispielsweise bei der Aktualisierung eines Service Bus verwendet erleben das System vorübergehend weniger Funktionen. Um derartige Probleme zu umgehen, Service Bus regelmäßig untersucht und Gegenmaßnahmen implementiert. Diese Abschwächung Nebenwirkungen werden angezeigt. Beispielsweise implementiert behandeln vorübergehender Probleme mit Service Bus Nachrichtenvorgänge konsistent funktioniert senden können. Aufgrund der Minderung dauert eine Nachricht bis zu 15 Minuten in der betroffenen Warteschlange oder Abonnement angezeigt und für einen Empfangsvorgang. Im Allgemeinen werden die meisten Entitäten tritt dieses Problem nicht. Allerdings ist angesichts der Anzahl von Entitäten in Service Bus in Azure, diese Abschwächung manchmal für eine kleine Teilmenge der Kunden Service Bus erforderlich.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Service Bus-Fehler auf einem subsystem

Bei jeder Anwendung kann Umstände eine interne Komponente des Service Bus inkonsistent wird. Wenn Service Bus erkennt, sammelt Daten aus der Anwendung zu diagnostizieren, was passiert. Nach den Daten wird die Anwendung versucht einen konsistenten Zustand wieder neu gestartet. Dabei geschieht schnell und einer Entität erscheint für einige Minuten nicht verfügbar wenn normale Ausfallzeiten sind viel kürzer.

In diesen Fällen generiert die Clientanwendung [System.TimeoutException][] oder [MessagingException][] Ausnahme. Service Bus enthält eine Lösung für dieses Problem in Form von automatisierten Client Wiederholungslogik. Die Wiederholungsperiode erschöpft ist, und die Nachricht nicht übermittelt, Erkunden Sie andere Funktionen wie [gepaarten Namespaces][]. Paarweise Namespaces sind andere Probleme, die in diesem Artikel beschriebenen.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Ausfall des Service Bus in Azure-Rechenzentrum

Die wahrscheinlichste Ursache für einen Fehler in Azure-Rechenzentrum ist eine fehlgeschlagene Aktualisierung Bereitstellung von Service Bus oder eines abhängigen. Wie die Plattform ausgereift ist, ist die Wahrscheinlichkeit, dass diese Art von Ausfällen verringert. Datacenter-Fehler kann auch Gründen auftreten, die Folgendes enthalten:

-   Stromausfall (Stromversorgung und generieren verschwindet macht).
-   Konnektivität (Internet Pause zwischen den Clients und Azure).

In beiden Fällen verursacht verursachten Katastrophe vom. [Paarweise Namespaces][] können Sie dieses Problem umgehen, stellen Sie sicher, dass Sie weiterhin Nachrichten senden können und Nachrichten an einem zweiten Ort gesendet werden, während der primäre Standort wieder fehlerfrei erfolgt. Weitere Informationen finden Sie unter [Best Practices für isolierende Anwendung Service Bus Ausfällen und Katastrophen][].

## <a name="paired-namespaces"></a>Paarweise namespaces

[Paarweise Namespaces][] Feature unterstützt Szenarios in der eine Entität Service Bus oder innerhalb eines Rechenzentrums verfügbar. Während dieses Ereignis nur selten auftritt, müssen verteilte Systeme weiterhin ungünstigsten Falle behandeln vorbereitet werden. Dieses Ereignis liegt in der Regel ein Element Service Bus hängt ein kurzfristiges Problem auftritt. Für die Aufrechterhaltung der Verfügbarkeit bei einem Ausfall können Service Bus Benutzer zwei separate Namespaces vorzugsweise in eigenen Rechenzentren Sie ihre Messaging-Elemente hosten. Der Rest dieses Abschnitts wird die folgende Terminologie verwendet:

-   Primäre Namespace: der Namespace, mit dem die Anwendung interagiert für Sende- und Empfangsvorgänge.

-   Sekundäre Namespace: der Namespace, der als Sicherung für den primären Namespace fungiert. Anwendungslogik interagiert nicht mit diesem Namespace.

-   Failover-Intervall: die Zeitspanne normale Fehlern zustimmen, bevor die Anwendung vom primären Namespace sekundäre Namespace wechselt.

Namespaces Support *Senden Verfügbarkeit*kombiniert. Senden Sie Verfügbarkeit behält die Möglichkeit, Nachrichten. Verfügbarkeit senden muss, die Anwendung Folgendes erfüllen:

1.  Nachrichten werden nur vom primären Namespace.

2.  Nachrichten an eine Warteschlange gesendet oder Thema möglicherweise in der falschen Reihenfolge eintreffen.

3.  Ihre Anwendung Sessions können Nachrichten in einer Sitzung nicht empfangen. Dies ist eine Pause von normalen Funktionen des Sessions. Dies bedeutet, dass Ihre Sessions Nachrichten logisch gruppieren Anwendung. Sitzungszustand wird nur auf dem primären Namespace beibehalten.

4.  In einer Sitzung kann nicht eingehen. Dies ist eine Pause von normalen Funktionen des Sessions. Dies bedeutet, dass Ihre Sessions Nachrichten logisch gruppieren Anwendung.

5.  Sitzungszustand wird nur auf dem primären Namespace beibehalten.

6.  Die primäre Warteschlange kann online geschaltet und akzeptieren Nachrichten vor dem sekundäre Warteschlange alle Nachrichten in die primäre Warteschlange übermittelt.

Die folgenden Abschnitte beschreiben die APIs, wie die APIs implementiert und Beispielcode zeigt verwendet die Funktion. Beachten Sie, dass die Abrechnung folgen dieser Funktion zugeordnet sind.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>Die MessagingFactory.PairNamespaceAsync-API

Die Funktion gepaarten Namespaces umfasst die [PairNamespaceAsync][] -Methode der [Microsoft.ServiceBus.Messaging.MessagingFactory][] -Klasse:

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Nach Abschluss die Aufgabe Namespace Verbindungsaufbau ist fertiggestellt und bereit für alle [MessageReceiver][], [QueueClient][]handeln oder [TopicClient][] mit der [Fall][] -Instanz erstellt. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] ist die Basisklasse für die verschiedenen koppeln, die mit einem [Fall][] Objekt verfügbar sind. Derzeit ist die einzige abgeleitete Klasse eine benannte [SendAvailabilityPairedNamespaceOptions][], der erforderlichen Verfügbarkeit senden implementiert. [SendAvailabilityPairedNamespaceOptions][] hat eine Reihe von Konstruktoren, die aufeinander aufbauen. Der Konstruktor mit den meisten Parametern betrachten, können Sie das Verhalten der anderen Konstruktoren verstehen.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Diese Parameter haben folgende Bedeutung:

-   *SecondaryNamespaceManager*: keine initialisierte Instanz [NamespaceManager][] für den zweiten Namespace, mit denen die [PairNamespaceAsync][] -Methode der sekundären Namespace einrichten. Namespace-Manager wird eine Liste der Warteschlangen im Namespace und sicherstellen, dass die erforderlichen Rückstand Warteschlangen verwendet. Wenn die Warteschlangen nicht vorhanden sind, werden sie erstellt. [NamespaceManager][] benötigen Sie zum Erstellen eines Tokens mit dem **Verwalten** .

-   *Fall*: [Fall][] Instanz für den zweiten Namespace. [Fall][] -Objekt zum Senden und wenn die [EnableSyphon][] -Eigenschaft auf **true**festgelegt ist von den Rückstand Warteschlangen empfangen.

-   *BacklogQueueCount*: die Anzahl der Rückstand Warteschlangen erstellen. Dieser Wert muss mindestens 1 sein. Beim Senden von Nachrichten an den Rückstand ist einer der folgenden Warteschlangen zufällig ausgewählt. Wenn Sie den Wert auf 1 festlegen, kann immer nur eine Warteschlange verwendet. In diesem Fall eine Backlogwarteschlange Fehler generiert, der Client wird eine andere Backlogwarteschlange versuchen nicht und möglicherweise Ihre Nachricht. Es wird empfohlen, diese Einstellung auf einen größeren Wert und standardmäßig des Wert 10. Sie können dies auf einen höheren oder niedrigeren Wert je nach Datenmenge ändern, die Anwendung pro Tag sendet. Jede Backlogwarteschlange kann bis zu 5 GB Nachrichten enthalten.

-   *FailoverInterval*: die Zeitspanne, während der akzeptieren Sie Fehler auf dem primären Namespace vor jeder Einheit auf den sekundären Namespace umschalten. Failover auftreten zu Person-Entität. Elemente in einem einzelnen Namespace live häufig in verschiedenen Knoten in Service Bus. Ein Fehler in einer Entität impliziert einen Fehler in einem anderen nicht. Sie können diesen Wert [System.TimeSpan.Zero][] Failover auf den sekundären unmittelbar nach der ersten, nicht flüchtigen Fehler fest. Fehler, die den Zeitgeber Failover auslösen sind alle [MessagingException][] in der [IsTransient][] -Eigenschaft False ist oder [System.TimeoutException][]. Andere Ausnahmen wie [UnauthorizedAccessException][] verursachen Failover, da sie zeigen, dass der Client nicht ordnungsgemäß konfiguriert ist. [ServerBusyException][] bewirkt keine Failover ist die korrekte Vorgehensweise 10 Sekunden warten, senden Sie die Nachricht erneut.

-   *EnableSyphon*: Gibt an, dass diese bestimmte Kombination auch Nachrichten von sekundären Namespace an den primären Namespace einen sollte. Im Allgemeinen sollten Anträge, die Nachrichten senden dieser Wert auf **false**festgelegt. Programme, die Nachrichten sollte dieser Wert auf **true**festgelegt. Der Grund dafür ist, dass häufig weniger Nachricht Empfänger als Absender sind. Je nach Anzahl der Empfänger können Sie eine einzelne Anwendung Instanz Siphon Aufgaben behandeln. Viele Empfänger mit wirkt Abrechnung für jede Backlogwarteschlange.

Erstellen Sie zum Verwenden des Codes eine primäre [Fall][] Instanz, eine zweite Instanz der [Fall][] eine sekundäre [NamespaceManager][] -Instanz und eine [SendAvailabilityPairedNamespaceOptions][] -Instanz. Der Aufruf kann so einfach wie folgt:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Nach Abschluss der [PairNamespaceAsync][] -Methode zurückgegebene Aufgabe wird alles um verwenden festgelegt. Bevor die Aufgabe zurückgegeben wird, möglicherweise nicht alle der Hintergrund arbeiten die Verbindung funktioniert Abschluss. Daher sollten Sie nicht starten, Nachrichten senden, bis die Aufgabe zurückgegeben. Wenn Fehler aufgetreten oder falsche Anmeldeinformationen, Backlog-Warteschlangen erstellt werden nach Abschluss der Aufgabe diese Ausnahmen ausgelöst. Nach Aufgabe zurückgibt, überprüfen Sie, ob die Warteschlangen gefunden oder mithilfe der [BacklogQueueCount][] -Eigenschaft die Instanz der [SendAvailabilityPairedNamespaceOptions][] erstellt wurden. Für das vorherige Codebeispiel wird angezeigt, wie folgt:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der asynchronen messaging Service Bus bearbeitet haben, lesen Sie mehr über [gepaarten Namespaces][].

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Best Practices für die Isolierung Applikationen Service Bus Ausfällen und Katastrophen]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [Fall]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [paarweise namespaces]: service-bus-paired-namespaces.md