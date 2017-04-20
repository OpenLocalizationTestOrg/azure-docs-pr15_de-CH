<properties
   pageTitle="Sichere Kommunikation für Webdienste in Service Fabric-Hilfe | Microsoft Azure"
   description="Übersicht über sichere Kommunikation für zuverlässige Dienste zu, die in Azure Service Fabric-Cluster ausgeführt werden."
   services="service-fabric"
   documentationCenter=".net"
   authors="suchiagicha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="suchiagicha"/>

# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Sichere Kommunikation für Webdienste in Azure Service Fabric-Hilfe

Sicherheit ist einer der wichtigsten Aspekte der Kommunikation. Zuverlässige Dienste Application Framework enthält einige vordefinierte Kommunikationsstapel und Tools, mit denen Sie die Sicherheit verbessern können. Dieser Artikel wird zum Erhöhen der Sicherheit bei Verwendung Service Remoting und Windows Communication Foundation (WCF) Kommunikationsstapel sprechen.

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Sichern Sie ein Dienstes beim Service Remoting verwenden

Wir verwenden ein [Beispiel](service-fabric-reliable-services-communication-remoting.md) , das Einrichten von Remoting für zuverlässige Dienste erläutert. Um einen Dienst Sichern beim Service Remoting verwenden, gehen Sie folgendermaßen vor:

1. Erstellen Sie eine Schnittstelle `IHelloWorldStateful`, definiert die Methoden, die für einen Remoteprozeduraufruf für den Dienst. Nutzung des Dienstes `FabricTransportServiceRemotingListener`, in deklariert die `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` Namespace. Dies ist ein `ICommunicationListener` -Implementierung, Funktionen bereitstellt.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Fügen Sie Listener Settings und Anmeldeinformationen.

    Stellen Sie sicher, dass das Zertifikat, das Sie schützen Ihre servicekommunikation verwenden möchten auf allen Knoten im Cluster installiert ist. Gibt es zwei Methoden, um Listener Settings und Anmeldeinformationen bereitstellen können:

    1. Geben sie direkt in den Code:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Geben sie mithilfe eines [Config-Paket](service-fabric-application-model.md):

        Hinzufügen einer `TransportSettings` Abschnitt in der Datei settings.xml.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        In diesem Fall die `CreateServiceReplicaListeners` -Methode wird wie folgt aussehen:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Beim Hinzufügen einer `TransportSettings` Abschnitt in der Datei settings.xml ohne Präfix, `FabricTransportListenerSettings` lädt alle Einstellungen aus diesem Abschnitt standardmäßig.

         ```xml
         <!--"TransportSettings" section without any prefix.-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         In diesem Fall die `CreateServiceReplicaListeners` -Methode wird wie folgt aussehen:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Beim Aufruf Methoden auf einem sicheren Dienst mithilfe des Stapels Remoting anstelle der `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` Klasse Service Proxy, erstellt `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Übergeben Sie `FabricTransportSettings`, enthält `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Wenn der Client-Code als Teil eines Diensts ausgeführt wird, laden Sie `FabricTransportSettings` aus der Datei settings.xml. Erstellen Sie einen TransportSettings Abschnitt den Code ähnlich wie oben beschrieben. Ändern der folgende Clientcode:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Wenn der Client nicht als Teil eines Diensts ausgeführt wird, können client_name.settings.xml Erstellen einer Datei am gleichen Speicherort der client_name.exe ist. Erstellen Sie einen TransportSettings-Abschnitt in der Datei.

    Ähnlich wie bei den Dienst Hinzufügen einer `TransportSettings` ohne Präfix in Client-settings.xml/client_name.settings.xml `FabricTransportSettings` lädt alle Einstellungen aus diesem Abschnitt standardmäßig.

    In diesem Fall ist der frühere Code weiter vereinfacht:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Sichern Sie ein Dienstes bei Verwendung ein Stapels WCF-Kommunikation

Wir verwenden ein [Beispiel](service-fabric-reliable-services-communication-wcf.md) die Verwendung einer WCF-Kommunikationsstapel für zuverlässige Dienste einrichten. Gehen Sie folgendermaßen vor, um zu sichern einen Dienst bei Verwendung ein Stapels WCF-Kommunikation:

1. Für den Dienst WCF kommunikationslistener sichern müssen (`WcfCommunicationListener`), die Sie erstellen. Ändern Sie dazu die `CreateServiceReplicaListeners` Methode.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. Der Client den `WcfCommunicationClient` im vorherigen [Beispiel](service-fabric-reliable-services-communication-wcf.md) erstellten bleibt unverändert. Jedoch müssen Sie überschreiben die `CreateClientAsync` -Methode des `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Mit `SecureWcfCommunicationClientFactory` Erstellen eines WCF-Kommunikation Clients (`WcfCommunicationClient`). Verwenden Sie den Client Webdienstmethoden aufrufen.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Web-API mit OWIN in zuverlässige Dienste](service-fabric-reliable-services-communication-webapi.md)
