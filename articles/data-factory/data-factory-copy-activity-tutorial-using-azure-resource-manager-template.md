<properties
    pageTitle="Lernprogramm: Erstellen eine Pipeline mit Ressourcen-Manager-Vorlage | Microsoft Azure"
    description="In diesem Lernprogramm erstellen Sie eine Azure Data Factory-Pipeline mit einer Kopie mit Azure-Ressourcen-Manager-Vorlage."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Lernprogramm: Erstellen einer Pipeline mit mit Azure Ressourcenmanager Vorlage kopieren
> [AZURE.SELECTOR]
- [Übersicht und Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure-Ressourcen-Manager-Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In diesem Lernprogramm wird das Erstellen und Überwachen einer Azure Data Factory mithilfe einer Vorlage Azure-Ressourcen-Manager veranschaulicht. Die Pipeline im Werk Daten kopiert Daten von Azure BLOB-Speicher in Azure SQL-Datenbank.

## <a name="prerequisites"></a>Erforderliche Komponenten
- [Lernprogramm Übersicht und Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) durchlaufen und die **erforderliche** Schritte.
- Anleitung [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Artikel Version von Azure PowerShell auf Ihrem Computer installieren. In diesem Lernprogramm verwenden Sie PowerShell Data Factory Entitäten bereitstellen. 
- (optional) [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md) Azure Resource Manager Vorlagen Informationen angezeigt.


## <a name="in-this-tutorial"></a>In diesem Lernprogramm

In diesem Lernprogramm erstellen Sie eine Factory Daten mit den folgenden Daten Factory:

Entität | Beschreibung  
------ | ----------- 
Azure verknüpft Speicherdienst | Mit der Data Factory verknüpft Azure Storage-Konto. Azure-Speicher Datenspeicher Quelle und SQL Azure-Datenbank Senke Datenspeicher für die kopieraktivität des Lernprogramms. Es gibt das Speicherkonto, das Eingabedaten für die kopieraktivität enthält. 
Azure SQL-Datenbank verknüpft service| Die SQL Azure-Datenbank verknüpft mit Daten Factory. SQL Azure-Datenbank, die Ausgabedaten für die kopieraktivität gespeichert, wird. 
Azure Blob Eingabedatasets | Bezeichnet den Dienst Azure Storage verknüpft. Der verknüpfte Dienst auf Azure Storage-Konto und Azure BLOB-Dataset gibt Container, Ordner und Dateinamen im Speicher, der Eingabedaten enthält. 
Azure SQL-Ausgabe-dataset | Bezeichnet den Dienst Azure SQL verknüpft. Verknüpfte Azure SQL-Dienst auf einem SQL Azure-Server und SQL Azure Dataset gibt den Namen der Tabelle, die die Ausgabedaten enthält. 
Datenpipeline | Die Pipeline hat eine Aktivität vom Typ kopieren, dem Dataset Azure Blob als Eingabe und SQL Azure Dataset als Ausgabe. Die kopieraktivität kopiert Daten aus einem Azure Blob zu einer Tabelle in SQL Azure-Datenbank.  

Eine Factory Daten können eine oder mehrere Rohrleitungen. Eine Pipeline kann eine oder mehrere Aktivitäten enthalten. Es gibt zwei Arten von Aktivitäten: [Transformation](data-factory-data-transformation-activities.md) [Daten bewegungsaktivitäten](data-factory-data-movement-activities.md) und Daten. In diesem Lernprogramm erstellen Sie eine Rohrleitung mit einer Aktivität (Copy-Aktivität).

![Kopieren von Azure Blob in Azure SQL-Datenbank](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

Der folgende Abschnitt enthält die vollständige Ressourcenmanager Vorlage Data Factory Entitäten, damit Sie schnell das Lernprogramm ausführen und Testen Sie die Vorlage definieren. Zu verstehen, wie jede Entität Data Factory definiert, siehe [Data Factory Einheiten in der Vorlage](#data-factory-entities-in-the-template) .

## <a name="data-factory-json-template"></a>Factory JSON-Vorlage
Auf der obersten Ebene Ressourcenmanager Vorlage für Data Factory definiert ist: 

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { ...
        },
        "variables": { ...
        },
        "resources": [
            {
                "name": "[parameters('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Erstellen einer JSON-Datei namens **ADFCopyTutorialARM.json** im **C:\ADFGetStarted** -Ordner mit folgendem Inhalt:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
        },
        "resources": [
          {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
              {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureStorage",
                  "description": "Azure Storage linked service",
                  "typeProperties": {
                    "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                  }
                }
              },
              {
                "type": "linkedservices",
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
                  }
                }
              },
              {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  }
                }
              },
              {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]",
                  "[variables('azureSqlLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('sqlOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "activities": [
                    {
                      "name": "CopyFromAzureBlobToAzureSQL",
                      "description": "Copy data frm Azure blob to Azure SQL",
                      "type": "Copy",
                      "inputs": [
                        {
                          "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                          "name": "[variables('sqlOutputDatasetName')]"
                        }
                      ],
                      "typeProperties": {
                        "source": {
                          "type": "BlobSource"
                        },
                        "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                        },
                        "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                        }
                      },
                      "Policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 3,
                        "timeout": "01:00:00"
                      }
                    }
                  ],
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>Parameter JSON 
Erstellen einer JSON-Datei mit dem Namen **ADFCopyTutorialARM-Parameters.json** , die Parameter der Azure-Ressourcen-Manager-Vorlage enthält. 

> [AZURE.IMPORTANT] Geben Sie den Namen und den Schlüssel des Kontos Azure-Speicher für **StorageAccountName** und **StorageAccountKey** .  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] Möglicherweise separate JSON Parameterdateien für Entwicklung, Test und produktionsumgebungen, die mit der gleichen Data Factory JSON-Vorlage verwendet. Mithilfe eines PowerShell-Skripts können Sie die Bereitstellung von Data Factory Entitäten in dieser Umgebung automatisieren.  

## <a name="create-data-factory"></a>Data Factory erstellen
1. Starten Sie **Azure PowerShell** , und führen Sie den folgenden Befehl:
    - Führen Sie `Login-AzureRmAccount` , und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei Azure-Portal.  
    - Ausführen `Get-AzureRmSubscription` alle Abonnements für dieses Konto an.
    - Ausführen `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` Abonnement wählen Sie arbeiten. 
2. Führen Sie den folgenden Befehl Data Factory Entitäten mit der Ressourcen-Manager-Vorlage bereitstellen, die Sie in Schritt 1 erstellt haben.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor-pipeline
1. Melden Sie sich mit Ihrem Azure-Konto [Azure-Portal](https://portal.azure.com) .
2. Klicken Sie im linken Menü auf **Daten Fabriken** () oder klicken Sie auf **Weitere Dienste** **Daten Factorys** **Intelligenz + ANALYTICS** Kategorie.

    ![Werke im Menü](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Suchen Sie auf der Seite **Data Factorys** und finden Sie Ihrer Data Factory. 

    ![Suchen Sie nach "Data Factory"](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Klicken Sie auf Ihre Azure Data Factory. Die Homepage für die Factory Daten angezeigt.

    ![Homepage "Data Factory"](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Klicken Sie auf **Diagramm** nebeneinander um Data Factory Diagramm anzuzeigen.

    ![Diagrammansicht Data Factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. Doppelklicken Sie auf das Dataset **SQLOutputDataset**in der Diagrammansicht. Dieser Status des Segments angezeigt. Wenn der Kopiervorgang abgeschlossen ist, legen Sie den Status **bereit**.

    ![Ausgabe-Segment bereit](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Überprüfen der **bereit** ist, dass die Daten der Tabelle **emp** in Azure SQL-Datenbank.

Finden Sie [Monitor Datasets und Pipeline](data-factory-monitor-manage-pipelines.md) Anleitung mit Azure Portal Blades Überwachen der Pipeline und des Datasets in diesem Lernprogramm erstellt haben.

Überwachen und Verwalten von App können auch Ihre Datenpipelines überwachen. Finden Sie [Überwachen und Verwalten von Azure Data Factory Rohrleitungen App überwachen](data-factory-monitor-manage-app.md) ausführliche Informationen zur Verwendung der Anwendungdes.


## <a name="data-factory-entities-in-the-template"></a>Data Factory Einheiten in der Vorlage

### <a name="define-data-factory"></a>Data Factory definieren
Sie definieren eine Factory Daten in der Ressourcen-Manager-Vorlage wie im folgenden Beispiel gezeigt:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

Der DataFactoryName ist wie folgt definiert: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

Ist eine eindeutige Zeichenfolge basierend auf die Kennung der Ressource.  

### <a name="defining-data-factory-entities"></a>Data Factory Entitäten definieren
Die folgenden Daten Factory Entitäten sind in JSON-Vorlage definiert: 

1. [Azure verknüpft Speicherdienst](#azure-storage-linked-service)
2. [Azure verknüpften SQL-Dienst](#azure-sql-database-linked-service)
3. [Azure Blob dataset](#azure-blob-dataset)
4. [Azure SQL-dataset](#azure-sql-dataset)
5. [Datenpipeline mit einer Kopie](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure verknüpft Speicherdienst
In diesem Abschnitt geben den Namen und Schlüssel Ihres Kontos Azure-Speicher. Details über JSON-Eigenschaften zum Definieren einer verknüpften Azure Storage-Diensts finden Sie unter [Azure Storage Service verknüpft](data-factory-azure-blob-connector.md#azure-storage-linked-service) . 

    {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureStorage",
            "description": "Azure Storage linked service",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
            }
        }
    }

ConnectionString verwendet die Parameter StorageAccountName und StorageAccountKey. Die Werte für diese Parameter mithilfe einer Konfigurationsdatei. Die Definition wird auch Variablen: AzureStroageLinkedService und DataFactoryName in der Vorlage definiert. 
    
#### <a name="azure-sql-database-linked-service"></a>Azure SQL-Datenbank verknüpft service
In diesem Abschnitt werden Azure SQL-Servername, Datenbankname, Benutzername und Kennwort angeben. Informationen zum JSON-Eigenschaften zum Definieren eines Azure SQL verknüpft Service finden Sie in der [SQL Azure Service verknüpft](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

ConnectionString verwendet SqlServerName, Datenbankname, SqlServerUserName und SqlServerPassword-Parameter, deren Werte mithilfe einer Konfigurationsdatei übergeben werden. Die Definition verwendet auch die folgenden Variablen aus der Vorlage: AzureSqlLinkedServiceName, DataFactoryName.

#### <a name="azure-blob-dataset"></a>Azure Blob dataset
Sie geben die Namen der BLOB-Container, Ordner und Datei, die Eingabedaten enthält. Details über ein Dataset Azure Blob definiert JSON-Eigenschaften finden Sie unter [Azure Blob Dataset-Eigenschaften](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) . 


    {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Azure SQL-dataset
Sie geben den Namen der Tabelle in SQL Azure-Datenbank, die mit den kopierten von Azure BLOB-Speicher Daten. Informationen zum JSON-Eigenschaften zum Definieren einer Azure SQL-Dataset finden Sie in [SQL Azure Dataset-Eigenschaften](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) . 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Datenpipeline
Definieren Sie eine Pipeline, die Daten aus dem Dataset Azure Blob in SQL Azure Dataset kopiert. Eine Beschreibung der JSON-Elemente zum Definieren einer Pipeline in diesem Beispiel finden Sie unter [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) . 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureStorageLinkedServiceName')]",
            "[variables('azureSqlLinkedServiceName')]",
            "[variables('blobInputDatasetName')]",
            "[variables('sqlOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "activities": [
            {
                "name": "CopyFromAzureBlobToAzureSQL",
                "description": "Copy data frm Azure blob to Azure SQL",
                "type": "Copy",
                "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
                ],
                "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "SqlSink",
                        "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                        "type": "TabularTranslator",
                        "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                },
                "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                }
            }
            ],
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>Die Vorlage wiederverwenden 
Im Lernprogramm erstellt Sie eine Vorlage zum Definieren von Data Factory Entitäten und eine Vorlage für Werte für Parameter übergeben. Pipeline kopiert Daten aus einem Azure Storage-Konto mit einer Azure SQL-Datenbank über Parameter angegeben. Um dieselbe Vorlage Data Factory Entitäten in unterschiedlichen Umgebungen bereitstellen, erstellen Sie eine Parameterdatei für jede Umgebung und verwenden, wenn in dieser Umgebung bereitstellen.     

Beispiel:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Beachten Sie, dass der erste Befehl Datei für die Development Environment, zweite für die Umgebung, und eine für die produktionsumgebung verwendet.  

Sie können auch die Vorlage wiederholte Aufgaben wiederverwenden. Beispielsweise zu viele Daten Factorys mit eine oder mehrere Rohrleitungen, die Logik implementieren müssen jedoch jedes Data Factory verwendet verschiedene Azure-Speicher und Azure SQL-Datenbank Konten. In diesem Szenario verwenden Sie dieselbe Vorlage in der gleichen Umgebung (Entwicklung, Tests oder Produktion) mit anderen Parameterdateien Daten Factorys erstellt.   

