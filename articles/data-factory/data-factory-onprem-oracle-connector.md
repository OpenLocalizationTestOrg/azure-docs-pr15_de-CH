<properties 
    pageTitle="Verschieben von Daten und Oracle mit Data Factory | Microsoft Azure" 
    description="Informationen Sie zum Verschieben der Daten nach Oracle Datenbank-lokal mit Azure Data Factory." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Verschieben von Daten in lokalen Oracle mit Azure Data Factory 

Dieser Artikel beschreibt die Verwendung Data Factory kopieraktivität zum Verschieben von Daten nach Oracle aus dem und in den anderen Daten gespeichert. Dieser Artikel baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel stellt eine allgemeine Übersicht über Daten kopieren und unterstützten Speicher-Kombinationen.

## <a name="installation"></a>Installation 
Für den Dienst Azure Data Factory zum lokalen Oracle-Datenbank herstellen können müssen Sie die folgenden Komponenten installieren: 

- Data Management Gateway auf dem Computer, der die Datenbank hostet oder auf einem separaten Computer zu Wettbewerb um Ressourcen mit der Datenbank. Data Management Gateway ist ein Client, die sicheren und verwalteten lokalen Datenquellen mit Cloud-Diensten verbindet. Siehe [Verschieben von Daten zwischen lokalen und](data-factory-move-data-between-onprem-and-cloud.md) Artikel über Data Management Gateway. 
- Oracle-Datenanbieter für .NET. Diese Komponente ist in [Oracle Data Access Komponenten für Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/)enthalten. Installieren Sie die entsprechende Version (32/64-Bit) auf dem Host, auf das Gateway installiert ist. [Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) möglich Oracle Database 10 g Version 2 oder höher.

    Wählen Sie "XCopy Installation" Schritte in der Datei readme.htm. Wir empfehlen Sie Benutzeroberfläche (nicht-XCopy eine) auswählen. 
 
    Starten Sie nach der Installation des Anbieters Daten Management Gateway-Server-Dienst auf dem Computer mit Dienste-Applet (oder) Data Management Gateway-Konfigurations-Manager.  

> [AZURE.NOTE] Tipps zur Behebung von Verbindungs-Gateways Siehe [Problembehandlung Gateway stellt](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Fragen. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline erstellen, die Daten aus und in einer Oracle-Datenbank auf einen Datenspeicher unterstützten Senke kopiert ist die Verwendung des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Das folgende Beispiel enthält Beispiel JSON Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Sie zeigen Daten aus und in einer Oracle-Datenbank in Azure BLOB-Speicher kopieren. Allerdings können Daten auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) mithilfe der Kopieraktivität in Azure Data Factory kopiert werden.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Beispiel: Daten aus Oracle in Azure Blob
Dieses Beispiel veranschaulicht, wie Daten aus einer Oracle-Datenbank für lokale in Azure BLOB-Speicher kopieren. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und als Senke verwendet.

Das Beispiel kopiert Daten aus einer Tabelle in einer Oracle-Datenbank lokal in ein Blob stündlich. Weitere Informationen zu Eigenschaften, die im Beispiel verwendeten Dokumentation in Abschnitten folgende Beispiele.

**Verknüpfte Oracle-Dienst:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure BLOB-Speicher verknüpft Service:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Oracle Eingabedatasets:**

Das Beispiel wird vorausgesetzt, Sie haben eine Tabelle "MyTable" in Oracle und eine Spalte namens "Timestampcolumn" für die Serie Daten enthält. 

Einstellung "extern": "true" informiert Data Factory-Dienst, dass das Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad und Namen des für den Blob werden dynamisch anhand der Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet Jahr, Monat, Tag und Teile Stunden von der Startzeit entfernt.
    
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


**Pipeline mit kopieren:**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **OracleSource** und **BlobSink** **Senke** Typ festgelegt ist.  **OracleReaderQuery** -Eigenschaft angegebene SQL-Abfrage wählt die Daten in der letzten Stunde kopieren.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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


Sie müssen die Abfragezeichenfolge anhand der Konfiguration der Daten in der Oracle-Datenbank anpassen. Wenn die folgende Fehlermeldung angezeigt: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

Möglicherweise müssen die Abfrage ändern, wie im folgenden Beispiel (To_date-Funktion):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Beispiel: Daten von Azure Blob zu Oracle
Dieses Beispiel zeigt, wie auf eine Oracle-Datenbank für lokale Daten von Azure BLOB-Speicher kopiert. Allerdings können kopierten **direkt** aus den Quellen angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) als [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als Senke Quelle verwendet.

Im Beispiel kopiert Daten von einem Blob in eine Tabelle in einer Oracle-Datenbank für lokale stündlich. Weitere Informationen zu Eigenschaften, die im Beispiel verwendeten Dokumentation in Abschnitten folgende Beispiele.

**Verknüpfte Oracle-Dienst:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure BLOB-Speicher verknüpft Service:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure Blob Eingabedatasets**

Daten werden abgeholt ein neues Blob stündlich (Häufigkeit: Stunde, Intervall: 1). Pfad und Namen des für den Blob werden dynamisch anhand der Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet, Jahr, Monat und Tagesanteil der Startzeit und Dateinamen verwendet Stundenteil der Startzeit. "externe": "true" Einstellung informiert Service Data Factory, diese Tabelle Data Factory ist nicht durch eine Aktivität im Werk Daten erzeugt.

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

**Oracle-Ausgabe-Dataset:**

Im Beispiel wird davon ausgegangen, dass Sie in Oracle eine Tabelle "MyTable" erstellt haben. Erstellen Sie die Tabelle in Oracle mit der gleichen Anzahl von Spalten wie BLOB-CSV-Datei gewünscht enthalten. Neue Zeilen werden stündlich zur Tabelle hinzugefügt.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Pipeline mit kopieren:**

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In der Pipeline JSON-Definition **Geben** auf **BlobSource** festgelegt ist und welche **Senke** auf **OracleSink**festgelegt ist.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
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


## <a name="oracle-linked-service-properties"></a>Eigenschaften von Oracle verknüpft

Die folgende Tabelle beschreibt für JSON-Elemente für Oracle verknüpft Service. 

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Type-Eigenschaft muss auf festgelegt sein: **OnPremisesOracle** | Ja
connectionString | Geben Sie Informationen für die Verbindung zu Oracle-Datenbankinstanz für die ConnectionString-Eigenschaft. | Ja 
gatewayName | Name des Gateways, zum Verbinden mit lokalen Oracle Server, | Ja

Informationen zum Festlegen von Anmeldeinformationen für eine lokale Oracle-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .
## <a name="oracle-dataset-type-properties"></a>Oracle Dataset Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen (Oracle, Azure Blob Azure Tabelle usw.).
 
Die TypeProperties unterscheidet sich für jeden Datensatz und enthält Informationen über den Speicherort der Daten in den Datenspeicher. Typ OracleTable Abschnitt TypeProperties für das Dataset hat die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Tabellenname | Name der Tabelle in der Oracle-Datenbank verknüpfte Dienst bezieht. | Nein ( **OracleReaderQuery** **OracleSource** angegeben wird).

## <a name="oracle-copy-activity-type-properties"></a>Oracle Kopie Typeneigenschaften

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

> [AZURE.NOTE]
Die Kopie wird nur eine Eingabe und nur eine Ausgabe.

Eigenschaften im Abschnitt TypeProperties der Aktivität je nach andererseits jeden Aktivitätstyp. Kopie Aktivität variieren abhängig von den Datenquellen und Datensenken.

### <a name="oraclesource"></a>OracleSource
Kopieren-Aktivität wird die Quelle des Typs **OracleSource** stehen die folgenden Eigenschaften im **TypeProperties** -Abschnitt:

Eigenschaft | Beschreibung |Zulässige Werte | Erforderlich
-------- | ----------- | ------------- | --------
oracleReaderQuery | Verwenden Sie die benutzerdefinierte Abfrage Daten lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie *from MyTable <br/> <br/>nicht angegeben, die SQL-Anweisung, die ausgeführt wird: Wählen* from MyTable | Nein ( **TableName** **Dataset** angegeben wird).

### <a name="oraclesink"></a>OracleSink
**OracleSink** unterstützt die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
writeBatchTimeout | Wartezeit für den Batch Einfügevorgang abgeschlossen, bevor das Zeitlimit überschritten. | TimeSpan<br/><br/> Beispiel: 00:30:00 (30 Minuten). | Nein
writeBatchSize | Fügt Daten in die SQL-Tabelle die Puffergröße WriteBatchSize erreicht.   | Ganzzahl (Zeilenanzahl)| Keine (Standardwert: 10000)  
sqlWriterCleanupScript | Geben Sie eine Abfrage kopieren Aktivität auszuführen, Daten in einem bestimmten Segment bereinigt werden. | Eine Anweisung. | Nein
sliceIdentifierColumnName | Geben Sie Spaltennamen für Kopie mit automatisch generierten Segment ID Füllen mit Daten von einem bestimmten Segment erneut beim Bereinigen. | Spaltenname einer Spalte mit Daten binary(32). | Nein


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Zuordnung für Oracle

Wie in Ausführende der [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel kopieren automatische Konvertierung von Quelltypen zum Auffangen von Typen mit dem folgenden Schritt 2:

1. Konvertieren von Typen von systemeigenen .NET Typ
2. Konvertieren von .NET Typ native Senke Typ

Beim Verschieben von Daten von Oracle werden die folgenden Zuordnungen von Oracle-Datentyp und .NET Typ verwendet.

Oracle-Datentyp | .NET Framework-Datentyp
---------------- | ------------------------
BFILE | Byte]
BLOB | Byte]
CHAR | Zeichenfolge
CLOB | Zeichenfolge
DATUM | DateTime
FLOAT | Dezimal
GANZE ZAHL | Dezimal
INTERVALL JAHR MONAT | Int32
INTERVALL TAG ZWEITEN | TimeSpan
LANGE | Zeichenfolge
LONG RAW | Byte]
NCHAR | Zeichenfolge
NCLOB | Zeichenfolge
ANZAHL | Dezimal
NVARCHAR2 | Zeichenfolge
RAW | Byte]
ROWID | Zeichenfolge
ZEITSTEMPEL | DateTime
TIMESTAMP MIT ZEITZONE | DateTime
TIMESTAMP MIT ZEITZONE | DateTime
GANZE ZAHL OHNE VORZEICHEN | Anzahl
VARCHAR2 | Zeichenfolge
XML | Zeichenfolge

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

**Problem:** Die folgende **Fehlermeldung**angezeigt: kopieraktivität erfüllt ungültige Parameter: UnknownParameterName, detaillierte Meldung: nicht angeforderte .net gefunden Datenanbieter. Es kann nicht installiert werden".  

**Mögliche Ursachen:**

1. Der.NET Framework-Datenanbieter für Oracle wurde nicht installiert.
2. Der.NET Framework-Datenanbieter für Oracle in.NET Framework 2.0 installiert wurde und in.NET Framework 4.0-Ordner nicht gefunden. 

**Auflösung/Lösung:**

1. Nicht installiert .NET Datenanbieter für Oracle, [Installieren](http://www.oracle.com/technetwork/topics/dotnet/downloads/) und das Szenario erneut. 
2. Wenn Sie auch nach der Installation des Anbieters die Fehlermeldung erhalten, führen Sie die folgenden Schritte aus: 
    1. Öffnen Sie Ordner Machine Config der .NET 2.0: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Suche für **Oracle-Datenanbieter für .NET**und einen Eintrag finden, wie im folgenden Beispiel Unwn im folgenden gezeigt werden soll Beispiel aufhebender **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Kopieren dieser Eintrag in der Datei machine.config Ordners v4. 0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, und ändern Sie die Version 4.xxx.x.x.
3.  Installieren Sie "< (ODP.NET installierte Pfad > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" dem globalen Assemblycache (GAC) mit `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.
