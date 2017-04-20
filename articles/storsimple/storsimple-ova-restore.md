<properties
   pageTitle="Wiederherstellung von einer Sicherung Ihres virtuellen StorSimple-Arrays"
   description="Weitere Informationen zum Wiederherstellen einer Sicherung des Arrays virtuellen StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Wiederherstellung von einer Sicherung Ihres virtuellen StorSimple-Arrays

## <a name="overview"></a>Übersicht 

Dieser Artikel bezieht sich auf Microsoft Azure StorSimple Virtual Array (auch bekannt als StorSimple lokalen virtuellen Gerät oder virtuelles Gerät StorSimple) ausgeführten März 2016 allgemein verfügbar (GA) oder höher. Dieser Artikel beschreibt Schritt für Schritt aus einem Sicherungssatz der Freigaben oder Volumes auf das virtuelle StorSimple Array wiederherstellen. Dieser Artikel erläutert auch die Wiederherstellung auf Elementebene auf Ihrem virtuellen StorSimple-Array funktioniert, die als Dateiserver konfiguriert.


## <a name="restore-shares-from-a-backup-set"></a>Aktien aus einem Sicherungssatz wiederherstellen


**Bevor Sie versuchen, die Freigaben wiederherzustellen, sicherstellen Sie, dass Sie ausreichend Speicherplatz auf dem Gerät für diesen Vorgang.** Wiederherstellung von einer Sicherung in [Azure-Verwaltungsportal](https://manage.windowsazure.com/)Schritte.

#### <a name="to-restore-a-share"></a>Eine Freigabe wiederherstellen

1.  Wechseln Sie zum **katalogisieren der Sicherung**. Filtern Sie Geräts und Zeit für Backups zu suchen. Klicken Sie auf Check ![](./media/storsimple-ova-restore/image1.png) Abfragen.


1.  In der Liste der Sicherungssätze angezeigt auf, und wählen Sie eine bestimmte Sicherung. Erweitern Sie die Sicherung, um die unterschiedlichen Freigaben wird angezeigt. Klicken Sie auf, und wählen Sie eine Freigabe, die Sie wiederherstellen möchten.

2.  Am unteren Rand der Seite klicken Sie auf **als neues wiederherstellen**.

3.  **Neue Freigabe** Assistenten wird gestartet. Auf der Seite **Geben Sie Namen und Speicherort** :


    1.  Überprüfen Sie den Gerätenamen Quelle. Dies sollte das Gerät mit der Freigabe, die Sie wiederherstellen möchten. Die Geräteauswahl ist deaktiviert. Um andere Geräte auszuwählen, müssen Sie den Assistenten beenden und erneut den Sicherungssatz erneut.

    2.  Geben Sie einen Freigabenamen ein. Der Freigabename muss 3 bis 127 Zeichen enthalten.

    3.  Überprüfen Sie Größe, Typ und Berechtigungen für die Freigabe, die Sie wiederherstellen möchten. Sie werden die Freigabeeigenschaften über Windows Explorer ändern, nachdem die Wiederherstellung abgeschlossen ist.

    4.  Klicken Sie auf Check ![](./media/storsimple-ova-restore/image1.png).

        ![](./media/storsimple-ova-restore/image9.png)

1.  Nachdem der Wiederherstellungsauftrag abgeschlossen ist, startet die Wiederherstellung und wird eine andere Meldung angezeigt. Um den Fortschritt der Wiederherstellung zu überwachen, klicken Sie auf **Auftrag anzeigen**. Dadurch gelangen Sie zur Seite " **Aufträge** ".

2.  Sie können den Status des Wiederherstellungsauftrags verfolgen. Wenn die Wiederherstellung auf 100 % abgeschlossen ist, navigieren Sie zur Seite **Freigaben** auf Ihrem Gerät.

3.  Sie können jetzt neue wiederhergestellte Freigabe in der Liste der Freigaben auf Ihrem Gerät anzeigen. Beachten Sie, dass die Wiederherstellung auf die Freigabe erfolgt. Mehrstufige Freigabe wiederhergestellt, wie Ebenen und lokales lokales freigeben.

Sie haben nun die Konfiguration abgeschlossen und gelernt, sichern oder Wiederherstellen einer Freigabe. 


## <a name="restore-volumes-from-a-backup-set"></a>Volumes von einem Sicherungssatz wiederherstellen


Wiederherstellung von einer Sicherung in der Azure-Verwaltungsportal Schritte. Die Wiederherstellung wird die Sicherung auf einem neuen Volume auf demselben virtuellen Gerät wiederhergestellt. Sie können nicht auf ein anderes Gerät wiederherstellen.

#### <a name="to-restore-a-volume"></a>Ein Volume wiederherstellen

1.  Wechseln Sie zum **katalogisieren der Sicherung**. Filtern Sie Geräts und Zeit für Backups zu suchen. Klicken Sie auf Check ![](./media/storsimple-ova-restore/image1.png) Abfragen.

2.  In der Liste der Sicherungssätze angezeigt auf, und wählen Sie eine bestimmte Sicherung. Erweitern Sie die Sicherung, um die verschiedenen Volumes wird angezeigt. Wählen Sie das Volume wiederherstellen möchten. 

5.  Am unteren Rand der Seite klicken Sie auf **als neues wiederherstellen**. **Als neue Volume wiederherstellen** -Assistent wird gestartet.

1.  Auf der Seite **Geben Sie Namen und Speicherort** :


    1.  Überprüfen Sie den Gerätenamen Quelle. Dies sollte das Gerät mit dem Volume, das Sie wiederherstellen möchten. Die Auswahl des Geräts ist nicht verfügbar. Um andere Geräte auszuwählen, müssen Sie den Assistenten beenden und erneut den Sicherungssatz erneut.

    2.  Benennen Sie Volume neu wiederhergestellten Volumes. Der Volume-Name muss 3 bis 127 Zeichen enthalten.

    3.  Klicken Sie auf das Pfeilsymbol.

        ![](./media/storsimple-ova-restore/image12.png)

1.  Wählen Sie auf der Seite **Geben Sie Hosts, die dieses Volume verwenden können,** entsprechende ACRs aus der Dropdownliste aus.

    ![](./media/storsimple-ova-restore/image13.png)

1.  Klicken Sie auf Check ![](./media/storsimple-ova-restore/image1.png). Einen Wiederherstellungsauftrag wird gestartet, und Sie sehen die folgende Benachrichtigung der Auftrag ausgeführt wird.

2.  Nachdem der Wiederherstellungsauftrag abgeschlossen ist, startet die Wiederherstellung und wird eine andere Meldung angezeigt. Um den Fortschritt der Wiederherstellung zu überwachen, klicken Sie auf **Auftrag anzeigen**. Dadurch gelangen Sie zur Seite " **Aufträge** ".

3.  Sie können den Status des Wiederherstellungsauftrags verfolgen. Navigieren Sie zu der Seite **Volumes** auf dem Gerät.

4.  Sie können jetzt neue wiederhergestellte Volume in der Liste der Volumes auf dem Gerät anzeigen. Beachten Sie, dass die Wiederherstellung auf demselben Volume erfolgt. Tiered-Volume wird als Ebenen und lokalen Volumes als lokales Volume wiederhergestellt.

5.  Wenn das Volume auf die Liste der Datenträger angezeigt wird, ist das Volume zur Verfügung.  ISCSI-Initiator-Server aktualisiert die Liste der Ziele iSCSI-Initiator-Eigenschaftenfenster.  Ein neues Ziel enthält den wiederhergestellte Volume-Namen erscheinen in der Statusspalte als "inaktiv".

6.  Wählen Sie das Ziel, und klicken Sie auf **Verbinden**.   Nachdem der Initiator an das Ziel verbunden ist, sollte den Status **verbunden**ändern. 

7.  Im Fenster **Datenträger** werden bereitgestellten Volumes angezeigt, wie in der folgenden Abbildung dargestellt. Maustaste ermittelten Volumes (klicken Sie auf Name des Laufwerks), und klicken Sie dann auf **Online**.

> [AZURE.IMPORTANT] Wenn Sie versuchen, ein Volume oder eine Freigabe aus einer Sicherung wiederherstellen festgelegt, schlägt die Auftrag ein Ziel-Volume oder Freigabe im Portal erstellt werden. Es ist wichtig, dass dieses Ziel-Volume löschen oder im Portal freigeben zu zukünftige Probleme aus diesem Element.

## <a name="item-level-recovery-ilr"></a>Wiederherstellung auf Elementebene (ILR)

Diese Version enthält die Wiederherstellung auf Elementebene (ILR) auf einem virtuellen StorSimple-Gerät als Dateiserver konfiguriert. Die Funktion ermöglicht Ihnen detaillierte Recovery von Dateien und Ordnern aus einer Cloud-Sicherung aller Freigaben auf dem Gerät StorSimple. Benutzer können neue Backups mithilfe eines Self-service-Modells Dateien abrufen.

Jede Aktie hat einen *.backups* -Ordner, der die aktuellsten Backups enthält. Benutzer kann auf die gewünschte Sicherung navigieren relevanten Dateien und Ordner aus der Sicherungskopie kopieren und wiederherstellen. Dadurch werden Aufrufe von Administratoren für Dateien aus Sicherungskopien wiederherstellen.

1.  Bei ILR können Sie Backups über Windows Explorer anzeigen. Klicken Sie auf bestimmte Freigabe der Sicherung anzeigen möchten. Sie sehen einen *.backups* Ordner erstellt die Freigabe, in dem die Sicherungskopien gespeichert. Erweitern Sie zum Anzeigen der Backups *.backups* . Der Ordner wird die Explosionsansicht der gesamten backup-Hierarchie angezeigt. Diese Ansicht wird bei Bedarf erstellt und dauert nur wenige Sekunden.

    Letzten 5 Backups werden so angezeigt und können verwendet werden, um eine Wiederherstellung auf Elementebene. Die 5 letzten Backups sind standardmäßig geplanten und manuellen Backups.

    
    -   **Geplante Backups** als &lt;Namen&gt;DailySchedule JJJJMMTT-SSMMSS UTC.

    -   **Manuelle Backups** als Ad-Hoc-JJJJMMTT-SSMMSS-UTC.
    
        ![](./media/storsimple-ova-restore/image14.png)

1.  Identifizieren Sie die Sicherung mit der neuesten Version der gelöschten Datei. Wenn der Ordnername UTC Timestamp in allen oben genannten Fällen enthält, ist die Zeit, an der der Ordner erstellt wurde, das Gerät Uhrzeit des Starts die Sicherung. Verwendet den Ordner Zeitstempel zum Auffinden und Identifizieren der Backups.

2.  Suchen Sie den Ordner oder die Datei, die in der Sicherung wiederherzustellen, die Sie im vorherigen Schritt ermittelt werden soll. Beachten Sie, dass nur die Dateien oder Ordner für Berechtigungen anzeigen können. Wenn Sie nicht auf bestimmte Dateien oder Ordner zugreifen können, müssen Sie einen Freigabe-Administrator wenden, die mit Windows Explorer die Freigabeberechtigungen bearbeiten und ermöglichen den Zugriff auf bestimmte Dateien oder Ordner. Es wird empfohlen werden Administrator Freigabe einer Benutzergruppe anstelle eines einzelnen Benutzers.

3.  Kopieren Sie die Datei oder den Ordner mit der entsprechenden Freigabe auf Ihrem Dateiserver StorSimple.

![Video_icon](./media/storsimple-ova-restore/video_icon.png) **Video verfügbar**

Das Video zeigt, wie Sie Freigaben erstellen, Freigaben sichern und Wiederherstellen von Daten auf ein virtuelles Array StorSimple.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Verwalten der virtuellen StorSimple-Array mit der lokalen Webbenutzeroberfläche](storsimple-ova-web-ui-admin.md).
