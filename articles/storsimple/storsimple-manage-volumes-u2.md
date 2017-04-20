<properties
   pageTitle="Verwalten Sie Ihre StorSimple-Volumes (U2) | Microsoft Azure"
   description="Erläutert das Hinzufügen, ändern, überwachen und StorSimple-Volumes löschen und zu offline ggf.."
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
   ms.workload="NA"
   ms.date="10/28/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>Verwenden des StorSimple Manager-Dienstes (Update 2) managen

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Übersicht

Diese Anleitung erläutert, wie den StorSimple Manager-Dienst erstellen und Verwalten von Datenträgern auf dem StorSimple-Gerät StorSimple virtuelles Gerät mit Update 2 installiert.

Der StorSimple-Manager ist eine Erweiterung im klassischen Azure-Portal, mit dem Sie die Projektmappe StorSimple über eine einzige Web-Oberfläche verwalten kann. Neben Volumes, können Sie den StorSimple Manager-Dienst erstellen StorSimple Dienste verwalten, anzeigen und Verwalten von Geräten, Alerts anzeigen anzeigen und Verwalten von backup-Policies und der Sicherungskatalog.

## <a name="volume-types"></a>Datenträgertypen

StorSimple-Volumes möglich:

- **Volumes lokal fixiert**: Daten in diese Volumes bleibt auf dem lokalen StorSimple-Gerät jederzeit.
- **Tiered-Volumes**: Daten in diese Volumes können in die Cloud verschütten.

Archivierung Volume ist ein mehrstufiges Volume. Deduplizierung Chunk größer zur Archivierung Volumes kann das Gerät die Cloud größere Datensegmente übertragen. 

Wenn erforderlich, Sie die Lautstärke ändern können lokal, Ebenen oder von lokalen gestuft. Weitere Informationen finden Sie im [Volume ändern](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Lokale volumes

Lokale Volumes vollständig bereitgestellte Volumes sind, die nicht Datenebenen in die Cloud garantiert somit lokalen primären Daten unabhängig von Cloud-Konnektivität. Daten auf lokalen Volumes ist nicht dedupliziert und komprimiert. Allerdings werden Snapshots lokale Volumes dedupliziert. 

Lokale Volumes werden vollständig bereitgestellt; Daher müssen Sie ausreichend Speicherplatz auf dem Gerät beim Erstellen. Sie können lokale Volumes bis zu einer maximalen Größe von 8 TB auf dem Gerät StorSimple 8100 und 20 TB auf dem Gerät 8600 bereitstellen. StorSimple reserviert verbleibenden lokalen Speicherplatz auf dem Gerät für Snapshots, Metadaten und Datenverarbeitung. Die Größe des lokalen Volumes auf maximal verfügbaren Speicherplatz erhöhen, jedoch können Sie die Größe eines Volumes nach der Erstellung nicht verringern.

Beim Erstellen von lokalen Volumes wird Platz für tiered Mengen reduziert. Umgekehrt gilt: haben Sie vorhandene mehrstufige Volumes Platz für lokal erstellen Volumes fixiert werden genannten Höchstmengen unter. Weitere Informationen zu lokalen Datenträgern finden Sie [häufig gestellte Fragen auf lokale Volumes](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Mehrstufige volumes

Mehrstufige Volumes sind Thin Provisioning, häufig benötigte Daten lokal auf dem Gerät bleibt und weniger häufig verwendeten Daten automatisch in die Cloud Ebenen. Thin provisioning ist ein Virtualisierungstechnologie in dem verfügbaren Speicher physischen Ressourcen angezeigt wird. Statt Reservierung ausreichend Speicherplatz, verwendet StorSimple genug Speicherplatz zum aktuellen Anforderungen thin provisioning. Flexible Art der Cloud-Speicher erleichtert dies, da StorSimple erhöhen oder verringern Cloud-Speicher ändern Anforderungen.

Bei Verwendung von tiered Volume für archivierte Daten ändert **dieses Volume für weniger häufig benötigte Daten Archivierung verwenden** das Kontrollkästchen Deduplizierung Chunk-Größe für den Datenträger auf 512 KB. Wenn diese Option nicht aktiviert, wird das entsprechende tiered Volume Chunk-Größe von 64 KB verwenden. Deduplizierung Ausschnitt vergrößern kann das Gerät die Übertragung von großen Archivdaten in die Cloud zu beschleunigen.

>[AZURE.NOTE] Archivierung mit einer Version vor dem Update 2 StorSimple erstellten Volumes werden als tiered Archivierung das Kontrollkästchen aktiviert.

### <a name="provisioned-capacity"></a>Bereitgestellte Kapazität

Finden Sie in der folgenden Tabelle bereitgestellten Kapazität für jedes Gerät und Volume. (Beachten Sie, dass lokale Volumes nicht auf einem virtuellen Gerät verfügbar sind.)

|             | Maximale Volumegröße tiered | Lokal maximale fixiert Volumegröße |
|-------------|----------------------------|------------------------------------|
| **Physische Geräte** |       |       |
| 8100                 | 64 TB | 8 TB |
| 8600                 | 64 TB | 20 TB |
| **Virtuelle Geräte**  |       |       |
| 8010                | 30 TB | N/A   |
| 8020               | 64 TB | N/A   |

## <a name="the-volumes-page"></a>Der Datenträger-Seite

Seite **Volumes** können Sie die Speicher-Volumes verwalten, die auf dem Gerät Microsoft Azure StorSimple für Ihre Initiatoren (Server) bereitgestellt werden. Die Liste der Volumes auf dem StorSimple-Gerät angezeigt.

 ![Datenträger-Seite](./media/storsimple-manage-volumes-u2/VolumePage.png)

Ein Volume besteht aus einer Reihe von Attributen:

- **Volume-Name** einen beschreibenden Namen, die eindeutig sein und bestimmen die Lautstärke. Dieser Name wird auch in Kontrollberichten Filtern auf einem bestimmten Volume verwendet.

- **Status** – kann online oder offline. Wenn ein Volume offline ist, ist es nicht sichtbar Initiatoren (Server), die Zugriff auf das Volume verwenden dürfen.

- **Kapazität** – gibt den Gesamtbetrag der Daten, die vom Initiator (Server) gespeichert werden können. Volumes lokal fixiert vollständig bereitgestellt und befinden sich auf dem Gerät StorSimple. Mehrstufige Volumes dünn bereitgestellt und die Daten werden dedupliziert Mit Thin Provisioning vorbelegen nicht Ihr Gerät physischen Speicherkapazität intern oder in der Cloud nach konfigurierten Datenträgerkapazität. Die Kapazität reserviert und bei Bedarf verbraucht.

- **Typ** – gibt an, ob das Volume **Tiered** (Standard) oder **lokal fixiert**ist.

- **Backup** – gibt an, ob eine Sicherung Standardrichtlinie für das Volume vorhanden ist.

- **Zugriff** – gibt die Initiatoren (Server), die auf diesen Datenträger zugreifen. Initiatoren nicht Access Steuerdatensatz (ACR) gehören, die dem Volume zugeordnet werden das Volume nicht angezeigt.

- **Überwachung** – gibt an, ob ein Volume überwacht wird. Ein Volume haben Überwachung bei der Erstellung standardmäßig aktiviert. Überwachung wird jedoch, für einen Klon Volume deaktiviert. Zum Aktivieren der Überwachung für ein Volume Anweisungen der [Monitor ein Volume](#monitor-a-volume). 

Verwenden Sie die Schritte in diesem Lernprogramm für die folgenden Aufgaben:

- Hinzufügen eines Datenträgers 
- Ein Volume bearbeiten 
- Lautstärke ändern
- Löschen eines Datenträgers 
- Ein Volume offline schalten 
- Überwachen eines Datenträgers 

## <a name="add-a-volume"></a>Hinzufügen eines Datenträgers

Sie [erstellt ein Volume](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) während der Bereitstellung der Projektmappe StorSimple. Ein Volume ist ein ähnliches Verfahren.

#### <a name="to-add-a-volume"></a>Hinzufügen ein Datenträgers

1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volumecontainer** .

2. Wählen Sie volumecontainer aus und doppelklicken auf das dem Container zugeordnete Volumes zugreifen.

3. Klicken Sie am unteren Rand der Seite **Hinzufügen** . Startet ein Assistenten hinzufügen.

     ![Assistenten Sie Volume-Standardeinstellungen](./media/storsimple-manage-volumes-u2/TieredVolEx.png)

4. Volume-Assistenten unter **Standardeinstellungen**folgendermaßen Sie vor:

  1. Geben Sie einen **Namen** für den Datenträger.
  2. Wählen Sie einen **Verwendungstyp** aus der Dropdownliste. Für Arbeitslasten, die Daten werden lokal auf dem Gerät jederzeit verfügbar, wählen Sie **Lokal fixiert**. Wählen Sie für alle anderen Daten **Tiered**. (**Tiered** ist die Standardeinstellung.)
  3. Wenn Sie **Tiered** in Schritt 2 ausgewählt haben, können **dieses Volume für weniger häufig benötigte Daten Archivierung** das Kontrollkästchen Archivierung Volume konfigurieren auswählen.
  4. Geben Sie für den Datenträger GB oder TB **Kapazität bereitgestellt** . Maximale Größe für jedes Gerät und Volumes finden Sie unter [Kapazität bereitgestellt](#provisioned-capacity) . Betrachten Sie die **Verfügbare Kapazität** bestimmen, wie viel Speicher auf dem Gerät verfügbar ist.

5. Klicken Sie auf das Pfeilsymbol![Pfeil](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Wenn Sie ein lokales Volume konfigurieren, sehen Sie die Meldung.

    ![Volume Nachricht ändern](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
   
5. Klicken Sie auf das Pfeilsymbol ![Pfeil](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)wieder zu der Seite **Weitere Einstellungen** .

    ![Fügen Sie weitere Assistenten-Lautstärke](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>

6. Fügen Sie unter **Zusätzliche Einstellungen**einen neuen Access Control Datensatz (ACR hinzu):
  
  1. Einen Datensatz auf Steuerelement (ACR) aus der Dropdown-Liste auswählen Alternativ können Sie eine neue ACR hinzufügen. ACRs bestimmen, welche Hosts die Volumes zugreifen können, durch entsprechende Host IQN mit angezeigten Datensatzes. Wenn Sie eine ACR nicht angeben, wird die folgende Meldung angezeigt.

        ![ACR angeben](./media/storsimple-manage-volumes-u2/SpecifyACR.png)

  2. Wir empfehlen das Kontrollkästchen **Standard Backup für dieses Volume aktivieren** .
  3. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Das Volume mit den angegebenen Einstellungen erstellt.

Neue Volume ist jetzt einsatzbereit.

>[AZURE.NOTE] Wenn Sie ein lokales Volume dann erstellen und einen anderen lokalen Volumes sofort danach Volumeerstellung Aufträge nacheinander ausführen. Der erste Datenträger erstellen Einzelvorgang muss abgeschlossen werden, vor dem Beginn der nächsten Einzelvorgang Volume erstellen.

## <a name="modify-a-volume"></a>Ein Volume bearbeiten

Ändern eines Volumes erweitern oder Ändern der Hosts, die auf den Datenträger zugreifen müssen.

> [AZURE.IMPORTANT] 
>
> - Wenn die Volumegröße auf dem Gerät ändern, muss die Volume-Größe auf dem Host geändert werden. 
> - Die hier beschriebenen Schritte hostseitige sind für Windows Server 2012 (2012R2). Werden verschiedene Verfahren für Linux oder anderen Host-Betriebssysteme. Lesen Sie die Host-Anweisungen Volume auf einem Server unter einem anderen Betriebssystem ändern. 

#### <a name="to-modify-a-volume"></a>So ändern Sie ein volume

1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volumecontainer** .

2. Wählen Sie volumecontainer aus, und doppelklicken Sie auf den Container zugeordneten Volumes anzeigen.

3. Wählen Sie ein Volume, und klicken Sie am unteren Rand der Seite auf **Ändern**. Der Assistent zum Ändern von Volume wird gestartet.

4. Assistenten Volume ändern unter **Standardeinstellungen**können Folgendes:

  - Bearbeiten Sie den **Namen**.
  - Konvertieren Sie den **Verwendungstyp** lokal fixiert, Ebenen oder von Ebenen, um lokal fixiert (Weitere Informationen finden Sie unter [Lautstärke ändern](#change-the-volume-type) ).
  - Erhöhen der **Kapazität bereitgestellt**. Die **Kapazität bereitgestellt** kann nur erhöht werden. Ein Volume kann nicht verkleinert werden, nachdem es erstellt wurde.

5. Unter **Weitere Optionen**können Sie die ACR ändern, ist das Volume offline. Wenn der Datenträger online ist, müssen Sie sie zuerst offline schalten. Finden Sie die Schritte in [einem Volumen offline](#take-a-volume-offline) ACR ändern.

    > [AZURE.NOTE] Sie können die Option **aktivieren eine Sicherung** für das Volume nicht ändern.

6. Speichern Sie auf das Kontrollkästchen Symbol ![Kontrollkästchen-Symbol](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Azure-Verwaltungsportal zeigt Fehlermeldung Volume aktualisieren. Es wird eine Erfolgsmeldung angezeigt, wenn das Volume erfolgreich aktualisiert wurde.

7. Wenn Sie ein Volume erweitern, gehen Sie auf dem Windows-Host-Computer:

   1. **Computermanagement**zu ->**Disk Management**.
   2. Maustaste auf **Datenträger** und wählen Sie **Datenträger aus**.
   3. Wählen Sie in der Liste der Laufwerke Volume Aktualisierung, mit der rechten Maustaste und wählen Sie **Volume erweitern**. Volume erweitern-Assistent wird gestartet. Klicken Sie auf **Weiter**.
   4. Führen Sie den Assistenten die Standardwerte. Nachdem der Assistent abgeschlossen ist, sollte das Volume die erhöhte Größe anzeigen.

    >[AZURE.NOTE] Wenn ein lokales Volume erweitern und dann fixiert anderen lokal Volume unmittelbar danach Volume-Erweiterung Aufträge nacheinander ausgeführt. Der erste Datenträger Erweiterung Auftrag muss vor dem Beginn der nächsten Einzelvorgang Volume-Erweiterung abgeschlossen.

![Video verfügbar](./media/storsimple-manage-volumes-u2/Video_icon.png) **Video verfügbar**

Ein Video, das veranschaulicht, wie ein Volume zu erweitern, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Lautstärke ändern

Sie können den Datenträgertyp aus Ebenen lokal fixiert oder von fixiert lokal zu tiered. Diese Konvertierung sollte jedoch nicht häufig. Konvertieren eines Datenträgers aus Gründen sind Ebenen lokal fixiert:

- Lokale Garantien hinsichtlich Verfügbarkeit und performance
- Eliminierung von Cloud Wartezeiten und Cloud-Konnektivitätsprobleme.

Normalerweise sind kleine vorhandenen Volumes, die Sie häufig zugreifen möchten. Ein lokales Volume wird bei Erstellung vollständig bereitgestellt. Wenn Sie ein gestufte Volume auf lokalen Volumes konvertieren, überprüft StorSimple haben Sie ausreichend Speicherplatz auf dem Gerät vor der Konvertierung. Sie haben nicht genügend Speicherplatz erhalten Sie eine Fehlermeldung und der Vorgang abgebrochen. 

> [AZURE.NOTE] Zunächst eine Konvertierung von lokal fixiert tiered unbedingt Platzbedarf der Arbeitslasten berücksichtigen. 

Sie möchten ein lokales Volume tiered Volume ändern, benötigen Sie zusätzlichen Speicherplatz auf anderen Volumes bereitzustellen. Wenn Sie die lokalen Volumes auf Ebenen, die verfügbare Kapazität auf dem Gerät durch die Größe der veröffentlichten Kapazität konvertieren. Verbindungsprobleme die Konvertierung eines Datenträgers aus dem lokalen Typ gestuften Typ verhindern, wird das lokale Volume aufweisen Eigenschaften eines gestuften Volumes bis die Konvertierung abgeschlossen ist. Deshalb, weil einige Daten in die Cloud verschüttet haben können. Verschütteten Daten weiterhin lokalen Speicherplatz auf dem Gerät zu belegen, die freigegeben werden kann, bis der Vorgang gestartet und abgeschlossen ist.

>[AZURE.NOTE] Konvertieren eines Datenträgers kann einige Zeit dauern und eine Konvertierung kann nicht abgebrochen werden, nachdem es gestartet wurde. Das Volume bleibt während der Konvertierung online und Backups nehmen jedoch nicht erweitern oder Wiederherstellen der Lautstärke während die Konvertierung stattfindet.  

Konvertierung eine mehrschichtige in lokalen Volumes kann die Leistung beeinträchtigen. Außerdem können folgende mehr Zeit benötigt wird, um die Konvertierung abzuschließen:

- Nicht genügend Bandbreite ist vorhanden.

- Keine aktuelle Sicherung ist vorhanden.

Zu minimieren, die folgenden Faktoren:

- Überprüfen Sie die Bandbreitenschränkung Richtlinien und sicherstellen Sie, dass eine dedizierte 40 Mbit/s Bandbreite verfügbar ist.
- Planen Sie die Konvertierung für Zeiten.
- Schnappschuss Cloud vor der Konvertierung.

Wenn Sie mehrere Volumes (unterstützt verschiedene Arbeitslasten) konvertieren, sollten Sie die Volume-Konvertierung priorisieren, dass höhere Priorität Volumes zuerst konvertiert werden. Beispielsweise sollten Sie vor dem Konvertieren von Datenträgern mit Datei-Freigabe-Workloads, die virtuelle Maschinen (VMs) hosten oder Volumes mit SQL Arbeitslasten konvertieren.

#### <a name="to-change-the-volume-type"></a>Lautstärke ändern

1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volumecontainer** .

2. Wählen Sie volumecontainer aus, und doppelklicken Sie auf den Container zugeordneten Volumes anzeigen.

3. Wählen Sie ein Volume, und klicken Sie am unteren Rand der Seite auf **Ändern**. Der Assistent zum Ändern von Volume wird gestartet.

4. Klicken Sie auf **Standardeinstellungen** ändern Sie Verwendung **Verwendungstyp** Dropdown-Liste den neuen Typ auswählen.

    - Wenn Sie den Typ **lokal**fixiert ändern, prüft StorSimple festzustellen, ob genügend Kapazität.
    - Ändern Sie den Typ auf **Tiered** und dieses Volume für archivierte Daten verwendet werden, aktivieren Sie das Kontrollkästchen **dieses Volume für weniger häufig benötigte Daten Archivierung verwenden** .

        ![Archiv-Kontrollkästchen](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)

5. Klicken Sie auf das Pfeilsymbol ![Pfeil](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) Seite **Weitere Einstellungen** zu. Konfigurieren von lokalen Volumes wird die folgende Meldung angezeigt.

    ![Volume Nachricht ändern](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)

6. Klicken Sie auf das Pfeilsymbol ![Pfeil](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) erneut, um fortzufahren.

7. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) um die Konvertierung zu starten. Azure-Portal wird eine Aktualisierung Volume angezeigt. Es wird eine Erfolgsmeldung angezeigt, wenn das Volume erfolgreich aktualisiert wurde.

## <a name="take-a-volume-offline"></a>Ein Volume offline schalten

Sie müssen einen Datenträger offline schalten, wenn Sie ihn ändern oder löschen möchten. Wenn ein Volume offline ist, ist nicht für Lese-/ Schreibzugriff. Sie müssen auf dem Server und auf dem Gerät das Volume offline nehmen. 

#### <a name="to-take-a-volume-offline"></a>Ein Volume offline schalten

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

#### <a name="to-delete-a-volume"></a>Löschen eines Volumes

1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volumecontainer** .

2. Wählen Sie den volumecontainer mit der Datenträger, den Sie löschen möchten. Klicken Sie auf den volumecontainer **Volumes** Seite.

3. Alle mit diesem Container verknüpften Volumes werden in Tabellenform angezeigt. Überprüfen Sie den Status des Volumes, das Sie löschen möchten. Ist das zu löschende Volume nicht offline schalten Sie sie offline, die Schritte in [einem Volume offline nehmen](#take-a-volume-offline).

4. Nachdem das Volume offline ist, klicken Sie am unteren Rand der Seite **Löschen** .

5. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**. Das Volume jetzt gelöscht und **Volumes** Seite zeigt die aktualisierte Liste der Volumes innerhalb des Containers.

    >[AZURE.NOTE]Wenn Sie ein lokales Volume löschen, kann Platz für neue Volumes nicht sofort aktualisiert. Der StorSimple Manager-Dienst aktualisiert den lokalen Speicherplatz regelmäßig. Wir empfehlen, warten Sie einige Minuten, bevor Sie das neue Volume erstellt.<br> Außerdem löschen lokal Volumes und weitere lokal löschen fixiert Volume unmittelbar danach Volume löschen Aufträge nacheinander ausführen. Der erste Datenträger Massenlöschungsauftrags muss vor dem Beginn der nächsten Volume Massenlöschungsauftrags abgeschlossen.
 
## <a name="monitor-a-volume"></a>Überwachen eines Datenträgers

Volume Überwachung können Sie I/O-bezogene Statistiken für ein Volume. Überwachung wird standardmäßig die ersten 32 Datenträger aktiviert, die Sie erstellen. Überwachung der zusätzlichen ist standardmäßig deaktiviert. Überwachung der geklonten Volumes wird ebenfalls standardmäßig deaktiviert.

Führen Sie die folgenden Schritte zum Aktivieren oder Deaktivieren der Überwachung für ein Volume.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Aktivieren oder Deaktivieren der Überwachung von Festplattenvolumes

1. Wählen Sie auf der Seite **Geräte** das Gerät Doppelklicken Sie darauf, und klicken Sie auf die Registerkarte **Volumecontainer** .

2. Wählen Sie volumecontainer, in dem sich das Volume befindet, und klicken Sie dann auf den volumecontainer **Volumes** Seite.

3. Alle mit diesem Container verknüpften Volumes sind in der Tabellenanzeige aufgeführt. Klicken Sie auf und wählen Sie das Volume oder Volume-Clone.

4. Klicken Sie am unteren Rand der Seite **Ändern**.

5. Wählen Sie im Assistenten bearbeiten unter **Standardeinstellungen**aus Dropdown- **Überwachung** **Aktivieren** oder **Deaktivieren** .

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie [Klon eines StorSimple-Volumes](storsimple-clone-volume.md).

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).

 
