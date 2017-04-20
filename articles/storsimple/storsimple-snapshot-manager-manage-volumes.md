<properties 
   pageTitle="StorSimple Snapshot-Manager und Volumes | Microsoft Azure"
   description="Beschreibt das Anzeigen und Verwalten von Datenträgern und Backups konfigurieren StorSimple Snapshot-Manager-MMC-Snap-in verwenden."
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

# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>Mit StorSimple Snapshot-Manager anzeigen und Verwalten von Datenträgern

## <a name="overview"></a>Übersicht

StorSimple Snapshot-Manager **Volumes** Knoten können (im **Bereich)** Sie Volumes auswählen und Anzeigen von Informationen. Die Volumes werden als Laufwerke, die entsprechen, der vom Host bereitgestellte Volumes angezeigt. Der Knoten **Volumes** zeigt lokale Volumes und Datenträgertypen unterstützt StorSimple einschließlich iSCSI und ein Gerät erkannt werden. 

Weitere Informationen zu unterstützten Volumes finden Sie [Unterstützung für mehrere Datenträger](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Volumeliste im Ergebnisbereich](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

Knoten **Volumes** können Sie scannen oder Volumes löschen, nachdem StorSimple Snapshot-Manager erkennt. 

Dieses Tutorial erklärt zum Bereitstellen, zu initialisieren, und formatieren Sie Volumes und verwenden StorSimple Snapshot-Manager:

- Anzeigen von Informationen über 
- Datenträger löschen
- Scannen von volumes 
- Ein Basisvolume konfigurieren und sichern
- Konfigurieren eines dynamischen gespiegelten Datenträgers und sichern

>[AZURE.NOTE] Alle **Datenträger** Knoten stehen auch im **Aktionsbereich** .
 
## <a name="mount-volumes"></a>Bereitstellen von volumes

Verwenden Sie folgende Verfahren bereitstellen, initialisieren und formatieren StorSimple-Volumes. Dieses Verfahren verwendet die Datenträgerverwaltung, ein Dienstprogramm zum Verwalten von Festplatten und der entsprechenden Volumes oder Partitionen. Weitere Informationen zu Datenträger finden Sie [Datenträger](https://technet.microsoft.com/library/cc770943.aspx) auf der Microsoft TechNet-Website.

#### <a name="to-mount-volumes"></a>Bereitstellen von volumes

1. Starten Sie den Microsoft iSCSI-Initiator auf dem Hostcomputer.

2. Eine Schnittstelle IP-Adressen als das Zielportal oder Discovery IP-Adresse und das Gerät an. Nachdem das Gerät werden die Volumes für Ihr Windows-System. Weitere Informationen zur Verwendung des Microsoft iSCSI-Initiators finden Sie im Abschnitt "Verbinden mit einem iSCSI-Zielgerät" [Installieren und Konfigurieren von Microsoft]iSCSI-Initiator[1].

3. Verwenden Sie eine der folgenden Optionen Datenträger starten:

    - Geben Sie im Feld **Ausführen** Diskmgmt.msc.

    - Starten Sie Server-Manager Knoten Sie den **Speicher** und wählen Sie **Disk Management**.

    - Start **Verwaltung**, erweitern Sie den Knoten **Computer Management** und wählen Sie **Disk Management**. 

    >[AZURE.NOTE] Sie müssen Administratorrechte Datenträger ausführen verwenden.
 
4. Nehmen Sie die Volumes online:

   1. Klicken Sie in der Datenträgerverwaltung jedes Volume als **Offline**markiert.

   2. Klicken Sie auf **Datenträger reaktivieren**. Die Festplatte sollte **Online** markiert werden, nachdem die Festplatte aktiviert ist.

5. Initialisieren Sie die Volumes:

   1. Klicken Sie auf den ermittelten Volumes.

   2. Wählen Sie im Menü **Datenträger initialisieren**.

   3. Klicken Sie im Dialogfeld **Datenträger initialisieren** wählen Sie die Laufwerke, die initialisiert werden soll und klicken Sie dann auf **OK**.

6. Formatieren Sie einfache Volumes:

   1. Klicken Sie auf ein Volume, das Sie formatieren möchten.

   2. Wählen Sie im Menü **Neues einfaches Volume**.

   3. Verwenden Sie den Assistenten Neues einfaches Volume, um das Volume zu formatieren:

      - Geben Sie die Volume-Größe.
      - Geben Sie einen Laufwerkbuchstaben.
      - Wählen Sie das NTFS-Dateisystem.
      - Geben Sie eine Größe der Zuordnungseinheiten 64 KB.
      - Durchführen Sie Formatierung mit QuickFormat.

7. Formatieren Sie Volumes mit mehreren Partitionen. Informationen finden Sie im Abschnitt "Partitionen und Volumes" in der [Datenträgerverwaltung implementieren](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Informationen zu den Datenträgern anzeigen

Gehen Sie die Informationen zu lokalen und Azure StorSimple-Volumes.

#### <a name="to-view-volume-information"></a>Volumeinformationen anzeigen

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager. 

2. Klicken Sie im **Bereich** den Knoten **Volumes** . Im **Ergebnisbereich** wird eine Liste von lokalen und bereitgestellte Volumes auch alle Azure StorSimple, angezeigt. Die Spalten im **Ergebnisbereich** sind konfigurierbar. (Knoten **Volumes** Maustaste wählen Sie **Ansicht**und wählen Sie **Spalten hinzufügen/entfernen**.)

    ![Spalten konfigurieren](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)

    Ergebnisspalte | Beschreibung 
    :--------------|:-------------
    Name           | **Die Spalte** enthält den Laufwerkbuchstaben ermittelten Volumes.
    Gerät         | In der Spalte **Gerät** enthält die IP-Adresse des Geräts mit dem Host-Computer verbunden.
    Volume-Gerätename | **Gerätename Volume** -Spalte enthält den Namen des ausgewählten Volumes gehört Gerätelautstärke. Dies ist der Datenträgername im klassischen Azure-Portal für das spezifische Volume definiert.
    Zugriffspfade   | Die Spalte **Zugriffspfade** zeigt den Zugriffspfad auf dem Volume. Darum Laufwerk Laufwerkbuchstabe oder Bereitstellungspunkt mit das Volume auf dem Hostcomputer zugegriffen werden kann.
 
## <a name="delete-a-volume"></a>Löschen eines Datenträgers

Gehen Sie zum Löschen eines Volumes von StorSimple Snapshot-Manager.

>[AZURE.NOTE] Sie können kein Volume löschen, ist Teil einer Volumegruppe. (Die Löschoption ist nicht verfügbar für Volumes, die Mitglieder einer Volume Group). Löschen Sie die gesamte Volume-Gruppe, um das Volume zu löschen.


#### <a name="to-delete-a-volume"></a>Löschen eines Volumes

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Klicken Sie im **Bereich** den Knoten **Volumes** . 

3. Klicken Sie im **Ergebnisbereich** auf das Volume, das Sie löschen möchten.

4. Klicken Sie im Menü auf **Löschen**. 

    ![Löschen eines Datenträgers](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 

5. Das Dialogfenster **Volume löschen** . Geben Sie in das Textfeld **bestätigen** , und klicken Sie dann auf **OK**.

6. Standardmäßig sichert StorSimple Snapshot-Manager ein Volume vor dem Löschen. Diese Vorsichtsmaßnahme können Sie Schutz vor Datenverlust wurde die Löschung unbeabsichtigt. StorSimple Snapshot-Manager zeigt eine **Automatische Snapshots** Fortschritt an, während das Volume gesichert. 

    ![Automatische Snapshots angezeigt](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Scannen von volumes

Gehen Sie zum Scannen von Volumes auf StorSimple Snapshot-Manager verbunden.

#### <a name="to-rescan-the-volumes"></a>Die Volumes Scannen

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Im **Bereichsfenster** Maustaste auf **Volumes**und klicken Sie dann auf **Datenträger neu einlesen**.

    ![Scannen von volumes](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
 
    Dieses Verfahren wird die Liste mit StorSimple Snapshot-Manager synchronisiert. Die Ergebnisse werden wie neue oder gelöschte Volumes Änderungen wider.

## <a name="configure-and-back-up-a-basic-volume"></a>Konfigurieren und Sichern eines Basisvolumes

Gehen Sie die Sicherung eines Basisvolumes konfigurieren und dann sofort eine Sicherung oder eine Richtlinie für geplante Backups erstellen.

### <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen:

- Stellen Sie sicher, dass StorSimple Gerät und Host-Computer korrekt konfiguriert sind. Finden Sie weitere Informationen zum [Bereitstellen der lokalen StorSimple-Gerät](storsimple-deployment-walkthrough-u2.md).

- Installieren und Konfigurieren von StorSimple Snapshot-Manager. Weitere Informationen finden Sie [StorSimple Snapshot-Manager bereitstellen](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Konfigurieren der Sicherung eines Basisvolumes

1. Erstellen eines Basisvolumes auf dem Gerät StorSimple.

2. Bereitstellen Sie, initialisieren Sie und formatieren Sie des Volumes Siehe [Volumes laden](#mount-volumes). 

3. Klicken Sie auf StorSimple Snapshot-Manager-Symbol auf Ihrem Desktop. StorSimple Snapshot-Manager angezeigt. 

4. Im **Bereichsfenster** Maustaste Knotens **Datenträger** und **Volumes Scannen**klicken. Wenn der Scan abgeschlossen ist, sollte eine Liste der Volumes im **Ergebnisbereich** angezeigt werden. 

5. Im **Ergebnisbereich** mit der rechten Maustaste des Volumes und wählen Sie die **Volume-Gruppe erstellen**. 

    ![Volume-Gruppe erstellen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 

6. Im Dialogfeld **Volume-Gruppe erstellen** einen Namen für die Volume-Gruppe Volumes zuweisen und klicken Sie dann auf **OK**.

7. Erweitern Sie im **Bereichsfenster** **Volume-Gruppen** . Die Volume-Gruppe erscheint unter dem Knoten **Volume-Gruppen** . 

8. Maustaste auf Volume Group.

    - Klicken Sie einen interaktiven (bei Bedarf) Sicherungsauftrag **Backup nutzen**. 

    - Um eine automatische Sicherung zu planen, klicken Sie auf **Backup-Richtlinie erstellen**. Wählen Sie auf der Registerkarte **Allgemein** eine Volume-Gruppe aus. Geben Sie auf der Seite **Zeitplan** Details für den Zeitplan. Wenn Sie fertig sind, klicken Sie auf **OK**. 

9. Bestätigen, dass der Sicherungsauftrag gestartet wurde, erweitern Sie den Knoten **Aufträge** im **Bereichsfenster** und klicken Sie auf den Knoten **ausgeführt** . Die Liste der aktuell ausgeführten Jobs wird im **Ergebnisbereich** angezeigt. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Konfigurieren und Sichern eines dynamischen gespiegelten Datenträgers

Gehen Sie folgendermaßen vor um Sicherung eines dynamischen gespiegelten Datenträgers zu konfigurieren:

- Schritt 1: Verwendung Datenträgerverwaltung ein dynamisches gespiegeltes Datenträgers erstellen. 

- Schritt 2: Mit StorSimple Snapshot-Manager Sicherung konfigurieren.

### <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen:

- Stellen Sie sicher, dass StorSimple Gerät und Host-Computer korrekt konfiguriert sind. Finden Sie weitere Informationen zum [Bereitstellen der lokalen StorSimple-Gerät](storsimple-deployment-walkthrough-u2.md).

- Installieren und Konfigurieren von StorSimple Snapshot-Manager. Weitere Informationen finden Sie [StorSimple Snapshot-Manager bereitstellen](storsimple-snapshot-manager-deployment.md).

- Konfigurieren Sie zwei Volumes auf dem Gerät StorSimple. (In den Beispielen sind die verfügbaren Volumes **Datenträger 1** und **Datenträger 2**.) 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Schritt 1: Verwendung Disk Management zum Erstellen eines dynamischen gespiegelten Datenträgers

Datenträger ist ein Dienstprogramm zum Verwalten von Festplatten und Volumes oder Partitionen. Weitere Informationen zu Datenträger finden Sie [Datenträger](https://technet.microsoft.com/library/cc770943.aspx) auf der Microsoft TechNet-Website.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Erstellen ein dynamisches gespiegeltes Datenträgers

1. Verwenden Sie eine der folgenden Optionen Datenträger starten: 

   - Öffnen Sie das **Ausführen** , geben Sie **Diskmgmt.msc**und drücken Sie die EINGABETASTE.

   - Starten Sie Server-Manager Knoten Sie den **Speicher** und wählen Sie **Disk Management**. 

   - Start **Verwaltung**, erweitern Sie den Knoten **Computer Management** und wählen Sie **Disk Management**. 

2. Achten Sie zwei Volumes verfügbar auf dem Gerät StorSimple. (Im Beispiel sind die verfügbaren Volumes **Datenträger 1** und **Datenträger 2**.) 

3. Rechts im unteren Fenster Laufwerk-Management Maustaste auf **Datenträger 1** und wählen Sie **Neue gespiegelte Volumes**. 

    ![Neues gespiegeltes Volume](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 

4. Klicken Sie auf der Seite **Neue gespiegelte Volumes** auf **Weiter**.

5. Wählen Sie auf der Seite **Datenträger wählen** **Diskette 2** in den Bereich **ausgewählte** klicken Sie auf **Hinzufügen**, und klicken Sie auf **Weiter**. 

6. Übernehmen Sie auf der Seite **Laufwerkbuchstaben zuweisen oder Pfad** die Standardeinstellungen, und klicken Sie auf **Weiter**. 

7. Wählen Sie auf der Seite **Volume formatieren** im Feld **Größe der Zuordnungseinheit** **64 KB**. Aktivieren Sie das Kontrollkästchen **Formatierung mit QuickFormat durchführen** und dann auf **Weiter**. 

8. **Neue gespiegelte Volumes** auf der Seite überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**. 

9. Eine Meldung an, dass der Basisdatenträger in einen dynamischen Datenträger konvertiert werden. Klicken Sie auf **Ja**.

    ![Dynamische Datenträger Konvertierung Nachricht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 

10. In der Datenträgerverwaltung sicher, dass Datenträger 1 und Datenträger 2 als dynamische gespiegelte Volumes angezeigt werden. (**Dynamische** erscheint in der Statusspalte und sollte die Kapazität Farbe Rot zeigt ein gespiegeltes Volume ändern.) 

    ![Dynamische Verwaltung gespiegelter Datenträger](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 
 
### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Schritt 2: Mit StorSimple Snapshot-Manager Sicherung konfigurieren

Gehen Sie zum Konfigurieren eines dynamischen gespiegelten Datenträgers und sofort eine Sicherung oder eine Richtlinie für geplante Backups erstellen.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Sicherung eines dynamischen gespiegelten Datenträgers konfigurieren

1. Klicken Sie auf StorSimple Snapshot-Manager-Symbol auf Ihrem Desktop. StorSimple Snapshot-Manager angezeigt. 

2. Im **Bereichsfenster** Maustaste Knoten **Volumes** und wählen **Volumes Scannen**. Wenn der Scan abgeschlossen ist, sollte eine Liste der Volumes im **Ergebnisbereich** angezeigt werden. Die dynamischen gespiegelten Datenträgers ist als ein einzelnes Volume aufgeführt. 

3. Im **Ergebnisbereich** mit der rechten Maustaste dynamischen gespiegelten Datenträgers und dann auf **Volume-Gruppe erstellen**. 

4. Im Dialogfeld **Volume-Gruppe erstellen** einen Namen für die Volume-Gruppe dynamischen gespiegelten Gruppe zuweisen und klicken Sie dann auf **OK**. 

5. Erweitern Sie im **Bereichsfenster** **Volume-Gruppen** . Die Volume-Gruppe erscheint unter dem Knoten **Volume-Gruppen** . 

6. Maustaste auf Volume Group. 

    - Klicken Sie einen interaktiven (bei Bedarf) Sicherungsauftrag **Backup nutzen**. 

    - Um eine automatische Sicherung zu planen, klicken Sie auf **Backup-Richtlinie erstellen**. Wählen Sie auf der Seite **Allgemein** der Volume-Gruppe aus. Geben Sie auf der Seite **Zeitplan** Details für den Zeitplan. Wenn Sie fertig sind, klicken Sie auf **OK**. 

7. Sie können den Sicherungsauftrag während der Ausführung überwachen. Erweitern Sie den Knoten **Aufträge** im **Bereichsfenster** klicken Sie auf **Ausführen**und die Auftragsdetails im **Ergebnisbereich** angezeigt. Nach Abschluss des Sicherungsauftrags werden die Details in der Liste der **letzten 24** Stunden übertragen. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager zum Verwalten der StorSimple Lösung](storsimple-snapshot-manager-admin.md).
- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager erstellen und Verwalten von Volume-Gruppen](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
