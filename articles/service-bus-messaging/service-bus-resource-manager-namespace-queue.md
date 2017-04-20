<properties
    pageTitle="Warteschlange mithilfe einer Vorlage Ressourcenmanager Azure Service Bus-Namespace erstellen | Microsoft Azure"
    description="Erstellen Sie einen Service Bus-Namespace und einer Warteschlange Azure Ressourcenmanager Vorlage"
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

# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Erstellen Sie einen Service Bus-Namespace und einer Warteschlange mit einer Azure-Ressourcen-Manager-Vorlage

Dieser Artikel beschreibt, wie eine Vorlage Azure-Ressourcen-Manager verwendet, die einen Service Bus-Namespace und eine Warteschlange erstellt. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt und Parameter definieren, die angegeben, die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen][].

Vollständige Vorlage finden Sie unter [Service Bus-Namespace und Warteschlange Vorlage][] auf GitHub.

>[AZURE.NOTE] Folgenden Azure Resource Manager Vorlagen stehen zum Download zur Verfügung.
>
>-    [Erstellen Sie einen Service Bus-Namespace mit Warteschlangen und Autorisierung](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Erstellen Sie einen Service Bus-Namespace Thema mit Abonnement](service-bus-resource-manager-namespace-topic.md)
>-    [Erstellen Sie einen Service Bus-namespace](service-bus-resource-manager-namespace.md)
>-    [Erstellen Sie einen Ereignis Hubs Namespace mit einem Event Hub und consumer](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Zum Überprüfen der neuesten Vorlagen besuchen Sie [Azure Schnellstart Vorlagen][] -Galerie und suchen Sie nach "Servicebus".

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage wird einen Service Bus-Namespace einer Warteschlange bereitgestellt.

[Servicebuswarteschlangen](service-bus-queues-topics-subscriptions.md#queues) bieten erste, Nachrichtenübermittlung an einen oder mehrere konkurrierende Consumer erste Out (FIFO).

Führen Sie die Bereitstellung automatisch klicken:

[![In Azure bereitstellen](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure-Ressourcen-Manager definieren Sie Parameter für Werte zu geben, wenn die Vorlage bereitgestellt wird. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Definieren Sie Parameter für die Werte, die variieren basierend auf das Projekt, das Sie bereitstellen oder die Umgebung, der Sie bereitstellen. Definieren Sie Parameter für die Werte, die immer gleich bleiben. Jeder Parameterwert wird in der Vorlage zur Ressourcen definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Der Name des Service Bus-Namespace erstellen.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Der Name der Warteschlange in Service Bus-Namespace erstellt.

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

Erstellt einen standard Service Bus-Namespace des Typs **Messaging**mit einer Warteschlange.

```
"resources ": [{
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
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Bereitstellung ausgeführt

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Nächste Schritte

Sie erstellt und mit der Azure-Ressourcen-Manager bereitgestellt haben, erfahren Sie, wie diese Ressourcen verwalten, indem Sie diesen Artikel:

- [Servicebus mit PowerShell verwalten](service-bus-powershell-how-to-provision.md)
- [Verwalten Sie Service Bus mit Service Bus-Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Azure-Ressourcen-Manager Vorlagen erstellen]: ../resource-group-authoring-templates.md
  [Service Bus-Namespace und Warteschlange Vorlage]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
  [Azure Schnellstart-Vorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
