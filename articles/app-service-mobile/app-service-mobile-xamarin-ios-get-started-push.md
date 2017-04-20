<properties
    pageTitle="Xamarin.iOS app mit Azure App Service Pushbenachrichtigungen hinzufügen"
    description="Informationen Sie zum Azure App Service senden Pushbenachrichtigungen für Ihre Anwendung Xamarin.iOS"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Xamarin.iOS App Pushbenachrichtigungen hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Übersicht

In diesem Lernprogramm hinzugefügt Pushbenachrichtigungen Projekt [Xamarin.iOS schnell starten](app-service-mobile-xamarin-ios-get-started.md) , damit eine Push-Benachrichtigung an das Gerät gesendet wird, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, benötigen Sie das Erweiterungspaket Push Notification. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Erforderliche Komponenten

* Führen Sie das [Xamarin.iOS Schnellstart](app-service-mobile-xamarin-ios-get-started.md) -Lernprogramm.

* Eine physische iOS-Gerät. Push-Benachrichtigung nicht iOS-Simulator unterstützt.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Registrieren Sie die app für Pushbenachrichtigungen Apple Developer Portal

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>Konfigurieren Sie Ihre Mobile App Push-Benachrichtigung senden

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualisieren von Project Server zum Senden von Pushbenachrichtigungen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Konfigurieren Sie das Projekt Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Push-Benachrichtigung für Ihre Anwendung hinzufügen

1. Fügen Sie in **QSTodoService**die folgende Eigenschaft **AppDelegate** des mobilen Clients erhalten können:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Fügen Sie den folgenden `using` -Anweisung an den Anfang der Datei **AppDelegate.cs** .

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. Überschreiben Sie im **AppDelegate**das **FinishedLaunching** -Ereignis:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. Überschreiben Sie in derselben Datei das **RegisteredForRemoteNotifications** -Ereignis. In diesem Code werden Sie für eine einfache vorlagenbenachrichtigung registrieren, die vom Server auf allen unterstützten Plattformen gesendet werden.

    Weitere Informationen zu Vorlagen mit Benachrichtigung finden Sie unter [Vorlagen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Überschreiben Sie das **DidReceivedRemoteNotification** -Ereignis:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Die Anwendung wurde aktualisiert, um Pushbenachrichtigungen unterstützen.

## <a name="test"></a>Pushbenachrichtigungen in Ihrer Anwendung testen

1. Drücken Sie die Schaltfläche **Ausführen** , erstellen Sie das Projekt und die Anwendung in ein Gerät mit iOS und klicken Sie auf **OK** , um Push-Benachrichtigung zu übernehmen.

    > [AZURE.NOTE] Sie müssen explizit Pushbenachrichtigungen aus Ihrer Anwendung akzeptieren. Diese Anforderung tritt nur beim ersten, der die Anwendung ausgeführt wird.

2. In der Anwendung geben Sie eine Aufgabe und klicken Sie dann auf das Pluszeichen (**+**) Symbol.

3. Stellen Sie sicher, dass eine Benachrichtigung empfangen wird, klicken Sie auf **OK** , um die Benachrichtigung zu schließen.

4. Wiederholen Sie Schritt 2 sofort schließen Sie die Anwendung, und stellen Sie sicher, dass eine Benachrichtigung angezeigt wird.

Sie haben dieses Lernprogramm erfolgreich abgeschlossen.

<!-- Images. -->

<!-- URLs. -->



