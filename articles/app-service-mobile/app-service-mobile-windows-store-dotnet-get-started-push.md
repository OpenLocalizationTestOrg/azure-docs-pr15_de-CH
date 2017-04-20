<properties
    pageTitle="Universelle Windows-Plattform (UWP) app Pushbenachrichtigungen hinzufügen | Azure mobiler Apps"
    description="Informationen Sie zum Azure App Service Mobile Apps und Azure Notification Hubs senden Pushbenachrichtigungen für Ihre Anwendung Universal Windows Plattform (UWP)."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Push-Benachrichtigung zu Ihrer Windows-Anwendung hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Übersicht

In diesem Lernprogramm fügen Sie Pushbenachrichtigungen Projekt [Windows schnell zu starten](app-service-mobile-windows-store-dotnet-get-started.md) , damit eine Push-Benachrichtigung an das Gerät gesendet wird, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, benötigen Sie das Erweiterungspaket Push Notification. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="configure-hub"></a>Konfigurieren eines Benachrichtigung Hubs

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Registrieren Sie Ihre Anwendung für Pushbenachrichtigungen

Sie möchten senden Ihre app im Windows Store und konfigurieren das Serverprojekt Integration mit Windows Notification Services (WNS) Push senden.

1. Im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste UWP-app-Projekt, klicken Sie auf **Speichern** > **App mit dem Speicher zuordnen**. 

    ![Windows Store app zuordnen](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. Im Assistenten **Weiter**, Ihr Microsoft-Konto melden Sie an, geben Sie einen Namen für Ihre Anwendung in **einer neuen Anwendungsname**, klicken auf **Reservieren**.

3. Nach Anwendung Registrierung erfolgreich erstellt wurde, wählen Sie den Namen der neuen app, klicken Sie auf **Weiter**und dann auf **zuordnen**. Dadurch wird das Anwendungsmanifest erforderliche Windows Store-Registrierungsinformationen hinzugefügt.  

7. Navigieren Sie im [Windows-Entwicklungscenter](https://dev.windows.com/en-us/overview)mit Ihrem Microsoft-Konto anmelden, klicken Sie auf die neue Anwendung Eintragung in **Meine apps**und **Dienste** > **Pushbenachrichtigungen**. 

8. Klicken Sie auf **Pushbenachrichtigungen** **Live Services Website** unter **Microsoft Azure Mobile Services**.

9. Notieren Sie in der Registrierungsseite den Wert unter **Anwendung Geheimnisse** und **Paket-SID**, die anschließend mit der mobilen Anwendung Backend konfigurieren. 

    ![Windows Store app zuordnen](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Die geheimen und Paket SID sind wichtige Anmeldeinformationen. Teilen Sie diese Werte mit weder mit Ihrer Anwendung verteilen. Die **Id der Anwendung** wird mit dem geheimen Schlüssel zum Microsoft Account-Authentifizierung konfigurieren.

##<a name="configure-the-backend-to-send-push-notifications"></a>Konfigurieren der Back-End-Push-Benachrichtigung senden

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Aktualisieren Sie den Server zum Senden von Pushbenachrichtigungen

Verwenden Sie das Verfahren, das Back-End-Projekttyp entspricht&mdash; [.NET Backend](#dotnet) oder [Node.js-Backend](#nodejs).

### <a name="dotnet"></a>.NET Backend-Projekt

1. In Visual Studio mit der rechten Maustaste des Serverprojekts und **NuGet-Pakete verwalten**, Microsoft.Azure.NotificationHubs suchen, klicken auf **Installieren**. Die Clientbibliothek Benachrichtigungshubs installiert.

2. **Blenden Sie**, öffnen Sie TodoItemController.cs und fügen Sie die folgende Anweisung verwenden:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. Fügen Sie in der **PostTodoItem** -Methode den folgenden Code nach dem Aufruf von **InsertAsync**:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Dieser Code weist den benachrichtigungshub eine Push-Benachrichtigung senden, wenn ein neues Element einfügen ist.

4. Veröffentlichen Sie Project Server.

### <a name="nodejs"></a>Node.js Backend-Projekt

1. Wenn Sie noch nicht geschehen, [Schnellstart-Projekt herunterladen](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) oder [online-Editor in Azure-Portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)verwenden.

2. Ersetzen Sie den vorhandenen Code in der Datei todoitem.js durch Folgendes:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    Dies Benachrichtigung eine WNS Spruch, der die item.text enthält ein neues Todo-Element eingefügt wird.

2. Beim Bearbeiten der Datei auf Ihrem lokalen Computer veröffentlichen Sie das Serverprojekt.

##<a id="update-app"></a>Push-Benachrichtigung für Ihre Anwendung hinzufügen

Anschließend muss die app für Pushbenachrichtigungen einschalten registrieren. Wenn Authentifizierung bereits aktiviert haben, stellen Sie sicher, dass der Benutzer Zeichen vor dem Registrieren für Pushbenachrichtigungen.

1. Öffnen Sie die Projektdatei **App.xaml.cs** , und fügen Sie Folgendes `using` Aussagen:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. Der **App** -Klasse in derselben Datei fügen Sie Methodendefinition von **InitNotificationsAsync hinzu** :

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Dieser Code Ruft die ChannelURI für die Anwendung von WNS und Ihre App Service Mobile-App, ChannelURI registriert.

3. Am oberen Rand der **OnLaunched** -Ereignishandler in **"App.Xaml.cs"**die Methodendefinition **Async** -Modifizierer hinzu, und fügen Sie den folgenden Aufruf der neuen **InitNotificationsAsync** -Methode, wie im folgenden Beispiel:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Dies garantiert, dass kurzlebige ChannelURI jedes Mal die Anwendung registriert ist.

4. Erstellen Sie Ihre UWP-app-Projekt neu. Ihre Anwendung kann jetzt Popupbenachrichtigungen erhalten.

##<a id="test"></a>Pushbenachrichtigungen in Ihrer Anwendung testen

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Nächste Schritte

Weitere Informationen über Push-Benachrichtigung:

* [Verwendung den verwalteten Client für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Vorlagen bieten Ihnen Flexibilität plattformübergreifende Push und lokalisierte drückt. Informationen Sie zum Registrieren von Vorlagen.

* [Push Notification Probleme diagnostizieren](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Es gibt verschiedene Gründe, warum Benachrichtigungen möglicherweise verloren oder Ende nicht auf Geräten. In diesem Thema wird das Analysieren und ermitteln die Ursache der Push-Benachrichtigung Fehler veranschaulicht. 

Beachten Sie auf der folgenden Tutorials:

+ [Authentifizierung für Ihre Anwendung hinzufügen](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Enthält Informationen zum Authentifizieren von Benutzern der Anwendung mit Identitätsanbieter.

+ [Offline-Synchronisierung für Ihre Anwendung aktivieren](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Informationen Sie zum offline-Unterstützung mithilfe einer Mobile App-Back-End hinzufügen. Offline synchronisieren können Endbenutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

