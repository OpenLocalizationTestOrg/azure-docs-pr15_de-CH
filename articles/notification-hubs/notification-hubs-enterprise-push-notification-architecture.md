<properties
    pageTitle="Benachrichtigungshubs - Push Unternehmensarchitektur"
    description="Anleitung zur Verwendung von Azure Notification Hubs in einem Unternehmen"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Push-Architekturrichtlinien Enterprise

Unternehmen sind heute schrittweise Verwirklichung Dienste für einen Endbenutzer (extern) oder für die Mitarbeiter (intern). Sie haben vorhandenen Back-End-Systeme er werden Mainframes oder einige LoB-Anwendung die mobile Anwendungsarchitektur integriert werden müssen. Dieses Handbuch wird wie sollten diese Szenarien Lösung empfehlen Integration sprechen.

Eine häufige Anforderung ist zum Senden von Push-Benachrichtigung an die Benutzer über mobile Anwendung beim Eintreten eines Ereignisses an Backend-Systeme. Z.b. eine Bank hat die Bank Banking app auf ihrem iPhone möchte ein Sollbetrag erfolgt über einer bestimmten Menge von ihrem Konto oder einem Intranetszenario, Mitarbeiter von Finanzabteilung eine Budget Genehmigung app auf seinen Windows Phone hat benachrichtigt werden möchte, wenn er eine Genehmigungsanfrage wird, benachrichtigt werden.

Bank- oder Genehmigung dürfte in einem Back-End-System einen Push für die Benutzer initiieren muss durchgeführt werden. Möglicherweise gibt es mehrere solche Backend-Systeme alle aufbauen müssen dieselbe Art von Logik Push durchzuführen, wenn ein Ereignis eine Benachrichtigung ausgelöst. Die Komplexität liegt in verschiedene Backend-Systeme mit einem Tastendruck, können Endbenutzer verschiedene Benachrichtigungen abonniert und sogar möglicherweise mehrere mobile Anwendung z.B. bei Intranet mobiler apps, eine mobile Anwendung mehrere Backend-Systeme Benachrichtigungen möchten, integrieren. Backend-Systeme nicht kennen oder müssen Push Semantik/Technologie, damit eine gemeinsame Lösung traditionell eine Komponente einzuführen, fragt die Backend-Systeme alle Ereignisse ist verantwortlich für die Push-Nachrichten an den Client gesendet.
Hier sprechen wir über eine noch bessere Lösung mit Azure Service Bus - Thema-Abonnement-Modell die Komplexität reduziert und die Lösung skalierbar.

So sieht die allgemeine Lösungsarchitektur (Allgemeine mit mehreren mobiler apps jedoch gleichermaßen wird nur eine mobile Anwendung)

## <a name="architecture"></a>Architektur

![][1]

In diesem Diagramm Architektur ist Azure Service Bus bietet eine Themen/Abonnements Programmiermodell (mehr sie am [Service Bus Pub/Sub Programmierung]). Empfänger in diesem Fall ist das Mobile Backend (normalerweise [Azure Mobile Service], der einen Push auf mobiler apps initiiert) erhält keine Nachrichten direkt aus dem Back-End-Systemen, sondern wir müssen intermediate Abstraktionsschicht von [Azure Service Bus] ermöglicht mobilen Backend von Nachrichten von einem oder mehreren Back-End-Systemen bereitgestellt. Ein Service Bus-Thema muss für jede Backend-Systeme z.B. Konto HR, Finanzen, die im Grunde "Themen" eingeleitet Nachrichten als Push-Benachrichtigung gesendet werden. Backend-Systeme sendet Nachrichten an diesen Themen. Mobile Backend können solche Themen Erstellen eines Service Bus-Abonnements abonnieren. Diese berechtigen mobile Back-End-Benachrichtigung vom entsprechenden Back-End-System. Mobile Backend weiterhin Nachrichten für ihre Abonnements zu überwachen und als eine Nachricht eintrifft, wird wieder und sendet sie als Benachrichtigung benachrichtigungshub. Benachrichtigungshubs dann schließlich übermittelt die Nachricht an die app. Wir haben also die Schlüsselkomponenten zusammengefasst:

1. Back-End-Systemen (LoB-Legacy-Systeme)
    - Service Bus Thema erstellt
    - Nachricht
2. Mobile backend
    - Dauerauftrag erstellt
    - Meldung (von Back-End-System)
    - Sendet eine Benachrichtigung an Kunden (Azure Notification Hub)
3. Mobile Anwendung
    - Empfängt und Benachrichtigung anzeigen

###<a name="benefits"></a>Vorteile:

1. Das entkoppeln (mobile Anwendung/Service Notification Hub) Empfänger und Absender (Back-End-Systeme) kann zusätzliche Back-End-Systeme mit minimalen integriert.
2. Dadurch wird auch das Szenario von mehreren apps Ereignisse aus einem oder mehreren Back-End-Systemen erhalten.  

## <a name="sample"></a>Beispiel:

###<a name="prerequisites"></a>Erforderliche Komponenten
Führen Sie die folgenden Lernprogramme, Konzepte und gemeinsame Entwicklung und Konfigurationsschritte vertraut zu machen:

1. [Programming Service Bus Pub/Sub] - Einzelheiten der Arbeit mit Bus Themen/Servicedaueraufträge deshalb einen Namespace enthält Themen/Abonnements erstellen wie senden und Nachrichten von ihnen erhalten.
2. [Benachrichtigungshubs - Universal Windows Tutorial] - dies erläutert die Windows Store-app einrichten und Verwenden von Notification Hubs registrieren und Benachrichtigungen erhalten.

###<a name="sample-code"></a>Beispielcode

Der vollständige Beispielcode steht unter [Notification Hub Samples]. Es wird in drei Komponenten aufgeteilt:

1. **EnterprisePushBackendSystem**

    ein. Dieses Projekt verwendet *WindowsAzure.ServiceBus* Nuget-Paket und [Service Bus Pub/Sub Programmierung]basiert.

    b. Dies ist eine einfache C#-Konsole-app, ein LoB-System zu simulieren, leitet die Nachricht an die app.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c. `CreateTopic`wird verwendet, um das Thema Service Bus erstellen, Nachrichten senden wir.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`wird verwendet, um die Nachrichten zu diesem Thema Service Bus. Hier senden wir einfach eine Reihe von zufälligen Nachrichten zum Thema regelmäßig für das Beispiel. Normalerweise werden ein Backend-System Nachrichten senden soll, wenn ein Ereignis eintritt.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    ein. Dieses Projekt verwendet die *WindowsAzure.ServiceBus* und *Microsoft.Web.WebJobs.Publish* Nuget-Pakete und [Service Bus Pub/Sub Programmierung]basiert.

    b. Dies ist eine andere C# Console app wir seit fortlaufend ausführen, um Nachrichten von LoB-Backend-Systeme überwachen einer [Azure Webauftrags] ausgeführt wird. Dadurch wird Ihre mobilen Backend enthalten.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription`Dient zum Erstellen einer Service Bus Abonnement für das Thema, Back-End-System Nachrichten senden. Je nach Szenario Business wird diese Komponente ein oder mehrere Abonnements zu entsprechenden Themen erstellen (z.B. einige können Nachrichten von HR-System von Finanz- und usw. erhalten)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification zum Lesen der Nachricht aus das Abonnement verwenden und wenn der Lesevorgang erfolgreich ist erstellen eine Benachrichtigung (im Beispielszenario eine systemeigene Windows-Popupbenachrichtigung) an die mobilen Anwendung Azure Notification Hubs gesendet werden.

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Klicken Sie für als **Webauftrags**Veröffentlichen mit der rechten Maustaste auf die Projektmappe in Visual Studio, und wählen Sie **als Webauftrags veröffentlichen**

    ![][2]

    f. Wählen Sie Ihr Profil veröffentlichen und eine neue Azure WebSite vorhanden nicht dieser Webauftrag gehostet wird und Sie die WebSite **Veröffentlichen**müssen.

    ![][3]

    g. Konfigurieren Sie das Projekt um "Kontinuierlich ausführen" beim Anmelden bei [Azure-Verwaltungsportal] sollten Sie Folgendes sehen:

    ![][4]


3. **EnterprisePushMobileApp**

    ein. Dies ist eine Windows Store-Anwendung als Teil des mobilen Backend Webauftrags Popupbenachrichtigungen empfangen und anzeigen. Diese [Benachrichtigungshubs - Universal Windows Tutorial]basiert.  

    b. Sicherstellen Sie, dass Ihre Anwendung Popupbenachrichtigungen empfangen aktiviert ist.

    c. Sicherstellen, dass folgende Benachrichtigungshubs Registrierungscode der App aufgerufen wird gestartet (ersetzen Sie *HubName* und *DefaultListenSharedAccessSignature*:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Beispiel ausführen:

1. Sicherstellen Sie, dass Ihre Webauftrag erfolgreich ausgeführt wird und zu"ausführen geplant".
2. Führen Sie **EnterprisePushMobileApp** die Windows Store-app gestartet wird.
3. Führen Sie die **EnterprisePushBackendSystem** -Konsolenanwendung simulieren LoB-Backend und startet das Senden von Nachrichten und sollte Popupbenachrichtigungen wie die folgende angezeigt:

    ![][5]

4. Die Nachrichten wurden ursprünglich zu Themen Service Bus, Service Bus-Abonnements in Ihrem Webauftrag überwacht wurde. Nachdem eine Nachricht empfangen wurde, wurde eine Benachrichtigung erstellt und an die app. Sehen Sie in den Protokollen Webauftrags Verarbeitung bestätigen, wenn Sie auf den Link Logs in [Azure-Verwaltungsportal] für Ihre Web-Projekt wechseln:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Benachrichtigung Hub-Beispiele]: https://github.com/Azure/azure-notificationhubs-samples
[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Servicebus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub/Sub-Programmierung]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure Webauftrag]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Benachrichtigungshubs - Universal Windows-Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure-Verwaltungsportal]: https://manage.windowsazure.com/
