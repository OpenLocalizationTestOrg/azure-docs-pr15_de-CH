<properties
   pageTitle="Laden von Daten mit Azure Data Factory | Microsoft Azure"
   description="Informationen Sie zum Laden von Daten mit Azure Data Factory"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>

# <a name="load-data-with-azure-data-factory"></a>Daten mit Azure Daten laden 

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)  

In diesem Lernprogramm wird veranschaulicht, wie eine Pipeline in Azure Data Factory zum Verschieben von Daten von Azure Storage Blob Azure SQL Data Warehouse erstellt. Mit den folgenden Schritten können wie folgt vor:

+ Richten Sie Beispieldaten in ein Azure-Speicher-Blob.
+ Verbinden Sie Ressourcen mit Azure Data Factory.
+ Erstellen Sie eine Pipeline zum Verschieben von Daten aus Speicher-Blobs mit SQL Data Warehouse.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Bevor Sie beginnen

Um sich mit Azure Data Factory, finden Sie unter [Einführung in Azure Data Factory][].

### <a name="create-or-identify-resources"></a>Erstellen oder Ressourcen

Bevor Sie dieses Lernprogramm starten, benötigen Sie Folgendes:

   + **Azure Storage Blob**: dieses Lernprogramms Azure Storage Blob als Datenquelle für die Pipeline Azure Data Factory und so benötigen Sie die Beispieldaten speichern verfügbar. Wenn Sie eine bereits haben, erfahren Sie erstellen [ein Speicherkonto][].

   + **SQL Data Warehouse**: Dieses Lernprogramm verschiebt die Daten von Azure Storage Blob SQL Data Warehouse und so ein Datawarehouse haben, die mit den Beispieldaten AdventureWorksDW geladen wird. Wenn Sie nicht bereits ein Datawarehouse haben, Informationen wie Bereitstellung [einer][Create a SQL Data Warehouse]. Wenn Sie ein Datawarehouse, aber nicht mit den Beispieldaten bereitstellen, können Sie [manuell laden][Load sample data into SQL Data Warehouse].

   + **Azure Data Factory**: Azure Data Factory schließt die tatsächliche Auslastung und also haben, mit denen Sie die Bewegung Datenpipeline erstellen. Wenn Sie eine bereits haben, erfahren Sie, wie in Schritt 1 [Erste Schritte mit Azure Data Factory (Data Factory Editor)][]erstellen.

   + **AZCopy**: AZCopy die Beispieldaten aus den lokalen Client Ihre Azure Storage BLOB kopiert werden sollen. Installationshinweise finden Sie in der [Dokumentation AZCopy][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Schritt 1: Kopieren Sie Beispieldaten in Azure Storage Blob

Haben Sie alle Komponenten bereit, können Sie Ihre Azure Storage Blob Beispieldaten kopieren.

1. [Laden Sie Beispieldaten herunter][]. Diese Daten hinzugefügt die Beispieldaten AdventureWorksDW noch drei Jahre Daten.

2. Dieser Befehl AZCopy drei Jahre Daten in Ihre Azure Storage Blob kopiert.

    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Schritt 2: Verbinden von Ressourcen mit Azure Data Factory

Ist Daten können wir Azure Data Factory-Pipeline zum Verschieben von Daten von Azure BLOB-Speicher in SQL Data Warehouse erstellen.

[Azure-Portal][] zu öffnen und Menüoptionen Werk Daten auswählen.

### <a name="step-21-create-linked-service"></a>Schritt 2.1: Verknüpften Dienst erstellen

Verknüpfung von Azure Speicherkonto und SQL Data Warehouse mit Daten Werk.  

1. Zunächst den Registrierungsvorgang durch Klicken auf den Abschnitt "Verknüpfte Dienste" Data Factory und klicken Sie dann auf "neue Datenspeicher". Wählen Sie einen Namen registrieren der Azure-Speicher unter select Azure-Speicher als auswählen und geben Sie Ihren Benutzernamen und Kontoschlüssel.

2. Registrieren navigieren SQL Data Warehouse zum "Autor und Bereitstellen", wählen Sie 'New Data Store' und "Azure SQL Data Warehouse". Kopieren Sie und füllen Sie die Informationen in dieser Vorlage einfügen.

    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>Schritt 2.2: Dataset definieren

Nach dem Erstellen der verknüpften Diensten, müssen wir die Daten definieren.  Hier bedeutet dies die Struktur der Daten, die aus dem Speicher in das Datawarehouse verschoben wird.  Erfahren Sie mehr über das Erstellen

1. Starten von Abschnitt "Autor und Bereitstellen" Data Factory navigieren.

2. Klicken Sie auf 'Neues Dataset' und 'Azure BLOB-Speicher Ihres Speichers Werk Daten verknüpfen.  Verwenden Sie die unten beschriebene Skript zum Definieren von Daten in Azure BLOB-Speicher:

    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
                "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }
    ```

3. Jetzt definieren wir unser Dataset für SQL Data Warehouse. Zunächst auf die gleiche Weise auf 'Neues Dataset' und "Azure SQL Data Warehouse".

    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Schritt 3: Erstellen Sie und führen Sie der Pipeline aus

Schließlich einrichten und Ausführen die Pipeline in Azure Data Factory.  Dies ist der Vorgang, der den eigentlichen Datentransfer abgeschlossen ist.  Vollständige Ansicht Vorgänge, die komplett mit SQL Data Warehouse und Azure Data Factory finden [hier][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

Klicken Sie im Abschnitt "Autor und Bereitstellen" auf "Mehr Befehle" und "Neue Pipeline".  Nach dem Erstellen der Pipeline können Sie den folgenden Code, um die Daten in das Datawarehouse übertragen:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Starten Sie weitere anzeigen:

- [Azure Data Factory Lernpfad][].
- [Azure SQL Data Warehouse-Verbindung][]. Dies ist im Kern-Referenzthema für Azure SQL Data Warehouse mit Azure Data Factory.


Diese Themen enthalten detaillierte Informationen zu Azure Data Factory. Azure SQL-Datenbank oder HDInsight behandelt, die Informationen betreffen jedoch auch Azure SQL Data Warehouse.

- [Einführung: Erste Schritte mit Azure Data Factory][] Dies ist das Lernprogramm Core Datenverarbeitung mit Azure Data Factory. In diesem Lernprogramm erstellen Sie Ihre erste Pipeline, die zum Transformieren und Analysieren Webprotokolle monatlich HDInsight verwendet. Beachten Sie in diesem Lernprogramm keine kopieraktivität vorhanden ist.
- [Einführung: Daten aus Azure Storage Blob in Azure SQL-Datenbank][]. In diesem Lernprogramm erstellen Sie eine Rohrleitung in Azure Data Factory Daten von Azure Storage Blob in Azure SQL-Datenbank kopiert.

<!--Image references-->

<!--Article references-->
[AZCopy Dokumentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Ein Speicherkonto erstellen]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Erste Schritte mit Azure Data Factory (Data Factory-Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Einführung in Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Praktische Einführung: Daten von Azure Storage Blob in Azure SQL-Datenbank]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Lernprogramm: Erste Schritte mit Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory-Lernpfad]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure-portal]: https://portal.azure.com
[Laden Sie Beispieldaten herunter]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
