<properties 
    pageTitle="Ereignis-Hubs häufig gestellte Fragen (FAQ) | Microsoft Azure"
    description="Ereignis-Hubs FAQ."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Ereignis-Hubs FAQ

Ereignis Hubs bietet umfangreiche Aufnahme, Dauerhaftigkeit und Verarbeitung der Ereignisse von hohem Durchsatz Datenquellen oder Millionen von Geräten. Mit Servicebuswarteschlangen und Themen können Ereignis-Hubs persistente und Installationen von [Internet der Dinge (IoT)](https://azure.microsoft.com/services/iot-hub/) Szenarien.

Dieser Artikel beschreibt Preisinformationen und beantwortet einige häufig gestellten Fragen zum Ereignis Hubs:

## <a name="pricing-information"></a>Preisinformationen

Vollständige Informationen zum Ereignis Hubs Preise [Ereignis Hubs Preisdetails](https://azure.microsoft.com/pricing/details/event-hubs/)anzeigen

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Berechnung von Event Hubs eingehende Ereignisse

Jedes Ereignis Event Hub gesendet zählt als berechenbare Nachricht. *Eingehende Ereignis* ist definiert als eine Dateneinheit, die kleiner oder gleich 64 KB. Jedes Ereignis, das kleiner als oder gleich 64 KB Größe gilt eine kostenpflichtig. Das Ereignis größer als 64 KB ist, wird die Anzahl der berechenbaren Ereignisse nach Ereignisgröße ein Vielfaches von 64 KB berechnet. Beispielsweise wird ein 8 KB-Ereignis Event Hub gesendet als ein Ereignis abgerechnet jedoch eine 96 KB Nachricht an Event Hub als zwei Ereignisse abgerechnet.

Ereignisse aus einem Ereignis-Hub als verbraucht sowie Verwaltungsvorgänge und wie Checkpoints steuern, zählen nicht als berechenbare eingehende Ereignisse jedoch fällig bis Durchsatz Einheit Vergütung.

## <a name="what-are-event-hubs-throughput-units"></a>Was sind Event Hubs durchsatzeinheiten?

Sie wählen explizit Event Hubs durchsatzeinheiten, Azure-Portal oder Ereignis Hubs Resource Manager Vorlagen. Durchsatzeinheiten gelten für alle Ereignis-Hubs in einem Ereignis Hubs und Stück Durchsatz berechtigt den Namespace mit folgenden Funktionen:

- Ruft auf 1 MB pro Sekunde eingehende Ereignisse Ereignisse in ein Ereignis gesendet, jedoch keine 1000 eingehende Ereignisse Verwaltungsvorgänge und Control API pro Sekunde.

- Bis zu 2 MB pro Sekunde Ausgang Ereignisse Ereignisse aus einem Ereignis-Hub verwendet.

- Bis zu 84 GB Ereignis Speicher (ausreichend für die Standardarchivierungszeit 24 Stunden).

Ereignis Hubs durchsatzeinheiten werden stündlich, berechnet anhand der Höchstanzahl der Einheiten während der angegebenen Stunden ausgewählt.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Wie werden Event Hubs Durchsatz Einheit Grenzwerte durchgesetzt?

Überschreitet Durchsatz insgesamt eindringen oder die gesamte eingehende Ereignis über alle Ereignis-Hubs in einem Namespace Gesamtdurchsatz Einheit Zertifikate, Absender gedrosselt werden und möglicherweise Fehlermeldungen angezeigt, dass Eindringen Kontingent überschritten wurde.

Überschreitet der gesamten Ausgang Durchsatz oder die Veranstaltung Ausgang über alle Ereignis-Hubs in einem Namespace Gesamtdurchsatz Einheit Zertifikate, Empfänger gedrosselt werden und Fehlermeldungen, dass Egress-Kontingent überschritten wurde. Ingress- und Egress Kontingente werden separat erzwungen, sodass kein Absender Ereignis Verbrauch verlangsamt kann und ein Empfänger kann Ereignisse in ein Ereignis gesendet wird.

Beachten Sie, dass die Durchsatz Auswahl unabhängig von der Anzahl der Ereignis-Hubs Partitionen. Jede Partition bietet maximalen Durchsatz von 1 MB pro zweite Eindringen (mit maximal 1000 Ereignisse pro Sekunde) und 2 MB pro zweiten Ausgang kostenfrei feste Partitionen selbst. Die Gebühr ist für die aggregierte Durchsatz auf alle Ereignis-Hubs in einem Ereignis Hubs. Mit diesem Muster erstellen Sie genug Partitionen unterstützen die erwartete maximale Belastung für ihre Systeme ohne Gebühren Einheit Durchsatz bis Ereignis Belastung des Systems tatsächlich erfordert höhere Durchsatzzahlen ohne Belastung des Systems ändern die Struktur und Architektur Ihres Systems erhöht.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Besteht eine Begrenzung für die Anzahl der durchsatzeinheiten, die ausgewählt werden können?

Es wird ein Kontingent von 20 durchsatzeinheiten pro Namespace. Sie können ein größeres Kontingent durchsatzeinheiten Einreichung von Support-Ticket anfordern. Überschritten Einheit 20 Durchsatz stehen Bündel in 20 und 100 Durchsatz. Beachten Sie, dass die Durchsatz Stückzahl ändern, ohne ein Support-Ticket mit mehr als 20 Durchsatz entfernt werden.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Gibt es eine Gebühr für Event Hubs Ereignisse für mehr als 24 Stunden beibehalten?

Ereignis-Hubs Standard Tier lässt Nachricht Aufbewahrung länger als 24 Stunden maximal 30 Tage. Überschreitet die Größe die Gesamtzahl der gespeicherten Ereignisse Speicher Aufmaß für die Anzahl der ausgewählten durchsatzeinheiten (84 GB pro Durchsatz), wird die Größe, die die Vergütung überschreitet die veröffentlichten Azure BLOB-Speicher berechnet. Storage Vergütung pro Einheit Durchsatz deckt alle Speicher für Aufbewahrungsfristen von 24 Stunden (Standardwert) selbst wenn durchsatzeinheit auf die maximale Eindringen verwendet wird.

## <a name="what-is-the-maximum-retention-period"></a>Was ist die maximale Beibehaltungsdauer?

Ereignis-Hubs Standard Tier unterstützt derzeit maximale Beibehaltungsdauer beträgt 7 Tage. Beachten Sie, dass Ereignis Hubs nicht als dauerhaften Datenspeicher sind. Aufbewahrungszeiten größer als 24 Stunden in dem es einfach wiedergeben ein Streams von Ereignissen in denselben Systemen ist für Szenarien vorgesehen sind; z. B. oder einen neuen Computer Lernmodell auf vorhandene Daten zu überprüfen.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Wie Ereignis Hubs Speichergröße berechnet und erhoben?

Die Gesamtgröße aller gespeicherten Ereignisse einschließlich Interner Aufwand Ereignisheader oder Datenträger Speicherstrukturen in alle Ereignis-Hubs wird im Laufe des Tages gemessen. Am Ende des Tages wird die maximale Speichergröße berechnet. Das Tagegeld Speicher auf die minimale Anzahl der durchsatzeinheiten errechnet, die während des Tages ausgewählt wurden (jeweils Durchsatz bietet eine Vergütung in Höhe von 84 GB). Übersteigt die Größe berechneten Tagegeld Speicher wird der zusätzliche Speicher Azure BLOB-Speicher (Rate **Lokal redundanter Speicher** ) mit abgerechnet.

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Kann ich verwenden eine AMQP Verbindung senden und Empfangen von Ereignis-Hubs und Service Bus Warteschlangen/Themen?

Ja, solange das Ereignis Hubs, Warteschlangen und Themen im gleichen Namespace. So können Sie bidirektionale, vermittelte Verbindung zu viele Geräte mit atemanhalten Wartezeiten kostengünstige und hochgradig skalierbare implementieren.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Gelten vermittelten Verbindungsgebühren Ereignis Hubs?

Absender beantragen Verbindungsgebühren Wenn das AMQP-Protokoll verwendet wird. Es fallen keine Verbindung zum Senden von Ereignissen über HTTP unabhängig von der Anzahl der Systeme oder Geräte senden. Möchten Sie AMQP (z. B. zu effizienter Ereignis streaming oder bidirektionale Kommunikation und Szenarien IoT) verwenden, finden Sie auf der Seite [Service Bus Preisinformationen](https://azure.microsoft.com/pricing/details/service-bus/) Informationen was vermittelte Verbindung, und wie sie gemessen werden.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Was ist der Unterschied zwischen Ereignis Hubs Basic und Standard Ebenen?

Ereignis Hubs Standard-Stufe bietet über das Ereignis Hubs Basic und einige vergleichbare Systeme verfügbar. Dazu gehören Aufbewahrungszeiten länger als 24 Stunden und die Möglichkeit, eine Verbindung AMQP Befehle an eine große Anzahl von Geräten mit atemanhalten Wartezeiten senden sowie Telemetrie von diesen Geräten zu Ereignis senden. Liste der Features finden Sie unter [Event Hubs Preisangaben](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="geographic-availability"></a>Geografische Verfügbarkeit

Event Hubs ist in den folgenden Bereichen:

|Geo|Regionen|
|---|---|
|USA|USA OST US USA 2 OST, Süd USA, Westen der USA|
|Europa|Nordeuropa, Westeuropa|
|Asien/Pazifik|Ostasien, Südostasien|
|Japan|Japan, Japan West|
|Brazilien|Brasilien Süd|
|Australien|Australien Osten, Australien Südost|

## <a name="support-and-sla"></a>Support und SLA

Support für Event Hubs ist über den [Community-Foren](https://social.msdn.microsoft.com/forums/azure/home). Abrechnung und Abonnement wird-Management nicht unterstützt.

Erfahren Sie mehr über unsere SLA, anzeigen Sie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/) Seite

## <a name="next-steps"></a>Nächste Schritte

Um weitere Informationen zu Ereignis-Hubs finden Sie in den folgenden Artikeln:

- [Ereignis-Hubs Overview][].
- Eine vollständige [Anwendung mit Event Hubs][].

[Übersicht über Hubs]: event-hubs-overview.md
[beispielanwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
