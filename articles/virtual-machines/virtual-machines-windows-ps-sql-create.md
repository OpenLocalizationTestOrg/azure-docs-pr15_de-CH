<properties
    pageTitle="Erstellen Sie eine SQL Server-VM in Azure PowerShell (Resource Manager) | Microsoft Azure"
    description="Enthält Schritte und PowerShell-Skripts zum Erstellen einer Azure VM mit SQL Server virtuellen Katalog."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Bereitstellen eines SQL Server virtuellen Computers mithilfe von Azure PowerShell (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht die einzelnen Azure mit einen virtuellen Computer mit Azure PowerShell-Cmdlets **Azure-Ressourcen-Manager** -Bereitstellungsmodell erstellen. In diesem Lernprogramm erstellen wir einen einzelnen virtuellen Computer mit einem einzelnen Laufwerk aus einem Bild in der Galerie SQL. Wir erstellen neue Anbieter für Speicher-, Netzwerk- und Serverressourcen, die vom virtuellen Computer verwendet werden. Wenn vorhandenen Anbieter für diese Ressourcen verfügen, können Sie stattdessen diese Anbieter.

Benötigen Sie die klassische Version dieses Themas finden Sie unter [Bereitstellen einer Azure PowerShell Classic mit SQL Server virtuellen Computer](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm benötigen Sie:

- Ein Azure-Konto und Abonnements, bevor Sie beginnen. Haben Sie eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.
- [Azure PowerShell)](../powershell-install-configure.md), mindestens Version 1.4.0 oder höher (dieses Lernprogramm mit Version 1.5.0 geschrieben).
    - Abrufen die Version Geben Sie **Get-Modul Azure ListAvailable**.

## <a name="configure-your-subscription"></a>Konfigurieren Sie Ihr Abonnement

Öffnen Sie Windows PowerShell und richten Sie Kontozugriff Azure ein, indem Sie das folgende Cmdlet ausführen. Eine Anmeldeseite Ihre Anmeldedaten eingeben wird angezeigt. Verwenden Sie dieselbe e-Mail und Kennwort für die Anmeldung bei Azure-Portal.

    Add-AzureRmAccount

Nach der erfolgreichen Anmeldung sehen Sie einige Informationen auf dem Bildschirm, die die SubscriptionId enthält, mit denen Sie angemeldet. Dies ist das Abonnement in dem Ressourcen für dieses Lernprogramm erstellt werden, wenn Sie ein anderes Abonnement ändern. Haben Sie mehrere SubscriptionIds, führen Sie das folgende Cmdlet um eine Liste aller von der SubscriptionIds zurückgegeben:

    Get-AzureRmSubscription

Informationen zum Ändern von anderen SubscriptionID führen Sie das folgende Cmdlet mit der gewünschten SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Image-Variablen definieren

Zur Vereinfachung der Nutzbarkeit und Verständnis des abgeschlossenen Skripts aus diesem Lernprogramm starten wir eine Anzahl von Variablen definieren. Ändern Sie die Parameterwerte, Bedarf jedoch Vorsicht Benennung Einschränkungen Namenslängen und Sonderzeichen beim Ändern der Werte.

### <a name="location-and-resource-group"></a>Speicherort und Ressourcengruppe
Verwenden Sie zwei Variablen Datenbereich und die Ressourcengruppe, in der Sie die Ressourcen für den virtuellen Computer erstellen.

Wie gewünscht ändern Sie, und führen Sie die folgenden Cmdlets zum Initialisieren dieser Variablen.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Eigenschaften von Speicher

Verwenden Sie die folgenden Variablen definieren das Speicherkonto und den Speichertyp vom virtuellen Computer verwendet werden.

Wie gewünscht ändern Sie, und führen Sie das folgende Cmdlet zum Initialisieren dieser Variablen. Beachten Sie, dass in diesem Beispiel [Premium Speicher](../storage/storage-premium-storage.md)verwendeten für Produktionsarbeitslasten empfohlen. Einzelheiten zu dieser Anleitung und anderen empfohlenen anzeigen Sie [best Practices zur Performance für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md)

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Netzwerk-Eigenschaften

Verwenden Sie die folgenden Variablen definieren die Netzwerkschnittstelle Zuordnungsmethode TCP/IP-Namen des virtuellen Netzwerks, virtuelle Teilnetznamen, Bereich von IP-Adressen für das virtuelle Netzwerk, das von IP-Adressen im Subnetz und Bezeichnung der Öffentlichkeit im Netzwerk auf dem virtuellen Computer verwendet werden.

Wie gewünscht ändern Sie, und führen Sie das folgende Cmdlet zum Initialisieren dieser Variablen.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Eigenschaften für virtuelle Computer

Verwenden Sie die folgenden Variablen Name des virtuellen Computers, den Computernamen, die Größe des virtuellen Computers und den Datenträgernamen Betriebssystem für den virtuellen Computer definieren.

Wie gewünscht ändern Sie, und führen Sie das folgende Cmdlet zum Initialisieren dieser Variablen.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Bildeigenschaften

Verwenden Sie die folgenden Variablen, um das Bild für den virtuellen Computer definieren. In diesem Beispiel wird das Bild SQL Server 2016 Enterprise verwendet.

Wie gewünscht ändern Sie, und führen Sie das folgende Cmdlet zum Initialisieren dieser Variablen.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Beachten Sie, dass Sie eine vollständige Liste der SQL Server-Image Angebote mit dem Befehl Get-AzureRmVMImageOffer bekommen:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

Und Sie sehen die Skus für ein Angebot mit dem Befehl Get-AzureRmVMImageSku. Der folgende Befehl zeigt alle Skus für **SQL2014SP1 WS2012R2** bieten.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Mit dem Ressourcen-Manager-Bereitstellungsmodell ist das erste Objekt, das Erstellen der Ressourcengruppe. Wir verwenden das [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) -Cmdlet erstellen Azure Ressourcengruppe und Ressourcen mit den Namen und den Speicherort von zuvor initialisiert Variablen definiert.

Führen Sie das folgende Cmdlet die neue Ressourcengruppe erstellen.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen

Der virtuelle Computer muss Speicherressourcen Betriebssystem-Datenträger und die SQL Server-Daten und-Protokolldateien. Aus Gründen der Einfachheit wird für beide eine Festplatte erstellt. Sie können zusätzliche Festplatten [Hinzufügen Azure Datenträger](https://msdn.microsoft.com/library/azure/dn495252.aspx) Cmdlet um Ihre SQL Server-Daten und Protokolldateien auf Datenträger später mit Anhängen. Wir verwenden das [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) -Cmdlet registrieren Standardspeicher in die neue Ressourcengruppe und Storage-Kontonamen, Storage Sku Name und Speicherort mit zuvor initialisiert Variablen definiert.

Führen Sie das folgende Cmdlet neue Speicherkonto erstellen.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen

Der virtuelle Computer benötigt eine Reihe von Netzwerkressourcen für die Netzwerkkonnektivität.

- Jeder virtuelle Computer benötigt ein virtuelles Netzwerk.
- Ein virtuelles Netzwerk müssen mindestens ein Subnetz definiert.
- Eine öffentliche oder eine private IP-Adresse muss eine Schnittstelle definiert werden.

### <a name="create-a-virtual-network-subnet-configuration"></a>Konfiguration eines virtuellen Subnetz erstellen

Zunächst wird ein Subnetzkonfiguration für unser virtuelles Netzwerk erstellen. Unser Lernprogramm erstellen wir eine Standard-Subnetz mit dem [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -Cmdlet. Wir erstellen unsere virtuelle Netzwerkkonfiguration Subnetz mit Name und Adresse Subnetzpräfix anhand der Variablen, die bereits initialisiert.

>[AZURE.NOTE] Zusätzliche Eigenschaften der virtuellen Subnetz Netzwerkkonfiguration mit diesem Cmdlet definieren, aber das ist nicht Gegenstand dieses Lernprogramm.

Führen Sie das folgende Cmdlet virtuellen Subnetzkonfiguration erstellen.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Als Nächstes erstellen wir unser virtuelle Netzwerk mit dem [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -Cmdlet. Wir unser virtuelle Netzwerk in die neue Ressourcengruppe erstellen, Name, Speicherort und Adresse Präfix definiert die Variablen, die Sie zuvor initialisiert und die Subnetzkonfiguration, die Sie im vorherigen Schritt definiert.

Führen Sie das folgende Cmdlet virtuelle Netzwerk erstellen.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Erstellen Sie die öffentliche IP-Adresse

Wir unsere virtuelle Netzwerk definiert haben, müssen wir eine IP-Adresse für die Verbindung mit dem virtuellen Computer konfigurieren. In diesem Lernprogramm erstellen wir eine öffentliche IP-Adresse mit dynamischen IP-Adressen um Internetkonnektivität unterstützen. Wir verwenden das [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -Cmdlet erstellen, öffentliche IP-Adresse in der bisher erstellten Ressourcengruppe mit Name, Speicherort, Zuordnungsmethode und DNS-Namen des Domänennamens mit zuvor initialisiert Variablen definiert.

>[AZURE.NOTE] Zusätzliche Eigenschaften der öffentlichen IP-Adresse mit diesem Cmdlet definieren jedoch, die Gegenstand dieser ersten Tutorial. Sie können auch eine private Adresse oder eine Adresse mit einer statischen Adresse erstellen, aber das ist auch im Rahmen dieses Lernprogramms.

Führen Sie das folgende Cmdlet die öffentliche IP-Adresse erstellen.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>Erstellen der Netzwerkschnittstelle

Wir können nun die Netzwerkschnittstelle zu erstellen, den virtuellen Computer. Unsere Netzwerkschnittstelle in früheren und mit Name, Speicherort, Subnetz und öffentliche IP-Adresse, die zuvor definierten Ressourcengruppe erstellen, verwenden Sie das [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) -Cmdlet.

Führen Sie das folgende Cmdlet die Netzwerkschnittstelle erstellen.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Konfigurieren Sie eine VM-Objekt

Jetzt haben wir Speicher- und Netzwerkressourcen definiert sind wir computeressourcen für den virtuellen Computer definieren. Unsere Tutorial wird wir die Größe des virtuellen Computers und Betriebssystem-Eigenschaften festlegen, die Netzwerkschnittstelle, die wir zuvor erstellt definieren BLOB-Speicher, und geben Sie die Betriebssystem-CD.

### <a name="create-the-vm-object"></a>VM-Objekt erstellen

Zunächst wird die Größe des virtuellen Computers angeben. In diesem Lernprogramm werden wir eine DS13 angeben. Wir verwenden das [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) -Cmdlet zum Erstellen eines Objekts konfigurierbare virtuellen Computer mit dem Namen und Größe mit den zuvor initialisiert Variablen definiert.

Führen Sie das folgende Cmdlet Objekt des virtuellen Computers erstellen.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Erstellen Sie ein Anmeldeinformationsobjekt zum Speichern von Namen und Kennwort für das lokale Administratorrechte

Bevor wir die Betriebssystem-Eigenschaften für den virtuellen Computer einrichten können, müssen wir die Anmeldeinformationen für das lokale Administratorkonto als sichere Zeichenfolge. Dazu verwenden wir das Cmdlet " [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) ".

Führen Sie das folgende Cmdlet aus, und geben Sie im Anforderungsfenster Windows PowerShell-Anmeldeinformationen den Namen und das Kennwort für das lokale Administratorkonto in Windows Virtual Machine.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Legen Sie die Betriebssystem-Eigenschaften für den virtuellen Computer

Jetzt können wir die Eigenschaften des virtuellen Computers Betriebssystem festgelegt. Wir verwenden das Cmdlet " [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) ", legen Sie den Typ des Betriebssystems als Windows erfordern [VM-Agent](virtual-machines-windows-classic-agents-and-extensions.md) installiert werden, anzugeben, dass das Cmdlet AutoUpdate ermöglicht und Name des virtuellen Computers, den Computernamen und die Anmeldeinformationen mit den zuvor initialisiert Variablen festlegen.

Führen Sie das folgende Cmdlet zum Festlegen der Betriebssystem-Eigenschaften für den virtuellen Computer.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Der virtuelle Computer die Netzwerkschnittstelle hinzufügen

Wir werden fügen Sie die Schnittstelle, die wir erstellt zuvor virtuellen Computer. Die mit der Netzwerk-Schnittstelle Variablen, die zuvor definierte Schnittstelle hinzufügen, verwenden Sie [Add-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) -Cmdlet.

Führen Sie das folgende Cmdlet die Netzwerkschnittstelle für den virtuellen Computer festgelegt.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Festlegen Sie den BLOB-Speicherort für den Datenträger vom virtuellen Computer verwendet werden

Als Nächstes setzen wir Blob-Speicherort für den Datenträger vom virtuellen Computer mit den zuvor definierten Variablen verwendet werden.

Führen Sie das folgende Cmdlet der BLOB-Speicherort festgelegt.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Festlegen des Betriebssystems Datenträgereigenschaften für den virtuellen Computer

Als Nächstes setzen wir das Betriebssystem Datenträgereigenschaften für den virtuellen Computer. Wir verwenden das Cmdlet " [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) ", um anzugeben, dass das Betriebssystem des virtuellen Computers von einem Bild zum Lesen nur (da SQL Server auf demselben Datenträger installiert wird) Zwischenspeichern festlegen und Definieren der virtuellen Computer und das Betriebssystem definiert die Variablen verwenden, die zuvor von uns definiert.

Führen Sie das folgende Cmdlet zum Betriebssystem Datenträgereigenschaften für den virtuellen Computer festlegen.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Plattform-Images für die virtuelle Maschine angeben

Unsere Konfigurationsschritt ist Plattform-Images für den virtuellen Computer an. Unsere Tutorial verwenden wir das neueste SQL Server 2016 CTP-Bild. Dieses Bild durch Variablen definiert, die zuvor definierten verwenden, verwenden Sie das Cmdlet " [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) ".

Führen Sie das folgende Cmdlet Plattform-Images für den virtuellen Computer angeben.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>Erstellen der SQL-VM

Jetzt Konfigurationsschritte abgeschlossen haben, können Sie den virtuellen Computer erstellt. Erstellen Sie den virtuellen Computer mit den Variablen definiert wurde, verwenden Sie das [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) -Cmdlet.

Führen Sie das folgende Cmdlet virtuellen Computers erstellen.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Der virtuelle Computer wird erstellt. Standard Speicherkonto für Boot-Diagnose erstellt wird, da das angegebene Speicherkonto für die virtuellen Computer Laufwerk ist ein Speicherkonto Premium.

Nun können diesen Computer Azure-Portal [die öffentliche IP-Adresse](virtual-machines-windows-portal-sql-server-provision.md#Connect)und der vollqualifizierte Domänenname angezeigt.  

## <a name="example-script"></a>Beispielskript

Das folgende Skript enthält das vollständige PowerShell-Skript für dieses Lernprogramm. Es nimmt bereits Azure-Abonnement mit den Befehlen **AzureRmAccount hinzufügen** und **Wählen Sie AzureRmSubscription** verwenden können.


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Nächste Schritte
Nach dem Erstellen der virtuellen Maschine können Sie verbinden die virtuellen Computer mit RDP und Setup-Konnektivität. Weitere Informationen finden Sie unter [Verbindung zu einem SQL Server virtuellen Computer in Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).
