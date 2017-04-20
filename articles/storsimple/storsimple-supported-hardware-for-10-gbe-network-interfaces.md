<properties 
   pageTitle="Hardware für StorSimple 10 GbE Schnittstellen | Microsoft Azure"
   description="Beschreibt die unterstützten kleinformatigen pluggable (SFP) Transceiver, Kabel und Switches für 10 GbE-Netzwerkschnittstellen auf Ihrem Gerät StorSimple."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Unterstützte Hardware für 10 GbE-Netzwerkschnittstellen auf dem StorSimple-Gerät

## <a name="overview"></a>Übersicht

Dieser Artikel enthält Informationen über zusätzliche Hardware, die mit Ihrem Gerät Microsoft Azure StorSimple.

## <a name="list-of-devices-tested-by-microsoft"></a>Liste der von Microsoft getestete Geräte

Microsoft hat folgende small-Form-Factor pluggable (SFP) Transceiver, Kabel und Switches um sicherzustellen, dass sie optimal mit Funktion getestet. (In den folgenden Tabellen aktualisiert werden wie neuer Hardware getestet.)

### <a name="sfp-transceivers"></a>+ SFP-Transceiver

|Stellen|Modell|
|---|---|
|Cisco|SFP-10G-SR|

### <a name="cables"></a>Kabel

|S. Nein. |Stellen|Modell|
|---|---|---|
| 1.|Cisco|SFP-H10GB-CU1M|
| 2.|Cisco|SFP-H10GB-CU2M|
| 3.|Cisco|SFP-H10GB-CU3M|
| 4.|Tripp Lite|N820 - 05M (OM3)|

### <a name="switches"></a>Switches

|S. Nein.|Stellen|Modell|
|---|---|---|
| 1. |Cisco|N3K-C3172PQ-10GE|
| 2. |Cisco|N3K-C3048-ZM-F|
| 3. |Cisco|N5K-C5596UP-ANLAGEN|

## <a name="list-of-devices-tested-in-the-field"></a>Geräteliste im Feld getestet

Dieser Abschnitt enthält die Liste der Geräte, die im Feld erfolgreich StorSimple Kunden bereitgestellt wurden. Diese von Microsoft nicht getestet aber wahrscheinlich mit dem StorSimple-Gerät.
 
| Parameter                         | Wert                                    |
|-----------------------------------|------------------------------------------|
| Wechseln                     | Wacholder                                  |
| Switch-Modell                    | ex4550 32F                               |
| Switch-Betriebssystemversion | JunOS 12.3R9.4                           |
| Blade-Modell                     | Integrierte Anschlüsse (PIC 0)                    |
| Transceiver machen                  | Wacholder                                  |
| Transceiver-Modell               | Teilenummer 021308 740 <br></br> Teilenummer 030658 740                   |
| Transceiver Firmware-version    | Rev 01 Version 0.0 (gemeldet)            |
| Kabel-Modell                     | Duplex LC-LC 50/125µ, OM3, LSZH-jumper |
| StorSimple-Modell                | 8600                                     |
| StorSimple-Softwareversion     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Liste der Geräte getestet von OEM-Anbieter (Mellanox)  

Mellanox hat folgenden small-Form-Factor pluggable (SFP) Transceiver, Kabel und Switches um sicherzustellen, dass sie optimal mit Mellanox Netzwerkschnittstellen wie 10 GbE-Netzwerkschnittstellen auf dem StorSimple-Gerät getestet.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kabel und Module von Mellanox unterstützt 

Die folgende Tabelle listet die Kabel und Module von Mellanox unterstützt. Diese von Microsoft nicht getestet aber wahrscheinlich mit dem StorSimple-Gerät.

| S. Nein. | Geschwindigkeit | Modell                 | Beschreibung                                            | Stellen                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| CAB-SFP-SFP - 1M        | passive Kupferkabel SFP + 10 Gb/s 1m                   | Arista                |
| 2.     | 10 GbE| CAB-SFP-SFP - 2M        | passive Kupferkabel SFP + 10 Gb/s 2m                   | Arista                |
| 3.     | 10 GbE| CAB-SFP-SFP - 3M        | passive Kupferkabel SFP + 10 Gb/s 3m                   | Arista                |
| 4.     | 10 GbE| CAB-SFP-SFP - 5M        | passive Kupferkabel SFP + 10 Gb/s 5m                   | Arista                |
| 5.     | 10 GbE| Cisco SFP-H10GBCU1M   | Cisco SFP + Kabel                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP-H10GBCU3M   | Cisco SFP + Kabel                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP-H10GBCU5M   | Cisco SFP + Kabel                                       | Cisco                 |
| 8.     | 10 GbE| HP J9281B X242 10 G    | SFP + SFP + 1m Direktanschluss Kupferkabel             | HP                    |
| 9.     | 10 GbE| HP BLc 455883-B21     | 10Gb SR SFP + Opt                                       | HP                    |
| 10.    | 10 GbE| HP BLc 455886-B21     | 10Gb LR SFP + Opt                                       | HP                    |
| 11.    |10 GbE | HP BLc 487649-B21     | SFP + 0,5 m 10GbE Kupferkabel                           | HP                    |
| 12.    |10 GbE | HP BLc 487652-B21     | SFP + 1m 10GbE Kupferkabel                             | HP                    |
| 13.    |10 GbE | HP BLc 487655-B21     | SFP + 3m 10GbE Kupferkabel                             | HP                    |
| 14.    |10 GbE | HP BLc 487658-B21     | SFP + 7m 10GbE Kupferkabel                             | HP                    |
| 15.    |10 GbE | HP BLc 537963-B21     | SFP + 5m 10GbE Kupferkabel                             | HP                    |
| 16.    |10 GbE | HP AP784A             | C-Serie passiven Kupfer SFP + 3m langen Kabel                  | HP                    |
| 17.    |10 GbE | HP AP785A             | 5m Reihe C passiven Kupfer SFP + Kabel                  | HP                    |
| 18.    |10 GbE | HP AP818A             | 1m Serie B aktiv Kupfer SFP + Kabel                   | HP                    |
| 19.    |10 GbE | HP AP819A             | 3m Serie B aktiv Kupfer SFP + Kabel                   | HP                    |
| 20.    |10 GbE | HP J9150A             | X132 10 G SFP + LC SR-Transceiver                        | HP                    |
| 21.    |10 GbE | HP J9151A             | X132 10 G SFP + LC LR Transceiver                        | HP                    |
| 22.    |10 GbE | HP J9283B             | X242 10 G SFP + SFP + 3 m DAC-Kabel                        | HP                    |
| 23.    |10 GbE | HP J9285B             | X242 10 G SFP + SFP + 7 m DAC-Kabel                        | HP                    |
| 24.    | 10 GbE| HP JD095B             | X240 10 G SFP + SFP + 0,65 m DAC-Kabel                     | HP                    |
| 25.    | 10 GbE| HP JD096B             | X240 10 G SFP + SFP + 1,2 m DAC-Kabel                      | HP                    |
| 26.    | 10 GbE| HP JD097B             | X240 10 G SFP + SFP + 3 m DAD Kabel                        | HP                    |
| 27.    | 10 GbE| MAM1Q00A QSA Mellanox | QSFP SFP + Adapter                                   | Mellanox Technologies |
| 28.    | 10 GbE| MC2309124-006 Mt      | Passive Kupferkabel 1 X SFP+, QSFP 10 Gb/s 24awg 7 m   | Mellanox Technologies |
| 29.    | 10 GbE| MC2309124-007 Mt      | Passive Kupferkabel 1 X SFP+, QSFP 10 Gb/s 24awg 7 m   | Mellanox Technologies |
| 30.    | 10 GbE| MC2309130-003 Mt      | Passive Kupferkabel 1 X SFP+, QSFP 10 Gb/s 30awg 3 m   | Mellanox Technologies |
| 31.    | 10 GbE| MC2309130 00A Mt      | Passive Kupferkabel 1 X SFP+, QSFP 10 Gb/s 30awg 0,5 m | Mellanox Technologies |
| 32.    | 10 GbE| MC3309124-005 Mt      | Passive Kupferkabel 1 X SFP+ 10 Gb/s 24awg 5 m           | Mellanox Technologies |
| 33.    | 10 GbE| MC3309124-007 Mt      | Passive Kupferkabel 1 X SFP+ 10 Gb/s 24awg 7 m           | Mellanox Technologies |
| 34.    | 10 GbE| MC3309130-003 Mt      | Passive Kupferkabel 1 X SFP+ 10 Gb/s 30awg 3 m           | Mellanox Technologies |
| 35.    | 10 GbE| MC3309130 00A Mt      | Passive Kupferkabel 1 X SFP+ 10 Gb/s 30awg 0,5 m         | Mellanox Technologies |

### <a name="switches-supported-by-mellanox"></a>Mellanox unterstützte Switches 

Die folgende Tabelle listet die Switches von Mellanox unterstützt. Diese von Microsoft nicht getestet aber wahrscheinlich mit dem StorSimple-Gerät.

| S. Nein. | Geschwindigkeit | Modell | Beschreibung                                                         | Stellen              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10 GbE | 516733 B21  | HP ProCurve 6120XG 10 GbE Ethernet-Blade-Switch                      | HP    |
| 2.     | 10 GbE | 538113 B21  | HP 10GbE Pass-Through-Modul (PTM)                                  | HP    |
| 3.     | 10 GbE | EN4093      | IBM PureFlex System Fabric EN4093 10-Gigabit-skalierbare Switch-Modul | IBM   |
| 4.     | 1GbE  | 3020        | Cisco Catalyst 3020 1GbE Switch blade                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Cisco Catalyst 3020 X 1GbE Switch blade                              | Cisco |
| 6.     | 1GbE  | 438030 B21  | HP 1GbE-Switch-Modul - GbE2c Layer 2/3 Ethernet-Blade-Switch       | HP    |
| 7.     | 1GbE  | 6120G       | HP ProCurve 6120G/XG 1GbE Switch blade                              | HP    |

## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen über die StorSimple Hardwarekomponenten und Status](storsimple-monitor-hardware-status.md).
