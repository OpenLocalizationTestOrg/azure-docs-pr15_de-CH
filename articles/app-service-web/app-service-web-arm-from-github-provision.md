<properties 
    pageTitle="Bereitstellen einer Web-app ein GitHub Repository verknüpft ist" 
    description="Verwenden einer Vorlage Ressourcenmanager Azure eine Webanwendung bereitstellen, die ein von einem GitHub-Repository Projekt." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Bereitstellen einer Web app GitHub Repository verknüpft

In diesem Thema erfahren Sie, wie eine Azure-Ressourcen-Manager-Vorlage erstellen, die eine Webanwendung bereitgestellt wird, die einem Projekt in ein GitHub Repository verknüpft ist. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt und Parameter definieren, die angegeben, die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md).

Vollständige Vorlage finden Sie unter [Web App mit GitHub-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen

Mit dieser Vorlage wird mit dem Code aus einem Projekt in GitHub Web app bereitstellen.

Führen Sie die Bereitstellung automatisch klicken:

[![In Azure bereitstellen](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

Der URL für GitHub Repository mit dem Projekt bereitstellen. Dieser Parameter enthält einen Standardwert, aber dieser Wert soll nur zeigen, wie Sie die URL für Repository bereitstellen. Sie können diesen Wert bei der Vorlage jedoch wird der URL beim Arbeiten mit der Vorlage eigene Repository bereitstellen möchten.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>Zweigstellen

Die Verzweigung des Repositorys beim Bereitstellen der Anwendungdes verwendet. Der Standardwert ist master können Sie den Namen jeder Zweig in das Repository, das Sie bereitstellen möchten.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Ressourcen zum Bereitstellen

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>WebApp

Webanwendung, die mit dem Projekt in GitHub erstellt. 

Geben Sie Namen Web App durch Parameter **SiteName** und den Speicherort der Web-app über den **SiteLocation** -Parameter. Im **DependsOn** -Element definiert die Vorlage Web app-Hosting-Plan Dienst abhängig. Da hosting Plan abhängig ist, wird Web app erst erstellt, Hosting-Plan wurde erstellt. **DependsOn** -Element dient nur Bereitstellungsreihenfolge angeben. Wenn Sie nicht die Web app hosting Plan abhängig markieren, Azure Resource Manager – versucht, beide Ressourcen gleichzeitig erstellen und möglicherweise Fehler Wenn Web app vor der Hosting-Plan erstellt wird.

Web app hat auch eine untergeordnete Ressource in Abschnitt **Ressourcen** definiert. Untergeordnete Ressource definiert Datenquellen-Steuerelement für Project Web App bereitgestellt. In dieser Vorlage wird das Datenquellen-Steuerelement mit einem bestimmten GitHub Repository verknüpft. GitHub Repository ist mit dem Code definiert **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** möglicherweise hartcodieren Repository-URL eine Vorlage erstellen, die ein einzelnes Projekt wiederholt bereitstellt, erfordert die minimale Anzahl von Parametern werden möchten.
Statt Programmieren Repository-URL, können Sie Parameter für die Repository-URL hinzufügen und verwenden diesen Wert für die Eigenschaft **RepoUrl** .

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Bereitstellung ausgeführt

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
