<properties 
   pageTitle="Ändern Sie den Gerätemodus StorSimple | Microsoft Azure"
   description="StorSimple gerätemodi beschrieben und erläutert, wie Windows PowerShell für StorSimple den Modus ändern."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>Ändern Sie den Gerätemodus StorSimple-Gerät

Dieser Artikel enthält eine kurze Beschreibung der verschiedenen Verkehrsträger in denen StorSimple-Gerät ausgeführt werden kann. Das StorSimple-Gerät funktioniert in drei Modi: Normal, Wartung und Recovery. 

Nach dem Lesen dieses Artikels werden Sie Folgendes wissen:

- Welche StorSimple Gerät sind
- Wie Sie herausfinden, welcher Modus StorSimple-Gerät ist
- Ändern von normalen Wartungsmodus und *umgekehrt*


Die oben aufgeführten Verwaltungsaufgaben können nur über die Windows PowerShell-Schnittstelle des Geräts StorSimple ausgeführt werden.

## <a name="about-storsimple-device-modes"></a>StorSimple Gerät Modi

Das Gerät StorSimple kann Normal, Wartung oder Wiederherstellung ausgeführt werden. Diese Modi werden unten kurz beschrieben.

### <a name="normal-mode"></a>Normalbetrieb

Dies wird als den normalen Betriebsmodus ein vollständig konfigurierten StorSimple definiert. Das Gerät sollte standardmäßig im normalen Modus.

### <a name="maintenance-mode"></a>Im Wartungsmodus

Manchmal müssen StorSimple-Gerät in den Wartungsmodus versetzt werden. Dieser Modus ermöglicht das Gerät warten und unterbrechungsfreie Installation, wie Festplatten-Firmware.

Sie können das System in den Wartungsmodus über Windows PowerShell StorSimple einfügen. In diesem Modus werden alle e/a-Anfragen angehalten. Auch werden Dienste wie RAM nichtflüchtigen Speicher (NVRAM) oder der Clusterdienst beendet. Beide Controller werden neu gestartet, wenn eingeben oder diesen Modus beenden. Beim Beenden des Wartungsmodus werden fortgesetzt, alle Dienste und gesund sein. Dies kann einige Minuten dauern.

>[AZURE.NOTE]**-Wartungsmodus wird nur auf einem funktionierenden Gerät unterstützt. Wird nicht auf einem Gerät mit mindestens der Controller nicht funktioniert unterstützt.**
</br>

### <a name="recovery-mode"></a>Wiederherstellungsmodus

Wiederherstellungsmodus kann als "Abgesicherter Modus mit Netzwerk-Unterstützung Windows" beschrieben. Wiederherstellungsmodus greift das Microsoft Support-Team und Diagnose ausführen kann. Das Hauptziel der Wiederherstellungsmodus ist die Systemprotokolle abrufen.

Ihr System in den Wiederherstellungsmodus wechselt wenden Sie Microsoft Support für weitere Schritte. Weitere Informationen finden Sie im [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md).

>[AZURE.NOTE] **Das Gerät kann nicht im Wiederherstellungsmodus versehen werden. Wenn das Gerät in einem fehlerhaften Zustand ist, versucht Wiederherstellungsmodus zu dem Gerät in einen Zustand in der Microsoft Support-Mitarbeiter es überprüft werden können.**

## <a name="determine-storsimple-device-mode"></a>Ermitteln von StorSimple-Modus

#### <a name="to-determine-the-current-device-mode"></a>Bestimmt den aktuellen Gerätemodus

1. Melden Sie sich an die serielle Konsole Gerät Schritte in [Kitten Verbindung zu Gerät serielle Konsole verwenden](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Betrachten Sie die Nachricht Banner im Menü seriellen Konsole des Geräts. Diese Meldung zeigt explizit an, ob das Gerät Wartung oder Wiederherstellung. Enthält die Nachricht keine bestimmten Informationen zum System-Modus, ist das Gerät im normalen Modus.

## <a name="change-the-storsimple-device-mode"></a>Ändern des StorSimple Modus 

StorSimple-Gerät setzen in den Wartungsmodus (im normalen Modus) Wartung oder Wartung-Modus installieren. Führen Sie die folgenden Verfahren oder beenden Sie den Wartungsmodus.

> [AZURE.IMPORTANT] Vor dem versetzen in den Wartungsmodus, überprüfen Sie beide Gerätecontroller fehlerfrei auf **Hardware-Status** auf der Seite **Wartung** im klassischen Azure-Portal. Wenn einer oder beide Controller nicht fehlerfrei sind, erhalten Sie Microsoft Support weiter. Weitere Informationen finden Sie im [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md).

#### <a name="to-enter-maintenance-mode"></a>Geben Sie im Wartungsmodus

1. Melden Sie sich an die serielle Konsole Gerät Schritte in [Kitten Verbindung zu Gerät serielle Konsole verwenden](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Wählen Sie im Menü seriellen Konsole Option 1 **Vollzugriff einloggen**. Wenn aufgefordert, geben Sie das **Administratorkennwort Gerät**. Das Standardkennwort lautet: `Password1`.

3. Geben Sie an der Befehlszeile 

    `Enter-HcsMaintenanceMode`

4. Sie sehen eine Warnmeldung Wartungsmodus unterbrochen werden alle e/a-Anfragen und trennt die Verbindung zum klassischen Azure-Portal, und Sie werden zur Bestätigung aufgefordert. Geben Sie **Y** Wartungsmodus eingeben.

5. Beide Controller werden neu gestartet. Wenn der Neustart abgeschlossen ist, wird eine weitere Meldung angezeigt, dass das Gerät im Wartungsmodus befindet.


#### <a name="to-exit-maintenance-mode"></a>Beenden des Wartungsmodus

1. Melden Sie sich an die serielle Konsole des Geräts an. Überprüfen der Banner-Nachricht, die das Gerät in den Wartungsmodus.

2. In der Befehlszeile eingeben:

    `Exit-HcsMaintenanceMode`

3. Eine Warnung und eine Meldung werden angezeigt. Geben Sie **Y** Wartungsmodus beenden.

4. Beide Controller werden neu gestartet. Wenn der Neustart abgeschlossen ist, erscheint eine weitere Meldung, dass das Gerät im normalen Modus ist.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie auf Ihrem Gerät StorSimple [Normal und Wartung-Modus installieren](storsimple-update-device.md) .

