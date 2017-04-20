<properties 
    pageTitle="Verschieben von Daten von Teradata | Azure Data Factory" 
    description="Erfahren Sie mehr über Teradata Connector für Data Factory-Dienst, der Daten von Teradata Datenbank verschieben können" 
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
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Verschieben von Daten von Teradata mit Azure Data Factory

Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung zum Verschieben von Daten von Teradata mit anderen Daten speichern. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und unterstützten Speicher-Kombinationen.

Data Factory unterstützt lokale Teradata Quellen über Data Management Gateway an. [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Siehe über Data Management Gateway und Anleitung zum Einrichten von Gateway. 

> [AZURE.NOTE]
Gateway ist auch die Teradata eine Neuerung Azure gehostet wird. Sie können das Gateway auf derselben IaaS VM als Datenspeicher oder einer anderen VM als Gateway mit der Datenbank herstellen kann. 

Data Factory unterstützt nur Daten von Teradata in anderen Datenspeichern und nicht von anderen Datenspeichern zu Teradata.

## <a name="installation"></a>Installation 

Data Management Gateway Teradata Datenbank herstellen müssen Sie den [Datenanbieter für Teradata .NET](http://go.microsoft.com/fwlink/?LinkId=278886) auf demselben System wie Data Management Gateway installieren.

> [AZURE.NOTE] Tipps zur Behebung von Verbindungs-Gateways Siehe [Problembehandlung Gateway stellt](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Fragen. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline erstellen, die Daten von Teradata aller unterstützten Senke Datenspeicher kopiert wird mithilfe des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Das folgende Beispiel enthält Beispiel JSON Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Sie zeigen, wie Daten von Teradata in Azure BLOB-Speicher kopiert. Allerdings können Daten auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) mithilfe der Kopieraktivität in Azure Data Factory kopiert werden.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Beispiel: Daten von Teradata Azure BLOB

Dieses Beispiel zeigt, wie Daten aus einer Datenbank Teradata in Azure BLOB-Speicher kopiert. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  Die [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Das Beispiel Kopiervorgang aus einem Abfrageergebnis Teradata Datenbank in ein Blob stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 

Als ersten Schritt setup Data Management Gateway. Die Schritte sind im [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Artikel.

**Teradata verknüpft Service:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Azure BLOB-Speicher verknüpft Service:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }


**Eingabedatasets Teradata:**

Das Beispiel wird vorausgesetzt, Sie haben eine Tabelle "MyTable" in Teradata und eine Spalte "Timestamp" für Serie Daten enthält.

Einstellung "extern": True informiert Data Factory-Dienst, die Tabelle Data Factory ist nicht durch eine Aktivität im Werk Daten erzeugt.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }


**Azure Blob Ausgabe Dataset:**

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch anhand die Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet Jahr, Monat, Tag und Teile Stunden von der Startzeit entfernt.

    {
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pipeline mit kopieren:**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **RelationalSource** und **BlobSink** **Senke** Typ festgelegt ist. Für die **Abfrage** -Eigenschaft angegebene SQL-Abfrage wählt die Daten in der letzten Stunde kopieren.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Eigenschaften von Teradata verknüpft

Die folgende Tabelle beschreibt JSON Elemente für verknüpfte Teradata Service. 

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Type-Eigenschaft muss auf festgelegt sein: **OnPremisesTeradata** | Ja
Server | Name des Servers Teradata. | Ja
Read | Art der Authentifizierung zum Verbinden mit der Datenbank Teradata. Mögliche Werte sind: anonym, Basic und Windows. | Ja
Benutzername | Geben Sie Benutzernamen an, wenn Sie Standardauthentifizierung oder Windows-Authentifizierung verwenden. | Nein 
Kennwort | Geben Sie Kennwort für das Benutzerkonto für den Benutzernamen angegeben. | Nein 
gatewayName | Name des Gateways Daten Factorydienst Verbindung zu lokalen Teradata Datenbank verwenden soll. | Ja

Informationen zum Festlegen von Anmeldeinformationen für lokale Teradata Datenquellen finden Sie unter [Einstellung und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="teradata-dataset-type-properties"></a>Teradata Dataset Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die **TypeProperties** unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. Derzeit sind keine Eigenschaften für das Dataset Teradata unterstützt. 


## <a name="teradata-copy-activity-type-properties"></a>Teradata Kopie Typeneigenschaften

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt TypeProperties der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

Wenn Quelle des Typs **RelationalSource** (einschließlich Teradata) stehen die folgenden Eigenschaften im **TypeProperties** -Abschnitt:

Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
Abfrage | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. | Ja

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Zuordnung für Teradata

Wie im Artikel [Daten Bewegung](data-factory-data-movement-activities.md) Ausführende der Kopie automatische Konvertierung von Quelltypen zum Auffangen von Typen mit dem folgenden Schritt 2:

1. Konvertieren von Typen von systemeigenen .NET Typ
2. Konvertieren von .NET Typ native Senke Typ

Beim Verschieben von Daten in Teradata werden die folgenden Zuordnungen von Teradata Typ in .NET Typ verwendet.

Teradata Datenbanktyp | .NET Framework-Typ
----------------- | ---------------------------
Char | Zeichenfolge
CLOB | Zeichenfolge
Grafik | Zeichenfolge
VarChar | Zeichenfolge
VarGraphic | Zeichenfolge
BLOB | Byte]
Byte | Byte]
VarByte | Byte]
BigInt | Int64
ByteInt | Int16
Dezimal | Dezimal
Double | Double
Ganze Zahl | Int32
Anzahl | Double
SmallInt | Int16
Datum | DateTime
Zeit | TimeSpan
Mit Zeitzone | Zeichenfolge
Zeitstempel | DateTime
Timestamp mit Zeitzone | DateTimeOffset
Intervall-Tag | TimeSpan
Intervall-Tag Stunde | TimeSpan
Intervall Tag Minute | TimeSpan
Intervall Tag zweiten | TimeSpan
Intervall-Stunde | TimeSpan
Intervall Stunde, Minute | TimeSpan
Zweite Stunde Intervall | TimeSpan
Intervall Minute | TimeSpan
Intervall Minute Sekunde | TimeSpan
Intervall zweite | TimeSpan
Intervall Jahr | Zeichenfolge
Intervall Jahr Monat | Zeichenfolge
Intervall-Monat | Zeichenfolge
Period(Date) | Zeichenfolge
Period(Time) | Zeichenfolge
Zeit (Zeitzone) | Zeichenfolge
Period(timestamp) | Zeichenfolge
Zeitraum (Zeitstempel Zeitzone) | Zeichenfolge
XML | Zeichenfolge

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.