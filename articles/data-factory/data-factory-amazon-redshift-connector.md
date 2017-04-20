<properties 
    pageTitle="Verschieben von Daten von Amazon Redshift mit Data Factory | Microsoft Azure" 
    description="Enthält Informationen zum Verschieben von Daten von Amazon Redshift mit Azure Data Factory." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Verschieben von Daten von Amazon Redshift mit Azure Data Factory

Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung zum Verschieben von Daten von Amazon Redshift mit anderen Daten speichern. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten mit Copy-Aktivität und eine Liste der Quelle-Senke Datenspeicher.  

Data Factory unterstützt derzeit nur Daten von Amazon Redshift für andere Datenspeicher, jedoch nicht zum Verschieben von Daten aus anderen Datenspeichern in Amazon Redshift.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Verschieben von Daten in einen lokalen Datenspeicher gewähren Sie Data Management Gateway (mit IP-Adresse des Computers) auf Amazon Redshift-Cluster. Eine Anleitung finden Sie unter [Zugriff auf den Cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) . 
- Verschieben von Daten in Azure Datenspeicher finden Sie unter [Azure Data Center IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=41653) Compute IP-Adressbereiche (einschließlich SQL Bereiche) von Microsoft Azure Rechenzentren verwendet. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline erstellen, die Daten von Amazon Redshift kopiert ist die Verwendung des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Das folgende Beispiel enthält Beispiel JSON Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Es veranschaulicht Daten von Amazon Redshift in Azure BLOB-Speicher kopieren. Allerdings können Daten auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores)kopiert werden.

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Beispiel: Daten von Amazon Redshift in Azure Blob
Dieses Beispiel zeigt, wie Daten aus einer Datenbank Amazon Redshift in Azure BLOB-Speicher kopiert. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

- Eine verknüpfte Dienst vom Typ [AmazonRedshift](#linked-service-properties).
- Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [RelationalTable](#dataset-type-properties).
- Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [RelationalSource](#copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Das Beispiel Kopiervorgang aus einem Abfrageergebnis Amazon Redshift in ein Blob stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 

**Dienst Amazon Redshift verknüpft**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
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

**Eingabedatasets Amazon Redshift**

Festlegen von **"extern": true** informiert Daten Factorydienst Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt. Legen Sie diese Eigenschaft auf True für Eingabedatasets, die nicht durch eine Aktivität in der Pipeline entsteht.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
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
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **RelationalSource** und **BlobSink** **Senke** Typ festgelegt ist. Für die **Abfrage** -Eigenschaft angegebene SQL-Abfrage wählt die Daten in der letzten Stunde kopieren.
    
    {
        "name": "CopyAmazonRedshiftToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonRedshiftInputDataset"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Verknüpfte Eigenschaften

Die folgende Tabelle beschreibt JSON Elemente bestimmten Dienst Amazon Redshift verknüpft.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **AmazonRedshift**. | Ja |
| Server | IP-Adresse oder Name des Amazon Redshift Server. | Ja |
| Anschluss | Die Anzahl der TCP-Port, den Amazon Redshift Server für Clientverbindungen verwendet. | Nein, Standardwert: 5439 |
| Datenbank | Name der Datenbank Amazon Redshift. | Ja |
| Benutzername | Name des Benutzers, der auf die Datenbank zugreifen.| Ja |
| Kennwort | Kennwort für das Benutzerkonto.| Ja |


## <a name="dataset-type-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die **TypeProperties** unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. TypeProperties Abschnitt Dataset vom Typ **RelationalTable** (einschließlich Amazon Redshift Dataset) hat die folgenden Eigenschaften

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Tabellenname | Name der Tabelle in der verknüpften Serviceartikel auf Amazon Redshift-Datenbank. | Nein (bei **Abfrage** des **RelationalSource** ) | 

## <a name="copy-activity-type-properties"></a>Typeneigenschaften kopieren

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt **TypeProperties** der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

Beim kopieraktivität vom Typ **RelationalSource** ist (einschließlich Amazon Redshift) stehen die folgenden Eigenschaften im TypeProperties-Abschnitt:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Abfrage | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. | Nein ( **TableName** **Dataset** angegeben wird). | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Zuordnung für Amazon Redshift

Wie im Artikel [Datenaktivitäten](data-factory-data-movement-activities.md) ausführt kopieraktivität automatische Konvertierung von Quelltypen mit folgenden zweistufiges Auffangen:

1. Konvertieren von Typen von systemeigenen .NET Typ
2. Konvertieren von .NET Typ native Senke Typ

Beim Verschieben von Daten in Amazon Redshift die folgenden Zuordnungen von Amazon Redshift Typen zu dienen.

Amazon Redshift Typ | .NET basierte Typ
-------------------- | ---------------
SMALLINT | Int16
GANZE ZAHL | Int32
BIGINT | Int64
DEZIMAL | Dezimal
REAL | Einzelne
MIT DOPPELTER GENAUIGKEIT | Double
BOOLESCHER WERT | Zeichenfolge
CHAR | Zeichenfolge
VARCHAR | Zeichenfolge
DATUM | DateTime
ZEITSTEMPEL | DateTime
TEXT | Zeichenfolge



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.

## <a name="next-steps"></a>Nächste Schritte
Finden Sie in folgenden Artikeln: 
- [Kopieraktivität Lernprogramm](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für eine schrittweise Anleitung zum Erstellen einer Pipeline mit einer Kopie. 