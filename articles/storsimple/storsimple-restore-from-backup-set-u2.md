<properties 
   pageTitle="StorSimple-Volumes von der Sicherung wiederherstellen | Microsoft Azure"
   description="Erläutert, wie die StorSimple Manager-Dienst Sicherungskatalog Seite StorSimple-Volume aus einem Sicherungssatz wiederherstellen."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/26/2016"
   ms.author="v-sharos" />

# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Ton aus einem Sicherungssatz (Update 2) StorSimple

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Übersicht

Die **Sicherungskatalog** Seite zeigt alle Sicherungssätze, die bei manuellen oder automatisierten Backups erstellt werden. Sie können mit Hilfe dieser Seite alle Backups für eine Sicherungsrichtlinie oder ein Volume auswählen oder Löschen von Backups oder mithilfe eine Sicherung wiederherstellen oder Klonen eines Volumes.

 ![Backup Katalogseite](./media/storsimple-restore-from-backup-set-u2/restore.png)

In diesem Lernprogramm erläutert, wie die **Sicherungskatalog** Seite Geräts aus einem Sicherungssatz wiederherstellen.

Sie können ein Volume von einem lokalen oder Cloud-Sicherung wiederherstellen. In beiden Fällen bringt die Wiederherstellung den Datenträger online sofort Daten im Hintergrund heruntergeladen werden. 

Bevor Sie einen Wiederherstellungsvorgang einleiten, sollten Sie Folgendes beachten:

- **Das Volume offline nehmen müssen** – das Volume offline nehmen initiieren auf dem Host oder das Gerät vor dem der Wiederherstellung Obwohl die Wiederherstellung auf dem Gerät automatisch den Datenträger online bringt, müssen Sie auf dem Host Gerät online schalten. Sie bringen den Datenträger online auf dem Host als das Volume auf dem Gerät online ist. (Sie müssen nicht warten, bis die Wiederherstellung abgeschlossen ist.) Prozeduren wechseln Sie zu [einem Volume offline](storsimple-manage-volumes-u2.md#take-a-volume-offline).

- **Volumetyp nach Wiederherstellung** gelöscht werden wiederhergestellt anhand des Snapshots; Volumes lokal fixiert wurden als lokale Volumes wiederhergestellt und Volumes gestuft wurden als mehrstufige Volumes wiederhergestellt.

    Vorhandenen Volumes überschreibt der aktuellen Verwendungstyp des Volumes den Typ, der in dem Snapshot gespeichert. Wenn Sie ein Volume von einem Snapshot wiederherstellen, die bei der Datenträgertyp gestuft wurde und Datenträgertyp fixiert ist jetzt lokal (durch ein Konvertierungsvorgang ausgeführt wurde), wird das Volume ebenfalls als lokales Volume wiederhergestellt werden. Ein vorhandenes lokales Volume erweitert und restauriert von einer älteren Snapshot ausgeführt, wenn das Volume kleiner ist, wird entsprechend das wiederhergestellte Volume aktuelle erweiterte Größe beibehalten.

    Kann nicht konvertieren ein Volumes tiered-Volume auf ein lokales Volume oder ein lokales Volume tiered Datenträger während das Volume wiederhergestellt wird. Warten Sie, bis die Wiederherstellung abgeschlossen und Sie können den Datenträger in einen anderen Typ konvertieren. Informationen zum Konvertieren eines Volumes gehen Sie [Volume ändern](storsimple-manage-volumes-u2.md#change-the-volume-type). 

- **Die Volume-Größe in das wiederhergestellte Volume angezeigt** – Dies ist ein wichtiger Aspekt Wiederherstellen von lokalen Volumes gelöscht wurde (weil lokale Volumes vollständig bereitgestellt werden). Stellen Sie sicher, dass ausreichend Platz haben, bevor Sie versuchen, ein lokales Volume wiederherstellen, das zuvor gelöscht wurde. 

- **Sie erweitern können ein Volume während es wiederhergestellt wird** , warten Sie, bis die Wiederherstellung abgeschlossen ist, bevor Sie versuchen, das Volume zu erweitern. Informationen zum Erweitern eines Datenträgers finden Sie [Ändern eines Volumes](storsimple-manage-volumes-u2.md#modify-a-volume).

- **Führen Sie eine Sicherung beim Wiederherstellen von eines lokalen Volumes** -Verfahren finden Sie unter [Verwendung der StorSimple Manager-Dienst zum Verwalten von backup-Policies](storsimple-manage-backup-policies.md).

- **Abbrechen eine Wiederherstellung** – stornieren der Wiederherstellungsauftrag und das Volume wird auf den Zustand zurückgesetzt, der vor den Wiederherstellungsvorgang initiiert. Verfahren finden Sie [einen Druckauftrag abzubrechen](storsimple-manage-jobs-u2.md#cancel-a-job).

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

**Katalog** -Seite können Sie Ihr StorSimple-Volume von einer bestimmten Sicherung wiederherstellen. Beachten Sie jedoch, ein Volume wiederherstellen wird das Volume den Status, den Zeitpunkt der Sicherung zurückgesetzt. Alle Daten, die hinzugefügt wurde, nachdem der Sicherungsvorgang verloren.

> [AZURE.WARNING] Wiederherstellung von einer Sicherung ersetzt die vorhandenen Volumes von der Sicherung. Dadurch kann den Verlust von Daten, die nach der Sicherung erstellt wurde.

### <a name="to-restore-your-volume"></a>Das Volume wiederherstellen

1. Klicken Sie auf der Seite StorSimple Manager auf **Backup** -Katalog.

    ![Backup-Katalog](./media/storsimple-restore-from-backup-set-u2/restore.png)

2. Wählen Sie einen Sicherungssatz wie folgt:
  1. Wählen Sie das entsprechende Gerät.
  2. Wählen Sie in der Dropdown Liste Volume oder Backup-Richtlinie für die Sicherung, die Sie auswählen möchten.
  3. Geben Sie den Zeitraum.
  4. Klicken Sie auf das Häkchensymbol ![Symbol überprüfen](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) für diese Abfrage.
 
    Backups mit dem ausgewählten Volume oder Sicherungsrichtlinie erscheint in der Liste der Sicherungssätze.

3. Erweitern Sie den Sicherungssatz mit den zugeordneten Volumes anzeigen. Diese Volumes müssen auf dem Host und dem Gerät offline ausgeführt werden, bevor Sie sie wiederherstellen können. Zugriff auf die Volumes auf der Seite **Volumecontainer** und gehen Sie dann in [ein Volumen offline](storsimple-manage-volumes-u2.md#take-a-volume-offline) zu offline.

    > [AZURE.IMPORTANT] Stellen Sie sicher, dass Sie sich die Volumes auf dem Host offline, bevor Sie die Volumes offline auf das Gerät. Wenn Sie nicht die Volumes offline auf dem Host, kann es zu einer datenbeschädigung führen.

4. Navigieren Sie zur Registerkarte **Katalog** und wählen Sie einen backup-Satz.

5. Klicken Sie am unteren Rand der Seite **Wiederherstellen** .

6. Sie werden zur Bestätigung aufgefordert. Überprüfen Sie die Informationen zum Wiederherstellen, und aktivieren Sie das Kontrollkästchen Auftragsbestätigung.

    ![Bestätigungsseite](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)

7. Klicken Sie auf Check ![Symbol überprüfen](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Einen Wiederherstellungsauftrag wird gestartet, den **Aufträge** Seite anzeigen können. 

8. Nachdem die Wiederherstellung abgeschlossen ist, können Sie überprüfen, dass der Inhalt der Volumes von Volumes aus der Sicherung ersetzt werden.

![Video verfügbar](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video verfügbar**

Ein Video, das veranschaulicht, wie Sie den Klon und Funktionen in StorSimple gelöschte Dateien wiederherzustellen, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Misslingt die Wiederherstellung

Sie erhalten eine Warnung, wenn Wiederherstellung aus irgendeinem Grund fehlschlägt. In diesem Fall aktualisiert die Sicherung überprüfen, ob die Sicherung noch gültig ist. Gilt die Sicherung und Wiederherstellung aus der Cloud, dann Konnektivitätsprobleme das Problem möglicherweise verursacht werden. 

Führen Sie die Wiederherstellung den Datenträger offline auf dem Host und Wiederherstellung erneut. Beachten Sie, dass Änderungen an der Datenmengen, die während der Wiederherstellung ausgeführt wurden verloren.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [Volumes verwalten StorSimple](storsimple-manage-volumes-u2.md).

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
