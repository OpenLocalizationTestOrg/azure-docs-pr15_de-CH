<properties
    pageTitle="Erstellen Sie einen Service Bus-Namespace Ressourcenmanager Vorlage | Microsoft Azure"
    description="Verwenden Sie Azure Ressourcenmanager Vorlage Service Bus-Namespace erstellen"
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
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Erstellen eines mit einer Vorlage Ressourcenmanager Azure Service Bus Namespaces

Dieser Artikel beschreibt die Vorlage eine Azure-Ressourcen-Manager verwenden, einen Service Bus-Namespace des Typs **Messaging** mit Standard-Basic SKU. Artikel definiert auch die Parameter, die für die Ausführung der Bereitstellung angegeben werden. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen][].

Vollständige Vorlage finden Sie unter [Service Bus-Namespace Vorlage][] auf GitHub.

>[AZURE.NOTE] Folgenden Azure Resource Manager Vorlagen stehen zum Download zur Verfügung. 
>
>-    [Erstellen Sie einen Ereignis Hubs Namespace mit einem Event Hub und consumer](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Erstellen Sie einen Service Bus-Namespace Warteschlange](service-bus-resource-manager-namespace-queue.md)
>-    [Erstellen Sie einen Service Bus-Namespace Thema mit Abonnement](service-bus-resource-manager-namespace-topic.md)
>-    [Erstellen Sie einen Service Bus-Namespace mit Warteschlangen und Autorisierung](service-bus-resource-manager-namespace-auth-rule.md)
>
>Besuchen Sie zum neuesten Vorlagen überprüfen [Azure Schnellstart][] Vorlagensammlung und nach Service Bus.

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage wird einen Service Bus-Namespace mit einer [einfachen, Standard oder Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU bereitgestellt.

Führen Sie die Bereitstellung automatisch klicken:

[![In Azure bereitstellen](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure-Ressourcen-Manager definieren Sie Parameter für Werte zu geben, wenn die Vorlage bereitgestellt wird. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Definieren Sie Parameter für die Werte, die variieren basierend auf das Projekt, das Sie bereitstellen oder die Umgebung, der Sie bereitstellen. Definieren Sie Parameter für die Werte, die immer gleich bleiben. Jeder Parameterwert wird in der Vorlage zur Ressourcen definieren, die bereitgestellt werden.

Diese Vorlage definiert die folgenden Parameter.

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

### <a name="servicebussku"></a>serviceBusSKU

Der Name des Service Bus [Lagerhaltungsdaten](https://azure.microsoft.com/pricing/details/service-bus/) erstellen.

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Die Vorlage definiert die Werte für diesen Parameter (Basic, Standard oder Premium) dürfen und weist einen Standardwert (Standard), wenn kein Wert angegeben ist.

Weitere Informationen zu Service Bus Preise finden Sie unter [Service Bus Preise und Abrechnung][].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Der Service Bus-API Version der Vorlage.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Ressourcen zum Bereitstellen

### <a name="service-bus-namespace"></a>Service Bus-namespace

Erstellt einen standard Service Bus-Namespace des Typs **Messaging**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Bereitstellung ausgeführt

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Nächste Schritte

Sie erstellt und mit der Azure-Ressourcen-Manager bereitgestellt haben, erfahren Sie, wie diese Ressourcen verwalten, indem Sie diesen Artikel lesen:

- [Servicebus mit PowerShell verwalten](service-bus-powershell-how-to-provision.md)
- [Verwalten Sie Service Bus mit Service Bus-Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Azure-Ressourcen-Manager Vorlagen erstellen]: ../resource-group-authoring-templates.md
  [Service Bus-Namespace Vorlage]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Azure Schnellstart-Vorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Service Bus Preise und Abrechnung]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
