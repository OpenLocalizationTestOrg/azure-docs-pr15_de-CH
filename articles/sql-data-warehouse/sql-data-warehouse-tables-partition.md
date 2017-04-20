<properties
   pageTitle="Partitionierung von Tabellen im SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit der Tabellenpartitionierung in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Partitionierung von Tabellen im SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

Partitionierung auf allen SQL Data Warehouse Tabelle unterstützt. einschließlich gruppierter Columnstore, gruppierten Index und Heap.  Partitionierung wird auch auf alle Verteilungstypen einschließlich Hash oder verteilte Roundrobin unterstützt.  Partitionierung können Sie Ihre Daten in kleinere Gruppen von Daten und in den meisten Fällen Partitionierung unterteilen eine Datumsspalte erfolgt.

## <a name="benefits-of-partitioning"></a>Vorteile der Partitionierung

Partitionierung profitieren Daten Wartung und Abfrage-Leistung.  Ob beide oder nur eine Vorteile hängt wie Daten geladen werden und ob die gleiche Spalte für beide Zwecke verwendet werden kann, da Partitionierung auf eine Spalte erfolgt.

### <a name="benefits-to-loads"></a>Vorteile der Lasten

Der Hauptvorteil einer Partitionierung in SQL Data Warehouse ist verbessern Effizienz und Leistung laden Daten der Partition löschen mithilfe wechseln und zusammenführen.  In den meisten Fällen werden Daten auf eine Datumsspalte aufgeteilt, die die Sequenz eng ist die Daten in die Datenbank geladen wird.  Einer der größten Vorteile von Partitionen Daten verwalten sie jeden Transaktionslog.  Zwar einfach einfügen, aktualisieren oder Löschen von Daten der unkomplizierteste Ansatz Gedanken und Aufwand kann kann durch Partitionierung, während der Ladevorgang erheblich verbessern.

Partition wechseln kann schnell entfernen oder Ersetzen einen Teil einer Tabelle verwendet werden.  Beispielsweise kann eine Tabelle Verkaufsfakten nur Daten der letzten 36 Monate enthalten.  Am Ende jedes Monats wird der älteste Monat Daten aus der Tabelle gelöscht.  Diese Daten konnte die Daten für den ältesten Monat löschen mit einer Delete-Anweisung gelöscht werden.  Jedoch kann eine große Menge von Daten Zeile für Zeile mit einer Delete-Anweisung löschen sehr lange dauern, sowie das Risiko große Transaktionen lange Rollback dauern kann, wenn etwas schief geht.  Ein optimaler Ansatz wird einfach die älteste Partition Daten fallen.  Wo die einzelnen Zeilen löschen Stunden dauern kann durch Löschen einer gesamten Partition Sekunden dauern.

### <a name="benefits-to-queries"></a>Vorteile für Abfragen

Partitionieren kann auch verwendet werden, um die Abfrageperformance verbessern.  Wenn eine Abfrage einen Filter für eine partitionierte Spalte bezieht, begrenzen dieser Scan nur qualifizierte Partitionen möglicherweise eine viel kleinere Teilmenge der Daten einen vollständigen Tabellenscan vermeiden.  Mit der Einführung des Columnstore gruppierter Indizes Prädikat Beseitigung Leistungsvorteile sind weniger nützlich, aber in einigen Fällen kann ein Vorteil Abfragen.  Z. B. wenn die Umsatzfaktentabelle in 36 Monate mit Feld Verkaufsdatum aufgeteilt und fragt dann den Filter auf dem Verkaufsdatum überspringen Suchen in Partitionen, die nicht gefiltert.

## <a name="partition-sizing-guidance"></a>Partition Größe Anleitung

Während der Partitionierung dienen einige Szenarien verbessern, kann **viele** Partitionen erstellen Leistung unter Umständen Schaden.  Diese Probleme sind besonders für gruppierte Columnstore Tabellen.  Für die Partitionierung hilfreich sein, ist es wichtig, Partitionierung verwenden und die Anzahl der zu erstellenden Partitionen.  Es ist keine Festplatte Regel, wie viele Partitionen zu viele, hängt Ihre Daten und wie viele Partitionen gleichzeitig zu laden sind.  Aber als allgemeine Faustregel 10 s, 100 s Partitionen nicht 1. 000ern hinzufügen.

Erstellung **gruppierter Columnstore** Tabellen partitionieren ist es wichtig zu berücksichtigen, wie viele Zeilen in jeder Partition landet.  Für optimale Komprimierung und Leistung von gruppierten Columnstore Tabellen ist mindestens 1 Million Zeilen pro Verteilung und Partition erforderlich.  Vor der Erstellung von Partitionen unterteilt SQL Data Warehouse jede Tabelle bereits in 60 verteilten Datenbanken.  Partitionierung einer Tabelle hinzugefügt wird neben der Verteilung im Hintergrund erstellt.  Verwenden in diesem Beispiel enthalten die Umsatzfaktentabelle 36 monatliche Partitionen und SQL Data Warehouse 60 Distributionen hat, dann die Umsatzfaktentabelle sollte 60 Millionen pro Monat, 2.1 Milliarden Zeilen oder alle Monate aufgefüllt wurden.  Wenn eine Tabelle deutlich weniger Zeilen als die empfohlene Mindestanzahl von Zeilen pro Partition enthält, sollten Sie weniger Partitionen zu erhöhen Sie die Anzahl der Zeilen pro Partition.  Außerdem finden Sie den [Indizierung][Index] Abfragen enthält, die auf die Qualität der Cluster Columnstore Indizes SQL Data Warehouse ausgeführt werden können.

## <a name="syntax-difference-from-sql-server"></a>Syntax Differenz von SQL Server

SQL Data Warehouse stellt eine vereinfachte Definition für Partitionen geringfügig von SQL Server ist.  Partitionierung Funktionen und Programme werden nicht im SQL Data Warehouse verwendet, sind in SQL Server.  Stattdessen müssen Sie lediglich partitionierte Spalte und die Berandung bestimmen.  Die Syntax der Partitionierung geringfügig von SQL Server möglicherweise stimmen die grundlegenden Konzepte.  SQL Server und SQL Data Warehouse unterstützen eine Partitionsspalte pro Tabelle ausgeschöpft Partition.  Finden Sie weitere Partitionierung [partitionierte Tabellen und Indizes][].

Die unter Beispiel für eine Anweisung SQL Data Warehouse partitionierte [Tabelle erstellen][] Partitionen die Tabelle FactInternetSales OrderDateKey Spalte:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migrieren von SQL Server-Partitionierung

SQL Data Warehouse einfach SQL Server Partitionsdefinitionen migrieren:

- Vermeiden Sie das SQL Server- [Partitionsschema][].
- Fügen Sie die [Partitionsfunktion][] Definition zur Tabelle erstellen.

Wenn Sie eine partitionierte Tabelle aus einer SQL Server-Instanz migrieren die unter SQL können Sie die Anzahl der Zeilen, die in jeder Partition abgefragt.  Denken Sie daran, wenn die gleiche Partitionierung Granularität SQL Data Warehouse verwendet wird, wird die Anzahl der Zeilen pro Partition Faktor 60 verringern.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Arbeitslast-management

Ein letzter Aspekt der Tabelle Partition Entscheidung Faktor ist [Arbeitslast-Management][].  Arbeitslast-Management im SQL Data Warehouse wird hauptsächlich die Verwaltung und Parallelität.  Im SQL Data Warehouse ist der maximale Arbeitsspeicher zu jeder während der Abfrage gesteuerten Ressourcenklassen.  Im Idealfall werden Partitionen unter Berücksichtigung anderer Faktoren wie Speicherbedarf Columnstore gruppierter Indizes erstellen angepasst.  Gruppierte Columnstore Indizes nutzen erheblich, wenn sie mehr Speicher zugewiesen werden.  Daher sollten Sie sicherstellen, dass die Partition Index Rebuild Speicher nicht verhungern ist. Erhöhung der Arbeitsspeicher für die Abfrage lässt sich durch den Wechsel von Standardrolle "smallrc", zu anderen Rollen wie Largerc.

Informationen über die Zuweisung von Speicher pro Verteilung wird Abfragen Resource Governor dynamische Ansichten verfügbar. In Wirklichkeit wird der Speicher gewähren die Zahlen unten unterschreiten. Allerdings bietet dies eine Anleitung, die Größe von Partitionen für Datenverwaltungsvorgänge verwendet werden können.  Versuchen Sie zu Partitionen über das Abgangsgeld Speicher durch die Klasse extra große Größe. Wenn diese Zahl Partitionen übersteigen riskieren Sie Speicherdruck weniger optimale Komprimierung führt.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Partition wechseln

SQL Data Warehouse unterstützt Partition aufteilen und Zusammenführen von wechseln. Jede dieser Funktionen wird mit der Anweisung [ALTER TABLE][] Excuted.

Partitionen zwischen zwei Tabellen wechseln müssen Sie sicherstellen, die Partitionen auf ihre jeweiligen Grenzen ausgerichtet und die Definitionen übereinstimmen. Da Prüf-Integritätsregeln nicht den Wertebereich in einer Tabelle erzwingen muss Quelltabelle als Zieltabelle derselben Partitionsgrenzen enthalten. Ist dies nicht der Fall, wird der Partition Switch fehlschlagen, da Partition Metadaten nicht synchronisiert werden.

### <a name="how-to-split-a-partition-that-contains-data"></a>Aufteilen eine Partition, die Daten enthält

Die effizienteste Methode eine Partition aufteilen, die bereits Daten enthält, ist die Verwendung einer `CTAS` Anweisung. Ist die partitionierte Tabelle ein gruppierter Columnstore muss dann Partition Table leer sein, bevor es geteilt werden kann.

Es folgt ein Beispiel Columnstore partitionierten Tabelle mit einer Zeile in jeder Partition:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] Statistik erstellen, stellen wir sicher, dass diese Tabellenmetadaten genauer ist. Wenn wir statistische Angaben weglassen, verwenden SQL Data Warehouse Standardwerte. Informationen über Statistiken finden Sie [Statistiken][].

Wir können dann Abfragen der Zeile Anzahl mit dem `sys.partitions` Katalog anzeigen:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Wenn wir versuchen, diese Tabelle teilen, erhalten wir einen Fehler:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346, Schweregrad 15, Status 1, Zeile 44 AUFGETEILT-Klausel der Anweisung ALTER PARTITION ist fehlgeschlagen, da die Partition nicht leer ist.  Leere Partitionen können in aufgeteilt werden, wenn ein Columnstore Index für die Tabelle vorhanden ist. Sollten Sie den Columnstore-Index vor der PARTITION ALTER-Anweisung und erneutes Erstellen des Indexes Columnstore nach Abschluss der PARTITION ändern deaktivieren.

Wir können jedoch `CTAS` zum Erstellen einer neuen Tabelle zum Speichern von Daten.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Wie die Partitionsgrenzen ausgerichtet werden darf ein Switch. Dadurch verbleibt die Quelltabelle mit einer leeren Partition, die anschließend lässt.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Noch dazu ist unsere Daten auf die neue Partition mit `CTAS` und die Daten wieder in die Haupttabelle

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Nach Abschluss die Verschiebung der Daten ist eine gute Idee, aktualisieren die Statistiken für die Zieltabelle um sicherzustellen, dass sie die neue Verteilung der Daten in die entsprechenden Partitionen widerspiegeln:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabellenpartitionierung Datenquellen-Steuerelement

Um die Tabellendefinition aus **Rost** im Quellcodeverwaltungssystem zu vermeiden sollten Sie die folgenden Ansatz:

1. Erstellen Sie die Tabelle eine partitionierte Tabelle als Partition Werte

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`die Tabelle als Teil der Bereitstellung:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Dadurch bleibt des Codes im Datenquellen-Steuerelement statisch und Partitionierung Begrenzungswerte können dynamisch sein; mit dem Warehouse entwickelt mit der Zeit.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Artikel auf [Tabelle][Übersicht], [Tabelle Datentypen][Datentypen] [eine Tabelle Verteilung][verteilen], [Indizieren einer Tabelle][Index], [Tabellenstatistik Verwalten von][Statistiken] und [Temporärtabellen][temporäre].  Weitere Informationen über bewährte Methoden finden Sie unter [SQL Data Warehouse Best Practices][].

<!--Image references-->

<!--Article references-->
[Übersicht]: ./sql-data-warehouse-tables-overview.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Temporäre]: ./sql-data-warehouse-tables-temporary.md
[Arbeitslast-management]: ./sql-data-warehouse-develop-concurrency.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Partitionierte Tabellen und Indizes]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[TABELLE ERSTELLEN]: https://msdn.microsoft.com/library/mt203953.aspx
[Partition-Funktion]: https://msdn.microsoft.com/library/ms187802.aspx
[Partitionsschema]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
