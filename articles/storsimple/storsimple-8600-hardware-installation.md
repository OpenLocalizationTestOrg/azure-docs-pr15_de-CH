<properties
   pageTitle="Installieren Sie das Gerät StorSimple 8600 | Microsoft Azure"
   description="Beschreibt das Entpacken und rack-Mount Geräts StorSimple 8600-Kabel vor der Installation und Konfiguration der Software."
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
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Entpacken, Rack-Montage und das Gerät StorSimple 8600

## <a name="overview"></a>Übersicht
Die Microsoft Azure StorSimple 8600 ist ein dual-Gehäuse und eine primäre und eine EBOD-Gehäuse aus. In diesem Lernprogramm wird erläutert, wie entpacken, Rackmontage und Kabel 8600 StorSimple Gerätehardware bevor Sie die StorSimple-Software konfigurieren.

## <a name="unpack-your-storsimple-8600-device"></a>Entpacken Sie das Gerät StorSimple 8600

Die folgenden Schritte bieten klare, detaillierte Anleitung zum Speichergerät StorSimple 8600 entpacken. Dieses Gerät wird in beiden Feldern für die primäre Einheit und weitere EBOD-Gehäuse geliefert. Diese beiden Felder werden anschließend in einem Feld platziert.

### <a name="prepare-to-unpack-your-device"></a>Entpacken Sie das Gerät vorbereiten

Lesen Sie die folgenden Informationen, bevor Sie Ihr Gerät entpacken.


![Symbol](./media/storsimple-safety/IC740879.png)![Symbol für schwere](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Warnung!**

1. Stellen Sie sicher, dass zwei für das Gewicht des Geräts zu verwalten Personen, wenn Sie es manuell verarbeiten. Eine vollständig konfigurierte Einheit wiegt 32 kg (70 Pfund).
1. Platzieren Sie das Feld auf einer Ebenen Fläche.

Anschließend gehen Sie zu Ihrem Gerät entpacken.

#### <a name="to-unpack-your-device"></a>Entpacken Sie das Gerät

1. Überprüfen Sie das Feld und Verpackung Schaum zerkleinert, schneidet, Wasserschaden oder sichtbare Beschädigung. Wenn das Feld oder die Verpackung stark beschädigt ist, öffnen Sie das nicht. Bitte [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , um zu prüfen, ob das Gerät ordnungsgemäß funktioniert.

2. Öffnen Sie das äußere und nehmen Sie die beiden Felder Primär- und EBOD Gehäuse. Sie können jetzt die Primär- und EBOD Gehäuse entpacken. Die folgende Abbildung zeigt die entpackt eines Gehäuse.

    ![Entpacken Sie das Speichergerät](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)

    **Entpackt Ansicht des Speichergeräts**

     Bezeichnung | Beschreibung
     ----- | -------------
     1     | Verpackung
     2     | SAS-Kabel (Zubehör und Kabel-Schacht)
     3     | Untere Schaum
     4     | Gerät
     5     | Top-Schaum
     6     | Zubehörkarton

3. Nach dem Entpacken Stellen der beiden Felder sicher, dass Sie:

  - 1 primäre Einheit (primäre Einheit und EBOD-Gehäuse sind in zwei separaten Feldern)
  - 1 EBOD-Gehäuse
  - 4 Netzkabel, 2 in jedem
  - 2 SAS-Kabel (für die Verbindung die primäre Einheit EBOD-Gehäuse)
  - 1 Crossover-Ethernet-Kabel
  - 2 serielle Konsole Kabel
  - 1 USB-Seriell-Konverter für seriellen Zugriff
  - 4 QSFP-zu-SFP + Adapter mit 10 GbE-Netzwerkschnittstellen
  - 2 rack-Mount-Kits (4 Halteschienen mit Hardware, 2 für die primäre Einheit und EBOD-Gehäuse), 1 in jedem
  - Erste Schritte-Dokumentation

    Wenn Sie keine der oben aufgeführten Elemente erhalten haben [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md).  

Der nächste Schritt ist Ihr Gerät für Rackmontage.

## <a name="rack-mount-your-storsimple-8600-device"></a>Befestigen Sie das Gerät StorSimple 8600

Die nächsten Schritte zum Installieren StorSimple 8600-Speichergeräts in ein standard-19-Zoll-Rack mit Vorder- und Rückseite. Dieses Gerät wird mit zwei: eine primäre Einheit und EBOD-Gehäuse. Beide Rack montiert werden müssen.

Die Installation umfasst mehrere Schritte, die jeweils in den folgenden Verfahren beschrieben wird.

> [AZURE.IMPORTANT]
StorSimple Geräte müssen für den ordnungsgemäßen Betrieb Rack montiert werden.



### <a name="site-preparation"></a>Vorbereitung

Gehäuse werden in ein 19-Zoll-standard-Rack installiert, die Vorder- und Rückseite Beiträge. Gehen Sie bei Rack-Installation vorzubereiten.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Vorbereiten die Website für Rackmontage

1. Stellen Sie sicher sind die Primär- und EBOD Gehäuse sicher auf eine flache, stabile und Ebene Arbeitsfläche (oder ähnlich).

2. Überprüfen Sie, ob die Website einrichten möchten standard Wechselstrom von einer unabhängigen Spannungsquelle oder einem Rack Power Distribution Unit (PDU) mit einer unterbrechungsfreien Stromversorgung (USV) verfügt.

3. Stellen Sie sicher ist, eine 4U (2 X 2 HE) Steckplatz am Rack Gehäuse bereitgestellt werden sollen.

![Symbol](./media/storsimple-safety/IC740879.png)![Symbol für schwere](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Warnung!**

 Stellen Sie sicher, dass zwei Personen des Gewichts der Geräteinstallation manuell verarbeitet werden. Eine vollständig konfigurierte Einheit wiegt 32 kg (70 Pfund).

### <a name="rack-prerequisites"></a>Rack-Komponenten

Die Gehäuse sind für die Installation in ein standard-Rack, 19 Zoll mit entwickelt:

- Mindesttiefe 27.84 Zoll Rack zu Posten
- Höchstgewicht von 32 kg für das Gerät
- Maximale Abgasgegendruck 5 Pascal (0,5 mm Wasser Monitor)

### <a name="rack-mounting-rail-kit"></a>Rackmontage-Schienen-kit

Eine Reihe von Montageschienen 19-Zoll-Racks mit erfolgt. Die Schienen wurden getestet, um die Höchstgewicht behandeln. Diese Schienen können Installation mehrere Anlagen ohne Platz im Rack. Installieren Sie zuerst das EBOD-Gehäuse.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>EBOD-Gehäuse auf den Schienen zu installieren

2. Führen Sie diesen Schritt nur dann, wenn innere Schienen nicht auf Ihrem Gerät installiert sind. Normalerweise werden die inneren Schienen werkseitig installiert. Wenn Schienen nicht installiert sind, installieren Sie die Schiene linke und Rechte Schiene Folien neben Gehäuse. Sie legen mit sechs metrischen Schrauben auf beiden Seiten. Bei Ausrichtung sind Folien Schiene **LH-Vorderseite** und **RH-Vorderseite**gekennzeichnet, und das Ende nach hinten am Gehäuse befestigt ist konisch zulaufende.

    ![Gehäuse Schiene Folien zuordnen](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Die Seiten des Gehäuses Schiene Folien zuordnen**

    Bezeichnung | Beschreibung
    ----- | -----------
    1     | 3 x 4 M Schaltfläche Kopf Schrauben
    2     | Gehäuse-Folien

3. Schiene linke und Rechte Schiene Assemblys Ablage Rack vertikal Mitglieder anschließen. Die Klammern kennzeichnen **LH**, **RH**und **oben** schrittweise richtig ausgerichtet.

4. Suchen Sie nach Schiene Pins auf der Vorder- und Rückseite der Schiene. Erweitern Sie die Schiene Übermittlung Rack und Stifte in der Vorder- und Rückseite des Einbaugehäuses Post vertikale Member Löcher einfügen. Achten Sie darauf, die Schiene ist.

5. Sichern Sie Schiene im Rack vertikale Mitglieder mithilfe zweier metrischen mitgelieferten Schrauben. Verwenden Sie eine Schraube auf der Vorderseite auf der Rückseite.

6. Wiederholen Sie diese Schritte für die andere Schiene.

     ![Rack-Gehäuse Schiene Folien zuordnen](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Rack Schiene Assemblys zuordnen**

     Bezeichnung | Beschreibung
     ----- | -----------
     1     | Klemmschraube
     2     | Quadrat-Loch Vorderseite des Einbaugehäuses Post Schraube
     3     | Links vorne Schiene Speicherort Stifte
     4     | Klemmschraube
     5     | Hintere Schiene linke Position Stifte

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>Montage im Rack EBOD-Gehäuse

Mit der neu installierten Rackschienen, die folgenden Schritte EBOD Einheit im Rack zu montieren.

#### <a name="to-mount-the-ebod-enclosure"></a>Bereitstellen von EBOD-Gehäuse

1. Assistent heben Sie die Einheit mit den Rackschienen ausgerichtet.

2. Legen Sie die Einheit in den Schienen, und drücken Sie es vollständig in das Rack CAB.

    ![Einfügen der Geräte im rack](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Die Einheit im Rack montieren**

3. Ziehen Sie den linken und rechten Flansch Caps die freien enden. Flansch Caps snap einfach auf die Flansche.

4. Befestigen Sie das Gehäuse in Racks mit einem bereitgestellten Kreuzschlitzschraube über jeden Flansch links und rechts.

4. Installieren Sie Flansch Caps in Position drücken und sie einrasten.

     ![Flansch Caps installieren](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)

    **Installieren den Flansch caps**

     Bezeichnung | Beschreibung
     ----- | -----------
     1     | Gehäuse Befestigungsschraube


### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Die primäre Einheit im Rack montieren

Nach dem EBOD-Gehäuse bereitstellen müssen Sie die primäre Einheit die gleichen Schritte bereitstellen.

> [AZURE.NOTE]
>
> - Es ist möglich, ein paar leere Steckplätze im Rack zwischen der primären Einheit und EBOD Gehäuse.
> - Verwenden Sie bereitgestellte 2m SAS-Kabel Verbindung die primäre Einheit EBOD Gehäuse.
> - Es gibt keine Einschränkungen auf die relative Position des Hauptgerät zu EBOD Einheit. Daher kann die primäre Einheit in den oberen Steckplatz und EBOD-Gehäuse unten platziert werden, oder umgekehrt.

Der nächste Schritt ist Kabel Geräts macht, Netzwerkzugriff und seriellen Zugriff.

## <a name="cable-your-storsimple-8600-device"></a>Verbinden Sie Ihr Gerät StorSimple 8600

Das folgende Verfahren wie das Gerät StorSimple 8600 Stromversorgung, Netzwerk und serielle Verbindung Kabel.

### <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, Ihr Gerät zu verbinden, müssen Sie:

- Die primäre Einheit und EBOD-Gehäuse vollständig entpackt
- 4 Netzkabel (2 der und EBOD-Gehäuse), die mit dem Gerät
- Verbindung EBOD-Gehäuse mit der primären Einheit mitgelieferten 2 SAS-Kabel
- Zugriff auf 2 Power Distribution Units (PDUs) (empfohlen)
- Netzwerkkabel
- Serielle Kabel bereitgestellt
- USB-Seriell-Konverter mit den entsprechenden Treiber installiert auf Ihrem PC (falls erforderlich)
- 4 QSFP bereitgestellten-zu-SFP + Adapter mit 10 GbE-Netzwerkschnittstellen
- [Unterstützte Hardware für 10 GbE-Netzwerkschnittstellen auf dem StorSimple-Gerät](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS und Verkabelung in ordnungsgemäßem Zustand

Das Gerät verfügt über eine primäre Einheit und EBOD-Gehäuse. Dies erfordert die Einheiten für Serial Attached SCSI (SAS)-Konnektivität und Leistung miteinander verbunden werden.

Beim Einrichten dieses Gerät zum ersten Mal für SAS-Kabel zuerst führen Sie aus, und führen Sie die Schritte für Power Verkabelung.

[AZURE.INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Netzwerkkabel

Das Gerät wird eine aktiv-Standby-Konfiguration: zu jedem Zeitpunkt eine Controller-Modul aktiv ist und Verarbeitung aller Datenträger und das Netzwerk während der Controller-Modul ist. Bei einem Controller-Fehler aktiviert Standby-Domänencontroller sofort und weiterhin alle Datenträger und Netzwerk-Operationen.

Zur Unterstützung dieser redundanten Controller-Failover müssen Sie Ihr Gerät Netzwerkkabel wie nachfolgend gezeigt.

#### <a name="to-cable-for-network-connection"></a>Kabel für die Netzwerkschnittstelle

1. Das Gerät hat sechs Netzwerkschnittstellen auf jedem Controller: vier 1 Gbit/s und zwei 10 Gbit/s-Ethernet-ports. In der folgenden Abbildung Daten Anschlüsse der Rückwandplatine des Geräts zu finden.

     ![Rückwandplatine 8600 Gerät](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Zeigen Daten-Ports des Geräts**

     Bezeichnung   | Beschreibung
     ------- | -----------
     0,1,4,5 |  1 Gigabit-Netzwerkschnittstellen
     2,3     | 10 GbE-Netzwerkschnittstellen
     6       | Serielle Anschlüsse



1. Das folgende Diagramm für Netzwerkkabel anzeigen (Mindest-Netzwerkkonfiguration wird als durchgezogene blaue Linien angezeigt. Für hohe Verfügbarkeit und Performance wird zusätzlich erforderliche Konfiguration durch gepunktete Linien angezeigt.)

![Das Gerät 4U Netzwerk verbinden](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Netzwerk-Verkabelung für Ihr Gerät**

Bezeichnung | Beschreibung
----- | -----------
EIN    | LAN mit Internetzugriff
B    | Controller 0
C    | PCM 0
D    | Domänencontroller 1
E    | PCM 1
F    | EBOD-Controller 0
G    | EBOD-Controller 1
H, I  | Hosts (z. B. Dateiserver)
0-5  | Netzwerk-Schnittstellen
6    | Primäre Einheit
7    | EBOD-Gehäuse

Wenn das Gerät Verkabelung minimale Konfiguration erforderlich:


- Mindestens zwei Netzwerkschnittstellen verbunden auf beiden Controllern auf Cloud und iSCSI. Die Daten 0 Port automatisch aktiviert und über die serielle Konsole des Geräts konfiguriert. Neben Daten 0 einen anderen Datenport auch über klassischen Azure-Portal konfiguriert werden muss. In diesem Fall Verbindungsdaten 0 Port primäre LAN (Netzwerk mit Internetzugang). Die Daten-Ports können mit SAN-iSCSI-LAN (VLAN) Netzwerksegment, abhängig von der gewünschten Rolle verbunden.

- Identische Schnittstellen auf jedem Domänencontroller verbunden mit demselben Netzwerk Verfügbarkeit bei einem Controller Failover. Möchten Sie Daten 0 und DATA 3 für einen der Controller verbinden, müssen Sie beispielsweise die entsprechenden Daten 0 und DATA 3 auf dem Controller verbinden.

Beachten Sie für hohe Verfügbarkeit und Performance:


- Wenn möglich, konfigurieren Sie eine Schnittstelle für Cloud-Zugriff (1 Gigabit) und ein weiteres Paar für iSCSI (10 GbE empfohlen) auf jedem Domänencontroller.

- Wenn möglich, schließen Sie Netzwerkschnittstellen aus jeder Controller an zwei verschiedene-Switches auf Verfügbarkeit gegen den Switchausfall eines an Die Abbildung zeigt zwei 10 GbE Netzwerkschnittstellen, Daten 2 und Daten 3 von jeder Controller mit zwei verschiedenen Switches verbunden. Weitere Informationen finden Sie in **Netzwerkschnittstellen** [hohe Verfügbarkeit an Ihrem Gerät StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Verwenden der 10 GbE Netzwerkschnittstellen SFP + Transceiver mit bereitgestellten QSFP-SFP + Adapter. Weitere Informationen finden Sie auf [unterstützte Hardware für 10 GbE-Netzwerkschnittstellen auf dem Gerät StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

### <a name="serial-port-cabling"></a>Serielle Kabel

Führen Sie die folgenden Schritte aus, um die serielle Kabel.

#### <a name="to-cable-for-serial-connection"></a>Kabel für die serielle Verbindung

1. Das Gerät hat einen seriellen Anschluss auf jedem Domänencontroller durch Schraubenschlüssel-Symbol gekennzeichnet. Serielle Anschlüsse finden Sie finden Sie in der Abbildung, die Daten Anschlüsse auf der Rückseite des Geräts anzeigt.

2. Identifizieren des aktiven Controllers auf Ihrem Gerät Rückwandplatine. Eine blinkende blaue LED zeigt an, dass der Controller aktiv ist.

3. Verwenden Sie bereitgestellte serielle Kabel (ggf. USB-Seriell-Konverter für den Laptop), und schließen Sie die Konsole oder Computer (mit terminal Emulation auf dem Gerät) an den seriellen Anschluss des aktiven Controllers.

4. Installieren Sie serielle USB-Treiber (mit dem Gerät geliefert) auf dem Computer.

5. Richten Sie die serielle Verbindung wie folgt:
   - 115.200 baud
   - 8 Datenbits
   - 1 Stoppbit
   - Keine Parität
   - Legen Sie **keine** Flusskontrolle

6. Überprüfen Sie die Verbindung durch Drücken der EINGABETASTE auf der Konsole. Das serielle Konsolenmenü sollte angezeigt werden.

> [AZURE.NOTE] **Lights Out Management:** Wenn das Gerät in einem remote-Datencenter oder im Serverraum mit eingeschränktem Zugriff installiert ist, sicherzustellen Sie, dass serielle Verbindung zu beiden Controllern immer einen seriellen Konsolen-Switch oder ähnliche Geräte verbunden sind. Dies ermöglicht Out-of-Band-Remote-Steuerung und Operationen bei Netzwerkausfall oder unerwartete Fehler.

Verkabelung Geräts macht, Netzwerkzugriff und seriellen Verbindung wurde abgeschlossen. Der nächste Schritt ist die Software auf Ihrem Gerät konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt zum [Bereitstellen und konfigurieren das lokalen StorSimple-Gerät](storsimple-deployment-walkthrough-u2.md).
