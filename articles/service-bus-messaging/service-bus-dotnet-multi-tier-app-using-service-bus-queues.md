<properties
    pageTitle="Multi-Tier-Anwendung .NET | Microsoft Azure"
    description="Ein .NET-Lernprogramm, mit der Sie entwickeln eine Anwendung mit mehreren Ebene in Azure Service Bus-Warteschlangen zwischen die Kommunikation von."
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
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>Multi-Tier-Anwendung .NET Azure Service Bus Warteschlangen

## <a name="introduction"></a>Einführung

Entwicklung für Microsoft Azure ist einfach mit Visual Studio und das kostenlose Azure SDK für .NET. Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer Anwendung, die mehrere Azure Ressourcen in der lokalen Umgebung verwendet. Den Schritten wird vorausgesetzt, dass Sie keine Erfahrung mit Azure haben.

Sie lernen Folgendes:

-   Wie Sie Ihren Computer Azure-Entwicklung mit einem einzigen Download und Installation aktivieren.
-   Wie Sie Visual Studio für Azure entwickeln.
-   Erstellen eine Anwendung mit mehreren Ebene in Azure mit Web-und Workerrollen
-   Wie zwischen Ebenen mit Servicebuswarteschlangen kommunizieren.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

In diesem Lernprogramm werden erstellen und Ausführen die Anwendung mit mehreren Ebene in Azure Cloud-Dienst. Front-End wird eine Webrolle ASP.NET MVC und Back-End eine Worker-Rolle, die Service Bus-Warteschlange verwendet. Dieselbe Multi-Tier-Anwendung können mit der als Webprojekt, die in Azure-Website statt einer Cloud-Dienst bereitgestellt wird. Eine Anleitung zur Vorgehensweise anders auf einem front-End Azure-Website finden Sie im Abschnitt [Weiter](#nextsteps) . Sie können [.NET betrieben und Cloud hybridanwendung](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) Lernprogramm ausprobieren.

Der folgende Screenshot zeigt die fertige Anwendung.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Szenario: Kommunikation zwischen Rolle

Um eine Bestellung für die Verarbeitung muss die Front-End-Komponente der Benutzeroberfläche in der Webrolle ausgeführt mit der Worker-Rolle auf mittlerer Ebene interagieren. Dieses Beispiel verwendet Service Bus vermittelte messaging für die Kommunikation zwischen den Ebenen.

Vermittelte messaging zwischen Web- und mittleren Ebenen mit entkoppelt zwei Komponenten. Im Gegensatz zu direkten messaging (TCP oder HTTP) verbindet die Webebene nicht auf der mittleren Ebene direkt; Stattdessen legt es Arbeitseinheiten, wie Nachrichten, Service Bus zuverlässig werden beibehalten bis die mittlere Ebene bereit und verarbeiten.

Service Bus bietet zwei Entitäten vermittelten messaging unterstützen: Warteschlangen und Themen. Warteschlangen wird jede Nachricht an die Warteschlange gesendet von einem einzelnen Empfänger genutzt. Themen unterstützen Veröffentlichen-Abonnieren-Musters in der jede veröffentlichte Nachricht für ein Abonnement registriert das Thema verfügbar ist. Jedes Abonnement verwaltet seine eigene Warteschlange Nachrichten logisch. Abonnements können auch mit Filterregeln konfiguriert werden, die mehrere Nachrichten an die Warteschlange Abonnements, die dem Filter entsprechen einschränken. Im folgende Beispiel wird Servicebuswarteschlangen verwendet.

![][1]

Dieser Kommunikationsmechanismus hat mehrere Vorteile gegenüber direkten messaging:

-   **Zeitliche Entkopplung.** Mit der asynchronen messaging erforderlich Hersteller und Verbraucher nicht online gleichzeitig. Service Bus speichert Nachrichten zuverlässig bis konsumierenden Partei empfangen werden. Dadurch werden die Komponenten einer verteilten Anwendung, entweder, z. B. für Wartung oder aufgrund eines Systemabsturzes Komponente getrennt beeinträchtigt das System als Ganzes. Darüber hinaus müssen die verarbeitende Anwendung nur während bestimmter Zeiten online geschaltet.

-   **Abgleich zu laden.** In vielen variiert Last im Laufe der Zeit, während die Bearbeitungszeit für jede Arbeitseinheit normalerweise konstant ist. Vermittlung Nachricht Erzeugern und Verbrauchern mit einer Warteschlange bedeutet, dass die verarbeitende Anwendung (Arbeitskraft) nur durchschnittliche Auslastung als Belastung angepasst bereitgestellt werden. Die Tiefe der Warteschlange wächst und Verträge, wie eingehende laden. Dies spart direkt in Bezug auf den Umfang der Infrastruktur auf die Anwendungslast.

-   **Lastenausgleich.** Auslastung zunimmt, können mehrere Arbeitsprozesse zum Lesen aus der Warteschlange hinzugefügt werden. Jede Nachricht wird nur von einem der Arbeitsprozesse verarbeitet. Darüber hinaus ermöglicht dieses Pull-basierten Lastenausgleich optimale Nutzung der Arbeitskraft Computer, auch wenn die Arbeitscomputer hinsichtlich Leistung, wie sie Nachrichten mit eigenen maximale Rate ziehen. Dieses Muster wird oft *konkurrierende Consumer* -Muster bezeichnet.

    ![][2]

Den folgenden Abschnitten wird des Codes, der diese Architektur implementiert.

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

Bevor Sie können Azure Anwendungsentwicklung, die Tools und Umgebung einrichten.

1.  Installieren Sie Azure SDK für .NET auf [Tools und SDK][].

2.  Die Version von Visual Studio, die Sie verwenden, klicken Sie auf **das SDK installieren** . Die Schritte in diesem Lernprogramm verwenden Sie Visual Studio 2015.

4.  Oder das Installationsprogramm zu speichern, klicken Sie auf **Ausführen**.

5.  Im **Web Platform Installer**auf **Installieren** und die Installation fortsetzen.

6.  Sobald die Installation abgeschlossen ist, haben Sie alles, was die Anwendung zu starten. Das SDK enthält Tools, mit denen Sie problemlos Azure Applications in Visual Studio entwickeln. Wenn Sie nicht Visual Studio installiert haben, installiert das SDK auch kostenlose Visual Studio Express.

## <a name="create-a-namespace"></a>Erstellen eines Namespaces

Im nächste Schritt wird einen Service-Namespace erstellen und einen freigegebenen Zugriff Signatur (SAS) Schlüssel. Ein Namespace bietet Anwendungsgrenzen für jede Anwendung über Service Bus zur Verfügung gestellt. Eine SAS-Taste wird vom System generiert, wenn ein Namespace erstellt. Die Kombination von Namespace und SAS-Taste stellt die Anmeldeinformationen für Service Bus Zugriff auf eine Anwendung zu authentifizieren.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Erstellen Sie eine Webrolle

In diesem Abschnitt erstellen Sie das front-End der Anwendung. Zuerst erstellen Sie die Seiten in der Anwendung angezeigt.
Danach fügen Sie Code Elemente einer Service Bus-Warteschlange sendet und zeigt Statusinformationen zur Warteschlange hinzu

### <a name="create-the-project"></a>Erstellen Sie das Projekt

1.  Starten Sie Microsoft Visual Studio mit Administratorrechten. Um Visual Studio mit Administratorberechtigungen starten, mit der rechten Maustaste das Symbol für das **Visual Studio** -Programm und klicken Sie dann auf **als Administrator ausführen**. Der Azure-Serveremulator später in diesem Artikel beschriebenen muss Visual Studio mit Administratorrechten gestartet werden.

    In Visual Studio im Menü **Datei** auf **neu**und klicken Sie dann auf **Projekt**.

2.  **Installierte Vorlagen**unter **Visual C#**auf **Cloud** und klicken Sie dann auf **Azure Cloud Service**. Nennen Sie das Projekt **MultiTierApp**. Klicken Sie auf **OK**.

    ![][9]

3.  Doppelklicken Sie in **.NET Framework 4.5** Rollen **ASP.NET Web-Rolle**.

    ![][10]

4.  Mauszeiger **WebRole1** **Azure Cloud Service**-Lösung, klicken Sie auf das Stiftsymbol und Webrolle in **FrontendWebRole**umbenennen. Klicken Sie auf **OK**. (Achten Sie "Frontend" mit einem Kleinbuchstaben "e" nicht "FrontEnd" eingeben.)

    ![][11]

5.  Klicken Sie in der Liste **Wählen Sie eine Vorlage** im Dialogfeld **Neues Projekt ASP.NET** **MVC**.

    ![][12]

6. Klicken Sie auf **Authentifizierung ändern** , noch im Dialogfeld **Neues Projekt von ASP.NET** . Klicken Sie im Dialogfeld **Authentifizierung ändern** klicken Sie auf **Keine Authentifizierung**, und klicken Sie auf **OK**. Für dieses Lernprogramm bereitstellen Sie eine Anwendung, die ein Benutzerkonto.

    ![][16]

7. Klicken Sie auf **OK** , um das Projekt zu erstellen, klicken Sie im Dialogfeld **Neues Projekt von ASP.NET** .

6.  Im **Projektmappen-Explorer**im Projekt **FrontendWebRole** Maustaste auf **Verweise**Sie **NuGet-Pakete verwalten**.

7.  Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**und akzeptieren Sie die Vereinbarung.

    ![][13]

    Beachten Sie, dass jetzt das erforderliche Client-Assemblys verwiesen wird und einige neue Dateien wurden hinzugefügt.

9.  Im **Projektmappen-Explorer**mit der rechten Maustaste **Modelle** und klicken auf **Hinzufügen, **Klasse**.** Geben Sie im Feld **Name** den Namen **OnlineOrder.cs**. Klicken Sie auf **Hinzufügen**.

### <a name="write-the-code-for-your-web-role"></a>Schreiben Sie Code für die Web-Rolle

In diesem Abschnitt erstellen Sie die verschiedenen Seiten der Anwendung angezeigt wird.

1.  Ersetzen Sie in der Datei OnlineOrder.cs in Visual Studio vorhandenen Namespacedefinition durch folgenden Code:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  Doppelklicken Sie im **Projektmappen-Explorer**auf **Controllers\HomeController**. Fügen Sie die folgende Anweisung **verwenden** am Anfang der Datei, die die Namespaces für das Modell enthalten erstellten, sowie Service Bus hinzu

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Ersetzen Sie in der Datei HomeController.cs in Visual Studio auch vorhandenen Namespace-Definition durch folgenden Code. Dieses Codebeispiel enthält Methoden für die Verarbeitung an die Warteschlange zum Einreichen von Elementen.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  Klicken Sie im Menü **Erstellen** auf **Projektmappe** bisher testen die Genauigkeit Ihrer Arbeit.

5.  Erstellen Sie nun die Ansicht für die `Submit()` Methode, die Sie zuvor erstellt haben. Mit der rechten Maustaste in die `Submit()` -Methode (die Überladung von `Submit()` , die keine Parameter akzeptiert), und wählen Sie dann **Ansicht hinzufügen**.

    ![][14]

6.  Dialogfeld zum Erstellen der Ansicht. Wählen Sie in der Liste **Vorlage** **Erstellen**. Klicken Sie in der **Model** auf die **OnlineOrder** -Klasse.

    ![][15]

7.  Klicken Sie auf **Hinzufügen**.

8.  Jetzt ändern Sie den angezeigten Namen der Anwendung. Doppelklicken Sie im **Projektmappen-Explorer**auf die **Views\Shared ebenfalls einen\\_Layout.cshtml** Datei in der Visual Studio-Editor geöffnet.

9.  Ersetzen Sie alle Vorkommen von **My ASP.NET Application** **LITWARE**Produkte.

10. Entfernen Sie die ** **Startseite**, **über**und** links Löschen Sie den hervorgehobenen Code:

    ![][28]

11. Ändern Sie abschließend die Absenden der Seite einige Informationen über die Warteschlange. Doppelklicken Sie im **Projektmappen-Explorer**auf die Datei **Views\Home\Submit.cshtml** im Editor von Visual Studio öffnen. Fügen Sie die folgende Zeile nach `<h2>Submit</h2>`. Jetzt die `ViewBag.MessageCount` ist leer. Sie werden später auffüllen.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. Sie haben jetzt die Benutzeroberfläche implementiert. Drücken Sie **F5** , um die Anwendung auszuführen und sicherzustellen, dass es wie erwartet aussieht.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Schreiben Sie Code für Elemente einer Service Bus-Warteschlange senden

Fügen Sie nun Code für Elemente an eine Warteschlange senden. Zuerst erstellen Sie eine Klasse, die Service Bus Warteschlange Verbindungsinformationen enthält. Initialisieren Sie die Verbindung von Global.aspx.cs. Schließlich aktualisieren den Übermittlung Code zuvor in erstellten HomeController.cs tatsächlich Elemente einer Service Bus-Warteschlange senden.

1.  Klicken Sie im **Projektmappen-Explorer**auf **FrontendWebRole** (rechte Maustaste das Projekt keine Rolle). Klicken Sie auf **Hinzufügen**und dann auf **Klasse**.

2.  Nennen Sie die Klasse **QueueConnector.cs**. Klicken Sie auf **Hinzufügen** , um die Klasse zu erstellen.

3.  Fügen Sie nun Code, kapselt die Verbindungsinformationen und die Verbindung mit einer Warteschlange Service Bus initialisiert. Ersetzen Sie den gesamten Inhalt des QueueConnector.cs durch den folgenden Code, und geben Sie Werte für `your Service Bus namespace` (Namespacenamen) und `yourKey`, also die **Primärschlüssel** von Azure-Portal zuvor abgerufen.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Nun sicherstellen Sie, dass die **Initialize** -Methode aufgerufen wird. Doppelklicken Sie im **Projektmappen-Explorer**auf **Global.asax\Global.asax.cs**.

5.  Fügen Sie die folgende Codezeile am Ende der **Application_Start** -Methode.

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Abschließend aktualisieren Sie Code für das zuvor erstellte um Elemente an die Warteschlange senden. Doppelklicken Sie im **Projektmappen-Explorer**auf **Controllers\HomeController**.

7.  Aktualisierung der `Submit()` Methode (die Überladung, die keine Parameter annimmt) wie folgt zu der Anzahl der Nachrichten für die Warteschlange.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Aktualisierung der `Submit(OnlineOrder order)` Methode (die Überladung, die einen Parameter annimmt) wie folgt, um Informationen an die Warteschlange senden.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Sie können nun die Anwendung erneut ausführen. Jedes Mal eine Bestellung erhöht die Anzahl der Nachrichten.

    ![][18]

## <a name="create-the-worker-role"></a>Worker-Rolle erstellen

Erstellen Sie nun die Worker-Rolle, die Übermittlungen Reihenfolge verarbeitet. Dieses Beispiel verwendet die **Arbeitskraft Rolle mit Service Bus-Warteschlange** Visual Studio-Projektvorlage. Sie erhalten die erforderlichen Anmeldeinformationen bereits vom Portal aus.

1. Stellen Sie sicher, dass Sie Visual Studio Azure-Konto verbunden.

2.  Klicken Sie in Visual Studio im **Projektmappen-Explorer** den Ordner **Rollen** unter dem **MultiTierApp** -Projekt.

3.  Klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neue Arbeitskraft Rolle Projekt**. Das Dialogfeld **Neue Rolle Projekt hinzufügen** wird angezeigt.

    ![][26]

4.  Klicken Sie im Dialogfeld **Neue Rolle Projekt hinzufügen** auf **Worker-Rolle mit Service Bus-Warteschlange**.

    ![][23]

5.  Projektnamen Sie im Feld **Name** im **OrderProcessingRole**. Klicken Sie auf **Hinzufügen**.

6.  Kopieren Sie die Verbindungszeichenfolge, die Sie erhalten in Schritt 9 des Abschnitts "Erstellen einen Servicebus-Namespace" in die Zwischenablage.

7.  Klicken Sie im **Projektmappen-Explorer**in Schritt 5 erstellte **OrderProcessingRole** (unbedingt **OrderProcessingRole** **Rollen**und nicht die Klasse Maustaste). Klicken Sie auf **Eigenschaften**.

8.  Im Dialogfeld **Eigenschaften** auf der Registerkarte **Einstellungen** klicken Sie in das Feld **Wert** für **Microsoft.ServiceBus.ConnectionString**und fügen Sie den Endpunktwert, den Sie in Schritt 6 kopiert.

    ![][25]

9.  Erstellen Sie eine **OnlineOrder** -Klasse, um Aufträge darstellen, wie Sie aus der Warteschlange verarbeiten. Sie können eine Klasse verwenden, die Sie bereits erstellt haben. Im **Projektmappen-Explorer**mit der rechten Maustaste der **OrderProcessingRole** -Klasse (Rechtsklick Klassensymbol keine Rolle). Klicken Sie auf **Hinzufügen**und dann auf **Vorhandenes Element**.

10. Suchen Sie den Unterordner für **FrontendWebRole\Models**und dann auf **OnlineOrder.cs** dieses Projekt hinzufügen.

11. Ändern Sie den Wert der Variablen **QueueName** aus **WorkerRole.cs** `"ProcessingQueue"` , `"OrdersQueue"` wie im folgenden Code gezeigt.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Fügen Sie die folgende Anweisung am Anfang der Datei WorkerRole.cs verwenden.

    ```
    using FrontendWebRole.Models;
    ```

13. In der `Run()` Funktion innerhalb der `OnMessage()` aufrufen, ersetzen Sie den Inhalt der `try` -Klausel mit dem folgenden Code.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Die Anwendung wurde abgeschlossen. Sie können zum Testen der vollständigen Anwendung MultiTierApp Projekt im Projektmappen-Explorer mit der rechten Maustaste, wählen **als Startprojekt festlegen**und F5 drücken. Beachten Sie, dass die Nachrichtenanzahl Worker-Rolle der Warteschlange verarbeitet und als erledigt markiert nicht erhöht. Sie können die Ablaufverfolgungsausgabe Worker-Rolle angezeigt Azure Compute Emulator Benutzeroberfläche. Sie können dazu das Emulator-Symbol im Infobereich der Taskleiste und wählen Sie **Berechnen Emulator-Benutzeroberfläche angezeigt**.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Nächste Schritte  

Weitere Informationen zu Service Bus finden Sie unter den folgenden Ressourcen:  

* [Azure Servicebus][sbmsdn]  
* [Service Bus-Seite][sbwacom]  
* [Das Verwenden von Service Bus Warteschlangen][sbwacomqhowto]  

Mehr über Multi-Tier-Szenarien finden Sie unter:  

* [Multi-Tier-Anwendung .NET mit Tabellen, Warteschlangen und Blobs][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Tools und SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  