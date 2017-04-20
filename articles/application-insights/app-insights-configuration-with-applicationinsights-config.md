<properties 
    pageTitle="Konfigurieren von Anwendung Einblicke SDK mit ApplicationInsights.config oder XML" 
    description="Aktivieren oder Deaktivieren von Data Collection Module und Leistungsindikatoren und andere Parameter hinzufügen" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Konfigurieren von Anwendung Einblicke SDK mit ApplicationInsights.config oder XML

Anwendung Einblicke .NET SDK besteht aus einer Reihe von NuGet-Paketen. Das [Paket](http://www.nuget.org/packages/Microsoft.ApplicationInsights) enthält die API Telemetrie an Application Insights senden. [Zusätzliche Pakete](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) bieten Telemetrie _Module_ und _Initialisierer_ für Telemetrie automatisch von der Anwendung und der Kontext verfolgen. Mithilfe der Konfigurationsdatei können Sie aktivieren oder deaktivieren Telemetrie Module und Initialisierer und für einige Parameter festlegen.

Die Konfigurationsdatei mit dem Namen `ApplicationInsights.config` oder `ApplicationInsights.xml`, abhängig vom Typ der Anwendung. Automatisch dem Projekt hinzugefügt bei [den meisten Versionen des SDK installieren][start]. Es ist zusätzlich eine Web app [Status]Monitor auf einem IIS-Server[redfield], oder wenn Sie Appplication Einblicke [Erweiterung für eine Azure-Website oder VM](app-insights-azure-web-apps.md)auswählen.

Es gibt keine entsprechende Datei [SDK auf einer Webseite]steuern[client].

Dieses Dokument beschreibt die Abschnitte, die Sie in der Konfiguration finden Sie unter, steuern sie die Komponenten des SDK und die NuGet-Pakete Laden dieser Komponenten.

## <a name="telemetry-modules-aspnet"></a>Telemetrie Module (ASP.NET)

Jedes Telemetrie einen bestimmten Typ von Daten sammelt und die Kern-API zum Senden der Daten verwendet. Die Module werden verschiedene NuGet-Pakete installiert die config-Datei auch die erforderlichen Zeilen hinzugefügt.

Es ist ein Knoten in der Konfigurationsdatei für jedes Modul. Um ein Modul zu deaktivieren, den Knoten löschen oder auskommentieren.



### <a name="dependency-tracking"></a>Abhängigkeit verfolgen

[Abhängigkeit tracking](app-insights-dependencies.md) sammelt Telemetrie zum Aufrufen Ihrer app Datenbanken und externe Dienste und Datenbanken durch. Um dieses Modul in einem IIS-Server zu ermöglichen, müssen Sie [Installieren Statusmonitor][redfield]. In Azure webapps oder VMs, [Wählen Sie die Anwendung Einblicke Erweiterung](app-insights-azure-web-apps.md)verwenden.

Sie können auch Abhängigkeit verfolgen mithilfe der [TrackDependency-API](app-insights-api-custom-events-metrics.md#track-dependency)Code schreiben.


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* NuGet-Paket [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) .

### <a name="performance-collector"></a>Performance-collector

[Systemleistungsindikatoren erfasst](app-insights-performance-counters.md) , wie CPU, Speicher und Laden von IIS-Installationen. Sie können Leistungsindikatoren zu sammeln, einschließlich Leistungsindikatoren selbst einrichten.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* NuGet-Paket [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) .


### <a name="application-insights-diagnostics-telemetry"></a>Anwendung Einblicke Diagnose Telemetrie

Das `DiagnosticsTelemetryModule` meldet Fehler in der Anwendung Einblicke Instrumentationscode selbst. Z. B. wenn der Code Leistungsindikatoren nicht zugreifen kann oder wenn ein `ITelemetryInitializer` löst eine Ausnahme aus. Trace Telemetrie verfolgt in diesem Modul wird in der [Diagnose suchen][diagnostic]. Sendet Daten an dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* NuGet-Paket [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) . Wenn Sie nur das Paket installieren, wird die Datei ApplicationInsights.config nicht automatisch erstellt. 

### <a name="developer-mode"></a>Entwicklermodus

`DeveloperModeWithDebuggerAttachedTelemetryModule`Erzwingt die Anwendung Erkenntnisse `TelemetryChannel` zum Senden von Daten sofort eine telemetrieelement, wenn ein Debugger an den Anwendungsprozess. Dies reduziert die Zeitspanne zwischen dem Zeitpunkt die Anwendung Telemetrie verfolgt und auf Application Insights-Portal. Es verursacht erheblichen Mehraufwand in CPU und Netzwerkbandbreite.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Anwendung Einblicke WindowsServer](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-Paket

### <a name="web-request-tracking"></a>Webanfrage

Meldet den [Antwortcode und Ergebnis](app-insights-asp-net.md) der HTTP-Anfragen. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* NuGet-Paket [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)

### <a name="exception-tracking"></a>Tracking-Ausnahme

`ExceptionTrackingTelemetryModule`Spuren nicht behandelte Ausnahmen in Ihrer Anwendung. [Fehler und Ausnahmen]Siehe[exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* NuGet-Paket [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`- [Ausnahmen nicht überwachten Vorgang](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx)verfolgt.
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-nicht behandelte Ausnahmen für Workerthreads, Windows-Dienste und Fenster verfolgt.
* [Anwendung Einblicke WindowsServer](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-Paket.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Microsoft.ApplicationInsights-Paket enthält die [Kern-API](https://msdn.microsoft.com/library/mt420197.aspx) des SDK. Telemetrie Module verwenden Sie diese, und Sie können auch [verwenden, um eigene Telemetrie definieren](app-insights-api-custom-events-metrics.md).

* Kein Eintrag in ApplicationInsights.config.
* NuGet-Paket [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) . Wenn nur diese NuGet Installation wird keine Konfigurationsdatei generiert.

## <a name="telemetry-channel"></a>Telemetrie Kanal

Pufferung und Übertragung von Telemetrie Dienst Anwendung Einblicke verwaltet Telemetrie Channel. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`ist der Standardkanal für Dienste. Daten werden im Arbeitsspeicher puffert.
* `Microsoft.ApplicationInsights.PersistenceChannel`ist eine Alternative für Fenster. Sie sparen unflushed Daten dauerhaft Ihre app schließt und sendet es, wenn die Anwendung erneut gestartet wird.


## <a name="telemetry-initializers-aspnet"></a>Telemetrie Initialisierer (ASP.NET)

Telemetrie Initialisierer Kontexteigenschaften festlegen, die mit jedem Element der Telemetrie gesendet werden. 

Sie können Rahmen Eigenschaften [Schreiben eigener Initialisierer](app-insights-api-filtering-sampling.md#add-properties) .

Die standardmäßige Initialisierer werden per Web oder WindowsServer NuGet-Pakete festgelegt:


* `AccountIdTelemetryInitializer`die AccountId-Eigenschaft festgelegt.
* `AuthenticatedUserIdTelemetryInitializer`Legt die AuthenticatedUserId-Eigenschaft festgelegten JavaScript-SDK.
* `AzureRoleEnvironmentTelemetryInitializer`Updates der `RoleName` und `RoleInstance` Eigenschaften der `Device` Kontext für alle Telemetrie Artikel mit Informationen von Azure Runtime-Umgebung.
* `BuildInfoConfigComponentVersionTelemetryInitializer`Updates der `Version` -Eigenschaft des der `Component` Kontext für alle telemetrieelemente aus extrahierten Wert der `BuildInfo.config` Datei von MS-Build erstellt.
* `ClientIpHeaderTelemetryInitializer`Updates `Ip` Eigenschaft den `Location` Kontext aller Telemetrie Elemente basierend auf der `X-Forwarded-For` -HTTP-Header der Anforderung.
* `DeviceTelemetryInitializer`aktualisiert die folgenden Eigenschaften von der `Device` Kontext für alle Telemetrie.
 - `Type`wird festgelegt auf "PC"
 - `Id`soll der Domänenname des Computers, auf dem die Anwendung ausgeführt.
 - `OemName`soll der Wert extrahiert aus den `Win32_ComputerSystem.Manufacturer` Feld mit WMI.
 - `Model`soll der Wert extrahiert aus den `Win32_ComputerSystem.Model` Feld mit WMI.
 - `NetworkType`soll der Wert extrahiert aus der `NetworkInterface`.
 - `Language`soll der Name der `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`Updates der `RoleInstance` -Eigenschaft eines der `Device` Kontext für alle telemetrieelemente mit dem Domänennamen des Computers, auf dem die Anwendung ausgeführt.
* `OperationNameTelemetryInitializer`Updates der `Name` -Eigenschaft des der `RequestTelemetry` und `Name` Eigenschaft den `Operation` Kontext aller Telemetrie Elemente basierend auf die HTTP-Methode als auch Namen von ASP.NET MVC-Controller und Aktion aufgerufen, um die Anforderung zu verarbeiten.
* `OperationIdTelemetryInitializer`oder `OperationCorrelationTelemetryInitializer` Updates der `Operation.Id` Kontexteigenschaft aller Telemetrie Elemente verfolgt während der Verarbeitung einer Anforderung mit automatisch generierten `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`Updates der `Id` -Eigenschaft eines der `Session` Kontext für alle telemetrieelemente aus der `ai_session` Cookies im Browser des Benutzers ApplicationInsights JavaScript-Instrumentationscode generiert. 
* `SyntheticTelemetryInitializer`oder `SyntheticUserAgentTelemetryInitializer` Updates der `User`, `Session` und `Operation` Kontexte Eigenschaften aller Telemetrie Elemente nachverfolgt, wenn eine Anforderung von einer synthetischen Quelle behandeln wie eine Verfügbarkeit testen oder suchen Engine Bot. Standardmäßig zeigt [Metrik-Explorer](app-insights-metrics-explorer.md) keine synthetischen Telemetrie. 

    Die `<Filters>` legen Eigenschaften Anfragen identifizieren.
* `UserAgentTelemetryInitializer`Updates der `UserAgent` -Eigenschaft eines der `User` Kontext aller Telemetrie Elemente basierend auf der `User-Agent` -HTTP-Header der Anforderung.
* `UserTelemetryInitializer`Updates der `Id` und `AcquisitionDate` Eigenschaften `User` Kontext für Telemetrie alles mit Werten aus extrahiert die `ai_user` Cookies im Browser des Benutzers ausgeführten Anwendung Einblicke JavaScript-Instrumentation-Code generiert.
* `WebTestTelemetryInitializer`Benutzer-Id, Sitzungsbezeichner und synthetische Quelleigenschaften für HTTP-Anfragen, die aus [Verfügbarkeitstests](app-insights-monitor-web-app-availability.md)festgelegt.
Die `<Filters>` legen Eigenschaften Anfragen identifizieren.

## <a name="telemetry-processors-aspnet"></a>Telemetrieprozessoren (ASP.NET)

Telemetrieprozessoren können gefiltert und jedes telemetrieelement vor dem Senden des SDK zum Portal ändern.

Sie können [telemetrieprozessoren schreiben](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Adaptive Sampling telemetrieprozessor (aus 2.0.0-beta3)

Dies ist standardmäßig aktiviert. Sendet Ihre app viel Telemetrie, entfernt dieser Prozessor davon.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Der Parameter stellt das Ziel zu erreichen versucht. Jede Instanz des SDK arbeitet, ist der Server ein Cluster von mehreren Computern wird die tatsächliche Menge der Telemetriedaten entsprechend multipliziert.

[Weitere Informationen zu Sampling](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Sampling-Rate telemetrieprozessor (aus 2.0.0-beta1)

Es gibt auch standard [Sampling telemetrieprozessor](app-insights-api-filtering-sampling.md#sampling) (von 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Die Kanalparameter (Java)

Diese Parameter beeinflussen wie Java SDK speichern und leeren die gesammelten Telemetriedaten.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Die Anzahl der telemetrieelemente, die in das SDK in-Speicher gespeichert werden können. Wenn diese Zahl erreicht ist, der Telemetrie Puffer geleert - d. h. telemetrieelemente an Application Insights-Server gesendet.

-   Min: 1
-   Max: 1000
-   Standard: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Bestimmt, wie oft die Daten in den Speicher gespeichert werden (an Application Insights) entleert werden soll.

-   Min: 1
-   Max: 300
-   Standard: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Bestimmt die maximale Größe in MB, die dauerhaft auf dem lokalen Datenträger zugewiesen wird. Dieser Speicher wird für persistente telemetrieelemente verwendet, die nicht der Anwendung Einblicke Endpunkt übermittelt. Traf die Speichergröße werden neue telemetrieelemente gelöscht.

-   Min: 1
-   Max: 100
-   Standard: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

Application Insights-Ressource, die die Daten bestimmt. Normalerweise erstellen Sie für jede Anwendung eine separate Ressource mit einem separaten Schlüssel.

Soll den Schlüssel dynamisch - Beispiel soll die Ergebnisse aus der Anwendung an unterschiedliche Ressourcen - können Sie weglassen den Schlüssel in der Konfigurationsdatei und stattdessen im Code stellen.

Zum Festlegen des Schlüssels für alle Instanzen des TelemetryClient legen einschließlich standard Telemetrie Module Sie Schlüssel in TelemetryConfiguration.Active fest. Hierzu eine Initialisierungsmethode, wie global.aspx.cs in einem ASP.NET:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Wenn Sie eine bestimmte Gruppe von Ereignissen an eine andere Ressource senden möchten, können Sie den Schlüssel für einen bestimmten TelemetryClient festlegen:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Erfahren Sie mehr über die API][api].

Ein neuer Schlüssel, [Erstellen Sie eine neue Ressource in Application Insights-Portal]zu[new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

