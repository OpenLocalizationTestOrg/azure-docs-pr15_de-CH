<properties
   pageTitle="StorSimple Gerätecontroller verwalten | Microsoft Azure"
   description="Informationen Sie zum Beenden, starten, Herunterfahren oder Zurücksetzen der Gerätecontroller StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="manage-your-storsimple-device-controllers"></a>Verwalten Sie Ihre Gerätecontroller StorSimple

## <a name="overview"></a>Übersicht

In diesem Lernprogramm beschreibt die unterschiedlichen Vorgänge, die auf Ihren Domänencontrollern StorSimple Gerät ausgeführt werden können. Controller in Ihr Gerät StorSimple sind redundant (Peer) Controller in einer Aktiv / Passiv-Konfiguration. Nur ein Domänencontroller zu einem bestimmten Zeitpunkt aktiv ist und alle Datenträger und Netzwerkbetrieb verarbeitet. Der Controller ist im passiven Modus. Ausfall des aktiven Controllers aktiv passive Controller automatisch.

Dieses Lernprogramm enthält schrittweise Anleitung Gerätecontroller verwalten die:

- **Abschnitt der Seite **Verwaltung** der StorSimple Manager-Dienst**
- Windows PowerShell für StorSimple.

Wir empfehlen, Gerätecontroller über StorSimple Manager-Dienst verwaltet. Wenn eine Aktion nur mithilfe von Windows PowerShell für StorSimple ausgeführt werden kann, wird das Lernprogramm dient.

Nach dem Lesen dieses Lernprogramm können, Sie:

- Ein StorSimple Gerätecontroller Herunterfahren oder neu starten
- Fahren Sie StorSimple-Gerät
- Das StorSimple-Gerät auf Standardwerte zurücksetzen


## <a name="restart-or-shut-down-a-single-controller"></a>Einen Controller Herunterfahren oder neu starten

Ein Neustart bzw. Herunterfahren muss nicht als Teil des normalen Systembetriebs. Vorgänge zum Herunterfahren für ein einzelnes Gerätecontroller sind nur in Fällen, in denen eine Hardwarekomponente Gerät ersetzt werden muss. Neustart eines Controllers auch in einer Situation müssen Leistung durch eine fehlerhafte oder übermäßiger Speicherverbrauch betroffen ist. Sie müssen auch Ersatz erfolgreich Controller einen Controller Neustart, möchten Sie aktivieren und Testen Sie den Controller ersetzten.

Ein Neustart ist nicht auf angeschlossenen Initiatoren, vorausgesetzt der passive Controller verfügbar. Passiver Controller ist nicht verfügbar oder ausgeschaltet off und Neustart des aktiven Controllers Unterbrechung und Ausfallzeiten führen.

> [AZURE.IMPORTANT]

> - **Ausgeführten Domänencontroller sollte nie physisch entfernt werden, wie dies der Redundanz und ein erhöhtes Risiko von Ausfallzeiten führen würde.**

> - Das folgende Verfahren gilt nur für das physische Gerät StorSimple. Informationen zum Starten, stoppen und Neustarten des virtuellen Geräts finden Sie unter [Arbeiten mit virtuellen Gerät](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).

Neu starten oder Herunterfahren Gerätecontroller mit klassischen Azure-Portal die StorSimple Manager oder Windows PowerShell für StorSimple

Gehen Sie zum Verwalten der Gerätecontroller auf klassischen Azure-Portal.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Neu starten oder Herunterfahren eines Domänencontrollers Verwaltungsportal

1. Navigieren Sie zu **Geräte > Wartung**.

1. Zur **Hardware-Status** , und überprüfen Sie der Status der Controller auf Ihrem Gerät **fehlerfrei**.

    ![Überprüfen Sie, ob StorSimple Gerätecontroller fehlerfrei sind](./media/storsimple-manage-device-controller/IC766017.png)

1. Klicken Sie am Ende der Seite **Wartung** auf **Controller verwalten**.

    ![StorSimple Gerätecontroller verwalten](./media/storsimple-manage-device-controller/IC766018.png)</br>

    >[AZURE.NOTE] Wenn **Domänencontroller verwalten**nicht angezeigt wird, müssen Sie Updates installieren. Weitere Informationen finden Sie unter [StorSimple-Gerät zu aktualisieren](storsimple-update-device.md).

1. Klicken Sie im Dialogfeld **Einstellungen ändern** folgendermaßen Sie vor:
    1. Wählen Sie aus der Dropdownliste **Wählen Sie Controller** den Controller, den Sie verwalten möchten. Die Optionen sind Controller 0 und Controller 1. Diese Domänencontroller werden auch als aktiv oder Passiv bezeichnet.

        >[AZURE.NOTE] Ein Domänencontroller kann nicht verwaltet werden, ist nicht verfügbar oder deaktiviert deaktiviert und erscheint nicht in der Dropdown Liste.

    2. Wählen Sie aus der Dropdownliste **Auswählen** **Domänencontroller neu starten** oder **Herunterfahren Controller**.

        ![StorSimple Gerät Passiv Domänencontroller neu starten](./media/storsimple-manage-device-controller/IC766020.png)
    3. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-manage-device-controller/IC740895.png).

Dies wird neu starten oder Herunterfahren des Controllers. In der folgenden Tabelle werden die Details entsprechend der Auswahl was im Dialogfeld **Ändern Einstellungen** vorgenommene zusammengefasst.  


|Auswahl #|Wenn Sie möchten.|Dies geschieht.|
|---|---|---|
|1.|Starten Sie passive Domänencontroller neu.|Neustart des Controllers ein Auftrag erstellt werden und Sie werden benachrichtigt, nachdem das Projekt erfolgreich erstellt wurde. Den Neustart wird gestartet. Überwachen des Neustarts auf **Service > Dashboard > Protokolle anzeigen** und Filterung von Parametern für den Dienst.|
|2.|Starten Sie aktive Domänencontroller neu.|Wird die folgende Warnung angezeigt: "Wenn die aktiven Domänencontroller neu starten, das Gerät fehl über passive Controller. Möchten Sie fortfahren?" </br>Möchten Sie diesen Vorgang fortsetzen, die nächsten Schritte werden mit den passiven Controller neu starten (Siehe Auswahl 1).|
|3.|Fahren Sie den passiven Controller.|Wird die folgende Meldung angezeigt: "nach dem Herunterfahren Sie müssen drücken Sie den Betriebsschalter auf dem Domänencontroller aktivieren. Sind Sie sicher, dass dieser Domänencontroller heruntergefahren werden soll?" </br>Möchten Sie diesen Vorgang fortsetzen, die nächsten Schritte werden mit den passiven Controller neu starten (Siehe Auswahl 1).|
|4.|Aktive Domänencontroller heruntergefahren.|Wird die folgende Meldung angezeigt: "nach dem Herunterfahren Sie müssen drücken Sie den Betriebsschalter auf dem Domänencontroller aktivieren. Sind Sie sicher, dass dieser Domänencontroller heruntergefahren werden soll?" </br>Möchten Sie diesen Vorgang fortsetzen, die nächsten Schritte werden mit den passiven Controller neu starten (Siehe Auswahl 1).|


#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Um einen Domänencontroller in Windows PowerShell für StorSimple Herunterfahren oder neu starten
Die folgenden Schritte zum Herunterfahren oder Neustarten einen Controller auf Ihrem Gerät StorSimple klassischen Azure-Portal.


1. Zugreifen Sie das Gerät mithilfe der seriellen Konsole oder eine Telnet-Sitzung von einem Remotecomputer. Verbindung mit Controller 0 oder Controller 1 Schritte in [Kitten Verbindung zu Gerät serielle Konsole verwenden](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

1. Wählen Sie im Menü seriellen Konsole Option 1 **Vollzugriff einloggen**.

1. Nachricht Banner Notieren Sie (Controller 0 oder 1 Controller) mit Controller und ob es das aktive oder passive (standby) Controller.
    - Zum Herunterfahren von eines Controllers aufgefordert werden, geben Sie Folgendes ein:

        `Stop-HcsController`

        Dadurch wird der Domänencontroller heruntergefahren mit der Sie verbunden sind. Wenn Sie aktiven Domänencontroller beenden, wird es über passiven Controller schlägt vor dem Herunterfahren.
    - Neustart einen Controller aufgefordert werden, geben Sie Folgendes ein:

        `Restart-HcsController`

        Dies startet den Controller mit der Sie verbunden sind. Wenn die aktiven Domänencontroller neu starten, wird es zum passiven Controller vor dem Neustart Failover.


## <a name="shut-down-a-storsimple-device"></a>Fahren Sie StorSimple-Gerät

Dieser Abschnitt erläutert, wie eine laufende oder ein StorSimple Gerät von einem Remotecomputer herunterfahren. Ein Gerät ist die Gerätecontroller Herunterfahren deaktiviert. Ein Gerät Herunterfahren erfolgt, wenn das Gerät physisch verschoben oder außer Betrieb genommen.

> [AZURE.IMPORTANT] Bevor Sie das Gerät Herunterfahren, Überprüfen der Komponenten. Navigieren Sie zu **Geräte > Verwaltung > Hardwarestatus** und überprüfen Sie der LED-Status aller Komponenten grün. Eine gesunde Gerät haben einen grünen Status. Wenn das Gerät heruntergefahren ist eine fehlerhafte Komponente ersetzen, sehen Sie (Rot) oder einen herabgesetzten (gelb) für die entsprechenden Komponenten.

#### <a name="to-shut-down-a-storsimple-device"></a>Ein StorSimple Herunterfahren

1. Gehen Sie [neu starten oder Herunterfahren einen Controller](#restart-or-shut-down-a-single-controller) zu den passiven Controller auf Ihrem Gerät Herunterfahren vor. Sie können diesen Vorgang im klassischen Azure-Portal oder in Windows PowerShell für StorSimple ausführen.
2. Wiederholen Sie die obige Schritte aktiven Domänencontroller heruntergefahren.
3. Jetzt müssen sich auf der Rückseite des Geräts. Nach zwei Controller vollständig heruntergefahren sind, sollte der Status-LEDs auf den Domänencontrollern rot blinken. Möchten Sie das Gerät vollständig zu diesem Zeitpunkt deaktivieren, kippen Sie Netzschalter auf Stromversorgung und Kühlung Module (PCM) aus. Dadurch sollte das Gerät deaktiviert.


<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Das Gerät auf die werkseitigen Standardeinstellungen zurückgesetzt

> [AZURE.IMPORTANT] Möchten Sie das Gerät auf die werkseitigen Standardeinstellungen zurückzusetzen, wenden Sie sich an Microsoft Support. Das folgende Verfahren sollte nur in Verbindung mit Microsoft Support verwendet werden.

Dieses Verfahren beschreibt das Geräts Microsoft Azure StorSimple StorSimple über Windows PowerShell werkseitigen Standardeinstellungen zurückgesetzt.
Zurücksetzen eines Geräts entfernt alle Daten und Einstellungen standardmäßig den gesamten Cluster.

Die folgenden Schritte das Microsoft Azure StorSimple-Gerät werkseitigen Standardeinstellungen zurückzusetzen:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Standardeinstellungen für StorSimple in Windows PowerShell das Gerät zurücksetzen

1. Zugriff auf die Geräte über die serielle Konsole. Prüfen Sie Banner-Nachrichten sicherstellen, dass die Verbindung mit dem aktiven Controller.

1. Wählen Sie im Menü seriellen Konsole Option 1 **Vollzugriff einloggen**.

1. Aufgefordert werden Geben Sie den folgenden Befehl den gesamten Cluster entfernen alle Daten, Metadaten und Controller-Einstellungen zurücksetzen:

    `Reset-HcsFactoryDefault`

    Um stattdessen einen Controller zurückzusetzen, verwenden Sie das [Reset-HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) -Cmdlet mit dem `-scope` Parameter.)

    Das System wird mehrmals neu gestartet. Sie werden benachrichtigt, wenn das Zurücksetzen erfolgreich abgeschlossen wurde. Je nach System dauert es 45 bis 60 Minuten für die 8100 Geräte und 60 bis 90 Minuten 8600, diesen Vorgang abzuschließen.

    > [AZURE.TIP]

    > - Wenn Sie Update 1.2 oder früher verwenden die `–SkipFirmwareVersionCheck` Parameter die Firmware versionsüberprüfung überspringen (andernfalls sehen Sie einen Übereinstimmungsfehler Firmware: Funktion kann nicht durch einen Konflikt in die Firmware-Versionen fortgesetzt. ).

    > - Das Rücksetzverfahren Factory möglicherweise StorSimple Geräte, das Update 1 oder 1.1 in das Portal der Behörde und Austausch erfolgreich ein oder zwei Controller (mit Ersatz-Controller, die Software vor dem Update 1 geliefert wurden) durchgeführt. Dies geschieht beim Zurücksetzen der Factory Image überprüft das Vorhandensein vor 1 Updates eine SHA1 auf dem Domänencontroller, der nicht vorhanden ist. Wenn Sie, dass diese Factory Fehler zurücksetzen sehen, wenden Sie sich an Microsoft Support bei den nächsten Schritten. Dieses Problem wird nicht mit Ersetzung angezeigt, die mit Update 1 oder höher ausgeliefert wurden.


## <a name="questions-and-answers-about-managing-device-controllers"></a>Fragen und Antworten zum Gerätecontroller verwalten

In diesem Abschnitt haben wir zusammengefasst einige häufig gestellten Fragen zur Verwaltung von StorSimple Gerätecontroller.

**Q.** Was geschieht, wenn der Controller auf meinem Gerät fehlerfrei und aktiviert sind auf und ich neu starten oder Herunterfahren des aktiven Controllers?

**A.** Controller auf Ihrem Gerät sind gesunde und deaktiviert, Sie werden zur Bestätigung aufgefordert. Sie können:

- **Aktive Domänencontroller neu starten** , werden Sie benachrichtigt, dass einen aktiven Domänencontroller neu Gerät Failover zum passiven Controller führt. Der Controller wird neu gestartet.

- **Beendet einen aktiven Domänencontroller** – Sie werden benachrichtigt, dass eine aktive Domänencontroller heruntergefahren Ausfallzeiten führen. Sie benötigen auch drücken Sie den Betriebsschalter auf dem Gerät auf dem Domänencontroller aktivieren.

**Q.** Was geschieht, wenn der passive Controller auf dem Gerät nicht verfügbar oder ausgeschaltet ist und ich neu starten oder Herunterfahren des aktiven Controllers?

**A.** Der passive Controller auf Ihrem Gerät nicht verfügbar oder ausgeschaltet off und möchten:

- **Aktive Domänencontroller neu starten** , werden Sie benachrichtigt, einer vorübergehenden Unterbrechung des Dienstes führen Sie den Vorgang fortsetzen, werden Sie zur Bestätigung aufgefordert.

- **Beendet einen aktiven Domänencontroller** – werden Sie benachrichtigt, dass Sie den Vorgang fortsetzen Ausfallzeiten führen und drücken Sie den Betriebsschalter auf eine oder beide Controller einschalten müssen. Sie werden zur Bestätigung aufgefordert.

**Q.** Beim Neustart oder Herunterfahren wird nicht ausgeführt?

**A.** Neustarten oder Herunterfahren eines Controllers fehlschlagen wenn:

- Geräte-Update wird ausgeführt.

- Ein Neustart wird bereits ausgeführt.

- Ein Controller Herunterfahren wird bereits ausgeführt.

**Q.** Wie können Sie herausfinden, ob ein Domänencontroller neu gestartet oder heruntergefahren wurde?

**A.** Überprüfen Sie den Status des Controllers auf der Seite Wartung. Controllerstatus zeigt, ob ein Domänencontroller neu gestartet oder heruntergefahren wurde. Außerdem enthält die Seite Alerts eine informative Warnung, wenn der Domänencontroller neu gestartet oder heruntergefahren wurde. Der Betrieb von Domänencontrollern Neustarten und Herunterfahren werden ebenfalls in der Protokolle aufgezeichnet. Weitere Informationen über Protokolle werden Sie [die Protokolle anzeigen](storsimple-service-dashboard.md#view-the-operations-logs).

**Q.** Gibt es keinen Einfluss auf die Ausgaben durch Controller-Failover?

**A.** TCP-Verbindungen zwischen Initiatoren und aktiven Domänencontroller durch Controller-Failover zurückgesetzt, aber wiederhergestellt, wenn passive Controller Betrieb setzt. Möglicherweise temporären (30 Sekunden) Pause in e/a-Aktivität zwischen Initiatoren und im Verlauf dieses Vorgangs.

**Q.** Wie kehre ich meine Domänencontroller auf nach wurde beendet und entfernt

**A.** Um einen Controller zu Service zurückzukehren, müssen in das Gehäuse unter [Controller-Modul auf dem StorSimple-Gerät ersetzen](storsimple-controller-replacement.md)einfügen.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Probleme mit Ihrem Gerät StorSimple, die nicht auftreten mithilfe der Verfahren in diesem Lernprogramm, [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md)lösen.

- Weitere Informationen zur Verwendung des StorSimple Manager-Dienstes finden Sie unter mit [der StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
