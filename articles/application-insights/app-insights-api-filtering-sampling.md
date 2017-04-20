<properties 
    pageTitle="Filtern und Application Insights SDK preprocessing | Microsoft Azure" 
    description="Schreiben Sie Telemetrieprozessoren und Telemetrie Initialisierer für das SDK zu filtern die Daten vor dem Senden der Telemetrie an Application Insights-Portal Eigenschaften hinzufügen." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filtern und preprocessing Telemetriedaten in Application Insights-SDK

*Anwendung Informationen ist in der Vorschau.*

Sie können schreiben und Anwendung Einblicke SDK erfasst und verarbeitet, bevor sie Application Insights-Dienst gesendet wird Telemetrie anpassen-Plug-ins konfigurieren. 

Diese Funktionen sind derzeit für ASP.NET SDK verfügbar.

* [Probenahme](app-insights-sampling.md) reduziert die Telemetrie ohne Statistiken. Hält zusammen bezogene Daten zeigt, dass Sie bei der Diagnose von Problemen navigieren können. Im Portal werden die Gesamtanzahl Ausgleich für die Probenahme multipliziert.
* [Filtern mit Telemetrieprozessoren](#filtering) können Sie auswählen oder Telemetrie im SDK ändern, bevor sie an den Server gesendet wird. Die Menge der Telemetriedaten beispielsweise konnte verringern, Anfragen Roboter ausgeschlossen. Aber Filtern ist eine grundlegende Datenverkehr als beim Sampling reduzieren. Können Sie besser steuern, was übertragen wird, aber bewusst sein, dass die Statistik -, z. B. betrifft Wenn Herausfiltern aller erfolgreichen Anfragen haben.
* Um alle Telemetriedaten aus Ihrer app Telemetriedaten aus Standardmodulen gesendet [Telemetrie Initialisierer Eigenschaften hinzufügen](#add-properties) . Beispielsweise können Sie berechnete Werte hinzufügen. oder Versionsnummern, Filtern der Daten im Portal.
* [Die SDK-API](app-insights-api-custom-events-metrics.md) zum Senden benutzerdefinierter Ereignisse und Metriken.


Bevor Sie beginnen:

* Installieren Sie [Application Insights SDK für ASP.NET v2](app-insights-asp-net.md) in Ihrer Anwendung. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Filter: ITelemetryProcessor

Diese Technik bietet direktere Kontrolle über Was ist oder Stream Telemetrie ausgeschlossen. Können Sie es zusammen mit Sampling oder getrennt.

Filtern Telemetrie telemetrieprozessor schreiben und registrieren sie mit dem SDK. Alle Telemetriedaten geht über den Prozessor, und können es aus dem Stream oder Eigenschaften hinzufügen. Dazu gehören Telemetriedaten aus den Standardmodulen der HTTP-Anforderung Collector und Abhängigkeit Collector und Telemetrie, die Sie selbst geschrieben haben. Sie können z. B. Telemetriedaten zu Anfragen von Robotern oder Aufrufe erfolgreich Abhängigkeit herausfiltern. 

> [AZURE.WARNING] Filtern aus dem SDK gesendete Telemetrie kann Prozessoren neigen die Statistiken, die im Portal angezeigt und erschweren verwandter Elemente folgen.
> 
> Stattdessen Sie verwenden Sie [Sampling](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Erstellen einer telemetrieprozessor

1. Überprüfen Sie, ob Application Insights-SDK im Projekt Version 2.0.0 oder höher. Maustaste auf das Projekt in Visual Studio Solution Explorer und NuGet-Pakete verwalten. Überprüfen Sie in NuGet-Paket-Manager Microsoft.ApplicationInsights.Web.

1. Um einen Filter zu erstellen, implementieren Sie ITelemetryProcessor. Dies ist Erweiterbarkeit Telemetrie Modul Telemetrie Initialisierung und Telemetrie Kanal. 

    Beachten Sie, dass Telemetrieprozessoren eine Kette von Verarbeitung erstellt. Beim Instanziieren telemetrieprozessor übergeben einen Link auf den nächsten Prozessor in der Kette. Wenn ein Datenpunkt Telemetrie die Process-Methode übergeben wird, führt diese Aufgaben und ruft dann den nächsten Telemetrieprozessor in der Kette.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. In ApplicationInsights.config einfügen: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Dies ist, einen Sampling-Filter initialisiert, gleichen Abschnitt.)

Öffentliche benannte Eigenschaften in der Klasse können Sie Werte aus der config-Datei übergeben. 

> [AZURE.WARNING] Der Typname und Eigenschaftennamen in der config-Datei in die Klassen- und Eigenschaftennamen in den Code kümmern. Wenn die config-Datei einer nicht vorhandenen Typ oder einer Eigenschaft verweist, möglicherweise das SDK automatisch alle Telemetriedaten senden.

 
**Sie können auch** Filter im Code zu initialisieren. Geeignete Initialisierung Klasse - beispielsweise AppStart in Global.asax.cs - Prozessors in der Kette einfügen:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients nach diesem Zeitpunkt erstellt wird Prozessoren verwenden.

### <a name="example-filters"></a>Beispiel-Filter

#### <a name="synthetic-requests"></a>Synthetische Anfragen

Filtern Sie Bots und Web. Obwohl Metriken Explorer Sie synthetische Quellen herausfiltern können, verringert diese Option Datenverkehr durch Filterung im SDK.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Authentifizierung ist fehlgeschlagen

Filtern Sie mit Antwort "401". 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Schnelle remote Abhängigkeit Anrufe filtern

Möchten Sie nur Aufrufe diagnostizieren, die langsam, die Fast gefiltert. 

> [AZURE.NOTE] Dadurch wird die Statistik neigen auf das Portal. Abhängigkeit Diagramm sehen, als ob die Abhängigkeit alle Fehler werden.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Abhängigkeitsprobleme diagnostizieren

[Diesen Blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) beschreibt ein Projekt zur Problemdiagnose per regulären Pings automatisch Abhängigkeiten Abhängigkeit.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Hinzufügen von Eigenschaften: ITelemetryInitializer

Verwenden Sie Telemetrie Initialisierer globale Eigenschaften definieren, die mit allen Telemetrie gesendet werden. und ausgewählte Verhalten der standardmäßigen Telemetrie Module. 

Beispielsweise sammelt Anwendung Einblicke Webpakets Telemetriedaten zu HTTP-Anfragen. Standardmäßig kennzeichnet jede Anforderung einen Antwortcode fehlgeschlagen > = 400. Aber wenn 400 erfolgreich behandelt werden soll, können Sie eine Telemetrie-Initialisierung, die Erfolg-Eigenschaft festgelegt.

Wenn Sie Telemetrie Initialisierer bereitstellen, heißt es bei jeder Änderung der Track*()-Methoden aufgerufen. Dazu gehören Methoden, durch die standardmäßige Telemetrie-Module. Gemäß der Konvention stellen diese Module keine Eigenschaft, die bereits durch eine Initialisierung festgelegt wurde. 

**Definieren der Initialisierung**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Laden der Initialisierung**

In ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Auch* die Initialisierung im Code beispielsweise in Global.aspx.cs instanziiert werden können:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Weitere dieses Beispiels.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>JavaScript Telemetrie Initialisierer

*JavaScript*

Fügen Sie unmittelbar nach der Initialisierungscode über das Portal wurde Telemetrie-Initialisierung: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Eine nicht benutzerdefinierte Eigenschaften auf der TelemetryItem finden Sie das [Datenmodell](app-insights-export-data-model.md#lttelemetrytypegt).

Sie können beliebig viele Initialisierer beliebig hinzufügen. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor und ITelemetryInitializer

Was ist der Unterschied zwischen telemetrieprozessoren und Telemetrie Initialisierer?

* Es gibt einige überlappt, was Sie damit tun können: beide wird Telemetrie Eigenschaften hinzufügen.
* TelemetryInitializers werden immer vor dem TelemetryProcessors ausgeführt.
* TelemetryProcessors können vollständig ersetzen oder Löschen eines Elements Telemetrie.
* TelemetryProcessors Performance Counter Telemetrie nicht verarbeitet werden.



## <a name="persistence-channel"></a>Dauerhaftigkeit Kanal 

Ihre Anwendung ausgeführt wird, die Internet-Verbindung nicht immer verfügbar oder langsam ist, sollten Sie Dauerhaftigkeit Kanal statt im Arbeitsspeicher Standardkanal verwenden. 

Im Arbeitsspeicher Standardkanal verliert alle Telemetrie app schließt nicht die Zeit gesendet wurde. Zwar können Sie `Flush()` um alle Daten im Puffer bleibt, es noch Datenverlust besteht keine Internet-Verbindung oder die Anwendung vor dem Herunterfahren Übertragung abgeschlossen ist.

Im Gegensatz dazu puffert Dauerhaftigkeit Channel Telemetrie in einer Datei vor dem Senden an das Portal. `Flush()`sichergestellt, dass Daten in der Datei gespeichert werden. Wenn Daten nicht gleichzeitig die Anwendung geschlossen wird übermittelt, bleibt es in der Datei. Beim Neustart der Anwendung werden Daten dann gesendet, wenn eine Internetverbindung vorhanden ist. Wenn sich in der Datei so lange, bis eine Verbindung verfügbar ist. 

### <a name="to-use-the-persistence-channel"></a>Den Channel Dauerhaftigkeit verwendet

1. Importieren Sie das NuGet-Paket [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3).
2. Vorwahl in Ihrer Anwendung eine geeignete Initialisierung an:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Mit `telemetryClient.Flush()` vor der Anwendung schließt, um sicherzustellen, dass Daten an das Portal gesendet oder in der Datei gespeichert.

    Beachten Sie, dass Flush() synchron für Dauerhaftigkeit Channel, aber für andere asynchrone.

 
Persistenzkanal ist für Geräte Szenarien optimiert, die Anzahl der Ereignisse, die von der Anwendung erzeugte relativ klein und die Verbindung häufig unzuverlässig. Dieser Kanal Ereignisse auf die Festplatte schreiben in zuverlässiger Speicher zuerst und versuchen, es zu senden. 

#### <a name="example"></a>Beispiel

Angenommen, Sie nicht behandelte Ausnahmen überwachen möchten. Abonnieren Sie die `UnhandledException` Ereignis. Der Rückruf können Sie einen Aufruf von Flush sicherstellen, dass die Telemetrie beibehalten wird.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Wenn die Anwendung heruntergefahren wird, sehen Sie eine Datei in `%LocalAppData%\Microsoft\ApplicationInsights\`, komprimierte Ereignisse enthält. 
 
Nächsten Start dieser Anwendung der Kanal Datei holen und Telemetrie Anwendung Einblicke liefern, wenn möglich.

#### <a name="test-example"></a>Beispiel für einen Test

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Der persistenzkanal ist auf [Github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Referenz-Dokumente

* [Übersicht über die API](app-insights-api-custom-events-metrics.md)

* [ASP.NET Verweis](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>SDK-Code

* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript-SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Nächste Schritte


* [Suchen von Ereignissen und Protokollen][diagnostic]
* [Probenahme](app-insights-sampling.md)
* [Problembehandlung][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
