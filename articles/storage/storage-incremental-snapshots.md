<properties
    pageTitle="Inkrementelle Snapshots für Backup und Recovery von Azure virtuellen Maschinen verwenden | Microsoft Azure"
    description="Erstellen Sie eine benutzerdefinierte Lösung für Backup und Recovery Ihrer Azure virtuellen Festplatten mit inkrementellen Snapshots."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>


# <a name="back-up-azure-virtual-machine-disks-with-incremental-snapshots"></a>Sichern von Azure virtuellen Datenträgern mit inkrementellen snapshots

## <a name="overview"></a>Übersicht

Azure-Speicher ermöglicht die Blobs Schnappschüsse. -Momentaufnahmen erfassen den Zustand Blob zu diesem Zeitpunkt. In diesem Artikel beschreiben wir ein Szenario wie Backups von virtuellen Festplatten mit Snapshots verwalten können. Azure Backup und Recovery-Service nicht verwenden und eine benutzerdefinierte Sicherungsstrategie für die virtuellen Datenträger erstellen möchten, können Sie diese Methode.

Azure virtuellen Festplatten werden als Seitenblobs in Azure-Speicher gespeichert. Da wir Sicherungsstrategie für die virtuellen Laufwerke in diesem Artikel reden werden wir Snapshots im Kontext der Seitenblobs beziehen. Weitere Informationen zu Snapshots finden Sie unter Erstellen [einer Momentaufnahme eines Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Was ist ein Snapshot?

Ein BLOB-Snapshot ist eine schreibgeschützte Version eines Blob, das zu einem Zeitpunkt erfasst wird. Nachdem ein Snapshot erstellt, kann es gelesen, kopiert, oder gelöscht, aber nicht geändert. Snapshots bieten eine Möglichkeit, um ein Blob zu einem Zeitpunkt angezeigt wird. Bis Version 2015-04-05 REST mussten Sie vollständige Snapshots kopieren. Mit Version 2015-07-08 und Sie können auch inkrementelle Snapshots.

## <a name="full-snapshot-copy"></a>Vollständige Snapshot-Kopie

Snapshots können als Blob Backups Basis-BLOB zu einem anderen Speicher-Konto kopiert. Sie können auch einen Snapshot über die Basis-Blob wie Blob in einer früheren Version wiederherstellen. Wenn ein Snapshot von einem Storage-Konto in eine andere kopiert wird, nimmt es Bereich Basisseite Blob. Daher kopieren ganze Snapshots von einem Speicherkonto zu einem anderen langsam und außerdem viel Platz im Zielkonto Speicher belegen.

>[AZURE.NOTE] Wenn Sie base-Blob an einen anderen Zielort kopieren, werden die Snapshots des Blob nicht mit kopiert. Wenn einen Basis-Blob mit einer Kopie überschreiben Snapshots Basis-Blob zugeordnet sind nicht betroffen und Basis-Blob Namen intakt bleiben.

### <a name="back-up-disks-using-snapshots"></a>Sichern von Datenträgern mit snapshots

Als eine backup-Strategie für Ihre virtuellen Laufwerke können regelmäßige Snapshots des Blob Datenträger oder Seite und in einem anderen Speicher-Konto mithilfe von Tools wie [Copy Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) -Vorgang oder [AzCopy](storage-use-azcopy.md)kopieren. Sie können einen Snapshot auf einer Seite Ziel-Blob mit einem anderen Namen. Resultierende Ziel Seitenblob ist ein Seitenblob beschreibbaren und keine Momentaufnahme. In diesem Artikel wird Schritte Backups von virtuellen Festplatten mit Snapshots beschrieben.

### <a name="restore-disks-using-snapshots"></a>Wiederherstellung mit snapshots

Wenn Festplatte Wiederherstellen einer früheren stabile Version eines backup-Snapshots erfasst wird, können Sie einen Snapshot über Basisseite Blob kopieren. Nachdem der Snapshot auf Basis Seite heraufgestuft wird BLOB-Snapshot bleibt jedoch der Quelle mit einer Kopie, die sowohl Lese- und Schreibvorgänge kann überschrieben. In diesem Artikel beschreiben wir Schritte zum Wiederherstellen einer früheren Version des Datenträgers aus der Momentaufnahme.

### <a name="implementing-full-snapshot-copy"></a>Implementieren der vollständigen Snapshot-Kopie

Implementiert eine vollständige Momentaufnahme folgt,

-   Erstens Momentaufnahme Basis-BLOB mit der [Snapshot-Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) .
-   Kopieren Sie den Snapshot auf ein Ziel Speicherkonto mit [Copy Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx).
-   Wiederholen Sie diesen Vorgang Sicherungskopien der Basis-Blob.

## <a name="incremental-snapshot-copy"></a>Inkrementelle Snapshot-Kopie

Das neue Feature in [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) API bietet ein viel besseres Snapshots der Seitenblobs oder Datenträger sichern. Die API gibt Änderungen zwischen Basis-Blob und Snapshots. Dies reduziert den erforderlichen Speicherplatz auf den Konten. Die API unterstützt Seitenblobs auf Premium-Speicher sowie Standard. Mit dieser API können Sie nun schnellere und effiziente backup-Lösungen für Azure VMs erstellen. Mit der anderen Version 2015-07-08 und höher werden.

Inkrementelle Snapshot Copy können Sie von einem Speicherkonto Differenz kopieren,

-   Basis-Blob und die Snapshot-oder
-   Keine zwei Snapshots Basis-BLOB

Sofern Folgendes erfüllt sind,

- Das Blob wurde Jan-1-2016 oder höher erstellt.
- Das Blob wurde nicht mit [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) oder [Copy Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) zwischen zwei Snapshots überschrieben.


**Hinweis**: Diese Funktion steht für Premium und Standard Azure Seitenblobs.

Wenn Sie eine benutzerdefinierte backup-Strategie, die Snapshots verwendet Snapshots von einem Storage-Konto in eine andere kopieren kann sehr langsam sein und viel Speicherplatz. Kopieren des gesamten Snapshots backup-Speicher-Konto können Sie den Unterschied zwischen aufeinander folgenden Snapshots Seite Sicherung Blob schreiben. Auf diese Weise ist der kopieren und Speicherplatz für Backups erheblich verkürzt.

### <a name="implementing-incremental-snapshot-copy"></a>Implementieren von inkrementellen Snapshot-Kopie

Implementieren inkrementeller Snapshot-Kopie folgt,

-   Eine Momentaufnahme Basis-BLOB mit [Snapshot-Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx).
-   Kopieren des Snapshots zum Zielkonto für die backup-Speicher mit [Copy Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). Das Blob Seite Sicherung wird. Eine Momentaufnahme dieser Seite Sicherung Blob und Konten speichern.
-   Eine andere Momentaufnahme Basis-BLOB mit Snapshot-Blob.
-   Erhalten Sie den Unterschied zwischen Snapshots und Basis BLOB mit [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx). Der neue Parameter **Prevsnapshot** DateTime-Wert des Snapshots Festlegen der Unterschied zu verwenden. Wenn dieser Parameter vorhanden ist, enthalten die übrigen Antwort nur die Seiten zwischen Ziel-Snapshot und vorherigen einschließlich löschen Seiten geändert wurden.
-   Mit [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) können diese Änderungen BLOB backup Seite anwenden.
-   Schließlich eine Momentaufnahme des BLOBs Seite Sicherung und backup-Speicher-Konto speichern.

Im nächsten Abschnitt wird ausführlich erläutert, wie Sie Backups mit inkrementellen Snapshot-Kopie verwalten

## <a name="scenario"></a>Szenario

In diesem Abschnitt beschreiben wir ein Szenario, bei dem eine benutzerdefinierte Sicherungsstrategie für Datenträger mit Snapshots virtueller Computer.

Sollten Sie eine DS-Serie Azure VM eine Prämie Storage P30 Festplatte angeschlossen. P30 Datenträger namens *Mypremiumdisk* wird in ein Premium-Speicherkonto namens *Mypremiumaccount*gespeichert. Ein standard Speicherkonto namens *Mybackupstdaccount* wird zum Speichern der Sicherung der *Mypremiumdisk*. Wir würden gerne eine Momentaufnahme der *Mypremiumdisk* alle 12 Stunden halten.

Weitere Informationen zum Erstellen von Speicherkonto und Festplatten, finden Sie [über Azure-Speicherkonten](storage-create-storage-account.md).

Zum Sichern von Azure VMs finden Sie unter [Planen der Azure-VM](../backup/backup-azure-vms-introduction.md)-Backups.

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Schritte zur Sicherung der Festplatte inkrementellen Snapshots verwalten

Die unten beschriebenen Schritte Snapshots des *Mypremiumdisk* und Backups in *Mybackupstdaccount*verwalten. Die Sicherung werden eine Standardseite Blob *Mybackupstdpageblob*aufgerufen. Das Seitenblob backup wider immer im gleichen Zustand wie der letzte Snapshot *Mypremiumdisk*.

1.  Erstellen Sie zunächst des Seite Sicherung BLOBs für Festplatte Storage Premium. Hierzu eine Momentaufnahme der *Mypremiumdisk* *mypremiumdisk_ss1*bezeichnet
2.  Soll dieser Snapshot Mybackupstdaccount als Seitenblob *Mybackupstdpageblob*aufgerufen.
3.  Eine Momentaufnahme der *Mybackupstdpageblob* namens *mybackupstdpageblob_ss1*, mit [Snapshot-Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) und in *Mybackupstdaccount*speichern.
4.  Während des Sicherungsfensters erstellen Sie einen weiteren Snapshot *Mypremiumdisk*sagen Sie *mypremiumdisk_ss2*und in *Mypremiumaccount*speichern.
5.  Inkrementelle Änderungen zwischen zwei Snapshots, *mypremiumdisk_ss2* und *mypremiumdisk_ss1*, [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) *mypremiumdisk_ss2* mit **Prevsnapshot** -Parameter mit dem Zeitstempel des *mypremiumdisk_ss1*festgelegt. Schreiben Sie in Seite Sicherung BLOB- *Mybackupstdpageblob* in *Mybackupstdaccount*ändert. Bei gelöschte Bereichen in der ändert, müssen sie aus dem Blob Seite Sicherung gelöscht werden. Verwenden Sie [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) ändert die Seite Sicherung Blob schreiben.
6.  Eine Momentaufnahme der Seite Sicherung Blob *Mybackupstdpageblob*, namens *mybackupstdpageblob_ss2*. Löschen Sie der vorherige Snapshot *mypremiumdisk_ss1* von Premium-Speicherkonto
7.  Wiederholen Sie die Schritte 4 bis 6 jeder backup-Fenster. Auf diese Weise können Sie Sicherungskopien der *Mypremiumdisk* in einem standardmäßigen Speicher-Konto verwalten.

![Sichern Sie Datenträger mit inkrementellen snapshots](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Schritte zum Wiederherstellen von snapshots

Die unten beschriebenen Schritte werden Premium Festplatte *Mypremiumdisk* eine frühere Momentaufnahme der Sicherungsbänder Konto *Mybackupstdaccount*wiederhergestellt.

1.  Identifizierung der Premium-Festplatte wiederherstellen möchten. Wir sagen, dass Snapshot *mybackupstdpageblob_ss2*der backup-Speicher-Konto *Mybackupstdaccount*gespeichert wird.
2.  Fördern Sie Snapshot *mybackupstdpageblob_ss2* Mybackupstdaccount als neue backup Basisseite Blob- *Mybackupstdpageblobrestored*.
3.  Eine Momentaufnahme dieser Seite wiederhergestellte Sicherung BLOB *mybackupstdpageblobrestored_ss1*aufgerufen.
4.  Soll wiederhergestellten Seite BLOB- *Mybackupstdpageblobrestored* von *Mybackupstdaccount* *Mypremiumaccount* als neue Premium Datenträger *Mypremiumdiskrestored*.
5.  Eine Momentaufnahme des *Mypremiumdiskrestored*, namens *mypremiumdiskrestored_ss1* für zukünftige inkrementellen Sicherungen.
6.  Der DS punktserie VM den Datenträger *Mypremiumdiskrestored* und alten *Mypremiumdisk* aus der VM trennen.
7.  Beginnen Sie den im vorherigen Abschnitt für die wiederhergestellten Datenträger *Mypremiumdiskrestored*, mit *Mybackupstdpageblobrestored* als Seitenblob backup beschriebenen Vorgang.

![Datenträger aus Snapshots wiederhergestellt](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Snapshots eines Blob erstellen und Planen der VM backup-Infrastruktur über die folgenden links

- [Erstellen einer Momentaufnahme eines Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [Planen Sie die VM-Backup-Infrastruktur](../backup/backup-azure-vms-introduction.md)
