<properties 
   pageTitle="Aktivieren oder Deaktivieren von Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert, wie ein neues StorSimple-Gerät aktivieren, aktivieren Sie ein Gerät, das Herunterfahren oder Stromausfall und laufenden Gerät ausschalten."
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
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>Aktivieren Sie oder Deaktivieren der StorSimple-Gerät 

## <a name="overview"></a>Übersicht

Beendet Microsoft Azure StorSimple-Gerät ist nicht als Teil des normalen Systembetriebs erforderlich. Allerdings müssen Sie ein neues Gerät oder ein Gerät, das stillgelegt werden aktivieren. Im Allgemeinen wird ein Herunterfahren erforderlich in der müssen fehlerhafte Hardware ersetzen, physische Einheit, übertragen oder ein Gerät aus. In diesem Lernprogramm beschreibt das erforderliche Verfahren zum Einschalten und Herunterfahren Geräts StorSimple Szenarien.

In der folgenden Tabelle listet verschiedene Szenarien für einschalten und Herunterfahren des Geräts StorSimple und Links zu den entsprechenden Verfahren.

|Szenario|Themen|
|:-------|:---------------|
|Ein neues Gerät einschalten|[Ein neues Gerät einschalten](#turn-on-a-new-device)<ul><li>[Neues Gerät mit dem primären Gehäuse](#new-device-with-primary-enclosure-only)</li><li>[Neues Gerät mit EBOD-Gehäuse](#new-device-with-ebod-enclosure)</li></ul>|
|Aktivieren Sie ein Gerät nach dem Herunterfahren|[Aktivieren Sie ein Gerät nach dem Herunterfahren](#turn-on-a-device-after-shutdown)<ul><li>[Gerät mit dem primären Gehäuse](#device-with-primary-enclosure-only)</li><li>[Mit EBOD-Gehäuse](#device-with-ebod-enclosure)</li></ul>|
|Aktivieren Sie ein Gerät nach einem Stromausfall|[Aktivieren Sie ein Gerät nach einem Stromausfall](#turn-on-a-device-after-a-power-loss)<ul><li>[Gerät mit dem primären Gehäuse](#8100)</li><li>[Mit EBOD-Gehäuse](#8600)</li></ul>|
|Aktivieren Sie ein Gerät nach der primären Einheit und EBOD Verbindung unterbrochen|[Aktivieren Sie ein Gerät nach dem primären und EBOD Gehäuse Verbindung unterbrochen](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Ausgeführte Gerät Herunterfahren|[Ausgeführte Gerät ausschalten](#turn-off-a-running-device)<ul><li>[Gerät mit dem primären Gehäuse](#8100a)</li><li>[Mit EBOD-Gehäuse](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Ein neues Gerät einschalten

Die Schritte für ein StorSimple-Gerät zum ersten Mal einschalten variieren je nachdem, ob das Gerät eine 8100 oder 8600-Modell. 8100 hat eine primäre Gehäuse der 8600 ist ein Dual-Gehäuse mit einem primären Gehäuse und EBOD-Gehäuse Die einzelnen Schritte für beide Modelle werden in den folgenden Abschnitten behandelt.

- [Neues Gerät mit dem primären Gehäuse](#new-device-with-primary-enclosure-only)

- [Neues Gerät mit EBOD-Gehäuse](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Neues Gerät mit dem primären Gehäuse

Das Modell StorSimple 8100 ist ein Gehäuse. Das Gerät enthält redundante Stromversorgung und Kühlung Module (PCM). Sowohl PCM müssen installiert und an verschiedene Stromquellen hohen Verfügbarkeit sichergestellt.

Führen Sie die folgenden Schritte aus, um Ihr Gerät für Power cable.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Vollständige Geräteinstallation und Kabel Informationen finden Sie [Ihr StorSimple 8100-Gerät installieren](storsimple-8100-hardware-installation.md). Stellen Sie sicher, dass genau der Anleitung befolgen.

### <a name="new-device-with-ebod-enclosure"></a>Neues Gerät mit EBOD-Gehäuse

StorSimple 8600-Modell verfügt über eine primäre Einheit und EBOD-Gehäuse. Dies erfordert die Einheiten für Serial Attached SCSI (SAS)-Konnektivität und Leistung miteinander verbunden werden.

Beim Einrichten dieses Gerät zum ersten Mal für SAS-Kabel zuerst führen Sie aus, und führen Sie die Schritte für Power Verkabelung.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Vollständige Geräteinstallation und Kabel Informationen finden Sie [Ihr StorSimple 8600-Gerät installieren](storsimple-8600-hardware-installation.md). Stellen Sie sicher, dass genau der Anleitung befolgen.

## <a name="turn-on-a-device-after-shutdown"></a>Aktivieren Sie ein Gerät nach dem Herunterfahren

Die Schritte für ein StorSimple Gerät einschalten, nachdem er heruntergefahren sind je nach, ob das Gerät eine 8100 oder 8600-Modell. 8100 hat eine primäre Gehäuse der 8600 ist ein Dual-Gehäuse mit einem primären Gehäuse und EBOD-Gehäuse

- [Gerät mit dem primären Gehäuse](#device-with-primary-enclosure-only)

- [Mit EBOD-Gehäuse](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Gerät mit dem primären Gehäuse

Heruntergefahren wurde gehen Sie folgendermaßen vor, um ein StorSimple Gerät mit einem primären Gehäuse und keine EBOD-Gehäuse.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>So aktivieren Sie ein Gerät mit einem primären Gehäuse

1. Sicherstellen Sie, dass der Netzschalter auf sowohl und Kühlmodule (PCM) ausgeschaltet sind. Wenn die Switches nicht ausgeschaltet, aus spiegeln und die LEDs erlöschen warten.

2. Schalten Sie das Gerät mit dem Netzschalter auf beide PCM in die Position. Das Gerät sollte zu aktivieren.

3. Überprüfen Sie um sicherzustellen, dass das Gerät vollständig auf Folgendes:

    1. OK LEDs auf beide PCM-Module sind grün.

    2. Status-LEDs auf beiden Domänencontrollern sind grün.

    3. Die blaue LED an einem der Controller blinkt gibt an, dass der Controller aktiv ist.

    Alle diese Bedingung nicht erfüllt, ist das Gerät nicht fehlerfrei. Bitte [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Mit EBOD-Gehäuse

Heruntergefahren wurde gehen Sie folgendermaßen vor, um ein StorSimple Gerät eine primäre Einheit mit einem EBOD-Gehäuse. Führen Sie jeden Schritt nacheinander genau beschrieben. Andernfalls kann zu Datenverlust führen.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>So aktivieren Sie ein Gerät mit einem primären und einem EBOD-Gehäuse

1. Stellen Sie sicher, dass die primäre Einheit EBOD-Gehäuse verbunden ist. Weitere Informationen finden Sie unter [StorSimple-8600-Gerät installieren](storsimple-8600-hardware-installation.md).

2. Stellen Sie sicher, dass die Leistungsfähigkeit und Kühlmodule (PCM) EBOD und primäre Gehäuse ausgeschaltet werden. Wenn die Switches nicht ausgeschaltet, aus spiegeln und die LEDs erlöschen warten.

3. Zuerst auf EBOD-Einheit mit dem Netzschalter auf beide PCM in die Position. PCM-LEDs sollte grün. Ein grüner EBOD Controller LED auf diesem Gerät gibt an, dass das EBOD-Gehäuse auf.

4. Aktivieren Sie die primäre Einheit mit dem Netzschalter auf beide PCM in die Position. Das gesamte System sollte jetzt auf.

5. Überprüfen Sie die SAS-LEDs grün, wird sichergestellt, dass die Verbindung zwischen EBOD-Gehäuse und die primäre Einheit ist.

## <a name="turn-on-a-device-after-a-power-loss"></a>Aktivieren Sie ein Gerät nach einem Stromausfall

Ein StorSimple kann ein Stromausfall oder Unterbrechung Herunterfahren. Stromausfall kann auf einem der Netzteile oder beide Netzteile auftreten. Die Wiederherstellungsschritte unterscheiden sich je nachdem, ob das Gerät ein 8100 oder 8600-Modell. 8100 hat eine primäre Gehäuse der 8600 ist ein Dual-Gehäuse mit einem primären Gehäuse und EBOD-Gehäuse Dieser Abschnitt beschreibt das Wiederherstellungsverfahren für jedes Szenario.

- [Gerät mit dem primären Gehäuse](#8100)

- [Mit EBOD-Gehäuse](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Gerät mit dem primären Gehäuse<a name="8100">

Das System kann Stromausfall eines der Netzteile Normalbetrieb fortfahren. Jedoch zur Sicherstellung hoher Verfügbarkeit des Geräts wieder an die Stromversorgung an die Stromversorgung so bald wie möglich.

Liegt ein Stromausfall oder Unterbrechung der Stromversorgung beider Netzteile, schaltet das System ordnungsgemäß kontrolliert und. Wenn die Stromversorgung wiederhergestellt ist, wird das System automatisch einschalten.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Mit EBOD-Gehäuse<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Stromausfall ein angeben

Das System kann Stromausfall eines der Netzteile auf der primären Einheit oder EBOD-Gehäuse Normalbetrieb fortfahren. Um hohe Verfügbarkeit des Geräts sicherzustellen, bitte wieder an die Stromversorgung an die Stromversorgung so bald wie möglich.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Stromausfall beide Netzteile auf primären und EBOD-Gehäuse

Liegt ein Stromausfall oder Unterbrechung der Stromversorgung beider Netzteile, EBOD-Gehäuse wird sofort heruntergefahren und geordnete kontrollierte und die primäre Einheit heruntergefahren wird. Nach Wiederherstellung der Stromzufuhr wird das Gerät automatisch gestartet.

Wenn manuell ausgeschaltet wird, gehen Sie folgendermaßen vor, macht das System wiederherstellen.

1. Aktivieren Sie die EBOD-Gehäuse.

2. Nach EBOD-Gehäuse auf, aktivieren Sie die primäre Einheit.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Stromausfall beide Netzteile auf EBOD-Gehäuse

Wenn Sie Ihre einrichten, müssen Sie sicherstellen, dass der EBOD nie allein eine separate PDU angeschlossen ist. Wenn die EBOD und die primäre Einheit gleichzeitig fehlschlagen, wird das System wiederhergestellt.

Nur EBOD-Gehäuse auf beide Netzteile fehlschlägt, wird das System nicht automatisch wiederhergestellt werden. Die folgenden Schritte auf dem System und in einen fehlerfreien Zustand wiederherstellen:

1. Wenn die primäre Einheit aktiviert ist, schalten Sie Power und Kühlmodule (PCM).

2. Warten Sie ein paar Minuten heruntergefahren.

3. Aktivieren Sie die EBOD-Gehäuse.

4. Nach EBOD-Gehäuse auf, aktivieren Sie die primäre Einheit.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Aktivieren Sie ein Gerät nach dem primären und EBOD Gehäuse Verbindung unterbrochen

Wenn die Verbindung zwischen dem standby Controller und entsprechenden EBOD-Controller ist weiterhin das Gerät. Wenn die Verbindung zwischen der aktiven System-Controller und den entsprechenden EBOD-Controller verloren geht, Failover tritt und Gerät sollte weiterhin normal.

Serial Attached SCSI (SAS) Kabel entfernt werden oder die Verbindung zwischen EBOD-Gehäuse und die primäre Einheit unterbrochen, wird das Gerät nicht mehr. An dieser Stelle gehen.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>So schalten Sie das Gerät nach Verbindung

1. Auf die Rückseite des Geräts zugreifen.

2. Wenn das SAS-Kabel zwischen EBOD-Gehäuse und die primäre Einheit beschädigt ist, werden alle SAS Lane LEDs auf EBOD-Einheit.

3. Stromversorgung und Kühlung Module (PCM) EBOD-Gehäuse und die primäre Einheit heruntergefahren.

4. Warten Sie, bis alle LEDs auf der Rückseite der Einheiten deaktivieren.

5. Setzen Sie die SAS-Kabel wieder ein und stellen sicher, dass eine gute zwischen EBOD-Gehäuse und die primäre Einheit Verbindung.

6. Zuerst auf die EBOD-Gehäuse mit beiden PCM-Schalter auf ON.

7. Sicherstellen Sie, dass das EBOD-Gehäuse auf überprüfen, dass die grüne LED auf.

8. Aktivieren Sie die primäre Einheit.

9. Sicherstellen Sie, dass die primäre Einheit zu überprüfen, dass die Domänencontroller grüne LED auf.

10. Überprüfen Sie, ob EBOD Gehäuse Verbindung mit der primären Einheit überprüfen ist, ob alle SAS Lane LEDs (4 pro EBOD-Controller) sind.

>[AZURE.IMPORTANT] Wenn die SAS-Kabel defekt oder die Verbindung zwischen EBOD-Gehäuse und die primäre Einheit nicht gut, ist Wenn Sie das System einschalten, gehen sie in den Wiederherstellungsmodus wechselt. Bitte [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , wenn dies.

## <a name="turn-off-a-running-device"></a>Ausgeführte Gerät ausschalten

Ein StorSimple ausführen müssen heruntergefahren werden, verschoben wird, außer Betrieb genommen oder eine fehlerhafte Komponente ersetzt werden. Die Schritte sind je nach, ob das Gerät StorSimple ein 8100 oder 8600-Modell. 8100 hat eine primäre Gehäuse der 8600 ist ein Dual-Gehäuse mit einem primären Gehäuse und EBOD-Gehäuse In diesem Abschnitt werden die Schritte zum Ausführen ein Herunterfahren.

- [Gerät mit dem primären Gehäuse](#8100a)

- [Mit EBOD-Gehäuse](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Gerät mit dem primären Gehäuse<a name="8100a"> 

Um das Gerät ordnungsgemäß und kontrolliert heruntergefahren, möglich es durch klassischen Azure-Portal oder über Windows PowerShell für StorSimple. 

>[AZURE.IMPORTANT] Schalten Sie nicht ausgeführten Gerät mit dem Netzschalter auf der Rückseite des Geräts.
>
>Vor dem Herunterfahren des Geräts sicherstellen Sie, dass das Gerät alle fehlerfrei sind. Navigieren Sie in der Azure-Verwaltungsportal zu **Geräten** > **Wartung** > **Hardware-Status**und Status aller Komponenten grün. Dies gilt nur für ein gesundes System. Wenn das System heruntergefahren wird eine fehlerhafte Komponente ersetzen, sehen (Rot) oder Status der jeweiligen Komponente des **Hardwarestatus**(gelb) beschädigt.

Nachdem Sie Windows PowerShell für StorSimple oder klassischen Azure-Portal zugreifen, folgen Sie den Schritten [ein StorSimple Gerät Herunterfahren](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Mit EBOD-Gehäuse<a name="8600a">

>[AZURE.IMPORTANT] Sicherstellen Sie bevor Sie die primäre Einheit und EBOD Gehäuse herunterfahren, dass das Gerät alle fehlerfrei sind. Navigieren Sie in der Azure-Verwaltungsportal zu **Geräten** > **Wartung** > **Hardwarestatus**, und stellen Sie sicher, dass alle Komponenten fehlerfrei sind.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Ein Ausführen mit EBOD Herunterfahren

1. Führen Sie alle Schritte in [ein StorSimple Herunterfahren](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) für die primäre Einheit.

2. Nachdem die primäre Einheit heruntergefahren, Herunterfahren der EBOD aus Strom und Kühlung Modul (PCM) spiegeln.

3. Überprüfen der EBOD heruntergefahren wurde, überprüfen Sie alle LEDs auf der Rückseite des Gehäuses EBOD aus.

>[AZURE.NOTE] SAS-Kabel, mit denen die primäre Einheit EBOD-Gehäuse herstellen sollte nicht bis entfernt werden, nachdem das System heruntergefahren wird.

## <a name="next-steps"></a>Nächste Schritte

[Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) Sie Problemen beim Einschalten oder ein StorSimple Gerät herunterfahren.

