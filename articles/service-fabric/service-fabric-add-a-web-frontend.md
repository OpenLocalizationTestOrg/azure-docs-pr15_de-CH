<properties
   pageTitle="Erstellen Sie ein Web-front-End für Ihre Anwendung ASP.NET Core | Microsoft Azure"
   description="Die Service Fabric-Anwendung im Web mit einem ASP.NET Core Web API und internen Kommunikation über ServiceProxy verfügbar."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Erstellen von Web Service front-End für Ihre Anwendung ASP.NET Core

Standardmäßig bieten Azure Service Fabric-Services keine öffentliche Schnittstelle im Web. Um die Funktionalität Ihrer Anwendung auf HTTP-Clients verfügbar zu machen, müssen Sie erstellen ein Webprojekt als Einstiegspunkt und von dort auf Ihre individuellen Services kommunizieren.

In diesem Lernprogramm wir wo wir im Lernprogramm [erstellen Ihre erste Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) unterbrochen und Hinzufügen eines Webdienstes vor statusbehaftete Zähler Service. Wenn nicht bereits geschehen, sollten Sie zurückgehen und dieses Lernprogramm zunächst schrittweise.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Eine ASP.NET Basisdienst zur Anwendung hinzufügen

ASP.NET Core ist eine einfache, plattformübergreifende-Webentwicklungsframework, die Sie verwenden können, erstellen Sie moderne Benutzeroberfläche und web-APIs. Fügen Sie ein ASP.NET Web API-Projekt vorhandenen Anwendung.

>[AZURE.NOTE] Um dieses Lernprogramm müssen Sie [installieren .NET Core 1.0][dotnetcore-install].

1. Im Projektmappen-Explorer Maustaste **Services** in das Anwendungsprojekt, und wählen Sie **Hinzufügen > Neues Fabric-Dienst**.

    ![Hinzufügen eines neuen Dienstes zu einer vorhandenen Anwendung][vs-add-new-service]

2. Wählen Sie auf der Seite **Erstellen eines Dienstes** **ASP.NET Core** und nennen Sie sie.

    ![ASP.NET WebService auswählen im neuen Dialogfeld Dienst][vs-new-service-dialog]

3. Die Seite bietet eine Reihe von ASP.NET Core Projektvorlagen. Beachten Sie, dass diese dieselben Vorlagen, die angezeigt, wenn ein Projekt ASP.NET Core außerhalb Service Fabric-Anwendung erstellt. Für dieses Lernprogramm wählen wir **Web-API**. Jedoch können Sie dieselben Konzepte auf eine vollständige Anwendung anwenden.

    ![ASP.NET Projekttyp auswählen][vs-new-aspnet-project-dialog]

    Nach Web API-Projekt erstellt wird, müssen Sie zwei Dienste in Ihrer Anwendung. Wie Sie die Anwendung erstellen, fügen Sie weitere Dienste auf genau die gleiche Weise. Separat versioniert und aktualisiert können werden.

>[AZURE.TIP] Erfahren Sie mehr über das Erstellen von ASP.NET Core Services finden Sie unter [ASP.NET Kerndokumentation](https://docs.asp.net).

## <a name="run-the-application"></a>Führen Sie die Anwendung

Um erhalten haben, die neue Anwendung bereitstellen und werfen einen Blick auf ASP.NET Core Web API-Vorlage enthält standardmäßig.

1. Drücken Sie F5 in Visual Studio zum Debuggen der Anwendung.

2. Nach Abschluss der Bereitstellung startet Visual Studio den Browser an den Stamm des Diensts ASP.NET Web API - etwas Http://localhost:33003. Die Portnummer nach dem Zufallsprinzip zugewiesen und möglicherweise auf Ihrem Computer anders. ASP.NET Core Web API Vorlage bereitstellen nicht standardmäßig für den Stamm, damit eine Fehlermeldung im Browser angezeigt wird.

3. Hinzufügen `/api/values` im Browser an. Das Aufrufen der `Get` Methode ValuesController in der Vorlage Web-API. Es wird standardmäßig zurück, die von der Vorlage ein JSON-Array bereitgestellt wird, das zwei Zeichenfolgen enthält:

    ![Standardwerte von ASP.NET Core Web API Vorlage zurückgegeben][browser-aspnet-template-values]

    Ende des Lernprogramms haben wir diese Werte mit den letzten Wert des Indikators aus unserer statusbehaftete Dienst ersetzt.


## <a name="connect-the-services"></a>Verbinden der Dienste

Fabric-Dienst bietet vollständige Flexibilität bei der Kommunikation mit zuverlässige Dienste. Innerhalb einer einzelnen Anwendung müssen Sie Dienste, die über TCP, andere Dienste, die über eine HTTP-REST-API und andere Dienste, die über Web Sockets. Hintergrundinformationen zu den verfügbaren Optionen und die Nachteile anzeigen Sie [Kommunikation mit services](service-fabric-connect-and-communicate-with-services.md) In diesem Lernprogramm wir einen einfacheren Ansätze und verwenden die `ServiceProxy` / `ServiceRemotingListener` im SDK enthaltenen Klassen.

In der `ServiceProxy` Ansatz (modelliert Remoteprozeduraufrufe remote Procedure Calls oder RPCs), definieren Sie eine Schnittstelle als dem öffentlichen Vertrag für den Dienst fungiert. Anschließend verwenden Sie diese Schnittstelle eine Proxyklasse für die Interaktion mit dem Dienst generiert.


### <a name="create-the-interface"></a>Erstellen der Benutzeroberfläche

Zunächst wird die Schnittstelle als Vertrag zwischen der statusbehaftete Dienst und seinen Clients einschließlich ASP.NET Core-Projekt handeln.

1. Im Projektmappen-Explorer mit der rechten Maustaste der Projektmappe, und wählen Sie **Hinzufügen** > **Neues Projekt**.

2. Wählen Sie im linken Navigationsbereich den Eintrag **Visual C#** und dann die Vorlage **Klassenbibliothek** . Stellen Sie sicher, dass die.NET Framework-Version **4.5.2**ist.

    ![Erstellen ein Projekt für die statusbehaftete Dienst][vs-add-class-library-project]

3. Reihenfolge für eine Schnittstelle von `ServiceProxy`, muss von der IService-Schnittstelle abgeleitet werden. Diese Schnittstelle ist in Service Fabric NuGet-Paketen enthalten. Fügen Sie das Paket Maustaste auf das neue Klassenbibliotheksprojekt und **NuGet-Pakete verwalten**.

4. Das **Microsoft.ServiceFabric.Services** -Paket suchen und installieren.

    ![Hinzufügen von Services NuGet-Paket][vs-services-nuget-package]

5. Erstellen Sie die Klassenbibliothek eine Schnittstelle mit einer einzelnen Methode `GetCountAsync`, und die Schnittstelle von IService.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>Implementieren Sie die Schnittstelle in der statusbehaftete Dienst

Die Schnittstelle definiert wurde, müssen wir in der statusbehaftete Dienst implementieren.

1. Die statusbehaftete Dienst fügen Sie einen Verweis auf das Klassenbibliotheksprojekt, das die Schnittstelle enthält.

    ![Einen Verweis auf das Klassenbibliotheksprojekt hinzufügen statusbehaftete Dienst][vs-add-class-library-reference]

2. Suchen Sie die Klasse von erbt `StatefulService`, wie `MyStatefulService`, und implementiert die `ICounter` Schnittstelle.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Nun implementieren die Methode, die in definiert die `ICounter` -Schnittstelle, `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Setzen des statusbehafteten Dienstes mit Service Remoting listener

Mit der `ICounter` implementierte Schnittstelle im letzten Schritt der statusbehaftete Dienst von anderen Diensten aufgerufen werden Kommunikationskanal geöffnet ist. Statusbehaftete Dienste Fabric Service bietet eine overridable-Methode aufgerufen `CreateServiceReplicaListeners`. Mit dieser Methode können Sie einen oder mehrere kommunikationslistener basierend auf dem Typ der Kommunikation mit dem Dienst aktivieren möchten.

>[AZURE.NOTE] Die entsprechende Methode zum Öffnen der Kommunikationskanal zum zustandsloser Dienste heißt `CreateServiceInstanceListeners`.

Wir werden in diesem Fall ersetzen Sie die vorhandene `CreateServiceReplicaListeners` Methode und eine Instanz von `ServiceRemotingListener`, dem RPC-Endpunkt erstellt wird von Clients aufgerufen `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Verwenden Sie die ServiceProxy-Klasse mit dem Dienst interagieren

Unsere statusbehaftete Dienst kann jetzt von anderen Diensten Datenverkehr empfangen. So, noch wird den Code Hinzufügen von ASP.NET Web Service mit ihm kommunizieren.

1. In Ihrem Projekt ASP.NET fügen Sie einen Verweis auf die Klassenbibliothek enthält die `ICounter` Schnittstelle.

2. Öffnen Sie im Menü **Erstellen** der **Konfigurations-Manager**. Folgendes sollte angezeigt werden:

    ![Konfigurations-Manager zeigt Klassenbibliothek als AnyCPU][vs-configuration-manager]

    Beachten Sie, dass das Klassenbibliotheksprojekt **MyStatefulService.Interface**erstellen für beliebige CPU konfiguriert. Mit Dienst ordnungsgemäß funktioniert, müssen sie explizit auf X64 ausgerichtet werden. Dropdownliste Plattform **neu**klicken und erstellen eine X64 Plattformkonfiguration.

    ![Neue Plattform für die Klassenbibliothek][vs-create-platform]

3. Hinzufügen des Microsoft.ServiceFabric.Services-Pakets Projekt ASP.NET wie für das Klassenbibliotheksprojekt oben Wird die `ServiceProxy` Klasse.

4. Öffnen Sie im Ordner **Controller** die `ValuesController` Klasse. Beachten Sie, dass die `Get` -Methode derzeit nur eine fest programmierte Zeichenfolgenarray "Wert1" und "Wert2"--dem entspricht, was wir zuvor im Browser gesehen zurück. Ersetzen Sie diese Implementierung durch folgenden Code:

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    Die erste Codezeile ist der Schlüssel. Zum Erstellen des ICounter-Proxys statusbehaftete Dienst müssen Sie zwei Informationen bereitzustellen: Partitions-ID und den Namen des Dienstes.

    Partitionierung Skalierung statusbehaftete Dienste durch aufspalten Zustand in verschiedenen Buckets anhand eines Schlüssels oder eine Kundennummer Postleitzahl definieren können. In unserer Anwendung einfach haben statusbehaftete Dienst nur eine Partition der Schlüssel keine Rolle spielt. Eine beliebige Taste, die Sie bereitstellen führt auf dieselbe Partition. Weitere Informationen zum Partitionieren von Diensten finden Sie unter [Partition zuverlässige Fabric-Dienst](service-fabric-concepts-partitioning.md).

    Der Dienstname ist ein URI Formular Fabric: /&lt;Application_name&gt;/&lt;Dienstname&gt;.

    Mit diesen zwei Informationen können Service Fabric den Computer eindeutig, dem Anfragen gesendet werden soll. Die `ServiceProxy` nahtlos Menüklasse Wenn Computer, die statusbehaftete Dienst Partition hostet fehlschlägt und einem anderen Computer muss gefördert werden. Diese Abstraktion wird den Client Code mit anderen Diensten erheblich einfacher.

    Haben wir den Proxy, rufen wir einfach die `GetCountAsync` -Methode und das Ergebnis zurückgibt.

5. Drücken Sie erneut F5, damit die geänderte Anwendung ausgeführt. Als startet, Visual Studio-Browser in das Stammverzeichnis des Webprojekts automatisch. Fügen Sie den Pfad "api/Values" und sieht der aktuelle Indikatorwert zurückgegeben.

    ![Die statusbehaftete Indikatorwert im Browser angezeigt][browser-aspnet-counter-value]

    Aktualisieren Sie den Browser, um den Zählerwert aktualisieren finden Sie unter.


>[AZURE.WARNING] ASP.NET Core ist gemäß der Vorlage als Kestrel, [für die Behandlung von direkten Internetverkehr derzeit nicht unterstützt](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). Produktionsszenarien sollten hosting ASP.NET Core Endpunkte hinter [API Management] [ api-management-landing-page] oder ein anderes Gateway Internetzugriff. Beachten Sie, dass Service Fabric Bereitstellung innerhalb von IIS nicht unterstützt wird.


## <a name="what-about-actors"></a>Was Akteure?

In diesem Lernprogramm konzentriert hinzufügen ein Web-front-End, die statusbehaftete Dienst kommuniziert. Folgen Sie jedoch ein sehr ähnliches Modell Akteure kommunizieren. Tatsächlich ist es etwas einfacher.

Bei einem Akteur-Projekt erstellen, generiert Visual Studio automatisch ein Projekt. Schnittstelle können Sie um einen Akteur Proxy im Webprojekt mit dem zu generieren. Der Kommunikationskanal wird automatisch bereitgestellt. So müssen Sie nicht tun, was zur Gründung entspricht einem `ServiceRemotingListener` wie für statusbehaftete Dienst in diesem Lernprogramm.

## <a name="how-web-services-work-on-your-local-cluster"></a>Funktionsweise von Webdiensten auf dem lokalen cluster

Im Allgemeinen können Sie genau dieselbe Service Fabric in bereitzustellen ein Cluster mit mehreren Computern, den auf dem lokalen Cluster bereitgestellt und sehr sicher sein, dass Sie erwartungsgemäß funktionieren. Ist der lokale Cluster einfach eine Konfiguration mit fünf Knoten ist, die auf einem einzigen Computer reduziert.

Geht es um Webdienste, ist es jedoch eine wichtige Nuance. Wenn Cluster wie in Azure hinter einem Lastenausgleich befindet, müssen Sie sicherstellen, die Webdienste auf jedem Computer ausgebracht werden, da Lastenausgleich wird einfach Roundrobin-Datenverkehr auf dem Computer. Sie können dazu die `InstanceCount` für den Dienst auf den speziellen Wert "-1".

Im Gegensatz dazu müssen Sie einen Webdienst lokal ausführen, um sicherzustellen, dass nur eine Instanz der Dienst ausgeführt wird. Andernfalls laufen Sie in Konflikte aus mehreren Prozessen, die auf den gleichen Pfad und Port überwachen. Daher sollte die Web Service-Instanzenzahl auf "1" für die lokale Bereitstellung festgelegt werden.

Verschiedene Werte für verschiedene Umgebung konfigurieren finden Sie unter [Verwalten von Anwendungsparameter für mehrere Unternehmen](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie einen Cluster in Azure zum Bereitstellen der Anwendung in die cloud](service-fabric-cluster-creation-via-portal.md)
- [Erfahren Sie mehr über die Kommunikation mit Diensten](service-fabric-connect-and-communicate-with-services.md)
- [Erfahren Sie mehr über Partitionierung statusbehaftete Dienste](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
