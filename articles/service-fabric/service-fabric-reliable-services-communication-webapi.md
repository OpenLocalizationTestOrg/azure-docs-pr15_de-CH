<properties
   pageTitle="Kommunikation mit ASP.NET Web API-Service | Microsoft Azure"
   description="Erfahren Sie schrittweise Kommunikation mit ASP.NET Web API owin-Self-hosting in zuverlässige Dienste-API implementiert."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Erste Schritte: Service Fabric Web API-Services mit owin-Self-hosting

Azure Service Fabric legt die macht in der Hand, wenn Sie entscheiden, wie Ihre Dienste mit Benutzern und miteinander kommunizieren sollen. In diesem Lernprogramm konzentriert sich auf Service Kommunikation mit ASP.NET Web API öffnen Weboberfläche owin .NET (-) Self-hosting Service Fabric zuverlässige Dienste-API. Wir werden zuverlässige Dienste austauschbare Kommunikation API-vertiefen. Wir auch verwenden Web-API in ein schrittweises Beispiel, wie Sie einen benutzerdefinierten kommunikationslistener einrichten.


## <a name="introduction-to-web-api-in-service-fabric"></a>Einführung in Web-API in Service Fabric

ASP.NET Web API ist ein bekannter und leistungsstarker Framework zum Erstellen von HTTP-APIs auf.NET Framework. Wenn Sie nicht bereits mit dem Framework vertraut sind, finden Sie unter [Erste Schritte mit ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) mehr.

Web-API in Service Fabric ist derselben ASP.NET Web API kennen und lieben. Der Unterschied besteht darin, wie *Host* Web API-Anwendung. Sie wird nicht Microsoft-Internetinformationsdienste (IIS) verwenden. Zum besseren Verständnis den Unterschied wir in zwei Teile aufteilen:

 1. Web API-Anwendung (einschließlich Controller und Modelle)
 2. Host (der Webserver in der Regel IIS)

Eine Web-API-Anwendung selbst nicht geändert. Es unterscheidet sich nicht vom Web API-Programme, die Sie bisher geschrieben haben, und Sie sollen für den Großteil des Anwendungscodes bewegen. Aber wenn in IIS gehostet wurden haben, Host der Anwendung möglicherweise anders aus, gewöhnt. Bevor wir Host zu erhalten, beginnen wir mit etwas mehr vertraut: Web-API-Anwendung.


## <a name="create-the-application"></a>Erstellen der Anwendung

Erstellen einer neuen Service Fabric-Anwendung mit einem einzelnen statusfreie Dienst Visual Studio 2015 starten:

![Erstellen einer neuen Service Fabric-Anwendung](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Eine Visual Studio-Vorlage Web API statusfreie warten ist verfügbar. In diesem Lernprogramm erstellen wir ein Web API-Projekt neu, die Ergebnisse in was Sie bekommen, wenn Sie diese Vorlage ausgewählt.

Wählen Sie ein leeres Projekt statusfreie Service erfahren, wie ein Web API Projekt neu erstellen oder die statusfreie Service Web API Sicherheitsvorlage und einfach folgen.  

![Erstellen Sie einen einzelnen statusfreien service](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

Der erste Schritt besteht darin in NuGet-Pakete für Web-API. Das Paket zu verwenden ist Microsoft.AspNet.WebApi.OwinSelfHost. Dieses Paket enthält alle notwendigen Web API-Pakete und *Host* -Pakete. Dies wird später wichtig sein.

![Mithilfe der NuGet Paket-Manager Web-API](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Nachdem die Pakete installiert haben, können Sie die grundlegenden Web API Projektstruktur erstellen beginnen. Wenn Sie Web-API verwendet haben, sollten die Projektstruktur vertraut vorkommen. Starten Sie durch Hinzufügen einer `Controllers` Verzeichnis und einem Controller Werten:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Fügen Sie eine Startklasse Projektstamm Arbeitsplan registrieren, Formatierungsprogramme und andere Konfiguration. Dies ist auch dem Web API *Host*wird, erneut überprüft werden. 

**Startup.cs**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

Das ist Teil der Anwendung. Jetzt haben wir nur die grundlegenden Web API Projektlayout einrichten. Bisher sollten nicht es anders viel Web API-Projekte, die Sie bisher geschrieben haben oder die einfache Web-API-Vorlage. Geschäftslogik geht die Controller und Modelle wie gewohnt.

Was tun wir zu hosten, damit wir tatsächlich ausführen können?

## <a name="service-hosting"></a>Hosting Service

Fabric Service wird der Dienst in einem *Diensthostprozess*, eine ausführbare Datei, die Ihr Service-Code ausgeführt wird. Beim Schreiben eines Dienstes mit zuverlässigen Services-API kompiliert Dienstprojekt nur auf eine ausführbare Datei, die der Dienst registriert und führt den Code. Dies gilt in den meisten Fällen beim Dienst auf Service in .NET schreiben. Beim Öffnen "Program.cs" im Projekt statusfreie Service erhalten Sie:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Die den Einstiegspunkt verdächtig, eine aussieht, ist das ist.

Weitere Informationen zum Diensthostprozess und Registrierung sind nicht Gegenstand dieses Artikels. Es ist jedoch wichtig jetzt wissen, dass *Ihr Service-Code in einem eigenen Prozess ausgeführt wird*.

## <a name="self-host-web-api-with-an-owin-host"></a>Self-Hosting Web API mit einem owin-host

Da Web API Anwendungscode in einem eigenen Prozess gehostet wird, kann wie es Web Server hook? Geben Sie [OWIN](http://owin.org/)ein. OWIN ist einfach ein Vertrag zwischen .NET Web Applications und Webservern. Traditionell wird ASP.NET (bis zu 5 MVC) wird die Anwendung mit IIS über System.Web eng. Web-API implementiert jedoch OWIN, eine Anwendung, die entkoppelt vom Webserver schreiben, die es hostet. Dadurch können Sie einen Webserver owin- *selbst* , den Sie in einem eigenen Prozess starten können. Dies passt perfekt hosting Service Fabric-Modell nur beschrieben.

In diesem Artikel verwenden wir Katana für Web-API-Anwendung als owin-Host. Katana ist eine Open Source-owin-Host-Implementierung basierend auf [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) und Windows [HTTP-Server-API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [AZURE.NOTE] Erfahren Sie mehr über Katana zum Wechseln der [Katana-Website](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) Für einen schnellen Überblick über die Verwendung von Katana, Self-Hosting Web-API finden Sie unter [Verwenden owin-Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).


## <a name="set-up-the-web-server"></a>Der Webserver einrichten

Zuverlässige Services-API bietet eine Kommunikation Kommunikationsstapel eingesteckt können, mit denen Benutzer und Clients mit dem Dienst herstellen können:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Webserver (und andere Kommunikationsstapel zukünftig wie WebSockets verwenden) verwenden die ICommunicationListener Schnittstelle ordnungsgemäß mit dem System integrieren. Dafür werden in den folgenden Schritten deutlich.

Erstellen Sie zuerst eine Klasse namens OwinCommunicationListener, die ICommunicationListener implementiert:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

ICommunicationListener-Schnittstelle stellt drei Methoden zum Verwalten der kommunikationslistener für den Dienst:

 - *OpenAsync*. Starten Sie Anfragen abhören.
 - *CloseAsync*. Anfragen reagiert, Fertig stellen Anfragen an Bord und ordnungsgemäß heruntergefahren.
 - *Abbrechen*. Alles abbrechen und sofort zu beenden.

Zunächst fügen Sie private Klassenmember Dinge müssen der Listener-Funktion. Diese werden durch den Konstruktor initialisiert und später beim Einrichten der Listener URL verwendet.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>Implementieren von OpenAsync

Um den Webserver einzurichten, benötigen Sie zwei Informationen:

 - *Ein URL-Pfad-Präfix*. Dies ist, zwar optional empfiehlt es sich für Sie dies jetzt so eingerichtet, dass Sie problemlos mehrere Webdienste in Ihrer Anwendung hosten können.
 - *Einen Port*.

Bevor Sie einen Anschluss für den Webserver erhalten, ist wichtig, dass Sie wissen, dass Service Fabric stellt als Puffer zwischen der Anwendung und dem zugrunde liegenden Betriebssystem läuft fungiert auf einer Anwendungsebene bereit. So ermöglicht Service Fabric *Endpunkte* für die Dienste konfigurieren. Fabric Service wird sichergestellt, dass Endpunkte für den Dienst verwenden. Auf diese Weise müssen Sie sie selbst im zugrunde liegenden Betriebssystem konfigurieren. Sie können problemlos Service Fabric-Anwendung in einer anderen Umgebung hosten, ohne die Anwendung ändern. (Beispielsweise können Sie dieselbe Anwendung in Azure oder in Ihrem eigenen Rechenzentrum hosten.)

Konfigurieren Sie einen HTTP-Endpunkt in PackageRoot\ServiceManifest.xml:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Dieser Schritt ist wichtig, da der Dienst Host eingeschränkte Anmeldeinformationen (Netzwerkdienst unter Windows) ausgeführt wird. Dies bedeutet, dass der Dienst Zugriff auf einen HTTP-Endpunkt auf eigene einrichten lassen. Mithilfe der Endpunktkonfiguration weiß Fabric Service Einrichten der entsprechenden Zugriffssteuerungsliste (ACL) für die URL, die der Dienst abgehört wird. Service Fabric bietet auch ein Standardelement Endpunkte konfigurieren.


In OwinCommunicationListener.cs beginnen Sie OpenAsync implementieren. Dadurch wird den Webserver. Zunächst Informationen Sie Endpunkt und erstellen Sie den URL der Dienst abgehört wird. Die URL werden je nach, ob der Listener eine statusfreie oder eine statusbehaftete Dienst verwendet wird. Der Listener muss für statusbehaftete Dienst eine eindeutige Adresse für jedes Replikat statusbehaftete Dienst erstellen, den sie überwacht. Zustandsloser Dienste kann die Adresse einfacher sein. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Beachten Sie, dass hier "http://+" verwendet wird. Dies ist sicherzustellen, dass der Webserver alle verfügbaren Adressen einschließlich Localhost, vollqualifizierten Domänennamen und IP-Computer überwacht.

Die OpenAsync-Implementierung ist einer der wichtigsten Gründe, warum der Webserver (oder alle Kommunikationsstapel) erfolgt eine ICommunicationListener, anstatt einfach direkt öffnen `RunAsync()` im Dienst. Der Rückgabewert OpenAsync ist die Adresse, der der Webserver überwacht. Wenn diese Adresse an das System zurückgegeben wird, registriert die Adresse mit dem Dienst. Fabric Service bietet eine API, die Clients und andere Dienste für diese Adresse nach Dienstnamen bitten kann. Dies ist wichtig, weil die Adresse nicht statisch ist. Dienste werden im Cluster für Lastenausgleich und Verfügbarkeit Ressource verschoben. Dies ist der Mechanismus, der Kunden die Überwachungsadresse für einen Dienst zu beheben.

Insofern OpenAsync startet den Webserver und gibt die Adresse, der abgehört wird. Beachten Sie, dass es hört auf "http://+", aber bevor OpenAsync die Adresse gibt der "+" mit IP oder FQDN des Knotens ist derzeit auf ersetzt. Die von dieser Methode zurückgegebene Adresse ist wie für das System registriert ist. Es ist auch Clients und anderen Diensten sehen fragt für die Serviceadresse. Für Clients korrekt herstellen benötigen sie eine tatsächliche IP oder FQDN in der Adresse.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Beachten Sie, dass dies die Startklasse verweist, die an die OwinCommunicationListener im Konstruktor übergeben wurde. Dieser Startinstanz wird von dem Webserver zum Web API-Anwendung zu starten.

Die `ServiceEventSource.Current.Message()` Zeile erscheint im Fenster Ereignisse später beim Ausführen der Anwendung zu bestätigen, dass der Webserver erfolgreich gestartet wurde.

## <a name="implement-closeasync-and-abort"></a>CloseAsync implementieren und Abbrechen

Abschließend implementieren Sie CloseAsync und Abbrechen um den Webserver zu beenden. Der Webserver kann durch Server-Handle, das bei OpenAsync erstellte disposing angehalten werden.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

In diesem Implementierungsbeispiel CloseAsync und Abbruch einfach Webserver anhalten. Sie können entscheiden, eine besser koordinierte Herunterfahren des Webservers in CloseAsync. Beispielsweise kann das Herunterfahren Flugzeug Anfragen vor der Rückgabe warten.

## <a name="start-the-web-server"></a>Starten des Webservers

Sie nun können erstellen und eine Instanz der OwinCommunicationListener an den Webserver zurück. Überschreiben Sie in der Dienstklasse (WebService.cs) der `CreateServiceInstanceListeners()` Methode:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Dies ist der Web-API- *Anwendung* und den owin- *Host* schließlich treffen. Host (OwinCommunicationListener) wird eine Instanz der *Anwendung* (Web API) über die Startklasse angegeben. Fabric-Dienst verwaltet Lebenszyklus. Dieses Muster kann in der Regel alle Kommunikationsstapel folgen.

## <a name="put-it-all-together"></a>Alles zusammen

In diesem Beispiel müssen Sie müssen nichts die `RunAsync()` -Methode auf, sodass die Außerkraftsetzung einfach entfernt werden kann.

Die endgültigen Implementierung sollte sehr einfach sein. Es muss nur den kommunikationslistener zu erstellen:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

Die vollständige `OwinCommunicationListener` Klasse:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Nun, Sie alle eingeführt haben, sollte Ihr Projekt einer normalen Anwendung Web API Einstiegspunkte zuverlässige Dienste-API mit einem owin-Host aussehen:


![Web API owin-Host mit zuverlässigen Services API-Einstiegspunkten](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Ausführen und eine Verbindung über einen Web-browser

Wenn Sie [Umgebung einrichten](service-fabric-get-started.md)nicht getan.


Sie können jetzt erstellen und den Dienst bereitstellen. Drücken Sie **F5** in Visual Studio erstellen und Bereitstellen der Anwendungdes. Im Fenster Ereignisse müsste eine Meldung angezeigt, dass der Webserver auf Http://localhost:8281 geöffnet /.


![Visual Studio Diagnoseereignissen Fenster](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] Wenn der Port bereits von einem anderen Prozess auf Ihrem Computer geöffnet wurde, kann einen Fehler angezeigt. Dies bedeutet, dass der Listener konnte nicht geöffnet werden. Wenn dies der Fall ist, versuchen Sie einen anderen Anschluss für die Endpunktkonfiguration in ServiceManifest.xml.


Sobald der Dienst ausgeführt wird, öffnen Sie einen Browser, und navigieren zu [api Http://localhost:8281 Werte](http://localhost:8281/api/values) zu testen.

## <a name="scale-it-out"></a>Skalieren Sie

Statusfreie webapps normalerweise skalieren bedeutet mehr Computer hinzufügen und Web Apps auf drehen. Service Fabric Orchestrierungsmodul dazu Sie neue Knoten zu einem Cluster hinzugefügt werden. Beim Erstellen von Instanzen eines Diensts statusfreie können Sie die Anzahl der Instanzen zu erstellen. Service Fabric platziert diese Anzahl von Instanzen auf Knoten im Cluster. Und es nicht mehr als eine Instanz auf jedem Knoten zu erstellen. Sie können auch Service Fabric immer eine Instanz auf jedem Knoten erstellen, indem Sie die Instanzenzahl **-1** anweisen. Dies gewährleistet, dass bei jedem Hinzufügen von Knoten zu Cluster Skalieren wird eine Instanz des statusfreien Service auf den neuen Knoten erstellt. Dieser Wert ist eine Eigenschaft der Dienstinstanz festgelegt, wenn Sie eine Instanz erstellen. Sie erreichen dies über PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Sie können auch dabei Standarddienst in Visual Studio statusfreie Service-Projekt definieren:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Weitere Informationen zum Erstellen der Anwendung und Instanzen finden Sie unter [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Nächste Schritte

[Debuggen Sie Service Fabric-Anwendung mithilfe von Visual Studio](service-fabric-debugging-your-application.md)
