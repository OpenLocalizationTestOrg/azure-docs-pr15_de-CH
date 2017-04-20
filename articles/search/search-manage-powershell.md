<properties 
    pageTitle="Azure Suche mit Powershell-Skripts verwalten | Microsoft Azure | Gehostete Cloud-Suchdienst" 
    description="Verwalten Sie Ihre Azure-Suchdienst mit PowerShell-Skripts. Oder aktualisieren Sie einen Azure-Suchdienst Schlüssel erstellen Sie und verwalten Sie Azure Search admin" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>Verwalten Sie Ihre Azure-Suchdienst mit PowerShell
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST-API](search-get-started-management-api.md)

Dieses Thema beschreibt die PowerShell-Befehlen zahlreiche Verwaltungsaufgaben für Azure Search Services ausführen. Wir gehen über einen Suchdienst erstellen, Skalierung und der API-Schlüssel verwalten.
Diese Befehle entsprechen Verwaltungsoptionen in [Azure Search Management REST API](http://msdn.microsoft.com/library/dn832684.aspx)verfügbar.

## <a name="prerequisites"></a>Erforderliche Komponenten
 
- Azure PowerShell 1.0 oder höher erforderlich. Eine Anleitung finden Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
- Sie müssen zu Ihrem Azure-Abonnement in PowerShell wie unten beschrieben angemeldet sein.

Zunächst muss bei Azure mit diesem Befehl:

    Login-AzureRmAccount

Geben Sie die e-Mail-Adresse der Azure-Konto und das Kennwort im Microsoft Azure-Login-Dialogfeld.

Alternativ können Sie [sich nicht interaktiv mit Dienstprinzipal](../resource-group-authenticate-service-principal.md).

Haben Sie mehrere Azure-Abonnements müssen Sie Ihre Azure-Abonnement festgelegt. Führen Sie diesen Befehl, um eine Liste Ihrer aktuellen Abonnements anzuzeigen.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Führen Sie den folgenden Befehl, um die Subskription anzugeben. Im folgenden Beispiel wird der Namen `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Befehle zu beginnen

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Nächste Schritte
    
Der Dienst erstellt, Sie können die nächsten Schritte: Erstellen eines [Index](search-what-is-an-index.md), [Abfrage einen Index](search-query-overview.md)erstellen und verwalten Ihre eigenen Suchanwendung, Azure Search verwendet.

- [Erstellen Sie ein Azure-Suchindex in Azure-portal](search-create-index-portal.md)

- [Fragen Sie ein Azure-Suchindex Search Explorer im Azure-Portal mit ab](search-explorer.md)

- [Einrichten eines Indexers zum Laden von Daten von anderen Diensten](search-indexer-overview.md)

- [Verwendung von Azure Search in .NET](search-howto-dotnet-sdk.md)

- [Analysieren Sie Ihre Azure-Suchanfragen](search-traffic-analytics.md)
