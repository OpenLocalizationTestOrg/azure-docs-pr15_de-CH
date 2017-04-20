<properties
    pageTitle="Azure Active Directory B2C: Rufen Sie Web-API eine iOS Anwendung Drittanbieterbibliotheken | Microsoft Azure"
    description="Dieser Artikel zeigt Ihnen wie eine iOS-app "Aufgabenliste" erstellen, die Node.js-Web-API aufruft, mit OAuth 2.0 trägertoken mithilfe einer Drittanbieter-Bibliothek"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< tags "Active Directory b2c" ms.service= ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "Helden-Artikel"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD B2C: Rufen Sie Web-API eine iOS-Anwendung mithilfe einer Drittanbieter-Bibliothek

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Microsoft Identitätsplattform verwendet offene Standards wie OAuth2 OpenID verbinden. Dadurch können Entwickler jede Bibliothek nutzen sie unsere Services integrieren möchten. Um Entwickler bei der Verwendung unserer Plattform mit anderen Bibliotheken haben wir einige Exemplarische Vorgehensweisen wie diese demonstrieren geschrieben Bibliotheken von Drittanbietern für die Verbindung zur Microsoft-Identitätsplattform konfigurieren. Bibliotheken, die [RFC6749 OAuth2-Spezifikation](https://tools.ietf.org/html/rfc6749) implementieren können die Plattform Microsoft Identity herstellen.


Wenn Sie neu OAuth2 oder OpenID kann dieses Beispiel-Konfiguration nicht viel Ihnen sinnvoll. Wir empfehlen eine kurze [Übersicht über das Protokoll, das wir hier dokumentiert haben](active-directory-b2c-reference-protocols.md)betrachten.

> [AZURE.NOTE]
    Einige Features der Plattform, die einen Ausdruck in diese Standards wie Zugangskontrolle und Intune Richtlinienmanagement erfordern unsere offene Quelle Microsoft Azure Identität Bibliotheken. 
   
Nicht alle Azure Active Directory Szenarien und Features werden von B2C-Plattform unterstützt.  Um festzustellen, ob die B2C-Plattform verwenden sollten, lesen Sie [B2C-Einschränkungen](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Ein Azure AD B2C-Verzeichnis abrufen

Vor der Verwendung Azure AD B2C müssen Sie ein Verzeichnis oder Mieter. Ein Verzeichnis ist ein Container für alle Benutzer, apps und Gruppen. Haben Sie eine bereits Fortfahren [ein B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine Anwendung in Ihrem B2C-Verzeichnis erstellen. Dies informiert Azure AD, um die sichere Kommunikation mit Ihrer Anwendung. Die app und Web-API werden durch eine einzelne **Anwendung ID** in diesem Fall dargestellt da bestehen aus einer logischen Anwendung. [Gehen folgendermaßen Sie vor, um eine Anwendung zu erstellen.](active-directory-b2c-app-registration.md) Achten Sie darauf,:

- Enthalten Sie ein **Gerät** in der Anwendung.
- Kopieren Sie die **ID der Anwendung** , die Ihre Anwendung zugewiesen ist. Sie auch benötigen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen von Richtlinien

In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese Anwendung enthält eine Identität Erfahrung: kombinierten Zeichen in und Anmeldung. Sie müssen diese Richtlinie jedes Typs erstellen gemäß [Richtlinie Referenzartikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wenn Sie die Richtlinie erstellen, müssen Sie:

- Der **Anzeigename** und die Anmeldung Attribute in Ihrer Richtlinie auswählen
- Ansprüche Anwendung **angezeigten Namen** und die **Objekt-ID** in jeder Richtlinie auswählen Sie können auch andere Ansprüche.
- Kopieren Sie den **Namen** jeder Richtlinie nach ihrer Erstellung. Das Präfix muss `b2c_1_`.  Sie benötigen später den Namen der Richtlinie.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie Ihre Richtlinien erstellt haben, können Sie Ihre app erstellen.


## <a name="download-the-code"></a>Den Code herunterladen

Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)verwaltet.  Um folgen, können Sie /archive/master.zip [Herunterladen als .zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)) oder Klonen Sie es:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Nur den vollständige Code herunterladen und loslegen: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Drittanbieter-Bibliothek nxoauth2 herunterladen und Ausführen eines Arbeitsbereichs

Für diese exemplarische Vorgehensweise verwenden wir den OAuth2Client von GitHub, ein OAuth2-Library für Mac OS X und iOS (Kakao und Kakao berühren). Diese Bibliothek basiert auf Entwurf 10 OAuth2-Spezifikation. Es implementiert das systemeigene Anwendungsprofil und Endbenutzer autorisierungsendpunkt unterstützt. Dies sind alles müssen wir zu Microsoft Identity Plattform integriert.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>Hinzufügen der Bibliothek mit CocoaPods

CocoaPods ist ein Abhängigkeit Xcode Projekte. Die Installationsschritte verwaltet automatisch.

```
$ vi Podfile
```
Dieser Podfile wird Folgendes hinzufügen:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Laden Sie Cocoapods mit Podfile. Dadurch entsteht ein neuer XCode Arbeitsbereich geladen werden.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>Die Struktur des Projekts

Wir haben die folgende Struktur für das Projekt im Skelett einrichten:

* Eine **Masteransicht** mit einem Aufgabenbereich
* Eine **Vorgangsansicht hinzufügen** der Daten zu den ausgewählten Vorgang
* Eine **Anmeldung anzeigen** , die ein Benutzer der Anwendung anmelden kann.

Wir springt in verschiedenen Dateien im Projekt Authentifizierung hinzuzufügen. Andere Teile des Codes wie den visual-Code ist nicht für Identität und für Sie bereitgestellt.

## <a name="create-the-settingsplist-file-for-your-application"></a>Erstellen der `settings.plist` -Datei für Ihre Anwendung

Es ist einfacher, die Anwendung konfigurieren, haben wir eine zentrale unsere Werte zu. Es hilft Ihnen zu verstehen, was jeder Einstellung in Ihrer Anwendung. Die *Eigenschaftenliste* nutzen wir auf diese Werte an die Anwendung.

* Erstellen/Öffnen der `settings.plist` Datei unter `Supporting Files` in Ihrem Arbeitsbereich der Anwendung

* Geben Sie die folgenden Werte (wir Sie ausführlich schnell gehen)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Jetzt rufen Sie ausführlich.


Für `authURL`, `loginURL`, `bhh`, `tokenURL` sehen Sie müssen Ihren Mandanten ein. Dies ist der Mieter Name der B2C-Mandanten, die Ihnen zugewiesen wurde. Beispielsweise `kidventusb2c.onmicrosoft.com`. Unsere offene Quelle Microsoft Azure Identität Bibliotheken verwenden wir diese Daten mit unseren Endpunkt ziehen nach unten. Wir haben die Festplatte extrahieren diese Werte für Sie erledigt.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Die `keychain` Wert ist der Container, der mithilfe der NXOAuth2Client-Bibliothek Schlüsselbund Token speichern zu erstellen. SSO zahlreiche Apps abrufen möchten können Sie denselben Schlüssel in jede Anwendung angeben sowie die XCode-Entitements die Verwendung dieser Schlüssel anfordern. Dies wird in der Apple-Dokumentation behandelt.

Die `<policy name>` am Ende jeder URL sind die Orte, wo, die oben erstellte Richtlinie. Die Anwendung ruft diese Richtlinien je nach.

Die `taskAPI` den REST-Endpunkt Wir rufen Ihre B2C-Token entweder hinzufügen oder Abfrage vorhandenen Aufgaben. Dies wurde speziell für dieses Beispiel eingerichtet. Sie müssen das Beispiel zu ändern.

Die übrigen Werte müssen zu der Bibliothek einfach stellen Sie Werte für den Kontext.

Jetzt haben wir die `settings.plist` erstellte Datei, wir brauchen lesen.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Einrichten einer Klasse AppData unsere gelesen

Lassen Sie eine einfache Datei, die gerade analysiert unsere `settngs.plist` Datei wir oben erstellten und in Zukunft die verfügbaren Einstellungen für jede Klasse. Da wir eine neue Kopie der Daten bei jedem eine Klasse fordert erstellen möchten, werden wir ein Singleton-Muster verwenden und nur die gleiche Instanz erstellt, wenn eine für die Einstellung Anforderung

* Erstellen einer `AppData.h` Datei:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Erstellen einer `AppData.m` Datei:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Wir jetzt problemlos in unserem rufen einfach `  AppData *data = [AppData getInstance];` in unsere Kurse Sie sehen unten.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>NXOAuth2Client Bibliothek in Ihr AppDelegate einrichten

Die NXOAuthClient muss einige Werte um. Danach können Sie Token verwenden, die sich die REST-API aufrufen. Da wir, dass wissen die `AppDelegate` heißen wir die Anwendung laden immer sinnvoll, daß wir unsere Werte in die Datei.
* Open `AppDelegate.m` Datei

* Importieren Sie einige Headerdateien, die wir später verwenden werden.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Hinzufügen der `setupOAuth2AccountStore` -Methode in der AppDelegate

Müssen wir eine AccountStore erstellen und füttern wir nur von gelesenen Daten in den `settings.plist` Datei.

Es gibt einige Dinge, denen Sie zum B2C-Dienst an diesem Punkt beachten, die diesen Code verständlicher machen:


1. Azure AD B2C verwendet die *Richtlinie* von den Abfrageparametern Ihre Anfrage. Dadurch Azure Active Directory fungiert als unabhängiger Dienst für Ihre Anwendung. Um bieten diese zusätzliche Abfrageparameter wir müssen die `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` mit unserer benutzerdefinierten Parameter. 

2. Azure AD B2C verwendet Bereiche im Wesentlichen genauso wie andere Server OAuth2. Aber da B2C zum Authentifizieren eines Benutzers Zugriff auf Ressourcen ist einige Bereiche absolut Flow ordnungsgemäß erforderlich sind. Dies ist die `openid` Bereich. Unsere Microsoft Identity SDKs automatisch Bereitstellen der `openid` Bereich für Sie, die im SDK-Konfiguration angezeigt wird. Da wir eine Drittanbieter-Bibliothek verwenden, müssen wir jedoch dieses Bereichs angeben.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Stellen Sie nun sicher rufen in AppDelegate unter `didFinishLaunchingWithOptions:` Methode. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Erstellen einer `LoginViewController` Klasse, die mit dem Authentifizierung Anfragen

Wir verwenden eine Webansicht für Konto anmelden. Dies ermöglicht die Benutzereingabe für Faktoren wie SMS-Nachricht (falls konfiguriert) oder geben Fehlermeldungen an den Benutzer zurück. Hier legen wir die Webansicht einrichten und den Code, um die Rückrufe zu behandeln, die in der Webansicht von Microsoft Identity Service geschieht später schreiben.

* Erstellen einer `LoginViewController.h` Klasse

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Wir erstellen diese Methoden unten.

> [AZURE.NOTE] 
    Stellen Sie sicher, dass Sie binden der `loginView` an der tatsächlichen Webview Storyboard befindet. Andernfalls müssen Sie eine Webansicht nicht, die beim Authentifizieren ist auftauchen können.

* Erstellen einer `LoginViewController.m` Klasse

* Fügen Sie einige Variablen Zustand tragen wir Authentifizierung hinzu

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Überschreiben Sie die Webansicht-Methoden für die Authentifizierung

Wir müssen die Webansicht das Verhalten wollen wir beim Anwender sich wie oben beschrieben. Sie können einfach Ausschneiden und fügen Sie den folgenden Code.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Schreiben Sie Code zum Behandeln der OAuth2 Ergebnis

Wir benötigen Code, die die RedirectURL behandelt, die von der Webansicht zurückkehrt. Wenn es nicht erfolgreich war, wird es erneut. Inzwischen bietet die Bibliothek den Fehler, finden Sie in der Konsole oder Asyncronously behandeln. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Factorys Benachrichtigung einrichten.

Wir erstellen wir haben dieselbe Methode der `AppDelegate` vor, aber dieses Mal, wir fügen einige `NSNotification`s, um uns mitzuteilen, was geschieht in unseren Service. Wir richten ein Beobachter, der uns sagen, wann mit der Änderung. Angekommen Token zurück wir den Benutzer an der `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Hinzufügen von Code, den Benutzer behandelt, wenn eine Anforderung für systemeigenen Zeichen ausgelöst wird

Erstellen Sie eine Methode, die aufgerufen wird, wenn wir eine Anforderung für die Authentifizierung haben. Der Methode werden die Webview erstellt

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Schließlich nennen diese Methoden, die wir über jedes Mal geschrieben haben die `LoginViewController` geladen. Wir dazu Verfahren unserer `viewDidLoad` Methode Apple erhalten

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Erstellen die Hauptmethode, wie wir mit unserer Anwendung anmelden wird jetzt beendet. Nachdem wir angemeldet haben, müssen wir unsere Token verwenden wir erhalten. Dafür erstellen wir Hilfscode, die wir mit dieser Bibliothek REST-APIs aufrufen.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Erstellen einer `GraphAPICaller` Klasse unserer Anfragen eine REST-API

Wir haben eine Konfiguration jedes Mal wir unsere Anwendung laden geladen. Jetzt müssen wir etwas haben wir einen Token. 

* Erstellen einer `GraphAPICaller.h` Datei

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Siehe Code wir zwei Methoden erstellt werden: eine, die Aufgaben von einer API und die API Aufgaben hinzufügen.

Da wir unsere Schnittstelle eingerichtet haben, fügen Sie die eigentliche Implementierung:

* Erstellen einer`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>Führen Sie die Beispiel-app

Schließlich erstellen Sie, und führen Sie die Anwendung in Xcode. Registrieren Sie oder melden Sie an, um die Anwendung und erstellen Sie Aufgaben für einen angemeldeten Benutzer. Melden Sie ab und melden Sie als anderer Benutzer an und erstellen Sie Aufgaben für den Benutzer.

Beachten Sie, dass Aufgaben gespeicherte benutzerdefinierte API, da die API die Identität des Benutzers aus dem Zugriffstoken extrahiert, die er erhält.


## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt auf B2C Themen verschieben. Sie können Folgendes versuchen:

[Rufen Sie Node.js Web API Node.js Web app]()

[Anpassen der UX für B2C-Anwendung]()
