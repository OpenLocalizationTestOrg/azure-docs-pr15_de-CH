<properties
   pageTitle="Tabelle auswählen (CTAS) im SQL Data Warehouse erstellen | Microsoft Azure"
   description="Tipps zur Codierung mit Create Table (CTAS) SELECT-Azure SQL Data Warehouse Lösungen."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Erstellen Sie Tabelle als auswählen (CTAS) im SQL Datawarehouse
Wählen Sie Tabelle erstellen oder `CTAS` ist eines der wichtigsten T-SQL-Funktionen verfügbar. Es ist ein vollständig parallelisierte Vorgang, der eine neue Tabelle basierend auf der SELECT-Anweisung erstellt. `CTAS`ist die einfachste und schnellste Weg eine Kopie einer Tabelle erstellen. Erachtens ein Kompressor Version von `SELECT..INTO` möchten. Dieses Dokument enthält Beispiele und best Practices für `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Kopieren Sie eine Tabelle mithilfe von CTAS

Vielleicht am häufigsten verwendet der `CTAS` ist eine Kopie einer Tabelle erstellen, sodass die DDL geändert werden können. Wenn beispielsweise die Tabelle als ursprünglich `ROUND_ROBIN` und jetzt wollen sie einer Tabelle eine Spalte verteilt `CTAS` Sie die verteilungsspalte ändern würde. `CTAS`kann auch verwendet werden, zu partitionieren, Indizierung oder Spalte ändern.

Nehmen Sie diese Tabelle mit Standard-Verteilung erstellt `ROUND_ROBIN` verteilt, da keine verteilungsspalte angegeben wurde die `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Nun möchten Sie eine neue Tabelle mit einem gruppierten Columnstore Index erstellen, damit Sie die Leistung von Columnstore gruppierte Tabellen nutzen können. Sie verteilen dieser Tabelle ProductKey da Joins Spalte erwarten auch Daten während Joins auf ProductKey vermeiden möchten. Schließlich auch möchten hinzufügen Partitionierung auf OrderDateKey, um schnell Daten löschen von alten Partitionen gelöscht. Hier wird die CTAS Anweisung die alte Tabelle in eine neue Tabelle kopieren.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Schließlich können Sie Ihre Tabellen in die neue Tabelle ersetzen und dann die alte Tabelle umbenennen.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Azure SQL Data Warehouse ist noch Unterstützung automatisch erstellen oder auto Update Statistics.  Um die Leistung von Abfragen zu erhalten, ist es wichtig, dass Statistiken für alle Spalten aller Tabellen nach dem ersten Laden erstellt werden oder wesentliche Änderungen in den Daten.  Eine ausführliche Erläuterung von Statistiken finden Sie unter [Statistiken][] in die Themengruppe entwickeln.

## <a name="using-ctas-to-work-around-unsupported-features"></a>Mithilfe von CTAS nicht unterstützte Funktionen

`CTAS`kann auch verwendet werden, eine Zahl die folgenden nicht unterstützten Features umgehen. Dies kann oft eine Win-Win-Situation werden nicht nur Code werden jedoch häufig führt schneller SQL Data Warehouse. Dies ist aufgrund der vollständig parallelisierte. Szenarien mit CTAS umgangen werden können, gehören:

- WÄHLEN SIE... IN
- ANSI-JOINS Updates
- ANSI-JOINs löschen
- MERGE-Anweisung

> [AZURE.NOTE] Denken Sie daran "CTAS erste". Wenn Sie denken, lösen Sie ein Problem mit `CTAS` , die wird im Allgemeinen am besten an-auch wenn Sie dadurch mehr Daten schreiben.
>

## <a name="selectinto"></a>WÄHLEN SIE... IN
Sie finden `SELECT..INTO` stellen in der Projektmappe angezeigt.

Im folgenden ist ein Beispiel für eine `SELECT..INTO` Anweisung:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Konvertieren der oben `CTAS` ist ganz einfach:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS erfordert zurzeit eine verteilungsspalte angegeben werden.  Wenn Sie absichtlich keine verteilungsspalte ändern möchten Ihre `CTAS` führen am schnellsten, wenn Sie eine Verteilung Spalte, der zugrunde liegenden Tabelle identisch ist diese Strategie Daten vermieden.  Wenn Sie eine kleine Tabelle erstellen, wo Leistung ist kein Faktor, dann können `ROUND_ROBIN` müssen Sie entscheiden, eine verteilungsspalte.

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI-Join Ersatz für die Update-Anweisung

Möglicherweise haben Sie eine komplexe Updates, die mehr als zwei Tabellen mit ANSI beitreten Syntax Ausführen von UPDATE oder DELETE verknüpft.

Stellen Sie sich vor, dass Sie diese Tabelle zu aktualisieren:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Die ursprüngliche Abfrage könnte folgendermaßen aussehen:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Da SQL Data Warehouse unterstützt ANSI joins in der `FROM` -Klausel ein `UPDATE` -Anweisung kann nicht kopieren diesen Code über ohne etwas ändern.

Verwenden Sie eine Kombination aus einer `CTAS` und eine implizite Verknüpfung dieser Code ersetzen:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI-Join Ersatz für delete-Anweisungen
Manchmal ist die beste Methode zum Löschen von Daten mit `CTAS`. Wählen Sie einfach wäre Daten beibehalten möchten. Dieses gilt insbesondere für `DELETE` Anweisungen, mit denen Ansi beitretenden Syntax, da SQL Data Warehouse nicht ANSI unterstützt in tritt der `FROM` -Klausel eine `DELETE` Anweisung.

Ein Beispiel für eine konvertierte DELETE-Anweisung ist verfügbar unter:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Merge-Anweisung ersetzen
MERGE-Anweisung ersetzt werden können, zumindest teilweise mit `CTAS`. Konsolidieren Sie die `INSERT` und `UPDATE` in einer einzigen Anweisung. Gelöschte Datensätze muss in eine zweite Anweisung geschlossen werden.

Ein Beispiel für eine `UPSERT` ist verfügbar unter:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS Empfehlung: ausdrücklich Datentyp und NULL-Zulässigkeit der Ausgabe

Beim Migrieren von Code finden Sie in dieser Art Codierungsmuster ausführen:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinktiv scheint Migrieren dieser Code sollte in CTAS und Sie korrekt. Allerdings ist eine verborgene Problem.

Der folgende Code ergibt nicht dasselbe Ergebnis:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Beachten Sie, dass die Spalte "Ergebnis" Vorwärts die Datenwerte Typ und NULL-Zulässigkeit des Ausdrucks führt. Dies kann zu feine Unterschiede in Werten führen, wenn Sie vorsichtig sind.

Versuchen Sie Folgendes Beispiel:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Ergebnis gespeicherte Wert unterscheidet. Der beibehaltene Wert in der Ergebnisspalte in anderen Ausdrücken verwendet wird der Fehler noch wichtiger.

![][1]

Dies ist besonders wichtig bei Datenmigrationen. Auch wenn die zweite Abfrage wohl genauer ist ein Problem. Daten wäre anderen gegenüber dem Quellsystem und Fragen der Integrität der Migration führt. Dies ist einer der seltenen Fälle, in denen "falsch" tatsächlich ist!

Sehen wir diese Unterschiede zwischen den beiden deshalb auf implizite Typumwandlung. Im ersten Beispiel definiert die Tabelle die Spaltendefinition. Wenn die Zeile eingefügt wird eine implizite Typumwandlung. Im zweiten Beispiel wird keine implizite Typumwandlung als der Datentyp der Spalte definiert. Beachten Sie, dass die Spalte im zweiten Beispiel ein zulassende definiert wurde im ersten Beispiel nicht hat. Erstellungsdatum die Tabelle im ersten Beispiel wird Spalte Zulässigkeit wurde explizit definiert. In der zweiten ergibt eine NULL-Definition wird es nur auf den Ausdruck und dies blieb.  

Zum Beheben dieser Probleme müssen explizit festlegen Konvertierung und NULL-Zulässigkeit in der `SELECT` Teil der `CTAS` Anweisung. Diese Eigenschaften können nicht an Tabelle erstellen festgelegt werden.

Das folgende Beispiel veranschaulicht den Code korrigieren:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Beachten Sie Folgendes:
- CAST oder CONVERT hätten
- ISNULL verwendet, um die NULL-Zulässigkeit nicht ZUSAMMENGESETZTEN erzwingen
- ISNULL ist die äußerste-Funktion
- Der zweite Teil der ISNULL ist eine Konstante also 0

> [AZURE.NOTE] Für die NULL-Zulässigkeit richtig festgelegt werden unbedingt mit `ISNULL` und `COALESCE`. `COALESCE`eine deterministische Funktion ist und so das Ergebnis des Ausdrucks ist immer NULL-Werte zulässt. `ISNULL`unterscheidet. Deterministisch ist. Daher beim zweiten Teil der `ISNULL` Funktion ist eine Konstante oder ein Literal und der resultierende Wert NOT NULL.

Dieser Tipp ist nicht nur nützlich für die Integrität der Ihrer Berechnung. Außerdem ist es wichtig für Tabelle Partition wechseln. Stellen Sie sich vor, Sie diese Tabelle als die Tatsache:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Das Feld "Wert" ist jedoch ein berechneter Ausdruck nicht Teil der Quelldaten ist.

Partitionierte Dataset erstellen, das Sie dies tun möchten:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Die Abfrage würde perfekt ausgeführt. Das Problem tritt beim Switch Partition ausführen. Die Tabellendefinitionen stimmen nicht überein. Zu den Definitionen der CTAS übereinstimmen muss geändert werden.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Sie sehen also, dass Typ Konsistenz und NULL-Zulässigkeit Eigenschaften auf CTAS empfiehlt engineering best. Es hilft, die Integrität in Ihrer Berechnung und gewährleistet, dass die Partition wechseln kann.

Weitere Informationen über [CTAS][]finden Sie unter MSDN. Es ist eines der wichtigsten Azure SQL Data Warehouse. Stellen Sie sicher, dass gründlich verstehen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[Übersicht über die Anwendungsentwicklung]: sql-data-warehouse-overview-develop.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
