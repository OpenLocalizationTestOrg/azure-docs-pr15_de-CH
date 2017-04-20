<properties
   pageTitle="Erstellen Sie eine Windows-VM mit mehreren NICs | Microsoft Azure"
   description="Informationen Sie zum Erstellen von Windows-VM mit mehreren NICs Azure PowerShell oder Ressourcen-Manager Vorlagen zugeordnet."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Erstellen eine Windows-VM mit mehreren NICs
Eine virtuellen Maschine (VM) kann in Azure erstellen, die mehrere virtuelle Netzwerkkarten (NICs) zugeordnet. Ein häufiges Szenario wäre Subnetzen für Front-End- und Back-End-Verbindung oder ein Netzwerk zu überwachen oder backup-Lösung. Dieser Artikel bietet Schnellzugriff erstellen einen virtuellen Computer mit mehreren NICs angefügt. Ausführliche Informationen, wie Sie mehrere Netzwerkkarten mit eigenen PowerShell-Skripts erstellen mehr zum [Bereitstellen von Multi-NIC VMs](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Verschiedene [Größen VM](virtual-machines-windows-sizes.md) unterstützen eine unterschiedliche Anzahl von Netzwerkkarten, so entsprechend Größe Ihrer VM.

>[AZURE.WARNING] Beim Erstellen einer VM - Sie können eine vorhandene VM Netzwerkkarten hinzufügen, müssen Sie mehrere NICs anfügen. Erstellen [eine VM basierend auf den ursprünglichen virtuellen Laufwerken](virtual-machines-windows-vhd-copy.md) und mehrere Netzwerkkarten VM bereitstellen.

## <a name="create-core-resources"></a>Core-Ressourcen erstellen
Stellen Sie sicher, dass Sie die [neuesten Azure PowerShell installiert und konfiguriert](../powershell-install-configure.md). Melden Sie sich bei Ihrem Azure-Konto:

```powershell
Login-AzureRmAccount
```

Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myVM`.

Erstellen Sie zunächst eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUs` Speicherort:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Erstellen Sie ein Speicherkonto Ihrer virtuellen Computer enthält. Im folgenden Beispiel wird ein Speicherkonto namens `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Virtuelles Netzwerk und Subnetze erstellen
Definieren Sie zwei Subnetzen virtuelles Netzwerk - Datenverkehr Front-End-und Back-End-Datenverkehr. Das folgende Beispiel definiert zwei Subnetzen mit dem Namen `mySubnetFrontEnd` und `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Erstellen Sie virtuelle Netzwerke und Subnets. Das folgende Beispiel erstellt ein virtuelles Netzwerk mit dem Namen `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Erstellen Sie mehrere Netzwerkkarten
Erstellen Sie zwei Netzwerkkarten im Back-End-Subnetz eine Netzwerkkarte mit dem Front-End-Subnetz und eine Netzwerkkarte zuweisen. Das folgende Beispiel erstellt zwei Netzwerkkarten mit dem Namen `myNic1` und `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Normalerweise erstellen Sie auch eine [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) oder [Lastenausgleich](../load-balancer/load-balancer-overview.md) , verwalten und Verteilen von Datenverkehr über die VMs. [Weitere Multi-NIC VM](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) Artikel leitet Sie durch eine Netzwerk-Sicherheitsgruppe erstellen und Zuweisen von Netzwerkkarten.


## <a name="create-the-virtual-machine"></a>Erstellen der virtuellen Computer
Jetzt die VM-Konfiguration erstellen. Jede VM Größe hat ein Limit für die Gesamtzahl der Netzwerkkarten eine VM hinzugefügt werden können. Weitere Informationen über [Windows-VM-Größen](virtual-machines-windows-sizes.md). 

Legen Sie zunächst die VM-Anmeldeinformationen auf dem `$cred` Variable wie folgt:

```powershell
$cred = Get-Credential
```

Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM` und eine VM-Größe, die bis zu zwei Netzwerkkarten unterstützt (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Die restlichen VS-Konfiguration zu erstellen. Im folgenden Beispiel wird ein Windows Server 2012 R2 VM:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Verbinden Sie die beiden Karten, die Sie zuvor erstellt haben:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

Konfigurieren der Speicher- und virtuellen Datenträger für Ihren neuen virtuellen Computer:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Erstellen Sie einen virtuellen Computer:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Mehrere Netzwerkkarten mit Ressourcen-Manager Vorlagen erstellen
Azure Schablonen Ressourcenmanager declarative JSON-Dateien Ihrer Umgebung definieren. Lesen Sie eine [Übersicht der Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md). Ressourcen-Manager Vorlagen bieten eine Möglichkeit, mehrere Instanzen einer Ressource während der Bereitstellung wie erstellen mehrere Netzwerkkarten erstellen. *Kopie* verwenden an die Anzahl der Instanzen zu:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Weitere Informationen zum [Erstellen mehrerer Instanzen *Kopie*](../resource-group-create-multiple.md). 

Können Sie auch eine `copyIndex()` dann einen Ressourcennamen angefügt erstellen ermöglicht eine Reihe `myNic1`, `MyNic2`usw.. Die folgenden zeigt ein Beispiel den Indexwert anzufügen:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Lesen Sie ein vollständiges Beispiel für [mehrere Netzwerkkarten mit Ressourcen-Manager Vorlagen erstellen](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie [Windows VM Größen](virtual-machines-windows-sizes.md) beim Erstellen eines virtuellen Computers mit mehreren Netzwerkkarten. Achten Sie auf maximale Anzahl Netzwerkkarten jede VM-Größe unterstützt. 

Beachten Sie, dass die vorhandenen VM keine zusätzliche Netzwerkkarten hinzufügen, müssen Sie alle Netzwerkkarten erstellen, wenn Sie den virtuellen Computer bereitstellen. Achten Sie bei der Planung der Bereitstellung sicherstellen, dass die erforderliche Netzwerkkonnektivität von Anfang an haben.