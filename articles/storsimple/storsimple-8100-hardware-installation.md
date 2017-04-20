<properties
   pageTitle="Installieren Sie das Gerät StorSimple 8100 | Microsoft Azure"
   description="Beschreibt das Entpacken und rack-Mount Geräts StorSimple 8100-Kabel vor der Installation und Konfiguration der Software."
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

# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Entpacken, Rack-Montage und das Gerät StorSimple 8100

## <a name="overview"></a>Übersicht

Die Microsoft Azure StorSimple 8100 ist ein Gehäuse, rackmontierte Geräte. In diesem Lernprogramm wird erläutert, wie entpacken, Rackmontage und Kabel 8100 StorSimple Gerätehardware konfigurieren und das StorSimple-Gerät bereitstellen.

## <a name="unpack-your-storsimple-8100-device"></a>Entpacken Sie das Gerät StorSimple 8100

Die folgenden Schritte bieten klare, detaillierte Informationen zum Speichergerät StorSimple 8100 entpacken. Dieses Gerät wird in einer einzigen Verpackung geliefert.

### <a name="prepare-to-unpack-your-device"></a>Entpacken Sie das Gerät vorbereiten

Lesen Sie die folgenden Informationen, bevor Sie Ihr Gerät entpacken.

![Symbol](./media/storsimple-safety/IC740879.png)![Symbol für schwere](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Warnung!**

1. Stellen Sie sicher, dass zwei für das Gewicht der Einheit verwalten Personen, wenn Sie es manuell verarbeiten. Eine vollständig konfigurierte Einheit wiegt 32 kg (70 Pfund).
1. Platzieren Sie das Feld auf einer Ebenen Fläche.

Anschließend gehen Sie zu Ihrem Gerät entpacken.

#### <a name="to-unpack-your-device"></a>Entpacken Sie das Gerät

1. Überprüfen Sie das Feld und Verpackung Schaum zerkleinert, schneidet, Wasserschaden oder sichtbare Beschädigung. Wenn das Feld oder die Verpackung stark beschädigt ist, öffnen Sie das nicht. Bitte [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , um zu prüfen, ob das Gerät ordnungsgemäß funktioniert.

2. Entpacken Sie das Feld. Das folgende Bild zeigt die entpackten des Geräts StorSimple.

     ![Entpacken Sie das Speichergerät](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)

    **Entpackt Ansicht des Speichergeräts**

     Bezeichnung | Beschreibung
     ----- | -------------
     1     | Verpackung
     2     | Untere Schaum
     3     | Gerät
     4     | Top-Schaum
     5     | Zubehörkarton


3. Nach dem Entpacken im Feld stellen Sie sicher, dass Sie:

   - 1 Gerät Gehäuse
   - 2 Netzkabel
   - 1 Crossover-Ethernet-Kabel
   - 2 serielle Konsole Kabel
   - 1 USB-Seriell-Konverter für seriellen Zugriff
   - 1 Manipulationen T10 Schraubendreher
   - 4 QSFP-zu-SFP + Adapter mit 10 GbE-Netzwerkschnittstellen
   - 1 Rack-Einbau-Kit (2 Halteschienen mit Hardware)
   - Erste Schritte-Dokumentation

    Wenn Sie keine der oben aufgeführten Elemente erhalten haben [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md).

Der nächste Schritt ist Ihr Gerät für Rackmontage.

## <a name="rack-mount-your-storsimple-8100-device"></a>Befestigen Sie das Gerät StorSimple 8100

Die nächsten Schritte zum Installieren StorSimple 8100-Speichergeräts in ein standard-19-Zoll-Rack mit Vorder- und Rückseite. StorSimple 8100-Gerät hat eine primäre Gehäuse.

Die Installation umfasst mehrere Schritte, die jeweils in den folgenden Verfahren beschrieben wird.

> [AZURE.IMPORTANT]
StorSimple Geräte müssen für den ordnungsgemäßen Betrieb Rack montiert werden.

### <a name="prepare-the-site"></a>Vorbereiten der site

Das Gerät werden in ein 19-Zoll-standard-Rack installiert, die Vorder- und Rückseite Beiträge. Gehen Sie bei Rack-Installation vorzubereiten.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Vorbereiten die Website für Rackmontage

1. Stellen Sie sicher, dass das Gerät sicher auf eine flache, stabile und Ebene Fläche (oder ähnlich) liegt.

2. Überprüfen Sie, ob die Website einrichten möchten standard Wechselstrom von einer unabhängigen Spannungsquelle oder einem Rack Stromverteilungseinheit (PDU) mit einer unterbrechungsfreien Stromversorgung (USV) verfügt.

3. Stellen Sie sicher ist, ein 2-HE-Steckplatz auf dem Gerät bereitgestellt werden soll.

![Symbol](./media/storsimple-safety/IC740879.png)![Symbol für schwere](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Warnung!**

Stellen Sie sicher, dass zwei Personen des Gewichts der Geräteinstallation manuell verarbeitet werden. Eine vollständig konfigurierte Einheit wiegt 32 kg (70 Pfund).

### <a name="rack-prerequisites"></a>Rack-Komponenten

8100-Gehäuse ist für die Installation in ein standard-Rack, 19 Zoll mit entwickelt:

- Mindesttiefe 27.84 Zoll Rack, Posten.
- Höchstgewicht von 32 kg für das Gerät
- Maximale Abgasgegendruck 5 Pascal (0,5 mm Wasser Monitor).

### <a name="rack-mounting-rail-kit"></a>Rackmontage-Schienen-kit

Eine Reihe von Montageschienen ist für 19-Zoll-Racks bereitgestellt. Die Schienen wurden getestet, um die Höchstgewicht behandeln. Diese Schienen können Installation mehrere Anlagen ohne Platz im Rack.


#### <a name="to-install-the-device-on-the-rails"></a>Zum Installieren des Geräts den Schienen

2. Führen Sie diesen Schritt nur dann, wenn innere Schienen nicht auf Ihrem Gerät installiert sind. Normalerweise werden die inneren Schienen werkseitig installiert. Wenn Schienen nicht installiert sind, installieren Sie die Schiene linke und Rechte Schiene Folien neben Gehäuse. Sie legen mit sechs metrischen Schrauben auf beiden Seiten. Bei Ausrichtung sind Folien Schiene **LH-Vorderseite** und **RH-Vorderseite**gekennzeichnet, und das Ende nach hinten am Gehäuse befestigt ist konisch zulaufende.<br/>

    ![Gehäuse Schiene Folien zuordnen](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
    **Anfügen inneren Führungsschienen Folien an den Seiten des Gehäuses**

       Bezeichnung | Beschreibung
       ----- | -----------
       1     | 3 x 4 M Schaltfläche Kopf Schrauben
       2     | Gehäuse-Folien

3. Äußere linke Schiene und äußere rechte Schiene Assemblys Ablage Rack vertikal Mitglieder anschließen. Die Klammern kennzeichnen **LH** **RH**und **oben** , durch die richtige Ausrichtung führen.

4. Suchen Sie nach Schiene Pins auf der Vorder- und Rückseite der Schiene. Erweitern Sie die Schiene Übermittlung Rack und Stifte in der Vorder- und Rückseite des Einbaugehäuses Post vertikale Member Löcher einfügen. Achten Sie darauf, die Schiene ist.

5. Verwenden Sie zwei der mitgelieferten Schrauben metrischen Schiene im Rack vertikale Elemente sichern. Verwenden Sie eine Schraube auf der Vorderseite auf der Rückseite.

6. Wiederholen Sie diese Schritte für die andere Schiene.<br/>

     ![Rack-Gehäuse Schiene Folien zuordnen](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Rack zuordnen äußeren Schienen Assemblys**

     Bezeichnung | Beschreibung
     ----- | -----------
     1     | Klemmschraube
     2     | Quadrat-Loch Vorderseite des Einbaugehäuses Post Schraube
     3     | Linke Schiene Lage Stifte
     4     | Klemmschraube
     5     | Linke Schiene hinteren Position Stifte


### <a name="mounting-the-device-in-the-rack"></a>Im Rack zu montieren

Mit der neu installierten Rackschienen, führen Sie die folgenden Schritte aus, um das Gerät im Rack.

#### <a name="to-mount-the-device"></a>Zum Laden des Gerätes

1. Assistent heben Sie die Einheit mit den Rackschienen ausgerichtet.

2. Legen Sie das Gerät in den Schienen und schieben Sie das Gerät vollständig in das Rack CAB.<br/>

    ![Einfügen der Geräte im rack](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Im Rack zu montieren**


3. Ziehen Sie den linken und rechten Flansch Caps die freien enden. Flansch Caps snap einfach auf die Flansche.

5. Befestigen Sie das Gehäuse in Racks mit einem bereitgestellten Kreuzschlitzschraube über jeden Flansch links und rechts.

4. Installieren Sie Flansch Caps in Position drücken und fangen sie an.<br/>

     ![Flansch Caps installieren](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)

    **Installieren den Flansch caps**

     Bezeichnung | Beschreibung
     ----- | -----------
     1     | Gehäuse Befestigungsschraube

Der nächste Schritt ist Kabel Geräts macht, Netzwerkzugriff und seriellen Zugriff.

## <a name="cable-your-storsimple-8100-device"></a>Verbinden Sie Ihr Gerät StorSimple 8100

Das folgende Verfahren wie das Gerät StorSimple 8100 Stromversorgung, Netzwerk und serielle Verbindung Kabel.

### <a name="prerequisites"></a>Erforderliche Komponenten

Zunächst die Verkabelung des Geräts müssen Sie:

- Speichergerät vollständig entpackt und Rack montiert.

- 2 mit dem Gerät gelieferte Netzkabel

- Zugriff auf 2 Power Distribution Units (empfohlen).

- Netzwerkkabel

- Serielle Kabel bereitgestellt

- Serielle USB-Adapter mit den entsprechenden Treiber installiert auf Ihrem PC (falls erforderlich)

- 4 QSFP bereitgestellten-zu-SFP + Adapter mit 10 GbE-Netzwerkschnittstellen

- [Unterstützte Hardware für 10 GbE-Netzwerkschnittstellen auf dem StorSimple-Gerät](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)


### <a name="power-cabling"></a>Power Verkabelung

Das Gerät enthält redundante Stromversorgung und Kühlung Module (PCM). Sowohl PCM müssen installiert und an verschiedene Stromquellen hohen Verfügbarkeit sichergestellt.

Führen Sie die folgenden Schritte aus, um Ihr Gerät für Power cable.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Netzwerkkabel

Das Gerät ist eine aktiv-Standby-Konfiguration: zu jedem Zeitpunkt eine Controller-Modul aktiv ist und Verarbeitung aller Datenträger und das Netzwerk während der Controller-Modul ist. Ein Controller ausfällt, Standby-Domänencontroller sofort aktiviert und weiterhin alle Datenträger und Netzwerkoperationen.

Zur Unterstützung dieser redundanten Controller-Failover müssen Sie Ihr Gerät Netzwerkkabel wie in den folgenden Schritten beschrieben.

#### <a name="to-cable-for-network-connection"></a>Kabel für die Netzwerkschnittstelle

1. Das Gerät hat sechs Netzwerkschnittstellen auf jedem Controller: vier 1 Gbit/s und zwei 10 Gbit/s-Ethernet-ports. Die verschiedenen Daten-Ports der Rückwandplatine des Geräts zu identifizieren.

    ![Rückwandplatine 8100 Gerät](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Anzeigen von Daten-Ports des Geräts**

     Bezeichnung   | Beschreibung
     ------- | -----------
     0,1,4,5 |  1 Gigabit-Netzwerkschnittstellen
     2,3     | 10 GbE-Netzwerkschnittstellen
     6       | Serielle Anschlüsse

2. Das folgende Diagramm für Netzwerkkabel anzeigen (Mindest-Netzwerkkonfiguration wird als durchgezogene blaue Linien angezeigt. Zusätzliche Konfigurationsaufwand für hohe Verfügbarkeit und Performance wird durch punktierte Linien angezeigt.)


    ![Das Gerät 2U Netzwerk verbinden](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Netzwerk-Verkabelung für Ihr Gerät**


  	|Bezeichnung | Beschreibung |
  	|----- | ----------- |
  	| EIN    | LAN mit Internetzugriff |
  	| B    | Controller 0 |
  	| C    | PCM 0 |
  	| D    | Domänencontroller 1 |
  	| E    | PCM 1 |
  	| F, G | Hosts |
  	| 0-5  | Netzwerk-Schnittstellen |



Wenn das Gerät Verkabelung minimale Konfiguration erforderlich:


- Mindestens zwei Netzwerkschnittstellen verbunden auf beiden Controllern auf Cloud und iSCSI. Die Daten 0 Port automatisch aktiviert und über die serielle Konsole des Geräts konfiguriert. Neben Daten 0 einen anderen Datenport auch über klassischen Azure-Portal konfiguriert werden muss. In diesem Fall Verbindungsdaten 0 Port primäre LAN (Netzwerk mit Internetzugang). Die Daten-Ports können mit SAN-iSCSI-LAN (VLAN) Netzwerksegment, abhängig von der gewünschten Rolle verbunden.

- Identische Schnittstellen auf jedem Domänencontroller verbunden mit demselben Netzwerk Verfügbarkeit bei einem Controller Failover. Möchten Sie Daten 0 und DATA 3 für einen der Controller verbinden, müssen Sie beispielsweise die entsprechenden Daten 0 und DATA 3 auf dem Controller verbinden.

Beachten Sie für hohe Verfügbarkeit und Performance:


- Wenn möglich, konfigurieren Sie eine Schnittstelle für Cloud-Zugriff (1 Gigabit) und ein weiteres Paar für iSCSI (10 GbE empfohlen) auf jedem Domänencontroller.

- Wenn möglich, schließen Sie Netzwerkschnittstellen aus jeder Controller an zwei verschiedene-Switches auf Verfügbarkeit gegen den Switchausfall eines an Die Abbildung zeigt zwei 10 GbE Netzwerkschnittstellen, Daten 2 und Daten 3 von jeder Controller mit zwei verschiedenen Switches verbunden.

Weitere Informationen finden Sie in **Netzwerkschnittstellen** [hohe Verfügbarkeit an Ihrem Gerät StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Bei Verwendung von SFP + Transceiver mit Ihrem 10 GbE Netzwerkschnittstellen verwenden bereitgestellten QSFP-SFP + Adapter. Weitere Informationen finden Sie auf [unterstützte Hardware für 10 GbE-Netzwerkschnittstellen auf dem Gerät StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).



### <a name="serial-port-cabling"></a>Serielle Kabel

Führen Sie die folgenden Schritte aus, um die serielle Kabel.

#### <a name="to-cable-for-serial-connection"></a>Kabel für die serielle Verbindung

1. Das Gerät hat einen seriellen Anschluss auf jedem Domänencontroller durch Schraubenschlüssel-Symbol gekennzeichnet. Siehe Abbildung im Abschnitt [Netzwerkkabel](#network-cabling) zu seriellen Anschlüssen auf der Rückwandplatine des Geräts.

2. Identifizieren des aktiven Controllers auf Ihrem Gerät Rückwandplatine. Eine blinkende blaue LED zeigt an, dass der Controller aktiv ist.

3. Bereitgestellte serielle Kabel (ggf. USB-Seriell-Konverter für den Laptop) und schließen Sie die Konsole oder Computer (mit terminal Emulation auf dem Gerät) an den seriellen Anschluss des aktiven Controllers.

4. Installieren Sie serielle USB-Treiber (mit dem Gerät geliefert) auf dem Computer.

5. Richten Sie die serielle Verbindung wie folgt: 115.200 Baud, 8 Datenbits, 1 Stoppbit, keine Parität und Flusskontrolle auf None festgelegt.

6. Überprüfen Sie die Verbindung durch Drücken der EINGABETASTE auf der Konsole. Das serielle Konsolenmenü sollte angezeigt werden.

>[AZURE.NOTE] **Lights Out Management**: Wenn das Gerät in einem remote-Datencenter oder im Serverraum mit eingeschränktem Zugriff installiert ist, sicherzustellen, dass serielle Verbindung zu beiden Controllern immer einen seriellen Konsolen-Switch oder ähnliche Geräte verbunden sind. Dadurch Out-of-Band-Remote-Steuerung und Operationen Netzwerk Störungen oder unerwartete Fehler.

Das Gerät ist jetzt für Stromversorgung, Netzwerkzugriff und serielle Konnektivität verbunden. Im nächste Schritt wird die Software und das Gerät bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum [Bereitstellen und konfigurieren das lokalen StorSimple-Gerät](storsimple-deployment-walkthrough.md).
