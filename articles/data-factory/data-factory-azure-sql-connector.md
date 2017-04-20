<properties 
    pageTitle="Verschieben von Daten und Azure SQL-Datenbank | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten mithilfe von Azure Data Factory Azure SQL-Datenbank." 
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

# <a name="move-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Verschieben von Daten zu und von Azure SQL-Datenbank mit Azure Data Factory
Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung Daten Azure SQL-Datenbank von/zu einem anderen verschieben. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und unterstützten Speicher-Kombinationen. 

## <a name="supported-sources-and-sinks"></a>Unterstützte Datenquellen und Datensenken
Siehe [unterstützte Datenspeichern](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Tabelle Datenspeicher als Quellen oder senken von kopieraktivität unterstützt. Sie können Daten aus einem beliebigen Datenspeicher unterstützte Datenquelle in Azure SQL-Datenbank oder Azure SQL-Datenbank alle unterstützten Senke Datenspeicher verschieben.

## <a name="create-pipeline"></a>Pipeline erstellen
Sie können eine Rohrleitung mit einer Kopie erstellen, die Daten einer Azure SQL-Datenbank mit anderen Tools-APIs bewegt.  

- Assistent zum Kopieren von
- Azure-portal
- Visual Studio
- Azure PowerShell
- .NET API
- REST-API

Eine schrittweise Anleitung zum Erstellen einer Pipeline auf unterschiedliche Weise mit einer Kopie finden Sie unter [Kopieren Aktivität Tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) .   

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline erstellen, die Daten in Azure SQL-Datenbank wird mithilfe des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Die folgenden Beispiele bieten Beispiel JSON-Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Sie zeigen, wie Daten zwischen Azure SQL-Datenbank und Azure BLOB-Speicher kopieren. Allerdings können kopierten **direkt** von Quellen zu der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.

## <a name="sample-copy-data-from-azure-sql-database-to-azure-blob"></a>Beispiel: Daten aus Azure SQL-Datenbank in Azure Blob

Identisch definiert die folgenden Data Factory Entitäten:

1. Eine verknüpfte Dienst vom Typ [AzureSqlDatabase](#azure-sql-linked-service-properties).
2. Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureSqlTable](#azure-sql-dataset-type-properties). 
4. Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [SQLSource aus](#azure-sql-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Das Beispiel Kopiervorgang Zeitreihen (stündlich, täglich, etc.) aus einer Tabelle in SQL Azure-Datenbank in ein Blob stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben.  

**Azure verknüpften SQL-Dienst**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Siehe Abschnitt [Azure SQL verknüpften Serviceartikel](#azure-sql-linked-service-properties) Liste unterstützt diesen Dienst verknüpften Eigenschaften. 

**Dienst von Azure BLOB-Speicher verknüpft**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Finden Sie den [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) für die Liste unterstützt diesen Dienst verknüpften Eigenschaften. 

**Azure SQL Eingabedatasets**

Das Beispiel wird vorausgesetzt, Sie haben eine Tabelle "MyTable" in SQL Azure und enthält eine Spalte namens "Timestampcolumn" für zeitreihendaten. 

Einstellung "extern": "true" wird dem Azure Data Factory-Dienst informiert, Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
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

Finden Sie im Abschnitt [SQL Azure Dataset Eigenschaften](#azure-sql-dataset-type-properties) der Liste der Eigenschaften, die von diesem Typ Dataset unterstützt.  

**Azure Blob Ausgabe dataset**

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch anhand die Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet Jahr, Monat, Tag und Teile Stunden von der Startzeit entfernt. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
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

Siehe Abschnitt [Azure Blob Dataset Eigenschaften](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) für die Liste von solchen Dataset unterstützt.  

**Pipeline mit Kopieren**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In der Pipeline JSON-Definition **Geben** soll **SQLSource aus** und **Senke** Typ auf **BlobSink**festgelegt ist. Für die **SqlReaderQuery** -Eigenschaft angegebene SQL-Abfrage wählt die Daten in der letzten Stunde kopieren.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

Im Beispiel wird die **SqlReaderQuery** für die SQLSource aus angegeben. Kopieren-Aktivität führt diese Abfrage für Quelle Azure SQL-Datenbank zum Abrufen der Daten. Alternativ können Sie eine gespeicherte Prozedur nach Angabe von **SqlReaderStoredProcedureName** und **StoredProcedureParameters** (wenn die gespeicherte Prozedur Parameter) angeben.

Wenn Sie keine SqlReaderQuery oder SqlReaderStoredProcedureName angeben, wird im Abschnitt Struktur des Datasets JSON definierten Spalten eine Abfrage Azure SQL-Datenbank erstellen. Beispiel: `select column1, column2 from mytable`. Wenn die Dataset-Definition nicht Struktur aufweisen, werden alle Spalten aus der Tabelle ausgewählt. 


Finden Sie die [Sql-Quelle](#sqlsource) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) für die Liste der Eigenschaften von SQLSource aus und BlobSink unterstützt. 


## <a name="sample-copy-data-from-azure-blob-to-azure-sql-database"></a>Beispiel: Daten aus Azure Blob in Azure SQL-Datenbank

Im Beispiel wird die folgenden Data Factory Entitäten definiert:  

1.  Eine verknüpfte Dienst vom Typ [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties)verwendet.

Beispiel Kopien Zeitreihen Azure Daten (stündlich, täglich, etc.) zu einer Tabelle in SQL Azure BLOB-Datenbank jede Stunde. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 


**Azure verknüpften SQL-Dienst**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Siehe Abschnitt [Azure SQL verknüpften Serviceartikel](#azure-sql-linked-service-properties) Liste unterstützt diesen Dienst verknüpften Eigenschaften. 

**Dienst von Azure BLOB-Speicher verknüpft**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Finden Sie den [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) für die Liste unterstützt diesen Dienst verknüpften Eigenschaften.

**Azure Blob Eingabedatasets**

Daten werden abgeholt ein neues Blob stündlich (Häufigkeit: Stunde, Intervall: 1). Pfad und Namen des für den Blob werden dynamisch anhand der Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet, Jahr, Monat und Tagesanteil der Startzeit und Dateinamen verwendet Stundenteil der Startzeit. "externe": "true" Einstellung informiert Service Data Factory, diese Tabelle Data Factory ist nicht durch eine Aktivität im Werk Daten erzeugt.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
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

Siehe Abschnitt [Azure Blob Dataset Eigenschaften](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) für die Liste von solchen Dataset unterstützt.

**Azure SQL-Ausgabe-dataset**

Im Beispiel werden Daten in eine Tabelle namens "MyTable" SQL Azure kopiert. Erstellen Sie die Tabelle in SQL Azure mit der gleichen Anzahl von Spalten wie BLOB-CSV-Datei gewünscht enthalten. Neue Zeilen werden stündlich zur Tabelle hinzugefügt. 

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Finden Sie im Abschnitt [SQL Azure Dataset Eigenschaften](#azure-sql-dataset-type-properties) der Liste der Eigenschaften, die von diesem Typ Dataset unterstützt.

**Pipeline mit Kopieren**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **BlobSource** und **SqlSink** **Senke** Typ festgelegt ist.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
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

Finden Sie die [Sql-Senke](#sqlsink) und [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) für die Liste der Eigenschaften von SqlSink und BlobSource unterstützt. 


## <a name="azure-sql-linked-service-properties"></a>Eigenschaften von verknüpften SQL Azure
In den Beispielen haben Sie einen verknüpften Dienst vom Typ **AzureSqlDatabase** verknüpfen eine Azure SQL-Datenbank mit Daten Factory verwendet. Die folgende Tabelle beschreibt für JSON-Elemente für verknüpfte Azure SQL Service.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **AzureSqlDatabase** | Ja |
| connectionString | Geben Sie Informationen zur Verbindung mit Azure SQL-Datenbank-Instanz für die ConnectionString-Eigenschaft. | Ja |

> [AZURE.NOTE] Konfigurieren Sie [Azure SQL-Datenbank-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) zu [Azure Services auf den Server zugreifen können](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Außerdem, wenn Sie Daten in Azure SQL-Datenbank kopieren davon außerhalb Azure aus lokalen Datenquellen mit Data Factory Gateway, konfigurieren Sie geeignete IP-Adressbereich für den Computer, der Senden von Daten an Azure SQL-Datenbank. 

## <a name="azure-sql-dataset-type-properties"></a>Azure SQL Dataset Eigenschaften
In den Beispielen haben Sie ein Dataset vom Typ **AzureSqlTable** Darstellung eine Tabelle in einer Azure SQL-Datenbank verwendet. 

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..). 

Die TypeProperties unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. Typ **AzureSqlTable** Abschnitt **TypeProperties** für das Dataset hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Tabellenname | Name der Tabelle in Azure SQL-Datenbank-Instanz, der verknüpfter Dienst bezieht. | Ja |

## <a name="azure-sql-copy-activity-type-properties"></a>Azure SQL Kopie Aktivitätseigenschaften
Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar.

> [AZURE.NOTE] Die Kopie wird nur eine Eingabe und nur eine Ausgabe.

Eigenschaften im Abschnitt **TypeProperties** der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken. 

Wenn Sie Daten aus einer SQL Azure-Datenbank verschieben, legen Sie die Herkunftsart die kopieraktivität **SQLSource aus**. Entsprechend Daten mit einer Azure SQL-Datenbank verschieben, geben Sie die Senke in die kopieraktivität **SqlSink**. Dieser Abschnitt enthält eine Liste der Eigenschaften von SQLSource aus und SqlSink unterstützt. 

### <a name="sqlsource"></a>SQLSource aus

Kopieren-Aktivität wird die Quelle des Typs **SQLSource aus**stehen die folgenden Eigenschaften im **TypeProperties** -Abschnitt:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | SQL-Abfragezeichenfolge. Beispiel: `select * from MyTable`.  | Nein |
| sqlReaderStoredProcedureName | Name der gespeicherten Prozedur, die Daten aus der Quelltabelle gelesen. | Name der gespeicherten Prozedur. | Nein |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur. | Name/Wert-Paaren. Namen und Gehäuse der Parameter müssen den Namen und die Schreibweise der Parameter der gespeicherten Prozedur übereinstimmen. | Nein |

Wenn für die SQLSource aus der **SqlReaderQuery** angegeben ist, führt Aktivität kopieren diese Abfrage für Azure SQL-Datenbank-Datenquelle zum Abrufen der Daten. Alternativ können Sie eine gespeicherte Prozedur nach Angabe von **SqlReaderStoredProcedureName** und **StoredProcedureParameters** (wenn die gespeicherte Prozedur Parameter) angeben. 

Wenn Sie keine SqlReaderQuery oder SqlReaderStoredProcedureName angeben, werden die Spalten gemäß Abschnitt Struktur des Datasets JSON zum Erstellen einer Abfrage verwendet (`select column1, column2 from mytable`) Azure SQL-Datenbank ausgeführt. Wenn die Dataset-Definition nicht Struktur aufweisen, werden alle Spalten aus der Tabelle ausgewählt. 

> [AZURE.NOTE] Wenn Sie **SqlReaderStoredProcedureName**verwenden, müssen Sie einen Wert für die Eigenschaft **TableName** im Dataset JSON angeben. Gibt es keine Validierung für diese Tabelle jedoch ausgeführt. 

### <a name="sqlsource-example"></a>SQLSource aus Beispiel

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Die Definition der gespeicherten Prozedur:** 

    CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
    (
        @stringData varchar(20),
        @id int
    )
    AS
    SET NOCOUNT ON;
    BEGIN
         select *
         from dbo.UnitTestSrcTable
         where dbo.UnitTestSrcTable.stringData != stringData
        and dbo.UnitTestSrcTable.id != id
    END
    GO


### <a name="sqlsink"></a>SqlSink 

**SqlSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Wartezeit für den Batch Einfügevorgang abgeschlossen, bevor das Zeitlimit überschritten. | TimeSpan<br/><br/> Beispiel: "00:30:00" (30 Minuten). | Nein | 
| writeBatchSize | Fügt Daten in die SQL-Tabelle die Puffergröße WriteBatchSize erreicht. | Ganzzahl (Zeilenanzahl)| Keine (Standardwert: 10000)
| sqlWriterCleanupScript | Geben Sie eine Abfrage kopieren Aktivität auszuführen, Daten in einem bestimmten Segment bereinigt werden. Weitere Informationen finden Sie im [Abschnitt Wiederholbarkeit](#repeatability-during-copy). | Eine Anweisung.  | Nein |
| sliceIdentifierColumnName | Geben Sie einen Spaltennamen für Kopie mit automatisch generierten Segment ID Füllen mit Daten von einem bestimmten Segment erneut beim Bereinigen. Weitere Informationen finden Sie im [Abschnitt Wiederholbarkeit](#repeatability-during-copy). | Spaltenname einer Spalte mit Daten binary(32). | Nein |
| sqlWriterStoredProcedureName | Name der gespeicherten Prozedur Upserts (Updates/Insert) Daten in die Zieltabelle. | Name der gespeicherten Prozedur. | Nein |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur. | Name/Wert-Paaren. Namen und Gehäuse der Parameter müssen den Namen und die Schreibweise der Parameter der gespeicherten Prozedur übereinstimmen. | Nein | 
| sqlWriterTableType | Geben Sie einen Typ Tabellennamen in der gespeicherten Prozedur verwendet werden. Kopieraktivität zur Verfügung verschobenen Daten in einer temporären Tabelle mit dieser Tabelle. Gespeicherte Prozedur kann dann die Daten mit vorhandenen Daten kopiert zusammenführen. | Ein Tabellenname Typ. | Nein |

#### <a name="sqlsink-example"></a>SqlSink-Beispiel

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## <a name="identity-columns-in-the-target-database"></a>Identitätsspalten in der Zieldatenbank
Dieser Abschnitt enthält ein Beispiel zum Kopieren von Daten aus einer Quelltabelle ohne Identity-Spalte in eine Zieltabelle mit einer Identitätsspalte. 

**Tabelle:** 

    create table dbo.SourceTbl
    (
           name varchar(100),
           age int
    )

**Zieltabelle:**

    create table dbo.TargetTbl
    (
           id int identity(1,1),
           name varchar(100),
           age int
    )


Beachten Sie, dass die Zieltabelle eine Identitätsspalte besitzt. 

**Quelle Dataset JSON-definition**

    {
        "name": "SampleSource",
        "properties": {
            "type": " SqlServerTable",
            "linkedServiceName": "TestIdentitySQL",
            "typeProperties": {
                "tableName": "SourceTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }

**Ziel-Dataset JSON-definition**

    {
        "name": "SampleTarget",
        "properties": {
            "structure": [
                { "name": "name" },
                { "name": "age" }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "TestIdentitySQLSource",
            "typeProperties": {
                "tableName": "TargetTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": false,
            "policy": {}
        }   
    }


Beachten, die die Tabelle Quelle und Ziel unterschiedliche Schema (Ziel hat eine zusätzliche Spalte mit). In diesem Szenario müssen Sie **Struktur** -Eigenschaft im Ziel-Dataset-Definition angegeben werden, die die Identity-Spalte enthält. 

Anschließend ordnen Sie Spalten Quell-Dataset Spalten im Ziel-Dataset. Siehe beispielsweise [Beispiele zuordnen](#column-mapping-samples) . 

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-sql-server--azure-sql-database"></a>Zuordnung für SQL Server und Azure SQL-Datenbank

Wie in Ausführende der [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel kopieren automatische Konvertierung von Quelltypen zum Auffangen von Typen mit dem folgenden Schritt 2:

1. Konvertieren von Typen von systemeigenen .NET Typ
2. Konvertieren von .NET Typ native Senke Typ

Beim Verschieben von Daten zu und von SQL Azure, SQL Server, Sybase folgenden Zuordnungen von SQL-Typ in .NET-Typ und umgekehrt verwendet werden.

Die Zuordnung ist identisch mit dem SQL Server Zuordnung für ADO.NET.

| SQL Server-Datenbank-Engine-Typ | .NET Framework-Typ |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| Binär | Byte] |
| Bit | Boolescher Wert |
| Char | String, Char] |
| Datum | DateTime |
| DateTime | DateTime |
| datetime2 | DateTime |
| DateTimeOffset | DateTimeOffset |
| Dezimal | Dezimal |
| FILESTREAM-Attribut (varbinary(max)) | Byte] |
| Float | Double |
| Bild | Byte] | 
| int | Int32 | 
| Money | Dezimal |
| NCHAR | String, Char] |
| ntext | String, Char] |
| numerische | Dezimal |
| nvarchar | String, Char] |
| Real | Einzelne |
| RowVersion | Byte] |
| smalldatetime | DateTime |
| smallint | Int16 |
| smallmoney | Dezimal | 
| sql_variant | Objekt * |
| Text | String, Char] |
| Zeit | TimeSpan |
| Zeitstempel | Byte] |
| tinyint | Byte |
| uniqueidentifier | GUID |
| varbinary |  Byte] |
| varchar | String, Char] |
| XML | XML |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.