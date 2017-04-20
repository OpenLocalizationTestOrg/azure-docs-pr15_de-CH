<properties
   pageTitle="Optimieren der Transaktionen für SQL Data Warehouse | Microsoft Azure"
   description="Best Practice-Anleitung zum Schreiben von effizienten Transaktionen Updates in Azure SQL Data Warehouse"
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Optimieren der Transaktionen für SQL Data Warehouse

Optimieren die Leistung von der Transaktionscode Risikos für lange Rollbacks erläutert.

## <a name="transactions-and-logging"></a>Transaktionen und Protokollierung

Transaktionen sind eine wichtige Komponente des relationalen Datenbankmoduls. SQL Data Warehouse verwendet Transaktionen während der Änderung von Daten. Diese Transaktionen können explizit oder implizit sein. Einzelne `INSERT`, `UPDATE` und `DELETE` Aussagen sind Beispiele für implizite Transaktionen. Explizite Transaktionen wurden explizit von einem Entwickler mit `BEGIN TRAN`, `COMMIT TRAN` oder `ROLLBACK TRAN` und werden normalerweise verwendet, wenn mehrere Anweisungen Änderung in eine atomare Einheit miteinander verbunden werden müssen. 

Azure SQL Data Warehouse verpflichtet der Datenbank Transaktionsprotokolle Änderungen. Jede Verteilung hat ihr eigenes Transaktionsprotokoll. Transaktionsprotokollen erfolgen automatisch. Es ist keine Konfiguration erforderlich. Während dieses Schreiben garantiert führt es als Aufwand in das System ein. Sie können diese Auswirkung minimieren, überführen effizienten Code schreiben. Überführen effizienter Code liegt Allgemein in zwei Kategorien.

- Nutzen Sie minimale Protokollierung Konstrukte Möglichkeit
- Prozessdaten mit Gültigkeitsbereich Batches zu singular Transaktionen
- Annehmen einer Partition Muster für große Änderungen an einer Partition wechseln

## <a name="minimal-vs-full-logging"></a>Minimaler und vollständige Protokollierung

Im Gegensatz zu vollständig protokollierte Vorgänge, die das Transaktionsprotokoll mit jeder zeilenänderung an, Verfolgen der minimal protokollierte Operationen sind und Metadaten geändert wird. Daher minimale Protokollierung umfasst nur die erforderlichen Informationen zum Rollback der Transaktion ein Fehler oder eine explizite Anforderung protokollieren (`ROLLBACK TRAN`). Wie viel weniger Informationen im Transaktionslog protokolliert werden, führt eine minimal protokollierte besser gleicher Größe vollständig protokollierten Vorgangs. Darüber hinaus denn weniger schreibt das Transaktionsprotokoll, viel weniger Daten generiert und so ist mehr effizient.

Buchung-Grenzwerte gelten nur für vollständig protokollierte Vorgänge.

>[AZURE.NOTE] Minimal protokollierte Operationen können explizite Transaktionen teilnehmen. Wie alle Änderungen im Reservierungsstrukturen nachverfolgt werden, ist es möglich, minimal protokollierte Operationen zurücksetzen. Es ist wichtig zu verstehen, dass die Änderung "Minimal" protokolliert ist nicht nicht angemeldet.

## <a name="minimally-logged-operations"></a>Minimal protokollierte Operationen

Die folgenden Vorgänge können nur minimal protokolliert:

- Erstellen der Tabelle als Option ([CTAS][])
- EINFÜGEN... WÄHLEN SIE
- INDEX ERSTELLEN
- ALTER INDEX NEU ERSTELLEN
- INDEX LÖSCHEN
- TRUNCATE TABLE
- TABELLE LÖSCHEN
- PARTITION DER ALTER TABLE SWITCH

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Interne Daten Bewegung Operationen (z. B. `BROADCAST` und `SHUFFLE`) Grenzwert Transaktion nicht betroffen.

## <a name="minimal-logging-with-bulk-load"></a>Minimale Protokollierung mit laden

`CTAS`und `INSERT...SELECT` sind beide Massenvorgänge laden. Jedoch sowohl die Tabellendefinition Ziel beeinflusst und Laden Szenario abhängig. Es folgt eine Tabelle, die erklärt, wenn der Massenvorgang vollständig oder minimal protokolliert werden:  

| Primärer Index               | Load-Szenario                                            | Protokollierungsmodus |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Heap                        | Alle                                                      | **Minimale**  |
| Gruppierter Index             | Leere Zieltabelle                                       | **Minimale**  |
| Gruppierter Index             | Geladene Zeilen überlappen sich nicht mit bestehenden Seiten Ziel | **Minimale**  |
| Gruppierter Index             | Überlappung mit vorhandener Seiten Ziel geladenen Zeilen        | Vollständige         |
| Gruppierten Index Columnstore | Losgröße > = 102.400 pro Partition ausgerichtet Verteilung | **Minimale**  |
| Gruppierten Index Columnstore | Batch-Größe < 102.400 pro Partition ausgerichtet Verteilung  | Vollständige         |

Es ist erwähnenswert, dass alle Schreibvorgänge auf sekundäre oder nicht gruppierten Indizes aktualisieren immer vollständig protokollierte Vorgänge.

> [AZURE.IMPORTANT] SQL Data Warehouse verfügt über 60-Distributionen. Wenn daher alle Zeilen gleichmäßig verteilt und Zielseite in einer Partition des Stapels müssen 6,144,000 Zeilen enthalten oder beim Schreiben in einen gruppierten Columnstore Index minimal protokolliert werden. Wenn die Tabelle partitioniert und die eingefügten Zeilen Partitionsgrenzen umfassen, dann brauchen Sie 6,144,000 Zeilen pro Partition Begrenzung auch Datendistribution vorausgesetzt. Jede Partition in jede Verteilung darf unabhängig 102.400 Zeile Schwellenwert für den Einsatz in der Verteilung minimal protokolliert werden.

Laden von Daten in einer nicht leeren Tabelle mit einem gruppierten Index kann oft aus vollständig protokollierte und minimal protokollierte Zeilen enthalten. Ein gruppierter Index ist eine ausgeglichene Struktur (b-Struktur) Seiten. Die Seite enthält bereits Zeilen von einer anderen Transaktion geschrieben wird, werden diese Schreibvorgänge vollständig protokolliert. Allerdings ist die Seite leer wird dann das Schreiben in diese Seite minimal protokolliert.

## <a name="optimizing-deletes"></a>Löscht optimieren

`DELETE`ist ein vollständig protokolliert.  Benötigen Sie eine große Menge von Daten in eine Tabelle oder eine Partition löschen, ist es oft sinnvoller, `SELECT` zu halten, Daten als minimal protokollierte Operation ausgeführt werden können.  Zu diesem Zweck erstellen Sie eine neue Tabelle mit [CTAS][].  Nach dem Erstellen Verwenden Sie [Umbenennen][] , die alte Tabelle mit der neu erstellten Tabelle auszutauschen.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Optimieren von updates

`UPDATE`ist ein vollständig protokolliert.  Benötigen Sie eine große Anzahl von Zeilen in einer Tabelle oder einer Partition aktualisieren kann es oft effizienter, eine minimal protokollierte Operation wie [CTAS][] verwenden.

Im Beispiel wird eine vollständige Tabelle Update wurde konvertiert eine `CTAS` minimale Protokollierung ist möglich.

In diesem Fall werden wir nachträglich einen Rabattbetrag in der Tabelle Sales hinzugefügt:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Neuerstellen große Tabellen profitieren mit SQL Data Warehouse Arbeitslast-Management-Funktionen. Weitere Informationen finden Sie Abschnitt Management Arbeitslast [Parallelität][] Artikel.

## <a name="optimizing-with-partition-switching"></a>Mit Partition optimieren

Bei umfangreichen Änderungen innerhalb einer [Partition Table][]ist eine Partition switching Muster sinnvoll. Wenn die Änderung von Daten ist, mehrere Partitionen umfasst erreicht dann einfach durchlaufen die Partitionen dasselbe Ergebnis.

Einen Partition Switch auszuführenden Schritte lauten:
1. Erstellen Sie eine leere, partition
2. Führen Sie das Update als CTAS
3. Die vorhandenen Daten aus Tabelle wechseln
4. Wechseln Sie in den neuen Daten
5. Daten bereinigen

Allerdings zum Identifizieren die Partitionen wechseln wir müssen Hilfsprozedur wie folgt erstellen. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Diese Prozedur maximiert die Wiederverwendung von Code und behält die Partition wird kompakter wechseln.

Der folgende Code veranschaulicht die fünf Schritte zu einer vollständigen Partition switching-Routine genannten.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Mit Kleinserien minimieren

Für große Datenänderungsvorgänge kann es Sinn Blöcken oder Batches die Arbeitseinheit Bereich des Vorgangs aufgeteilt.

Nachfolgend finden Sie ein Beispiel. Die Stapelgröße wurde einfach an die Technik markieren festgelegt. In Wirklichkeit würden die Batchgröße erheblich sein. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Anhalten und Anleitung skalieren

Azure SQL Data Warehouse können Sie anhalten, fortsetzen und Datawarehouse bei Bedarf zu skalieren. Beim Anhalten oder SQL Data Warehouse skalieren ist zu beachten, dass alle an Bord Transaktionen sofort beendet werden. dass alle offenen Transaktionen zurückgesetzt werden. Wenn Ihre Arbeitslast eine lange ausgeführt und unvollständige Datenänderung vor dem Anhalten oder Skalierung ausgegeben hat, müssen diese Arbeit rückgängig gemacht werden. Dies kann die Zeit zu unterbrechen oder skalieren Azure SQL Data Warehouse-Datenbank auswirken. 

> [AZURE.IMPORTANT] Beide `UPDATE` und `DELETE` sind vollständig protokollierte Vorgänge und damit diese Rückgängig/Wiederholen Operationen können erheblich länger dauern als Entsprechung minimal protokolliert. 

Bestenfalls ist zu Datenänderungstransaktionen Flug vor anhalten oder Skalierung SQL Data Warehouse abgeschlossen. Jedoch kann dies nicht immer praktikabel. Um das Risiko eines langen Rollback sollten Sie eine der folgenden Optionen:

- Lang andauernde Vorgänge umschreiben mit [CTAS][]
- Brechen Sie den Vorgang in Segmente; die auf eine Teilmenge der Zeilen

## <a name="next-steps"></a>Nächste Schritte

Anzeigen Sie weitere Informationen zu Isolationsstufen und transaktionale Grenzen [Transaktionen im SQL Data Warehouse][]  Eine Übersicht über die empfohlenen Vorgehensweisen finden Sie unter [SQL Data Warehouse Best Practices][].

<!--Image references-->

<!--Article references-->
[Transaktionen im SQL Datawarehouse]: ./sql-data-warehouse-develop-transactions.md
[Partition Table]: ./sql-data-warehouse-tables-partition.md
[Parallelität]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[UMBENENNEN]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

