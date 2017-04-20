<properties
    pageTitle="Verschieben von Fenstern VM | Microsoft Azure"
    description="Verschieben einer Windows-VM zu einer anderen Azure-Abonnement oder Ressourcen Gruppe Ressourcen-Manager-Bereitstellungsmodell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Verschieben einer Windows-VM in anderen Azure-Abonnement oder eine Ressourcengruppe aus 

Dieser Artikel führt Sie durch eine Windows-VM Ressourcengruppen oder Abonnements verschieben. Verschieben zwischen Abonnements kann nützlich sein, wenn Sie einen virtuellen Computer in einem persönlichen Abonnement ursprünglich und jetzt Ihr Unternehmen Abonnement weiterhin Ihre Arbeit verschieben möchten.

> [AZURE.NOTE] Neue Ressourcen-IDs werden als Teil der Verschiebung erstellt werden. Sobald die VM verschoben wurde, müssen Sie die Tools und Skripts verwenden, die neue Ressourcen-IDs aktualisieren. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Mithilfe von Powershell einen virtuellen Computer verschieben

Um einen virtuellen Computer in eine andere Ressourcengruppe verschieben, müssen Sie sicherstellen, dass auch alle abhängigen Ressourcen verschieben. Um das Verschieben-AzureRMResource-Cmdlet verwenden, benötigen Sie den Ressourcennamen und den Typ der Ressource. Sie erhalten von Cmdlets finden AzureRMResource.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Um einen virtuellen Computer verschieben müssen mehrere Ressourcen. Wir erstellen Sie zwei separate Variablen für jede Ressource und Listen Sie diese. In diesem Beispiel enthält die meisten grundlegenden Ressourcen für eine VM können mehr erforderlich.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Um die Ressourcen zu unterschiedlichen Abonnement verschieben schließen Sie den **DestinationSubscriptionId -** Parameter ein 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Sie werden aufgefordert zu bestätigen, dass die angegebenen Ressourcen verschieben möchten. Geben Sie **J** ein, um sicherzustellen, dass Sie die Ressourcen verschieben möchten.

  
## <a name="next-steps"></a>Nächste Schritte

Sie können viele verschiedene Arten von Ressourcen zwischen Ressourcengruppen und Abonnements verschieben. Weitere Informationen finden Sie unter [Ressourcen neue Ressourcengruppe oder Abonnement](../resource-group-move-resources.md).    