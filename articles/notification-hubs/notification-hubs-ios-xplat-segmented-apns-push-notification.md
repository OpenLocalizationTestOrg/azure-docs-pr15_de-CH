<properties
    pageTitle="Benachrichtigungshubs Breaking News Tutorial - iOS"
    description="Informationen Sie zum Azure Service Bus Notification Hubs senden wichtige Nachrichten Meldungen iOS-Geräte."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Übersicht

Dieses Thema beschreibt wie Sie Azure Notification Hubs Breaking News Benachrichtigungen iOS-app übertragen. Zum Abschluss werden Sie registrieren für breaking News-Kategorien, die, denen Sie interessieren, und nur Pushbenachrichtigungen für diese Kategorien erhalten. Dieses Szenario ist ein allgemeines Muster für viele apps wo Benachrichtigung an Gruppen von Benutzern, die zuvor interessieren, z. B. RSS-Reader, apps für Musik usw. deklariert haben.

Broadcast-Szenarien werden durch ein oder mehrere _Tags_ einschließlich der Erstellung einer Registrierung im Notification Hub aktiviert. Benachrichtigung zu einem Tag gesendet, erhalten alle Geräte, die Registrierung für das Tag der Benachrichtigung. Da Tags einfach Zeichenfolgen sind, haben sie nicht vorab bereitgestellt werden. Weitere Informationen zu Tags finden Sie in der [Benachrichtigung Hubs und Tag-Ausdrücke](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Erforderliche Komponenten

Dieses Thema baut auf der app in [Erste Schritte mit Notification Hubs]erstellt[get-started]. Bevor Sie dieses Lernprogramm starten, Sie müssen bereits abgeschlossen haben [Erste Schritte mit Notification Hubs][get-started].

##<a name="add-category-selection-to-the-app"></a>Auswahl der app hinzufügen

Der erste Schritt ist vorhandene Storyboards, die dem Benutzer ermöglichen, sich Kategorien auswählen die UI-Elemente hinzufügen. Die vom Benutzer ausgewählten Kategorien werden auf dem Gerät gespeichert. Beim Starten der Anwendung wird eine geräteregistrierung als Tags im Notification Hub mit den ausgewählten Kategorien erstellt.

1. Fügen Sie die folgenden Komponenten in der Objektbibliothek, in der MainStoryboard_iPhone.storyboard:
    + Eine Bezeichnung mit dem Text "Breaking News"
    + Etiketten mit Kategorie "World", "Politik", "Business", "Technologie", "Naturwissenschaften", "Sport"
    + Sechs Switches pro Kategorie, schalten Sie jedes **Status** standardmäßig **deaktiviert** werden.
    + Eine Schaltfläche mit der Bezeichnung "Abonnieren"

    Das Storyboard sollte wie folgt aussehen:

    ![][3]

2. Im Editor Assistent Absatzmöglichkeiten für alle Switches erstellen, und nennen Sie sie "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Erstellen einer Aktion für die Schaltfläche "abonnieren". Die ViewController.h sollte Folgendes enthalten:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Erstellen Sie eine neue **Kakao Touch Klasse** aufgerufen `Notifications`. Kopieren Sie folgenden Code in der Interface-Abschnitt der Datei Notifications.h:

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. Notifications.m der Import-Direktive hinzufügen:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Kopieren Sie folgenden Code im Implementierungsabschnitt der Notifications.m-Datei.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Diese Klasse verwendet die lokalen Speicher zum Speichern und Abrufen von Kategorien von Nachrichten, die dieses Gerät erhalten. Außerdem enthält eine Methode für diese Kategorien mithilfe einer [Vorlage](notification-hubs-templates-cross-platform-push-messages.md) Registrierung registrieren.

7. In der Datei AppDelegate.h hinzufügen eine Import-Anweisung für Notifications.h und Hinzufügen einer Eigenschaft für eine Instanz der Klasse Benachrichtigung:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. Fügen Sie in der Methode **DidFinishLaunchingWithOptions** AppDelegate.m Code zum Initialisieren der Benachrichtigung Instanz am Anfang der Methode.  
 
    `HUBNAME`und `HUBLISTENACCESS` (definiert in hubinfo.h) sollte die `<hub name>` und `<connection string with listen access>` Platzhalter ersetzt die Verbindungszeichenfolge mit Ihrem Hubnamen für *DefaultListenSharedAccessSignature* , die Sie zuvor erworben haben

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Da Anmeldeinformationen, die mit einer Clientanwendung verteilt werden nicht im Allgemeinen sicher sind, sollten Sie nur den Schlüssel auf Listen mit der Clientanwendung verteilen. Access ermöglicht Ihrer Anwendung für Benachrichtigungen, aber vorhandene Registrierung registrieren geändert werden kann, hören und Benachrichtigung nicht gesendet werden. Vollzugriff-Schlüssel wird in einem sicheren Back-End-Dienst für Benachrichtigungen und Ändern der vorhandenen Registrierung verwendet.


9. Ersetzen Sie in der Methode **DidRegisterForRemoteNotificationsWithDeviceToken** AppDelegate.m den Code in der Methode durch den folgenden Code in das gerätetoken Notifications-Klasse übergeben. Registrieren für Benachrichtigungen mit Kategorien führt Notifications-Klasse. Ändert der Benutzer Kategorien, rufen wir die `subscribeWithCategories` -Methode auf die Schaltfläche **Abonnieren** aktualisieren.

    > [AZURE.NOTE] Da gerätetoken zugewiesen von Apple Push Notification Service (APN) jederzeit ändern kann, sollten Sie für Benachrichtigungen häufig zu Fehlern Benachrichtigung registrieren. Dieses Beispiel registriert für Benachrichtigung bei jedem Starten der Anwendung. Für apps, die häufig ausgeführt werden überspringen mehrmals täglich wahrscheinlich Sie Registrierung um Bandbreite zu sparen, wenn Sie länger als ein Tag nach der vorherigen Registrierung verstrichen ist.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Beachten Sie, dass zu diesem Zeitpunkt sollte kein anderer Code in der **DidRegisterForRemoteNotificationsWithDeviceToken** -Methode.

10. Die folgenden Methoden müssen bereits im AppDelegate.m [Erste Schritte mit Notification Hubs] vollständig vorhanden sein[ get-started] Tutorial.  Wenn dies nicht der Fall ist, fügen sie hinzu.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Diese Methode behandelt die Benachrichtigung erhalten, wenn die Anwendung ausgeführt wird, mit einem einfachen **UIAlert**.

11. Fügen Sie in ViewController.m eine Import-Anweisung für AppDelegate.h, und kopieren Sie folgenden Code in XCode generiert **subscribe** -Methode. Dieser Code aktualisiert Notification-Registrierung, um die neuen Kategorietags verwenden, die der Benutzer in der Benutzeroberfläche ausgewählt wurde.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Diese Methode erstellt ein **NSMutableArray** Kategorien und **Notifications** -Klasse zum Speichern der Liste im lokalen Speicher verwendet und die entsprechenden Tags mit der Benachrichtigung registriert. Wenn Kategorien geändert werden, wird die Registrierung mit neuen Kategorien neu erstellt.

12. Fügen Sie in ViewController.m den folgenden Code in der **Spiele** -Methode auf basierte der zuvor gespeicherten Benutzeroberfläche festgelegt.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Die Anwendung kann jetzt speichern Kategorien im lokalen Speicher Geräts mit der Benachrichtigung registriert, wenn die app gestartet wird.  Der Benutzer kann Kategorien zur Laufzeit ändern und klicken Sie auf **Abonnieren** Methode zum Aktualisieren der Registrierung für das Gerät. Anschließend aktualisieren Sie die Anwendung auf die aktuelle News Benachrichtigungen direkt in der Anwendung selbst.


##<a name="optional-sending-tagged-notifications"></a>(optional) Markierte Benachrichtigungen senden

Haben Sie Zugriff auf Visual Studio, können Sie zum nächsten Abschnitt übergehen und Benachrichtigung von der Anwendung selbst. Sie können auch die richtige [Azure-Verwaltungsportal] Benachrichtigung mithilfe der Registerkarte Debuggen Notification-Hub. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(optional) Benachrichtigung vom Gerät

Normalerweise Benachrichtigungen gesendet von einem Back-End-Dienst jedoch wichtige Nachrichten Meldungen können direkt senden. Wollen wir aktualisieren die `SendNotificationRESTAPI` wir in [Erste Schritte mit Notification Hubs] definiert[ get-started] Tutorial.


1. ViewController.m Aktualisierung der `SendNotificationRESTAPI` wie folgt akzeptiert einen Parameter Category-Tag und die richtige [Vorlage](notification-hubs-templates-cross-platform-push-messages.md) Benachrichtigung gesendet.

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

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



2. Aktualisieren Sie in ViewController.m die **Benachrichtigung senden** -Aktion folgenden Code dargestellt. So, dass jeden Tag einzeln mit Benachrichtigung senden an mehrere Plattformen.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Erstellen Sie das Projekt neu und stellen Sie sicher, dass keine Fehler aufgetreten sind.


##<a name="run-the-app-and-generate-notifications"></a>Führen Sie die Anwendung und generieren Benachrichtigungen

1. Drücken Sie auf Ausführen, um das Projekt und die Anwendung starten. Optionen Sie einige wichtige Nachrichten abonnieren und klicken Sie dann auf **Anmelden** . Sehen Sie ein Dialogfeld, dass Benachrichtigungen abonniert haben.

    ![][1]

    **Abonnieren**, die app konvertiert die ausgewählten Kategorien in Tags und fordert eine neue geräteregistrierung für den ausgewählten Tags von Notification Hub.

2. Geben Sie eine Nachricht als Nachrichten gesendet werden, und drücken die **Benachrichtigung senden** -Schaltfläche. Alternativ führen Sie .NET Konsole app Benachrichtigungen generiert.

    ![][2]


3. Aktuelle Nachrichten Meldungen erhalten gerade gesendet jedes Gerät abonniert Nachrichten.



## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir gelernt übertragen von Nachrichten nach Kategorie. Sollten Sie einen der folgenden Lernprogramme, in denen andere erweiterten Benachrichtigungshubs Szenarien vorgestellt:

+ **[Verwenden Sie Notification Hubs lokalisierte Nachrichten übertragen]**

    Informationen Sie zum Erweitern der aktuelle News app senden lokalisierte Notifications zu aktivieren.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Verwenden Sie Notification Hubs lokalisierte Nachrichten übertragen]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure-Verwaltungsportal]: https://manage.windowsazure.com
