<properties 
   pageTitle="Batterie auf einem Gerät StorSimple | Microsoft Azure"
   description="Beschreibt das Entfernen und ersetzen das Modul backup-Batterie auf dem StorSimple-Gerät verwalten."
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

# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Ersetzen Sie das Modul backup-Batterie auf dem StorSimple-Gerät

## <a name="overview"></a>Übersicht

Die primäre Einheit Strom und Kühlung Modul (PCM) auf Ihrem Gerät Microsoft Azure StorSimple hat einen zusätzlichen Akku. Dieses Pack versorgt, damit das Gerät StorSimple bei Stromausfall mit der primären Einheit Daten speichern kann. Dieser Akku wird als *backup-Batterie-Modul*bezeichnet. Backup-Batterie-Modul ist nur für die primäre Einheit Geräts StorSimple (EBOD-Gehäuse enthält keine backup-Batterie-Modul). 

In diesem Lernprogramm wird erläutert, wie Sie:

- Backup-Batterie Modul 
- Installieren einer neuen backup-Batterie
- Backup-Batterie-Modul verwalten

>[AZURE.IMPORTANT] Entfernen und Austauschen eines Moduls Sicherungsbatterie die Sicherheitshinweise in der [Einführung StorSimple Hardware-Komponente ersetzt](storsimple-hardware-component-replacement.md).

## <a name="remove-the-backup-battery-module"></a>Backup-Batterie Modul

Backup-Batterie-Modul für das StorSimple-Gerät ist ein Ort austauschbare. Vor der Installation in das PCM sollte das Batteriemodul originalverpackt gespeichert werden. Durchführen Sie so entfernen Sie die Sicherungsbatterie

#### <a name="to-remove-the-backup-battery-module"></a>Backup-Batterie-Modul entfernen

1. Wechseln Sie in der Azure-Verwaltungsportal zu **Geräten** > **Wartung** > **Hardware-Status**. Suchen Sie unter **Freigegebene Komponenten**den Status des Akkus.

2. Identifizieren Sie PCM der Akku fehlgeschlagen ist. Abbildung 1 zeigt die Rückseite des Geräts StorSimple.

    ![Rückwandplatine Gerät primäre Einheit Module](./media/storsimple-battery-replacement/IC740994.png)

    **Abbildung 1** Der primäre Gerät zeigt PCM und Controller-Module

  	|Bezeichnung|Beschreibung|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Controller 0|
  	|4|Domänencontroller 1|

    Wie Nummer 3 in Abbildung 2 dargestellt, sollten Überwachung Indikator auf PCM 0 entspricht **Batteriefehler** LED leuchten.

    ![Rückwandplatine Gerät PCM Überwachung Anzeige-LEDs](./media/storsimple-battery-replacement/IC740992.png)

    **Abbildung 2** Der PCM zeigt Überwachung LED

  	|Bezeichnung|Beschreibung|
  	|:---|:-----------|
  	|1|AC-Stromausfall|
  	|2|Fehler beim Lüfter|
  	|3|Batterie defekt|
  	|4|PCM-OK|
  	|5|DC Stromausfall|
  	|6|Akku fehlerfrei|

3. Um PCM durch einen fehlerhaften Akku zu entfernen, folgen Sie den Schritten [PCM entfernen](storsimple-power-cooling-module-replacement.md#remove-a-pcm).

4. Entfernt PCM heben Sie an drehen Sie den Batterie Griff nach oben, wie in der folgenden Abbildung und ziehen sie die Batterie entfernen.

    ![PCM entfernen Akku](./media/storsimple-battery-replacement/IC741019.png)

    **Abbildung 3** Entfernen des Akkus aus PCM

5. Stellen Sie das Modul in der Verpackung Ort austauschbare Einheit.

6. Das defekte Gerät an Microsoft Service und Behandlung zurück

## <a name="install-a-new-backup-battery-module"></a>Installieren einer neuen backup-Batterie

Die folgenden Schritte Akku Ersatzmodul in PCM in der primären Einheit StorSimple-Gerät installieren.

#### <a name="to-install-the-battery-module"></a>Akkumodul installiert

1. Platzieren Sie backup-Batterie-Modul in der richtigen Ausrichtung im PCM.

2. Drücken Sie Modulhandle Akku zu den Anschluss einzusetzen.

3. Ersetzen Sie PCM in der primären Einheit anhand der Richtlinien in [eine und Kühlmodul Geräts StorSimple ersetzen](storsimple-power-cooling-module-replacement.md).

4. Nach Abschluss des Austausches **Geräte**gehen > **Wartung** > **Hardwarestatus** im klassischen Azure-Portal. Überprüfen Sie den Status des Akkus um sicherzustellen, dass die Installation erfolgreich war. Ein grüner Status gibt an, dass der Akku fehlerfrei ist.

## <a name="maintain-the-backup-battery-module"></a>Backup-Batterie-Modul verwalten

In Ihrem Gerät StorSimple versorgt Sicherungsbatterie Modul der Controller während einer Stromversorgung verloren gehen. Es kann das StorSimple-Gerät speichern wichtige Daten vor kontrolliert heruntergefahren. Mit zwei voll aufgeladenen Batterien in das PCM kann das System zwei aufeinander folgenden Ereignissen behandeln.

In der Azure-Verwaltungsportal gibt den **Hardwarestatus** auf der Seite **Wartung** die Batterie defekt oder End-of-Life nähert. Der Akkustatus wird durch **in PCM 0** oder **Akku PCM 1** unter **Freigegebene Komponenten**angezeigt. Diese Seite zeigt einen Status **DEGRADED** für End-of-Life nähert und **fehlgeschlagen** für End-of-Life erreicht. 

>[AZURE.NOTE] Die Batterie kann **Fehler** melden, wenn einfach berechnet werden muss.
 
Status **DEGRADED** angezeigt wird, sollten die folgende Vorgehensweise:

- Das System kann haben kürzlich Stromausfall oder Batterien derzeit regelmäßig gewartet. Beachten Sie das System 12 Stunden vor.

    - Wenn der Status nach 12 Stunden kontinuierlicher Verbindung mit Wechselstrom mit Controllern und PCM ausgeführt noch **beschädigt** ist, muss die Batterie ersetzt werden. Nehmen Sie für ein Ersatzmodul Sicherungsbatterie [Support von Microsoft](storsimple-contact-microsoft-support.md) .

    - Wird der Status OK nach 12 Stunden betriebsbereit ist und nur benötigt eine Wartung kostenlos.

- Wurde ein zugeordnete Verlust von Wechselstrom und PCM eingeschaltet und ans Stromnetz angeschlossen ist, sollte die Batterie ersetzt werden. [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) ein Ersatzmodul backup-Batterie bestellen.

>[AZURE.IMPORTANT] Entsorgen der fehlerhaften Akku nach nationalen oder regionalen Vorschriften. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [StorSimple Hardware-Komponente ersetzt](storsimple-hardware-component-replacement.md).
