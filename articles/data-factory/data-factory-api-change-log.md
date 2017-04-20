<properties 
    pageTitle="Data Factory - .NET API Protokoll | Microsoft Azure" 
    description="Beschreibt die Änderungen, Erweiterungen, Updates usw.... eine spezifische Version der .NET API für Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure Data Factory - .NET API-Protokoll 
Dieser Artikel enthält Informationen über Änderungen Azure Data Factory SDK in einer bestimmten Version. Finden Sie das neueste NuGet-Paket für Azure Data Factory [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Version 4.11.0
Leistungsmerkmale:

- Folgende verknüpften Serviceartikel wurden hinzugefügt:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Folgende Dataset wurden hinzugefügt: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- Copy Quelltypen wurden hinzugefügt:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Version 4.10.0
- TextFormat haben die folgenden optionalen Eigenschaften hinzugefügt:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- Folgende verknüpften Serviceartikel wurden hinzugefügt:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Folgende Dataset wurden hinzugefügt:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- Copy Quelltypen wurden hinzugefügt:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- AzureMLBatchExecutionActivity [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) Eigenschaft hinzufügen 
    - Aktivieren Sie mehrere Web Service Eingaben Versuch Azure Machine Learning übergeben


## <a name="version-491"></a>4.9.1 Version

### <a name="bug-fix"></a>Fehlerkorrektur

- WebApi-Authentifizierung für [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx)setzt.

## <a name="version-490"></a>Version 4.9.0

### <a name="feature-additions"></a>Leistungsmerkmale

- CopyActivity [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) und [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) -Eigenschaften hinzugefügt. Das Feature Einzelheiten finden Sie unter [Kopieren bereitgestellt](data-factory-copy-activity-performance.md#staged-copy) .


### <a name="bug-fix"></a>Fehlerkorrektur

- Stellen Sie eine Überladung der [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) -Methode, die eine [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) -Instanz akzeptiert.
- [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) und [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) in CopySink als optional markiert.

## <a name="version-480"></a>Version 4.8.0

### <a name="feature-additions"></a>Leistungsmerkmale
- Aktivitätstyp Kopie ermöglicht die Optimierung der Leistung haben die folgenden optionalen Eigenschaften hinzugefügt:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Version 4.7.0

### <a name="feature-additions"></a>Leistungsmerkmale
* Neue StorageFormat Typ [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) , optimierte Zeile Spaltenformat (ORK) Dateien hinzugefügt.
* SqlDWSink [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) und PolyBaseSettings-Eigenschaften hinzugefügt.
    * Ermöglicht die Verwendung von PolyBase Kopieren von Daten in SQL Data Warehouse.

## <a name="version-461"></a>Version 4.6.1

### <a name="bug-fixes"></a>Fehlerkorrekturen
* HTTP-Anforderung mit Aktivität Windows behebt.
    * Ressource-Gruppennamen und den Namen der Factory entfernt aus der Anforderungsnutzlast.

## <a name="version-460"></a>Version 4.6.0

### <a name="feature-additions"></a>Leistungsmerkmale

- [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx)haben die folgenden Eigenschaften hinzugefügt:
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Datasets](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx)haben die folgenden Eigenschaften hinzugefügt:
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Hinzugefügte neue [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) Typ [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) Datasets definieren, deren Daten im JSON-Format. 

## <a name="version-450"></a>Version 4.5.0

### <a name="feature-additions"></a>Leistungsmerkmale
* Hinzugefügte [Liste Vorgänge für Aktivitätsfenster](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Zusätzliche Methoden um Aktivität Windows Filter basierend auf der Entität (d. h. Daten Fabriken, Datasets, Pipelines und Aktivitäten).
* Folgende verknüpften Serviceartikel wurden hinzugefügt: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Folgende Dataset wurden hinzugefügt: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Copy Quelltypen wurden hinzugefügt:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Version 4.4.0

### <a name="feature-additions"></a>Leistungsmerkmale

- Folgenden verknüpften Diensttyp als Datenquellen hinzugefügt wurde und Channelsenken für Aktivitäten kopieren:
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Grundlegende Informationen und Beispiele finden Sie unter [Azure Storage SAS verknüpften Serviceartikel](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) . 

## <a name="version-430"></a>Version 4.3.0

### <a name="feature-additions"></a>Leistungsmerkmale

- Verknüpfte Dienst Typen Hafen wurde als Datenquellen für Aktivitäten kopieren:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Weitere [Verschieben von Daten bietet Data Factory verwenden](data-factory-hdfs-connector.md) grundlegende Informationen und Beispiele. 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Weitere [Verschieben von ODBC Daten speichert Azure Data Factory verwenden](data-factory-odbc-connector.md) grundlegende Informationen und Beispiele. 

## <a name="version-420"></a>Version 4.2.0

### <a name="feature-additions"></a>Leistungsmerkmale

- Der folgenden neuen Aktivitätstyp hinzugefügt: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Details zur Aktivität finden Sie unter [Aktualisieren von Azure ML Modelle Ressource Aktualisierungsaktivitäten](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Die [AzureMLLinkedService-Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx)wurde eine neue optionale Eigenschaft [UpdateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) hinzugefügt. 
- Eigenschaften [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) und [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) wurden die [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) -Klasse hinzugefügt. 
- Konfiguration des Timeouts für Clientaufrufe Data Factory-Service möglich. 


## <a name="version-410"></a>Version 4.1.0

### <a name="feature-additions"></a>Leistungsmerkmale
* Folgende verknüpften Serviceartikel wurden hinzugefügt: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Folgende Aktivitäten wurden hinzugefügt: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Folgende Dataset wurden hinzugefügt: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Quelle und Empfänger folgende Kopie Aktivität wurden hinzugefügt:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Version 4.0.1

### <a name="breaking-changes"></a>Grundlegend geändert
Die folgenden Klassen wurden umbenannt. Die neuen Namen waren die ursprünglichen Namen von 4.0.0 freizugeben. 
 
Namen in 4.0.0 | 4.0.1 benennen
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Version 4.0.0

### <a name="breaking-changes"></a>Grundlegend geändert



- Die folgenden Klassen/Schnittstellen wurden umbenannt.

| Alter name | Neuer name |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Tabelle | [DataSet](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- **Die Methoden** geben jetzt ausgelagerter Ergebnisse zurück. Die Antwort nicht leere **NextLink** Eigenschaft enthält, muss die Clientanwendung weiterhin die Seite abrufen, bis alle Seiten zurückgegeben werden.  Hier ist ein Beispiel: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Liste** Pipeline API gibt nur die Zusammenfassung einer Pipeline statt Einzelheiten. Aktivitäten in eine Pipeline enthalten beispielsweise nur Namen und Typ.

### <a name="feature-additions"></a>Leistungsmerkmale
- Die [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) -Klasse unterstützt zwei neue Eigenschaften **SliceIdentifierColumnName** und **SqlWriterCleanupScript**, idempotente Kopie Azure SQL Data Warehouse unterstützen. Finden Sie den [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) , insbesondere [Mechanismus 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) und [2 Mechanismus](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) Abschnitte für Details über diese Eigenschaften.

- Wir unterstützen nun die gespeicherte Prozedur als Teil der Kopie gegen Azure SQL-Datenbank und Azure SQL Data Warehouse ausgeführt. Die [SQLSource aus](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) und [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) -Klassen haben folgende Eigenschaften: **SqlReaderStoredProcedureName** und **StoredProcedureParameters**. Weitere Informationen zu diesen Eigenschaften finden Sie unter Artikel [Azure SQL-Datenbank](data-factory-azure-sql-connector.md#sqlsource) und [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) auf Azure.com.  