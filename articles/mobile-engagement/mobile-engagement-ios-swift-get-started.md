<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für iOS in Swift | Microsoft Azure"
    description="Informationen Sie zum Azure Mobile Engagement für iOS-Apps mit Analysen und Pushbenachrichtigungen verwenden."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Erste Schritte mit Azure Mobile Engagement für iOS-Apps in Swift

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird veranschaulicht, wie mit Azure Mobile Engagement kennen Ihre app-Nutzung und Pushbenachrichtigungen an segmentierte einer iOS-Anwendung.
In diesem Lernprogramm erstellen Sie eine leere iOS-app, die sammelt Daten und Pushbenachrichtigungen mit Apple Push Notification System (APN) erhält.

In diesem Lernprogramm ist Folgendes erforderlich:

+ XCode 8 von MAC App Store Installation
+ [Mobile Engagement iOS SDK]
+ Push Notification Zertifikat (p12) Sie erhalten auf der Apple Developer Center

> [AZURE.NOTE] Diese praktische Einführung verwendet Swift Version 3.0. 

Dieses Lernprogramm ist eine Voraussetzung für andere Mobile Engagement Lernprogramme für iOS-apps.

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Mobile Engagement für iOS-app einrichten

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Mobile Engagement Backend Ihrer app herstellen

In diesem Lernprogramm wird eine "einfache Integration", ist minimal erforderlichen Daten sammeln und eine Push-Benachrichtigung zu senden. Die vollständige Integration-Dokumentation finden in [Mobile Engagement iOS SDK-integration](mobile-engagement-ios-sdk-overview.md)

Wir erstellen eine einfache Anwendung mit XCode die Integration zu veranschaulichen:

###<a name="create-a-new-ios-project"></a>Erstellen eines neuen Projekts iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Mobile Engagement Backend Ihrer app herstellen

1. [Mobile Engagement iOS SDK] herunterladen
2. Extrahieren der. tar.gz Datei in einen Ordner auf Ihrem Computer
3. Wählen Sie die, und wählen Sie "Dateien hinzufügen..."

    ![][1]

4. Navigieren Sie zum Ordner SDK und wählen Sie extrahierten die `EngagementSDK` Ordner und drücken Sie OK.

    ![][2]

5. Öffnen der `Build Phases` Registerkarte und die `Link Binary With Libraries` Menü Frameworks wie folgt hinzufügen:

    ![][3]

8. Einen Bridging Header um des SDKS Ziel C APIs verwenden, wählen Sie Datei > Neu > Datei > iOS > Quelle > Headerdatei.

    ![][4]

9. Bearbeiten Sie bridging Headerdatei Mobile Engagement Objective-C Code Swift-Code verfügbar machen, fügen die folgenden Einfuhren:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. Unter Buildeinstellungen Vergewissern Sie Objective-C Bridging Header Buildeinstellung unter Compiler Swift - Code Generation einen Pfad für diesen Header. Hier ist ein Beispiel Pfad: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (je nach Pfad)**

    ![][6]

11. Zurück zum Azure-Portal auf Ihre app- *Verbindungsinformationen* und die Verbindungszeichenfolge kopieren

    ![][5]

12. Nun fügen Sie die Verbindungszeichenfolge in der `didFinishLaunchingWithOptions` delegieren

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Aktivieren der Überwachung in Echtzeit

Starten Sie Daten senden und sicherstellen, dass der Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm (Aktivität) Mobile Engagement Backend senden.

1. Öffnen Sie die Datei **ViewController.swift** , und Ersetzen Sie die Basisklasse von **ViewController** zu **EngagementViewController**:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging-Anwendung

Mobile Engagement können Sie zu Interaktion mit den Benutzern Pushbenachrichtigungen mit Messaging-app im Rahmen von Kampagnen. Dieses Modul heißt REICHWEITE im Mobile Engagement-Portal.
In den folgenden Abschnitten werden Ihre Anwendung zum Empfangen einrichten.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Aktivieren Sie Ihre app automatische Push Benachrichtigungen

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Reach-Bibliothek zum Projekt hinzufügen

1. Klicken Sie mit der rechten Maustaste das Projekt
2. Wählen Sie`Add file to ...`
3. Navigieren Sie zum Ordner SDK entpackten
4. Wählen Sie die `EngagementReach` Ordner
5. Klicken Sie auf Hinzufügen
6. Bearbeiten Sie bridging Headerdatei Header Mobile Engagement Objective-C erreichen und fügen die folgenden Einfuhren:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Ändern Sie Ihre Anwendung Stellvertretung

1. In der `didFinishLaunchingWithOptions` - ein Reach-Modul erstellt und zu Ihrem vorhandenen Engagement Initialisierung übergeben:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Aktivieren Sie Ihre app APN Push Benachrichtigungen
1. Fügen Sie folgende Zeile in die `didFinishLaunchingWithOptions` Methode:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Fügen Sie die `didRegisterForRemoteNotificationsWithDeviceToken` -Methode wie folgt:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Fügen Sie die `didReceiveRemoteNotification:fetchCompletionHandler:` -Methode wie folgt:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
