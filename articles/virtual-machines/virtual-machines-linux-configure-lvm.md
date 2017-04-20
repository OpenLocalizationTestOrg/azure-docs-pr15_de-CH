<properties 
    pageTitle="Konfigurieren Sie auf einem virtuellen Computer mit Linux LVM | Microsoft Azure" 
    description="Erfahren Sie, wie LVM auf Linux in Azure konfigurieren." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>LVM auf Linux VM in Azure konfigurieren

Dieses Dokument diskutieren Azure virtuellen Maschine Logical Volume Manager (LVM) konfigurieren. Während LVM auf jedem virtuellen Computer angefügten Datenträger konfigurieren kann, werden standardmäßig die meisten Cloud-Bilder nicht LVM auf dem Betriebssystem-Datenträger konfiguriert haben. Dadurch wird verhindert Probleme mit doppelten Volume-Gruppen, wenn der Betriebssystem-Datenträger immer eine andere VM Verteilung und Typ z. B. im Fall einer Wiederherstellung zugeordnet ist. Daher empfiehlt es sich nur mit LVM auf dem Datenträger.


## <a name="linear-vs-striped-logical-volumes"></a>Arithmetischen und logischen Stripesetvolumes

LVM kann eine Anzahl von physischen Festplatten in ein einzelnes Speicher-Volume kombiniert verwendet werden. Standardmäßig erstellt LVM normalerweise linear logische Volumes, sodass der physische Speicher verkettet. In diesem Fall Lese-und Schreibvorgänge in der Regel nur auf einen einzelnen Datenträger erhalten. Im Gegensatz dazu können wir auch logische Stripesetvolumes erstellen, Lese- und Schreibvorgänge auf mehrere Datenträger enthaltenen Volumes (d. h. ähnelt RAID 0) verteilt werden. Aus Leistungsgründen ist es wahrscheinlich möchten Sie die logischen Volumes Streifen, Lese- und Schreibvorgänge alle angeschlossenen Datenträger verwenden.

Dieses Dokument wird das Kombinieren mehrere Datenträger in einem einzelnen Volume-Gruppe, und erstellen Sie ein Stripesetvolume logische beschrieben. Die Schritte sind mit den meisten Distributionen etwas verallgemeinert. In den meisten Fällen sind Dienstprogramme und Workflows zum Verwalten von LVM auf Azure nicht grundlegend anders als andere Umgebung. Wie gewohnt auch finden Sie in den Linux-Hersteller Dokumentationen und best Practices für Ihre besondere Verteilung LVM mit.


## <a name="attaching-data-disks"></a>Datenträger anfügen
Eine möchten Verwendung LVM mit mindestens zwei leere Datenträger starten. Basierend auf Ihren Bedürfnissen IO können Sie Festplatten hinzufügen, die in unser Standardspeicher mit bis zu 500 IO/s pro Datenträger oder unseren Premium-Speicher mit bis zu 5000 IO/s pro Datenträger gespeichert sind. In diesem Artikel werden nicht ins Detail gehen, wie bereitstellen und Datenträger in einem virtuellen Linux-Maschine. Finden Sie in der Microsoft Azure Artikel [Fügen Sie einen Datenträger](virtual-machines-linux-add-disk.md) für detaillierte Informationen an eine leere Datenträger auf einer virtuellen Linux-Maschine in Azure.

## <a name="install-the-lvm-utilities"></a>Installieren Sie die LVM-Dienstprogramme

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL CentOS und Oracle Linux**

        # sudo yum install lvm2

- **SLES 12 und openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    In SLES11 müssen auch /etc/sysconfig/lvm bearbeiten und festlegen `LVM_ACTIVATED_ON_DISCOVERED` auf "aktivieren":

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>LVM konfigurieren
In diesem Handbuch wird davon ausgegangen, haben Sie drei Datenträger, die als bezeichnet werden angehängt `/dev/sdc`, `/dev/sdd` und `/dev/sde`. Beachten Sie, dass diese möglicherweise nicht immer die gleichen Pfadnamen in der VM. Führen Sie "`sudo fdisk -l`" oder ähnliche verfügbaren Festplatten aufgelistet.

1. Vorbereiten der physischen Volumes:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Erstellen einer Volume-Gruppe. In diesem Beispiel rufen wir die Volume Group "Daten-vg01":

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. Erstellen Sie die logischen Volumes. Der Befehl im folgenden erstellen ein einzelnes logisches Volume namens "Daten-lv01" umfassen die gesamte Volume-Gruppe, jedoch ist es auch möglich, mehrere logische Volumes in der Volume-Gruppe zu erstellen.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. Der logische Datenträger formatieren

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] SLES11 mit "-t ext3" statt ext4. SLES11 unterstützt nur nur-Lese-Zugriff auf ext4-Dateisysteme.


## <a name="add-the-new-file-system-to-etcfstab"></a>Das neue Dateisystem/etc/fstab hinzufügen

**Vorsicht:** / Etc/fstab-Datei nicht ordnungsgemäß bearbeiten kann in einem nicht mehr startfähigen System führen. Wenn Sie unsicher sind, finden Sie die Verteilung der Dokumentation wie diese Datei zu bearbeiten. Es wird auch empfohlen, vor der Bearbeitung eine Sicherungskopie der Datei/etc/fstab erstellt wird.

1. Erstellen Sie die gewünschten Bereitstellungspunkts für das neue Dateisystem beispielsweise:

        # sudo mkdir /data


2. Suchen Sie den Pfad des logischen Volumes

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. Öffnen Sie/etc/fstab in einem Texteditor, und fügen Sie einen Eintrag für das neue Dateisystem beispielsweise:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Anschließend speichern Sie und schließen Sie/etc/fstab.


4. Überprüfen Sie, ob der/Etc/Fstab-Eintrag richtig ist:

        # sudo mount -a

    Wenn dieser Befehl in einer Fehlermeldung führt überprüfen Sie die Syntax in der Datei/Etc/Fstab.

    Nächste Ausführung der `mount` aus, um sicherzustellen, das Dateisystem bereitgestellt:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Optional) Failsafe Boot-Parameter in/etc/fstab

    Viele Distributionen enthalten entweder die `nobootwait` oder `nofail` Bereitstellen von Parametern, die die Datei/Etc/Fstab hinzugefügt werden können. Dieser Parameter lässt für Fehler bei einem bestimmten Dateisystem und das linuxsystem weiterhin, selbst wenn das RAID-Dateisystem ordnungsgemäß geladen werden kann. Siehe Dokumentation weitere Informationen über diese Parameter der Verteilung.

    Beispiel (Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
