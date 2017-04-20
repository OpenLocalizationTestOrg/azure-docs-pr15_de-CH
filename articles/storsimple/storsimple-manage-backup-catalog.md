<properties 
   pageTitle="Verwalten den Sicherungskatalog StorSimple | Microsoft Azure"
   description="Erläutert, wie die StorSimple Manager-Dienst Sicherungskatalog Seite aufgeführt, wählen und Sicherungssätze für ein Volume löschen."
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
   ms.date="04/28/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Verwenden des StorSimple Manager-Dienstes den Sicherungskatalog verwalten

## <a name="overview"></a>Übersicht

StorSimple Manager Service **Sicherungskatalog** Seite zeigt alle Sicherungssätze, die bei manuellen oder geplanten Backups erstellt werden. Sie können mit Hilfe dieser Seite alle Backups für eine Sicherungsrichtlinie oder ein Volume auswählen oder Löschen von Backups oder mithilfe eine Sicherung wiederherstellen oder Klonen eines Volumes.

In diesem Lernprogramm wird die Liste auswählen und Löschen eines Sicherungssatzes erläutert. Das Gerät aus einer Sicherung wiederherstellen finden Sie unter [das Gerät aus einem Sicherungssatz](storsimple-restore-from-backup-set.md)wiederherstellen. Weitere Informationen zum Klonen eines Volumes zu wechseln [Klon eines StorSimple-Volumes](storsimple-clone-volume.md)

![Backup-Katalog](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

**Katalog** Seite enthält eine Abfrage die Sicherung einschränken Auswahl festgelegt. Sie können die Sicherungssätze filtern, die auf Grundlage der folgenden Parameter abgerufen werden:

- **Gerät** das Gerät, an dem der Sicherungssatz erstellt wurde.

- **Backup oder Volume** Sicherungsrichtlinie oder Volume dieser Sicherungssatz zugeordnet.

- **Aus** den Datums- und Zeitbereich bei der Erstellung des Sicherungssatzes.

Die gefilterten Sicherungssätze werden dann tabellarisch basierend auf den folgenden Attributen:

- **Name** – der Name der Sicherungsrichtlinie oder Sicherungssatz zugeordnet.

- **Größe** -die tatsächliche Größe des Sicherungssatzes.

- **Erstellt am** Datum und Uhrzeit, wann die Sicherung erstellt wurden. 

- **Typ** – Backup-Sets können lokale Snapshots oder cloud-Snapshots. Lokaler Snapshot ist eine Sicherung der alle Datenträger Daten lokal auf dem Gerät ein Cloud-Snapshot Backup Volumendaten in der Cloud bezeichnet. Lokale Snapshots bieten schnelleren Zugriff während Cloud Snapshots Daten Robustheit ausgewählt werden.

- **Initiiert von** Backups automatisch nach einem Zeitplan oder manuell durch einen Benutzer initiiert werden können. Eine Sicherungsrichtlinie können Sicherungen. Die Option **Sicherung** können Sie auch eine manuelle Sicherung.

## <a name="list-backup-sets-for-a-volume"></a>Liste der Sicherungssätze für ein volume
 
Gehen Sie zum Auflisten aller Backups für ein Volume.

#### <a name="to-list-backup-sets"></a>Zu Liste Sicherungssätze

1. Klicken Sie auf der Seite StorSimple Manager auf **Backup** -Katalog.

2. Filtern Sie die Optionen wie folgt:

    1. Wählen Sie das entsprechende Gerät.

    2. Wählen Sie in der Dropdown-Liste ein Volume anzeigen entspricht der Backups.

    3. Geben Sie den Zeitraum.

    4. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) für diese Abfrage.
 
    Das ausgewählte Volume zugeordnet Backups sollte in der Liste der Sicherungssätze angezeigt werden.

## <a name="select-a-backup-set"></a>Wählen Sie einen backup-Satz

Gehen Sie folgendermaßen vor um einen Sicherungssatz für ein Volume oder eine Sicherungsrichtlinie auszuwählen.

#### <a name="to-select-a-backup-set"></a>Einen backup-Satz auswählen

1. Klicken Sie auf der Seite StorSimple Manager auf **Backup** -Katalog.

2. Filtern Sie die Optionen wie folgt:

    1. Wählen Sie das entsprechende Gerät.

    2. Wählen Sie in der Dropdown Liste Volume oder Backup-Richtlinie für die Sicherung, die Sie auswählen möchten.

    3. Geben Sie den Zeitraum.

    4. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) für diese Abfrage.

    Backups mit dem ausgewählten Volume oder Sicherungsrichtlinie erscheint in der Liste der Sicherungssätze.

3. Markieren Sie und erweitern Sie einen Sicherungssatz. Optionen **Wiederherstellen** und **Löschen** werden am unteren Rand der Seite angezeigt. Diese Aktionen möglich in der Sicherungskopie, die Auswahl.

## <a name="delete-a-backup-set"></a>Löschen eines Sicherungssatzes

Löschen einer Sicherung nicht mehr die Daten behalten möchten. Führen Sie die folgenden Schritte aus, um einen backup-Satz zu löschen.

#### <a name="to-delete-a-backup-set"></a>Löschen ein Sicherungssatzes

1. Klicken Sie auf der Seite StorSimple Manager **Backup-Katalog**.

2. Filtern Sie die Optionen wie folgt:

    1. Wählen Sie das entsprechende Gerät.

    2. Wählen Sie in der Dropdown Liste Volume oder Backup-Richtlinie für die Sicherung, die Sie auswählen möchten.

    3. Geben Sie den Zeitraum.

    4. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) für diese Abfrage.

    Backups mit dem ausgewählten Volume oder Sicherungsrichtlinie erscheint in der Liste der Sicherungssätze.

3. Markieren Sie und erweitern Sie einen Sicherungssatz. Optionen **Wiederherstellen** und **Löschen** werden am unteren Rand der Seite angezeigt. Klicken Sie auf **Löschen**.

4. Sie werden benachrichtigt, wenn der Löschvorgang ist und wenn es erfolgreich abgeschlossen wurde. Nach Abschluss des Löschvorgangs wird aktualisieren Sie die Abfrage auf dieser Seite. Gelöschte Sicherungssatz wird nicht mehr in der Liste der Sicherungssätze angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [den Sicherungskatalog um das Gerät aus einem Sicherungssatz wiederherstellen](storsimple-restore-from-backup-set.md).

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
