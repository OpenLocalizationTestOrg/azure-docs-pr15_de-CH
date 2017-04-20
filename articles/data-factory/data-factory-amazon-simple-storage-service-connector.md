<properties 
    pageTitle="Verschieben von Daten von Amazon Simple Storage Service mit Data Factory | Microsoft Azure" 
    description="Enthält Informationen zum Verschieben von Daten von Amazon Simple Storage Service (S3) mit Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Verschieben von Daten von Amazon Simple Storage Service mit Azure Data Factory

Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung zum Verschieben von Daten von Amazon Simple Storage Service (S3) eine andere Daten gespeichert. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel, der eine allgemeine Übersicht über Daten und eine Liste der unterstützten Datenquellen-Senke Datenspeicher mit Kopie enthält.  

Data Factory unterstützt derzeit nur Daten von Amazon S3 in anderen Datenspeichern, jedoch nicht zum Verschieben von Daten aus anderen Datenspeichern Amazon S3.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Kopieren Sie Daten von Amazon S3 sicherstellen Sie, dass Sie unter Berechtigungen erteilt:

- **S3:GetObject** und **S3:GetObjectVersion** für Amazon S3 Objektoperationen
- **S3:ListBucket** und **S3:ListAllMyBuckets** (Copy-Assistenten verwendet) für Amazon S3 Eimer 

Finden Sie die vollständige Liste der Berechtigungen mit Amazon S3 von [Berechtigungen in einer Richtlinie angeben](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline erstellen, die Daten von Amazon S3 kopiert ist die Verwendung des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Das folgende Beispiel enthält Beispiel JSON Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Es veranschaulicht Daten von Amazon S3 in Azure BLOB-Speicher kopieren. Allerdings können Daten auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores)kopiert werden.

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Beispiel: Daten aus Amazon S3 in Azure Blob
Dieses Beispiel veranschaulicht, wie Daten von Amazon S3 in Azure BLOB-Speicher kopieren. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

- Eine verknüpfte Dienst vom Typ [AwsAccessKey](#linked-service-properties).
- Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AmazonS3](#dataset-type-properties).
- Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [FileSystemSource](#copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Das Beispiel Kopiervorgang von Amazon S3 Azure BLOB stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 

**Dienst Amazon S3 verknüpft**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
            }
        }
    }

**Azure verknüpft Speicherdienst**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Amazon S3 Eingabedatasets**

Festlegen von **"extern": true** informiert Daten Factorydienst Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt. Legen Sie diese Eigenschaft auf True für Eingabedatasets, die nicht durch eine Aktivität in der Pipeline entsteht.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }



**Azure Blob Ausgabe dataset**

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch anhand die Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet Jahr, Monat, Tag und Teile Stunden von der Startzeit entfernt.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Pipeline mit Kopieren**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **FileSystemSource** und **BlobSink** **Senke** Typ festgelegt ist. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
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
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Verknüpfte Eigenschaften

Die folgende Tabelle enthält die Beschreibung für JSON Elemente bestimmte Amazon S3 (**AwsAccessKey**) Dienst verknüpft.

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | ID der geheimen Tastenkombination. | Zeichenfolge | Ja |
| secretAccessKey | Die geheime Tastenkombination selbst. | Verschlüsselte geheime Zeichenfolge | Ja | 


## <a name="dataset-type-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die **TypeProperties** unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. TypeProperties Abschnitt Dataset vom Typ **AmazonS3** (einschließlich Amazon S3 Dataset) hat die folgenden Eigenschaften

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------- | ------ | 
| bucketName | Der Name des S3-Bucket. | Zeichenfolge | Ja |
| Schlüssel | Objektschlüssel S3. | Zeichenfolge | Nein | 
| Präfix | Präfix für Objektschlüssel S3. Objekte, deren Schlüssel mit diesem Präfix beginnen, werden ausgewählt. Gilt nur beim leer ist. | Zeichenfolge | Nein | 
| Version | Die Version der S3-Objekt, wenn S3 Versionskontrolle aktiviert ist. | Zeichenfolge | Nein |  
| Format | Folgende Format unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. **Typeigenschaft unter Format** auf einen der folgenden Werte festgelegt. [TextFormat angeben](#specifying-textformat), [Angabe AvroFormat](#specifying-avroformat) [JsonFormat angeben](#specifying-jsonformat), [OrcFormat angeben](#specifying-orcformat)und [Festlegen ParquetFormat](#specifying-parquetformat) Abschnitte für Details anzeigen Wenn Sie Dateien kopieren möchten-wird zwischen dateibasierte Speicher (binären Kopie) überspringen Formatabschnitt sowohl Eingabe- und Dataset-Definitionen.| Nein
| Komprimierung | Geben Sie Art und Grad der Komprimierung der Daten. Typen werden unterstützt: **GZip**, **Deflate**, **BZip2** und unterstützte sind: **Optimal** und **schnell**. Derzeit werden die Einstellungen für Daten in **AvroFormat** oder **OrcFormat**nicht unterstützt. Weitere Informationen siehe [Komprimierung unterstützt](#compression-support) .  | Nein |

> [AZURE.NOTE] BucketName + Taste gibt den Speicherort des Objekts S3, Bucket Stammcontainer für S3-Objekte und Schlüssel der vollständige Pfad S3-Objekt.

### <a name="sample-dataset-with-prefix"></a>Beispiel-Dataset mit Präfix

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Beispieldataset (mit Version)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dynamische Pfade für S3

Im Beispiel verwenden wir feste Werte für Schlüssel und BucketName Eigenschaften im Dataset Amazon S3. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

Data Factory Schlüssel und BucketName dynamisch zur Laufzeit mithilfe von Variablen wie SliceStart berechnen können.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

Sie können auch für die Prefix-Eigenschaft eines Datasets Amazon S3 vornehmen. Eine Liste der unterstützten Funktionen und Variablen finden Sie unter [Data Factory Funktionen und Variablen](data-factory-functions-variables.md) . 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Typeneigenschaften kopieren

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt **TypeProperties** der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

Wird Quelle kopieren-Aktivität des Typs **FileSystemSource** (einschließlich Amazon S3), stehen die folgenden Eigenschaften im TypeProperties-Abschnitt:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- | 
| Rekursive | Gibt an, ob die Liste rekursiv S3 Objekte im Verzeichnis. | True/false | Nein | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.

## <a name="next-steps"></a>Nächste Schritte
Finden Sie in folgenden Artikeln: 
- [Kopieraktivität Lernprogramm](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für eine schrittweise Anleitung zum Erstellen einer Pipeline mit einer Kopie. 
