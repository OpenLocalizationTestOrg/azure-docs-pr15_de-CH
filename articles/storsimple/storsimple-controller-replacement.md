<properties 
   pageTitle="Ersetzen Sie einen StorSimple Gerätecontroller | Microsoft Azure"
   description="Erläutert das Entfernen und Ersetzen Sie eine oder beide controllermodule auf dem Gerät StorSimple."
   services="storsimple"
   documentationCenter=""
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

# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Controller-Modul auf dem StorSimple-Gerät ersetzen

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erläutert und eine oder beide controllermodule in ein StorSimple. Es behandelt auch die zugrunde liegende Logik für Ersatzszenarios Einzel- oder dual-Controller.

>[AZURE.NOTE] Vor Ersatz Controller, wird empfohlen, immer die Controller-Firmware auf die neueste Version aktualisieren.
>
>Um Schäden an Ihrem Gerät StorSimple werfen nicht aus dem Controller bis die LEDs der folgenden angezeigt werden:
>
>- Alle Lichter sind aus.
>- LED 3 ![Symbol für grünes Häkchen](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), und ![rotes Kreuz](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) blinken, und **0-LED und LED-7 sind**.

Die folgende Tabelle zeigt Ersatzszenarios unterstützten Controller.

|Anfrage|Ersatz-Szenario|Verfahren|
|:---|:-------------------|:-------------------|
|1|Ein Domänencontroller ist ausgefallen, der andere Controller aktiv ist.|[Ein Controller ersetzt](#replace-a-single-controller)die [Logik hinter einem Controller Ersatz](#single-controller-replacement-logic)sowie [Ersatz Schritte](#single-controller-replacement-steps)beschreibt.|
|2|Die Domänencontroller sind ausgefallen und ersetzt. Gehäuse, Laufwerke, and.disk Gehäuse sind.|[Dual-Controller-Ersatz](#replace-both-controllers), der [Logik mit zwei Controllern Ersatz](#dual-controller-replacement-logic)sowie [Ersatz Schritte](#dual-controller-replacement-steps)beschrieben. |
|3|Controller desselben Geräts oder von anderen Geräten ausgetauscht werden. Gehäuse, Laufwerken und Laufwerksgehäusen sind unbeschädigt.|Ein Steckplatz Konflikt Warnung erscheint.|
|4|Ein Controller fehlt und der andere Controller ausfällt.|[Dual-Controller-Ersatz](#replace-both-controllers), der [Logik mit zwei Controllern Ersatz](#dual-controller-replacement-logic)sowie [Ersatz Schritte](#dual-controller-replacement-steps)beschrieben.|
|5|Einer oder beide Domänencontroller sind ausgefallen. Das Gerät kann nicht über die serielle Konsole oder Windows PowerShell-Remoting zugreifen werden.|[Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) Verfahren eine manuelle Steuerung.|
|6|Die Controller verfügen über einen anderen Build-Version kann:<ul><li>Controller haben eine andere Version.</li><li>Controller haben eine andere Firmware-Version.</li></ul>|Controller-Softwareversionen unterschiedliche, Ersatz-Logik erkennt und aktualisiert die Software auf dem Ersatzcontroller.<br><br>Wenn die Controller-Firmware-Versionen unterscheiden und die alte Firmwareversion **nicht** automatisch aktualisiert wird eine Warnung im klassischen Azure-Portal angezeigt. Sie suchen nach Updates und Firmwareupdates installieren.</br></br>Wenn die Controller-Firmware-Versionen unterscheiden sich die alte Firmwareversion automatisch aktualisiert wird und Ersatz Controllerlogik erkannt und nach dem Start des Controllers die Firmware wird automatisch aktualisiert.|

Sie müssen eine Controller-Modul entfernen Fehler. Einer oder beide controllermodule können Fehlschlagen einer Einzel-Controller ersetzt oder dual-Controller-Ersatz kommen. Verfahren für den Austausch und die Logik finden Sie hier:

- [Ersetzen Sie einen controller](#replace-a-single-controller)
- [Ersetzen Sie beide Controller](#replace-both-controllers)
- [Einen Controller entfernen](#remove-a-controller)
- [Fügen Sie einen controller](#insert-a-controller)
- [Identifizieren des aktiven Controllers auf Ihrem Gerät](#identify-the-active-controller-on-your-device)

>[AZURE.IMPORTANT] Entfernen und Austauschen von einem Domänencontroller die Sicherheitshinweise [StorSimple Hardware-Komponente](storsimple-hardware-component-replacement.md)ersetzen.

## <a name="replace-a-single-controller"></a>Ersetzen Sie einen controller

Wenn eine der beiden Controllern auf dem Gerät Microsoft Azure StorSimple ist fehlgeschlagen, defekt ist oder fehlt, müssen Sie einen Controller ersetzen. 

### <a name="single-controller-replacement-logic"></a>Ein Controller ersetzt Logik

In einem Controller Ersatz sollten Sie zuerst den fehlgeschlagenen Controller entfernen. (Verbleibende Controller im Gerät ist active Controller.) Wenn Sie Domänencontroller ersetzen einfügen folgende Aktionen ausgeführt:

1. Ersatzcontroller beginnt sofort mit dem StorSimple-Gerät.

2. Ein Snapshot der virtuellen Festplatte (VHD) für den aktiven Controller wird auf dem Ersatz-Domänencontroller kopiert.

3. Der Snapshot wird so geändert, dass diese VHD Ersatzcontroller beginnt als Standby-Domänencontroller erkennbar wird.

4. Wenn die Änderungen abgeschlossen sind, beginnt der Ersatzcontroller als Standby-Domänencontroller.

5. Wenn beide Controller ausgeführt werden, geht der Cluster online.

### <a name="single-controller-replacement-steps"></a>Ein Controller ersetzt Schritte

Gehen Sie bei Ausfall eines Geräts Microsoft Azure StorSimple Controller. (Die anderen aktiv sind und ausgeführt werden. [Wenn beide Controller ausfällt oder Fehlfunktionen, zwei Controller Ersatz Schritten](#dual-controller-replacement-steps)fort.)

>[AZURE.NOTE] Es dauert 30 bis 45 Minuten für die Domänencontroller neu starten und das Austauschverfahren durch einzelne Domänencontroller vollständig wiederherstellen. Die Gesamtzeit für das gesamte Verfahren ist einschließlich der Kabel anschließen 2 Stunden.

#### <a name="to-remove-a-single-failed-controller-module"></a>Fehler beim Entfernen einer Controller-Modul

1. In der Azure-Verwaltungsportal zum StorSimple Manager Service, klicken Sie auf die Registerkarte **Geräte** und klicken Sie dann auf den Namen des Geräts, das Sie überwachen möchten.

2. Gehen Sie zu **Verwaltung > Hardwarestatus**. Der Status der Controller 0 oder Controller 1 sollte Rot, einen Fehler anzeigt.

    >[AZURE.NOTE] Fehlerhaften Domänencontroller in einem Controller Ersatz ist immer ein Standby-Controller.

3. Verwenden Sie Abbildung 1 und in der folgenden Tabelle zu fehlerhaften Controller-Modul.  

    ![Rückwandplatine Gerät primäre Einheit Module](./media/storsimple-controller-replacement/IC740994.png)

    **Abbildung 1** Rückseite StorSimple-Gerät

  	|Bezeichnung|Beschreibung|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Controller 0|
  	|4|Domänencontroller 1|

4. Entfernen Sie auf dem fehlgeschlagenen Controller alle angeschlossenen Kabel von den Datenports. Bei Verwendung von 8600 Modell auch entfernen Sie die SAS-Kabel, die den Controller mit EBOD-Controller verbunden.

5. Folgen Sie den Schritten zu fehlerhaften Controller [einen Controller zu entfernen](#remove-a-controller) . 

6. Installieren Sie die Factory Ersatz im selben Steckplatz aus dem fehlgeschlagene Controller entfernt wurde. Dadurch wird ein Controller ersetzt Logik. Weitere Informationen finden Sie in [einem Controller Ersatz Logik](#single-controller-replacement-logic).

7. Schließen Sie die Kabel, während ein Controller ersetzt Logik im Hintergrund abläuft. Achten Sie darauf, alle Kabel genau so verbinden, dass sie vor dem Austausch verbunden.

8. Überprüfen Sie nach dem Neustart des Controllers **Controllerstatus** und der **Cluster-Status** im klassischen Azure-Portal, um sicherzustellen, dass der Controller in einen fehlerfreien Zustand und im standby-Modus.

>[AZURE.NOTE] Wenn Sie das Gerät über die serielle Konsole überwachen, sehen Sie mehrere Neustarts während der Controller in das Austauschverfahren wiederhergestellt wird. Wenn Menü der seriellen Konsole angezeigt wird, wissen Sie, dass der Austausch abgeschlossen ist. Erscheint das Menü nicht innerhalb von zwei Stunden ab der Controller ersetzt, wenden Sie sich an [Microsoft Support Services](storsimple-contact-microsoft-support.md).

## <a name="replace-both-controllers"></a>Ersetzen Sie beide Controller

Bei beiden Controllern auf dem Gerät Microsoft Azure StorSimple haben, fehlerhaft sind oder fehlen, müssen Sie beide Controller ersetzen. 

### <a name="dual-controller-replacement-logic"></a>Zwei Controller Ersatz Logik

In dual-Controller-Ersatz zuerst beide fehlerhaften Controller und fügen Sie Ersatz. Wenn zwei Ersatz-Controller eingefügt werden, werden folgende Aktionen ausgeführt:

1. Ersatzcontroller in Steckplatz 0 prüft Folgendes:
 
   1. Ist aktuelle Version der Firmware und Software verwenden?

   2. Es ist ein Teil des Clusters?

   3. Peer-Domänencontrollern und es gruppiert?
                            
    Wenn der Vererbungshierarchie zutrifft, sucht der Controller die neueste tägliche Sicherung ( **NonDOMstorage** auf Laufwerk S). Der Controller kopiert des aktuellsten Snapshots der virtuellen Festplatte aus der Sicherung.

2. Der Controller in Steckplatz 0 verwendet den Snapshot selbst Abbild.

3. In der Zwischenzeit wartet Controller in Steckplatz 1 Controller 0 das Imaging und starten.

4. Nach dem Start von Controller 0 erkennt Domänencontroller 1 Clusters Controller 0 erstellt ein Controller ersetzt Logik auslöst. Weitere Informationen finden Sie in [einem Controller Ersatz Logik](#single-controller-replacement-logic).

5. Danach beide Controller ausgeführt, und der Cluster online geschaltet.

>[AZURE.IMPORTANT] Nach einem Austausch mit zwei Controllern nach StorSimple-Gerät konfiguriert ist, muss eine manuelle Sicherung des Geräts vornehmen. Tägliche Device Configuration Backups werden nach 24 Stunden nicht erst ausgelöst. Arbeiten Sie mit [Microsoft Support](storsimple-contact-microsoft-support.md) zu einer manuellen Sicherung Ihres Geräts.

### <a name="dual-controller-replacement-steps"></a>Zwei Controller Ersatz Schritte

Dieser Workflow ist erforderlich, wenn der Controller in das Microsoft Azure StorSimple Gerät ausgefallen. Kann dies in einem Datencenter in dem Kühlsystem funktioniert und folglich beide Controller fehl, innerhalb kurzer Zeit. Je nachdem, ob das Gerät StorSimple ein- oder ausgeschaltet ist und ob eine 8600 arbeiten oder ein Modell 8100 ist eine andere Reihe von Schritten erforderlich.

>[AZURE.IMPORTANT] Es dauert 45 Minuten, 1 Stunde für den Controller zu starten und ein Verfahren mit zwei Controllern vollständig wiederherstellen. Die Gesamtzeit für das gesamte Verfahren ist einschließlich der Kabel anschließen ca. 2,5 Stunden.

#### <a name="to-replace-both-controller-modules"></a>Beide controllermodule ersetzen

1. Wenn das Gerät ausgeschaltet ist, diesen Schritt überspringen und mit dem nächsten Schritt fort. Wenn das Gerät eingeschaltet ist, schalten Sie das Gerät.
                                        
    1. Bei Verwendung ein Modells 8600 aus der primären Einheit zuerst, und schalten Sie EBOD-Gehäuse.

    2. Warten Sie, bis das Gerät vollständig heruntergefahren wurde. Die LEDs auf der Rückseite des Geräts werden.

2. Entfernen Sie alle Netzwerkkabel, die Daten-Ports verbunden sind. Bei Verwendung von 8600 Modell auch entfernen Sie die SAS-Kabel, die primäre Einheit EBOD Gehäuse verbinden.

3. Entfernen Sie beide Controller vom StorSimple-Gerät. Weitere Informationen finden Sie in [einem Domänencontroller zu entfernen](#remove-a-controller).

4. Fügen Sie Factory Ersatz für Controller 0 zuerst, und fügen Sie die Controller 1. Weitere Informationen finden Sie unter [Einfügen von einem Domänencontroller](#insert-a-controller). Dadurch wird die Logik Austausch mit zwei Controllern. Weitere Informationen finden Sie unter [zwei Controller Ersatz Logik](#dual-controller-replacement-logic).

5. Schließen Sie die Kabel wieder während der Ersetzung Controllerlogik im Hintergrund abläuft. Achten Sie darauf, alle Kabel genau so verbinden, dass sie vor dem Austausch verbunden. Siehe die detaillierte Anleitung für das Modell an Ihr Gerät Abschnitt des [Geräts StorSimple 8100](storsimple-8100-hardware-installation.md) oder [installieren das Gerät StorSimple 8600](storsimple-8600-hardware-installation.md).

6. Schalten Sie das Gerät StorSimple. Wenn Sie ein Modell 8600 verwenden:

    1. Stellen Sie sicher, dass EBOD-Gehäuse zunächst aktiviert ist.

    2. Warten Sie, bis das Gehäuse EBOD ausgeführt wird.

    3. Aktivieren Sie die primäre Einheit.

    4. Wenn der erste Domänencontroller gestartet wurde und fehlerfrei ist, läuft das System.

    >[AZURE.NOTE] Wenn Sie das Gerät über die serielle Konsole überwachen, sehen Sie mehrere Neustarts während der Controller in das Austauschverfahren wiederhergestellt wird. Wenn Menü der seriellen Konsole angezeigt wird, wissen Sie, dass der Austausch abgeschlossen ist. Wenn 2,5 Stunden ab der Controller ersetzt nicht im Menü angezeigt wird, wenden Sie sich an [Microsoft Support](storsimple-contact-microsoft-support.md).

## <a name="remove-a-controller"></a>Einen Controller entfernen

Gehen Sie zu einem fehlerhaften Controller-Modul vom Gerät StorSimple.

>[AZURE.NOTE] Die folgenden Illustrationen sind für Controller 0. Domänencontroller 1 würde diese storniert werden.

#### <a name="to-remove-a-controller-module"></a>Controller-Modul entfernen

1. Fassen Sie die Verriegelung zwischen Daumen und Zeigefinger.

2. Drücken Sie Daumen und Zeigefinger zusammen freizugebende Controller Verriegelung.

    ![Controller-Latch freigeben](./media/storsimple-controller-replacement/IC741047.png)

    **Abbildung 2** Controller-Latch freigeben

2. Verwenden Sie den Riegel als Handle auf den Controller aus dem Gehäuse ziehen.

    ![Schieben die Controller aus dem Gehäuse](./media/storsimple-controller-replacement/IC741048.png)

    **Abbildung 3** Schieben den Controller aus dem Gehäuse

## <a name="insert-a-controller"></a>Fügen Sie einen controller

Gehen Sie nach dem Entfernen eines fehlerhaften Moduls aus StorSimple-Gerät Factory bereitgestellte Controllermodul installieren.

#### <a name="to-install-a-controller-module"></a>Controller-Modul installieren

1. Überprüfen Sie, ob an Schnittstelle Anschlüsse Schäden. Installieren Sie das Modul die Steckerstifte beschädigt oder verbogen werden.

2. Schieben Sie das Controller-Modul in das Gehäuse, während des Riegels vollständig freigegeben. 

    ![Controller in Gehäuse schieben](./media/storsimple-controller-replacement/IC741053.png)

    **Abbildung 4** Controller in das Gehäuse schieben

3. Beginnen Sie mit dem Controllermodul eingefügt schließen die Verriegelung weiterhin Controller-Modul in das Gehäuse schieben. Die Verriegelung rastet den Controller zu führen.

    ![Controller-Verriegelung schließen](./media/storsimple-controller-replacement/IC741054.png)

    **Abbildung 5** Schließen die Controller-Verriegelung

4. Sie sind fertig, wenn die Verriegelung einrastet. **OK** -LED sollte jetzt auf.  

    >[AZURE.NOTE] Es dauert bis zu 5 Minuten für den Controller und die LED aktivieren.

5. Gehen Sie zum Überprüfen, ob der Austausch erfolgreich im klassischen Azure-Portal **Geräte** > **Wartung** > **Hardwarestatus**und Controller 0 und 1 Controller fehlerfrei sind (Status ist Grün).

## <a name="identify-the-active-controller-on-your-device"></a>Identifizieren des aktiven Controllers auf Ihrem Gerät

Es gibt viele Situationen, wie die erstmalige Registrierung oder Controller Austausch, die aktive Domänencontrollers auf einem StorSimple-Gerät erforderlich. Aktive Domänencontroller verarbeitet alle Datenträger Firmware und Netzwerke Operationen. Eines der folgenden Verfahren können Sie den aktiven Controller identifizieren:

- [Verwenden der Azure-Verwaltungsportal zu aktiven Domänencontroller](#use-the-azure-classic-portal-to-identify-the-active-controller)

- [Mit dem aktiven Controller bestimmt Windows PowerShell für StorSimple](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)

- [Das physische Gerät aktiven Controller identifizieren](#check-the-physical-device-to-identify-the-active-controller)

Jedes dieser Verfahren wird weiter beschrieben.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>Verwenden der Azure-Verwaltungsportal zu aktiven Domänencontroller

Navigieren Sie in der Azure-Verwaltungsportal zu **Geräten** > **Wartung**und wählen Sie **den Abschnitt** . Hier können Sie überprüfen, welche Domänencontroller aktiv ist.

![Aktive Domänencontroller in Azure-Verwaltungsportal zu identifizieren](./media/storsimple-controller-replacement/IC752072.png)

**Abbildung 6** Klassische Azure-Portal mit aktiven Domänencontroller

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Mit dem aktiven Controller bestimmt Windows PowerShell für StorSimple

Beim Zugriff auf das Gerät über die serielle Konsole präsentiert ein Banner angezeigt. Banner-Nachricht enthält grundlegende Informationen wie Modell, Name, installierte Softwareversion und Status des Controllers, auf die Sie zugreifen. Die folgende Abbildung zeigt ein Beispiel für ein Banner angezeigt:

![Serielle Banner Nachricht](./media/storsimple-controller-replacement/IC741098.png)

**Abbildung 7** Nachricht mit Controller 0 als aktive Banner

Banner-Nachricht können Sie bestimmen, ob der Controller, mit dem Sie verbunden wird.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Das physische Gerät aktiven Controller identifizieren

Aktiven Controller auf Ihrem Gerät identifizieren, suchen das blaue geführt über Daten 5 Port auf der Geräterückseite der primären Einheit

Wenn diese LED blinkt, die Domänencontroller aktiv ist und des andere Controllers im standby-Modus. Verwenden Sie das folgende Diagramm und Tabelle als Hilfsmittel.

![Gerät primäre Einheit Backplane dataports](./media/storsimple-controller-replacement/IC741055.png)

**Abbildung 8** Rückseite des primären Gehäuses Überwachung LEDs mit Daten-ports

|Bezeichnung|Beschreibung|
|:----|:----------|
|1 bis 6|0 – Netzwerk-5 ports|
|7|Blaue LED|


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [StorSimple Hardware-Komponente ersetzt](storsimple-hardware-component-replacement.md).
