<properties
    pageTitle="VM aus einer allgemeinen VHD erstellen | Microsoft Azure"
    description="Informationen Sie zu einem Windows-Computer aus ein generalisiertes VHD-Abbild mithilfe von Azure PowerShell in der Ressourcen-Manager-Bereitstellungsmodell."
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
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Einen virtueller Computer aus ein generalisiertes VHD-Abbild erstellen

Ein generalisiertes VHD-Abbild hat alle Ihre persönlichen Kontoinformationen mit [Sysprep](virtual-machines-windows-generalize-vhd.md)entfernt. Sie können eine verallgemeinerte virtuelle Festplatte durch Ausführen von Sysprep auf eine lokale VM dann [VHD in Azure hochladen](virtual-machines-windows-upload-image.md), oder durch Ausführen von Sysprep auf einem vorhandenen Azure VM und dann [kopieren die VHD-Datei](virtual-machines-windows-vhd-copy.md)erstellen.

Wenn Sie einen virtuellen Computer aus einer speziellen virtuellen Festplatte erstellen möchten, finden Sie unter [Erstellen einer VM aus einer speziellen virtuellen](virtual-machines-windows-create-vm-specialized.md).

Am schnellsten eine VM aus einer allgemeinen VHD erstellen [Schnellstart Vorlage] verwendet wird (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie beabsichtigen, verwenden Sie eine virtuelle Festplatte Hochladen einer VM vor Ort wie einem Hyper-V, stellen Sie sicher, folgen die Schritte unter [Vorbereiten einer virtuellen Windows Azure hochladen](virtual-machines-windows-prepare-for-upload-vhd-image.md). 

Hochgeladene VHDs und vorhandene Azure VM VHDs müssen verallgemeinert werden, bevor Sie einen virtuellen Computer erstellen können mit dieser Methode. Weitere Informationen finden Sie unter [verallgemeinern einem Windows-Computer mithilfe von Sysprep](virtual-machines-windows-generalize-vhd.md). 


## <a name="set-the-uri-of-the-vhd"></a>Den URI der virtuellen Festplatte festgelegt.

Der URI für die virtuelle Festplatte verwendet wird im Format: https://**Mystorageaccount**.blob.core.windows.net/**Mycontainer**/**MyVhdName**VHD. In diesem Beispiel die VHD-Datei mit dem Namen **MyVHD** wird Speicher Konto **Mystorageaccount** in Container **Mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie vNet und Subnetz [virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md).


1. Erstellen Sie das Subnetz. Im folgende Beispiel erstellt ein Subnetz mit dem Namen **MySubnet** in Ressource Gruppe **MyResourceGroup** mit der Adresse des **10.0.0.0/24**.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. Erstellen Sie virtuelle Netzwerk. Das folgende Beispiel erstellt ein virtuelles Netzwerk mit dem Namen **MyVnet** an **Westen der USA** mit der Adresse **10.0.0.0/16**.  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Erstellen Sie eine öffentliche IP-Adresse und Schnittstelle

Um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk benötigen Sie eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle.

1. Erstellen Sie eine öffentliche IP-Adresse. Dieses Beispiel erstellt eine öffentliche IP-Adresse mit dem Namen **MyPip**. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Erstellen Sie die Netzwerkkarte. Dieses Beispiel erstellt eine Netzwerkkarte mit dem Namen **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Erstellen Sie Netzwerk-Sicherheitsgruppe und eine RDP-Regel

Um die VM mit RDP anmelden können, müssen Sie eine Sicherheitsregel verfügen über RDP Zugriff auf Port 3389. 

Dieses Beispiel erstellt eine NSG namens **MyNsg** , die enthält eine Regel mit der Bezeichnung **MyRdpRule** , die RDP-Datenverkehr über Port 3389 zulässt. Weitere Informationen zu NSGs finden Sie unter [Öffnen von Ports einer VM in Azure mithilfe von PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Erstellen Sie eine Variable für das virtuelle Netzwerk

Erstellen Sie eine Variable für das virtuelle Netzwerk abgeschlossen. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>Erstellen Sie den virtuellen Computer

PowerShell-Skript veranschaulicht die Konfigurationen virtueller Computer einrichten und verwenden das hochgeladene VM-Image als Quelle für die neue Installation.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Stellen Sie sicher, dass die VM erstellt wurde 

Nach Abschluss des Vorgangs sollte die neu erstellte VM in [Azure-Portal](https://portal.azure.com) unter **Durchsuchen**angezeigt > **virtuellen Computern**oder mit dem folgenden PowerShell-Befehlen:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte

Um den neuen virtuellen Computer mit Azure PowerShell verwalten anzeigen Sie [Verwalten virtuelle Maschinen mit Azure Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md)


