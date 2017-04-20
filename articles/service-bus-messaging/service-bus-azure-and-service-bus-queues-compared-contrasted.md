<properties 
    pageTitle="Azure Warteschlangen und Service Bus Warteschlangen - Vergleich und bereitstehen | Microsoft Azure"
    description="Unterschiede und Ähnlichkeit zwischen zwei Warteschlangentypen Azure angebotenen analysiert."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure Warteschlangen und Service Bus Warteschlangen - Vergleich und Gegensatz

In diesem Artikel analysiert die Unterschiede und die Ähnlichkeit zwischen zwei Arten von Warteschlangen Angeboten von Microsoft Azure heute: Azure-Warteschlangen und Service Bus-Warteschlangen. Mithilfe dieser Informationen können Sie verglichen und Kontrast Technologien und können eine fundiertere Entscheidung über die Lösung am besten Ihren Bedürfnissen entspricht.

## <a name="introduction"></a>Einführung

Microsoft Azure unterstützt zwei Arten von Warteschlangen Mechanismen: **Azure** und **Service Bus-Warteschlangen**.

**Azure-Warteschlangen**sind Bestandteil der [Azure-Speicher](https://azure.microsoft.com/services/storage/) bieten eine einfache REST-basierte Get/Put/Peek-Schnittstelle bietet zuverlässige, permanente messaging innerhalb und zwischen.

**Servicebuswarteschlangen** sind Teil einer größeren [Azure Messaging-](https://azure.microsoft.com/services/service-bus/) Infrastruktur, die queuing veröffentlichen/abonnieren, Web Service Remoting und Integration Muster unterstützt. Informationen über Servicebuswarteschlangen Themen-Abonnements und Relais finden Sie unter [Übersicht über messaging Service Bus](service-bus-messaging-overview.md).

Gleichzeitig ist beiden Warteschlangen eingeführt Azure-Warteschlangen zunächst eine dedizierte Warteschlange Speichermechanismus Azure-Speicherdienste aufbaut. Servicebuswarteschlangen basieren auf der breiteren "vermittelte messaging" Infrastruktur integrieren Applikationen oder Komponenten, die mehrere Kommunikationsprotokolle Datenverträgen umfassen Domänen vertrauen und Netzwerk-Umgebung.

Dieser Artikel vergleicht die zwei Warteschlange Technologien von Azure die Unterschiede im Verhalten und Implementierung der Features von jedem. Der Artikel enthält außerdem Hinweise zum Auswählen mit Application Development anpassen können.

## <a name="technology-selection-considerations"></a>Auswahl Technologiefragen

Azure-Warteschlangen und Service Bus-Warteschlangen sind Implementierungen der Message Queuing-Dienst auf Microsoft Azure angeboten. Jeder hat einen geringfügig Funktionsumfang, d. h., Sie können eine oder das andere oder beides muss von einer bestimmten Projektmappe oder Business-technische Problem Sie lösen.

Festlegung die Warteschlangen-Technologie für eine gegebene Lösung entspricht sollten Lösungsarchitekten und Entwickler die folgenden empfohlenen. Weitere Informationen finden Sie im nächsten Abschnitt.

Solution Architect/Entwickler **erwägen Sie Azure-Warteschlangen** beim:

- Ihre Anwendung muss über 80 GB Nachrichten in einer Warteschlange speichern, die Nachrichten über eine Lebensdauer weniger als 7 Tage.

- Die Anwendung möchte für die Verarbeitung einer Nachricht in der Warteschlange verfolgen. Dies ist hilfreich, wenn die Verarbeitung einer Nachricht Arbeitskraft stürzt ab. Eine nachfolgende Arbeitskraft können Sie die Informationen weiterhin aus, wo die vorherige Arbeitskraft.

- Sie benötigen Server Seite Protokolle aller Transaktionen in Warteschlangen ausgeführt.

Solution Architect/Entwickler **Servicebuswarteschlangen verwenden,** wenn:

- Die Projektmappe muss Nachrichten empfangen, ohne die Warteschlange abrufen. Service Bus kann dies durch die Long Polling Empfangsvorgang mit TCP-basierte Protokolle, Service Bus unterstützt.

- Die Lösung erfordert die Warteschlange zu einer garantierten First-in-First-Out (FIFO) geordnete Übermittlung.

- Sie möchten eine symmetrische Erfahrung in Azure und Windows Server (private Cloud). Weitere Informationen finden Sie unter [Service Bus für Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).

- Ihre Lösung muss automatische Duplikaterkennungsregeln unterstützen.

- Sie möchten die Anwendung Nachrichten verarbeiten wie parallele langer (Nachrichten sind verknüpft mit der Nachricht über die [SessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) -Eigenschaft). In diesem Modell konkurriert jeder Knoten in der verwendeten Anwendung im Gegensatz zu Nachrichten-Streams. Wenn Stream verwendeten Knoten angegeben wird, kann der Knoten den Zustand des Streams Anwendungsstatus Transaktionen untersuchen.

- Die Lösung erfordert Transaktionsverhalten und Unteilbarkeit beim Senden oder mehrere Nachrichten aus einer Warteschlange empfangen.

- Time-to-live (TTL) Merkmal der spezifischen Arbeitslast kann 7 Tage überschreiten.

- Die Anwendung behandelt Nachrichten, die 64 KB überschreiten können, aber wird nicht Ansatz 256 KB beschränkt.

- Sie arbeiten mit rollenbasierten Modell Warteschlangen und andere Berechtigungen für Absender und Empfänger bereitstellen.

- Die Größe wächst nicht mehr als 80 GB.

- AMQP 1.0 standardbasierte Messaging-Protokoll verwendet werden soll. Weitere Informationen zu AMQP finden Sie unter [Service Bus AMQP Overview](./service-bus-amqp-overview.md).

- Sie können eine letztendliche Migration Queue-basierten Point-Mitteilung zu einer Nachrichtenaustauschmuster vorstellen, die nahtlose Integration von zusätzlichen Empfängern (Abonnenten) ermöglicht jeweils unabhängige Kopien einiger oder aller Nachrichten in der Warteschlange empfangen. Diese bezieht sich auf den Veröffentlichen-Abonnieren Funktion systemintern Service Bus.

- Messaging-Lösung muss "Am meisten-einmalige" Liefergarantie ohne zusätzliche Infrastrukturkomponenten erstellen unterstützen können.

- Sie würden gerne veröffentlichen und Nachrichtenbatches verarbeiten können.

- Vollständige Integration mit Windows Communication Foundation (WCF) Kommunikationsstapel in.NET Framework erforderlich.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Vergleich von Azure-Warteschlangen und Service Bus-Warteschlangen

Die Tabellen in den folgenden Abschnitten eine logische Gruppierung von Features Warteschlange und auf einen Blick die Azure-Warteschlangen und Servicebuswarteschlangen Funktionen verglichen.

## <a name="foundational-capabilities"></a>Grundlegende Funktionen

In diesem Abschnitt werden einige der grundlegenden queuing Funktionen von Azure-Warteschlangen und Service Bus Warteschlangen verglichen.

|Vergleichskriterien|Azure-Warteschlangen|Service Bus-Warteschlangen|
|---|---|---|
|Reihenfolge der Garantie|**Nein** <br/><br>Weitere Informationen finden Sie in der ersten Notiz im Abschnitt "Weitere Informationen".</br>|**Ja - First In First Out (FIFO)**<br/><br>(durch messaging Sitzungen)|
|Liefergarantie|**Einmal am wenigsten**|**Einmal am wenigsten**<br/><br/>**Einmal am meisten**|
|Atomare Operation support|**Nein**|**Ja**<br/><br/>|
|Das Empfangsverhalten|**Nicht blockieren**<br/><br/>(sofort abgeschlossen, wenn keine neue Meldung gefunden)|**Mit oder ohne Timeout blockiert**<br/><br/>(Angebote long Polling oder ["Comet-Technik"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Nicht blockieren**<br/><br/>(durch .NET API nur verwaltet)|
|Push-Format-API|**Nein**|**Ja**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) und **OnMessage** -Sessions .NET API.|
|Empfangsmodus|**Peek & Leasing**|**Peek & Sperren**<br/><br/>**Löschen & empfangen**|
|Exklusiv-Modus|**Leasing-Basis**|**Auf Sperren basierende**|
|Leasing/Lock Dauer|**30 Sekunden (Standard)**<br/><br/>**7 Tage (maximal)** (Erneuern oder eine Nachricht Lease mit [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API freigeben.)|**Sekunden (Standard)**<br/><br/>Sie können eine Nachricht Sperren mit [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API erneuern.|
|Leasing/Lock Genauigkeit|**Meldung**<br/><br/>(jede Nachricht haben einen anderen Timeoutwert Sie dann nach Bedarf aktualisieren können, während der Verarbeitung der Nachricht mit [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API)|**Warteschlange-Ebene**<br/><br/>(jede Warteschlange weist eine Sperre Genauigkeit auf alle Nachrichten angewendet, aber die Sperre mit [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API erneuern.)|
|Zusammengefasst angezeigt|**Ja**<br/><br/>(beim explizit angeben Nachrichtenanzahl Nachrichten maximal 32 Nachrichten abrufen)|**Ja**<br/><br/>(eine Pre-Fetch-Eigenschaft implizit aktivieren oder explizit Transaktionen)|
|Im Batch senden|**Nein**|**Ja**<br/><br/>(durch Transaktionen oder clientseitige Batchverarbeitung)|

### <a name="additional-information"></a>Weitere Informationen

- Nachrichten in Azure-Warteschlangen sind normalerweise First-in-First-Out, aber manchmal können sie angeordnet sein; z. B. wenn eine Nachricht Sichtbarkeit Timeoutdauer (beispielsweise aufgrund einer Clientanwendung abstürzt, während der Verarbeitung) abläuft. Sichtbarkeitstimeout abläuft, wird die Meldung erneut auf die Warteschlange für eine andere Arbeitskraft aus der Warteschlange entfernt. An diesem Punkt möglicherweise neu sichtbare Nachricht in der Warteschlange (erneut aus der Warteschlange entfernt werden) nach einer Nachricht befinden, die Warteschlange nach dem ursprünglich.

- Sie verwenden bereits Azure Storage Blobs oder Tabellen und starten mithilfe von Warteschlangen, die Verfügbarkeit von 99,9 % garantiert. Wenn Sie Blobs und Tabellen mit Servicebuswarteschlangen verwenden, müssen Sie niedrigere Verfügbarkeit.

- Garantierte FIFO-Muster in Servicebuswarteschlangen erfordert die Verwendung von Messaging-Sitzung. Die Anwendung stürzt ab beim Verarbeiten einer Nachricht im **Blick & Lock** -Modus erhalten, wird das nächste Mal, das ein Warteschlange Empfänger eine messaging-Sitzung akzeptiert, mit fehlgeschlagenen Nachricht gestartet nachdem die Time-to-live (TTL) abgelaufen.

- Azure-Warteschlangen sollen queuing Standardszenarien wie entkoppeln Anwendungskomponenten Skalierbarkeits-und Toleranz bei Fehlern laden, Abgleich und Prozess-Workflows erstellen.

- Servicebuswarteschlangen unterstützen Liefergarantie *Einmal am geringsten* . Darüber hinaus können *Einmal am meisten* semantische Sitzungszustand Zustand der Anwendung gespeichert und mithilfe von Transaktionen atomar empfangen und Aktualisieren des Sitzungszustands unterstützt werden.

- Azure Warteschlangen bieten eine einheitliche und konsistente Programmiermodell in Warteschlangen und Tabellen mit BLOBs – für Entwickler und Betriebsteams.

- Servicebuswarteschlangen unterstützen lokale Transaktionen im Kontext einer einzelnen Warteschlange.

- Service Bus unterstützte **empfangen und löschen** Modus ermöglicht messaging Vorgang Count (und Kosten) zu für geringere liefersicherheit.

- Azure-Warteschlangen bieten Leases Leases für Nachrichten erweitern. Dadurch wird die Arbeitskräfte zu kurzen Leasings Nachrichten. Stürzt eine Arbeitskraft kann die Nachricht so schnell wieder eine andere Arbeitskraft verarbeitet. Darüber hinaus kann eine Arbeitskraft die Lease für eine Nachricht erweitern sollte es verarbeitet länger als die aktuelle Leasezeit

- Azure-Warteschlangen bieten sichtbarkeitstimeout, die Sie auf den Enqueueing festlegen oder Entfernen einer Nachricht. Darüber hinaus können Sie eine Aktualisierungsnachricht unterschiedliche Leasing-Werte zur Laufzeit und unterschiedliche Werte über Nachrichten in der gleichen Warteschlange aktualisieren. Service Bus Sperre Timeouts werden in den Metadaten Warteschlange definiert. Allerdings können Sie die Sperre durch Aufrufen der [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) -Methode erneuern.

- Das maximale Timeout beträgt ein blockierender Vorgang in Servicebuswarteschlangen erhalten 24 Tage. REST-basierte Timeouts haben jedoch maximal 55 Sekunden.

- Clientseitige Batchverarbeitung bereitgestellten Service Bus ermöglicht einem Client Warteschlange mehrere Nachrichten in einer einzigen Sendevorgang Batch. Batchverarbeitung ist nur für asynchrone Sendevorgänge.

- Funktionen wie die 200 TB von Azure (wenn Virtualisierung Konten mehr) und unbegrenzte Warteschlangen erleichtern eine ideale Plattform für SaaS-Anbieter.

- Azure-Warteschlangen bieten eine flexible und leistungsfähige Zugriffssteuerungsmechanismus delegiert.

## <a name="advanced-capabilities"></a>Erweiterte Funktionen

In diesem Abschnitt werden erweiterte Funktionen von Azure-Warteschlangen und Service Bus Warteschlangen verglichen.

|Vergleichskriterien|Azure-Warteschlangen|Service Bus-Warteschlangen|
|---|---|---|
|Geplante Lieferung|**Ja**|**Ja**|
|Deaktiviert automatische Beschriftung|**Nein**|**Ja**|
|Warteschlange TTL Wert erhöhen|**Ja**<br/><br/>(über direktes Update Sichtbarkeit Timeout)|**Ja**<br/><br/>(über ein dediziertes API-Funktion bereitgestellt)|
|Poison Message-Unterstützung|**Ja**|**Ja**|
|In-Place-Aktualisierung|**Ja**|**Ja**|
|Serverseitige Transaktionsprotokoll|**Ja**|**Nein**|
|Speichermetriken|**Ja**<br/><br/>**Minute Metriken**: bietet Echtzeit-Metriken für Verfügbarkeit, TPS-API aufrufen, zählt, Fehler und mehr in Echtzeit (pro Minute aggregiert und in wenigen Minuten von was Produktion zufällig gemeldet. Weitere Informationen finden Sie unter [Über Storage Analytics-Metriken](https://msdn.microsoft.com/library/azure/hh343258.aspx).|**Ja**<br/><br/>(Bulk Abfragen durch Aufrufen von [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx))|
|Staatliche Verwaltung|**Nein**|**Ja**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Automatische Weiterleitung|**Nein**|**Ja**|
|Bereinigen der Warteschlangen-Funktion|**Ja**|**Nein**|
|Nachrichtengruppen|**Nein**|**Ja**<br/><br/>(durch messaging Sitzungen)|
|Anwendungsstatus pro Nachricht|**Nein**|**Ja**|
|Duplikaterkennungsregeln|**Nein**|**Ja**<br/><br/>(auf der Seite Absender konfigurierbar)|
|WCF-integration|**Nein**|**Ja**<br/><br/>(bietet Out-of-the-Box-WCF-Bindung)|
|WF integration|**Benutzerdefinierte**<br/><br/>(erstellen eine benutzerdefinierte WF-Aktivität erforderlich)|**Systemeigene**<br/><br/>(bietet Out-of-the-Box-WF-Aktivitäten)|
|Durchsuchen von Nachrichtengruppen|**Nein**|**Ja**|
|Abrufen der Nachricht Sessions nach ID|**Nein**|**Ja**|

### <a name="additional-information"></a>Weitere Informationen

- Warteschlangen-Technologie ermöglichen eine Nachricht für die Übermittlung zu einem späteren Zeitpunkt geplant werden.

- Automatische Weiterleitung der Warteschlange kann Tausende von Warteschlangen automatisch ihre Nachrichten an einzelne Warteschlangen Vorlauf aus der empfangende Anwendung die Nachricht verwendet. Sie können dieser Mechanismus mit der Sicherheit, steuern und Speicher zwischen jeder Nachricht isolieren.

- Azure-Warteschlangen unterstützen Inhalt aktualisieren. Diese Funktion können zum Speichern von Zustandsinformationen und inkrementellen Fortschritt in die Nachricht damit vom letzten bekannten Prüfpunkt statt völlig verarbeitet werden können. Service Bus-Warteschlangen können Sie das gleiche Szenario durch Nachricht Sessions. Sessions können Sie speichern und Abrufen des Verarbeitungsstatus Anwendung (mit [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)).

- [Tote Schrift](service-bus-dead-letter-queues.md)unterstützt nur durch Servicebuswarteschlangen, kann hilfreich, um Nachrichten, die von der empfangenden Anwendung oder Nachrichten wegen einer abgelaufenen Time to live (TTL) Ziel können nicht erfolgreich verarbeitet werden können. Der TTL-Wert gibt an, wie lange eine Meldung in der Warteschlange verbleibt. Mit Service Bus wird die Nachricht an eine spezielle Warteschlange $DeadLetterQueue aufgerufen, wenn die Gültigkeitsdauer abläuft verschoben werden.

- Zu "unzustellbaren" Nachrichten in Azure-Warteschlangen, wenn eine Nachricht aus der Anwendung untersucht die **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** -Eigenschaft der Nachricht. Ist **DequeueCount** über einem bestimmten Schwellenwert, verschiebt die Anwendung die Nachricht an eine anwendungsdefinierte "" Warteschlange für unzustellbare.

- Azure-Warteschlangen können Sie erhalten ein detailliertes Protokoll aller Transaktionen in der Warteschlange als auch aggregierte Metriken ausgeführt. Beide Optionen sind nützlich zum Debuggen und verstehen, wie Ihre Anwendung Azure-Warteschlangen. Sie sind auch nützlich für die Anwendung optimieren und Senkung der Kosten für die Verwendung von Warteschlangen.

- Das Konzept der "Nachricht Sessions" Service Bus unterstützt kann Nachrichten, die an bestimmte logische für einen bestimmten Empfänger wiederum erzeugt eine Sitzung wie Affinität zwischen Nachrichten und ihre jeweiligen Empfänger gehören. Diese erweiterte Funktionalität in Service Bus können die [SessionID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) -Eigenschaft für eine Nachricht festlegen. Empfänger können eine bestimmte Sitzung ID Abhören und Empfangen von Nachrichten mit der angegebenen Sitzungsbezeichner.

- Der Duplizierung Erkennung Funktionen Servicebuswarteschlangen automatisch entfernt doppelte Nachrichten an eine Warteschlange oder ein Thema basierend auf dem Wert der [MessageID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) -Eigenschaft.

## <a name="capacity-and-quotas"></a>Kapazität und Kontingente

Dieser Abschnitt vergleicht Azure-Warteschlangen und Service Bus Warteschlangen hinsichtlich [Kapazität und Kontingente](service-bus-quotas.md) , die gelten.

|Vergleichskriterien|Azure-Warteschlangen|Service Bus-Warteschlangen|
|---|---|---|
|Maximale Warteschlangengröße|**200 TB**<br/><br/>(beschränkt auf ein einzelnes Konto Speicherkapazität)|**1 GB bis 80 GB**<br/><br/>(beim Erstellen einer Warteschlange und [Partitionierung aktivieren](service-bus-partitioning.md) – definiert finden Sie im Abschnitt "Weitere Informationen")|
|Maximale Nachrichtengröße|**64 KB**<br/><br/>(48 KB bei mit **Base64** -Codierung)<br/><br/>Azure unterstützt große Nachrichten Warteschlangen und Blobs – Zeitpunkt Enqueue können bis zu 200GB für ein einzelnes Element.|**256 KB** oder **1 MB**<br/><br/>(einschließlich der Header und Body, maximale Headergröße: 64 KB).<br/><br/>Hängt von der [Dienstebene](service-bus-premium-messaging.md).|
|Maximale Nachrichtengröße TTL|**7 Tage**|**`TimeSpan.Max`**|
|Maximale Anzahl der Warteschlangen|**Unbegrenzt**|**10.000**<br/><br/>(pro Dienstnamespace kann erhöht werden)|
|Maximale Anzahl gleichzeitig verbundener clients|**Unbegrenzt**|**Unbegrenzt**<br/><br/>(100 gleichzeitige Verbindung Grenzwert gilt nur für TCP-Protokoll-Kommunikation)|

### <a name="additional-information"></a>Weitere Informationen

- Service Bus erzwingt Größe Beschränkung. Die maximale Warteschlangengröße wird beim Erstellen der Warteschlange und kann einen Wert zwischen 1 und 80 GB. Der Größenwert Warteschlange beim Erstellen der Warteschlange erreicht ist, zusätzliche Nachrichten abgelehnt, und der aufrufende Code eine Ausnahme erhalten werden. Weitere Informationen zu Kontingenten in Service Bus finden Sie unter [Service Bus Kontingente](service-bus-quotas.md).

- Sie können Servicebuswarteschlangen 1, 2, 3, 4 oder 5 GB Größe erstellen (die Standardeinstellung ist 1 GB). Mit Partitionierung aktiviert (Standard), Service Bus 16 Partitionen für jedes GB angeben. Wenn Sie eine Warteschlange, die 5 GB Größe erstellen, mit 16 Partitionen die maximale Warteschlangengröße wird (5 * 16) = 80 GB. Die maximale Größe der partitionierten Warteschlange oder Thema sehen von [Azure-Portal][]auf der auf.

- Azure-Warteschlangen ist der Inhalt der Nachricht nicht XML-sichere muss es **Base64** -codiert sein. Wenn Sie **Base64**-Codierung der Nachricht, Benutzer Nutzlast kann bis zu 48 KB anstelle von 64 KB.

- Mit Servicebuswarteschlangen, jede Nachricht in einer Warteschlange gespeichert besteht aus zwei Teilen: einem Header und Text. Die Gesamtgröße der Nachricht darf die maximale Größe der Dienstebene unterstützt nicht überschreiten.

- Kommunikation von Clients mit Servicebuswarteschlangen über das TCP-Protokoll wird die maximale Anzahl von aktiven Verbindungen zu einer einzigen Service Bus-Warteschlange auf 100. Diese Nummer ist zwischen Absender und Empfänger freigegeben. Dieses Kontingent erreicht, nachfolgende Anforderungen für zusätzliche Verbindungen abgelehnt, und der aufrufende Code eine Ausnahme erhalten werden. Dieses Limit wird mit Warteschlangen mit REST-basierten API nicht auferlegt.

- Benötigen Sie mehr als 10.000 Warteschlangen in einem einzelnen Service Bus-Namespace können Azure wenden und eine Erhöhung anfordern. Um über 10.000 Warteschlangen mit Service Bus skalieren, können Sie auch zusätzliche Namespaces mithilfe der [Azure-Portal][]erstellen.

## <a name="management-and-operations"></a>Verwaltung und Betrieb

Dieser Abschnitt vergleicht die Verwaltungsfunktionen von Azure-Warteschlangen und Service Bus Warteschlangen bereitgestellt.

|Vergleichskriterien|Azure-Warteschlangen|Service Bus-Warteschlangen|
|---|---|---|
|Management-Protokoll|**Zeigen Sie über HTTP/HTTPS**|**Positionieren Sie über HTTPS**|
|Laufzeit-Protokoll|**Zeigen Sie über HTTP/HTTPS**|**Positionieren Sie über HTTPS**<br/><br/>**AMQP 1.0 Standard (TCP mit TLS)**|
|.NET verwalteten API|**Ja**<br/><br/>(Verwalteten Speicher-Client-API)|**Ja**<br/><br/>(Verwalteter vermittelte messaging-API)|
|Systemeigenes C++|**Ja**|**Nein**|
|Java API|**Ja**|**Ja**|
|PHP-API|**Ja**|**Ja**|
|Node.js API|**Ja**|**Ja**|
|Beliebige Metadaten-Unterstützung|**Ja**|**Nein**|
|Warteschlange Regeln|**Bis zu 63 Zeichen lang sein**<br/><br/>(Buchstaben im Namen einer Warteschlange müssen Kleinbuchstaben sein.)|**Bis zu 260 Zeichen.**<br/><br/>(Warteschlangenpfade und Namen sind Groß-und Kleinschreibung.)|
|Queue Length Funktion|**Ja**<br/><br/>(Ungefährer Wert Wenn Nachrichten über die Gültigkeitsdauer abläuft, ohne gelöscht.)|**Ja**<br/><br/>(Genau, Point-in-Time-Wert.)|
|Peek-Funktion|**Ja**|**Ja**|

### <a name="additional-information"></a>Weitere Informationen

- Azure-Warteschlangen bieten Unterstützung für beliebige Attribute, die Beschreibung der Warteschlange in Form von Name-Wert-Paare angewendet werden können.

- Warteschlange Technologie ermöglichen einsehen einer Meldung ohne, sperren, die hilfreich beim Implementieren einer Warteschlange Explorer-Browser-Tool.

- Service Bus .NET vermittelte messaging APIs nutzen Vollduplex-TCP-Verbindungen für verbesserte Leistung im Vergleich zu anderen über HTTP und das Standardprotokoll AMQP 1.0 unterstützen.

- Namen der Azure-Warteschlangen können 3 63 Zeichen lang sein, Kleinbuchstaben, Zahlen und Bindestriche enthalten. Weitere Informationen finden Sie unter [Benennen von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/azure/dd179349.aspx).

- Service Bus Namen können bis zu 260 Zeichen lang sein und weniger restriktive Regeln. Service Bus Namen können Buchstaben, Zahlen, Punkte, Bindestriche und Unterstriche enthalten.

## <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Dieser Abschnitt beschreibt die Authentifizierung und Autorisierung Features Azure-Warteschlangen und Service Bus-Warteschlangen.

|Vergleichskriterien|Azure-Warteschlangen|Service Bus-Warteschlangen|
|---|---|---|
|Authentifizierung|**Symmetrischen Schlüssel**|**Symmetrischen Schlüssel**|
|Sicherheitsmodell|Delegierter Zugriff über SAS-Token.|SAS|
|Anbieter Identitätsverbund|**Nein**|**Ja**|

### <a name="additional-information"></a>Weitere Informationen

- Jede Anforderung einer Warteschlange Technologies muss authentifiziert werden. Öffentliche Warteschlangen mit anonymem Zugriff werden nicht unterstützt. Mithilfe [SAS](service-bus-sas-overview.md), können Sie dieses Szenario veröffentlichen lesegeschützte SAS, schreibgeschützte SAS oder sogar Vollzugriff SAS adressieren.

- Azure-Warteschlangen bereitgestellte Authentifizierungsschema beinhaltet die Verwendung eines symmetrischen Schlüssels eine Hash-basierten HMAC Message Authentication Code (), SHA-256-Algorithmus berechnet und als eine **Base64** -Zeichenfolge codiert wird. Weitere Informationen zu den jeweiligen finden Sie unter [Authentifizierung für Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179428.aspx). Servicebuswarteschlangen unterstützt ein ähnliches Modell mit symmetrischen Schlüsseln. Weitere Informationen finden Sie unter [Freigegebene Signatur Authentifizierung mit Service Bus](service-bus-shared-access-signature-authentication.md).

## <a name="cost"></a>Kosten

Dieser Abschnitt vergleicht Azure-Warteschlangen und Service Bus Warteschlangen von Kosten.

|Vergleichskriterien|Azure-Warteschlangen|Service Bus-Warteschlangen|
|---|---|---|
|Warteschlange Transaktionskosten|**$0.0036**<br/><br/>(pro 100.000 Transaktionen)|**Grundlegende Ebene**: **0,05**<br/><br/>(pro million Operationen)|
|Berechenbare Vorgänge|**Alle**|**Übermittlung nur**<br/><br/>(kostenlos für andere Vorgänge)|
|Im Leerlauf Transaktionen|**Berechenbare**<br/><br/>(Leere Warteschlange Abfragen als berechenbare Transaktion zählt.)|**Berechenbare**<br/><br/>(Leere Warteschlange empfangen gilt eine berechenbare Nachricht.)|
|Speicherkosten|**$0,07**<br/><br/>(pro GB pro Monat)|**0,00 DM**|
|Ausgehende Datenübertragung Kosten|**$0,12 - $0,19**<br/><br/>(Je nach Region).|**$0,12 - $0,19**<br/><br/>(Je nach Region).|

### <a name="additional-information"></a>Weitere Informationen

- Datenübertragungen werden basierend auf der gesamten Daten Azure Rechenzentren über das Internet in einem bestimmten Abrechnungszeitraum belastet.

- Datenübertragungen zwischen Azure Services innerhalb derselben Region sind nicht gebührenpflichtig.

- Derzeit werden alle eingehenden Datenübertragungen nicht berechnet.

- Unterstützung für lange Polling, kann mit Servicebuswarteschlangen Kosten in Situationen wirksam, niedriger Latenz Übermittlung erforderlich ist.

>[AZURE.NOTE] Alle Kosten vorbehalten. Diese Tabelle gibt aktuelle Preise und keiner Sonderangebote, die derzeit verfügbar sind. Aktuelle Informationen über Preise Azure finden Sie auf Seite [Azure Preise](https://azure.microsoft.com/pricing/) . Weitere Informationen zu Service Bus Preise finden Sie unter [Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="conclusion"></a>Abschluss

Um ein tieferes Verständnis der beiden Technologien, Sie werden auf die Warteschlange Technologie, fundierte Entscheidung und. Die Entscheidung zur Verwendung von Azure-Warteschlangen oder Service Bus Warteschlangen hängt eindeutig eine Reihe von Faktoren. Diese Faktoren können die Bedürfnisse der Anwendung und der Architektur stark abhängig. Wenn die Anwendung bereits die Kernfunktionen von Microsoft Azure verwendet, können Sie bevorzugen Azure-Warteschlangen besonders benötigen Sie grundlegende Kommunikations- und messaging-Dienste oder Warteschlangen erforderlich, die größer als 80 GB groß sein können.

Denn Servicebuswarteschlangen einige erweiterte Features, z. B., Transaktionen, automatische Erkennung doppelter Dead Buchstaben und permanente Veröffentlichen/Abonnieren-Funktionen können sie eine bevorzugte Wahl sein, wenn Sie eine hybridanwendung erstellen oder andernfalls die Anwendung diese Features erfordert.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten weitere Leitfäden und Informationen über Azure-Warteschlangen oder Service Bus Warteschlangen.

- [Das Verwenden von Service Bus Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
- [Verwendung der Warteschlangendienst Speicher](../storage/storage-dotnet-how-to-use-queues.md)
- [Best Practices für die Verwendung des Service Bus Leistungssteigerungen vermittelte messaging](service-bus-performance-improvements.md)
- [Einführung in Warteschlangen und Themen in Azure Servicebus](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [Developer's Guide to Servicebus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Verwenden der Warteschlangendienst in Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Grundlegendes zur Abrechnung der Azure-Speicher-Bandbreite, Transaktionen und Kapazität](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Azure-portal]: https://portal.azure.com
 
