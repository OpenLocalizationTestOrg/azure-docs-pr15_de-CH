<properties 
    pageTitle="Verschieben von Daten von Sybase | Azure Data Factory" 
    description="Enthält Informationen zum Verschieben von Daten von Sybase-Datenbank mithilfe von Azure Data Factory." 
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

# <a name="move-data-from-sybase-using-azure-data-factory"></a>Verschieben von Daten von Azure Data Factory mit Sybase 

Dieser Artikel beschreibt die kopieren-Aktivität in einer Azure Daten Verwendung zum Verschieben von Daten von Sybase zu einem anderen. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und unterstützten Speicher-Kombinationen.

Data Factory unterstützt lokale Sybase Quellen Data Management Gateway herstellen. [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Siehe über Data Management Gateway und Anleitung zum Einrichten von Gateway. 

> [AZURE.NOTE]
> Gateway ist auch die Sybase-Datenbank eine Neuerung Azure gehostet wird. Sie können das Gateway auf derselben IaaS VM als Datenspeicher oder einer anderen VM als Gateway mit der Datenbank herstellen kann. 

Data Factory unterstützt derzeit nur Daten von Sybase zu anderen Datenspeichern und nicht von anderen Datenspeichern, Sybase.

## <a name="installation"></a>Installation

Data Management Gateway die Sybase-Datenbank herstellen müssen Sie den [Datenanbieter für Sybase](http://go.microsoft.com/fwlink/?linkid=324846) auf demselben System wie Data Management Gateway installieren.

> [AZURE.NOTE] Tipps zur Behebung von Verbindungs-Gateways Siehe [Problembehandlung Gateway stellt](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Fragen. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline erstellen, die Daten aller unterstützten Senke Datenspeicher aus eine Sybase-Datenbank kopiert wird mithilfe des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Das folgende Beispiel enthält Beispiel JSON Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Sie zeigen, wie Daten von Sybase-Datenbank in Azure BLOB-Speicher kopieren. Allerdings können Daten auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) mithilfe der Kopieraktivität in Azure Data Factory kopiert werden.   

## <a name="sample-copy-data-from-sybase-to-azure-blob"></a>Beispiel: Daten aus Sybase in Azure Blob
Dieses Beispiel veranschaulicht, wie Daten aus einer Datenbank in Sybase in Azure BLOB-Speicher kopieren. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties).
2.  Ein gefallen Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties).
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Die [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Das Beispiel Kopiervorgang aus einem Abfrageergebnis in Sybase-Datenbank in ein Blob stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 

Als ersten Schritt setup Data Management Gateway. Die Schritte sind im [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) Artikel.

**Sybase verknüpft Service:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
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


**Sybase Eingabedatasets:**

Das Beispiel wird vorausgesetzt, Sie haben eine Tabelle "MyTable" in Sybase und eine Spalte "Timestamp" für Serie Daten enthält.

Einstellung "extern": True teilt Daten Factorydienst Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt. Beachten Sie, dass der **Typ** der verknüpften Serviceartikel an: **RelationalTable**. 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
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
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **RelationalSource** und **BlobSink** **Senke** Typ festgelegt ist. Für die **Abfrage** -Eigenschaft angegebene SQL-Abfrage wählt die Daten aus der DBA. Orders-Tabelle in der Datenbank.


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
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
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="sybase-linked-service-properties"></a>Eigenschaften von Sybase verknüpft

Die folgende Tabelle beschreibt für JSON-Elemente für Sybase verknüpft Service.

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Type-Eigenschaft muss auf festgelegt sein: **OnPremisesSybase** | Ja
Server | Name der Sybase-Server. | Ja
Datenbank | Name der Sybase-Datenbank. | Ja 
Schema  | Name des Schemas in der Datenbank. | Nein
Read | Art der Authentifizierung zur Verbindung mit Sybase-Datenbank verwendet. Mögliche Werte sind: anonym, Basic und Windows. | Ja
Benutzername | Geben Sie Benutzernamen an, wenn Sie Standardauthentifizierung oder Windows-Authentifizierung verwenden. | Nein
Kennwort | Geben Sie Kennwort für das Benutzerkonto für den Benutzernamen angegeben. |  Nein
gatewayName | Name des Gateways Daten Factorydienst Verbindung zu lokalen Sybase-Datenbank verwenden soll. | Ja 

Informationen zum Festlegen von Anmeldeinformationen für eine lokale Sybase-Datenquelle finden Sie unter [festlegen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="sybase-dataset-type-properties"></a>Sybase Dataset Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Azure SQL, Azure Blob Azure Tabelle usw..).

Die TypeProperties unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. **TypeProperties** Abschnitt Dataset vom Typ **RelationalTable** (einschließlich Sybase Dataset) hat die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Tabellenname | Name der Tabelle in der verknüpften Serviceartikel auf Sybase-Datenbank-Instanz. | Nein (bei **Abfrage** des **RelationalSource** )

## <a name="sybase-copy-activity-type-properties"></a>Sybase kopieren Typeneigenschaften 

Für eine vollständige Liste der Abschnitte und verfügbaren Aktivitäten [Erstellen Sie Rohrleitungen](data-factory-create-pipelines.md) Artikel definieren. Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt TypeProperties der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

Wenn Quelle des Typs **RelationalSource** (einschließlich Sybase) stehen die folgenden Eigenschaften im **TypeProperties** -Abschnitt:

Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
Abfrage | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. | Nein ( **TableName** **Dataset** angegeben wird).

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-sybase"></a>Zuordnung für Sybase

Wie im Artikel [Daten Bewegung](data-factory-data-movement-activities.md) Ausführende der Kopie automatische Konvertierung von Quelltypen zum Auffangen von Typen mit dem folgenden Schritt 2:

1. Konvertieren von Typen von systemeigenen .NET Typ
2. Konvertieren von .NET Typ native Senke Typ

Sybase unterstützt T-SQL und T-SQL-Typen. Zuordnung von Sql-Typen in .NET Typ, finden Sie unter [Azure SQL Connector](data-factory-azure-sql-connector.md) Artikel.

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.