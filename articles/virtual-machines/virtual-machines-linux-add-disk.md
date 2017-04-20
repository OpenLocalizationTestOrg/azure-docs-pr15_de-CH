<properties
    pageTitle="Hinzufügen eines Datenträgers zu Linux VM | Microsoft Azure"
    description="Informationen Sie zum Linux VM andauernde Datenträger hinzufügen"
    keywords="virtuellen Linux-Maschine Festplatte hinzufügen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Hinzufügen eines Datenträgers zu Linux VM

Dieser Artikel beschreibt, wie eine dauerhafte Datenträger VM an, damit Ihre Daten - zu erhalten, selbst wenn die VM Wartung oder Größe ausgebessert. Fügen Sie einen Datenträger benötigen [Azure CLI](../xplat-cli-install.md) im Ressourcen-Manager-Modus konfiguriert (`azure config mode arm`).  

## <a name="quick-commands"></a>Schnellzugriff

In den folgenden Beispielen Befehl ersetzen die Werte zwischen &lt; und &gt; mit den Werten aus Ihrer Umgebung.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Fügen Sie einen Datenträger

Anfügen eines neuen Datenträgers ist schnell. Typ `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` erstellen und eine neue GB-Festplatte für die VM. Wenn Sie nicht explizit ein Speicherkonto identifizieren, erstellte Laufwerk in demselben Speicherkonto befindet sich die Betriebssystem-Datenträger befindet.  Es sollte wie folgt aussehen:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Ausgabe

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Verbinden Sie mit Linux VM auf die neue Festplatte

> [AZURE.NOTE] In diesem Thema verbindet einen virtuellen Computer mit Benutzernamen und Kennwörtern. Um öffentliche und private Schlüsselpaare mit VM kommunizieren, finden Sie unter [verwenden SSH mit Linux auf Azure](virtual-machines-linux-mac-create-ssh-keys.md). Ändern Sie die **SSH** -Verbindung VMs mit erstellt die `azure vm quick-create` Befehl mithilfe der `azure vm reset-access` Befehl **SSH** -Zugriff vollständig zurückgesetzt, hinzufügen oder Entfernen von Benutzern oder öffentliche Schlüssel Dateien zum sicheren Zugriff hinzufügen.

Sie müssen SSH in Azure-VM Partition formatieren und Bereitstellen der neuen Festplatte Linux VM verwenden können. Wenn Sie nicht mit **ssh**vertraut sind, der Befehl erfolgt `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`, und sieht folgendermaßen aus:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Ausgabe

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Da die VM verbunden sind, können Sie eine Festplatte.  Zuerst finden Sie den Datenträger mit `dmesg | grep SCSI` (die Methode, die zu dem neuen Datenträger kann abweichen). In diesem Fall sieht etwa so:

```bash
dmesg | grep SCSI
```

Ausgabe

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

Bei diesem Thema die `sdc` Datenträger ist diejenige, die wir wollen. Nun partitionieren die Festplatte mit `sudo fdisk /dev/sdc` – vorausgesetzt, dass in Ihrem Fall der Datenträger war `sdc`, und stellen eine primäre Festplatte auf Partition 1 und übernehmen Sie die Standardeinstellungen.

```bash
sudo fdisk /dev/sdc
```

Ausgabe

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Erstellen Sie die Partition durch Eingabe `p` aufgefordert werden:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Und ein Dateisystem auf der Partition mit **Mkfs** Befehl der Dateisystemtyp und den Gerätenamen angeben. In diesem Thema verwenden wir `ext4` und `/dev/sdc1` ab:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Ausgabe

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Jetzt erstellen wir ein Verzeichnis bereitstellen mithilfe `mkdir`:

```bash
sudo mkdir /datadrive
```

Und Sie Verzeichnis mit `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Der Datenträger ist jetzt einsatzbereit als `/datadrive`.

```bash
ls
```

Ausgabe

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Um sicherzustellen, dass das Laufwerk nach einem Neustart automatisch eingehängt ist müssen sie die Datei/Etc/Fstab hinzugefügt werden. Darüber hinaus wird empfohlen, dass UUID (Universally Unique IDentifier) in/Etc/Fstab verwendet wird, anstatt nur den Gerätenamen auf (z. B. `/dev/sdc1`). Erkennt das Betriebssystem einen Fehler während des Startvorgangs, vermeidet der UUID den falschen Datenträger an einem angegebenen Speicherort bereitgestellt. Verbleibende Datenträger würden dann die gleichen Geräte-IDs zugewiesen werden. Verwenden Sie die UUID des neuen Laufwerks, um das Dienstprogramm **Blkid** :

```bash
sudo -i blkid
```

Die Ausgabe sieht etwa wie folgt:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] **/ Etc/fstab** -Datei nicht ordnungsgemäß bearbeiten kann in einem nicht mehr startfähigen System führen. Wenn Sie unsicher sind, finden Sie in der Verteilung dieser Datei korrekt Informationen. Es wird auch empfohlen, vor der Bearbeitung eine Sicherungskopie der Datei/etc/fstab erstellt wird.

Anschließend öffnen **Sie/etc/fstab** -Datei in einem Texteditor:

```bash
sudo vi /etc/fstab
```

In diesem Beispiel verwenden wir den UUID-Wert für das neue **/dev/sdc1** Gerät, das in den vorherigen Schritten und Mountpoint **/datadrive**erstellt wurde. Ende der **Datei/etc/fstab** die folgende Zeile hinzufügen:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Später entfernen Datenträger ohne Fstab bearbeiten könnte die VM neu gestartet. Die meisten Distributionen bieten entweder die `nofail` oder `nobootwait` Fstab Optionen. Diese Optionen ermöglichen, selbst wenn der Datenträger nicht beim Booten ein Systems. Die Verteilung Dokumentation weitere Informationen zu diesen Parametern.

>[AZURE.NOTE] Die Option **Nofail** wird sichergestellt, dass die VM startet, auch wenn das Dateisystem beschädigt ist oder der Datenträger beim Starten nicht vorhanden. Ohne diese Option können Sie Verhalten auftreten [Kann nicht SSH Linux VM FSTAB Fehlern](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/) beschrieben


### <a name="trimunmap-support-for-linux-in-azure"></a>Unterstützung für Linux in Azure TRIM-ZUORDNUNG
Einige Linux-Kernels unterstützen TRIM-ZUORDNUNG um nicht verwendeten Blöcke auf der Festplatte zu löschen. Dies ist in erster Linie im Standardspeicher Azure, die Seiten gelöscht sind nicht mehr gültig und können gelöscht werden. Dies spart Kosten, wenn Sie große Dateien erstellen und löschen.

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

- Beachten Sie, dass der neuen Festplatte nicht verfügbar, der VM wenn Neustart, wenn Sie Informationen zu Ihrer [Fstab](http://en.wikipedia.org/wiki/Fstab) -Datei schreiben.
- Um sicherzustellen, dass Ihre Linux VM ordnungsgemäß konfiguriert ist, überprüfen Sie [optimieren die Leistung Ihres Linux-Computer](virtual-machines-linux-optimization.md) empfohlen.
- Erweitern Sie die Speicherkapazität zusätzliche Festplatten und [RAID konfigurieren](virtual-machines-linux-configure-raid.md) für zusätzliche Leistung.
