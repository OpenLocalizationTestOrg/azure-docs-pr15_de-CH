<properties 
    pageTitle="Verschieben von Daten von MySQL | Azure Data Factory" 
    description="Enthält Informationen zum Verschieben von Daten von MySQL-Datenbank mithilfe von Azure Data Factory." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Verschieben von Daten von MySQL mit Azure Data Factory

Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung zum Verschieben von Daten von MySQL zu anderen Daten speichern. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und unterstützten Speicher-Kombinationen.

Data Factory unterstützt mit lokalen MySQL Quellen Data Management Gateway. [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Siehe über Data Management Gateway und Anleitung zum Einrichten von Gateway. 

> [AZURE.NOTE] Gateway ist auch die MySQL-Datenbank in eine IaaS Azure Virtual Machine (VM) gehostet wird. Sie können das Gateway auf dem gleichen virtuellen Computer als Datenspeicher oder auf einen anderen virtuellen Computer als Gateway mit der Datenbank herstellen kann.  

Data Factory unterstützt derzeit nur Daten von MySQL in anderen Datenspeichern, jedoch nicht zum Verschieben von Daten aus anderen Datenspeichern in MySQL.

## <a name="installation"></a>Installation 
Daten-Management Gateway der MySQL-Datenbank herstellen müssen Sie [MySQL Connector/Net 6.6.5 für Windows](http://go.microsoft.com/fwlink/?LinkId=278885) auf demselben System wie Data Management Gateway installieren.

> [AZURE.NOTE] Tipps zur Behebung von Verbindungs-Gateways Siehe [Problembehandlung Gateway stellt](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Fragen. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline zu erstellen, die Daten aus einer MySQL-Datenbank eines unterstützten Senke Datenspeicher kopiert ist die Verwendung des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Das folgende Beispiel enthält Beispiel JSON Definitionen, mit denen Sie eine Pipeline erstellen. Umfassende Schritt finden Sie unter [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Artikel. Daten können auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) mithilfe der Kopieraktivität in Azure Data Factory kopiert.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Beispiel: Daten von MySQL in Azure Blob
Dieses Beispiel zeigt, wie Daten aus einer lokalen MySQL-Datenbank in Azure BLOB-Speicher kopiert. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  

> [AZURE.IMPORTANT] Dieses Beispiel stellt JSON Ausschnitte. Eine schrittweise Anleitung zum Erstellen der Data Factory ist nicht enthalten. [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Siehe für eine schrittweise Anleitung. 
 
Das Beispiel hat Daten Factory Entitäten:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel kopiert Daten aus einem Abfrageergebnis in MySQL-Datenbank in ein Blob stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 

Als ersten Schritt setup Data Management Gateway. Die Schritte sind im [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Artikel. 

**Dienst MySQL verknüpft**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**MySQL Eingabedatasets**

Das Beispiel wird vorausgesetzt, Sie haben eine Tabelle "MyTable" in MySQL und enthält eine Spalte namens "Timestampcolumn" für zeitreihendaten.

Einstellung "extern": "true" informiert Data Factory-Dienst, dass die Tabelle Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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



**Azure Blob Ausgabe dataset**

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch anhand die Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet Jahr, Monat, Tag und Teile Stunden von der Startzeit entfernt.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyMySqlToBlob",
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
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Eigenschaften von MySQL verknüpft

Die folgende Tabelle beschreibt für JSON-Elemente für Dienst MySQL verknüpft.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **OnPremisesMySql** | Ja |
| Server | Name der MySQL-Server. | Ja |
| Datenbank | Name der MySQL-Datenbank. | Ja | 
| Schema  | Name des Schemas in der Datenbank. | Nein | 
| Read | Art der Authentifizierung zum Herstellen der MySQL-Datenbank. Mögliche Werte sind: anonym, Basic und Windows. | Ja | 
| Benutzername | Geben Sie Benutzernamen an, wenn Sie Standardauthentifizierung oder Windows-Authentifizierung verwenden. | Nein | 
| Kennwort | Geben Sie Kennwort für das Benutzerkonto für den Benutzernamen angegeben. | Nein | 
| gatewayName | Name des Gateways Daten Factorydienst Verbindung zu lokalen MySQL-Datenbank verwenden soll. | Ja |

Informationen über das Festlegen von Anmeldeinformationen für eine lokale MySQL-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="mysql-dataset-type-properties"></a>MySQL Dataset Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die **TypeProperties** unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. TypeProperties Abschnitt Dataset vom Typ **RelationalTable** (einschließlich MySQL Dataset) hat die folgenden Eigenschaften

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Tabellenname | Name der Tabelle in die MySQL-Datenbank-Instanz, der verknüpfter Dienst bezieht. | Nein (bei **Abfrage** des **RelationalSource** ) | 

## <a name="mysql-copy-activity-type-properties"></a>MySQL Kopie Typeneigenschaften

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen sind Richtlinien für alle Arten von Aktivitäten. 

Eigenschaften im Abschnitt **TypeProperties** der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

Wenn Quelle kopieren-Aktivität des Typs **RelationalSource** (einschließlich MySQL) stehen die folgenden Eigenschaften im TypeProperties-Abschnitt:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Abfrage | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. | Nein ( **TableName** **Dataset** angegeben wird). | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>MySQL-Zuordnung

Wie im Artikel [Datenaktivitäten](data-factory-data-movement-activities.md) ausführt kopieraktivität automatische Konvertierung von Quelltypen mit folgenden zweistufiges Auffangen:

1. Konvertieren von Typen von systemeigenen .NET Typ
2. Konvertieren von .NET Typ native Senke Typ

Beim Verschieben von Daten in MySQL dienen die folgenden Zuordnungen von MySQL-Typen zu.

| Typ der MySQL-Datenbank | .NET Framework-Typ |
| ------------------- | ------------------- | 
| unsigned bigint | Dezimal |
| bigint | Int64 |
| Bit | Dezimal |
| BLOB | Byte] |
| bool |  Boolescher Wert | 
| Char | Zeichenfolge |
| Datum | DateTime |
| DateTime | DateTime |
| Dezimal | Dezimal |
| mit doppelter Genauigkeit | Double |
| Doppelte | Double |
| Enum | Zeichenfolge |
| float | Einzelne |
| unsigned int | Int64 |
| int | Int32 |
| ganze Zahl ohne Vorzeichen | Int64 |
| ganze Zahl | Int32 | 
| Long varbinary | Byte] |
| long varchar | Zeichenfolge |
| longblob | Byte] |
| LONGTEXT | Zeichenfolge | 
| mediumblob |  Byte] | 
| Mediumint ohne Vorzeichen | Int64 |
| mediumint | Int32 | 
| Mittel | Zeichenfolge |
| numerische | Dezimal |
| Real |  Double |
| Festlegen | Zeichenfolge |
| unsigned smallint | Int32 |
| smallint | Int16 |
| Text | Zeichenfolge |
| Zeit | TimeSpan |
| Zeitstempel | DateTime |
| tinyblob | Byte] |
| Tinyint ohne Vorzeichen |  Int16 | 
| tinyint | Int16 |
| tinytext | Zeichenfolge | 
| varchar | Zeichenfolge | 
| Jahr | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.

