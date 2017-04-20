<properties
   pageTitle="Zuverlässige Dienste WCF Kommunikationsstapel | Microsoft Azure"
   description="Integrierte WCF Kommunikationsstapel Fabric Service bietet Kundenservice WCF-Kommunikation für zuverlässige Dienste."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF-Kommunikation für zuverlässige Dienste Stapel
Zuverlässige Dienste Framework ermöglicht Service-Autoren Kommunikationsstapel auswählen, den sie für den Dienst verwenden möchten. Sie können Kommunikationsstapel ihrer Wahl von [CreateServiceReplicaListeners oder CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) -Methoden zurückgegebenen **ICommunicationListener** anschließen. Das Framework bietet eine Implementierung des Stapels Kommunikation basiert auf Windows Communication Foundation (WCF) Service-Autoren, die WCF-Kommunikation verwenden möchten.

## <a name="wcf-communication-listener"></a>WCF-Kommunikationslistener
Die WCF-spezifische Implementierung von **ICommunicationListener** wird von der **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** -Klasse bereitgestellt.

Damit sagen wir einen Servicevertrag Typ haben`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Wir erstellen einen WCF-kommunikationslistener im Dienst folgt.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Clients für WCF Kommunikationsstapel schreiben
Schreibpapier mit Diensten kommunizieren mithilfe von WCF-Clients bietet das Framework **WcfClientCommunicationFactory**, die WCF-spezifische Implementierung von [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

WCF-Kommunikationskanal erfolgt über die **WcfCommunicationClient** von **WcfCommunicationClientFactory**erstellt.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

**WcfCommunicationClientFactory** mit **WcfCommunicationClient** können **ServicePartitionClient** Dienstendpunkt und Kommunikation mit dem Dienst implementiert Clientcode.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] Der Standardwert ServicePartitionResolver setzt voraus, dass der Client in demselben Cluster als Dienst ausgeführt wird. Wenn dies nicht der Fall, erstellen Sie ein ServicePartitionResolver-Objekt und Verbindungsendpunkte Cluster übergeben.

## <a name="next-steps"></a>Nächste Schritte
* [Remoteprozeduraufruf mit Remoting zuverlässige Dienste](service-fabric-reliable-services-communication-remoting.md)

* [Web-API mit OWIN in zuverlässige Dienste](service-fabric-reliable-services-communication-webapi.md)

* [Sichern der Kommunikation für zuverlässige Dienste](service-fabric-reliable-services-secure-communication.md)
