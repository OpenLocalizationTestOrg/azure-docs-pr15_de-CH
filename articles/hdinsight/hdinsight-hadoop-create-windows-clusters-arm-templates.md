<properties
   pageTitle="Hadoop Windows-basierten Cluster in HDInsight Azure-Ressourcen-Manager Vorlagen erstellen | Microsoft Azure"
    description="Informationen Sie zum Cluster für Azure HDInsight Azure-Ressourcen-Manager Vorlagen erstellen."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Erstellen Sie Hadoop Windows-basierten Cluster in HDInsight Azure-Ressourcen-Manager Vorlagen

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Informationen Sie zum HDInsight-Cluster mithilfe von Azure-Ressourcen-Manager Vorlagen erstellen. Weitere Informationen finden Sie unter [Bereitstellen einer Anwendung mit Azure-Ressourcen-Manager-Vorlage](../resource-group-template-deploy.md). Andere Clustererstellung Tools und Features klicken Sie auf die Registerkarte wählen oben auf dieser Seite finden Sie oder [Cluster Methoden](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Komponenten:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Vor diesem Artikel benötigen Sie Folgendes:

- [Azure-Abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell oder Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Steuerelement erforderlich

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Ressourcen-Manager-Vorlagen

Ressourcenmanager Vorlage erleichtert die HDInsight-Cluster, deren abhängigen Ressourcen (wie das Standardkonto Speicher) und andere Ressourcen (wie Azure SQL-Datenbank mit Apache Sqoop) für die Anwendung in einer einzigen koordinierten Operation erstellen. In der Vorlage die Ressourcen für die Anwendung definieren und Bereitstellungsparameter Werte für verschiedene umge bungen angeben. Die Vorlage umfasst JSON und Ausdrücke mit Werten für die Bereitstellung erstellen.

Ressourcenmanager Vorlage für einen HDInsight-Cluster und von Azure Storage-Konto finden Sie [Anhang A](#appx-a-arm-template). Verwenden Sie einen Text-Editor die Vorlage in einer Datei auf Ihrer Arbeitsstation speichern. Wie die Vorlage mit verschiedenen Tools erfahren.

Weitere Informationen zu Ressourcen-Manager-Vorlage finden Sie unter

- [Autor Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md)
- [Bereitstellen einer Anwendung mit Azure-Ressourcen-Manager-Vorlage](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>PowerShell bereitstellen

Die folgende Prozedur erstellt einen HDInsight-Cluster.

**Bereitstellen ein Clusters mithilfe der Ressourcen-Manager-Vorlage**

1. Speichern Sie die Datei Json in [Anhang A](#appx-a-arm-template) auf der Arbeitsstation.
2. Legen Sie die Parameter bei Bedarf.
3. Führen Sie die Vorlage mit dem folgenden PowerShell-Skript aus:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    PowerShell-Skript konfiguriert nur der Clustername und Storage-Kontoname.  In der Ressourcen-Manager-Vorlage können Sie andere Werte festlegen. 
    
Weitere Informationen finden Sie unter [Bereitstellen von PowerShell](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Bereitstellen von Azure CLI

Das folgende Beispiel erstellt einen Cluster von Speicherkonto und der zugehörigen Container durch Aufrufen einer Ressourcen-Manager-Vorlage:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>REST-API bereitstellen

Finden Sie [die REST-API bereitstellen](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>Bereitstellung mit Visual Studio

Mit Visual Studio können Sie Projekt Ressourcen erstellen und über die Benutzeroberfläche in Azure bereitstellen. Wählen Sie den Typ von Ressourcen in Ihr Projekt aufnehmen und Ressourcenmanager Vorlage werden automatisch Ressourcen hinzugefügt. Das Projekt bietet auch ein PowerShell-Skript, um die Vorlage bereitzustellen.

Einführung in Ressourcengruppen mit Visual Studio finden Sie unter [Erstellen und Bereitstellen von Azure Ressourcengruppen über Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie verschiedene HDInsight-Cluster erstellen. Weitere finden Sie in folgenden Artikeln:


- Ein Beispiel zum Bereitstellen von Ressourcen durch die Clientbibliothek .NET finden Sie unter [Ressourcen bereitstellen Proxybibliothek und eine Vorlage verwenden](../virtual-machines/virtual-machines-windows-csharp-template.md).
- Ein ausführliches Beispiel für die Bereitstellung einer Anwendung finden Sie unter [Bereitstellen und Microservices vorhersehbar in Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
- Bereitstellen der Projektmappe in unterschiedlichen Umgebungen Siehe [Entwicklung und testumgebungen in Microsoft Azure](../solution-dev-test-environments.md).
- Zu den Abschnitten der Azure-Ressourcen-Manager-Vorlage finden Sie unter [Erstellung Vorlagen](../resource-group-authoring-templates.md).
- Eine Liste der Funktionen, die in einer Vorlage Azure-Ressourcen-Manager verwendet werden kann, finden Sie unter [Vorlagenfunktionen](../resource-group-template-functions.md).



##<a name="appx-a-resource-manager-template"></a>Anlage a Resource Manager-Vorlage

Die folgende Vorlage Azure Resource Manager erstellt einen Cluster Hadoop Windows-Berücksichtigung von Azure-Speicher.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }

