<properties
    pageTitle="Alle Themen für Daten Factorydienst | Microsoft Azure"
    description="Alle Themen Azure Service Namen Data Factory, die http://azure.microsoft.com/documentation/articles/, Titel und Beschreibung vorhanden."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Alle Themen für Azure Data Factory-Dienst

Dieses Thema listet jedes Thema direkt auf **Daten** Factorydienst Azure angewendet wird. Diese Webseite Schlüsselwörter können mit **STRG + F**, die aktuellen Themen finden.




## <a name="new"></a>Neu

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [Verschieben von Daten von Amazon Redshift mit Azure Data Factory](data-factory-amazon-redshift-connector.md) | Enthält Informationen zum Verschieben von Daten von Amazon Redshift mit Azure Data Factory. |
| 2 | [Verschieben von Daten von Amazon Simple Storage Service mit Azure Data Factory](data-factory-amazon-simple-storage-service-connector.md) | Enthält Informationen zum Verschieben von Daten von Amazon Simple Storage Service (S3) mit Azure Data Factory. |
| 3 | [Azure Data Factory - Assistenten kopieren](data-factory-azure-copy-wizard.md) | Informationen Sie zur Verwendung des Assistenten zum Kopieren von Daten Factory Azure Kopieren von Daten aus unterstützten Datenquellen zu senken. |
| 4 | [Lernprogramm: Erstellen Sie Ihre erste Azure Data Factory mit Data Factory REST-API](data-factory-build-your-first-pipeline-using-rest-api.md) | In diesem Lernprogramm erstellen Sie eine Beispiel Azure Data Factory-Pipeline mit Data Factory REST-API. |
| 5 | [Lernprogramm: Erstellen einer Pipeline mit .NET API mit Kopieren](data-factory-copy-activity-tutorial-using-dotnet-api.md) | In diesem Lernprogramm erstellen Sie mit einer Kopie eine Azure Data Factory-Pipeline mit .NET API. |
| 6 | [Lernprogramm: Erstellen einer Pipeline mit Kopie mit REST-API](data-factory-copy-activity-tutorial-using-rest-api.md) | In diesem Lernprogramm erstellen Sie mit einer Kopie eine Azure Data Factory-Pipeline mit REST-API. |
| 7 | [Assistent zum Kopieren von Factory](data-factory-copy-wizard.md) | Enthält Informationen zum Assistenten zum Kopieren von Factory verwenden zum Kopieren von Daten aus unterstützten Datenquellen zu senken. |
| 8 | [Data Management Gateway](data-factory-data-management-gateway.md) | Ein datengateway zum Verschieben von Daten zwischen lokalen und der Cloud einrichten. Verwenden Sie Daten-Management Gateway in Azure Data Factory Daten verschieben. |
| 9 | [Verschieben von Daten aus einer lokalen Cassandra Datenbank mit Azure Data Factory](data-factory-onprem-cassandra-connector.md) | Enthält Informationen zum Verschieben von Daten aus einer lokalen Cassandra Datenbank mit Azure Data Factory. |
| 10 | [Verschieben von Daten von MongoDB mit Azure Data Factory](data-factory-on-premises-mongodb-connector.md) | Enthält Informationen zum Verschieben von Daten von MongoDB Datenbank Azure Data Factory. |
| 11 | [Verschieben von Daten aus "Salesforce.com" mithilfe von Azure Data Factory](data-factory-salesforce-connector.md) | Enthält Informationen zum Verschieben von Daten aus "Salesforce.com" mithilfe von Azure Data Factory. |


## <a name="updated-articles-data-factory"></a>Aktualisierte Artikel Data Factory

Dieser Abschnitt enthält Artikel, die kürzlich aktualisiert wurden, wurde das Update Groß oder erheblich. Für jede aktualisierte Artikel grober Ausschnitt hinzugefügten Abzug Text angezeigt. Die Artikel wurden innerhalb des Datumsbereichs **2016-08-22** **2016 10**05 aktualisiert.

| &nbsp; | Artikel | Aktualisierter Text, Ausschnitt | Aktualisiert, wenn |
| --: | :-- | :-- | :-- |
| 12 | [Azure Data Factory - .NET API-Protokoll](data-factory-api-change-log.md) | Dieser Artikel enthält Informationen über Änderungen Azure Data Factory SDK in einer bestimmten Version. Finden Sie das neueste NuGet-Paket für Azure Data Factory hier (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **Version 4.11.0** -Erweiterungen: folgende verknüpften Serviceartikel wurde hinzugefügt: - OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) - AmazonRedshiftLinkedService (https://msdn.microsoft.com/library/mt765121.aspx) - AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) folgende Dataset hinzugefügt wurden: - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) - AmazonS3Dataset (https://msdn.microsoft.com/library/mt765112.aspx) Kopie Quelltypen hinzugefügt wurden :-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx) **Version 4.10.0** / TextFormat wurden die folgenden optionalen Eigenschaften hinzugefügt:-Ski | 2016-09-22 |
| 13 | [Verschieben von Daten zu und von Azure Blob mithilfe von Azure Data Factory](data-factory-azure-blob-connector.md) |  / CopyBehavior / des Kopierverhaltens definiert, wenn die Quelle BlobSource oder Dateisystem.  / **PreserveHierarchy:** behält die Hierarchie im Zielordner. Der relative Pfad der Quelldatei Quellordner entspricht den relativen Pfad der Zieldatei zum Zielordner... Br /... Br /. **FlattenHierarchy:** alle Dateien aus dem Quellordner sind auf der ersten Ebene des Zielordners. Die Zieldateien haben Namen automatisch generiert. .b /... Br /. **MergeFiles: (Standard)** führt alle Dateien aus dem Quellordner in eine Datei. Datei-Blob-Name angegeben wird, wäre der Namen der zusammengeführten Datei angegebenen Namen; Andernfalls wäre automatisch generierten Dateinamen.  / Nicht **BlobSource** dieser beiden Eigenschaften auch Abwärtskompatibilität unterstützt. / **TreatEmptyAsNull**: Gibt an, ob null oder eine leere Zeichenfolge als null-Wert behandelt. / **SkipHeaderLineCount** - gibt an, wie viele Zeilen übersprungen werden müssen. Gilt nur beim Eingabedatasets TextFormat verwendet. Entsprechend unterstützt **BlobSink** ten | 2016-09-28 |
| 14 | [Erstellen von vorhersehbaren Rohrleitungen Azure Computer lernen](data-factory-azure-ml-batch-execution-activity.md) | **Webdienst erfordert mehrere Eingaben** Wenn der Webdienst mehrere Eingaben akzeptiert, verwenden Sie die **WebServiceInputs** -Eigenschaft anstelle von **WebServiceInput**. Datasets, die von der **WebServiceInputs** verwiesen wird, muss auch in Aktivität **Eingaben**enthalten. In das Experiment Azure ML haben Web Service Eingangs- und Ausgangsports und globale Parameter Standardnamen ("input1", "input2"), die Sie anpassen können. Die Namen für WebServiceInputs und WebServiceOutputs sowie GlobalParameters müssen exakt die Namen in den Experimenten. Sie können die Anforderungsnutzlast Beispiel auf Batch Execution-Hilfeseite für Ihren Azure ML Endpunkt erwartende Zuordnung überprüfen anzeigen.    {"Name": "PredictivePipeline", "Eigenschaften": {"Beschreibung": "AzureML Modell verwenden", "Aktivitäten": {"Name": "MLActivity", "Typ": "AzureMLBatchExecution", "Beschreibung": "Vorhersage Analyse Batch-input", "Eingaben": {"Name": "inputDataset1"}, {"Name": "InputDatase | 2016-09-13 |
| 15 | [Kopieren Sie Aktivität Performance und tuning guide](data-factory-copy-activity-performance.md) | 1. **Einrichten einer Baseline**. Während der Entwicklungsphase mit Kopieraktivität gegen eine repräsentative Datenbeispiel Testen der Pipeline. Data Factory schneiden Modell (Daten-Factory-scheduling-und-execution.md Time-series-datasets-and-data-slices) können Sie die Datenmenge beschränken, arbeiten Sie mit.  Sammeln Sie Ausführungszeit und Leistungsmerkmale mit **Überwachung und Anwendung**. Wählen Sie auf Ihrer Homepage Data Factory **Überwachen und verwalten** . Wählen Sie in der Strukturansicht das **ausgabedataset**. In der **Aktivität** Aktivität der Kopie ausführen. **Aktivität Windows** Listet die Aktivitätsdauer kopieren und die Größe der Daten, die kopiert werden. Der Durchsatz wird in **Windows Explorer Aktivität**aufgeführt. Erfahren Sie mehr über die app überwachen und verwalten Sie Azure Data Factory Rohrleitungen mit Überwachung und Management-Anwendung (Daten-Factory-Monitor-Management-app.md).  ! Führen Sie Details (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn Aktivität | 2016-09-27 |
| 16 | [Lernprogramm: Erstellen einer Pipeline mit Kopie mit Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Beachten Sie folgende Punkte:- **Typ** Dataset auf **AzureBlob**festgelegt ist.     - **Nameverknüpfterdienst** auf **AzureStorageLinkedService**festgelegt ist. Dieser verknüpften Dienst erstellt in Schritt2.     - **Ordnerpfad** **Adftutorial** Container festgelegt ist. Sie können auch den Namen eines Blob innerhalb des Ordners mit der **FileName** -Eigenschaft. Da Sie nicht den Namen des BLOBs angeben, gilt die Daten alle Blobs im Container als eingegebenen Daten.    - **Typ** soll **TextFormat** - es gibt zwei Felder, in die Text-Datei ΓÇô **Vorname** und **Nachname** ΓÇô getrennt durch ein Komma (**ColumnDelimiter**) - **Verfügbarkeit** soll **stündlich** (** **Stunde** festgelegt ist** und **Intervall** auf **1**festgelegt ist). Daher sucht Data Factory Eingabedaten stündlich im Stammordner der BLOB-Container (**Adftutorial**) angegebenen.  Wenn Sie einen **Dateinamen** für **Eingabedatasets** angeben, werden alle Dateien/Blobs aus dem Eingabeordner (**FolderPath**) consid | 2016-09-29 |
| 17 | [Erstellen, überwachen und Verwalten von Azure Data Fabriken Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Notieren Sie die ID und das Kennwort (geheimen) und in der exemplarischen Vorgehensweise verwenden. **Abrufen von Azure-Abonnement und Mieter IDs** Wenn Sie keine Version von PowerShell auf Ihrem Computer installiert haben, führen Sie Schritte zum Installieren und Konfigurieren von Azure PowerShell (... / Powershell installieren configure.md) Artikel installieren. 1. Starten Sie Azure PowerShell, und führen Sie den folgenden Befehl 2. Führen Sie den folgenden Befehl, und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei Azure-Portal.         Login-AzureRmAccount haben Sie nur ein Azure-Abonnement mit diesem Konto verknüpft ist, müssen Sie nicht die folgenden zwei Schritte durchführen. 3. Führen Sie den folgenden Befehl an alle Abonnements für dieses Konto.       Get-AzureRmSubscription 4. Führen Sie den folgenden Befehl Abonnements wählen Sie arbeiten. Ersetzen Sie **NameOfAzureSubscription** durch den Namen Ihres Azure-Abonnements.       Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription / AzureRmCo festlegen | 2016-09-14 |
| 18 | [Pipelines und Aktivitäten in Azure Data Factory](data-factory-create-pipelines.md) |       "start": "2016-07-12T00:00:00Z", "Ende": "2016-07-13T00:00:00Z"}} Beachten Sie folgende Punkte: Abschnitt Aktivitäten ist nur eine Aktivität vom **Typ** **Kopieren**soll. / Eingabemethoden für die Aktivität, **InputDataset** und Ausgabe für die Aktivität ist auf **OutputDataset**festgelegt. In Abschnitt **TypeProperties** **BlobSource** als Quelltyp angegeben und **SqlSink** Senke Dateityp angegeben. Diese Pipeline erstellen umfassende, finden Sie unter Tutorial: Daten aus dem BLOB-Speicher mit SQL-Datenbank (Data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Beispiel Transformation pipeline** In der folgenden Beispiel-Pipeline ist eine Aktivität vom Typ **HDInsightHive** Abschnitt **Aktivitäten** . In diesem Beispiel transformiert HDInsight Struktur Aktivität (Daten-Factory-Struktur-activity.md) Daten von Azure Blob-Speicher durch eine Strukturdatei Skript in einem Cluster Azure HDInsight Hadoop ausführen.  {"Name": "TransformPipeline", "p | 2016-09-27 |
| 19 | [Transformieren von Daten in Azure Data Factory](data-factory-data-transformation-activities.md) | Data Factory unterstützt folgende Daten Transformation, die hinzugefügt werden können (Daten-Factory-erstellen-pipelines.md) entweder einzeln Rohrleitungen oder mit anderen Aktivitäten verkettet. .  AZURE. Hinweis eine exemplarische Vorgehensweise mit einer schrittweisen Anleitung finden Sie unter Erstellen einer Pipeline Struktur Transformation (Data-factory-build-your-first-pipeline.md) Artikel. **HDInsight-Struktur-Aktivität** HDInsight Struktur Aktivität in einer Pipeline Data Factory führt Abfragen eigene Struktur oder bei Bedarf Windows, Linux-basierte HDInsight-Cluster. Siehe Hive-Aktivität (Daten-Factory-Struktur-activity.md) Weitere Informationen zu dieser Aktivität. **HDInsight Schwein Aktivität** HDInsight Schwein Aktivität in einer Pipeline Data Factory führt Schwein Abfragen selbst oder bei Bedarf Windows, Linux-basierte HDInsight-Cluster. Siehe Schwein Aktivität (Daten-Factory-Schwein-activity.md) Weitere Informationen zu dieser Aktivität. **HDInsight MapReduce-Aktivität** HDInsight MapReduce-Aktivität in einer Pipeline Data Factory führt MapReduce Programme auf | 2016-09-26 |
| 20 | [Data Factory Planung und Ausführung](data-factory-scheduling-and-execution.md) | CopyActivity2 wird nur ausgeführt, wenn die CopyActivity1 erfolgreich und Datensatz2 verfügbar ist. Hier ist Beispiel Pipeline JSON: {"Name": "ChainActivities", "Eigenschaften": {"Beschreibung": "Run Aktivitäten nacheinander" "Aktivitäten": {"Typ": "Kopieren", "TypeProperties": {"Quelle": {"Typ": "BlobSource"}, "Weiterleitungspostfach": {"Typ": "BlobSink", "CopyBehavior": "PreserveHierarchy", "WriteBatchSize": 0, "WriteBatchTimeout": "00: 00:00"}}, "Inputs": {"Name": "Dataset1"}, "Ausgaben": {"Name": "Datensatz2"}, "Policy": {"Timeout": "01: 00:00"}, "Planer": {"Häufigkeit": "Stunde", "Interval": 1}, "Name": "CopyFromBlob1ToBlob2", "Beschreibung": "Daten von einem Blob zu einem anderen"}, {"Typ": "Kopieren", "TypeProperties": {"Quelle": {"Typ": "BlobSource"}, "Empfänger" : {"Typ": "BlobSink", "WriteBatchSize": 0, "WriteBatchTimeout": "00: 00:00"}}, "in | 2016-09-28 |





## <a name="tutorials"></a>Lernprogramme

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 21 | [Lernprogramm: Erstellen Sie Ihre erste Pipeline Hadoop Cluster mit Daten](data-factory-build-your-first-pipeline.md) | In diesem Lernprogramm Azure Data Factory wird das Erstellen und planen eine Daten-Factory, die Daten mit Struktur-Skript in einem Cluster Hadoop veranschaulicht. |
| 22 | [Lernprogramm: Erstellen Sie Ihrer erste Azure Data Factory mit Azure-Ressourcen-Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md) | In diesem Lernprogramm erstellen Sie eine Beispiel Azure Data Factory-Pipeline mit einer Azure-Ressourcen-Manager-Vorlage. |
| 23 | [Lernprogramm: Erstellen Sie Ihrer erste Azure Data Factory mit Azure-portal](data-factory-build-your-first-pipeline-using-editor.md) | In diesem Lernprogramm erstellen Sie eine Beispiel Azure Data Factory-Pipeline mit Factory im Azure-Portal. |
| 24 | [Lernprogramm: Erstellen Sie Ihre erste Azure Data Factory mit Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md) | In diesem Lernprogramm erstellen Sie eine Beispiel Azure Data Factory-Pipeline mit Azure PowerShell. |
| 25 | [Lernprogramm: Erstellen Sie Ihre erste Azure Data Factory mit Microsoft Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) | In diesem Lernprogramm erstellen Sie eine Probe Azure Data Factory-Pipeline mit Visual Studio. |
| 26 | [Lernprogramm: Erstellen einer Pipeline mit Kopie mit Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) | In diesem Lernprogramm erstellen Sie eine Azure Data Factory-Pipeline mit einer Kopie von mit dem Werk in Azure-Portal. |
| 27 | [Lernprogramm: Erstellen einer Pipeline mit Azure PowerShell mit Kopieren](data-factory-copy-activity-tutorial-using-powershell.md) | In diesem Lernprogramm erstellen Sie mit einer Kopie eine Azure Data Factory-Pipeline mit Azure PowerShell. |
| 28 | [Lernprogramm: Erstellen einer Pipeline mit Kopie mit Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) | In diesem Lernprogramm erstellen Sie mit einer Kopie eine Azure Data Factory-Pipeline mit Visual Studio. |
| 29 | [Daten Sie aus dem BLOB-Speicher mit SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | In diesem Lernprogramm wird veranschaulicht, wie Kopieraktivität in Azure Data Factory-Pipeline mit Daten aus dem BLOB-Speicher mit SQL-Datenbank kopieren. |
| 30 | [Lernprogramm: Erstellen einer Pipeline mit Kopie mithilfe des Assistenten zum Kopieren von Factory](data-factory-copy-data-wizard-tutorial.md) | In diesem Lernprogramm erstellen Sie eine Azure Data Factory-Pipeline mit einer Kopie mithilfe des Assistenten zum Kopieren von Daten Factory unterstützt |



## <a name="data-movement"></a>Datentransfer

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 31 | [Verschieben von Daten zu und von Azure Blob mithilfe von Azure Data Factory](data-factory-azure-blob-connector.md) | Informationen Sie zum Kopieren von BLOB-Daten in Azure Data Factory. Unser Beispiel: Kopieren von Daten zwischen Azure BLOB-Speicher und Azure SQL-Datenbank. |
| 32 | [Verschieben von Daten zu und von Azure See Datenspeicher Azure Data Factory](data-factory-azure-datalake-connector.md) | Erfahren Sie, wie Daten in Azure See Datenspeicher Azure Data Factory |
| 33 | [Verschieben von Daten zu und von DocumentDB mit Azure Data Factory](data-factory-azure-documentdb-connector.md) | Erfahren Sie, wie Daten und Azure DocumentDB Auflistung Azure Data Factory verschieben |
| 34 | [Verschieben von Daten zu und von Azure SQL-Datenbank mit Azure Data Factory](data-factory-azure-sql-connector.md) | Informationen Sie zum Verschieben von Daten mithilfe von Azure Data Factory Azure SQL-Datenbank. |
| 35 | [Verschieben von Daten zu und von Azure SQL Data Warehouse mithilfe von Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) | Erfahren Sie, wie Daten in Azure SQL Data Warehouse mithilfe von Azure Data Factory |
| 36 | [Verschieben von Daten zu und von Azure Tabelle Azure Data Factory](data-factory-azure-table-connector.md) | Informationen Sie zum Verschieben von Daten in Azure Table Storage mit Azure Data Factory. |
| 37 | [Kopieren Sie Aktivität Performance und tuning guide](data-factory-copy-activity-performance.md) | Erfahren Sie mehr über wichtige Faktoren, die Leistung des Datentransfers in Azure Data Factory beim Kopieren-Aktivität verwenden. |
| 38 | [Verschieben von Daten mithilfe von Kopieraktivität](data-factory-data-movement-activities.md) | Datentransfer im Data Factory Rohrleitungen lernen: Datenmigration zwischen Cloud-Speicher und zwischen einem lokalen Speicher und einen Cloud-Speicher. Verwenden Sie Kopieraktivität. |
| 39 | [Versionshinweise für Data Management Gateway](data-factory-gateway-release-notes.md) | Data Management Gateway Geschichte-Versionsinformationen |
| 40 | [Verschieben von Daten von lokalen bietet mit Azure Data Factory](data-factory-hdfs-connector.md) | Enthält Informationen zum Verschieben von Daten von lokalen bietet Azure Data Factory verwenden. |
| 41 | [Überwachen und Verwalten von Azure Data Factory Rohrleitungen neue Überwachung und Anwendung](data-factory-monitor-manage-app.md) | Enthält Informationen zum Verwenden von Überwachung und Management App überwachen und Verwalten von Azure Data Factorys und Rohrleitungen. |
| 42 | [Verschieben von Daten zwischen lokalen und Cloud mit Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) | Ein datengateway zum Verschieben von Daten zwischen lokalen und der Cloud einrichten. Verwenden Sie Daten-Management Gateway in Azure Data Factory Daten verschieben. |
| 43 | [Verschieben von Daten mithilfe von Azure Data Factory ein OData source](data-factory-odata-connector.md) | Enthält Informationen zum Verschieben von Daten von OData-Quellen mithilfe von Azure Data Factory. |
| 44 | [Verschieben von Daten aus ODBC-Datenspeicher mit Azure Data Factory](data-factory-odbc-connector.md) | Enthält Informationen zum Verschieben von Daten aus ODBC-Datenspeichern mit Azure Data Factory. |
| 45 | [Verschieben von Daten von DB2 mit Azure Data Factory](data-factory-onprem-db2-connector.md) | Weitere Informationen über das Verschieben von Daten von Azure Data Factory mit DB2-Datenbank |
| 46 | [Verschieben von Daten in und aus einem lokalen Dateisystem mit Azure Data Factory](data-factory-onprem-file-system-connector.md) | Erfahren Sie, wie Daten in und aus einem lokalen Dateisystem mit Azure Data Factory. |
| 47 | [Verschieben von Daten von MySQL mit Azure Data Factory](data-factory-onprem-mysql-connector.md) | Enthält Informationen zum Verschieben von Daten von MySQL-Datenbank mithilfe von Azure Data Factory. |
| 48 | [Verschieben von Daten in lokalen Oracle mit Azure Data Factory](data-factory-onprem-oracle-connector.md) | Informationen Sie zum Verschieben der Daten nach Oracle Datenbank-lokal mit Azure Data Factory. |
| 49 | [Verschieben von Daten von PostgreSQL mit Azure Data Factory](data-factory-onprem-postgresql-connector.md) | Enthält Informationen zum Verschieben von Daten von PostgreSQL Datenbank Azure Data Factory. |
| 50 | [Verschieben von Daten von Azure Data Factory mit Sybase](data-factory-onprem-sybase-connector.md) | Enthält Informationen zum Verschieben von Daten von Sybase-Datenbank mithilfe von Azure Data Factory. |
| 51 | [Verschieben von Daten von Teradata mit Azure Data Factory](data-factory-onprem-teradata-connector.md) | Erfahren Sie mehr über Teradata Connector für Data Factory-Dienst, der Daten von Teradata Datenbank verschieben können |
| 52 | [Verschieben von Daten und SQL Server lokal oder auf IaaS (Azure VM) mit Azure Data Factory](data-factory-sqlserver-connector.md) | Enthält Informationen zum Verschieben von Daten ist für lokale SQL Server-Datenbank oder in einer Azure VM mit Azure Data Factory. |
| 53 | [Verschieben von Daten von einer Tabelle Webquelle mit Azure Data Factory](data-factory-web-table-connector.md) | Enthält Informationen zum Verschieben von Daten von lokalen Tabelle auf einer Webseite mithilfe von Azure Data Factory. |



## <a name="data-transformation"></a>Datentransformation

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 54 | [Erstellen von vorhersehbaren Rohrleitungen Azure Computer lernen](data-factory-azure-ml-batch-execution-activity.md) | Beschreibt das Erstellen von Azure Data Factory mit Azure Machine Learning vorhersehbaren Pipelines erstellen |
| 55 | [Berechnen von verknüpften Diensten](data-factory-compute-linked-services.md) | Erfahren Sie mehr über Compute-Umgebungen, Sie in Azure Data Factory Pipelines zum Transformieren-Prozess Daten verwenden können. |
| 56 | [Verarbeiten umfangreicher Datasets mit Data Factory und Batch](data-factory-data-processing-using-batch.md) | Beschreibt, wie große Datenmengen in eine Azure Data Factory-Pipeline mit parallelen Verarbeitungskapazität von Azure Batch verarbeiten. |
| 57 | [Transformieren von Daten in Azure Data Factory](data-factory-data-transformation-activities.md) | Informationen Sie zum Transformieren von Daten oder Daten in Azure Data Factory Hadoop, Computerlernen oder in Azure Data Lake Analytics. |
| 58 | [Hadoop Streaming-Aktivität](data-factory-hadoop-streaming-activity.md) | Erfahren Sie Hadoop Streaming-Aktivität in einer Azure Daten Verwendung Hadoop Streaming Programme auf einen auf-Anforderung/Ihre eigenen HDInsight-Cluster ausführen. |
| 59 | [Hive-Aktivität](data-factory-hive-activity.md) | Erfahren Sie die Hive-Aktivität in einer Azure Daten Verwendung Hive-Abfragen in einem auf-Anforderung/Ihrem eigenen HDInsight Cluster ausgeführt. |
| 60 | [Data Factory MapReduce Programme aufrufen](data-factory-map-reduce.md) | Erfahren Sie, wie Daten durch Programme MapReduce in Azure HDInsight-Cluster aus einer Fabrik Azure Daten verarbeiten. |
| 61 | [Schwein-Aktivität](data-factory-pig-activity.md) | Erfahren Sie wie Schwein Aktivität in einer Azure Daten verwenden, einen auf-Anforderung/Ihre eigenen HDInsight-Cluster Schwein Skripts auszuführen. |
| 62 | [Data Factory Spark Programme aufrufen](data-factory-spark.md) | Informationen Sie zum Spark Programme eine Azure Data Factory mithilfe der MapReduce-Aktivität aufgerufen. |
| 63 | [SQL Server gespeicherte Prozedur-Aktivität](data-factory-stored-proc-activity.md) | Erfahren Sie SQL Server gespeicherte Prozedur Aktivität Verwendung zum Aufrufen einer gespeicherten Prozedur in einer Azure SQL-Datenbank oder Azure SQL Data Warehouse von Data Factory-Pipeline. |
| 64 | [Benutzerdefinierte Aktivitäten in Azure Data Factory-pipeline](data-factory-use-custom-activities.md) | Informationen Sie zum Erstellen von benutzerdefinierten Aktivitäten und deren Verwendung in Azure Data Factory-Pipeline. |
| 65 | [Skript U SQL Azure Data Lake Analytics von Azure Data Factory](data-factory-usql-activity.md) | Informationen Sie zum Prozessdaten U-SQL-Skripts See Datenanalyse Azure Compute-Dienst ausführen. |



## <a name="samples"></a>Beispiele

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 66 | [Azure Data Factory - Beispiele](data-factory-samples.md) | Enthält Details zu liefern Beispiele mit dem Azure Data Factory-Dienst. |



## <a name="use-cases"></a>Anwendungsbeispiele

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 67 | [Fallstudien](data-factory-customer-case-studies.md) | Erfahren Sie, wie einige unserer Kunden Azure Data Factory verwendet haben. |
| 68 | [Anwendungsfall - Kunden Profiling](data-factory-customer-profiling-usecase.md) | Erfahren Sie, wie Azure Data Factory zum Erstellen eines datengesteuerten Workflows (Pipeline) so Gaming-Kunden verwendet wird. |
| 69 | [Anwendungsfall - Produkten](data-factory-product-reco-usecase.md) | Lernen Sie ein Anwendungsfall zusammen mit anderen Diensten mithilfe von Azure Data Factory implementiert. |



## <a name="monitor-and-manage"></a>Überwachen und verwalten

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 70 | [Überwachen und Verwalten von Azure Data Factory Rohrleitungen](data-factory-monitor-manage-pipelines.md) | Erfahren Sie, wie Sie Azure-Portal und Azure PowerShell zum Überwachen und Verwalten von Azure Data Factorys und Pipelines, die Sie erstellt haben. |



## <a name="sdk"></a>SDK

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 71 | [Azure Data Factory - .NET API-Protokoll](data-factory-api-change-log.md) | Beschreibt die Änderungen, Erweiterungen, Updates usw.... eine spezifische Version der .NET API für Azure Data Factory. |
| 72 | [Erstellen, überwachen und Verwalten von Azure Data Fabriken Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Informationen Sie zum programmgesteuert erstellen, überwachen und Verwalten von Azure Data Factorys mit Data Factory SDK. |
| 73 | [Azure Data Factory-Entwicklerreferenz](data-factory-sdks.md) | Informationen Sie zu verschiedenen Methoden zum Erstellen, überwachen und Verwalten von Azure Data Factorys |



## <a name="miscellaneous"></a>Sonstige

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 74 | [Azure Data Factory - häufig gestellte Fragen](data-factory-faq.md) | Häufig gestellte Fragen zur Azure Data Factory. |
| 75 | [Azure Data Factory - Funktionen und Variablen](data-factory-functions-variables.md) | Enthält eine Liste von Azure Data Factory Funktionen und Variablen |
| 76 | [Azure Data Factory - Benennungsregeln](data-factory-naming-rules.md) | Beschreibt die Benennungsregeln für Data Factory Entitäten. |
| 77 | [Problembehandlung bei Data Factory](data-factory-troubleshoot.md) | Informationen Sie zu Problemen mit Azure Data Factory. |

