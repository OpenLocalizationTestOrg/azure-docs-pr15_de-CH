<properties
    pageTitle="Azure AD iOS Einstieg | Microsoft Azure"
    description="Erstellen eine iOS-Anwendung, die Integration mit Azure anmelden und ruft Azure AD geschützt APIs mit OAuth."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>IOS-App integrieren Sie Azure AD

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD bietet Active Directory-Authentifizierung Library oder ADAL, iOS-Clients, die auf geschützte Ressourcen zugreifen müssen.  Zweck des ADAL im Leben ist Ihre app Zugriffstoken zu erleichtern.  Um zu veranschaulichen, wie einfach es ist, bauen hier einer Liste Ziel C Anwendung, die wir:

-   Ruft zugreifen Token für das [Authentifizierungsprotokoll OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)Azure AD Graph-API aufrufen.
-   Sucht in einem Verzeichnis für Benutzer mit bestimmten Aliasnamen.

Um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Registrieren Sie Ihre Anwendung in Azure AD.
3. Installieren und Konfigurieren von ADAL.
5. ADAL von Azure AD Token zu verwenden.

Um zu beginnen, [das Skelett app](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) [herunterladen](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip)oder vollständiges Beispiel.  Sie benötigen auch einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Haben Sie bereits [erfahren Sie, wie man](active-directory-howto-tenant.md)Mieter.

> [AZURE.TIP] Testen Sie die Vorschau unsere neue [Entwicklerportal](https://identity.microsoft.com/Docs/iOS) , mit denen Sie die laufenden Azure Active Directory in wenigen Minuten abrufen und!  Entwicklerportal führt Sie durch den Prozess der Registrierung einer app und Azure AD in Code.  Wenn Sie fertig sind, haben Sie eine einfache Anwendung, die in Ihrem Mandanten und Backend-Benutzer authentifizieren, die Tokens akzeptieren und Validierung. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. Ermitteln der URI umleiten für iOS*

In bestimmten Szenarien SSO-Anwendung sicher starten müssen Sie einen **URI umleiten** in einem bestimmten Format erstellen. Ein URI umleiten wird zum Token an die Anwendung zurückgegeben, die Ihnen gestellt.

IOS-Format für einen URI umleiten ist:

```
<app-scheme>://<bundle-id>
```

-   **Aap Schema** - dies in XCode Projekt registriert ist. Es ist wie andere Programme aufrufen können. Finden Sie unter Info.plist -> URL-Typen -> URL-Bezeichner. Sie sollten eine erstellen, haben Sie bereits eine oder mehrere konfiguriert.
-   **Paket-Id** - Dies ist der Bundle-Bezeichner finden Sie unter "Identity" un projekteinstellungen XCode.

Beispiel für diesen Code QuickStart: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2 Registrieren Sie 2 die DirectorySearcher-Anwendung*
Um Ihre app Token zu aktivieren, müssen Sie zunächst in Azure AD-Mandanten registrieren und gewährt Zugriff auf Azure AD Graph-API:

-   Melden Sie sich bei Azure-Verwaltungsportal
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten in der Anwendung registriert.
-   Klicken Sie auf die Registerkarte **Applications** , und klicken Sie auf **Hinzufügen** in der unteren Schublade.
-   Befolgen Sie, und erstellen Sie eine neue **Systemeigene Clientanwendung**.
    -   Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.
    -   **Uri umleiten** ist eine Kombination von Schema und String, mit dem Azure AD token Antworten zurückgeben.  Geben Sie einen bestimmten Wert Ihrer Anwendung anhand der obigen Informationen.
-   Nach Abschluss Anmeldung zuweisen AAD app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten so kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Auch der Registerkarte **Konfigurieren** , suchen Sie den Abschnitt "Berechtigungen to Other Applications".  Fügen Sie für die Anwendung "Azure Active Directory" die Berechtigung **Verzeichnis Zugriff auf Ihre Organisation** **Delegiert**Berechtigungen hinzu.  Dadurch kann die Anwendung die Graph-API für Benutzer Abfragen.

## <a name="3-install--configure-adal"></a>*3. installieren und Konfigurieren von ADAL*
Jetzt haben Sie eine Anwendung in Azure AD können Sie ADAL installieren und identitätsbezogene Code schreiben.  Damit ADAL mit Azure kommunizieren müssen Sie einige Informationen über Ihre app-Registrierung.
-   Zunächst ADAL mit Cocapods DirectorySearcher-Projekt.

```
$ vi Podfile
```
Dieser Podfile wird Folgendes hinzufügen:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Laden Sie Cocoapods mit Podfile. Dadurch entsteht ein neuer XCode Arbeitsbereich geladen werden.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   Öffnen Sie im Projekt Schnellstart Plist-Datei `settings.plist`.  Ersetzen Sie die Werte der Elemente im Abschnitt zu den Werten, die in Azure-Portal eingeben.  Code verweist diese Werte, wenn ADAL verwendet.
    -   Die `tenant` ist die Domäne der Azure AD-Mandanten, z.B. contoso.onmicrosoft.com
    -   Die `clientId` ClientId der Anwendung aus dem Portal kopiert wird.
    -   Die `redirectUri` ist die Umleitung Url im Portal registriert.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. ADAL Token AAD zu verwenden*
Das Grundprinzip hinter ADAL ist, wenn Ihre Anwendung ein Zugriffstoken benötigt, einfach eine CompletionBlock aufgerufen `+(void) getToken : `, und ADAL übernimmt den Rest.  

-   In der `QuickStart` Projekt, `GraphAPICaller.m` und die `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` Kommentar oben.  Dies ist, wo Sie ADAL Koordinaten durch CompletionBlock Kommunikation mit Azure AD und sagen wie Token zwischengespeichert.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Nun müssen wir dieses Token verwenden, um Benutzer im Diagramm suchen. Suchen der `// TODO: implement SearchUsersList` CommentThis-Methode wird eine GET-Anforderung Azure AD Graph API Abfrage für Benutzer, deren UPN mit dem Suchbegriff beginnt.  Aber Abfragen Graph-API müssen Sie ein Zugriffstoken in der `Authorization` Header der Anforderung - Dadurch kommt ADAL.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

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
                         s.name =[keyValuePairs valueForKey:@"givenName"];

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
     }];

}

```
- Wenn Ihre app einen Token anfordert, durch Aufrufen von `getToken(...)`, ADAL versucht, ein Token zurück, ohne den Benutzer Anmeldeinformationen.  ADAL fest, dass der Benutzer muss sich anmelden, ein Token abzurufen, wird ein Anmeldedialogfeld angezeigt, die Anmeldeinformationen des Benutzers erfassen und ein Token nach erfolgreicher Authentifizierung.  Wenn ADAL einen Token aus irgendeinem Grund zurückgeben kann, löst eine `AdalException`.
- Beachten Sie, dass die `AuthenticationResult` -Objekt enthält eine `tokenCacheStoreItem` Objekt, mit Informationen Ihrer app müssen.  In den Schnellstart `tokenCacheStoreItem` wird verwendet, um festzustellen, ob Authenitcation bereits aufgetreten ist.


## <a name="step-5-build-and-run-the-application"></a>Schritt 5: Erstellen und Ausführen der Anwendungdes



Herzlichen Glückwunsch! Jetzt haben eine funktionierende iOS-Anwendung, mit der Benutzer authentifiziert, sicheres Aufrufen von APIs mit OAuth 2.0, und grundlegende Informationen über den Benutzer.  Wenn nicht bereits geschehen ist jetzt die Zeit Ihren Mandanten mit Benutzern aufgefüllt.  Der Schnellstart-Anwendung ausführen und dieser Benutzer anmelden.  Suchen Sie nach anderen Benutzern basierend auf ihrem UPN.  Die Anwendung schließen und erneut ausführen.  Beachten Sie, wie die Sitzung intakt bleibt.

ADAL erleichtert diese gemeinsamen Identität Features in Ihre Anwendung integrieren.  Er übernimmt alle Prozesse laufen für Cache-Verwaltung, OAuth Unterstützung Anmeldung UI aktualisieren abgelaufenen Token und mehr Benutzer präsentiert.  Wirklich wissen müssen lediglich einen einzigen API-Aufruf `getToken`.

Zu Referenzzwecken vollständiges Beispiel (ohne die Konfigurationswerte) bereitgestellt [hier](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Weitere Szenarien
Sie können zusätzliche Szenarien nun.  Möglicherweise möchten versuchen:

- [Sichere Node.JS-Web API mit Azure](active-directory-devquickstarts-webapi-nodejs.md)
- Informationen Sie [zum Aktivieren von SSO Cross-app für iOS mit ADAL](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
