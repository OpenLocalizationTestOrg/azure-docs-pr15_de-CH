<properties
    pageTitle="Web app verlangsamen in App Service | Microsoft Azure"
    description="Dieser Artikel hilft Ihnen langsam Web app in Azure App Service Leistungsproblemen."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="Web app Leistung langsam app app langsam"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Problembehandlung bei langsamen Web app Leistungsprobleme in Azure App Service

Dieser Artikel hilft Ihnen langsam Web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)Leistungsproblemen.

Benötigen Sie weitere Hilfe zu diesem Artikel erhalten Sie Azure-Experten auf [MSDN Azure und Foren Stapelüberlauf](https://azure.microsoft.com/support/forums/). Alternativ können Sie eine Azure Supportfall Datei. Der [Azure-Support-Website](https://azure.microsoft.com/support/options/) und klicken Sie auf **Support zu erhalten**.

## <a name="symptom"></a>Symptom

Beim Browsen Web app Seiten laden langsam und Timeout.

## <a name="cause"></a>Ursache

Dieses Problem wird häufig durch Anwendung auf Probleme ausgelöst:

-   Anfragen dauert sehr lange
-   mit Speicher/Anwendung
-   die Anwendung aufgrund einer Ausnahme abstürzt.

## <a name="troubleshooting-steps"></a>Schritte zur Fehlerbehebung

Problembehandlung in drei unterschiedliche Aufgaben nacheinander unterteilen:

1.  [Beobachten und Verhalten der Anwendung überwachen](#observe)
2.  [Sammeln von Daten](#collect)
3.  [Minderung des Problems](#mitigate)

[App Service Web Apps](/services/app-service/web/) können Sie verschiedene Optionen bei jedem Schritt.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. beobachten und Verhalten der Anwendung überwachen

#### <a name="track-service-health"></a>Dienststatus überwachen

Microsoft Azure veröffentlicht jedes Mal eine Service-Unterbrechung oder eine Leistungsminderung. Sie können den Zustand des Dienstes in [Azure-Portal](https://portal.azure.com/)nachverfolgen. Weitere Informationen finden Sie unter [Dienststatus verfolgen](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Überwachen Sie Ihrer Anwendung

Diese Option können Sie herausfinden, ob Ihre Anwendung Probleme auftreten. Ihre Web app Blatt klicken Sie **Anfragen und Fehler** . Das Blade **Metrik** zeigt Ihnen alle Metriken können Sie.

Metriken, die Sie möglicherweise überwachen Ihrer Anwendung möchten sind

-   Durchschnittliche Arbeitsspeicher Arbeitssatz
-   Durchschnittliche Antwortzeit
-   CPU-Zeit
-   Arbeit
-   Anfragen

![Web app Leistung überwachen](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Weitere Informationen finden Sie unter:

-   [Monitor Web Apps in Azure App Service](web-sites-monitor.md)
-   [Benachrichtigung erhalten](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Web-Endpunktstatus überwachen

Wenn Sie Ihrer Anwendung im **Standard** Tarif ausgeführt, können Web Apps 2 Endpunkte von 3 Standorten zu überwachen.

Überwachen der Endpunkt konfiguriert Webtests von geografisch verteilten Standorten, die Reaktionszeit und Verfügbarkeit von Web-URLs zu testen. Der Test führt eine HTTP GET-Operation für bestimmt die Reaktionszeit und Verfügbarkeit von jedem Web-URL. Jeder konfigurierten Speicherort führt einen Test fünf Minuten.

Betriebszeit mit HTTP-Antwortcodes überwacht und Antwortzeit in Millisekunden gemessen. Überwachung Nichterfüllung der HTTP-Antwortcode ist größer als oder gleich 400 oder die Antwort mehr als 30 Sekunden dauert. Ein Endpunkt gilt gelingt Überwachung Tests aus den angegebenen Ordnern verfügbar.

Einrichten, finden Sie unter [wie: Überwachen Endpunktstatus Web](web-sites-monitor.md#webendpointstatus).

Siehe auch [halten Azure Websites, plus Endpunkt Überwachen - mit Stefan Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) ein Video zum Endpunkt zu überwachen.

#### <a name="application-performance-monitoring-using-extensions"></a>Anwendung mit Extensions Performance-Überwachung

Sie können auch die Leistung Ihrer Anwendung durch Nutzung der _Website Extensions_überwachen.

Jede App Service Web app bietet einen erweiterbare Verwaltung Endpunkt, der leistungsfähigsten Tools als Website Extensions nutzen können. Diese Tools reichen von Code-Editoren, wie [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) Verwaltungstools für Ressourcen wie eine MySQL-Datenbank mit einer Webanwendung verbunden.

[Azure Anwendung Einblicke](/services/application-insights/) und [Neue Relikt](/marketplace/partners/newrelic/newrelic/) sind zwei Systemmonitor Website Erweiterungen verfügbar. Um neue Relikt verwenden, installieren Sie einen Agent zur Laufzeit. Verwendung von Azure-Anwendung Einblicke rebuild Code mit einem SDK und Sie können auch eine Erweiterung, die Zugriff auf zusätzliche Daten. Das SDK können Sie Code schreiben, um die Auslastung und Leistung der Anwendung genauer überwachen.

Application Insights finden Sie unter [Überwachen der Leistung in ASP.NET-Webanwendungen](../application-insights/app-insights-web-monitor-performance.md).

Neue Relikt finden Sie unter [Neue Relikt Application Performance Management auf Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. Daten

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Aktivieren des Diagnoseprotokolls für Ihr Web app

Web Apps-Umgebung bietet diagnostische Funktionen zum Protokollieren von Informationen aus der Webserver und der Anwendung. Diese sind logisch in Web Server Fehlerdiagnose und Anwendung Diagnose getrennt.

##### <a name="web-server-diagnostics"></a>Web Server-Diagnose

Sie können aktivieren oder deaktivieren folgende Protokolle:

-   **Detaillierte Fehler protokollieren** - Informationen für HTTP-Statuscodes, die einen Fehler (Statuscode 400 oder höher). Dies kann Informationen enthalten, die helfen zu bestimmen, warum der Server den Fehlercode zurückgegeben.
-   **Fehler bei Anforderung Tracing** - detaillierte Informationen zu fehlgeschlagenen Anfragen Trace verarbeitet die Anforderung und die einzelnen Komponenten verwendeten IIS-Komponenten. Dies ist hilfreich, wenn Sie versuchen, Web app Leistung oder isolieren, was einen bestimmten HTTP-Fehler verursacht.
-   **Web Server anmelden** - Informationen über HTTP-Transaktionen mit der W3C-Protokolldateiformat. Dies ist nützlich beim Bestimmen der gesamten Web app Metriken wie die Anfragen behandelt oder wie viele Anfragen von einer bestimmten IP-Adresse.

##### <a name="application-diagnostics"></a>Anwendung-Diagnose

Anwendung-Diagnose können Sie Informationen von einer Webanwendung erfassen. ASP.NET Applications können die `System.Diagnostics.Trace` Klasse Informationen im Anwendungsprotokoll Diagnose protokollieren.

Informationen zum Konfigurieren einer Anwendung für die Protokollierung finden Sie unter [Diagnoseprotokoll für Web-apps in Azure App Service aktivieren](web-sites-enable-diagnostic-log.md).

#### <a name="use-remote-profiling"></a>Remoteprofilerstellung verwenden

In Azure App Service können Remote Web Apps, API-Apps und Webaufträge profiliert werden. Wenn der Prozess langsamer läuft als erwartet, oder die Wartezeit von HTTP-Anfragen höher und die CPU-Auslastung des Prozesses ebenfalls hoch, können Sie Remote Profil des Prozesses und CPU-Sampling Aufruflisten Verarbeitungsaktivität und hot Codepfade zu analysieren.

Weitere Hinweise finden Sie unter [Remoteprofilerstellung Unterstützung in Azure App Service](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Verwenden von Azure App Service Support-Portal

Web Apps bietet die Möglichkeit von Problemen im Zusammenhang mit Ihrer Anwendung auf HTTP-Protokolle, Ereignisprotokolle, Prozess Dumps und mehr. Alle diese Informationen über unser Supportportal unter Zugriff **http://&lt;Namen >.scm.azurewebsites.net/Support**

Unterstützung von Azure App Service Portal bietet drei separate Registerkarten die drei Schritte zur Problembehandlung häufig unterstützt:

1.  Aktuelle Verhalten
2.  Analysieren von Diagnoseinformationen sammeln und integrierte Analyzer ausführen
3.  Minimieren

Wenn das Problem jetzt geschieht, klicken Sie auf **Analyse** > **Diagnose** > **Jetzt Diagnose** erstellt ein, die HTTP sammeln anmeldet, Ereignisanzeige, Speicher Dumps, PHP Fehlerprotokolle und PHP Bericht verarbeiten.

Nachdem die Daten gesammelt werden, wird auch eine Analyse der Daten ausgeführt und bieten einen HTML-Bericht.

Falls Sie die Daten standardmäßig herunterladen möchten, wird es im Ordner D:\home\data\DaaS gespeichert.

Weitere Informationen zu Azure App Service Support-Portal finden Sie unter [Neue Updates Support Site Erweiterung für Azure Websites](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Kudu der Debug-Konsole verwenden

Web Apps enthält eine Debugkonsole, die Sie Debuggen, untersuchen, Hochladen von Dateien sowie JSON Endpunkte zum Abrufen von Informationen über Ihre Umgebung verwenden können. Dies ist der _Kudu-Konsole_ oder _SCM Dashboard_ für Ihre Web bezeichnet.

Mit diesem Dashboard können auf den Link zugreifen **https://&lt;Namen >.scm.azurewebsites.net/**.

Was Kudu bietet gehören:

-   umgebungseinstellungen für Ihre Anwendung
-   Datenstrom
-   diagnostische dump
-   Debug-Konsole in der Powershell-Cmdlets und grundlegende DOS-Befehle ausführen.


Ein weiteres nützliches Feature von Kudu ist, dass Ihre Anwendung nicht abgefangene Ausnahmen auslösen, Kudu können und SysInternals-Tool Services Speicher sichert. Diese Speicherabbilder werden Snapshots des Prozesses und häufig komplexen Probleme mit Ihrer Anwendung bei der Problembehandlung hilfreich.

Weitere Informationen zu Funktionen in Kudu finden Sie unter [Azure Websites Team Services-Tools, denen Sie kennen sollten](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. das Problem verringern

####    <a name="scale-the-web-app"></a>Die Webanwendung skalieren

In Azure App Service können für mehr Leistung und Durchsatz Sie skalieren mit der Ihre Anwendung ausgeführt wird. Web app Heraufskalierung zwei verwandte Aktionen: App Service-Plan in einen höheren Tarif ändern und Konfigurieren bestimmter Preise hochgestuft geschaltet haben.

Weitere Informationen zu skalieren finden Sie unter [Skalierung eine Webanwendung in Azure App Service](web-sites-scale.md).

Darüber hinaus können Sie auf mehrere Instanzen die Anwendung ausgeführt. Nicht nur mehr Verarbeitungsfunktionen bietet dies ermöglicht ebenfalls gewisse Fehlertoleranz. Wenn der Prozess auf einmal ausfällt, weiterhin die andere Instanz bedient keine weiteren Anfragen.

Sie können die Skalierung manuell oder automatisch festlegen.

####    <a name="use-autoheal"></a>AutoHeal verwenden

AutoHeal Wiederverwendung des Arbeitsprozesses für Ihre app auf Einstellungen (z. B. Konfiguration, Anfragen, Grenzwerte Speicher oder die Zeit zum Ausführen einer Anforderung). Meistens, ist recyceln des Prozesses die schnellste Möglichkeit, ein Problem zu beheben. Obwohl immer Web app direkt in Azure-Portal starten können, tun AutoHeal es automatisch für Sie. Müssen Sie lediglich einige Trigger in der Root.Web.config Ihrer Anwendung hinzufügen. Beachten Sie, dass diese Einstellung auch wenn Ihre Anwendung nicht eine .net genauso funktioniert.

Weitere Informationen finden Sie unter [Selbstkonfiguration Azure-Websites](/blog/auto-healing-windows-azure-web-sites/).

####    <a name="restart-the-web-app"></a>Die Webanwendung starten

Dies ist oft die einfachste Möglichkeit, einmalige Probleme beheben. [Azure-Portal](https://portal.azure.com/)können auf Ihre Web-app, Sie beenden oder starten Sie Ihre Anwendung neu.

 ![Starten von WebApp Leistungsprobleme beheben](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Sie können auch Ihrer Anwendung mithilfe von Azure Powershell verwalten. Weitere Informationen finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).
