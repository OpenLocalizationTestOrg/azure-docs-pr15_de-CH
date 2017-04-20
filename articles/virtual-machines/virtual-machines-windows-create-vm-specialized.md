<properties
    pageTitle="Erstellen Sie eine Kopie Ihrer Windows-VM | Microsoft Azure"
    description="Erfahren Sie, wie eine spezielle Ressourcen-Manager-Bereitstellungsmodell aus Windows Azure-VM erstellt."
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
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Einen virtueller Computer aus einer speziellen virtuellen erstellen

Erstellen einer neuen VM eine spezielle virtuelle Festplatte als mit Powershell Betriebssystemdatenträger anfügen. Eine spezielle virtuelle Festplatte verwaltet Benutzerkonten, Applikationen und anderen Zustandsdaten aus Ihrer ursprünglichen VM. 

Wenn Sie einen virtuellen Computer aus eine verallgemeinerte virtuelle Festplatte erstellen möchten, finden Sie unter [Erstellen einer VM ein generalisiertes VHD-Abbild](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Erstellen Sie die Subnetzmaske und vNet

Erstellen Sie vNet und Subnetz [virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie das Subnetz. Dieses Beispiel erstellt ein Subnetz mit dem Namen **MySubNet**in Ressource Gruppe **MyResourceGroup**und Adresse Subnetzpräfix auf **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Erstellen des Vnets. In diesem Beispiel wird den virtuellen Netzwerknamen **MyVnetName**, den Speicherort für **Westen der USA**und das Adresspräfix für das virtuelle Netzwerk zu **10.0.0.0/16**. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Erstellen Sie eine öffentliche IP-Adresse und NIC

Um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk benötigen Sie eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle.

1. Die öffentliche IP-Adresse zu erstellen. In diesem Beispiel wird die öffentliche IP-Adressnamen **MyIP**festgelegt.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Erstellen Sie die Netzwerkkarte. In diesem Beispiel wird der Namen **MyNicName**festgelegt.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Erstellen Sie Netzwerk-Sicherheitsgruppe und eine RDP-Regel

Um die VM mit RDP anmelden können, müssen Sie eine Sicherheitsregel verfügen über RDP Zugriff auf Port 3389. Da die virtuelle Festplatte für den neuen virtuellen Computer aus einer vorhandenen erstellt wurde können spezielle VM, nachdem die VM erstellt wurde ein vorhandenes Konto aus virtuellen Quellcomputers, die über RDP anmelden.

In diesem Beispiel wird der Name NSG RDP Regelname, **MyRdpRule**und **MyNsg** .

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Weitere Informationen über Endpunkte und NSG finden Sie unter [Öffnen von Ports einer VM in Azure mithilfe von PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>Die VM-Konfiguration erstellen

Die VM-Konfiguration für den kopierten virtuellen Festplatte als virtuelle Festplatte Betriebssystem eingerichtet.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Haben Sie Datenträger, die für die VM zugeordnet werden müssen, sollten Sie auch Folgendes hinzufügen: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

Daten und Betriebssystem-Datenträger-URLs lauten wie folgt: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Finden Sie diese im Portal in den Zielcontainer Speicher durchsuchen, indem Betriebssystem oder virtuelle Festplatte kopierte Daten und kopieren den Inhalt der URL.


## <a name="create-the-vm"></a>Erstellen Sie den virtuellen Computer

Erstellen Sie die soeben erstellte Konfigurationen mit VM.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Wenn dieser Befehl erfolgreich ausgeführt wurde, sehen Sie die Ausgabe wie folgt:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Stellen Sie sicher, dass die VM erstellt wurde 
 
Müsste der neu erstellten VM in [Azure-Portal](https://portal.azure.com)unter **Durchsuchen** > **virtuellen Computern**oder mit dem folgenden PowerShell-Befehlen:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte

Den neuen virtuellen Computer anmelden, der VM im [Portal](https://portal.azure.com)durchsuchen, klicken Sie auf **Verbinden**und RDP Remote Desktop öffnen. Verwenden Sie die Anmeldeinformationen des ursprünglichen virtuellen Computers auf den neuen virtuellen Computer anmelden. Weitere Informationen finden Sie unter [Verbinden und eine unter Windows Azure virtuellen Computer anmelden](virtual-machines-windows-connect-logon.md).







