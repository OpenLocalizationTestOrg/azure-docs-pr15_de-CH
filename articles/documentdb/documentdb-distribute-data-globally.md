<properties
   pageTitle="Daten mit DocumentDB | Microsoft Azure"
   description="Enthält Informationen Sie zu weltweit operierenden Geo-Replikation, Failover und Recovery mit globaler Datenbanken von Azure DocumentDB vollständig verwaltete NoSQL-Datenbankdienst."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Daten Sie mit DocumentDB

> [AZURE.NOTE] Globales DocumentDB Datenbanken ist allgemein verfügbar und für alle neu erstellten DocumentDB-Konten automatisch aktiviert. Wir arbeiten daran, globale Verteilerlisten für alle vorhandenen Konten aktivieren, aber in der Zwischenzeit sollten globale Verteilerlisten für Ihr Konto aktiviert [wenden](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , und wir werden aktivieren sie Sie jetzt.

Azure DocumentDB soll Bedürfnisse IoT Applikationen mit Millionen von weltweit verteilten Geräte und Skalierung Internetprogramme, die Benutzer weltweit reaktionsfähiges Erlebnis bereitzustellen. Diese Datenbanksysteme Herausforderung Latenz Zugriff auf Daten aus mehreren Regionen mit klar definierten Daten Konsistenz und Verfügbarkeit erreichen. Als Datenbanksystem Global verteilten vereinfacht DocumentDB die globale Verbreitung von Daten mit vollständig verwaltete Datenbank mit mehreren Konten, die klar Kompromisse zwischen Konsistenz, Verfügbarkeit und Leistung mit entsprechenden Garantien bieten. DocumentDB Datenbankkonten Angeboten mit hoher Verfügbarkeit einstelligen ms Wartezeiten mehrere [konsistenzebenen definierten] [consistency], transparente regionale Failover mit Multi-homing APIs und elastisch Durchsatz und Speicher weltweit skalieren. 

  
## <a name="configuring-multi-region-accounts"></a>Konfiguration mit mehreren Konten

Konfigurieren Ihr Konto DocumentDB weltweit skalieren kann in weniger als einer Minute [Azure-Portal](documentdb-portal-global-replication.md)erfolgen. Müssen Sie lediglich wählen Sie die Konsistenz zwischen mehreren unterstützten definierten Konsistenz und Ihr Konto eine beliebige Anzahl von Azure Regionen zugeordnet. DocumentDB bieten Konsistenz klare Kompromisse zwischen bestimmten Konsistenz gewährleistet und Leistung. 

![DocumentDB bietet mehrere definiert auch Konsistenz (relaxed) Modelle][1]

DocumentDB bietet mehrere (relaxed) Konsistenz Modelle gut definiert.

Die richtige Auswahl hängt Daten gewährleisten Konsistenz Bedarf der Anwendung. DocumentDB automatisch repliziert Daten in allen angegebenen Regionen und gewährleistet die Konsistenz für Ihr Konto ausgewählt haben. 


## <a name="using-multi-region-failover"></a>Verwenden der Failover-Regionen 

Azure DocumentDB kann transparent Failover Datenbankkonten Regionen mehrere Azure – neue [Multi-homing APIs] [ developingwithmultipleregions] gewährleistet, dass Ihre app kann weiterhin einen logischen Endpunkt und durch das Failover ist. Failover von Ihnen gesteuert, bieten die Flexibilität, Ihre Datenbank Stammservers Konto bei einem Bereich von möglichen Fehlerzustände auftreten Anwendung, Infrastruktur, Service oder regionale Ausfälle (echten oder simulierten). Im Fehlerfall regionalen DocumentDB der Dienst nicht transparent über Ihr Konto und Ihre Anwendung Datenzugriff ohne Verfügbarkeit weiter. DocumentDB [Verfügbarkeit von 99,99 % SLAs]bietet[sla], testen die Anwendung Ende Verfügbarkeitseigenschaften von einem regionalen Stromausfall, [programmgesteuert] simulieren[ arm] sowie der Azure-Portal.


## <a name="scaling-across-the-planet"></a>Weltweit skalieren
DocumentDB können Sie unabhängige Durchsatz bereitstellen und verbrauchen Speicher für jede Auflistung DocumentDB jeder Ebene Global in allen Regionen, die Ihrem Konto zugeordnet. Eine DocumentDB Auflistung automatisch Global verteilt und verwaltet alle Bereiche, die Ihrem Konto zugeordnet. Sammlungen in Ihrem Konto können auf Azure Regionen in der verteilt [DocumentDB Service steht][serviceregions]. 

Durchsatz erworben und Speicher für jede Auflistung DocumentDB belegt wird automatisch bereitgestellt in allen Regionen gleichermaßen. Dadurch kann die Anwendung eine nahtlose Skalierung über Welt [für Durchsatz und Speicher in jeder Stunde verwendeten Zahlen][pricing]. Beispielsweise wenn 2 Millionen RUs für eine DocumentDB Auflistung bereitgestellt haben, erhält jeder der Regionen Ihr Konto zugeordnet 2 Millionen RUs für diese Auflistung. Dies ist unten dargestellt.

![Skalierung Durchsatz für eine DocumentDB Auflistung in vier Regionen][2]

DocumentDB garantiert < 10 ms Lesen und < 15 ms Schreibwartezeiten bei P99. Die Leseanfragen umfassen nie Datacenter Grenze garantieren [konsistenzanforderungen gewählte][consistency]. Die Schreibvorgänge sind immer Quorum lokal begangen werden, bevor sie Clients bestätigt werden. Jedes Konto ist mit Schreibzugriff Region Priorität konfiguriert. Der Bereich mit der höchsten Priorität bestimmt fungiert als der aktuelle Bereich Schreiben für das Konto. Alle SDKs leitet transparent Konto Schreibvorgänge in den aktuellen Bereich schreiben. 

Schließlich DocumentDB vollständig [unabhängig Schema] ist[ vldb] -nie Schemata verwalten/aktualisieren oder sekundäre Indizes für mehrere Rechenzentren kümmern müssen. [SQL-Abfragen] [ sqlqueries] weiterarbeiten, während Ihre Anwendung und Daten Modelle weiterhin. 


## <a name="enabling-global-distribution"></a>Globales aktivieren 

Sie können Ihre Daten lokal oder Global verteilten entweder zuordnen mindestens Azure Regionen mit einer DocumentDB Datenbank-Konto. Sie können hinzufügen oder Entfernen von Bereichen zu Ihrem Konto jederzeit. Globale Verteilerlisten mit dem Portal finden Sie [DocumentDB globale Datenbank-Replikation mithilfe von Azure-Portal durchführen](documentdb-portal-global-replication.md). Globales programmgesteuert finden Sie [Entwickeln mit mit mehreren DocumentDB](documentdb-developing-with-multiple-regions.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über das Verteilen von Daten mit DocumentDB in den folgenden Artikeln:

* [Bereitstellung von Durchsatz und Speicher für eine Auflistung] [throughputandstorage]
* [Multi-homing APIs über REST. .NET, Java, Python und Knoten SDKs] [developingwithmultipleregions]
* [Konsistenzebenen in DocumentDB] [consistency]
* [Verfügbarkeit SLAs] [sla]
* [Konto verwalten] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

