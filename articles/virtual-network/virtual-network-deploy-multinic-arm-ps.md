<properties
   pageTitle="Bereitstellen von Multi-NIC VMs mit PowerShell in Ressourcen-Manager | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von Multi-NIC VMs mit PowerShell in Ressourcen-Manager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-powershell"></a>Bereitstellen von Multi-NIC VMs mit PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Derzeit sein nicht virtueller Computer mit einer NIC und VMs mit mehreren NICs in Verfügbarkeit dieselben werden. Daher müssen Sie die Back-End-Server in einer anderen Ressourcengruppe als alle anderen Komponenten implementieren. Die folgenden Schritte verwenden eine Ressourcengruppe mit dem Namen *IaaSStory* für die wichtigsten Ressourcengruppe und *IaaSStory-Back-End-* Back-End-Server.

## <a name="prerequisites"></a>Erforderliche Komponenten

Vor der Bereitstellung von Back-End-Servern müssen Sie die wichtigsten Ressourcengruppe mit den erforderlichen Ressourcen für dieses Szenario bereitstellen. Gehen Sie folgendermaßen vor um diese Ressourcen bereitzustellen.

1. Navigieren Sie zur [Vorlagenseite](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klicken Sie auf der Vorlagenseite neben **übergeordneten Ressourcengruppe** **Bereitstellen in Azure**.
3. Ggf. ändern Sie Parameterwerte, und führen Sie die Schritte im vorschauportal Azure die Ressourcengruppe bereitstellen.

> [AZURE.IMPORTANT] Stellen Sie sicher, dass Ihre Storage Namen eindeutig sind. Sie keinen Kontonamen doppelter Speicher in Azure.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Bereitstellen der Back-End-VMs

Die Back-End-VMs hängt die Erstellung der unten aufgeführten Ressourcen.

- Das **Speicherkonto für Datenträger**. Für eine bessere Leistung verwendet die Datenträger auf den Datenbankservern solid State Drive (SSD)-Technologie erfordert ein Speicherkonto Premium. Vergewissern Sie sich Azure Speicherort bereitstellen, um Premium-Speicher unterstützen.
- **Netzwerkkarten**. Jede VM haben zwei Netzwerkkarten Datenbank zugreifen und Management.
- **Verfügbarkeit**. Alle Datenbankserver werden einzelne Verfügbarkeit festgelegt, um sicherzustellen, dass mindestens die VMs einrichten und Wartungsarbeiten ausführen hinzugefügt werden.  

### <a name="step-1---start-your-script"></a>Schritt 1 - Skript starten

Sie können das vollständige PowerShell-Skript verwendet [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1). Gehen Sie folgendermaßen vor, so ändern Sie das Skript in Ihrer Umgebung funktionieren.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Ändern Sie die Werte der folgenden Variablen anhand der vorhandenen Ressourcengruppe über [Komponenten](#Prerequisites)bereitgestellt.

        $existingRGName        = "IaaSStory"
        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"
        $remoteAccessNSGName   = "NSG-RemoteAccess"
        $stdStorageAccountName = "wtestvnetstoragestd"

2. Ändern Sie die Werte der folgenden Variablen basierend auf den Werten für die Back-End-Bereitstellung verwenden möchten.

        $backendRGName         = "IaaSStory-Backend"
        $prmStorageAccountName = "wtestvnetstorageprm"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $publisher             = "MicrosoftSQLServer"
        $offer                 = "SQL2014SP1-WS2012R2"
        $sku                   = "Standard"
        $version               = "latest"
        $vmNamePrefix          = "DB"
        $osDiskPrefix          = "osdiskdb"
        $dataDiskPrefix        = "datadisk"
        $diskSize              = "120"  
        $nicNamePrefix         = "NICDB"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

3. Abrufen der vorhandenen Mittel für die Bereitstellung.

        $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
        $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
        $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
        $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Schritt 2 - Erstellen der erforderlichen Ressourcen für Ihre VMs

Sie müssen eine neue Ressourcengruppe erstellen ein Speicherkonto für die Datenträger und Verfügbarkeit für VMs festgelegt. Benötigen Sie auch lokaler Administrator-Anmeldeinformationen für jede VM. Um diese Ressourcen zu erstellen, führen Sie die folgenden Schritte aus.

1. Erstellen Sie eine neue Ressourcengruppe.

        New-AzureRmResourceGroup -Name $backendRGName -Location $location

2. Erstellen Sie neue Premium-Speicherkonto oben erstellten Ressourcengruppe.

        $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
            -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location

3. Erstellen einer neuen Verfügbarkeit.

        $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location

4. Erhalten Sie der lokale Administrator jeder VM Kontoanmeldeinformationen.

        $cred = Get-Credential -Message "Type the name and password for the local administrator account."

### <a name="step-3---create-the-nics-and-backend-vms"></a>Schritt 3 - NICs und Back-End-VMs erstellen

Sie müssen mit einer Schleife, und erstellen Sie die erforderlichen NICs und VMs innerhalb der Schleife so viele virtuelle Computer erstellen. Netzwerkkarten und VMs erstellen, führen Sie die folgenden Schritte aus.

1. Starten einer `for` Schleife wiederholt die Befehle zum Erstellen einer VM und zwei NICs so oft wie erforderlich, basierend auf dem Wert der `$numberOfVMs` Variable.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Erstellen Sie die Netzwerkkarte für den Datenbankzugriff verwendet.

            $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
            $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
            $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1

3. Erstellen Sie die Netzwerkkarte für den Remotezugriff verwendet. Beachten Sie, wie diese Netzwerkkarte eine NSG zugeordnet wurde.

            $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
            $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
            $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
                -NetworkSecurityGroupId $remoteAccessNSG.Id

4. Erstellen `vmConfig` Objekt.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id

5. Erstellen Sie zwei Datenträger pro VM. Beachten Sie, dass der Datenträger in der zuvor erstellten Premium Speicherkonto.

            $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"    
            $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
                -VhdUri $data1VhdUri -CreateOption empty -Lun 0

            $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"    
            $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
                -VhdUri $data2VhdUri -CreateOption empty -Lun 1

6. Konfigurieren des Betriebssystems und image für den virtuellen Computer verwendet werden.

            $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
            $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version

7. Fügen Sie die beiden Karten, die oben erstellte der `vmConfig` Objekt.

            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id

8. Erstellen Sie den Betriebssystem-Datenträger und die VM. Beachten Sie die `}` endet die `for` Schleife.

            $osDiskName = $vmName + "-" + $osDiskSuffix
            $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
            $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
            New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
        }

### <a name="step-4---run-the-script"></a>Schritt 4: Führen Sie das Skript

Heruntergeladen und Skript Ihren Bedürfnissen entsprechend geändert Skript zwerg er erstellen das Back-End Datenbank VMs mit mehreren NICs.

1. Speichern Sie Ihr Skript und **PowerShell** -Befehlszeile oder **PowerShell ISE**ausführen. Erste Ausgabe sehen, wie unten dargestellt.

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        ResourceId        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/IaaSStory-Backend

2. Füllen Sie nach einigen Minuten Anmeldeinformationen aufgefordert, und klicken Sie auf **OK**. Die folgende Ausgabe stellt eine VM. Beachten Sie den gesamten Prozess hat 8 Minuten gedauert.

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        EndTime             : 10/30/2015 9:30:03 AM -08:00
        Error               :
        Output              :
        StartTime           : 10/30/2015 9:22:54 AM -08:00
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK
