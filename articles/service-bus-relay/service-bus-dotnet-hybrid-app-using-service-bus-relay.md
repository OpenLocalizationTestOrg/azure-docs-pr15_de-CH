<properties
    pageTitle="Hybride betrieben und Cloud Anwendung (.NET) | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer .NET betrieben und Cloud hybridanwendung Azure Service Bus Relay."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>.NET betrieben und Cloud hybridanwendung Azure Service Bus Relay

## <a name="introduction"></a>Einführung

Dieser Artikel beschreibt das Erstellen einer hybriden Cloudanwendung Microsoft Azure mit Visual Studio. Es wird vorausgesetzt, dass Sie keine Erfahrung mit Azure haben. In weniger als 30 Minuten haben Sie eine Anwendung, die mehrere Azure Ressourcen verwendet und in der Cloud ausgeführt.

Lernen Sie Folgendes:

-   Zum Erstellen oder einen vorhandenen Webdienst für den Verbrauch von einer Lösung anpassen.
-   Wie Sie Azure Service Bus-Dienst Daten zwischen Azure-Anwendung und einem Webdienst gehostet anderswo.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Wie hilft Service Bus Relay mit Hybrid-Lösung

Business Solutions bestehen normalerweise aus einer Kombination von benutzerdefinierten Code gegen neue und einzigartige Business und Funktionalität von Lösungen und Systeme, die bereits bereitgestellt.

Lösungsarchitekten beginnen mit der Cloud für einfachere Skalierung Vorschriften und geringere Betriebskosten. Dabei finden sie, dass Service Ressourcen, die möchten sie nutzen wie Bausteine für deren Lösung innerhalb der Unternehmensfirewall und einfach, auf die Cloud-Lösung erreichen. Viele interne Dienste werden nicht erstellt oder so, dass sie problemlos an das Unternehmensnetzwerk ausgesetzt werden gehostet.

Service Bus Relay ist für den Anwendungsfall mit vorhandener Webdienste in Windows Communication Foundation (WCF) und machen diese Dienste sicher Lösungen, die außerhalb des Unternehmens ohne aufdringlich ändert die Infrastruktur des Unternehmens befinden. Solche Service Bus Relay-Dienste noch in ihre vorhandene Umgebung gehostet, aber sie delegieren überwacht eingehende Sessions und der Cloud gehosteter Service Bus. Service Bus schützt diese Dienste auch vor unbefugtem Zugriff mithilfe [SAS](../service-bus-messaging/service-bus-sas-overview.md) (SAS)-Authentifizierung.

## <a name="solution-scenario"></a>Szenario

In diesem Lernprogramm erstellen Sie eine ASP.NET Website, die Sie eine Liste der Produkte auf Lager Produktseite ermöglicht.

![][0]

Das Lernprogramm wird vorausgesetzt, dass Sie Produktinformationen in einem vorhandenen lokalen System, und Service Bus Relay zu diesem System verwendet. Von einem Webdienst wird simuliert, die in einer einfachen Konsolenanwendung ausgeführt und wird durch ein Speichersatz Produkte. Diese Konsolenanwendung auf Ihrem eigenen Computer ausführen und die Web-Rolle in Azure bereitgestellt werden. Dadurch sehen Sie wie die Web-Rolle in Azure-Rechenzentrum ausgeführt tatsächlich in dem Computer aufrufen, obwohl mindestens eine Firewall und eine Adresse (Netzwerkadressübersetzung)-Netzwerkschicht sicherlich Computers gespeichert wird.

Im folgenden finden ein Bildschirmabbild der Startseite der Website abgeschlossen.

![][1]

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

Bevor Sie können Azure Anwendungsentwicklung, die Tools und Umgebung einrichten.

1.  Installieren Sie Azure SDK für .NET [erhalten Tools und SDK][] -Seite.

2.  Die Version von Visual Studio, die Sie verwenden, klicken Sie auf **das SDK installieren** . Die Schritte in diesem Lernprogramm verwenden Sie Visual Studio 2015.

4.  Oder das Installationsprogramm zu speichern, klicken Sie auf **Ausführen**.

5.  Im **Web Platform Installer**auf **Installieren** und die Installation fortsetzen.

6.  Sobald die Installation abgeschlossen ist, haben Sie alles, was die Anwendung zu starten. Das SDK enthält Tools, mit denen Sie problemlos Azure Applications in Visual Studio entwickeln. Wenn Sie nicht Visual Studio installiert haben, installiert das SDK auch kostenlose Visual Studio Express.

## <a name="create-a-namespace"></a>Erstellen eines Namespaces

Azure Service Bus Features bei zunächst muss einen Dienstnamespace erstellen. Ein Namespace bietet ein Bereichseinheit Service Bus Ressourcen in der Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Erstellen eines lokalen Servers

Erstellen Sie zunächst eine lokale (simulierten) Produktkatalogsystem. Einfaches werden; sehen Sie diese Darstellung einer tatsächlichen lokalen Produktkatalogsystem mit umfassenden Service, das wir integrieren möchten.

Dieses Projekt ist ein Visual Studio-Konsolenanwendungsprojekt und [Azure Service Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) Service Bus Bibliotheken und Konfigurationsdateien verwendet.

### <a name="create-the-project"></a>Erstellen Sie das Projekt

1.  Starten Sie Microsoft Visual Studio mit Administratorrechten. Um Visual Studio mit Administratorberechtigungen starten, mit der rechten Maustaste das Symbol für das **Visual Studio** -Programm und klicken Sie dann auf **als Administrator ausführen**.

2.  In Visual Studio im Menü **Datei** auf **neu**und klicken Sie dann auf **Projekt**.

3.  Wählen Sie **Installierte Vorlagen**unter **Visual C#** **Konsolenanwendungsprojekt**. Geben Sie im Feld **Name** den Namen **ProductsServer**:

    ![][11]

4.  Klicken Sie auf **OK** , um das **ProductsServer** -Projekt zu erstellen.

7.  Wenn NuGet-Paket-Manager für Visual Studio bereits installiert haben, fahren Sie mit dem nächsten Schritt. Andernfalls besuchen Sie [NuGet][] und auf [NuGet installieren](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Folgen Sie den NuGet Package Manager zu installieren und starten Sie Visual Studio neu.

7.  Im Projektmappen-Explorer mit der rechten Maustaste des **ProductsServer** Projekts und dann auf **NuGet-Pakete verwalten**.

8.  Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**und akzeptieren Sie die Vereinbarung.

    ![][13]

    Beachten Sie, dass jetzt das erforderliche Client-Assemblys verwiesen wird.

9.  Fügen Sie eine neue Klasse für den Produkt-Vertrag. Im Projektmappen-Explorer mit der rechten Maustaste des **ProductsServer** Projekts und klicken Sie auf **Hinzufügen**und dann auf **Klasse**.

10. Geben Sie im Feld **Name** den Namen **ProductsContract.cs**. Klicken Sie auf **Hinzufügen**.

11. Ersetzen Sie die Namespacedefinition in **ProductsContract.cs**durch den folgenden Code, der den Vertrag für den Dienst definiert.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. Ersetzen Sie in Program.cs die Namespacedefinition mit dem folgenden Code den Profildienst und Host für sie.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. Doppelklicken Sie im Projektmappen-Explorer auf die Datei **App.config** , um sie in der Visual Studio-Editor öffnen. Am Ende der ** &lt;System. ServiceModel&gt; ** Element (aber noch &lt;System. ServiceModel&gt;), die folgenden XML-Code hinzu. Werden Sie *YourServiceNamespace* mit dem Namen des Namespace und *YourKey* mit SAS-Schlüssel ersetzen, den Sie zuvor aus dem Portal abgerufen:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Weiterhin in App.config, in der ** &lt;AppSettings&gt; ** Element, ersetzen die Verbindungszeichenfolge Wert mit der Verbindungszeichenfolge, die Sie zuvor aus dem Portal erhalten. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Drücken Sie **STRG + UMSCHALT + B** oder klicken Sie im Menü **Erstellen** auf **Projektmappe** erstellen Sie die Anwendung und überprüfen Ihre Arbeit so weit.

## <a name="create-an-aspnet-application"></a>Erstellen einer Anwendung ASP.NET

In diesem Abschnitt erstellen Sie eine einfache ASP.NET Anwendung, die Daten von Ihrem Produkt angezeigt.

### <a name="create-the-project"></a>Erstellen Sie das Projekt

1.  Stellen Sie sicher, dass Visual Studio mit Administratorrechten ausgeführt wird.

2.  In Visual Studio im Menü **Datei** auf **neu**und klicken Sie dann auf **Projekt**.

3.  Wählen Sie **Installierte Vorlagen**unter **Visual C#** **ASP.NET Web Application**. Nennen Sie das Projekt **ProductsPortal**. Klicken Sie auf **OK**.

    ![][15]

4.  Klicken Sie in der Liste **Wählen Sie eine Vorlage** auf **MVC**. 

6.  Kontrollkästchen Sie für **Host in der Cloud**.

    ![][16]

5. Klicken Sie auf **Authentifizierung ändern** . Klicken Sie im Dialogfeld **Authentifizierung ändern** klicken Sie auf **Keine Authentifizierung**, und klicken Sie auf **OK**. Für dieses Lernprogramm bereitstellen Sie eine Anwendung, die ein Benutzerkonto.

    ![][18]

6.  Im Abschnitt **Microsoft Azure** im Dialogfeld **Neues Projekt ASP.NET** sicher, dass **in der Cloud zu hosten** aktiviert ist und **App-Dienst** in der Liste ausgewählt ist.

    ![][19]

7. Klicken Sie auf **OK**. 

8. Jetzt müssen Sie Azure Ressourcen für eine neue Webanwendung konfigurieren. Führen Sie die Schritte im Abschnitt [Konfigurieren Azure Ressourcen für eine neue Webanwendung](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Dann zurück zu diesem Lernprogramm und mit dem nächsten Schritt fort.

5.  Im Projektmappen-Explorer **Modelle** Maustaste dann klicken Sie auf **Hinzufügen**, und klicken Sie auf **Klasse**. Geben Sie im Feld **Name** den Namen **Product.cs**. Klicken Sie auf **Hinzufügen**.

    ![][17]

### <a name="modify-the-web-application"></a>Ändern der Anwendung

1.  Ersetzen Sie in Visual Studio die Datei Product.cs die vorhandenen Namespacedefinition durch folgenden Code.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  Im Projektmappen-Explorer den Ordner **Controller** und doppelklicken Sie auf die Datei **HomeController.cs** in Visual Studio öffnen.

3. Ersetzen Sie vorhandenen Namespace-Definition in **HomeController.cs**durch folgenden Code.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  Im Projektmappen-Explorer den Views\Shared ebenfalls einen Ordner, und doppelklicken Sie dann **_Layout.cshtml** um im Visual Studio-Editor zu öffnen.

5.  Ändern Sie alle Vorkommen von **My ASP.NET Application** **LITWARE**Produkte.

6. Entfernen Sie die ** **Startseite**, **über**und** links Löschen Sie im folgenden Beispiel hervorgehobenen code

    ![][41]

7.  Im Projektmappen-Explorer den Views\Home den Ordner, und doppelklicken Sie auf **Index.cshtml** , um sie in der Visual Studio-Editor öffnen.
    Ersetzen Sie den gesamten Inhalt der Datei durch folgenden Code.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Überprüfen Sie die Richtigkeit Ihrer Arbeit, können Sie **STRG + UMSCHALT + B** zum Erstellen des Projekts drücken.


### <a name="run-the-app-locally"></a>Führen Sie die Anwendung lokal

Ausführen der Anwendung überprüfen, ob es funktioniert.

1.  Stellen Sie sicher, dass **ProductsPortal** das aktive Projekt. Mit der rechten Maustaste im Projektmappen-Explorer des Projektnamen, und wählen Sie **Als Startprojekt festlegen**.
2.  Drücken Sie F5 in Visual Studio.
3.  Die Anwendung sollte angezeigt werden, in einem Browser ausgeführt.

    ![][21]

## <a name="put-the-pieces-together"></a>Teile eines ganzen

Im nächste Schritt wird der lokalen Produkte Server mit ASP.NET Anwendung einbinden.

1.  Wenn es nicht bereits geöffnet, um in Visual Studio erneut **ProductsPortal** im Abschnitt [Erstellen einer Anwendung ASP.NET](#create-an-aspnet-application) erstellte.

2.  Ähnlich wie der Schritt im Abschnitt "Erstellen einer auf lokale Server" die Projektverweise NuGet-Paket hinzufügen. Im Projektmappen-Explorer mit der rechten Maustaste des **ProductsPortal** Projekts und dann auf **NuGet-Pakete verwalten**.

3.  "Service Bus" Suchen Sie, und wählen Sie **Microsoft Azure Service Bus** . Führen Sie die Installation und schließen Sie das Dialogfeld zu.

4.  Im Projektmappen-Explorer mit der rechten Maustaste des **ProductsPortal** Projekts und dann auf **Hinzufügen**und anschließend **Vorhandenes Element**.

5.  Navigieren Sie zur Datei **ProductsContract.cs** aus dem Projekt **ProductsServer** -Konsole. Klicken Sie auf ProductsContract.cs. Klicken Sie auf den Pfeil neben **Hinzufügen**und dann auf **als Verknüpfung hinzufügen**.

    ![][24]

6.  Nun öffnen Sie die Datei **HomeController.cs** im Visual Studio-Editor und Ersetzen Sie die Namespacedefinition durch folgenden Code. Achten Sie darauf, dass *YourServiceNamespace* mit SAS-Schlüssel mit dem Namen Dienstnamespace und *YourKey* ersetzen. Dadurch kann den Client den lokalen Dienst gibt das Ergebnis des Aufrufs aufrufen.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  Im Projektmappen-Explorer mit der rechten Maustaste **ProductsPortal** Lösung (achten auf die Lösung nicht das Projekt). Klicken Sie auf **Hinzufügen**und dann auf **Vorhandenes Projekt**.

8.  Navigieren Sie zu **ProductsServer** -Projekt, und doppelklicken Sie auf die **ProductsServer.csproj** -Lösung hinzufügen.

9.  **ProductsServer** muss ausgeführt werden, um die **ProductsPortal**anzuzeigen. Im Projektmappen-Explorer mit der rechten Maustaste **ProductsPortal** Lösung und klicken Sie auf **Eigenschaften**. Das Dialogfeld **Eigenschaftenseiten** wird angezeigt.

10. Klicken Sie auf der linken Seite auf **Startprojekt**. Klicken Sie auf der rechten Seite auf **mehrere Startprojekte**. Stellen Sie sicher, **ProductsServer** und **ProductsPortal** , in dieser Reihenfolge als Aktion für beide **Starten** angezeigt.

      ![][25]

11. Klicken Sie noch im Dialogfeld **Eigenschaften** auf der linken Seite auf **Project Dependencies** .

12. Klicken Sie in der Liste **Projekte** auf **ProductsServer**. Wird **ProductsPortal** **nicht** aktiviert.

14. Klicken Sie in der Liste **Projekte** auf **ProductsPortal**. Stellen Sie sicher, dass **ProductsServer** aktiviert ist. 

    ![][26]

15. Klicken Sie auf **OK** im Dialogfeld **Eigenschaftenseiten** .

## <a name="run-the-project-locally"></a>Führen Sie das Projekt lokal

Drücken Sie in Visual Studio **F5**, um die Anwendung lokal testen. Die lokale Server (**ProductsServer**) sollte zunächst und **ProductsPortal** Anwendung sollte in einem Browserfenster gestartet. Dieses Mal sehen Sie, dass der Warenbestand vom Product Service lokalen System abgerufene Daten enthält.

![][10]

Drücken Sie auf der Seite **ProductsPortal** **Aktualisieren** . Bei jedem Aktualisieren der Seite, sehen Sie die Serveranwendung eine Meldung angezeigt Wenn `GetProducts()` von **ProductsServer** aufgerufen.

Schließen Sie beide Programme vor dem Fortfahren mit dem nächsten Schritt.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Bereitstellen Sie ProductsPortal einer Azure Web App

Im nächste Schritt wird eine Azure Web app Frontend **ProductsPortal** konvertieren. Bereitstellen Sie **ProductsPortal** Projekt, alle Schritte im Abschnitt [Bereitstellen Webprojekt Azure Web App](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app)zunächst. Nach Abschluss der Bereitstellung zurück zu diesem Lernprogramm und mit dem nächsten Schritt fort.

> [AZURE.NOTE] Fehlermeldung im Browserfenster möglicherweise angezeigt werden, wenn das Webprojekt **ProductsPortal** nach der Bereitstellung automatisch gestartet wird. Dies dürfte und tritt auf, weil die **ProductsServer** Anwendung noch ausgeführt wird.

Kopieren Sie die URL der Anwendung bereitgestellten als URL im nächsten Schritt benötigen. Azure App Serviceaktivität im Fenster in Visual Studio können Sie auch diese URL abrufen:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Set ProductsPortal Web App

Vor dem Ausführen der Anwendungdes in der Cloud, müssen **ProductsPortal** in Visual Studio Web App gestartet wird.

1. In Visual Studio mit der rechten Maustaste des **ProjectsPortal** Projekts und dann auf **Eigenschaften**.

3. **Klicken Sie in der linken Spalte.**

5. Klicken Sie im Bereich **Startaktion** **URL starten** und in das Textfeld Geben Sie die URL für Ihre Anwendung zuvor bereitgestellten; z. B. `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Klicken Sie in Visual Studio im Menü **Datei** auf **Alles speichern**.

7. Klicken Sie in Visual Studio im Menü erstellen auf **Projektmappe neu erstellen**.

## <a name="run-the-application"></a>Führen Sie die Anwendung

2.  Drücken Sie F5, um die Anwendung auszuführen. Die lokalen Server (die Anwendung **ProductsServer** Konsole) sollte zunächst und **ProductsPortal** Anwendung sollte in einem Browserfenster starten, wie im folgenden Screenshot gezeigt. Beachten Sie wieder den Warenbestand enthält Daten vom Product Service lokalen System und zeigt die Daten in der Webanwendung. Überprüfen Sie die URL zu **ProductsPortal** in der Cloud als Azure Web app ausgeführt wird. 

    ![][1]

    > [AZURE.IMPORTANT] **ProductsServer** -Konsolenanwendungsprojekt muss ausgeführt werden und Daten für die Anwendung **ProductsPortal** dienen. Wenn der Browser eine Fehlermeldung angezeigt, warten Sie mehr für **ProductsServer** geladen und die folgende Meldung angezeigt. Im Browser **Aktualisieren** drücken.

    ![][37]

3. Drücken Sie im Browser auf **ProductsPortal** **aktualisiert** . Bei jedem Aktualisieren der Seite, sehen Sie die Serveranwendung eine Meldung angezeigt Wenn `GetProducts()` von **ProductsServer** aufgerufen.

    ![][38]

## <a name="next-steps"></a>Nächste Schritte  

Weitere Informationen zu Service Bus finden Sie unter den folgenden Ressourcen:  

* [Azure Servicebus][sbwacom]  
* [Das Verwenden von Service Bus Warteschlangen][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Tools und SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

