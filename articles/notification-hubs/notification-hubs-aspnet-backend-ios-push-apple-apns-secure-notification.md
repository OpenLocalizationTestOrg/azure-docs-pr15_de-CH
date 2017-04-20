<properties
    pageTitle="Azure Notification Hubs sichere Push"
    description="Erfahren Sie mehr über das sichere Pushbenachrichtigungen von Azure einen iOS-App senden. Beispiele in Objective-C und C#."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs sichere Push

> [AZURE.SELECTOR]
- [Universal Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Übersicht

Push-Benachrichtigung-Unterstützung in Microsoft Azure ermöglicht eine einfach zu verwendende für mehrere Plattformen, skalierte Push-Infrastruktur die Implementierung von Pushbenachrichtigungen für Verbraucher und Unternehmen für mobile Plattformen vereinfacht zugreifen.

Durch behördliche oder Einschränkungen, eine Anwendung ggf. etwas in der Benachrichtigung enthalten, die über die standard-Push Notification Infrastruktur übertragen werden können. In diesem Lernprogramm beschrieben genauso erreichen vertraulichen Informationen über eine sichere, authentifizierte Verbindung zwischen dem Clientgerät und Backend-Anwendung.

Auf einer hohen Ebene ist der Fluss wie folgt:

1. App-Back-End:
    - Sichere Nutzlast im Back-End-Datenbank speichert.
    - Sendet die ID dieser Benachrichtigung an das Gerät (keine sichere Informationen gesendet).
2. Die Anwendung auf dem Gerät bei der Benachrichtigung:
    - Das Gerät kontaktiert Back-End-secure Payload anfordern.
    - Die Anwendung kann die Nutzlast als Benachrichtigung an das Gerät anzeigen

Es ist wichtig zu beachten, dass im vorherigen Fluss und in diesem Lernprogramm wird angenommen, dass das Gerät ein Authentifizierungstoken im lokalen Speicher, speichert nachdem der Benutzer anmeldet. Dies garantiert eine vollständig nahtlose Gerät die Benachrichtigung sichere Nutzlast mit diesem Token abrufen kann. Wenn Ihre Anwendung Authentifizierungstoken nicht auf dem Gerät speichern oder diese Token abgelaufen sein, sollte eine generische Meldung Benutzereingabe starten app Gerät nach Erhalt der Benachrichtigung angezeigt werden. Die Anwendung authentifiziert den Benutzer und zeigt die benachrichtigungsnutzlast.

Dieses sichere Push-Lernprogramm zeigt, wie sicher eine Push-Benachrichtigung senden. Das Lernprogramm baut auf das Lernprogramm [Benutzer benachrichtigen](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) , die in diesem Lernprogramm Erste Schritte sollten.

> [AZURE.NOTE] Diese praktische Einführung geht, erstellt und den benachrichtigungshub konfiguriert unter [Erste Schritte mit Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Ändern Sie das iOS-Projekt

Ihr app-Back-End nur die *Id* einer Benachrichtigung senden geändert, besitzen Sie ändern Ihre iOS-app, Benachrichtigung und Rückruf der Backend-rufen Sie die sichere Nachricht angezeigt werden.

Zur Erreichung dieses Ziels haben wir die Logik rufen Sie sichere Inhalte von app-Back-End zu schreiben.

1. Stellen Sie **AppDelegate.m**sicher aus der Back-End-app Register für die automatische Benachrichtigung so die Benachrichtigung Id verarbeitet gesendet. Fügen Sie die **UIRemoteNotificationTypeNewsstandContentAvailability** -Option DidFinishLaunchingWithOptions hinzu:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. Fügen Sie in der **AppDelegate.m** einen Implementierungsabschnitt mit der Deklaration:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Fügen Sie im Abschnitt über die Implementierung den folgenden Code ersetzt den Platzhalter `{back-end endpoint}` mit dem Endpunkt für die Back-End zuvor abgerufen:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Jetzt haben wir Benachrichtigung über eingehende Verarbeitung und Verwendung der oben erwähnten Methode zum Abrufen des Inhalts angezeigt. Erstens haben wir die iOS-app im Hintergrund ausgeführt, wenn eine Push-Benachrichtigung aktivieren. **XCode**wählen Sie auf der linken Seite app klicken Sie Ziel-Haupt-app im Abschnitt **Ziele** aus dem mittleren Bereich.

5. Klicken Sie auf die Registerkarte **Capabilities** oberen mittleren Bereich, und aktivieren Sie das Kontrollkästchen **Remote-Benachrichtigung** .

    ![][IOS1]


6. Fügen Sie die folgende Methode zum Behandeln von Pushbenachrichtigungen in **AppDelegate.m** :

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Beachten Sie, dass die Fälle fehlender Authentication Header oder Ablehnung von Back-End vorzuziehen. Spezifische Behandlung dieser Fälle wird meist angenehmer Ziel abhängig. Eine Möglichkeit ist, eine Benachrichtigung mit einer generischen Meldung für den Benutzer zu authentifizieren, zum Abrufen der aktuellen Benachrichtigung angezeigt.

## <a name="run-the-application"></a>Führen Sie die Anwendung

Führen Sie folgende Schritte aus, um die Anwendung auszuführen:

1. Führen Sie in XCode die Anwendung auf einem physischen iOS-Gerät (Push Notifications funktioniert nicht im Simulator).

2. IOS-app UI Geben Sie Benutzername und Kennwort ein. Dies können eine Zeichenfolge sein, aber sie müssen dem Wert entsprechen.

3. IOS-app UI klicken Sie auf **Anmelden**. Klicken Sie auf **Push senden**. Sie sollten die sichere mittig Benachrichtigung angezeigt Meldung.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
