<properties
   pageTitle="Ein lokales Windows-Kennwort zurücksetzen, wenn Azure Gast-Agent nicht installiert ist | Microsoft Azure"
   description="Das Kennwort des lokalen Windows-Benutzerkonto zurücksetzen, wenn Azure Gast-Agent nicht installiert oder funktioniert auf einem virtuellen Computer"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="iainfou"/>

# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Windows-Kennwort für Azure VM zurücksetzen
Sie können das Kennwort des lokale Windows VM in Azure mit dem [Azure-Portal oder Azure PowerShell](virtual-machines-windows-reset-rdp.md) Azure Gast-Agent installiert zurücksetzen. Diese Methode ist die gängigste Zurücksetzen des Kennworts für ein Azure-VM. Treten Probleme mit dem Azure Gast-Agent reagiert nicht oder nicht installieren, nachdem Sie ein benutzerdefiniertes Bild hochladen, können Sie manuell zurücksetzen eine Windows-Kennwort. Dieser Artikel beschreibt, wie ein lokales Konto zurücksetzen, indem Sie eine andere VM OS virtuellen Quelllaufwerks zuordnen. 

> [AZURE.WARNING] Verwenden Sie dabei nur als letzten Ausweg. Versuchen Sie immer mit der [Azure-Portal oder Azure PowerShell](virtual-machines-windows-reset-rdp.md) zuerst ein Kennwort zurücksetzen.


## <a name="overview-of-the-process"></a>Übersicht über den Prozess
Die wichtigsten Schritte zur Durchführung eines lokalen Kennworts zurückgesetzt für einen virtuellen Computer Windows Azure, wenn kein auf den Azure Gast-Agenten Zugriff lautet wie folgt:

- Löschen Sie VM. Die virtuellen Laufwerke bleiben.
- Eine andere VM in Azure-Abonnement Quelle VM Betriebssystemdatenträger zuordnen. Diese VM wird die Problembehandlung VM genannt.
- Erstellen Sie die Problembehandlung VM einige Konfigurationsdateien Betriebssystemdatenträger Quelle VM.
- Trennen Sie die VM Betriebssystemdatenträger Problembehandlung VM.
- Verwenden einer Vorlage Ressourcenmanager VM mit ursprünglichen virtuellen Laufwerks erstellen.
- Neue VM hochgefahren, aktualisieren die Config-Dateien erstellte das Kennwort für den Benutzer.


## <a name="detailed-steps"></a>Ausführliche Anleitung
Versuchen Sie immer Zurücksetzen des Kennworts mithilfe der [Azure-Portal oder Azure PowerShell](virtual-machines-windows-reset-rdp.md) bevor Sie versuchen, die folgenden Schritte aus. Stellen Sie sicher, Sie haben eine Sicherung Ihrer VM starten. 

1. Löschen der betreffenden VM in Azure-Portal. Die VM löschen, nur die Metadaten der Verweis der VM in Azure. Virtuelle Laufwerke werden beibehalten, wenn die VM gelöscht wird:

    - Wählen Sie die VM in Azure-Portal, klicken Sie auf *Löschen*:

    ![Löschen Sie vorhandene VM](./media/virtual-machines-windows-reset-local-password-without-guest-agent/delete_vm.png)

2. Die Problembehandlung VM Betriebssystemdatenträger Quelle VM zuordnen. Die Problembehandlung VM muss im Bereich der Quelle VM Betriebssystemdatenträger (z. B. `West US`):

    - Die Problembehandlung VM in Azure-Portal auswählen Klicken Sie auf *Datenträger* | *vorhandene anfügen*:

    ![Fügen Sie vorhandenen Datenträger](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_attach_existing.png)

    Wählen Sie *VHD-Datei aus* und dann das Speicherkonto, das der VM enthält:

    ![Konto wählen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_storageaccount.PNG)

    Auswählen des Containers. Des Containers ist normalerweise *Festplatten*:

    ![Wählen Sie Behälter](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_container.png)

    Wählen Sie Vhd Betriebssystem an. Klicken Sie auf *auswählen* , um den Vorgang abzuschließen:

    ![Virtuelles Quelllaufwerk auswählen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_source_vhd.png)

3. Verbindung zur Problembehandlung VM Remotedesktop und sicherzustellen, dass die Quelle VM Betriebssystemdatenträger angezeigt wird:

    - Wählen Sie zur Problembehandlung VM in Azure-Portal und klicken Sie auf *Verbinden*.
    - Öffnen Sie die RDP-Datei, die heruntergeladen. Geben Sie Benutzername und Kennwort der Problembehandlung VM.
    - Suchen Sie im Datei-Explorer für Datenträger, den Sie angehängt. Wenn die Quelle VM VHD nur Datenträger für die Problembehandlung VM zugeordnet ist, sollte es Laufwerk F::

    ![Angeschlossenen Datenträger anzeigen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/troubleshooting_vm_fileexplorer.png)

4. Erstellen Sie `gpt.ini` in `\Windows\System32\GroupPolicy` auf der Quell-VM (ggf. gpt.ini umbenennen in gpt.ini.bak):

    > [AZURE.WARNING] Stellen Sie sicher, dass Sie nicht versehentlich den folgenden Dateien unter C:\Windows OS-Laufwerk für die Problembehandlung VM erstellen. Erstellen Sie die folgenden Dateien im Betriebssystem Laufwerk für die Datenquelle VM als Datenträger zugeordnet ist.

    - Fügen Sie folgende Zeilen in der `gpt.ini` erstellten Datei:

    ```
    [General]
    gPCFunctionalityVersion=2
    gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
    Version=1
    ```

    ![Erstellen von "GPT.ini"](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_gpt_ini.png)
 
5. Erstellen Sie `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`. Stellen Sie sicher, dass versteckte Ordner angezeigt werden. Erstellen Sie bei Bedarf die `Machine` oder `Scripts` Ordner.

    - Fügen Sie die folgenden Zeilen der `scripts.ini` erstellten Datei:

    ```
    [Startup]
    0CmdLine=C:\Windows\System32\FixAzureVM.cmd
    0Parameters=
    ```

    ![Scripts.ini erstellen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_scripts_ini.png)
 
6. Erstellen Sie `FixAzureVM.cmd` in `\Windows\System32` mit folgendem Inhalt ersetzen `<username>` und `<newpassword>` mit Ihren eigenen Werten:

    ```
    NET USER <username> <newpassword>
    ```

    ![FixAzureVM.cmd erstellen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_fixazure_cmd.png)

    Sie müssen das Kennwort Komplexität für die VM Anforderungen beim Definieren des neuen Kennworts.

7. Trennen Sie den Datenträger aus der Problembehandlung VM in Azure-Portal:

    - Wählen Sie die Problembehandlung VM in Azure-Portal, klicken Sie auf *Datenträger*.
    - Wählen Sie in Schritt 2 angefügten Datenträger, klicken Sie auf *Trennen*:

    ![Diskette trennen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/detach_disk.png)

8. Bevor Sie einen virtuellen Computer erstellen, erhalten Sie den URI Quelllaufwerks OS:

    - Wählen Sie das Speicherkonto in Azure-Portal, auf *Blobs*.
    - Wählen Sie den Container aus. Des Containers ist normalerweise *Festplatten*:

    ![Wählen Sie Konto Blob Speicher](./media/virtual-machines-windows-reset-local-password-without-guest-agent/select_storage_details.png)

    Wählen Sie die VM OS VHD-Quelle aus, und klicken Sie neben dem Namen *URL* *Kopieren* :

    ![Datenträger kopieren URI](./media/virtual-machines-windows-reset-local-password-without-guest-agent/copy_source_vhd_uri.png)

9. Erstellen einer VM Betriebssystemdatenträger Quelle VM:

    - Verwenden Sie [Diese Vorlage Azure-Ressourcen-Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) einen virtuellen Computer aus einer speziellen virtuellen Festplatte erstellen. Klicken Sie auf die `Deploy to Azure` zu Azure-Portal mit den Vorlagen ausgefüllt zu öffnen.
    - Wenn Sie die vorherige Einstellung für den virtuellen Computer beibehalten möchten, wählen Sie Ihre vorhandenen VNet, Subnetz, Netzwerkadapter oder öffentliche IP- *Vorlage bearbeiten* .
    - In der `OSDISKVHDURI` Textfeld Parameter, den URI der Quelle VHD erhalten im vorherigen Schritt, einfügen:

    ![Eine VM aus Vorlage erstellen](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_new_vm_from_template.png)

10. Nachdem die neue VM ausgeführt wird, Verbinden mit Remotedesktop mit dem neuen Kennwort in angegeben VM der `FixAzureVM.cmd` Skript.

11. Entfernen Sie von der Remotesitzung zur neuen VM Bereinigen der Umgebung die folgenden Dateien:

    - Von %windir%\System32
        - FixAzureVM.cmd entfernen
    - Von %windir%\System32\GroupPolicy\Machine\
        - scripts.ini entfernen
    - Von %windir%\System32\GroupPolicy
        - Entfernen Sie gpt.ini (wenn gpt.ini gegeben und umbenannt in gpt.ini.bak umbenennen die BAK-Datei gpt.ini an)

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weiterhin keine Verbindung nicht Remote Desktop verwenden herstellen können, finden Sie die [RDP-Fehlerbehebung](virtual-machines-windows-troubleshoot-rdp-connection.md). [Detaillierte RDP-Fehlerbehebung](virtual-machines-windows-detailed-troubleshoot-rdp.md) untersucht Methoden als spezifische Schritte zur Problembehandlung. Sie können auch praktische Unterstützung [Azure Support-Anfrage öffnen](https://azure.microsoft.com/support/options/) .