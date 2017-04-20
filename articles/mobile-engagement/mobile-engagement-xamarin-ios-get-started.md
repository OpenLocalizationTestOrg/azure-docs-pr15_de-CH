<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Xamarin.iOS"
    description="Informationen Sie zum Azure Mobile Engagement für Xamarin.iOS Apps mit Analysen und Pushbenachrichtigungen verwenden."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Erste Schritte mit Azure Mobile Engagement für Xamarin.iOS Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird veranschaulicht, wie mit Azure Mobile Engagement kennen Ihre app-Nutzung und Pushbenachrichtigungen an segmentierte Xamarin.iOS Anwendung.
In diesem Lernprogramm erstellen Sie eine leere Xamarin.iOS-app, die sammelt Daten und empfängt Pushbenachrichtigungen Apple Push Notification System (APN) verwenden.

In diesem Lernprogramm ist Folgendes erforderlich:

+ [Xamarin Studio](http://xamarin.com/studio). Sie können auch Visual Studio mit Xamarin, aber dieses Lernprogramms Xamarin Studio. Installationshinweise finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>Mobile Engagement für iOS-app einrichten

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Mobile Engagement Backend Ihrer app herstellen

In diesem Lernprogramm wird eine grundlegende "Integration" ist die erforderlichen Daten sammeln und eine Push-Benachrichtigung zu senden.

Wir erstellen eine einfache Anwendung mit Xamarin die Integration zu veranschaulichen:

###<a name="create-a-new-xamarinios-project"></a>Erstellen eines neuen Projekts Xamarin.iOS

1. Starten Sie Xamarin Studio. Klicken Sie auf **Datei** -> **neue** -> **Lösung** 

    ![][1]

2. Wählen Sie **Einzelne Ansicht App**, stellen Sie sicher, dass die Sprache **C#** und klicken Sie auf **Weiter**.

    ![][2]

3. Der **Anwendungsname** und die **Organisations-ID** , und klicken Sie auf **Weiter**. 

    ![][3]

    > [AZURE.IMPORTANT] Stellen Sie sicher mit Präsentationsprofil verwendeten schließlich iOS-app bereitstellen eine App-ID die entspricht genau mit der Paket-ID hier haben. 

4. Aktualisieren Sie das **Projekt**, **Lösung** und **Speicherort** , falls erforderlich, und klicken Sie auf **Erstellen**.

    ![][4]
 
Xamarin Studio erstellt demoApp, in der wir Mobile Engagement integrieren. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Mobile Engagement Backend Ihrer app herstellen

1. Rechts klicken Sie **Pakete** in die Lösung Windows und wählen **Pakete hinzufügen aus**

    ![][5]

2. **Microsoft Azure Mobile Engagement Xamarin SDK** gesucht und der Projektmappe hinzufügen.  

    ![][6]
   
3. Öffnen Sie **AppDelegate.cs** und fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Engagement.Xamarin;

4. Fügen Sie in der Methode **FinishedLaunching** Folgendes ein, um die Verbindung mit Mobile Engagement-Backend zu initialisieren. Stellen Sie Ihre **ConnectionString**hinzu. Dieser Code verwendet außerdem eine dummy- **NotificationIcon** von Mobile Engagement SDK hinzugefügt wird, die Sie ersetzen möchten. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Aktivieren der Überwachung in Echtzeit

Starten Sie Daten senden und sicherstellen, dass der Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm Mobile Engagement Backend senden.

1. Öffnen Sie **ViewController.cs** und fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Engagement.Xamarin;

2. Ersetzen Sie die Klasse, von der `ViewController` erbt von `UIViewController` , `EngagementViewController`. 

##<a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-Benachrichtigung und messaging-Anwendung aktivieren

Mobile Engagement ermöglicht die Interaktion mit den Benutzern und app im Rahmen von Kampagnen messaging mit Push-Benachrichtigung. Dieses Modul heißt REICHWEITE im Mobile Engagement-Portal.
In den folgenden Abschnitten richten Sie Ihre app erhalten.

### <a name="modify-your-application-delegate"></a>Ändern Sie Ihre Anwendung Stellvertretung

1. Öffnen Sie **AppDelegate.cs** und fügen Sie die folgende Anweisung verwenden:

        using System; 

2. Jetzt in die `FinishedLaunching` -Methode fügen die Push-Nachrichten nach dem Registrieren`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Finally - aktualisieren Sie, oder fügen Sie die folgenden Methoden:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. Bestätigen Sie in der **Info.plist** -Datei in der Projektmappe, dass die **Paket-ID** mit der **App-ID** entspricht haben in Ihrem Bereitstellungsprofil in Apple Developer Center. 

    ![][7]

5. **Info.plist** Datei stellen Sie sicher, dass **Hintergrund-Modus aktivieren** und **Remote Benachrichtigung**aktiviert haben. 

    ![][8]

6. Führen Sie die Anwendung auf dem Gerät, das publishing Profil zugeordnet haben. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
