<properties
   pageTitle="Übersicht über zuverlässige Kommunikation | Microsoft Azure"
   description="Übersicht über Kommunikationsmodell zuverlässige Dienste, einschließlich der öffnenden Listener auf Services, Endpunkte behoben und Kommunikation zwischen Diensten."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>Wie Sie zuverlässige Dienste Kommunikations-APIs

Azure Service als Plattform ist Kommunikation zwischen vollständig unabhängig. Alle Protokolle und Stapeln sind von UDP HTTP akzeptabel. Es obliegt den Dienstentwickler Kommunikation Dienste auswählen. Zuverlässige Dienste Application Framework enthält integrierte Kommunikationsstapel sowie APIs, mit denen Sie benutzerdefinierte Kommunikationskomponenten erstellen. 

## <a name="set-up-service-communication"></a>Kommunikation einrichten

Zuverlässige Services-API verwendet eine einfache Schnittstelle für die Kommunikation. Öffnen Sie einen Endpunkt für den Dienst einfach implementieren Sie diese Schnittstelle:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

In der Überschreibung eine Service-basierte Klasse zurückgeben können Sie dann Ihre Kommunikation Listener Implementierung hinzufügen.

Für zustandsloser Dienste:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Statusbehaftete Dienste:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

In beiden Fällen wird eine Auflistung von Listenern zurückzugeben. Dadurch wird Ihr Dienst mehrere Endpunkte mit potenziell mehrere Listener mit unterschiedlichen Protokollen überwachen. Beispielsweise haben Sie einen HTTP-Listener und einen separaten WebSocket-Listener. Jeder Listener wird ein Name und die resultierende Auflistung *Name: Adresse* Paar als ein JSON-Objekt dargestellt wird, wenn ein Client die Adressen für eine Instanz oder eine Partition anfordert.

Die Überschreibung gibt statusfreie Service eine Auflistung von ServiceInstanceListeners. Ein ServiceInstanceListener enthält eine Funktion zum Erstellen einer ICommunicationListener und weist ihr einen Namen. Die Überschreibung gibt für statusbehaftete Dienste eine Auflistung von ServiceReplicaListeners. Deswegen geringfügig von der entsprechenden statusfreie eine ServiceReplicaListener Option ICommunicationListener auf sekundären Replikaten geöffnet hat. Nicht nur können Sie mehrere kommunikationslistener in einem Dienst, sondern Sie können auch angeben, welche Listener auf sekundäre Replikate übernehmen und welche nur primäre Replikate zu überwachen.

Beispielsweise können Sie eine ServiceRemotingListener, die nur auf primären Replikate RPC-Aufrufe akzeptiert und ein zweiter, benutzerdefinierter Listener, die gelesen fordert sekundäre Kopien über HTTP:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Wenn mehrere Listener für einen Dienst erstellen, jeder Listener **muss** erhalten einen eindeutigen Namen.

Beschreiben Sie abschließend die Endpunkte für den Dienst in das [manifest Service](service-fabric-application-model.md) auf Endpunkte unter erforderlich sind.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Der kommunikationslistener möglich Endpunkt Mittel, die `CodePackageActivationContext` in der `ServiceContext`. Listener kann Anfragen Abhören beim Öffnen starten.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Ressourcen sind für das gesamte Paket, und sie werden durch Service zugeordnet, wenn das Service-Paket aktiviert ist. Mehrere Replikate der gleichen ServiceHost gehostet möglicherweise denselben Port verwenden. Dies bedeutet, dass der kommunikationslistener Portfreigabe unterstützen soll. Die empfohlene Vorgehensweise ist für den kommunikationslistener mit der Partition und Replikat-Instanz-ID Listen Adresse generiert.

### <a name="service-address-registration"></a>Service-Adressregistrierung

Ein Systemdienst namens *Naming Service* läuft auf Service Fabric-Cluster. Der Namensdienst ist Registrar für Dienste und deren Adressen, denen jede Instanz oder Kopie der Dienst überwacht. Wenn die `OpenAsync` Methode ein `ICommunicationListener` Abschluss seiner Rückkehr-Wert wird in der DNS-Dienst registriert. Dieser Rückgabewert, das im Namensdienst veröffentlicht ist eine Zeichenfolge, deren Wert etwas sein kann. Diese Zeichenfolge wird vom Naming Service für ein Wahldomizil Fragen Clients angezeigt werden.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Service Fabric enthält APIs, die Clients und andere Dienste für diese Adresse nach Dienstnamen bitten können. Dies ist wichtig, weil die Adresse nicht statisch ist. Dienste werden im Cluster für Lastenausgleich und Verfügbarkeit Ressource verschoben. Dies ist der Mechanismus, der Kunden die Überwachungsadresse für einen Dienst zu beheben.

> [AZURE.NOTE] Für eine vollständige Anleitung zum Schreiben einer `ICommunicationListener`, finden Sie unter [Service Fabric Web API-Services mit owin-Self-hosting](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Kommunikation mit einem Dienst
Zuverlässige Dienste-API bietet die folgenden Bibliotheken Kunden schreiben, die mit Diensten kommunizieren.

### <a name="service-endpoint-resolution"></a>Service-Endpunkt Auflösung
Der erste Schritt zur Kommunikation mit einem Dienst ist ein Endpunkt Adresse der Partition oder eine Instanz des Dienstes, den Sie sprechen möchten. Die `ServicePartitionResolver` Utility-Klasse ist eine grundlegende Primitive, die Kunden den Endpunkt eines Dienstes zur Laufzeit bestimmen. Service Fabric-Terminologie ist der Prozess des Endpunkts eines Diensts *Service Endpunkt Auflösung*genannt.

Verbindung mit Diensten in einem Cluster `ServicePartitionResolver` mit Standardeinstellungen erstellt werden. Dies ist für die meisten Situationen empfohlen:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Zu den Services in einem anderen Cluster Herstellen einer `ServicePartitionResolver` mit Cluster Gateway Endpunkte erstellt werden. Beachten Sie, dass Gateway Endpunkte nur verschiedene Endpunkte für die Verbindung mit demselben Cluster. Zum Beispiel:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Alternativ `ServicePartitionResolver` erhalten eine Funktion zum Erstellen einer `FabricClient` intern verwendet: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`ist das Objekt, mit dem Fabric Service für verschiedene Verwaltungsvorgänge im Cluster kommunizieren. Dies ist nützlich, wenn Sie mehr Kontrolle über das `ServicePartitionResolver` mit Ihr interagiert. `FabricClient`führt intern Zwischenspeichern und ist in der Regel teuer, unbedingt wiederverwenden `FabricClient` möglichst viele Instanzen. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Resolve-Methode wird die Adresse eines Dienstes oder Service-Partition für partitionierte Dienste abrufen verwendet.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Eine leicht aufgelöst werden kann ein `ServicePartitionResolver`, aber mehr Arbeit erforderlich ist die aufgelöste Adresse korrekt verwendet werden. Ihre Kunden müssen erkennen, ob der Verbindungsversuch wegen Übertragungsfehler fehlgeschlagen und wiederholt werden kann (z. B. Service verschoben oder ist vorübergehend nicht verfügbar) oder permanent (z.B. Dienst gelöscht wurde oder die angeforderte Ressource nicht mehr vorhanden ist). Dienstinstanzen oder Replikate können von Knoten zu Knoten jederzeit aus mehreren Gründen bewegen. Die Serviceadresse gelöst `ServicePartitionResolver` Zeitpunkt Clientcode versucht, die Verbindung möglicherweise veraltet. In diesem Fall erneut müssen Client neu Auflösen der Adresse. Bereitstellen der `ResolvedServicePartition` gibt an, dass die Auflösung erneut einfach abrufen eine zwischengespeicherte Adresse.

Normalerweise muss der Clientcode nicht arbeiten mit der `ServicePartitionResolver` direkt. Es erstellt und zu Kommunikation Client zuverlässige Dienste-API übergeben. Die Factorys verwenden den Resolver intern eine Clientobjekt zu generieren, die zur Kommunikation mit Services.

### <a name="communication-clients-and-factories"></a>Kommunikation Clients und Fabriken

Kommunikation Factory Bibliothek implementiert ein normale Fehlerbehandlung wiederholen Muster, das Wiederholung Verbindungen gelöst Dienstendpunkte erleichtert. Die Factory-Bibliothek stellt die Wiederholungsmechanismus ermöglichen die Fehler-Handler.

`ICommunicationClientFactory`definiert die Basisschnittstelle implementiert von einer Kommunikation Client Factory, die Kunden erstellt, die einen Service Fabric-Dienst kommunizieren kann. Die Implementierung der CommunicationClientFactory hängt vom Service Fabric-Dienst verwendet, der Client kommunizieren möchte Kommunikationsstapel. Zuverlässige Dienste-API bietet eine `CommunicationClientFactoryBase<TCommunicationClient>`. Dies stellt eine grundlegende Implementierung der `ICommunicationClientFactory` -Schnittstelle und Aufgaben für die Kommunikationsstapel. (Dazu zählen mithilfe einer `ServicePartitionResolver` Dienstendpunkt bestimmt). Kunden implementieren in der Regel abstrakte CommunicationClientFactoryBase Klasse Logik zu behandeln, die für die Kommunikationsstapel.

Kommunikationsclient erhält eine Adresse einfach und Verbindung zu einem Dienst verwendet. Der Client kann welches Protokoll verwenden sollen.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Client-Factory ist hauptsächlich verantwortlich für Kommunikation Clients erstellen. Für Clients, die eine permanente Verbindung wie ein HTTP-Client verwalten muss die Factory nur erstellen und den Client zurückgeben. Andere Protokolle, die beibehalten einer permanenten Verbindung wie einige binäre Protokolle sollten auch geprüft werden, von der Factory zu ermitteln, ob die Verbindung neu erstellt werden muss.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Ein Ausnahmehandler ist verantwortlich dafür, welche Aktion beim Auftreten einer Ausnahme. Ausnahmen in kategorisiert **wiederholbar** und **nicht wiederholbar**. 

 - **Nicht wiederholbar** Ausnahmen erhalten einfach wieder an den Aufrufer ausgelöst. 
 - **Retriable** Ausnahmen sind **vorübergehend** und **nicht flüchtigen**weiter kategorisiert.
  - **Vorübergehende** Ausnahmen werden einfach wiederholt werden kann, ohne erneut die Endpunktadresse Service. Dazu gehören vorübergehende Netzwerkprobleme oder Fehlerantworten als die an die Endpunktadresse Service nicht vorhanden. 
  - **Nicht flüchtigen** Ausnahmen sind die Endpunktadresse Service wieder gelöst werden müssen. Diese Ausnahmen enthalten, die angeben, dass der Endpunkt nicht erreicht werden konnte, wurde vom Dienst an einen anderen Knoten verschoben. 

Die `TryHandleException` entscheidet über eine bestimmte Ausnahme. Wenn sie **weiß nicht,** welche Beschlüsse über eine Ausnahme machen, es **false**zurück. Wenn er **weiß,** welche Entscheidung zu, es sollte das Ergebnis entsprechend und **true**zurückgeben.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Fazit
Mit einer `ICommunicationClient`, `ICommunicationClientFactory`, und `IExceptionHandler` ein Kommunikationsprotokoll basiert eine `ServicePartitionClient` ist alles umschließt und bietet Service Partition Fehlerbehandlung Adresse Auflösung Schleife um diese Komponenten.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Nächste Schritte
 - Finden Sie ein Beispiel für HTTP-Kommunikation zwischen Diensten in einem [Beispielprojekt auf GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Remoteprozeduraufrufe mit Remoting zuverlässige Dienste](service-fabric-reliable-services-communication-remoting.md)

 - [Web-API, die zuverlässige Dienste OWIN verwendet](service-fabric-reliable-services-communication-webapi.md)

 - [Zuverlässige Dienste mit WCF-Kommunikation](service-fabric-reliable-services-communication-wcf.md)
