<properties
   pageTitle="Bereitstellen von Multi-NIC VMs mit PowerShell im klassischen Bereitstellungsmodell | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von Multi-NIC VMs mit PowerShell im klassischen Bereitstellungsmodell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Bereitstellen von Multi-NIC VMs (classic) mit PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Sie können virtuelle Maschinen (VMs) in Azure erstellen und jeder Ihrer virtuellen Computer mehrere Netzwerkkarten (NICs) zuordnen. Mehrere Netzwerkkarten ermöglichen Trennung der Datenverkehr über Netzwerkkarten. Beispielsweise könnte eine Netzwerkkarte mit dem Internet kommunizieren, während anderen nur mit internen Ressourcen nicht mit dem Internet verbunden kommuniziert. Trennen Sie den Netzwerkverkehr über mehrere Netzwerkkarten ist für viele virtuelle Netzwerkgeräte, wie Bereitstellung und WAN-optimierungslösungen erforderlich.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Derzeit sein nicht virtueller Computer mit einer NIC und VMs mit mehreren NICs im gleichen Clouddienst werden. Daher müssen Sie die Back-End-Server in eine andere Cloud-Dienst als und anderen Komponenten im Szenario implementieren. Die folgenden Schritte verwenden einen Cloud-Dienst mit dem Namen *IaaSStory* für die wichtigsten Ressourcen und *IaaSStory-Back-End-* Back-End-Server.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie die Back-End-Server bereitstellen können, müssen Sie main Cloud-Dienst mit den erforderlichen Ressourcen für dieses Szenario bereitstellen. Zumindest müssen Sie ein virtuelles Netzwerk mit einem Subnetz für das Backend erstellen. Besuchen Sie [erstellen ein virtuelles Netzwerk mit PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md) Informationen zum Bereitstellen eines virtuellen Netzwerks.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Bereitstellen der Back-End-VMs

Die Back-End-VMs hängt die Erstellung der unten aufgeführten Ressourcen.

- **Back-End-Subnetz**. Der Datenbankserver werden ein separates Subnetz Aufteilen des Datenverkehrs gehören. Das folgende Skript erwartet dieses Subnetz in ein Vnet mit dem Namen *WTestVnet*vorhanden sein.
- Das **Speicherkonto für Datenträger**. Für eine bessere Leistung verwendet die Datenträger auf den Datenbankservern solid State Drive (SSD)-Technologie erfordert ein Speicherkonto Premium. Vergewissern Sie sich Azure Speicherort bereitstellen, um Premium-Speicher unterstützen.
- **Verfügbarkeit**. Alle Datenbankserver werden einzelne Verfügbarkeit festgelegt, um sicherzustellen, dass mindestens die VMs einrichten und Wartungsarbeiten ausführen hinzugefügt werden.

### <a name="step-1---start-your-script"></a>Schritt 1 - Skript starten

Sie können das vollständige PowerShell-Skript verwendet [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Gehen Sie folgendermaßen vor, so ändern Sie das Skript in Ihrer Umgebung funktionieren.

1. Ändern Sie die Werte der folgenden Variablen anhand der vorhandenen Ressourcengruppe über [Komponenten](#Prerequisites)bereitgestellt.

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Ändern Sie die Werte der folgenden Variablen basierend auf den Werten für die Back-End-Bereitstellung verwenden möchten.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Schritt 2 - Erstellen der erforderlichen Ressourcen für Ihre VMs

Sie müssen einen neuen Clouddienst und ein Speicherkonto für die Datenträger für alle virtuellen Computer erstellen. Sie möchten ein Bild und ein lokales Administratorkonto für die virtuellen Computer angeben. Um diese Ressourcen zu erstellen, führen Sie die folgenden Schritte aus.

1. Erstellen Sie einen neuen Clouddienst.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Erstellen Sie ein neue Premium-Speicherkonto.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Als aktuelle Speicherkonto für Ihr Abonnement oben erstellte Speicherkonto festgelegt.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Wählen Sie ein Bild für die VM.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Festlegen Sie der lokale Administrator Anmeldeinformationen.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Schritt 3 - VMs erstellen

Sie müssen mit einer Schleife, und erstellen Sie die erforderlichen NICs und VMs innerhalb der Schleife so viele virtuelle Computer erstellen. Netzwerkkarten und VMs erstellen, führen Sie die folgenden Schritte aus.

1. Starten einer `for` Schleife wiederholt die Befehle zum Erstellen einer VM und zwei NICs so oft wie erforderlich, basierend auf dem Wert der `$numberOfVMs` Variable.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Erstellen einer `VMConfig` Objekt, Bild, Größe und Verfügbarkeit für die VM festgelegt.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. Bereitstellung der VM als Windows VM.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. NIC festlegen und eine statische IP-Adresse zuweisen.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Fügen Sie eine zweite Netzwerkkarte für jede VM.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Auf Datenträger für jeden virtuellen Computer erstellen.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Jede VM erstellen, und die Schleife.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Schritt 4: Führen Sie das Skript

Heruntergeladen und Skript Ihren Bedürfnissen entsprechend geändert Skript zwerg er erstellen das Back-End Datenbank VMs mit mehreren NICs.

1. Speichern Sie Ihr Skript und **PowerShell** -Befehlszeile oder **PowerShell ISE**ausführen. Erste Ausgabe sehen, wie unten dargestellt.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Füllen Sie die nötigen Anmeldeinformationen aufgefordert, und klicken Sie auf **OK**. Die folgende Ausgabe wird angezeigt.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
