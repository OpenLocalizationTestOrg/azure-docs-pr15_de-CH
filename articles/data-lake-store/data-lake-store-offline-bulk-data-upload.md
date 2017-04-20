<properties
   pageTitle="Laden große Datenmengen in See Datenspeicher mit offline-Methoden | Microsoft Azure"
   description="Verwenden von AdlCopy Tool zum Kopieren von Daten von Azure Storage Blobs See Datenspeicher"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Verwenden Sie Azure Import / Export-Dienst für offline-Kopien der Daten auf See Datenspeicher

Dieser Artikel enthält Informationen zum Kopieren von großen Datasets (> 200GB) in einem Azure See Datenspeicher Offlinekopie Methoden wie [Azure Import/Export](../storage/storage-import-export-service.md). Insbesondere wird die Datei als Beispiel in diesem Artikel verwendet 339,420,860,416 Byte rund 319GB auf der Festplatte. Nennen Sie diese Datei 319GB.tsv.

Azure Import/Export-Dienst ermöglicht sichere Datenmengen Azure BLOB-Speicher übertragen von Festplatten ein Azure-Rechenzentrum Versand.

## <a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Storage-Konto**.

- **Azure Data Lake Analytics ein (optional)** - Siehe [Einstieg in Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) Anleitung See Datenspeicher-Konto erstellen.


## <a name="preparing-the-data"></a>Vorbereiten der Daten

Vor der Verwendung des Import/Export, müssen wir die Datendatei übertragen **in Kopien weniger als 200 GB sind** Größe aufteilen. Ist Import-Tool funktioniert nicht mit mehr als 200 GB. Entspricht, in diesem Lernprogramm teilen wir die Datei in Einheiten von 100GB Bytes. Sie können dazu einfach [Cygwin](https://cygwin.com/install.html). Cygwin unterstützt Linux-Befehl ermöglicht Benutzern einfach. In unserem Fall verwenden wir den folgenden Befehl ein.

    split -b 100m 319GB.tsv

Diese Teilung erstellt Dateien mit dem Namen.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Bereiten Sie Festplatten mit Daten

Anweisungen Sie in [Azure Import/Export-Dienst verwenden](../storage/storage-import-export-service.md) (Abschnitt **Vorbereiten Ihrer Laufwerke** ) Festplatte vorzubereiten. Hier ist insgesamt wie das Laufwerk vorbereiten.

1. Bereitstellen einer Festplatte, die Anforderungen für Import/Export Auzre verwendet werden.

2. Identifiziert ein Azure Storage-Konto, die Daten werden, einmal kopiert es Si Azure Data Center ausgeliefert.

3. Verwenden Sie [Azure Import/Export Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), Befehlszeilen-Dienstprogramm. Hier ist ein Beispiel-Ausschnitt zur Verwendung des Tools.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Weitere Beispiel Ausschnitte zur Verwendung von **Azure Import/Export Tool**finden Sie unter [Azure Import/Export-Dienst verwenden](../storage/storage-import-export-service.md) .

4. Der obige Befehl erstellt eine Journaldatei an der angegebenen Position. Diese Journaldatei verwendet einen Importauftrag aus dem [Azure-Verwaltungsportal](https://manage.windowsazure.com)zu erstellen.

## <a name="create-an-import-job"></a>Erstellen Sie einen Importauftrag

Sie können jetzt einen Importauftrag mit der bei [Azure Import/Export-Dienst verwenden](../storage/storage-import-export-service.md) (Abschnitt **Erstellen des Importauftrags** ) erstellen. Für dieses Projekt importieren Weitere Details auch bieten Sie die Journaldatei beim Vorbereiten der Festplattenlaufwerke erstellt.

## <a name="physically-ship-the-disks"></a>Die Datenträger physisch geliefert

Sie können nun physisch Datenträgern, Azure-Rechenzentrum liefern die Daten über Azure Storage BLOBs kopiert werden, die beim Erstellen des Importauftrags bereitgestellt. Auch beim Erstellen des Projekts, wenn Sie sich entschieden, diese Informationen später angeben Sie können jetzt zurück zu der Importauftrag und aktualisiert Nachverfolgungsnummer.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Daten aus Azure Storage Blobs in Azure See Datenspeicher

Sobald der Status des Importauftrags abgeschlossen wird, können Sie überprüfen, ob die Daten in Azure Storage Blobs verfügbar war angegebenen. Eine Vielzahl von Methoden können Sie um Daten von Azure Storage Blobs in Azure See Datenspeicher zu verschieben. Alle verfügbaren Optionen für das Hochladen von Daten finden Sie unter [Einlesen von Daten in dem Datenspeicher](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

In diesem Abschnitt bieten wir JSON-Definitionen, mit denen Sie eine Pipeline Azure Data Factory zum Kopieren von Daten erstellen. Diese JSON Definitionen von [Azure-Portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) , [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md)können.

### <a name="source-linked-service-azure-storage-blob"></a>Verknüpfte Datenquelle Service (Blob Azure-Speicher)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Ziel verknüpft Service (Azure Datenspeicher See)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Eingabedatensatzes
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Ausgabe-DataSet
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Rohrleitung (Copy-Aktivität)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Weitere Informationen zur Verwendung von Azure Data Factory zum Verschieben von Daten von Azure Storage Blob und Azure See Datenspeicher finden Sie unter [Verschieben von Daten von Azure Storage Blob Azure See Datenspeicher mit Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Wiederherstellen von Datendateien in Azure See Datenspeicher

Wir beginnen mit einer Datei, die 319GB Größe und haben sie in Dateien kleiner, damit es mit der Azure-Import/Export-Dienst übertragen werden. Daten in Azure See Datenspeicher können wir Reconstrcut der Datei auf die ursprüngliche Größe. Die folgenden Azure PowerShell Cmldts können dazu.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Nächste Schritte

- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
