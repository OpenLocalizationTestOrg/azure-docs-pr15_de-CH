<properties 
    pageTitle="Verschieben von Daten von mit Data Factory MongoDB | Microsoft Azure" 
    description="Enthält Informationen zum Verschieben von Daten von MongoDB Datenbank Azure Data Factory." 
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
    ms.date="08/04/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Verschieben von Daten von MongoDB mit Azure Data Factory

Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung zum Verschieben von Daten aus einer lokalen MongoDB-Datenbank auf einem anderen. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und Quelle-Senke Daten Speicher Kombinationen, die kopieraktivität unterstützt.

Data Factory unterstützt lokale MongoDB Quellen Data Management Gateway herstellen. Anzeigen Sie [Data Management Gateway](data-factory-data-management-gateway.md) Data Management Gateway Informationen und [Daten lokal in die cloud verschieben](data-factory-move-data-between-onprem-and-cloud.md) um eine schrittweise Anleitung zum Einrichten von Gateway einer Datenpipeline Daten 

> [AZURE.NOTE] Sie müssen mit dem Gateway MongoDB herstellen, auch wenn es in Azure IaaS VMs gehostet wird. Wenn Sie eine Instanz der gehostete Cloud MongoDB herstellen, können Sie die Gatewayinstanz IaaS-VM installieren.

Data Factory unterstützt derzeit nur Daten von MongoDB in anderen Datenspeichern, jedoch nicht zum Verschieben von Daten aus anderen Datenspeichern in MongoDB.

## <a name="prerequisites"></a>Erforderliche Komponenten
Für den Dienst Azure Data Factory Ihrer lokalen MongoDB-Datenbank herstellen können müssen Sie die folgenden Komponenten installieren: 

- Data Management Gateway 2.0 oder höher auf dem Computer, der die Datenbank hostet oder auf einem separaten Computer zu Wettbewerb um Ressourcen mit der Datenbank. Data Management Gateway ist eine Software, die sicheren und verwalteten lokalen Datenquellen mit Cloud-Diensten verbindet. Siehe [Data Management Gateway](data-factory-data-management-gateway.md) Artikel über Data Management Gateway.
  
    Bei der Installation des Gateways wird automatisch einen Verbindung mit MongoDB verwendeten Microsoft MongoDB ODBC-Treiber installiert. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline zu erstellen, die Daten aus einer Datenbank MongoDB aller unterstützten Senke Datenspeicher kopiert wird mithilfe des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Das folgende Beispiel enthält Beispiel JSON Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Sie zeigen, wie Daten aus Datenbank MongoDB in Azure BLOB-Speicher kopiert. Allerdings können Daten auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) mithilfe der Kopieraktivität in Azure Data Factory kopiert werden.

## <a name="sample-copy-data-from-mongodb-to-azure-blob"></a>Beispiel: Daten aus MongoDB in Azure Blob
Dieses Beispiel zeigt, wie Daten aus einer lokalen MongoDB-Datenbank in Azure BLOB-Speicher kopiert. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesMongoDb](#linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [MongoDbCollection](#dataset-type-properties).
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [MongoDbSource](#copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Das Beispiel Kopiervorgang aus einem Abfrageergebnis MongoDB-Datenbank in ein Blob stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 

Als ersten Schritt setup Data Management Gateway nach Artikel [Data Management Gateway](data-factory-data-management-gateway.md) . 

**MongoDB verknüpft service**

    { 
        "name": "OnPremisesMongoDbLinkedService", 
        "properties": 
        { 
            "type": "OnPremisesMongoDb", 
            "typeProperties": 
            { 
                "authenticationType": "<Basic or Anonymous>", 
                "server": "< The IP address or host name of the MongoDB server >",  
                "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>", 
                "username": "<username>", 
                "password": "<password>",
               "authSource": "< The database that you want to use to check your credentials for authentication. >",
                "databaseName": "<database name>",
                "gatewayName": "<mygateway>"
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

**MongoDB Eingabedatasets** Einstellung "extern": "true" informiert Data Factory-Dienst, dass die Tabelle Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.
    
    {
         "name":  "MongoDbInputDataset",
        "properties": { 
            "type": "MongoDbCollection", 
            "linkedServiceName": "OnPremisesMongoDbLinkedService", 
            "typeProperties": { 
                "collectionName": "<Collection name>"   
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
                "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
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

Die Pipeline enthält eine Aktivität kopieren, die die obigen Eingabe und Ausgabe Datasets konfiguriert und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **MongoDbSource** und **BlobSink** **Senke** Typ festgelegt ist. Für die **Abfrage** -Eigenschaft angegebene SQL-Abfrage wählt die Daten in der letzten Stunde kopieren.
    
    {
        "name": "CopyMongoDBToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "MongoDbSource",
                            "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MongoDbInputDataset"
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
                    "name": "MongoDBToAzureBlob"
                }
            ],
            "start": "2016-06-01T18:00:00Z",
            "end": "2016-06-01T19:00:00Z"
        }
    }


## <a name="linked-service-properties"></a>Verknüpfte Eigenschaften

Die folgende Tabelle beschreibt JSON Elemente für Service **OnPremisesMongoDB** verknüpft.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **OnPremisesMongoDb** | Ja |
| Server | IP-Adresse oder Name des Server MongoDB. | Ja | 
| Anschluss | TCP-Port, MongoDB-Server für Clientverbindungen verwendet. | Optional, Standardwert: 27017 |
| Read | Grundlegende oder anonym. | Ja | 
| Benutzername | Benutzerkonto MongoDB auf. | Ja (wenn die Standardauthentifizierung verwendet wird).|
| Kennwort | Kennwort für den Benutzer. |   Ja (wenn die Standardauthentifizierung verwendet wird). | 
| authSource | Name der MongoDB-Datenbank, die Sie zum Überprüfen der Anmeldeinformationen für die Authentifizierung verwenden möchten. | Optional (wenn die Standardauthentifizierung verwendet wird). Standard: verwendet das Administratorkonto und die Datenbank mit DatabaseName-Eigenschaft angegeben. |  
| Datenbankname | Name der MongoDB-Datenbank, auf die Sie zugreifen möchten. | Ja |
| gatewayName | Name des Gateways, die auf den Datenspeicher zugreift. | Ja | 
| encryptedCredential | Anmeldeinformationen verschlüsselt von Gateway. | Optionale |


Informationen zum Festlegen von Anmeldeinformationen für eine lokale MongoDB-Datenquelle finden Sie unter [festlegen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="dataset-type-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die **TypeProperties** unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. TypeProperties Abschnitt Dataset vom Typ **MongoDbCollection** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Auflistungsname | Name der Sammlung MongoDB-Datenbank. | Ja | 

## <a name="copy-activity-type-properties"></a>Typeneigenschaften kopieren

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt **TypeProperties** der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

Bei die Quelle vom Typ **MongoDbSource** stehen die folgenden Eigenschaften im TypeProperties-Abschnitt:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Abfrage | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | SQL-92-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. | Nein ( **CollectionName** **Dataset** angegeben wird). | 

## <a name="schema-by-data-factory"></a>Schema von Data Factory
Azure Data Factory-Dienst leitet Schema aus MongoDB mithilfe der neuesten 100 Dokumente in der Auflistung ab. Wenn diese 100 Dokumente keine vollständigen Schemas enthalten möglicherweise einige Spalten während des Kopiervorgangs ignoriert. 

## <a name="type-mapping-for-mongodb"></a>Zuordnung für MongoDB

Wie im Artikel [Datenaktivitäten](data-factory-data-movement-activities.md) führt kopieraktivität automatische Konvertierung von Quelltypen zum Auffangen von Typen mit dem folgenden Schritt 2:

1. Konvertieren von Typen von systemeigenen .NET Typ
2. Konvertieren von .NET Typ native Senke Typ

Beim Verschieben von Daten in MongoDB dienen die folgenden Zuordnungen aus MongoDB zu.

| MongoDB-Typ | .NET Framework-Typ |
| ------------------- | ------------------- | 
| Binäre | Byte] |
| Boolescher Wert | Boolescher Wert |
| Datum | DateTime |
| NumberDouble | Double |
| NumberInt | Int32 |
| NumberLong | Int64 |
| ObjectID | Zeichenfolge |
| Zeichenfolge | Zeichenfolge |
| UUID | GUID | 
| Objekt | Renormalisiert in "_" als Trennzeichen für geschachtelte Spalten reduzieren |

> [AZURE.NOTE]  
> Informationen zu Unterstützung für Arrays virtuellen Tabellen finden Sie [Unterstützung für komplexe Typen mit virtuellen Tabellen](#support-for-complex-types-using-virtual-tables) unten. 

MongoDB-Datentypen werden derzeit nicht unterstützt: DBPointer, JavaScript, Max/Min Schlüssel, Ausdruck, Symbol, Zeitstempel, undefiniert

## <a name="support-for-complex-types-using-virtual-tables"></a>Unterstützung für komplexe Typen mit virtuellen Tabellen
Azure Data Factory verwendet einen integrierten ODBC-Treiber und Daten aus der Datenbank MongoDB. Für komplexe Typen wie Arrays oder Objekte mit unterschiedlichen in Dokumenten normalisiert wieder der Treiber Daten in entsprechende virtuelle Tabellen. Wenn eine Tabelle diese Spalten enthält, wird der Treiber, virtuellen Tabellen generiert:

-   Eine **Basistabelle**, die die gleichen Daten wie die tatsächliche Tabelle außer den komplexen Typ-Spalten enthält. Die Basistabelle wird derselbe Name wie der eigentlichen Tabelle darstellt.
-   Eine **virtuelle Tabelle** für jede Spalte komplexen Typ geschachtelten Daten erweitert. Virtuellen Tabellen heißen den Namen der eigentlichen Tabelle Trennzeichen "_" und den Namen der das Array oder Objekt.

Virtuelle Tabellen beziehen sich auf die Daten in der eigentlichen Tabelle Aktivieren des Treibers auf die denormalisierten Daten zugreifen. Siehe Beispielabschnitt Details. Sie können Inhalt MongoDB Arrays Abfragen und virtuellen Tabellen zugreifen.

Sie können mithilfe des [Assistenten zum Kopieren von](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuitiv die Liste der Tabellen in MongoDB-Datenbank einschließlich virtuellen Tabellen anzeigen und eine Vorschau der Daten in. Sie erstellen eine Abfrage in der Kopie und das Ergebnis zu überprüfen.

### <a name="example"></a>Beispiel

Beispielsweise ist "ExampleTable" unten eine MongoDB Tabelle eine Spalte mit einem Array von Objekten in jeder Zelle – Rechnung und eine Spalte mit einem skalare Typen – Filter. 

_id | Kundenname | Rechnung | Service-Level | Bewertung
--- | ------------- | -------- | ------------- | -------
1111 | ABC | [{Invoice_id: "123" Element: "Toaster", Preis: "456" Rabatt: "0,2"}, {Invoice_id: "124" Element: "Ofen", Preis: "1235" Rabatt: "0,2"}] | Silber | [5,6]
2222 | XYZ | [{Invoice_id: "135" Element: "Kühlschrank", Preis: "12543" Rabatt: "0,0"}] | Gold | [1,2]

Der Treiber erzeugt mehrere virtuelle Tabellen zu dieser Tabelle darstellen. Die erste virtuelle Tabelle ist der Basistabelle mit dem Namen "ExampleTable" unten. Die Basistabelle enthält die Daten der ursprünglichen Tabelle, aber die Daten aus den Arrays fehlt und in virtuellen Tabellen erweitert.

_id | Kundenname | Service-Level
--- | ------------- | -------------
1111 | ABC | Silber
2222 | XYZ | Gold

Die folgenden Tabellen zeigen die virtuellen Tabellen, die die ursprünglichen Arrays im Beispiel darstellen. Diese Tabellen enthalten Folgendes: 

- Ein Verweis auf den ursprünglichen Primärschlüsselspalte in die Zeile des ursprünglichen Arrays (über die Spalte _id)
- Angabe der Position der Daten im ursprünglichen array 
- Zusätzliche Daten für jedes Element innerhalb des Arrays

Tabelle "ExampleTable_Invoices":

_id | ExampleTable_Invoices_dim1_idx | invoice_id | Artikel | Preis | Rabatt
--- | -------------- | ---------- | ---- | ----- | --------
1111 | 0 | 123 | Toaster | 456 | 0,2
1111 | 1 | 124 | Ofen | 1235 | 0,2
2222 | 0 | 135 | Kühlschrank | 12543 | 0.0

Tabelle "ExampleTable_Ratings":

_id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings
--- | ------------- | -------------
1111 | 0 | 5
1111 | 1 | 6
2222 | 0 | 1
2222 | 1 | 2

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.

## <a name="next-steps"></a>Nächste Schritte
Finden Sie Artikel für eine schrittweise Anleitung zum Erstellen einer Datenpipeline, [Verschieben von Daten zwischen lokalen und](data-factory-move-data-between-onprem-and-cloud.md) Daten aus einem lokalen Datenspeicher in Azure Datenspeicher verschoben. 

