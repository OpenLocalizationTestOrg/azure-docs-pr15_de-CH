<properties 
    pageTitle="Indikatoren StorSimple | Microsoft Azure" 
    description="Beschreibt die Leuchtdioden (LEDs) und akustischen Alarm zur Überwachung des Status von Gerät StorSimple."
    services="storsimple"
    documentationCenter="NA"
    authors="alkohli"
    manager="carmonm"
    editor="" />
 <tags 
    ms.service="storsimple"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="TBD"
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Verwenden Sie StorSimple Indikatoren für das Management   

## <a name="overview"></a>Übersicht

Das StorSimple-Gerät umfasst Leuchtdioden (LEDs) und Alarme, die Module und Gesamtstatus des Geräts StorSimple verwenden können. Die Indikatoren für die Begleitung finden auf den Hardwarekomponenten des primären Gehäuse des Geräts und EBOD-Gehäuse. Die Indikatoren für die Begleitung möglich LEDs oder akustische Alarmsignale.

Drei LED-Status den Status eines Moduls angegeben: Grün, Rot gelb oder rot orange blinkt grün.  

- Grüne LEDs darstellen einer fehlerfreien Betriebszustand.  
- Anzeige blinkt grün, Rot gelbe LEDs darstellen Vorhandensein nicht kritischen Zuständen, die Benutzereingriff erfordern.  
- Rot-Gelb-LEDs zeigen einen kritischen Fehler im Modul vorhanden.  

Der Rest dieses Artikels beschreibt die verschiedenen Überwachung Indikator-LEDs die Speicherorte auf dem StorSimple basierend auf den LED-Status und zugeordnete akustische Alarmsignale Gerätestatus.

## <a name="front-panel-indicator-leds"></a>Vorderseite-LED

Der Frontblende auch *Bedienfeld* oder *Ops Panel*zeigt aggregierten Status aller Module im System. Die Vorderseite ist identisch für primäre StorSimple und EBOD-Gehäuse und ist unten dargestellt.  

   ![Geräte-Vorderseite][1]
 
Die Vorderseite enthält die folgenden Indikatoren:  

1. Stummtaste
2. Betriebsanzeige-LED (Rot/Grün-Gelb)
3. Modul-Fehleranzeige LEUCHTET (Rot-Gelb/aus)
4. Logische Fehleranzeige LEUCHTET (Rot-Gelb/aus
5. Einheit-ID anzeigen  

Der Hauptunterschied zwischen der Vorderseite LEDs für das Gerät und den EBOD-Gehäuse ist die **System Einheit-ID** auf der LED-Anzeige angezeigt. Die Einheit-ID auf dem Gerät angezeigten ist **00**die Einheit-ID angezeigt EBOD-Gehäuse **01**. Dadurch können Sie schnell zwischen dem Gerät und dem EBOD-Gehäuse unterscheiden, wenn das Gerät eingeschaltet ist. Verwenden Sie das Gerät deaktiviert ist, die Angaben [ein neues Gerät einschalten](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) , EBOD Gehäuse des Geräts unterscheiden.  

## <a name="front-panel-led-status"></a>Vorderseite LED-status  

In der folgenden Tabelle den Status der LEDs auf der Vorderseite des Geräts oder EBOD-Gehäuse zu verwenden.  

|Stromversorgung | Modul-Fehler | Logische Fehler | Alarm | Status|
|-------------|---------------|-----------------|-------|-------|
|Rot-Gelb | AUSSCHALTEN     | AUSSCHALTEN | N/A | Wechselstrom unterbrochen, unter Strom oder Stromversorgung und Controller Module wurden entfernt.|
|Grün | AUF | AUF | N/A | OPS Panel einschalten (5 s) Zustand testen|
|Grün | AUSSCHALTEN | AUSSCHALTEN | N/A | Auf alle Funktionen gut|
|Grün | AUF |N/A | PCM-Fehler-LED, Lüfterfehler-LED | Verschulden PCM Lüfterfehler über oder unter Temperatur|
| Grün | AUF | N/A | Modul LEDs  | Jeder Controller-Modul-Fehler|
| Grün | AUF | N/A | N/A | Gehäuse Logik Fehler|
| Grün | Flash | N/A | Modulstatus-LED auf Controller-Modul. PCM-Fehler-LED, Lüfterfehler-LED | Unbekannte Controller installiert-Modul, I2C-Bus Fehler Modulfehler mit Controller wichtige Produkt Daten (VPD) |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Stromversorgung, Kühlung (PCM) Modul-LED   

Stromversorgung Kühlung Modul (PCM) LED finden auf der Geräterückseite der primären Einheit oder EBOD-Gehäuse jedes PCM. In diesem Thema wird erläutert, wie folgende LEDs mit der Überwachung des Status des Geräts StorSimple.  

- PCM-LEDs für die primäre Einheit
- PCM-LEDs für das EBOD-Gehäuse

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM-LEDs für die primäre Einheit  

StorSimple-Gerät hat ein 764W PCM-Modul mit einem zusätzlichen Akku. Die folgende Abbildung zeigt das LED-Display für das Gerät.  

   ![PCM-LEDs auf der primären Einheit][2]

LED-Legende:

1. AC-Stromausfall
2. Fehler beim Lüfter
3. Batterie defekt
4. PCM-OK
5. DC-Ausfall
6. Gute Akku  

Der Status des PCM ist der LED-Anzeige angezeigt. PCM-LED Gerätebereich verfügt über sechs LEDs. Vier diese LEDs zeigen den Status der Stromversorgung und der Lüfter. Die verbleibenden zwei LEDs zeigen den Status des Moduls Sicherungsbatterie PCM. In den folgenden Tabellen können Sie den Status der PCM ermitteln.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM-LED für Stromversorgung und Lüfter
| Status | PCM-OK (Grün) | AC-Fehler (gelb) | Lüfter-Fehler (gelb) | DC-Fehler (gelb) |
|--------|----------------|-----------------------|------------------|----------------------|
| Kein Wechselstrom (mit Gehäuse) | AUSSCHALTEN | AUSSCHALTEN | AUSSCHALTEN | AUSSCHALTEN|
| Kein Wechselstrom (diese PCM) | AUSSCHALTEN | AUF | AUSSCHALTEN | AUF |
| Wechselstrom vorhanden auf PCM - OK     | AUF | AUSSCHALTEN | AUSSCHALTEN | AUSSCHALTEN |
| PCM-Fehler (Fan Fail) | AUSSCHALTEN | AUSSCHALTEN | AUF | N/A |
| PCM-Fehler (über Überspannung gegenüber EVA) | AUSSCHALTEN | AUF | AUF | AUF |
| PCM (Lüfter außerhalb der Toleranz) | AUF | AUSSCHALTEN | AUSSCHALTEN | AUF |
| Standby-Modus | Blinkt | AUSSCHALTEN | AUSSCHALTEN | AUSSCHALTEN |
| PCM-Firmware-download | AUSSCHALTEN | Blinkt | Blinkt | Blinkt |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>PCM-LED für backup-Batterie  

| Status | Gute Akku (Grün) | Batteriefehler (gelb) |
|--------|----------------------|-----------------------|
| Akku nicht vorhanden | AUSSCHALTEN | AUSSCHALTEN |
| Akku vorhanden und geladen | AUF | AUSSCHALTEN |
| Entladung der Batterie aufgeladen oder Wartung | Blinkt | AUSSCHALTEN |
| "Soft" Batteriefehler (wiederhergestellt) | AUSSCHALTEN | Blinkt |
| "Harten" Batteriefehler (nicht erstattungsfähig) | AUSSCHALTEN | AUF |
| Akku unscharf | Blinkt | AUSSCHALTEN |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM-LEDs für das EBOD-Gehäuse  

EBOD-Gehäuse ist 580 w bei PCM und keine zusätzliche Batterie. PCM-Bereich für EBOD-Gehäuse ist Anzeige-LED für die Stromversorgung und den Lüfter. Die folgende Abbildung zeigt diese LEDs.

   ![PCM-LEDs auf der EBOD-Gehäuse][3] 
 
In der folgenden Tabelle können Sie den Status der PCM ermitteln.  

| Status | PCM-OK (Grün) | AC-Fehler (gelb) | Lüfter-Fehler (gelb) | DC-Fehler (gelb) |
|--------|---------------|------------------------|------------------|----------------------|
| Kein Wechselstrom (mit Gehäuse) | AUSSCHALTEN | AUSSCHALTEN | AUSSCHALTEN | AUSSCHALTEN |
| Kein Wechselstrom (diese PCM) | AUSSCHALTEN | AUF | AUSSCHALTEN | AUF |
| Wechselstrom vorhanden auf PCM-OK | AUF | AUSSCHALTEN | AUSSCHALTEN | AUSSCHALTEN |
| PCM-Fehler (Fan Fail) | AUSSCHALTEN | AUSSCHALTEN | AUF | X |
| PCM-Fehler (über EVA über aktuelle Überspannung | AUSSCHALTEN | AUF | AUF | AUF |
| PCM (Lüfter außerhalb der Toleranz) | AUF | AUSSCHALTEN | AUSSCHALTEN | AUF |
| Standby-Modell | Blinkt | AUSSCHALTEN | AUSSCHALTEN | AUSSCHALTEN |
| PCM-Firmware-download | AUSSCHALTEN | Blinkt | Blinkt | Blinkt |

## <a name="controller-module-indicator-leds"></a>Controller-Modul LED  

StorSimple-Gerät enthält LEDs für den primären Controller und EBOD Controller-Module.   

### <a name="monitoring-leds-for-the-primary-controller"></a>LEDs für den primären Domänencontroller überwachen
Die folgende Abbildung können Sie die LEDs auf dem primären Controller identifizieren. (Alle Komponenten aufgeführt um Orientierung zu erleichtern.)  

   ![Überwachen von LED - primäre Domänencontroller][4]
 
Verwenden Sie folgende Tabelle, um festzustellen, ob das Controllermodul richtig.  

### <a name="controller-indicator-leds"></a>Controller-LED  

| LED | Beschreibung                                                                            
|---- | ----------- |
| ID-LED (Blau) | Gibt an, dass das Modul identifiziert wird. Wenn auf einem Domänencontroller ausgeführten blaue LED blinkt, der Controller ist der aktive Controller und ist der Standby-Domänencontroller. Weitere Informationen finden Sie unter [Identifizieren der aktiven Controller auf Ihrem Gerät](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Fehler-LED (gelb) | Gibt einen Fehler des Controllers.        
| OK-LED (Grün) | Grün zeigt an, dass der Domänencontroller OK. Blinkt grün gibt einen Controller VPD Fehler. |
| SAS-Aktivität LED (Grün) | Grün zeigt eine Verbindung ohne aktuelle Aktivität. Blinkt grün zeigt an, dass die Verbindung laufende Aktivität verfügt. |
| Ethernet-Status-LEDs | Rechts zeigt Link-Netzwerk-Aktivität: (Grün) Verbindung aktiv (Grün blinkend) Netzwerkaktivität. Links gibt die Geschwindigkeit an: 1000 Mb/s (gelb), 100 Mbit/s (Grün) und (OFF) 10 Mb/s. Je Komponente könnte dieses Licht blinkt, selbst wenn die Netzwerkschnittstelle nicht aktiviert ist. |
| POST-LEDs | Gibt laufenden Boot an der Controller aktiviert ist. Falls StorSimple-Gerät starten, hilft diese LED Microsoft Support der Startvorgang identifizieren den Fehler. |

>[AZURE.IMPORTANT] 
Wenn die Fehler-LED leuchtet, ist ein Problem mit Controllermodul, das durch einen Neustart des Controllers behoben werden kann. Wenden Sie sich an Microsoft Support, Neustart des Controllers dieses Problem nicht behoben.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Überwachen der LEDs für EBOD (EBOD-Gehäuse)  

6 Gb/s SAS EBOD Controller jeweils LEDs, die den Status angeben, wie in der folgenden Abbildung dargestellt.  

  ![Überwachen von LED - EBOD-Gehäuse][5]

Verwenden Sie folgende Tabelle, um festzustellen, ob EBOD Controller-Modul ordnungsgemäß funktioniert.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD-Controller-Modul-LED  

|Status | Modul OK (Grün) | E/a-Modul-Fehler (gelb) | Host-Port-Aktivität (Grün) |
|-------|----------------------|-------------------------------|----------------------------|
| Controller-Modul OK | AUF | AUSSCHALTEN | - |
| Controller-Modul-Fehler | AUSSCHALTEN | AUF | - |
| Keine externen Host-Anschluss | - | - | AUSSCHALTEN |
| Anschluss für externen Host-keine Aktivität | - | - | AUF |
| Anschluss für externen Host - Aktivität | - | - | Blinkt |
| Controller-Modul Metadaten-Fehler | Blinkt | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Laufwerk-LED für die primäre Einheit und EBOD-Gehäuse

Das Gerät StorSimple hat Laufwerke in der primären Einheit und EBOD-Gehäuse befindet. Jedes Laufwerk enthält Überwachung Anzeige-LEDs in diesem Abschnitt beschrieben. 

Die Statusinformationen des Laufwerks wird für die Laufwerke durch ein grünes und ein rot orange LED auf der Vorderseite des Moduls Carrier Laufwerk bereitgestellt. Die folgende Abbildung zeigt diese LEDs.

  ![Laufwerk-LEDs][6]
 
Anhand der folgenden Tabelle den Status jeder Festplatte Feststellung der gesamten Vorderseite LED-Status auswirkt.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Laufwerk-LED für EBOD-Gehäuse  

| Status | Aktivität OK-LED (Grün) | Fehler-LED (Rot-gelb) | Zugeordnete Ops Bereich LED |
|-------|--------------------------|----------------------|-------------------------|
| Kein Laufwerk installiert | AUSSCHALTEN | AUSSCHALTEN | Keine |
| Laufwerk installiert und betriebsbereit | Blinken bei Aktivität ausschalten | X | Keine |
| Identität Gerätegruppe SCSI Enclosure Services (SES) | AUF | 1 Sekunde auf/1 Sekunde blinkt | Keine |
| SES Gerätegruppe Fehler bit | AUF | AUF | Logische Fehler (Rot) |
| Stromsteuerungsausfall circuit | AUSSCHALTEN | AUF | Modul-Fehler (Rot) |

## <a name="audible-alarms"></a>Akustische Alarmsignale  

Ein StorSimple enthält akustischen Alarm zugeordnete primäre Einheit und EBOD Gehäuse. Ein akustisches befindet sich auf der Vorderseite (auch bekannt als der Bereich Ops) beide Einheiten. Akustische Alarm gibt an, wann ein Fehlerzustand vorliegt. Folgendes werden den Alarm aktivieren:  

- Lüfterfehler oder Fehler
- Spannung außerhalb des Definitionsbereichs
- Über oder unter Temperatur
- Thermische Pufferüberlauf
- Systemfehler
- Logische Fehler
- Stromversorgungsfehler
- Entfernt ein Netzteil-Kühlmodul (PCM)  

Die folgende Tabelle beschreibt die verschiedenen Alarmzustand.  

### <a name="alarm-states"></a>Alarmzustand  

| Alarmzustand | Aktion | Mit mute Taste |
|------------|---------|---------------------------------|
| S0 | Standardmodus: im Hintergrund | Signalton zweimal |
| S1 | Störung: 1 Sekunde auf/1 Sekunde | Übergang zu S2 oder S3 (siehe Hinweise) |
| S2 | Erinnern Modus: zeitweise Signalton | Keine |
| S3 | Stumm-Modus: automatische | Keine |
| S4 | Kritische Störung: kontinuierliche Alarm | Nicht verfügbar: Ton aus nicht aktiviert |

> [AZURE.NOTE] 

>  - In Alarmzustand S1 Wenn Sie nicht stumm innerhalb von 2 Minuten drücken wechselt der Zustand automatisch S2 oder S3.  
>  - Alarmzustand S1 S4 zurück zu S0 nach den Fehlerzustand deaktiviert ist.  
>  - Kritischen Fehlerzustand S4 kann aus einem beliebigen Zustand eingegeben werden.  

Durch Drücken der Taste Ton aus auf die Ops können akustischen Alarm stumm geschaltet werden. Automatische Stummschalten wird nach zwei Minuten auftreten, wenn der Stummschalter nicht manuell betrieben wird. Wenn der Alarm ausgeschaltet ist, weiterhin mit kurze zeitweise Signaltöne aus, um anzugeben, dass ein Problem weiterhin besteht. Alarm wird im Hintergrund, wenn alle Probleme behoben werden.  

Die folgende Tabelle beschreibt die verschiedenen alarmbedingungen.  

### <a name="alarm-conditions"></a>Alarme  

| Status | Schweregrad | Alarm | OPS panel LED |
|--------|---------|--------|----------------|
| PCM-Alert-DC Stromausfall von einzelnen PCM | Fehler – kein Verlust der Redundanz | S1 | Modul-Fehler|
|PCM-Alert-DC Stromausfall von einzelnen PCM | Fehler – Verlust der Redundanz | S1 | Modul-Fehler |
| PCM Lüfter fehlgeschlagen | Fehler – Verlust der Redundanz | S1 | Modul-Fehler |
| SBB Modul PCM-Fehler gefunden | Fehler | S1 | Modul-Fehler |
| PCM entfernt | Konfigurationsfehler | Keine | Modul-Fehler |
| Gehäuse-Konfigurationsfehler | Kritische Fehler – | S1 | Modul-Fehler |
| Niedrige Temperatur-Warnung | Warnung | S1 | Modul-Fehler |
| Hoher Temperatur-Warnung | Warnung | S1 | Modul-Fehler |
| Über Temperatur | Kritische Fehler – | S1 | Modul-Fehler |
| I2C-Bus-Fehler | Fehler – Verlust der Redundanz | S1 | Modul-Fehler |
| OPS panel Kommunikationsfehler (I2C) | Kritische Fehler –     | S1 | Modul-Fehler |
| Controller-Fehler | Kritische Fehler – | S1 | Modul-Fehler |
| SBB Schnittstelle Modul-Fehler | Kritische Fehler – | S1 | Modul-Fehler |
| SBB Modulfehler Interface-keine funktionierende Module verbleibende | Kritische Fehler – | S4 | Modul-Fehler |
| Schnittstellenmodul SBB entfernt | Warnung | Keine | Modul-Fehler |
| Power Control Laufwerksfehler | Warnung – kein Leistungsverlust Laufwerk | S1 | Modul-Fehler |
| Power Control Laufwerksfehler | Fehler – wichtige; Laufwerk Stromausfall | S1 | Modul-Fehler |
| Laufwerk entfernt | Warnung | Keine | Modul-Fehler |
| Nicht genügend Energie zu Verfügung | Warnung | keine | Modul-Fehler |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [StorSimple Hardwarekomponenten und Status](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
