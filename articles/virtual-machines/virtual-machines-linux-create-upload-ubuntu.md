<properties
    pageTitle="Erstellen Sie und Hochladen Sie einer VHD Ubuntu Linux in Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (VHD), die ein Betriebssystem Ubuntu Linux enthält."
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

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Vorbereiten einer virtuellen Maschine Ubuntu für Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Offizielle Ubuntu Cloud Bilder
Ubuntu veröffentlicht jetzt offizielle Azure VHDs unter [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/)heruntergeladen werden. Benötigen Sie ein eigene spezielle Ubuntu Bild für Azure erstellen, sondern als das manuelle Verfahren unten verwenden empfiehlt es diese bekannten VHDs arbeiten und nach Bedarf anpassen. Bild Neuerscheinungen können immer an folgenden Speicherorten befinden:

 - Ubuntu 12.04/präzise: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04/treuen: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Erforderliche Komponenten

Es wird vorausgesetzt, dass Sie bereits ein Betriebssystem Ubuntu Linux auf einer virtuellen Festplatte installiert haben. Es gibt mehrere Tools VHD-Dateien, z. B. eine Virtualisierungslösung wie Hyper-V. Eine Anleitung finden Sie in der [Hyper-V-Rolle und konfigurieren einen virtuellen Computer zu installieren](http://technet.microsoft.com/library/hh846766.aspx).

**Installationshinweise für Ubuntu**

- Siehe auch [Allgemeine Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) auf Azure Linux vorbereiten.
- Das VHDX-Format wird in Azure nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mit Hyper-V-Manager oder das Cmdlet Vhd konvertieren VHD-Format konvertieren.
- Bei der Installation von linuxsystem empfohlen Standardpartitionen als LVM (oft Standard für viele Installationen) verwenden. Dies vermeidet LVM Namenskonflikte mit geklonten VMs, insbesondere dann, wenn ein Betriebssystemdatenträger muss immer eine andere VM zur Problembehandlung an. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) kann wahlweise auf Datenträger verwendet werden.
- Konfigurieren Sie eine Swap-Partition auf dem Betriebssystem-Laufwerk. Linux-Agent kann konfiguriert eine Auslagerungsdatei auf dem temporären Datenträger erstellen.  Weitere Informationen finden in den folgenden Schritten.
- Alle die VHDs müssen Größen, die Vielfache von 1 MB sind.


## <a name="manual-steps"></a>Manuelle Schritte

> [AZURE.NOTE] Beachten Sie bevor Sie eigene benutzerdefinierte Ubuntu Bild für Azure erstellen Bilder aus [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) verwenden.


1. Wählen Sie im mittleren Bereich der Hyper-V-Manager den virtuellen Computer.

2. Klicken Sie auf **Verbinden** , um das Fenster für den virtuellen Computer.

3.  Ersetzen Sie die aktuelle Repositories im Bild mit Ubuntu Azure Repogeschäfte. Die Schritte variieren leicht je Ubuntu.

    Vor dem Bearbeiten /etc/apt/sources.list wird empfohlen, eine Sicherungskopie:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. Ubuntu Azure Bilder folgen jetzt den Kernel *Hardware-Aktivierung* (HWE). Aktualisieren des Betriebssystems auf den neuesten Kernel mit folgenden Befehlen:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. Ändern der Kernel-Startzeile für Grub weitere Parameter für Azure enthalten. Dazu öffnen "/ Etc/Default/Grub" finden Sie in einem Texteditor die Variable namens `GRUB_CMDLINE_LINUX_DEFAULT` (oder bei Bedarf) und bearbeiten, um die folgenden Parameter:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Speichern und schließen Sie diese Datei und führen Sie "`sudo update-grub`". Dadurch wird sichergestellt, dass alle Konsole den ersten seriellen Anschluss Nachrichten sind die Azure technische Probleme Debuggen unterstützen können.

6.  Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet.  Dies ist normalerweise die Standardeinstellung.

7.  Installieren Sie Azure Linux-Agent:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Beachten Sie diese Installation der `walinuxagent` Paket entfernt die `NetworkManager` und `NetworkManager-gnome` Pakete, wenn sie installiert sind.

8.  Führen Sie die folgenden Befehle zu entziehen der virtuelle Computer für die Bereitstellung von Azure vorzubereiten:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klicken Sie auf **Aktion -> Beenden Sie** im Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.


## <a name="next-steps"></a>Nächste Schritte
Sie nun können mit Ubuntu Linux virtuelle Festplatte erstellen neuen virtueller Maschinen in Azure. Ist dies das erste Mal, dass Sie die VHD-Datei in Azure hochladen, finden Sie die Schritte 2 und 3 [Erstellen und Hochladen einer virtuellen Festplatte mit dem Betriebssystem Linux](virtual-machines-linux-classic-create-upload-vhd.md).

## <a name="references"></a>Referenzen ##

Ubuntu Hardware-Aktivierung (HWE) Kernel:

- [http://Blog.utlemming.org/2015/01/Ubuntu-1404-Azure-Images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://Blog.utlemming.org/2015/02/1204-Azure-Cloud-Images-Now-Using-Hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)
