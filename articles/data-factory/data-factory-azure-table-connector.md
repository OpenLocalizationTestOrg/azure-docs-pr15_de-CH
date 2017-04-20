<properties 
    pageTitle="Verschieben von Daten und Azure Tabelle | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten in Azure Table Storage mit Azure Data Factory." 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Verschieben von Daten zu und von Azure Tabelle Azure Data Factory

Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung Daten Azure-Tabelle von/zu einem anderen verschieben. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten und unterstützten Speicher Kombinationen mit kopieren.

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die Rohrleitung zu erstellen, die Daten in Azure-Tabellenspeicher einfachsten mithilfe des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Die folgenden Beispiele bieten Beispiel JSON-Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Sie zeigen, wie Daten zwischen Azure-Tabellenspeicher und Azure BLOB-Datenbank kopieren. Allerdings können kopierten **direkt** von Quellen zu der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Beispiel: Daten Sie aus Tabelle Azure Azure BLOB

Das folgende Beispiel zeigt:

1.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (für Tabellen & Blob verwendet).
2.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureTable](#azure-table-dataset-type-properties).
3.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  Die [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [AzureTableSource](#azure-table-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet. 

Das Beispiel kopiert zur Standardpartition einer Tabelle Azure Blob stündlich Daten. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben.

**Azure verknüpfter Dienst:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory unterstützt zwei Arten von verknüpften Azure Storage Services: **AzureStorage** und **AzureStorageSas**. Für die erste Geben Sie die Verbindungszeichenfolge mit den kontoschlüssel und für die spätere Shared Access Signatur (SAS)-Uri angeben. Siehe Einzelheiten [Verknüpften Diensten](#linked-services) .  

**Azure Tabelle Eingabe-Dataset:**

Im Beispiel wird davon ausgegangen, dass eine Tabelle "MyTable" in Azure-Tabelle erstellt haben.
 
Einstellung "extern": "true" informiert Data Factory-Dienst, dass das Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
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

**Azure Blob Ausgabe Dataset:**

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch anhand die Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet Jahr, Monat, Tag und Teile Stunden von der Startzeit entfernt. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Pipeline mit der Kopie:**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **AzureTableSource** und **BlobSink** **Senke** Typ festgelegt ist. **AzureTableSourceQuery** -Eigenschaft angegebene SQL-Abfrage wählt die Daten aus der Standardpartition stündlich kopieren.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },              
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 0,
                        "timeout": "01:00:00"
                    }
                }
             ]  
        }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Beispiel: Daten aus Azure Blob in Azure-Tabelle

Das folgende Beispiel zeigt:

1.  Verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (für Tabellen & Blob verwendet)
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureTable](#azure-table-dataset-type-properties). 
4.  Die [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [AzureTableSink](#azure-table-copy-activity-type-properties)verwendet. 


Beispiel Kopien Zeitreihen Daten aus einer Azure ein Azure blob Tabelle stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben.

**Azure-Speicher (für Azure Tabelle & Blob) verknüpft Service:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory unterstützt zwei Arten von verknüpften Azure Storage Services: **AzureStorage** und **AzureStorageSas**. Für die erste Geben Sie die Verbindungszeichenfolge mit den kontoschlüssel und für die spätere Shared Access Signatur (SAS)-Uri angeben. Siehe Einzelheiten [Verknüpften Diensten](#linked-services) . 

**Azure Blob Eingabedatasets:**

Daten werden abgeholt ein neues Blob stündlich (Häufigkeit: Stunde, Intervall: 1). Pfad und Namen des für den Blob werden dynamisch anhand der Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet, Jahr, Monat und Tagesanteil der Startzeit und Dateinamen verwendet Stundenteil der Startzeit. "externe": "true" Einstellung informiert Daten Factorydienst Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.
    
    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
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

**Azure Tabelle Ausgabe Dataset:**

Im Beispiel werden Daten in eine Tabelle namens "MyTable" Azure-Tabelle kopiert. Erstellen einer Azure-Tabelle mit der gleichen Anzahl von Spalten erwartungsgemäß die BLOB-CSV-Datei enthält Neue Zeilen werden stündlich zur Tabelle hinzugefügt. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Pipeline mit der Kopie:**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **BlobSource** und **AzureTableSink** **Senke** Typ festgelegt ist. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
              }
            },
            "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },                      
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

## <a name="linked-services"></a>Verknüpfte Dienste
Es gibt zwei Arten von verknüpften Diensten, mit einen Azure BLOB-Speicher mit einer Azure Data Factory verknüpfen. Sie sind: **AzureStorage** und **AzureStorageSas** verknüpft Service verknüpft. Der Azure-Speicher verknüpft Service bietet Data Factory globalen Zugriff auf den Azure-Speicher. Während der Azure Storage SAS (SAS) verknüpft bietet Daten Factory eingeschränkt/zeitgebundene Zugriff auf den Azure-Speicher. Es gibt keine Unterschiede zwischen diesen beiden verknüpften Dienste. Wählen Sie den verknüpften Dienst, der Ihren Bedürfnissen entspricht. Die folgenden Abschnitte enthalten weitere Details auf zwei verknüpfte.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure Tabelle Dataset Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die TypeProperties unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. Typ **AzureTable** Abschnitt **TypeProperties** für das Dataset hat die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Tabellenname | Name der Tabelle in der Azure-Datenbank-Instanz, der verknüpfter Dienst bezieht. | Ja. Wenn ein Tabellenname ohne einen AzureTableSourceQuery angegeben wird, werden alle Datensätze aus der Tabelle in das Ziel kopiert. Wenn eine AzureTableSourceQuery angegeben wird, werden Datensätze aus der Tabelle, die die Abfrage entspricht zum Ziel kopiert. |

### <a name="schema-by-data-factory"></a>Schema von Data Factory
Bei Schema Datenspeichern wie Azure leitet Daten Factorydienst das Schema in einer der folgenden Methoden:

1.  Wenn Sie die Struktur der Daten mithilfe der **Struktur** -Eigenschaft in der datasetdefinition angeben, berücksichtigt Daten Factorydienst diese Struktur als Schema. In diesem Fall wird eine Zeile einen Wert für eine Spalte nicht enthalten, ein null-Wert für sie bereitgestellt.
2. Wenn Sie die Struktur von Daten mit der **Struktur** -Eigenschaft in der datasetdefinition angeben, leitet "Data Factory" das Schema mithilfe der ersten Zeile der Daten. In diesem Fall enthält die erste Zeile nicht das vollständige Schema, einige Spalten im Ergebnis der Kopiervorgang fehlen.

Daher empfiehlt Schema Daten Quellen, die Struktur von Daten der **Struktur** -Eigenschaft angeben.

## <a name="azure-table-copy-activity-type-properties"></a>Azure Tabelle kopieren Typeneigenschaften

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Datasets und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt TypeProperties der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

**AzureTableSource** unterstützt die folgenden Eigenschaften im TypeProperties-Abschnitt:

Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | Abfragezeichenfolge Azure-Tabelle. Beispiele im nächsten Abschnitt. | Nein. Wenn ein Tabellenname ohne einen AzureTableSourceQuery angegeben wird, werden alle Datensätze aus der Tabelle in das Ziel kopiert. Wenn eine AzureTableSourceQuery angegeben wird, werden Datensätze aus der Tabelle, die die Abfrage entspricht zum Ziel kopiert.
azureTableSourceIgnoreTableNotFound | Geben Sie an, ob schlucken Ausnahme der Tabelle nicht vorhanden. | TRUE<br/>FALSE | Nein |

### <a name="azuretablesourcequery-examples"></a>AzureTableSourceQuery Beispiele

Wenn Azure Tabelle Spalte Typ String: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Wenn Azure Spalte vom Typ Datetime ist: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** unterstützt die folgenden Eigenschaften im TypeProperties-Abschnitt:


Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Partition Standardwert, die durch den Empfänger verwendet werden kann. | Ein String-Wert. | Nein 
azureTablePartitionKeyName | Namen der Spalte, deren Werte als Partitionsschlüssel verwendet werden. Wenn nicht angegeben, wird AzureTableDefaultPartitionKeyValue als Partitionsschlüssel verwendet. | Ein Spaltenname. | Nein |
azureTableRowKeyName | Namen der Spalte, deren Werte als Zeilenschlüssel verwendet werden. Wenn nicht angegeben, verwenden Sie eine GUID für jede Zeile. | Ein Spaltenname. | Nein  
azureTableInsertType | Der Modus zum Einfügen von Daten in Azure-Tabelle.<br/><br/>Diese Eigenschaft steuert, ob Zeilen der Ausgabetabelle übereinstimmenden Schlüsseln Partition und Zeile ihre Werte ersetzt oder zusammengeführt haben. <br/><br/>Informationen über die Funktionsweise dieser Einstellung (Zusammenführen und ersetzen) finden Sie unter [Einfügen oder Zusammenführen](https://msdn.microsoft.com/library/azure/hh452241.aspx) und [Einfügen oder ersetzen](https://msdn.microsoft.com/library/azure/hh452242.aspx) . <br/><br> Diese Einstellung gilt auf Zeilenebene nicht der Tabellenebene und keine Option löscht Zeilen in der Ausgabetabelle, die in der Eingabe nicht. | Zusammenführen (Standard)<br/>Ersetzen | Nein 
writeBatchSize | Fügt Daten in Azure-Tabelle, wenn WriteBatchSize oder WriteBatchTimeout erreicht wird. | Ganzzahl (Zeilenanzahl)| Keine (Standardwert: 10000) 
writeBatchTimeout | Fügt Daten in Azure-Tabelle, wenn WriteBatchSize oder WriteBatchTimeout erreicht wird | TimeSpan<br/><br/>Beispiel: "00:20 00" (20 Minuten) | Nein (um Speicher Client Standardtimeout Standardwert 90 Sekunden)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Zuordnen einer Quellspalte einer Zielspalte Übersetzer JSON-Eigenschaft verwenden, bevor die Zielspalte als der AzureTablePartitionKeyName verwenden können.

Im folgenden Beispiel wird Spalte DivisionID in die Zielspalte zugeordnet: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

Die DivisionID wird als Partitionsschlüssel angegeben. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Zuordnung für Azure-Tabelle

Wie im Artikel [Datenaktivitäten](data-factory-data-movement-activities.md) führt kopieraktivität automatische Konvertierung von Quelltypen mit folgenden zweistufiges auffangen.

1. Konvertieren von Typen von systemeigenen .NET Typ
2. Konvertieren von .NET Typ native Senke Typ

Beim Verschieben von Daten zu und von Azure-Tabelle die folgenden [Zuordnungen festgelegten Azure-Diensts](https://msdn.microsoft.com/library/azure/dd179338.aspx) wird von Azure Tabelle OData-Typen in .NET-Typ und umgekehrt. 

| OData-Datentyp | .NET-Typ | Details |
| --------------- | --------- | ------- |
| Edm.Binary | Byte] | Ein Array von Bytes bis zu 64 KB. |
| Edm.Boolean | bool | Ein boolescher Wert. |
| Edm.DateTime | DateTime | Ein 64-Bit-Wert, ausgedrückt als koordinierte Weltzeit (UTC). Die unterstützten DateTime beginnt von 12:00 Uhr, 1. Januar 1601 n. Chr. (UNSERE ZEITRECHNUNG) UTC. Der Bereich endet am 31. Dezember 9999. |
| Edm.Double | Doppelte | Eine 64-Bit-Gleitkommawert. |
| Edm.Guid | GUID | Ein 128-Bit-global eindeutiger Bezeichner. |
| Edm.Int32 | Int32 oder int | Eine 32-Bit-Ganzzahl. |
| Int64 | Int64 oder long | Eine 64-Bit-Ganzzahl. |
| Edm.String | Zeichenfolge | UTF-16-codierten Wert. Zeichenfolgenwerte können bis zu 64 KB sein. |

### <a name="type-conversion-sample"></a>Konvertierung Beispiel

Im folgende Beispiel wird zum Kopieren von Daten aus einer Azure Blob Azure Tabelle mit Typumwandlungen. 

Angenommen Sie, das Blob Dataset im CSV-Format ist und enthält drei Spalten. Davon ist einer Datetime-Spalte mit einem benutzerdefinierten Datetime Format abgekürzten französische Namen für den Wochentag. 

Definieren Sie BLOB-Quell-Dataset wie folgt mit Typdefinitionen für die Spalten.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

.NET Typ die Zuordnung von Azure Tabelle OData Typ gegeben, definieren Sie die Tabelle in Azure-Tabelle durch das folgende Schema. 

**Azure Tabellenschema:**

Spaltenname | Typ
----------- | --------
Benutzer-ID | Int64
Name | Edm.String 
LastLoginDate | Edm.DateTime

Anschließend definieren Sie Dataset Azure-Tabelle wie folgt. Sie müssen nicht mit den Typinformationen "Struktur" Abschnitt angeben, da die Informationen bereits im zugrunde liegenden Datenspeicher angegeben.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Data Factory geben in diesem Fall automatisch mit Datetime-Feld unter Verwendung der Kultur "fr-fr" beim Verschieben von Daten von Blob Azure Tabelle benutzerdefinierte Datetime-Format konvertiert.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Zu den wichtigsten Faktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren finden Sie unter [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md).







