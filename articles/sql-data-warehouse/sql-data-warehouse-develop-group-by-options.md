<properties
   pageTitle="Gruppe von Optionen in SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Implementierung von Gruppe von Optionen in Azure SQL Data Warehouse Lösungen."
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

# <a name="group-by-options-in-sql-data-warehouse"></a>Gruppe von Optionen in SQL Data Warehouse

Die [GROUP BY][] -Klausel wird auf Zusammenfassung Zeilen Daten verwendet. Es hat auch einige Optionen, die erweitern die Funktionalität, die umgangen werden, da sie nicht direkt von Azure SQL Data Warehouse unterstützt werden.

Diese Optionen sind
- GROUP BY mit ROLLUP
- GROUPING SETS
- GROUP BY mit CUBE

## <a name="rollup-and-grouping-sets-options"></a>Rollup- und Grouping Sets-Optionen
Die einfachste Option ist die Verwendung `UNION ALL` stattdessen das Rollup ausführen anstatt der expliziten Syntax. Das Ergebnis ist das gleiche

Unten ist ein Beispiel einer Gruppe Anweisung mit der `ROLLUP` Option:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Mit ROLLUP haben wir die folgenden Aggregationen angefordert:
- Land und Region
- Land
- Gesamtsumme

Ersetzen Sie müssen mit `UNION ALL`; Angeben der Aggregationen muss explizit dieselben Ergebnisse zurückgeben:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Für GROUPING SETS wir tun müssen, ist dem gleichen principal jedoch nur erstellen UNION ALL Abschnitten Aggregationsebene zu sehen

## <a name="cube-options"></a>Cube-Optionen
Es ist möglich, eine GROUP BY mit CUBE mithilfe des UNION ALL Ansatzes erstellen. Das Problem ist, dass der Code schnell aufwändig und unhandlich werden kann. Um dies zu vermeiden können Sie erweiterte Verfahren.

Nehmen wir das Beispiel oben.

Der erste Schritt ist definieren "Kubus", die alle Ebenen der Aggregation definiert, die wir erstellen möchten. Es ist wichtig zu beachten CROSS JOIN abgeleiteten Tabellen. Dadurch werden alle Ebenen für uns generiert. Der Rest des Codes ist wirklich für Formatierung.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Die Ergebnisse der CTAS angezeigt werden:

![][1]

Der zweite Schritt ist an eine Zieltabelle Zwischenergebnisse zu speichern:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Der dritte Schritt ist über unsere Cube Spalten durchführen der Aggregation durchlaufen. Die Abfrage wird einmal für jede Zeile in der temporären Tabelle #Cube ausführen und die Ergebnisse in der temporären Tabelle #Results

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Schließlich können wir den Ergebnissen aus der temporären Tabelle #Results lesen

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Durch den Code in Abschnitte unterteilen und generieren ein Schleifenkonstrukt wird Code überschaubar und verwaltet.


## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[Übersicht über die Anwendungsentwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GRUPPIEREN NACH]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
