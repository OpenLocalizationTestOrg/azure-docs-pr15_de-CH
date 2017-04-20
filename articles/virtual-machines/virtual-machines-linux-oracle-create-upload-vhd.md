<properties
    pageTitle="Erstellen und Hochladen einer Oracle Linux VHD | Microsoft Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (VHD), die ein Oracle Linux-Betriebssystem enthält."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Vorbereiten einer virtuellen Maschine Oracle Linux für Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten ##

Es wird vorausgesetzt, dass Sie bereits ein Oracle Linux Betriebssystem auf einer virtuellen Festplatte installiert haben. Es gibt mehrere Tools VHD-Dateien, z. B. eine Virtualisierungslösung wie Hyper-V. Eine Anleitung finden Sie in der [Hyper-V-Rolle und konfigurieren einen virtuellen Computer zu installieren](http://technet.microsoft.com/library/hh846766.aspx).


### <a name="oracle-linux-installation-notes"></a>Oracle Installationshinweise für Linux

- Siehe auch [Allgemeine Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) auf Azure Linux vorbereiten.

- Oracle Red Hat kompatibel Kernel und ihre UEK3 (unverwüstliche Enterprise Kernel) werden auf Hyper-V und Azure unterstützt. Für optimale Ergebnisse unbedingt auf den neuesten Kernel beim Vorbereiten der Oracle Linux VHD aktualisieren.

- Oracle UEK2 nicht auf Hyper-V und Azure werden als nicht die benötigten Treiber enthalten ist.

- Das VHDX-Format wird in Azure nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mit Hyper-V-Manager oder das Cmdlet Vhd konvertieren VHD-Format konvertieren.

- Bei der Installation von linuxsystem empfohlen Standardpartitionen als LVM (oft Standard für viele Installationen) verwenden. Dies vermeidet LVM Namenskonflikte mit geklonten VMs, insbesondere dann, wenn ein Betriebssystemdatenträger muss immer eine andere VM zur Problembehandlung an. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) kann wahlweise auf Datenträger verwendet werden.

- NUMA ist für größere VM aufgrund eines Fehlers in Linux Kernel-Versionen unter 2.6.37 nicht unterstützt. Dieses Problem betrifft hauptsächlich unter Verwendung die Red Hat 2.6.32 Kernel. Manuelle Installation des Agenten Azure Linux (Waagent) deaktiviert automatisch NUMA in GRUB-Konfiguration für den Linux-Kernel. Weitere Informationen finden in den folgenden Schritten.

- Konfigurieren Sie eine Swap-Partition auf dem Betriebssystem-Laufwerk. Linux-Agent kann konfiguriert eine Auslagerungsdatei auf dem temporären Datenträger erstellen.  Weitere Informationen finden in den folgenden Schritten.

- Alle die VHDs müssen Größen, die Vielfache von 1 MB sind.

- Stellen Sie sicher, dass die `Addons` Repository aktiviert ist. Bearbeiten Sie die Datei `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) oder `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), und ändern Sie die Zeile `enabled=0` , `enabled=1` **[ol6_addons]** oder **[ol7_addons]** in dieser Datei.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 + ##

Sie müssen bestimmte Konfigurationsschritte im Betriebssystem des virtuellen Computers in Azure ausgeführt.

1. Wählen Sie im mittleren Bereich der Hyper-V-Manager den virtuellen Computer.

2. Klicken Sie auf **Verbinden** , um das Fenster für den virtuellen Computer.

3. Deinstallieren Sie NetworkManager durch Ausführen des folgenden Befehls:

        # sudo rpm -e --nodeps NetworkManager

    **Hinweis:** Das Paket noch nicht installiert ist, wird dieser Befehl mit einer Fehlermeldung fehl. Wird erwartet.

4.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Ändern von Udev-Regeln, um zu vermeiden, dass statische Regeln für Ethernet-Schnittstellen. Diese Regeln können Probleme verursachen, beim Klonen eines virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # chkconfig network on

8. Installieren Sie Python-pyasn1 durch den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

9.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Soll dieses "/ boot/grub/menu.lst" in einem Texteditor und sicherstellen, dass der Kernel standardmäßig die folgenden Parameter:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Auch dadurch alle Konsole werden Nachrichten an den ersten seriellen Anschluss Azure helfen kann mit debugging verzichten. Dadurch wird aufgrund eines Fehlers in Oracle Red Hat kompatibel Kernel NUMA deaktiviert.

    Darüber hinaus empfiehlt es sich *Entfernen* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter reduziert die Größe des verfügbaren Speichers in der VM von mindestens 128MB die VM kleinere Probleme möglicherweise.


10. Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet.  Dies ist normalerweise die Standardeinstellung.

11. Installieren Sie Azure Linux-Agent durch Ausführen des folgenden Befehls. Die neueste Version ist 2.0.15.

        # sudo yum install WALinuxAgent

    Beachten Sie, dass WALinuxAgent Paket Entfernen der NetworkManager NetworkManager Gnome-Pakete, wenn sie nicht bereits entfernt wurden, wie in 2 Schritt.

12. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt.

    Azure Linux-Agent kann automatisch die lokale Ressource Datenträger, der die VM zugeordnet ist, nach der Bereitstellung in Azure Auslagerungsspeicher konfigurieren. Beachten Sie, dass die lokale Festplatte *temporärer* Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Klicken Sie auf **Aktion -> Beenden Sie** im Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.


----------


## <a name="oracle-linux-70"></a>Oracle Linux 7.0 + ##

**Ändert in Oracle Linux 7**

Vorbereiten einer virtuellen Maschine Oracle Linux 7 für Azure ähnelt Oracle Linux 6, es gibt jedoch einige wichtige Unterschiede zu beachten:

 - Red Hat kompatibel Kernel und Oracle UEK3 werden in Azure unterstützt.  UEK3-Kernel wird empfohlen.
 - Das Paket NetworkManager widerspricht Azure Linux-Agent nicht mehr. Dieses Paket wird standardmäßig installiert, und wir empfehlen nicht entfernt.
 - Das Verfahren zum Bearbeiten von Kernel-Parameter geändert wurde (siehe unten) wird GRUB2 jetzt als Standard-Bootloader verwendet.
 - XFS ist nun die Standardeinstellung. Ext4 Dateisystem kann weiterhin verwendet werden.


**Konfigurationsschritte**

1. Wählen Sie im Hyper-V-Manager den virtuellen Computer.

2. Klicken Sie auf **Verbinden** , um ein Konsolenfenster für den virtuellen Computer zu öffnen.

3.  Erstellen Sie eine Datei mit dem Namen **Netzwerk** der `/etc/sysconfig/` Verzeichnis, den folgenden Text enthält:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Erstellen Sie eine Datei mit dem Namen **Ifcfg eth0** in der `/etc/sysconfig/network-scripts/` Verzeichnis, den folgenden Text enthält:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

5.  Ändern von Udev-Regeln, um zu vermeiden, dass statische Regeln für Ethernet-Schnittstellen. Diese Regeln können Probleme verursachen, beim Klonen eines virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # sudo chkconfig network on

7. Installieren Sie das Paket Python-pyasn1 durch den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

8.  Führen Sie den folgenden Befehl die aktuellen Yum Metadaten und installieren:

        # sudo yum clean all
        # sudo yum -y update

9.  Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Dazu öffnen "/ Etc/Default/Grub" in einem Texteditor und Bearbeiten der `GRUB_CMDLINE_LINUX` Parameter, beispielsweise:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Auch dadurch alle Konsole werden Nachrichten an den ersten seriellen Anschluss Azure helfen kann mit debugging verzichten. Außerdem deaktiviert den neuen OEL 7 Benennungskonventionen für Netzwerkkarten. Darüber hinaus empfiehlt es sich *Entfernen* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter reduziert die Größe des verfügbaren Speichers in der VM von mindestens 128MB die VM kleinere Probleme möglicherweise.


10. Sobald Sie fertig sind, bearbeiten "/ Etc/Default/Grub" pro, führen Sie folgenden Befehl GRUB-Konfiguration neu erstellen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet.  Dies ist normalerweise die Standardeinstellung.

12. Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt.

    Azure Linux-Agent kann automatisch die lokale Ressource Datenträger, der die VM zugeordnet ist, nach der Bereitstellung in Azure Auslagerungsspeicher konfigurieren. Beachten Sie, dass die lokale Festplatte *temporärer* Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klicken Sie auf **Aktion -> Beenden Sie** im Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.


## <a name="next-steps"></a>Nächste Schritte
Sie nun können Ihre Oracle Linux VHD zum Erstellen neuen virtueller Maschinen in Azure verwendet. Ist dies das erste Mal, dass Sie die VHD-Datei in Azure hochladen, finden Sie die Schritte 2 und 3 [Erstellen und Hochladen einer virtuellen Festplatte mit dem Betriebssystem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
