<properties
    pageTitle="Wie mit iOS SDK für Azure Mobile Apps"
    description="Wie mit iOS SDK für Azure Mobile Apps"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Wie mit iOS-Clientbibliothek für Azure Mobile Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Dabei lernen Sie das allgemeine Szenarien mit den neuesten [Azure Mobile Apps iOS SDK][1]. Sind Sie neu in Azure Mobile Apps, vollständige [Azure Mobile Apps Quick Start] einer Back-End erstellen eine Tabelle erstellen und eine vordefinierte iOS Xcode Projekt herunterladen. In diesem Handbuch liegt der Schwerpunkt auf der Clientseite iOS SDK. Erfahren Sie mehr über serverseitige SDK für die Back-End-Server SDK Anleitungen anzeigen

## <a name="reference-documentation"></a>Dokumentation

Die Referenzdokumentation für iOS Client SDK befindet sich hier: [Azure Mobile Apps iOS Clientverweis][2].

## <a name="supported-platforms"></a>Unterstützte Plattformen

IOS SDK unterstützt Objective-C-Projekte, Swift 2.2 und 2.3 Swift Projekte für iOS-Version 8.0 oder höher.

"Server-Flow" Authentifizierung verwendet eine Webansicht dargestellten Benutzeroberfläche.  Wenn das Gerät nicht an eine UI WebView kann dann einer anderen Authentifizierungsmethode erforderlich ist, die außerhalb des Produkts ist.  
Dieses SDK deshalb nicht für Art oder eingeschränkten Geräten.

##<a name="Setup"></a>Setup und Komponenten

Dieses Handbuch setzt voraus, dass eine Back-End-Tabellen erstellt haben. Dieses Handbuch setzt voraus, dass die Tabelle das gleiche Schema wie die Tabellen in diesen Lernprogrammen. Dabei wird vorausgesetzt, dass in Ihrem Code verweisen `MicrosoftAzureMobile.framework` und `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Gewusst wie: Erstellen

Zugriff auf einen Backend Azure Mobile Apps im Projekt erstellen ein `MSClient`. Ersetzen Sie `AppUrl` app-URL. Lassen Sie `gatewayURLString` und `applicationKey` leer. Wenn Sie ein Gateway für die Authentifizierung eingerichtet, füllen `gatewayURLString` Gateway-URL.

**Objective-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Gewusst wie: Referenz erstellen

Zugriff oder Update-Daten auf Back-End-Tabelle erstellen. Ersetzen Sie `TodoItem` mit dem Namen der Tabelle

**Objective-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Gewusst wie: Abfragen von Daten

Erstellen einer Abfrage der `MSTable` Objekt. Die folgende Abfrage ruft alle Elemente in `TodoItem` und den Text der einzelnen Elemente.

**Objective-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>How to: Filter zurückgegebenen Daten

Zum Filtern der Ergebnisse sind viele Optionen verfügbare.

Zum Verwenden eines Prädikats filtern verwenden ein `NSPredicate` und `readWithPredicate`. Die folgenden Filter zurückgegeben Daten nur unvollständig Aufgaben zu.

**Objective-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Gewusst wie: MSQuery verwenden

Führen Sie eine komplexe Abfrage (einschließlich Sortierung und Paging), erstellen Sie ein `MSQuery` Objekt direkt oder über ein Prädikat:

**Objective-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`können Sie mehrere Abfrage Verhalten steuern.

* Geben Sie die Reihenfolge der Ergebnisse
* Welche Felder zurückgegeben beschränken
* Einschränken Sie, wie viele Datensätze zurück
* Geben Sie auf insgesamt
* Geben Sie benutzerdefinierte Abfragezeichenfolgenparameter Anforderung
* Zusätzliche Funktionen übernehmen

Ausführen einer `MSQuery` Abfrage durch Aufrufen von `readWithCompletion` auf das Objekt.

## <a name="sorting"></a>Gewusst wie: Sortieren von Daten mit MSQuery

Um die Ergebnisse zu sortieren, betrachten wir ein Beispiel. Rufen Sie zum Sortieren nach aufsteigender Feld 'Text' und 'abgeschlossen' absteigend nach `MSQuery` wie folgt:

**Objective-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Gewusst wie: Felder einschränken und erweitern Sie Abfragezeichenfolgen-Parameter mit MSQuery

Geben Sie die Namen der Felder in der **SelectFields** -Eigenschaft, zum Begrenzen von Feldern in einer Abfrage zurückgegeben werden. In diesem Beispiel gibt nur den Text und die Felder abgeschlossen:

**Objective-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

Zusätzliche Abfragezeichenfolgen-Parameter in die Anforderung einschließen (z. B. weil einen benutzerdefinierten serverseitigen Skripts verwendet), füllen `query.parameters` wie folgt:

**Objective-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Gewusst wie: Konfigurieren der Seitengröße

Azure Mobile Apps steuert die Größe die Anzahl der Datensätze, die gleichzeitig vom Back-End-Tabellen abgerufen werden. Ein Aufruf von `pull` Daten würden dann batch-Daten basierend auf diese Seitengröße, bis keine weiteren Datensätze zu ziehen sind.

Es ist möglich, eine Seite mit **MSPullSettings** wie folgt konfigurieren. Die Standardseitengröße 50 und im folgenden Beispiel ändert es 3.

Sie können eine andere Seitengröße aus Leistungsgründen konfigurieren. Haben Sie eine große Anzahl von kleinen Datensätzen reduziert hohe Seitengröße der Server-Roundtrips. 

Diese Einstellung steuert nur die Seitengröße auf dem Client. Fragt der Client Seite größer als Backend Mobile Apps unterstützt, ist die Größe höchstens begrenzt, die Back-End-Unterstützung konfiguriert. 

Diese Einstellung ist auch die _Anzahl_ der Datensätze nicht die _Bytegröße_.

Wenn Sie die Seitengröße Client, [erhöhen Sie auch die Größe auf dem Server](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size)erhöhen.

**Objective-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Gewusst wie: Einfügen von Daten

Um eine neue Tabellenzeile einzufügen, erstellen Sie einen `NSDictionary` und `table insert`. Bei aktiviertem [Dynamic Schema] generiert Azure App Service mobile Backend automatisch neue Spalten basierend auf der `NSDictionary`.

Wenn `id` nicht bereitgestellt, Backend erstellt automatisch eine neue eindeutige ID. Geben Sie einen eigenen `id` e-Mail Adressen, Benutzernamen oder eigene benutzerdefinierte ID-Werte Eigene ID bereitstellen kann Joins und geschäftsorientierte Datenbanklogik erleichtern.

Die `result` enthält das neue Element eingefügt wurde. Je nach der Serverlogik möglicherweise zusätzliche oder geänderte Daten, was an den Server übergeben wurde.

**Objective-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Gewusst wie: Ändern von Daten

Aktualisieren eine vorhandene Zeile bearbeiten ein Element und rufen `update`:

**Objective-C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Geben Sie alternativ die Zeilen-ID und dem aktualisierten Feld:

**Objective-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Mindestens die `id` -Attribut muss festgelegt werden, wenn Updates.

##<a name="deleting"></a>Gewusst wie: Löschen

Um ein Element zu löschen, rufen Sie `delete` mit dem Element:

**Objective-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Sie können auch löschen Sie, indem Sie eine Zeilen-ID:

**Objective-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Mindestens die `id` -Attribut muss festgelegt werden, wenn zu löschen.

##<a name="customapi"></a>Gewusst wie: Aufrufen von benutzerdefinierten API

Mit einer benutzerdefinierten API können Sie Back-End-Funktionalität bereitstellen. Es muss eine tabellenvorgang zuordnen. Nicht nur gewinnen Sie mehr Kontrolle über messaging, können Lese-/Set Header und Body Antwortformat ändern. Lesen Sie Informationen zum Erstellen einer benutzerdefinierten API im Back-End [Benutzerdefinierte APIs](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Um eine benutzerdefinierte API aufzurufen, rufen `MSClient.invokeAPI`. Anforderung und Antwort Inhalte werden als JSON behandelt. Verwenden Sie andere Medien [die andere Überladung der `invokeAPI` ] [ 5].  Zu einer `GET` statt anfordern ein `POST` anfordern, festgelegte Parameter `HTTPMethod` , `"GET"` und `body` , `nil` (da GET-Anfragen nicht Nachrichtentexte.) Wenn Ihre benutzerdefinierte API anderen HTTP-Verben unterstützt, ändern `HTTPMethod` entsprechend.

**Objective-C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Gewusst wie: Registrieren Push Vorlagen plattformübergreifende Benachrichtigungen senden

Um Vorlagen zu registrieren, übergeben Sie Vorlagen mit der Methode **client.push RegisterDeviceToken** in der Clientanwendung.

**Objective-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Vorlagen sind NSDictionary und können mehrere Vorlagen im folgenden Format enthalten:

**Objective-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Aus der Sicherheit werden alle Tags entfernt.  Tags für Anlagen oder Vorlagen in Installationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile Apps][4].  Zum Senden von Benachrichtigungen mit Vorlagen registrierten [Benachrichtigung Hubs APIs]arbeiten[3].

##<a name="errors"></a>Gewusst wie: Behandeln von Fehlern

Beim Aufrufen einer Azure App Service mobile Back-End-Block Abschluss enthält ein `NSError` Parameter. Wenn ein Fehler auftritt, ist dieser Parameter nicht NULL. In Ihrem Code überprüfen Sie diesen Parameter und den Fehler ggf. behandeln, wie in den vorhergehenden Codeausschnitten.

Die Datei [`<WindowsAzureMobileServices/MSError.h>`] [6] definiert die Konstanten `MSErrorResponseKey`, `MSErrorRequestKey`, und `MSErrorServerItemKey`. Weitere Angaben zum Fehler abrufen:

**Objective-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Außerdem definiert die Datei Konstanten für jeden Fehlercode:

**Objective-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Gewusst wie: Benutzerauthentifizierung mit der Active Directory-Authentifizierungsbibliothek

Active Directory Authentifizierung Library (ADAL) können Sie die Anwendung mithilfe von Azure Active Directory Benutzer anmelden. Flow-Clientauthentifizierung mit Identitätsanbieter SDK vorzuziehen ist die `loginWithProvider:completion:` Methode.  Clientauthentifizierung Fluss ermöglicht sich mehr systemeigene UX und für die weitere Anpassung.

1. Konfigurieren der mobilen Anwendung Backend AAD anmelden mithilfe [App Service für Active Directory-Konto konfigurieren] [ 7] Tutorial. Stellen Sie sicher, das optionale Schritt eine native Client-Anwendung registrieren. Für iOS, empfehlen wir die Umleitung URI das Format hat `<app-scheme>://<bundle-id>`. Weitere Informationen finden Sie unter [ADAL iOS Schnellstart][8].

2. Installieren Sie ADAL mit Cocoapods. Bearbeiten Sie die Podfile um die folgende Definition, **Ihr Projekt** mit dem Namen des Projekts Xcode ersetzen:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   und Hülsen:

        pod 'ADALiOS'

3. Mithilfe der Terminaldienste `pod install` aus dem Verzeichnis enthält das Projekt, und öffnen Sie generierte Xcode Workspace (nicht das Projekt).

4. Fügen Sie folgenden Code der Anwendung verwendete Sprache. Stellen Sie, diesen Austausch:

    * Ersetzen Sie **Einfügen Behörde hier** durch den Namen des Mandanten, in dem die Anwendung bereitgestellt. Das Format muss https://login.windows.net/contoso.onmicrosoft.com sein. Dieser Wert kann aus der Registerkarte Domänen Azure Active Directory in [klassischen Azure-Portal] kopiert.
    * Die Client-ID für die mobile Anwendung Backend-ersetzen Sie **Einfügen-Ressource-ID-hier** . Die Client-ID erhalten Sie auf der Registerkarte **Erweitert** unter **Azure Active Directory Einstellungen** im Portal.
    * Ersetzen Sie **INSERT-CLIENT-ID-hier** durch die Client-ID von der systemeigenen Clientanwendung kopiert.
    * Ersetzen Sie **INSERT-REDIRECT-URI-hier** mit Ihrer Site _/.auth/login/done_ Endpunkt mit HTTPS-Schema. Dieser Wert sollte ähnlich wie _https://contoso.azurewebsites.net/.auth/login/done_.

**Objective-C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Gewusst wie: Benutzerauthentifizierung mit Facebook SDK für iOS

Facebook-SDK für iOS können Benutzer der Anwendung mit Facebook anmelden.  Mit einer fortlaufenden Clientauthentifizierung vorzuziehen ist die `loginWithProvider:completion:` Methode.  Fluss Clientauthentifizierung ermöglicht mehr systemeigene UX Verhalten und für die weitere Anpassung.

1. Konfigurieren Sie mobile app Backend bei Facebook anmelden mithilfe [App Service für Facebook Anmelden konfigurieren] [ 9] Tutorial.

2. Facebook-SDK für iOS anhand der [Facebook-SDK für iOS - erste Schritte] installieren[ 10] Dokumentation. Anstatt einer Anwendung können Sie Ihre Eintragung iOS-Plattform hinzufügen. 

3. Facebook Dokumentation enthält einige Objective-C-Code in der App-Delegat. Wenn **Swift**verwenden, können Sie die folgenden Übersetzungen für AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Neben `FBSDKCoreKit.framework` auch auf dem Projekt hinzufügen `FBSDKLoginKit.framework` auf die gleiche Weise. 

4. Fügen Sie folgenden Code der Anwendung verwendete Sprache. 

**Objective-C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Gewusst wie: Benutzerauthentifizierung mit Twitter für iOS

Fabric für iOS können Sie die Anwendung mit Twitter Benutzer anmelden. Fluss Clientauthentifizierung vorzuziehen ist die `loginWithProvider:completion:` -Methode, eine weitere systemeigene UX Verhalten und zusätzliche Anpassung ermöglicht.

1. Konfigurieren Sie mobile-app Backend Twitter-Anmeldung nach dem Tutorial [App Service für Twitter Anmeldung konfigurieren](app-service-mobile-how-to-configure-twitter-authentication.md) .

2. Fügen Sie Fabric folgende [Stoff für iOS - Einsteiger] -Dokumentation und TwitterKit dem Projekt hinzu.

    > [AZURE.NOTE] Standardmäßig erstellt Fabric Twitter-Anwendung. Sie können vermeiden einer Anwendung registrieren Consumer Schlüssel und Verbrauchergeheimnis zuvor mit folgenden Codeausschnitte erstellt.  Alternativ können Sie die Verbraucherschlüssel und Verbrauchergeheimnis Werte ersetzen die App Service Werte enthalten im [Fabric Dashboard]sehen. Wenn Sie diese Option auswählen, werden Sie den Rückruf-URL auf einen Platzhalterwert wie `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    App-Delegat Wunsch Kennwörter verwenden, die Sie zuvor erstellt haben, fügen Sie den folgenden Code hinzu:
    
    **Objective-C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Fügen Sie folgenden Code der Anwendung verwendete Sprache. 

**Objective-C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Gewusst wie: Benutzerauthentifizierung mit Google-In SDK für iOS

Google-In SDK für iOS können Benutzer Ihre Anwendung mit einem googlekonto anmelden.  Google angekündigt ändert ihre OAuth-Sicherheitsrichtlinien.  Diese Änderungen werden Google-SDK in der Zukunft benötigen.

1. Die [App-Service für Google Anmeldung konfigurieren](app-service-mobile-how-to-configure-google-authentication.md) Lernprogramm konfigurieren Sie mobile app Backend Google-Anmeldung.

2. Installieren Sie Google SDK für iOS, indem der Dokumentation [Google anmelden für iOS - Start integrieren](https://developers.google.com/identity/sign-in/ios/start-integrating) . Abschnitt "Authentifizierung mit einem Back-End-Server" überspringen.

3. Fügen Sie Folgendes zu Ihrer Stellvertretung `signIn:didSignInForUser:withError:` Verfahren nach der verwendeten Sprache.

**Objective-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Achten Sie auch Folgendes hinzufügen `application:didFinishLaunchingWithOptions:` in der app-Delegat, ersetzen "SERVER_CLIENT_ID" mit der gleichen ID, mit der App-Dienst in Schritt 1 konfigurieren.

**Objective-C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Fügen Sie folgenden Code zu der Anwendung ein UIViewController implementiert die `GIDSignInUIDelegate` Protokoll nach der verwendeten Sprache.  Werden Sie abgemeldet, bevor erneut signiert wird, und zwar benötigen Ihre Anmeldeinformationen erneut eingeben, sehen Sie ein Zustimmungsdialogfeld.  Rufen Sie diese Methode nur das Sitzungstoken abgelaufen ist.
 
 **Objective-C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile Apps Quick Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamische Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Fabric-Dashboard]: https://www.fabric.io/home
[Fabric für iOS - erste Schritte]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
