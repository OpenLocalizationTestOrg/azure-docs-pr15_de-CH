<properties
   pageTitle="Temporäre Tabellen im SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit temporären Tabellen in Azure SQL Data Warehouse."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Temporäre Tabellen im SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

Temporäre Tabellen sind sehr nützlich, wenn Daten - vor allem bei der Transformation, die Zwischenergebnisse vorübergehend. Im SQL Data Warehouse vorhanden temporäre Tabellen auf Sitzungsebene.  Sie sind nur für die Sitzung, in der sie erstellt wurden, und werden automatisch gelöscht, wenn die Sitzung abmeldet.  Temporäre Tabellen bieten einen Leistungsvorteil, da die Ergebnisse als Remotespeicher lokale geschrieben werden.  Temporäre Tabellen sind geringfügig Azure SQL Data Warehouse Azure SQL-Datenbank, wie sie von überall innerhalb der Sitzung einschließlich innerhalb und außerhalb einer gespeicherten Prozedur zugegriffen werden können.

Dieser Artikel enthält wichtige Hinweise zur Verwendung von temporären Tabellen und hebt die Grundsätze der Sitzung auf temporäre Tabellen. Mithilfe der Informationen in diesem Artikel helfen modularisiert Code, Wiederverwendung und Wartung des Codes zu verbessern.

## <a name="create-a-temporary-table"></a>Erstellen Sie eine temporäre Tabelle

Temporäre Tabellen werden erstellt, indem einfach die Tabellennamen voranstellen einer `#`.  Zum Beispiel:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Temporäre Tabellen können auch erstellt werden, mit einem `CTAS` genau die gleiche Weise:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`ist ein sehr leistungsfähiger Befehl und bietet den zusätzlichen Vorteil der effizient in seiner Verwendung des Transaktionsprotokolls. 


## <a name="dropping-temporary-tables"></a>Löschen von temporären Tabellen

Wenn eine neue Sitzung erstellt wird, sollten keine temporären Tabellen vorhanden.  Jedoch wenn Sie dieselbe gespeicherte Prozedur erzeugt ein temporäres mit demselben Namen aufrufen, um sicherzustellen, dass Ihre `CREATE TABLE` sind erfolgreich eine einfache Überprüfung vor Existenz mit einer `DROP` einsetzbar als die unter Beispiel:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Kodierung Konsistenz ist es empfehlenswert, dieses Muster für Tabellen und temporäre Tabellen verwenden.  Es ist auch eine gute Idee, verwenden `DROP TABLE` temporäre Tabellen entfernen, wenn sie in Ihrem Code abgeschlossen ist.  In gespeicherten Prozedur Entwicklung ist es üblich, finden Sie unter Drop-Befehle am Ende der Prozedur, die diese Objekte sicherzustellen gebündelt werden bereinigt.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularisierung code

Da temporäre Tabellen in einer Stelle angezeigt werden, kann dies helfen Anwendungscode modularisiert ausgenutzt werden.  Beispielsweise bringt die folgende gespeicherte Prozedur die empfohlenen Vorgehensweisen von oben DDL generieren von Statistiknamen wird alle Statistiken in der Datenbank aktualisiert.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

In dieser Phase ist die einzige Aktion, wurde die Erstellung einer gespeicherten Prozedur die einfach erstellt eine temporäre Tabelle #Stats_ddl mit DDL-Anweisungen.  Diese gespeicherte Prozedur wird #Stats_ddl löschen, wenn er bereits vorhanden ist, um sicherzustellen, dass er nicht fehlschlägt, wenn innerhalb einer Sitzung mehr als einmal ausführen.  Allerdings gibt es keine `DROP TABLE` am Ende der Prozedur nach Abschluss die gespeicherte Prozedur beibehalten erstellte Tabelle, damit sie außerhalb der gespeicherten Prozedur gelesen werden kann.  SQL Data Warehouse kann im Gegensatz zu anderen SQL Server-Datenbanken, die temporäre Tabelle außerhalb der Prozedur verwenden, die es erstellt.  Temporäre Tabellen SQL Data Warehouse können verwendete **überall** innerhalb der Sitzung sein. Können dadurch mehr modular und verwaltbaren Code wie in dem Beispiel unten:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Temporäre Tabelle Grenzen

SQL Data Warehouse erlegt ein paar Nachteile beim Implementieren von temporärer Tabellen.  Derzeit werden nur bewertete temporäre Tabellen unterstützt.  Globale Temporärtabellen werden nicht unterstützt.  Darüber hinaus können Sichten für temporäre Tabellen erstellt werden.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Artikel auf [Tabelle][Übersicht], [Tabelle Datentypen][Datentypen] [eine Tabelle Verteilung][verteilen], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition] und [Wartung Tabellenstatistik][Statistiken].  Weitere Informationen über bewährte Methoden finden Sie unter [SQL Data Warehouse Best Practices][].

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

<!--MSDN references-->

<!--Other Web references-->
