<properties
    pageTitle="Azure Notification Hubs Rich Push"
    description="Erfahren Sie, wie eine iOS-app von Azure rich Pushbenachrichtigungen an. Beispiele in Objective-C und C#."
    documentationCenter="ios"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-rich-push"></a>Azure Notification Hubs Rich Push


##<a name="overview"></a>Übersicht

Um Benutzer mit instant rich engagieren, kann eine Anwendung über nur-Text übertragen möchten. Diese Notifizierungen Förderung Benutzerinteraktionen vorhanden Inhalt wie Urls, Sounds und Bilder-Gutscheine. Dieses Lernprogramm beruht auf dem Thema [Benutzer benachrichtigen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) und zeigt, wie Push-Benachrichtigung zu senden, die Nutzlast (z. B. Bild) integrieren.


Dieses Lernprogramm ist kompatibel mit iOS 7 und 8.

  ![][IOS1]

Hoch:

1. App-Backend:
    - Speichert die umfangreiche Nutzlast (in diesem Fall Bild) in der Backend-Datenbank-lokale Speicherung
    - ID des rich Benachrichtigung an das Gerät sendet
2. Anwendung auf dem Gerät:
    - Wendet die Back-End-rich Nutzlast mit Erhalt ID anfordern
    - Beim Datenabruf abgeschlossen ist, und die Nutzlast zeigt, sobald Benutzer Tippen Sie auf Weitere sendet Benutzer Benachrichtigungen auf dem Gerät


## <a name="webapi-project"></a>WebAPI Projekt

1. Öffnen Sie in Visual Studio das **AppBackend** -Projekt, das im Lernprogramm [Benachrichtigen Benutzer](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) erstellt.
2. Erhalten Sie ein Bild Anwendern, und in einem Ordner **Img** im Projektverzeichnis speichern möchten.
3. Klicken Sie im Projektmappen-Explorer **Alle Dateien anzeigen** und mit der rechten Maustaste des Ordners zum **Projekt hinzufügen**.
4. Ändern Sie das Bild im Eigenschaftenfenster den Buildvorgang in **Eingebettete Ressource**.

    ![][IOS2]

5. **Notifications.cs**, fügen Sie folgende Anweisung verwenden:

        using System.Reflection;

6. Aktualisieren Sie **Benachrichtigungen** Klasse durch den folgenden Code. Achten Sie darauf, dass die Benachrichtigung Hub Anmeldeinformationen und Dateinamen Platzhalter ersetzen.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (optional) Weitere Informationen zum Hinzufügen und Abrufen von Ressourcen finden Sie [einbetten](http://support.microsoft.com/kb/319292) und Zugriff auf Ressourcen mithilfe von Visual C#.

7. Definieren Sie in **NotificationsController.cs** **NotificationsController** mit den folgenden. Diese umfassende automatische Mitteilung Id Gerät sendet und ermöglicht clientseitige Abruf Bild:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Jetzt werden wir app Azure-Website erneut bereitstellen, um von allen Geräten zugänglich machen. Mit der rechten Maustaste auf das Projekt **AppBackend** , und wählen Sie **Veröffentlichen**.

9. Wählen Sie Azure-Website als Ziel veröffentlichen. Ein Azure-Konto Loggen Sie ein und notieren Sie sich die **Ziel-URL** -Eigenschaft der Registerkarte **Verbindung** wählen Sie eine vorhandene oder neue Website. Wir verweisen diese URL wie die *Back-End-Endpunkt* später in diesem Lernprogramm. Klicken Sie auf **Veröffentlichen**.

## <a name="modify-the-ios-project"></a>Ändern Sie das iOS-Projekt

Da Ihre app Backend lediglich die *Id* einer Benachrichtigung senden geändert haben, ändern Sie die iOS-app, Id und umfangreiche Nachricht vom Backend abzurufen.

1. Öffnen Sie das iOS-Projekt und remote Benachrichtigungen auf Haupt-app Ziel im Abschnitt " **Ziele** ".

2. Klicken Sie auf **Funktionen**und **Remote-Benachrichtigung** das Kontrollkästchen aktivieren auf **Hintergrund**.

    ![][IOS3]

3. Gehen Sie zu **Main.storyboard**, und sorgen Sie View Controller (bezeichnet als Home View Controller in diesem Lernprogramm) [Benutzer benachrichtigen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) Tutorial.

4. Storyboard **Navigation Controller** fügen Sie hinzu und Home View Controller zu **Stammansicht** Navigation Steuerelement ziehen. Sicherstellen Sie, dass der **Erste View Controller ist** im Eigenschaften-Inspektor für den Navigationsbereich-Controller aktiviert ist.

5. Fügen Sie einen **View Controller** storyboard und ein **Bild**hinzufügen. Dies ist die Seite, die Benutzern angezeigt wird, sobald sie mehr auf die Notifiication wählen. Das Storyboard sollte wie folgt aussehen:

    ![][IOS4]

6. Klicken Sie auf **Home View Controller** im Storyboard und sicherzustellen Sie, dass es **HomeViewController** als **Benutzerdefinierte Klasse** und **Storyboard-ID** unter der Identität Inspektor.

7. Dasselbe Bild View Controller als **ImageViewController**.

8. Erstellen Sie eine neue View Controller-Klasse mit der Bezeichnung **ImageViewController** , die Benutzeroberfläche behandeln Sie gerade erstellt haben.

9. Fügen Sie folgende **imageViewController.h**auf dem Controller Schnittstellendeklarationen. Unbedingt Steuerelement von der Storyboardansicht Bild auf diese Eigenschaften verbinden ziehen:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. Fügen Sie folgende **imageViewController.m**Ende **Spiele**:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. **AppDelegate.m**importieren Sie erstellten Grafik-Controller:

        #import "imageViewController.h"

12. Fügen Sie einen Abschnitt Schnittstelle mit der folgenden Deklaration:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. **AppDelegate**, vergewissern Sie sich Ihre app registriert für automatische Benachrichtigung in **Anwendung: DidFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Ersetzen in der folgenden Implementierung für **Anwendung: DidRegisterForRemoteNotificationsWithDeviceToken** zu dem Storyboard ändert UI berücksichtigt:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Dann fügen Sie die folgenden Methoden zu **AppDelegate.m** Bild von Ihrem Endpunkt abrufen und lokale Benachrichtigung senden, wenn Abruf abgeschlossen ist. Sicherstellen den Platzhalter ersetzen `{backend endpoint}` mit der Back-End-Endpunkt:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Behandeln der lokalen Benachrichtigung über Image View Controller in **AppDelegate.m** mit den folgenden Methoden öffnen:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Führen Sie die Anwendung

1. Führen Sie in XCode die Anwendung auf einem physischen iOS-Gerät (Push Notifications funktioniert nicht im Simulator).

2. Geben Sie in iOS-app UI Benutzername und Kennwort den gleichen Wert für die Authentifizierung, und klicken Sie auf **Anmelden**.

3. Klicken Sie auf **Push senden** und Sie sollten eine Warnung in app. Klick auf **mehr**wird das Bild geöffnet gewählte app Backend enthalten.

4. Sie können auch auf **Push senden** und sofort Taste home Ihres Geräts. In wenigen Minuten erhalten Sie eine Push-Benachrichtigung. Wenn Sie darauf tippen oder klicken Sie auf mehr, werden Sie Ihre app und umfangreiche Bildinhalt gebracht.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
