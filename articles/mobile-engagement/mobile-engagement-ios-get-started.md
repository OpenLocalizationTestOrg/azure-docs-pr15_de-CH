<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für iOS in Objective C | Microsoft Azure"
    description="Informationen Sie zum Azure Mobile Engagement für iOS-apps mit Analysen und Push Notifications verwenden."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Erste Schritte mit Azure Mobile Engagement für iOS-apps in Objective C

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird veranschaulicht, wie mit Azure Mobile Engagement kennen Ihre app-Nutzung und Pushbenachrichtigungen an segmentierte einer iOS-Anwendung.
In diesem Lernprogramm erstellen Sie eine leere iOS-app, die sammelt Daten und Pushbenachrichtigungen mit Apple Push Notification System (APN) erhält.

In diesem Lernprogramm ist Folgendes erforderlich:

+ XCode 8 von MAC App Store Installation
+ [Mobile Engagement iOS SDK]

Dieses Lernprogramm ist eine Voraussetzung für andere Mobile Engagement Lernprogramme für iOS-apps.

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Mobile Engagement für iOS-app einrichten

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Mobile Engagement Backend Ihrer app herstellen

In diesem Lernprogramm wird eine "einfache Integration", ist minimal erforderlichen Daten sammeln und eine Push-Benachrichtigung zu senden. Die vollständige Integration-Dokumentation finden in [Mobile Engagement iOS SDK-integration](mobile-engagement-ios-sdk-overview.md)

Wir erstellen eine einfache Anwendung mit XCode die Integration zu veranschaulichen.

###<a name="create-a-new-ios-project"></a>Erstellen eines neuen Projekts iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Mobile Engagement Backend Ihrer app herstellen

1. [Mobile Engagement iOS SDK]herunterladen
2. Extrahieren der. tar.gz Datei in einen Ordner auf Ihrem Computer.
3. Maustaste auf das Projekt und wählen Sie **Dateien zum Hinzufügen**.

    ![][1]

4. Navigieren Sie zum Ordner SDK, wählen Sie extrahierten die `EngagementSDK` Ordner und dann **OK**.

    ![][2]

5. Öffnen Sie die Registerkarte **Phasen erstellen** und im Menü **Binäre mit Verknüpfungsbibliotheken** fügen Sie Frameworks wie folgt hinzu:

    ![][3]

6. Zurück zum Azure-Portal auf Ihre app- **Verbindungsinformationen** und die Verbindungszeichenfolge kopieren.

    ![][4]

7. Fügen Sie die folgende Codezeile in der Datei **AppDelegate.m** .

        #import "EngagementAgent.h"

8. Nun fügen Sie die Verbindungszeichenfolge in der `didFinishLaunchingWithOptions` delegieren.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`Optionale Anweisung SDK Protokolle zur Fehlersuche ermöglicht wird. 

##<a id="monitor"></a>Echtzeit-Überwachung aktivieren

Starten Sie Daten senden und sicherstellen, dass der Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm (Aktivität) Mobile Engagement Backend senden.

1. Öffnen Sie die Datei **ViewController.h** und importieren Sie **EngagementViewController.h**zu:

    `# import "EngagementViewController.h"`

2. Ersetzen Sie nun die übergeordnete Klasse der **ViewController** -Schnittstelle `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-Benachrichtigung und messaging-Anwendung aktivieren

Mobile Engagement ermöglicht die Interaktion mit den Benutzern und app im Rahmen von Kampagnen messaging mit Push-Benachrichtigung. Dieses Modul heißt REICHWEITE im Mobile Engagement-Portal.
In den folgenden Abschnitten richten Sie Ihre app erhalten.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Aktivieren Sie Ihre app automatische Push Benachrichtigungen

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>Reach-Bibliothek zum Projekt hinzufügen

1. Klicken Sie auf das Projekt.
2. Wählen Sie **Datei hinzufügen**.
3. Navigieren Sie zum Ordner SDK entpackten.
4. Wählen Sie die `EngagementReach` Ordner.
5. Klicken Sie auf **Hinzufügen**.

### <a name="modify-your-application-delegate"></a>Ändern Sie Ihre Anwendung Stellvertretung

1. Importieren Sie in **AppDeletegate.m** Datei das Modul Engagement zu erreichen.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. In der `application:didFinishLaunchingWithOptions` , erstellen Sie ein Modul erreichen und zu Ihrer vorhandenen Engagement Initialisierung übergeben:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Aktivieren Sie Ihre app APN Push Benachrichtigungen

1. Fügen Sie folgende Zeile in die `application:didFinishLaunchingWithOptions` Methode:

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Fügen Sie die `application:didRegisterForRemoteNotificationsWithDeviceToken` -Methode wie folgt:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Fügen Sie die `didFailToRegisterForRemoteNotificationsWithError` -Methode wie folgt:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Fügen Sie die `didReceiveRemoteNotification:fetchCompletionHandler` -Methode wie folgt:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

