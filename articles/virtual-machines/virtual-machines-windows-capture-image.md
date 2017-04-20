<properties
    pageTitle="Ein VM Bild von allgemeinen Azure VM | Microsoft Azure"
    description="Erfahren Sie, wie ein VM Bild aus einer allgemeinen Azure VM in der Ressourcen-Manager-Bereitstellungsmodell erstellt"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>VM-Image aus einer allgemeinen Azure VM erfassen


Dieser Artikel veranschaulicht das Azure PowerShell verwenden, um ein Bild für eine verallgemeinerte Azure VM erstellen. Das Bild können Sie einen anderen virtuellen Computer erstellen. Das Bild enthält die Betriebssystem-Datenträger und Datenträger, die den virtuellen Computer zugeordnet sind. Das Bild enthalten nicht virtuelle Netzwerkressourcen, müssen Sie Ressourcen einrichten, wenn Sie den neuen virtuellen Computer erstellen. 


## <a name="prerequisites"></a>Erforderliche Komponenten

- Sie müssen bereits [die VM verallgemeinert](virtual-machines-windows-generalize-vhd.md). Verallgemeinern einer VM entfernt alle Ihre persönlichen Kontoinformationen, unter anderem und bereitet den Computer als ein Bild verwendet werden soll.

- Sie benötigen Azure PowerShell Version 1.0.x oder höher installiert. Wenn Sie PowerShell bereits installiert haben, lesen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Installationsschritte.


## <a name="log-in-to-azure-powershell"></a>Melden Sie sich bei Azure PowerShell

1. Azure PowerShell öffnen und Ihre Azure-Konto anmelden.

    ```powershell
    Login-AzureRmAccount
    ```

    Popup-Fenster für Ihre Azure-Konto-Anmeldeinformationen eingeben.

2. Erhalten Sie das Abonnement-IDs für Abonnements verfügbar.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Legen Sie das richtige Abonnement mit der Abonnement-ID.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Die VM freigegeben und den Zustand um verallgemeinert       

1. Freigeben Sie VM-Ressourcen.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    Der *Status* der VM in Azure-Portal wird von **beendet** zu **beendet (freigegeben)**.

2. Den Status des virtuellen Computers auf **Generalized**festgelegt. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Überprüfen Sie den Status des virtuellen Computers. Abschnitt **OSState/verallgemeinert** der VM müssen der **DisplayStatus** **VM generalized**festgelegt.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Erstellen des Bilds 

1. Mit diesem Befehl Ziel-Behälter Abbild des virtuellen PCs soll. Das Bild wird in demselben Speicherkonto als der ursprüngliche virtuelle Computer erstellt. Die `-Path` Parameter der JSON-Vorlage lokal speichert. Die `-DestinationContainerName` Parameter ist der Name des Containers zu Bilder enthalten. Wenn der Container vorhanden ist, wird sie für Sie erstellt.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    Die URL des Bildes erhalten von JSON-Vorlagendatei. **Ressourcen**zur > **StorageProfile** > **OsDisk** > **Bild** > **Uri** -Abschnitt für den vollständigen Pfad des Bilds. Die URL des Bildes sieht folgendermaßen aus: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    Sie können auch den URI im Portal überprüfen. Das Bild wird in einem Container mit dem Namen Ihres Speicherkontos **System** kopiert. 


## <a name="next-steps"></a>Nächste Schritte

- Jetzt können Sie [einen virtuellen Computer aus dem Bild erstellen](virtual-machines-windows-create-vm-generalized.md).

