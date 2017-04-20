<properties
    pageTitle="Erstellen und Hochladen einer VHD Linux in Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (VHD) mit dem Betriebssystem Linux."
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
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Informationen für die Verteilung nicht unterstützt #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Wichtig**: der Azure-Plattform SLA gilt für das Linux-Betriebssystem virtuellen Computern nur bei Verwendung eines [Distributionen unterstützt](virtual-machines-linux-endorsed-distros.md) . Alle Linux-Distributionen, die in Azure Bildergalerie bereitgestellt werden gebilligt Distributionen mit der erforderlichen Konfiguration.

- [Linux auf Azure - unterstützt Distributionen](virtual-machines-linux-endorsed-distros.md)
- [Unterstützung für Linux-Bilder in Microsoft Azure](https://support.microsoft.com/kb/2941892)

Alle Distributionen, die auf Windows Azure ausgeführte müssen eine Reihe von Komponenten der Plattform ordnungsgemäß ausgeführt haben.  Dieser Artikel ist jedoch keine umfassende wie jede Verteilung unterscheidet; und es ist durchaus möglich, dass selbst wenn die unten aufgeführten Kriterien erfüllen müssen Sie noch erheblich optimieren Ihre Linux-System, um sicherzustellen, dass sie ordnungsgemäß auf der Plattform ausgeführt wird.

Es wird deshalb empfohlen, mit einem unserer [Linux auf Azure unterstützt Distributionen](virtual-machines-linux-endorsed-distros.md) möglichst starten. Die folgenden Artikel führt Sie durch verschiedene gebilligten Linux-Distributionen vorbereiten, die in Azure unterstützt werden:

- **[Verteilung von centOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

Dieser Artikel konzentriert sich auf allgemeine Hinweise zum Linux-Distribution auf Windows Azure ausgeführte.


## <a name="general-linux-installation-notes"></a>Installationshinweise für allgemeine Linux ##

- Das VHDX-Format wird in Azure nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mit Hyper-V-Manager oder das Cmdlet Vhd konvertieren VHD-Format konvertieren. Verwenden Sie VirtualBox bedeutet dies **fester Größe** anstelle der dynamisch zugewiesen, wenn den Datenträger erstellen auswählen.

- Das Linux-System installieren wird *empfohlen* , Standardpartitionen als LVM (oft Standard für viele Installationen). Dies vermeidet LVM Namenskonflikte mit geklonten VMs, insbesondere dann, wenn ein Betriebssystemdatenträger muss immer eine andere identische VM zur Problembehandlung an. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) kann auf Datenträger verwendet werden.

- Kernel-Unterstützung für UDF-Dateisysteme Einhängen ist erforderlich. Beim ersten Start auf Azure ist die Bereitstellung Konfiguration Linux VM UDF-formatierte Medien übergeben, der dem Gast zugeordnet ist. Azure Linux-Agent muss das UDF-Dateisystem zum Lesen der Konfigurations und Bereitstellung der VM bereitstellen.

- Linux Kernel-Versionen unter 2.6.37 unterstützen keine NUMA auf Hyper-V mit größeren VM. Dieses Problem wirkt sich hauptsächlich auf ältere Distributionen, die mit Red Hat 2.6.32 Kernel und wurde in RHEL 6.6 (Kernel-2.6.32-504). Systeme mit benutzerdefinierten Kernel älter als 2.6.37 oder RHEL-basierten Kernel älter als 2.6.32-504 den Boot Parameter müssen `numa=off` auf der Kernel-Befehlszeile in grub.conf. Weitere Informationen finden Sie unter Red Hat [436883 KB](https://access.redhat.com/solutions/436883).

- Konfigurieren Sie eine Swap-Partition auf dem Betriebssystem-Laufwerk. Linux-Agent kann konfiguriert eine Auslagerungsdatei auf dem temporären Datenträger erstellen.  Weitere Informationen finden in den folgenden Schritten.

- Alle die VHDs müssen Größen, die Vielfache von 1 MB sind.


### <a name="installing-linux-without-hyper-v"></a>Installieren von Linux ohne Hyper-V ###

In einigen Fällen Linux-Installer darf keinen Treiber für Hyper-V in der anfänglichen Ramdisk (Initrd oder Initramfs), wenn es erkennt, dass es ausgeführt wird ein Hyper-V-Umgebung.  Bei Verwendung eines anderen Virtualisierung (Virtualbox, KVM, etc.) Linux-Image vorbereiten müssen Initrd um sicherzustellen, dass neu erstellen die `hv_vmbus` und `hv_storvsc` Kernel-Module werden auf die ursprünglichen Ramdisk.  Dies ist ein bekanntes Problem mindestens auf Systemen upstream Red Hat-Distribution.

Der Mechanismus für den Neuaufbau Initrd oder Initramfs Bild variieren je nach der Verteilung. Dokumentation der Verteilung oder das richtige Verfahren unterstützen.  Hier ist ein Beispiel für Neuerstellen Initrd mit den `mkinitrd` Dienstprogramm:

Sichern Sie zunächst vorhandene Initrd-Images:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Als Nächstes rebuild Initrd mit den `hv_vmbus` und `hv_storvsc` -Kernel-Module:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Ändern der Größe von virtuellen Festplatten ###

Abbildern auf Azure muss eine virtuelle Größe auf 1 MB ausgerichtet.  Normalerweise sollten Festplatten mit Hyper-V erstellt bereits ordnungsgemäß ausgerichtet.  Wenn die virtuelle Festplatte nicht korrekt ausgerichtet ist dann erhalten Sie eine Fehlermeldung ähnlich der folgenden beim Erstellen ein *Bild* von der virtuellen Festplatte:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Abhilfe können Sie die VM mit der Hyper-V-Manager-Konsole oder [Größe VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell-Cmdlet ändern.  Wenn Sie nicht in einer Windows-Umgebung ausgeführt wird empfohlen Img Qemu konvertieren (falls erforderlich) und Größe der virtuellen Festplatte.

> [AZURE.NOTE] Es ist ein bekanntes Problem in Img Qemu Versionen > = eine falsch formatierte VHD führt 2.2.1. Das Problem wurde in QEMU 2.6 behoben. Es wird empfohlen, Qemu-Img 2.2.0 oder niedriger oder 2.6 oder höher aktualisieren. Referenz: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Ändern der Größe der virtuellen Festplatte direkt mit Tools wie `qemu-img` oder `vbox-manage` möglicherweise in einem nicht mehr startfähigen VHD.  So wird ersten konvertieren die virtuellen Festplatte um eine RAW Image empfohlen.  Wenn die VM-Image als Image, REINE Daten (Standard für einige Hypervisoren wie KVM) bereits erstellt wurde, können Sie diesen Schritt überspringen:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Berechnen Sie die erforderliche Größe des Festplatten-Images, um sicherzustellen, dass die virtuelle Größe auf 1 MB ist.  Bash Shellskript kann helfen.  Verwendet das Skript "`qemu-img info`" virtuelle Größe das Datenträgerabbild bestimmt und berechnet dann den nächsten 1 MB Größe:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Ändern Sie raw-Festplatte mit $Rounded_size wie im obigen Skript:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Jetzt die Konvertierung RAW auf einer virtuellen Festplatte fester Größe:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Linux-Kernel Vorschriften ##

Die Linux Integration Services (LIS) Treiber für Hyper-V und Azure werden direkt an den upstream Kernel bereitgestellt. Viele Distributionen, die aktuelle Linux Kernel-Version (d. h. 3.x) enthalten diese Treiber bereits oder andernfalls die Kernel bieten mehr Versionen dieser Treiber.  Diese Treiber werden laufend aktualisiert im upstream-Kernel mit neuen Updates und Funktionen nach Möglichkeit empfohlen wird ein [Sichtvermerk Verteilung](virtual-machines-linux-endorsed-distros.md) ausgeführt, die die Fehlerbehebungen und Updates enthalten.

Wenn Sie eine Variante von Red Hat Enterprise Linux Versionen **6.0 6.3**ausgeführt, müssen Sie für Hyper-V LIS-Treiber installieren. Die Treiber können [an dieser Stelle](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409)gefunden. RHEL **6.4 +** (und Derivate) LIS-Treiber bereits in den Kernel enthalten und daher keine zusätzlichen Pakete müssen diese Systeme in Azure ausgeführt.

Wenn ein benutzerdefinierter Kernel erforderlich ist, empfiehlt es eine neuere Kernelversion (d. h. **3.8 +**) verwenden. Für diese Verteilung oder Kreditoren ihre eigenen Kernel verwalten werden einige Aufwand müssen regelmäßig Backport LIS-Treiber vom upstream-Kernel benutzerdefinierte Kernel.  Auch wenn bereits eine relativ neue Kernel-Version ausführen, wird empfohlen alle vorgeschalteten Fixes LIS-Treiber und Backport den Überblick nach Bedarf. Der Speicherort der Quelldateien LIS-Treiber ist in der Datei [BETREUER](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) in der Quellstruktur Linux Kernel verfügbar:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Auf ein minimum die Abwesenheit der folgenden Patches bekanntermaßen Probleme Azure und so müssen diese im Kernel enthalten sein. Diese Liste ist nicht erschöpfend oder für alle Distributionen:

- [Ata_piix: Laufwerke für die Hyper-V-Treiber standardmäßig verschieben](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [Storvsc: Konto für Pakete in Transit im Pfad zurücksetzen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [Storvsc: Vermeiden der Verwendung von WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [Storvsc: RAID und virtuellen Host Adaptertreiber Schreiben gleichen deaktivieren](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [Storvsc: NULL-Zeiger dereferenziert korrigieren](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [Storvsc: Ring Puffer möglicherweise fehlschlagen e/a-fixieren](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [Scsi_sysfs: Doppelte Ausführung __scsi_remove_device schützen](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>Azure Linux-Agent ##

[Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md) (Waagent) muss ordnungsgemäß auf eine virtuellen Linux-Maschine in Azure bereitstellen. Sie können die neueste Version, Probleme mit oder Pull Anträge auf [Linux Agent GitHub Repo](https://github.com/Azure/WALinuxAgent).

- Linux-Agent wird unter Apache 2.0-Lizenz freigegeben. Viele Versionen bereits ermöglichen RPM oder Deb Pakete für den Agent, und dies in einigen Fällen kann installiert und mit geringem Aufwand aktualisiert.

- Azure Linux-Agent benötigt Python v2. 6 +.

- Der Agent muss auch Python-pyasn1-Modul. Die meisten Distributionen bieten dies als separates Paket installiert werden kann.

- In einigen Fällen kann der Azure Linux-Agent mit NetworkManager nicht kompatibel. Viele Distributionen bereitgestellten RPM-Deb-Pakete NetworkManager Konflikt Waagent Paket konfigurieren und somit deinstalliert NetworkManager Agentenpaket Linux installieren.


## <a name="general-linux-system-requirements"></a>Allgemeine Linux System Requirements ##

- Ändern der Kernel-Startzeile im GRUB oder GRUB2 die folgenden Parameter enthalten. Auch dadurch alle Konsole werden Nachrichten an den ersten seriellen Anschluss Azure helfen kann mit Debuggen Probleme:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Auch dadurch alle Konsole werden Nachrichten an den ersten seriellen Anschluss Azure helfen kann mit debugging verzichten.

    Darüber hinaus *Entfernen* die folgenden Parameter sollten ggf.:

        rhgb quiet crashkernel=auto

    Grafik- und quiet Boot eignen sich nicht in einer Cloudumgebung, wo wir alle Protokolle an den seriellen Anschluss gesendet werden soll.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass dieser Parameter reduziert die Größe des verfügbaren Speichers in der VM von mindestens 128MB die VM kleinere Probleme möglicherweise.

- Installieren von Azure Linux-Agent

    Azure Linux-Agent ist erforderlich für die Bereitstellung von Linux-Image in Azure.  Viele Versionen Bereitstellen des Agenten als RPM oder Deb-Paket (das Paket wird normalerweise 'WALinuxAgent' oder 'Walinuxagent' bezeichnet).  Der Agent kann auch manuell mithilfe der Schritte im [Handbuch für Linux-Agent](virtual-machines-linux-agent-user-guide.md)installiert werden.

- Sicherstellen Sie, dass der SSH-Server installiert und konfiguriert, beim Booten gestartet.  Dies ist normalerweise die Standardeinstellung.

- Erstellen Sie Auslagerungsspeicher nicht auf dem Betriebssystem-Datenträger

    Azure Linux-Agent kann automatisch die lokale Ressource Datenträger, der die VM zugeordnet ist, nach der Bereitstellung in Azure Auslagerungsspeicher konfigurieren. Beachten Sie, dass die lokale Festplatte *temporärer* Speicherplatz ist und gelöscht werden, kann Wenn die VM hat. Nach der Installation von Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie die folgenden Parameter in /etc/waagent.conf entsprechend:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Im letzten Schritt führen Sie die folgenden Befehle auf den virtuellen Computer zu entziehen:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Auf Virtualbox kann Fehlermeldung folgenden nach Ausführung ' Waagent-force - entziehen ": `[Errno 5] Input/output error`. Diese Fehlermeldung ist nicht kritisch und kann ignoriert werden.

- Dann müssen den virtuellen Computer herunterfahren und die virtuelle Festplatte in Azure hochladen.
