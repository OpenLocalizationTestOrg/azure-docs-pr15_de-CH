<properties
    pageTitle="Mithilfe von Powershell Alarme in Application Insights festlegen"
    description="Automatisieren Sie Konfiguration der Anwendung Einblicke in e-Mails metrische Änderungen."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Mithilfe von PowerShell Alarme in Application Insights festlegen

Sie können die Konfiguration der [Alarme](app-insights-alerts.md) in [Visual Studio Application Insights](app-insights-overview.md)automatisieren.

Darüber hinaus können Sie [Webhooks zum Automatisieren der Antwort auf eine Warnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Einmalige Installation

Wenn Sie Ihr-Abonnement Azure vor PowerShell verwendet haben:

Installieren Sie das Azure Powershell-Modul für die Skripts ausgeführt werden soll.

 * Installieren [Microsoft-Webplattform-Installer (v5 oder höher)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Installieren Sie Microsoft Azure Powershell verwenden


## <a name="connect-to-azure"></a>Verbinden mit Azure

Starten Sie Azure PowerShell und [Ihr Abonnement an](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Alarme

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Benachrichtigung hinzufügen


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Beispiel 1

E-Mail-Benachrichtigung senden, wenn die Antwort des Servers auf HTTP-Anfragen, gemittelt über 5 Minuten langsamer als 1 Sekunde ist. Meine Anwendung Einblicke heißt IceCreamWebApp und ist in Ressourcengruppe Fabrikam. Ich bin Besitzer der Azure-Abonnement.

Die GUID ist die Abonnement-ID (nicht der Anwendung instrumentationsschlüssel).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Beispiel 2

Ich habe eine Anwendung, in der ich [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) mit Bericht mit dem Namen "SalesPerHour" Metrik Senden Sie eine e-Mail an Kollegen fällt "SalesPerHour" unter 100 über 24 Stunden gemittelt.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Dieselbe Regel kann mit der [Messgröße](app-insights-api-custom-events-metrics.md#properties) einen anderen Tracking-Aufruf wie TrackEvent oder TrackPageView gemeldet Metrik verwendet werden.

## <a name="metric-names"></a>Metrische Namen

Metrische name | Pseudonym | Beschreibung
---|---|---
`basicExceptionBrowser.count`|Browser-Ausnahmen|Anzahl der nicht abgefangene Ausnahmen im Browser.
`basicExceptionServer.count`|Server-Ausnahmen|Anzahl der nicht behandelte Ausnahmen von der app
`clientPerformance.clientProcess.value`|Client-Verarbeitungszeit|Zeit zwischen das letzte Byte eines Dokuments erhalten, bis das DOM geladen ist. Asynchrone Anfragen möglicherweise weiterhin verarbeitet.
`clientPerformance.networkConnection.value`|Netzwerk verbinden Seitenladezeit| Zeit, der Browser mit dem Netzwerk herstellen. 0 kann sein, wenn zwischengespeichert.
`clientPerformance.receiveRequest.value`|Antwort empfangen| Zeit zwischen Browser Anforderung auf Antwort senden.
`clientPerformance.sendRequest.value`|Anfrage senden| Zeit von Browser-Anforderung senden.
`clientPerformance.total.value`|Browser Seitenladezeit|Benutzer-Anforderung bis DOM, Stylesheets, Skripts und Bilder geladen werden.
`performanceCounter.available_bytes.value`|Arbeitsspeicher|Arbeitsspeicher für einen Prozess oder für das System sofort verfügbar.
`performanceCounter.io_data_bytes_per_sec.value`|Prozess e/a-Rate|Gesamtanzahl der Bytes pro Sekunde gelesenen und geschriebenen Dateien, Netzwerk und Geräte.
`performanceCounter.number_of_exceps_thrown_per_sec`|Exception-rate|Ausnahmen pro Sekunde.
`performanceCounter.percentage_processor_time.value`|Prozess-CPU|Der Prozentsatz der verstrichenen Zeit alle Prozessthreads Prozessor Ausführung Anleitung für den Prozess Applikationen verwendet.
`performanceCounter.percentage_processor_total.value`|Prozessorzeit|Der Prozentsatz der Zeit, die der Prozessor nicht im Leerlauf befindlichen Threads benötigt.
`performanceCounter.process_private_bytes.value`|Prozess private bytes|Ausschließlich für die überwachte Anwendung Prozesse zugewiesenen Speichers.
`performanceCounter.request_execution_time.value`|ASP.NET Ausführungszeit der Anforderung|Ausführungszeit der letzten Anforderung
`performanceCounter.requests_in_application_queue.value`|ASP.NET Anfragen in Warteschlange|Länge der Anforderungswarteschlange Anwendung.
`performanceCounter.requests_per_sec`|ASP.NET Anforderungsrate|Rate der alle Anfragen pro Sekunde Anwendung von ASP.NET.
`remoteDependencyFailed.durationMetric.count`|Abhängigkeitsfehler|Anzahl der fehlgeschlagenen Aufrufe von der Serveranwendung auf externe Ressourcen.
`request.duration`|Antwortzeit|Zeit zwischen eine HTTP-Anforderung empfangen und Senden der Antwort.
`request.rate`|Anforderungsrate|Rate der alle Anfragen an die Anwendung pro Sekunde.
`requestFailed.count`|Fehlgeschlagene Anfragen|Anzahl der HTTP-Anfragen, die den Antwortcode führte > = 400
`view.count`|Seitenaufrufe|Anzahl der Benutzer Clientanforderungen für eine Webseite. Synthetischer Datenverkehr herausgefiltert wird.
{der benutzerdefinierte Metrik Name}|{Metrikname}|Der Metrikwert gemeldet [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) oder [Maßangaben Parameter eines Aufrufs nachverfolgen](app-insights-api-custom-events-metrics.md#properties).

Die Metriken werden verschiedene Telemetrie Module:

Metrische Gruppe | Kollektor-Modul
---|---
BasicExceptionBrowser,<br/>ClientPerformance,<br/>Ansicht | [Browser JavaScript](app-insights-javascript.md)
performanceCounter | [Leistung](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Abhängigkeit](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
Anforderung<br/>Fehler bei Anforderung|[Anforderung](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

Sie können [Ihre Antwort auf eine Warnung zu automatisieren](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure rufen eine Webadresse Ihrer Wahl, wenn eine Warnung ausgelöst wird.

## <a name="see-also"></a>Siehe auch


* [Skript zum Konfigurieren von Anwendung Einblicke](app-insights-powershell-script-create-resource.md)
* [Erstellen Sie Anwendung Einblicke und Webressourcen Test aus Vorlagen](app-insights-powershell.md)
* [Automatisieren Sie Microsoft Azure Diagnostics Anwendung Erkenntnisse Kopplung](app-insights-powershell-azure-diagnostics.md)
* [Automatisieren Sie Ihre Antwort auf eine Warnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
