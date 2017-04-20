<properties 
    pageTitle="Verschieben von Daten von lokalen bietet | Azure Data Factory" 
    description="Enthält Informationen zum Verschieben von Daten von lokalen bietet Azure Data Factory verwenden." 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Verschieben von Daten von lokalen bietet mit Azure Data Factory
Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung zum Verschieben von Daten von einem lokalen bietet einen anderen Datenspeicher. Dieser Artikel baut auf Artikel [Datenaktivitäten](data-factory-data-movement-activities.md) , der einen allgemeinen Überblick über die Daten kopieren und unterstützten Speicher Kombinationen anzeigt.

Data Factory unterstützt derzeit nur Daten von einem lokalen bietet anderen Datenspeichern jedoch nicht zum Verschieben von Daten aus anderen Datenspeichern zu einer lokalen bietet.


## <a name="enabling-connectivity"></a>Verbindung aktivieren
Data Factory unterstützt mit lokalen bietet Daten-Management-Gateway verwenden. [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Siehe über Data Management Gateway und Anleitung zum Einrichten von Gateway. Verwenden Sie das Gateway Verbindung mit bietet, auch wenn es eine Neuerung Azure gehostet wird. 

Während der Installation von Gateway auf demselben lokalen Computer oder der Azure-VM als die bietet empfehlen wir das Gateway auf einem separaten Computer/Azure IaaS VM installieren. Gateway auf einem separaten Computer reduziert Ressourcenkonflikte und verbessert die Leistung. Wenn Sie das Gateway auf einem separaten Computer installieren, sollte der Computer mit der bietet Zugriff auf. 


## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline erstellen, die Daten aus lokalen bietet kopiert ist die Verwendung des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Die folgenden Beispiele bieten Beispiel JSON-Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Sie zeigen, wie Daten aus einer lokalen bietet in Azure BLOB-Speicher kopieren. Allerdings können Daten auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) mithilfe der Kopieraktivität in Azure Data Factory kopiert werden.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Beispiel: Daten aus lokalen bietet in Azure Blob

Dieses Beispiel veranschaulicht, wie Daten aus einer lokalen bietet in Azure BLOB-Speicher kopieren. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [Dateifreigabe](#hdfs-dataset-type-properties).
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [FileSystemSource](#hdfs-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Das Beispiel kopiert Daten aus einem lokalen bietet Azure BLOB fest stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 

Als ersten Schritt richten Sie das Daten-Management Gateway. Die Anleitung in der [Übertragung von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Artikel. 

**Bietet Service verknüpft** Dieses Beispiel verwendet die Windows-Authentifizierung. Siehe Abschnitt [bietet verknüpfte Dienst](#hdfs-linked-service-properties) für verschiedene Arten der Authentifizierung können. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
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

**Bietet Eingabe dataset** Dieses Dataset verweist auf den Ordner bietet DataTransfer-UnitTest /. Die Pipeline kopiert alle Dateien in diesem Ordner zum Ziel. 

Einstellung "extern": "true" informiert Data Factory-Dienst, dass das Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure Blob Ausgabe dataset**

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch anhand die Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet Jahr, Monat, Tag und Teile Stunden von der Startzeit entfernt.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Die Pipeline enthält eine Aktivität kopieren, wird konfiguriert diese Eingabe- und Datasets und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **FileSystemSource** und **BlobSink** **Senke** Typ festgelegt ist. Für die **Abfrage** -Eigenschaft angegebene SQL-Abfrage wählt die Daten in der letzten Stunde kopieren.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>BIETET verknüpften Eigenschaften

Tabelle Beschreibung für JSON Elemente bestimmte bietet Service verknüpft.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **bietet** | Ja | 
| URL | URL der bietet | Ja |
| encryptedCredential | [Neu AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) Ausgabe der Zugriffsberechtigung. | Nein |
| Benutzername | Benutzername für die Windows-Authentifizierung. | Ja (für Windows-Authentifizierung)
| Kennwort | Kennwort für die Windows-Authentifizierung. | Ja (für Windows-Authentifizierung)
| Read | Windows oder anonym. | Ja |
| gatewayName | Name des Gateways, die Verbindung zu den bietet Data Factory-Dienst verwenden soll. | Ja |   

Weitere Informationen zum Festlegen von Anmeldeinformationen für lokale bietet [Einstellung und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) zu.

### <a name="using-anonymous-authentication"></a>Anonyme Authentifizierung

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Windows-Authentifizierung
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>BIETET Dataset-Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die **TypeProperties** unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. TypeProperties Abschnitt Dataset vom Typ **Dateifreigabe** (einschließlich bietet Dataset) hat die folgenden Eigenschaften

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
folderPath | Pfad des Ordners. Beispiel:`myfolder`<br/><br/>Verwenden Sie Escapezeichen ' \ ' für spezielle Zeichen in der Zeichenfolge. Beispiel: Folder\subfolder, geben Sie im Ordner\\\\Unterordner und für d:\samplefolder, d:\\\\Beispielordner.<br/><br/>Kombinieren diese Eigenschaft mit **PartitionBy** basierend auf Pfade zu Start/Ende Datum / Uhrzeit. | Ja
Dateiname | Geben Sie den Namen der Datei in **FolderPath** soll die Tabelle auf eine bestimmte Datei im Ordner. Wenn Sie einen Wert für diese Eigenschaft nicht angeben, zeigt die Tabelle für alle Dateien im Ordner.<br/><br/>Wenn Dateiname für ein ausgabedataset nicht angegeben wird, wäre der Name der generierten Datei im folgenden dieses Format: <br/><br/>Daten. <Guid>.txt (z. B.:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Nein
partitionedBy | PartitionedBy kann verwendet werden, dynamische FolderPath Dateinamen zeitreihendaten an. Beispiel: FolderPath Stunde Daten parametrisiert. | Nein
fileFilter | Gibt einen Filter an eine Teilmenge statt alle Dateien in den Ordnerpfad verwendet werden. <br/><br/>Zulässige Werte: `*` (mehrere Zeichen) und `?` (einzelnes Zeichen).<br/><br/>Beispiel 1:`"fileFilter": "*.log"`<br/>Beispiel 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Hinweis**: FileFilter gilt für Dateifreigabe Eingabedatasets | Nein
| Format | Folgende Format unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**und **ParquetFormat**. **Typeigenschaft unter Format** auf einen der folgenden Werte festgelegt. [TextFormat angeben](#specifying-textformat), [Angabe AvroFormat](#specifying-avroformat) [JsonFormat angeben](#specifying-jsonformat), [OrcFormat angeben](#specifying-orcformat)und [Festlegen ParquetFormat](#specifying-parquetformat) Abschnitte für Details anzeigen Wenn Sie Dateien kopieren möchten-wird zwischen dateibasierte Speicher (binären Kopie) überspringen Formatabschnitt sowohl Eingabe- und Dataset-Definitionen. | Nein 
| Komprimierung | Geben Sie Art und Grad der Komprimierung der Daten. Typen werden unterstützt: **GZip**, **Deflate**, **BZip2** und unterstützte sind: **Optimal** und **schnell**. Derzeit werden Einstellungen für Daten in **AvroFormat** oder **OrcFormat**nicht unterstützt. Weitere Informationen siehe [Komprimierung unterstützt](#compression-support) .  | Nein |



> [AZURE.NOTE] Dateiname und FileFilter können nicht gleichzeitig verwendet werden.


### <a name="using-partionedby-property"></a>Mithilfe der PartionedBy-Eigenschaft

Wie im vorherigen Abschnitt erwähnt, können Sie dynamische FolderPath Dateinamen Serie Daten mit PartitionedBy. Sie können dazu mit Data Factory Makros und die Systemvariable SliceStart, SliceEnd, die zum logischen Zeitraum einen bestimmten Datenslice anzugeben. 

Weitere Informationen über Time Series Datasets finden Sie in Planung und Slices [Datasets erstellen](data-factory-create-datasets.md), [Planung & Ausführung](data-factory-scheduling-and-execution.md)und [Pipelines erstellen](data-factory-create-pipelines.md) Artikeln. 

#### <a name="sample-1"></a>Beispiel 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In diesem Beispiel {Segment} mit dem angegebenen Wert "Data Factory" Systemvariablen SliceStart im Format (YYYYMMDDHH) ersetzt. Die SliceStart verweist auf das Slice Startzeit. Der Ordnerpfad ist für jedes Segment. Beispiel: Wikisampledataout/Wikidatagateway/2014100103 oder Wikisampledataout/Wikidatagateway/2014100104.

#### <a name="sample-2"></a>Beispiel 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

In diesem Beispiel werden Jahr, Monat, Tag und Zeitpunkt der SliceStart in separaten Variablen extrahiert, die von Ordnerpfad und Dateiname verwendet werden.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>BIETET Kopie Typeneigenschaften

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt TypeProperties der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

Für Kopie bei Quelle vom Typ **FileSystemSource** stehen die folgenden Eigenschaften im TypeProperties-Abschnitt:

**FileSystemSource** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Rekursive | Gibt an, ob die Daten rekursiv aus dem Unterordner oder aus dem angegebenen Ordner schreibgeschützt ist. | True, False (Standard)| Nein |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.

