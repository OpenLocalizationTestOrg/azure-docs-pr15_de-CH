<properties
    pageTitle="Verwendung von Service Bus Relay mit .NET | Microsoft Azure"
    description="Informationen Sie zum Azure Service Bus-Dienst verwenden, um zwei Applikationen an Standorten herstellen."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Wie Azure Service Bus-Dienst

Dieser Artikel beschreibt das Service Bus-Dienst verwenden. Die Proben sind in C# geschrieben und mit Service Bus wurde Windows Communication Foundation (WCF) API verwenden. Weitere Informationen zu Service Bus Relay finden Sie unter Übersicht über [messaging Service Bus weitergeleitet](service-bus-relay-overview.md) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Was ist das Relay Service Bus?

Der [Service Bus *Relay* ](service-bus-relay-overview.md) Service ermöglicht hybride Anwendung erstellen, die in Azure-Rechenzentrum und Ihrer lokalen Enterprise-Umgebung ausgeführt. Service Bus Relay erleichtert aktivieren Sie sicheren Windows Communication Foundation (WCF)-Dienste verfügbar machen, die in einem Unternehmensnetzwerk für die öffentliche Cloud befinden, ohne eine Firewall Verbindung oder Einfluss sich eine Netzwerk-Infrastruktur.

![Relay-Konzepte](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Service Bus Relay können Sie Host WCF-Dienste in Ihre vorhandenen Enterprise-Umgebung. Sie können dann delegieren überwacht eingehende Sessions und Anfragen an die WCF-Dienste in Azure Service Bus-Dienst. Dadurch werden diese Dienste Anwendungscode in Azure ausgeführt oder mobile Mitarbeiter oder Partner extranet-Umgebung. Service Bus können Sie sicher steuern, die hierfür abgestimmte Ebene zugreifen können. Es ermöglicht Sicherheit Anwendungsfunktionen und-Daten von der vorhandenen Enterprise-Lösung und Nutzen von der Cloud.

Dieser Artikel veranschaulicht, wie das Service Bus Relay erstellt einen WCF-Webdienst, ein TCP-Channelbindung, die eine sichere Konversation zwischen zwei Parteien mit verfügbar gemacht.

## <a name="create-a-service-namespace"></a>Erstellen eines Service-Namespaces

Zunächst in Azure Service Bus Relay verwenden müssen Sie zunächst einen Namespace erstellen. Ein Namespace bietet ein Bereichseinheit Service Bus Ressourcen in der Anwendung.

So erstellen Sie einen Service-namespace

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Erhalten Sie Service Bus NuGet-Paket

[Service Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) ist am einfachsten zu Service Bus-API und die Anwendung mit allen Service Bus Abhängigkeiten konfigurieren. Gehen Sie wie folgt vor, um das NuGet-Paket im Projekt zu installieren:

1.  Im Projektmappen-Explorer mit der rechten Maustaste **Verweise**und dann auf **NuGet-Pakete verwalten**.
2.  "Service Bus" Suchen Sie, und wählen Sie **Microsoft Azure Service Bus** . Klicken Sie auf **Installieren** , um die Installation abzuschließen und schließen Sie das Dialogfeld zu:

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Verwenden Sie Service Bus und einen SOAP-Webdienst mit TCP verwenden

Um einen vorhandenen WCF SOAP-Webdienst für die externe Verwendung verfügbar zu machen, müssen Sie Service Bindings und Adressen ändern. Dies muss geändert Konfigurationsdatei oder Code ändert, je nachdem, wie Sie eingerichtet und konfiguriert Ihre WCF-Dienste konnte erfordert. Beachten Sie, dass WCF können Sie mehrere Netzwerkendpunkte über denselben Dienst so vorhandene interne Endpunkte beibehalten werden kann, während gleichzeitig Service Bus Endpunkte für den externen Zugriff hinzufügen.

In dieser Aufgabe wird einen einfachen WCF-Dienst erstellen und Service Bus Listener hinzufügen. In dieser Übung übernimmt mit Visual Studio vertraut und daher nicht alle Details erstellen ein Projekt durchlaufen. Stattdessen konzentriert sich auf den Code.

Gehen Sie vor Schritte zum Einrichten der Umgebung:

1.  Erstellen Sie in Visual Studio eine, die zwei Projekte "Client" und "Service" in der Projektmappe enthält.
2.  Beide Projekte Microsoft Azure Service Bus NuGet-Paket hinzufügen. Dieses Paket wird die erforderlichen Assemblyverweise zu Projekten hinzugefügt.

### <a name="how-to-create-the-service"></a>Den Dienst erstellen

Erstellen Sie zunächst den Dienst selbst. WCF-Dienst besteht aus drei Teilen:

-   Definition eines, das beschreibt, welche Nachrichten ausgetauscht werden und welche Vorgänge aufgerufen werden.
-   Implementierung des Vertrages.
-   Host, den WCF-Dienst gehostet und mehrere Endpunkte verfügbar macht.

Die Codebeispiele in diesem Abschnitt behandeln jede dieser Komponenten.

Der Vertrag definiert einen einzelnen Vorgang `AddNumbers`, die zwei Zahlen addiert und das Ergebnis zurückgibt. Die `IProblemSolverChannel` Schnittstelle ermöglicht es dem Client Proxy Leben leichter verwalten. Erstellen einer Schnittstelle gilt als bewährte Methode. Es empfiehlt sich, diese Vertragsdefinition in eine separate Datei, so dass diese Datei aus Ihrem "Client" und "Service verweisen", aber Sie können den Code auch in beiden Projekten kopieren.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Der Vertrag ist die Implementierung einfach.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Einen Diensthost programmgesteuert konfigurieren

Der Vertrag und Implementierung vorhanden können Sie nun den Dienst hosten. Tritt in ein [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) -Objekt, das Verwaltung von Instanzen des Dienstes kümmert und hostet die Endpunkte, die Nachrichten empfangen werden. Der folgende Code konfiguriert den Dienst mit regulären lokalen Endpunkt und Service Bus-Endpunkt Darstellung nebeneinander interner und externer Endpunkte zu erläutern. Ersetzen Sie String *Namespace* mit Namespacenamen und *YourKey* mit SAS-Schlüssel, den im vorherigen Schritt von Setup abgerufen.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

Im Beispiel erstellen Sie zwei Endpunkte, die auf denselben Vertrag Implementierung. Eine lokale und eine voraussichtlich über Service Bus. Die wichtigsten Unterschiede sind Bindung. [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) für das lokale und [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) für den Service Bus-Endpunkt und Adressen. Der lokale Endpunkt hat eine lokale Netzwerkadresse mit unterschiedlichen Port. Service Bus-Endpunkt wurde die Zeichenfolge aus eine Endpunktadresse `sb`, die Namespacenamen und den Pfad "Solver". Dies führt im URI `sb://[serviceNamespace].servicebus.windows.net/solver`, den Endpunkt als Service Bus TCP-Endpunkt einen vollqualifizierten externen DNS-Namen identifiziert. Setzen Sie den Code ersetzen die Platzhalter in der `Main` Funktion der **Service** -Anwendung haben einen funktionierenden Dienstes. Wenn Sie Ihren Dienst ausschließlich auf Service Bus hören möchten, entfernen Sie die Deklaration lokalen Endpunkt.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Einen Diensthost in der Datei App.config konfigurieren

Sie können auch mithilfe der Datei App.config Host konfigurieren. Im nächsten Beispiel wird der Dienst Hostingcode in diesem Fall angezeigt.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Endpunktdefinitionen verschieben in der Datei App.config. NuGet-Paket hat bereits zahlreiche Definitionen Datei, die die erforderliche Konfiguration Extensions for Service Bus. Im folgende Beispiel ist die genaue Entsprechung des vorherigen Code sollte direkt unter dem **system.serviceModel** -Element angezeigt werden. In diesem Codebeispiel wird vorausgesetzt, dass Projektnamespace C# **Dienst**heißt.
Ersetzen Sie die Platzhalter mit Service Bus Service Namespace und SAS-Taste.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Nach dem vornehmen dieser Änderung der Dienst beginnt wie zuvor, aber mit zwei aktiven Endpunkte: eine lokale und eine Überwachung in der Cloud.

### <a name="create-the-client"></a>Client erstellen

#### <a name="configure-a-client-programmatically"></a>Konfigurieren eines Clients programmgesteuert

Um den Dienst zu verwenden, können Sie einen WCF-Client mit einem [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) -Objekt erstellen. Service Bus verwendet eine Token-Sicherheitsmodell mithilfe SAS implementiert. Die [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) -Klasse stellt ein Sicherheitstokenanbieter mit integrierten Factorymethoden, die einige bekannte Tokenanbieter zurückgeben. Im folgende Beispiel verwendet die [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) -Methode mit der Übernahme von entsprechenden SAS-Token. Den Namen und Schlüssel sind die vom Portal aus wie im vorherigen Abschnitt beschrieben.

Zunächst verweisen oder kopieren Sie die `IProblemSolver` Vertrag aus der Dienst in dem Clientprojekt.

Ersetzen Sie den Code in die `Main` -Methode des Clients erneut Ersetzen des Platzhaltertextes einen Service Bus-Namespace und die SAS-Taste.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Jetzt Erstellen der Client und der Dienst ausgeführt werden (den Dienst zuerst ausgeführt) und der Client Ruft den Dienst und **9**druckt. Sie können Client und Server auf verschiedenen Computern ausführen, sogar über Netzwerke und Kommunikation sind weiterhin funktionsfähig. Der Clientcode kann auch in der Cloud oder lokal ausführen.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Konfigurieren eines Clients in der Datei App.config

Im folgenden Codebeispiel wird das Verwenden der Datei App.config Client konfigurieren.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Endpunktdefinitionen verschieben in der Datei App.config. Identisch mit den zuvor aufgeführten Code wird Beispiel sollte direkt unter dem **system.serviceModel** -Element angezeigt werden. Hier müssen wie zuvor Sie Platzhalter mit Service Bus-Namespace und SAS-Schlüssel ersetzen.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Service Bus-Dienst bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

- [Service Bus Relay Übersicht über messaging](service-bus-relay-overview.md)
- [Azure Service Bus-Architektur im Überblick](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- Aus [Azure][] herunterladen Sie Service Bus Beispiele oder finden Sie unter [Übersicht über Service Bus-Beispiele][].

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Azure-Beispiele]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Überblick über Service Bus-Beispiele]: ../service-bus-messaging/service-bus-samples.md