<properties
    pageTitle="SQL im Arbeitsspeicher Einstieg | Microsoft Azure"
    description="SQL In-Memory-Technologie verbessern die Leistung von Transaktions- und Analytics Arbeitslasten. Erfahren Sie, wie diese Technologie nutzen."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Erste Schritte mit im Speicher (Vorschau) SQL-Datenbank

Im Speicher Funktionen verbessern die Leistung von Transaktions- und Analytics Arbeitslasten in den richtigen Situationen.

Dieses Thema betont zwei Demonstrationen, speicherinterne OLTP und Analysen im Arbeitsspeicher. Jede Demo komplett mit Schritte und die Demo ausführen möchten. Sie können entweder:

- Verwenden Sie Code, um Varianten Unterschiede in Ergebnisse testen; oder
- Lesen Sie den Code zum Szenario verstehen und wie erstellen und die Objekte im Arbeitsspeicher verwenden.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [Quick Start 1: im Arbeitsspeicher OLTP-Technologie für schnellere T-SQL-](http://msdn.microsoft.com/library/mt694156.aspx) -ist ein weiterer Artikel zu beginnen.

#### <a name="in-memory-oltp"></a>OLTP Speicher

Die Features im Arbeitsspeicher [OLTP](#install_oltp_manuallink) (online Transaction Processing) sind:

- Speicheroptimierte Tabellen.
- Systemeigen kompiliert gespeicherte Prozeduren.


Speicheroptimierte Tabelle hat eine Darstellung von sich selbst im aktiven Speicher neben der standardmäßigen Darstellung auf einer Festplatte. Geschäftliche Transaktionen für die Tabelle schneller ausgeführt, da sie direkt die Darstellung interagieren, die im Arbeitsspeicher.

Mit OLTP Speicher erreichen, Sie zu 30 Mal in Transaktionsdurchsatz abhängig von der Arbeitslast.


Nativ kompilierte gespeicherte Prozeduren erfordern weniger Anweisung zur Laufzeit als herkömmliche interpretiert gespeicherten Prozeduren. Wir habe systemeigene Kompilierungsergebnis in Dauer 1/100 interpretiert Dauer.


#### <a name="in-memory-analytics"></a>Im Arbeitsspeicher Analytics 

Das Feature im Arbeitsspeicher [Analytics](#install_analytics_manuallink) ist:

Columnstore Indizes verbessern die Leistung von Analysen und Berichte Abfragen. 


#### <a name="real-time-analytics"></a>Echtzeit-Analysen

Für [Echtzeit-Analysen](http://msdn.microsoft.com/library/dn817827.aspx) werden im Arbeitsspeicher OLTP und Analytics zu kombinieren:

- Echtzeit-Geschäftsinformationen basierend auf Daten.


#### <a name="availability"></a>Verfügbarkeit


GA, allgemeine Verfügbarkeit:

- [Columnstore Indizes](http://msdn.microsoft.com/library/dn817827.aspx) *auf dem Datenträger*.


Vorschau:

- OLTP Speicher
- Betriebliche Echtzeitanalysen


In der Vorschau zwar In-Memory-Funktionen sind Beschreibung [Weiter unten in diesem Thema](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Diese Vorschaufunktionen sind nur für [*Premium*](sql-database-service-tiers.md) -Datenbanken nicht im elastischen Pools verfügbar und für alle Datenbanken Basic oder Standard nicht verfügbar.  Unterstützung für Premium-Datenbanken im elastischen Pools kommt bald. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>A. Installieren des Beispiels im Arbeitsspeicher OLTP

Die Beispieldatenbank AdventureWorksLT [V12] können mit wenigen Klicks in [Azure-Portal](https://portal.azure.com/). Die Schritte in diesem Abschnitt erläutern Sie dann wie die AdventureWorksLT-Datenbank mit gestalten können:

- Tabellen im Arbeitsspeicher.
- Nativ kompilierte gespeicherte Prozedur.


#### <a name="installation-steps"></a>Installationsschritte

1. Erstellen Sie in [Azure-Portal](https://portal.azure.com/)eine Premium-Datenbank auf einem Server V12. Legen Sie die **Quelle** auf der Beispieldatenbank AdventureWorksLT [V12].
 - Weitere Informationen zum [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md)angezeigt.

2. Verbinden Sie mit SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. [Im Arbeitsspeicher OLTP Transact-SQL-Skript](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) in die Zwischenablage kopieren.
 - T-SQL-Skript erstellt die erforderlichen Speicher-Objekte in der Beispieldatenbank AdventureWorksLT in Schritt 1 erstellte.

4. SSMS fügen Sie T-SQL-Skript ein, und führen Sie das Skript.
 - Entscheidend ist die `MEMORY_OPTIMIZED = ON` Klausel CREATE TABLE-Anweisung als in:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Fehler 40536


Wenn Sie Fehler 40536 beim Ausführen des T-SQL-Skripts erhalten, führen Sie das folgende T-SQL-Skript überprüfen, ob die Datenbank im Speicher unterstützt:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Ein Ergebnis von **0** bedeutet, dass im Speicher wird nicht unterstützt und 1 bedeutet, dass es unterstützt wird. Um das Problem zu diagnostizieren:

- Sicherstellen Sie, dass die Datenbank erstellt wurde, In-Memory OLTP-Funktionen für die Vorschau aktiv wurde.
- Sicherstellen Sie, dass die Datenbank zur Premium-Service-Tier.


#### <a name="about-the-created-memory-optimized-items"></a>Über die erstellten Artikel Speicher optimiert

**Tabellen**: enthält die folgenden Tabellen Speicher optimiert:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Sie können Tabellen im **Objekt-Explorer** in SSMS von speicheroptimierten prüfen:

- Maustaste **Tabellen** > **Filter** > **Filtern** > **Speicher optimiert ist** gleich 1.


Oder Sie können die Katalogsichten wie:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Nativ kompilierte gespeicherte Prozedur**: SalesLT.usp_InsertSalesOrder_inmem durch eine Abfrage Katalog überprüft werden:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>Der OLTP-Workload Beispiel ausführen

Der einzige Unterschied zwischen den folgenden beiden *Prozeduren* ist die erste Prozedur speicheroptimierten Versionen Tabellen verwendet, die zweite Prozedur verwendet die regulären Tabellen auf der Festplatte:

- SalesLT**.** Usp_InsertSalesOrder**_inmem**
- SalesLT**.** Usp_InsertSalesOrder**_ondisk**


In diesem Abschnitt erfahren Sie, wie praktisch **ostress.exe** -Dienstprogramm verwenden, um zwei gespeicherte Prozeduren anstrengend Ebene ausführen. Wie lange dauert zwei Stress führt zum abschließen können.


Beim Ausführen von ostress.exe wird empfohlen, Sie sollen beide Parameterwerte übergeben:

- Führen Sie eine große Anzahl von aktiven Verbindungen mithilfe - n100.
- Haben Sie jede Verbindungsschleife Hunderte oft durch Verwendung - r500.


Jedoch viel kleinere Werte wie - n10 starten möchten und r50 das alles funktioniert.


### <a name="script-for-ostressexe"></a>Skript für ostress.exe


Dieser Abschnitt zeigt das T-SQL-Skript, das unsere Befehlszeile ostress.exe eingebettet ist. Das Skript verwendet, das T-SQL-Skript erstellt wurden, früher installiert.


Das folgende Skript fügt einen beispielauftrag mit fünf Artikel in den folgenden speicheroptimierten *Tabellen*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Um die _ondisk Version des vorherigen T-SQL für ostress.exe, würden Sie beide Vorkommen der Teilzeichenfolge *_inmem* mit *_ondisk*ersetzen. Dieser ersetzt Einfluss auf die Namen der Tabellen und gespeicherten Prozeduren.


### <a name="install-rml-utilities-and-ostress"></a>Installieren Sie RML-Dienstprogramme und ostress


Idealerweise würden Sie ostress.exe auf Azure-VM ausführen möchten. Ein [Azure Virtual Machine](https://azure.microsoft.com/documentation/services/virtual-machines/) erstellen in Azure derselben geographischen Region Sie die AdventureWorksLT-Datenbank sich befindet. Aber Sie können ausführen ostress.exe auf Ihrem Laptop.


VM und den host auswählen, darunter ostress.exe Dienstprogrammen Replay Markup Language (RML) installieren.

- Finden Sie im [In-Memory-OLTP-Beispieldatenbank](http://msdn.microsoft.com/library/mt465764.aspx)ostress.exe.
 - Oder [Speicherresidenten OLTP-Beispieldatenbank](http://msdn.microsoft.com/library/mt465764.aspx).
 - Oder [Blog für die Installation von ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Führen Sie zuerst _inmem Stress Arbeitslast


Ein *RML Cmd Prompt* Fenster können unsere ostress.exe-Befehlszeile ausführen. Die Befehlszeilenparameter direkte Ostress an:

- 100 Verbindungen gleichzeitig ausgeführt (-n100).
- Jede Verbindung 50 Mal das T-SQL-Skript ausgeführt (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Die vorstehenden ostress.exe-Befehlszeile ausführen:


1. Zurücksetzen des Dateninhalt Datenbank durch Ausführen des folgenden Befehls in SSMS zu löschen, die durch alle früher eingefügt wurde:
```
EXECUTE Demo.usp_DemoReset;
```

2. Kopieren Sie den Text der vorherigen ostress.exe-Befehlszeile in die Zwischenablage.

3. Ersetzen Sie die `<placeholders>` für den Parameter -S - U -P -d mit der tatsächlichen Werte.

4. Führen Sie die bearbeitete Befehlszeile im RML-Befehlsfenster.


#### <a name="result-is-a-duration"></a>Ergibt eine Dauer


Abschluss ostress.exe schreibt es die Testlaufdauer als die letzte Zeile der Ausgabe RML Cmd-Fenster. Kürzerer Testlauf dauerte beispielsweise etwa 1,5 Minuten:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Zurücksetzen, _ondisk bearbeiten und erneut ausführen


Nachdem Sie das Ergebnis _inmem ausgeführt haben, gehen Sie folgendermaßen für _ondisk ausführen:


1. Zurücksetzen der Datenbank durch Ausführen des folgenden Befehls in SSMS zu löschen, die von der vorherigen Ausführung eingefügt wurde:
```
EXECUTE Demo.usp_DemoReset;
```

2. Bearbeiten der Befehlszeile ostress.exe *_ondisk*alle *_inmem* ersetzen.

3. Führen Sie ostress.exe zum zweiten Mal und Dauer Ergebnis erfasst.

4. Zurücksetzen Sie Datenbank für verantwortlich löschen, der eine große Menge von Daten können wieder.


#### <a name="expected-comparison-results"></a>Erwartete Vergleichsergebnisse

Unsere Speichertests haben eine Leistungssteigerung **9-fache** vereinfachte erfolgen, mit Ostress auf eine Azure VM in der Azure Region der Datenbank angezeigt.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B. Installieren des Beispiels im Arbeitsspeicher Analytics


In diesem Abschnitt vergleichen Sie i/o und Statistics Verwendung Columnstore-Index im Vergleich zu herkömmlichen b-Baumstruktur.


Echtzeit-Analysen für eine OLTP-Arbeitslast empfiehlt sich oft einen Columnstore nicht gruppierten Index verwendet. Weitere Informationen finden Sie unter [Columnstore Indizes beschrieben](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Vorbereiten des Columnstore Analytics-Tests


1. Verwenden Sie Azure-Portal zum Erstellen einer neuen Datenbank AdventureWorksLT aus dem Beispiel.
 - Verwenden Sie genau diesem Namen.
 - Alle Premium-Service-Tier auswählen

2. [Sql_in-Memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) in die Zwischenablage kopieren.
 - T-SQL-Skript erstellt die erforderlichen Speicher-Objekte in der Beispieldatenbank AdventureWorksLT in Schritt 1 erstellte.
 - Das Skript erstellt die Dimensionstabelle und zwei Faktentabellen. Faktentabellen sind mit 3,5 Millionen Zeilen aufgefüllt.
 - Das Skript kann 15 Minuten dauern.

3. SSMS fügen Sie T-SQL-Skript ein, und führen Sie das Skript.
 - Spielt das **COLUMNSTORE** -Schlüsselwort in einer **CREATE INDEX** -Anweisung in:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. AdventureWorksLT auf 130-Kompatibilitätsgrad festgelegt:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Auf 130 bezieht nicht direkt im Arbeitsspeicher Features. Sondern auf 130 im Allgemeinen Abfrage schneller als 120.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Wichtige Tabellen und Indizes columnstore


- Dbo. FactResellerSalesXL_CCI ist eine Tabelle, einen Index clustered **Columnstore** , der Komprimierung auf *Datenebene* erweitert wurde.

- Dbo. FactResellerSalesXL_PageCompressed ist eine Tabelle mit einer entsprechenden regulären gruppierten Index *nur auf* komprimierten.


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Wichtige Abfragen Columnstore Index vergleichen


[Hier](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) gibt mehrere T-SQL-Abfrage Sie führen Leistungssteigerungen anzeigen. Schritt 2 im T-SQL-Skript ist ein Paar von Abfragen, die von Interesse sind. Die beiden Abfragen unterscheiden sich nur in einer Zeile:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Gruppierte Columnstore-Index ist in der FactResellerSalesXL**_CCI** -Tabelle.

T-SQL-Skript Auszug druckt Statistics i/o und Zeit für die Abfrage der einzelnen Tabellen.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Vorschau Aspekte der OLTP Speicher


[Vorschau auf 28. Oktober 2015](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/)wurde im Arbeitsspeicher OLTP-Funktionen in Azure SQL-Datenbank.


In der aktuellen Vorschau ist nur für OLTP Speicher unterstützt:

- Datenbanken, die auf ein *Premium* -Service-Tier.

- Datenbanken, die erstellt wurden, im Arbeitsspeicher OLTP-Features aktiv wurde.
 - Eine neue Datenbank unterstützt keine In-Memory OLTP aus einer Datenbank wiederhergestellt wird, die im Arbeitsspeicher OLTP-Features aktiv wurde erstellt wurde.


Im Zweifelsfall führen immer Sie folgende T-SQL SELECT zu prüfen, ob die Datenbank im Speicher OLTP unterstützt. Ein Ergebnis von **1** bedeutet, dass die Datenbank im Speicher OLTP unterstützt:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Die Abfrage gibt **1**zurück, OLTP Speicher in dieser Datenbank unterstützt und Kopie und Wiederherstellung der Datenbank erstellt basierend auf dieser Datenbank.


#### <a name="objects-allowed-only-at-premium"></a>Objekte auf Premium möglich


Enthält eine Datenbank folgende im Arbeitsspeicher OLTP-Objekte oder Typen, wird der Herabstufung der Dienstebene der Datenbank Premium Basic oder Standard nicht unterstützt. Um downgrade die Datenbank zunächst löschen Sie diese Objekte:

- Speicheroptimierte Tabellen
- Speicheroptimierte Tabellentypen
- Nativ kompilierte Module


#### <a name="other-relationships"></a>Andere Geschäftsbeziehungen


- Mithilfe einer speicherinternen OLTP-Funktionen mit Datenbanken elastische Pools wird während der Vorschau nicht unterstützt.
 - Gehen Sie folgendermaßen vor, um eine Datenbank zu verschieben, die oder elastischen Pool im Arbeitsspeicher OLTP Objekte musste:
  - 1. Löschen der Datenbank alle speicheroptimierten Tabellen Tabellentypen und nativ kompilierte T-SQL-Module
  - 2. Ändern der Dienstebene der Datenbank-Standard
  - 3. Verschieben Sie die Datenbank im elastischen Pool

- Verwendung im Arbeitsspeicher OLTP SQL Data Warehouse wird nicht unterstützt.
 - Die Columnstore Index im Speicher Analytics ist im SQL Data Warehouse unterstützt.

- Abfrage-Speicher erfasst Abfragen innerhalb systemeigene Module kompiliert nicht.

- Einige Transact-SQL-Funktionen werden nicht mit OLTP Speicher unterstützt. Dies gilt für Microsoft SQL Server und Azure SQL-Datenbank. Weitere Informationen finden Sie unter:
 - [Transact-SQL-Unterstützung für OLTP Speicher](http://msdn.microsoft.com/library/dn133180.aspx)
 - [Transact-SQL-Konstrukten von OLTP Speicher nicht unterstützt](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Nächste Schritte


- Versuchen Sie [mit im Arbeitsspeicher OLTP in einer vorhandenen SQL Azure Anwendung.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Zusätzliche Ressourcen

#### <a name="deeper-information"></a>Mehr Informationen

- [Erfahren Sie mehr über In-Memory OLTP für Microsoft SQL Server und Azure SQL-Datenbank gilt](http://msdn.microsoft.com/library/dn133186.aspx)

- [Erfahren Sie mehr über operative Echtzeitanalysen auf MSDN](http://msdn.microsoft.com/library/dn817827.aspx)

- Whitepaper auf [Allgemeine Aufgaben und Aspekte der Migration](http://msdn.microsoft.com/library/dn673538.aspx)die Arbeitslast Muster beschreibt, wo im Arbeitsspeicher OLTP häufig erhebliche Leistungssteigerungen ermöglicht.

#### <a name="application-design"></a>Anwendungsentwurf

- [Speicher OLTP (In-Memory-Optimierung)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Im Arbeitsspeicher OLTP in einer vorhandenen Azure SQL-Anwendung verwenden.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Tools

- [SQL Server Data Tools Vorschau (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), die aktuelle monatliche Version.

- [Beschreibung der Wiedergabe Markup Language (RML) Dienstprogramme für SQL Server](http://support.microsoft.com/en-us/kb/944837)

- [Monitor In - Speicher](sql-database-in-memory-oltp-monitoring.md) für OLTP Speicher.

