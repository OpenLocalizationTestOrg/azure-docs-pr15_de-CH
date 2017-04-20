<properties
   pageTitle="Verwaltung Compute Azure SQL Data Warehouse (Übersicht) | Microsoft Azure"
   description="Performance-Skalierung in Azure SQL Data Warehouse-Funktionen. Skalierung von DWUs anpassen oder anhalten und Fortsetzen von computeressourcen, um Kosten zu sparen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Verwaltung Compute Azure SQL Data Warehouse (Übersicht)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

Die Architektur von SQL Data Warehouse trennt Speicher- und Compute jeweils unabhängig voneinander zu skalieren. Dadurch können Sie Performance und sparen Kosten nur bei Bedarf bezahlen Performance skalieren. 

Diese Übersicht beschreibt Dezentrales Skalieren der folgenden Leistungsfähigkeit SQL Data Warehouse und wie und wann sie gibt. 

- Skalierung rechenleistung mithilfe der [Data warehouse-Einheiten (DWUs)][]
- Anhalten oder Fortsetzen von Serverressourcen

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Skalierung von performance

SQL Data Warehouse können Sie schnell Leistung oder zurück durch erhöhen oder Verringern der Serverressourcen von CPU, Speicher und e/a-Bandbreite skalieren. Zum Skalieren der Leistung müssen Sie lediglich die Anzahl der [Data warehouse-Einheiten (DWUs)][] anpassen, die Ihre Datenbank SQL Data Warehouse zuweist. SQL Data Warehouse schnell die Änderung und behandelt alle zugrunde liegenden Änderungen an Hardware oder Software.

Vorbei sind die Zeiten, in denen Sie müssen welche Prozessoren, Arbeitsspeicher oder welchen Speicher Sie benötigen gute Leistung im Datawarehouse. Setzen Sie das Data Warehouse in der Cloud, Ihnen mehr mit Gerätestufe. Stattdessen SQL Data Warehouse fragt Sie: wie schnell möchten Sie Ihre Daten analysieren? 

### <a name="how-do-i-scale-performance"></a>Wie wird skalieren Leistung?

Elastisch erhöhen oder Verringern der Compute-Stromversorgung, einfach die Einstellung [Data warehouse-Einheiten (DWUs)][] für die Datenbank. Leistung erhöht linear Weitere DWU hinzufügen.  Höherer DWU müssen Sie 100 DWUs zu eine erhebliche Verbesserung der Leistung bemerken hinzufügen. Wählen Sie aussagekräftige springt DWUs helfen, bieten wir DWU, die besten Ergebnisse.
 
DWUs anpassen, können Sie die einzelnen Methoden.

- [Skalierung berechnen mit Azure-portal][]
- [Skalieren mit PowerShell berechnen][]
- [Skalierung Compute-Leistung mit REST-APIs][]
- [Skalieren mit TSQL berechnen][]

### <a name="how-many-dwus-should-i-use"></a>Wie viele DWUs sollte ich verwenden?
 
Performance im SQL Data Warehouse linear skaliert und in Sekunden zufällig aus einem Compute in anderen (z. B. von 100 DWUs auf 2000 DWUs) ändern. Dies gibt Ihnen Flexibilität mit verschiedenen DWU experimentieren, bis Ihr Szenario optimale Breite bestimmen.

Wissen, was der ideale DWU-Wert ist, versuchen Skalierung nach oben und unten und einige Abfragen nach dem Laden der Daten ausführen. Da Skalierung schnell, probieren Sie eine Anzahl verschiedener Leistungsstufen innerhalb einer Stunde oder weniger. Bedenken, dass SQL Data Warehouse große Datenmengen verarbeiten und echten-Funktionen für die Skalierung konzipiert ist, insbesondere bei größeren Maßstäben bieten wir wollen mit einer großen Datenmenge erreicht oder überschreitet 1 TB.

Empfohlene best DWU für Ihre Arbeitslast zu finden:

1. Für ein Datawarehouse Entwicklung zunächst eine kleine Anzahl von DWUs auswählen.  Ein guter Ausgangspunkt ist DW400 oder DW200.
2. Überwachen Sie die Leistung Ihrer Anwendung, wobei die Anzahl der ausgewählten DWUs verglichen mit der Leistung, die Sie beobachten.
3. Bestimmen Sie, wie viel schneller oder langsamer Leistung für Sie optimale Leistung für Ihre Bedürfnisse erreichen von linearen Maßstab angenommen werden soll.
4. Erhöhen Sie oder verringern Sie die Anzahl der DWUs im Verhältnis wie viel schneller oder langsamer Workload ausführen möchten. Der Dienst schnell und Datenverarbeitungsressourcen neue DWU Anforderungen anpassen.
5. Weiterhin Anpassungen bis eine optimale Leistung bei Bedarf erreicht.

### <a name="when-should-i-scale-dwus"></a>Wann sollten DWUs skalieren?

Wenn Sie schneller, erhöhen die DWUs und bezahlen für mehr Leistung.  Beim benötigen weniger Strom zu berechnen, die DWUs verringern und nur bezahlen, was Sie brauchen. 

Empfehlung bei DWUs skalieren:

1. Anzupassen Sie ist Ihre Anwendung schwankende Arbeitslast skalieren, DWU, Ebenen, oder auf Spitzen und niedrigen Punkte. Beispielsweise wenn Ihre Arbeitslast in der Regel am Ende des Monats Gipfel, möchten Sie mehr DWUs Peak damals und dann nach dem Spitzenwert Ablauf verkleinern.
2. Bevor Sie ein großer Datenmengen laden oder Transformation durchführen, skalieren Sie DWUs, damit die Daten schneller verfügbar ist.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pause compute

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Verwenden Sie zum Anhalten einer Datenbank eine einzelne Methoden.

- [Pause Compute mit Azure-portal][]
- [Pause Compute mit PowerShell][]
- [Pause Compute mit REST-APIs][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Resume compute

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Um eine Datenbank wieder aufzunehmen, verwenden Sie diese einzelnen Methoden.

- [Resume Compute mit Azure-portal][]
- [Resume Compute mit PowerShell][]
- [Resume Compute mit REST-APIs][]

## <a name="permissions"></a>Berechtigungen

Skalierung der Datenbank ist in [ALTER DATABASE][]beschriebenen Berechtigungen erforderlich.  Anhalten und fortsetzen erfordert [SQL DB Contributor][] Berechtigung speziell Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Finden Sie in den folgenden Artikeln um einige zusätzliche Leistungskennzahlen Konzepte verstehen:

- [Verwaltung von Arbeitslast und Parallelität][]
- [Tabelle entwerfen (Übersicht)][]
- [Tabellenverteilung][]
- [Tabelle indizieren][]
- [Tabellenpartitionierung][]
- [Tabellenstatistiken][]
- [Bewährte Methoden][]

<!--Image reference-->

<!--Article references-->
[Data warehouse-Einheiten (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Skalierung berechnen mit Azure-portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Skalieren mit PowerShell berechnen]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Skalierung Compute-Leistung mit REST-APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Skalieren mit TSQL berechnen]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause Compute mit Azure-portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause Compute mit PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause Compute mit REST-APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume Compute mit Azure-portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume Compute mit PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume Compute mit REST-APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Verwaltung von Arbeitslast und Parallelität]: ./sql-data-warehouse-develop-concurrency.md
[Tabelle entwerfen (Übersicht)]: ./sql-data-warehouse-tables-overview.md
[Tabellenverteilung]: ./sql-data-warehouse-tables-distribute.md
[Tabelle indizieren]: ./sql-data-warehouse-tables-index.md
[Tabellenpartitionierung]: ./sql-data-warehouse-tables-partition.md
[Tabellenstatistiken]: ./sql-data-warehouse-tables-statistics.md
[Bewährte Methoden]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
