<properties
    pageTitle="Xamarin.Forms app Pushbenachrichtigungen hinzufügen | Microsoft Azure"
    description="Informationen Sie zum Azure Services verwenden Xamarin.Forms apps Multiplattform-Push-Benachrichtigung an."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Xamarin.Forms app Pushbenachrichtigungen hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Übersicht

In dieser praktischen Einführung fügen Sie Push Benachrichtigungen für alle Projekte [Xamarin.Forms quick start](app-service-mobile-xamarin-forms-get-started.md) geführt, sodass eine Push-Benachrichtigung an alle plattformübergreifende Clients gesendet wird, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, benötigen Sie das Erweiterungspaket Push Notification. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Erforderliche Komponenten

* IOS, benötigen Sie eine [Apple Developer Mitgliedschaft](https://developer.apple.com/programs/ios/) und einem physischen iOS da [iOS Simulator Pushbenachrichtigungen nicht unterstützt](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Konfigurieren eines Benachrichtigung Hubs

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualisieren von Project Server zum Senden von Pushbenachrichtigungen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Optional) Konfigurieren und Ausführen von Android-Projekt

Führen Sie in diesem Abschnitt aktivieren Pushbenachrichtigungen für das Projekt Xamarin.Forms Droid für Android.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Aktivieren Sie FB Cloud Messaging (FCM)

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>Konfigurieren Sie das Mobile Anwendung Backend FCM mit Push-Anforderung senden

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Pushbenachrichtigungen Android Projekt hinzufügen

Mit dem Back-End mit FCM konfiguriert können wir Komponenten und Codes an den Client zu registrierenden FCM, für Pushbenachrichtigungen mit Azure Notification Hubs durch Backend-mobile Anwendung registrieren und Benachrichtigungen.

1. Im Projekt **Droid** Maustaste Ordner **Komponenten** **Erhalten mehr Komponenten...**, **Google Cloud Messaging-Client** -Komponente suchen und auf dem Projekt hinzufügen. Diese Komponente unterstützt Pushbenachrichtigungen für Xamarin Android-Projekt.


2. Öffnen Sie die Projektdatei MainActivity.cs und fügen Sie die folgende Anweisung am Anfang der Datei verwenden:

        using Gcm.Client;

3. Fügen Sie folgenden Code der **OnCreate** -Methode nach dem Aufruf von **LoadApplication**:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Fügen Sie eine neue **CreateAndShowDialog** -Hilfsmethode wie folgt:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. Der **MainActivity** -Klasse den folgenden Code hinzufügen:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Dies macht die aktuelle **MainActivity** -Instanz für den Hauptthread der Benutzeroberfläche ausgeführt werden kann.

6. Initialisieren der `instance`Variablen am Anfang der **OnCreate** -Methode wie folgt.

        // Set the current instance of MainActivity.
        instance = this;

2. Angegebene **Droid** -Projekt eine neue Klassendatei hinzufügen `GcmService.cs`, und vergewissern Sie sich **mithilfe von** Aussagen sind am oberen Rand der Datei:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Fügen Sie folgenden Berechtigungsanfragen am oberen Rand der Datei nach der Anweisung **verwenden** und vor **der Namespacedeklaration** .

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. Der Namespace die folgenden Klassendefinition hinzufügen. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]Ersetzen Sie **< PROJECT_NUMBER >** mit Ihrem Projekt zuvor notierten.   

11. Ersetzen Sie leere **GcmService** -Klasse mit der neuen broadcast Receiver verwendet folgenden Code:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. Fügen Sie folgenden Code zum **GcmService** -Klasse überschreibt den **OnRegistered** -Ereignishandler und implementiert eine Methode **Registrieren** .

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Fügen Sie den folgenden Code, **der onMessage**implementiert: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Diese eingehende Benachrichtigungen verarbeitet und an Benachrichtigungssystem angezeigt werden.

14. **GcmServiceBase** müssen Sie die Methoden **OnUnRegistered** und **OnError** -Handler implementieren, Sie wie folgt:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Jetzt sind Sie bereit Test Pushbenachrichtigungen in der app auf einem Android-Gerät oder dem Emulator.

###<a name="test-push-notifications-in-your-android-app"></a>Push-Benachrichtigung in Ihrem Android Test

Die ersten beiden Schritte müssen nur beim Testen auf einem Emulator.

1. Stellen Sie sicher, dass Sie zum Bereitstellen oder auf ein virtuelles Gerät mit Google APIs angestrebt Debuggen, wie folgt Android Virtual Device (AVD)-Manager.

2. Android Gerät von **Apps**auf einem Konto hinzufügen > **Settings** > **Konto hinzufügen**und dann auf verwenden fügen Sie ein vorhandenes Konto zu folgen eine neue erstellen.

1. Wählen Sie in Visual Studio oder Xamarin Studio die **Droid** , und klicken Sie auf **als Startprojekt festlegen**.

2. Schaltfläche **Ausführen** , erstellen Sie das Projekt und die Anwendung auf Ihrem Android-Gerät oder Emulator starten.

3. In der Anwendung geben Sie eine Aufgabe und klicken Sie dann auf das Pluszeichen (**+**) Symbol.

4. Stellen Sie sicher, dass eine Benachrichtigung, wenn ein Element hinzugefügt wird.


##<a name="optional-configure-and-run-the-ios-project"></a>(Optional) Konfigurieren und Ausführen des Projekts iOS

Dieser Abschnitt ist für die Ausführung des Projekts Xamarin iOS für iOS-Geräte. Wenn Sie nicht mit iOS-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>Konfigurieren des Benachrichtigung Hubs für APN

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Als Nächstes konfigurieren Sie Einstellung Projekt iOS Xamarin Studio oder Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>IOS-app Pushbenachrichtigungen hinzufügen

1. Öffnen Sie im Projekt **iOS** AppDelegate.cs die folgende **using** -Anweisung am Anfang der Codedatei hinzufügen.

        using Newtonsoft.Json.Linq;

4. Fügen Sie in der **AppDelegate** -Klasse eine Außerkraftsetzung für das **RegisteredForRemoteNotifications** -Ereignis für Benachrichtigungen registriert:

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. In **AppDelegate**auch fügen Sie die folgende Überschreibung für den **DidReceivedRemoteNotification** -Ereignishandler hinzu:

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Diese Methode behandelt eingehende Benachrichtigungen, während die Anwendung ausgeführt wird.

2. **FinishedLaunching** -Methode in der **AppDelegate** -Klasse fügen Sie den folgenden Code hinzu: 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Dies ermöglicht die Unterstützung für remote-Benachrichtigung und Anfragen push Registrierung.

Die Anwendung wurde aktualisiert, um Pushbenachrichtigungen unterstützen.

####<a name="test-push-notifications-in-your-ios-app"></a>Test Pushbenachrichtigungen in iOS-app

1. Wählen Sie die iOS, und klicken Sie auf **als StartPp**.

2. Drücken Sie die Schaltfläche **Ausführen** oder **F5** in Visual Studio das Projekt und die app in iOS-Gerät und klicken Sie auf **OK** , um Push-Benachrichtigung zu übernehmen.

    > [AZURE.NOTE] Sie müssen explizit Pushbenachrichtigungen aus Ihrer Anwendung akzeptieren. Diese Anforderung tritt nur beim ersten, der die Anwendung ausgeführt wird.

3. In der Anwendung geben Sie eine Aufgabe und klicken Sie dann auf das Pluszeichen (**+**) Symbol.

4. Stellen Sie sicher, dass eine Benachrichtigung empfangen wird, klicken Sie auf **OK** , um die Benachrichtigung zu schließen.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Optional) Konfigurieren und Ausführen von Windows-Projekte

Dieser Abschnitt ist für die Ausführung der Xamarin.Forms WinApp und WinPhone81 für Windows-Geräte. Diese Schritte unterstützen auch Universal Windows Plattform (UWP). Wenn Sie nicht mit Windows arbeiten, können Sie diesen Abschnitt überspringen.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrieren Sie Ihre Windows-Anwendung für Pushbenachrichtigungen WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>Konfigurieren des Benachrichtigung Hubs für WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Push-Benachrichtigung zu Ihrer Windows-Anwendung hinzufügen

1. Öffnen Sie in Visual Studio **App.xaml.cs** in ein Windows-Projekt und fügen Sie die folgende Anweisung **verwenden** .

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Ersetzen Sie `<your_TodoItemManager_portable_class_namespace>` mit Namespace Ihres tragbaren Projekts enthält den `TodoItemManager` Klasse.
 

2. Fügen Sie in "App.Xaml.cs" die folgende **InitNotificationsAsync** -Methode: 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Diese Methode ruft die pushbenachrichtigungskanal und eine Vorlage Vorlage aus Ihrem Notification Haupt-Benachrichtigungen registriert. Eine vorlagenbenachrichtigung, die *MessageParam* unterstützt wird für diesen Client zugestellt.

3. In App.xaml.cs, aktualisieren Sie die Methodendefinition **OnLaunched** Event Handler Hinzufügen der `async` -Modifizierer, fügen Sie die folgende Codezeile am Ende der Methode hinzu: 

        await InitNotificationsAsync();

    Dadurch wird sichergestellt, dass Push Notification Registrierung erstellt oder aktualisiert jedes Mal, wenn die Anwendung gestartet wird. Es ist wichtig, um sicherzustellen, dass WNS Push Channel immer aktiv ist.  

4. Im Projektmappen-Explorer für Visual Studio **Package.appxmanifest** öffnen und auf **Ja** unter **Benachrichtigung** **Spruch kann** festgelegt.

5. Erstellen Sie die Anwendung und stellen Sie sicher, dass keine Fehler auftreten.  Client app sollte für Vorlage Notifizierungen Backend-Mobile-Anwendung registrieren. Wiederholen Sie diesen Abschnitt für alle Windows-Projekt in der Projektmappe.


####<a name="test-push-notifications-in-your-windows-app"></a>Pushbenachrichtigungen in Ihrer Windows-Anwendung testen

1. In Visual Studio rechts auf ein Windows-Projekt, und klicken Sie auf **als Startprojekt festlegen**.

2. Schaltfläche **Ausführen** , erstellen Sie das Projekt und die Anwendung starten.

3. In der Anwendung einen Namen für eine neue Todoitem und klicken Sie dann auf das Pluszeichen (**+**) Symbol hinzufügen.

4. Stellen Sie sicher, dass eine Benachrichtigung empfangen, wenn das Element hinzugefügt wird.

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Push-Benachrichtigung:

* [Push Notification Probleme diagnostizieren](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Es gibt verschiedene Gründe, warum Benachrichtigungen möglicherweise verloren oder Ende nicht auf Geräten. In diesem Thema wird das Analysieren und ermitteln die Ursache der Push-Benachrichtigung Fehler veranschaulicht. 

Beachten Sie auf der folgenden Tutorials:

* [Authentifizierung für Ihre Anwendung hinzufügen](app-service-mobile-xamarin-forms-get-started-users.md)  
Enthält Informationen zum Authentifizieren von Benutzern der Anwendung mit Identitätsanbieter.

* [Offline-Synchronisierung für Ihre Anwendung aktivieren](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Informationen Sie zum offline-Unterstützung mithilfe einer Mobile App-Back-End hinzufügen. Offline synchronisieren können Endbenutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

