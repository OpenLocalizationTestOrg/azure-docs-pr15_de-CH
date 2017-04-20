<properties
    pageTitle="Senden von Pushbenachrichtigungen IOS mit Azure Notification Hubs | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie, wie Sie Azure Notification Hubs Pushbenachrichtigungen iOS-Anwendung senden."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="Push-Benachrichtigung Pushbenachrichtigungen Ios Pushbenachrichtigungen"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Push-Benachrichtigung zu iOS mit Azure Benachrichtigung senden

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Übersicht

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

Dieses Lernprogramm zeigt wie Azure Notification Hubs Pushbenachrichtigungen iOS-Anwendung an. Erstellen Sie eine leere iOS-app, die Pushbenachrichtigungen mit [Apple Push Notification Service (APN)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)erhält. 

Wenn Sie fertig sind, werden Sie mit Ihrem Haupt-Benachrichtigung Push-Benachrichtigung an alle Ihre App Geräte übertragen.

## <a name="before-you-begin"></a>Bevor Sie beginnen

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Der vollständige Code für dieses Lernprogramm finden Sie [auf GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm ist Folgendes erforderlich:

+ [Mobile Dienste iOS SDK Version 1.2.4]
+ Neueste Version von [Xcode]
+ Ein iOS 8 (oder höher)-Gerät
+ [Apple Developer](https://developer.apple.com/programs/) Mitgliedschaft.

   > [AZURE.NOTE] Konfiguration Anforderungen für Pushbenachrichtigungen müssen Sie bereitstellen und Testen von Pushbenachrichtigungen auf einem physischen iOS-Gerät (iPhone und iPad) statt der iOS-Simulator.

Dieses Lernprogramm ist eine Voraussetzung für alle anderen Notification Hubs Lernprogramme für iOS-apps.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurieren der Benachrichtigungshub für iOS Notifications Push

Dieser Abschnitt führt Sie durch eine neue benachrichtigungshub erstellen und Konfigurieren von Authentifizierung mit APN mit dem **P12** Push-Zertifikat, das Sie erstellt haben. Möchten Sie Notification Hub verwenden, den Sie bereits erstellt haben, können Sie zu Schritt 5 überspringen.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p>Klicken Sie im Blatt <b>Einstellungen</b> <b>Notification Services</b> und wählen Sie <b>Apple (APN)</b>. Klicken Sie auf <b>Hochladen</b> und wählen Sie <b>P12</b> -Datei, die Sie zuvor exportiert. Stellen Sie sicher, dass Sie auch das Kennwort angeben.</p>
<p>Stellen Sie sicher <b>Sandkastenmodus</b> aktivieren, da dies für die Entwicklung. Verwenden Sie die <b>Produktion</b> nur, wenn Sie Push-Benachrichtigung an Benutzer senden, die Ihre app im Store erwerben möchten.</p>
</li>
</ol>
&emsp;&emsp;![APN in Azure-Portal konfigurieren](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![APN Zertifizierung in Azure konfigurieren](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Der benachrichtigungshub ist jetzt konfiguriert APN und haben die Verbindungszeichenfolgen für Ihre Anwendung registrieren und Push-Benachrichtigung zu senden.

##<a name="connect-your-ios-app-to-notification-hubs"></a>IOS-app mit Notification Hubs verbinden

1. Erstellen eines neuen Projekts iOS Xcode und wählen Sie **einzelne Ansicht** Anwendungsvorlage.

    ![Xcode - Einzelansicht Anwendung][8]

2. Festlegen die Optionen für das neue Projekt Achten Sie die gleichen **Produktnamen** und **Organisations-ID** verwendet, wenn Sie zuvor die Paket-ID auf der Apple Developer Portal.

    ![Xcode - Projektoptionen][11]

3. Klicken Sie unter **Ziele**auf den Projektnamen klicken Sie auf die Registerkarte **Buildeinstellungen** und **Codeidentität Signieren**und anschließend unter **Debuggen**, setzen Sie die Codesignatur Identity. **Ebenen** von **grundlegenden** **Alle**umschalten und **Profil bereitstellen** , die Sie zuvor erstellt haben Bereitstellungsprofil festgelegt.

    Wenn die in Xcode erstellte neue Bereitstellungsprofil nicht angezeigt wird, aktualisieren Sie die Profile für die Signaturidentität. Klicken Sie auf der Menüleiste auf **Xcode** auf **Voreinstellungen**, klicken Sie auf die Registerkarte **Konto** , klicken Sie auf **Details anzeigen** , klicken Sie auf Signatur-Identität, und klicken Sie auf die Aktualisierungsschaltfläche in der unteren rechten Ecke.

    ![Xcode - provisioning-Profil][9]

4. [Mobile Dienste iOS SDK Version 1.2.4] herunterladen Sie und Entpacken Sie die Datei. In Xcode Projekt und die **Dateien hinzufügen** können Xcode Projekt **WindowsAzureMessaging.framework** Ordner hinzu. Wählen Sie **Elemente bei Bedarf kopieren**und dann auf **Hinzufügen**.

    >[AZURE.NOTE] Benachrichtigungshubs SDK unterstützt derzeit nicht Bitcode Xcode 7.  **Bitcode aktivieren** müssen **keine** **Buildoptionen** für das Projekt festgelegt werden.

    ![Extrahieren von Azure SDK][10]

5. Eine neue Headerdatei zum Projekt hinzufügen `HubInfo.h`. Diese Datei enthält die Konstanten für die benachrichtigungshub.  Fügen Sie die folgenden Definitionen hinzu und Ersetzen Sie das literal Platzhalter mit Ihrem *Hubnamen* und *DefaultListenSharedAccessSignature* , die bereits erwähnt.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Öffnen der `AppDelegate.h` Datei hinzufügen die folgenden Import-Direktiven:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. In der `AppDelegate.m file`, fügen Sie den folgenden Code in die `didFinishLaunchingWithOptions` -Methode basierend auf der iOS-Version. Dieser Code registriert die Zugriffsnummer APN:

    Für iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    IOS-Versionen vor 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. Fügen Sie die folgenden Methoden in der gleichen Datei. Dieser Code verbindet mit Verbindungsinformationen in HubInfo.h angegebene Notification-Hub. Dann gibt gerätetoken an den benachrichtigungshub, damit benachrichtigungshub Benachrichtigungen senden kann:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. Fügen Sie die folgende Methode **UIAlert** anzeigen, wenn die Benachrichtigung empfangen wird, während die Anwendung aktiv ist, in derselben Datei:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Erstellen Sie und führen Sie die Anwendung auf dem Gerät, um sicherzustellen, dass keine Fehler.

## <a name="send-test-push-notifications"></a>Test-Push-Benachrichtigung senden


Sie können in Ihrer Anwendung Benachrichtigungen per Push-Benachrichtigung in der [Azure-Portal] über den Abschnitt **Problembehandlung** Hub Blade (verwenden Sie die Option *Test senden* ) testen.

![Azure-Portal - Test senden][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Optional) Push-Benachrichtigung der App

>[AZURE.IMPORTANT] In diesem Beispiel Senden von Benachrichtigungen von der Clientanwendung erfolgt nur zu. Da dies erfordert die `DefaultFullSharedAccessSignature` um auf die Clientanwendung, macht Ihre benachrichtigungshub das Risiko, dass Benutzer nicht autorisierte Benachrichtigungen an die Clients zugreifen kann.

Wenn Sie Push-Benachrichtigung innerhalb einer Anwendung senden möchten, enthält dieser Abschnitt ein Beispiel zum Verwenden der REST-Schnittstelle.

1. Öffnen in Xcode, `Main.storyboard` und die folgenden UI-Komponenten aus der Objektbibliothek in der app Pushbenachrichtigungen senden können:

    - Eine Bezeichnung mit kein Beschriftungstext. Er wird zum Melden von Fehlern Benachrichtigungen senden verwendet werden. Die **Lines** -Eigenschaft sollte auf **0** festgelegt werden, damit automatisch Größe eingeschränkt auf dem rechten und linken Ränder und dem oberen Rand der Ansicht.
    - Ein Textfeld mit **Platzhaltertext **Benachrichtigung geben**soll** . Einschränken der Bereich direkt unterhalb der Bezeichnung wie folgt. Festlegen der Ansicht delegierter an.
    - Eine Schaltfläche mit der Bezeichnung **Benachrichtigung senden** direkt unter dem Textfeld und in der horizontalen Mitte eingeschränkt.

    Die Ansicht sollte wie folgt aussehen:

    ![Xcode designer][32]


2. [Hinzufügen-Ausgänge](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) für die Bezeichnung und Text die Ansicht verbunden und Aktualisieren der `interface` Definition unterstützen `UITextFieldDelegate` und `NSXMLParserDelegate`. Fügen Sie drei Deklarationen unterstützen die REST-API aufrufen und Analysieren der Antwort unten.

    Die ViewController.h-Datei sollte wie folgt aussehen:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Open `HubInfo.h` und die folgenden Konstanten die Benachrichtigung an den Hub senden verwendet werden. Die tatsächliche *DefaultFullSharedAccessSignature* Verbindungszeichenfolge ersetzen Sie Zeichenfolgenliteral Platzhalter.

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Fügen Sie den folgenden `#import` Aussagen auf Ihre `ViewController.h` Datei.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. In `ViewController.m` die Implementierung den folgenden Code hinzufügen. Dieser Code wird die Verbindungszeichenfolge *DefaultFullSharedAccessSignature* analysieren. Wie die [REST-API-Referenz](http://msdn.microsoft.com/library/azure/dn495627.aspx)wird diese analysierten Informationen zu SAS-Token für Anforderungsheader **Autorisierung** verwendet.

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. In `ViewController.m`, Aktualisieren der `viewDidLoad` Methode, um die Verbindungszeichenfolge beim Laden der Ansicht analysieren. Fügen Sie Dienstprogrammmethoden unten, um die Implementierung auch hinzu.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. In `ViewController.m`, fügen Sie den folgenden Code, um die Implementierung zu SaS Autorisierungstoken, die gemäß der **Authorization** -Header wie die [REST-API-Referenz](http://msdn.microsoft.com/library/azure/dn495627.aspx).

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. Taste STRG + Ziehen aus der **Benachrichtigung senden** `ViewController.m` eine Aktion mit dem Namen **SendNotificationMessage** für das Ereignis **Berühren Sie** hinzuzufügen. Update-Methode mit dem folgenden Code der Benachrichtigung über die REST-API.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. In `ViewController.m`, fügen Sie folgende Delegatmethode Unterstützung schließen die Tastatur für das Textfeld. STRG + Ziehen aus dem Textfeld View Controller-Symbol im Designer Benutzeroberfläche View Controller delegierter Ausgang festgelegt.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. In `ViewController.m`, fügen Sie folgende Delegatmethoden Unterstützung Analysieren der Antwort mit `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Erstellen Sie das Projekt, und stellen Sie sicher, dass keine Fehler vorhanden sind.


> [AZURE.NOTE] Sollten einen Buildfehler im Xcode7 zur Unterstützung von Bitcode sollte, ändern Sie die **Buildeinstellungen** > **Aktivieren Bitcode (ENABLE_BITCODE)** **nicht** in Xcode. Benachrichtigung Hubs SDK unterstützt derzeit keine Bitcode. 

Die möglichen Benachrichtigung Nutzlasten finden Sie im Apple [lokale und Push Notification Programming Guide].


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Überprüfen, ob Ihre Anwendung Pushbenachrichtigungen empfangen kann

Pushbenachrichtigungen IOS testen, müssen Sie die app physischen iOS-Gerät bereitstellen. Sie können nicht mit iOS Simulator Apple Push-Benachrichtigung senden.

1. Führen Sie die Anwendung und überprüfen Sie, ob die Registrierung erfolgreich und klicken Sie dann auf **OK**.

    ![iOS-App Push Notification Registrierung Test][33]

2. Wie oben beschrieben, können Sie eine Push-testbenachrichtigung von [Azure-Portal]senden. Wenn Sie Code für das Senden von Pushbenachrichtigungen in der app hinzugefügt, tippen Sie im Feld Geben Sie eine Benachrichtigung. Klicken Sie dann auf **Senden** auf der Tastatur oder **Benachrichtigung senden** -Schaltfläche in der Ansicht, die Nachricht zu senden.

    ![iOS-App Push Notification Test senden][34]

3. Push-Benachrichtigung wird an alle Geräte gesendet, die von bestimmten Notification Hub Benachrichtigungen registriert sind.

    ![iOS-App Push Benachrichtigung empfangen Test][35]


##<a name="next-steps"></a>Nächste Schritte

In diesem einfachen Beispiel wird Push-Benachrichtigung an alle registrierten iOS-Geräte übertragen. Wir empfehlen, als Nächstes die lernen Sie in [Azure Notification Hubs Benutzer benachrichtigen für iOS mit .NET Back-End] -Lernprogramm nacheinander gehen Backend zum Senden von Pushbenachrichtigungen mit Tags erstellen. 

Ggf. Benutzer Interessengruppen segment können Sie zusätzlich zur praktischen [Verwendung Benachrichtigungshubs Nachrichten senden] übergehen. 

Allgemeine Informationen zu Notification Hubs anzeigen Sie [Benachrichtigung Hubs Anleitung]



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Dienste iOS SDK Version 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Benachrichtigung Hubs Anleitung]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs Benutzer benachrichtigen für iOS mit .NET Back-End]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Lokale und Benachrichtigung Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure-Portal]: https://portal.azure.com