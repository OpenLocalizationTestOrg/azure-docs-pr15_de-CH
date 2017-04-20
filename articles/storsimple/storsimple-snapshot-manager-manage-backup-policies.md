<properties 
   pageTitle="StorSimple Snapshot-backup-Richtlinien | Microsoft Azure"
   description="Beschreibt das Erstellen und Verwalten von backup-Policies, die geplante Backups steuern StorSimple Snapshot-Manager-MMC-Snap-in verwenden."
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
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Mit StorSimple Snapshot-Manager erstellen und Verwalten von backup-policies

## <a name="overview"></a>Übersicht

Eine Sicherungsrichtlinie erstellt einen Zeitplan für die Sicherung von Volume-Daten lokal oder in der Cloud. Wenn Sie eine Sicherungsrichtlinie erstellen, können Sie eine Aufbewahrungsrichtlinie angeben. (Sie können bis zu 64 Snapshots beibehalten.) Weitere Informationen zu backup-Richtlinien finden Sie unter [Backup-Typen](storsimple-what-is-snapshot-manager.md#backup-type) in [StorSimple 8000-Serie: Hybriden Cloud-Lösung](storsimple-overview.md).

In diesem Lernprogramm wird erläutert, wie Sie:

- Backup-Richtlinie erstellen 
- Bearbeiten einer Sicherungsrichtlinie 
- Löschen einer backup-Richtlinie 

## <a name="create-a-backup-policy"></a>Backup-Richtlinie erstellen

Gehen Sie zum Erstellen einer neuen backup-Richtlinie.

#### <a name="to-create-a-backup-policy"></a>Backup-Richtlinie erstellen

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Im **Bereichsfenster** mit der rechten Maustaste **Sicherungsrichtlinien**und auf **Backup-Richtlinie erstellen**.

    ![Backup-Richtlinie erstellen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Das Dialogfenster **Richtlinie erstellen** . 

    ![Erstellen einer Richtlinie - Allgemein](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Führen Sie auf der Registerkarte **Allgemein** die folgende Informationen ein:

   1. Geben Sie im Feld **Name** einen Namen für die Richtlinie.

   2. Geben Sie im Textfeld **Volume-Gruppe** den Namen Volume-Gruppe, die der Richtlinie zugeordnet.

   3. Wählen Sie **lokale Snapshot** oder **Snapshot Cloud**.

   4. Anzahl der Snapshots beibehalten. Wenn Sie **Alle**auswählen, werden 64 Snapshots beibehalten (Maximum). 

4. Klicken Sie auf die Registerkarte **Zeitplan** .

    ![Erstellen einer Richtlinie – Registerkarte Zeitplan](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Führen Sie auf der Registerkarte **Zeitplan** die folgende Informationen: 

   1. Klicken Sie auf das Kontrollkästchen **Aktivieren** , um die nächste Sicherung planen.

   2. Wählen Sie unter **Optionen** **einmal**, **täglich**, **wöchentlich**oder **monatlich**. 

   3. Klicken Sie im Textfeld **Starten** auf das Kalendersymbol, und wählen Sie ein Startdatum.

   4. Unter **Erweiterte Einstellungen**können Sie optionale wiederholen Sie Zeitpläne und ein Enddatum festlegen.

   5. Klicken Sie auf **OK**.

Nach dem Erstellen einer Sicherungsrichtlinie im **Ergebnisbereich** die folgenden Informationen angezeigt:

- **Name** – der Name des backup-Richtlinie.

- **Typ** – lokale Snapshots oder Cloud-Snapshot.

- **Volume-Gruppe** – der Volume-Gruppe, die der Richtlinie zugeordnet.

- **Archivierung** – die Anzahl der Snapshots beibehalten; Der Höchstwert beträgt 64.

- **Erstellt** – das Datum, das diese Richtlinie erstellt wurde.

- **Aktiviert** –, ob die Richtlinie derzeit gültig ist: **True** gibt an, dass es aktiviert; **False** gibt an, dass es nicht aktiviert ist. 

## <a name="edit-a-backup-policy"></a>Bearbeiten einer Sicherungsrichtlinie

Gehen Sie zum Bearbeiten einer vorhandenen backup-Richtlinie.

#### <a name="to-edit-a-backup-policy"></a>So bearbeiten Sie eine Sicherungsrichtlinie

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager. 

2. Klicken Sie im **Bereich** den Knoten **Backup-Richtlinien** . Alle backup-Policies werden im **Ergebnisbereich** angezeigt. 

3. Maustaste auf die Richtlinie, die Sie bearbeiten möchten, und klicken Sie auf **Bearbeiten**. 

    ![Bearbeiten einer Sicherungsrichtlinie](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. Die **Richtlinie erstellen** im Fenster geben Sie die Änderungen und klicken Sie auf **OK**. 

## <a name="delete-a-backup-policy"></a>Löschen einer backup-Richtlinie

Gehen Sie zum Löschen einer Sicherungsrichtlinie.

#### <a name="to-delete-a-backup-policy"></a>So löschen Sie eine Sicherungsrichtlinie

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager. 

2. Klicken Sie im **Bereich** den Knoten **Backup-Richtlinien** . Alle backup-Policies werden im **Ergebnisbereich** angezeigt. 

3. Maustaste auf backup-Richtlinie, die Sie löschen möchten, und klicken Sie auf **Löschen**.

4. Klicken Sie auf **Ja**, wenn die Bestätigungsnachricht angezeigt wird.

    ![Backup-Richtlinie Bestätigung löschen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager zum Verwalten der StorSimple Lösung](storsimple-snapshot-manager-admin.md).
- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager anzeigen und Verwalten von backup-Jobs](storsimple-snapshot-manager-manage-backup-jobs.md).
