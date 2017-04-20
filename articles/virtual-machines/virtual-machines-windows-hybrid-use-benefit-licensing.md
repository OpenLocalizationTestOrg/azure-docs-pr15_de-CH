<properties
   pageTitle="Azure Hybrid mit Vorteil für Windows Server | Microsoft Azure"
   description="So maximieren Sie Ihre Windows Server Software Assurance Vorteile bringen lokale Lizenzen in Azure"
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
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Azure Hybrid mit Vorteil für WindowsServer

Für Kunden mit Windows Server mit Software Assurance Sie bringen Ihre lokalen Serverlizenzen Windows Azure und Windows Server VMs zu reduzierten Preisen in Azure ausgeführt. Azure hybride Einsatz nutzen können Sie zu Windows Server VMs in Azure erhalten nur die Basis Compute-Rate abgerechnet. Weitere Informationen finden Sie in [Azure Hybrid mit Vorteil Seite Lizenzierung](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Erläutert, wie Windows-VMs in Azure nutzen dieser Lizenz bereitstellen.

> [AZURE.NOTE] Azure Marketplace Bilder können Windows Server VMs mit Azure Hybrid verwenden nutzen bereitstellen. Sie müssen Ihre VMs mit PowerShell oder Ressourcen-Manager Vorlagen korrekt registriert Ihre VMs als Basis Compute Rate Rabatt bereitstellen.

## <a name="pre-requisites"></a>Erforderliche Komponenten
Gibt es eine Reihe von erforderlichen Komponenten um Azure Hybrid mit Vorteil für Windows Server-VMs in Azure nutzen:

- Haben Sie das Azure PowerShell-Modul installiert
- Haben Sie die Windows Server-VHD in Azure-Speicher hochgeladen

### <a name="install-azure-powershell"></a>Azure PowerShell installieren
Stellen Sie sicher, Sie [installiert und konfiguriert die neuesten Azure PowerShell](../powershell-install-configure.md). Auch wenn Sie Ihre VMs Ressourcenmanager Vorlagen bereitstellen möchten, Sie noch Azure PowerShell installiert, um Ihre Windows Server-VHD Hochladen (Siehe den folgenden Schritt).

### <a name="upload-a-windows-server-vhd"></a>WindowsServer virtuelle Festplatte hochladen

Um eine Windows Server-VM in Azure bereitzustellen, müssen Sie eine virtuelle Festplatte erstellen, Ihre Basisbuild von Windows Server enthält. Diese virtuelle Festplatte muss entsprechend durch Sysprep vorbereitet werden in Azure hochladen. Sie können [Weitere Informationen über die VHD Vorschriften und Sysprep-Prozess](./virtual-machines-windows-upload-image.md) und [Sysprep-Unterstützung für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Sichern Sie die VM vor dem Ausführen von Sysprep. Nachdem Sie die virtuelle Festplatte vorbereitet haben, auf Azure Storage-Konto verwenden die virtuelle Festplatte Hochladen der `Add-AzureRmVhd` Cmdlets wie folgt:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server, SharePoint Server und Dynamik auch nutzen Ihre Software Assurance-Lizenzen. Sie müssen weiterhin Windows Server-Abbilds vorbereiten durch Installieren der Komponenten entsprechend Lizenzschlüssel bereitstellen, und das Image in Azure hochladen. Überprüfen Sie die entsprechende Dokumentation mit der Anwendung oder [Hinweise für die Installation von SQL Server mithilfe von Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) [erstellen eine SharePoint Server 2016 Reference (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

Sie können auch mehr zum [Hochladen der virtuellen Festplatte Azure Prozess](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] Dieser Artikel konzentriert sich auf die Bereitstellung von Windows Server-VMs. Sie können auch Windows Client VMs auf dieselbe Weise bereitstellen. In den folgenden Beispielen ersetzen Sie `Server` mit `Client` entsprechend.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>Bereitstellen einer VM über PowerShell Schnellstart
Bereitstellen von Windows Server-VM über PowerShell verfügen Sie über einen zusätzlichen Parameter für `-LicenseType`. Sobald Ihre VHD in Azure hochgeladen haben, erstellen Sie eine neue VM mit `New-AzureRmVM` und geben Sie die Lizenz wie folgt:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

Sie können unten [eine ausführlichere Anleitung Bereitstellen eines virtuellen Computers in Azure über PowerShell gelesen](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) oder beschreibenden Guide auf die verschiedenen Schritte zum [Erstellen einer Windows-VM Ressourcenmanager mit PowerShell](./virtual-machines-windows-ps-create.md)lesen.

## <a name="deploy-a-vm-via-resource-manager"></a>Bereitstellen einer VM über Ressourcen-Manager
In den Ressourcen-Manager-Vorlagen einen zusätzlichen Parameter für `licenseType` angegeben werden. Erfahren Sie mehr über [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md). Haben Sie Ihre VHD in Azure hochgeladen bearbeiten Sie Ressourcenmanager Vorlage Lizenztyp als Teil des Compute-Anbieter und die Dokumentvorlage normal bereitstellen:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Überprüfen Sie, ob die VM Lizenzierungen Vorteil nutzen
Nachdem Sie die VM über die PowerShell oder Ressourcenmanager Deployment-Methode bereitgestellt haben, überprüfen Sie den Lizenz mit `Get-AzureRmVM` wie folgt:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

Die Ausgabe ähnelt der folgenden:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Dies folgenden VM bereitgestellt ohne Azure Hybrid Verwendung nutzen, wie ein virtueller Computer direkt aus der Galerie Azure bereitgestellt:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>Detaillierte PowerShell Exemplarische Vorgehensweise

Die folgenden detaillierten Schritte PowerShell anzeigen vollständige Bereitstellung eines virtuellen Computers Lesen Sie mehr Kontext tatsächliche Cmdlets und anderen Komponenten im [Erstellen einer Windows-VM Ressourcenmanager mit PowerShell](./virtual-machines-windows-ps-create.md). Sie die Ressourcengruppe Speicherkonto und virtuelle Netzwerke erstellen und definieren die VM und die VM erstellen.
 
Zunächst sichere erhalten Sie Anmeldeinformationen zu, legen Sie einen Speicherort und Namen:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Erstellen Sie eine öffentliche IP-Adresse:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Definieren Sie die Subnetzmaske NIC und VNET:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Name der VM und Erstellen einer VM-Konfiguration:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definieren Sie Ihr Betriebssystem:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Fügen Sie Ihre Netzwerkkarte für die VM:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definieren Sie das Speicherkonto verwenden:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Hochladen Sie Ihre VHD entsprechend vorbereitet, und zuordnen Sie der VM verwenden:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Schließlich die VM und definieren Sie die Lizenzierung Azure Hybrid verwenden Vorteile nutzen:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur [Lizenzierung Azure Hybrid Verwendung nutzen](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Weitere Informationen zur [Verwendung von Ressourcen-Manager-Vorlagen](../azure-resource-manager/resource-group-overview.md).
