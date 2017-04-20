<properties
    pageTitle="Einen Namespace Ereignis Hubs mit Ereignis erstellen und Archiv mithilfe einer Vorlage Azure-Ressourcen-Manager aktivieren | Microsoft Azure"
    description="Erstellen Sie einen Namespace Ereignis Hubs mit Ereignis und aktivieren Sie Archiv mit Azure-Ressourcen-Manager-Vorlage"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Einen Namespace Ereignis Hubs mit Ereignis erstellen und Archiv mithilfe einer Vorlage Azure-Ressourcen-Manager aktivieren

Dieser Artikel beschreibt, wie eine Vorlage Azure-Ressourcen-Manager verwenden, die erstellt einen Namespace Ereignis Hubs mit einem Ereignis-Hub und Archive auf Ihrem Haupt-Ereignis ermöglicht. Erfahren Sie wie um zu definieren, welche Ressourcen bereitgestellt und Parameter definieren, die angegeben, die Bereitstellung ausgeführt wird. Diese Vorlage für Ihre eigenen Bereitstellungen verwenden können oder Ihren Anforderungen anpassen

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen][].

Weitere Informationen über Verfahren und Muster auf Azure Ressourcen Benennungskonventionen finden Sie unter [Namenskonventionen für Azure Ressourcen][].

Vollständige Vorlage finden Sie unter [Event Hub und Archiv-Vorlage aktivieren][] auf GitHub.

>[AZURE.NOTE]
>Zum Überprüfen der neuesten Vorlagen finden Sie auf [Azure Schnellstart][] Vorlagensammlung und nach Ereignis Hubs.

## <a name="what-you-deploy"></a>Was Sie bereitstellen?

Mit dieser Vorlage einen Ereignis Hubs Namespace mit einem Ereignis-Hub bereitstellen und Archivierung aktivieren.

[Ereignis-Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) ist ein Ereignis Verarbeitung Ereignis und Telemetrie Eindringen in Azure in großem Maßstab geringe Latenz und hohe Zuverlässigkeit bereitgestellt. Ereignis Hubs Archiv können Sie die Daten automatisch in Ihr Ereignis Hubs Azure BLOB-Speicher Ihrer Wahl innerhalb einer angegebenen Zeit oder Größe Intervall Ihrer Wahl zu liefern.

Führen Sie die Bereitstellung automatisch klicken:

[![In Azure bereitstellen](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure-Ressourcen-Manager definieren Sie Parameter für Werte zu geben, wenn die Vorlage bereitgestellt wird. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Definieren Sie Parameter für die Werte, die variieren basierend auf das Projekt, das Sie bereitstellen oder die Umgebung, der Sie bereitstellen. Definieren Sie Parameter für die Werte, die immer gleich bleiben. Jeder Parameterwert wird in der Vorlage zur Ressourcen definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Der Name des Ereignisses Hubs Namespace erstellen.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

Der Name des Ereignisses erstellt im Namespace Hubs.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Die Anzahl der Tage beibehalten, Nachrichten in Ihrem Haupt-Ereignis. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

Die Anzahl der Partitionen in Ihrem Haupt-Ereignis soll.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Aktivieren Sie das Archiv auf Ihrem Haupt-Ereignis.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

Das Codierungsformat Geben Sie Daten zu serialisieren.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Das Zeitintervall, das Archiv Beginn Archivierung in Azure BLOB-Speicher.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

Das Intervall, Größe des Archivs Beginn Archivierung in Azure BLOB-Speicher.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Das Archiv erfordert ein Speicherkonto Ressourcennr Archiv dem gewünschten Azure-Speicher zu aktivieren.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

BLOB-Container an die Daten archiviert werden.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>Anforderungsparameter

Die API-Version der Vorlage.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Ressourcen zum Bereitstellen

Erstellt einen Namespace des Typs **EventHubs**mit einem Ereignis-Hub und Archiv ermöglicht.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
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
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Bereitstellung ausgeführt

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Azure-Ressourcen-Manager Vorlagen erstellen]: ../resource-group-authoring-templates.md
[Azure Schnellstart-Vorlagen]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Benennungskonventionen Azure-Ressourcen]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Ereignis-Hub und Archiv-Vorlage aktivieren]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
