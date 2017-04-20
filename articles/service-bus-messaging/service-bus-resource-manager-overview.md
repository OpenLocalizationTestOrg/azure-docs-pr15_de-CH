<properties
    pageTitle="Service Bus Ressourcen Azure-Ressourcen-Manager Vorlagen erstellen | Microsoft Azure"
    description="Verwenden von Azure-Ressourcen-Manager Vorlagen automatisieren die Erstellung von Service Bus-Ressourcen"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Erstellen Sie Service Bus Ressourcen Azure-Ressourcen-Manager Vorlagen

Dieser Artikel beschreibt das Erstellen und Bereitstellen von Service Bus und Ereignis Hubs Ressourcen Azure Ressourcenmanager Vorlagen PowerShell und Service Bus Ressourcenanbieter.

Azure Ressourcenmanager Vorlagen können Sie Ressourcen für eine Lösung bereitstellen und Parameter und Variablen, mit denen Sie Werte für verschiedene Unternehmen definieren. Die Vorlage umfasst JSON und Ausdrücke mit Werten für die Bereitstellung erstellen. Ausführliche Informationen zum Schreiben von Azure-Ressourcen-Manager-Vorlagen und eine Beschreibung der Vorlage finden Sie unter [Erstellen von Azure-Ressourcen-Manager Vorlagen](../resource-group-authoring-templates.md). 

>[AZURE.NOTE] Die Beispiele in diesem Artikel zeigen, wie Ressourcenmanager Azure Service Bus-Namespace und Messaging-Entität (Warteschlange) verwendet. Weitere Beispiele Vorlage finden Sie in [Azure Schnellstart Vorlagen-Galerie][] und suchen Sie nach "Servicebus".

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Service Bus und Ereignis Hubs Resource Manager Vorlagen

Vorlagen Service Bus und Ereignis Hubs Azure Resource Manager stehen zum Download zur Verfügung. Klicken Sie auf die folgenden Links Informationen jeweils mit Links zu den Vorlagen auf GitHub: 

- [Erstellen Sie einen Service Bus-namespace](service-bus-resource-manager-namespace.md)
- [Erstellen Sie einen Service Bus-Namespace Warteschlange](service-bus-resource-manager-namespace-queue.md)
- [Erstellen Sie einen Service Bus-Namespace Thema mit Abonnement](service-bus-resource-manager-namespace-topic.md)
- [Erstellen Sie einen Service Bus-Namespace mit Warteschlangen und Autorisierung](service-bus-resource-manager-namespace-auth-rule.md)
- [Erstellen Sie einen Ereignis Hubs Namespace mit einem Event Hub und consumer](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>PowerShell bereitstellen

Das folgende Verfahren beschreibt, wie mit PowerShell Azure-Ressourcen-Manager-Vorlage bereitstellen, die eine **Standard** -Tier Service Bus-Namespace und eine Warteschlange in diesem Namespace erstellt. Dieses Beispiel basiert auf der Vorlage [erstellen einen Service Bus-Namespace mit Warteschlange](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) . Ungefähre Workflow lautet wie folgt:

1. Installieren Sie PowerShell.
2. Erstellen der Vorlage und (optional) eine Datei.
2. Melden Sie sich in PowerShell Azure-Konto an.
3. Erstellen Sie eine neue Ressourcengruppe, wenn nicht vorhanden.
4. Testen der Bereitstellung.
5. Legen Sie ggf. den Bereitstellungsmodus.
6. Bereitstellen der Vorlage.

Vollständige Informationen zum Bereitstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie [Bereitstellen Ressourcen Azure Ressourcenmanager Vorlagen][].

### <a name="install-powershell"></a>Installieren Sie PowerShell

Installieren Sie Azure PowerShell gemäß der Anleitung [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

### <a name="create-a-template"></a>Erstellen einer Vorlage

Klonen Sie oder kopieren Sie die Vorlage [201 Servicebus erstellen Warteschlange](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) von GitHub:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Erstellen Sie eine Datei (optional)

Ein optionaler Parameter Datei kopieren Sie [201 Servicebus erstellen Warteschlange](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) Datei. Ersetzen Sie den Wert der `serviceBusNamespaceName` mit dem Namen des Service Bus-Namespace soll in dieser Bereitstellung erstellt und der Wert des `serviceBusQueueName` mit dem Namen der zu erstellenden Warteschlange. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Weitere Informationen finden Sie auf [Datei](../resource-group-template-deploy.md#parameter-file) .

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Melden Sie sich bei Azure und Azure-Abonnement festlegen

Führen Sie den folgenden Befehl aus einem PowerShell:

```
Login-AzureRmAccount
```

Sie werden aufgefordert, Ihre Azure-Konto anmelden. Nach dem anmelden, führen Sie den folgenden Befehl an Abonnements verfügbar.

```
Get-AzureRMSubscription
```

Dieser Befehl gibt eine Liste der verfügbaren Azure-Abonnements. Wählen Sie ein Abonnement für die aktuelle Sitzung durch Ausführen des folgenden Befehls. Ersetzen Sie `<YourSubscriptionId>` mit der GUID für die Azure-Abonnement verwenden möchten.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Festlegen der Ressourcengruppe

Wenn Sie eine vorhandene Ressourcengruppe erstellen Sie eine neue Ressourcengruppe mit dem Befehl **Neu AzureRmResourceGroup** keinen. Geben Sie den Namen der Ressourcengruppe und Speicherort, den Sie verwenden möchten. Zum Beispiel:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Wenn erfolgreich, wird eine neue Ressourcengruppe angezeigt.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Testen der Bereitstellung

Überprüfen Sie die Bereitstellung mit den `Test-AzureRmResourceGroupDeployment` Cmdlet. Beim Testen der Bereitstellung Geben Sie Parameter, genau wie bei die Bereitstellung ausführen.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Erstellen der Bereitstellung

Führen Sie zum Erstellen der neuen Bereitstellung der `New-AzureRmResourceGroupDeployment` Befehl, und geben Sie die erforderlichen Parameter Aufforderung. Die Parameter umfassen einen Namen für die Bereitstellung der Name der Ressourcengruppe und den Pfad oder URL der Vorlagendatei. Wenn der **Mode** -Parameter nicht angegeben ist, wird der Standardwert **inkrementell** verwendet. Weitere Informationen finden Sie unter [inkrementelle und vollständige Bereitstellung](../resource-group-template-deploy.md#incremental-and-complete-deployments).

Der folgende Befehl fordert Sie drei Parameter im PowerShell-Fenster:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Verwenden Sie den folgenden Befehl, um eine Datei stattdessen angeben.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Auch können Inline Parameter beim Deployment-Cmdlet ausführen. Der Befehl ist wie folgt:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Führen Sie eine [vollständige](../resource-group-template-deploy.md#incremental-and-complete-deployments) Bereitstellung den **Mode** -Parameter auf **abgeschlossen**festgelegt:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Wenn Ressourcen erfolgreich bereitgestellt werden, wird eine Zusammenfassung der Bereitstellung im PowerShell-Fenster angezeigt:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt grundlegende Workflow und Befehle für die Bereitstellung von Azure-Ressourcen-Manager-Vorlage angezeigt. Weitere Informationen finden Sie auf der folgenden Links:

- [Azure-Ressourcen-Manager (Übersicht)][]
- [Bereitstellen von Ressourcen mit Azure-Ressourcen-Manager-Vorlagen][]
- [Erstellen von Vorlagen](../resource-group-authoring-templates.md)


[Azure-Ressourcen-Manager (Übersicht)]: ../resource-group-overview.md
[Bereitstellen von Ressourcen mit Azure-Ressourcen-Manager-Vorlagen]: ../resource-group-template-deploy.md
[Azure Schnellstart Vorlagensammlung]: https://azure.microsoft.com/documentation/templates/?term=service+bus