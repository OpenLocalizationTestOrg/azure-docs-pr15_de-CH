<properties
    pageTitle="Erste Schritte mit Notification Hubs für apps Xamarin.Android | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie, wie Sie Azure Notification Hubs Pushbenachrichtigungen Xamarin Android-Anwendung senden."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Erste Schritte mit Notification Hubs mit Xamarin für Android

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Übersicht

In diesem Lernprogramm wird veranschaulicht, wie mit Azure Notification Hubs Pushbenachrichtigungen Xamarin.Android Anwendung senden.
Erstellen Sie eine leere Xamarin.Android app, die Pushbenachrichtigungen mit Google Cloud Messaging (GCM) erhält. Wenn Sie fertig sind, werden Sie mit Ihrem Haupt-Benachrichtigung Push-Benachrichtigung an alle Ihre App Geräte übertragen. Der fertige Code ist in [NotificationHubs app] [ GitHub] Beispiel.

Dieses Lernprogramm demonstriert die einfache Übertragung Szenario mit Notification Hubs.


## <a name="before-you-begin"></a>Bevor Sie beginnen

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Der vollständige Code für dieses Lernprogramm auf GitHub finden [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm ist Folgendes erforderlich:

+ Visual Studio mit Xamarin unter Windows oder Xamarin Studio auf Mac OS x vollständige Installation sind für [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ Aktive Konto
+ [Azure Messaging Komponente]
+ [Google Cloud Messaging-Clientkomponente]

Dieses Lernprogramm ist eine Voraussetzung für weitere Benachrichtigungshubs Lernprogramme für Xamarin.Android apps.

> [AZURE.IMPORTANT] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##<a name="enable-google-cloud-messaging"></a>Aktivieren Sie Google Cloud Messaging

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>Konfigurieren Sie Ihren Notification hub

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Klicken Sie auf die Registerkarte <b>Konfigurieren</b> oben Geben Sie den <b>API-Schlüssel</b> im vorherigen Abschnitt erhalten und klicken Sie dann auf <b>Speichern</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Der benachrichtigungshub ist jetzt konfiguriert GCM und Sie Verbindungszeichenfolgen auf Ihre app Benachrichtigungen und Push-Benachrichtigungen registriert haben.

##<a name="connect-your-app-to-the-notification-hub"></a>Ihre app Hub Benachrichtigung verbinden

###<a name="create-a-new-project"></a>Erstellen eines neuen Projekts

1. In Xamarin Studio auf **Neue Projektmappe**, klicken Sie auf **Android**und klicken Sie auf **Weiter**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Geben Sie Ihren **App** und **Bezeichner**. Klicken Sie auf der **Ziel-Plattformen** zu unterstützen, und klicken Sie auf **Weiter** und **Erstellen**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Dies erstellt ein neues Android-Projekt.

2. Öffnen der Eigenschaften das neue Projekt in der Projektmappe **Optionen**auswählen. Wählen Sie im Abschnitt **Erstellen** der **Android-Anwendung** .

    Sicherstellen Sie, dass der erste Buchstabe der **Paketname** Kleinbuchstaben.

    > [AZURE.IMPORTANT] Der erste Buchstabe der Paketname muss klein sein. Andernfalls erhalten Sie manifest Anwendungsfehler beim Registrieren der **Broadcastreceivers** und **IntentFilter** für Pushbenachrichtigungen unten.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Legen Sie ggf. **mindestens Android-Version** auf eine andere API.

4. Legen Sie gegebenenfalls **Ziel Android-Version** auf eine andere API-Version, die Sie Ziel (muss API-Ebene 8 oder höher).

Klicken Sie auf **OK** , und schließen Sie das Dialogfeld Projektoptionen.


###<a name="add-the-required-components-to-your-project"></a>Die erforderlichen Komponenten zum Projekt hinzufügen

Die Google Cloud Messaging-Client verfügbar auf der Komponentenspeicher Xamarin vereinfacht das Pushbenachrichtigungen Xamarin.Android unterstützt.

1. Maustaste auf den Ordner Komponenten Xamarin.Android app und **Weitere Komponenten**.

2. **Azure** -Messagingkomponente suchen und dem Projekt hinzufügen.

3. Die **Google Cloud Messaging** -Clientkomponente suchen und dem Projekt hinzufügen.


###<a name="set-up-notification-hubs-in-your-project"></a>Benachrichtigungshubs in Ihrem Projekt einrichten

1. Sammeln Sie die folgende Informationen für die Android app und Benachrichtigung Hub:

    - **GoogleProjectNumber**: dieses Projekt Nutzen aus der Übersicht der app auf Google-Entwicklerportal. Sie notiert, dieser Wert bereits beim Erstellen der Anwendung im Portal.
    - **Verbindungszeichenfolge hören**: Klicken Sie im Schaltpult im [Azure-Verwaltungsportal] **Verbindungszeichenfolgen anzeigen**. Kopieren Sie die *DefaultListenSharedAccessSignature* Verbindung für diesen Wert.
    - **Hub-Name**: Dies ist der Name des Hubs aus dem [Azure-Verwaltungsportal]. Beispielsweise *mynotificationhub2*.

    Erstellen Sie eine **Constants.cs** -Klasse für das Projekt Xamarin und definieren Sie Konstanten Werte in der Klasse. Ersetzen Sie die Platzhalter mit Ihren Werten.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Fügen Sie die folgende using-Anweisung auf **MainActivity.cs**:

        using Android.Util;
        using Gcm.Client;

3. Hinzufügen eine Instanzvariablen, die `MainActivity` Klasse, die eine Warnung anzeigen, wenn die Anwendung ausgeführt wird:

        public static MainActivity instance;


3. Erstellen Sie die folgende Methode in der **MainActivity** -Klasse:

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. In der `OnCreate` Methode **MainActivity.cs**initialisiert das `instance` Variable und fügen Sie einen Aufruf an `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Erstellen Sie eine neue Klasse **MyBroadcastReceiver**.

    > [AZURE.NOTE] Wir gehen durch Erstellen einer **Broadcastreceivers** Klasse von unten. Schnelle Alternative zu manuell erstellen **MyBroadcastReceiver.cs** jedoch auf die Datei **GcmService.cs** im Beispielprojekt Xamarin.Android enthaltene [NotificationHubs Proben][GitHub]. Duplizieren von **GcmService.cs** und Klassennamen ändern kann eine hervorragende ebenso.

5. Fügen Sie die folgende using-Anweisung zu **MyBroadcastReceiver.cs** (auf der Komponente und Baugruppe bereits hinzugefügt):

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. Fügen Sie folgenden Berechtigungsanfragen zwischen die Anweisungen **verwenden** und **die Namespacedeklaration** in **MyBroadcastReceiver.cs**:

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. **MyBroadcastReceiver.cs**Ändern der Klasse **MyBroadcastReceiver** folgt:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Hinzufügen einer anderen Klasse in **MyBroadcastReceiver.cs** mit dem Namen **PushHandlerService**, die von **GcmServiceBase**abgeleitet wird. Stellen Sie sicher das **Service** -Attribut auf die Klasse anwenden:

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** implementiert Methoden **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**und **OnError()**. Unsere **PushHandlerService** -Implementierungsklasse muss diese Methoden überschreiben und diese Methoden auf der Interaktion mit dem benachrichtigungshub ausgelöst.


9. Überschreiben Sie die Methode **OnRegistered()** in **PushHandlerService** mit dem folgenden Code:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] Beachten Sie im obigen **OnRegistered()** Code die Möglichkeit, sich für bestimmte messaging Kanäle angeben.

10. Überschreiben Sie die **OnMessage** -Methode **PushHandlerService** durch den folgenden Code:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. Fügen Sie die folgenden Methoden **CreateNotification** und **DialogNotify** , **PushHandlerService** Benutzer benachrichtigt, wenn eine Benachrichtigung empfangen wird.

    >[AZURE.NOTE] Benachrichtigung Design Android Version 5.0 oder höher stellt eine erhebliche Abweichung von früheren Versionen. Wenn Sie diese Android 5.0 oder höher testen, muss die app ausgeführt werden, damit eine Benachrichtigung. Weitere Informationen finden Sie unter [Android-Benachrichtigungen](http://go.microsoft.com/fwlink/?LinkId=615880).

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Überschreiben Sie abstrakte Member **OnUnRegistered()**, **OnRecoverableError()**und **OnError()** , sodass der Code kompiliert wird:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Führen Sie die Anwendung im emulator

Beim Ausführen dieser Anwendung im Emulator stellen Sie sicher, dass Sie ein Android Virtual Device (AVD) verwenden, Google APIs unterstützt.

> [AZURE.IMPORTANT] Um Push-Benachrichtigung zu erhalten, müssen Sie ein Konto auf Ihrem Android virtuelles Gerät einrichten. (Im Emulator **Settings** und Sie auf **Konto hinzufügen**.) Stellen Sie außerdem sicher, dass der Emulator mit dem Internet verbunden ist.

>[AZURE.NOTE] Benachrichtigung Design Android Version 5.0 oder höher stellt eine erhebliche Abweichung von früheren Versionen. Weitere Informationen finden Sie unter [Android-Benachrichtigungen](http://go.microsoft.com/fwlink/?LinkId=615880).


1. **Tools**auf **Android Geräteemulator-Manager öffnen**, wählen Sie das Gerät, und klicken Sie dann auf **Bearbeiten**.

    ![][18]

2. **Wählen Sie im **Ziel** **Google APIs** **

    ![][19]

3. In der oberen Symbolleiste auf **Ausführen**, und wählen Sie dann die Anwendung. Dies startet den Emulator und die Anwendung führt.

  Die app GCM *RegistrationId* entnimmt und den benachrichtigungshub registriert.

##<a name="send-notifications-from-your-backend"></a>Benachrichtigung von Ihrem Back-End


Sie können testen, Benachrichtigungen in Ihrer Anwendung in [Azure-Verwaltungsportal] über der Registerkarte Debuggen benachrichtigungshub Benachrichtigungen senden wie im folgenden Bildschirm dargestellt.

![][30]


Push-Benachrichtigung werden normalerweise in einem Back-End-Dienst wie Mobile Dienste oder ASP.NET über eine kompatible Bibliothek gesendet. Die REST-API können direkt zum Senden von Nachrichten ist eine Bibliothek nicht für die Back-End.

Nachfolgend einige Lernprogramme, die Sie zum Senden von Benachrichtigungen überprüfen möchten:

- ASP.NET: [Mit Notification Hubs Pushbenachrichtigungen Benutzern]anzeigen
- Azure Notification Hubs Java SDK: finden Sie unter [Notification Hubs aus Java verwenden](notification-hubs-java-push-notification-tutorial.md) zum Senden von Nachrichten von Java. Dies wurde in Eclipse für Android Development getestet.
- PHP: Finden Sie unter [Notification Hubs von PHP verwenden](notification-hubs-php-push-notification-tutorial.md).


In den nächsten Abschnitten des Lernprogramms Benachrichtigung Sie mithilfe einer Anwendung .NET Konsole und Mobilfunkanbieter über ein Skript Knoten.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Optional) Mithilfe von eine senden Sie Benachrichtigung

In diesem Abschnitt werden wir Benachrichtigungen mithilfe einer Konsolenanwendung .NET

1. Erstellen Sie eine neue Visual C#:

    ![][20]

2. Klicken Sie in Visual Studio auf **Extras**, klicken Sie auf **NuGet Paket-Manager**und klicken Sie **Paket-Manager Konsole**.

    Die Konsole Paket-Manager in Visual Studio angezeigt.

3. Im Konsolenfenster Paket-Manager **Project Standard** auf Ihr neues Konsolenanwendungsprojekt festgelegt, und führen Sie folgenden Befehl im Konsolenfenster angezeigt:

        Install-Package Microsoft.Azure.NotificationHubs

    Einen Verweis auf Azure Notification Hubs SDK mit <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-Paket</a>hinzugefügt.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Öffnen Sie die Datei Program.cs, und fügen Sie den folgenden `using` Anweisung:

        using Microsoft.Azure.NotificationHubs;

5. In der `Program` Klasse, fügen Sie die folgende Methode hinzu. Aktualisieren Sie den Platzhaltertext mit Ihrem *DefaultFullSharedAccessSignature* Verbindung und Hub von [Azure-Verwaltungsportal].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Fügen Sie folgenden Zeilen in der **Main** -Methode hinzu:

         SendNotificationAsync();
         Console.ReadLine();

7. Drücken Sie F5, um die Anwendung auszuführen. Sie erhalten eine Benachrichtigung in der app.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Optional) Benachrichtigung mithilfe mobilen service

1. [Erste Schritte mit Mobile Services]folgen

1. Melden Sie sich bei [Azure-Verwaltungsportal]und wählen Sie den mobilen Dienst.

2. Wählen Sie auf der Registerkarte **Planer** .

    ![][22]

3. Einen neuen geplanten Auftrag erstellen, geben Sie einen Namen und wählen **bei Bedarf**.

    ![][23]

4. Wenn der Auftrag erstellt wurde, klicken Sie auf den Auftragsnamen. Klicken Sie auf die Registerkarte **Skript** in der oberen Leiste.

5. Fügen Sie das folgende Skript in der Planer-Funktion. Stellen Sie sicher, die Platzhalter Namen der Hub mit der Verbindungszeichenfolge *DefaultFullSharedAccessSignature* ersetzen, die Sie zuvor erworben haben. Klicken Sie auf **Speichern**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Klicken Sie in der Leiste unten auf **Ausführen** . Sie erhalten eine Popupbenachrichtigung.

##<a name="next-steps"></a>Nächste Schritte

In diesem einfachen Beispiel wird Benachrichtigung an alle Android Geräte übertragen. Um bestimmte Zielgruppe finden Sie das Lernprogramm [Verwendet Notification Hubs Push-Benachrichtigung an Benutzer]. Ggf. Benutzer Interessengruppen segment erhalten Sie [Mit Notification Hubs Nachrichten senden]. Weitere Informationen zum Notification Hubs [Benachrichtigung Hubs Anleitung] und das- [Benachrichtigung Hubs Vorgehensweisen für Android]verwenden.

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Erste Schritte mit Mobile Dienste]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure-Verwaltungsportal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Benachrichtigung Hubs Anleitung]: http://msdn.microsoft.com/library/jj927170.aspx
[Benachrichtigung Hubs Vorgehensweisen für Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Benachrichtigungshubs mit der Push-Benachrichtigung an Benutzer]: /manage/services/notification-hubs/notify-users-aspnet
[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Google Cloud Messaging-Clientkomponente]: http://components.xamarin.com/view/GCMClient/
[Azure Messaging Komponente]: http://components.xamarin.com/view/azure-messaging
