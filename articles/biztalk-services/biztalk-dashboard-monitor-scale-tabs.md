<properties 
    pageTitle="Dashboard, Monitor, Skalierung, konfigurieren und Hybrid BizTalk-Dienste | Microsoft Azure" 
    description="Erfahren Sie mehr über Steuerelemente und Überwachen der Leistung auf der Portalwebsite Registerkarten für BizTalk-Dienste: Dashboard überwachen, Skalierung, konfigurieren und Hybrid-Verbindungen. MAK, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Überprüfen Sie Registerkarte Dashboard überwachen, Skalierung, konfigurieren und Hybrid-Verbindung

BizTalk Service erstellen und Bereitstellen der Anwendung können Sie einige von BizTalk Service-Optionen ändern und überwachen die Leistung der Anwendung. 

Beim klassischen Azure-Portal öffnen, werden Sie automatisch auf der Registerkarte **Alle Elemente** platziert. BizTalk Service anzeigen, wählen auf der Registerkarte **Alle Elemente** BizTalk Service oder Registerkarte **BIZTALK-Dienste** ; und wählen Sie dann Ihren Namen BizTalk Service.

Dies öffnet ein neues Fenster mit den folgenden Registerkarten. Diese Registerkarten beschrieben.

## <a name="quick-start-quick-startquickstart"></a>Schnellstart (![Schnellstart][QuickStart])
Je nach der Edition BizTalk Services alle aufgeführten Optionen möglicherweise nicht verfügbar. 
<table border="1">
    <tr>
        <td><strong>Die Tools</strong></td>
        <td>Herunterladen der BizTalk-Dienste SDK zum Visual Studio-Projektvorlagen auf Ihrem lokalen Computer installieren. Diese Vorlagen erstellen Sie die <strong>BizTalk-Dienste</strong> (Bridge) und <strong>BizTalk-Serviceartefakte</strong> (Transformation) Visual Studio-Projekte, die Ihr BizTalk-Service bereitgestellt werden.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Vorgehensweise beim Start verwenden Azure BizTalk-Dienste SDK</a> und <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Azure BizTalk-Dienste SDK installieren</a> Listet die Schritte.
        </td>
    </tr>
    <tr>
        <td><strong>Verträge erstellen</strong></td>
        <td>Öffnet das Azure BizTalk-Portal auf Partner hinzugefügt und erstellen X12 AS2, Azure gehostet und EDIFACT EDI.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurieren der Komponenten für Messaging EDI in BizTalk Services-Portal</a> Listet die Schritte.
        </td>
    </tr>

<tr>
        <td><strong>Erfahren Sie mehr über BizTalk-Dienste</strong></td>
        <td>Gehen Sie zu der <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning Center</a> erfahren Sie mehr über Azure BizTalk Services.</td>
</tr>
</table>


In der Taskleiste unten können Sie folgende Aktionen ausführen:

<table border="1">

<tr>
<td><strong>Verwalten von</strong> Bereitstellung der Anwendung</td>
<td>Öffnet das Azure BizTalk-Portal. Das BizTalk-Portal ist der Eingang zu EDI-Konfiguration, einschließlich Partner hinzufügen und X12 AS2, erstellen und EDIFACT.
<br/><br/>
Dies entspricht der <strong>Partnerverträge erstellen</strong> auf der Registerkarte <strong>Quick Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurieren der Komponenten für Messaging EDI in BizTalk Services-Portal</a> bietet weitere Informationen auf das BizTalk-Portal.</td>
</tr>

<tr>
<td><strong>Informationen</strong> von Access Control Namespace</td>
<td>Bei Auswahl von Verbindungsinformationen werden Access Control Namespace, Standard Aussteller und Standardschlüssel angezeigt. Sie können diese Werte.
<br/><br/>
Sie können auch Access Control Portal öffnen. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Erstellen einer Access Control Namespace</a> enthält weitere Informationen zu Access Control Portal.</td>
</tr>

<tr>
<td><strong>Synchronisieren der Schlüssel</strong> im Speicherkonto</td>
<td>Ein Speicherkonto erstellen, werden automatisch ein Primärschlüssel und Sekundärschlüssel erstellt. Diese Schlüssel steuern den Zugriff auf das Speicherkonto. BizTalk Service verwendet automatisch den Primärschlüssel. <strong>Sync-Schlüssel</strong> können Benutzer den Primärschlüssel und den Sekundärschlüssel ohne Unterbrechung der BizTalk Service wechseln.
<br/><br/>
Sie möchten z. B. BizTalk Service einen neuen Primärschlüssel für das Speicherkonto verwenden. Dazu:
<br/><br/>
<ol>
<li>Wählen Sie Ihre BizTalk Service und <strong>Sync-Schlüssel</strong>. Wählen Sie die sekundäre Taste. Wenn Sie dies tun, startet der BizTalk Service sekundäre Taste.</li>
<li>Wählen Sie im klassischen Azure-Portal das Speicherkonto und Regenerieren der Primärschlüssel. Beachten Sie, dass BizTalk Service den Sekundärschlüssel verwenden.</li>
<li>Wählen Sie Ihre BizTalk Service und <strong>Sync-Schlüssel</strong>. Wählen Sie jetzt den Primärschlüssel. Dies ist der neue Primärschlüssel Sie regeneriert.</li>
<li>Wählen Sie im klassischen Azure-Portal das Speicherkonto und regenerieren Sie dem Sekundärschlüssel.</li>
</ol>
<br/>
Dieser Vorgang wird "Rollover-Schlüssel" bezeichnet. Damit Benutzer wechseln zwischen den Primärschlüssel und den Sekundärschlüssel ohne Unterbrechung der BizTalk Service dient.</td>
</tr>

<tr>
<td><strong>Löschen Sie</strong> der Anwendung</td>
<td>Bei der Auswahl löschen, BizTalk Service und alle Objekte bereitgestellt werden entfernt.</td>
</tr>
</table>


## <a name="dashboard"></a>Dashboard
Je nach der Edition BizTalk Services alle aufgeführten Optionen möglicherweise nicht verfügbar. 

Wenn Sie Ihre BizTalk Service Name auswählen, wird die Dashboard-Registerkarte angezeigt. Im Dashboard können Sie:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>: Verwendung Übersicht der verwendeten Hybrid-Verbindungen
Zeigt außerdem die Datenverwendung in GB an. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>: Metrische Diagramm eine feste Liste von Leistungsindikatoren
Diese Metriken bieten Messwerte über den Zustand des BizTalk Services. Sie können auch **Relative** oder **Absolute** Werte und den Zeitraum **Intervall** von Metriken, die im Diagramm angezeigt werden. 

Eine Beschreibung dieser Performance-Werte werden Sie in diesem Thema [Verfügbaren Metriken](#Metrics) .


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Blick: Listet die BizTalk Service-Eigenschaften

<table border="1">

<tr>
<td><strong>Überwachungsdatenbank Anmeldeinformationen aktualisieren</strong></td>
<td>Ändert den Benutzernamen und das Kennwort in der Überwachungsdatenbank protokolliert.</td>
</tr>
<tr>
<td><strong>SSL-Zertifikat aktualisieren</strong></td>
<td>Sie können BizTalk Service um ein anderes SSL-Zertifikat verwenden. Ein selbstsigniertes SSL-Zertifikat wird automatisch erstellt, wenn Sie <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">den BizTalk-Dienst erstellen</a>.</td>
</tr>
<tr>
<td><strong>Zertifikat herunterladen</strong></td>
<td>Sie können das SSL-Zertifikat von Ihrem BizTalk Service auf einem lokalen Computer verwendet.</td>
</tr>
<tr>
<td><strong>Status</strong></td>
<td>Zeigt den aktuellen Status des BizTalk Service. Siehe <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk-Dienste: Zustandsdiagramms Service</a>. </td>
</tr>
<tr>
<td><strong>Dienst-URL</strong></td>
<td>Die URL für den BizTalk-Dienst. Dies entspricht der <strong>Domänen-URL</strong> eingegeben, wenn BizTalk Service erstellt wird.</td>
</tr>
<tr>
<td><strong>Öffentliche virtuelle IP-Adresse (VIP) Adresse</strong></td>
<td>Die IP-Adresse der BizTalk-Service. Es ist für alle input-Endpunkte verwendet und ist die Quelladresse für ausgehenden Datenverkehr. Diese IP-Adresse gehört zu Ihrem BizTalk Service als erstellt wird. Löschen von BizTalk Service erhält die IP-Adresse einer anderen BizTalk-Service.</td>
</tr>
<tr>
<td><strong>ACS-Namespace</strong></td>
<td>Mit der BizTalk-Dienst authentifiziert.</td>
</tr>
<tr>
<td><strong>Edition</strong></td>
<td>Listen eingegeben die Ausgabe, wenn BizTalk Service erstellt wird.</td>
</tr>
<tr>
<td><strong>Speicherort</strong></td>
<td>Zeigt die geografische Region, die BizTalk Service gehostet.</td>
</tr>
<tr>
<td><strong>Erstellt</strong></td>
<td>Zeigt Datum und Uhrzeit der Erstellung der BizTalk Service.</td>
</tr>
<tr>
<td><strong>Überwachungsdatenbank</strong></td>
<td>Der Azure SQL-Datenbank-Name, der von BizTalk Service verwendete Nachverfolgungstabellen speichert. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Anforderungen erläutert</a> Informationen in der Überwachungsdatenbank.</td>
</tr>
<tr>
<td><strong>Überwachung/Archivierung Speicher</strong></td>
<td>Der Name des Azure-Speicher, der die Überwachung Ausgabe des BizTalk Service speichert.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Anforderungen erläutert</a> enthält Informationen über das Speicherkonto.</td>
</tr>
<tr>
<td><strong>Namen</strong></td>
<td>Listet das Abonnement, das BizTalk Service hostet. Das Abonnement steuert den Zugriff auf den Azure-Verwaltungsportal.</td>
</tr>
<tr>
<td><strong>Abonnement-ID</strong></td>
<td>Wenn ein Abonnement erstellt wird, wird automatisch eine Abonnement-ID generiert. Wenn REST-APIs verwenden, müssen Sie die Abonnement-ID eingeben.</td>
</tr>
</table>

[Der BizTalk-Dienste: Bereitstellung mithilfe von Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=302280) Listet die Schritte zum Erstellen einer BizTalk Service.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Verwalten von Verbindungsinformationen Sync-Schlüssel, und löschen Sie in der Taskleiste:

<table border="1">

<tr>
<td><strong>Verwalten von</strong> Bereitstellung der Anwendung</td>
<td>Öffnet das Azure BizTalk-Portal. Das BizTalk-Portal ist der Eingang zu EDI-Konfiguration, einschließlich Partner hinzufügen und X12 AS2, erstellen und EDIFACT.
<br/><br/>
Dies entspricht der <strong>Partnerverträge erstellen</strong> auf der Registerkarte <strong>Quick Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurieren der Komponenten für Messaging EDI in BizTalk Services-Portal</a> bietet weitere Informationen auf das BizTalk-Portal.</td>
</tr>
<tr>
<td><strong>Informationen</strong> von Access Control Namespace</td>
<td>Zeigt Access Control Namespace, Standard Aussteller und Standard Schlüsselwerte. die kopiert werden können.
<br/><br/>
Sie können auch Access Control Portal öffnen. Dieses Portal Zugriff Steuerelement ist identisch mit der Active Directory-Option im linken Navigationsbereich.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Verwalten der ACS Namespace</a> enthält weitere Informationen zu Access Control Portal.</td>
</tr>
<tr>
<td><strong>Synchronisieren der Schlüssel</strong> im Speicherkonto</td>
<td>Ein Speicherkonto erstellen, werden automatisch ein Primärschlüssel und Sekundärschlüssel erstellt. Diese Schlüssel steuern den Zugriff auf das Speicherkonto. BizTalk Service verwendet automatisch den Primärschlüssel. <strong>Sync-Schlüssel</strong> können Benutzer den Primärschlüssel und den Sekundärschlüssel ohne Unterbrechung der BizTalk Service wechseln.
<br/><br/>
Sie möchten z. B. BizTalk Service einen neuen Primärschlüssel für das Speicherkonto verwenden. Dazu:
<br/><br/>
<ol>
<li>Wählen Sie Ihre BizTalk Service und <strong>Sync-Schlüssel</strong>. Wählen Sie die sekundäre Taste. Wenn Sie dies tun, startet der BizTalk Service sekundäre Taste.</li>
<li>Wählen Sie im klassischen Azure-Portal das Speicherkonto und Regenerieren der Primärschlüssel. Beachten Sie, dass BizTalk Service den Sekundärschlüssel verwenden.</li>
<li>Wählen Sie Ihre BizTalk Service und <strong>Sync-Schlüssel</strong>. Wählen Sie jetzt den Primärschlüssel. Dies ist der neue Primärschlüssel Sie regeneriert.</li>
<li>Wählen Sie im klassischen Azure-Portal das Speicherkonto und regenerieren Sie dem Sekundärschlüssel.</li>
</ol>
<br/>
Dieser Vorgang wird "Rollover-Schlüssel" bezeichnet. Damit Benutzer wechseln zwischen den Primärschlüssel und den Sekundärschlüssel ohne Unterbrechung der BizTalk Service dient.</td>
</tr>

<tr>
<td><strong>Löschen Sie</strong> der Anwendung</td>
<td>Der BizTalk-Service und alle Elemente bereitgestellt werden entfernt.</td>
</tr>
</table>


## <a name="monitor"></a>Monitor
Gilt nicht für die kostenlose Version.

Wenn Sie Ihren BizTalk Service-Namen auswählen, die Registerkarte Monitor steht und zeigt Folgendes an:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metrische Graph: Zeigt ausgewählte Leistungsdaten
Diese Metriken bieten Messwerte über den Zustand des BizTalk Services. Sie wählen die Leistungsmetriken angezeigt werden. Maximal sechs Leistungsindikatoren kann gleichzeitig angezeigt werden. 

Sie können auch **Relative** oder **Absolute** Werte und den Zeitraum **Intervall** von Metriken, die angezeigt werden. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Entfernen oder Metriken im Diagramm:
1. Wählen Sie die Registerkarte **Monitor** .
2. Wählen Sie in der Taskleiste **Metriken hinzufügen** :  
![Wählen Sie hinzufügen Kennzahlen][AddMetrics]
3. Überprüfen Sie die Leistungsmetrik, die Sie anzeigen möchten.
4. Wählen Sie das Häkchen wieder die Registerkarte **Monitor** .
5. Wählen Sie den Kreis neben der Metrik die Metrik im Diagramm angezeigt.  

    Beispielsweise ist die **CPU-Auslastung** Metrik grau unterlegt. die Ausgabe wird im Diagramm nicht angezeigt:  
![CPU-Auslastung Metrik ist grau unterlegt.][GrayedMetric]  

    Wählen Sie die grauen Kreis ermöglichen die **CPU-Auslastung** Metrik die Ausgabe im Diagramm angezeigt:  
![CPU-Auslastung Metrik ist aktiviert][EnabledMetric]

6. Um eine Metrik aus das Diagramm anzeigen und der Liste zu entfernen, wählen Sie **Löschen Metrik** in der Taskleiste. Um metrische zurück zur Liste hinzuzufügen, wählen Sie **Metriken hinzufügen** in der Taskleiste, überprüfen Sie die Metrik und das Häkchen wieder die Registerkarte **Monitor** . Auswählen der grauen Kreis die Metrik aktivieren

## <a name="Metrics"></a>Verfügbare statistische Werte
Performance-Indikatoren/Werte sind verfügbar:

<table border="1">

<tr>
<td><strong>RountdTrip-Wartezeit</strong></td>
<td>Zeigt die durchschnittliche Verarbeitungszeit in Millisekunden (ms) für eine Meldung ab, die die Nachricht empfangen wird, bis die Nachricht in BizTalk Service vollständig verarbeitet wird über alle Brücken. Nur erfolgreich verarbeitete Nachrichten werden gezählt.<br/><br/>
Bei den folgenden Ereignissen wird ein Zeitstempel erstellt:
<ul>
<li>Meldung gibt das gateway</li>
<li>Nachricht wird an das Ziel weitergeleitet.</li>
<li>Ziel-Antwort empfangen</li>
<li>Ziel Bestätigung Antwort an das Gateway gesendet</li>
</ul>
<br/>
Diese Metrik zeigt das Ergebnis der folgenden Berechnung:
<br/><br/>
[Ziel Bestätigung Antwort an das Gateway gesendet] - [Meldung gibt das Gateway]</td>
</tr>
<tr>
<td><strong>Fehler an der Quelle</strong></td>
<td>Zeigt die Gesamtanzahl der Nachrichten, die nicht von BizTalk Service Nachrichten Quelle Endpunkte ziehen.</td>
</tr>
<tr>
<td><strong>CPU-Auslastung</strong></td>
<td>Listet die durchschnittliche Prozessorzeit alle Instanzen.</td>
</tr>
<tr>
<td><strong>Verarbeitung Wartezeit</strong></td>
<td>Zeigt die durchschnittliche Verarbeitungszeit In Millisekunden zum Verarbeiten einer Nachricht von BizTalk Service alle Brücken, ausgenommen die Zeit Ziele. Nur erfolgreich verarbeitete Nachrichten werden gezählt.<br/><br/>
Bei jeder der folgenden Ereignisse wird ein Zeitstempel erstellt:

<ul>
<li>Meldung gibt das gateway</li>
<li>Nachricht wird an das Ziel weitergeleitet.</li>
<li>Ziel-Antwort empfangen</li>
<li>Ziel Bestätigung Antwort an das Gateway gesendet</li>
</ul>
<br/>Diese Metrik zeigt das Ergebnis der folgenden Berechnung:<br/><br/>
[Ziel Bestätigung Antwort an das Gateway gesendet] - [Meldung gibt das Gateway] - [Ziel ist Antwort] + [Nachricht an das Ziel weitergeleitet.]</td>
</tr>
<tr>
<td><strong>Fehler bei Vorgang</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten, die während der Verarbeitung durch BizTalk Service über die Brücken innerhalb eines Zeitintervalls konnten.</td>
</tr>
<tr>
<td><strong>Gesendete Nachrichten</strong></td>
<td>Zeigt die Gesamtanzahl der Nachrichten von BizTalk Service über alle Brücken innerhalb eines Zeitintervalls. Diese Metrik wird erhöht, wenn eine Nachricht von einer Rohrleitung das Ziel erreicht. Diese Metrik bedeutet nicht, dass eine Nachricht verarbeitet wird.<br/><br/>
In einem Szenario mit Anforderung und Antwort wird die Metrik erhöht, sendet das Ziel eine Empfangsbestätigung Bestätigung in der Pipeline.</td>
</tr>
<tr>
<td><strong>Empfangene Nachrichten</strong></td>
<td>Zeigt die Gesamtanzahl der Nachrichten von BizTalk Service über alle Brücken innerhalb eines Zeitintervalls. Diese Metrik wird erhöht, wenn eine neue Nachricht von der Pipeline empfangen wird.</td>
</tr>
<tr>
<td><strong>Nachrichten im Prozess</strong></td>
<td>Zeigt die Gesamtanzahl der derzeit verarbeiteten Nachrichten von BizTalk Service innerhalb eines Zeitintervalls.</td>
</tr>
<tr>
<td><strong>Verarbeitete Nachrichten</strong></td>
<td>Zeigt die Gesamtzahl der Nachrichten verarbeitet BizTalk Service über alle Brücken innerhalb eines Zeitintervalls. Diese Metrik wird erhöht, wenn eine Nachricht von der Pipeline empfangen und erfolgreich zum Ziel weitergeleitet.</td>
</tr>
</table>


## <a name="scale"></a>Skalierung
Auf der Registerkarte Skalierung können Sie addieren oder subtrahieren die Anzahl der Einheiten der BizTalk Service. Es wird standardmäßig ein Gerät konfiguriert. Zusätzliche Einheiten können BizTalk Service skalieren hinzugefügt werden. Wenn Sie die Skalierung erhöhen, erhöhen Durchsatz. Ressourcen hinaus, einschließlich bereitgestellten Brücken, Abkommen, LOB-Verbindungen und Verarbeitung. Erhöhen Sie beispielsweise die Skalierung von 1 Einheit 2 Einheiten. In diesem Fall können Sie bereitstellen doppelt so viele Brücken, Abkommen Doppelklicken LOB-Verbindungen doppelklicken und verdoppeln die Leistung.

Einige BizTalk-Editionen bieten keine Scale-Option. In diesem Fall darf eine Einheit. Ermitteln, wie viele Einheiten die Edition skaliert werden kann, finden Sie in [BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md).

Die Zahl der Einheiten kann als Preise auswirken. Erhöht die Einheiten zeigt das **auswählen** eine Meldung, wenn Abrechnung beeinträchtigt wird. Sie wählen dann fort. Wenn erhöhen Sie die Anzahl der Einheiten, BizTalk Service Status ändert sich von aktiv zu aktualisieren. BizTalk Service weiterhin in der Aktualisierung ausgeführt.

[Der BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md) "Einheiten" definiert.


## <a name="configure"></a>Konfigurieren
Gilt nicht für Hybridverbindungen.

Setzt den Sicherungsstatus auf None oder automatisch. Festlegen, ohne automatisch Sicherungskopien erstellt werden. Wenn auf automatisch gesetzt, den Speicherort, die Häufigkeit der Sicherung und wie lange die Sicherungsdateien zu konfigurieren. 

[Der BizTalk-Dienste: Sicherung und Wiederherstellung](biztalk-backup-restore.md) beschrieben. 


## <a name="HybridConnections"></a>Hybridverbindungen
Hybride Verbindung eine Azure-Anwendung wie Web Apps oder Mobile in Azure App Service auf eine lokale Ressource, die einen statischen TCP-Port wie SQL Server, MySQL, HTTP-Web-APIs und die meisten benutzerdefinierte Webdienste verwendet. Hybridverbindungen werden im klassischen Azure-Portal BizTalk-Dienste verwaltet.

Hybrid-Verbindungen in Azure App Service erstellen, finden Sie unter [Zugriff auf lokale Ressourcen hybridverbindungen in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).

Erstellen oder Hybrid-Verbindungen Azure BizTalk-Dienste verwalten, finden Sie unter [Hybridverbindungen](integration-hybrid-connection-overview.md).



## <a name="next"></a>Weiter
Nun, dass Sie die verschiedenen Registerkarten kennen, erfahren Sie mehr über die Funktionen von Azure BizTalk-Dienste:

- [BizTalk-Dienste: Drosselung](biztalk-throttling-thresholds.md)  
- [BizTalk-Dienste: Ausstellername und Aussteller Schlüssel](biztalk-issuer-name-issuer-key.md)  
- [Der BizTalk-Dienste: Sicherung und Wiederherstellung](biztalk-backup-restore.md)

## <a name="see-also"></a>Siehe auch
- [Hybridverbindungen](integration-hybrid-connection-overview.md)  
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium Editions Diagramm](biztalk-editions-feature-chart.md)  
- [BizTalk-Dienste: Bereitstellung mithilfe von Azure-Verwaltungsportal](biztalk-provision-services.md)  
- [BizTalk-Dienste: BizTalk-Dienststatus Diagramm](biztalk-service-state-chart.md)  
- [Wie kann ich starten mithilfe von Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
