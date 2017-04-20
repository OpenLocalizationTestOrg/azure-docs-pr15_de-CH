<properties
    pageTitle="Erstellen und Hochladen einer CentOS-basierten Linux VHD in Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (VHD) mit einer CentOS-basierten Linux-Betriebssystem."
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
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Vorbereiten eines virtuellen CentOS-Computers für Azure

- [Vorbereiten einer CentOS 6.x virtuellen Computers für Azure](#centos-6x)
- [Vorbereiten einer virtuellen Maschine CentOS 7.0 + für Azure](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten ##

Vorausgesetzt, dass Sie einer CentOS installiert haben (oder ähnliche) Linux-Betriebssystem auf einer virtuellen Festplatte. Es gibt mehrere Tools VHD-Dateien, z. B. eine Virtualisierungslösung wie Hyper-V. Eine Anleitung finden Sie in der [Hyper-V-Rolle und konfigurieren einen virtuellen Computer zu installieren](http://technet.microsoft.com/library/hh846766.aspx).


**CentOS Installationshinweise**

- Siehe auch [Allgemeine Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) auf Azure Linux vorbereiten.

- Das VHDX-Format wird in Azure nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mit Hyper-V-Manager oder das Cmdlet Vhd konvertieren VHD-Format konvertieren.

- Bei der Installation von linuxsystem empfohlen Standardpartitionen als LVM (oft Standard für viele Installationen) verwenden. Dies vermeidet LVM Namenskonflikte mit geklonten VMs, insbesondere dann, wenn ein Betriebssystemdatenträger muss immer eine andere VM zur Problembehandlung an.  [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) kann wahlweise auf Datenträger verwendet werden.

- NUMA ist für größere VM aufgrund eines Fehlers in Linux Kernel-Versionen unter 2.6.37 nicht unterstützt. Dieses Problem betrifft hauptsächlich unter Verwendung die Red Hat 2.6.32 Kernel. Manuelle Installation des Agenten Azure Linux (Waagent) deaktiviert automatisch NUMA in GRUB-Konfiguration für den Linux-Kernel. Weitere Informationen finden in den folgenden Schritten.

- Konfigurieren Sie eine Swap-Partition auf dem Betriebssystem-Laufwerk. Linux-Agent kann konfiguriert eine Auslagerungsdatei auf dem temporären Datenträger erstellen.  Weitere Informationen finden in den folgenden Schritten.

- Alle die VHDs müssen Größen, die Vielfache von 1 MB sind.


## <a name="centos-6x"></a>CentOS 6.x ##

1. Wählen Sie im Hyper-V-Manager den virtuellen Computer.

2. Klicken Sie auf **Verbinden** , um ein Konsolenfenster für den virtuellen Computer zu öffnen.

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

        # sudo chkconfig network on


8. **Nur CentOS 6.3**: Installieren Sie die Treiber für Linux Integration Services (LIS).

    **Hinweis: Der Schritt ist nur für CentOS 6.3 und früher.**  CentOS 6.4 + stehen in Linux Integration Services *bereits im standard-Kernel*.

    - Anweisungen Sie der Installation auf der [Downloadseite LIS](https://www.microsoft.com/en-us/download/details.aspx?id=46842) und u/min auf das Bild.  


9. Installieren Sie das Paket Python-pyasn1 durch den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

10. Möchten Sie OpenLogic Spiegelung verwenden, die in Azure Rechenzentren gehostet werden, ersetzen Sie die Datei /etc/yum.repos.d/CentOS-Base.repo mit folgenden Repositories.  Dadurch wird auch **[Openlogic]** -Repository hinzugefügt, das Pakete für Azure Linux-Agent enthält:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Hinweis:** Der Rest dieser Anleitung geht davon aus, daß Sie mindestens Repo [Openlogic], mit folgenden Azure Linux-Agent installieren.


11. /Etc/yum.conf die folgende Zeile hinzufügen:

        http_caching=packages

    Und **nur CentOS 6.3** fügen die folgende Zeile:

        exclude=kernel*

12. Deaktivieren Sie Yum Modul "Fastestmirror" der Datei "/ etc/yum/pluginconf.d/fastestmirror.conf", und `[main]`, geben Sie Folgendes ein:

        set enabled=0

13. Führen Sie den folgenden Befehl zum Löschen der aktuellen Yum Metadaten:

        # yum clean all

14. **Auf CentOS 6.3 nur**aktualisieren den Kernel mit dem folgenden Befehl:

        # sudo yum --disableexcludes=all install kernel

15. Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Öffnen Sie hierzu "/ boot/grub/menu.lst" in einem Texteditor und sicherstellen, dass der Kernel standardmäßig die folgenden Parameter:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Auch dadurch alle Konsole werden Nachrichten an den ersten seriellen Anschluss Azure helfen kann mit debugging verzichten. Dies deaktiviert NUMA aufgrund eines Fehlers in der Kernelversion von CentOS 6 verwendet.

    Darüber hinaus empfiehlt es sich *Entfernen* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter reduziert die Größe des verfügbaren Speichers in der VM von mindestens 128MB die VM kleinere Probleme möglicherweise.


16. Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet.  Dies ist normalerweise die Standardeinstellung.

17. Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # sudo yum install WALinuxAgent

    Beachten Sie, dass WALinuxAgent Paket Entfernen der NetworkManager NetworkManager Gnome-Pakete, wenn sie nicht bereits entfernt wurden, wie in 2 Schritt.

18. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt.

    Azure Linux-Agent kann automatisch die lokale Ressource Datenträger, der die VM zugeordnet ist, nach der Bereitstellung in Azure Auslagerungsspeicher konfigurieren. Beachten Sie, dass die lokale Festplatte *temporärer* Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Klicken Sie auf **Aktion -> Beenden Sie** im Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Änderung CentOS 7 (und ähnliche Erzeugnisse)**

Vorbereiten einer virtuellen Maschine CentOS 7 für Azure ähnelt CentOS 6, es gibt jedoch einige wichtige Unterschiede zu beachten:

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

6.  Ändern von Udev-Regeln, um zu vermeiden, dass statische Regeln für Ethernet-Schnittstellen. Diese Regeln können Probleme verursachen, beim Klonen eines virtuellen Computers in Microsoft Azure oder Hyper-v

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Sicherstellen Sie, dass der Netzwerkdienst beim Booten gestartet wird, durch den folgenden Befehl ausführen:

        # sudo chkconfig network on

7. Installieren Sie das Paket Python-pyasn1 durch den folgenden Befehl ausführen:

        # sudo yum install python-pyasn1

8. Möchten Sie OpenLogic Spiegelung verwenden, die in Azure Rechenzentren gehostet werden, ersetzen Sie die Datei /etc/yum.repos.d/CentOS-Base.repo mit folgenden Repositories.  Dadurch wird auch **[Openlogic]** -Repository hinzugefügt, das Pakete für Azure Linux-Agent enthält:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Hinweis:** Der Rest dieser Anleitung geht davon aus, daß Sie mindestens Repo [Openlogic], mit folgenden Azure Linux-Agent installieren.

9.  Führen Sie den folgenden Befehl die aktuellen Yum Metadaten und installieren:

        # sudo yum clean all
        # sudo yum -y update

10. Ändern der Kernel-Startzeile im Grub-Konfiguration weitere Parameter für Azure einschließen. Dazu öffnen "/ Etc/Default/Grub" in einem Texteditor und Bearbeiten der `GRUB_CMDLINE_LINUX` Parameter, beispielsweise:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Auch dadurch alle Konsole werden Nachrichten an den ersten seriellen Anschluss Azure helfen kann mit debugging verzichten. Außerdem deaktiviert den neuen CentOS 7 Benennungskonventionen für Netzwerkkarten. Darüber hinaus empfiehlt es sich *Entfernen* die folgenden Parameter:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter reduziert die Größe des verfügbaren Speichers in der VM von mindestens 128MB die VM kleinere Probleme möglicherweise.

11. Sobald Sie fertig sind, bearbeiten "/ Etc/Default/Grub" pro, führen Sie folgenden Befehl GRUB-Konfiguration neu erstellen:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet.  Dies ist normalerweise die Standardeinstellung.

13. **Nur, wenn das Bild von VMWare, VirtualBox oder KVM erstellen:** Fügen Sie Initramfs mit Hyper-V-Module:

    Bearbeiten `/etc/dracut.conf`, Inhalt hinzufügen:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Erstellen Sie die Initramfs neu:

        # dracut –f -v

14. Installieren der Azure Linux-Agent durch Ausführen des folgenden Befehls:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Swap-Bereich wird nicht auf dem Betriebssystem-Datenträger erstellt.

    Azure Linux-Agent kann automatisch die lokale Ressource Datenträger, der die VM zugeordnet ist, nach der Bereitstellung in Azure Auslagerungsspeicher konfigurieren. Beachten Sie, dass die lokale Festplatte *temporärer* Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klicken Sie auf **Aktion -> Beenden Sie** im Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.

## <a name="next-steps"></a>Nächste Schritte
Sie nun können mit Linux CentOS virtuelle Festplatte erstellen neuen virtueller Maschinen in Azure. Ist dies das erste Mal, dass Sie die VHD-Datei in Azure hochladen, finden Sie die Schritte 2 und 3 [Erstellen und Hochladen einer virtuellen Festplatte mit dem Betriebssystem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
