<properties
    pageTitle="Erstellen einer Azure VM mit PowerShell | Microsoft Azure"
    description="Mit der Azure PowerShell und Azure Resource Manager einfach einen neuen virtuellen Computer mit Windows Server erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Erstellen einer Windows-VM Ressourcenmanager mit PowerShell

Dieser Artikel veranschaulicht das Erstellen einer Azure Virtual Machine unter Windows Server und erforderlichen [Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md) mit PowerShell Ressourcen. 

Die Schritte in diesem Artikel müssen einen virtuellen Computer erstellen und dauert ca. 30 Minuten der Schritte. Namen, die für Ihre Umgebung sinnvoll ersetzen Sie Beispiel Parameterwerte in die Befehle.

## <a name="step-1-install-azure-powershell"></a>Schritt 1: Installieren von Azure PowerShell

Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen und an Ihrem Konto anmelden.
        
## <a name="step-2-create-a-resource-group"></a>Schritt 2: Erstellen einer Ressourcengruppe

Alle Ressourcen in einer Ressourcengruppe enthalten sein müssen, so können diesen zuerst erstellen.  

1. Enthält eine Liste der verfügbaren Speicherorte, in denen Ressourcen erstellt werden können.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Festlegen des Speicherorts für die Ressourcen. Dieser Befehl wird den Speicherort **Centralus**.

    ```powershell
    $location = "centralus"
    ```
    
3. Erstellen Sie eine Ressourcengruppe. Dieser Befehl erstellt die Ressourcengruppe mit dem Namen **MyResourceGroup** an, die Sie festlegen.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Schritt 3: Erstellen Sie ein Speicherkonto

Ein [Speicherkonto](../storage/storage-introduction.md) musste die virtuelle Festplatte speichern, die vom virtuellen Computer verwendet wird, die Sie erstellen. Speicher-Kontonamen müssen zwischen 3 und 24 Zeichen lang sein und darf Zahlen und nur Kleinbuchstaben enthalten.

1. Testen Sie Name des Speicher für Eindeutigkeit. Dieser Befehl testet den Namen **MyStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Wenn dieser Befehl **True**zurückgibt, ist der vorgeschlagene Name eindeutig in Azure. 
    
2. Erstellen Sie jetzt das Speicherkonto.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Schritt 4: Erstellen eines virtuellen Netzwerks

Alle virtuellen Maschinen sind Teil eines [virtuellen Netzwerks](../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie ein Subnetz für das virtuelle Netzwerk. Dieser Befehl erstellt ein Subnetz mit dem Namen **MySubnet** mit einem Adresspräfix 10.0.0.0/24.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Erstellen Sie jetzt das virtuelle Netzwerk. Dieser Befehl erstellt ein virtuelles Netzwerk mit dem Namen **MyVnet** über das Subnetz, das Sie erstellt haben und ein Adresspräfix **10.0.0.0/16**.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Schritt 5: Erstellen Sie eine öffentliche IP-Adresse und Schnittstelle

Um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk benötigen Sie eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle.

1. Die öffentliche IP-Adresse zu erstellen. Dieser Befehl erstellt eine öffentliche IP-Adresse mit dem Namen **MyPublicIp** und eine Zuordnungsmethode **dynamisch**.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. Erstellen Sie die Netzwerkschnittstelle. Dieser Befehl erstellt eine Schnittstelle mit dem Namen **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Schritt 6: Erstellen einer virtuellen Maschine

Jetzt haben Sie alle Teile an, ist es Zeit, den virtuellen Computer erstellt.

1. Führen Sie diesen Befehl Administrator-Kontoname und Kennwort für den virtuellen Computer festgelegt.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Das Kennwort muss 12 123 Zeichen lang sein und mindestens einen Kleinbuchstaben, einen Großbuchstaben eine Zahl und ein Sonderzeichen. 
        
2. Das Konfigurationsobjekt für den virtuellen Computer zu erstellen. Dieser Befehl erstellt ein Configuration-Objekt mit dem Namen **MyVmConfig** , die den Namen des virtuellen Computers und der Größe der VM definiert.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Finden Sie eine Liste der verfügbaren Größen für eine virtuelle Maschine [Größen für virtuelle Computer in Azure](virtual-machines-windows-sizes.md) .
    
3. Konfigurieren Sie Betriebssystem für den virtuellen Computer. Mit diesem Befehl wird der Computername, Betriebssystem und Anmeldeinformationen für die VM.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Definieren Sie das Bild mit den virtuellen Computer bereitstellen. Dieser Befehl definiert den Windows Server-Abbilds für die VM verwenden. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Weitere Informationen zum Auswählen von Bildern finden Sie unter [Navigieren und wählen Windows virtuellen Computerimages in Azure PowerShell oder CLI](virtual-machines-windows-cli-ps-findimage.md).
        
5. Die Konfiguration fügen Sie die Netzwerkschnittstelle erstellen hinzu.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. Definieren Sie den Namen und Speicherort der VM-Festplatte. Die virtuelle Festplatte wird in einem Container gespeichert. Dieser Befehl erstellt die Diskette in einem Container mit dem Namen **vhds/WindowsVMosDisk.vhd** in das Speicherkonto, das Sie erstellt haben.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Die VM-Konfiguration die Datenträgerinformationen Betriebssystem hinzufügen. Ersetzen Sie den Wert des **$diskName** mit einem Namen für die Betriebssystem-CD. Die Variable erstellen und die Informationen zur Konfiguration hinzufügen.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Erstellen Sie den virtuellen Computer.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Nächste Schritte

- Es gab Probleme mit der Bereitstellung, wäre Nächstes sich [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-portal](../resource-manager-troubleshoot-deployments-portal.md)
- Erfahren Sie, wie der virtuelle Computer verwalten, die anhand der [Azure-Ressourcen-Manager mit PowerShell verwalten virtuelle Computer](virtual-machines-windows-ps-manage.md)erstellt.
- Nutzen Sie mit einer Vorlage zum Erstellen eines virtuellen Computers anhand der Informationen in [einem Windows-Computer mit einer Ressourcen-Manager-Vorlage erstellen](virtual-machines-windows-ps-template.md)
