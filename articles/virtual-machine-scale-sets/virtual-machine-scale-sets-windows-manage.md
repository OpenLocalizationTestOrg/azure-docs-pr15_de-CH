<properties
    pageTitle="Verwalten von virtuellen Computern in einer virtuellen Maschine Skalierung | Microsoft Azure"
    description="Verwalten von virtuellen Computern in einer virtuellen Maschine Maßstab mit Azure PowerShell."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Verwaltung virtueller Computer in einer virtuellen Maschine skalieren

Verwenden Sie Aufgaben in diesem Artikel zum Verwalten von virtuellen Computern in der virtuellen Maschine Skalierung.

Die meisten der Aufgaben mit Management eines virtuellen Rechners in einer Skala müssen Sie die Instanz-ID des Computers kennen, die Sie verwalten möchten. [Azure-Ressourcen-Explorer](https://resources.azure.com) können Sie die Instanz-ID eines virtuellen Computers in einer Skala. Sie verwenden Sie Resource Explorer zum Überprüfen des Status der Aufgaben, die Sie abgeschlossen haben.

Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen und an Ihrem Konto anmelden.

## <a name="display-information-about-a-scale-set"></a>Informationen über eine Skalierungsgruppe

Sie erhalten Informationen über eine Skalierungsgruppe, auch auf die Instanzansicht genannt. Oder Sie erhalten weitere Informationen, wie Informationen zu den Ressourcen in der Skalierungsgruppe.

Ersetzen Sie die Werte in Anführungszeichen oder der Ressourcengruppe und Skalierung festlegen und dann den Befehl:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Es gibt etwa Folgendes:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
Den Namen der Ressource und Ersetzen Sie die Werte in Anführungszeichen. Ersetzen Sie *#* mit der Instanz-ID des virtuellen Computers die gewünschten Informationen und dann ausführen:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Es gibt etwa diesem Beispiel:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Starten eines virtuellen Computers in einer Skala

Den Namen der Ressource und Ersetzen Sie die Werte in Anführungszeichen. Ersetzen Sie *#* mit der ID die virtuellen Computer starten und ausführen:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Im Ressourcen-Explorer sehen wir, dass der Status der Instanz **ausgeführt**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Skalierung einstellen nicht InstanceId - Parameter können Sie die virtuellen Computer starten.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Beenden Sie einen virtuellen Computer in einer Skala

Den Namen der Ressource und Ersetzen Sie die Werte in Anführungszeichen. Ersetzen Sie *#* mit der ID des virtuellen Computers die zu beenden, und führen es:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Im Ressourcen-Explorer sehen wir, dass der Status der Instanz **freigegeben**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Eine virtuelle Maschine nicht freigeben und verwenden Sie StayProvisioned - Parameter. Sie können alle virtuellen Computer in der Gruppe nicht mit dem Parameter - InstanceId beenden.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Starten Sie einen virtuellen Computer in einer Skala

Den Namen der Ressourcengruppe und die Skalierung ersetzen Sie der Werte in Anführungszeichen. Ersetzen Sie *#* mit der ID der virtuellen Computer neu starten und dann ausführen:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
Sie können alle virtuellen Computer in den neu starten nicht mit dem Parameter - InstanceId.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Entfernen Sie einen virtuellen Computer aus einer Skalierung

Den Namen der Ressourcengruppe und die Skalierung ersetzen Sie der Werte in Anführungszeichen. Ersetzen Sie *#* mit der ID des virtuellen Computers, die Sie entfernen möchten und führen Sie es:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Sie können die virtuellen Computer skalieren gleichzeitig entfernen nicht mit der InstanceId - Parameter.

## <a name="change-the-capacity-of-a-scale-set"></a>Ändern Sie die Kapazität einer Gruppe skalieren

Sie können hinzufügen oder Entfernen virtueller Maschinen durch Ändern der Speicherkapazität des Satzes. Erhalten Sie festgelegten Skala zu ändern, legen Sie die Kapazität zu sein, aktualisieren den Maßstab mit neuen. Ersetzen Sie in diesen Befehlen die angegebenen Werte mit den Namen der Ressourcengruppe und den Maßstab.

  $vmss = Get-AzureRmVmss - ResourceGroupName "Ressourcengruppenname" - VMScaleSetName "Namen skalieren" $vmss.sku.capacity = 5 Update-AzureRmVmss - ResourceGroupName "Ressourcengruppenname"-Name "scale Namen" - VirtualMachineScaleSet $vmss 

Beim Entfernen von VMs aus der Skalierung werden die virtuellen Computer mit den höchsten Ids zunächst entfernt.
