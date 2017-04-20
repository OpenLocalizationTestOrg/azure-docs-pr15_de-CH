<properties
   pageTitle="Verwalten von Statistiken für Tabellen im SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit Statistiken für Tabellen in Azure SQL Data Warehouse."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Verwalten von Statistiken für Tabellen im SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

Je mehr Daten SQL Data Warehouse kennt, schneller können sie Abfragen für die Daten ausführen.  Die Möglichkeit, Ihre Daten SQL Data Warehouse erzählen ist Statistiken über Ihre Daten.  Statistiken ist für Ihre Daten eines der wichtigsten Dinge, die Sie tun können, um Abfragen zu optimieren.  Statistiken können SQL Data Warehouse den optimalen Plan für Abfragen erstellen.  Ist der Abfrageoptimierer SQL Data Warehouse Kosten Basis-Optimierer ist.  D. h. vergleicht die Kosten der verschiedenen Abfragepläne und wählt dann den Plan mit den niedrigsten Kosten, die auch den Plan, der die schnellste ausgeführt wird.

Statistiken können für eine einzelne Spalte mehrere Spalten oder einem Index einer Tabelle erstellt werden.  Statistiken werden in einem Histogramm den Bereich und die Selektivität der Werte erfasst gespeichert.  Dies ist von besonderem Interesse, wenn muss der Optimierer auswerten JOINs, GROUP BY, HAVING und WHERE-Klauseln in einer Abfrage.  Beispielsweise, wenn der Optimierer Zurückgeben des Datums in der Abfrage gefiltert 1 Zeile, sie können sehr unterschiedliche als If-plan schätzt, dass sie bisher ausgewählt haben wird 1 Million Zeilen zurück.  Beim Erstellen von Statistiken sehr wichtig ist, ist es wichtig, dass Statistiken *genau* entsprechen den aktuellen Zustand der Tabelle.  Aktuelle Statistiken wird sichergestellt, dass gut vom Optimierer ausgewählt wird.  Der Optimierer erstellt Pläne sind nur so gut wie statistischen Daten.

Der Prozess des Erstellens und Aktualisieren der Statistiken ist derzeit ein manueller Prozess, aber ist sehr einfach.  Dies ist im Gegensatz zu SQL Server automatisch erstellt und aktualisiert die Statistik für die einzelnen Spalten und Indizes.  Mithilfe der folgenden Informationen können Sie die Verwaltung der Statistiken erheblich Daten automatisieren. 

## <a name="getting-started-with-statistics"></a>Erste Schritte mit Statistiken

 Aufgenommene Statistik für jede Spalte erstellen ist leicht mit beginnen.  Ist unumstritten Stand Statistiken möglicherweise ein konservativer Ansatz täglich oder nach jedem Laden Statistiken aktualisieren. Es gibt immer Kompromisse zwischen Leistung und die Kosten für Statistiken erstellen und aktualisieren.  Sie finden alle Statistiken zu lange dauert, sollten Sie versuchen, präziser Spalten Statistiken haben oder welche Spalten häufig aktualisiert werden müssen.  Sie möchten z. B. täglich, neue Werte hinzugefügt werden können statt nach jedem Ladevorgang Datumsspalten aktualisieren. In diesem Fall erhalten Sie den größten Nutzen mit Statistiken für Spalten in JOINs, GROUP BY, HAVING und WHERE-Klauseln.  Wenn Sie eine Tabelle mit vielen Spalten, die nur in der SELECT-Klausel verwendet werden haben, Statistiken für diese Spalten können nicht und Ausgaben ein wenig zu identifizieren, in denen Statistiken hilft, Spalten reduzieren die Zeit der Statistiken.

## <a name="multi-column-statistics"></a>Mehrspaltigen Statistik

Statistiken für einzelne Spalten, sondern möglicherweise mehrere Spaltenstatistiken Abfragen profitieren.  Mehrspaltige sind Statistiken für eine Liste mit Spalten erstellt.  Werden einzelne Spaltenstatistiken für die erste Spalte in der Liste und einige Cross-Spalte Korrelationsinformationen Dichte aufgerufen.  Beispielsweise haben Sie eine Tabelle, die auf zwei Spalten hinzufügt, möglicherweise SQL Data Warehouse besser Plan optimieren können, wenn die Beziehung zwischen zwei Spalten versteht.   Mehrspaltige Statistiken können für einige Vorgänge wie composite-Joins und Gruppieren nach steigern.

## <a name="updating-statistics"></a>Aktualisieren von Statistiken

Aktualisieren von Statistiken ist ein wichtiger Teil der Datenbank-Management-Routine.  Die Verteilung der Daten in der Datenbank ändert, wenn Statistiken aktualisiert werden.  Veraltete Statistiken werden keine optimale Leistung führen.

Eine empfiehlt Datumsspalten täglich Aktualisieren der Statistiken in neue Daten hinzugefügt werden.  Jedes Mal neue Zeilen in das Datawarehouse geladen werden, werden neue laden Daten oder Buchungsdaten hinzugefügt. Diese Verteilung der Daten zu ändern und die Statistiken veraltet. Umgekehrt müssen Statistik Country-Spalte in einer Kundentabelle aktualisiert werden, wie die Verteilung von Werten in der Regel ändert. Vorausgesetzt, die Verteilung zwischen Kunden, wird neue Zeilen zu der Tabelle hinzugefügt nicht die Daten ändern. Allerdings müssen Wenn Datawarehouse nur einem Land enthält und Sie Daten in ein neues Land, Daten in mehreren Ländern gespeichert, Sie definitiv Statistiken über die Country-Spalte zu aktualisieren.

Eine der ersten Fragen bei der Problembehandlung einer Abfrage ist "Aktuelle Statistiken sind?"

Diese Frage ist nicht das Alter der Daten beantwortet werden können. Aktualisiert Statistik werden sehr alte wurde keine wesentliche Änderung der zugrunde liegenden Daten. Wenn die Anzahl der Zeilen erheblich geändert oder eine wesentliche Änderung in der Verteilung von Werten für eine bestimmte Spalte *dann* es Zeit Statistiken aktualisieren.  

Referenz aktualisiert **SQL Server** (nicht SQL Data Warehouse) automatisch Statistiken für diese Situationen:

- Haben Sie keine Zeilen in der Tabelle Zeilen hinzufügen, erhalten Sie eine automatische Aktualisierung der Statistiken
- Wenn Sie eine Tabelle mit weniger als 500 Zeilen 500 Zeilen hinzufügen (z. B. am Anfang Sie 499 und 500 Zeilen auf 999 Zeilen hinzufügen), erhalten Sie ein automatisches Update 
- Wenn Sie mehr als 500 Zeilen 500 zusätzliche Zeilen + 20 % der Größe der Tabelle hinzufügen sind, bevor Sie ein automatisches Update in der Statistik sehen müssen

Da gibt es keine DMV bestimmen, ob Daten in der Tabelle geändert hat, seit die letzten Statistiken aktualisiert wurden, bieten das Alter Ihrer Statistik wissen Teil der Grafik Ihnen.  Können Sie die folgende Abfrage zuletzt Statistiken ermitteln wo für jede Tabelle aktualisiert.  

> [AZURE.NOTE] Denken Sie daran, ist eine wesentliche Änderung in der Verteilung von Werten für eine bestimmte Spalte, sollten Sie unabhängig von der letzten Statistiken aktualisieren, aktualisiert wurden.  

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Datenspalten in einem Datawarehouse müssen z. B. normalerweise Statistik häufig. Jedes Mal neue Zeilen in das Datawarehouse geladen werden, werden neue laden Daten oder Buchungsdaten hinzugefügt. Diese Verteilung der Daten zu ändern und die Statistiken veraltet.  Umgekehrt müssen Statistiken für eine Spalte Geschlecht Tabelle nicht aktualisiert werden. Vorausgesetzt, die Verteilung zwischen Kunden, wird neue Zeilen zu der Tabelle hinzugefügt nicht die Daten ändern. Wenn das Datawarehouse nur ein einziges Geschlecht und eine neue Anforderung in mehrere Geschlechter enthält müssen Sie definitiv Aktualisieren der Statistiken in der Spalte Geschlecht.

Weitere Hinweise finden Sie unter [Statistics][] auf MSDN.

## <a name="implementing-statistics-management"></a>Implementierung von Statistiken management

Es ist häufig sinnvoll, erweitern Ihre Daten laden um sicherzustellen, dass am Ende des Ladevorgangs Statistiken aktualisiert werden. Laden von Daten wird häufig die Größe oder die Verteilung der Werte Tabellen ändern. Daher ist dies ein logischer Ausgangspunkt für einige Prozesse implementieren.

Einige Leitlinien sind unten zum Aktualisieren von Statistiken während des Ladevorgangs:

- Sicherstellen Sie, dass jede geladene Tabelle mindestens eine Statistik aktualisiert hat. Die Tabellen (Zeilenanzahl und Seitenzahl) Informationen als Teil der Aktualisierung Statistiken aktualisiert.
- Schwerpunkt auf Spalten JOIN, GROUP BY, ORDER BY und DISTINCT-Klausel
- Sollten Sie aktualisieren "Aufsteigend Schlüssel" Spalten, z. B. Datumsangaben, da diese Werte nicht in dem Histogramm Statistik berücksichtigt werden.
- Sollten Sie statische Verteilung Spalten häufig aktualisieren.
- Beachten Sie, dass jedes Objekt Statistik Reihe aktualisiert wird. Einfach implementieren `UPDATE STATISTICS <TABLE_NAME>` möglicherweise nicht ideal - insbesondere bei breiten Tabellen mit vielen Objekten Statistiken.

> [AZURE.NOTE] Weitere Informationen zu [aufsteigend Schlüssel] finden Sie SQL Server 2014 Kardinalität Schätzung Modell Whitepaper.

Weitere Hinweise finden Sie im MSDN [Kardinalität Schätzung][] .

## <a name="examples-create-statistics"></a>Beispiele: Create statistics

Diese Beispiele veranschaulichen verschiedene Optionen zum Erstellen von Statistiken verwendet. Die Optionen für jede Spalte hängt die Merkmale der Daten und wie die Spalte in Abfragen verwendet werden.

### <a name="a-create-single-column-statistics-with-default-options"></a>A. Spalte Statistik mit Standardoptionen erstellen

Geben Sie einen Namen für die Statistik und den Namen der Spalte, um Statistiken für eine Spalte zu erstellen.

Diese Syntax verwendet alle Standardoptionen. Standardmäßig nimmt SQL Data Warehouse 20 Prozent der Tabelle Statistik verwendet werden.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Zum Beispiel:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Erstellen Sie Spalte Statistik anhand jeder Zeile

Die standardmäßige Sampling-Rate von 20 Prozent ist in den meisten Fällen ausreichend. Allerdings können Sie die Sampling-Rate anpassen.

Um vollständige Tabelle aufnehmen möchten, verwenden Sie folgende Syntax:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Zum Beispiel:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a>C. Erstellen Sie Spalte Statistik, indem die Größe der Stichprobe

Alternativ können Sie die Größe der Stichprobe als Prozentwert angeben:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a>D. Nur einige Zeilen erstellen Sie einer Spalte Statistiken

Eine weitere Option können Sie Statistiken für einen Teil der Zeilen in der Tabelle erstellen. Dies ist eine gefilterte Statistik bezeichnet.

Beispielsweise können Sie gefilterte Statistiken eine bestimmte Partition einer großen partitionierten Tabelle abgefragt werden soll. Statistiken auf nur die Partition erstellen, wird die Genauigkeit der Statistiken verbessern und somit die Leistung von Abfragen verbessern.

Dieses Beispiel erstellt eine Statistik auf einen Wertebereich. Die Werte können einfach entsprechend den Wertebereich in einer Partition definiert werden.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [AZURE.NOTE] Der Abfrageoptimierer gefilterte Statistiken verwenden, wenn es die verteilte Abfrage auswählt muss die Abfrage in der Definition der Statistik passen. Verwenden im vorherige Beispiel der Abfrage Klausel benötigt col1 Werte zwischen 2000101 und 20001231.

### <a name="e-create-single-column-statistics-with-all-the-options"></a>E. Erstellen Sie Spalte Statistik mit allen Optionen

Sie können natürlich die Optionen kombinieren. Im folgenden Beispiel erstellt eine gefilterte Statistik mit einem benutzerdefinierten:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Die vollständige Referenz finden Sie unter [CREATE STATISTICS][] auf MSDN.

### <a name="f-create-multi-column-statistics"></a>F. Mehrspaltige Statistiken erstellen

Erstellen einer mehrspaltigen Statistik einfach den vorherigen Beispielen, aber mehr Spalten angeben.

> [AZURE.NOTE] Das Histogramm mit der Anzahl der Zeilen im Abfrageergebnis schätzen, steht nur für die erste Spalte in der Objektdefinition Statistik aufgeführt.

In diesem Beispiel wird das Histogramm auf *Produkt\_Kategorie*. Cross-Spaltenstatistiken anhand *Produkt\_Kategorie* und *Produkte\_Sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Da es eine Korrelation zwischen *Produkt\_Kategorie* und *Produkt\_Sub\_Kategorie*, mehrspaltige Statistik kann nützlich sein, wenn diese Spalten gleichzeitig zugegriffen werden.

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a>G. Erstellen Sie Statistiken für alle Spalten in einer Tabelle

Eine Möglichkeit zum Erstellen von Statistiken werden Probleme CREATE STATISTICS Befehle nach dem Erstellen der Tabelle.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>H. Verwenden Sie eine gespeicherte Prozedur, um Statistiken für alle Spalten in einer Datenbank erstellen

SQL Data Warehouse verfügt eine gespeicherte Systemprozedur [Sp_create_stats] [] entspricht nicht in SQL Server. Diese gespeicherte Prozedur erstellt eine einzelne Spalte Statistik für jede Spalte der Datenbank, die Statistiken noch nicht.

Dadurch Datenbankentwurf Einstieg. Sie können Ihren Bedürfnissen anpassen.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN   sys.[external_tables] e ON  e.[object_id]       = t.[object_id]
WHERE       l.[object_id] IS NULL
AND         e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Um Statistiken für alle Spalten in der Tabelle mit diesem Verfahren erstellen, rufen Sie die Prozedur.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Beispiele: Update statistics

Sie können zum Aktualisieren der Statistiken:

1. Eine Statistik zu aktualisieren. Geben Sie den Namen der Statistik an, die Sie aktualisieren möchten.
2. Aktualisierung aller statistikobjekte in einer Tabelle. Geben Sie den Namen der Tabelle statt einer bestimmten Statistik.


### <a name="a-update-one-specific-statistics-object"></a>A. Aktualisieren einer bestimmten Statistik ###
Verwenden Sie die folgende Syntax eine bestimmte Statistik aktualisieren:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Zum Beispiel:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Bestimmte statistikobjekte aktualisieren, können Sie Zeit- und Ressourcenaufwand Statistiken zu minimieren. Dies erfordert einige Gedanken, jedoch die optimale Statistiken aktualisieren Objektauswahl.


### <a name="b-update-all-statistics-on-a-table"></a>B. Aktualisieren Sie aller Statistiken für eine Tabelle ###
Zeigt eine einfache Methode zum Aktualisieren der statistikobjekte in einer Tabelle.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Zum Beispiel:

```sql
UPDATE STATISTICS dbo.table1;
```

Dies ist einfach zu verwenden. Denken Sie dies alle Statistiken für die Tabelle aktualisiert und daher möglicherweise arbeiten mehr als notwendig ist. Wenn die Leistung nicht geht, ist dies definitiv einfachste und umfassendste Möglichkeit, Statistiken auf dem neuesten Stand sind.

> [AZURE.NOTE] Aktualisieren aller Statistiken für eine Tabelle enthält SQL Data Warehouse einen Scan der Tabelle für jede Statistik Beispiel. Wenn die Tabelle groß ist, viele Spalten und Statistiken hat möglicherweise effizienter, je nach Bedarf einzelne Statistiken aktualisieren.

Eine Implementierung einer `UPDATE STATISTICS` Prozedur finden Sie unter[temporäre] Artikel [Temporärtabellen]. Die Implementierungsmethode ist etwas anders als die `CREATE STATISTICS` beschriebenen, aber das Ergebnis ist identisch.

Die vollständige Syntax finden Sie unter [Update Statistics][] auf MSDN.

## <a name="statistics-metadata"></a>Statistiken-Metadaten
Gibt es mehrere Systemansicht und Funktionen, mit denen Sie Informationen zu Statistiken. Beispielsweise können Sie finden eine Statistik möglicherweise veraltete Statistiken Datum Funktion sehen Statistik zuletzt erstellt oder aktualisiert wurden.

### <a name="catalog-views-for-statistics"></a>Katalogsichten Statistiken
Die Systemansichten Informationen Statistiken:

| Katalog anzeigen | Beschreibung |
| :----------- | :---------- |
| [Sys.Columns][]  | Eine Zeile für jede Spalte. |
| [Sys.Objects][]  | Eine Zeile für jedes Objekt in der Datenbank. |  |
| [Sys.Schemas][]  | Eine Zeile für jedes Schema in der Datenbank. |  |
| [Sys.Stats][] | Eine Zeile für jede Statistik. |
| [Sys.stats_columns][] | Eine Zeile für jede Spalte in der Statistik. Links zu sys.columns. |
| [' sys.Tables '][] | Eine Zeile für jede Tabelle (einschließlich externe Tabellen). |
| [Sys.table_types][] | Eine Zeile für jeden Datentyp. |


### <a name="system-functions-for-statistics"></a>Funktionen für Statistik
Diese Funktionen sind nützlich für die Arbeit mit Statistiken:

| Funktion | Beschreibung |
| :-------------- | :---------- |
| [STATS_DATE][]    | Datum der letzten Aktualisierung der Statistik. |
| [DBCC SHOW_STATISTICS][] | Zusammenfassung und detaillierte Informationen über die Verteilung der Werte enthält, wie die Statistik. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Kombinieren von Spalten für Statistiken und Funktionen in einer Ansicht

Diese Ansicht zeigt Spalten Statistiken beziehen und [STATS_DATE()] [] Funktion zusammen führt.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>DBCC SHOW_STATISTICS() Beispiele

DBCC SHOW_STATISTICS() zeigt die Daten in einer Statistik. Diese Daten stammen aus drei Teilen.

1. Kopfzeile
2. Dichte Vektor
3. Histogramm

Die Header-Metadaten über die Statistiken. Histogramm zeigt die Verteilung der Werte in der ersten von der Statistik. Dichte Vektor Maßnahmen Spalte übergreifende Korrelation. SQLDW berechnet Kardinalität schätzt mit den Daten in der Statistik.

### <a name="show-header-density-and-histogram"></a>Header, Dichte und Histogramm anzeigen

Dieses einfache Beispiel zeigt alle drei Teile einer Statistik.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Zum Beispiel:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Ein oder mehrere Teile der DBCC SHOW_STATISTICS() anzeigen;

Wenn Sie nur bestimmte Teile anzeigen möchten, verwenden Sie die `WITH` Klausel und welche Teile Sie anzeigen möchten:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Zum Beispiel:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() Unterschiede
DBCC SHOW_STATISTICS() ist strenger im SQL Data Warehouse im Vergleich zu SQL Server implementiert.

1. Nicht dokumentierte Funktionen werden nicht unterstützt.
- Stats_stream kann nicht verwendet werden.
- Ergebnisse für spezifische Statistiken z.B. nicht beitreten (STAT_HEADER-JOIN-DENSITY_VECTOR)
2. NO_INFOMSGS kann für die Unterdrückung der Meldung festgelegt werden
3. Klammern um Statistiknamen von kann nicht verwendet werden
4. Spaltennamen können keine statistikobjekte identifizieren
5. Benutzerdefinierte Fehlermeldung 2767 wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [DBCC SHOW_STATISTICS][] auf MSDN.  Finden Sie weitere Artikel auf [Tabelle][Übersicht], [Tabelle Datentypen][Datentypen] [eine Tabelle Verteilung][verteilen], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition] und [Temporärtabellen][temporäre].  Weitere Informationen über bewährte Methoden finden Sie unter [SQL Data Warehouse Best Practices][].  

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
[Kardinalität Vorkalkulation]: https://msdn.microsoft.com/library/dn600374.aspx
[STATISTIKEN ERSTELLEN]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistik]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[Sys.Columns]: https://msdn.microsoft.com/library/ms176106.aspx
[Sys.Objects]: https://msdn.microsoft.com/library/ms190324.aspx
[Sys.Schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[Sys.Stats]: https://msdn.microsoft.com/library/ms177623.aspx
[Sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[' sys.Tables ']: https://msdn.microsoft.com/library/ms187406.aspx
[Sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[STATISTIKEN AKTUALISIEREN]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
