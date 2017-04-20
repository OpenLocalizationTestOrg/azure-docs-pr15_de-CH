<properties
    pageTitle="Erstellen Sie einen SQL Server virtueller Computer in Azure PowerShell (klassisch) | Microsoft Azure"
    description="Enthält Schritte und PowerShell-Skripts zum Erstellen einer Azure VM mit SQL Server virtuellen Katalog. Dieses Thema verwendet das klassische Bereitstellungsmodus."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Bereitstellen eines SQL Server-virtuellen Computers mithilfe von Azure PowerShell (klassisch)

## <a name="overview"></a>Übersicht

Dieser Artikel enthält Schritte zum Erstellen eine SQL Server-VM in Azure mithilfe von PowerShell-Cmdlets.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Der Ressourcen-Manager-Version dieses Themas finden Sie unter [Bereitstellen einer Azure PowerShell Ressourcenmanager mit SQL Server virtuellen Computer](virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Installieren und Konfigurieren von PowerShell:

1. Wenn Sie nicht über ein Azure-Konto verfügen, besuchen Sie [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

2. [Herunterladen und installieren die neuesten Azure PowerShell-Befehlen](../powershell-install-configure.md).

3. Starten Sie Windows PowerShell und Azure Abonnements mit dem Befehl **Add-AzureAccount** an.

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Bestimmt das Ziel Azure-region

SQL Server virtuellen Computers wird in einem Clouddienst gehostet werden, die einen bestimmten Azure Bereich befindet. Die folgenden Schritte ermittelt Ihre Region, Speicherkonto und cloud-Dienst, für den Rest des Lernprogramms verwendet werden.

1. Bestimmen Sie das Rechenzentrum Ihrer SQL Server-VM gehostet werden soll. Die folgenden PowerShell Befehle werden verfügbaren Regionen mit einer zusammenfassenden Liste Ende angezeigt.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Wenn Ihr Standort ermittelt haben, legen Sie eine Variable namens **$dcLocation** für diese Region.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Ihr Abonnement und Storage-Konto festlegen

1. Bestimmen des Azure-Abonnements für den neuen virtuellen Computer verwenden.

        (Get-AzureSubscription).SubscriptionName

1. Ziel Azure-Abonnement der Variablen **$subscr** zuzuweisen. Legen Sie dies als Ihre Azure-Abonnement.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Überprüfen Sie, ob vorhandene Speicherkonten. Das folgende Skript zeigt alle Speicherkonten in Ihrem ausgewählten Bereich:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Benötigen Sie ein neues Speicherkonto, erstellen Sie zunächst einen Kontonamen alle Kleinbuchstaben Speicher mit dem Befehl neu AzureStorageAccount wie im folgenden Beispiel: **AzureStorageAccount New - StorageAccountName "<storage account name>"-Speicherort $dcLocation**

1. **$Staccount**der Zielkontoname Speicher zuweisen. Können Sie **Set AzureSubscription** die Abonnements und aktuelle Speicherkonto festgelegt.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>SQL Server virtuellen Abbild auswählen

1. Finden Sie die Liste der verfügbaren SQL Server virtuellen Maschinen Bilder aus der Galerie. Alle Bilder haben eine **ImageFamily** -Eigenschaft, die mit "SQL". Die folgende Abfrage zeigt die Familie Bild, dass SQL Server vorinstalliert.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Beim virtuellen Computer Bild-Familie Sie finden möglicherweise mehrere veröffentlichte Bilder in dieser Familie. Verwenden Sie das folgende Skript zu den neuesten veröffentlichten VM Abbildnamen Zuhause ausgewähltes Bild (z. B. **SQL Server 2016 RTM Enterprise auf Windows Server 2012 R2**):

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>Erstellen der virtuellen Computer

Abschließend erstellen Sie den virtuellen Computer mit PowerShell:

1. Erstellen Sie einen Cloud-Dienst zum Hosten von neuen VM. Beachten Sie, dass es auch möglich, einen vorhandenen Cloud-Dienst verwenden. Erstellen Sie eine neue Variable **$svcname** Kurznamen Cloud-Dienst.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Name des virtuellen Computers und einer Größe angeben Weitere Informationen zu virtuellen Größen finden Sie [Größen für Azure Virtual Machine](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Geben Sie das lokale Administratorkonto und Kennwort.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Führen Sie das folgende Skript zum Erstellen des virtuellen Computers.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Zusätzliche Erläuterung und Konfigurationsoptionen finden Sie im Abschnitt **Erstellen der Befehlssatz** in [Verwendung Azure PowerShell zu Windows-basierten virtuellen Maschinen vorkonfigurieren](virtual-machines-windows-classic-create-powershell.md).

## <a name="example-powershell-script"></a>PowerShell-Beispielskript

Das folgende Skript enthält und ein vollständiges Skript Beispiel erstellt einen **SQL Server 2016 RTM Enterprise auf Windows Server 2012 R2** virtuellen Computer. Wenn Sie dieses Skript verwenden, müssen Sie die anfänglichen Variablen anhand der Schritte in diesem Thema anpassen.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Verbinden mit Remotedesktop

1. Erstellen der. RDP-Dateien im Dokument-Ordner des Benutzers auf diese virtuellen Computer zum Abschließen der Installation starten:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. Starten Sie die RDP-Datei im Verzeichnis Dateien. Verbinden mit Administrator-Benutzernamen und Kennwort zuvor (beispielsweise wurde Ihr Benutzername VMAdmin, geben Sie "\VMAdmin" als Benutzer und das Kennwort).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Schließen Sie die Konfiguration des SQL Server-Computers für den Remotezugriff

Konfigurieren Sie nach der Anmeldung auf dem Computer mit Remotedesktop SQL Server anhand der Anleitung im [Schritte zum Konfigurieren von SQL Server-Konnektivität in Azure-VM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Hinweise zur Bereitstellung virtueller Maschinen mit PowerShell in der [Dokumentation zu virtuellen Maschinen](virtual-machines-windows-classic-create-powershell.md)finden. Zusätzliche Skripts im Zusammenhang mit SQL Server und Storage Premium finden Sie unter [Verwenden Azure Premium-Speicher mit SQL Server auf virtuellen Computern](virtual-machines-windows-classic-sql-server-premium-storage.md).

In vielen Fällen ist der nächste Schritt die Datenbanken für diese neue SQL Server-VM migriert. Datenbank Migration Siehe [Migrieren einer Datenbank in SQL Server auf eine Azure-VM](virtual-machines-windows-migrate-sql.md).

Wenn Sie auch zum Erstellen virtueller Computer SQL Azure-Portal verwenden, finden Sie unter [Bereitstellen einer SQL Server-VM in Azure](virtual-machines-windows-portal-sql-server-provision.md). Beachten Sie, dass das Lernprogramm, das Sie über das Portal führt erstellt VMs mit empfohlenen Ressourcen-Manager-Modell, sondern Klassisch in diesem Thema PowerShell verwendet.

Neben diesen Ressourcen empfiehlt [anderen Themen im Zusammenhang mit SQL Server in Azure-Computer](virtual-machines-windows-sql-server-iaas-overview.md)überprüfen.
