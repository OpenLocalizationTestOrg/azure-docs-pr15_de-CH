<properties 
    pageTitle="Isolierende Service Bus Applikationen vor Ausfällen und Katastrophen | Microsoft Azure"
    description="Beschreibt Techniken, mit denen Sie vor potenziellen Systemausfall Service Bus schützen."
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
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Best Practices für die Isolierung Applikationen Service Bus Ausfällen und Katastrophen

Unternehmenswichtige Anwendung en unterhält, auch bei ungeplanten Ausfällen und Katastrophen. Dieses Thema beschreibt Techniken, mit denen Sie Service Bus auf einen möglichen Dienstausfall oder Notfall schützen.

Ein Ausfall ist definiert als vorübergehenden Ausfall von Azure Service Bus. Der Ausfall kann einige Komponenten des Service Bus wie einen Nachrichtenspeicher oder sogar das gesamte Rechenzentrum auswirken. Nachdem das Problem behoben wurde, wird wieder Service Bus verfügbar. Ein Ausfall verursacht normalerweise keine Nachrichten oder andere Daten verloren gehen. Ein Beispiel der Ausfall einer Komponente ist dazu bestimmten Nachrichtenspeicher. Ein Beispiel für einen Rechenzentrums Netzausfall ist ein Stromausfall Datencenter oder fehlerhafte Datacenter Netzwerk Switches. Ein Ausfall kann wenige Minuten einige Tage dauern.

Katastrophenfall ist definiert als eine Skalierungseinheit Service Bus oder Datacenter gehen unwiderruflich verloren. Das Rechenzentrum kann oder möglicherweise nicht verfügbar erneut. Normalerweise führt ein Notfall einige oder alle Nachrichten oder andere Daten. Beispiele für Notfälle sind Feuer, Überflutung oder Erdbeben.

## <a name="current-architecture"></a>Aktuelle Architektur

Service Bus verwendet mehrere messaging speichert Nachrichten gespeichert, die Themen zu Warteschlangen gesendet werden. Eine nicht partitionierte Warteschlange oder ein Thema ein Nachrichtenspeicher erhält. Wenn dieser Nachrichtenspeicher nicht verfügbar ist, werden alle Vorgänge in dieser Warteschlange oder ein Thema fehl.

Alle messaging Service Bus-Elemente (Warteschlangen, Themen Relays) befinden sich in einem Dienstnamespace mit einem Datencenter. Service Bus ermöglicht keine automatische Geo-Replikation der Daten und ermöglicht es einen Namespace mehrere Rechenzentren umfassen.

## <a name="protecting-against-acs-outages"></a>Schutz gegen Ausfälle ACS

Wenn Sie ACS Anmeldeinformationen verwenden und ACS verfügbar, können Clients nicht mehr Token abrufen. Kunden, die einen Token gleichzeitig ACS ausfällt können weiterhin Service Bus bis Token abläuft. Die token Standardgültigkeitsdauer beträgt drei Stunden.

Verwenden Sie zum Schutz vor Ausfällen ACS Shared Access Signatur (SAS)-Token. In diesem Fall authentifiziert der Client direkt mit Service Bus selbst geprägten Token mit einem geheimen Schlüssel signieren. Aufrufe von ACS sind nicht mehr erforderlich. Weitere Informationen zu SAS-Token finden Sie unter [Service Bus-Authentifizierung][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Warteschlangen und Themen für messaging Store-Fehler

Eine nicht partitionierte Warteschlange oder ein Thema ein Nachrichtenspeicher erhält. Wenn dieser Nachrichtenspeicher nicht verfügbar ist, werden alle Vorgänge in dieser Warteschlange oder ein Thema fehl. Auf der anderen Seite besteht eine partitionierte Warteschlange mehrere Fragmente. Jedes Fragment wird in eine andere messaging gespeichert. Wenn eine an eine partitionierte Warteschlange oder ein Thema Nachricht zugewiesen Service Bus Nachricht eines der Fragmente. Wenn die entsprechenden Nachrichtenspeicher nicht verfügbar ist, schreibt Service Bus die Meldung zu einem anderen Fragment möglichst ein. Weitere Informationen über partitionierte Entitäten finden Sie unter [messaging Entitäten partitioniert][].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Schutz vor Ausfällen Datacenter oder Katastrophen

Um ein Failover zwischen zwei Rechenzentren zu ermöglichen, können Sie einen Namespace Service Servicebus in jedem Datencenter erstellen. Beispielsweise Service Bus Service Namespace **contosoPrimary.servicebus.windows.net** kann im Bereich Norden/Mitte der USA, und **contosoSecondary.servicebus.windows.net** könnte uns Süd/zentrale Region befinden. Wenn eine Entität messaging Service Bus in einem Datacenter-Ausfall verfügbar bleiben muss, können Sie die Entität in beiden Namespaces erstellen.

Weitere Informationen finden Sie im Abschnitt "Fehler Servicebus in Azure-Rechenzentrum" [asynchronen messaging Mustern und hohe Verfügbarkeit][].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Relay-Endpunkte Datacenter Ausfälle oder Katastrophen schützen

Geo-Replikation Relay-Endpunkte ermöglicht einem Dienst, der einen Relay-Endpunkt in Service Bus Ausfälle erreichbar. Um georeplikation zu erreichen, muss der Dienst zwei relayendpunkte in verschiedenen Namespaces erstellen. Namespaces in verschiedenen Rechenzentren befinden muss, und die beiden Endpunkte müssen unterschiedliche Namen haben. Z. B. einen primären unter **contosoPrimary.servicebus.windows.net/myPrimaryService**erreichbar und sekundäre Gegenstück unter **contosoSecondary.servicebus.windows.net/mySecondaryService**.

Der Dienst überwacht dann beide Endpunkte und ein Client den Dienst über Endpunkten aufrufen kann. Eine Clientanwendung Relays als den primären wählt zufällig und sendet die Abfrage an den aktiven Endpunkt. Wenn fehl mit Fehler gibt Fehler Relay-Endpunkt ist nicht verfügbar. Die Anwendung öffnet einen Kanal zu backup Endpunkt und gibt die Anforderung erneut. An dieser Stelle der aktiven und backup Endpunkte Rollen wechseln: hält die Clientanwendung alten aktiven Endpunkt der neuen backup-Endpunkt und der alte backup Endpunkt der neuen aktiven Endpunkt. Wenn beide Vorgänge Fehler senden, die Rollen der beiden Entitäten bleiben unverändert und ein Fehler zurückgegeben.

Das [Geo-Replikation mit Service Bus Relay Nachrichten][] demonstriert Relays replizieren.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Warteschlangen und Themen Datacenter Ausfälle oder Notfälle

Erreichung Widerstandsfähigkeit gegen Datacenter Ausfälle bei Verwendung vermittelte messaging Service Bus unterstützt zwei Verfahren: *aktive* und *passive* Replikation. Jeder Ansatz eine Warteschlange oder ein Thema in einem Ausfall Datacenter zugänglich bleiben muss können Sie sie in beiden Namespaces erstellen. Beide Entitäten können den gleichen Namen haben. Beispielsweise eine primäre Warteschlange unter **contosoPrimary.servicebus.windows.net/myQueue**erreichbar und sekundäre Gegenstück unter **contosoSecondary.servicebus.windows.net/myQueue**.

Wenn die Anwendung keine ständige Verbindung zwischen Sender-Empfänger erforderlich, kann die Anwendung dauerhafte clientseitige Warteschlange Datenverlust verhindern und Schützen der Sender vorübergehende Service Bus-Fehler implementieren.

## <a name="active-replication"></a>Aktive Replikation

Aktive Replikation verwendet Entitäten in beiden Namespaces für jeden Vorgang. Jeder Client, der ein Signal sendet zwei Kopien derselben Nachricht. Die erste Kopie geht an die primäre Entität (z. B. **contosoPrimary.servicebus.windows.net/sales**) und die zweite Kopie der Nachricht an die sekundäre Entität (z. B. **contosoSecondary.servicebus.windows.net/sales**) gesendet.

Ein Client empfängt Nachrichten von beiden Warteschlangen. Der Empfänger verarbeitet die erste Kopie der Nachricht und die zweite Kopie wird unterdrückt. Um doppelte Nachrichten zu unterdrücken, muss der Absender jede Nachricht mit einem eindeutigen Bezeichner tag. Beide Kopien der Nachricht müssen mit derselben ID gekennzeichnet. Eigenschaften [BrokeredMessage.MessageId][] oder [BrokeredMessage.Label][] oder eine benutzerdefinierte Eigenschaft können Sie die Nachricht markieren. Der Empfänger müssen eine Liste von Nachrichten, die er bereits empfangen hat.

[Geo-Replikation mit Service Bus vermittelte Nachrichten][] demonstriert Replikation active messaging Entitäten.

> [AZURE.NOTE] Aktive Replikationsverfahren verdoppelt die Anzahl der Vorgänge, daher dadurch zu höheren Kosten führen kann.

## <a name="passive-replication"></a>Passive Replikation

Bei fehlerfrei verwendet passiven Replikation nur eines der beiden messaging Entitäten. Ein Client sendet die Nachricht an das aktive Element. Schlägt der Vorgang in der aktiven Entität einen Fehlercode, der angibt, dass das Rechenzentrum, das das aktive Element hostet möglicherweise nicht verfügbar, sendet der Client eine Kopie der Nachricht an die Sicherung Entität. Zu diesem Zeitpunkt aktive und backup Entitäten wechseln Rollen: der sendende Client betrachtet alte aktive Entität zu neuen backup-Entität und die alte Sicherung Entität ist die neue active. Wenn beide Vorgänge Fehler senden, die Rollen der beiden Entitäten bleiben unverändert und ein Fehler zurückgegeben.

Ein Client empfängt Nachrichten von beiden Warteschlangen. Ist eine Möglichkeit, dass der Empfänger zwei Kopien derselben Nachricht erhält, muss der Empfänger doppelte Nachrichten unterdrücken. Duplikate können genauso unterdrückt werden, wie active-Replikation.

Passive Replikation ist im Allgemeinen günstiger als aktive Replikation da in den meisten Fällen nur ein Vorgang ausgeführt wird. Latenz, Durchsatz und Kosten sind identisch mit dem Szenario nicht repliziert.

Bei passiver Verwendung in den folgenden Szenarien können zweimal Nachrichten verloren gehen oder empfangen werden:

-   **Nachricht verzögert oder Verlust**: davon aus, dass der Absender Nachricht m1 primäre Warteschlange gesendet und dann die Warteschlange steht bevor Empfänger m1 erhält. Der Absender sendet eine nachfolgende Meldung m2 sekundäre Warteschlange. Die primäre Warteschlange vorübergehend nicht verfügbar ist, erhält der Empfänger m1 nach die Warteschlange wieder zur Verfügung steht. Notfall erhalten der Empfänger nie m1.

-   **Doppelte Empfang**: davon aus, dass der Absender eine Nachricht m an die primäre Warteschlange sendet. Service Bus erfolgreich verarbeitet m aber keine Antwort gesendet. Nach der Sendevorgang, sendet der Absender eine identische Kopie von m der sekundären Warteschlange. Ist der Empfänger erhalten die erste Kopie von m vor die primäre Warteschlange nicht verfügbar ist, erhält der Empfänger beide Kopien der m ungefähr zur gleichen Zeit. Ist der Empfänger nicht empfangen die erste Kopie von m vor die primäre Warteschlange nicht verfügbar ist, der Empfänger erhält zunächst die zweite Kopie von m; erhält eine Kopie von m wird die primäre Warteschlange verfügbar

[Geo-Replikation mit Service Bus vermittelte Nachrichten][] demonstriert passiven Replikation Messaging Entitäten.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Wiederherstellung finden Sie in diesen Artikeln:

- [SQL Azure-Datenbank Business Continuity][]
- [Technische Anleitung Azure Stabilität][]

  [Service Bus Authentifizierung]: service-bus-authentication-and-authorization.md
  [Partitionierte messaging Entitäten]: service-bus-partitioning.md
  [Asynchrones messaging Muster und hohe Verfügbarkeit]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [Geo-Replikation mit Servicebus Nachrichten weitergeleitet]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [Geo-Replikation mit Servicebus vermittelte Nachrichten]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [SQL Azure-Datenbank Business Continuity]: ../sql-database/sql-database-business-continuity.md
  [Technische Anleitung Azure Stabilität]: ../resiliency/resiliency-technical-guidance.md
