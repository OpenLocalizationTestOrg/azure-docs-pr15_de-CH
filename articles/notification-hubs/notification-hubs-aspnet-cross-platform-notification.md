<properties
    pageTitle="Plattformübergreifende Benachrichtigung Benutzern mit Notification Hubs (ASP.NET)"
    description="Erfahren Sie, wie Vorlagen in einer einzelnen Anforderung plattformunabhängigen Benachrichtigung senden, die alle Plattformen Notification Hubs."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Plattformübergreifende Benachrichtigung Benutzern mit Benachrichtigung


Im vorherigen Lernprogramm [Benutzer benachrichtigen Benachrichtigungshubs]gelernt Sie push Notifications für alle Geräte eines bestimmten authentifizierten Benutzers registriert. In diesem Lernprogramm wurden mehrere Anfragen Benachrichtigung Plattform unterstützte Client erforderlich. Benachrichtigung Hubs unterstützt Vorlagen können angeben wie ein bestimmtes Gerät Benachrichtigungen erhalten möchte. Dies vereinfacht die plattformübergreifende Benachrichtigung gesendet. In diesem Thema veranschaulicht, wie Vorlagen in einer einzelnen Anforderung plattformunabhängigen Benachrichtigung senden, die alle Plattformen nutzen. Ausführlichere Informationen finden Sie unter [Übersicht über Azure Notification Hubs][Templates].

> [AZURE.NOTE] Benachrichtigungshubs können einem Gerät mehrere Vorlagen mit demselben Tag registriert. In diesem Fall führt eine eingehende Nachricht auf das Tag mehrere Anträge auf das Gerät, für jede Vorlage bereitgestellt. Dadurch werden mehrere visuelle Benachrichtigung wie Namensschild und als Popupbenachrichtigung in einer Windows Store-app dieselbe Meldung angezeigt.

Gehen Sie folgendermaßen vor um Benachrichtigungen plattformübergreifende Vorlagen:

1. Im Projektmappen-Explorer in Visual Studio erweitern Sie **den Ordner** , und öffnen Sie die Datei RegisterController.cs.

2. Suchen Sie den Codeblock in die **Post** -Methode, eine neue Registrierung ersetzen erstellt, die `switch` mit dem folgenden Code:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Dieser Code Ruft die Methode plattformspezifische eine vorlagenregistrierung statt systemeigene Registrierung erstellen. Vorhandene Registrierung müssen nicht geändert werden, da systemeigene Registrierungen vorlagenregistrierungen abgeleitet.

3. Ersetzen Sie im Controller **Benachrichtigungen** **SendNotification** -Methode durch folgenden Code:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Dieser Code sendet eine Benachrichtigung an alle Plattformen, zur gleichen Zeit und ohne systemeigene Nutzlast anzugeben. Benachrichtigungshubs erstellt und die richtige Nutzlast an jedes Gerät mit dem Wert angegebene _Tag_ gemäß registrierten Vorlagen.

4. Veröffentlichen Sie WebApi Backend-Projekt erneut.

5. Führen Sie die Clientanwendung erneut aus und stellen Sie sicher, dass die Registrierung erfolgreich ist.

6. (Optional) Die Clientanwendung zweiten Gerät bereitstellen und die Anwendung ausführen.

    Beachten Sie, dass eine Benachrichtigung für jedes Gerät angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Hat in diesem Lernprogramm erfahren Sie mehr über Benachrichtigungshubs und Vorlagen in diesen Themen:

+ **[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]** <br/>Zeigt ein weiteres Szenario für die Verwendung von Vorlagen

+  **[Übersicht über Azure Notification Hubs][Templates]**<br/>Übersicht ist auf Vorlagen Informationen.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Benutzer mit Benachrichtigung benachrichtigen]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
