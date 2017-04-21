Datenbank Transaktion Einheit (DTU) ist die Maßeinheit in SQL-Datenbank, die die relative macht Datenbanken basierend auf eine reale darstellt: die Datenbanktransaktion. Eine Reihe von Vorgängen, die typisch für ein online Transaction processing (OLTP) Anforderung und messen, wie viele Transaktionen pro Sekunde unter vollständig abgeschlossen werden konnte Startbedingungen geladen haben (die Kurzversion ist, komplizierten Details im [Benchmark-Übersicht](../articles/sql-database/sql-database-benchmark-overview.md)lesen). 

Beispielsweise bietet eine Datenbank Premium P11 1750 DTUs 350 x DTU mehr Strom als eine einfache Datenbank mit 5 DTUs berechnen. 

![Einführung in SQL-Datenbank: einzelne DTUs Tier und Datenbank.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Wenn Sie eine vorhandene SQL Server-Datenbank migrieren, können Sie ein Drittanbietertool [Azure SQL Datenbank DTU Rechner](http://dtucalculator.azurewebsites.net/)schätzen die Leistungsfähigkeit und service-Stufe, die Ihre Datenbank in Azure SQL-Datenbank benötigen.

### <a name="dtu-vs-edtu"></a>DTU und eDTU

Die DTU für einzelne Datenbanken übersetzt eDTU elastische Datenbanken an. Eine Datenbank in einem Pool elastische Datenbank bietet bis zu 5 eDTUs. Das ist die gleiche Leistung wie eine einzelne einfache Datenbank. Der Unterschied ist, dass elastische Datenbank wird keine eDTUs aus dem Pool verwenden, erst zu. 

![Einführung in SQL-Datenbank: elastische Pools Tier.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Ein Beispiel hilft. Einen Pool elastische Datenbank mit 1000 DTUs nehmen und 800 Datenbanken einlassen. Als nur 200 800 Datenbanken jederzeit verwendet werden rechtzeitig (5 DTU X 200 = 1000) wird nicht Sie Kapazität des Pools erreicht und datenbankleistung wird nicht beeinträchtigt. In diesem Beispiel wird aus Gründen der Übersichtlichkeit vereinfacht. Echte Mathematik ist etwas komplizierter. Das Portal wird Mathematik für Sie und eine Empfehlung Verlaufsdatenbank Verwendung abhängig. [Preis- und Hinweise für einen Datenbankpool elastische](../articles/sql-database/sql-database-elastic-pool-guidance.md) Funktionsweise Empfehlung oder selbst dafür anzeigen 
