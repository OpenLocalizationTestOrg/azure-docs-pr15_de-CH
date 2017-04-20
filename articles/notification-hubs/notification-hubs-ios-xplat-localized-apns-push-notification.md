<properties
    pageTitle="Benachrichtigung Hubs lokalisiert Breaking News Lernprogramm für iOS"
    description="Erfahren Sie, wie Azure Service Bus Notification Hubs mit lokalisierten Breaking News (iOS) Benachrichtigungen."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Verwenden Sie Notification Hubs iOS Geräte lokalisierte Nachrichten an

> [AZURE.SELECTOR]
- [Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>Übersicht

In diesem Thema veranschaulicht die Funktion [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md) von Azure Notification Hubs übertragen Gerät mit Sprache lokalisiert wurden wichtige Nachrichten Meldungen. In diesem Lernprogramm beginnen Sie mit der iOS-app [Mit Notification Hubs zum Senden von Nachrichten]erstellt. Nach Abschluss des Vorgangs wird möglicherweise für Kategorien, die Sie registrieren, eine Sprache, die Benachrichtigungen erhalten und nur Pushbenachrichtigungen für ausgewählten Kategorien in dieser Sprache angeben.


Es gibt zwei Teile dieses Szenarios:

- iOS-app kann Client Geräte an einer Sprache und andere wichtige News-Kategorien abonnieren.

- Back-End sendet die Benachrichtigung mit dem **Tag-** und **Vorlage** Funktionen Azure Notification Hubs.



##<a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen bereits abgeschlossen haben das Lernprogramm [Verwendet Notification Hubs Nachrichten senden] und den Code zur Verfügung, da diese Anleitung direkt auf Code aufbaut.

Visual Studio 2012 oder höher ist optional.



##<a name="template-concepts"></a>Konzepte

[Mit Notification] Hubs Nachrichten senden erstellt Sie eine Anwendung, **Tags** zum Abonnieren von Benachrichtigungen für verschiedene neue Kategorien.
Viele apps jedoch mehrere Märkte und Lokalisierung. Dies bedeutet, dass der Inhalt der Benachrichtigung selbst lokalisiert und an den richtigen Satz an Geräte übermittelt.
In diesem Thema zeigen wir, wie Sie die **Vorlage** Funktion der Benachrichtigung einfach lokalisierte Breaking News Benachrichtigungen.

Hinweis: können lokalisierte Benachrichtigungen werden mehrere Versionen jedes Tag erstellen. Beispielsweise unterstützen Mandarin, Englisch und Französisch, müssten drei verschiedenen Tags für Nachrichten: "World_en", "World_fr" und "World_ch". Wir müssen eine lokalisierte Version der World News einzelnen Tags an. In diesem Thema verwenden wir die Verbreitung von Tags und die Senden mehrerer Nachrichten zu Vorlagen.

Auf einer hohen Ebene sind Vorlagen angeben, wie ein bestimmtes Gerät eine Benachrichtigung erhalten sollen. Die Vorlage gibt das genaue Nutzlast Format Teil der Nachricht Ihre app Backend sind-Eigenschaften auf. In diesem Fall werden wir eine Gebietsschema unabhängig, alle unterstützte Sprachen Nachricht:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Dann stellen wir sicher, dass Geräte mit einer Vorlage auf die richtige Eigenschaft zu registrieren. IOS-app, die für französische registrieren möchte beispielsweise registriert folgende:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Vorlagen sind ein sehr leistungsfähiges Feature, das Sie im Artikel [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md) erfahren können.

##<a name="the-app-user-interface"></a>Der app-Benutzeroberfläche

Wir werden jetzt im Thema [Mit Notification Hubs Nachrichten senden] lokalisierte Vorlagen Nachrichten senden erstellte Breaking News app ändern.


In der MainStoryboard_iPhone.storyboard hinzufügen segmentierten Steuerelement mit drei Sprachen wir unterstützen: Mandarin, Englisch und Französisch.

![][13]

Stellen Sie sicher, ein IBOutlet in der ViewController.h hinzufügen, wie unten dargestellt:

![][14]

##<a name="building-the-ios-app"></a>IOS-app erstellen


1. Fügen Sie die Methode *RetrieveLocale* in der Notification.h Speicher ändern und Abonnieren von Methoden wie folgt:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    Ändern Sie Ihre Notification.m Methode *StoreCategoriesAndSubscribe* Gebietsschemaparameter hinzufügen und Speichern der Benutzer standardmäßig:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    *Subscribe* -Methode auf das Gebietsschema ändern:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Beachten Sie, wir jetzt die Methode *RegisterTemplateWithDeviceToken*statt *RegisterNativeWithDeviceToken*Verwendung. Wenn wir eine Vorlage registrieren müssen wir Json-Vorlage und einen Namen für die Vorlage bereitzustellen (wie unsere Anwendung andere Vorlagen registrieren möchten). Prüfen Sie Ihre Kategorien als Tags registrieren wir sicherstellen, dass Benachrichtigungen für diese Nachrichten empfangen möchten.

    Fügen Sie eine Methode zum Abrufen des Gebietsschemas von Standardeinstellungen für Benutzer:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Jetzt, dass wir unsere Klasse Benachrichtigung geändert, müssen wir sicherstellen, dass unsere ViewController nutzt das neue UISegmentControl. Fügen Sie die folgende Zeile in der *Spiele* -Methode, um sicherzustellen, dass das Gebietsschema an, das derzeit ausgewählt ist:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    Ändern Sie in der Methode *Abonnieren* rufen Sie *StoreCategoriesAndSubscribe* wie folgt:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Schließlich müssen Sie die *DidRegisterForRemoteNotificationsWithDeviceToken* -Methode in der AppDelegate.m, damit Ihre Registrierung ordnungsgemäß aktualisieren können, wenn Ihre Anwendung gestartet wird. Ändern Sie der Aufruf der Methode *Abonnieren* von Benachrichtigungen mit folgenden:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(optional) Lokalisierte Vorlage Benachrichtigung aus .NET Konsole app.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(optional) Lokalisierte Vorlage Benachrichtigung vom Gerät

Wenn Sie nicht berechtigt, Visual Studio oder nur testen lokalisierte Vorlage Benachrichtigung direkt aus der Anwendung auf dem Gerät senden möchten.  Können Sie einfach die lokalisierte Vorlagenparameter Hinzufügen der `SendNotificationRESTAPI` -Methode in der vorherigen praktischen Einführung definiert.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Vorlagen finden Sie unter:

- [Benutzer mit Benachrichtigung benachrichtigen: ASP.NET]
- [Benutzer mit Benachrichtigung benachrichtigen: Mobile Dienste]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Benutzer mit Benachrichtigung benachrichtigen: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Benutzer mit Benachrichtigung benachrichtigen: Mobile Dienste]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
