<properties
    pageTitle="Ressourcen-Manager mit PowerShell VMs verwalten | Microsoft Azure"
    description="Verwalten Sie virtuelle Maschinen mit Azure Resource Manager und PowerShell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Verwalten von Azure Virtual Machines Ressourcenmanager mit PowerShell

## <a name="install-azure-powershell"></a>Azure PowerShell installieren
 
Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen und an Ihrem Konto anmelden.

## <a name="set-variables"></a>Festgelegten Variablen

Alle Befehle im Artikel erfordern den Namen der Ressourcengruppe, der virtuelle Computer befindet, und der Name des virtuellen Computers zu verwalten. Ersetzen Sie den Wert des **$rgName** mit dem Namen der Ressource, die dem virtuellen Computer enthält. Ersetzen Sie den Wert des **$vmName** mit dem Namen des virtuellen Computers. Erstellen Sie die Variablen.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Anzeigen von Informationen zu einem virtuellen Computer

Erhalten Sie die virtuellen Computerinformationen.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Es gibt etwa diesem Beispiel:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Eine virtuelle Maschine

Die ausgeführten virtuellen Computer anhalten.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Sie werden zur Bestätigung aufgefordert:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Geben Sie **Y** ein, um die virtuelle Maschine.

Nach einigen Minuten wird etwas dabei:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Starten eines virtuellen Computers

Starten Sie den virtuellen Computer, wenn er beendet wird.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Nach einigen Minuten wird etwas dabei:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Neustarten eines virtuellen Computers, das bereits ausgeführt werden soll, beschrieben mit **Neustart AzureRmVM** weiter.

## <a name="restart-a-virtual-machine"></a>Starten Sie einen virtuellen Computer

Starten Sie ausgeführten virtuellen Computer neu.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Es gibt etwa diesem Beispiel:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Löschen Sie einen virtuellen Computer

Löschen Sie den virtuellen Computer.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Können Sie die **-Force** Parameter der Bestätigung überspringen.

Verwenden Sie nicht den Parameter Force aufgefordert bestätigen:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Es gibt etwa diesem Beispiel:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Aktualisieren einer virtuellen Maschine

Dieses Beispiel zeigt, wie Sie die Größe des virtuellen Computers aktualisieren.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Es gibt etwa diesem Beispiel:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Finden Sie eine Liste der verfügbaren Größen für eine virtuelle Maschine [Größen für virtuelle Computer in Azure](virtual-machines-windows-sizes.md) .

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Einem virtuellen Computer einen Datenträger hinzufügen

Dieses Beispiel zeigt einen Datenträger vorhandenen virtuellen Computer hinzufügen.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Der Datenträger, den Sie hinzufügen ist nicht initialisiert. Um zu initialisieren, können sie anmelden und Volume. Wenn Sie installiert WinRM und ein Zertifikat beim Erstellen, können Sie remote-PowerShell, initialisieren. Sie können auch eine benutzerdefiniertes Skript-Erweiterung: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Die Skriptdatei kann etwa diesen Code zum Initialisieren der Datenträger enthalten:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Nächste Schritte

Würde Probleme eine Bereitstellung, können Sie [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) anzeigen
