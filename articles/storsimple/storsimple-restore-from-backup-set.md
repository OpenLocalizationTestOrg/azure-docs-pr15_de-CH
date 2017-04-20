<properties 
   pageTitle="StorSimple-Volumes von der Sicherung wiederherstellen | Microsoft Azure"
   description="Erläutert, wie die StorSimple Manager-Dienst Sicherungskatalog Seite StorSimple-Volume aus einem Sicherungssatz wiederherstellen."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>StorSimple-Volume aus einem Sicherungssatz wiederherstellen

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Übersicht

Die **Sicherungskatalog** Seite zeigt alle Sicherungssätze, die bei manuellen oder automatisierten Backups erstellt werden. Sie können mit Hilfe dieser Seite alle Backups für eine Sicherungsrichtlinie oder ein Volume auswählen oder Löschen von Backups oder mithilfe eine Sicherung wiederherstellen oder Klonen eines Volumes.

 ![Backup Katalogseite](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

In diesem Lernprogramm erläutert, wie die **Sicherungskatalog** Seite ein Volume auf dem Gerät aus einem Sicherungssatz wiederherstellen.

## <a name="how-to-use-the-backup-catalog"></a>Wie Sie den Sicherungskatalog 

**Katalog** -Seite enthält eine Abfrage, mit der die Sicherung eingrenzen Auswahl festgelegt. Filtern Sie die Sicherungssätze, die abgerufen werden anhand der folgenden Parameter:

- **Gerät** das Gerät, an dem der Sicherungssatz erstellt wurde.
- **Backup-Richtlinie** oder **Volume** -Sicherungsrichtlinie oder Lautstärke mit diesem Sicherungssatz.
- **Von** und **zu** den Datums- und Zeitbereich bei der Erstellung des Sicherungssatzes.

Die gefilterten Sicherungssätze werden dann tabellarisch basierend auf den folgenden Attributen:

- **Name** – der Name der Sicherungsrichtlinie oder Sicherungssatz zugeordnet.
- **Größe** -die tatsächliche Größe des Sicherungssatzes.
- **Erstellt am** – Datum und Uhrzeit, wann die Sicherung erstellt wurden. 
- **Typ** – Backup-Sets können lokale Snapshots oder cloud-Snapshots. Lokaler Snapshot ist eine Sicherung der alle Datenträger Daten lokal auf dem Gerät ein Cloud-Snapshot Backup Volumendaten in der Cloud bezeichnet. Lokale Snapshots bieten schnelleren Zugriff während Cloud Snapshots Daten Robustheit ausgewählt werden.
- **Initiiert von** – die Backups automatisch nach einem Zeitplan oder manuell durch einen Benutzer initiiert werden können. (Eine Sicherungsrichtlinie können Sicherungen. Auch können die Option **Sicherung** Sie eine interaktive Sicherung.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>StorSimple-Volume von einer Sicherungskopie wiederherstellen

**Katalog** -Seite können Sie Ihr StorSimple-Volume von einer bestimmten Sicherung wiederherstellen. 

> [AZURE.WARNING] Wiederherstellung von einer Sicherung ersetzt die vorhandenen Volumes von der Sicherung. Dadurch kann den Verlust von Daten, die nach der Sicherung erstellt wurde.

Bevor Sie eine Wiederherstellung auf einem Volume initiieren, sicherzustellen Sie, dass der Datenträger offline ist. Sie müssen das Volume offline auf dem Host zu beginnen und dann das Gerät. Gehen Sie in [einen Datenträger offline nehmen](storsimple-manage-volumes.md#take-a-volume-offline). Führen Sie die folgenden Schritte aus, um ein Volume aus einem Sicherungssatz wiederherstellen.

### <a name="to-restore-from-a-backup-set"></a>Aus einem Sicherungssatz wiederherstellen

1. Klicken Sie auf der Seite StorSimple Manager auf **Backup** -Katalog.

    ![Backup-Katalog](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. Wählen Sie einen Sicherungssatz wie folgt:
  1. Wählen Sie das entsprechende Gerät.
  2. Wählen Sie in der Dropdown Liste Volume oder Backup-Richtlinie für die Sicherung, die Sie auswählen möchten.
  3. Geben Sie den Zeitraum.
  4. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) für diese Abfrage.
 
    Backups mit dem ausgewählten Volume oder Sicherungsrichtlinie erscheint in der Liste der Sicherungssätze.

3. Erweitern Sie den Sicherungssatz mit den zugeordneten Volumes anzeigen. Diese Volumes müssen auf dem Host und dem Gerät offline ausgeführt werden, bevor Sie sie wiederherstellen können. Gehen Sie in [einen Datenträger offline nehmen](storsimple-manage-volumes.md#take-a-volume-offline).

    >  [AZURE.IMPORTANT] Stellen Sie sicher, dass Sie sich die Volumes auf dem Host offline, bevor Sie die Volumes offline auf das Gerät. Wenn Sie nicht die Volumes offline auf dem Host, kann es zu einer datenbeschädigung führen.

4. Wählen Sie einen backup-Satz. Klicken Sie am unteren Rand der Seite **Wiederherstellen** .

6. Sie werden zur Bestätigung aufgefordert. 

    ![Bestätigungsseite](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. Informationen wiederherstellen und klicken Sie auf Check ![Symbol überprüfen](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Einen Wiederherstellungsauftrag wird gestartet, den **Aufträge** Seite anzeigen können. 

8. Nachdem die Wiederherstellung abgeschlossen ist, können Sie überprüfen, dass der Inhalt der Volumes von Volumes aus der Sicherung ersetzt werden.

![Video verfügbar](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video verfügbar**

Ein Video, das veranschaulicht, wie Sie den Klon und Funktionen in StorSimple gelöschte Dateien wiederherzustellen, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [Volumes verwalten StorSimple](storsimple-manage-volumes.md).

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
