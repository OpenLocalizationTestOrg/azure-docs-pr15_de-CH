<properties
    pageTitle="Einen Namespace Ereignis Hubs mit Event Hub und der Verbraucher mit einer Azure-Ressourcen-Manager-Vorlage erstellen | Microsoft Azure"
    description="Erstellen Sie einen Ereignis Hubs Namespace Ereignis Hub und consumergruppe Azure-Ressourcen-Manager-Vorlage"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Erstellen Sie einen Namespace Ereignis Hubs mit Event Hub und der Verbraucher mit einer Azure-Ressourcen-Manager-Vorlage

Dieser Artikel beschreibt, wie eine Vorlage Azure-Ressourcen-Manager verwendet, die einen Ereignis Hubs Namespace mit einem Ereignis-Hub und eine. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt und Parameter definieren, die angegeben, die Bereitstellung ausgeführt wird. Diese Vorlage für Ihre eigenen Bereitstellungen verwenden können oder Ihren Anforderungen anpassen

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen][].

Vollständige Vorlage finden Sie unter [Event Hub und Consumer Gruppenvorlage][] auf GitHub.

>[AZURE.NOTE]
>Zum Überprüfen der neuesten Vorlagen finden Sie auf [Azure Schnellstart][] Vorlagensammlung und nach Ereignis Hubs.

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage wird einen Ereignis Hubs Namespace mit einem Ereignis-Hub und eine bereitstellen.

[Ereignis-Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) ist ein Ereignis Verarbeitung Ereignis und Telemetrie Eindringen in Azure in großem Maßstab geringe Latenz und hohe Zuverlässigkeit bereitgestellt.

Führen Sie die Bereitstellung automatisch klicken:

[![In Azure bereitstellen](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure-Ressourcen-Manager definieren Sie Parameter für Werte zu geben, wenn die Vorlage bereitgestellt wird. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Definieren Sie Parameter für die Werte, die variieren basierend auf das Projekt, das Sie bereitstellen oder die Umgebung, der Sie bereitstellen. Definieren Sie Parameter für die Werte, die immer gleich bleiben. Jeder Parameterwert wird in der Vorlage zur Ressourcen definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Der Name des Ereignisses Hubs Namespace erstellen.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

Der Name des Ereignisses erstellt im Namespace Hubs.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Der Name der Verbrauchergruppe für Event Hub erstellt.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>Anforderungsparameter

Die API-Version der Vorlage.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Ressourcen zum Bereitstellen

Erstellt einen Namespace des Typs **EventHubs**ein Ereignis Hub und eine.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Bereitstellung ausgeführt

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Azure-Ressourcen-Manager Vorlagen erstellen]: ../resource-group-authoring-templates.md
[Azure Schnellstart-Vorlagen]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Ereignis-Hub und Consumer Gruppenvorlage]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
