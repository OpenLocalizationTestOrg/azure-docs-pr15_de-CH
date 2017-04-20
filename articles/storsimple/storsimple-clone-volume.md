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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>Verwenden des StorSimple Manager-Dienstes ein Volume Klonen

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Übersicht

StorSimple Manager Service **Sicherungskatalog** Seite zeigt alle Sicherungssätze, die bei manuellen oder automatisierten Backups erstellt werden. Sie können mit Hilfe dieser Seite alle Backups für eine Sicherungsrichtlinie oder ein Volume auswählen oder Löschen von Backups oder mithilfe eine Sicherung wiederherstellen oder Klonen eines Volumes.

![Katalog-Seite](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

In diesem Lernprogramm beschreibt die Verwendung einer Sicherungssatzes auf einem einzelnen Volume Klonen festgelegt. Darüber hinaus unterscheiden *transiente* und *permanente* Clones. 

## <a name="create-a-clone-of-a-volume"></a>Klonen eines Volumes

Einen lokalen oder einen Snapshot Cloud können Sie einen Klon auf dasselbe Gerät, ein anderes Gerät oder sogar einen virtuellen Computer erstellen.

#### <a name="to-clone-a-volume"></a>Klonen ein Volumes

1. Auf der Seite StorSimple Manager auf **Backup** -Katalog und wählen Sie einen backup-Satz.

2. Erweitern Sie den Sicherungssatz mit den zugeordneten Volumes anzeigen. Klicken Sie auf, und wählen Sie ein Volume aus dem Sicherungssatz.

     ![Klonen eines Volumes](./media/storsimple-clone-volume/HCS_Clone.png) 

3. Klicken Sie auf **Clone** zunächst das ausgewählte Volume klonen.

4. Im Assistenten Clone-Volume unter **Speicherort und Namen angeben**:

  1. Ein Zielgerät zu identifizieren. Dies ist der Speicherort, in dem der Klon erstellt werden. Sie können dasselbe Gerät auswählen oder ein anderes Gerät. Wählt ein Volume mit anderen Cloud verbundenen (nicht Azure) die Dropdown-Liste für das Zielgerät nur physische Geräte anzeigen. Ein Volume auf einem virtuellen Gerät mit anderen Cloud zugewiesen kann nicht geklont werden.

        >  [AZURE.NOTE] Stellen Sie sicher, dass für den Klon erforderliche Kapazität kleiner als die Kapazität des Zielgeräts ist.
  2. Geben Sie einen eindeutigen Datenträgernamen der Klon. Der Name muss zwischen 3 und 127 Zeichen enthalten.
  3. Klicken Sie auf das Pfeilsymbol ![Pfeil-icon](./media/storsimple-clone-volume/HCS_ArrowIcon.png) auf der nächsten Seite fortgesetzt.

5. Klicken Sie unter **Angeben von Hosts, die dieses Volume verwenden können**:

  1. Geben Sie einen Steuerelement Zugriffsdatensatz (ACR) für den Klon. Sie können eine neue ACR oder aus der vorhandenen Liste.
  2. Klicken Sie auf das Häkchensymbol ![Kontrollkästchen-Symbol](./media/storsimple-clone-volume/HCS_CheckIcon.png)um den Vorgang abzuschließen.

6. Ein klonauftrag initiiert und Sie werden benachrichtigt, wenn der Klon erstellt. Klicken Sie auf **Auftrag anzeigen** klonauftrag auf der Seite **Projekte** überwachen.

7. Nach dem Klonvorgang ist abgeschlossen:

  1. **Geräte** Seite, und wählen Sie die Registerkarte **Volumecontainer** . 
  2. Wählen Sie volumecontainer, der dem Quell-Volume zugeordnet ist, die Sie geklont. In der Liste der Datenträger sollte den Klon finden Sie, der soeben erstellte.

>[AZURE.NOTE] Überwachung und standardmäßige Sicherung automatisch auf einem geklonten Volume deaktiviert.

Ein Klon auf diese Weise erstellt ist ein vorübergehender Klon. Weitere Informationen zu Klonen finden Sie unter [transiente und permanente Clones](#transient-vs.-permanent-clones).

Diese Clone ist jetzt eine reguläre Volume und jeder Vorgang auf einem Volume werden der Klon. Sie müssen dieses Volume für Sicherungen zu konfigurieren.

## <a name="transient-vs-permanent-clones"></a>Flüchtige und permanente clones

Transiente und permanente Clones werden nur beim Klonen auf einem anderen Gerät erstellt. Sie können ein bestimmtes Volume von einem Sicherungssatz auf einem anderen Gerät klonen. Ein Klon, so ist ein *vorübergehender* Klon. Vorübergehende Klon muss Verweise auf das ursprüngliche Volume und das Volume zum Lesen und Schreiben von lokal verwenden. 

Nachdem Sie Cloud eine vorübergehende Klon Snapshots wird das geklonte *permanent* werden. Permanente Klon ist unabhängig und hat alle Verweise auf das ursprüngliche Volume aus geklont wurde.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Szenarien für die vorübergehende und dauerhafte clones

Die folgenden Abschnitte beschreiben Beispiel Situationen in denen vorübergehenden und dauerhaften Clones verwendet werden können.

### <a name="item-level-recovery-with-a-transient-clone"></a>Wiederherstellung auf Elementebene durch einen vorübergehenden Klon

Sie müssen eine-einjährigen Microsoft PowerPoint-Präsentationsdatei wiederherstellen. Der IT-Administrator bestimmte Sicherung aus diesem Zeitrahmen identifiziert und filtert das Volume. Administrator das Volume-clones, sucht nach der Datei, die Sie suchen, und stellt Ihnen. In diesem Szenario ist ein vorübergehender Klon verwendet. 
 
![Video verfügbar](./media/storsimple-clone-volume/Video_icon.png) **Video verfügbar**

Ein Video, das veranschaulicht, wie Sie den Klon und Funktionen in StorSimple gelöschte Dateien wiederherzustellen, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>In dieser Umgebung mit einer permanenten testen

Sie müssen einen testen Fehler in der Produktion zu überprüfen. Sie Klonen des Datenträgers in der Produktion von Momentaufnahmen dieser Klon Cloud. Das geklonte Volume ist jetzt unabhängig. In diesem Szenario wird ein permanenter Klon verwendet.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie [ein StorSimple-Volume aus einem Sicherungssatz](storsimple-restore-from-backup-set.md)wiederherstellen.

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).

 
