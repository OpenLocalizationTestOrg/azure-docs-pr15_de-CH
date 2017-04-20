<properties
    pageTitle="Fügen Sie einen Datenträger für Linux VM | Microsoft Azure"
    description="Erfahren Sie, wie ein Azure VM unter Linux einen Datenträger an, und initialisieren sie einsatzbereit ist."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Wie einen Datenträger auf einer virtuellen Linux-Maschine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Finden Sie [mit dem Ressourcen-Manager-Bereitstellungsmodell Datenträger](virtual-machines-linux-add-disk.md)anfügen.

Sie können leere Festplatten und Datenträger, die Ihre Azure-VMs Daten anfügen. Beide Laufwerke sind VHD-Dateien in einem Konto Azure-Speicher. Als verbinden Sie die Festplatte müssen mit Datenträger auf einem Linux-Computer Sie initialisiert und bereit für die Verwendung formatieren. Dieser Artikeldetails leeren Festplatten und Datenträger bereits mit Daten zu Ihrer virtuellen Computer dann initialisieren und formatieren eine neue Festplatte anschließen.

> [AZURE.NOTE]In diesem Zusammenhang empfiehlt es sich, eine oder mehrere separate Festplatten zum Speichern des virtuellen Computers Daten verwenden. Beim Erstellen einer Azure virtuellen Computers hat es ein Betriebssystem und einen temporären Datenträger. **Verwenden Sie die temporäre Diskette nicht beständige Daten speichern.** Wie der Name schon sagt, wird die temporäre Speicherung. Sie bietet Redundanz keine Sicherung, da in Azure-Speicher befinden wird.
> Die temporäre Diskette in der Regel von Azure Linux-Agent verwaltet und automatisch **/mnt/resource** (oder **mnt** Ubuntu Bilder) bereitgestellt. Auf der anderen Seite Datenträger u. u. andere Namen vom Linux-Kernel etwas `/dev/sdc`, und partitionieren, formatieren und diese Ressource bereitstellen möchten. Finden Sie im [Benutzerhandbuch von Azure Linux Agent] [ Agent] Weitere Informationen.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Initialisieren Sie neuer Datenträger unter Linux

1. SSH für die VM. Weitere Informationen finden Sie unter [Anmelden bei einem virtuellen Computer mit Linux][Logon].

2. Anschließend Sie die Geräte-ID für die Datenträger initialisieren müssen. Es gibt zwei Wege:

    eine) Grep für SCSI-Geräte in den Protokollen, wie den folgenden Befehl:

            $sudo grep SCSI /var/log/messages

    Für aktuelle Ubuntu Distributionen müssen mit `sudo grep SCSI /var/log/syslog` da anmelden `/var/log/messages` möglicherweise standardmäßig deaktiviert.

    Die Kennung der letzten Datenträger finden, die in den Nachrichten hinzugefügt wurde, die angezeigt werden.

    ![Datenträger-Nachrichten abrufen](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

    ODER

    (b) mit den `lsscsi` Befehl die Geräte-Id zu. `lsscsi`installiert werden, indem entweder `yum install lsscsi` (Red hat basiert Distributionen) oder `apt-get install lsscsi` (Debian basiert Distributionen). Sie können den Datenträger finden nach _Lun_ oder **logische Gerätenummer**suchen. Z. B. _Lun_ für den Datenträgern Sie leicht erkennbar von `azure vm disk list <virtual-machine>` als:

            ~$ azure vm disk list TestVM
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        TestVM-2645b8030676c8f8.vhd  Linux
            data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    Vergleichen Sie diese Daten mit den `lsscsi` für dasselbe Beispiel für virtuelle Computer:

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

    Die letzte Zahl im Tupel in jeder Zeile wird die _Lun_. Siehe `man lsscsi` Weitere Informationen.

3. Aufgefordert werden Geben Sie den folgenden Befehl auf das Gerät zu erstellen:

        $sudo fdisk /dev/sdc


4. Wenn Sie aufgefordert werden, geben Sie **n** , um eine neue Partition erstellen.


    ![Gerät erstellen](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Geben Sie bei Aufforderung **p** damit der Partition die primäre Partition. Typ **1** zu der ersten Partition und dann eingeben, übernehmen Sie den Standardwert für den Zylinder. Auf einigen Systemen können sie die Standardwerte des ersten und letzten Sektoren statt der Zylinder anzeigen. Sie können diese Standardeinstellungen übernehmen.


    ![Partition erstellen](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)



6. Geben Sie **p** , um Einzelheiten über das Laufwerk, das partitioniert ist.


    ![Datenträgerinformationen](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)



7. Geben Sie **mit** die Einstellungen für den Datenträger zu schreiben.


    ![Schreiben der Festplatte](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Jetzt können Sie das Dateisystem für die neue Partition erstellen. Fügen Sie die Partitionsnummer an die Geräte-ID (im folgenden Beispiel `/dev/sdc1`). Das folgende Beispiel erstellt eine ext4 Partition auf /dev/sdc1:

        # sudo mkfs -t ext4 /dev/sdc1

    ![Dateisystem erstellen](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

    >[AZURE.NOTE] SuSE Linux Enterprise 11 Systeme unterstützen nur schreibgeschützten Zugriff für ext4-Dateisysteme. Für diese Systeme empfiehlt es als ext3 ext4 das neuen Dateisystem formatieren.


9. Erstellen Sie ein Verzeichnis für das neue Dateisystem wie folgt geladen:

        # sudo mkdir /datadrive


10. Schließlich können Sie das Laufwerk folgendermaßen bereitstellen:

        # sudo mount /dev/sdc1 /datadrive

    Der Datenträger kann jetzt als **/datadrive**verwenden.
    
    ![Erstellen Sie das Verzeichnis, und hängen Sie die Festplatte](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Das neue Laufwerk und/etc/fstab hinzufügen:

    Um sicherzustellen, dass das Laufwerk nach einem Neustart automatisch eingehängt ist müssen sie die Datei/Etc/Fstab hinzugefügt werden. Darüber hinaus wird empfohlen, die UUID (Universally Unique IDentifier) in/etc/fstab verwendet wird, anstatt nur der Gerätename (d. h. /dev/sdc1) auf. Mithilfe der UUID vermeidet den falschen Datenträger an einem angegebenen Speicherort bereitgestellt werden, erkennt das Betriebssystem ein Datenträgerfehler Boot und alle verbleibenden Datenträger dann diese Geräte-IDs zugewiesen wird. Die UUID des neuen Laufwerks finden können Sie das Dienstprogramm **Blkid** :

        # sudo -i blkid

    Die Ausgabe sieht etwa wie folgt:

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] **/ Etc/fstab** -Datei nicht ordnungsgemäß bearbeiten kann in einem nicht mehr startfähigen System führen. Wenn Sie unsicher sind, finden Sie in der Verteilung dieser Datei korrekt Informationen. Es wird auch empfohlen, vor der Bearbeitung eine Sicherungskopie der Datei/etc/fstab erstellt wird.

    Anschließend öffnen **Sie/etc/fstab** -Datei in einem Texteditor:

        # sudo vi /etc/fstab

    In diesem Beispiel verwenden wir den UUID-Wert für das neue **/dev/sdc1** Gerät, das in den vorherigen Schritten und Mountpoint **/datadrive**erstellt wurde. Ende der **Datei/etc/fstab** die folgende Zeile hinzufügen:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2

    Oder auf SuSE Linux Systemen müssen ein geringfügig anderes Format verwenden:

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    
    >[AZURE.NOTE] Die `nofail` Option wird sichergestellt, dass die VM startet, auch wenn das Dateisystem beschädigt ist oder der Datenträger beim Starten nicht vorhanden. Ohne diese Option können Sie Verhalten auftreten [Kann nicht SSH Linux VM FSTAB Fehlern](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)beschrieben.

    Nun können Sie testen, ob das Dateisystem ordnungsgemäß bereitgestellt, Aufheben der Bereitstellung und erneutes Bereitstellen des Dateisystems d.h. Das Beispiel Bereitstellungspunkt `/datadrive` in den vorhergehenden Schritten erstellt:

        # sudo umount /datadrive
        # sudo mount /datadrive

    Wenn die `mount` Befehl erzeugt einen Fehler, überprüfen Sie die Datei/Etc/Fstab auf korrekte Syntax. Wenn zusätzliche Datenlaufwerke oder Partitionen erstellt werden, in/Etc/Fstab separat eingeben.

    Schreibschutz das Laufwerk mit dem folgenden Befehl:

        # sudo chmod go+w /datadrive

>[AZURE.NOTE] Anschließend entfernen Datenträger Fstab bearbeiten könnte die VM neu gestartet. Dies ist häufig die meisten Distributionen bieten entweder die `nofail` oder `nobootwait` Fstab Optionen System, selbst wenn der Datenträger nicht zur boot-Zeit. Die Verteilung Dokumentation weitere Informationen zu diesen Parametern.

### <a name="trimunmap-support-for-linux-in-azure"></a>Unterstützung für Linux in Azure TRIM-ZUORDNUNG
Einige Linux-Kernels unterstützen TRIM-ZUORDNUNG um nicht verwendeten Blöcke auf der Festplatte zu löschen. Diese Vorgänge sind in erster Linie im Standardspeicher Azure, die Seiten gelöscht sind nicht mehr gültig und können gelöscht werden. Verwerfen Seiten sparen Kosten, wenn Sie große Dateien erstellen und löschen.

Es gibt zwei Arten Trimmen aktivieren unterstützen Linux VM. Finden Sie die Verteilung wie gewohnt empfohlen:

- Verwenden der `discard` Option mount `/etc/fstab`, beispielsweise:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Führen Sie die `fstrim` Befehl manuell über die Befehlszeile oder die Crontab regelmäßig hinzuzufügen:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL-CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Problembehandlung
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]


## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Verwendung von Linux VM in den folgenden Artikeln:

- [Anmelden bei einem virtuellen Computer mit Linux][Logon]

- [So trennen Sie einen Datenträger aus einem virtuellen Linux-Maschine](virtual-machines-linux-classic-detach-disk.md)

- [Verwenden der Azure-CLI mit klassischen Bereitstellungsmodell](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md
