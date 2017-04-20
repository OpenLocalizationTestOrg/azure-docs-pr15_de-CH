<properties 
   pageTitle="StorSimple Snapshot-Manager Volumegruppen | Microsoft Azure"
   description="Beschreibt das Erstellen und Verwalten von Volume-Gruppen StorSimple Snapshot-Manager-MMC-Snap-in verwenden."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>Mit StorSimple Snapshot-Manager erstellen und Verwalten von Volume-Gruppen

## <a name="overview"></a>Übersicht

Verwenden den **Volume-Gruppen** Knoten auf **den Bereich** Volumegruppen Volumes zuweisen, Informationen über eine Volume-Gruppe, Backups plant und Volume-Gruppen bearbeiten. 

Volume-Gruppen sind Pools verknüpften Volumes sichergestellt, dass Backups Anwendung konsistent sind. Weitere Informationen finden Sie unter [Volumes und Volume-Gruppen](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) und [Integration mit Windows Volumeschattenkopie-Dienst](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

>[AZURE.IMPORTANT] 
>
> * Alle Volumes in einer Volume-Gruppe müssen aus einer einzelnen Cloud-Dienstanbieter kommen.
> 
> * Beim Konfigurieren von Volume-Gruppen nicht Mischen Sie Cluster freigegebenen Clustervolumes (CSVs) und CSVs in der selben Volume Group. StorSimple Snapshot-Manager unterstützt nicht aus CSVs und nicht CSVs in der gleichen Momentaufnahme.
 
![Volume Groups-Knoten](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Abbildung 1: StorSimple Snapshot-Manager Volume Groups-Knoten** 

Dieses Tutorial erläutert die Verwendung StorSimple Snapshot-Manager:

- Informationen zu Volume-Gruppen anzeigen 
- Erstellen einer Volume-Gruppe
- Sichern einer Volume-Gruppe
- Bearbeiten einer Volume-Gruppe
- Löschen einer Volume-Gruppe

Alle diese Aktionen stehen im **Aktionsbereich** .
 
## <a name="view-volume-groups"></a>Volume-Gruppen anzeigen

Wenn Sie **Volume Groups** -Knoten klicken, werden im **Ergebnisbereich** die folgende Informationen zu jeder Volumegruppe je nach ausgewählten Spalte soll (Die Spalten im **Ergebnisbereich** sind konfigurierbar. Klicken Sie auf **Datenträger** Knoten, wählen Sie **Ansicht**und wählen Sie dann **Spalten hinzufügen/entfernen**.)

Ergebnisspalte | Beschreibung 
:--------------|:------------ 
Name           | Die **Name** -Spalte enthält den Namen der Volume-Gruppe.
Anwendung    | Die **Applikationen** Spalte zeigt die Anzahl der VSS Writer derzeit installiert und auf dem Windows-Host ausgeführt.
Ausgewählt       | Die **ausgewählte** Spalte zeigt die Anzahl enthaltenen Volumes in der Volume-Gruppe. 0 (null) gibt an, dass keine Anwendung der Volumes in der Volume-Gruppe zugeordnet ist.
Importiert       | Die **importierten** Spalte zeigt die Anzahl der importierten Volumes. Wenn **True**festgelegt ist, zeigt diese Spalte eine Volume-Gruppe wurde von klassischen Azure-Portal importiert und StorSimple Snapshot-Manager wurde nicht erstellt.
 
>[AZURE.NOTE] StorSimple Snapshot-Manager Volume-Gruppen werden auch auf der Registerkarte **Sicherungsrichtlinien** im klassischen Azure-Portal angezeigt.
 
## <a name="create-a-volume-group"></a>Erstellen einer Volume-Gruppe

Gehen Sie folgendermaßen vor, um eine Volume-Gruppe zu erstellen.

#### <a name="to-create-a-volume-group"></a>Erstellen eine Volumegruppe

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager. 

2. Im **Bereichsfenster** Maustaste auf **Volume-Gruppen**und klicken Sie dann auf **Volume-Gruppe erstellen**. 

    ![Volume-Gruppe erstellen](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    Das Dialogfeld **Erstellen einer Volume-Gruppe** . 

    ![Erstellen Sie ein Dialogfeld Gruppe](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  Geben Sie Folgendes ein: 

    1. Geben Sie im Feld **Name** einen eindeutigen Namen für das neue Volume-Gruppe. 

    2. Wählen Sie im **Applikationen** Applikationen Volumes zu der Volume-Gruppe hinzufügen zugeordnet. 

        Die **Applikationen** Listen sie nur Programme, die StorSimple-Volumes und VSS Writer aktiviert. VSS Writer ist nur aktiviert, wenn die Volumes, die der Writer über StorSimple-Volumes sind. Wenn das Applikationen leer ist, werden Anträge, die Azure StorSimple-Volumes und unterstützt VSS Writer installiert. (Derzeit unterstützt Azure StorSimple Microsoft Exchange und SQL Server.) Weitere Informationen zu VSS Writer finden Sie unter [Integration mit Windows Volumeschattenkopie-Dienst](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

        Wenn Sie eine Anwendung auswählen, werden alle zugeordneten Volumes automatisch ausgewählt. Bei Auswahl von Volumes, die einer bestimmten Anwendung zugeordnet ist die Anwendung dagegen automatisch im **Applikationen** ausgewählt. 

    3. Wählen Sie im **Volumes** StorSimple-Volumes der Volume-Gruppe hinzugefügt. 

      - Sie können mit einem oder mehreren Partitionen enthalten. (Mehrere Partition Volumes dynamischen Festplatten und Basisfestplatten mit mehreren Partitionen kann.) Ein Volume, das mehrere Partitionen enthält, wird als eine Einheit behandelt. Daher, wenn Sie nur eine der Partitionen einer Volume-Gruppe hinzufügen, werden die anderen Partitionen dieser Volume-Gruppe gleichzeitig automatisch. Mehrere Partition Volume weiterhin nach dem Hinzufügen von mehreren Partition Datenträger einer Volume Group als eine Einheit behandelt werden.

      - Leeres Volume-Gruppen können durch keine Volumes zuweisen. 

      - Mischen Sie keine Cluster freigegebenen Clustervolumes (CSVs) und CSVs in der selben Volume Group. StorSimple Snapshot-Manager unterstützt keine aus CSV-Volumes und nicht-CSV-Volumes in denselben Datensnapshot. 

4. Klicken Sie auf **OK** , um die Volume-Gruppe speichern.

## <a name="back-up-a-volume-group"></a>Sichern einer Volume-Gruppe

Gehen Sie zum Sichern einer Volume-Gruppe.

#### <a name="to-back-up-a-volume-group"></a>So sichern Sie eine Volume-Gruppe

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Im **Bereichsfenster** **Volumegruppen** Knoten einer Volume Group Namen und klicken Sie dann auf **Backup nutzen**. 

    ![Volume-Gruppe sofort sichern](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. Im Dialogfeld **Nehmen Sicherung** wählen Sie **Lokale Snapshot** oder **Snapshot Cloud**, und klicken Sie auf **Erstellen**. 

    ![Das Dialogfeld Sicherung übernehmen](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. Bestätigen, dass die Sicherung ausgeführt wird, erweitern Sie den Knoten **Aufträge** und dann auf **Ausführen**. Die Sicherung sollte aufgeführt sein.

5. Zum Anzeigen des abgeschlossenen Snapshots **Sicherungskatalog** Knoten dem Volume Gruppennamen und klicken Sie dann auf **Lokale Snapshot** oder **Snapshot Cloud**. Die Sicherung wird aufgeführt, wenn erfolgreich abgeschlossen. 

## <a name="edit-a-volume-group"></a>Bearbeiten einer Volume-Gruppe

Gehen Sie folgendermaßen vor, um eine Volume-Gruppe zu bearbeiten.

#### <a name="to-edit-a-volume-group"></a>Eine Volume-Gruppe bearbeiten

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Im **Bereichsfenster** Knoten **Volume-Gruppen** mit der rechten Maustaste ein Volume-Gruppenname und klicken Sie dann auf **Bearbeiten**. 

3. Das Dialogfeld **Erstellen einer Volume-Gruppe **. Sie können die Einträge ** **Name**, **Applikationen**und** ändern. 

4. Klicken Sie auf **OK** , um die Änderungen zu speichern.

## <a name="delete-a-volume-group"></a>Löschen einer Volume-Gruppe

Gehen Sie folgendermaßen vor, um eine Volume-Gruppe zu löschen. 

>[AZURE.WARNING] Dies löscht auch alle Backups der Volume-Gruppe.

#### <a name="to-delete-a-volume-group"></a>Eine Volumegruppe löschen

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager. 

2. Im **Bereichsfenster** **Volumegruppen** Knoten einer Volume Group Namen und klicken Sie dann auf **Löschen**. 

3. Das Dialogfenster **Volume-Gruppe löschen** . Geben Sie in das Textfeld **bestätigen** , und klicken Sie dann auf **OK**. 

    Die gelöschten Volume-Gruppe aus der Liste im **Ergebnisbereich** verschwindet, und alle Sicherungen, die mit dieser Volume-Gruppe aus dem Katalog gelöscht.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager zum Verwalten der StorSimple Lösung](storsimple-snapshot-manager-admin.md).
- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager erstellen und Verwalten von backup-Policies](storsimple-snapshot-manager-manage-backup-policies.md).
