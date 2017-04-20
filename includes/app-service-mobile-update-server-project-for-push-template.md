In diesem Abschnitt Aktualisieren Sie Code im vorhandenen Mobile Apps Backend-Projekt eine Push-Benachrichtigung senden, ein neues Element hinzugefügt wird. Dies erfolgt [Benachrichtigungshubs Vorlagenobjekt plattformübergreifende Push aktivieren](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) . Die verschiedenen Clients für Pushbenachrichtigungen verwenden Vorlagen registriert und universelle Tastendruck für alle Clientplattformen erhalten.

Wählen Sie die Prozedur, die Back-End-Projekttyp entspricht&mdash; [.NET Backend](#dotnet) oder [Node.js-Backend](#nodejs).

### <a name="dotnet"></a>.NET Backend-Projekt
1. In Visual Studio mit der rechten Maustaste des Serverprojekts und klicken Sie auf **NuGet-Pakete verwalten**, Suchen nach `Microsoft.Azure.NotificationHubs`, klicken Sie auf **Installieren**. Benachrichtigungshubs Bibliothek zum Senden von Nachrichten von Backend installiert.

3. Öffnen Sie in Project Server **Controller** > **TodoItemController.cs**, und fügen Sie die folgende using-Anweisung:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. Fügen Sie in der **PostTodoItem** -Methode den folgenden Code nach dem Aufruf von **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Eine vorlagenbenachrichtigung mit dem Element gesendet. Text, wenn ein neues Element eingefügt wird.

4. Veröffentlichen Sie Project Server. 

### <a name="nodejs"></a>Node.js Backend-Projekt

1. Wenn Sie noch nicht geschehen, [Schnellstart Backend-Projekt herunterladen](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) oder [online-Editor in Azure-Portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)verwenden.

2. Ersetzen Sie den vorhandenen Code in todoitem.js durch Folgendes:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
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

    Dies Benachrichtigung eine Vorlage, die die item.text enthält, wenn ein neues Element eingefügt wird.

2. Beim Bearbeiten der Datei auf Ihrem lokalen Computer veröffentlichen Sie das Serverprojekt.
