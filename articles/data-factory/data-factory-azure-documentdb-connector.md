<properties 
    pageTitle="Verschieben von Daten und DocumentDB | Microsoft Azure" 
    description="Erfahren Sie, wie Daten und Azure DocumentDB Auflistung Azure Data Factory verschieben" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Verschieben von Daten zu und von DocumentDB mit Azure Data Factory

Dieser Artikel beschreibt die Verwendung kopieren-Aktivität in einer Azure Daten Daten Azure DocumentDB aus anderen Daten speichern und Verschieben von Daten von DocumentDB in einem anderen Datenspeicher. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und unterstützten Speicher-Kombinationen.

Beispiele zeigen, wie Daten zwischen Azure DocumentDB und Azure BLOB-Speicher kopieren. Allerdings können kopierten **direkt** von Quellen zu der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  

> [AZURE.NOTE] Kopiert Daten aus lokalen/Azure IaaS Azure DocumentDB gespeichert und umgekehrt mit Data Management Gateway Version 2.1 und höher unterstützt.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Beispiel: Daten aus DocumentDB in Azure Blob

Das folgende Beispiel zeigt:

1. Eine verknüpfte Dienst vom Typ [DocumentDb](#azure-documentdb-linked-service-properties).
2. Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel werden Daten in Azure DocumentDB in Azure Blob kopiert. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben.

**Dienst für Azure DocumentDB verknüpft:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure BLOB-Speicher verknüpft Service:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure Dokument DB Eingabedatasets:**

Im Beispiel werden eine Sammlung namens **Person** in einer Datenbank Azure DocumentDB vorausgesetzt.
 
Einstellung "extern": "true" und ExternalData Informationen Azure Data Factory-Dienst, dass die Tabelle Data Factory ist und nicht durch eine Aktivität im Werk Daten angeben.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Azure Blob Ausgabe Dataset:**

Daten werden in ein neues Blob stündlich mit dem Pfad für bestimmte Datetime mit Stunde Granularität reflektieren Blob kopiert.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

JSON-Beispieldokument in der Sammlung in einer DocumentDB: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB unterstützt hierarchische JSON-Dokumente über SQL Syntax Abfragen Dokumente. 

Beispiel: SELECT Person.Name.Middle Person.PersonId Person.Name.First als Vorname als MiddleName, LastName Person.Name.Last als Person

Die folgende Pipeline kopiert Daten aus der Sammlung in der DocumentDB-Datenbank in Azure Blob. Als Teil der Kopie die Eingabe und Ausgabe wurden Datensätze angegeben.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Beispiel: Daten aus Azure Blob in Azure DocumentDB

Das folgende Beispiel zeigt:

1. Eine verknüpfte Dienst vom Typ [DocumentDb](#azure-documentdb-linked-service-properties).
2. Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties)verwendet.


Das Beispiel kopiert Daten von Azure Blob, Azure DocumentDB. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben.

**Azure BLOB-Speicher verknüpft Service:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Dienst für Azure DocumentDB verknüpft:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob Eingabedatasets:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB Ausgabe Dataset:**

Im Beispiel werden Daten in eine Sammlung namens "Person" kopiert.

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Die folgende Pipeline kopiert Daten von Azure Blob in der Sammlung in der DocumentDB. Als Teil der Kopie die Eingabe und Ausgabe wurden Datensätze angegeben. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Wenn der Blob Beispieleingabe ist 

    1,John,,Doe

Dann werden die Ausgabe JSON in DocumentDB als:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB ist ein NoSQL-Speicher für JSON-Dokumente, in dem geschachtelte Strukturen zulässig sind. Azure Data Factory ermöglicht Hierarchie über **NestingSeparator**bezeichnen die "." In diesem Beispiel. Mit dem Trennzeichen kopieraktivität generiert-Objekt "Name" mit drei untergeordnete Elemente, mittleren und letzten "Name.First", "Name.Middle" und "Name.Last" in der Tabellendefinition.

## <a name="azure-documentdb-linked-service-properties"></a>Azure DocumentDB verknüpften Eigenschaften

Die folgende Tabelle beschreibt JSON Elemente für Azure DocumentDB verknüpft Service. 

| **Eigenschaft** | **Beschreibung** | **Erforderlich** |
| -------- | ----------- | --------- |
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **DocumentDb** | Ja |
| connectionString | Geben Sie Informationen zum Azure DocumentDB Datenbank herstellen. | Ja |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure DocumentDB Dataset Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md) . Wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).
 
Die TypeProperties unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. Typ **DocumentDbCollection** Abschnitt TypeProperties für das Dataset hat die folgenden Eigenschaften.

| **Eigenschaft** | **Beschreibung** | **Erforderlich** |
| -------- | ----------- | -------- |
| Auflistungsname | Name der Dokumentgruppe DocumentDB. | Ja |


Beispiel:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Schema von Data Factory
Bei Schema Datenspeichern wie DocumentDB leitet Daten Factorydienst das Schema in einer der folgenden Methoden:  

1.  Wenn Sie die Struktur der Daten mithilfe der **Struktur** -Eigenschaft in der datasetdefinition angeben, berücksichtigt Daten Factorydienst diese Struktur als Schema. In diesem Fall enthält eine Zeile keinen Wert für eine Spalte, ein Nullwert für ihn bereitgestellt.
2.  Wenn Sie die Struktur der Daten nicht mit der **Struktur** -Eigenschaft in der datasetdefinition angeben, leitet der Data Factory-Dienst das Schema mithilfe der ersten Zeile der Daten. In diesem Fall enthält die erste Zeile nicht das vollständige Schema, werden einige Spalten im Ergebnis der Kopiervorgang fehlen.

Daher empfiehlt Schema Daten Quellen, die Struktur von Daten der **Struktur** -Eigenschaft angeben.

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure DocumentDB Kopie Typeneigenschaften

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar.
 
**Hinweis:** Die Kopie wird nur eine Eingabe und nur eine Ausgabe.

Eigenschaften im Abschnitt TypeProperties der Aktivität zum variieren nach jeder Aktivität und bei Aktivität kopieren sie je nach Arten von Datenquellen und Datensenken.

Bei kopieraktivität bei Quelle vom Typ **DocumentDbCollectionSource** stehen die folgenden Eigenschaften im **TypeProperties** -Abschnitt:

| **Eigenschaft** | **Beschreibung** | **Zulässige Werte** | **Erforderlich** |
| ------------ | --------------- | ------------------ | ------------ |
| Abfrage | Geben Sie die Abfrage, um Daten zu lesen. | Abfragezeichenfolge von DocumentDB unterstützt. <br/><br/>Beispiel:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | Nein <br/><br/>Wenn nicht angegeben, die SQL-Anweisung, die ausgeführt wird:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Sonderzeichen, um anzugeben, dass das Dokument geschachtelt ist | Beliebiges Zeichen. <br/><br/>DocumentDB ist ein NoSQL-Speicher für JSON-Dokumente, in dem geschachtelte Strukturen zulässig sind. Azure Data Factory ermöglicht Hierarchie über NestingSeparator, kennzeichnen ist "." in den Beispielen oben. Mit dem Trennzeichen kopieraktivität generiert-Objekt "Name" mit drei untergeordnete Elemente, mittleren und letzten "Name.First", "Name.Middle" und "Name.Last" in der Tabellendefinition. | Nein

**DocumentDbCollectionSink** unterstützt die folgenden Eigenschaften:

| **Eigenschaft** | **Beschreibung** | **Zulässige Werte** | **Erforderlich** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Sonderzeichen im Namen Quellspalte an verschachtelten Dokuments ist erforderlich. <br/><br/>Zum Beispiel oben: `Name.First` in der Ausgabe die folgende JSON-Struktur im Dokument DocumentDB Tabelle erzeugt:<br/><br/>"Name": {<br/>  "First": "John"<br/>}, | Zeichen, Schachtelungsebenen trennen.<br/><br/>Standardwert ist `.` (Punkt). | Zeichen, Schachtelungsebenen trennen. <br/><br/>Standardwert ist `.` (Punkt). | Nein | 
| writeBatchSize | Anzahl paralleler DocumentDB Dienst zum Erstellen von Dokumenten.<br/><br/>Beim Kopieren von Daten in DocumentDB mit dieser Eigenschaft können Sie die Leistung optimieren. Wenn Sie WriteBatchSize erhöhen, da mehr parallelen Anfragen an DocumentDB gesendet werden, erwarten Sie eine bessere Leistung. Jedoch müssen Sie vermeiden, die Drosselung können die Fehlermeldung auslösen: "Request ist groß".<br/><br/>Drosselung eine Reihe von Faktoren, einschließlich von Dokumenten, Anzahl der Dokumente, Indizierung Richtlinie Zielsammlung usw. bestimmt. Für Kopiervorgänge, können bessere Sammlung (z. B. S3) den Durchsatz verfügbar (2.500 Anforderung Einheiten pro Sekunde). | Ganze Zahl | Keine (Standardwert: 10000) |
| writeBatchTimeout | Wartezeit für den Vorgang abschließen, bevor das Zeitlimit überschritten. | TimeSpan<br/><br/> Beispiel: "00:30:00" (30 Minuten). | Nein |
 
## <a name="appendix"></a>Anhang
1. **Frage:**  
    Kopieraktivität Support Update der vorhandenen Datensätze werden?

    **Antwort:**  
    Nr.

2. **Frage:**  
    wie funktioniert eine Wiederholung eine Kopie DocumentDB Umgang mit bereits Datensätze kopiert?

    **Antwort:**  
    Wenn Datensätze ein Feld "ID" und der Kopiervorgang versucht, einen Datensatz mit der gleichen ID einzufügen, der Kopiervorgang löst einen Fehler aus.  
 
3. **Frage:** 
    Data Factory unterstützt [Bereich oder Hashbasierte Datenpartitionierung](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Antwort:** 
    Nr. 
4. **Frage:** 
    können mehrere DocumentDB-Auflistung für eine Tabelle angeben?
    
    **Antwort:** 
    Nr. Zu diesem Zeitpunkt kann nur eine Auflistung angegeben werden.
     
## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.
