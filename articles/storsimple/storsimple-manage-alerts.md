<properties 
   pageTitle="Anzeigen und Verwalten von StorSimple Alerts | Microsoft Azure"
   description="Beschreibt StorSimple beschriebener Schweregrad, Benachrichtigung konfigurieren, und mit der StorSimple Manager-Dienst Benachrichtigungen verwalten."
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
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>Verwenden des StorSimple Manager-Dienstes zu StorSimple Alerts verwalten

## <a name="overview"></a>Übersicht

Registerkarte **Alerts** in der StorSimple Manager-Dienst ermöglicht das Überprüfen und löschen StorSimple Gerät bezogenen Alarme in Echtzeit. Auf dieser Registerkarte können Sie die Gesundheit der StorSimple Geräte und Microsoft Azure StorSimple Lösung zentral überwachen.

In diesem Lernprogramm beschreibt allgemeine beschriebener Warnungsschweregrade und Benachrichtigung konfigurieren. Darüber hinaus enthält alert Kurzübersicht Tabellen können Sie schnell eine bestimmte Warnung und entsprechend reagieren.

![Seite Alerts](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Allgemeine beschriebener

Das Gerät StorSimple Alerts auf eine Reihe von Fehlerzuständen generiert. Es folgen die häufigsten beschriebener:

- **Hardwareprobleme** – diese Warnung informiert Sie über den Zustand Ihrer Hardware. Sie können Firmware-Upgrades erforderlich sind, wenn eine Netzwerkschnittstelle Probleme, oder es gibt ein Problem mit einem Datenlaufwerke kennen.

- **Verbindungsprobleme** – diese Alarme auftreten, wenn Probleme bei der Datenübertragung. Kommunikationsprobleme können während der Übertragung von Daten und von Azure Storage-Konto oder aufgrund der Verbindung zwischen den Geräten und der StorSimple Manager-Dienst auftreten. Kommunikationsprobleme sind am schwersten zu beheben, da so viele Fehlerquellen. Sie sollten immer zuerst überprüfen Sie Netzwerkkonnektivität und Internetzugang verfügbar sind, bevor auf Erweiterte Problembehandlung. Hilfe zur Problembehandlung finden Sie unter [Problembehandlung mit dem Cmdlet Verbindung testen](storsimple-troubleshoot-deployment.md).

- **Performance-Probleme** – diese Warnung tritt auf, wenn Ihr System nicht optimal, beispielsweise wenn es unter hoher Auslastung funktioniert.

Darüber hinaus sehen Sie Alerts Sicherheit, Updates oder Auftragsfehler beziehen.

## <a name="alert-severity-levels"></a>Warnungsschweregrade

Alerts sind verschiedene Schweregrade der Auswirkung, die die alert-Situation und eine Reaktion auf die Warnung. Die Schweregrade sind:

- **Kritisch** – diese Warnung wird als Antwort auf eine Bedingung, die den Erfolg des Systems auswirken. Aktion ist erforderlich, um sicherzustellen, dass die StorSimple Dienst nicht unterbrochen wird.

- **Warnung** – dieser Zustand kritisch, wenn nicht aufgelöst werden konnte. Sie untersuchen die Situation und keine Aktion erforderlich, um dieses Problem zu beheben.

- **Informationen** – diese Warnung enthält nützliche Informationen und Ihr System verwalten kann.

## <a name="configure-alert-settings"></a>Warnung konfigurieren

Sie können per e-Mail beschriebener aller Geräte StorSimple benachrichtigt werden möchten. Außerdem können Sie andere Benachrichtigungsempfänger identifizieren, durch Eingabe ihrer e-Mail-Adresse im Feld **andere e-Mail-Empfänger** , durch Semikolon getrennt.

>[AZURE.NOTE] Sie können maximal 20 e-Mail-Adressen pro Gerät eingeben.

Nach dem Aktivieren der e-Mail-Benachrichtigung für ein Gerät erhalten Mitglieder der Benachrichtigungsliste e-Mail jedes Mal eine wichtige Warnung auftritt. Die Nachrichten werden von *storsimple-alerts-noreply@mail.windowsazure.com* und die Warnung. Empfänger können **Abonnement** um sich von der e-Mail-Benachrichtigungsliste entfernen klicken.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>E-Mail Alerts für ein Gerät aktivieren

1. **Geräte**zur > **Konfigurieren** für das Gerät.

2. Legen Sie unter **Einstellungen**Folgendes:

    1. Wählen Sie **Ja**im Feld **e-Mail-Benachrichtigung senden** .

    2. Wählen Sie **Ja** , im Feld **e-Mail-Administratoren** zu Dienstadministrator und alle CO-Administratoren die Benachrichtigung erhalten.

    3. Geben Sie im Feld **Weitere e-Mail-Empfänger** die e-Mail-Adressen aller Empfänger, die die Benachrichtigung erhalten sollen. Geben Sie Namen im Format *someone@somewhere.com*. Verwenden Sie Semikolons zum Trennen von e-Mail-Adressen. Sie können maximal 20 e-Mail-Adressen pro Gerät konfigurieren. 

        ![Alarmkonfiguration Benachrichtigung](./media/storsimple-manage-alerts/AlertNotify.png)

3. Zum Senden einer Test-Benachrichtigung klicken Sie auf den Pfeil neben **e-Mail-Test**. Der StorSimple Manager-Dienst wird wie testbenachrichtigung leitet Nachrichten angezeigt. 

4. Wenn die folgende Meldung angezeigt wird, klicken Sie auf **OK**. 

    ![Alarme testen e-Mail gesendet](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Wenn die Test-Nachricht gesendet werden kann, wird der StorSimple Manager-Dienst eine entsprechende Meldung angezeigt. Klicken Sie auf **OK**, warten Sie einige Minuten und versuchen Sie Ihr Test-Benachrichtigung erneut senden. 

## <a name="view-and-track-alerts"></a>Alerts anzeigen und Nachverfolgen

StorSimple Manager Service Dashboard bietet einen Einblick in die Anzahl der Warnungen auf Ihren Geräten nach Schweregrad sortiert.

![Alerts-dashboard](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Auf den Schweregrad öffnet die Registerkarte **Alarme** . Die Ergebnisse enthalten Alarme, die diesem Schweregrad entsprechen.

![Alerts-Bericht Alarmtyp Gültigkeitsbereich](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Klicken in der Liste eine Warnung bietet zusätzliche Details für die Warnung zuletzt die Warnung gemeldet wurde, einschließlich der Anzahl der Warnung auf das Gerät und der empfohlenen Maßnahme zum Beheben der Warnung. Es ist einem wird auch die Hardwarekomponente erkannt.

![Warnung wird Hardware](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Sie können die Details der Warnung in einer Datei benötigen Sie die Informationen an Microsoft Support senden. Nach der Empfehlung und die lokalen Warnung aufgelöst sollte Warnung vom Gerät löschen Registerkarte **Alerts** auswählen und auf **Löschen**klicken. Um mehrere Warnungen löschen, wählen Sie jede Warnung klicken Sie auf eine beliebige Spalte außer der Spalte **Warnung** und klicken Sie dann auf **Deaktivieren Sie** nach Auswahl der Alarme gelöscht. Beachten Sie, dass einige Warnungen automatisch gelöscht werden, wenn das Problem behoben ist oder wenn die Warnung mit neuen Informationen aktualisiert.

Wenn Sie auf **Löschen**klicken, haben Sie die Möglichkeit, Kommentare zu der Warnung und die Schritte zur Behebung des Problems hat. Einige Ereignisse werden ausgelöst ein anderes Ereignis mit neuen Informationen vom System gelöscht. In diesem Fall wird die folgende Meldung angezeigt.

![Warnung löschen](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Sortieren und Überprüfung alerts

Unter Umständen effizienter Warnungen Berichten, sodass überprüft und in Gruppen löschen. Registerkarte **Alerts** kann außerdem bis zu 250 Warnungen anzeigen. Überschreiten die Anzahl der Warnungen werden nicht alle Warnungen in der Standardansicht angezeigt. Kombinieren Sie die folgenden Felder anpassen, welche Warnungen angezeigt werden:

- **Status** – Warnungen **aktiv** oder **deaktiviert** angezeigt. Aktive Warnungen werden noch auf Ihrem System ausgelöst, während gelöschte Warnung manuell von einem Administrator gelöscht oder wurden programmgesteuert gelöscht, da das System die Warnung mit neuen Informationen aktualisiert.

- **Schweregrad** – Warnungen alle Schweregrade (kritisch, Warnung, Information) oder nur einen bestimmten Schweregrad, z. B. nur kritische Alarme anzuzeigen.

- **Datenquellen** können aus allen Quellen Benachrichtigung anzeigen oder Alarme, die aus dem Dienst oder eine oder alle Geräte einschränken.

- **Zeitraum** – durch Angabe der **von-** und **bis-** Datum und Zeitstempel Sie Alerts Zeitraum sehen, denen Sie interessieren.

## <a name="alerts-quick-reference"></a>Alerts-Kurzübersicht

Die folgenden Tabellen sind einige der Microsoft Azure StorSimple-Alarme, sowie zusätzliche Informationen und Vorschläge gegebenenfalls auftreten können. StorSimple Gerät Alerts fallen in die folgenden Kategorien:

- [Cloud-Konnektivität alerts](#cloud-connectivity-alerts)

- [Cluster-Alarme](#cluster-alerts)

- [Disaster Recovery-Alarme](#disaster-recovery-alerts)

- [Hardware-Alarme](#hardware-alerts)

- [Warnung wegen fehlgeschlagener Job](#job-failure-alerts)

- [Lokales Volume alerts](#locally-pinned-volume-alerts)

- [Netzwerk-alerts](#networking-alerts)

- [Performance-Alarme](#performance-alerts)

- [Sicherheitshinweise](#security-alerts)

- [Support-Paket alerts](#support-package-alerts)

- [Update-alerts](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Cloud-Konnektivität alerts

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Verbindung zu <*Cloud Anmeldenamen*> kann nicht hergestellt werden.|Das Speicherkonto herstellen nicht.|Es sieht aus wie ein Konnektivitätsproblem mit Ihrem Gerät möglicherweise. Führen Sie die `Test-HcsmConnection` Cmdlet von Windows PowerShell-Schnittstelle für StorSimple auf dem Gerät zu identifizieren und beheben Sie das Problem. Wenn die Einstellung richtig sind, das Problem mit den Anmeldeinformationen des Speicherkontos möglicherweise für das die Warnung ausgelöst wurde. In diesem Fall verwenden das `Test-HcsStorageAccountCredential` -Cmdlet zum feststellen, ob Probleme beheben können.<ul><li>Überprüfen Sie die Netzwerkkonfiguration.</li><li>Überprüfen Sie Ihre Anmeldeinformationen speichern.</li></ul>|
|Wir haben nicht sofort aus dem Gerät <*Anzahl*> Minuten erhalten.|Gerät herstellen nicht.|Es sieht aus wie ein Konnektivitätsproblem mit dem Gerät vorhanden ist. Verwenden Sie die `Test-HcsmConnection` Cmdlet von Windows PowerShell-Schnittstelle für StorSimple auf dem Gerät zu identifizieren und beheben Sie das Problem oder wenden Sie sich an Ihren Netzwerkadministrator.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Cloud-Verbindung kann StorSimple Verhalten

Was geschieht, wenn Gerät StorSimple, die als Produktionssysteme Cloud Verbindung fehlschlägt?

Cloud-Konnektivität auf Ihrem Gerät StorSimple Produktion andernfalls kann je nach dem Gerät die folgenden auftreten: 

- **Für die lokalen Daten auf Ihrem Gerät**: für einige Zeit werden keine Unterbrechung und Lesevorgänge weiterhin bedient werden. Die Anzahl der ausstehenden i/o erhöht und eine Obergrenze übersteigt konnte jedoch die Lesevorgänge starten, fehlschlagen. 

    Je nach der Menge der Daten auf Ihrem Gerät weiterhin Schreibvorgänge den ersten Stunden nach der Unterbrechung in die Cloud-Konnektivität auftreten. Die schreibt dann verlangsamen und irgendwann fehl, wenn die Cloud-Konnektivität für mehrere Stunden unterbrochen wird. (Ist temporärer Speicher auf dem Gerät für Daten in der Cloud abgelegt werden. Dieser Bereich wird geleert, wenn die Daten gesendet werden. Ausfall der Verbindung Daten in diesen Speicherbereich nicht in die Cloud abgelegt, und IO fehl.)   

 
- **Für die Daten in der Cloud**: für die meisten Cloud Konnektivitätsfehler, wird ein Fehler zurückgegeben. Sobald die Verbindung wiederhergestellt ist, werden ohne dass der Benutzer den Datenträger online zu IOs fortgesetzt. In seltenen Fällen kann Benutzereingriff aus klassischen Azure-Portal den Datenträger online bringen müssen. 
 
- **Für Cloud-Snapshots ausgeführt**: der Vorgang wird in 4 bis 5 Stunden mehrmals wiederholt und Snapshots Cloud schlägt fehl, wenn die Verbindung nicht wiederhergestellt wird,.


### <a name="cluster-alerts"></a>Cluster-Alarme

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Gerät hat <*Gerätename*>.|Gerät ist im Wartungsmodus.|Gerät eingeben oder Beenden des Wartungsmodus Failover. Dies ist normal und keine Aktion erforderlich. Nachdem Sie diese Warnung bestätigt haben, entfernen Sie ihn aus der Warnungsseite.|
|Gerät hat <*Gerätename*>.|Geräte-Firmware oder Software wurde gerade aktualisiert.|Cluster-Failover durch ein Update ist. Dies ist normal und keine Aktion erforderlich. Nachdem Sie diese Warnung bestätigt haben, entfernen Sie ihn aus der Warnungsseite.|
|Gerät hat <*Gerätename*>.|Controller wurde heruntergefahren oder neu gestartet.|Gerät fehlgeschlagen über aktive Domänencontroller wurde heruntergefahren oder neu gestartet werden. Es ist keine Maßnahme erforderlich. Nachdem Sie diese Warnung bestätigt haben, entfernen Sie ihn aus der Warnungsseite.|
|Gerät hat <*Gerätename*>.|Geplantes Failover.|Stellen Sie sicher, dass dies einem geplanten Failover. Nachdem Sie entsprechende Maßnahmen ergriffen haben, deaktivieren Sie diese Warnung von der Warnungsseite.|
|Gerät hat <*Gerätename*>.|Ungeplante Failover.|StorSimple wird erstellt, um ungeplante Failovers automatisch zu beheben. Wenn eine große Anzahl dieser Warnung angezeigt wird, wenden Sie sich an Microsoft Support.|
|Gerät hat <*Gerätename*>.|Andere/Unbekannte Ursache.|Wenn eine große Anzahl dieser Warnung angezeigt wird, wenden Sie sich an Microsoft Support. Nachdem das Problem behoben ist, deaktivieren Sie diese Warnung von der Warnungsseite.|
|Wichtige gerätedienst meldet den Status fehlgeschlagen.|DataPath Dienstfehler. |Hilfe erhalten Sie bei Microsoft Support.|
|Virtuelle IP-Adresse für die Netzwerkschnittstelle <*Daten #*> meldet den Status fehlgeschlagen. |Andere/Unbekannte Ursache. |Manchmal temporäre Bedingung kann diese Alarme. Wenn dies der Fall ist, wird diese Warnung automatisch nach einiger Zeit gelöscht. Wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support.|
|Virtuelle IP-Adresse für die Netzwerkschnittstelle <*Daten #*> meldet den Status fehlgeschlagen.|Schnittstelle: <*Daten #*> IP-Adresse <IP address> nicht online geschaltet werden, da eine doppelte IP-Adresse im Netzwerk festgestellt. |Sicherzustellen Sie, dass doppelte IP-Adresse aus dem Netzwerk entfernt wird oder konfigurieren Sie die Schnittstelle mit einer anderen IP-Adresse.|


### <a name="disaster-recovery-alerts"></a>Disaster Recovery-Alarme

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Wiederherstellungsvorgänge konnte nicht alle für diesen Dienst wiederherstellen. Gerätekonfigurationsdaten ist in einem inkonsistenten Zustand für einige Geräte.|Dateninkonsistenz nach der Wiederherstellung.|Verschlüsselte Daten vom Dienst werden mit dem auf dem Gerät nicht synchronisiert. Autorisieren des Geräts <*Gerätename*> von StorSimple Manager den Synchronisierungsprozess gestartet. Verwenden Sie die Windows PowerShell-Schnittstelle für StorSimple Ausführen der `Restore-HcsmEncryptedServiceData` Gerät <*Gerätename*> Cmdlet bietet das alte Kennwort als Eingabe für dieses Cmdlet das Sicherheitsprofil wiederherstellen. Führen Sie die `Invoke-HcsmServiceDataEncryptionKeyChange` Cmdlet den Verschlüsselungsschlüssel Daten aktualisieren. Nachdem Sie entsprechende Maßnahmen ergriffen haben, deaktivieren Sie diese Warnung von der Warnungsseite.|


### <a name="hardware-alerts"></a>Hardware-Alarme

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Hardwarekomponente <*Komponenten-ID*> Status <*Status*> meldet.||Manchmal temporäre Bedingung kann diese Alarme. In diesem Fall wird diese Warnung automatisch nach einiger Zeit gelöscht. Wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support.|
|Passive Controller fehlerhaft.|Passive (sekundäre) Controller funktioniert nicht.|Das Gerät ist betriebsbereit, doch einen Controller ist fehlerhaft. Starten Sie diesen Controller. Wenn das Problem nicht behoben wird, wenden Sie sich an Microsoft Support.|

### <a name="job-failure-alerts"></a>Warnung wegen fehlgeschlagener Job

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Fehler bei der Sicherung von <*Source Volume-Gruppen-ID*>.|Fehler beim Sicherungsauftrag.|Möglicherweise verhindern Konnektivitätsprobleme den Sicherungsvorgang nicht erfolgreich abgeschlossen. Sind keine Konnektivitätsprobleme, haben Sie möglicherweise die maximale Anzahl von Sicherungen erreicht. Löschen Sie alle Backups, die nicht mehr benötigt werden und wiederholen Sie den Vorgang. Nachdem Sie entsprechende Maßnahmen ergriffen haben, deaktivieren Sie diese Warnung von der Warnungsseite.|
|Klon <*backup Quellelement IDs*> <*Ziel Datenträger-Seriennummern*> ist fehlgeschlagen.|Clone-Auftrag ist fehlgeschlagen.|Aktualisiert die Sicherung überprüfen, ob die Sicherung noch gültig ist. Falls die Sicherung gültig ist, ist möglich, dass Cloud-Konnektivitätsprobleme Klonvorgang ausführen verhindern. Wenn keine Konnektivitätsprobleme sind haben Sie die Speichergrenze erreicht. Löschen Sie alle Backups, die nicht mehr benötigt werden und wiederholen Sie den Vorgang. Nach Maßnahmen zur Fehlerbehebung durchgeführt haben, löschen Sie diese Warnung von der Warnungsseite.|
|Fehler beim Wiederherstellen des <*backup Quellelement IDs*>.|Fehler beim Wiederherstellungsauftrag.|Aktualisiert die Sicherung überprüfen, ob die Sicherung noch gültig ist. Falls die Sicherung gültig ist, ist möglich, dass Cloud-Konnektivitätsprobleme verhindern die Wiederherstellung erfolgreich abgeschlossen. Wenn keine Konnektivitätsprobleme sind haben Sie die Speichergrenze erreicht. Löschen Sie alle Backups, die nicht mehr benötigt werden und wiederholen Sie den Vorgang. Nach Maßnahmen zur Fehlerbehebung durchgeführt haben, löschen Sie diese Warnung von der Warnungsseite.|

### <a name="locally-pinned-volume-alerts"></a>Lokales Volume alerts

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Fehler beim Erstellen des lokalen Datenträgers <*Volumename*>.| Volume Creation Auftrag ist fehlgeschlagen. <*Fehler Nachricht entspricht dem Fehlercode*>.|Konnektivitätsprobleme möglicherweise Speicherplatz Erstellungsvorgang ausführen verhindern. Lokale Volumes dick bereitgestellt und Leerraum umfasst verschütten mehrstufige Volumes in der Cloud. Wenn keine Konnektivitätsprobleme sind haben Sie lokalen Speicherplatz auf dem Gerät ausgeschöpft. Ermitteln Sie, ob der Speicherplatz auf dem Gerät vorhanden ist, bevor Sie diesen Vorgang wiederholen.|
|Erweiterung des lokalen Datenträgers <*Volumename*> fehlgeschlagen.|Lautstärke ändern Auftrag ist aufgrund <*Fehler Nachricht entspricht dem Fehlercode*> fehlgeschlagen.|Konnektivitätsprobleme möglicherweise Erweiterungsvorgang Volume ausführen verhindern. Lokale Volumes dick bereitgestellt und erweitern den vorhandenen Speicherplatz umfasst verschütten mehrstufige Volumes in der Cloud. Wenn keine Konnektivitätsprobleme sind haben Sie lokalen Speicherplatz auf dem Gerät ausgeschöpft. Ermitteln Sie, ob der Speicherplatz auf dem Gerät vorhanden ist, bevor Sie diesen Vorgang wiederholen.|
|Fehler beim Konvertieren des Datenträgers <*Volumename*>.|Der Konvertierungsauftrag Volume lokal konvertieren den Datenträger aus fixiert tiered fehlgeschlagen.|Konvertierung des Volumes vom Typ lokal auf fixierten Ebenen nicht abgeschlossen. Sicherstellen Sie, dass es keine Konnektivitätsprobleme verhindert den Vorgang erfolgreich abgeschlossen. Problembehandlung bei Verbindungsproblemen finden Sie unter [Problembehandlung mit dem Test-HcsmConnection-Cmdlet](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Ursprünglichen lokalen Volumes wurde nun als tiered Volume markiert, da einige Daten aus der lokalen Volumes wurde während der Konvertierung in die Cloud verschüttet. Das resultierende tiered Volume belegten weiterhin lokalen Speicherplatz auf dem Gerät für zukünftige lokale Volumes kann nicht freigegeben werden.<br>Konnektivität klären, die Warnung, und konvertieren Sie diesen Datenträger wieder in den ursprünglichen Typ lokalen Volumes auf alle Daten lokal wieder verfügbar ist.|
|Fehler beim Konvertieren des Datenträgers <*Volumename*>.|Der Konvertierungsauftrag Datenträger konvertieren den Volumetyp lokal fixiert tiered fehlgeschlagen.|Konvertierung des Volumes vom Typ tiered lokal fixiert konnte nicht abgeschlossen werden. Sicherstellen Sie, dass es keine Konnektivitätsprobleme verhindert den Vorgang erfolgreich abgeschlossen. Problembehandlung bei Verbindungsproblemen finden Sie unter [Problembehandlung mit dem Test-HcsmConnection-Cmdlet](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Das ursprüngliche tiered Volume jetzt als Teil des Konvertierungsvorgangs weiterhin Daten in der Cloud dick bereitgestellte Speicherplatz auf dem Gerät für dieses Volume kann nicht für zukünftige lokale Volumes freigegeben als lokales Volume markiert.<br>Konnektivität klären, die Warnung, und konvertieren Sie diesen Datenträger wieder in den ursprünglichen Volumetyp tiered um sicherzustellen, dass lokale Linienstärke auf dem Gerät bereitgestellt Speicherplatz freigegeben werden kann.|
|Lokale Speicherplatzverbrauch für lokale Snapshots von fast <*Volume-Gruppenname*>|Lokale Snapshots für backup-Richtlinie möglicherweise bald Speicherplatz und Vermeidung von Host Registrierungsschreibfehler ungültig gemacht werden.|Häufige lokale Snapshots neben eine hohe Fluktuation in dieser Gruppe Sicherungsrichtlinie Volumes verursachen lokalen Speicherplatz auf dem Gerät schnell verbraucht werden. Löschen Sie alle lokalen Momentaufnahmen nicht mehr benötigt werden. Aktualisieren Sie auch Ihre lokale Snapshot-Zeitpläne für diese Sicherungsrichtlinie seltener lokale Snapshots und sicherzustellen, dass Cloud Snapshots regelmäßig erstellt werden. Wenn diese Aktionen nicht ausgeführt werden, lokalen Speicherplatz für diese Snapshots kann schnell erschöpft und wird automatisch gelöscht, um sicherzustellen, dass Host-Schreibvorgänge weiterhin erfolgreich verarbeitet.|
|Lokale Snapshots für <*Volume Group Name*> ist ungültig.|Lokale Snapshots für <*Volume Group Name*> ungültig und dann gelöscht, weil sie den lokalen Speicherplatz auf dem Gerät überschritten wurden.|Um sicherzustellen, dass dies in Zukunft nicht mehr auftritt, überprüfen Sie lokale Snapshot-Zeitpläne für diese Sicherungsrichtlinie und löschen Sie alle lokalen Momentaufnahmen nicht mehr benötigt werden. Häufige lokale Snapshots neben eine hohe Fluktuation in dieser Gruppe Sicherungsrichtlinie Volumes möglicherweise lokalen Speicherplatz auf dem Gerät schnell verbraucht werden.|
|Fehler beim Wiederherstellen des <*backup Quellelement IDs*>.|Fehler beim Wiederherstellungsauftrag.|Haben lokal fixiert oder aus lokal fixierte und tiered-Volumes in diesem Sicherungsrichtlinie aktualisieren die Liste die Sicherungskopie zu überprüfen, ob ist das Backup weiterhin gültig. Falls die Sicherung gültig ist, ist möglich, dass Cloud-Konnektivitätsprobleme verhindern die Wiederherstellung erfolgreich abgeschlossen. Die lokale Volumes, die als Teil dieser Momentaufnahme wiederhergestellt wurden haben nicht alle ihre Daten auf das Gerät heruntergeladen, und haben Sie aus tiered und lokalen Volumes dieser Gruppe Snapshot, sie werden nicht miteinander synchronisiert. Um die Wiederherstellung abgeschlossen nehmen Sie Volumes dieser Gruppe auf dem Server offline, und wiederholen Sie den Wiederherstellungsvorgang. Beachten Sie, dass Änderungen an der Datenmengen, die während der Wiederherstellung ausgeführt wurden verloren.|

### <a name="networking-alerts"></a>Netzwerk-alerts

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|StorSimple-Dienste konnte nicht gestartet werden.|DataPath Fehler |Wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support.|
|Doppelte IP-Adresse für 'Data0' gefunden.| |Das System hat einen Adressenkonflikt für IP-Adresse 10.0.0.1". Die Netzwerkressource 'Data0' auf dem Gerät *<device1>* ist offline. Stellen Sie sicher, dass diese IP-Adresse nicht von einem anderen Element in diesem Netzwerk verwendet wird. Gehen Sie Netzwerk Problembehandlung [Problembehandlung mit dem Cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Wenden Sie sich an Ihren Netzwerkadministrator, zur Behebung dieses Problems. Wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support. |
|IPv4-(oder IPv6) ist 'Data0' offline.| |Die Netzwerkressource 'Data0' mit IP-Adresse "10.0.0.1." und Präfixlänge "22" auf dem Gerät *<device1>* ist offline. Sicherstellen Sie, dass die Switch-Ports, mit denen diese Schnittstelle verbunden ist, betriebsbereit. Gehen Sie Netzwerk Problembehandlung [Problembehandlung mit dem Cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
 

### <a name="performance-alerts"></a>Performance-Alarme

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Die Device-Auslastung überschritten <*Schwellenwert*>.|Langsamer als erwartet Reaktionszeiten.|Das Gerät meldet Auslastung/Ausgang überlastet. Dadurch kann das Gerät nicht so gut wie gewünscht funktioniert. Überprüfen Sie die Arbeitslasten an das Gerät angeschlossen haben und ermitteln, ob eine an ein anderes Gerät verschoben werden konnte oder keine mehr erforderlich.<br>Um den aktuellen Status zu verstehen, gehen Sie zu [StorSimple Manager-Dienst das Gerät überwachen](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Sicherheitshinweise

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Microsoft Support-Sitzung wurde gestartet.|Drittanbieter-Zugriff auf Support-Sitzung.|Bestätigen Sie, dass dieser Zugriff autorisiert ist. Nachdem Sie entsprechende Maßnahmen ergriffen haben, deaktivieren Sie diese Warnung von der Warnungsseite.|
|Kennwort für <*Element*> läuft in <*Zeit*>.|Kennwortablauf nähert.|Ändern Sie Ihr Kennwort, bevor es abläuft.|
|Informationen zur Sicherheitskonfiguration für <*Elementname*> fehlt.||Mit diesem Datenträger verbundenen Volumes können verwendet werden, die StorSimple Konfiguration zu replizieren. Um sicherzustellen, dass Ihre Daten sicher gespeichert sind, wird empfohlen, volumecontainer und volumecontainer zugeordneten Volumes löschen. Nachdem Sie entsprechende Maßnahmen ergriffen haben, deaktivieren Sie diese Warnung von der Warnungsseite.|
|<*Anzahl*> Anmeldeversuche für <*Elementname*> fehlgeschlagen.|Mehrere Anmeldeversuche fehlgeschlagene.|Das Gerät könnte angegriffen werden oder ein autorisierter Benutzer versucht, mit einem falschen Kennwort eine Verbindung herzustellen.<ul><li>Wenden Sie sich an Ihren autorisierten Benutzern und überprüfen Sie, ob diese Versuche von einer seriösen Quelle wurden. Sollten Sie weiterhin große Anzahl fehlgeschlagener Login-Versuche, Deaktivieren der Remoteverwaltung und den Netzwerkadministrator. Nachdem Sie entsprechende Maßnahmen ergriffen haben, deaktivieren Sie diese Warnung von der Warnungsseite.</li><li>Überprüfen Sie, dass Instanzen Snapshot-Manager mit dem richtigen Kennwort konfiguriert sind. Nachdem Sie entsprechende Maßnahmen ergriffen haben, deaktivieren Sie diese Warnung von der Warnungsseite.</li></ul>Weitere Informationen finden Sie [eine abgelaufene Gerätekennwort ändern](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password).|
|Beim Ändern des Daten Verschlüsselungsschlüssels sind ein oder mehrere Fehler aufgetreten.||Fehler beim Ändern des Verschlüsselungsschlüssels Daten entdeckt. Nachdem Sie den Fehler behoben haben, führen die `Invoke-HcsmServiceDataEncryptionKeyChange` Cmdlet von Windows PowerShell-Schnittstelle für StorSimple auf dem Gerät, um den Dienst zu aktualisieren. Wenn dieses Problem weiterhin besteht, wenden Sie sich an Microsoft Support Services. Beheben Sie das Problem, deaktivieren Sie diese Warnung von der Warnungsseite.|

### <a name="support-package-alerts"></a>Support-Paket alerts

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Fehler beim Erstellen des Support-Paket.|StorSimple konnte nicht das Paket generieren.|Wiederholen Sie den Vorgang. Wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support. Nachdem das Problem behoben haben, löschen Sie diese Warnung von der Warnungsseite.|

### <a name="update-alerts"></a>Update-alerts

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Hotfix installiert.|Software-Firmware-Aktualisierung abgeschlossen.|Das Update wurde erfolgreich auf dem Gerät installiert.|
|Manuelle Updates verfügbar.|Benachrichtigung über verfügbare Updates.|Verwenden Sie die Windows PowerShell-Schnittstelle für StorSimple auf Ihrem Gerät, um diese Updates zu installieren. <br>Weitere Informationen finden Sie aktualisieren [Ihr Gerät StorSimple 8000-Serie](storsimple-update-device.md).|
|Neue Updates verfügbar.|Benachrichtigung über verfügbare Updates.|Sie können diese Updates auf der Seite **Wartung** oder über die Windows PowerShell-Schnittstelle für StorSimple auf dem Gerät. <br>Weitere Informationen finden Sie aktualisieren [Ihr Gerät StorSimple 8000-Serie](storsimple-update-device.md).|
|Fehler beim Installieren von Updates.|Updates wurden nicht erfolgreich installiert.|Das System konnte nicht die Updates zu installieren. Sie können diese Updates auf der Seite **Wartung** oder über die Windows PowerShell-Schnittstelle für StorSimple auf dem Gerät. Wenn das Problem weiterhin besteht, wenden Sie sich an Microsoft Support. <br>Weitere Informationen finden Sie aktualisieren [Ihr Gerät StorSimple 8000-Serie](storsimple-update-device.md).|
|Nicht automatisch nach neuen Updates gesucht.|Automatische Überprüfung fehlgeschlagen.|Sie können manuell nach neuen Updates auf **Wartung** überprüfen.|
|Neue WUA-Agent verfügbar.|Benachrichtigung über verfügbare Update.|Neueste Windows Update Agent herunterladen und Installieren von Windows PowerShell-Schnittstelle.|
|Version der Firmware Komponente <*Komponenten-ID*> entspricht nicht mit Hardware.|Firmware-Updates wurden nicht erfolgreich installiert.|Wenden Sie sich an Microsoft Support Services.|

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [StorSimple Fehler und Problembehandlung eine Gerät betriebsbereit](storsimple-troubleshoot-operational-device.md).

