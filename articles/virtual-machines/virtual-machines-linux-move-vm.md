<properties
    pageTitle="Verschieben einer Linux VM | Microsoft Azure"
    description="Verschieben einer Linux VM zu einer anderen Azure-Abonnement oder Ressourcen Gruppe Ressourcen-Manager-Bereitstellungsmodell."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Verschieben Sie Linux VM ein anderes Abonnement oder Ressource Gruppe

Dieser Artikel führt Sie durch Verschieben von Linux VM Ressourcengruppen oder Abonnements. Verschieben einer VM zwischen Abonnements kann nützlich sein, wenn einen virtueller Computer in einem persönlichen Abonnement erstellt und jetzt zu Ihrem Unternehmen Abonnement verschieben möchten.

> [AZURE.NOTE] Neue Ressourcen-IDs werden als Teil der Verschiebung erstellt. Sobald die VM verschoben wurde, müssen Sie Ihre Tools und Skripts verwenden, die neue Ressourcen-IDs aktualisieren. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Verwenden der Azure-CLI einen virtuellen Computer verschieben 

Um erfolgreich einen virtuellen Computer verschieben, müssen Sie die VM und seine unterstützenden Ressourcen verschieben. Befehl **Azure Gruppe** alle Ressourcen in einer Ressourcengruppe und ihrer IDs aufgeführt. Sie können die Ausgabe dieses Befehls in eine Datei kopieren und später Befehle IDs einfügen.

    azure group show <resourceGroupName>

Um einen virtuellen Computer und seine Ressourcen in einer anderen Ressourcengruppe verschieben, Befehl **Azure Ressource verschieben** CLI. Im folgenden Beispiel wird veranschaulicht, wie ein virtueller Computer und die am häufigsten verwendeten Ressourcen erfordert. Wir verwenden den Parameter **-i** und eine durch Kommas getrennte Liste (ohne Leerzeichen) von IDs für die Ressourcen zu übergeben.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Fügen Sie ggf. den virtuellen Computer und seine Ressourcen zu einem anderen Abonnement Verschieben der **-Ziel SubscriptionId & #60; DestinationSubscriptionID & #62;** Parameter, um das Zielabonnement.

Wenn Sie über die Befehlszeile unter Windows arbeiten, müssen Hinzufügen einer **$** vor den Variablennamen beim Deklarieren sie. Dies ist nicht erforderlich, Linux.

Sie werden aufgefordert zu bestätigen, dass die angegebene Ressource verschieben möchten. Geben Sie **J** ein, um sicherzustellen, dass Sie die Ressourcen verschieben möchten.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können viele verschiedene Arten von Ressourcen zwischen Ressourcengruppen und Abonnements verschieben. Weitere Informationen finden Sie unter [Ressourcen neue Ressourcengruppe oder Abonnement](../resource-group-move-resources.md).    