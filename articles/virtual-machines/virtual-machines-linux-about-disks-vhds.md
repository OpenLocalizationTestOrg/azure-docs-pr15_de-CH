<properties
    pageTitle="Laufwerke und Festplatten für Linux VMs | Microsoft Azure"
    description="Lernen Sie die Grundlagen von Datenträgern und Festplatten für virtuelle Linux-Computer in Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Festplatten für Azure virtuelle Maschinen zu Datenträgern

Wie andere Computer verwenden Sie virtuelle Computer in Azure Datenträger als Ort ein Betriebssystem, Anwendung und Daten zu speichern. Alle Azure virtuelle Computer verfügen über mindestens zwei Datenträger – ein Linux Betriebssystem (bei Linux VM) und einen temporären Datenträger. Die Betriebssystem-CD aus einem Abbild erstellt und Betriebssystem-Datenträger und das Bild tatsächlich virtuelle Festplatten (VHDs) in einem Konto Azure-Speicher gespeichert werden. Virtuelle Computer können auch ein oder mehrere Datenträger, die auch als virtuelle Festplatten gespeichert sind. Dieser Artikel steht auch für [virtuelle Windows-Maschinen](virtual-machines-windows-about-disks-vhds.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Haben Sie einen Moment, Hilfe zu Azure Linux VM-Dokumentation mit dieser [Kurzübersicht](https://aka.ms/linuxdocsurvey) Erfahrungen. Jede Antwort kann wir Ihnen helfen, Ihre Arbeit.

## <a name="operating-system-disk"></a>Betriebssystem-CD

Jeder virtuellen Maschine verfügt einen angefügten Betriebssystem-Datenträger. Als eine SATA-Festplatte und als Laufwerk C: standardmäßig. Diese Diskette enthält maximal 1023 Gigabyte (GB). 

##<a name="temporary-disk"></a>Temporärer Speicherplatz

Die temporäre Diskette wird automatisch für Sie erstellt. Auf virtuelle Linux-Computer die Festplatte ist in der Regel/dev/sdb formatiert und von Azure Linux-Agent /mnt/resource bereitgestellt.

Die Größe des temporären Datenträgers hängt von der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größen für virtuelle Linux-Computer](virtual-machines-linux-sizes.md).

>[AZURE.WARNING] Speichern Sie nicht Daten auf dem temporären Datenträger. Temporären Speicher für Programme und Prozesse bereitstellt, und nur Daten wie Seite oder Swap-Dateien speichern soll. 

Weitere Informationen zur Verwendung Azure temporären Speicherplatz finden Sie unter [Understanding temporäre Laufwerk auf Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Datenträger

Ein Datenträger ist eine virtuelle Festplatte an einem virtuellen Computer Daten oder andere Daten zu speichern. Datenträger als SCSI-Laufwerke registriert und mit einer Auswahl von Buchstaben gekennzeichnet.  Jeder Datenträger verfügt über maximal 1023 GB. Die Größe des virtuellen Computers bestimmt wie viele Datenträger und Speichertyp Anfügen verwenden Datenträger hosten können.

>[AZURE.NOTE] Weitere Informationen zu virtuellen Maschinen Kapazität anzeigen Sie [Größen für virtuelle Linux-Computer](virtual-machines-linux-sizes.md)

Azure erstellt eine Betriebssystem-CD, wenn Sie einen virtuellen Computer aus einem Bild erstellen. Wenn Sie ein Bild, der Datenträger enthält verwenden, erstellt Azure auch der Datenträger, den virtuellen Computer erstellt. Andernfalls fügen Sie Datenträger nach dem Erstellen des virtuellen Computers.

Sie können Datenträger einer virtuellen Maschine jederzeit durch **Anfügen** der Festplatte zum virtuellen Computer hinzufügen. Eine virtuelle Festplatte können, die hochgeladen oder kopiert das Speicherkonto oder eine Azure erstellt haben. Datenträger anfügen ordnet die VHD-Datei aus Ihrem Speicherkonto virtuellen Computer eine Lease auf der virtuellen Festplatte platzieren, damit aus dem Speicher gelöscht werden kann zwar immer noch verbunden ist.

## <a name="about-vhds"></a>Zu virtuellen Festplatten

Die VHDs in Azure verwendet werden VHD-Dateien als Seitenblobs in einem standard oder Premium Speicherkonto in Azure. Einzelheiten der Seitenblobs finden Sie unter [Understanding Block Blobs und Seitenblobs](https://msdn.microsoft.com/library/ee691964.aspx). Details zu Premium-Speicher finden Sie unter [Storage Premium: Hochleistungsspeicher für Azure virtuellen Arbeitslasten](../storage/storage-premium-storage.md).

Azure unterstützt Festplatte VHD-Format. Feste Format legt den logischen Datenträger linear innerhalb der Datei Datenträger Versatz X BLOB-Offset X gespeichert ist. Eine Fußzeile am Ende des BLOBs beschreibt die Eigenschaften der virtuellen Festplatte. Feste Format oft verschwendet Platz, da die meisten Festplatten große ungenutzte Bereiche enthalten. Azure speichert jedoch VHD-Dateien mit geringer Datendichte Format erhalten die Vorteile der festen und dynamischen Festplatten gleichzeitig. Weitere Informationen finden Sie unter [Erste Schritte mit virtuellen Festplatten](https://technet.microsoft.com/library/dd979539.aspx).

Alle VHD-Dateien in Azure, die Sie als Quelle verwenden, um Datenträger oder Abbilder erstellen möchten, sind schreibgeschützt. Wenn Sie eine Festplatte oder ein Bild erstellen, wird Azure VHD-Dateien. Diese Kopien können Lese- oder Lese-/ Schreib-, je nachdem, wie Sie die virtuelle Festplatte sein.

Wenn Sie einen virtuellen Computer aus einem Bild erstellen, erstellt Azure Datenträger für den virtuellen Computer, die eine Kopie der VHD-Datei. Zum Schutz vor dem versehentlichen Löschen platziert Azure eine Lease Quelle VHD-Datei, mit der ein Bild, eine Betriebssystem-CD oder eine Daten-CD erstellen.

Vor dem Löschen einer VHD-Datei müssen Sie die Lease löschen das Bild zu entfernen. VHD-Datei löschen, die von einem virtuellen Computer als Betriebssystem-CD verwendet wird, können Sie den virtuellen Computer, die Betriebssystem-CD löschen und die VHD-Datei gleichzeitig der virtuelle Computer wird gelöscht und alle verbundenen Laufwerke. Jedoch eine VHD-Datei eine Quelle für einen Datenträger löschen erfordert mehrere Schritte nacheinander eine Reihe – trennen Sie den Datenträger vom virtuellen Computer löschen, und löschen Sie die VHD-Datei.

>[AZURE.WARNING] Wenn Sie eine VHD-Datei aus dem Speicher löschen oder das Speicherkonto löschen, kann nicht Microsoft Daten für Sie wiederherzustellen.


## <a name="troubleshooting"></a>Problembehandlung
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nächste Schritte

-  [Anfügen eines Datenträgers](virtual-machines-linux-add-disk.md) zusätzlichen Speicher für die VM hinzugefügt.
-  [Konfigurieren Software RAID-](virtual-machines-linux-configure-raid.md) Redundanz.
-  [Erfassen einer virtuellen Linux-Maschine](virtual-machines-linux-classic-capture-image.md) so schnell zusätzliche VMs bereitstellen können.


