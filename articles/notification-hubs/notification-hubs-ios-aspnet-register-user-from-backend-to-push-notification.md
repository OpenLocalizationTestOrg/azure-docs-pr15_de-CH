<properties
    pageTitle="Registrieren des Benutzers für Pushbenachrichtigungen Web API | Microsoft Azure"
    description="Informationen Sie zum Push Notification Eintragung in iOS-app mit Azure Benachrichtigung anfordern, wenn Registeration durch ASP.NET Web API erfolgt."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>Registrieren Sie den aktuellen Benutzer für Pushbenachrichtigungen mit ASP.NET

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>Übersicht

In diesem Thema veranschaulicht, Push Notification Anmeldung Azure Notification Hubs bei Registrierung von ASP.NET Web API ausgeführt wird. In diesem Thema erweitert das Lernprogramm [benachrichtigen Benutzer mit Notification Hubs]. Sie müssen die erforderlichen Schritte in diesem Lernprogramm authentifizierte mobile Service erstellen bereits abgeschlossen sein. Finden Sie weitere Szenario Benutzer benachrichtigen [Benutzer benachrichtigen Benachrichtigungshubs].

##<a name="update-your-app"></a>Aktualisieren Sie Ihrer Anwendung  

1. Fügen Sie die folgenden Komponenten in der Objektbibliothek, in der MainStoryboard_iPhone.storyboard:

    + **Bezeichnung**: "Push Notification Hubs mit"
    + **Bezeichnung**: "InstallationId"
    + **Bezeichnung**: "Benutzer"
    + **Textfeld**: "Benutzer"
    + **Bezeichnung**: "Kennwort"
    + **Textfeld**: "Kennwort"
    + **Schaltfläche**: "Login"

    Jetzt sieht das Storyboard folgendermaßen aus:

    ![][0]

2. Erstellen Sie im Editor Assistent Absatzmöglichkeiten für switched Steuerelemente rufen verbinden Sie Textfelder mit der Ansicht (Stellvertreter), und erstellen Sie eine **Aktion** für **die Anmeldeschaltfläche** .

    ![][1]

    Die BreakingNewsViewController.h-Datei sollte jetzt folgenden Code enthalten:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Erstellen Sie eine Klasse mit dem Namen **DeviceInfo**, und kopieren Sie folgenden Code in der Interface-Abschnitt der Datei DeviceInfo.h:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. Kopieren Sie folgenden Code im Implementierungsabschnitt der DeviceInfo.m-Datei:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. Fügen Sie die folgende Eigenschaft Singleton PushToUserAppDelegate.h hinzu:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. Fügen Sie den folgenden Code in der Methode **DidFinishLaunchingWithOptions** PushToUserAppDelegate.m:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    Die erste Zeile initialisiert **DeviceInfo** Singleton. Die zweite Zeile beginnt die Registrierung für abgeschlossenen Lernprogramm [Erste Schritte mit Notification Hubs] Push Notifications bereits vorhanden ist.

9. Implementieren Sie die Methode **DidRegisterForRemoteNotificationsWithDeviceToken** in der AppDelegate PushToUserAppDelegate.m und fügen Sie folgenden Code:

        self.deviceInfo.deviceToken = deviceToken;

    Das gerätetoken für die Anforderung fest.

    > [AZURE.NOTE] An dieser Stelle sollte es jeder andere Code in dieser Methode. Haben Sie bereits einen Aufruf der **RegisterNativeWithDeviceToken** -Methode, die nach Beendigung das Lernprogramm [Erste Schritte mit Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) hinzugefügt wurde, müssen Sie auskommentieren oder Entfernen dieses Aufrufs.

10. Fügen Sie in der Datei PushToUserAppDelegate.m die folgende Methode hinzu:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Diese Methode zeigt eine Warnung in der Benutzeroberfläche Ihrer Anwendung Benachrichtigung erhält, während er ausgeführt wird.

9. Öffnen Sie die Datei PushToUserViewController.m und die Tastatur in folgende Implementierung zurück:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. Initialisieren Sie in der **Spiele** -Methode in der Datei PushToUserViewController.m die Bezeichnung InstallationId wie folgt:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. Fügen Sie die folgenden Eigenschaften in Schnittstelle PushToUserViewController.m:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Fügen Sie die folgende Implementierung:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Kopieren Sie folgenden Code in die **Anmeldung** Handlermethode XCode erstellt:

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Diese Methode ruft eine Installations-ID und Channel für Pushbenachrichtigungen und sendet, mit den Gerätetyp authentifizierte Web-API-Methode, die eine Registrierung in Notification Hubs erstellt. Diese Web-API wurde [Benachrichtigen]Benutzer mit Benachrichtigung definiert.

Aktualisiert die Clientanwendung [Benutzer benachrichtigen Benachrichtigungshubs] zurück, und aktualisieren Sie den mobilen Dienst Benachrichtigungen Benachrichtigungshubs mit.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Benutzer mit Benachrichtigung benachrichtigen]: /manage/services/notification-hubs/notify-users-aspnet

[Erste Schritte mit Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
