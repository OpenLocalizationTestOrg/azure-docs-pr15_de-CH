<properties 
   pageTitle="Allgemeine PowerShell-Befehlen für VMs | Microsoft Azure"
   description="Allgemeine PowerShell-Befehlen zum Erstellen und verwalten Ihre virtuellen Computer in Windows Azure Einstieg"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Allgemeine PowerShell-Befehlen zum Erstellen und Verwalten von VMs

Dieser Artikel behandelt einige Azure PowerShell-Befehle, die Sie zum Erstellen und Verwalten von virtuellen Computern in der Azure-Abonnement verwenden können.  Ausführliche Hilfe zu bestimmten Befehlszeilenoptionen können Sie **Hilfe** *Befehl*.

Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen und an Ihrem Konto anmelden.

## <a name="create-a-vm"></a>Erstellen Sie einen virtuellen Computer

Aufgabe | Befehl
-------------- | -------------------------
Erstellen Sie eine VM-Konfiguration | $vm = [New AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName "Vm_name" - VMSize "Vm_size"<BR></BR><BR></BR>Die VM-Konfiguration zum Definieren oder Einstellungen für die VM. Die Konfiguration wird mit dem Namen der VM und dessen [Größe](virtual-machines-windows-sizes.md)initialisiert.
Fügen Sie die | $vm = $vm [Set AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - VM-Windows - ComputerName "Computername"-Anmeldeinformationen $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Standardeinstellungen des Betriebssystems einschließlich [Anmeldeinformationen](https://technet.microsoft.com/library/hh849815.aspx) werden Configuration-Objekt hinzugefügt, die bereits mit AzureRmVMConfig neu erstellt.
Hinzufügen einer Netzwerkschnittstelle | $vm = $vm [Add-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - VM-Id $NIC ID<BR></BR><BR></BR>Ein virtueller Computer benötigen eine [Netzwerkschnittstelle](virtual-machines-windows-ps-create.md) in einem virtuellen Netzwerk kommunizieren. [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) können Sie eine vorhandene Netzwerkschnittstellenobjekt abrufen.
Geben Sie eine Plattform-Images | $vm = Herausgebername" [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) - VM $vm - PublisherName"-"Publisher_offer" "Product_sku" - Skus bieten-Version "neueste"<BR></BR><BR></BR>[Bildinformationen](virtual-machines-windows-cli-ps-findimage.md) wird das Konfigurationsobjekt hinzugefügt, die bereits mit AzureRmVMConfig neu erstellt. Dieser Befehl zurückgegebene Objekt wird nur verwendet, wenn Betriebssystemdatenträger mit einem Plattform-Image festgelegt.
Legen Sie Betriebssystemdatenträger mit einem Plattform-image | $vm = $vm [Set AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - VM-Name "Disk_name" - VhdUri "http://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - CreateOption FromImage<BR></BR><BR></BR>Name Betriebssystem und [dessen Speicherort](../storage/storage-powershell-guide-full.md) ist das Konfigurationsobjekt hinzugefügt, die Sie zuvor erstellt haben.
Festlegen Sie Betriebssystem-Datenträger ein generalisiertes Abbild verwenden | $vm = $vm Set AzureRmVMOSDisk - VM-Name "Disk_name" - SourceImageUri "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk. {Guid} VHD"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>Das Configuration-Objekt der Namen der Betriebssystem-CD, die Position des Bildes und Speicherort im [Speicher](../storage/storage-powershell-guide-full.md) hinzugefügt.
Legen Sie Betriebssystemdatenträger ein spezielles Bild verwenden | $vm = $vm Set AzureRmVMOSDisk - VM-Namen "Name_of_disk" - VhdUri "http://mystore1.blob.core.windows.net/vhds/" CreateOption - Attach - Windows
Erstellen Sie einen virtuellen Computer | "Resource_group_name" [Neu AzureRmVM]() - ResourceGroupName-Speicherort "Location_name" - VM $vm<BR></BR><BR></BR>Alle Ressourcen werden in einer [Ressourcengruppe](../powershell-azure-resource-manager.md)erstellt. Die VM muss am gleichen [Standort](https://msdn.microsoft.com/library/azure/dn495177.aspx) wie die Ressourcengruppe erstellt werden. Führen Sie vor dem Ausführen dieses Befehls neu AzureRmVMConfig, Set AzureRmVMOperatingSystem Set AzureRmVMSourceImage, hinzufügen AzureRmVMNetworkInterface und AzureRmVMOSDisk festlegen.

## <a name="get-information-about-vms"></a>Informationen über VMs

Aufgabe | Befehl
-------------- | -------------------------
Liste VMs in einem Abonnement| [AzureRmVM abrufen](https://msdn.microsoft.com/library/mt603718.aspx)
Liste virtueller Computer in einer Ressourcengruppe | Get-AzureRmVM - ResourceGroupName "Resource_group_name"<BR></BR><BR></BR>Um eine Liste der Ressourcengruppen in Ihrem Abonnement abzurufen, verwenden Sie [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
Informationen Sie über einen virtuellen Computer | "Resource_group_name" Get-AzureRmVM - ResourceGroupName-Namen "Vm_name"

## <a name="manage-vms"></a>Verwalten von virtuellen Computern

Aufgabe | Befehl
-------------- | -------------------------
Starten Sie einen virtuellen Computer | "Resource_group_name" [Start-AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName-Namen "Vm_name"
Beenden einer VM | "Resource_group_name" [Stop-AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName-Namen "Vm_name"
Starten Sie einen ausgeführten virtuellen Computer | "Resource_group_name" [Neustart-AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName-Namen "Vm_name"
Löschen einer VM | "Resource_group_name" [Remove-AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName-Namen "Vm_name"
Verallgemeinern einer VM | [Set-AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-Namen "Vm_name"-Generalized<BR></BR><BR></BR>Führen Sie diesen Befehl, bevor Sie speichern AzureRmVMImage ausführen.
Erfassen einer VM | [Speichern-AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "Resource_group_name" - VMName "Vm_name" - DestinationContainerName "Image_container" - VHDNamePrefix "Image_name_prefix"-Pfad "C:\filepath\filename.json"<BR></BR><BR></BR>Ein virtuellen Computer muss [heruntergefahren und generalisierte](virtual-machines-windows-generalize-vhd.md) verwendet werden, um ein Abbild zu erstellen. Bevor Sie diesen Befehl ausführen, führen Sie AzureRmVm festlegen.
Aktualisieren einer VM | [Update-AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "Resource_group_name" - VM $vm<BR></BR><BR></BR>Abrufen der aktuellen VM-Konfigurations mit Get-AzureRmVM und führen Sie diesen Befehl auf VM-Objekt ändern.
Ein virtueller Computer einen Datenträger hinzufügen | $Vm [Add-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - VM-Name "Disk_name" - VhdUri "https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - LUN #-Caching ReadWrite - DiskSizeinGB # - CreateOption leeren<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM VM-Objekt abrufen. Geben Sie die LUN-Anzahl und Größe der Festplatte. Update-AzureRmVM, um die Konfiguration der VM Änderungen zu führen. Der Datenträger, den Sie hinzufügen ist nicht initialisiert. Informationen zum Initialisieren von Festplatten hinzugefügt werden finden Sie unter [Verwalten von Azure Virtual Machines verwenden Ressourcenmanager und PowerShell](virtual-machines-windows-ps-manage.md).
Ein virtueller Computer einen Datenträger entfernen | $Vm [Entfernen AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - VM-Name "Laufwerkname"<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM VM-Objekt abrufen. Update-AzureRmVM, um die Konfiguration der VM Änderungen zu führen.
Hinzufügen einer Erweiterung einer VM | [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName "Resource_group_name"-Speicherort "Azure_location" - VMName "Vm_name"-Namen "Extension_name"-Publisher "Herausgebername"-Typ "Extension_type" - TypeHandlerVersion "##"-Einstellungen $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Führen Sie diesen Befehl mit den entsprechenden [Informationen](virtual-machines-windows-extensions-configuration-samples.md) für die Erweiterung, die Sie installieren möchten.
VM-Erweiterung entfernen | "Resource_group_name" [Remove-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName-Namen "Extension_name" - VMName "Vm_name"

## <a name="next-steps"></a>Nächste Schritte

- Finden Sie die grundlegenden Schritte zum Erstellen eines virtuellen Computers in [einer Windows-VM Ressourcenmanager mit PowerShell erstellen](virtual-machines-windows-ps-create.md).

