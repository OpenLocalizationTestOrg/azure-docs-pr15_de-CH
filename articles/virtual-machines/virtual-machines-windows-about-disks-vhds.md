<properties
    pageTitle="Laufwerke und Festplatten für Windows VMs | Microsoft Azure"
    description="Lernen Sie die Grundlagen von Datenträgern und Festplatten für Windows virtuelle Computer in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Festplatten für Azure virtuelle Maschinen zu Datenträgern

Wie andere Computer verwenden Sie virtuelle Computer in Azure Datenträger als Ort ein Betriebssystem, Anwendung und Daten zu speichern. Alle virtuellen Computer Azure sind mindestens zwei Festplatten – einem Windows Betriebssystem und einen temporären Datenträger. Die Betriebssystem-CD aus einem Abbild erstellt und Betriebssystem-Datenträger und Bild sind virtuelle Festplatten (VHDs) in Azure Storage-Konto gespeichert. Virtuelle Computer können auch ein oder mehrere Datenträger, die auch als virtuelle Festplatten gespeichert sind. Dieser Artikel steht auch für [virtuelle Linux-Computer](virtual-machines-linux-about-disks-vhds.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



## <a name="operating-system-disk"></a>Betriebssystem-CD

Jeder virtuellen Maschine verfügt einen angefügten Betriebssystem-Datenträger. Als eine SATA-Festplatte und als Laufwerk C: standardmäßig. Diese Diskette enthält maximal 1023 Gigabyte (GB). 

##<a name="temporary-disk"></a>Temporärer Speicherplatz

Die temporäre Diskette wird automatisch für Sie erstellt. Die temporäre Diskette wird als Laufwerk D: und verwendet zum Speichern von pagefile.sys bezeichnet. 

Die Größe des temporären Datenträgers hängt von der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größe für Windows virtuelle Computer](virtual-machines-windows-sizes.md).

>[AZURE.WARNING] Speichern Sie nicht Daten auf dem temporären Datenträger. Temporären Speicher für Programme und Prozesse bereitstellt, und nur Daten wie Seite oder Swap-Dateien speichern soll. Um diesen Datenträger auf einen anderen Laufwerkbuchstaben zuordnen, finden Sie unter [Ändern der Laufwerkbuchstabe des temporären Windows-Datenträger](virtual-machines-windows-classic-change-drive-letter.md).

Weitere Informationen zur Verwendung Azure temporären Speicherplatz finden Sie unter [Understanding temporäre Laufwerk auf Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Datenträger

Ein Datenträger ist eine virtuelle Festplatte an einem virtuellen Computer Daten oder andere Daten zu speichern. Datenträger als SCSI-Laufwerke registriert und mit einer Auswahl von Buchstaben gekennzeichnet.  Jeder Datenträger verfügt über maximal 1023 GB. Die Größe des virtuellen Computers bestimmt wie viele Datenträger und Speichertyp Anfügen verwenden Datenträger hosten können.

>[AZURE.NOTE] Weitere Informationen zu virtuellen Maschinen Kapazität finden Sie [Größen für Windows virtuelle Computer](virtual-machines-windows-sizes.md).

Azure erstellt eine Betriebssystem-CD, wenn Sie einen virtuellen Computer aus einem Bild erstellen. Wenn Sie ein Bild, der Datenträger enthält verwenden, erstellt Azure auch der Datenträger, den virtuellen Computer erstellt. Andernfalls fügen Sie Datenträger nach dem Erstellen des virtuellen Computers.

Sie können Datenträger einer virtuellen Maschine jederzeit durch **Anfügen** der Festplatte zum virtuellen Computer hinzufügen. Eine virtuelle Festplatte können, die hochgeladen oder kopiert das Speicherkonto oder eine Azure erstellt haben. Datenträger anfügen ordnet die VHD-Datei die VM eine Lease auf der virtuellen Festplatte platzieren, damit aus dem Speicher gelöscht werden kann zwar immer noch verbunden ist.

## <a name="about-vhds"></a>Zu virtuellen Festplatten

Die VHDs in Azure verwendet werden VHD-Dateien als Seitenblobs in einem standard oder Premium Speicherkonto in Azure. Informationen zur Seitenblobs finden Sie unter [Understanding Block Blobs und Seitenblobs](https://msdn.microsoft.com/library/ee691964.aspx). Weitere Informationen zum Premium-Speicher finden Sie unter [Storage Premium: Hochleistungsspeicher für Azure virtuellen Arbeitslasten](../storage/storage-premium-storage.md).

Azure unterstützt Festplatte VHD-Format. Das festen Format legt den logischen Datenträger linear innerhalb der Datei Datenträger Versatz X BLOB-Offset X gespeichert. Eine Fußzeile am Ende des BLOBs beschreibt die Eigenschaften der virtuellen Festplatte. Häufig Speicherplatz Abfälle-Format, da die meisten Festplatten große ungenutzte Bereiche enthalten. Azure speichert jedoch VHD-Dateien mit geringer Datendichte Format erhalten die Vorteile der festen und dynamischen Festplatten gleichzeitig. Weitere Informationen finden Sie unter [Erste Schritte mit virtuellen Festplatten](https://technet.microsoft.com/library/dd979539.aspx).

Alle VHD-Dateien in Azure, die Sie als Quelle verwenden, um Datenträger oder Abbilder erstellen möchten, sind schreibgeschützt. Wenn Sie eine Festplatte oder ein Bild erstellen, wird Azure VHD-Dateien. Diese Kopien können Lese- oder Lese-/ Schreib-, je nachdem, wie Sie die virtuelle Festplatte sein.

Wenn Sie einen virtuellen Computer aus einem Bild erstellen, erstellt Azure Datenträger für den virtuellen Computer, die eine Kopie der VHD-Datei. Zum Schutz vor dem versehentlichen Löschen platziert Azure eine Lease Quelle VHD-Datei, mit der ein Bild, eine Betriebssystem-CD oder eine Daten-CD erstellen.

Vor dem Löschen einer VHD-Datei müssen Sie die Lease löschen das Bild zu entfernen. Sie können den virtuellen Computer, die Betriebssystem-CD und die VHD-Datei gleichzeitig der virtuelle Computer wird gelöscht und alle verbundenen Laufwerke Eine VHD-Datei eine Quelle für einen Datenträger löschen erfordert jedoch mehrere Schritte in einer Reihenfolge festlegen. Zuerst trennen Sie den Datenträger vom virtuellen Computer dann löschen, und löschen Sie die VHD-Datei.

>[AZURE.WARNING] Wenn Sie eine VHD-Datei aus dem Speicher löschen oder das Speicherkonto löschen, kann nicht Microsoft Daten für Sie wiederherzustellen.



## <a name="next-steps"></a>Nächste Schritte
-  [Anfügen eines Datenträgers](virtual-machines-windows-attach-disk-portal.md) zusätzlichen Speicher für die VM hinzugefügt.
-  Beim Erstellen einer neuen VM verwendet [ein Bild VM Windows Azure hochladen](virtual-machines-windows-upload-image.md) .
-  [Ändern der Laufwerkbuchstabe des temporären Datenträgers Windows](virtual-machines-windows-classic-change-drive-letter.md) damit die Anwendung das Laufwerk D: für Daten verwenden kann.
