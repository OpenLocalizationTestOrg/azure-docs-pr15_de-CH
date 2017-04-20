<properties
    pageTitle="SQL-Datenbank: Was ist ein DTU? | Microsoft Azure"
    description="Welche einer Azure SQL-Datenbank versteht Buchungseinheit."
    keywords="Datenbankoptionen, Datenbank-performance"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Erläutert Datenbank Transaktion (DTUs) und elastische Datenbank Transaktion (eDTUs)

Dieser Artikel beschreibt die Datenbank Transaktionen (DTUs) und elastische Datenbank Buchungseinheiten (eDTUs) und was geschieht, wenn Sie die maximale DTUs oder eDTUs.  

## <a name="what-are-database-transaction-units-dtus"></a>Welche Datenbank Buchungseinheiten (DTUs)

Eine DTU ist eine Einheit der Ressourcen, die eine eigenständige SQL Azure-Datenbank Ebene Leistung innerhalb einer [eigenständigen Datenbank Dienstebene](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels)verfügbar. Ein DTU ist ein blended Maß CPU, Speicher und Daten-e/a Transaction Log e/a-in einer durch eine OLTP-Benchmark-Arbeitslast der realen OLTP-Arbeitslasten werden bestimmt. Verdoppelung der DTUs durch Erhöhung der Leistung einer Datenbank entspricht verdoppelt festgelegten Ressourcen Datenbank. Beispielsweise bietet eine Datenbank Premium P11 1750 DTUs 350 x DTU mehr Strom als eine einfache Datenbank mit 5 DTUs berechnen. Die Methodik hinter der OLTP-Benchmark-Workload DTU Blend bestimmt finden Sie unter [SQL-Datenbank Benchmark-Übersicht](sql-database-benchmark-overview.md).

![Einführung in SQL-Datenbank: einzelne Datenbank DTUs Tier und](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Sie können [Ändern](sql-database-scale-up.md) , Dienstebenen mit minimaler Ausfallzeit der Anwendung (im Allgemeinen durchschnittlich weniger als vier Sekunden). Für viele Unternehmen und apps ist Datenbanken erstellen und wählen Datenbank-Performance bei Bedarf nach oben oder unten, besonders wenn Verwendungsmuster vorhersehbaren. Aber haben Sie unvorhersehbare Verwendungsmuster, es kann machen es schwer zu Kosten und Ihr Geschäftsmodell. In diesem Szenario verwenden Sie elastischen Pool mit einer bestimmten Anzahl von eDTUs.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Was sind elastische Datenbank Buchungseinheiten (eDTUs)

Ein eDTU ist eine Einheit der Gruppe von Ressourcen (DTUs) zwischen einer Reihe von Datenbanken auf einem SQL Azure-Server - Namens einer [elastischen Pool](sql-database-elastic-pool.png)freigegeben werden können. Elastische Pools stellen eine einfache kostengünstige Lösung zum Verwalten der Leistungsziele für mehrere Datenbanken mit unterschiedlichen und unvorhersehbaren Verwendungsmuster. Finden Sie die [elastische Pools und Dienstebenen](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) .

![Einführung in SQL-Datenbank: eDTUs Tier und](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Ein Pool erhält eine bestimmte Anzahl von eDTUs für einen festgelegten Preis. Innerhalb des Pools einzelne Datenbanken Flexibilität automatische Skalierung in festgelegten Parameter erhalten. Stark ausgelastet kann eine Datenbank weitere eDTUs Bedarf beanspruchen. Datenbanken bei hellen verbrauchen weniger und Datenbanken keine Belastung verbrauchen keine eDTUs. Bereitstellen von Ressourcen für das gesamte Pool nicht für einzelne Datenbanken vereinfacht Ihre. Außerdem haben Sie einem vorhersagbaren für den Pool.

Weitere eDTUs können zu einem vorhandenen Pool mit keine Datenbank Ausfallzeiten oder Datenbanken im elastischen Pool hinzugefügt werden. Entsprechend mehr benötigt zusätzliche eDTUs können sie aus einem vorhandenen Pool jederzeit Zeitpunkt entfernt werden. Hinzufügen oder abziehen Datenbanken zum Pool. Wenn eine Datenbank erwartungsgemäß unter-verwendet Ressourcen verschieben.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Wie kann die Anzahl der DTUs von meinem Arbeitslast benötigt feststellen?

Wenn Sie migrieren auf eine vorhandene lokale oder SQL Server virtuellen Arbeitslast in Azure SQL-Datenbank können Sie [DTU Rechner](http://dtucalculator.azurewebsites.net/) ungefähre Anzahl der erforderlichen DTUs. [SQL Datenbank Abfrage Performance Insight](sql-database-query-performance.md) können Sie für eine vorhandene Azure SQL-Datenbank-Arbeitslast um Ihre Datenbank Ressourcenverbrauch (DTUs) Einblick in Ihre Arbeit optimieren zu verstehen. [Dm_db_ Resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV können Sie die Informationen für eine Stunde Verbrauch abrufen. Alternativ können Katalog anzeigen [resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) auch abgefragt werden um die gleichen Daten der letzten 14 Tage zwar am unteren Treue fünf Minuten Durchschnitt.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Wie erkenne ich aus einer elastischen Ressourcen profitieren?

Pools eignen sich für eine große Anzahl von Datenbanken mit bestimmten Muster. Für eine bestimmte Datenbank dieses Muster niedrigen durchschnittlichen Auslastung mit relativ selten Verwendung Spitzen charakterisiert. SQL-Datenbank automatisch wertet die Verlaufsdaten Ressourcenverwendung Datenbanken in einer vorhandenen SQL-Datenbankserver und empfiehlt die entsprechenden Pool-Konfiguration im Azure-Portal. Weitere Informationen finden Sie unter [Wenn sollte ein Datenbankpool elastische verwendet werden?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Was passiert, wenn ich meine maximale DTUs erreicht

Kalibriert sind und unterliegt zum Ausführen der Arbeitslast bis max. Rahmen für die ausgewählten Tier-Performance auf die benötigten Ressourcen bereitstellen. Wenn Workload Grenzen eines CPU-Daten IO-Protokoll IO Grenzen stößt, wahrscheinlich erhöhte Wartezeiten für Abfragen können Sie weiterhin die Ressourcen auf die maximale Stufe Solange die Verlangsamung so schwerwiegend, dass Abfragen Timing beginnen führen diese Grenzwerte nicht Fehler, sondern vielmehr eine Verlangsamung der Auslastung. Wenn Sie Grenzwerte der maximal zulässigen gleichzeitigen Benutzer Sessions/Anfragen (Arbeitsthreads) treffen, explizite Fehler angezeigt. Siehe [Ressourcengrenzen Azure SQL-Datenbank](sql-database-resource-limits.md) Informationen auf Ressourcen als CPU, Arbeitsspeicher, e/a Daten und Transaction Log-e/a.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu DTUs und eDTUs für eigenständige Datenbanken und elastische Pools finden Sie unter [Service-Tier](sql-database-service-tiers.md) .
- Siehe [Ressourcengrenzen Azure SQL-Datenbank](sql-database-resource-limits.md) Informationen auf Ressourcen als CPU, Arbeitsspeicher, e/a Daten und Transaction Log-e/a.
- [SQL Datenbank Abfrage Leistung Einblick](sql-database-query-performance.md) zu verstehen, den Verbrauch (DTUs) anzeigen
- Finden Sie [SQL Datenbank Benchmark-Übersicht](sql-database-benchmark-overview.md) verständlichen Methodik hinter der OLTP-Benchmark-Workload Blend DTU bestimmt.