<properties
   pageTitle="StorSimple Manager Gerät Dashboard verwenden | Microsoft Azure"
   description="Beschreibt StorSimple Manager Service Gerät Dashboard und speichermetriken und angeschlossenen Initiatoren und suchen und der IQN verwenden."
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

# <a name="use-the-storsimple-manager-device-dashboard"></a>StorSimple-Gerät Ansicht verwenden

## <a name="overview"></a>Übersicht

Dashboard Gerät StorSimple Manager bietet Informationen für ein bestimmtes StorSimple-Gerät im Gegensatz zu Informationen über die Geräte in der Microsoft Azure StorSimple Lösung ermöglicht Service-Dashboards.

![Gerät Dashboardseite](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Das Schaltpult enthält die folgende Informationen:

- **Diagrammbereich** Metriken betreffenden im Diagrammbereich oben im Schaltpult angezeigt. In diesem Diagramm können Sie Metriken für den primären Speichers (der Betrag von Hosts auf das Gerät geschrieben) und total cloudspeicher belegt durch das Gerät über einen Zeitraum anzeigen.

     In diesem Zusammenhang *Primärspeicher* bezieht sich auf die Gesamtmenge der Daten vom Host geschrieben und kann vom Datenträgertyp unterteilt: *primäre tiered Storage* enthält sowohl lokal gespeicherten Daten und Daten in der Cloud, Ebenen *Primärer Speicher lokal fixiert* enthält nur die Daten lokal gespeichert. *Cloud-Speicher*ist ein Maß für den Gesamtbetrag der Daten in der Cloud auf der anderen Seite. Dazu gehören tiered Daten und Backups. Beachten Sie, dass Daten in der Cloud dedupliziert und komprimiert, dagegen Primärspeicher Speicher vor dedupliziert und komprimiert die Daten verwendet. (Sie können diese beiden Zahlen um eine Vorstellung der Kompressionsrate vergleichen.) Für Primär- und Cloud-Speicher, der Beträge basiert auf Überwachung Häufigkeit konfigurieren. Beispielsweise wählt eine Woche Frequenz wird dann das Diagramm anzeigen Daten für jeden Tag in der vorherigen Woche.

     Sie können das Diagramm wie folgt konfigurieren:

     - Cloud-Speicher verbraucht langfristig finden die Option **CLOUD Speicher verwendet** . Um den gesamten Speicher anzuzeigen, der vom Host geschrieben wurde, wählen Sie Optionen **primäre TIERED STORAGE** und **Primäre lokal FIXIERT Speicher verwendet** . In der Abbildung sind beide Optionen ausgewählt. das Diagramm zeigt daher Lagermengen Cloud und primären Speicher. Beachten Sie, dass alle primären Speicher vor der Installation von Update 2 verwendet die **Primäre TIERED STORAGE verwendet** Linie dargestellt wird.
     - Verwenden Sie im Dropdown-Menü in der rechten oberen Ecke des Diagramms 1 Woche, 1 Monat, 3 Monate oder 1-Jahres-Zeitraum an. Beachten Sie, dass der obersten Ebene Diagramm nur einmal täglich aktualisiert wird und daher den vorherigen Tag summiert wird.

     Weitere Informationen finden Sie unter [Verwenden der StorSimple Manager-Dienst das Gerät StorSimple überwachen](storsimple-monitor-device.md).

- **Verwendung-Übersicht** – im Übersichtsbereich **Verwendung** primären Speicher verwendet, der bereitgestellte Speicherplatz und die maximale Speicherkapazität Ihres Geräts angezeigt. Vergleichen Sie diese Zahlen zur Verwendung auf maximal verfügbaren, sehen auf einen Blick Sie ggf. weiteren Speicherplatz zu erhalten. Beachten Sie, dass in dieser Übersicht alle 15 Minuten aktualisiert wird und aufgrund in Aktualisierungsintervall kann zeigen andere Zahlen als dargestellt im Diagrammbereich oben, die täglich aktualisiert wird. Weitere Informationen finden Sie unter [Verwenden der StorSimple Manager-Dienst das Gerät StorSimple überwachen](storsimple-monitor-device.md).


- **Warnung** – **Warnungen** Bereich enthält einen Überblick über die für Ihr Gerät. Alarme werden nach Schweregrad gruppiert und Anzahl die Anzahl der Warnungen jeden Schweregrad angegeben. Auf den Warnungsschweregrad öffnet eine bewertete Ansicht der Registerkarte Alerts nur Alarme, Schweregrad für dieses Gerät anzeigen.

- **Aufträge** **im Projektbereich** ist das Ergebnis der letzten Auftragsaktivität. Dies kann Ihnen versichern, dass das System funktioniert wie erwartet, oder Sie wissen lassen, dass Sie Maßnahmen. Klicken Sie weitere Informationen zum zuletzt abgeschlossenen Aufträge **Aufträge in den letzten 24 Stunden wurde erfolgreich abgeschlossen**.

- **Blick** Bereich auf der rechten Seite des Dashboards bietet nützliche Informationen wie das Modell, Seriennummer, Status, Beschreibung und Anzahl der Volumes.

Sie können auch konfigurieren Failover und angeschlossene Initiatoren vom Gerät Dashboard anzeigen.

Allgemeinen Aufgaben, die auf dieser Seite möglich sind:

- Angeschlossene Initiatoren anzeigen

- Seriennummer des Geräts gefunden

- Zielgerät IQN suchen

## <a name="view-connected-initiators"></a>Angeschlossene Initiatoren anzeigen

Sie können die iSCSI-Initiatoren anzeigen, die mit dem Gerät verbunden sind, über den Link **View verbunden Initiatoren** im Bereich **Blick** Gerät Dashboard bereitgestellt. Diese Seite enthält eine tabellarische Auflistung der Initiatoren mit dem Gerät hergestellt werden. Für jeden Initiator angezeigt:

- ISCSI qualifizierten Namen (IQN) des verbundenen Initiator.

- Der Name der Access Control Datensatz (ACR), der diesen verbundenen Initiator ermöglicht.

- Die IP-Adresse des Initiators verbunden.

- Der Initiator auf dem Speichergerät an Netzwerkschnittstellen. Diese reichen von Daten 0 Daten 5.

- Alle Datenträger, die der verbundene Initiator gemäß der aktuellen Konfiguration ACR zugreifen darf.

Wenn unerwartete Initiatoren in dieser Liste sehen oder nicht die erwartete sehen überprüfen Sie die ACR-Konfiguration. Maximal 512 Initiatoren kann mit Ihrem Gerät verbinden.

## <a name="find-the-device-serial-number"></a>Seriennummer des Geräts gefunden

Seriennummer des Geräts möglicherweise beim Konfigurieren von Microsoft Multipfad-e/a (MPIO) auf dem Gerät. Die folgenden Schritte Seriennummer des Geräts zu finden.

#### <a name="to-find-the-device-serial-number"></a>Seriennummer des Geräts gefunden

1. Navigieren Sie zum **Geräte** > **Dashboard**.

2. Suchen Sie im rechten Bereich des Dashboards Bereich **Blick** .

3. Bildlauf und die Seriennummer.

## <a name="find-the-device-target-iqn"></a>Zielgerät IQN suchen

Möglicherweise Zielgerät IQN Challenge Handshake Authentication Protocol (CHAP) auf dem StorSimple-Gerät konfigurieren. Die folgenden Schritte Zielgerät IQN finden.

### <a name="to-find-the-device-target-iqn"></a>Zielgerät IQN gefunden

1. Navigieren Sie zum **Geräte** > **Dashboard**.

1. Suchen Sie im rechten Bereich des Dashboards Bereich **Blick** .

1. Blättern Sie nach unten, und suchen Sie Ziel IQN.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [StorSimple Manager Service Dashboard](storsimple-service-dashboard.md).
- Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md)
