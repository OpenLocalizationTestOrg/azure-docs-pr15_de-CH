<properties
    pageTitle="Leistungsindikatoren für mobile webapps mit Developer Analytics | Microsoft Azure"
    description="Leistung der Anwendung und Überwachung der Verwendung für mobile Entwickler. , Desktop, WebService und Back-End-apps mit HockeyApp Anwendung."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobile Analysen mit HockeyApp Anwendung

Überwachen Sie die Leistung und die Verwendung der Beta-Test und bereitgestellte mobile und desktop-apps mit [HockeyApp](https://hockeyapp.net/). Überwachen Sie den unterstützenden Web Service und Back-End-apps mit [Visual Studio Application Insights](app-insights-overview.md). Datenanalyse von Client und Server apps in Analytics und Diagramme nebeneinander in Azure Dashboard anzeigen.

Microsoft Developer Analytics bietet zwei Optionen für die Performance-Überwachung:

* **HockeyApp für Mobile** und Desktop-apps.
 * Verteilen Sie Testversionen für ausgewählte Benutzer.
 * Crash Analysis.
 * Benutzerdefinierte Telemetrie für die Verwendungsanalyse.
* **Anwendung Einblicke für Websites** und Dienste und Back-End-Anwendung.
 * Leistung und Auslastung Metriken und Warnungen.
 * Ausnahme-reporting und Diagnoseprotokollierungstypen.
 * Diagnostische Suche mit Filtern und verwandte Telemetrie.

Beide bieten:

 * Leistungsstarke **[analytische Abfragesprache](app-insights-analytics.md)** für Diagnose und Analyse.
 * **[Exportieren von Daten](app-insights-export-telemetry.md)** in Ihren eigenen Speicher.
 * **Integriertes Dashboard** Anzeige Analysediagramme und Tabellen.

## <a name="monitor-your-app-components"></a>Überwachen Sie Ihre app-Komponenten

Führen Sie die Schritte dieser Seiten zu SDK im Code Überwachen Ihrer app.

### <a name="web-apps---application-insights"></a>Web apps - Anwendung Einblicke

* [ASP.NET WebApp](app-insights-asp-net.md) 
* [Java WebApp](app-insights-java-get-started.md)
* [Node.js WebApp](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Azure-Cloud-Dienste](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobiler apps - HockeyApp

* [iOS-app](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X-Anwendung](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Android](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Universelle Windows-app](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Windows Phone 8 und 8.1 app](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Windows Presentation Foundation-Anwendung](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Windows desktop-apps empfehlen wir HockeyApp. Sie können aber auch [Telemetriedaten aus einer app Windows Application Insights senden](app-insights-windows-desktop.md). Sie möchten das Experimentieren mit Application Insights.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Analytics und Export HockeyApp Telemetrie

HockeyApp benutzerdefinierte untersuchen, und melden Sie sich mit der Analyse und kontinuierliche exportieren Anwendung Erkenntnisse durch [eine Brücke](app-insights-hockeyapp-bridge-app.md)Telemetrie.




