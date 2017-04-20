<properties
   pageTitle="Klonen das StorSimple Volume | Microsoft Azure"
   description="Beschreibt verschiedene Clone-Typen und ihre Verwendung und beschreibt, wie Sie einen Sicherungssatz auf einem einzelnen Volume klonen."
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
   ms.date="07/27/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Verwenden des StorSimple Manager-Dienstes Klonen ein Volumes (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Übersicht

StorSimple Manager Service **Sicherungskatalog** Seite zeigt alle Sicherungssätze, die bei manuellen oder automatisierten Backups erstellt werden. Sie können mit Hilfe dieser Seite alle Backups für eine Sicherungsrichtlinie oder ein Volume auswählen oder Löschen von Backups oder mithilfe eine Sicherung wiederherstellen oder Klonen eines Volumes.

![Katalog-Seite](./media/storsimple-clone-volume-u2/backupCatalog.png)  

In diesem Lernprogramm beschreibt die Verwendung einer Sicherungssatzes auf einem einzelnen Volume Klonen festgelegt. Darüber hinaus unterscheiden *transiente* und *permanente* Clones.

>[AZURE.NOTE] 
>
>Ein lokales Volume wird als mehrstufige Volume geklont. Benötigen Sie das geklonte Volume lokal fixiert werden, können Sie den Klon nach der Klonvorgang erfolgreich auf lokalen Volumes konvertieren. Informationen zum Konvertieren eines gestuften Volumes zu lokalen Volumes finden Sie [Volume ändern](storsimple-manage-volumes-u2.md#change-the-volume-type).
>
>Beim Konvertieren eines geklonten Datenträgers aus fehl Ebenen unmittelbar nach dem Klonen (wenn es noch ein vorübergehender Klon) lokal fixiert die Konvertierung mit der folgenden Fehlermeldung:
>
>`Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
>
>Diese Fehlermeldung wird nur, wenn Sie auf einem anderen Gerät klonen. Erfolgreich konvertieren Sie das Volume lokal fixiert zuerst vorübergehenden Klon zu einem dauerhaften Clone konvertieren. Um permanente Klon vorübergehenden Klon konvertieren, eine Momentaufnahme Cloud davon.

## <a name="create-a-clone-of-a-volume"></a>Klonen eines Volumes

Erstellen einen Klon auf dasselbe Gerät, ein anderes Gerät oder sogar einen virtuellen Computer mit einem lokalen oder Cloud-Snapshot.

#### <a name="to-clone-a-volume"></a>Klonen ein Volumes

1. Auf der Seite StorSimple Manager auf **Backup** -Katalog und wählen Sie einen backup-Satz.

2. Erweitern Sie den Sicherungssatz mit den zugeordneten Volumes anzeigen. Klicken Sie auf, und wählen Sie ein Volume aus dem Sicherungssatz.

     ![Klonen eines Volumes](./media/storsimple-clone-volume-u2/CloneVol.png) 

3. Klicken Sie auf **Clone** zunächst das ausgewählte Volume klonen.

4. Im Assistenten Clone-Volume unter **Speicherort und Namen angeben**:

  1. Ein Zielgerät zu identifizieren. Dies ist der Speicherort, in dem der Klon erstellt werden. Sie können dasselbe Gerät auswählen oder ein anderes Gerät. Wählt ein Volume mit anderen Cloud verbundenen (nicht Azure) die Dropdown-Liste für das Zielgerät nur physische Geräte anzeigen. Ein Volume auf einem virtuellen Gerät mit anderen Cloud zugewiesen kann nicht geklont werden.

        >[AZURE.NOTE] Stellen Sie sicher, dass für den Klon erforderliche Kapazität kleiner als die Kapazität des Zielgeräts ist.

  2. Geben Sie einen eindeutigen Datenträgernamen der Klon. Der Name muss zwischen 3 und 127 Zeichen enthalten. 
    
        >[AZURE.NOTE] Feld **Clone-Volume als** werden **Tiered** , selbst wenn Sie ein lokales Volume klonen. Sie können diese Einstellung nicht ändern; Allerdings benötigen Sie das geklonte Volume lokal auch fixiert werden, verwandeln Sie den Klon auf lokalen Volumes nach erfolgreich das Klonen. Informationen zum Konvertieren eines gestuften Volumes zu lokalen Volumes finden Sie [Volume ändern](storsimple-manage-volumes-u2.md#change-the-volume-type).

        ![Clone-Assistent 1](./media/storsimple-clone-volume-u2/clone1.png) 

  3. Klicken Sie auf das Pfeilsymbol ![Pfeil-icon](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) auf der nächsten Seite fortgesetzt.

5. Klicken Sie unter **Angeben von Hosts, die dieses Volume verwenden können**:

  1. Geben Sie einen Steuerelement Zugriffsdatensatz (ACR) für den Klon. Sie können eine neue ACR oder aus der vorhandenen Liste.

        ![Clone-Assistent 2](./media/storsimple-clone-volume-u2/clone2.png) 

  2. Klicken Sie auf das Häkchensymbol ![Kontrollkästchen-Symbol](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)um den Vorgang abzuschließen.

6. Ein klonauftrag initiiert und Sie werden benachrichtigt, wenn der Klon erstellt. Klicken Sie auf **Auftrag anzeigen** klonauftrag auf der Seite **Projekte** überwachen. Sie sehen die Meldung Abschluss der Klon ist:

    ![Clone-Nachricht](./media/storsimple-clone-volume-u2/CloneMsg.png) 

7. Nach dem Klonvorgang ist abgeschlossen:

  1. **Geräte** Seite, und wählen Sie die Registerkarte **Volumecontainer** . 
  2. Wählen Sie volumecontainer, der dem Quell-Volume zugeordnet ist, die Sie geklont. In der Liste der Datenträger sollte den Klon finden Sie, der soeben erstellte.

>[AZURE.NOTE] Überwachung und standardmäßige Sicherung automatisch auf einem geklonten Volume deaktiviert.

Ein Klon auf diese Weise erstellt ist ein vorübergehender Klon. Weitere Informationen zu Klonen finden Sie unter [transiente und permanente Clones](#transient-vs.-permanent-clones).

Diese Clone ist jetzt eine reguläre Volume und jeder Vorgang auf einem Volume werden der Klon. Sie müssen dieses Volume für Sicherungen zu konfigurieren.

## <a name="transient-vs-permanent-clones"></a>Flüchtige und permanente clones

Vorübergehende Clones werden nur beim Klonen auf einem anderen Gerät erstellt. Sie können ein bestimmtes Volume von einem Sicherungssatz auf einem anderen Gerät vom StorSimple-Manager verwalteten klonen. Vorübergehende Klon verweisen auf Daten in das ursprüngliche Volume und verwenden diese Daten lesen und Schreiben lokal auf dem Zielgerät. 

Nachdem Sie Cloud eine vorübergehende Klon Snapshots wird das geklonte *permanent* werden. Dabei wird eine Kopie der Daten in der Cloud erstellt und die Daten kopieren ist bestimmt durch die Größe der Daten und Azure Wartezeiten (Dies ist eine Azure Azure Kopie). Dieser Prozess kann Wochen dauern. Vorübergehende Klon wird eine permanente Clone und keine Verweise auf das ursprüngliche Volume-Daten aus geklont wurde. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Szenarien für die vorübergehende und dauerhafte clones

Die folgenden Abschnitte beschreiben Beispiel Situationen in denen vorübergehenden und dauerhaften Clones verwendet werden können.

### <a name="item-level-recovery-with-a-transient-clone"></a>Wiederherstellung auf Elementebene durch einen vorübergehenden Klon

Sie müssen eine-einjährigen Microsoft PowerPoint-Präsentationsdatei wiederherstellen. Der IT-Administrator bestimmte Sicherung aus diesem Zeitrahmen identifiziert und filtert das Volume. Administrator das Volume-clones, sucht nach der Datei, die Sie suchen, und stellt Ihnen. In diesem Szenario ist ein vorübergehender Klon verwendet. 
 
![Video verfügbar](./media/storsimple-clone-volume-u2/Video_icon.png) **Video verfügbar**

Ein Video, das veranschaulicht, wie Sie den Klon und Funktionen in StorSimple gelöschte Dateien wiederherzustellen, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>In dieser Umgebung mit einer permanenten testen

Sie müssen einen testen Fehler in der Produktion zu überprüfen. Einen Klon des Volumes in dieser Umgebung erstellen und dann eine Momentaufnahme Cloud diesen Klon geklontes unabhängiges Volume erstellen. In diesem Szenario wird ein permanenter Klon verwendet.  

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie [ein StorSimple-Volume aus einem Sicherungssatz](storsimple-restore-from-backup-set-u2.md)wiederherstellen.

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).

 
