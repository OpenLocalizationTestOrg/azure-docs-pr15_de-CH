<properties
    pageTitle="Erstellen Ihrer erste Data Factory (Ressourcenmanager Vorlage) | Microsoft Azure"
    description="In diesem Lernprogramm erstellen Sie eine Beispiel Azure Data Factory-Pipeline mit einer Azure-Ressourcen-Manager-Vorlage."
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
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Lernprogramm: Erstellen Sie Ihrer erste Azure Data Factory mit Azure-Ressourcen-Manager-Vorlage
> [AZURE.SELECTOR]
- [Übersicht und Komponenten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressourcen-Manager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In diesem Artikel verwenden Sie eine Vorlage Azure-Ressourcen-Manager zum Erstellen Ihrer ersten Azure Data Factory.

## <a name="prerequisites"></a>Erforderliche Komponenten
- [Lernprogramm](data-factory-build-your-first-pipeline.md) Artikel lesen und die **erforderliche** Schritte.
- Anleitung [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Artikel Version von Azure PowerShell auf Ihrem Computer installieren.
- [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md) Azure Resource Manager Vorlagen Informationen angezeigt. 

## <a name="in-this-tutorial"></a>In diesem Lernprogramm
Entität | Beschreibung  
------ | ----------- 
Azure verknüpft Speicherdienst | Mit der Data Factory verknüpft Azure Storage-Konto. Azure Storage-Konto enthält die Eingabe- und Daten für die Pipeline in diesem Beispiel. 
HDInsight-Anforderung verknüpften Serviceartikel| Links eine bei Bedarf HDInsight cluster Data Factory. Der Cluster wird automatisch erstellt, verarbeitet und wird nach der Verarbeitung gelöscht.
Azure Blob Eingabedatasets | Bezeichnet den Dienst Azure Storage verknüpft. Der verknüpfte Dienst auf Azure Storage-Konto und Azure BLOB-Dataset gibt Container, Ordner und Dateinamen im Speicher, der Eingabedaten enthält. 
Azure Blob Ausgabe dataset | Bezeichnet den Dienst Azure Storage verknüpft. Verknüpfte Dienst auf Azure Storage-Konto und Azure Blob Dataset gibt den Container, Ordner und Dateinamen im Speicher, der die Ausgabedaten enthält. 
Datenpipeline | Die Pipeline hat eine Aktivität vom Typ HDInsightHive Eingabedatasets verwendet und erzeugt das ausgabedataset.   

Eine Factory Daten können eine oder mehrere Rohrleitungen. Eine Pipeline kann eine oder mehrere Aktivitäten enthalten. Es gibt zwei Arten von Aktivitäten: [Transformation](data-factory-data-transformation-activities.md) [Daten bewegungsaktivitäten](data-factory-data-movement-activities.md) und Daten. In diesem Lernprogramm erstellen Sie eine Rohrleitung mit einer Aktivität (Copy-Aktivität).

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

Erstellen einer JSON-Datei namens **ADFTutorialARM.json** im **C:\ADFGetStarted** -Ordner mit folgendem Inhalt:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
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
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
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
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
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
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] Finden Sie ein weiteres Beispiel der Ressourcen-Manager-Vorlage zum Erstellen einer Azure Data Factory auf [Tutorial: Erstellen Sie eine Pipeline mit Kopie mithilfe einer Vorlage Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>Parameter JSON 
Erstellen einer JSON-Datei mit dem Namen **ADFTutorialARM-Parameters.json** , die Parameter der Azure-Ressourcen-Manager-Vorlage enthält.  

> [AZURE.IMPORTANT] Geben Sie den Namen und den Schlüssel des Kontos Azure-Speicher für die Parameter **StorageAccountName** und **StorageAccountKey** in dieser Datei. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] Möglicherweise separate JSON Parameterdateien für Entwicklung, Test und produktionsumgebungen, die mit der gleichen Data Factory JSON-Vorlage verwendet. Mithilfe eines PowerShell-Skripts können Sie die Bereitstellung von Data Factory Entitäten in dieser Umgebung automatisieren. 

## <a name="create-data-factory"></a>Data Factory erstellen

1. Starten Sie **Azure PowerShell** , und führen Sie den folgenden Befehl: 
    - Führen Sie `Login-AzureRmAccount` , und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei Azure-Portal.  
    - Ausführen `Get-AzureRmSubscription` alle Abonnements für dieses Konto an.
    - Ausführen `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` Abonnement wählen Sie arbeiten. Dieses Abonnement sollte identisch mit der Azure-Portal verwendet.
1. Führen Sie den folgenden Befehl Data Factory Entitäten mit der Ressourcen-Manager-Vorlage bereitstellen, die Sie in Schritt 1 erstellt haben. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor-pipeline
 
1.  Nach der Anmeldung an der [Azure-Portal](https://portal.azure.com/), klicken Sie auf **Durchsuchen** und wählen Sie **Daten Factorys**.
        ![Durchsuchen-Factorys Daten >](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  Klicken Sie im Blade **Daten Factorys** erstellten Data Factory (**TutorialFactoryARM**).   
2.  Klicken Sie auf **Diagramm**Blatt **Data Factory** für die Daten-Factory.
        ![Diagramm-Kachel](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  Im **Diagramm**sehen Sie eine Übersicht der Rohrleitungen und Datasets in diesem Lernprogramm verwendet.
    
    ![Diagramm anzeigen](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. Doppelklicken Sie auf das Dataset **AzureBlobOutput**in der Diagrammansicht. Sie sehen, dass das Segment, das derzeit verarbeitet wird.

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Nach Abschluss der Verarbeitung sehen Sie das Segment **bereit** . Erstellung eines Clusters HDInsight auf Anforderung dauert einige Zeit (ca. 20 Minuten). Pipeline **30 Minuten** Segment zu erwarten.

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Wenn der Speicherbereich **bereit** ist, überprüfen Sie den Ordner **Partitioneddata** im Container **Adfgetstarted** im BLOB-Speicher für die Ausgabe von Daten.  

Finden Sie [Monitor Datasets und Pipeline](data-factory-monitor-manage-pipelines.md) Anleitung mit Azure Portal Blades Überwachen der Pipeline und des Datasets in diesem Lernprogramm erstellt haben.

Überwachen und Verwalten von App können auch Ihre Datenpipelines überwachen. Finden Sie [Überwachen und Verwalten von Azure Data Factory Rohrleitungen App überwachen](data-factory-monitor-manage-app.md) ausführliche Informationen zur Verwendung der Anwendungdes. 

> [AZURE.IMPORTANT] Die Eingabedatei wird gelöscht, wenn das Segment verarbeitet. Daher ggf. erneut das Segment oder das Lernprogramm erneut hochladen der Eingabedatei (input.log) in den Ordner Inputdata Adfgetstarted Container.

## <a name="data-factory-entities-in-the-template"></a>Data Factory Einheiten in der Vorlage
### <a name="define-data-factory"></a>Data Factory definieren
Definieren eine Data Factory in der Ressourcen-Manager-Vorlage wie im folgenden Beispiel gezeigt:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

Der DataFactoryName ist wie folgt definiert: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

Es ist eine eindeutige Zeichenfolge basierend auf die Kennung der Ressource.  

### <a name="defining-data-factory-entities"></a>Data Factory Entitäten definieren
Die folgenden Daten Factory Entitäten sind in JSON-Vorlage definiert: 

- [Azure verknüpft Speicherdienst](#azure-storage-linked-service)
- [HDInsight-Anforderung verknüpften Serviceartikel](#hdinsight-on-demand-linked-service)
- [Azure Blob Eingabedatasets](#azure-blob-input-dataset)
- [Azure BLOB-Ausgabe-dataset](#azure-blob-output-dataset)
- [Datenpipeline mit einer Kopie](#data-pipeline)

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

**ConnectionString** verwendet die Parameter StorageAccountName und StorageAccountKey. Die Werte für diese Parameter mithilfe einer Konfigurationsdatei. Die Definition wird auch Variablen: AzureStroageLinkedService und DataFactoryName in der Vorlage definiert. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight-Anforderung verknüpften Serviceartikel
[Berechnen von verknüpften Diensten](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) finden Sie im Artikel Informationen JSON-Eigenschaften einen HDInsight auf Anforderung verknüpften Dienst definiert.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Beachten Sie folgende Punkte: 

- Der erstellt eines **Windows-basierte** HDInsight-Clusters mit der obigen JSON. Sie können auch einen **Linux-basierten** HDInsight Cluster erstellen lassen. Einzelheiten finden Sie [Bei Bedarf HDInsight verknüpften Serviceartikel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- **HDInsight-Cluster** können anstelle eines bedarfsgesteuerten HDInsight Clusters. Details finden Sie in der [Verknüpften HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- HDInsight-Cluster erstellt einen **Standardcontainer** im BLOB-Speicher, in JSON (**Nameverknüpfterdienst angegeben**). HDInsight löscht keine Container, wenn Cluster gelöscht wird. Dieses Verhalten ist entwurfsbedingt. Auf Anforderung verknüpft HDInsight Service ein Cluster erstellt wird, jedes Mal ein Slice muss verarbeitet werden, es sei denn eine vorhandene HDInsight Cluster (**TimeToLive**) live und wird gelöscht, wenn die Verarbeitung abgeschlossen ist.

    Mehrere Segmente verarbeitet werden, sehen Sie viele Container im Azure BLOB-Speicher. Benötigen Sie nicht diese für die Problembehandlung der Aufträge, möchten Sie löschen, um die Speicherkosten zu senken. Die Namen dieser Container ein Muster folgen: "Adf**Yourdatafactoryname**-**nameverknüpfterdienst**- datumuhrzeitstempel". Verwenden Sie Tools wie [Microsoft Storage Explorer](http://storageexplorer.com/) Container im Azure BLOB-Speicher löschen.

Einzelheiten finden Sie [Bei Bedarf HDInsight verknüpften Serviceartikel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .



#### <a name="azure-blob-input-dataset"></a>Azure Blob Eingabedatasets
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
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

Diese Definition verwendet die folgenden Parameter im Parameter Vorlage definiert: BlobContainer, InputBlobFolder und InputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Azure Blob Ausgabe dataset
Sie geben die Namen der BLOB-Container und Ordner, die Ausgabedaten enthält. Details über ein Dataset Azure Blob definiert JSON-Eigenschaften finden Sie unter [Azure Blob Dataset-Eigenschaften](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          }
        }
      }

Diese Definition verwendet die folgenden Parameter in der Vorlage Parameter definiert: BlobContainer und OutputBlobFolder. 

#### <a name="data-pipeline"></a>Datenpipeline
Definieren Sie eine Pipeline, die Daten transformieren, indem Hive-Skript in einem Cluster bei Bedarf Azure HDInsight. Eine Beschreibung der JSON-Elemente zum Definieren einer Pipeline in diesem Beispiel finden Sie unter [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) . 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>Die Vorlage wiederverwenden 
Im Lernprogramm erstellt Sie eine Vorlage zum Definieren von Data Factory Entitäten und eine Vorlage für Werte für Parameter übergeben. Um dieselbe Vorlage Data Factory Entitäten in unterschiedlichen Umgebungen bereitstellen, erstellen Sie eine Parameterdatei für jede Umgebung und verwenden, wenn in dieser Umgebung bereitstellen.     

Beispiel:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

Beachten Sie, dass der erste Befehl Datei für die Development Environment, zweite für die Umgebung, und eine für die produktionsumgebung verwendet.  

Sie können auch die Vorlage wiederholte Aufgaben wiederverwenden. Beispielsweise zu viele Daten Factorys mit eine oder mehrere Rohrleitungen, die Logik implementieren müssen jedoch jedes Data Factory verwendet verschiedene Azure-Speicher und Azure SQL-Datenbank Konten. In diesem Szenario verwenden Sie dieselbe Vorlage in der gleichen Umgebung (Entwicklung, Tests oder Produktion) mit anderen Parameterdateien Daten Factorys erstellt. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Ressourcen-Manager-Vorlage zum Erstellen eines Gateways
Hier ist eine Beispielvorlage Ressourcen-Manager zum Erstellen einer logischen Gateway im zurück. Installieren Sie ein Gateway auf dem lokalen Computer oder Azure Neuerung und registrieren Sie das Gateway mit Data Factory mithilfe eines Schlüssels. Einzelheiten finden Sie unter [Verschieben von Daten zwischen lokalen und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Diese Vorlage erstellt eine mit dem Namen GatewayUsingArmDF und ein Gateway mit dem Namen "Data Factory": GatewayUsingARM. 

## <a name="see-also"></a>Siehe auch
| Thema | Beschreibung |
| :---- | :---- |
| [Aktivitäten für die Transformation von Daten](data-factory-data-transformation-activities.md) | Dieser Artikel enthält eine Liste der Data Transformationsaktivitäten (z. B. HDInsight Struktur Transformation in diesem Lernprogramm verwendeten) von Azure Data Factory unterstützt. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | Dieser Artikel beschreibt die Planung und Ausführung Aspekte der Azure Data Factory-Anwendungsmodell. |
| [Rohrleitungen](data-factory-create-pipelines.md) | Dieser Artikel hilft Pipelines und Aktivitäten in Azure Data Factory und wie sie End-to-End datengesteuerten Workflows für Ihre Situation oder Unternehmen erstellen. |
| [Datasets](data-factory-create-datasets.md) | Dieser Artikel hilft Ihnen zu Datasets in Azure Data Factory.
| [Überwachen und Verwalten von Rohrleitungen App überwachen](data-factory-monitor-manage-app.md) | Dieser Artikel beschreibt das Überwachen, verwalten und Rohrleitungen überwachen und Anwendung debuggen. 

  

