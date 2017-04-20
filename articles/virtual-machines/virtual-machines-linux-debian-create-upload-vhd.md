<properties
    pageTitle="Vorbereiten von Debian Linux VHD | Microsoft Azure"
    description="Informationen Sie zum Erstellen von Debian 7 und 8 VHD-Dateien für die Bereitstellung in Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Azure Debian VHD vorbereiten

## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Abschnitt wird vorausgesetzt, dass aus einer ISO-Datei von der [Debian-Website](https://www.debian.org/distrib/) auf einer virtuellen Festplatte heruntergeladen bereits Debian Linux-Betriebssystem installiert haben. Mehrere Tools vorhanden zum Erstellen von VHD-Dateien. Hyper-V ist nur ein Beispiel. Mit Hyper-V-Anleitung finden Sie in der [Hyper-V-Rolle und konfigurieren einen virtuellen Computer zu installieren](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>Installationshinweise

- Siehe auch [Allgemeine Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) auf Azure Linux vorbereiten.
- Neuere VHDX-Format wird in Azure nicht unterstützt. Sie können den Datenträger mit Hyper-V-Manager oder das Cmdlet **Vhd konvertieren** VHD-Format konvertieren.
- Bei der Installation von linuxsystem empfohlen Standardpartitionen als LVM (oft Standard für viele Installationen) verwenden. Dies vermeidet LVM Namenskonflikte mit geklonten VMs, insbesondere dann, wenn ein Betriebssystemdatenträger muss immer eine andere VM zur Problembehandlung an. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) kann wahlweise auf Datenträger verwendet werden.
- Konfigurieren Sie eine Swap-Partition auf dem Betriebssystem-Laufwerk. Azure Linux-Agent kann eine Auslagerungsdatei auf der Festplatte temporäre Ressourcengruppe erstellen konfiguriert werden. Weitere Informationen finden in den folgenden Schritten.
- Alle die VHDs müssen Größen, die Vielfache von 1 MB sind.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Mit der Debian VHDs erstellt Azure verwalten

Gibt es Tools zum Generieren von Debian VHDs für Azure, wie die [Azure-Verwaltung](https://github.com/credativ/azure-manage) Skripts [Credativ](http://www.credativ.com/). Dies ist der empfohlene Ansatz und erstellen ein Bild von. Beispielsweise erstellt ein Debian 8 VHD führen Sie die folgenden Befehle herunterladen Azure verwalten (und Abhängigkeit) und das Azure_build_image-Skript ausführen:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Manuell Debian VHD vorbereiten

1. Wählen Sie im Hyper-V-Manager den virtuellen Computer.

2. Klicken Sie auf **Verbinden** , um ein Konsolenfenster für den virtuellen Computer zu öffnen.

3. Kommentieren Sie die Zeile für **Deb Cdrom** in `/etc/apt/source.list` VM gegen eine ISO-Datei festlegen.

4. Bearbeiten der `/etc/default/grub` -Datei und ändern Sie den **GRUB_CMDLINE_LINUX** -Parameter wie folgt, um weitere Parameter für Azure enthalten.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. Grub neu erstellen und ausführen:

        # sudo update-grub

6. Hinzufügen des Debian Azure Repositories /etc/apt/sources.list für Debian 7 oder 8:

    **Debian 7.x "Squeeze"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Installieren Sie Azure Linux-Agent:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Debian 7 ist es aus dem Repository Squeeze Backports 3,16-basierten Kernel ausgeführt. Erstellen Sie zunächst eine Datei namens /etc/apt/preferences.d/linux.pref mit dem folgenden Inhalt:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Führen Sie dann "Sudo apt-Get Install Linux-Image-amd64" neuen Kernel zu installieren.

8. Entziehen Sie dem virtuellen Computer auf Azure-Bereitstellung bereiten Sie vor und ausführen:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Klicken Sie auf **Aktion** -> Beenden Sie Hyper-V-Manager. Da Ihre VHD Linux kann jetzt in Azure hochgeladen werden.


## <a name="next-steps"></a>Nächste Schritte

Sie nun können die Debian virtuelle Festplatte verwendet zum Erstellen neuen virtueller Maschinen in Azure. Ist dies das erste Mal, dass Sie die VHD-Datei in Azure hochladen, finden Sie die Schritte 2 und 3 [Erstellen und Hochladen einer virtuellen Festplatte mit dem Betriebssystem Linux](virtual-machines-linux-classic-create-upload-vhd.md).
