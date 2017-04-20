Erstellen Sie mehrere Dienste in einem Abonnement jeweils auf einer bestimmten Ebene durch die erlaubte Anzahl von Diensten auf jeder Ebene nur begrenzt bereitgestellt. Beispielsweise könnte bis zu 12 grundlegende Ebene und anderen 12 Dienstleistungen S1 Ebene innerhalb des gleichen Abonnements erstellen. Weitere Informationen zu Ebenen finden Sie unter [Auswählen einer SKU oder Tier Azure Suche](../articles/search/search-sku-tier.md).

Maximale Grenzwerte können auf Anforderung ausgelöst. Support von Azure benötigen Sie weitere Dienste innerhalb des gleichen Abonnements.

Ressource|Frei|Grundlegende|S1|S2|S3 |S3 HD <sup>1</sup>
---|---|---|---|----|---|----
Maximale services |1 |12 |12  |6 |6 |6 
Maximale Skalierung SU <sup>2</sup>|N/v <sup>3</sup>|3 SU <sup>4</sup> |36 SU|36 SU|36 SU|12 SU 3 SU <sup>5</sup>

<sup>1</sup> S3 HD unterstützt keine [Indexer](../articles/search/search-indexer-overview.md) zu diesem Zeitpunkt. 

<sup>2</sup> suchen (SU) werden berechenbarer Einheiten pro Dienst als ein *Replikat* oder eine *Partition*zugeordnet. Beide Ressourcen erforderlich für Speicher, Indizierung und Abfrageoperationen. Um weitere Informationen über gültige Kombinationen, die unter die Höchstbeträge bleiben Siehe [Skalieren Ressource für Abfrage- und Arbeitslasten](../articles/search/search-capacity-planning.md). 

<sup>3</sup> frei basiert auf freigegebene Ressourcen von mehreren Abonnenten verwendet. Auf dieser Ebene sind keine Ressourcen für eine einzelne Abonnenten. Aus diesem Grund ist höchste als nicht anwendbar gekennzeichnet.

<sup>4</sup> Basic hat eine feste Partition. Auf dieser Stufe werden zusätzliche SUs zum mehrere Replikate für erhöhte Abfrage Arbeitslasten zuordnen.

<sup>5</sup> S3 HD hat eine unterschiedliche Allocation-Struktur als zulässige Kombinationen. Sie können maximal 12, Replikate. Für Partitionen ist maximal 3.




