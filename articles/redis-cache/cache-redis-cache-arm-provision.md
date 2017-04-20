<properties 
    pageTitle="Redis Cache bereitstellen | Microsoft Azure" 
    description="Vorlage verwenden Azure-Ressourcen-Manager einen Azure Redis Cache bereitgestellt." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Erstellen einer Redis mithilfe einer Vorlage

In diesem Thema erfahren Sie, wie eine Azure-Ressourcen-Manager-Vorlage erstellen, die eine Azure Redis Cache bereitgestellt. Cache kann zu Daten mit einem vorhandenen Storage-Konto verwendet werden. Außerdem erfahren Sie wie um zu definieren, welche Ressourcen bereitgestellt und Parameter definieren, die angegeben, die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Derzeit sind Diagnose Einstellungen für alle Caches in derselben Region für ein Abonnement freigegeben. Aktualisiert einen Cache im Bereich betrifft alle Caches in der Region.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md).

Vollständige Vorlage finden Sie unter [Vorlage Redis Cache](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

>[AZURE.NOTE] Ressourcen-Manager Vorlagen für neue [Premium-Ebene](cache-premium-tier-intro.md) stehen. 
>
>-    [Erstellen Sie einen Premium Redis Cache mit clustering](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Premium Redis Cache mit Daten erstellen](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Erstellen Sie Premium Redis Cache mit VNet und optionale clustering](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>Zum Überprüfen der neuesten Vorlagen [Azure Schnellstart](https://azure.microsoft.com/documentation/templates/) Vorlagen, und suchen Sie nach `Redis Cache`.

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen

In dieser Vorlage wird eine Azure Redis Cache bereitgestellt, die ein vorhandenes Speicherkonto für diagnostische Daten verwendet.

Führen Sie die Bereitstellung automatisch klicken:

[![In Azure bereitstellen](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure-Ressourcen-Manager definieren Sie Parameter für Werte zu geben, wenn die Vorlage bereitgestellt wird. Die Vorlage enthält einen Abschnitt Parameter, der alle Parameterwerte enthält.
Definieren Sie Parameter für die Werte, die variieren basierend auf das Projekt, das Sie bereitstellen oder die Umgebung, der Sie bereitstellen. Definieren Sie Parameter für die Werte, die immer gleich bleiben. Jeder Parameterwert wird in der Vorlage zur Ressourcen definieren, die bereitgestellt werden. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

Der Speicherort des Caches Redis. Verwenden Sie für optimale Leistung dieselbe App im Cache verwendet werden.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

Der Name des vorhandenen Speicherkonto für die Diagnose verwendet. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Ein boolescher Wert, der angibt, ob der Zugriff über nicht-SSL-Ports.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Ein Wert, der angibt, ob die Diagnose aktiviert ist. Verwenden aktivieren oder deaktivieren.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Ressourcen zum Bereitstellen

### <a name="redis-cache"></a>Redis Cache

Azure Redis Cache erstellt.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Bereitstellung ausgeführt

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


