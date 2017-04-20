<properties 
    pageTitle="Verschieben von Daten von DB2 | Azure Data Factory" 
    description="Weitere Informationen über das Verschieben von Daten von Azure Data Factory mit DB2-Datenbank" 
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

# <a name="move-data-from-db2-using-azure-data-factory"></a>Verschieben von Daten von DB2 mit Azure Data Factory
Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung zum Verschieben von Daten von DB2 auf anderen Daten speichern. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und unterstützten Speicher-Kombinationen.

Data Factory unterstützt lokale DB2 Quellen Data Management Gateway herstellen. [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Siehe über Data Management Gateway und Anleitung zum Einrichten von Gateway. 

> [AZURE.NOTE]
Verwenden Sie das Gateway Verbindung mit DB2, auch wenn es in Azure IaaS VMs gehostet wird. Wenn Sie Verbinden mit einer Instanz von DB2 Cloud gehostet, können Sie die Gatewayinstanz IaaS-VM installieren.

Data Factory unterstützt derzeit nur Daten aus anderen Datenspeichern DB2, nicht von anderen Datenspeichern mit DB2. 

## <a name="installation"></a>Installation 

Daten-Management Gateway bietet einen integrierten DB2-Treiber, der das Folgendes unterstützt: 

- SQLAM 9 / 10 / 11
- DB2 LUW (Linux, Unix, Windows) für
- DB2 Z/OS und DB2 für i (auch als / 400)

Daher müssen Sie nicht mehr die Treiber manuell installieren, beim Kopieren von Daten von DB2.

> [AZURE.NOTE] Tipps zur Behebung von Verbindungs-Gateways Siehe [Problembehandlung Gateway stellt](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Fragen. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline erstellen, die Daten von einer DB2-Datenbank kopiert wird mithilfe des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Die folgenden Beispiele bieten Beispiel JSON-Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Sie zeigen, wie Daten von DB2-Datenbank und Azure BLOB-Speicher kopieren. Allerdings können Daten auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) mithilfe der Kopieraktivität in Azure Data Factory kopiert werden.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Beispiel: Daten von DB2 Azure BLOB

Dieses Beispiel zeigt, wie Daten aus einer lokalen DB2-Datenbank in Azure BLOB-Speicher kopiert. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties).
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
5.  Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet. 

Im Beispiel kopiert Daten aus einem Abfrageergebnis in einer DB2-Datenbank in Azure Blob stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 

Als ersten Schritt installieren und Konfigurieren eines Daten-Management-Gateways. Hinweise finden Sie im Artikel der [Übertragung von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) .

**Verknüpfte DB2-Dienst:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
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

**DB2 Eingabedatasets:**

Das Beispiel wird vorausgesetzt, Sie haben eine Tabelle "MyTable" in DB2 und eine Spalte "Timestamp" für Serie Daten enthält.

Einstellung "extern": True teilt Daten Factorydienst Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt. Beachten Sie, dass der **Typ** auf **RelationalTable**festgelegt ist. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
            "typeProperties": {},
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
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Pipeline mit kopieren:**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **RelationalSource** und **BlobSink** **Senke** Typ festgelegt ist. Für die **Abfrage** -Eigenschaft angegebene SQL-Abfrage wählt die Daten aus der Tabelle Orders.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Eigenschaften von DB2 verknüpft

Die folgende Tabelle beschreibt für JSON-Elemente für verknüpfte DB2-Dienst. 

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **OnPremisesDB2** | Ja |
| Server | Name des DB2-Servers. | Ja |
| Datenbank | Name der DB2-Datenbank. | Ja |
| Schema | Name des Schemas in der Datenbank. Der Schemaname beachtet werden. | Nein |
| Read | Art der Authentifizierung für die Verbindung mit der DB2-Datenbank verwendet. Mögliche Werte sind: anonym, Basic und Windows. | Ja |
| Benutzername | Geben Sie Benutzernamen an, wenn Sie Standardauthentifizierung oder Windows-Authentifizierung verwenden. | Nein |
| Kennwort | Geben Sie Kennwort für das Benutzerkonto für den Benutzernamen angegeben. | Nein |
| gatewayName | Name des Gateways Daten Factorydienst Verbindung zu lokalen DB2-Datenbank verwenden soll. | Ja |

Informationen zum Festlegen von Anmeldeinformationen für eine lokale DB2-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) . 


## <a name="db2-dataset-type-properties"></a>DB2 Dataset Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die TypeProperties unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. TypeProperties Abschnitt Dataset vom Typ RelationalTable (einschließlich DB2 Dataset) hat die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Tabellenname | Name der Tabelle in der DB2-Datenbank-Instanz, der verknüpfter Dienst bezieht. Der Tabellenname wird beachtet. | Nein (bei **Abfrage** des **RelationalSource** ) |

## <a name="db2-copy-activity-type-properties"></a>DB2 kopieren Typeneigenschaften

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt TypeProperties der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

Für Kopie bei Quelle vom Typ **RelationalSource** (einschließlich DB2) stehen die folgenden Eigenschaften im TypeProperties-Abschnitt:


| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------- | -------------- |
| Abfrage | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | SQL-Abfragezeichenfolge. Beispiel: `"query": "select * from "MySchema"."MyTable""`. | Nein ( **TableName** **Dataset** angegeben wird).|

> [AZURE.NOTE] Schema und-Tabelle Groß-und Kleinschreibung. Die Namen in "" (Anführungszeichen) in der Abfrage.  

**Beispiel:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>Zuordnung für DB2
Wie im Artikel [Daten Bewegung](data-factory-data-movement-activities.md) Ausführende der Kopie automatische Konvertierung von Quelltypen zum Auffangen von Typen mit dem folgenden Schritt 2:

1. Konvertieren von Typen von systemeigenen .NET Typ
2. Konvertieren von .NET Typ native Senke Typ

Beim Verschieben von Daten in DB2 sind die folgenden Zuordnungen von DB2-Typ in .NET Typ verwendet.

Typ der DB2-Datenbank | .NET Framework-Typ 
----------------- | ------------------- 
SmallInt | Int16
Ganze Zahl | Int32
BigInt | Int64
Real | Einzelne
Double | Double
Float | Double
Dezimal | Dezimal
DecimalFloat | Dezimal
Numerische | Dezimal
Datum | DateTime
Zeit | TimeSpan
Zeitstempel | DateTime
XML | Byte]
Char | Zeichenfolge
VarChar | Zeichenfolge
LongVarChar | Zeichenfolge
DB2DynArray | Zeichenfolge
Binäre | Byte]
VarBinary | Byte]
LongVarBinary | Byte]
Grafik | Zeichenfolge
VarGraphic | Zeichenfolge
LongVarGraphic | Zeichenfolge
CLOB | Zeichenfolge
BLOB | Byte]
DbClob | Zeichenfolge
SmallInt | Int16
Ganze Zahl | Int32
BigInt | Int64
Real | Einzelne
Double | Double
Float | Double
Dezimal | Dezimal
DecimalFloat | Dezimal
Numerische | Dezimal
Datum | DateTime
Zeit | TimeSpan
Zeitstempel | DateTime
XML | Byte]
Char | Zeichenfolge


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.


