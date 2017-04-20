<properties 
   pageTitle="StorSimple Snapshot-Manager Sicherungskatalog | Microsoft Azure"
   description="Beschreibt das Anzeigen und verwalten den Sicherungskatalog StorSimple Snapshot-Manager-MMC-Snap-in verwenden."
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

# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>StorSimple-Snapshot-Manager den Sicherungskatalog verwalten

## <a name="overview"></a>Übersicht

Die Hauptfunktion von StorSimple Snapshot-Manager werden anwendungskonsistente Sicherungskopien der StorSimple-Volumes aus Snapshots erstellen können. Snapshots werden dann in eine XML-Datei mit einem *Katalog*aufgeführt. Der Sicherungskatalog organisiert Snapshots von Volume-Gruppe und lokale Snapshot oder Snapshot Cloud. 

In diesem Lernprogramm beschreibt die Verwendung des **Sicherungskatalog** Knotens die folgenden Aufgaben durchführen:

- Wiederherstellen eines Volumes 
- Klonen eines Volumes oder Volume-Gruppe 
- Löschen einer Sicherung 
- Wiederherstellen einer Datei
- Storsimple Snapshot-Manager-Datenbank wiederherstellen

Zum Anzeigen des Sicherungskatalog **Sicherungskatalog** Knoten im **Bereichsfenster** erweitern und Erweitern der Volume-Gruppe.

- Wenn Sie Datenträger Gruppennamen klicken, werden im **Ergebnisbereich** Anzahl lokaler Snapshots und Cloud Snapshots für die Volume-Gruppe 

- Wenn Sie **Lokale Snapshot** oder **Snapshot Cloud** **im Ergebnisbereich** werden die folgende Informationen über jede Sicherungssnapshot (je nach Einstellung **Anzeigen** ): 

    - **Name** – die Zeit, die die Momentaufnahme erstellt wurde. 

    - **Typ** – lokale Snapshots oder einen Cloud-Snapshot. 

    - **Besitzer** – der Besitzer des Inhalts. 

    - **Verfügbar** – ob der Snapshot verfügbar ist. **True** gibt an, dass der Snapshot wiederhergestellt werden; **False** gibt an, dass der Snapshot nicht mehr verfügbar. 

    - **Importierte** – gibt an, ob die Sicherung importiert wurde. **True** bedeutet, dass die Sicherung der StorSimple Manager-Dienst gleichzeitig importiert wurde, das Gerät StorSimple Snapshot-Manager konfiguriert wurde. **False** gibt an, dass es nicht importiert, es wurde aber vom StorSimple Snapshot-Manager. (Sie können einfach importierte Volume-Gruppe identifizieren, weil ein Suffix hinzugefügt, das Gerät identifiziert die Volume-Gruppe importiert wurde.)

    ![Backup-Katalog](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)

- Wenn Sie **Lokale Snapshot** oder **Snapshot Cloud**erweitern und klicken Sie dann auf eine einzelne Snapshot-Name, zeigt **der Ergebnisbereich** die folgende Informationen über den ausgewählten Snapshot:

    - **Name** Datenträger anhand von Laufwerkbuchstaben. 

    - **Lokaler Name** der lokale Name des Laufwerks (falls verfügbar). 

    - **Device** -Namen des Geräts, auf dem das Volume befindet. 

    - **Verfügbar** – ob der Snapshot verfügbar ist. **True** gibt an, dass der Snapshot wiederhergestellt werden; **False** gibt an, dass der Snapshot nicht mehr verfügbar. 


## <a name="restore-a-volume"></a>Wiederherstellen eines Volumes

Gehen Sie folgendermaßen vor, um ein Volume aus einer Sicherung wiederherstellen.

#### <a name="prerequisites"></a>Erforderliche Komponenten

Wenn nicht bereits geschehen, erstellen Sie eines Volumes und Volume-Gruppe und dann löschen. Standardmäßig sichert StorSimple Snapshot-Manager ein Volume steuern gelöscht werden. Diese Vorsichtsmaßnahme kann Datenverlust verhindern, wenn das Volume versehentlich gelöscht wird oder Daten aus irgendeinem Grund wiederhergestellt werden müssen. 

StorSimple Snapshot-Manager zeigt die folgende Meldung vorbeugende Sicherung erstellt.

![Automatische Snapshots angezeigt](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

>[AZURE.IMPORTANT]Sie können ein Volume löschen, Teil einer Volume-Gruppe ist. Die Löschoption ist nicht verfügbar. <br>

#### <a name="to-restore-a-volume"></a>Ein Volume wiederherstellen

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager. 

2. Im **Bereichsfenster** den **Sicherungskatalog** Knoten erweitern einer Volume-Gruppe und klicken Sie dann auf **Lokale Snapshots** oder **Cloud Snapshots**. Eine Liste von backup-Snapshots wird im **Ergebnisbereich** angezeigt. 

3. Suchen Sie die Sicherung, die Sie wiederherstellen möchten, mit der rechten Maustaste, und klicken Sie dann auf **Wiederherstellen**. 

    ![Backup-Katalog wiederherstellen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 

4. Auf der Bestätigungsseite Details zu geben **bestätigen**und klicken Sie dann auf **OK**. StorSimple Snapshot-Manager verwendet die Sicherung auf den Datenträger wiederherstellen. 

    ![Bestätigungsnachricht wiederherstellen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 

5. Sie können die Wiederherstellen-Aktion während der Ausführung überwachen. Klicken Sie im **Bereich** Knoten Sie **Aufträge** und dann auf **Ausführen**. Die Details werden im **Ergebnisbereich** angezeigt. Wenn die Auftrag abgeschlossen ist, werden die Auftragsdetails Liste **Letzte 24 Stunden** übertragen.

## <a name="clone-a-volume-or-volume-group"></a>Klonen eines Volumes oder Volume-Gruppe

Gehen Sie folgendermaßen vor, um ein Duplikat (Klon) oder eine Volume-Gruppe erstellen.

#### <a name="to-clone-a-volume-or-volume-group"></a>Klonen eines Volumes oder Volume-Gruppe

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Klicken Sie im **Bereich** den **Sicherungskatalog** Knoten, eine Volume-Gruppe und dann auf **Cloud-Snapshots**. Im **Ergebnisbereich** wird eine Liste der Sicherungskopien angezeigt.

3. Suchen Sie Volume oder zu klonen, Maustaste Datenträger oder Volume-Gruppenname und **Clone**auf Volume-Gruppe. Das Dialogfenster **Klon Cloud-Snapshot** .

    ![Cloudmomentaufnahme Klonen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Führen Sie im Dialogfeld **Klon Cloud Snapshot** wie folgt: 

    1. Namen Sie im Feld **Name** einen für die geklonte Volume. Dieser Name wird im Knoten **Volumes** angezeigt. 

    2. (Optional) Wählen Sie **Laufwerk**und einen Laufwerkbuchstaben aus der Dropdown-Liste wählen. 

    3. (Optional) Wählen Sie **Ordner (NTFS)**, geben Sie einen Pfad oder klicken Sie auf Durchsuchen und wählen Sie einen Speicherort für den Ordner. 

    4. Klicken Sie auf **Erstellen**.

5. Beim cloning-Prozess abgeschlossen ist, müssen Sie das geklonte Volume initialisieren. Starten Sie Server-Manager, und starten Sie die Datenträgerverwaltung. Detaillierte Informationen finden Sie unter [Laden von Volumes](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Nachdem er initialisiert wurde, wird das Volume unter dem Knoten **Volumes** im **Bereichsfenster** aufgeführt. Aufgelisteten Volumes nicht angezeigt wird, aktualisieren Sie die Liste der Volumes ( **Volumes** Knoten klicken und dann auf **Aktualisieren**).

## <a name="delete-a-backup"></a>Löschen einer Sicherung

Gehen Sie um einen Snapshot aus dem Katalog zu löschen. 

>[AZURE.NOTE]Löschen Löscht die Daten des Snapshots zugeordnet. Jedoch kann die Bereinigen von Daten aus der Cloud einige Zeit dauern.<br>
 
#### <a name="to-delete-a-backup"></a>So löschen Sie eine Sicherung

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Im **Bereichsfenster** den **Sicherungskatalog** Knoten erweitern einer Volume-Gruppe und klicken Sie dann auf **Lokale Snapshots** oder **Cloud Snapshots**. Eine Liste von Snapshots wird im **Ergebnisbereich** angezeigt. 

3. Maustaste auf Snapshot löschen möchten, und klicken Sie auf **Löschen**.

4. Wenn die Bestätigungsnachricht angezeigt wird, klicken Sie auf **OK**. 

## <a name="recover-a-file"></a>Wiederherstellen einer Datei

Wenn eine Datei versehentlich von einem Volume gelöscht wird, können Sie die Datei durch einen Snapshot, der den Löschvorgang vorab Daten abrufen, mit dem Snapshot einen Klon des Volumes erstellen und kopieren die Datei aus der geklonten auf das ursprüngliche Volume wiederherstellen.

#### <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass Sie eine aktuelle Sicherung der Volume-Gruppe. Löschen Sie eine Datei auf einem der Volumes in der Volumegruppe. Schließlich gehen Sie folgendermaßen vor, um die gelöschte Datei aus der Sicherung wiederherstellen. 

#### <a name="to-recover-a-deleted-file"></a>Eine gelöschte Datei wiederherstellen

1. Klicken Sie auf StorSimple Snapshot-Manager-Symbol auf Ihrem Desktop. StorSimple Snapshot-Manager-Konsolenfenster wird angezeigt. 

2. Im **Bereichsfenster** den **Sicherungskatalog** Knoten und einen Snapshot der gelöschten Datei durchsuchen. Wählen Sie in der Regel einen Snapshot, der vor der Löschung erstellt wurde. 

3. Finden Sie den Datenträger, den Sie klonen möchten mit der rechten Maustaste, und klicken Sie auf **Klonen**. Das Dialogfenster **Klon Cloud-Snapshot** .

    ![Cloudmomentaufnahme Klonen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Führen Sie im Dialogfeld **Klon Cloud Snapshot** wie folgt: 

   1. Namen Sie im Feld **Name** einen für die geklonte Volume. Dieser Name wird im Knoten **Volumes** angezeigt. 

   2. (Optional) Wählen Sie **Laufwerk**, und wählen Sie einen Laufwerkbuchstaben aus der Dropdown-Liste. 

   3. (Optional) Wählen Sie **Ordner (NTFS)**, geben Sie einen Pfad oder klicken Sie auf **Durchsuchen** und wählen Sie einen Speicherort für den Ordner. 

   4. Klicken Sie auf **Erstellen**. 

5. Beim cloning-Prozess abgeschlossen ist, müssen Sie das geklonte Volume initialisieren. Starten Sie Server-Manager, und starten Sie die Datenträgerverwaltung. Detaillierte Informationen finden Sie unter [Laden von Volumes](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Nachdem er initialisiert wurde, wird das Volume unter dem Knoten **Volumes** im **Bereichsfenster** aufgeführt. 

    Aufgelisteten Volumes nicht angezeigt wird, aktualisieren Sie die Liste der Volumes ( **Volumes** Knoten klicken und dann auf **Aktualisieren**).

6. Den NTFS-Ordner mit dem geklonten Volume **Volumes** Knoten und öffnen Sie dann das geklonte Volume. Suchen Sie die Datei, die Sie wiederherstellen möchten, und auf den primären Datenträger kopieren.

7. Nach dem Wiederherstellen der Datei können Sie den NTFS-Ordner mit dem geklonten Volume löschen.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>StorSimple Snapshot-Manager-Datenbank wiederherstellen

Sie sollten regelmäßig StorSimple Snapshot-Manager-Datenbank auf dem Hostcomputer sichern. Wenn ein Notfall eintritt oder der Host-Computer aus irgendeinem Grund fehlschlägt, können Sie es aus der Sicherung wiederherstellen. Erstellen der Sicherung ist ein manueller Prozess.

#### <a name="to-back-up-and-restore-the-database"></a>Sichern und Wiederherstellen der Datenbank

1. Beenden Sie Management von Microsoft StorSimple:

    1. Starten Sie Server-Manager.

    2. Wählen Sie auf dem Schaltpult Server-Manager im Menü **Extras** **Dienste**.

    3. Wählen Sie im Fenster **Dienste** **Microsoft StorSimple-Verwaltungsdienst**.

    4. Klicken Sie im rechten Bereich **Microsoft StorSimple-Verwaltungsdienst**auf **Beenden Sie den Dienst**.

2. Navigieren Sie auf dem Hostcomputer zu C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData ist ein versteckter Ordner.
 
3. Suchen Sie die XML-Katalogdatei, kopieren Sie die Datei und speichern Sie die Kopie an einem sicheren Ort oder in der Cloud. Wenn der Host, können diese Sicherungsdatei Sie backup-Policies wiederherzustellen, die StorSimple Snapshot-Manager erstellt.

    ![Azure backup StorSimple-Katalogdatei](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)

4. Management von Microsoft StorSimple Dienst: 

    1. Wählen Sie auf dem Schaltpult Server-Manager im Menü **Extras** **Dienste**.
    
    2. Wählen Sie im Fenster **Dienste** **Microsoft StorSimple-Verwaltungsdienst**.

    3. Klicken Sie im rechten **Microsoft StorSimple-Verwaltungsdienst**auf **Dienst neu starten**.

5. Navigieren Sie auf dem Hostcomputer zu C:\ProgramData\Microsoft\StorSimple\BACatalog. 

6. Löschen Sie die XML-Katalogdatei und Ersetzen Sie ihn durch die Sicherungskopie, die Sie erstellt haben. 

7. Klicken Sie auf das Desktopsymbol StorSimple Snapshot-Manager StorSimple Snapshot-Manager starten. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [StorSimple Snapshot-Manager zum Verwalten der StorSimple Lösung](storsimple-snapshot-manager-admin.md).
- Erfahren Sie mehr über [StorSimple Snapshot-Manager-Aufgaben und Workflows](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).
