<properties 
    pageTitle="Erstellen einer Logik mit Azure Resource Manager Vorlagen in Azure App Service | Microsoft Azure" 
    description="Verwenden einer Vorlage Azure-Ressourcen-Manager eine leere Anwendung Logik zum Definieren von Workflows bereitstellen." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Erstellen einer Logik mit einer Vorlage

Verwenden einer Vorlage Ressourcenmanager Azure app leere Logik erstellen, die zum Definieren des Workflows verwendet werden kann. Sie können definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Weitere Informationen zur Logik app Eigenschaften finden Sie unter [Logik App Workflow-Management-API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Beispiele für die Definition anzeigen Sie [Autor Logik App-Definitionen](app-service-logic-author-definitions.md) 

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md).

Vollständige Vorlage finden Sie unter [Logik-App-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen

Mit dieser Vorlage wird eine Logik-app bereitgestellt.

Führen Sie die Bereitstellung automatisch klicken:  

[![In Azure bereitstellen](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Ressourcen zum Bereitstellen

### <a name="logic-app"></a>Logik-app

Erstellt die Anwendung Logik.

Die Vorlagen verwendet einen Parameterwert für Anwendungsname Logik. Den Speicherort der Logik-app festgelegt auf demselben Speicherort wie die Ressourcengruppe. 

Dieser bestimmten Definition wird einmal pro Stunde ausgeführt und Ping-Signale in der **TestUri** -Parameter angegebenen Speicherort. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Bereitstellung ausgeführt

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
