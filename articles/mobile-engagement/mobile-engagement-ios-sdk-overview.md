<properties
    pageTitle="Azure Mobile Engagement iOS SDK Übersicht | Microsoft Azure"
    description="Neueste Updates und Verfahren für iOS SDK für Azure Mobile Engagement"
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK für Azure Mobile Engagement

Hier die Details zum Azure Mobile Engagement in iOS-App integrieren zu beginnen. Möchten Sie es zuerst versuchen, sicherzustellen Sie, dass unsere [15 Minuten Tutorial](mobile-engagement-ios-get-started.md)zu tun.

Klicken Sie, um die [SDK-Inhalt](mobile-engagement-ios-sdk-content.md)

##<a name="integration-procedures"></a>Integrationsverfahren
1. Hier beginnen: [Mobile Engagement in der iOS-app integrieren](mobile-engagement-ios-integrate-engagement.md)

2. Für Benachrichtigungen: [Reichweite (Benachrichtigung) in der iOS-app integrieren](mobile-engagement-ios-integrate-engagement-reach.md)

3. Tag-Umsetzung: [Verwendung der erweiterte Mobile Engagement tagging-API in Ihrer app für iOS](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Versionshinweise

### <a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Feste Benachrichtigung iOS 10 Geräte nicht ausgeführt.
-   XCode 7 setzt.

Frühere Version finden Sie in der [vollständigen Versionsinformationen](mobile-engagement-ios-release-notes.md)

##<a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn bereits eine ältere Version des Projekts in die Anwendung integriert haben, müssen Sie Punkte beim Aktualisieren des SDK.

Sie müssen mehrere Verfahren, wenn mehrere Versionen des SDK Siehe vollständige [Aktualisierung Verfahren](mobile-engagement-ios-upgrade-procedure.md)übersehen.

Für jede neue Version des SDKS Sie ersetzen müssen (entfernen und neu importieren in Xcode) die Ordner EngagementSDK und EngagementReach.

###<a name="from-300-to-400"></a>Von 3.0.0 auf 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 ist ab Version 4.0.0 des SDK erforderlich.

> [AZURE.NOTE] Wenn Sie wirklich XCode 7 abhängig können Sie [iOS v3.2.4 Engagement SDK](https://aka.ms/r6oouh)verwenden. Es ist ein bekanntes Problem des Moduls erreichen dieser vorherigen Version bei unter iOS 10 Geräte: systembenachrichtigungen werden nicht verarbeitet. Zur Behebung dieses Problems Sie veraltete API implementieren müssen `application:didReceiveRemoteNotification:` in Ihrer Anwendung delegieren wie folgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Dies **nicht empfohlen, diese Lösung** können alle anstehenden (auch kleine) iOS Version Upgrade nicht ändern, da diese iOS API veraltet ist. Sie sollten möglichst bald XCode 8 wechseln.

#### <a name="usernotifications-framework"></a>UserNotifications Rahmen
Müssen Sie Hinzufügen der `UserNotifications` Framework in die Phasen erstellen.

Öffnen Sie im Projekt-Explorer Ihr im Projektbereich, und wählen Sie das richtige Ziel. Dann öffnen Sie die Registerkarte **"Phasen erstellen"** und fügen Sie im Menü **"Link Binary mit Libraries"** Framework `UserNotifications.framework` -Verknüpfung als festlegen`Optional`

#### <a name="application-push-capability"></a>Fähigkeit von push
XCode 8 kann Ihre app zurücksetzen Funktion drücken, überprüfen sie der `capability` Registerkarte des ausgewählten Ziel.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Fügen Sie den neuen iOS 10 Registrierung Benachrichtigungscode hinzu
Ältere Codeausschnitt app Benachrichtigungen registriert ist noch verwendet veraltete APIs bei unter iOS 10. 

Importieren der `User Notification` Rahmen:

        #import <UserNotifications/UserNotifications.h>

In Ihrer Anwendung Stellvertretung `application:didFinishLaunchingWithOptions` Methode ersetzen:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

von:

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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Wenn Sie bereits eine eigene Implementierung UNUserNotificationCenterDelegate

Das SDK enthält eine eigene Implementierung des Protokolls UNUserNotificationCenterDelegate. Es wird von SDK zum Lebenszyklus Engagement Benachrichtigungen auf Geräten unter iOS 10 oder höher überwacht. Wenn das SDK Ihre Stellvertretung nicht ihre eigene Implementierung verwenden werden erkennt, denn nur ein UNUserNotificationCenter-Delegat pro Anwendung. Dies bedeutet, dass Sie eigene Delegaten Engagement Logik hinzufügen.

Es gibt zwei Wege.

Durch Ihre Stellvertretung weiterleiten Ruft das SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Oder durch Vererbung von der `AEUserNotificationHandler` Klasse

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Sie können bestimmen, ob eine Benachrichtigung aus oder nicht übergeben wird seine `userInfo` Wörterbuch der Agent `isEngagementPushPayload:` class-Methode.