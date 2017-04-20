<properties
   pageTitle="Laden von Daten aus Azure BLOB-Speicher in SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Informationen Sie zum PolyBase verwenden, um Daten von Azure BLOB-Speicher in SQL Data Warehouse zu laden. Laden Sie einige Tabellen von Daten in Contoso Retail Data Warehouse-Schema."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Laden von Daten aus Azure BLOB-Speicher in SQL Data Warehouse (PolyBase)

> [AZURE.SELECTOR]
- [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Befehlen Sie PolyBase und T-SQL-in Azure SQL Data Warehouse Daten aus Azure BLOB-Speicher geladen. 

Um es einfach zu halten, lädt dieses Lernprogramm zwei Tabellen von einem öffentlichen Azure Storage Blob in Contoso Retail Data Warehouse-Schema. Zum Laden den vollständigen Datensatz führen Sie [vollständige Contoso Retail Datawarehouse geladen][] wird von Microsoft SQL Server Samples Repository.

In diesem Lernprogramm werden Sie:

1. Konfigurieren von Azure BLOB-Speicher geladen PolyBase
2. Öffentliche Daten in die Datenbank laden
3. Durchführen Sie Optimierungen, nachdem der Ladevorgang abgeschlossen ist.


## <a name="before-you-begin"></a>Bevor Sie beginnen
Führen Sie in diesem Lernprogramm benötigen Sie ein Azure-Konto, die bereits eine SQL Data Warehouse-Datenbank. Wenn Sie dies noch nicht, finden Sie unter [Erstellen einer SQL Data Warehouse][].

## <a name="1-configure-the-data-source"></a>1. Konfiguration von der Datenquelle

PolyBase verwendet externe T-SQL-Objekte definieren die Position und die externen Daten. Die externe Objektdefinitionen werden im SQL Data Warehouse gespeichert. Die Daten selbst werden extern gespeichert.

### <a name="11-create-a-credential"></a>1.1. Anmeldeinformationen erstellen

**Überspringen** Wenn Contoso öffentlichen Daten geladen werden. Sie brauchen nicht sicheren Zugriff auf die öffentlichen Daten bereits für jeden zugänglich ist.

**Überspringen Sie diesen Schritt nicht** , wenn Sie dieses Lernprogramm als Vorlage verwenden für Ihre eigenen Daten laden. Für den Zugriff auf Daten über Anmeldeinformationen mit der Anmeldeinformationen-Datenbank begrenzt erstellt das folgende Skript, und dann verwenden Sie, wenn Sie den Speicherort der Datenquelle definieren.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

Fahren Sie mit Schritt 2.

### <a name="12-create-the-external-data-source"></a>1.2. Die externe Datenquelle erstellen

Dieser Befehl [Externe Datenquelle erstellen][] auf um den Speicherort der Daten und der Typ der Daten zu speichern. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Möchten Sie Ihre Azure Blob Speichercontainer veröffentlichen, beachten Sie, dass als Datenbesitzer Sie Daten Ausgang Zuschläge berechnet Daten im Rechenzentrum verlässt. 

## <a name="2-configure-data-format"></a>2. konfigurieren Sie Daten

Daten in Textdateien Azure BLOB-Speicher, und jedes Feld ist durch ein Trennzeichen getrennt. Diese [EXTERNEN DATEIFORMAT erstellen][] Befehl an das Format der Daten in Textdateien. Die Contoso-Daten werden dekomprimiert und Pipe getrennt.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. erstellen Sie externen Tabellen

Da das Datenformat der Quell- und Datei angegeben haben, können Sie externen Tabellen erstellen. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Erstellen Sie ein Schema für die Daten. 

Erstellen Sie ein Schema, erstellen Sie die Contoso-Daten in der Datenbank speichern.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3.2. Erstellen Sie externen Tabellen. 

Führen Sie dieses Skript zum Erstellen der externen Tabellen DimProduct und FactOnlineSales. Hier definieren Spaltennamen und Datentypen, und binden und Formatieren von Azure BLOB-Speicher-Dateien. Die Definition im SQL Data Warehouse gespeichert und die Daten weiterhin in Azure Storage-Blob.

Der **Position** -Parameter wird der Ordner im Stammordner in Azure Storage Blob. Jede Tabelle wird in einem anderen Ordner.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. die Daten laden
Es gibt verschiedene Arten Zugriff auf externe Daten.  Sie können Daten direkt aus der externen Tabelle Abfragen, laden die Daten in neue Tabellen oder externe Daten vorhandene Datenbanktabellen hinzufügen.  


### <a name="41-create-a-new-schema"></a>4.1. Erstellen eines neuen Schemas

CTAS erstellt eine neue Tabelle, die Daten enthält.  Erstellen Sie zuerst ein Schema für die Contoso-Daten.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Die Daten in neue Tabellen laden

Daten von Azure BLOB-Speicher laden in eine Tabelle in der Datenbank speichern und verwenden Sie die Anweisung [CREATE TABLE AS SELECT (Transact-SQL)][] . Mit CTAS nutzt die stark typisierten externen Tabellen, die Sie gerade erstellt haben. Verwenden Sie zum Laden von Daten in neue Tabellen eine [CTAS][] Anweisung pro Tabelle. 

CTAS erstellt eine neue Tabelle und füllt sie mit den Ergebnissen einer select-Anweisung. CTAS wird die neue Tabelle dieselben Spalten und Datentypen als die Ergebnisse der select-Anweisung definiert. Wenn Sie alle Spalten aus einer externen Tabelle auswählen, werden die neue Tabelle ein Replikat der Spalten und Datentypen in der externen Tabelle.

In diesem Beispiel erstellen Sie die Dimension und die Faktentabelle als verteilte Tabellen Hashtabellen. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 die Last verfolgen

Sie können den Fortschritt der Last über dynamische Verwaltungsansichten (DMVs) verfolgen. 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5 Columnstore Kompression optimieren

SQL Data Warehouse speichert die Tabelle als gruppierter Columnstore-Index. Nach Abschluss eine Last möglicherweise einige Datenzeilen nicht in der Columnstore komprimiert.  Es gibt zahlreiche Gründe, warum dies geschieht. Weitere Informationen finden Sie unter [Columnstore Indizes verwalten][].

Zur Optimierung der Leistung und Columnstore Komprimierung nach einer Last neu erstellen Sie erzwingen Columnstore Index komprimiert alle Zeilen die Tabelle. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Weitere Informationen zur Verwaltung Columnstore Indizes finden Sie im Artikel [Columnstore Indizes verwalten][] .

## <a name="6-optimize-statistics"></a>6. Statistiken optimieren

Es empfiehlt sich, Spalte Statistik nach einem Auslastungstest erstellen. Es gibt einige Optionen für die Statistik. Spalte Statistik für jede Spalte erstellen kann es lange alle Statistiken neu erstellen dauern. Wenn Sie, dass bestimmte Spalten nicht wissen in Abfrage Prädikate, überspringen Sie Erstellen von Statistiken für diese Spalten.

Spalte Statistik für jede Spalte jeder Tabelle erstellen möchten, können Sie das Codebeispiel gespeicherte Prozedur `prc_sqldw_create_stats` im [Statistik][] -Artikel.

Im folgende Beispiel ist ein guter Ausgangspunkt für die Erstellung von Statistiken. Spalte Statistik erstellt für jede Spalte in der Dimensionstabelle und auf jede zu verknüpfende Spalte in den Faktentabellen. Sie können immer einzelne oder mehrere Spalte Statistiken andere Spalten der Faktentabelle später hinzufügen.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Entsperrt Erfolg!

Daten wurden erfolgreich in Azure SQL Data Warehouse geladen werden. Großartig

Nun können Sie Abfragen Abfragen wie folgt verwenden:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Nächste Schritte
Contoso Retail Data Warehouse Daten zu laden, verwenden Sie das Skript in Weitere Hinweise zur Entwicklung finden Sie unter [SQL Data Warehouse Development Overview][].

<!--Image references-->

<!--Article references-->
[Erstellen Sie ein SQL Datawarehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse-Anwendungsentwicklung, Überblick]: sql-data-warehouse-overview-develop.md
[Columnstore Indizes verwalten]: sql-data-warehouse-tables-index.md
[Statistik]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[EXTERNE DATENQUELLE ERSTELLEN]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[EXTERNES DATEIFORMAT ERSTELLEN]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[Erstellen der Tabelle AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Laden Sie das vollständige Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
