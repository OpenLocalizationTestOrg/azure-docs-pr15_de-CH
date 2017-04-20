<properties
    pageTitle="Optimierung Ihrer Linux VM in Azure | Microsoft Azure"
    description="Tipps Optimierung, stellen Sie sicher, dass Ihre Linux VM für optimale Leistung von Azure"
    keywords="virtuellen Linux-Maschine, virtuellen Linux VM ubuntu" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Optimieren Sie Ihrer Linux VM in Azure

Erstellen einer virtuellen Linux-Maschine (VM) ist einfach von der Befehlszeile aus oder über das Portal. Dieses Lernprogramm zeigt wie es eingerichteten optimieren die Leistung auf der Plattform Microsoft Azure. Dieses Thema verwendet ein Ubuntu Server VM, jedoch können auch virtuellen Linux-Maschine mit [Ihren eigenen Bildern als Vorlagen](virtual-machines-linux-create-upload-generic.md).  

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Thema wird vorausgesetzt, bereits eine funktionierende Azure-Abonnement ([kostenlose Testversion Anmeldung](https://azure.microsoft.com/pricing/free-trial/)) [Azure-CLI installiert](../xplat-cli-install.md) und haben eine VM in Azure-Abonnement bereits bereitgestellt. Vor etwas mit Azure - müssen Sie Ihr Abonnement zu authentifizieren. Geben Sie hierfür mit Azure CLI `azure login` interaktiven Prozess gestartet. 

## <a name="azure-os-disk"></a>Azure Betriebssystemdatenträger

Nach dem Erstellen einer Linux Vm in Azure hat zwei Datenträger zugeordnet. SDA ist Festplatte OS ist/dev/sdb als temporären Datenträger.  Verwenden der OS Hauptfestplatte (/ Dev/Sda) für alles außer dem Betriebssystem optimiert für schnelle VM Booten und bietet keine gute Leistung für Ihre Arbeitslasten. Ein oder mehrere Laufwerke für die VM zu permanenten zugeordnet und Speicher für Ihre Daten. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Hinzufügen von Datenträgern für Größe und Leistung 

Anhand der VM-Größe, können Sie bis zu 16 zusätzliche Festplatten auf eine ein-Serie 32 Festplatten auf D-Serie und 64 Festplatten G-Serie machine - bis zu 1 TB. Zusätzliche Datenträger wird der Speicherplatz und die IOPS-Anforderung en Bedarf hinzufügen. Jeder Datenträger hat ein Performance-Ziel von 500 IOps für Standardspeicher bis 5000 IOps pro Datenträger für Premium-Speicher  Weitere Informationen zu Premium-Datenträgern finden Sie in [Storage Premium: Hochleistungsspeicher für Azure VMs](../storage/storage-premium-storage.md)

Zu den höchsten IOps Premium-Datenträger, in dem die cacheeinstellungen "ReadOnly" oder "None" festgelegt wurden, müssen Sie "Handelshemmnisse" deaktivieren, während das Dateisystem unter Linux. Barrieren sind nicht erforderlich, da Schreibvorgänge auf Premium unterstützt Datenträger für diesen Cache Settings sind.

- Verwenden **ReiserFS**Barrieren mit der Einhängeoption deaktivieren "Barriere = keine" (verwenden Sie zum Aktivieren der Hindernisse, "Barriere Flush =")
- Verwenden **ext3-ext4**mit der Einhängeoption Hindernisse deaktivieren "Barriere = 0" (verwenden Sie zum Aktivieren der Barrieren, "Barriere = 1")
- Verwenden Sie **XFS**deaktivieren Hindernisse mit der Mount-Option "Nobarrier" (zum Aktivieren der Barrieren, verwenden Sie die Option "Grenze")

## <a name="storage-account-considerations"></a>Storage Aspekte

Wenn Sie Linux VM in Azure erstellen, sollten Sie sicherstellen, dass Festplatten Speicher Konten im Bereich der VM Nähe und minimieren Netzwerk anfügen.  Jedes Konto Standardspeicher kann maximal 20 IO/s und einer Kapazität von 500 TB Größe.  Dies funktioniert auf 40 verwendeten Datenträger, einschließlich der Betriebssystem-Datenträger und alle Datenträger, die Sie erstellen. Für Premium-Speicherkonten ist unbegrenzt maximale IOps jedoch ist 32 TB Größe beschränkt. 

Beim Umgang mit hoher Arbeitslasten IOps für Datenträger Standardspeicher gewählt haben, müssen Sie mehrere Speicherkonten um sicherzustellen, dass 20.000 IOps Limit für Standardspeicher Konten nicht erreicht haben die Festplatten verteilt. VM kann aus Datenträgern in verschiedenen Konten und Kontenarten die optimale Konfiguration zu Speicher enthalten. 

## <a name="your-vm-temporary-drive"></a>VM temporären Laufwerk

In der Standardeinstellung beim Erstellen einer VM bietet Azure ein OS (/ Dev/Sda) und einen temporären Datenträger (/ Dev/Sdb).  Alle zusätzlichen Datenträger, die Sie hinzufügen werden als/dev/SDC, /dev/sdd, /dev/sde usw.. Alle Daten auf dem temporären Datenträger (/ Dev/Sdb) ist nicht dauerhaft und können verloren gehen, wenn bestimmte Ereignisse Umschichtung VM Größe oder Wartung einen Neustart des VM erzwingt.  Größe und Typ des temporären Datenträgers bezieht sich auf die VM-Größe zur Bereitstellungszeit gewählte. Bei einem Premium Größe VMs (Serie DS, G und DS_V2) das temporäre Laufwerk von lokalen SSD für zusätzliche Leistung von bis zu 48 k IOps stützt. 

## <a name="linux-swap-file"></a>Linux Swap-Datei

Ist der Azure-VM aus einem Bild Ubuntu oder CoreOS können Sie dann CustomData Cloud Init Cloud-Konfiguration an. Wenn Sie [benutzerdefiniertes Linux Bild hochgeladen](virtual-machines-linux-upload-vhd.md) , die Cloud-Init verwendet, auch Swap-Partitionen mit Cloud-Init konfiguriert.

Cloud-Init verwenden Sie Ubuntu Cloud Bilder die Swap-Partition konfigurieren. Siehe folgende Seite Ubuntu Wiki für Weitere Informationen: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Bei Bildern ohne Initialisierung Cloud haben VM-Abbilder von Azure Marketplace bereitgestellt eine VM Linux in das Betriebssystem integriert. Dieser Agent ermöglicht die VM mit verschiedenen Azure interagieren. Angenommen, ein Standardbild Azure Marketplace bereitgestellt haben, müssen Sie Folgendes tun, um die Einstellungen Linux Swap korrekt zu konfigurieren:

Suchen und zwei Einträge in der Datei **/etc/waagent.conf** . Sie steuern, ob eine dedizierte Auslagerungsdatei und die Auslagerungsdatei. Parameter suchen ändern `ResourceDisk.EnableSwap=N` und`ResourceDisk.SwapSizeMB=0` 

Sie müssen wie folgt ändern:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size MB bedarfsgerechte} 

Nach der Änderung müssen Sie einen Neustart der Waagent oder Linux VM entsprechend.  Sie kennen die Änderungen umgesetzt und Verwendung eine Auslagerungsdatei erstellt wurde das `free` Befehl, um freien Speicherplatz. Im folgenden Beispiel hat eine 512MB Auslagerungsdatei durch Ändern der Datei waagent.conf erstellt.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>E/a-scheduling-Algorithmus für Premium-Speicher

Mit der 2.6.18 Linux-Kernel Standard e/a-scheduling-Algorithmus von Frist CFQ (vollständig fair Queuing-Algorithmus wurde). RAM e/a-Muster ist unerheblich Unterschied Leistungsunterschiede zwischen CFQ und Stichtag.  Bei SSD-basierte überwiegend sequenziellen Datenträger-e/a-Muster ist erzielen auf der Algorithmus NOOP oder Frist eine bessere e/a-Leistung.

### <a name="view-the-current-io-scheduler"></a>Anzeigen des aktuellen e/a-Planers

Verwenden Sie den folgenden Befehl ein:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Sie sehen nach Ausgabe, die aktuellen Planer angibt.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Das aktuelle Gerät (/ Dev/Sda) i/o-Algorithmus planen

Verwenden Sie die folgenden Befehle:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Einstellung für sda allein ist nicht sinnvoll. E/a-Muster beherrscht sequenzieller Zugriff auf alle Datenträger festgelegt werden muss.  

Die folgende Ausgabe zeigt an, dass diese grub.cfg erfolgreich erstellt wurde und der Standardplaner NOOP aktualisiert wurde, sollte angezeigt werden.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Familie Verteilung Redhat benötigen Sie nur den folgenden Befehl:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Mit Software-RAID, erzielen höhere I / Ops

Benötigen Ihre Arbeitslasten mehr IOps als ein einzelner Datenträger bereitstellen kann, müssen Sie eine Software RAID-Konfiguration aus mehreren Datenträgern. Da Azure bereits Datenträger Resiliency Ebene lokale Fabric durchführt, erreichen Sie die höchste Leistung von einem RAID 0 Striping.  Sie möchten bereitstellen, erstellen Sie neue Festplatten in der Azure-Umgebung und Linux VM vor Partitionierung, Formatierung und Montage Laufwerke zuordnen.  Weitere Informationen zur Konfiguration einer Software-RAID-Setups auf Ihrem Linux VM in Azure finden im Dokument **[Konfigurieren des Software-RAID auf Linux](virtual-machines-linux-configure-raid.md)** .


## <a name="next-steps"></a>Nächste Schritte

Beachten Sie, dass wie alle Optimierung Diskussionen Tests durchzuführenden vor und nach jeder Änderung die Auswirkung, die der Änderung, messen.  Optimierung ist ein Schritt für Schritt, die unterschiedliche Ergebnisse auf verschiedenen Computern in Ihrer Umgebung.  Was für eine Konfiguration möglicherweise nicht für andere arbeiten.

Einige nützliche Links zu zusätzlichen Ressourcen: 

- [Premium-Speicher: Hochleistungsspeicher für Azure Virtual Machine-Arbeitslasten](../storage/storage-premium-storage.md)
- [Azure Linux-Agent-Benutzerhandbuch](virtual-machines-linux-agent-user-guide.md)
- [Optimieren der Leistung MySQL Azure Linux VMs](virtual-machines-linux-classic-optimize-mysql.md)
- [Konfigurieren von Software-RAID auf Linux](virtual-machines-linux-configure-raid.md)
