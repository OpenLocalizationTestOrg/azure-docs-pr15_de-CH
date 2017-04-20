<properties
    pageTitle="Azure Notification Hubs sichere Push"
    description="Informationen Sie zum Senden von sicheren Pushbenachrichtigungen in Azure. Beispiele in C# mit .NET API."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs sichere Push

> [AZURE.SELECTOR]
- [Universal Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Übersicht

Push-Benachrichtigung-Unterstützung in Microsoft Azure ermöglicht eine einfach zu verwendende für mehrere Plattformen, skalierte Push-Infrastruktur die Implementierung von Pushbenachrichtigungen für Verbraucher und Unternehmen für mobile Plattformen vereinfacht zugreifen.

Durch behördliche oder Einschränkungen, eine Anwendung ggf. etwas in der Benachrichtigung enthalten, die über die standard-Push Notification Infrastruktur übertragen werden können. In diesem Lernprogramm beschrieben genauso erreichen vertraulichen Informationen über eine sichere, authentifizierte Verbindung zwischen dem Clientgerät und Backend-Anwendung.

Auf einer hohen Ebene ist der Fluss wie folgt:

1. App-Back-End:
    - Sichere Nutzlast im Back-End-Datenbank speichert.
    - Sendet die ID dieser Benachrichtigung an das Gerät (keine sichere Informationen gesendet).
2. Die Anwendung auf dem Gerät bei der Benachrichtigung:
    - Das Gerät kontaktiert Back-End-secure Payload anfordern.
    - Die Anwendung kann die Nutzlast als Benachrichtigung an das Gerät anzeigen

Es ist wichtig zu beachten, dass im vorherigen Fluss und in diesem Lernprogramm wird angenommen, dass das Gerät ein Authentifizierungstoken im lokalen Speicher, speichert nachdem der Benutzer anmeldet. Dies garantiert eine vollständig nahtlose Gerät die Benachrichtigung sichere Nutzlast mit diesem Token abrufen kann. Wenn Ihre Anwendung Authentifizierungstoken nicht auf dem Gerät speichern oder diese Token abgelaufen sein, sollte eine generische Meldung Benutzereingabe starten app Gerät nach Erhalt der Benachrichtigung angezeigt werden. Die Anwendung authentifiziert den Benutzer und zeigt die benachrichtigungsnutzlast.

Dieses sichere Push-Lernprogramm zeigt, wie sicher eine Push-Benachrichtigung senden. Das Lernprogramm baut auf das Lernprogramm [Benutzer benachrichtigen](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) , die in diesem Lernprogramm Erste Schritte sollten.

> [AZURE.NOTE] Diese praktische Einführung geht, erstellt und den benachrichtigungshub konfiguriert unter [Erste Schritte mit Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Beachten Sie, dass Windows Phone 8.1 (nicht Windows Phone) Windows-Anmeldeinformationen, und Hintergrundaufgaben in Windows Phone 8.0 oder Silverlight 8.1 nicht funktionieren. Für Windows Store-Apps erhalten Sie per Hintergrund nur, wenn die app Sperrbildschirm aktiviert ist (klicken Sie auf das Kontrollkästchen in der Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Ändern Sie das Windows Phone-Projekt

1. App.xaml.cs Hintergrundtask Push Registrieren des Projekts **NotifyUserWindowsPhone** fügen Sie folgenden Code hinzu. Fügen Sie folgenden Code am Ende der `OnLaunched()` Methode:

        RegisterBackgroundTask();

2. In App.xaml.cs, fügen Sie folgenden code unmittelbar nach der `OnLaunched()` Methode:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei App.xaml.cs:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. Klicken Sie in Visual Studio im Menü **Datei** auf **Alles speichern**.

## <a name="create-the-push-background-component"></a>Push Hintergrundkomponente erstellen

Der nächste Schritt ist die Erstellung Push hintergrundkomponente.

1. Im Projektmappen-Explorer mit der rechten Maustaste den Knotens des obersten Ebene der Lösung (**SecurePush Lösung** in diesem Fall), dann klicken Sie auf **Hinzufügen**, dann klicken Sie auf **Neues Projekt**.

2. Erweitern Sie **Store-Apps**und **Windows Phone Apps**klicken Sie auf **Windows-Runtime-Komponente (Windows Phone)**. Nennen Sie das Projekt **PushBackgroundComponent**, und klicken Sie dann auf **OK** , um das Projekt zu erstellen.

    ![][12]

3. Im Projektmappen-Explorer mit der rechten Maustaste des Projekts **PushBackgroundComponent (Windows Phone 8.1)** und klicken auf **Hinzufügen, **Klasse**.** Name der neuen Klasse **PushBackgroundTask.cs**. Klicken Sie auf **Hinzufügen** , um die Klasse zu generieren.

4. Ersetzt den gesamten Inhalt der Definition der **PushBackgroundComponent** -Namespace durch den folgenden Code ersetzen den Platzhalter `{back-end endpoint}` mit dem Back-End-Endpunkt beim Bereitstellen der Back-End abgerufen:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. Im Projektmappen-Explorer mit der rechten Maustaste **(Windows Phone 8.1) PushBackgroundComponent** Projekt und dann auf **NuGet-Pakete verwalten**.

6. Klicken Sie auf der linken Seite auf **Online**.

7. Geben Sie im **Suchfeld** **HTTP-Client**.

8. In der Ergebnisliste auf **Microsoft HTTP-Clientbibliotheken**und klicken Sie dann auf **Installieren**. Abschließen der Installation.

9. Geben Sie im Feld NuGet **Suche** **Json.net**. Installieren Sie das Paket **Json.NET** und NuGet Paket-Manager schließen.

10. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei **PushBackgroundTask.cs** :

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. Im Projektmappen-Explorer im Projekt **NotifyUserWindowsPhone (Windows Phone 8.1)** Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**. Kontrollkästchen Sie im Dialogfeld Verweis-Manager das neben **PushBackgroundComponent**und klicken Sie dann auf **OK**.

12. Doppelklicken Sie im Projektmappen-Explorer im Projekt **NotifyUserWindowsPhone (Windows Phone 8.1)** auf **Package.appxmanifest** . Unter **Benachrichtigung**auf **Ja**festgelegt **Toast kann** .

    ![][3]

13. In **Package.appxmanifest**Menü **Deklarationen** im oberen Bereich. In der Dropdownliste **Verfügbaren Deklarationen** auf **Hintergrund-Tasks**und klicken Sie auf **Hinzufügen**.

14. **Package.appxmanifest**unter **Eigenschaften**checken Sie **Pushbenachrichtigung ein**.

15. Geben Sie im **Package.appxmanifest**unter **Appeinstellungen** **PushBackgroundComponent.PushBackgroundTask** im Feld **Einstiegspunkt** .

    ![][13]

16. Klicken Sie im Menü **Datei** auf **Alles speichern**.

## <a name="run-the-application"></a>Führen Sie die Anwendung

Führen Sie folgende Schritte aus, um die Anwendung auszuführen:

1. Führen Sie **AppBackend** Web API-Anwendung in Visual Studio. Eine ASP.NET Web angezeigt wird.

2. Führen Sie **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-Anwendung in Visual Studio. Windows Phone-Emulator ausgeführt wird und die Anwendung automatisch geladen.

3. **NotifyUserWindowsPhone** app UI Geben Sie Benutzername und Kennwort ein. Dies können eine Zeichenfolge sein, aber sie müssen dem Wert entsprechen.

4. Klicken Sie in der Anwendung **NotifyUserWindowsPhone** UI auf **anmelden und**. Klicken Sie auf **Push senden**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
