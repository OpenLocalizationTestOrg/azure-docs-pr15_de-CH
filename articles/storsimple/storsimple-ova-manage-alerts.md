<properties 
   pageTitle="Anzeigen und Verwalten von StorSimple Virtual Array Alerts | Microsoft Azure"
   description="Beschreibt StorSimple Virtual Array beschriebener und Schweregrad sowie den StorSimple Manager-Dienst verwenden, um Benachrichtigungen verwalten."
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
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-alerts-for-the-storsimple-virtual-array"></a>Verwenden des StorSimple Manager-Dienstes zu Alerts für StorSimple Virtual Array verwalten

## <a name="overview"></a>Übersicht

Registerkarte **Alerts** in der StorSimple Manager-Dienst ermöglicht und StorSimple Virtual Array bezogenen Alarme in Echtzeit deaktivieren. Auf dieser Registerkarte können zentral überwachen die Gesundheit der virtuellen StorSimple-Arrays (auch lokal StorSimple virtuelle Geräte) und Microsoft Azure StorSimple Lösung.

In diesem Lernprogramm beschreibt konfigurieren Benachrichtigungen allgemeine beschriebener Warnungsschweregrade und zu Warnung nachzuverfolgen. Darüber hinaus enthält alert Kurzübersicht Tabellen können Sie schnell eine bestimmte Warnung und entsprechend reagieren.

![Seite Alerts](./media/storsimple-ova-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Warnung konfigurieren

Sie können auswählen, ob Sie per e-Mail beschriebener für jede StorSimple virtuelle Geräte. Außerdem können Sie andere Benachrichtigungsempfänger identifizieren, durch Eingabe ihrer e-Mail-Adresse im Feld **andere e-Mail-Empfänger** , durch Semikolon getrennt.

>[AZURE.NOTE] Sie können maximal 20 e-Mails pro virtuelles Gerät eingeben.

Nach dem Aktivieren der e-Mail-Benachrichtigung für ein virtuelles Gerät erhalten Mitglieder der Benachrichtigungsliste einer e-Mail jedes Mal eine wichtige Warnung auftritt. Die Nachrichten werden von *storsimple-alerts-noreply@mail.windowsazure.com* und die Warnung. Empfänger können **Abonnement** um sich von der e-Mail-Benachrichtigungsliste entfernen klicken.

#### <a name="to-enable-email-notification-of-alerts-for-a-virtual-device"></a>E-Mail Alerts für ein virtuelles Gerät aktivieren

1. **Geräte**zur > **Konfiguration** des virtuellen Geräts. Gehen Sie zum Abschnitt **Einstellungen** .

    ![Einstellungen](./media/storsimple-ova-manage-alerts/alerts2.png)

2. Legen Sie unter **Einstellungen**Folgendes:

    1. Wählen Sie **Ja**im Feld **e-Mail-Benachrichtigung senden** .

    2. Wählen Sie **Ja** , im Feld **e-Mail-Administratoren** zu Dienstadministrator und alle CO-Administratoren die Benachrichtigung erhalten.

    3. Geben Sie im Feld **Weitere e-Mail-Empfänger** die e-Mail-Adressen aller Empfänger, die die Benachrichtigung erhalten sollen. Geben Sie Namen im Format *someone@somewhere.com*. Verwenden Sie Semikolons zum Trennen von e-Mail-Adressen. Sie können maximal 20 e-Mails pro virtuelles Gerät konfigurieren. 

        ![Alarmkonfiguration Benachrichtigung](./media/storsimple-ova-manage-alerts/alerts3.png)

3. Klicken Sie am unteren Rand der Seite auf **Speichern** , um die Konfiguration zu speichern.

4. Zum Senden einer Test-Benachrichtigung klicken Sie auf den Pfeil neben **e-Mail-Test**. Der StorSimple Manager-Dienst wird wie testbenachrichtigung leitet Nachrichten angezeigt. 

5. Wenn die folgende Meldung angezeigt wird, klicken Sie auf **OK**. 

    ![Alarme testen e-Mail gesendet](./media/storsimple-ova-manage-alerts/alerts4.png)

    >[AZURE.NOTE] Wenn die Test-Nachricht gesendet werden kann, wird der StorSimple Manager-Dienst eine entsprechende Meldung angezeigt. Klicken Sie auf **OK**, warten Sie einige Minuten und versuchen Sie Ihr Test-Benachrichtigung erneut senden. 

    Die Test-Nachricht werden ähnlich der folgenden.

    ![Alerts Test e-Mail-Beispiel](./media/storsimple-ova-manage-alerts/alerts5.png)

## <a name="common-alert-conditions"></a>Allgemeine beschriebener

Virtuelle StorSimple Array Alerts auf eine Reihe von Fehlerzuständen generiert. Es folgen die häufigsten beschriebener:

- **Verbindungsprobleme** – diese Alarme auftreten, wenn Probleme bei der Datenübertragung. Kommunikationsprobleme können während der Übertragung von Daten und von Azure Storage-Konto oder aufgrund der Verbindung zwischen der virtuellen Geräte und der StorSimple Manager-Dienst auftreten. Kommunikationsprobleme sind am schwersten zu beheben, da so viele Fehlerquellen. Sie sollten immer zuerst überprüfen Sie Netzwerkkonnektivität und Internetzugang verfügbar sind, bevor auf Erweiterte Problembehandlung. Informationen zu Ports und Firewall Settings finden Sie [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md). Hilfe zur Problembehandlung finden Sie unter [Problembehandlung mit dem Cmdlet Verbindung testen](storsimple-troubleshoot-deployment.md).

- **Performance-Probleme** – diese Warnung tritt auf, wenn Ihr System nicht optimal, beispielsweise wenn es unter hoher Auslastung funktioniert.

Darüber hinaus sehen Sie Alerts Sicherheit, Updates oder Auftragsfehler beziehen.

## <a name="alert-severity-levels"></a>Warnungsschweregrade

Alerts sind verschiedene Schweregrade der Auswirkung, die die alert-Situation und eine Reaktion auf die Warnung. Die Schweregrade sind:

- **Kritisch** – diese Warnung wird als Antwort auf eine Bedingung, die den Erfolg des Systems auswirken. Aktion ist erforderlich, um sicherzustellen, dass die StorSimple Dienst nicht unterbrochen wird.

- **Warnung** – dieser Zustand kritisch, wenn nicht aufgelöst werden konnte. Sie untersuchen die Situation und keine Aktion erforderlich, um dieses Problem zu beheben.

- **Informationen** – diese Warnung enthält nützliche Informationen und Ihr System verwalten kann.

## <a name="view-and-track-alerts"></a>Alerts anzeigen und Nachverfolgen

StorSimple Manager Service Dashboard bietet einen Einblick in die Anzahl der Benachrichtigungen auf der virtuellen Geräte nach Schweregrad sortiert.

![Alerts-dashboard](./media/storsimple-ova-manage-alerts/alerts6.png)

Auf den Schweregrad öffnet die Registerkarte **Alarme** . Die Ergebnisse enthalten Alarme, die diesem Schweregrad entsprechen.

![Alerts-Bericht Alarmtyp Gültigkeitsbereich](./media/storsimple-ova-manage-alerts/alerts7.png)

Klicken in der Liste eine Warnung bietet zusätzliche Details für die Warnung zuletzt die Warnung gemeldet wurde, einschließlich der Anzahl der Warnung auf das Gerät und der empfohlenen Maßnahme zum Beheben der Warnung.

Sie können die Details der Warnung in einer Datei benötigen Sie die Informationen an Microsoft Support senden. Nach der Empfehlung und die lokalen Warnung aufgelöst sollten Sie die Benachrichtigung deaktivieren, Registerkarte **Alarme** auswählen und auf **Löschen**klicken. Um mehrere Warnungen löschen, wählen Sie jede Warnung klicken Sie auf eine beliebige Spalte außer der Spalte **Warnung** und klicken Sie dann auf **Deaktivieren Sie** nach Auswahl der Alarme gelöscht. Beachten Sie, dass einige Warnungen automatisch gelöscht werden, wenn das Problem behoben ist oder wenn die Warnung mit neuen Informationen aktualisiert.

Wenn Sie auf **Löschen**klicken, haben Sie die Möglichkeit, Kommentare zu der Warnung und die Schritte zur Behebung des Problems hat. 

![Warnung Kommentare](./media/storsimple-ova-manage-alerts/clear-alert.png)

Klicken Sie auf das Häkchensymbol ![Kontrollkästchen-Symbol](./media/storsimple-ova-manage-alerts/check-icon.png) Ihre Kommentare zu speichern.

Einige Ereignisse werden ausgelöst ein anderes Ereignis mit neuen Informationen vom System gelöscht. In diesem Fall wird die folgende Meldung angezeigt.

![Warnung löschen](./media/storsimple-ova-manage-alerts/alerts8.png)

## <a name="sort-and-review-alerts"></a>Sortieren und Überprüfung alerts

Registerkarte **Alerts** kann bis zu 250 Warnungen anzeigen. Überschreiten die Anzahl der Warnungen werden nicht alle Warnungen in der Standardansicht angezeigt. Kombinieren Sie die folgenden Felder anpassen, welche Warnungen angezeigt werden:

- **Status** – Warnungen **aktiv** oder **deaktiviert** angezeigt. Aktive Warnungen werden noch auf Ihrem System ausgelöst, während gelöschte Warnung manuell von einem Administrator gelöscht oder wurden programmgesteuert gelöscht, da das System die Warnung mit neuen Informationen aktualisiert.

- **Schweregrad** – Warnungen alle Schweregrade (kritisch, Warnung, Information) oder nur einen bestimmten Schweregrad, z. B. nur kritische Alarme anzuzeigen.

- **Datenquellen** können aus allen Quellen Benachrichtigung anzeigen oder Alarme, die aus dem Dienst oder eine oder alle virtuellen Geräte einschränken.

- **Zeitraum** – durch Angabe der **von-** und **bis-** Datum und Zeitstempel Sie Alerts Zeitraum sehen, denen Sie interessieren.

## <a name="alerts-quick-reference"></a>Alerts-Kurzübersicht

Die folgenden Tabellen sind einige der Microsoft Azure StorSimple-Alarme, sowie zusätzliche Informationen und Vorschläge gegebenenfalls auftreten können. StorSimple virtuelles Gerät Alerts fallen in die folgenden Kategorien:

- [Cloud-Konnektivität alerts](#cloud-connectivity-alerts)

- [Konfiguration-Alarme](#configuration-alerts)

- [Warnung wegen fehlgeschlagener Job](#job-failure-alerts)

- [Performance-Alarme](#performance-alerts)

- [Sicherheitshinweise](#security-alerts)

- [Update-alerts](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Cloud-Konnektivität alerts

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Gerät *<device name>* nicht mit der Cloud verbunden ist.|Das benannte Gerät Verbindung keine mit der Cloud. |Nicht möglich in der Cloud. Dies kann eine der folgenden haben:<ul><li>Möglicherweise ein Problem mit dem Netzwerk auf Ihrem Gerät.</li><li>Möglicherweise ein Problem mit Anmeldeinformationen für das Speicherkonto.</li></ul>Weitere Informationen zur Behebung von Verbindungsproblemen finden Sie im [lokalen Web-Benutzeroberfläche](storsimple-ova-web-ui-admin.md) des Geräts.|


### <a name="configuration-alerts"></a>Konfiguration-Alarme

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Lokale virtuelle Medienkonfiguration nicht unterstützt.|Langsam.|Die aktuelle Konfiguration kann Leistungseinbußen führen. Stellen Sie sicher, dass der Server die Mindestkonfiguration erfüllt. Weitere Informationen finden Sie an [StorSimple Virtual Array](storsimple-ova-system-requirements.md). 
|Bereitgestellte Speicherplatz laufen auf <*Gerätename*>.|Warnung.|Sie sind bereitgestellte Speicherplatz knapp. Um Speicherplatz freizugeben, sollten Sie Arbeitslasten auf ein anderes Volume oder Freigabe verschieben oder Löschen von Daten.

### <a name="job-failure-alerts"></a>Warnung wegen fehlgeschlagener Job

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Sicherung der <*Gerätename*> konnte nicht abgeschlossen werden.|Fehler beim Sicherungsauftrag.|Eine Sicherung konnte nicht erstellt werden. Berücksichtigen Sie die folgenden:<ul><li>Möglicherweise verhindern Konnektivitätsprobleme den Sicherungsvorgang nicht erfolgreich abgeschlossen. Sicherstellen Sie, dass keine Konnektivitätsprobleme vorliegen. Weitere Informationen zur Behebung von Verbindungsproblemen finden Sie im [lokalen Web-Benutzeroberfläche](storsimple-ova-web-ui-admin.md) für das virtuelle Gerät.</li><li>Sie haben die Speicherkapazität erreicht. Um Speicherplatz freizugeben, sollten Sie alle nicht mehr benötigten Backups löschen.</li></ul> Probleme zu beheben, die Warnung, und wiederholen Sie den Vorgang.|
|<*Gerätename*> konnte nicht wiederhergestellt werden.|Wiederherstellen Sie Auftragsfehler.|Konnte nicht aus einer Sicherung wiederherstellen. Berücksichtigen Sie die folgenden:<ul><li>Die Liste möglicherweise nicht gültig. Aktualisieren Sie die Liste, um sicherzustellen, dass es noch gültig ist.</li><li>Möglicherweise verhindern Verbindungsprobleme die Wiederherstellung erfolgreich abgeschlossen. Sicherstellen Sie, dass keine Konnektivitätsprobleme vorliegen.</li><li>Sie haben die Speicherkapazität erreicht. Um Speicherplatz freizugeben, sollten Sie alle nicht mehr benötigten Backups löschen.</li></ul>Probleme zu beheben und die Warnung erneut wiederherstellen.|
|Klon des <*Gerätename*> konnte nicht abgeschlossen werden.|Klonen Sie Auftragsfehler.|Einen Klon konnte nicht erstellt werden. Berücksichtigen Sie die folgenden:<ul><li>Die Liste möglicherweise nicht gültig. Aktualisieren Sie die Liste, um sicherzustellen, dass es noch gültig ist.</li><li>Konnektivitätsprobleme konnte der Klonvorgang ausführen verhindern. Sicherstellen Sie, dass keine Konnektivitätsprobleme vorliegen.</li><li>Sie haben die Speicherkapazität erreicht. Um Speicherplatz freizugeben, sollten Sie alle nicht mehr benötigten Backups löschen.</li></ul>Probleme zu beheben, die Warnung, und wiederholen Sie den Vorgang.|

### <a name="performance-alerts"></a>Performance-Alarme

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Unerwarteter bei der Datenübertragung auftreten.|Langsame Datenübertragung.|Übersteigen die Skalierbarkeitsziele eines auftreten Drosselung Fehler. Der Speicherdienst wird keine einzelnen Client oder Mieter auf Kosten anderer Dienst verwenden kann. Finden Sie weitere Informationen zur Problembehandlung der Azure-Speicher-Konto zu [überwachen, zu diagnostizieren und Beheben von Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).
|Niedrige Reservierung lokalen Speicherplatz auf <*Gerätename*> ausgeführt.|Langsame Reaktionszeiten.|10 % der bereitgestellten Gesamtgröße <*Gerätename*> ist auf dem lokalen Gerät und Sie sind nun knapp reservierter Speicherplatz. Die Arbeitslast auf <*Gerätename*> erzeugt eine höhere Abwanderung oder Sie möglicherweise haben vor kurzem migriert eine große Datenmenge. Dies kann Leistungseinbußen führen. Berücksichtigen Sie die folgenden Aktionen zur Lösung des Problems:<ul><li>Vergrößern Sie die Bandbreite Cloud für dieses Gerät.</li><li>Reduzieren Sie oder verschieben Sie der Arbeitslast auf ein anderes Volume oder eine Freigabe.</li></ul>

### <a name="security-alerts"></a>Sicherheitshinweise

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Kennwort <*Gerätename*> wird in <*Anzahl*> Tagen ablaufen.|Kennwort-Warnung.| Ihr Kennwort läuft < Zahl < Tage. Sollten Sie Ihr Kennwort ändern. Weitere Informationen finden Sie im [Virtual Array StorSimple Gerät Administratorkennwort ändern](storsimple-ova-change-device-admin-password.md).

### <a name="update-alerts"></a>Update-alerts

|Warnungstext|Ereignis|Weitere Informationen / empfohlene Aktionen|
|:---|:---|:---|
|Es stehen neue Updates für Ihr Gerät.|Updates für virtuelle StorSimple Array sind verfügbar.|Sie können neue Updates auf **Wartung** .|
|Konnte nicht nach neuen Updates <*Gerätename*> Scannen.|Aktualisieren Sie Fehler beim. |Fehler bei der Installation von neuen Updates. Sie können die Updates manuell installieren. Wenn das Problem weiterhin besteht, wenden Sie sich an [Microsoft Support](storsimple-contact-microsoft-support.md).|


## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über StorSimple Virtual Array](storsimple-ova-overview.md).


