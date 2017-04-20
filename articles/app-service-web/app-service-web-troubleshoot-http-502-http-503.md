<properties
    pageTitle="502 unzulässiges Gateway beheben, 503 Dienst nicht verfügbar Fehler | Microsoft Azure"
    description="Problembehandlung bei 502 unzulässiges Gateway und 503 Dienst nicht verfügbar Fehler in Ihrer Anwendung in Azure App Service gehostet."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 unzulässiges Gateway-503 Dienst nicht verfügbar Fehler 503, Fehler 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>Problembehandlung bei HTTP-Fehlern "502 unzulässiges Gateway" und "503 Service unavailable" in Azure Web apps

"502 unzulässiges Gateway" und "503 Service unavailable" sind häufige Fehler in Ihrer Anwendung in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)gehostet. Dieser Artikel hilft diese Fehler beheben.

Benötigen Sie weitere Hilfe zu diesem Artikel erhalten Sie Azure-Experten auf [MSDN Azure und Foren Stapelüberlauf](https://azure.microsoft.com/support/forums/). Alternativ können Sie eine Azure Supportfall Datei. Der [Azure-Support-Website](https://azure.microsoft.com/support/options/) und klicken Sie auf **Support zu erhalten**.

## <a name="symptom"></a>Symptom

Beim Durchsuchen der Web App gibt eine HTTP "502-Fehlerhaftes Gateway" oder ein HTTP-Fehler "503 Service Unavailable".

## <a name="cause"></a>Ursache

Dieses Problem wird häufig durch Anwendung auf Probleme ausgelöst:

-   Anfragen dauert sehr lange
-   mit Speicher/Anwendung
-   die Anwendung aufgrund einer Ausnahme abstürzt.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Problembehandlungsschritte "502 unzulässiges Gateway" und "503 Service unavailable"-Fehler

Problembehandlung in drei unterschiedliche Aufgaben nacheinander unterteilen:

1.  [Beobachten und Verhalten der Anwendung überwachen](#observe)
2.  [Sammeln von Daten](#collect)
3.  [Minderung des Problems](#mitigate)

[App Service Web Apps](/services/app-service/web/) können Sie verschiedene Optionen bei jedem Schritt.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. beobachten und Verhalten der Anwendung überwachen

####    <a name="track-service-health"></a>Dienststatus überwachen

Microsoft Azure veröffentlicht jedes Mal eine Service-Unterbrechung oder eine Leistungsminderung. Sie können den Zustand des Dienstes in [Azure-Portal](https://portal.azure.com/)nachverfolgen. Weitere Informationen finden Sie unter [Dienststatus verfolgen](../monitoring-and-diagnostics/insights-service-health.md).

####    <a name="monitor-your-web-app"></a>Überwachen Sie Ihrer Anwendung

Diese Option können Sie herausfinden, ob Ihre Anwendung Probleme auftreten. Ihre Web app Blatt klicken Sie **Anfragen und Fehler** . Das Blade **Metrik** zeigt Ihnen alle Metriken können Sie.

Metriken, die Sie möglicherweise überwachen Ihrer Anwendung möchten sind

-   Durchschnittliche Arbeitsspeicher Arbeitssatz
-   Durchschnittliche Antwortzeit
-   CPU-Zeit
-   Arbeit
-   Anfragen

![Monitor WebApp zur Lösung HTTP-Fehler 502 unzulässiges Gateway und 503 Dienst nicht verfügbar](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Weitere Informationen finden Sie unter:

-   [Monitor Web Apps in Azure App Service](web-sites-monitor.md)
-   [Benachrichtigung erhalten](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2. Daten

####    <a name="use-the-azure-app-service-support-portal"></a>Verwenden von Azure App Service Support-Portal

Web Apps bietet die Möglichkeit von Problemen im Zusammenhang mit Ihrer Anwendung auf HTTP-Protokolle, Ereignisprotokolle, Prozess Dumps und mehr. Alle diese Informationen über unser Supportportal unter Zugriff **http://&lt;Namen >.scm.azurewebsites.net/Support**

Unterstützung von Azure App Service Portal bietet drei separate Registerkarten die drei Schritte zur Problembehandlung häufig unterstützt:

1.  Aktuelle Verhalten
2.  Analysieren von Diagnoseinformationen sammeln und integrierte Analyzer ausführen
3.  Minimieren

Wenn das Problem jetzt geschieht, klicken Sie auf **Analyse** > **Diagnose** > **Jetzt Diagnose** erstellt ein, die HTTP sammeln anmeldet, Ereignisanzeige, Speicher Dumps, PHP Fehlerprotokolle und PHP Bericht verarbeiten.

Nachdem die Daten gesammelt werden, wird auch eine Analyse der Daten ausgeführt und bieten einen HTML-Bericht.

Falls Sie die Daten standardmäßig herunterladen möchten, wird es im Ordner D:\home\data\DaaS gespeichert.

Weitere Informationen zu Azure App Service Support-Portal finden Sie unter [Neue Updates Support Site Erweiterung für Azure Websites](/blog/new-updates-to-support-site-extension-for-azure-websites).

####    <a name="use-the-kudu-debug-console"></a>Kudu der Debug-Konsole verwenden

Web Apps enthält eine Debugkonsole, die Sie Debuggen, untersuchen, Hochladen von Dateien sowie JSON Endpunkte zum Abrufen von Informationen über Ihre Umgebung verwenden können. Dies ist der _Kudu-Konsole_ oder _SCM Dashboard_ für Ihre Web bezeichnet.

Mit diesem Dashboard können auf den Link zugreifen **https://&lt;Namen >.scm.azurewebsites.net/**.

Was Kudu bietet gehören:

-   umgebungseinstellungen für Ihre Anwendung
-   Datenstrom
-   diagnostische dump
-   Debug-Konsole in der Powershell-Cmdlets und grundlegende DOS-Befehle ausführen.


Ein weiteres nützliches Feature von Kudu ist, dass Ihre Anwendung nicht abgefangene Ausnahmen auslösen, Kudu können und SysInternals-Tool Services Speicher sichert. Diese Speicherabbilder werden Snapshots des Prozesses und häufig komplexen Probleme mit Ihrer Anwendung bei der Problembehandlung hilfreich.

Weitere Informationen zu Funktionen in Kudu finden Sie unter [Azure Websites online-Tools, denen Sie kennen sollten](/blog/windows-azure-websites-online-tools-you-should-know-about/).

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

 ![Starten der app lösen HTTP-Fehler 502 unzulässiges Gateway und 503 Dienst nicht verfügbar](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Sie können auch Ihrer Anwendung mithilfe von Azure Powershell verwalten. Weitere Informationen finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).
