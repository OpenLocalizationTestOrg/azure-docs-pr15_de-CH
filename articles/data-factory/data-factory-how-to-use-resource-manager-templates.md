<properties 
    pageTitle="Mit Ressourcen-Manager Vorlagen in Data Factory | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Verwenden von Azure-Ressourcen-Manager Vorlagen Data Factory Entitäten erstellt." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Verwenden von Vorlagen zum Azure Data Factory Entitäten erstellen

## <a name="overview"></a>Übersicht
Beim Verwenden von Azure Data Factory für Ihren Bedürfnissen Integration können Sie finden Wiederverwendung dasselbe Muster in unterschiedlichen Umgebungen oder die gleiche Aufgabe wiederholt in derselben Projektmappe implementieren. Vorlagen können Sie die Implementierung und Verwaltung dieser Szenarien einfach. Vorlagen in Azure Data Factory sind ideal für Szenarien, die Wiederverwendung und Wiederholung.
 
Wäre die Situation eine Organisation 10 Produktionsstätten weltweit hat. Die Protokolle Pflanze werden in einer separaten lokalen SQL Server-Datenbank gespeichert. Das Unternehmen möchte ein einzelnes Datawarehouse in der Cloud für Ad-hoc-Analysen erstellen. Er möchte die gleiche Logik aber unterschiedliche Konfigurationen für Entwicklung, Test und Produktion. 

In diesem Fall muss eine Aufgabe in derselben Umgebung jedoch mit anderen Werten über 10 Daten Factorys für jedes Herstellungsbetriebs wiederholt werden. **Wiederholung** ist vorhanden. Vorlagen ermöglicht die Abstraktion dieser generischen Flow (d. h. jede Factory Daten dieselben Aktivitäten unter Rohrleitungen). verwendet eine separate Datei für jedes Herstellungsbetriebs

Darüber hinaus können die Organisation dieser Factorys 10 Daten mehrmals in unterschiedlichen Umgebungen bereitstellen möchte, Vorlagen dieses **Wiederverwendung** mithilfe separater Parameterdateien für Entwicklung, Test und Produktion.

## <a name="templating-with-azure-resource-manager"></a>Vorlagen mit Azure Resource Manager
[Azure-Ressourcen-Manager Vorlagen](../azure-resource-manager/resource-group-overview.md#template-deployment) eignen sich hervorragend zu Vorlagen in Azure Data Factory. Ressourcen-Manager Vorlagen definieren die Infrastruktur und die Konfiguration von Azure-Lösung über eine JSON. Da alle die meisten Azure Services Azure Resource Manager Vorlagen arbeiten, kann es weit zum Verwalten Ihrer Azure Ressourcen alle Ressourcen verwendet werden. Siehe [Erstellen von Azure Resource Manager Vorlagen](../resource-group-authoring-templates.md) Ressourcenmanager Vorlagen im Allgemeinen erfahren 

## <a name="tutorials"></a>Lernprogramme
Siehe die folgenden Lernprogramme Anleitungsschritte Data Factory Entitäten mit Ressourcen-Manager Vorlagen erstellen:

- [Lernprogramm: Erstellen einer Pipeline zum Kopieren von Daten mithilfe von Azure-Ressourcen-Manager-Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Lernprogramm: Mithilfe einer Pipeline Daten Azure Ressourcenmanager Vorlage](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Factory Datenvorlagen auf Github
Schauen Sie sich die folgenden Vorlagen Azure Schnellstart auf Github: 

- [Erstellen einer Data Factory, um Daten von Azure BLOB-Speicher in Azure SQL-Datenbank kopieren](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Erstellen einer Data Factory Azure HDInsight Cluster mit Struktur](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Erstellen einer Data Factory zum Kopieren von Daten aus "Salesforce.com" Azure-BLOBs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Ihre Azure Data Factory Vorlagen [Azure schnell](https://azure.microsoft.com/documentation/templates/)beginnen können. Finden Sie im [Beitrag Guide](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) beim Entwickeln von Vorlagen über dieses Repository gemeinsam genutzt werden können. 

Die folgenden Abschnitte enthalten Informationen zum Definieren von Data Factory-Ressourcen in einem Ressourcen-Manager-Vorlage. 

## <a name="defining-data-factory-resources-in-templates"></a>Definieren von Data Factory Ressourcen in Vorlagen
Auf der obersten Ebene Vorlage für Data Factory definiert ist:

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
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Data Factory definieren

Definieren eine Data Factory in der Ressourcen-Manager-Vorlage wie im folgenden Beispiel gezeigt:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

Die DataFactoryName ist in "Variablen" wie folgt definiert:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Verknüpfte Dienste definieren 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Details über die JSON-Eigenschaften für verknüpfte Dienst bereitstellen möchten finden Sie unter [verknüpfte Speicher](data-factory-azure-blob-connector.md#azure-storage-linked-service) oder [Verknüpften Dienste berechnet](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . Der Parameter "DependsOn" gibt Namen der entsprechenden Daten Factory. Ein Beispiel definieren einen verknüpften Dienst für Azure-Speicher ist in folgenden JSON-Definition:

### <a name="define-datasets"></a>Definieren von datasets

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Details finden Sie [Datenspeicher unterstützt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) über JSON-Eigenschaften für den bestimmten Dataset bereitstellen möchten. Hinweis: der Parameter "DependsOn" Name der entsprechenden Daten Factory und Speicher gibt verknüpft Service. Ein Beispiel der Definition Dataset Azure BLOB-Speicher ist in folgenden JSON-Definition:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Definieren von pipelines

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Finden Sie unter [Definieren von Rohrleitungen](data-factory-create-pipelines.md#pipeline-json) für Details über die JSON-Eigenschaften definieren bestimmte Pipeline und Aktivitäten, die Sie bereitstellen möchten. Hinweis "DependsOn"-Parameter gibt Namen Data Factory und entsprechenden Dienste oder Datasets verknüpft. Ein Beispiel einer Pipeline, die Daten von Azure BLOB-Speicher in Azure SQL-Datenbank kopiert wird im folgenden Codeausschnitt JSON:

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
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Parametrisieren Data Factory-Vorlage
Best Practices zur Parametrisierung finden Sie im Artikel [Best practices für Azure-Ressourcen-Manager Vorlagen erstellen](../resource-manager-template-best-practices.md#parameters) . Verwendung des Parameters sollte im Allgemeinen besonders wenn Variablen stattdessen können minimiert werden. Geben Sie nur Parameter in den folgenden Szenarien:

- Einstellungen variieren je nach Umgebung (Beispiel: Entwicklung, Test und Produktion)
- Schlüssel (z. B. Kennwörter)

Ggf. Geheimnisse von [Azure Key Vault](../key-vault/key-vault-get-started.md) ziehen, bei der Bereitstellung von Azure Data Factory Entitäten Vorlagen geben Sie **wichtige Depot** und **geheimen Namen** wie im folgenden Beispiel gezeigt:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Beim Exportieren von Vorlagen für vorhandene Daten Factorys derzeit noch nicht unterstützt wird, wird es. 


