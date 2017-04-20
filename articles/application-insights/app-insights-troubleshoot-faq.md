<properties 
    pageTitle="Fehlerbehebung und Fragen zur Anwendung Einblicke" 
    description="In Visual Studio Application Insights unklar oder nicht? Versuchen Sie es hier." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Fragen - Anwendung Einblicke für ASP.NET

## <a name="configuration-problems"></a>Konfigurationsprobleme

*Ich habe Probleme beim Einrichten von meinem:*

* [.NET app](app-insights-asp-net-troubleshoot-no-data.md)
* [Überwachen einer bereits ausgeführten Anwendung](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure-Diagnose](app-insights-azure-diagnostics.md)
* [Java WebApp](app-insights-java-troubleshoot.md)
* [Andere Plattformen](app-insights-platforms.md)

*Ich erhalte keine Daten vom server*

* [Firewallausnahmen festlegen](app-insights-ip-addresses.md)
* [Richten Sie einen Server mit ASP.NET](app-insights-monitor-performance-live-website-now.md)
* [Java-Server einrichten](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Verwende Einblicke Anwendung ich...?

[Siehe Plattformen][platforms]


## <a name="is-it-free"></a>Ist es?

* Ja, wenn die freie [Tarif](app-insights-pricing.md)wählen. Sie erhalten die meisten Features und großzügige Kontingent von Daten. 
* Müssen Sie Ihre Daten mit Microsoft Azure registriert, aber keine Gebühren erfolgt, wenn Sie anderen bezahlten Azure Service oder explizit auf Zahlen aktualisieren.
* Sendet Ihre Anwendung mehr Daten als das monatliche Kontingent freier Tier, beendet es protokolliert. In diesem Fall können Sie bezahlen möchten oder warten, bis das Kontingent am Ende des Monats zurückgesetzt wird.
* Einfache Verwendung und Sitzung Daten unterliegt ein Kontingent.
* Außerdem ist eine kostenlose 30-tägige Testversion der bezahlten Funktionen kostenlos erhalten.
* Jede Anwendungsressource verfügt über ein separates Kontingent und der Tarif unabhängig von jeder anderen festlegen.

#### <a name="what-do-i-get-if-i-pay"></a>Was erhalte ich, wenn ich bezahlen?

* Ein größeres [monatliche Kontingent von Daten](https://azure.microsoft.com/pricing/details/application-insights/).
* Option veralteten weiterhin Sammeln von Daten über das monatliche Kontingent bezahlen. Wenn Ihre Daten Kontingent überschreitet, werden Sie pro Mb berechnet.
* [Fortlaufend zu exportieren](app-insights-export-telemetry.md).


## <a name="q14"></a>Was ändert Anwendung Einblicke in meinem Projekt?

Die Details hängen vom Projekttyp ab. Für eine Webanwendung:


+ Diese Dateien hinzugefügt dem Projekt:

 + ApplicationInsights.config. 
 + AI.js


+ Diese NuGet-Pakete installiert:

 -  *Application Insights API* - Kern-API

 -  *Application Insights API for Web Applications* - mit der Telemetrie vom Server übertragen

 -  *Application Insights API für JavaScript* - zum Telemetrie vom Client senden

    Die Pakete enthalten diese Assemblys:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Fügt Elemente:

 - Web.config

 - Packages.config

+ (Neue Projekte – Wenn [Anwendung Einblicke in ein vorhandenes Projekt hinzufügen][start], müssen Sie dies manuell tun.) Fügt Ausschnitte in Client- und Servercode Initialisierung mit Application Insights Ressourcen-ID. Beispielsweise ist in einer MVC-Anwendung Code in die Masterseite Views/Shared/_Layout.cshtml eingefügt


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Wie aktualisiere ich ältere SDK?

Das SDK, der Typ der Anwendung finden Sie unter [Versionsinformationen](app-insights-release-notes.md) . 



## <a name="update"></a>Wie kann ich ändern, welche Azure Ressource Projekt Daten sendet?

Im Projektmappen-Explorer mit der rechten Maustaste `ApplicationInsights.config` , und wählen Sie **Update Application Insights**. Sie können die Daten einer vorhandenen oder neuen Ressource in Azure senden. Der Assistent ändert den instrumentationsschlüssel in ApplicationInsights.config bestimmt, wo der Server SDK Daten sendet. Wenn Sie "Alle aktualisieren" deaktivieren, wird auch den Schlüssel geändert, auf Ihren Webseiten angezeigt.


#### <a name="data"></a>Wie lange werden Daten im Portal beibehalten? Ist sie sicher?

[Datenschutz und Daten]betrachten[data].

## <a name="logging"></a>Protokollierung

#### <a name="post"></a>Sehe Postbackdaten Diagnose Suche

Wir nicht POST-Daten automatisch anmelden, aber können Sie einen TrackTrace-Aufruf: die Daten der Message-Parameter gestellt. Maximal Größe länger als die Grenzwerte für die Eigenschaften, hat Wenn Sie auf Filtern können 

## <a name="security"></a>Sicherheit

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Sind meine Daten sicher im Portal? Wie lange bleibt?

Siehe [Data Retention und Datenschutz][data].


## <a name="q17"></a>Aktiviert ich alles in Application Insights haben?

<table border="1">
<tr><th>Vorgehensweise</th><th>Wie man es</th><th>Warum soll</th></tr>
<tr><td>Verfügbarkeit-Diagramme</td><td><a href="../app-insights-monitor-web-app-availability/">Webtests</a></td><td>Ihrer Anwendung ist weiß</td></tr>
<tr><td>Server app Perf: Antwortzeiten,...
</td><td><a href="../app-insights-asp-net/">Anwendung Erkenntnisse zu Ihrem Projekt hinzufügen</a><br/>oder <br/><a href="../app-insights-monitor-performance-live-website-now/">AI-Statusmonitor auf Server installieren</a> (oder Ihren eigenen Code schreiben, um <a href="../app-insights-api-custom-events-metrics/#track-dependency">Abhängigkeiten zu verfolgen</a>)</td><td>Perf Probleme erkennen</td></tr>
<tr><td>Abhängigkeit Telemetrie</td><td><a href="../app-insights-monitor-performance-live-website-now/">AI-Statusmonitor auf Server installieren</a></td><td>Diagnostizieren von Problemen mit Datenbanken oder andere externen Komponenten</td></tr>
<tr><td>Stack-Traces Ausnahmen erhalten</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">TrackException Aufrufe in den Code einfügen</a> (aber einige automatisch angegeben)</td><td>Erkennung und diagnose von Ausnahmen</td></tr>
<tr><td>Suche protokollablaufverfolgungen</td><td><a href="../app-insights-search-diagnostic-logs/">Einen Protokollierung Adapter hinzufügen</a></td><td>Ausnahmen Perf Probleme diagnostizieren</td></tr>
<tr><td>Client Verwendung Grundlagen: Seitenaufrufe, Sessions...</td><td><a href="../app-insights-javascript/">JavaScript-Initialisierung in Webseiten</a></td><td>Verwendungsanalyse</td></tr>
<tr><td>Client benutzerdefinierte Messgrößen</td><td><a href="../app-insights-api-custom-events-metrics/">Überwachung wird auf Webseiten</a></td><td>Verbessern der Benutzerfunktionalität</td></tr>
<tr><td>Server benutzerdefinierte Messgrößen</td><td><a href="../app-insights-api-custom-events-metrics/">Tracking-Aufrufe im Servercode</a></td><td>Business intelligence</td></tr>
</table>


## <a name="automation"></a>Automatisierung

Sie können [PowerShell Skripts](app-insights-powershell.md) zum Erstellen und Aktualisieren von Application Insights-Ressourcen.

## <a name="more-answers"></a>Weitere Antworten

* [Anwendung Einblicke forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 