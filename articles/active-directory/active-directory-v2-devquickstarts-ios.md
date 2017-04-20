<properties
    pageTitle="Azure AD v2. 0 iOS-App | Microsoft Azure"
    description="Wie eine iOS-app erstellen, die Benutzer mit beiden Microsoft-Konto und Geschäfts-oder signiert mit Drittanbieter-Bibliotheken."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Ein Graph-API v2. 0-Endpunkt mit einer Drittanbieter-Bibliothek mit iOS-app-in hinzufügen

Microsoft Identitätsplattform verwendet offene Standards wie OAuth2 OpenID verbinden. Jede Bibliothek können Entwickler wollen sie unsere Services integriert. Um unsere Plattform mit anderen Bibliotheken verwenden Entwickler unterstützen, haben wir einige Exemplarische Vorgehensweisen wie diese wie Drittanbieterbibliotheken Verbindung zur Microsoft-Identitätsplattform konfiguriert wird geschrieben. Bibliotheken, die [RFC6749 OAuth2-Spezifikation](https://tools.ietf.org/html/rfc6749) implementieren können zur Microsoft Identity-Plattform.

Mit der in dieser exemplarischen Vorgehensweise erstellte Anwendung Benutzer ihrer Organisation anmelden und suchen dann mithilfe der Graph-API in ihrer Organisation für andere.

Wenn Sie OAuth2 oder OpenID nicht vertraut sind, kann dieses Beispiel-Konfiguration nicht Ihnen sinnvoll. [V2. 0-Protokolle - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) Hintergrund gelesen werden empfohlen.


> [AZURE.NOTE]
    Einige Features der Plattform, die einen Ausdruck in Standards OAuth2 oder OpenID verbinden Zugangskontrolle wie Intune Richtlinienmanagement erfordern unsere offene Quelle Microsoft Azure Identität Bibliotheken.

V2. 0-Endpunkt unterstützt nicht alle Azure Active Directory Szenarien und Features.

> [AZURE.NOTE]
    Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Code von GitHub herunterladen
Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2)verwaltet.  Um folgen, können Sie [die Anwendung Skelett als .zip downloaden](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) oder das Skelett Klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Sie können auch das Beispiel herunterladen und loslegen:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Registrieren einer Anwendung
Erstellen einer neuen Applikation [Anwendung registrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)oder detaillierte Schritte wie [sich eine Anwendung mit dem Endpunkt v2. 0](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

- Kopieren Sie die **Id der Anwendung** , die Ihre Anwendung zugewiesen ist, da Sie bald benötigt werden.
- **Mobile** Plattform für Ihre Anwendung hinzufügen.
- Kopieren der **URI umleiten** vom Portal aus. Verwenden Sie den Standardwert `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Bibliothek NXOAuth2 von Drittanbietern herunterladen und einen Arbeitsbereich erstellen

In dieser exemplarischen Vorgehensweise verwenden Sie OAuth2Client von GitHub, eine OAuth2 Bibliothek für Mac OS X und iOS (Kakao und Kakao berühren). Diese Bibliothek basiert auf Entwurf 10 OAuth2-Spezifikation. Es implementiert das systemeigene Anwendungsprofil und autorisierungsendpunkt des Benutzers unterstützt. Dies sind alles müssen Sie Microsoft Identity-Plattform integriert.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>Fügen Sie Bibliothek mit CocoaPods zum Projekt hinzu

CocoaPods ist ein Abhängigkeit Xcode Projekte. Die Installationsschritte verwaltet automatisch.

```
$ vi Podfile
```
1. Dieser Podfile wird Folgendes hinzufügen:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Laden der Podfile CocoaPods. Dadurch entsteht einen neuen Arbeitsbereich Xcode, den geladen wird.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Die Struktur des Projekts

Die folgende Struktur ist für unser Projekt im Skelett eingerichtet:

- Masteransicht mit einem UPN-Suche
- Eine Detailansicht der Daten zum ausgewählten Benutzer
- Eine Anmeldung anzeigen, wo ein Benutzer der Anwendung anmelden kann Diagramm Abfragen

Wir werden in verschiedenen Dateien im Skelett Authentifizierung hinzufügen verschoben. Andere Teile des Codes wie visual Code beziehen sich nicht auf Identität jedoch für Sie bereitgestellt.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Richten Sie die settings.plst-Datei in der Bibliothek

-   Öffnen Sie im Schnellstart-Projekt die `settings.plist` Datei. Ersetzen Sie die Werte der Elemente im Abschnitt zu den Werten, die in Azure-Portal verwendet. Code verweist diese Werte, wenn Active Directory-Authentifizierung Library verwendet.
    -   Die `clientId` ist die Client-ID der Anwendung, die über das Portal kopiert.
    -   Die `redirectUri` ist die im Portal bereitgestellte URL umleiten.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>NXOAuth2Client Bibliothek in Ihr LoginViewController einrichten

Die NXOAuth2Client muss einige Werte um. Nach Abschluss dieser Aufgabe können erworbene Token Sie die Graph-API aufzurufen. Da `LoginView` wird aufgerufen, wenn wir authentifizieren müssen, sinnvoll Werte in dieser Datei gesetzt.

- Fügen Sie einige Werte, die `LoginViewController.m` Datei der Kontext für die Authentifizierung und Autorisierung. Details zu Werten führen Sie den Code.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Details zu den Code betrachten.

Die erste Zeichenfolge ist für `scopes`.  Die `User.Read` Wert können Sie die grundlegenden Profil des angemeldeten Benutzers.

Erfahren Sie mehr über die verfügbaren Bereiche [Microsoft Graph Berechtigung](https://graph.microsoft.io/docs/authorization/permission_scopes)Bereiche.

Für `authURL`, `loginURL`, `bhh`, und `tokenURL`, die zuvor bereitgestellten Werte verwenden. Bei Verwendung die open Source Microsoft Azure Identität Bibliotheken Ziehen wir diese Daten Sie für Sie mit unseren Endpunkt. Wir haben die Festplatte extrahieren diese Werte für Sie erledigt.

Die `keychain` Wert ist der Container, der mithilfe der NXOAuth2Client-Bibliothek Schlüsselbund Token speichern zu erstellen. Wenn Sie einmaliges Anmelden (SSO) über zahlreiche apps erhalten möchten, können denselben Schlüssel in jede Anwendung angeben und mit diesem Schlüssel die Xcode Berechtigungen anfordern. Dies wird in der Apple-Dokumentation erläutert.

Die übrigen Werte müssen zu der Bibliothek stellen Sie Werte für den Kontext.

### <a name="create-a-url-cache"></a>Erstellen Sie einen URL-cache

In `(void)viewDidLoad()`, die wird immer aufgerufen, nachdem die Ansicht geladen wurde, folgende Code Primzahlen Cache können.

Fügen Sie den folgenden Code hinzu:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>WebView-Anmeldung erstellen

Eine Webansicht kann Benutzereingabe für Faktoren wie SMS-Nachricht (falls konfiguriert) oder Fehlermeldungen an den Benutzer zurückgegeben. Sie legen hier die Webansicht einrichten und den Code, um die Rückrufe zu behandeln, die in der Webansicht aus der Identitätsdienste geschieht später schreiben.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Überschreiben Sie die Webansicht-Methoden für die Authentifizierung

Um die Webansicht mitzuteilen, was geschieht, wenn ein anmelden Anwender, wie bereits erwähnt, können Sie folgenden Code einfügen.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Schreiben Sie Code zum Behandeln der OAuth2 Ergebnis

Der folgende Code verarbeitet die RedirectURL, die aus der Webansicht zurückgibt. Authentifizierung nicht erfolgreich war, wird der Code erneut. Dagegen bietet die Bibliothek den Fehler, den in der Konsole angezeigt oder asynchron verarbeiten.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Einrichten von OAuth Kontext (Kontospeicher genannt)

Hier können Sie rufen `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` auf den freigegebenen Kontospeicher für jeden Dienst die Anwendung zugreifen können. Der Kontotyp ist eine Zeichenfolge, die als Bezeichner für einen bestimmten Dienst verwendet wird. Da die Graph-API zugreifen, bezieht sich der Code als `"myGraphService"`. Anschließend definieren Sie einen Beobachter, der Sie informiert, wenn etwas mit dem Token geändert wird. Nachdem Sie das Token erhalten, Sie der Benutzer zurück zu den `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Richten Sie die Masteransicht zur Suche der Benutzer die Graph-API

Eine Master-View-Controller (MVC)-Anwendung, die die zurückgegebenen Daten im Raster angezeigt wird, im Rahmen dieser exemplarischen Vorgehensweise und viele Onlinelernprogramme erläutert, wie Sie eines erstellen. Dieser Code ist in die Skelettdatei. Allerdings müssen Sie einige Dinge in MVC Anwendung behandeln:

* Wenn der Benutzer etwas in das Suchfeld eingibt abfangen
* Ein Objekt Daten an die MasterView bereitgestellt, damit die Ergebnisse im Raster angezeigt werden können

Wir werden die folgenden tun.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Fügen Sie eine Überprüfung, wenn Sie angemeldet sind

Die Anwendung ist wenig, wenn der Benutzer nicht angemeldet ist intelligent zu überprüfen, ob bereits ein Token im Cache ist. Andernfalls leiten, LoginView für den Benutzer anmelden. Zurückrufen am besten Aktionen beim Laden einer Ansicht ist mit der `viewDidLoad()` Methode, Apple uns bereitstellt.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>Die Tabellenansicht zu aktualisieren, wenn Daten empfangen werden

Wenn die Graph-API Daten zurückgibt, müssen Sie zum Anzeigen der Daten. Der Einfachheit halber ist hier der Code die Tabelle zu aktualisieren. Sie können die richtigen Werte der MVC-Standardcode einfügen.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Die Möglichkeit, die Graph-API aufrufen, wenn eine Person in das Suchfeld eingibt

Wenn ein Benutzer eine Suche im Feld eingibt, müssen Sie das Graph-API über schieben. Die `GraphAPICaller` -Klasse, die im folgenden Code erstellt wird, trennt die Funktionalität von der Präsentation. Jetzt schreiben wir den Code, der feeds Suchzeichen der Graph-API. Dazu wird eine Methode namens `lookupInGraph`, womit die Zeichenfolge gesucht werden soll.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Schreiben einer Hilfsklasse zum Diagramm-API zugreifen

Dies ist das Herzstück der Anwendung. Während der Rest wurde Code in die Standard-MVC-Muster von Apple einfügen schreiben hier Sie Code zum Diagramm während der Benutzereingabe abzufragen und diese Daten zurückgeben. Hier ist der Code, und eine ausführliche Erläuterung.

### <a name="create-a-new-objective-c-header-file"></a>Erstellen Sie eine neue Objective C-Headerdatei

Benennen Sie die Datei `GraphAPICaller.h`, und fügen Sie den folgenden Code hinzu.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Hier sehen Sie, dass eine angegebene Methode eine Zeichenfolge akzeptiert und gibt einen CompletionBlock zurück. Diese CompletionBlock wird wie Sie schon erraten haben vielleicht, die Tabelle aktualisieren, indem aufgefüllten Daten in Echtzeit sucht der Benutzer ein Objekt mit.


### <a name="create-a-new-objective-c-file"></a>Erstellen einer neuen Datei Objective C

Benennen Sie die Datei `GraphAPICaller.m`, und fügen Sie die folgende Methode hinzu.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

Lassen Sie uns diese Methode im Detail.

Das Kernstück dieses Codes ist den `NXOAuth2Request`, Methode nimmt die Parameter, die Sie bereits definiert haben in der Datei settings.plist.

Der erste Schritt ist den rechten Graph API-Aufruf. Da Sie aufrufen `/users`, angeben, durch Anfügen an die Graph-API Ressource zusammen mit der Version. Es ist sinnvoll, in einer externen Datei speichern, da diese API entwickelt sich ändern können.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Als Nächstes müssen Sie Parameter angeben, die auch für den Graph API-Aufruf bereitgestellt wird. Es ist *sehr wichtig* , dass Sie nicht die Parameter im Endpunkt Ressource da, für alle nicht-URI übereinstimmenden Zeichen zur Laufzeit bereinigt wird. Alle Abfragecode muss in den Parametern angegeben werden.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Bemerken Sie dazu einen `convertParamsToDictionary` Methode, die Sie noch nicht geschrieben haben. Führen wir nun am Ende der Datei:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Als Nächstes verwenden wir die `NXOAuth2Request` -Methode zum Abrufen von Daten aus der API im JSON-Format.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Abschließend sehen wie die Daten an die MasterViewController zurückzugeben. Die Daten gibt wie serialisiert und deserialisiert und in einem der MainViewController belegen kann Objekt geladen werden müssen. Zu diesem Zweck wurde das Skelett einer `User.m/h` Datei, ein Benutzerobjekt erstellt. Sie füllen dieses Benutzerobjekt mit Informationen aus dem Diagramm.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Führen Sie das Beispiel

Das Skelett verwendet oder mit der exemplarischen Vorgehensweise folgen sollten die Anwendung jetzt ausgeführt. Starten Sie den Simulator, und klicken Sie auf **Anmelden** , um diese Anwendung verwenden.

## <a name="get-security-updates-for-our-product"></a>Abrufen von Sicherheitsupdates für das Produkt

Wir empfehlen Ihnen Benachrichtigungen Sicherheitsvorfälle auftreten [Sicherheits-TechCenter](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.
