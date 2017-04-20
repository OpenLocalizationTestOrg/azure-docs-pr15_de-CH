<properties
    pageTitle="Erstellen einer virtuellen Maschine Skalieren mit PowerShell | Microsoft Azure"
    description="Erstellen einer virtuellen Maschine Skalieren mit PowerShell"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Erstellen Sie einen Windows Virtual Machine Maßstab mit Azure PowerShell

Diese Schritte eines fill-in-the-Leerzeichen Ansatzes zum Erstellen einer Azure Virtual Machine skalieren. Finden Sie [Virtual Machine Maßstab legt Übersicht](virtual-machine-scale-sets-overview.md) Maßstab legt erfahren.

Etwa 30 Minuten sollte dauert, führen Sie die Schritte in diesem Artikel.

## <a name="step-1-install-azure-powershell"></a>Schritt 1: Installieren von Azure PowerShell

Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen und an Ihrem Konto anmelden.

## <a name="step-2-create-resources"></a>Schritt 2: Erstellen von Ressourcen

Erstellen der Ressourcen für Ihre neue skalieren.

### <a name="resource-group"></a>Ressourcengruppe

Eine virtuelle Maschine Skalierungsgruppe muss in einer Ressourcengruppe enthalten sein.

1. Abrufen einer Liste der verfügbaren Speicherorte Ressourcen platzieren:

        Get-AzureLocation | Sort Name | Select Name

2. Wählen Sie einen Speicherort, der für Sie am besten, ersetzen Sie den Wert des **$locName** mit Namen, Speicherort und erstellen Sie die Variable:

        $locName = "location name from the list, such as Central US"

3. Ersetzen Sie den Wert des **$rgName** mit dem Namen für die neue Ressourcengruppe und erstellen Sie dann die Variable: 

        $rgName = "resource group name"
        
4. Erstellen Sie die Ressourcengruppe:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Wie in diesem Beispiel sollte angezeigt werden:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Konto

Ein Speicherkonto wird zum Speichern der Betriebssystem-CD und Diagnosedaten zur Skalierung von einem virtuellen Computer verwendet. Nach Möglichkeit ist es empfehlenswert, ein Speicherkonto für jeden virtuellen Computer in einer Skala erstellt haben. Wenn dies nicht möglich, Plan für nicht mehr als 20 VMs pro Konto. Das Beispiel in diesem Artikel zeigt drei Speicherkonten für drei virtuelle Computer.

1. Ersetzen Sie den Wert des **$saName** mit einem Namen für das Speicherkonto. Testname Eindeutigkeit. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    **Stimmt die Antwort**ist der vorgeschlagene Name eindeutig.

3. Ersetzen Sie den Wert des **$saType** mit dem Speicher-Konto und erstellen Sie die Variable:  

        $saType = "storage account type"
        
    Mögliche Werte sind: Standard_LRS, Standard_GRS, Standard_RAGRS oder Premium_LRS.
        
4. Erstellen Sie das Konto:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Wie in diesem Beispiel sollte angezeigt werden:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Wiederholen Sie die Schritte 1 bis 4, um drei Speicherkonten, z. B. myst1, myst2 und myst3 erstellen.

### <a name="virtual-network"></a>Virtuelles Netzwerk

Ein virtuelles Netzwerk ist erforderlich für die virtuellen Computer in der Skalierungsgruppe.

1. Ersetzen Sie den Wert des **$subnetName** mit dem Namen für das Subnet in das virtuelle Netzwerk und erstellen Sie die Variable: 

        $subnetName = "subnet name"
        
2. Erstellen Sie die Subnetzkonfiguration:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Das Adresspräfix abweichen des virtuellen Netzwerks.

3. Ersetzen Sie den Wert des **$netName** mit dem Namen für das virtuelle Netzwerk und erstellen Sie dann die Variable: 

        $netName = "virtual network name"
        
4. Das virtuelle Netzwerk zu erstellen:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Konfiguration der eingestellten Skalierung

Sie haben die Ressourcen, die Sie für die Skalierung der Konfiguration festlegen, also erstellen.  

1. Ersetzen Sie den Wert des **$ipName** mit dem Namen für die IP-Konfiguration und erstellen Sie dann die Variable: 

        $ipName = "IP configuration name"
        
2. Erstellen Sie die IP-Konfiguration:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Ersetzen Sie den Wert des **$vmssConfig** mit dem Namen Konfiguration definieren skalieren und anschließend die Variable:   

        $vmssConfig = "Scale set configuration name"
        
3. Erstellen Sie die Konfiguration für die Skalierung festlegen:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    Dieses Beispiel zeigt eine Skala mit drei virtuelle Computer. Finden Sie unter [Virtual Machine Maßstab legt Überblick](virtual-machine-scale-sets-overview.md) über die Kapazität der Maßstab legt. Dieser Schritt umfasst auch Größe (SkuName genannt) der virtuellen Computer in der Gruppe festgelegt. Betrachten Sie [Größen für virtuelle Computer](../virtual-machines/virtual-machines-windows-sizes.md)eine Größe zu finden, die Ihren Bedürfnissen entspricht.
    
4. Die Skalierung setzt die Konfiguration die Konfiguration der Netzwerkschnittstelle hinzufügen:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Wie in diesem Beispiel sollte angezeigt werden:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>-Profil

1. Ersetzen Sie den Wert des **$computerName** mit dem Präfix des Computernamens, das Sie verwenden möchten und erstellen Sie dann die Variable: 

        $computerName = "computer name prefix"
        
2. Der Wert des **$adminName** den Namen des Administratorkontos auf dem virtuellen Computer, und erstellen Sie die Variable:

        $adminName = "administrator account name"
        
3. Ersetzen Sie den Wert des **$adminPassword** mit dem Kennwort und erstellen Sie der Variablen:

        $adminPassword = "password for administrator accounts"
        
4. Erstellen Sie-Profil:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Speicherprofil

1. Ersetzen Sie den Wert des **$storageProfile** mit dem Namen Storage-Profil und erstellen Sie dann die Variable:  

        $storageProfile = "storage profile name"
        
2. Erstellen Sie die Variablen, die das zu verwendende Bild definieren:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Um Informationen über andere Bilder suchen Sie [Wählen Azure virtuellen Computerimages Azure-CLI mit Windows PowerShell](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md)zu navigieren.
        
3. Ersetzen Sie den Wert des **$vhdContainers** mit einer Liste mit den Pfaden, virtuellen Festplatten, wie "https://mystorage.blob.core.windows.net/vhds gespeichert sind", und erstellen Sie die Variable:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Erstellen Sie Speicherprofil:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Virtual Machine Skalierung festlegen

Schließlich können Sie die Skalierungsgruppe erstellen.

1. Ersetzen Sie den Wert des **$vmssName** mit dem Namen des virtuellen Computers Skalierung festlegen und erstellen Sie die Variable:

        $vmssName = "scale set name"
        
2. Erstellen Sie die Skalierung:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Etwa wie im folgenden Beispiel sollte angezeigt werden, die eine erfolgreiche Bereitstellung anzeigt:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Schritt 3: Ressourcen

Verwenden Sie diese Ressourcen zu virtuellen Skalierung festlegen, die Sie erstellt haben:

- Azure-Portal - steht eine begrenzte Menge an Informationen über das Portal.
- [Azure-Ressourcen-Explorer](https://resources.azure.com/) - dieses Tool ist am besten zu den aktuellen Zustand der Skalierung festlegen.
- Azure PowerShell - verwenden Sie diesen Befehl, um Informationen zu erhalten:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Nächste Schritte

- Verwalten der Skalierung, gerade erstellt anhand der Informationen in der [Verwaltung virtueller Computer in einem virtuellen Computer skalieren](virtual-machine-scale-sets-windows-manage.md)
- Erwägen Sie, automatische Skalierung der Waage mit Informationen in [Automatische Skalierung und Virtual Machine Skala wird](virtual-machine-scale-sets-autoscale-overview.md) festgelegt
- Erfahren Sie mehr über vertikale Skalierung [vertikale Skalieren mit virtuellen Skalierung](virtual-machine-scale-sets-vertical-scale-reprovision.md) überprüfen
