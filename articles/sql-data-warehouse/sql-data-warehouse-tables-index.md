<properties
   pageTitle="Indizierung Tabellen im SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit Tabelle Indizieren in Azure SQL Data Warehouse."
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
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="indexing-tables-in-sql-data-warehouse"></a>Indizierung Tabellen im SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

SQL Data Warehouse bietet mehrere Indizierungsoptionen einschließlich [Columnstore gruppierter Indizes][] [gruppierte Indizes und nicht gruppierte Indizes][].  Darüber hinaus bietet es eine keine Indexoption auch [Heap][].  Dieser Artikel behandelt die Vorteile jeder Index sowie Tipps zu den meisten Leistung Ihrer Indizes. Finden Sie weitere Details [Table-Syntax erstellen][] zum Erstellen einer Tabelle im SQL Data Warehouse.

## <a name="clustered-columnstore-indexes"></a>Columnstore gruppierte Indizes

SQL Data Warehouse erstellt keine Indexoptionen für eine Tabelle angegeben Columnstore gruppierter Index. Gruppierte Columnstore Tabellen bieten sowohl höchste Datenkompression sowie optimale allgemeine abfrageleistung.  Gruppierte Columnstore Tabellen gruppierte Index oder Heap-Tabellen werden in der Regel übertreffen und sind in der Regel die beste Wahl für große Tabellen.  Aus diesen Gründen ist gruppierte Columnstore am besten starten, wenn Sie nicht wissen, wie Ihre Tabelle indiziert werden.  

Zum Erstellen einer gruppierten Columnstore Tabelle einfach GRUPPIERTER INDEX COLUMNSTORE in der WITH-Klausel angeben oder die WITH-Klausel weglassen:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Es gibt einige Szenarien, in denen gruppierte Columnstore nicht empfohlen:

- Columnstore Tabellen unterstützen keine sekundären nicht gruppierte Indizes.  Sollten Sie Heap oder gruppierten indextabellen.
- Columnstore Tabellen unterstützen keine 'varchar(max)', 'nvarchar(max)' und 'varbinary(max)'.  Sollten Sie Heap oder gruppierten Index.
- Columnstore Tabellen möglicherweise weniger effizient für vorübergehende Daten.  Berücksichtigen Sie Heap und vielleicht sogar temporären Tabellen.
- Kleine Tabellen mit 100 Millionen Zeilen.  Berücksichtigen Sie Heaptabellen.

## <a name="heap-tables"></a>Heap-Tabellen

Wenn Sie vorübergehend Daten im SQL Data Warehouse Landung finden Sie mit einer Heap-Tabelle den Gesamtprozess schneller machen.  Dies ist für Heaps wird schneller als Index-Tabellen und in einigen Fällen das nachfolgende Lesen aus Cache erfolgen kann.  Laden von Daten um Schritt vor der Ausführung Weitere Transformationen werden beim Laden der Tabelle Heap schneller Laden von Daten in Tabellenform Cluster Columnstore. Darüber hinaus wird Laden von Daten in eine [temporäre Tabelle][temporäre] auch schneller als das Laden einer Tabelle in einen permanenten Speicher geladen.  

Für kleine Nachschlagetabellen 100 Millionen Zeilen sinnvoll häufig Heaptabellen.  Cluster Columnstore Tabellen beginnen optimale Komprimierung wird mehr als 100 Millionen Zeilen.

Zum Erstellen einer Heap-Tabelle einfach Geben Sie HEAP in der WITH-Klausel an:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Gruppierte und nicht gruppierte Indizes

Gruppierte Indizes können gruppierte Columnstore Tabellen übertreffen, wenn eine einzelne Zeile schnell abgerufen werden soll.  Abfragen einer oder wenigen Zeilen Suche mit Höchstgeschwindigkeit erforderlich ist, sollten Sie ein gruppierter Index oder nicht gruppierten sekundärer Index.  Die Verwendung eines gruppierten Indexes Nachteil ist, dass nur die zielgenaue Filter auf den gruppierten Index-Spalte Abfragen profitieren.  Zur Verbesserung der Filter für andere Spalten kann ein nicht gruppierter Index auf andere Spalten hinzugefügt werden.  Jeder Index zu einer Tabelle hinzugefügt wird jedoch Speicherplatz und Verarbeitungszeit zu Lasten hinzufügen.

Erstellen Sie eine gruppierten Indextabelle einfach Geben Sie GRUPPIERTER INDEX in der WITH-Klausel an:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Um einen nicht gruppierten Index für eine Tabelle hinzuzufügen, verwenden Sie einfach die folgende Syntax:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] Ein nicht gruppierter Index wird standardmäßig erstellt, wenn CREATE INDEX verwendet wird. Außerdem darf ein nicht gruppierter Index nur für eine Zeile Storage-Tabelle (HEAP oder GRUPPIERTER INDEX). Nicht gruppierte Indizes auf einem gruppierten INDEX COLUMNSTORE ist zurzeit nicht zulässig.


## <a name="optimizing-clustered-columnstore-indexes"></a>Columnstore gruppierte Indizes optimieren

Columnstore gruppierte Tabellen sind Daten in Segmente unterteilt.  Segment hohe Qualität ist entscheidend für eine optimale Leistung auf Columnstore-Tabelle.  Die Anzahl der Zeilen im komprimierten Zeilengruppe kann Segment Qualität gemessen.  Segment Qualität ist optimal mit mindestens 100K Zeilen pro komprimierte gruppieren und erhalten Leistung, wenn die Anzahl der Zeilen pro Zeile Ansatz 1.048.576 Zeilen ist die meisten Zeilen, die eine Zeilengruppe enthalten kann.

Die folgenden Ansicht erstellt und berechnet die durchschnittlichen Zeilen pro Gruppe optimal Cluster Columnstore Indizes identifizieren auf Ihrem System verwendet werden kann.  Die letzte Spalte für diese Ansicht wird als SQL-Anweisung generiert, die mit Ihrer Indizes neu erstellen.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,   COUNT(DISTINCT rg.[partition_number])                   AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,   CEILING ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Erstellung die Ansicht führen Sie diese Abfrage um Tabellen mit Zeilengruppen mit weniger als 100 K Zeilen zu identifizieren.  Natürlich sollten Sie den Schwellenwert von 100 KB suchen optimale Segment Qualität erhöhen. 

```sql
SELECT  *
FROM    [dbo].[vColumnstoreDensity]
WHERE   COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Nachdem Sie die Abfrage ausgeführt haben Daten und Analysieren der Ergebnisse beginnen können. Diese Tabelle erläutert, wie Sie in der Zeile Analyse gesucht.


| Spalte                             | Verwendung dieser Daten                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Table_partition_count]            | Wenn der partitionierten Tabelle dann erwarten Sie möglicherweise zu höheren öffnen Zeilengruppe zählt. Jede Partition in der Verteilung konnte theoretisch ein Öffnen Zeilengruppe zugeordnet haben. Berücksichtigen Sie dies in der Analyse. Eine kleine Tabelle partitioniert wurde konnte durch Entfernen der Partitionierung insgesamt optimiert diese Komprimierung verbessern würde.                                                                        |
| [Row_count_total]                  | Gesamtzeilenanzahl für die Tabelle. Diesen Wert können Sie um Prozentsatz der Zeilen im komprimierten Zustand zu berechnen.                                                                      |
| [Row_count_per_distribution_MAX]   | Wenn alle Zeilen gleichmäßig verteilen dieser Wert wäre Zielanzahl Zeilen pro Verteilung. Vergleichen Sie diesen Wert mit der Compressed_rowgroup_count.                                 |
| [COMPRESSED_rowgroup_rows]         | Gesamtanzahl der Zeilen im Columnstore Format für die Tabelle.                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_AVG]     | Ist die durchschnittliche Anzahl an Zeilen erheblich kleiner als die maximale Anzahl der Zeilen einer Zeilengruppe, sollten Sie die Daten komprimieren mit CTAS oder ALTER INDEX neu erstellen                     |
| [COMPRESSED_rowgroup_count]        | Anzahl von Zeilengruppen Columnstore Format. Ist diese Anzahl sehr hoch in Bezug auf die Tabelle ist ein Indikator, dass die Columnstore niedrig ist.                                  |
| [COMPRESSED_rowgroup_rows_DELETED] | Zeilen werden im Format Columnstore logisch gelöscht. Sollten Sie die Anzahl relativ Tabellengröße hohe die Partition neu oder erneutes Erstellen des Indexes, wie diese physisch entfernt. |
| [COMPRESSED_rowgroup_rows_MIN]     | Können Sie zusammen mit AVG und MAX Spalten um den Wertebereich für die Zeilengruppen in der Columnstore zu verstehen. Nur wenige laden Schwelle (102.400 pro Partition ausgerichtet Verteilung) zufolge Optimierungen im Laden von Daten verfügbar sind                                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_MAX]     | Wie oben                                                                                                                                                                                  |
| [OPEN_rowgroup_count]              | Öffnen Zeilengruppen sind normal. Eine würde einen offenen Zeilengruppe pro tabellenverteilung (60) erwarten. Überflüssige vorschlagen partitionsübergreifend Laden von Daten. Überprüfen die Partitionierungsstrategie sicher ist                                                                                                                                                                                                |
| [OPEN_rowgroup_rows]               | Jede Zeilengruppe kann 1.048.576 Zeilen maximal enthalten. Verwenden Sie diesen Wert sehen wie öffnen Zeilengruppen aktuell                                                                 |
| [OPEN_rowgroup_rows_MIN]           | Offene Gruppen angeben, dass Daten durchlassen in die Tabelle geladen oder dass die vorherige Ladung verschüttet verbleibenden Zeilen in diese Zeilengruppe. Verwenden MIN, MAX, AVG Spalten zu sehen, wie viele Daten in GEÖFFNETEN sat Zeilengruppen. Bei kleinen Tabellen könnte es 100 % aller Daten! In diesem Fall ALTER INDEX neu erstellen, um Daten zu Columnstore erzwingen.                                                                       |
| [OPEN_rowgroup_rows_MAX]           | Wie oben                                                                                                                                                                                  |
| [OPEN_rowgroup_rows_AVG]           | Wie oben                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows]             | Geschlossene Gruppenzeilen als Überprüfung betrachten.                                                                                                                                       |
| [CLOSED_rowgroup_count]            | Die Anzahl der geschlossenen Zeilengruppen sollte niedrig sein, wenn überhaupt gesehen werden. Komprimierte Rowg Roups mit der ALTER INDEX können geschlossene Gruppen konvertiert... Befehl neu. Dies ist jedoch nicht erforderlich. Geschlossene Gruppen werden automatisch vom Prozess "Tupel Mover" Hintergrund auf Zeilengruppen Columnstore konvertiert.                                                                                               |
| [CLOSED_rowgroup_rows_MIN]         | Geschlossene Gruppen müssen eine sehr hohe Rate. Wenn die Füllrate für eine geschlossene niedrig ist, ist weitere Analysen der Columnstore erforderlich.                                   |
| [CLOSED_rowgroup_rows_MAX]         | Wie oben                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows_AVG]         | Wie oben                                                                                                                                                                                  |
| [Rebuild_Index_SQL]         | SQL Columnstore-Index für eine Tabelle neu erstellen                                                                                                                                                     |

## <a name="causes-of-poor-columnstore-index-quality"></a>Ursachen für schlechte Columnstore Index Qualität

Wenn Sie Tabellen mit schlechter Segment identifiziert haben, sollten Sie die Ursache ermitteln.  Im folgenden sind einige häufige Ursachen für schlechte Segment Qualität:

1. Arbeitsspeicher, wenn der Index erstellt wurde
2. Große Anzahl von DML-Vorgänge
3. Kleine oder Ladevorgängen Durchlassen
4. Zu viele Partitionen

Diese Faktoren kann ein Columnstore Index deutlich kleiner als die optimalen 1 Million Zeilen pro Zeile.  Sie können auch Zeilen Delta Zeilengruppe statt einer komprimierten Zeilengruppe zu verursachen. 

### <a name="memory-pressure-when-index-was-built"></a>Arbeitsspeicher, wenn der Index erstellt wurde

Die Anzahl der Zeilen pro komprimierte Zeilengruppe beziehen sich direkt auf die Breite der Zeile und den Arbeitsspeicher zum Verarbeiten der Zeilengruppe.  Wenn Zeilen in Tabellen unter Speicherdruck Columnstore geschrieben werden, beeinträchtigt Columnstore Segment Qualität.  Daher empfiehlt der Sitzung zu dem Schreiben in die Columnstore Index Tabellen auf so viel Speicher wie möglich.  Gibt ein Kompromiss zwischen Speicher und Parallelität, die Hinweise auf die richtige Speicherzuordnungs hängt die Daten in jeder Zeile der Tabelle an DWU Sie Ihrem System zugeordnet haben und an Parallelität Steckplätze können Sie der Sitzung das Schreiben von Daten in der Tabelle.  Als bewährte Methode wird empfohlen, beginnend mit Xlargerc verwenden Sie DW300 oder weniger Largerc verwenden Sie DW400, DW600 und Mediumrc bei DW1000 und höher.

### <a name="high-volume-of-dml-operations"></a>Große Anzahl von DML-Vorgänge

Eine große Anzahl von DML-Vorgänge aktualisieren und Löschen von Zeilen kann Ineffizienz in den Columnstore einführen. Dies gilt insbesondere, wenn die meisten Zeilen in einer Zeilengruppe geändert.

- Löschen einer Zeile aus einer komprimierten Zeilengruppe nur logisch kennzeichnet die Zeile als gelöscht. Zeile verbleibt im komprimierten Zeilengruppe, bis die Partition oder die Tabelle neu erstellt wird.
- Einfügen einer Zeile hinzugefügt eine interne Rowstore Tabelle eine Zeilengruppe Delta zu Zeile. Die eingefügte Zeile wird nicht in Columnstore konvertiert, bis die Zeilengruppe Delta ist als geschlossen markiert. Zeilengruppen schließen 1.048.576 Zeilen maximal erreichen. 
- Aktualisieren einer Zeile im Columnstore-Format wird eine logische Löschung und eine Einfügung verarbeitet. Die eingefügte Zeile möglicherweise im Delta-Speicher gespeichert.

Update zusammengefasst und Einfügevorgänge, die die Massen 102.400 Zeilen pro Partition Schwellenwert ausgerichtete Verteilung an das Columnstore-Format geschrieben. Aber müssten Wenn eine gleichmäßige Verteilung, Sie 6.144 Millionen Zeilen in einer einzigen Operation dazu ändern. Wenn die Anzahl der Zeilen für eine bestimmte Partition ausgerichtet wird Verteilung unter 102.400 Zeilen an den Delta-Speicher und bleibt dort, bis genügend Zeilen eingefügt oder schließen die Zeilengruppe geändert wurden oder der Index neu erstellt.

### <a name="small-or-trickle-load-operations"></a>Kleine oder Ladevorgängen Durchlassen

Kleine lädt, fließen SQL Data Warehouse werden manchmal auch als Durchlassen von Lasten bezeichnet. Sie sind in der Regel einen Nahen Konstanten Datenstrom vom System aufgenommen. Wie dieses Streams in fortlaufenden ist die Anzahl von Zeilen nicht besonders groß. Meistens sind die Daten deutlich unter dem Grenzwert für eine direkte Columnstore Format erforderlich.

In diesen Fällen ist es oft besser die Daten zuerst in Azure BLOB-Speicher und vor der Verladung sammeln lassen. Dieses Verfahren wird häufig als *Micro Batchverarbeitung*bezeichnet.

### <a name="too-many-partitions"></a>Zu viele Partitionen

Zu beachten ist die Auswirkung auf den gruppierten Columnstore Tabellen partitionieren.  Vor dem Partitionieren teilt SQL Data Warehouse Daten bereits in 60 Datenbanken.  Partitionierung teilt weitere Daten.  Wenn Partitionierung Ihrer Daten sollten Sie berücksichtigen, dass **jede** Partition mindestens 1 Million Zeilen Columnstore gruppierter Index nutzen muss.  Wenn Sie die Tabelle in 100 Partitionen partitionieren, müssen die Tabelle mindestens 6 Milliarden Zeilen von gruppierten Columnstore-Index (60 Distributionen *100 Partitionen* 1 Million Zeilen) profitieren. Die 100 Partitionstabelle keinen 6 Milliarden Zeilen, reduzieren Sie die Anzahl der Partitionen oder Heap-Tabelle stattdessen verwenden.

Tabellen mit Daten geladen wurde, führen Sie die folgenden Schritte zu Tabellen mit optimalen Cluster Columnstore Indizes neu erstellen.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Neuerstellen von Indizes Segment zu verbessern

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Schritt 1: Identifizieren Sie oder erstellen Sie Benutzer, die richtige Klasse verwendet

Können sofort Segment Verbesserung werden der Index neu erstellt.  Durch die oben ausgewählte Ansicht zurückgegebenen SQL wird eine Anweisung ALTER INDEX neu erstellen zurückgegeben mit Ihre Indizes neu erstellen.  Wenn Ihre Indizes neu aufbauen, werden Sie sicher, dass genügend Arbeitsspeicher für die Sitzung reserviert werden der Index neu erstellt wird.  Dazu erhöhen Sie die Klasse von einem Benutzer die Berechtigungen für diese Tabelle als das empfohlene Minimum Index neu erstellen.  Die Klasse von der Datenbankbesitzer kann geändert werden, wenn einen Benutzer nicht auf dem System erstellt haben, Sie dies zuerst tun müssen.  Wir empfehlen mindestens ist Xlargerc verwenden Sie DW300 oder weniger Largerc verwenden Sie DW400, DW600 und Mediumrc bei DW1000 und höher.

Es folgt ein Beispiel zum erhöhen ihre Klasse Benutzer mehr Arbeitsspeicher zuweisen.  Weitere Informationen zu Ressourcen Klassen und zum Erstellen eines neuen Benutzers finden die [Parallelität und Arbeitslast-Management] [ Concurrency] Artikel.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Schritt 2: Rebuild Columnstore gruppierter Indizes mit höheren Resource-Klasse
Melden Sie sich als Benutzer aus Schritt 1 (z.B. LoadUser) nun mithilfe eine höhere Ressourcenklasse und der ALTER INDEX-Anweisung ausführen.  Werden Sie sicher, dass dieser Benutzer ALTER-Berechtigung für die Tabellen, in denen der Index neu erstellt wird.  Diese Beispiele zeigen den gesamten Columnstore Index neu erstellen oder eine einzelne Partition neu erstellen. Bei großen Tabellen ist praktikabler neu erstellen jeweils eine einzelne Partition indiziert.

Alternativ kann statt erneutes Erstellen des Indexes, die Tabelle in eine neue Tabelle mit [CTAS][]kopieren.  Welche Methode am besten geeignet ist? Für große Datenmengen ist [CTAS][] in der Regel schneller als [ALTER INDEX][]. Für kleinere Datenmengen [ALTER INDEX][] ist einfacher zu verwenden und müssen Sie die Tabelle wechseln.  Weitere Informationen zu Indizes mit CTAS Wiederherstellen finden Sie unter **Rebuilding Indizes mit CTAS und switching-Partition** unter.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Erneutes Erstellen eines Indexes im SQL Data Warehouse ist offline.  Weitere Informationen zum Neuerstellen von Indizes finden Sie im Abschnitt ALTER INDEX neu erstellen [Columnstore Indizes Defragmentierung][] und im Thema zur Befehlssyntax [ALTER INDEX][].
 
### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Schritt 3: Überprüfen Sie gruppierte Columnstore Segment Qualität verbessert
Wiederholung der Abfrage die identifizierte Tabelle mit schlechter Qualität segmentieren und Segment Qualität verbessert.  Wenn Segment nicht verbessern, könnte es sein, dass Zeilen in der Tabelle extra breit.  Verwenden Sie eine höhere Klasse oder DWU Wenn Ihre Indizes neu aufbauen.

 
## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Neuerstellen von Indizes mit CTAS Partition wechseln

Dieses Beispiel verwendet [CTAS][] und Partition wechseln, um eine Partition wiederherzustellen. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
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
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
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
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Weitere Informationen zum erneuten Erstellen Partitionen mit `CTAS`, [Partition][] finden.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Artikel auf [Tabelle][Übersicht], [Tabelle Datentypen][Datentypen] [eine Tabelle Verteilung][verteilen], [Partitionieren einer Tabelle][Partition], [Tabellenstatistik Verwalten von][Statistiken] und [Temporärtabellen][temporäre].  Weitere Informationen über bewährte Methoden finden Sie unter [SQL Data Warehouse Best Practices][].

<!--Image references-->

<!--Article references-->
[Übersicht]: ./sql-data-warehouse-tables-overview.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Temporäre]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[Heap]: https://msdn.microsoft.com/library/hh213609.aspx
[gruppierte Indizes und nicht gruppierte Indizes]: https://msdn.microsoft.com/library/ms190457.aspx
[Table-Syntax erstellen]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore Indizes Defragmentierung]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[Columnstore gruppierte Indizes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
