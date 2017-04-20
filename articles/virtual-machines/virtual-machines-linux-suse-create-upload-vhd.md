<properties
    pageTitle="Erstellen und Hochladen einer VHD SUSE Linux in Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (VHD) mit dem Betriebssystem SUSE Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Vorbereiten einer virtuellen Maschine SLES oder OpenSUSE für Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten ##

Es wird vorausgesetzt, dass Sie eine SUSE oder OpenSUSE Linux-Betriebssystem auf einer virtuellen Festplatte installiert haben. Es gibt mehrere Tools VHD-Dateien, z. B. eine Virtualisierungslösung wie Hyper-V. Eine Anleitung finden Sie in der [Hyper-V-Rolle und konfigurieren einen virtuellen Computer zu installieren](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / OpenSUSE-Installationshinweise

- Siehe auch [Allgemeine Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) auf Azure Linux vorbereiten.

- Das VHDX-Format wird in Azure nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mit Hyper-V-Manager oder das Cmdlet Vhd konvertieren VHD-Format konvertieren.

- Bei der Installation von linuxsystem empfohlen Standardpartitionen als LVM (oft Standard für viele Installationen) verwenden. Dies vermeidet LVM Namenskonflikte mit geklonten VMs, insbesondere dann, wenn ein Betriebssystemdatenträger muss immer eine andere VM zur Problembehandlung an. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) kann wahlweise auf Datenträger verwendet werden.

- Konfigurieren Sie eine Swap-Partition auf dem Betriebssystem-Laufwerk. Linux-Agent kann konfiguriert eine Auslagerungsdatei auf dem temporären Datenträger erstellen.  Weitere Informationen finden in den folgenden Schritten.

- Alle die VHDs müssen Größen, die Vielfache von 1 MB sind.


## <a name="use-suse-studio"></a>SUSE Studio verwenden
[SUSE Studio](http://www.susestudio.com) können problemlos erstellen und Verwalten von SLES und OpenSUSE Bilder für Azure und Hyper-V. Dies ist die empfohlene Vorgehensweise zum Anpassen von Eigene Bilder SLES und OpenSUSE.

Als Alternative zum Erstellen eigener VHD veröffentlicht SUSE BYOS (bringen Ihr eigenes Abonnement) Bilder für SLES auf [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>SUSE Linux Enterprise Server 11 SP4 vorbereiten ##

1. Wählen Sie im mittleren Bereich der Hyper-V-Manager den virtuellen Computer.

2. Klicken Sie auf **Verbinden** , um das Fenster für den virtuellen Computer.

3. Registrieren Sie Ihr SUSE Linux Enterprise System so Updates downloaden und Installieren von Paketen.

4. Aktualisieren Sie das System mit den neuesten Patches:

        # sudo zypper update

5. Installieren Sie aus dem Repository SLES Azure Linux-Agent:

        # sudo zypper install WALinuxAgent

6. Chkconfig checken Sie Wenn Waagent auf "on" gesetzt ist ein, und andernfalls für Autostart aktivieren:
               
        # sudo chkconfig waagent on

7. Überprüfen Sie, ob Waagent Dienst ausgeführt wird, und falls nicht, starten Sie ihn: 

        # sudo service waagent start
                
8. Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Soll dieses "/ boot/grub/menu.lst" in einem Texteditor und sicherstellen, dass der Kernel standardmäßig die folgenden Parameter:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Dies gewährleistet, dass alle Konsole werden Nachrichten an den ersten seriellen Anschluss Azure helfen kann mit debugging verzichten.

9. Bestätigen Sie, dass /boot/grub/menu.lst und/etc/fstab Disk Datenträger ID (nach Id) anstelle der UUID (von Uuid) verweisen. 

    Festplatte UUID
    
        # ls /dev/disk/by-uuid/

    Wenn /dev/disk/by-id ist /boot/grub/menu.lst und/Etc/Fstab verwendet, mit dem richtigen von Uuid-Wert aktualisieren

    Vor Änderung
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Nach der Änderung
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Ändern von Udev-Regeln, um zu vermeiden, dass statische Regeln für Ethernet-Schnittstellen. Diese Regeln können Probleme verursachen, beim Klonen eines virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. Es wird empfohlen, die Datei "/ Etc/Sysconfig/Network/Dhcp" und ändern die `DHCLIENT_SET_HOSTNAME` der folgenden Parameter:

        DHCLIENT_SET_HOSTNAME="no"

12. Kommentieren Sie in "/ Etc/Sudoers" oder entfernen Sie die folgenden Zeilen, falls vorhanden:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet.  Dies ist normalerweise die Standardeinstellung.

14. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt.

    Azure Linux-Agent kann automatisch die lokale Ressource Datenträger, der die VM zugeordnet ist, nach der Bereitstellung in Azure Auslagerungsspeicher konfigurieren. Beachten Sie, dass die lokale Festplatte *temporärer* Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klicken Sie auf **Aktion -> Beenden Sie** im Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.


----------

## <a name="prepare-opensuse-131"></a>OpenSUSE 13.1 + Vorbereiten ##

1. Wählen Sie im mittleren Bereich der Hyper-V-Manager den virtuellen Computer.

2. Klicken Sie auf **Verbinden** , um das Fenster für den virtuellen Computer.

3. Die Shell Ausführen den Befehl '`zypper lr`'. Repositories werden konfiguriert, da keine Korrekturen erforderlich – wenn dieses Befehls sieht etwa wie folgt (Beachten Sie, dass die Versionsnummern variieren):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Kehrt der Befehl "Keine Repositories definiert..." verwenden Sie die folgenden Befehle um diesen Repos hinzuzufügen:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    Sie können dann überprüfen, Repositories durch Ausführen des Befehls hinzugefügten '`zypper lr`' erneut. Bei einem entsprechenden Update-Repositories nicht aktiviert ist, aktivieren sie mit dem folgenden Befehl:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. Den Kernel auf die neueste Version zu aktualisieren:

        # sudo zypper up kernel-default

    Oder das System mit den neuesten Patches aktualisieren:

        # sudo zypper update

5.  Installieren Sie Azure Linux-Agent.

        # sudo zypper install WALinuxAgent

6.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Öffnen Sie hierzu "/ boot/grub/menu.lst" in einem Texteditor und sicherstellen, dass der Kernel standardmäßig die folgenden Parameter:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Dies gewährleistet, dass alle Konsole werden Nachrichten an den ersten seriellen Anschluss Azure helfen kann mit debugging verzichten. Entfernen Sie die folgenden Parameter außerdem von Kernel-Startzeile, falls vorhanden:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  Es wird empfohlen, die Datei "/ Etc/Sysconfig/Network/Dhcp" und ändern die `DHCLIENT_SET_HOSTNAME` der folgenden Parameter:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Wichtig:** Kommentieren Sie in "/ Etc/Sudoers" oder entfernen Sie die folgenden Zeilen, falls vorhanden:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet.  Dies ist normalerweise die Standardeinstellung.

10. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt.

    Azure Linux-Agent kann automatisch die lokale Ressource Datenträger, der die VM zugeordnet ist, nach der Bereitstellung in Azure Auslagerungsspeicher konfigurieren. Beachten Sie, dass die lokale Festplatte *temporärer* Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Sicherstellen Sie, dass Azure Linux-Agent beim Systemstart ausgeführt wird:

        # sudo systemctl enable waagent.service

13. Klicken Sie auf **Aktion -> Beenden Sie** im Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.

## <a name="next-steps"></a>Nächste Schritte
Sie nun können mit SUSE Linux virtuelle Festplatte erstellen neuen virtueller Maschinen in Azure. Ist dies das erste Mal, dass Sie die VHD-Datei in Azure hochladen, finden Sie die Schritte 2 und 3 [Erstellen und Hochladen einer virtuellen Festplatte mit dem Betriebssystem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
