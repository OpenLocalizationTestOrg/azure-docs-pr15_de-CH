<properties 
   pageTitle="StorSimple-Gerät überwachen | Microsoft Azure"
   description="Beschreibt, wie mit der StorSimple Manager-Dienst e/a-Leistung, Auslastung Netzwerkdurchsatz und Leistung überwachen."
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
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>Verwenden des StorSimple Manager-Dienstes das Gerät StorSimple überwachen 

## <a name="overview"></a>Übersicht

StorSimple Manager Service können Sie bestimmte Geräte innerhalb der Projektmappe StorSimple überwachen. Sie können benutzerdefinierte Diagramme auf Basis von e/a-Leistung, Auslastung Netzwerkdurchsatz und Leistungsdaten Gerät erstellen. 

Wählen Sie zum Anzeigen von Überwachungsinformationen für ein bestimmtes Gerät im klassischen Azure-Portal StorSimple Manager-Dienst. Klicken Sie auf die Registerkarte **Monitor** , und wählen Sie dann aus der Liste der Geräte. Die Seite " **Monitor** " enthält die folgenden Informationen.

## <a name="io-performance"></a>E/a-Leistung 

**E/a-** Tracks Leistungskennzahlen der Anzahl der Lese- und Schreibvorgänge zwischen-iSCSI-Initiator-Schnittstellen auf den Hostserver und das Gerät oder das Gerät und der Cloud. Diese Leistung kann für ein bestimmtes Volume, ein bestimmtes volumecontainer oder Container für alle Datenträger gemessen werden.

Das folgende Diagramm zeigt die e/a für den Initiator an Ihr Gerät für alle Volumes ein Produktion. Metriken gezeichnet werden und Schreiben Bytes pro Sekunde, und e/a-Schreibvorgänge pro Sekunde zu lesen und und Schreibwartezeiten.

![E/a-Leistung von Initiator Gerät](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Für dieses Gerät sind e/a-Operationen für die Daten vom Gerät in die Cloud für alle volumecontainer gezeichnet. Auf diesem Gerät hat werden nur in der linearen und nichts in der Cloud. Es gibt keine Lese-/ Schreibvorgänge vom Gerät in die Cloud auftritt. Daher sind die Spitzen im Diagramm im Abstand von 5 Minuten, die der Häufigkeit entspricht mit der Takt zwischen dem Gerät und der Dienst aktiviert ist. 

![E/a-Leistung von Gerät in die cloud](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


Für dieses Gerät wurde eine Cloud für Volumendaten ab 14 Uhr Momentaufnahme. Dadurch Daten vom Gerät in die Cloud. Lesen Schreiben war in der Cloud in diesem Zeitraum. E/a-Diagramm zeigt einen Spitzenwert in verschiedenen Metriken entspricht der Zeit, wenn die Momentaufnahme erstellt wurde. 

![E/a-Leistung Gerät nach Cloud Snapshot cloud](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## <a name="capacity-utilization"></a>Kapazitätsauslastung 

**Auslastung** verfolgt Kennzahlen Menge Speicherplatz von Volumes, volumecontainer oder Gerät verwendet wird. Sie können Berichte auf Grundlage der Auslastung der primären Speicher, Cloud-Speicher oder der Speicher des Geräts erstellen. Die Auslastung kann auf ein bestimmtes Volume, ein bestimmtes volumecontainer oder alle volumecontainer gemessen werden.


Primäre, Cloud und Speicherkapazität Geräts kann wie folgt beschrieben:

###<a name="primary-storage-capacity-utilization"></a>Primäre Speicher-Auslastung
 
Diese Diagramme zeigen die Daten in StorSimple-Volumes dedupliziert und komprimiert die Daten. Sie können die primären Speicher-Auslastung alle Volumes oder für ein einzelnes Volume anzeigen.

Beim primären Speicher-Volume Kapazität Auslastung für alle Volumes und jede einzelne Volumes Diagramme und die primären Daten in beiden Fällen fassen möglicherweise ein Konflikt zwischen den zwei Zahlen. Gesamten primären Daten auf allen Volumes können nicht auf die Summe der primären Daten der einzelnen Volumes hinzufügen. Dies kann eine der folgenden haben:

- **Snapshotdaten für alle Volumes enthalten**: Dies gilt nur, wenn Sie vor Update 3 Version. Primären Daten für alle Volumes angezeigt, ist die Summe der primären Daten für jedes Volume und die Snapshot-Daten. Die primären Daten für ein bestimmtes Volume entspricht der Menge der Daten auf dem Volume (und das entsprechende Volume Snapshot Daten).

    Dies kann auch durch folgende Gleichung erklärt:

    *Primäre Daten (alle Volumes) = Summe (primäre Daten (Volume i) + Snapshot Daten (Volume i))*
    
    *Wo Primärdaten (Volume i) = Größe der primären Daten auf Volume i*
 
    Wenn Snapshots über den Dienst gelöscht werden, erfolgt die Löschung asynchron im Hintergrund. Es dauert einige Zeit Volumegröße Daten nach Löschung Snapshot aktualisiert werden. 

    Update 3 oder höher ausgeführt, ist die Snapshot-Daten nicht in der Volume-Daten enthalten. Und die primäre Nutzung wird wie folgt berechnet:

    * Primäre Daten (alle Volumes) = Summe (primärer Daten (Volume i)
    
    *Wo Primärdaten (Volume i) = Größe der primären Daten auf Volume i*
 
- **Volumes mit deaktiviert alle Volumes enthalten**: haben Sie Volumes auf dem Gerät für die Überwachung ist ausgeschaltet, die Überwachungsdaten für diese einzelne Volumes wird nicht in den Diagrammen. Die Daten für alle Volumes im Diagramm enthält jedoch die Volumes für die Überwachung deaktiviert ist. 
 
- **Gelöscht mit live zugeordneten Backups für alle Volumes enthalten**: Datenträger mit Daten werden gelöscht, aber zugeordneten Snapshots bestehen, wenn ein Konflikt auftreten.

- **Gelöschte Volumes für alle Volumes enthalten**: In einigen Fällen alten Volumes existiert, obwohl diese gelöscht wurden. Auswirkung eines Löschvorgangs nicht angezeigt und das Gerät möglicherweise niedriger verfügbare Kapazität angezeigt. Wenden Sie sich an Microsoft Support, um diese Volumes entfernt werden müssen.

Die folgenden Diagramme zeigen Primärspeicher Auslastung eines Geräts StorSimple vor und nach cloudmomentaufnahme erstellt wurde. Wie dieser Volume-Daten sollten ein Snapshot Cloud primären Speicher nicht ändern. Wie Sie sehen können, zeigt das Diagramm keinen Unterschied zwischen primären Auslastung durch Cloud Momentaufnahme. Cloud-Snapshot gestartet um 2:00 Uhr auf dem Gerät.

![Primäre Auslastung before-Snapshot der cloud](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Primäre Auslastung nach Cloud snapshot](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Ausführen Update 2 oder höher, Sie können brechen Primärspeicher Auslastung von einem einzelnen Volume, alle Volumes, alle tiered Volumes und alle lokalen Volumes wie unten dargestellt. Aufschlüsselung nach alle lokalen Volumes können Sie schnell feststellen, wie viel der lokalen Ebene verwendet wird.

![Primäre Auslastung für alle lokalen volumes](./media/storsimple-monitor-device/localvolumes.png)


###<a name="cloud-storage-capacity-utilization"></a>Cloud-Speicher-Auslastung

Diese Diagramme zeigen die Cloud-Speicher verwendet. Diese Daten werden dedupliziert und komprimiert. Dieser Betrag umfasst Cloud Snapshots die Daten enthalten, die im primären Laufwerk wiedergegeben ist nicht für Archivierung Legacy- oder benötigt werden. Vergleichen der primären und cloud-Speicher Verbrauch um eine Vorstellung der Datenrate Verringerung zwar die Nummer nicht genau. Die folgenden Diagramme zeigen Cloud Storage Auslastung ein StorSimple Gerät und die cloudmomentaufnahme erstellt wurde. Cloud-Snapshot Beginn um 2:00 Uhr auf dem Gerät und Kapazitätsauslastung Cloud sehen geschossen zur gleichen Zeit ab Version 4.04 GB 5,73 MB erhöhen.

![Cloud-Auslastung before-Snapshot der cloud](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Cloud-Auslastung nach Cloud snapshot](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


###<a name="device-storage-capacity-utilization"></a>Gerät Speicher-Auslastung

Diese Diagramme zeigen die Gesamtauslastung für das Gerät mehr als primärem Speicher-Auslastung, weil lineare SSD-Ebene enthält. Diese Ebene enthält eine Datenmenge, die auch auf das Gerät die anderen Ebenen. Die Kapazität in der linearen SSD wird aus-und wieder eingeschaltet, bei neue Daten die alten Daten verschoben werden auf der Festplatte (wobei es dedupliziert und komprimiert) und der Cloud.

Im Laufe der Zeit erhöht primäre Auslastung und geräteauslastung wahrscheinlich zusammen bis Daten beginnt, in die Cloud gestuft werden soll. An diesem Punkt Device-Auslastung wahrscheinlich Plateau beginnt jedoch primäre Auslastung wird erhöht, da mehr Daten geschrieben werden.

Die folgenden Diagramme zeigen Primärspeicher Auslastung eines Geräts StorSimple vor und nach cloudmomentaufnahme erstellt wurde. Cloud-Snapshot wurde um 2:00 Uhr ebenso geräteauslastung gleichzeitig verringern. Gerät Speicher-Kapazitätsauslastung ging von 11,58 GB 7,48 GB. Dies bedeutet, dass wahrscheinlich nicht komprimierten Daten im linearen SSD-Tier dedupliziert, komprimiert und in HDD-Tier verschoben wurde. Beachten Sie, dass wenn das Gerät eine große Datenmenge in die SSD und HDD-Stufen bereits dieser Rückgang Sie nicht sehen eventuell. In diesem Beispiel hat das Gerät eine kleine Datenmenge.

![Device-Auslastung before-Snapshot der cloud](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Device-Auslastung nach Cloud snapshot](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## <a name="network-throughput"></a>Netzwerkdurchsatz

**Netzwerkdurchsatz** verfolgt Kennzahlen, die Datenmenge von iSCSI-Initiator-Netzwerkschnittstellen auf dem Hostserver und dem Gerät und zwischen dem Gerät und der Cloud. Sie können diese Metrik auf Ihrem Gerät für jeden iSCSI-Netzwerkschnittstellen überwachen.

Die folgenden Diagramme zeigen den Netzwerkdurchsatz Daten 0 und 4 Daten sowohl 1 Gigabit-Netzwerkschnittstellen auf dem Gerät. In diesem Fall wurde 0 Cloud datengestützte Daten 4 iSCSI aktiviert war. Sie können die eingehenden und ausgehenden Datenverkehr für Ihr StorSimple-Gerät finden Sie unter. Flache Linie im Diagramm ab 24 Uhr ist darauf zurückzuführen, sammeln Sie Daten nur alle 5 Minuten und sollte ignoriert werden. 

![Netzwerkdurchsatz Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Netzwerkdurchsatz Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## <a name="device-performance"></a>Leistung 

**Leistung** überwacht Metriken bezüglich der Leistung Ihres Geräts. Das folgende Diagramm zeigt die CPU-Auslastung Statistiken für ein Gerät in der Produktion.

![CPU-Auslastung für Gerät](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [StorSimple Manager Service Gerät Dashboard](storsimple-device-dashboard.md).

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
