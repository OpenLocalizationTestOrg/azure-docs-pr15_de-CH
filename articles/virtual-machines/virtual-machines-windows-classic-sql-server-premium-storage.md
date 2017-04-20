<properties
    pageTitle="Verwenden Sie Azure Premium-Speicher mit SQL Server | Microsoft Azure"
    description="Dieser Artikel verwendet Ressourcen mit dem klassischen Bereitstellungsmodell erstellt und enthält Hilfestellung bei Azure Premium-Speicher mit SQL Server auf Azure virtuellen Computer ausgeführt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Verwenden Sie Azure Premium-Speicher mit SQL Server auf virtuellen Computern


## <a name="overview"></a>Übersicht

[Azure Storage Premium](../storage/storage-premium-storage.md) ist die nächste Generation von Storage, geringe Latenz und hohen Durchsatz IO bereitstellt. Funktioniert am besten für wichtige e/a-intensive Arbeitslasten wie SQL Server IaaS [VMs](https://azure.microsoft.com/services/virtual-machines/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Dieser Artikel bietet Planung und bei der Migration einen virtuellen Computer mit SQL Server, Premium-Speicher zu verwenden. Dazu gehören Azure-Infrastruktur (Netzwerkspeicher) und Gast Windows VM Schritte. Das Beispiel im [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) zeigt eine vollständige umfassende Ende Migration der größeren VMs zu verbesserten lokalen SSD-Speicher mit PowerShell verschieben.

Es ist wichtig zu verstehen, End-to-End-Prozess mit Azure Premium-Speicher mit SQL Server auf IAAS VMs. Dazu gehören:

- Kennung der Komponenten Premium Speicher verwendet.
- Beispiele für SQL Server auf IaaS bei Neuinstallationen Premium Speicher bereitstellen.
- Beispiele für Migration vorhandenen Bereitstellungen eigenständigen Servern und Bereitstellungen SQL immer auf Verfügbarkeit Gruppen.
- Migration mögliche Ansätze.
- Vollständige End-to-End-Beispiel Azure, Windows und SQL Server Schritte für die Migration einer vorhandenen Implementierung immer auf.

Weitere Informationen zu SQL Server in Azure Virtual Machines finden Sie unter [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

**Autor:** Daniel Sol **technische Prüfer:** Luis Carlos Vargas Hering Sanjay Mishra Pravin Mital, Jürgen Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Erforderliche Komponenten für Premium-Speicher

Es gibt mehrere Komponenten für Premium-Speicher.

### <a name="machine-size"></a>Computer-Größe

Für Premium-Speicher müssen Sie DS-Serie von virtuellen Maschinen (VM) verwenden. Wenn DS Serie in Ihrem Cloud-Dienst vor nicht verwendet haben, müssen Sie vorhandene VM löschen, die angeschlossenen Festplatten und erstellen einen neuen Clouddienst vor dem Neuerstellen der VM DS * Rolle Größe. Weitere Informationen zu Virtual Machine Größen finden Sie unter [virtueller Computer und Azure Cloud Service Größe](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Cloud-Dienste

Wenn sie in einem neuen Clouddienst erstellt werden, können Sie nur DS * VMs mit Premium verwenden. Wenn Sie SQL Server ständig in Azure verwenden, verweisen immer auf Listener Azure interne oder externe Load Balancer IP-Adresse, die Cloud-Dienst zugeordnet ist. Dieser Artikel befasst sich gleichzeitig Verfügbarkeit in diesem Szenario migrieren.

> [AZURE.NOTE] DS * Serie muss die erste VM, die neue Cloud-Dienst bereitgestellt wird.

### <a name="regional-vnets"></a>Regionale VNETS

DS * VMs müssen Sie das hosting der VMs zu regionalen virtuellen Netzwerk (VNET) konfigurieren. Diese "Erweitert" das VNET ermöglicht mehr VMs in anderen Clustern bereitgestellt werden und Kommunikation. Im folgenden Screenshot zeigt die markierte Position regionalen VNETs das erste Ergebnis "narrow" VNET zeigt.

![RegionalVNET][1]

Microsoft-Support-Ticket zu einem regionalen VNET migrieren auslösen, Microsoft ändern, dann zum Abschließen der Migrations zu regionalen VNETs ändern Sie die Eigenschaft AffinityGroup in der Netzwerkkonfiguration. Zuerst exportieren Sie die Netzwerkkonfiguration in PowerShell, und Ersetzen Sie die **AffinityGroup** -Eigenschaft im **VirtualNetworkSite** -Element mit **einer Eigenschaft** . Geben Sie `Location = XXXX` , `XXXX` ist eine Azure. Importieren Sie die neue Konfiguration.

Z. B. unter Berücksichtigung der folgenden VNET-Konfigurations:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Um dies in regionalen VNET in Europa verschieben, ändern Sie die Konfiguration wie folgt:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Speicherkonten

Sie müssen ein neues Speicherkonto erstellen, das für Premium-Speicher konfiguriert. Beachten Sie, dass die Verwendung von Premium-Speicher an das Speicherkonto nicht auf einzelne Festplatten ist jedoch bei DS * Reihe virtueller VHD Premium und Standardspeicher anfügen können. Sie sollten dies soll nicht VHD OS Storage Premium-Konto an.

**Neu AzureStorageAccountPowerShell** Folgendes mit "Premium_LRS" **Typ** erstellt ein Speicherkonto Premium:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Festplatten-Cache Settings

Der Hauptunterschied zwischen Festplatten, die Teil des Premium-Speicherkonto ist der Datenträger-Cache-Einstellung. SQL Server Datenträger Volume wird empfohlen, "**Read Caching**" verwenden. Für Transaktions Protokoll-Volumes sollten Datenträger-Cache-Einstellung auf '**None**' festgelegt werden. Dies unterscheidet sich von der empfohlenen standardspeicherkonten.

Sobald die VHDs angefügt wurden, kann die cacheeinstellung geändert werden. Sie müssten und schließen die VHD-Datei mit einem aktualisierten Cache.

### <a name="windows-storage-spaces"></a>Windows Storage spaces

[Windows Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx) können wie vorherige Standardspeicher, dadurch können Sie einen virtuellen Computer migrieren, die bereits Speicherplätze verwendet. Wird in [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (Schritt 9 und forward) veranschaulicht den Powershell Code extrahieren und importieren Sie einen virtuellen Computer mit mehreren angeschlossenen Festplatten.

Speicher-Pools wurden mit standardmäßigen Azure Storage-Konto verwendet, Durchsatz und Wartezeit. Unter Umständen Wert mit Premium-Speicher-Pools bei Neuinstallationen testen, aber fügen sie zusätzliche Komplexität mit Remotespeicher-Setup.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>So finden Sie die Azure virtuelle Laufwerke zuordnen Speicher-pools

Wie andere Cache Einstellung Rahmen angeschlossenen Festplatten vorhanden sind, empfiehlt es sich, die VHDs Premium Speicherkonto kopieren. Wenn Sie neue DS Serie VM erneut hinzuzufügen, müssen Sie die Cache-Einstellung zu ändern. Es ist einfacher anzuwendende Premium Speicher Cache-Einstellungen bei separaten Festplatten für SQL-Dateien und Protokolldateien (anstatt eine einzelne virtuelle Festplatte, die sowohl) empfohlen.

> [AZURE.NOTE] Haben Sie SQL Server-Daten und Protokolldateien Dateien auf demselben Volume hängt Zwischenspeicherungsoption gewählte IO Zugriffsmuster für Ihre datenbankarbeitslasten. Nur Tests zeigen auf die Option zum Zwischenspeichern für dieses Szenario am besten geeignet ist.

Wenn Sie Windows Storage Spaces, die aus mehreren Festplatten bestehen verwenden sich müssen Ihre ursprünglichen Skripts identifizieren die VHDs angefügt sind jedoch in welche bestimmten Pool so dann Cache-Einstellung festlegen können entsprechend für jede Festplatte.

Wenn Sie keinen ursprünglichen Skript anzeigen virtueller Festplatten, die dem Speicherpool zugeordnet, können die folgenden Schritte Sie um Festplatten-Storage Pool-Zuordnung zu bestimmen.

Gehen Sie für jedes Laufwerk folgendermaßen vor:

1. Abrufen Sie Liste der Datenträgern VM mit dem Befehl **Get-AzureVM** :

    Get-AzureVM - ServiceName <servicename> -Namen <vmname> | AzureDataDisk abrufen

1. Hinweis Diskname und LUN.

    ![DisknameAndLUN][2]

1. Remote Desktop in der VM. Wechseln Sie zum **Computer Management** | **Gerätemanager** | **Festplatten**. Sehen Sie sich die Eigenschaften der "Microsoft virtuellen Laufwerke"

    ![VirtualDiskProperties][3]

1. Die LUN-Nummer ist die LUN-Nummer angeben, wenn die VM die VHD-Datei zuordnen.
1. Zum Einfügen der 'Microsoft Virtual Disk' auf die Registerkarte **Details** in der Liste **Eigenschaft** fahren Sie **Treiber**Schlüssel. **Wert**Hinweis **versetzt**, der im folgenden Screenshot 0002. Die 0002 kennzeichnet den Speicherpool verweist auf PhysicalDisk2.

    ![VirtualDiskPropertyDetails][4]

2. Sichern Sie für jeden Speicherpool zugeordneten Festplatten:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-Physikalischer Datenträger

    ![GetStoragePool][5]

Jetzt können Sie an der diese Informationen zuordnen VHDs Festplatten Speicher-Pools.

Sobald Sie VHDs Festplatten Speicher-Pools zugewiesen haben dann trennen, und kopieren Sie diese in ein Premium-Speicherkonto, fügen Sie diese mit der korrekten cacheeinstellung. Finden Sie im Beispiel im [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)Schritte 8 bis 12. Diese Schritte zeigen, wie eine VM-attached VHD Datenträgerkonfiguration in eine CSV-Datei extrahieren, kopieren die VHDs, Disk-Cache Konfigurationsinformationen ändern und schließlich die VM als DS Serie VM mit angeschlossenen Datenträger erneut.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM Speicherbandbreite und VHD Durchsatz

Der Speicher-Performance hängt angegebene DS * VM-Größe und der Größe der VHD. Die VMs haben verschiedene Zertifikate für die Anzahl von VHDs angefügt werden kann und die maximale Bandbreite (MB/s) sie unterstützen. Bestimmte Bandbreiten-Zahlen finden Sie unter [virtuelle Computer und Azure Cloud Service Größe](virtual-machines-linux-sizes.md).

Höhere IOPS erzielen mit größeren Datenträger. Dies sollten bei Ihren Migrationspfad denken. Einzelheiten [entnehmen Sie der Tabelle IOPS und Datenträgertypen](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Schließlich Meinung VMs unterschiedliche maximale Datenträger Bandbreiten, die sie für alle angeschlossenen Datenträger unterstützen. Hoher Belastung kann die maximale Bandbreite für VM Rolle Größe auslasten. Beispielsweise wird ein Standard_DS14 bis zu 512 MB/s unterstützt. daher mit drei P30 Festplatten konnte Datenträgerbandbreite der VM auslasten. Aber in diesem Beispiel konnte abhängig von Lese- und IOs Durchsatz Limit überschritten werden.

## <a name="new-deployments"></a>Bereitstellung neuer

In den nächsten beiden Abschnitten zeigen, wie Sie SQL Server VMs Premium Speicher bereitstellen. Wie bereits erwähnt, müssen Sie nicht unbedingt die OS Diskette auf Premium-Speicher. Sie können, wenn Sie beabsichtigen, eine intensive IO-Workloads auf OS VHD platzieren.

Das erste Beispiel zeigt vorhandene Azure Galeriebilder nutzen. Im zweiten Beispiel wird veranschaulicht, wie benutzerdefiniertes VM-Image verwenden, die Sie in ein vorhandenes Konto Standardspeicher.

> [AZURE.NOTE] Diese Beispiele gehen Sie regionale VNET bereits erstellt haben.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Erstellen Sie einen neuen virtuellen Computer mit Premium mit Galeriebild

Im folgenden Beispiel veranschaulicht, wie OS VHD auf Premium-Speicher und Premium Speicher VHDs anfügen. Sie können auch ein standardspeicherkonto versehen den Betriebssystem-Datenträger und Anhängen VHDs Premium Speicherkonto befinden. Beide Szenarien werden veranschaulicht.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Schritt 1: Erstellen Sie ein Speicherkonto Premium


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Schritt 2: Erstellen eines neuen Cloud-Diensts

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Schritt 3: Reservieren Sie Cloud Service VIP (Optional)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Schritt 4: Erstellen eines VM-Containers
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Schritt 5: Inverkehrbringen OS VHD Standard oder Premium-Speicher
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Schritt 6: Erstellen Sie virtueller Computer
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Erstellen einer neuen VM um Premium-Speicher durch ein benutzerdefiniertes Bild verwenden

Dieses Szenario veranschaulicht vorhandene benutzerdefinierte Bilder kommen, die sich im Standardspeicher Konto befinden. Wie erwähnt, VHD OS auf Premium-Speicher müssen Sie das vorhandene Bild im Standardspeicher Konto kopieren und auf ein Premium-Speicher übertragen, bevor es verwendet werden kann. Haben Sie ein Bild lokal können Sie diese Methode auch verwenden, kopieren, die direkt an das Speicherkonto Premium.

#### <a name="step-1-create-storage-account"></a>Schritt 1: Speicher-Konto erstellen
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Schritt 2 Cloud-Dienst erstellen
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Schritt 3: Vorhandenes Bild verwenden
Sie können ein vorhandenes Bild. Oder Sie können [ein Bild der vorhandenen Computer](virtual-machines-windows-classic-capture-image.md). Beachten Sie Bild keinen DS* Computer Computer. Haben Sie das Bild, die folgenden Schritte zeigen kopieren auf Premium-Speicherkonto mit den * *Start AzureStorageBlobCopy** PowerShell-Cmdlet.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Schritt 4: Kopieren BLOBs zwischen Speicherkonten
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Schritt 5: Regelmäßig Kopie:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Schritt 6: Azure Datenträger Repository Abonnement Bild Datenträger hinzufügen
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Möglicherweise, obwohl die Statusberichte Erfolg noch einen Fehler Lease erhalten konnte. In diesem Fall warten Sie 10 Minuten.

#### <a name="step-7--build-the-vm"></a>Schritt 7: Erstellen Sie den virtuellen Computer
Hier erstellen Sie die VM aus dem Bild sowie zwei Premium Speicher VHDs anfügen:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Vorhandene Installationen, die nicht immer auf Verfügbarkeitsgruppen verwenden

> [AZURE.NOTE] Vorhandene Installationen zuerst finden Sie im Abschnitt [erforderliche Komponenten](#prerequisites-for-premium-storage) dieses Themas.

Gibt es andere Aspekte für SQL Server-Installationen, die immer auf Verfügbarkeit und Gruppen, die nicht verwenden. Wenn Sie nicht immer verwenden und eine bestehende eigenständige SQL Server, können Sie eine neue Cloud Service und Speicher Konto Premium Speicher aktualisieren. Berücksichtigen Sie die folgenden Optionen:

- **Erstellen einer neuen SQL Server-VM**. Sie können Erstellen einer neuen SQL Server-VM, die Premium-Speicherkonto verwendet Neuinstallationen dokumentiert. Dann sichern und Wiederherstellen der Konfiguration und die Benutzerdatenbanken für SQL Server. Die Anwendung muss aktualisiert werden, um neue SQL Server verweisen, wenn sie intern oder extern zugegriffen wird. Sie müssten alle 'aus Db"Objekte kopieren, als ob Sie nebeneinander (SxS) SQL Server Migration ausführen. Dies umfasst Objekte wie Benutzernamen, Zertifikate und Verbindungsserver.
- **Migrieren einer vorhandenen SQL Server-VM**. Dies erfordert, dass die SQL Server-VM offline geschaltet und dann auf neue Cloud-Dienst übertragen, einschließlich aller angeschlossenen Festplatten Premium Speicherkonto kopieren. Wenn die VM online geschaltet wird, verweist die Anwendung der Serverhostname als vor. Beachten Sie, dass die Größe des vorhandenen Datenträger die Eigenschaften beeinflussen. Beispielsweise wird eine 400-GB-Festplatte einem P20 aufgerundet. Wenn Sie wissen, dass dieser datenträgerleistung erforderlich ist, konnte Sie neu erstellen VM als eine DS Serie VM und Anhängen Premium Speicher VHDs/-Leistung-Spezifikation, die Sie benötigen. Sie können dann trennen und erneut anfügen SQL DB-Dateien.

> [AZURE.NOTE] Wenn Sie Größe je beachten VHD-Datenträgern kopieren welche Premium Datenträger bedeuten sie in, Lastenheft Datenträger bestimmt. Azure wird auf den nächsten Datenträger Größe, haben eine 400 GB Festplatte diese auf eine P20 aufgerundet wird. Je nach vorhandenen e/a-Anforderung der virtuellen Festplatte OS müssen Sie nicht diese Prämie Speicherkonto migrieren.

Wenn der SQL Server-extern zugegriffen wird, wird die Cloud Service VIP ändern. Außerdem müssen Endpunkte Update ACLs und DNS-Einstellungen.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Vorhandene Installationen, die immer auf Verfügbarkeit Gruppen

> [AZURE.NOTE] Vorhandene Installationen zuerst finden Sie im Abschnitt [erforderliche Komponenten](#prerequisites-for-premium-storage) dieses Themas.

Zunächst in diesem Abschnitt betrachten wir wie immer auf Azure Netzwerk interagiert. Wir dann Migrationen in zwei Szenarien brechen: Migrationen, Ausfallzeiten toleriert werden kann, und wo Sie Ausfälle erreichen müssen Migration.

Lokale mithilfe SQL Server immer auf Verfügbarkeit von Gruppen eine Listener-lokal, das einen virtuellen DNS-Name und IP-Adresse registriert, die von SQL Server gemeinsam. Wenn Clients eine Verbindung herstellen werden sie über die Listener IP primäre SQL Server weitergeleitet. Dies ist der Server, der zu diesem Zeitpunkt immer auf IP-Adressressource besitzt.

![DeploymentsUseAlways auf][6]

Microsoft Azure Ihnen nur eine IP-Adresse zu einer Netzwerkkarte auf dem virtuellen Computer nutzt so Erreichung derselben Ebene der Abstraktion lokal Azure die IP-Adresse, die interne/externe Load Balancers (ILB/ELB) zugeordnet ist. Dieselbe IP-Adresse als ILB/ELB IP-Adressressource, die zwischen den Servern fest. Dies erscheint im DNS und Clientdatenverkehr ist an primäre SQL Server-Replikat über ILB/ELB. ILB/ELB weiß die SQL Server-primäre es Prüfpunkte verwendet immer auf IP-Adressressource. Im vorherigen Beispiel untersucht jeden Knoten mit einem Endpunkt auf die ELB/ILB ist je antwortet der primäre SQL Server.

> [AZURE.NOTE] ILB und ELB sind an einen bestimmten Azure-Cloud-Dienst zugewiesen, daher jede Cloud-Migration in Azure wahrscheinlich bedeutet, dass Load Balancer IP ändert.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migrieren von immer auf Installationen, die Ausfallzeiten können

Es gibt zwei Strategien immer auf Installationen migrieren, die Ausfallzeiten zu ermöglichen:

1. **Weitere sekundäre Replikate zu einem vorhandenen immer auf Cluster hinzufügen**
1. **Migration zu einem neuen immer auf Cluster**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. Fügen Sie 1. weitere sekundäre Replikate zu einem vorhandenen immer auf Cluster

Eine Strategie ist das immer auf Availability Group weitere sekundäre hinzu. Sie müssen in einen neuen Clouddienst und den Listener mit neuen Load Balancer IP aktualisieren.

##### <a name="points-of-downtime"></a>Punkte von Ausfallzeiten:

- Clustervalidierung.
- Neue sekundäre immer auf Failover testen.

Bei Verwendung von Windows-Speicher-Pools innerhalb der virtuellen Maschine für höhere e/a-Durchsatz dann diese offline während einer vollständigen Clusterüberprüfung erfolgt. Die Überprüfung ist erforderlich, wenn Knoten zum Cluster hinzufügen. Zum Ausführen des Tests benötigte Zeit kann variieren, so sollten Sie testen dies in Ihrer Umgebung repräsentative Tests erhalten eine ungefähre wie lange dies dauert.

Sie sollten Zeit bereitstellen, wo Manuelles Failover und Chaos Testen der neu hinzugefügte Knoten immer hohe Verfügbarkeit wie erwartet funktionieren ausführen können.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Beenden Sie alle Instanzen von SQL Server, Speicher-Pools wird, vor der Validierung ausgeführt wird.
##### <a name="high-level-steps"></a>Schritte

1. Erstellen Sie zwei neue SQL Server neue Cloud Service mit Premium-Speichermedien.
1. Kopieren Sie über vollständige Backups und Wiederherstellung mit **NORECOVERY**.
1. Kopieren Sie über 'aus Benutzer DB' abhängige Objekte wie Benutzernamen usw.
1. Neue eine neue interne Load Balancer (ILB) oder eine externe Load Balancer (ELB) verwenden und dann Load Balanced Endpunkte auf beiden neuen Knoten eingerichtet.
> [AZURE.NOTE] Überprüfen Sie alle Knoten die richtige Endpunktkonfiguration verfügen, bevor Sie fortfahren

1. Beenden Sie Benutzer-Anwendung Zugriff auf SQL Server (bei Speicher-Pools).
1. SQL Server Engine Services (bei Speicher-Pools) auf allen Knoten beendet.
1. Fügen Sie neuer Knoten zum cluster und vollständige Überprüfung ausführen hinzu.
1. Wenn die Validierung erfolgreich ist, starten Sie alle SQL Server-Dienste.
1. Transaktionsprotokolle sichern und Wiederherstellen von Datenbanken.
1. Knoten in der immer auf Availability Group und Replikation in **synchron**.
1. Fügen Sie die IP-Adressressource für das neue Cloud Service ILB/ELB über PowerShell für immer anhand der mehrere Standorte im [Anhang hinzu](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) In Windows soll clustering die **möglichen Besitzer** der **IP-** Adressenressource alte neuen Knoten. Siehe Abschnitt "Hinzufügen von IP-Adressenressource in demselben Subnetz" im [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
1. Failover zu einem neuen Knoten.
1. Stellen Sie die neuen Knoten automatische Failover Partner und Test-Failover.
1. Entfernen Sie ursprüngliche Knoten aus Verfügbarkeit Gruppe.

##### <a name="advantages"></a>Vorteile

- Neue SQL Server möglich (SQL Server und Anwendung) getestet, bevor sie ständig hinzugefügt werden.
- Die VM-Größe ändern können und den Speicher an Ihre Bedürfnisse anpassen. Allerdings wäre es vorteilhaft, den SQL-Dateipfaden beibehalten.
- Sie können steuern, wenn die Übertragung der DB-Backups auf sekundäre Replikate gestartet. Dies unterscheidet sich von Azure **Start-AzureStorageBlobCopy** -Cmdlet VHDs kopieren verwenden, da das asynchrone Kopie ist.

##### <a name="disadvantages"></a>Nachteile
- Wenn Windows-Speicher-Pools mit wird Cluster Ausfallzeiten während der vollständige Cluster-Validierung für die neuen zusätzlichen Knoten.
- Die SQL Server-Version und die Anzahl der vorhandenen sekundären Replikate Sie weitere sekundäre Replikate hinzufügen, ohne vorhandene sekundäre möglicherweise nicht.
- Es kann SQL Data Transfer lange der sekundäre einrichten.
- Während der Migration ist zusätzliche Kosten, während Sie neue Computer parallel ausgeführt haben.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. Migration zu einem neuen immer auf Cluster

Eine andere Strategie ist eine neue immer auf Cluster neue Knoten im neuen Cloud-Dienst erstellen und dann die Clients verwenden.

##### <a name="points-of-downtime"></a>Punkte von Ausfallzeiten

Ausfallzeiten wird Applikationen und Benutzer neue Listener ständig zu übertragen. Die Ausfallzeit abhängig:

- Die Zeit zum endgültigen Transaktionsprotokollen Datenbanken auf neuen Servern wiederherstellen.
- Die Verarbeitungszeit für Clientanwendungen mit neuen Listener ständig aktualisieren.

##### <a name="advantages"></a>Vorteile

- Die tatsächliche Erzeugung Umgebung SQL Server testen und OS erstellen ändern.
- Sie können den Speicher anpassen und VM möglicherweise verkleinern. Kostensenkungen möglich.
- Dabei können Sie die SQL Server-Build oder Version aktualisieren. Sie können auch das Betriebssystem aktualisieren.
- Vorherige immer auf Cluster können als feste Rollback Ziel fungieren.

##### <a name="disadvantages"></a>Nachteile

- Sie müssen den DNS-Namen des Listeners ggf. beide Cluster stets gleichzeitig ändern. Das erhöht Verwaltung Aufwand während der Migration Client anwendungszeichenfolgen neuen Listenernamen entsprechen müssen.
- Sie müssen einen Synchronisierungsmechanismus zwischen den zwei zu möglichst die letzte Synchronisierung Vorschriften vor der Migration zu implementieren.
- Es ist zusätzliche Kosten während der Migration während die neue Umgebung ausgeführt haben.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migrieren von immer auf Installationen für Ausfälle

Es gibt zwei Strategien für Migration immer auf Installationen für Ausfälle:

1. **Verwenden einer vorhandenen sekundären: Standortes**
1. **Nutzen Sie vorhandene sekundäre Replikate: mehrere Standorte**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. verwenden Sie vorhandene sekundäre: Standortes

Eine Strategie für Ausfälle ist eine vorhandene Cloud sekundären und aktuelle Cloud-Dienst entfernen. Die VHDs in das neue Premium-Speicherkonto kopieren, und die VM in der neuen Cloud-Dienst erstellen. Aktualisieren des Listeners clustering und Failover.

##### <a name="points-of-downtime"></a>Punkte von Ausfallzeiten

- Ausfallzeiten wird den letzten Knoten mit Lastenausgleich Endpunkt zu aktualisieren.
- Der Client-Verbindung kann je nach Konfiguration DNS Clients verzögert werden.
- Zusätzliche Ausfallzeiten ist Sie wählen, die immer auf Cluster-Gruppe, die IP-Adressen auszutauschen. Sie können dies vermeiden, mit Abhängigkeit oder als mögliche Besitzer für die hinzugefügte IP-Adressressource. Siehe Abschnitt "Hinzufügen von IP-Adressenressource in demselben Subnetz" im [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).

> [AZURE.NOTE] Wenn den hinzugefügten Knoten als immer auf Failover Partner teilnehmen möchten, müssen Sie einen Endpunkt Azure auf Load Balanced festgelegt hinzufügen. Beim Ausführen des **Hinzufügen-AzureEndpoint** Befehls dürfen aktuelle Verbindungen bleiben geöffnet, aber neue Verbindungen mit dem Listener bis Lastenausgleich wurde eingerichtet werden. Tests trat zum letzten 90-120seconds, das getestet werden soll.

##### <a name="advantages"></a>Vorteile

- Ohne zusätzliche Kosten während der Migration.
- Eine 1: 1-Migration.
- Geringere Komplexität.
- Ermöglicht höhere IOPS von Premium Speicher SKUs. Wenn Datenträger von der VM und 3. kopiert neue Cloud-Dienst eines Drittanbieters Tool lässt sich VHD, vergrößern die höheren Durchsatz bietet. Erhöhen der Größe der VHD-Datei finden Sie unter [Diskussionsforum](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Nachteile

- Während der Migration ist Funktionalitätsverlust HA und DR.
- Wie eine 1:1-Migration sieht müssen Sie eine Mindestgröße von VM verwenden, die Anzahl von VHDs unterstützen, damit Sie Ihre VMs verkleinern möglicherweise nicht.
- Dieses Szenario würde Azure **Start-AzureStorageBlobCopy** -Cmdlet verwenden asynchrone. Es besteht kein DIENSTVERTRAG Kopieren-Abschluss. Die Dauer der Kopien ist während dieser Wartezeit in Warteschlangen auch auf die hängt zu übertragenden Daten abhängen. Gleichzeitig kopieren erhöht, wenn die Übertragung auf einen anderen Azure-Rechenzentrum, die Premium-Speicher in einen anderen Bereich unterstützt. Haben Sie nur 2 Knoten, sollten Sie eine mögliche Lösung bei die Kopie länger als testen dauert. Dazu zählen die folgenden Punkte.
    - Temporären 3. SQL Server-Knoten für HA vor der Migration vereinbarten Ausfallzeiten hinzufügen.
    - Führen Sie die Migration außerhalb Azure Wartungsarbeiten.
    - Stellen Sie sicher, dass Ihre Clusterquorum ordnungsgemäß konfiguriert.  

##### <a name="high-level-steps"></a>Schritte

Dieses Dokument zeigt ein vollständiges Beispiel Ende nicht jedoch [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) Details, die genutzt werden können enthält, um dies durchzuführen.

![MinimalDowntime][8]

- Festplattenkonfiguration sammeln und entfernen den Knoten (angeschlossene Festplatten nicht gelöscht).
- Ein Premium-Speicherkonto erstellen und Kopieren von VHDs Ausgangskonto Standardspeicher
- Neue Cloud-Dienst erstellen und SQL2 VM im Cloud-Dienst erneut. Erstellen Sie die VM kopierte ursprüngliche OS VHD verwenden und die kopierten VHDs anfügen.
- Konfigurieren der ILB / ELB und Endpunkte hinzufügen.
- Aktualisieren Sie Listener entweder:
    - Immer auf Gruppe offline geschaltet und immer auf Listener mit neuen ILB aktualisiert / ELB IP Adresse.
    - Oder die IP-Adressressource der neue Cloud Service ILB/ELB über PowerShell in Windows clustering. Dann legen Sie die möglichen Besitzer der IP-Adressenressource migrierten Knoten SQL2, und dies als Abhängigkeit oder Netzwerkname. Siehe Abschnitt "Hinzufügen von IP-Adressenressource in demselben Subnetz" im [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
- Überprüfen Sie DNS-Konfiguration/Propagierung Clients.
- SQL1 VM migrieren und Schritte 2 bis 4.
- Wenn Schritte 5ii, fügen Sie SQL1 als möglichen Besitzer für die hinzugefügte IP-Adressenressource
- Failover zu testen.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. verwenden Sie vorhandene sekundäre Replikate: mehrere Standorte

Wenn Knoten in mehreren Azure-Rechenzentrum (DC) haben oder eine hybridumgebung haben, dann können immer auf Konfiguration in dieser Umgebung Sie um Ausfallzeiten zu minimieren.

Der Ansatz ist synchron für die lokale oder sekundäre Azure DC und Failover über auf dieser SQL Server-Synchronisierung immer auf Änderung. Die VHDs Storage Premium-Konto kopieren, und den Computer in einen neuen Clouddienst erneut. Den Listener aktualisieren und dann ein Failback.

##### <a name="points-of-downtime"></a>Punkte von Ausfallzeiten

Die Ausfallzeit besteht Zeit für Failover und die anderen DC. Er hängt auch von der Client-DNS-Konfigurations und der Client-Verbindung verzögert.
Beispiel der ständig Mischkonfiguration:

![MultiSite1][9]

##### <a name="advantages"></a>Vorteile

- Sie können vorhandene Infrastruktur nutzen.
- Sie können den Azure-Speicher auf dem DC DR Azure zuerst vor der Aktualisierung.
- DR Azure DC-Speicher kann geändert werden.
- Während der Migration, ausgenommen Test-Failover ist mindestens zwei Failover.
- Sie müssen keine SQL Server-Daten mit Backup und restore.

##### <a name="disadvantages"></a>Nachteile

- Je nach Clientzugriff auf SQL Server möglicherweise eine erhöhte Latenz bei SQL Server an die Anwendung in einem anderen DC ausgeführt wird.
- Die Kopie VHDs Premium Speicher kann long werden. Dies kann die Entscheidung, ob der Knoten in der Availability Group erhalten bleiben. Folgendes Protokoll intensiv laufen Lasten während der Migration erforderlich ist seit der Primärknoten replizierten Transaktionen in das Transaktionsprotokoll beibehalten. Daher konnte diese erheblich vergrößert.
- Dieses Szenario würde Azure **Start-AzureStorageBlobCopy** -Cmdlet verwenden asynchrone. Es besteht kein DIENSTVERTRAG Abschluss. Die Kopien variiert, während dieser Wartezeit in Warteschlangen, hängt er hängt auch die Datenmenge übertragen. Daher Sie nur einen Knoten im 2. Rechenzentrum, ergreifen Sie Maßnahmen bei die Kopie dauert länger als testen. Dazu zählen die folgenden Punkte.
    - Temporären 2. SQL-Knoten für HA vor der Migration vereinbarten Ausfallzeiten hinzufügen.
    - Führen Sie die Migration außerhalb Azure Wartungsarbeiten.
    - Stellen Sie sicher, dass Ihre Clusterquorum ordnungsgemäß konfiguriert.

Dieses Szenario setzt voraus, dass Ihre Installation dokumentiert und wissen, wie der Speicher zugeordnet ist, um optimalen Disk-Cache-Einstellung ändern.

##### <a name="high-level-steps"></a>Schritte
![Multisite2][10]

- Lokalen / wechseln Sie DC Azure SQL Server primäre, und die andere automatische Failover Partner (AFP).
- Konfigurationsinformationen aus SQL2 sammeln und entfernen den Knoten (angeschlossene Festplatten nicht gelöscht).
- Erstellen ein Speicherkonto Premium und VHDs Standardspeicher Konto kopieren.
- Erstellen eines neuen Cloud-Diensts und SQL2 VM mit der Prämien Datenträger zugeordnet.
- Konfigurieren der ILB / ELB und Endpunkte hinzufügen.
- Aktualisieren Sie immer auf Listener mit neuen ILB / ELB IP-Adresse und Test-Failover.
- Überprüfen Sie die DNS-Konfiguration.
- Ändern der AFP SQL2, migrieren SQL1 und Schritte 2 bis 5.
- Failover zu testen.
- Die AFP SQL1 und SQL2 wechseln

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Anhang: Migration mehrere Standorte immer im Cluster zu Premium-Speicher

Dieses Thema enthält ein ausführliches Beispiel Premium Speicher einen Multi-Site immer im Cluster umwandeln. Auch wird den Listener ein externer Lastenausgleich (ELB) mit einer internen Lastenausgleich (ILB) konvertiert.

### <a name="environment"></a>Umgebung

- Windows 2k 12 / SQL 2k 12
- Dateien auf SP 1 DB
- 2 x pro Node-Speicher-Pools

![Appendix1][11]

### <a name="vm"></a>VS:

In diesem Beispiel wollen wir demonstrieren, Verschieben von einer ELB ILB. ELB wurde vor ILB, so zeigt dies während der Migration wechseln.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Schritte vor: Abonnement verbinden

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Schritt 1: Erstellen neuer Speicherkonto und Cloud-Dienst
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Schritt 2: Erhöhen der zulässigen Fehler für Ressourcen<Optional>
Auf bestimmte Ressourcen, die Ihr immer auf Verfügbarkeit Gruppe angehören, werden Grenzwerte für wie viele Fehler in einem Zeitraum erfolgen, in dem der Clusterdienst versucht die Ressourcengruppe starten Es wird empfohlen, dies während Sie dieses Verfahren seit gehen, wenn Failover und Trigger Failover manuell Herunterfahren von Computern können Sie dieses Limit nicht erhöhen.

Es wäre die Vergütung Fehler dazu im Failovercluster-Manager, doppelte Eigenschaften der Ressourcengruppe zu:

![Appendix3][13]

Ändern Sie die maximale Fehler 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Schritt 3: Hinzufügen-IP-Adressressource für Cluster-Gruppe<Optional>

Wenn Sie nur eine IP-Adresse für die Clustergruppe und dieses Subnetz Cloud ausgerichtet, beachten Sie, wenn Sie versehentlich offline alle Clusterknoten in der Cloud in diesem Netzwerk dann die Cluster-IP-Adressressource und der Netzwerkname nicht online geschaltet werden. Bei dieser verhindert es Updates Clusterressourcen.

#### <a name="step-4-dns-configuration"></a>Schritt 4: DNS-Konfiguration

Implementieren Sie eine Überblendung hängt wie DNS wird verwendet und aktualisiert.
Bei der Installation immer auf eine Windows Cluster-Ressourcengruppe erstellt Failovercluster-Manager öffnen, sehen sie haben mindestens drei Ressourcen, das Dokument auf, beide sind:

- Virtueller Name (VVN) – Dies ist der DNS-Name, Client eine Verbindung herstellen wenn Verbindung zu SQL Server über immer.
- IP-Adressressource – Dies ist die IP-Adresse, die mit der VVN können Sie mehrere und in einer Konfiguration für mehrere Standorte müssen eine IP-Adresse pro Site-Subnetz.

Bei Verbindung mit SQL Server, SQL Server Client Treiber werden DNS-Datensätze mit dem Listener abrufen und jede immer in Verbindung mit IP-Adresse besprechen wir unten einige Faktoren, die diese beeinflussen können.

Die Anzahl der gleichzeitigen DNS-Datensätze, die mit den Listenernamen hängt nicht nur die Anzahl der IP-Adressen die "Failover-Clustering für die Ressource stets auf VVN RegisterAllIpProviders'setting.

Wenn Sie ständig in Azure bereitstellen verschiedene Schritte zur Erstellung der Listener und IP-Adressen müssen Sie die RegisterAllIpProviders manuell konfigurieren 1, Dies unterscheidet sich von einer vor Ort stets Bereitstellung, ist es bereits auf 1 gesetzt.

Ist 'RegisterAllIpProviders' 0, dann sehen Sie nur einen DNS-Eintrag in DNS Listener zugeordnet:

![Appendix4][14]

Wenn 'RegisterAllIpProviders' 1:

![Appendix5][15]

Im folgenden Code wird VVN Einstellungen dump für Sie festgelegt und Hinweis für die Änderung wirksam wird, müssen die VVN offline und wieder online schalten, diese Listener offline verursacht dauert Client Connectivity Unterbrechung.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

Migration später müssen Listener ständig aktualisierte IP-Adresse zu aktualisieren, die ein Lastenausgleich auf beinhaltet eine IP-Adresse Ressource entfernen und hinzufügen. Nach IP-Aktualisierung müssen Sie die neue IP-Adresse im DNS-Zone aktualisiert wurde und die Clients ihre lokalen DNS-Cache aktualisieren.

Die Clients befinden sich in einem anderen Netzwerksegment, einen anderen DNS-Server verweisen müssen Fall über DNS-Zonentransfer während der Migration wie schließen die Anwendung werden eingeschränkt durch mindestens eine neue IP-Adressen der Zeitzone übertragen für den Listener. Sind hier Zeit Einschränkung sollten Sie diskutieren und zwingt ein inkrementeller Zonentransfer mit Ihrem Windows testen und DNS-Hosteintrag setzen auch auf eine niedrigere Zeit Gültigkeitsdauer (TTL), die Clients so aktualisieren. Weitere Informationen finden Sie unter [Inkrementeller Zonentransfer](https://technet.microsoft.com/library/cc958973.aspx) und [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Standard-TTL für DNS-Datensatz des Listeners ständig in Azure zugeordnet ist 1200 Sekunden. Sie können dadurch sind unter Aktualisierung Einschränkung während der Migration der Kunden ihre DNS die aktualisierte IP-Adresse für den Listener. Sie sehen und ändern Sie die Konfiguration durch die Konfiguration der VVN abbilden:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Hinweis: Je niedriger die 'HostRecordTTL', ein höherer Betrag DNS-Verkehr erfolgt.

##### <a name="client-application-settings"></a>Anwendung von Clienteinstellungen

Wenn die Clientanwendung SQL .net 4.5 unterstützt SQLClient, und Sie können ' MULTISUBNETFAILOVER = TRUE' Schlüsselwort, dies empfiehlt sich wie schnellere Verbindung SQL immer auf Verfügbarkeit Gruppe bei einem Failover ermöglicht. Es durchläuft alle IP-Adressen der Listener immer parallel und führt eine aggressivere TCP wiederholen Übertragungsrate bei einem Failover.

Weitere Informationen über die Einstellungen finden Sie unter [MultiSubnetFailover-Schlüsselwort mit verknüpft ist](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Siehe auch ["SqlClient"-Unterstützung für hohe Verfügbarkeit, Disaster Recovery](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Schritt 5: Aktionsbereich

Wie Sie Sie unten mindestens eine SQL Server gleichzeitig stattfinden, sollten Sie Cluster Quorum Einstellung ändern, wenn 2 Knoten (Datei Share Witness, FSW) mit Quorum Knotenmehrheit und dynamische Abstimmung verwenden soll und dies ist ein einzelner Knoten stehen bleiben.


    Set-ClusterQuorum -NodeMajority  

Weitere Informationen zum Verwalten und Konfigurieren des Clusterquorums finden Sie unter [Konfigurieren und verwalten das Quorum in einem Failovercluster Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Schritt 6: Vorhandene Endpunkte und ACLs extrahieren
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Speichern Sie diese in eine Textdatei.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Schritt 7: Ändern Sie Failover Partner und Replikationsmodi

Haben Sie mehr als 2 SQL Server Sie sollten "Synchron" Failover von einem anderen sekundären in einem anderen DC oder lokal ändern und ein automatisches Failover Partner (AFP), daher HA verwalten, während Sie Änderungen. Dies erfolgt über TSQL der aber SSMS ändern:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Schritt 8: Entfernen Sie sekundäre VM aus Cloud-Dienst

Sie sollten zuerst einen sekundärer Knoten Cloud migrieren planen ist dies derzeit primären, sollten Sie ein manuelles Failover initiieren.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Schritt 9: Festplatte Zwischenspeichern in CSV-Datei ändern und speichern

Für Daten-Volumes sollten diese READONLY festgelegt werden.

Nachverfolgungsprotokoll Volumes sollten diese auf NONE festgelegt.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Schritt 10: Kopieren virtueller Festplatten
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Überprüfen Sie den Kopierstatus die VHDs Storage Premium-Konto:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Warten Sie, bis diese Erfolg aufgezeichnet werden.

Informationen für einzelne Blobs:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Schritt 11: Register OS Datenträger

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Schritt 12: Importieren Sie sekundäre neue Cloud-Dienst

Der folgende Code verwendet auch die zusätzliche Option hier den Computer importieren und welche VIP verwenden.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Schritt 13: Erstellen ILB auf neue Cloud Svc hinzufügen Load Balanced Endpunkte und ACLs
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Schritt 14: Immer auf aktualisieren.
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Jetzt entfernen Sie alte Cloud-Dienst-IP-Adresse.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Schritt 15: DNS-Updates

Jetzt sollten Sie DNS-Server im Netzwerk SQL Server Client überprüfen und sicherstellen, dass clustering zusätzliche Hosteintrag für hinzugefügte IP-Adresse hat. Wenn dieser DNS-Server nicht aktualisiert, Erzwingen einer DNS-Zone Übertragung und stellen Sie sicher, dass die Clients es Subnetz sind beide immer auf IP-Adressen auflösen, daher nicht für die automatische DNS-Replikation warten müssen.

#### <a name="step-16-reconfigure-always-on"></a>Schritt 16: Immer auf Konfigurieren

Jetzt warten Sie die sekundäre dieser Knoten migriert wurde, um mit dem lokalen Knoten vollständig synchronisieren und wechseln Sie zum Knoten synchrone Replikation und der AFP.  

#### <a name="step-17-migrate-second-node"></a>Schritt 17: Zweiten Knoten migrieren
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Schritt 18: Datenträger Zwischenspeichern in CSV-Datei und speichern

Für Daten-Volumes sollten diese READONLY festgelegt werden.

Nachverfolgungsprotokoll Volumes sollten diese auf NONE festgelegt.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Schritt 19: Erstellen Sie neuen unabhängigen Speicherkonto für die sekundären Knoten
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Schritt 20: Kopieren virtueller Festplatten
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Überprüfen den VHD-Datei Kopierstatus für alle Festplatten: ForEach ($disk in $diskobjects) {$lun = $disk. LUN-$vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Diskettenetikett $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Warten Sie, bis diese Erfolg aufgezeichnet werden.

Informationen für einzelne Blobs: #Check tragen BLOB-Status abrufen AzureStorageBlobCopyState-Blob "DanRegSvcAms-dansqlams1-2014-07-03.vhd"-$containerName Container-Kontext $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Schritt 21: Register OS Datenträger
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Schritt 22: Hinzufügen von Load Balanced Endpunkte und ACLs
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Schritt 23: Test-failover

Sie sollten jetzt migrierten Knoten Lokale ständig Knoten synchronisieren, platzieren Sie ihn im Modus synchrone Replikation und erst synchronisiert ist. Dann migriert Failover von auf Prem auf dem ersten Knoten der AFP ist. Sobald die gearbeitet hat, den letzten migrierten Knoten zu wechseln der AFP

Testen Failover zwischen allen Knoten, und obwohl Chaos Tests für Failover Arbeit wie erwartet und rechtzeitig gut ausgeführt.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Schritt 24: Zurückgestellt Aktionsbereich / DNS TTL / Failover Pntrs / Sync-Optionen
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Subnetz-IP-Adressressource hinzufügen

Wenn nur 2 SQL Server und einer neuen Cloud-Dienst migrieren möchten aber sie im selben Subnetz möchten können Sie vermeiden, dauert des Listeners offline Löschen der ursprünglichen immer auf IP-Adresse und die neue IP-Adresse. Migration von VMs in ein anderes Subnetz müssen Sie nicht dazu werden eine zusätzlichen Cluster-Netzwerk, die diesem Subnetz verweisen.

Sobald die migrierten sekundäre gebracht und neue IP-Adressressource für neue Cloud-Dienst vor dem Failover des vorhandenen primären hinzugefügt nehmen Sie folgendermaßen in der Cluster-Failover-Manager:

IP-Adresse finden Sie unter [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)Schritt 14.

1. Die aktuelle IP-Adressressource ändern Sie den möglichen Besitzer "Vorhandene primäre SQL Server", im folgenden Beispiel, 'dansqlams4':

    ![Appendix13][23]

1. Die neue IP-Adressressource ändern Sie den möglichen Besitzer 'Migriert sekundären SQL Server ", im folgenden Beispiel, 'dansqlams5':

    ![Appendix14][24]

1. Sobald dies können Sie Failover und beim letzte Knoten migrieren sollten die möglichen Besitzer dieser Knoten als möglicher Besitzer hinzugefügt bearbeitet werden:

    ![Appendix15][25]

## <a name="additional-resources"></a>Zusätzliche Ressourcen
- [Azure Premium-Speicher](../storage/storage-premium-storage.md)
- [Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/)
- [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
