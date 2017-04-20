<properties
   pageTitle="Verwalten Sie Ihre virtuellen Computer mithilfe von Azure PowerShell | Microsoft Azure"
   description="Enthält Befehle zum Automatisieren von Aufgaben bei der Verwaltung Ihrer virtuellen Maschinen."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Verwalten Sie Ihre virtuellen Computer mithilfe von Azure PowerShell

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Viele Aufgaben werden täglich um Ihre VMs verwalten können automatisiert werden, mithilfe von Azure PowerShell-Cmdlets. Dieser Artikel enthält Beispiele für Befehle für einfachere Aufgaben und Links zu Artikeln, die Befehle für komplexere Aufgaben angezeigt.

>[AZURE.NOTE] Wenn Sie nicht installiert und konfiguriert Azure PowerShell, erhalten Sie im Artikel [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)Anleitung.

## <a name="how-to-use-the-example-commands"></a>Zum Beispielbefehle verwenden
Sie müssen einen Teil des Textes in den Befehlen durch Text ersetzen, die für Ihre Umgebung geeignet ist. Das < und > Symbole Text ersetzt werden muss. Beim Ersetzen von Text entfernen Sie die Symbole, aber lassen Sie die Anführungszeichen.

## <a name="get-a-vm"></a>Abrufen einer VM
Dies ist eine einfache Aufgabe, die Sie häufig verwenden. Verwenden Sie diese Informationen eine VM, Aufgaben auf einem virtuellen Computer oder in einer Variablen speichern Ausgabe erhalten.

Informationen zur VM Ausführen dieser Befehl Ersetzen alles in Anführungszeichen, einschließlich der Zeichen < und >:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Führen Sie zum Speichern der Ausgabe in eine $vm-Variable:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Einen virtuellen Windows-basierten Computer anmelden

Folgende Befehle ausführen:

>[AZURE.NOTE] Der Dienstname virtuellen und Cloud erhalten von der Ausgabe des Befehls **Get-AzureVM** .
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>Beenden einer VM

Führen Sie diesen Befehl:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Verwenden Sie diesen Parameter zu den VIP (virtual IP) des Cloud-Dienst ist die letzte VM, Cloud-Dienst. <br><br> Bei Verwendung den StayProvisioned-Parameter müssen Sie weiterhin für die VM Abrechnung.

## <a name="start-a-vm"></a>Starten Sie einen virtuellen Computer

Führen Sie diesen Befehl:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Legen Sie einen Datenträger
Diese Aufgabe erfordert einige Schritte. Verwenden Sie zuerst die *** hinzufügen-AzureDataDisk *** Cmdlet den Datenträger für das $vm-Objekt hinzufügen. Verwenden Sie dann **Update-AzureVM** -Cmdlet die Konfiguration der VM aktualisieren.

Sie müssen auch entscheiden, ob Sie fügen eine neue Festplatte oder Daten enthält. Der Befehl für eine neue Festplatte VHD-Datei erstellt und angefügt.

Führen Sie diesen Befehl, um einen neuen Datenträger installieren:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Führen Sie diesen Befehl, um einen vorhandenen Datenträger hinzuzufügen:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Führen Sie diesen Befehl, um Datenträger aus einer vorhandenen .vhd-Datei im BLOB-Speicher hinzuzufügen:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Erstellen Sie einen virtuellen Windows-basierten Computer

Verwenden Sie zum Erstellen eines neuen Windows-basierten virtuellen Computers in Azure die Anleitung in [Azure PowerShell zu Vorkonfigurieren der Windows-basierten virtuellen Maschinen verwenden](virtual-machines-windows-classic-create-powershell.md). In diesem Thema schrittweise durch die Erstellung einer Azure PowerShell Befehl festlegen eine Windows-basierten VM, die vorkonfiguriert werden erstellt:

- Mit Active Directory-Domänenmitgliedschaft.
- Mit zusätzlichen Festplatten.
- Als Mitglied eines vorhandenen Lastenausgleich festgelegt.
- Mit einer statischen IP-Adresse.
