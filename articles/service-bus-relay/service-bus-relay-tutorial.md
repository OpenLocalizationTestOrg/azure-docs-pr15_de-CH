<properties 
    pageTitle="Service Bus Relay Tutorial | Microsoft Azure"
    description="Erstellen Sie ein Client Service Bus Anwendung und Service Bus Relay-Dienst."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Service Bus Relay-Lernprogramm

In diesem Lernprogramm beschrieben erstellen Sie eine einfache Anwendung mit Service Bus und Service Service Bus "Relay" Funktionen. Einen entsprechenden Lehrgang, Service Bus [vermittelten messaging](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)verwendet, finden Sie im [Service Bus vermittelte Messaging .NET Tutorial](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Dieses Lernprogramm bietet einen Überblick über die erforderlichen Schritte zum Erstellen einer Service Bus Client und Dienst-Anwendung. Wie ihre Gegenstücke WCF ist ein Dienst ein Konstrukt, das einen oder mehrere Endpunkte verfügbar macht jeweils eine oder mehrere Dienstvorgänge verfügbar macht. Der Endpunkt eines Diensts gibt eine Adresse, in dem der Dienst gefunden werden kann, eine Bindung mit den Informationen, die ein Client kommunizieren müssen, und der Dienst einen Vertrag, der die Funktionalität der Dienst Clients definiert. Der Hauptunterschied zwischen einem WCF und Service Bus Service ist, dass der Endpunkt in der Cloud nicht lokal auf dem Computer verfügbar gemacht wird.

Nachdem Sie die Reihenfolge der Themen in diesem Lernprogramm durcharbeiten, müssen Sie einen ausgeführten Dienst und einem Client, der die Vorgänge des Diensts aufrufen kann. Das erste Thema beschreibt, wie ein Konto einrichten. Die nächsten Schritte beschreiben den Dienst implementieren Dienst definieren, der einen Vertrag verwendet, und konfigurieren den Dienst im Code. Sie beschreiben außerdem hosten und führen Sie den Dienst. Der Dienst wurde ist selbst gehostete und Client und Dienst auf demselben Computer ausgeführt. Sie können den Dienst konfigurieren, Code oder eine Konfigurationsdatei.

Die letzten drei Schritte beschreiben, wie eine Clientanwendung erstellen, konfigurieren Sie die Clientanwendung erstellen und einen Client, der Zugriff auf die Funktionen des Hosts.

Alle Themen in diesem Abschnitt wird davon ausgegangen, dass Sie Visual Studio Development-Umgebung verwenden.

## <a name="sign-up-for-an-account"></a>Melden Sie sich für ein Konto

Der erste Schritt ist zu einem Namespace und einer freigegebenen Access Signatur (SAS) Schlüssel. Ein Namespace bietet Anwendungsgrenzen für jede Anwendung über Service Bus zur Verfügung gestellt. SAS-Taste wird automatisch vom System generiert, wenn ein Dienstnamespace erstellt. Die Kombination von Dienstnamespace und SAS-Taste stellt die Anmeldeinformationen für Dienst zum Authentifizieren des Zugriffs auf eine Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>Definieren einer WCF-Dienstvertrag mit Service Bus

Servicevertrag gibt an, welche Vorgänge (Web serviceterminologie für Methoden oder Funktionen) den Dienst unterstützt. Verträge werden durch die Definition einer Schnittstelle C++, C# oder Visual Basic erstellt. Jede Methode in der Schnittstelle entspricht einem bestimmten Dienst. Jede Schnittstelle muss [dem ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) Attribut angewendet, und jeder Vorgang muss [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) Attribut angewendet. Wenn eine Methode in einer Schnittstelle mit dem [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) Attribut nicht das Attribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) verfügen, ist diese Methode nicht verfügbar. Der Code für diese Aufgaben wird im Beispiel der Vorgehensweise bereitgestellt. Finden Sie mehr Informationen und Services [Entwerfen und Implementieren von Services](https://msdn.microsoft.com/library/ms729746.aspx) in WCF-Dokumentation.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Erstellen Sie einen Service Bus-Vertrag mit einer Schnittstelle

1. Öffnen Sie Visual Studio als Administrator das Programm im **Startmenü** und wählen **als Administrator ausführen**.

2. Erstellen Sie ein neues Konsolenanwendungsprojekt. Klicken Sie im Menü **Datei** auf **neu**, und klicken Sie auf **Projekt**. Klicken Sie im Dialogfeld **Neues Projekt** klicken Sie auf **Visual C#** (Wenn **Visual C#** nicht sehen unter **Andere Sprachen**angezeigt wird). Klicken Sie auf **die Konsolenanwendungsvorlage** und nennen Sie **EchoService**. Klicken Sie auf **OK** , um das Projekt zu erstellen.

    ![][2]

3. Installieren Sie Service Bus NuGet-Paket. Dieses Paket hinzugefügt automatisch Verweise auf Bibliotheken Service Bus als auch WCF- **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) ist der Namespace, der den programmgesteuerten Zugriff auf die grundlegenden Funktionen von WCF ermöglicht. Service Bus verwendet viele der Objekte und Attribute von WCF Dienstverträge definieren.

    Im Projektmappen-Explorer mit der rechten Maustaste der Projektmappe und dann auf **NuGet-Pakete verwalten, Lösung**. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Sicherstellen Sie, dass der Projektname im **Versionen** aktiviert ist. Klicken Sie auf **Installieren**und akzeptieren Sie die Vereinbarung.

    ![][3]

3. Doppelklicken Sie im Projektmappen-Explorer auf die Datei Program.cs, um sie im Editor zu öffnen, wenn es nicht bereits geöffnet ist.

4. Fügen Sie die folgende using-Anweisung am Anfang der Datei:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Ändern des Namespacenamens aus den Standardnamen des **EchoService** in **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] Dieses Tutorium stellt C# Namespace **Microsoft.ServiceBus.Samples**, die den Namespace der Vertragstyp verwaltet, die in der Konfigurationsdatei Schritt [Konfigurieren des WCF-Clients](#configure-the-wcf-client) verwendet wird. Sie können jeder Namespace angeben, wann dieses Beispiel. Das Lernprogramm funktioniert jedoch nicht, dann ändern Sie den Namespace des Vertrags und service entsprechend der Anwendungskonfigurationsdatei. In der Datei App.config angegebene Namespace muss Ihre C#-Dateien angegebenen Namespace identisch sein.

1. Direkt nach der `Microsoft.ServiceBus.Samples` Namespacedeklaration, aber innerhalb des Namespace eine neue Schnittstelle mit dem Namen `IEchoContract` und die `ServiceContractAttribute` -Attribut auf die Schnittstelle mit einem Wert **http://samples.microsoft.com/ServiceModel/Relay/**. Der Wert unterscheidet sich von der Namespace, den Gültigkeitsbereich des Codes verwenden. Stattdessen wird der Wert als eindeutiger Bezeichner für diesen Vertrag verwendet. Den Namespace explizit angeben verhindert der Vertragsname hinzufügen Namespace Standardwert.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Dienstvertragsnamespace enthält normalerweise ein Benennungsschema, die Versionsinformationen enthält. Einschließlich der Informationen in den Vertragsnamespace Service kann Dienste Änderungen isolieren, indem Sie einen neuen Servicevertrag mit einem neuen Namespace definieren und an einem neuen Endpunkt verfügbar machen. Auf diese Weise können Clients weiterhin den alten Servicevertrag Verwendung aktualisiert werden. Versionsinformationen kann ein Datum oder eine bestehen. Weitere Informationen finden Sie unter [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498). Für die Zwecke dieses Lernprogramms enthält das Namensschema Vertragsnamespace Service keine Versionsinformationen.

1. In der `IEchoContract` Schnittstelle, deklarieren Sie eine Methode für die Operation der `IEchoContract` macht die Benutzeroberfläche und Anwenden der `OperationContractAttribute` -Attribut auf die Methode, die Sie als Bestandteil des öffentlichen Vertrags Service Bus verfügbar machen möchten.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Direkt nach dem `IEchoContract` Schnittstellendefinition, deklarieren Sie einen Kanal, der beide erbt `IEchoContract` und die `IClientChannel` Schnittstelle, wie hier gezeigt:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Ein Kanal ist das WCF-Objekt über den Host und Client Informationen zu übergeben. Später, Schreiben Sie Code für den Kanal zwischen gegenseitige Echo.

1. Im Menü **Erstellen** auf **Projektmappe erstellen** , oder drücken Sie **STRG + UMSCHALT + B** , um die Richtigkeit Ihrer Arbeit bisher bestätigen.

### <a name="example"></a>Beispiel

Der folgende Code zeigt eine grundlegende Schnittstelle, die einen Service Bus-Vertrag definiert.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Die Schnittstelle erstellt wird, können Sie die Schnittstelle implementieren.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>Implementieren der WCF-Vertrag Service Bus verwenden

Service Bus Relay muss zum Erstellen den Vertrag erstellen mithilfe einer Schnittstelle definiert ist. Weitere Informationen zum Erstellen der Benutzeroberfläche finden Sie im vorherigen Schritt. Der nächste Schritt ist die Schnittstelle implementieren. Dies umfasst das Erstellen einer Klasse mit dem Namen `EchoService` , implementiert der benutzerdefinierte `IEchoContract` Schnittstelle. Nachdem Sie die Schnittstelle implementieren, werden anschließend mithilfe einer Konfigurationsdatei App.config Schnittstelle konfigurieren. Die Konfigurationsdatei enthält die erforderlichen Informationen für die Anwendung den Namen des Dienstes, den Namen des Vertrags und den Typ des Protokolls zur Kommunikation mit Service Bus. Der Code für diese Aufgaben wird im Beispiel die Verfahren bereitgestellt. Finden Sie allgemeine Informationen über das Implementieren eines Servicevertrags [Serviceverträge implementieren](https://msdn.microsoft.com/library/ms733764.aspx) in WCF-Dokumentation.

1. Erstellen Sie eine neue Klasse mit dem Namen `EchoService` direkt nach der Definition von der `IEchoContract` Schnittstelle. Die `EchoService` -Klasse implementiert den `IEchoContract` Schnittstelle. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Wie andere schnittstellenimplementierungen können Sie die Definition in einer anderen Datei implementieren. Für dieses Lernprogramm die Implementierung befindet sich in derselben Datei wie die Schnittstellendefinition und `Main` Methode.

1. Das [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) Attribut gelten die `IEchoContract` Schnittstelle. Das Attribut gibt den Dienstnamen und Namespace. Danach, die `EchoService` Klasse wie folgt:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Implementieren der `Echo` definierte Methode der `IEchoContract` -Schnittstelle in der `EchoService` Klasse. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. **Erstellen**klicken Sie auf **Projektmappe erstellen** , bestätigen Sie die Richtigkeit Ihrer Arbeit.

### <a name="to-define-the-configuration-for-the-service-host"></a>Um die Konfiguration des Diensthosts

1. Die Konfigurationsdatei ist ähnlich einer WCF-Konfigurationsdatei. Sie enthält die Namen, Endpunkt (d. h. den Speicherort Service Bus stellt für Clients und Server kommunizieren) und die Bindung (Typ Protokoll kommunizieren). Der Hauptunterschied ist, dass diese konfigurierten Endpunkt ist nicht Teil von.NET Framework eine Bindung [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) bezieht. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) ist eine Bindung Service Bus definiert.

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die Datei App.config, um sie in der Visual Studio-Editor öffnen.

2. In der `<appSettings>` Element, ersetzen Sie den Platzhalter mit dem Namen des Service-Namespace und die SAS-Taste in einem früheren Schritt kopiert. 

1. In der `<system.serviceModel>` -Tags Hinzufügen einer `<services>` Element. Sie können Service Bus Multifunktionsgeräte in einer Konfigurationsdatei definieren. In diesem Lernprogramm definiert jedoch nur ein.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. In der `<services>` Element hinzufügen ein `<service>` Element den Namen des Dienstes definieren.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. In der `<service>` Element definieren die Position der den Endpunktevertrag und den Typ der Bindung für den Endpunkt.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Der Endpunkt definiert, der Client für die Host-Anwendung aussieht. Das Lernprogramm anhand dieser Schritt einen URI erstellen, vollständig den Host über Service Bus. Die Bindung deklariert, dass TCP als Protokoll verwendeten Service Bus kommunizieren.

1. Klicken Sie im Menü **Erstellen** auf **Projektmappe** bestätigen Sie die Richtigkeit Ihrer Arbeit.

### <a name="example"></a>Beispiel

Im folgenden Codebeispiel wird der Implementierung des Servicevertrags.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Der folgende Code zeigt das Format der Datei App.config Diensthost zugeordnet.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Hosten Sie und führen Sie einen einfachen Webdienst Service Bus registriert

Dieser Schritt beschrieben, wie einen grundlegenden Service Bus-Dienst ausgeführt.

### <a name="to-create-the-service-bus-credentials"></a>Service Bus-Anmeldeinformationen erstellen

1. In `Main()`, erstellen Sie zwei Variablen zum Speichern des Namespaces und die SAS-Taste vom Konsolenfenster aus lesen.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Die SAS-Taste wird später auf das Projekt Service Bus verwendet werden. Der Namespace als Parameter übergebene `CreateServiceUri` Dienst-URI erstellen.

4. Mit einem [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) -Objekt deklarieren Sie, dass Sie einen SAS-Schlüssel als Anmeldeinformationstyp verwenden. Fügen Sie den folgenden Code direkt nach dem Code im letzten Schritt hinzugefügt.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>Eine Basisadresse für den Dienst erstellen

1. Erstellen Sie nach dem letzten Schritt hinzugefügten Code einen `Uri` Instanz für die Basisadresse des Diensts. Dieser URI gibt Schema Service Bus, den Namespace und den Pfad der Dienstschnittstelle.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "Sb" ist eine Abkürzung für Service Bus-Schema und gibt an, dass wir als Protokoll TCP verwenden. Dies wurde auch in der Konfigurationsdatei angegeben, [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) als Bindung angegeben wurde.
    
    Für dieses Lernprogramm ist `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Erstellen und Konfigurieren des Diensthosts

1. Legen die Konnektivität auf `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Konnektivität-Modus beschreibt das Protokoll der Dienst verwendet für die Kommunikation mit Service Bus; HTTP oder TCP. Mit der Standardeinstellung `AutoDetect`, das versucht, über TCP verfügbar ist und HTTP mit Service Bus verbinden ist TCP nicht verfügbar. Beachten Sie, dass dies den Dienst das Protokoll unterscheidet gibt für Client-Kommunikation. Dieses Protokoll bestimmt die verwendete Bindung. Beispielsweise können ein Dienst [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) Bindung gibt an, dass Clients über HTTP Endpunkt (Service Bus verfügbar) kommuniziert. Diesen Dienst konnte **ConnectivityMode.AutoDetect** angeben, sodass der Dienst über TCP mit Service Bus kommuniziert.

1. Den Diensthost erstellen, mit dem URI weiter oben in diesem Abschnitt erstellt.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Der Diensthost wird WCF-Objekt, das den Dienst instanziiert. Hier, Sie übergeben sie den Typ des zu erstellenden Dienstes (ein `EchoService` Typ), und auch an den Dienst verfügbar gemacht werden soll.

1. Fügen Sie am Anfang der Datei Program.cs Verweise auf [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) und [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. In `Main()`, Konfigurieren des Endpunkts zum öffentlichen Zugriff aktivieren.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Dieser Schritt informiert feed Service Bus, der die Anwendung öffentlich anhand der Service Bus ATOM finden für Ihr Projekt. Festlegen von **DiscoveryType** auf **private**wäre ein Clients weiterhin auf den Dienst zugreifen. Der Dienst wird jedoch nicht angezeigt, wenn Service Bus-Namespace durchsucht. Stattdessen müsste der Client den endpunktpfad kennen.

1. Gelten Sie die Anmeldeinformationen in der Datei App.config definierten Dienstendpunkte:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Wie im vorherigen Schritt erwähnt, können Sie mehrere Dienste und Endpunkte in der Konfigurationsdatei deklariert haben. Hätte, würde dieser Code durchlaufen Konfigurationsdatei und suchen Sie für jeden Endpunkt auf die Anmeldedaten angewendet werden soll. Allerdings hat die Konfigurationsdatei für dieses Lernprogramm nur einen Endpunkt.

### <a name="to-open-the-service-host"></a>Den Diensthost öffnen

1. Öffnen Sie den Service.

    ```
    host.Open();
    ```

1. Informieren Sie dem Benutzer, dass der Dienst ausgeführt wird, und erläutert, wie Sie den Dienst beenden.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Schließen Sie den Diensthost.

    ```
    host.Close();
    ```

1. Drücken Sie **STRG + UMSCHALT + B** zum Erstellen des Projekts.

### <a name="example"></a>Beispiel

Im folgenden Beispiel enthält den Servicevertrag und Implementierung von Schritten im Lernprogramm und stellt den Service in eine. Kompilieren Sie Folgendes in eine ausführbare Datei mit dem Namen EchoService.exe.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Erstellen eines WCF-Clients für den Servicevertrag

Der nächste Schritt ist basic Service Bus-Clientanwendung erstellen und definieren den Servicevertrag, die später implementiert werden sollen. Beachten Sie, dass viele Schritte die Schritte zum Erstellen eines Dienstes ähneln: definiert einen Vertrag, Bearbeiten einer App.config-Datei mit Anmeldeinformationen herstellen Service Bus usw.. Der Code für diese Aufgaben wird im Beispiel die Verfahren bereitgestellt.

1. Erstellen Sie ein neues Projekt in der aktuellen Visual Studio-Projektmappe für den Client folgendermaßen:
    1. Im Projektmappen-Explorer in der gleichen Projektmappe mit dem Dienst, mit der rechten Maustaste in der aktuellen Projektmappe (nicht das Projekt), und klicken Sie auf **Hinzufügen**. Klicken Sie auf **Neues Projekt**.
    2. Klicken Sie im Dialogfeld **Neues Projekt hinzufügen** auf **Visual C#** (Wenn **Visual C#** nicht sehen unter **Andere Sprachen**angezeigt wird), wählen Sie die Vorlage **Konsolenanwendungsprojekt** und nennen Sie es **EchoClient**.
    3. Klicken Sie auf **OK**.
<br />

1. Doppelklicken Sie im Projektmappen-Explorer Program.cs in das Projekt **EchoClient** im Editor geöffnet, wenn es nicht bereits geöffnet ist.

1. Ändern Sie den Namespacenamen aus den Standardnamen des `EchoClient` , `Microsoft.ServiceBus.Samples`.

1. Installieren Sie das [Service Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Im Projektmappen-Explorer mit der rechten Maustaste des **EchoClient** Projekts und dann auf **NuGet-Pakete verwalten**. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**und akzeptieren Sie die Vereinbarung.

    ![][3]

1. Hinzufügen einer `using` -Anweisung für den Namespace " [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) " in der Datei Program.cs. 

    ```
    using System.ServiceModel;
    ```

1. Fügen Sie Vertragsdefinition Service auf den Namespace hinzu, wie im folgenden Beispiel gezeigt. Beachten Sie, dass diese Definition Definition verwendeten **Dienst** entspricht. Fügen Sie diesen Code am oberen Rand der `Microsoft.ServiceBus.Samples` Namespace.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Drücken Sie **STRG + UMSCHALT + B** zum Erstellen des Clients.

### <a name="example"></a>Beispiel

Der folgende Code zeigt den aktuellen Status der Datei "Program.cs" im Projekt EchoClient.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>Konfigurieren Sie den WCF-client

In diesem Schritt erstellen Sie eine Datei App.config für eine grundlegende Anwendung, die den zuvor in diesem Lernprogramm erstellten Dienst zugreift. Diese Datei App.config definiert den Vertrag, Bindung und Name des Endpunkts. Der Code für diese Aufgaben wird im Beispiel die Verfahren bereitgestellt.

1. Doppelklicken Sie im Projektmappen-Explorer das Projekt **EchoClient** auf **App.config** zum Öffnen der Datei in der Visual Studio-Editor.

2. In der `<appSettings>` Element, ersetzen Sie den Platzhalter mit dem Namen des Service-Namespace und die SAS-Taste in einem früheren Schritt kopiert.

1. Fügen Sie innerhalb des Elements system.serviceModel ein `<client>` Element.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Dadurch erklärt, dass Sie eine Clientanwendung WCF-Format definieren.

1. In der `client` Element Name, Vertrag und Bindungstyp für den Endpunkt definiert.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Dieser Schritt definiert den Namen des Endpunkts der Vertrag gemäß der Dienst und die Tatsache, dass die Clientanwendung TCP für die Kommunikation mit Service Bus verwendet. Der Endpunktname wird im nächsten Schritt verknüpfen dieses Endpunktkonfiguration mit dem Dienst-URI verwendet.

1. Klicken Sie auf **Datei**und dann auf **Speichern**.

## <a name="example"></a>Beispiel

Der folgende Code zeigt die Datei App.config für den Echo-Client.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>Implementieren des WCF-Clients rufen Service Bus

In diesem Schritt implementieren Sie eine grundlegende Clientanwendung, die zuvor in diesem Lernprogramm erstellte auf den Dienst zugreift. Ähnlich wie der Dienst führt der Client die gleichen Vorgänge auf Service Bus:

1. Legt den Modus Konnektivität.
1. Den URI, der den Hostdienst sucht erstellt.
1. Definiert die Anmeldeinformationen.
1. Gilt die Anmeldeinformationen für die Verbindung.
1. Die Verbindung wird geöffnet.
1. Führt anwendungsspezifische Aufgaben aus.
1. Schließt die Verbindung.

Einer der Hauptunterschiede ist jedoch, dass die Clientanwendung einen Kanal zum Service Bus herstellen, verwendet während der Dienst einen Aufruf **ServiceHost**verwendet. Der Code für diese Aufgaben wird im Beispiel die Verfahren bereitgestellt.

### <a name="to-implement-a-client-application"></a>Eine Clientanwendung

1. Legen Sie die Konnektivität auf **AutoDetect**. Fügen Sie folgenden Code in die `Main()` -Methode der **EchoClient** -Anwendung.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Definieren Sie Variablen Werte für den Dienstnamespace und SAS-Schlüssel enthalten, die in der Konsole lesen.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Erstellen Sie den URI, der den Speicherort des Hosts im Service Bus Projekt definiert.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Erstellen Sie Anmeldeinformationsobjekt für Ihr Dienstendpunkt Namespace.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Erstellen der Kanalfactory, die in der Datei App.config beschriebene Konfiguration geladen.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Eine Kanalfactory ist ein WCF-Objekt, das eine über die Dienst- und Clientanwendungen Kommunikation erstellt.

1. Wenden Sie Service Bus-Anmeldeinformationen.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Erstellen Sie und öffnen Sie den Kanal an den Dienst.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Schreiben Sie einfache Benutzeroberfläche und Funktionen für das Echo.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Beachten Sie, dass der Code die Instanz des Objekts Kanal als Proxy für den Dienst verwendet.

1. Schließen Sie des Kanals und der Factory.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>Zur Ausführung der Anwendung

1. Drücken Sie **STRG + UMSCHALT + B** , um die Projektmappe zu erstellen. Damit wird das Clientprojekt und Service-Projekt, das in den vorherigen Schritten erstellt.

2. Bevor Sie die Clientanwendung ausführen, müssen sicherstellen, dass die Dienst-Anwendung ausgeführt wird. Im Projektmappen-Explorer in Visual Studio mit der rechten Maustaste **EchoService** Lösung und anschließend auf **Eigenschaften**.

3. Klicken Sie im Dialogfeld Eigenschaften von Projektmappe **Startprojekt**, klicken Sie die Schaltfläche **mehrere Startprojekte** . Stellen Sie sicher, dass **EchoService** in der Liste zuerst angezeigt wird. 

4. **Feld für die **EchoService** und **EchoClient** ** **zu**legen.

    ![][5]

5. Klicken Sie auf **Project Dependencies**. Wählen Sie im Feld **Projekte** **EchoClient**. Stellen Sie im Feld **hängt** sicher, dass **EchoService** aktiviert ist.

    ![][6]

6. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften** zu schließen.

7. Drücken Sie **F5** , um beide Projekte auszuführen.

8. Beide Fenster öffnen und den Namespacenamen aufgefordert. Der Dienst muss zuerst so im Konsolenfenster **EchoService** Geben Sie den Namespace und **drücken**.

9. Anschließend werden Sie aufgefordert, SAS-Taste. Geben Sie die SAS-Taste und drücken Sie die EINGABETASTE.

    Hier ist der Ausgabe vom Konsolenfenster aus. Beachten Sie, dass die Werte sind z. B. nur.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Die Dienst-Anwendung wird gedruckt im Konsolenfenster auf dem Abhören Adresse wie im folgenden Beispiel gezeigt.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. Geben Sie im Konsolenfenster **EchoClient** dieselbe Informationen, die Sie zuvor eingegeben, für die Anwendung haben. Führen Sie die obigen Schritte für die Clientanwendung den gleichen Dienstnamespace und SAS-Werte eingeben.

11. Nach dieser Eingabe der Client öffnet einen Kanal für den Dienst und fordert Sie zur Eingabe von Text als Konsole Leistung wird angezeigt.

    `Enter text to echo (or [Enter] to exit):` 

    Geben Sie Text zum Senden an die Service-Anwendung und drücken die EINGABETASTE. Dieser Text wird über den Dienstvorgang Echo an den Dienst gesendet und im Konsolenfenster Service wie die folgende Ausgabe angezeigt.

    `Echoing: My sample text`

    Die Clientanwendung erhält den Rückgabewert der `Echo` Vorgang wird der ursprüngliche Text und die im Konsolenfenster ausgegeben. Sieht die Ausgabe im Konsolenfenster des Clients.

    `Server echoed: My sample text`

12. Sie können Textnachrichten vom Client an den Dienst so weiter. Wenn Sie fertig sind, drücken Sie die EINGABETASTE im Konsolenfenster Client und Dienst beide Programme beenden.

## <a name="example"></a>Beispiel

Im folgende Beispiel zeigt die Vorgänge des Diensts Aufrufen eine Clientanwendung erstellen und zum Client zu schließen, nachdem der Vorgangsaufruf abgeschlossen ist.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm zeigte, wie ein Client Service Bus Anwendung und Service Service Bus "Relay" Funktionen erstellen. Ein ähnlich, die Service Bus [Messaging](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)verwendet, finden Sie unter [Service Bus vermittelte Messaging .NET Tutorial](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Weitere Informationen zu Service Bus finden Sie unter den folgenden Themen.

- [Übersicht über Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)
- [Service Bus-Grundlagen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Service-Architektur](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
