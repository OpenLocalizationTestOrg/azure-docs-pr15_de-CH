<properties 
    pageTitle="Verschieben von Daten von mit Data Factory Cassandra | Microsoft Azure" 
    description="Enthält Informationen zum Verschieben von Daten aus einer lokalen Cassandra Datenbank mit Azure Data Factory." 
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
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Verschieben von Daten aus einer lokalen Cassandra Datenbank mit Azure Data Factory
Dieser Artikel beschreibt, wie Kopieraktivität in einer Azure Daten verwenden, Daten aus einer lokalen Cassandra Datenbank in jeder Spalte im Abschnitt [unterstützte Datenquellen und Datensenken](data-factory-data-movement-activities.md#supported-data-stores) Senke Datenspeicher kopiert. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und unterstützten Speicher-Kombinationen.

Data Factory unterstützt derzeit nur Daten aus einer Datenbank Cassandra [Senke Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores)unterstützt jedoch keine Daten aus anderen Datenspeichern Cassandra Datenbank.

## <a name="prerequisites"></a>Erforderliche Komponenten
Der Azure Data Factory Service mit der lokalen Cassandra Datenbank herstellen können müssen Sie Folgendes installieren: 

- Data Management Gateway 2.0 oder höher auf dem Computer, der die Datenbank hostet oder auf einem separaten Computer zu Wettbewerb um Ressourcen mit der Datenbank. Data Management Gateway ist eine Software, die sicheren und verwalteten lokalen Datenquellen mit Cloud-Diensten verbindet. Siehe [Verschieben von Daten zwischen lokalen und](data-factory-move-data-between-onprem-and-cloud.md) Artikel über Data Management Gateway.
  
    Bei der Installation des Gateways wird automatisch einen verwendeten zu Datenbank Cassandra Microsoft Cassandra ODBC-Treiber installiert. 

> [AZURE.NOTE] Tipps zur Behebung von Verbindungs-Gateways Siehe [Problembehandlung Gateway stellt](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Fragen. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline zu erstellen, die Daten aus einer Datenbank Cassandra aller unterstützten Senke Datenspeicher kopiert wird mithilfe des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Das folgende Beispiel enthält Beispiel JSON Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Sie zeigen, wie Daten aus Datenbank Cassandra in Azure BLOB-Speicher kopiert. Allerdings können Daten auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) mithilfe der Kopieraktivität in Azure Data Factory kopiert werden.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Beispiel: Daten Sie aus Cassandra BLOB
Im Beispiel kopiert Daten aus einer Datenbank Cassandra in Azure Blob fest stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. Daten können direkt an die Senken im Artikel [Datenaktivitäten](data-factory-data-movement-activities.md#supported-data-stores) mithilfe der Kopieraktivität in Azure Data Factory angegeben kopiert. 

- Eine verknüpfte Dienst vom Typ [OnPremisesCassandra](#onpremisescassandra-linked-service-properties).
- Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [CassandraTable](#cassandratable-properties).
- Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [CassandraSource](#cassandrasource-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

**Cassandra verknüpft service**

In diesem Beispiel verwendet der Dienst **Cassandra** verknüpft. Siehe Abschnitt [Cassandra verknüpfte Service](#onpremisescassandra-linked-service-properties) unterstützt diesen Dienst verknüpften Eigenschaften.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
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

**Cassandra Eingabedatasets**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
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

**Externe** auf **true** festlegen informiert Daten Factorydienst Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.

**Azure Blob Ausgabe dataset**

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pipeline mit Kopieren**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **CassandraSource** und **BlobSink** **Senke** Typ festgelegt ist. 

Finden Sie eine Liste der Eigenschaften, die RelationalSource unterstützt [RelationalSource Eigenschaften](#cassandrasource-type-properties) . 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
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
## <a name="onpremisescassandra-linked-service-properties"></a>Eigenschaften von OnPremisesCassandra verknüpft

Die folgende Tabelle beschreibt JSON Elemente für Cassandra verknüpft Service.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **OnPremisesCassandra** | Ja |
| Host | IP-Adressen oder Hostnamen Cassandra Server.<br/><br/>Geben Sie eine durch Trennzeichen getrennte Liste von IP-Adressen oder Hostnamen auf allen Servern gleichzeitig herstellen. | Ja | 
| Anschluss | Der TCP-Port, den Cassandra-Server für Clientverbindungen verwendet. | Nein, Standardwert: 9042 |
| Read | Grundlegende oder anonym | Ja |
| Benutzername |Geben Sie Benutzernamen für das Benutzerkonto. | Ja, wenn AuthenticationType Basic festgelegt ist. |
| Kennwort | Kennwort für das Benutzerkonto angeben.  | Ja, wenn AuthenticationType Basic festgelegt ist. |
| gatewayName | Der Name des Gateways, die Verbindung zu lokalen Cassandra Datenbank verwendet wird. | Ja |
| encryptedCredential | Anmeldeinformationen vom Gateway verschlüsselt. | Nein | 

## <a name="cassandratable-properties"></a>CassandraTable Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die **TypeProperties** unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. TypeProperties Abschnitt Dataset vom Typ **CassandraTable** hat die folgenden Eigenschaften

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| erledigten | Name der erledigten oder Schema Cassandra Datenbank. | Ja (Wenn **Abfrage** für **CassandraSource** nicht definiert ist). |
| Tabellenname | Name der Tabelle in Cassandra Datenbank. | Ja (Wenn **Abfrage** für **CassandraSource** nicht definiert ist). |


## <a name="cassandrasource-type-properties"></a>CassandraSource Eigenschaften
Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt TypeProperties der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

Wenn Quelle vom Typ **CassandraSource**ist, stehen die folgenden Eigenschaften im TypeProperties-Abschnitt:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Abfrage | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | SQL-92-Abfrage oder CQL Abfrage. Siehe [CQL](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Geben Sie beim SQL-Abfrage **erledigten name.table Namen** die Tabelle entspricht, die Sie abfragen möchten. | Nein (TableName und erledigten Datasets definiert sind).  |
| consistencyLevel | Die konsistenzebene gibt an, wie viele Replikate einer Leseoperation beantworten müssen, bevor Daten an die Clientanwendung zurückgegeben. Cassandra überprüft die angegebene Anzahl von Replikaten Daten lesen Anforderung. | EINS, ZWEI, DREI, QUORUM, ALLE LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. Einzelheiten finden Sie unter [Konfigurieren von Datenkonsistenz](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) . | Nein. Standardwert ist 1. |  


### <a name="type-mapping-for-cassandra"></a>Zuordnung für Cassandra
Cassandra Typ | .NET basierte Typ
--------------- | ---------------
ASCII | Zeichenfolge 
BIGINT | Int64
BLOB | Byte]
BOOLESCHER WERT | Boolescher Wert
DEZIMAL | Dezimal
DOUBLE | Double
FLOAT | Einzelne
INET | Zeichenfolge
INT | Int32
TEXT | Zeichenfolge
ZEITSTEMPEL | DateTime
TIMEUUID | GUID
UUID | GUID
VARCHAR | Zeichenfolge
VARINT | Dezimal

> [AZURE.NOTE]  
> Auflistung finden [mit Cassandra Auflistungstypen virtuelle Tabelle](#work-with-collections-using-virtual-table) Abschnitt Typen (Karte, Satz, Liste, etc.). 
> 
> Benutzerdefinierte Typen werden nicht unterstützt.
> 
> Die Länge des Binary-Spalte und die Spalte Länge kann nicht größer als 4000 sein. 

## <a name="work-with-collections-using-virtual-table"></a>Arbeiten Sie mit virtuellen Tabelle
Azure Data Factory verwendet einen integrierten ODBC-Treiber und Daten aus der Datenbank Cassandra. Einschließlich, Zuordnung und Liste Auflistungstypen normalisiert wieder der Treiber Daten in entsprechende virtuelle Tabellen. Enthält eine Tabelle Spalten Auflistung wird der Treiber, virtuellen Tabellen generiert:

-   Eine **Basistabelle**, die die gleichen Daten wie die tatsächliche Tabelle außer der Auflistung Spalten enthält. Die Basistabelle wird derselbe Name wie der eigentlichen Tabelle darstellt.
-   Eine **virtuelle Tabelle** für jede Spalte Auflistung geschachtelten Daten erweitert. Virtuellen Tabellen, die Sammlungen darstellen, werden Namen den Namen der eigentlichen Tabelle, Trennzeichen "_vt_" und den Namen der Spalte.

Virtuelle Tabellen beziehen sich auf die Daten in der eigentlichen Tabelle Aktivieren des Treibers auf die denormalisierten Daten zugreifen. Siehe Abschnitt "Beispiel" Details. Sie können Inhalt Cassandra Sammlungen Abfragen und virtuellen Tabellen zugreifen.

Sie nutzen den [Assistenten zum Kopieren von](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuitiv die Liste der Tabellen in Cassandra Datenbank virtuellen Tabellen anzeigen und eine Vorschau der Daten in. Sie erstellen eine Abfrage in der Kopie und das Ergebnis zu überprüfen.

### <a name="example"></a>Beispiel
Die folgenden "ExampleTable" ist z. B. eine Datenbanktabelle Cassandra, die eine Ganzzahl-Primärschlüsselspalte namens "Pk_int", Textspalte Wert einer Listenspalte, eine Map-Spalte und eine Spalte (mit dem Namen "StringSet") enthält.

pk_int | Wert | Liste | Karte |   StringSet
------ | ----- | ---- | --- | --------
1 | "Beispielwert 1" | ["1", "2", "3"]  | {"S1": "a", "S2": "b"} | {"A", "B", "C"}
3 | "Beispielwert 3" | ["100", "101", "102", "105"] | {"S1": "t"} | {"A", "E"}

Der Treiber erzeugt mehrere virtuelle Tabellen zu dieser Tabelle darstellen. Fremdschlüsselspalten in virtuellen Tabellen die Primärschlüsselspalten in der eigentlichen Tabelle verweisen und zeigen die realen Tabellenzeile virtuelle Tabellenzeile entspricht. 

Erste virtuelle Tabelle ist die Basistabelle mit dem Namen "ExampleTable" in der folgenden Tabelle gezeigt. Die Basistabelle enthält die gleichen Daten wie der ursprüngliche Datenbanktabelle mit Ausnahme der Sammlungen in dieser Tabelle ausgelassen und in anderen virtuellen Tabellen erweitert.

pk_int | Wert
------ | -----
1 | "Beispielwert 1"
3 | "Beispielwert 3"

Die folgenden Tabellen zeigen die virtuellen Tabellen, die Daten aus den Spalten Liste, Zuordnung und StringSet renormalize. Spalten mit Namen, die mit "_index" oder "Key" geben die Position der Daten in der ursprünglichen Liste oder Karte. Spalten mit Namen, die mit "_value" enthalten zusätzliche Daten aus der Auflistung.

#### <a name="table-exampletablevtlist"></a>Tabelle "ExampleTable_vt_List":
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>Tabelle "ExampleTable_vt_Map":
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | EIN
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>Tabelle "ExampleTable_vt_StringSet":
pk_int | StringSet_value
------ | ---------------
1 | EIN
1 | B
1 | C
3 | EIN
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.
