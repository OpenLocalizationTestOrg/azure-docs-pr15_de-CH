<properties
    pageTitle="Eine mit einer Vorlage Ressourcenmanager Azure Service Bus Autorisierungsregel erstellen | Microsoft Azure"
    description="Erstellen Sie eine Autorisierungsregel Service Bus für Namespace und Warteschlange Azure-Ressourcen-Manager-Vorlage"
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
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Erstellen Sie eine Autorisierungsregel Service Bus für Namespace und Warteschlange mit einer Azure-Ressourcen-Manager-Vorlage

Dieser Artikel beschreibt, wie eine Vorlage Azure-Ressourcen-Manager verwendet, die eine [Autorisierungsregel](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) Service Bus-Namespace und Warteschlange erstellt. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt und Parameter definieren, die angegeben, die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen][].

Vollständige Vorlage finden Sie unter [Service Bus Auth-Vorlage][] auf GitHub.

>[AZURE.NOTE] Folgenden Azure Resource Manager Vorlagen stehen zum Download zur Verfügung.
>
>-    [Erstellen Sie einen Ereignis Hubs Namespace mit einem Event Hub und consumer](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Erstellen Sie einen Service Bus-Namespace Warteschlange](service-bus-resource-manager-namespace-queue.md)
>-    [Erstellen Sie einen Service Bus-Namespace Thema mit Abonnement](service-bus-resource-manager-namespace-topic.md)
>-    [Erstellen Sie einen Service Bus-namespace](service-bus-resource-manager-namespace.md)
>
>Zum Überprüfen der neuesten Vorlagen besuchen Sie [Azure Schnellstart Vorlagen][] -Galerie und suchen Sie nach "Servicebus".

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage wird eine Autorisierungsregel Service Bus-Namespace und Messaging-Entität (in diesem Fall eine Warteschlange) bereitgestellt werden.

Diese Vorlage verwendet [Shared Access Signatur (SAS)](service-bus-sas-overview.md) für die Authentifizierung. SAS ermöglicht Clientanwendungen zur Authentifizierung von Service Bus mit einer Zugriffstaste konfiguriert den Namespace oder messaging Entität (Warteschlange oder Thema) spezifische Berechtigungen zugeordnet sind. Dieser Schlüssel können Sie um SAS-Token zu generieren, mit denen Clients wiederum Service Bus authentifizieren.

Führen Sie die Bereitstellung automatisch klicken:

[![In Azure bereitstellen](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure-Ressourcen-Manager definieren Sie Parameter für Werte zu geben, wenn die Vorlage bereitgestellt wird. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Definieren Sie Parameter für die Werte, die variieren basierend auf das Projekt, das Sie bereitstellen oder die Umgebung, der Sie bereitstellen. Definieren Sie Parameter für die Werte, die immer gleich bleiben. Jeder Parameterwert wird in der Vorlage zur Ressourcen definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Der Name des Service Bus-Namespace erstellen.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

Der Name der Autorisierungsregel für den Namespace.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Der Name der Warteschlange im Service Bus-Namespace.

```
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Der Service Bus-API Version der Vorlage.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Ressourcen zum Bereitstellen

Erstellt ein standard Service Bus-Namespace des Typs **Messaging**und eine Autorisierungsregel Service Bus für Namespace und Entität.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Bereitstellung ausgeführt

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Nächste Schritte

Sie erstellt und mit der Azure-Ressourcen-Manager bereitgestellt haben, erfahren Sie, wie diese Ressourcen verwalten, indem Sie diesen Artikel:

- [Servicebus mit PowerShell verwalten](service-bus-powershell-how-to-provision.md)
- [Verwalten Sie Service Bus mit Service Bus-Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Service Bus Authentifizierung und Autorisierung](service-bus-authentication-and-authorization.md)

  [Azure-Ressourcen-Manager Vorlagen erstellen]: ../resource-group-authoring-templates.md
  [Azure Schnellstart-Vorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Service Bus Auth-Vorlage]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
