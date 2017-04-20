<properties 
    pageTitle="Verschieben von Daten vom FTP-Server | Microsoft Azure" 
    description="Enthält Informationen zum Verschieben von Daten von einem ftpserver mithilfe von Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Verschieben von Daten von einem ftpserver mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie die Kopie in Azure Data Factory Verwendung zum Verschieben von Daten von einem FTP-Server mit einem unterstützten Senke Datenspeicher. Dieser Artikel baut auf Artikel [Datenaktivitäten](data-factory-data-movement-activities.md) , der einen allgemeinen Überblick über die Daten kopieren und die Liste der Datenspeicher als Quellen-Ereignissenken bietet. 

Data Factory unterstützt derzeit nur Daten von einem FTP-Server in anderen Datenspeichern, jedoch nicht zum Verschieben von Daten aus anderen Datenspeichern einen FTP-Server. Unterstützt sowohl lokal und FTP-Servern. 

Wenn Sie Daten von einem **lokalen** FTP-Server in einem Datenspeicher Cloud verschieben (Beispiel: Azure BLOB-Speicher) installiert und Data Management Gateway. Data Management Gateway ist ein Client, der auf dem lokalen Computer installiert, die Cloud-Services vor Ort herstellen können. Einzelheiten Sie [Data Management Gateway](data-factory-data-management-gateway.md) auf dem Gateway. Siehe [Verschieben von Daten zwischen lokalen Speicherorten und](data-factory-move-data-between-onprem-and-cloud.md) ausführliche Anleitung zum Gateway einrichten und verwenden. Sie verwenden das Gateway auf einem FTP-Server herstellen, auch wenn der Server auf eine IaaS Azure Virtual Machine (VM). 

Sie können das Gateway auf demselben lokalen Computer oder Neuerung Azure als FTP-Server. Allerdings empfiehlt das Gateway auf einem Computer oder einem separaten Azure Neuerung um Ressourcenkonflikte zu vermeiden und die Leistung installieren. Wenn Sie das Gateway auf einem separaten Computer installieren, sollte der Computer auf den FTP-Server zugreifen. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Am einfachsten eine Pipeline erstellen, die Daten von einem FTP-Server kopiert wird mithilfe des Assistenten zum Kopieren von Daten. Siehe [Tutorial: eine Rohrleitung mit Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Die folgenden Beispiele bieten Beispiel JSON-Definitionen, mit denen Sie eine Rohrleitung mit [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Beispiel: Daten Sie vom FTP-Server in Azure blob

Dieses Beispiel veranschaulicht, wie Daten aus einem FTP-Server in Azure BLOB-Speicher kopieren. Allerdings können kopierten **direkt** auf der Ereignissenken angegebenen [hier](data-factory-data-movement-activities.md#supported-data-stores) Kopieraktivität in Azure Data Factory verwenden.  
 
Das Beispiel hat Daten Factory Entitäten:

- Eine verknüpfte Dienst des Typs [FTP-Server](#ftp-linked-service-properties).
- Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Ein Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [Dateifreigabe](#fileshare-dataset-type-properties).
- Ein Ausgabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Eine [Pipeline](data-factory-create-pipelines.md) mit kopieren, die [FileSystemSource](#ftp-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Das Beispiel Kopiervorgang von einem FTP-Server in Azure Blob stündlich. In diesen Beispielen verwendeten JSON-Eigenschaften werden in Abschnitten nach den Beispielen beschrieben. 

**FTP Dienst verknüpft** Dieses Beispiel verwendet die Standardauthentifizierung mit Benutzername und Kennwort in Klartext. Sie können auch eine der folgenden Methoden verwenden: 

- Anonyme Authentifizierung 
- Standardauthentifizierung mit verschlüsselten Anmeldeinformationen
- FTP über SSL/TLS (FTP)

Siehe Abschnitt [FTP verknüpfte Dienst](#ftp-linked-service-properties) für verschiedene Arten der Authentifizierung können. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
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

**FTP-Eingabedatasets** Dieser Datensatz bezieht sich auf FTP-Ordner `mysharedfolder` und `test.csv`. Die Pipeline kopiert die Datei in das Ziel. 

Einstellung "extern": "true" informiert Data Factory-Dienst, dass das Dataset Data Factory ist und nicht durch eine Aktivität im Werk Daten erzeugt.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Azure Blob Ausgabe dataset**

Jede Stunde Daten in ein neues Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch anhand die Startzeit der Schicht, die verarbeitet wird. Der Ordnerpfad verwendet Jahr, Monat, Tag und Teile Stunden von der Startzeit entfernt.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Die Pipeline enthält eine Aktivität kopieren, konfiguriert die Eingabe- und Datasets verwenden und stündlich ausgeführt werden soll. In JSON-Definition Rohrleitung soll **des Typs** **FileSystemSource** und **BlobSink** **Senke** Typ festgelegt ist. 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
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
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>Verknüpfte FTP-Eigenschaften

Die folgende Tabelle beschreibt für JSON-Elemente für die verknüpften Serviceartikel FTP.

| Eigenschaft | Beschreibung | Erforderlich | Standard |
| -------- | ----------- | -------- | ------- | 
| Typ | Die Type-Eigenschaft muss auf eine festgelegt werden | Ja | &nbsp;
| Host | Name oder IP-Adresse des FTP-Servers | Ja | &nbsp;
| Read | Authentifizierungstyp angeben | Ja | Standardauthentifizierung, die anonyme |
| Benutzername | Benutzer mit Zugriff auf den FTP-server | Nein | &nbsp;
| Kennwort | Kennwort des Benutzers (Benutzername) | Nein | &nbsp;
| encryptedCredential | Verschlüsselte Anmeldeinformationen Zugriff auf den FTP-server | Nein | &nbsp;
| gatewayName | Name des Daten-Management Gateway Gateway zum lokalen FTP-Server herstellen | Nein    | &nbsp;
| Anschluss | Auf dem FTP-Server überwachten Port | Nein | 21 |
| enableSsl | Gibt an, ob die Verwendung von FTP über SSL/TLS-Kanal | Nein | true | 
| enableServerCertificateValidation | Geben Sie an, ob SSL-Überprüfung des Serverzertifikats zu aktivieren, wenn FTP über SSL/TLS-Kanal | Nein | true | 

### <a name="using-anonymous-authentication"></a>Anonyme Authentifizierung

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Benutzernamen und Kennwörter verwenden in Klartext Standardauthentifizierung
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Verwendung von Port, EnableSsl, enableServerCertificateValidation

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Verwenden von EncryptedCredential für Authentifizierung und gateway

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Informationen zum Festlegen von Anmeldeinformationen für einen lokalen FTP-Datenquelle finden Sie unter [festlegen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>FTP-Kopie Typeneigenschaften

Eine vollständige Liste der Eigenschaften für Aktivitäten definieren & Abschnitte finden Sie [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien sind für alle Aktivitäten verfügbar. 

Eigenschaften im Abschnitt TypeProperties der Aktivität je nach andererseits jeden Aktivitätstyp. Arten von Datenquellen und Datensenken Kopie Aktivität abhängig die Eigenschaften.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung  
Siehe [Aktivität Leistung & Tuning Guide](data-factory-copy-activity-performance.md) Kennenlernen Schlüsselfaktoren, Auswirkung Leistung des Datentransfers (Kopieraktivität) in Azure Data Factory und verschiedene Methoden zum optimieren.

## <a name="next-steps"></a>Nächste Schritte
Finden Sie in folgenden Artikeln: 

- [Kopieraktivität Lernprogramm](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für eine schrittweise Anleitung zum Erstellen einer Pipeline mit einer Kopie. 
