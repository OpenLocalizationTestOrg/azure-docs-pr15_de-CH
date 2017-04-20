<properties
    pageTitle="Service Bus-Namespace Thema Abonnement erstellen und Regel mithilfe einer Vorlage Ressourcenmanager Azure | Microsoft Azure"
    description="Erstellen Sie Service Bus-Namespace Thema Abonnement und Regel Azure-Ressourcen-Manager-Vorlage"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Thema Abonnement und Regel mithilfe einer Vorlage Ressourcenmanager Azure erstellen Sie Service Bus-namespace

Dieser Artikel beschreibt, wie eine Vorlage Azure-Ressourcen-Manager verwendet, die einen Service Bus-Namespace mit einem Thema Abonnement und Regel (Filter). Erfahren Sie wie um zu definieren, welche Ressourcen bereitgestellt und Parameter definieren, die angegeben, die Bereitstellung ausgeführt wird. Diese Vorlage für Ihre eigenen Bereitstellungen verwenden können oder Ihren Anforderungen anpassen

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen][].

Weitere Informationen über Verfahren und Muster Azure Ressourcen Namenskonventionen finden Sie unter [Azure Ressourcen Benennungskonventionen][].

Vollständige Vorlage finden Sie unter [Service Bus-Namespace Thema Abonnement und Regel][] -Vorlage.

>[AZURE.NOTE] Folgenden Azure Resource Manager Vorlagen stehen zum Download zur Verfügung.
>
>-    [Erstellen Sie einen Service Bus-Namespace mit Warteschlangen und Autorisierung](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Erstellen Sie einen Service Bus-Namespace Warteschlange](service-bus-resource-manager-namespace-queue.md)
>-    [Erstellen Sie einen Service Bus-namespace](service-bus-resource-manager-namespace.md)
>-    [Erstellen Sie einen Service Bus-Namespace Thema mit Abonnement](service-bus-resource-manager-namespace-topic.md)
>
>Besuchen Sie zum neuesten Vorlagen überprüfen [Azure Schnellstart][] Vorlagensammlung und nach Service Bus.

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage stellen Sie Service Bus-Namespace Thema Abonnement und Regel (Filter).

[Service Bus-Themen und Abonnements](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) bieten eine n-Form der Kommunikation im *Veröffentlichen-Abonnieren* -Musters. Wenn Themen und Abonnements mit Komponenten einer verteilten Anwendung kommunizieren nicht direkt miteinander, tauschen sie stattdessen Nachrichten über Thema als Vermittler fungiert. Ein Abonnement für ein Thema ähnelt eine virtuelle Warteschlange, die Nachrichten empfängt, die dem Thema gesendet wurden. Filter für Abonnement ermöglicht sollten Sie angeben, welche Nachrichten an ein Thema innerhalb eines bestimmten Themas Abonnements angezeigt.

## <a name="what-are-rules-filters"></a>Was sind Regeln (Filter)?

In vielen Szenarios müssen Nachrichten, die bestimmte Merkmale unterschiedlich verarbeitet werden. Dazu können Sie Abonnements Nachrichten finden, die Eigenschaften gewünscht führen Sie bestimmte Änderungen an den Eigenschaften konfigurieren. Service Bus-Abonnements alle Nachrichten zum Thema sehen, können Sie nur eine Teilmenge dieser Nachrichten Warteschlange virtuellen Abonnement kopieren. Dies geschieht mithilfe der Abonnementfilter. Informationen zu rules(filters) finden Sie unter [Servicebuswarteschlangen, Themen und Abonnements][].

Führen Sie die Bereitstellung automatisch klicken:

[![In Azure bereitstellen](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure-Ressourcen-Manager sollten Sie Parameter für Werte definieren Sie beim Bereitstellen der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Definieren Sie Parameter für die Werte, die variieren basierend auf das Projekt, das Sie bereitstellen oder die Umgebung, der Sie bereitstellen. Definieren Sie Parameter für die Werte, die immer gleich bleiben. Jeder Parameterwert wird in der Vorlage zur Ressourcen definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Der Name des Service Bus-Namespace erstellen.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

Der Name des Themas in Service Bus-Namespace erstellt.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

Der Name des Abonnements in Service Bus-Namespace erstellt.

```
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a>serviceBusRuleName

Der Name des rule(filter) in Service Bus-Namespace erstellt.

```
   "serviceBusRuleName": {
   "type": "string",
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

Erstellt einen standard Service Bus-Namespace des Typs **Messaging**Thema Abonnement und Regeln.

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Bereitstellung ausgeführt

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Nächste Schritte

Sie erstellt und mit der Azure-Ressourcen-Manager bereitgestellt haben, erfahren Sie, wie diese Ressourcen verwalten, indem Sie diesen Artikel:

- [Verwalten von Azure Service Bus Azure Automatisierung](service-bus-automation-manage.md)
- [Servicebus mit PowerShell verwalten](service-bus-powershell-how-to-provision.md)
- [Verwalten Sie Service Bus mit Service Bus-Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Azure-Ressourcen-Manager Vorlagen erstellen]: ../resource-group-authoring-templates.md
  [Azure Schnellstart-Vorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Benennungskonventionen Azure-Ressourcen]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [Service Bus-Namespace Thema Abonnement und Regel]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Servicebuswarteschlangen, Themen und Abonnements]:service-bus-queues-topics-subscriptions.md
  
