<properties
   pageTitle="StorSimple-Volumes verwalten | Microsoft Azure"
   description="Erläutert das Hinzufügen, ändern, überwachen und StorSimple-Volumes löschen und zu offline ggf.."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/11/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>Verwenden des StorSimple Manager-Dienstes zu managen

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erläutert, wie den StorSimple Manager-Dienst erstellen und Verwalten von Datenträgern auf dem StorSimple-Gerät StorSimple virtuelles Gerät.

Der StorSimple-Manager ist eine Erweiterung der klassischen Azure-Portal, mit dem Sie die Projektmappe StorSimple über eine einzige Web-Oberfläche verwalten kann. Neben Volumes, können Sie den StorSimple Manager-Dienst erstellen StorSimple Dienste verwalten, anzeigen und Verwalten von Geräten, Alerts anzeigen anzeigen und Verwalten von backup-Policies und der Sicherungskatalog.

> [AZURE.NOTE] Azure StorSimple kann Thin Provisioning Volumes erstellen. Erstellen kann nicht vollständig bereitgestellt oder teilweise bereitgestellte Volumes auf einem System Azure StorSimple.
>
> Thin provisioning ist ein Virtualisierungstechnologie in dem verfügbaren Speicher physischen Ressourcen angezeigt wird. Statt Reservierung ausreichend Speicherplatz, verwendet Azure StorSimple genug Speicherplatz zum aktuellen Anforderungen thin provisioning. Flexible Art der Cloud-Speicher erleichtert dies, da Azure StorSimple erhöhen oder verringern Cloud-Speicher ändern Anforderungen.

## <a name="the-volumes-page"></a>Der Datenträger-Seite

Seite **Volumes** können Sie die Speicher-Volumes verwalten, die auf dem Gerät Microsoft Azure StorSimple für Ihre Initiatoren (Server) bereitgestellt werden. Die Liste der Volumes auf dem StorSimple-Gerät angezeigt.

 ![Datenträger-Seite](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Ein Volume besteht aus einer Reihe von Attributen:

- **Name** – beschreibender Namen, muss eindeutig sein und den Datenträger identifizieren. Dieser Name wird auch in Kontrollberichten Filtern auf einem bestimmten Volume verwendet.

- **Status** – kann online oder offline. Wenn ein Volume bei offline ist nicht sichtbaren Initiatoren (Server), die Zugriff auf das Volume verwenden dürfen.

- **Kapazität** – gibt an, wie groß das Volume, wie vom Initiator (Server). Kapazität gibt den Gesamtbetrag der Daten, die vom Initiator (Server) gespeichert werden können. Volumes dünn bereitgestellt, und Daten werden dedupliziert. Dies bedeutet, dass das Gerät physischen Speicherkapazität intern oder in der Cloud nach konfigurierten Datenträgerkapazität vorbelegen nicht. Die Kapazität reserviert und bei Bedarf verbraucht.

- **Typ** – Volume möglich tiered oder Archivierung (ein Untertyp gestuft)

- **Zugriff** – gibt die Initiatoren (Server), die auf diesen Datenträger zugreifen. Initiatoren nicht Access Steuerdatensatz (ACR) gehören, die dem Volume zugeordnet werden das Volume nicht angezeigt.

- **Überwachung** – gibt an, ob ein Volume überwacht wird. Ein Volume haben Überwachung bei der Erstellung standardmäßig aktiviert. Überwachung wird jedoch, für einen Klon Volume deaktiviert. Zum Aktivieren der Überwachung für ein Volume Anweisungen der Monitor ein Volume.

Die häufigsten Aufgaben mit sind:

- Hinzufügen eines Datenträgers
- Ein Volume bearbeiten
- Löschen eines Datenträgers
- Ein Volume offline schalten
- Überwachen eines Datenträgers

## <a name="add-a-volume"></a>Hinzufügen eines Datenträgers

Sie [erstellt ein Volume](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) während der Bereitstellung der Projektmappe StorSimple. Ein Volume ist ein ähnliches Verfahren.

### <a name="to-add-a-volume"></a>Hinzufügen ein Datenträgers

1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volumecontainer** .

2. Wählen Sie einen volumecontainer und klicken Sie auf den Pfeil in der entsprechenden Zeile auf dem Container zugeordneten Volumes zugreifen.

3. Klicken Sie am unteren Rand der Seite **Hinzufügen** . Startet ein Assistenten hinzufügen.

     ![Assistenten Sie Volume-Standardeinstellungen](./media/storsimple-manage-volumes/AddVolume1.png)

4. Volume-Assistenten unter **Standardeinstellungen**folgendermaßen Sie vor:

  1. Geben Sie einen **Namen** für den Datenträger.
  2. Geben Sie die **Kapazität bereitgestellt** für den Datenträger, GB oder TB. Die Kapazität muss zwischen 1 GB und 64 TB für ein physikalisches Gerät sein. Die maximale Kapazität für ein Volume auf einem virtuellen Gerät StorSimple bereitgestellt werden kann beträgt 30 TB.
  3. Wählen Sie den **Verwendungstyp** für den Datenträger. Bei Verwendung von tiered Volume für archivierte Daten ändert **dieses Volume für weniger häufig benötigte Daten Archivierung verwenden** das Kontrollkästchen Deduplizierung Chunk-Größe für den Datenträger auf 512 KB. Wenn diese Option nicht aktiviert, wird das entsprechende tiered Volume Chunk-Größe von 64 KB verwenden. Deduplizierung Ausschnitt vergrößern kann das Gerät die Übertragung von großen Archivdaten in die Cloud zu beschleunigen. (Mehrstufige Volumes wurden früher primäre Volumes bezeichnet.)
  5. Klicken Sie auf das Pfeilsymbol ![Pfeil](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)Seite **Weitere Einstellungen** zu.

        ![Fügen Sie weitere Assistenten-Lautstärke](./media/storsimple-manage-volumes/AddVolume2.png)

5. Fügen Sie unter **Zusätzliche Einstellungen**einen neuen Access Control Datensatz (ACR hinzu):

  1. Einen Datensatz auf Steuerelement (ACR) aus der Dropdown-Liste auswählen Alternativ können Sie eine neue ACR hinzufügen. ACRs bestimmen, welche Hosts die Volumes zugreifen können, durch entsprechende Host IQN mit angezeigten Datensatzes.
  2. Wir empfehlen, eine Sicherung **einen Standard-Backup für dieses Volume aktivieren** das Kontrollkästchen zu aktivieren.
   3. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-manage-volumes/HCS_CheckIcon.png) Das Volume mit den angegebenen Einstellungen erstellt.

Neue Volume ist jetzt einsatzbereit.

## <a name="modify-a-volume"></a>Ein Volume bearbeiten

Ändern eines Volumes erweitern oder Ändern der Hosts, die auf den Datenträger zugreifen müssen.

> [AZURE.IMPORTANT]
>
> - Wenn die Volumegröße auf dem Gerät ändern, muss die Volume-Größe auf dem Host geändert werden.
> - Die hier beschriebenen Schritte hostseitige sind für Windows Server 2012 (2012R2). Werden verschiedene Verfahren für Linux oder anderen Host-Betriebssysteme. Lesen Sie die Host-Anweisungen Volume auf einem Server unter einem anderen Betriebssystem ändern.

### <a name="to-modify-a-volume"></a>So ändern Sie ein volume

1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volume** . Diese Seite enthält in Tabellenform die volumecontainer, die dem Gerät zugeordnet sind.

2. Wählen Sie einen volumecontainer, und klicken sie zum Anzeigen der Liste aller Volumes innerhalb des Containers.

3. Wählen Sie auf der Seite **Datenträger** ein Volume, und klicken Sie auf **Ändern**.

4. Assistenten Volume ändern unter **Standardeinstellungen**können Folgendes:

  - Bearbeiten Sie **Name** und **Typ** tiered Volume zu einem Archivierung **verwenden dieses Volume für weniger häufig benötigte Daten Archivierung** das Kontrollkästchen Deduplizierung Chunk-Größe für den Datenträger auf 512 KB ändern ändern möchten.
  - Erhöhen der **Kapazität bereitgestellt**. Die **Kapazität bereitgestellt** kann nur erhöht werden. Ein Volume kann nicht verkleinert werden, nachdem es erstellt wurde.

    > [AZURE.NOTE] Nach einem Volume zugeordnet ist, können Sie volumecontainer ändern.

5. Unter **Einstellungen**können Sie Folgendes ein:

  - ACRs, ist das Volume offline bearbeiten Wenn der Datenträger online ist, müssen Sie sie zuerst offline schalten. Finden Sie die Schritte in [einem Volumen offline](#take-a-volume-offline) ACR ändern.
  - Ändern Sie die Liste der ACRs, nachdem das Volume offline ist.

    > [AZURE.NOTE] Sie können die Option **aktivieren eine standardmäßige Sicherung für diesen Datenträger** für das Volume nicht ändern.

6. Speichern Sie auf das Kontrollkästchen Symbol ![Kontrollkästchen-Symbol](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Azure-Verwaltungsportal zeigt Fehlermeldung Volume aktualisieren. Es wird eine Erfolgsmeldung angezeigt, wenn das Volume erfolgreich aktualisiert wurde.

7. Wenn Sie ein Volume erweitern, gehen Sie auf dem Windows-Host-Computer:

   1. **Computermanagement**zu ->**Disk Management**.
   2. Maustaste auf **Datenträger** und wählen Sie **Datenträger aus**.
   3. Wählen Sie in der Liste der Laufwerke Volume Aktualisierung, mit der rechten Maustaste und wählen Sie **Volume erweitern**. Volume erweitern-Assistent wird gestartet. Klicken Sie auf **Weiter**.
   4. Führen Sie den Assistenten die Standardwerte. Nachdem der Assistent abgeschlossen ist, sollte das Volume die erhöhte Größe anzeigen.

![Video verfügbar](./media/storsimple-manage-volumes/Video_icon.png) **Video verfügbar**

Ein Video, das veranschaulicht, wie ein Volume zu erweitern, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Ein Volume offline schalten

Sie müssen einen Datenträger offline schalten, wenn Sie ihn ändern oder löschen möchten. Wenn ein Volume offline ist, ist nicht für Lese-/ Schreibzugriff. Sie müssen auf dem Server und auf dem Gerät das Volume offline nehmen. Die folgenden Schritte ein Volume offline geschaltet werden.

### <a name="to-take-a-volume-offline"></a>Ein Volume offline schalten

1. Stellen Sie sicher, dass der betreffende Datenträger nicht vor offline geschaltet ist.

2. Zuerst das Volume offline auf dem Host. Dadurch werden potenzielles Risiko der Beschädigung von Daten auf dem Datenträger. Bestimmte Schritte finden Sie in der Anleitung für das Hostbetriebssystem.

3. Nachdem der Server offline ist, übernehmen Sie die Lautstärke Gerät offline durch Ausführen der folgenden Schritte:

  1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volumecontainer** . Die Registerkarte **Volumecontainer** enthält in Tabellenform die volumecontainer, die dem Gerät zugeordnet sind.
  2. Wählen Sie einen volumecontainer, und klicken sie zum Anzeigen der Liste aller Volumes innerhalb des Containers.
  3. Wählen Sie ein Volume, und klicken Sie auf **offline schalten**.
  4. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**. Das Volume sollte jetzt offline.

    Nachdem ein Datenträger offline ist, wird im **Online schalten** die Option.

> [AZURE.NOTE] **Offline nehmen** Befehl sendet eine Anforderung an das Gerät das Volume offline geschaltet werden. Wenn Hosts noch das Volume verwenden, das Volume offline geschaltet wird nicht dadurch unterbrochene Verbindung

## <a name="delete-a-volume"></a>Löschen eines Datenträgers

> [AZURE.IMPORTANT] Sie können ein Volume nur dann, wenn es offline ist.

Gehen Sie zum Löschen eines Volumes.

### <a name="to-delete-a-volume"></a>Löschen eines Volumes

1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volumecontainer** .

2. Wählen Sie den volumecontainer mit der Datenträger, den Sie löschen möchten. Klicken Sie auf den volumecontainer **Volumes** Seite.

3. Alle mit diesem Container verknüpften Volumes werden in Tabellenform angezeigt. Überprüfen Sie den Status des Volumes, das Sie löschen möchten. Ist das zu löschende Volume nicht offline schalten Sie sie offline, die Schritte in [einem Volume offline nehmen](#take-a-volume-offline).

4. Nachdem das Volume offline ist, klicken Sie am unteren Rand der Seite **Löschen** .

5. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**. Das Volume jetzt gelöscht und **Volumes** Seite zeigt die aktualisierte Liste der Volumes innerhalb des Containers.

## <a name="monitor-a-volume"></a>Überwachen eines Datenträgers

Volume Überwachung können Sie I/O-bezogene Statistiken für ein Volume. Überwachung wird standardmäßig die ersten 32 Datenträger aktiviert, die Sie erstellen. Überwachung der zusätzlichen ist standardmäßig deaktiviert. Überwachung der geklonten Volumes wird ebenfalls standardmäßig deaktiviert.

Führen Sie die folgenden Schritte zum Aktivieren oder Deaktivieren der Überwachung für ein Volume.

### <a name="to-enable-or-disable-volume-monitoring"></a>Aktivieren oder Deaktivieren der Überwachung von Festplattenvolumes

1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volumecontainer** .

2. Wählen Sie volumecontainer, in dem sich das Volume befindet, und klicken Sie dann auf den volumecontainer **Volumes** Seite.

3. Alle mit diesem Container verknüpften Volumes sind in der Tabellenanzeige aufgeführt. Klicken Sie auf und wählen Sie das Volume oder Volume-Clone.

4. Klicken Sie am unteren Rand der Seite **Ändern**.

5. Wählen Sie im Assistenten bearbeiten unter **Standardeinstellungen**aus Dropdown- **Überwachung** **Aktivieren** oder **Deaktivieren** .

    ![Ein Volume Standardeinstellungen ändern](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [Klon eines StorSimple-Volumes](storsimple-clone-volume.md).

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
