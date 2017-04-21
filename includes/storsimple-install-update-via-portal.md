<!--author=SharS last changed: 01/15/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>Update 1.2 von klassischen Azure-Portal installieren

1. Wählen Sie auf der Seite StorSimple Ihr Gerät aus. Navigieren Sie zum **Geräte** > **Wartung**.

2. Klicken Sie am unteren Rand der Seite auf **Updates überprüfen**. Ein Auftrag wird erstellt, um nach verfügbaren Updates zu suchen. Sie werden benachrichtigt, wenn der Auftrag erfolgreich abgeschlossen wurde.

3. Im Abschnitt **Softwareupdates** auf derselben Seite sehen Sie, dass neue Softwareupdates verfügbar sind. Wir empfehlen, den Release Notes zu lesen, bevor Sie Update 1.2 auf Ihrem Gerät installieren.

    ![Softwareupdates installieren](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)

4. Klicken Sie am unteren Rand der Seite auf **Updates installieren**.

5. Sie werden zur Bestätigung aufgefordert. Klicken Sie auf **OK**.

6. Ein Dialogfeld **Updates installieren** wird angezeigt. Das Gerät sollte in diesem Dialogfeld aufgeführten Kontrollen erfüllen. Diese Schritte wurden vor dem Update. Wählen Sie **ich verstehe den obigen Vorschrift und möchte mein Gerät aktualisieren**. Klicken Sie auf Kontrollkästchen.

    ![Bestätigung](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)

7. Eine Reihe von Vorabprüfungen automatisch startet. Dazu gehören:

    - **Zustand überprüft** , um sicherzustellen, dass beide Gerätecontroller fehlerfrei und online sind.
    
    - Überprüfen Sie, ob die Hardwarekomponenten auf dem StorSimple-Gerät fehlerfrei **Hardwarestatus Komponente überprüft** .
    
    - **Daten 0** zu überprüfen, ob Daten 0 auf Ihrem Gerät aktiviert ist. Wenn diese Schnittstelle nicht aktiviert ist, müssen Sie aktivieren und wiederholen.
    
    - **Daten 2 und Daten 3 überprüft** um sicherzustellen, dass Daten 2 und Daten 3 Netzwerkschnittstellen nicht aktiviert sind. Wenn diese Schnittstellen aktiviert sind, müssen Sie deaktivieren und dann versuchen, das Gerät zu aktualisieren. Diese Überprüfung erfolgt nur, wenn Sie von einem Gerät unter GA aktualisieren. Geräte mit Version 0.1, 0.2 oder 0,3 ist dazu nicht erforderlich.
    
    - Eine Version vor Update 1 Gerät **Gateways überprüfen** . Diese Überprüfung erfolgt auf alle Gerät 1 vor dem Update unter aber nicht auf den Geräten, die einen Gateway für eine Netzwerkschnittstelle Daten 0 konfiguriert.
 
    Update 1.2 wird nur angewendet, wenn der oben stehenden vor dem Update-Tests erfolgreich abgeschlossen werden. Sie werden benachrichtigt, dass vor dem Update überprüft werden.
  
    ![Überprüfen Sie vor Benachrichtigung](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)

    Nachfolgend ein Beispiel, in dem vor der Aktualisierung Fehler bei der Überprüfung. Sie müssen überprüfen, ob die Gerätecontroller fehlerfrei und online sind. Sie müssen auch den Zustand der Hardwarekomponenten überprüfen. In diesem Beispiel brauchen Controller 0 und Controller 1 Komponenten Aufmerksamkeit. Sie müssen Microsoft-Support wenden, wenn Sie diese Probleme selbst beheben können nicht.

     ![Fehler beim Überprüfen der vor](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

    > [AZURE.NOTE] Nach dem Update 1.2 auf Ihrem Gerät StorSimple anwenden, werden Daten 2 und Daten 3 überprüft und die Gateway-Überprüfung nicht mehr für zukünftige Updates erforderlich. Vor Kontrollen werden erforderlich.


8. Nach der erfolgreichen Überprüfung vor dem upgrade wird ein Auftrag erstellt. Sie werden benachrichtigt, wenn die Aktualisierung erfolgreich erstellt wurde.
 
    ![Aktualisieren von Arbeitsplätzen](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)

    Das Update wird dann auf das Gerät angewendet werden.
 
9. Klicken Sie zum Überwachen des Fortschritts der Auftrag **Auftrag anzeigen**. Auf der Seite **Projekte** Aktualisierungsfortschritt angezeigt. 

    ![Aktualisieren des Projektstatus](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)

10. Die Aktualisierung wird einige Stunden dauern. Sie können die Details des Auftrags jederzeit anzeigen.

    ![Job-Details aktualisieren](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)

11. Nachdem der Auftrag abgeschlossen ist, navigieren Sie zur Seite **Wartung** und scrollen Sie **Softwareupdates**.

12. Stellen Sie sicher, dass Ihr Gerät **StorSimple 8000 Serie Update 1.2 (6.3.9600.17584)**ausgeführt wird. **Datum letzte Aktualisierung** sollte ebenfalls geändert werden.

    ![WARTUNGSSEITE](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)

13. Sie sehen jetzt, dass Wartung Modus gibt. Diese Updates sind unterbrechungsfrei Updates, die Gerät Ausfallzeiten führen und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden. Führen Sie die Schritte [Wartung-Modus installieren](storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple) , installieren Sie diese Updates über Windows PowerShell für StorSimple.

> [AZURE.NOTE] In bestimmten Fällen kann Wartung Modus gibt die Meldung von 24 Stunden nach der Wartung Modus Updates erfolgreich auf dem Gerät angewendet werden, angezeigt.  


