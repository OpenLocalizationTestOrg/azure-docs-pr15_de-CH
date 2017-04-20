<properties 
    pageTitle="Überwachung der Nutzung und Leistung für die Windows desktop-apps" 
    description="Verwendung und Leistung der Windows Desktop-Anwendung mit HockeyApp Anwendung zu analysieren." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Überwachung der Nutzung und Leistung in Windows-Desktop-apps

*Anwendung Informationen ist in der Vorschau.*

[Visual Studio Application Insights](app-insights-overview.md) und [HockeyApp](https://hockeyapp.net) können Sie die bereitgestellte Anwendung für die Verwendung und Leistung zu überwachen.

> [AZURE.IMPORTANT] [HockeyApp](https://hockeyapp.net) verteilen und Desktop- und apps überwachen sollten. Mit HockeyApp können Sie Verteilung live testen und Feedback von Benutzern zu verwalten sowie Verwendung und Crash-Berichte überwachen. Sie können auch [Exportieren und Abfragen der Telemetrie-Analysefunktionen](app-insights-hockeyapp-bridge-app.md).

> Obwohl Telemetrie Anwendung Erkenntnisse aus einer desktop-Anwendung gesendet werden kann, dies ist vor allem nützlich zum Debuggen und experimentelle.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>An Telemetrie Anwendung Einblicke aus einer Windows-Anwendung

1. Im [Azure-Portal](https://portal.azure.com) [Application Insights-Ressource erstellen](app-insights-create-new-resource.md). Anwendungstyp wählen Sie ASP.NET app.
2. Erstellen Sie eine Kopie der Instrumentation. Suchen Sie den Schlüssel in der Dropdownliste Essentials die neue Ressource, die Sie gerade erstellt haben. 
3. In Visual Studio die NuGet-Pakete des Projekts app bearbeiten und Microsoft.ApplicationInsights.WindowsServer hinzufügen. (Oder wählen Sie Microsoft.ApplicationInsights, wenn Sie nur das API ohne Module Auflistung standard Telemetrie.)
4. Festlegen des Instrumentation Schlüssels entweder im Code:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*der Schlüssel*`";` 

    oder ApplicationInsights.config (Wenn Sie einen standard Telemetrie Pakete installiert):
 
    `<InstrumentationKey>`*der Schlüssel*`</InstrumentationKey>` 

    Wenn Sie ApplicationInsights.config verwenden, müssen Sie seine Eigenschaften im Projektmappen-Explorer sollen **Build Action = Content in Ausgabeverzeichnis kopieren Kopieren =**.
5. [Verwenden der API](app-insights-api-custom-events-metrics.md) Telemetrie senden.
6. Führen Sie die Anwendung, und Telemetrie in Azure-Portal erstellte Ressource angezeigt.

## <a name="telemetry"></a>Beispielcode

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Dashboards](app-insights-dashboards.md)
* [Diagnose suchen](app-insights-diagnostic-search.md)
* [Untersuchen von Metriken](app-insights-metrics-explorer.md)
* [Analytics Abfragen schreiben](app-insights-analytics.md)
 
