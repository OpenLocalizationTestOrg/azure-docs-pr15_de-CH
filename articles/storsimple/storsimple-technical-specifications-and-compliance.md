<properties 
   pageTitle="Technische StorSimple | Microsoft Azure"
   description="Beschreibt die technischen Spezifikationen und Informationen gesetzliche Vorschriften für die StorSimple Hardwarekomponenten."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Technische Spezifikationen und Compliance für StorSimple-Gerät

## <a name="overview"></a>Übersicht

Hardware-Komponenten des Geräts Microsoft Azure StorSimple erfüllen technische Spezifikationen und Einhaltung von Standards in diesem Artikel beschriebenen. Die technischen Spezifikationen beschreiben Strom und Kühlmodule (PCM), Festplattenlaufwerke, Speicherkapazität und Anlagen. Die Kompatibilitätsinformationen umfasst z. B. internationale Standards, Sicherheit und Emissionen und Verkabelung.


## <a name="power-and-cooling-module-specifications"></a>Stromversorgung und Kühlung Modul Spezifikationen  

Das Gerät StorSimple hat zwei 100-240V zwei Lüfter, SBB kompatibel macht Kühlmodule (PCM). Dies bietet eine redundanten Konfiguration. Schlägt PCM erfolgt das Gerät auf andere PCM bis die fehlerhafte Einheit ausgetauscht wird.  

EBOD-Gehäuse verwendet 580 W PCM und primäre Einheit 764 W PCM. Den folgenden Tabellen sind die Spezifikationen der PCM zugeordnet.

| Spezifikation           | 580 W PCM (EBOD)                                    | 764 W PCM (primär)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Maximale Leistung    | 580 W                                               | 764                                                |
| Häufigkeit               | 50-60 Hz                                            | 50-60 Hz                                           |
| Auswahl der Spannung | Automatische hin: 90 – 264 VAC, 47-63 Hz               | Automatische hin: 90-264 VAC, 47-63 Hz               |
| Maximaler Eingangsstrom  | 20 A                                                | 20 A                                               |
| Leistungsfaktorkorrektur | > 95 % input Spannung                          | > 95 % input Spannung                         |
| Harmonische               | Erfüllt EN61000-3-2                                   | Erfüllt EN61000-3-2                                  |
| Ausgabe                  | Standby-Spannung 5 v @ 2.0 A                          | Standby-Spannung 5 v @ 2.7 ein                         |
|                         | + 5 v @ 42 ein                                          | + 5 v @ 40 A                                         |
|                         | + 12V @ 38 A                                         | + 12V @ 38 A                                        |
| Hot-Plug           |  Ja                                                | Ja                                                |
| Switches und LEDs       | AC-Netzschalter und vier Status-LED     | AC-Netzschalter und sechs Status-LED     |
| Gehäuse-Kühlung       | Lüfter mit variabler Lüfter Axial  | Lüfter mit variabler Lüfter Axial |

 
## <a name="power-consumption-statistics"></a>Power Statistiken  

Die folgende Tabelle listet die Leistungsaufnahme Daten (tatsächliche Werte variieren von der veröffentlichten) für Modelle StorSimple-Gerät. 
 
 Startbedingungen | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Lüfter langsam Laufwerke im Leerlauf | 1,45 EIN  |0,31 kW | 1057.76 BTU/h | 3.19 EIN | 0,34 kW | 1160.13 BTU/h 
 Lüfter langsamer Zugriff auf Laufwerke | 1,54 EIN | 0,33 kW | 1126.01 BTU/h | 3.27 EIN | 0,36 kW | 1228.37 BTU/h 
 Lüfter schnell Laufwerke im Leerlauf, zwei Netzteile mit Strom versorgt | 2.14 EIN | 0,49 kW  | 1671.95 BTU/h | 4,99 EIN | 0,54 kW | 1842.56 BTU/h 
 Lüfter schnell Laufwerke im Leerlauf, ein Netzteil mit Strom versorgt eine im Leerlauf | 2.05 A | 0,48 kW | 1637.83 BTU/h | 4,58 EIN | 0,50 kW | 1706.07 BTU/h 
 Lüfter schnell Laufwerke zugreifen, zwei Netzteile mit Strom versorgt | 2,26 EIN | 0.51 kW | 1740.19 BTU/h | 4,95 EIN | 0,54 kW | 1842.56 BTU/h 
 Schnelle Lüfter eingeschaltet Laufwerke zugreifen, ein Netzteil eine im Leerlauf | 2.14 EIN |0,49 kW | 1671.95 BTU/h | 4,81 EIN  | 0,53 kW | 1808.44 BTU/h 

## <a name="disk-drive-specifications"></a>Laufwerk-Spezifikationen  

Das Gerät StorSimple unterstützt bis zu 12 3,5-Zoll-Faktor Serial Attached SCSI (SAS) Laufwerke. Die tatsächliche Laufwerke können Solid-State-Laufwerken (SSDs) oder Festplatten (HDDs) je Produkt aus. 12 Steckplätze befinden sich in einer 3 x 4-Konfiguration vor dem Gehäuse. EBOD-Gehäuse kann nach zusätzlichem Speicher für einen anderen 12 Laufwerken. Diese sind immer Festplatten.  

## <a name="storage-specifications"></a>Speicher-Spezifikationen
StorSimple Geräte haben eine Mischung von Festplatten und Solid-State-Laufwerke 8100 und 8600. Die nutzbare Gesamtkapazität 8100 und 8600 sind ungefähr 15 TB und 38 TB. Die folgende Tabelle beschreibt die Details des SSD HDD und Cloud-Kapazität im Rahmen der StorSimple Lösung Kapazität.

| Modell / Kapazität                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| Anzahl der Festplatten (HDDs)              |   8                                                     |  19                                                     |
| Anzahl von Solid-State-Laufwerken (SSDs)            |   4                                                     |  5                                                      |
| Einzelne Festplattenkapazität                            |   4 TB                                                  |  4 TB                                                   |
| Einzelne SSD-Kapazität                            |   400 GB                                                |  800 GB                                                 |
| Freie Kapazität                                 |   4 TB                                                  | 4 TB                                                    |
| Nutzbare Festplattenkapazität                            | 14 TB                                                   | 36 TB                                                   |
| SSD-Nutzkapazität                            | 800 GB                                                  | 2 TB                                                    |
| Gesamte nutzbare Kapazität *                         | ~ 15 TB                                                 | ~ 38 TB                                                 |
| Maximale Lösung Kapazität (einschließlich Cloud)    | 200 TB                                                  | 500 TB                                                  |


<sup> * </sup> -  *Die gesamte Nutzkapazität enthält die Kapazität für Daten, Metadaten und buffers.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Gehäuse-Dimensionen und Gewichtsangaben  

Den folgenden Tabellen sind die verschiedenen Gehäuse-Spezifikationen für Maße und Gewicht.  

### <a name="enclosure-dimensions"></a>Gehäuse-Dimensionen
Die folgende Tabelle enthält die Dimensionen der Einheit Millimeter und Zoll.

|Gehäuse |Millimeter |Zoll |
|----------|------------|-------| 
| Höhe |87,9 | 3,46 |
| Breite über Rackbefestigungsflansch | 483 | 19.02. |
| Breite Körper des Gehäuses | 443 | 17,44 |
| Tiefe von vorne Flansch zu Gehäuse Körper | 577 | 22.72 |
| Tiefe Bedienfeld am Ende des Gehäuses | 630.5 | 24.82 |
| Tiefe Rackbefestigungsflansch am Ende des Gehäuses |   603 | 23,74 |

### <a name="enclosure-weight"></a>Gewicht der Einheit  

Je voll primäre Einheit von 21 bis 33 kg wiegen und erfordert zwei behandelt. 
 
| Gehäuse | Gewicht |
|-----------|--------| 
| Gewicht (je nach Konfiguration) |30 kg – 33 kg |
| Leer (keine Laufwerke eingebaut) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Gehäuse Spezifikationen  

Dieser Abschnitt enthält die Spezifikationen für die Gehäuse-Umgebung. Temperatur, Luftfeuchtigkeit, Höhe, Schock, Vibration, Ausrichtung, Sicherheit und elektromagnetische Kompatibilität (EMC) sind in dieser Kategorie enthalten.  

### <a name="temperature-and-humidity"></a>Temperatur und Feuchtigkeit

| Gehäuse        | Umgebungstemperatur  | Relative Luftfeuchtigkeit | Maximale Feuchttemperatur   |
|------------------|----------------------------|---------------------------|--------------------|
| Betriebsbereit      | 5° C BIS 35° C (41° C - 95° F)    | 20-80 % nicht kondensierend- | 28° C (82° F)        |
| Außer Betrieb  | – 40° C BIS 70° C (40° C - 158° F) | 5 % bis 100 %, nicht kondensierend  | 29° C (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Luftstrom, Höhe Schock, Vibration, Ausrichtung, Sicherheit und EMC
 
| Gehäuse          | Operative Daten                                                |
|--------------------|---------------------------------------------------------------------------| 
| Luftzufuhr            | Luftfluss im System ist von vorne nach hinten. System muss mit Unterdruck hinten Abgas Installation betrieben werden. Druck von Rack-Türen und Hindernisse erstellt sollte 5 Pascal (0,5 mm Wasser Monitor) nicht überschreiten. |
| Höhe, Betrieb  | -30 m 3045 Metern-100 bis 10.000 Fuß mit maximale Betriebstemperatur de 5 ° C über 7000 Fuß bewertet. |
| Höhe nicht betriebsbereit  | -305 Meter 12.192 Metern-1,000 auf 40.000 Fuß |
| Operativen Stromschlag  | 5g 10 ms ½ Sinus |
| Schlag, Lagerung  | 30g 10 ms ½ Sinus |
| Vibration bei Betrieb  | 0,21 g RMS zufällige 5 bis 500 Hz |
| Vibration bei Lagerung  | 1,04 g RMS zufällige 2-200 Hz |
| Vibration, Umsetzung  | 3g 2-200 Hz Sinus |
| Ausrichtung und Montage  | 19-Zoll-Rack-Montage (2 EIA-Einheiten) |
| Rack-Schienen  | Racks mit IEC 297 angepasst Tiefe mindestens 700 mm (31.50 Zoll) |
| Sicherheit und Genehmigung  |   CE und UL EN 61000-3 IEC 61000-3, UL 61000-3 |
| EMC  | EN55022 (CISPR - A), FCC EIN |

## <a name="international-standards-compliance"></a>Einhaltung der internationalen standards
Ihr Gerät Microsoft Azure StorSimple entspricht der folgenden internationalen Normen:  

- CE - EN 60950-1  
- CB Bericht IEC 60950-1  
- UL und cUL-Zulassung nach UL 60950-1  

## <a name="safety-compliance"></a>Sicherheit-compliance  

Das Microsoft Azure StorSimple Gerät erfüllt die folgenden Sicherheits-Ratings:  

- Produkt Typgenehmigung: UL, cUL CE  
- Sicherheit Compliance: UL 60950, IEC 60950, EN 60950  

## <a name="emc-compliance"></a>EMC compliance 

Das Microsoft Azure StorSimple Gerät erfüllt die folgenden EMC Bewertung.  

### <a name="emissions"></a>Emissionen

Das Gerät ist durchgeführt und bestrahlten Emissionen EMC kompatibel.  

- Geleitete Emissionen begrenzen Ebenen: CFR 47 Teil 15 b Klasse A EN55022 Klasse A CISPR Klasse A  
- Ausgestrahlte Emissionen begrenzen Ebenen: CFR 47 Teil 15 b Klasse A EN55022 Klasse A CISPR Klasse A   

### <a name="harmonics-and-flicker"></a>Harmonische und Flimmern  

Das Gerät entspricht EN61000-3-2/3.  

### <a name="immunity-limit-levels"></a>Immunität Limit Ebenen  
Das Gerät entspricht EN55024.  

## <a name="ac-power-cord-compliance"></a>AC Power Cord compliance
  
Stecker und komplette Kabel Assembly müssen Ansprüche für das Land, in dem das Gerät verwendet wird, und sie müssen in diesem Land werden Zulassungen. Die folgenden Tabellen sind Standards für USA und Europa.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>Netzkabel - USA (NRTL aufgeführt sein muss)

| Komponente       | Spezifikation                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Kabeltyp       | SV oder SVT mindestens 18 AWG, 3 Leiter 2.0 Meter Maximallänge |
| Stecker            | NEMA 5-15P Dreiadrige Anlage Plug bewertet 120 V 10 A; oder IEC 320-C14, 250 V, 10 A |
| Socket          | IEC 320 C-13, 250 V 10 A                                         |

### <a name="ac-power-cords---europe"></a>Netzkabel - Europa

| Komponente       | Spezifikation                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Kabeltyp       | Harmonisierte H05 VVF 3G1.0                                         |
| Socket          | IEC 320 C-13, 250 V 10 A                                         |

## <a name="supported-network-cables"></a>Unterstützte Netzwerkkabel  

10 GbE Netzwerkschnittstellen Daten 2 und Daten 3 finden Sie in der [Liste der unterstützten Netzwerkkabel und Module](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt ein StorSimple Gerät im Rechenzentrum bereitstellen. Weitere Informationen finden Sie unter [dem lokalen Gerät bereitstellen](storsimple-deployment-walkthrough-u2.md).  
