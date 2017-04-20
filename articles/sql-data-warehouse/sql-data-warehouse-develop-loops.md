<properties
   pageTitle="Schleifen im SQL Datawarehouse | Microsoft Azure"
   description="Tipps für Transact-SQL-Loops und Ersetzen Cursor in Azure SQL Data Warehouse Lösungen."
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

# <a name="loops-in-sql-data-warehouse"></a>Schleifen im SQL Datawarehouse
SQL Data Warehouse unterstützt die [WHILE][] -Schleife Anweisungsblöcken wiederholt ausführen. Weiterhin für angegebene zutrifft oder bis Code ausdrücklich mit Schleife beendet die `BREAK` Schlüsselwort. Schleifen sind besonders nützlich zum Ersetzen in SQL-Code definierten Cursor. Glücklicherweise lesen sind fast alle Cursor in SQL-Code geschrieben werden schneller Vorlauf nur. Daher sind [WHILE] -Schleifen eine großartige Alternative Wenn Sie sich eine ersetzen.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Nutzung von Schleifen und Cursor im SQL Data Warehouse ersetzen
Jedoch vor dem ersten Head Einstieg sollten Sie sich Fragen die folgende Frage: "Kann dieser Cursor neu geschrieben werden basiert ausgeführt?". In vielen Fällen die Antwort Ja werden und ist oft der beste Ansatz. Eine basiert häufig durchgeführt erheblich schneller als eine iterative Ansatz zeilenweise.

Schneller Vorlauf schreibgeschützten Cursor können problemlos mit einem Schleifenkonstrukt ersetzt werden. Es folgt ein einfaches Beispiel. In diesem Codebeispiel wird die Statistik für alle Tabellen in der Datenbank aktualisiert. Durch die Tabellen in der Schleife durchlaufen können jeden Befehl in Folge.

Erstellen Sie zunächst eine temporäre Tabelle mit einer eindeutigen Zeile Zahl zur Identifizierung der einzelnen Aussagen:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Zweitens initialisieren Sie Schleife ausführen erforderlichen Variablen:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Durchlaufen Sie jetzt Ausführung einer Anweisung jeweils:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Schließlich löschen Sie im ersten Schritt erstellten temporäre Tabelle

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Anwendungsentwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[WÄHREND]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
