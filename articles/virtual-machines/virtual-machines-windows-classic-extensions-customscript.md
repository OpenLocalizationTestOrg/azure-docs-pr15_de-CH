<properties
   pageTitle="Benutzerdefinierte Skripts Erweiterung auf einer Windows-VM | Microsoft Azure"
   description="Automatisieren von Konfigurationsaufgaben Azure VM mithilfe von benutzerdefinierten Skripts Erweiterung auf einem virtuellen Computer remote Windows PowerShell-Skripts ausführen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Benutzerdefinierte Skript-Erweiterung für Windows virtuelle Maschinen

Dieser Artikel bietet eine Übersicht über Verwendung benutzerdefinierten Skripts Erweiterung auf virtuellen Computern Windows Azure Service Management APIs mit Azure PowerShell-Cmdlets.

Virtual Machine (VM) Extensions von Microsoft erstellt und vertrauenswürdige Herausgeber von Drittanbietern zum Erweitern der Funktionalität des virtuellen Computers. Übersicht der VM-Erweiterungen finden Sie unter [Azure VM-Erweiterungen und Funktionen](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Benutzerdefinierte Skripts Erweiterung (Übersicht)

Mit der Erweiterung benutzerdefinierte Skript für Windows führen PowerShell-Skripts auf einer remote-VM Sie ohne Anmeldung zu. Sie können Skripts nach der Bereitstellung der VM oder jederzeit während des Lebenszyklus der VM ausführen, ohne zusätzliche Ports öffnen. Gängige Anwendungsbeispiele für die Ausführung von benutzerdefinierten Skripts Erweiterung enthalten ausführen, installieren und konfigurieren zusätzlichen Software auf dem virtuellen Computer, nachdem sie bereitgestellt wurde.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Voraussetzung für die Ausführung von benutzerdefinierten Skripts Erweiterung

1. Installieren von <a href="http://azure.microsoft.com/downloads" target="_blank">Azure PowerShell-Cmdlets</a> Version 0.8.0 oder höher.
2. Wenn Skripts auf einen vorhandenen virtuellen Computer ausgeführt werden soll, unbedingt VM-Agent auf dem virtuellen Computer aktiviert ist. Wenn es nicht installiert ist, führen Sie diese [Schritte](virtual-machines-windows-classic-agents-and-extensions.md). Wenn die VM von Azure-Portal erstellt wird, ist VM-Agent standardmäßig installiert.
3. Upload-Skripten, die in Azure-Speicher auf dem virtuellen Computer ausführen möchten. Die Skripts kommen in einem einzelnen Container oder mehrere Speichercontainer.
4. Das Skript sollte erstellt werden, sodass Skript Eintrag über die Erweiterung gestartet wird, andere Skripts beginnt.

## <a name="custom-script-extension-scenarios"></a>Erweiterung Skript Szenarien

### <a name="upload-files-to-the-default-container"></a>Der standardmäßige Container Dateien hochladen

Das folgende Beispiel zeigt, wie Ausführen der Skripts auf dem virtuellen Computer sind in der Box des Standardkontos Ihres Abonnements. Sie können Skripts auf ContainerName hochladen. Überprüfen das Standardkonto Speicher mit der **Get-AzureSubscription-Standard** Befehl.

Im folgenden Beispiel wird einen virtueller Computer, aber dasselbe Szenario kann auch auf einen vorhandenen virtuellen Computer ausgeführt.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Hochladen von Dateien auf einen nicht standardmäßigen Behälter

Dieses Szenario veranschaulicht, wie eine nicht standardmäßige Behälter innerhalb des gleichen Abonnements oder in ein anderes Abonnement für Skripts und Dateien hochladen. Dieses Beispiel zeigt eine vorhandene VM, aber dieselben Operationen können beim Erstellen einer VM ausgeführt werden.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Upload-Skripten mit mehreren Containern verschiedene Konten

  Wenn die Skriptdateien auf mehrere Container gespeichert sind, müssen Sie freigegebenen Vollzugriff Signaturen (SAS) URL für die Dateien zum Ausführen der Skripts bereitstellen.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Fügen Sie die benutzerdefinierten Skripts Erweiterung von Azure-portal

VM in <a href="https://portal.azure.com/ " target="_blank">Azure-Portal</a> wechseln Sie und fügen Sie die Erweiterung durch Angabe der Skriptdatei ausgeführt.

  ![Skriptdatei angeben][5]


### <a name="uninstall-the-custom-script-extension"></a>Die benutzerdefinierten Skripts Erweiterung deinstallieren

Mit dem folgenden Befehl können Sie die benutzerdefinierten Skripts Erweiterung vom virtuellen Computer deinstallieren.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Verwenden Sie die benutzerdefinierten Skripts Erweiterung mit Vorlagen

Zur Verwendung von benutzerdefinierten Skripts Erweiterung Azure Ressourcenmanager Vorlagen finden Sie unter [benutzerdefinierte Skript Erweiterung für VMs Windows Azure-Ressourcen-Manager-Vorlagen verwenden](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png
