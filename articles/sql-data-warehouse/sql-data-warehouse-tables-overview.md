<properties
   pageTitle="Übersicht über Tabellen im SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit Azure SQL Data Warehouse-Tabellen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Übersicht über Tabellen im SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

Einstieg in das Erstellen von Tabellen in SQL Data Warehouse ist einfach.  Die grundlegende Syntax [CREATE TABLE][] folgende allgemeine Syntax wahrscheinlich bereits vertraut mit anderen Datenbanken.  Um eine Tabelle zu erstellen, müssen Sie einfach nennen Sie Ihre Tabelle, Spalten benennen und Definieren von Datentypen für jede Spalte.  Wenn Sie haben Tabellen in anderen Datenbanken erstellen, sieht dies sehr vertraut.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Im obige Beispiel erstellt eine Tabelle namens Customers mit zwei Spalten Vorname und Nachname.  Jede Spalte wird mit einem VARCHAR(25), definiert, die Daten auf 25 Zeichen beschränkt.  Diese grundlegende Attribute einer Tabelle sowie andere sind meist mit anderen Datenbanken identisch.  Datentypen für jede Spalte definierten und Integrität Ihrer Daten.  Leistungssteigerung durch Reduzierung der e/a können Indizes hinzugefügt werden.  Partitionierung möglich zur Leistungssteigerung, wenn Sie Daten ändern.

[Umbenennen von] [ RENAME] SQL Data Warehouse-Tabelle sieht folgendermaßen aus:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Verteilte Tabellen

Eine grundlegende Attributs von verteilten Systemen wie SQL Data Warehouse die **verteilungsspalte**eingeführt.  Verteilungsspalte ist sehr klingt.  Es ist die Spalte, die bestimmt, wie verteilen oder Teilen, die Daten im Hintergrund.  Beim Erstellen einer Tabelle ohne Angabe der verteilungsspalte wird die Tabelle automatisch mit **Round Robin**verteilt.  Round-Robin-Tabellen können in einigen Fällen ausreichend sein, kann Verteilung Spalten definieren Daten Abfragen optimieren Leistung erheblich.  Finden Sie [eine Tabelle Verteilung][verteilen] Weitere Informationen zum auswählen eine verteilungsspalte.

## <a name="indexing-and-partitioning-tables"></a>Indizierung und Partitionierung von Tabellen

Werden erweiterte mit SQL Data Warehouse und Leistung optimieren möchten, sollten Sie Tabellenentwurf erfahren.  Finden Sie weitere Artikel [Datentypen Tabelle][Datentypen]und [eine Tabelle Verteilung][verteilen], [Indizieren einer Tabelle][Index] [Partitionieren einer Tabelle][Partition].

## <a name="table-statistics"></a>Tabellenstatistiken

Statistiken sind ein sehr wichtig, um die bestmögliche Leistung aus SQL Data Warehouse.  Da SQL Data Warehouse noch nicht automatisch erstellt und Statistiken aktualisieren Sie, wie Sie uns möglicherweise in Azure SQL-Datenbank lesen Artikel [Statistiken][] erwarten könnte eines der wichtigsten Artikel lesen Sie, um sicherzustellen, dass die Leistung von Abfragen zu verbessern.

## <a name="temporary-tables"></a>Temporäre Tabellen

Temporäre Tabellen sind Tabellen, die nur für die Dauer der Anmeldung vorhanden und können nicht von anderen Benutzern angezeigt werden.  Temporäre Tabellen können eine gute Möglichkeit zu verhindern, dass andere temporäre Ergebnisse auch Cleanup erforderlich sein.  Temporäre Tabellen auch lokalen Speicher verwenden, bieten sie für einige Vorgänge schneller.  Die [Temporärtabelle][temporäre] Artikel Weitere Informationen zu temporären Tabellen anzeigen

## <a name="external-tables"></a>Externe Tabellen

Externe Tabellen auch Polybase Tabellen sind Tabellen aus SQL Data Warehouse, zeigen Sie auf externe Daten aus SQL Data Warehouse abgefragt werden kann.  Beispielsweise können Sie auf Dateien auf Azure BLOB-Speicher verweist eine externe Tabelle erstellen.  Weitere Informationen zum Erstellen und Abfragen einer externen Tabelle finden Sie unter [Laden von Daten mit Polybase][].  

## <a name="unsupported-table-features"></a>Nicht unterstützte Tabellenfunktionen

Während SQL Data Warehouse viele andere Datenbanken Tabellen-Features enthält, sind einige Funktionen nicht unterstützt werden.  Es folgt eine Liste einiger Tabellen-Features nicht unterstützt werden.

| Nicht unterstützte Funktionen |
| --- |
|[Identity-Eigenschaft][] (siehe [Zuweisen von Ersatzzeichen Schlüssel umgehen][])|
|Primärschlüssel, Fremdschlüssel eindeutig und Check- [Tabellen-Integritätsregeln][]|
|[Eindeutige Indizes][]|
|[Berechnete Spalten][]|
|[SPARSE-Spalten][]|
|[Benutzerdefinierte Typen][]|
|[Sequenz][]|
|[Trigger][]|
|[Indizierte Sichten][]|
|[Synonyme][]|

## <a name="table-size-queries"></a>Größe Abfragen

Leerzeichen und Zeilen von einer Tabelle in einer 60-Distributionen verwendet eine einfache Möglichkeit ist die Verwendung von [DBCC PDW_SHOWSPACEUSED][].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Allerdings kann Befehle DBCC sehr begrenzt.  Dynamische Verwaltungsansichten (DMVs) können Sie viel mehr Details sichtbar sowie Sie viel größere Kontrolle über die Ergebnisse der Abfrage.  Zunächst erstellen dieser Ansicht zu viele Beispiele und weitere Artikel verwiesen wird.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Tablespace-Zusammenfassung

Diese Abfrage gibt Tabelle Zeilen und Leerzeichen.  Es ist eine große welche Tabellen die größten Tabellen sind und ob sie Round-Robin oder distributed Hash.  Für verteilte Hashtabellen wird auch die verteilungsspalte.  In den meisten Fällen sollte die größten Tabellen Hash mit einem gruppierten Columnstore verteilt.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Bereich von Verteilergruppen

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Bereich von Indextyp

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Verteilung Speicherplatz Zusammenfassung

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Artikel [Tabelle Datentypen][Datentypen] [eine Tabelle Verteilung][verteilen], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition], [Tabellenstatistik Verwalten von][Statistiken] und [Temporärtabellen][temporäre].  Weitere Informationen über bewährte Methoden finden Sie unter [SQL Data Warehouse Best Practices][].

<!--Image references-->

<!--Article references-->
[Übersicht]: ./sql-data-warehouse-tables-overview.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Temporäre]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Laden von Daten mit Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[TABELLE ERSTELLEN]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Identity-Eigenschaft]: https://msdn.microsoft.com/library/ms186775.aspx
[Zuweisen von Ersatzzeichen Schlüssel umgehen]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Tabellen-Integritätsregeln]: https://msdn.microsoft.com/library/ms188066.aspx
[Berechnete Spalten]: https://msdn.microsoft.com/library/ms186241.aspx
[SPARSE-Spalten]: https://msdn.microsoft.com/library/cc280604.aspx
[Benutzerdefinierte Typen]: https://msdn.microsoft.com/library/ms131694.aspx
[Sequenz]: https://msdn.microsoft.com/library/ff878091.aspx
[Trigger]: https://msdn.microsoft.com/library/ms189799.aspx
[Indizierte Sichten]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyme]: https://msdn.microsoft.com/library/ms177544.aspx
[Eindeutige Indizes]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
