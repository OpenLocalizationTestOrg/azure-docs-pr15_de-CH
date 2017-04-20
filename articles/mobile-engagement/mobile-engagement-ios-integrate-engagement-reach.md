<properties
    pageTitle="Azure Mobile Engagement iOS SDK erreichen Integration | Microsoft Azure"
    description="Neueste Updates und Verfahren für iOS SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Wie zu integrieren Engagement für iOS

Sie müssen die Integration [wie iOS Dokument zu integrieren](mobile-engagement-ios-integrate-engagement.md) , bevor Sie dieses Handbuch beschriebenen Verfahren.

Diese Dokumentation ist XCode 8 erforderlich. Wenn Sie wirklich XCode 7 abhängig können Sie [iOS v3.2.4 Engagement SDK](https://aka.ms/r6oouh)verwenden. Es ist ein bekanntes Problem in dieser vorherigen Version bei unter iOS 10 Geräte: systembenachrichtigungen werden nicht verarbeitet. Zur Behebung dieses Problems Sie veraltete API implementieren müssen `application:didReceiveRemoteNotification:` in Ihrer Anwendung delegieren wie folgt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Dies **nicht empfohlen, diese Lösung** können alle anstehenden (auch kleine) iOS Version Upgrade nicht ändern, da diese iOS API veraltet ist. Sie sollten möglichst bald XCode 8 wechseln.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Aktivieren Sie Ihre app automatische Push Benachrichtigungen

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Schritte zur Datenintegration

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>Engagement erreichen SDK in iOS Projekt einbetten

-   Das Reach-Sdk im Xcode Projekt hinzufügen. Wechseln Sie in Xcode zu **Projekt \> zu Projekt hinzufügen** und die `EngagementReach` Ordner.

### <a name="modify-your-application-delegate"></a>Ändern Sie Ihre Anwendung Stellvertretung

-   Importieren Sie am Anfang der Implementierung Modul Engagement erreichen:

        [...]
        #import "AEReachModule.h"

-   In Methode `applicationDidFinishLaunching:` oder `application:didFinishLaunchingWithOptions:`, ein Reach-Modul erstellt und zu Ihrem vorhandenen Engagement Initialisierung übergeben:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   Bearbeiten Sie **'icon.png'** Zeichenfolge als Ihr Symbol gewünschten Image-Namen
-   Möchten Sie die Option *Update Badge Wert* erreichen Kampagnen oder systemeigenen Push soll \<SaaS-Reichweite API Kampagne Format/Native Push\> Kampagnen müssen Modul verwalten das Badge Symbol (es automatisch deaktivieren Anwendung Badge und auch jedes Mal die Anwendung gestartet oder Vordergrund Engagement gespeicherten Wert zurücksetzen) erreichen können. Dies erfolgt durch Hinzufügen der folgenden Codezeile nach der Initialisierung des Moduls erreichen:

        [reach setAutoBadgeEnabled:YES];

-   Ggf. Reichweite datenpush behandeln sollten Sie Ihre Anwendung Delegaten entsprechen den `AEReachDataPushDelegate` Protokoll. Fügen Sie die folgende Zeile nach der Initialisierung des Moduls erreichen:

        [reach setDataPushDelegate:self];

-   Die Methoden implementieren können `onDataPushStringReceived:` und `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` in Ihrer Anwendung Stellvertretung:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Kategorie

Der Kategorieparameter ist beim Erstellen einer Kampagne Daten optional und legt Sie Filterdaten kann. Dies ist hilfreich, wenn verschiedene push von `Base64` Daten und Typ identifizieren, bevor sie analysieren möchten.

**Die Anwendung kann jetzt angezeigt und Reichweite Inhalte!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Ankündigungen und Umfragen jederzeit empfangen

Engagement kann Reichweite für Ihre Endbenutzer jederzeit Benachrichtigungen mit Apple Push Notification Service.

Um diese Funktion zu aktivieren, müssen Sie Ihre Anwendung Stellvertretung zu bereiten Sie Ihre Anwendung für Apple Pushbenachrichtigungen.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Bereiten Sie Ihre Anwendung für Apple Pushbenachrichtigungen

Bitte folgen Sie die Anleitung: [Vorbereiten die Anwendung für Apple Pushbenachrichtigungen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>Hinzufügen des erforderlichen Client-Codes

*An diesem Punkt müssen die Anwendung ein registriertes Apple pushzertifikat im Frontend Engagement.*

Wenn nicht bereits erfolgt, müssen Sie die Anwendung zum Empfangen von Pushbenachrichtigungen registrieren.

* Importieren der `User Notification` Rahmen:

        #import <UserNotifications/UserNotifications.h>

* Fügen Sie folgende Zeile beim Start der Anwendung (in der Regel in `application:didFinishLaunchingWithOptions:`):

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

Dann müssen Sie zu bieten gerätetoken von Apple-Server zurückgegeben. Dies geschieht in der Methode mit dem Namen `application:didRegisterForRemoteNotificationsWithDeviceToken:` in Ihrer Anwendung Stellvertretung:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Schließlich haben Sie Engagement SDK informieren, wenn Ihre Anwendung remote benachrichtigt. Dazu rufen Sie die Methode `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` in Ihrer Anwendung Stellvertretung:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] IOS 7 die obige Methode eingeführt. Wenn Sie iOS abzielen < 7 sicher Methode implementieren `application:didReceiveRemoteNotification:` in Ihre Anwendung Delegaten und rufen `applicationDidReceiveRemoteNotification` auf EngagementAgent durch das Übergeben von NULL anstelle der `handler` Argument:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Standardmäßig steuert das Engagement erreichen der CompletionHandler. Ggf. manuell Bearbeiten der `handler` im Code blockieren, übergeben Sie NULL für die `handler` Argument und Kontrolle der Abschluss selbst blockieren. Siehe die `UIBackgroundFetchResult` Typ eine Liste möglicher Werte.


### <a name="full-example"></a>Vollständiges Beispiel

Hier ist ein vollständiges Beispiel der Integration:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Haben Sie eine eigene Implementierung UNUserNotificationCenterDelegate

Das SDK enthält auch eine eigene Implementierung des Protokolls UNUserNotificationCenterDelegate. Es wird von SDK zum Lebenszyklus Engagement Benachrichtigungen auf Geräten unter iOS 10 oder höher überwacht. Wenn das SDK Ihre Stellvertretung nicht ihre eigene Implementierung verwenden werden erkennt, denn nur ein UNUserNotificationCenter-Delegat pro Anwendung. Dies bedeutet, dass Sie eigene Delegaten Engagement Logik hinzufügen.

Es gibt zwei Wege.

Durch Ihre Stellvertretung weiterleiten Ruft das SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Oder durch Vererbung von der `AEUserNotificationHandler` Klasse

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Sie können bestimmen, ob eine Benachrichtigung aus oder nicht übergeben wird seine `userInfo` Wörterbuch der Agent `isEngagementPushPayload:` class-Methode.

##<a name="how-to-customize-campaigns"></a>Anpassen von Kampagnen

### <a name="notifications"></a>Benachrichtigung

Es gibt zwei Arten von Benachrichtigungen: Benachrichtigungen System und in der Anwendung.

Systembenachrichtigungen von iOS behandelt und können nicht angepasst werden.

In app-Benachrichtigungen bestehen aus einer Ansicht, die der aktuellen Anwendungsfensters dynamisch hinzugefügt wird. Dies ist eine Benachrichtigung Überlagerung bezeichnet. Benachrichtigung Overlays sind für eine schnelle Integration, da sie müssen Sie keine Ansicht in der Anwendung ändern.

#### <a name="layout"></a>Layout

Ändern Sie Benachrichtigungen innerhalb der app suchen Sie die Datei einfach ändern `AENotificationView.xib` an Ihre Bedürfnisse, solange die Variablenwerte und vorhandenen Unteransichten beibehalten.

Standardmäßig werden in app-Benachrichtigungen am unteren Bildschirmrand angezeigt. Möchten Sie am oberen Bildschirmrand angezeigt, bearbeitet die `AENotificationView.xib` und die `AutoSizing` -Eigenschaft der Hauptansicht so am oberen Rand der Superview aufbewahrt werden können.

#### <a name="categories"></a>Kategorien

Beim Ändern des bereitgestellten Layouts ändern der Darstellung von der Benachrichtigung. Kategorien können Sie verschiedene gezielte sucht (möglicherweise Verhalten) für Benachrichtigungen definieren. Eine Kategorie kann beim Erstellen einer Kampagne Reichweite angegeben. Denken Sie daran Kategorien können auch Ankündigungen und Umfragen, anpassen, die in diesem Dokument beschrieben.

Registrieren Sie einen Kategorie-Handler für Benachrichtigungen müssen Sie nach der Initialisierung des Reach-Moduls einen Aufruf hinzufügen.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`muss eine Instanz eines Objekts, das dem Protokoll entspricht `AENotifier`.

Implementieren Sie die Protokollmethoden selbst oder Sie können die vorhandene Klasse implementieren `AEDefaultNotifier` das bereits die meisten Aufgaben ausführt.

Möchten Sie Benachrichtigung für eine bestimmte Kategorie neu zu definieren, können Sie dieses Beispiel ausführen:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Dieses einfache Beispiel Kategorie davon aus, dass eine Datei mit dem Namen `MyNotificationView.xib` in Ihrer Anwendung Bündel. Ist die Methode nicht können Sie ein entsprechendes `.xib`die Benachrichtigung nicht angezeigt und Engagement eine Meldung in der Konsole ausgegeben.

Bereitgestellte Nib-Datei sollte die folgenden Regeln beachten:

-   Es sollte nur eine Ansicht.
-   Unteransichten sollten von den gleichen Typen wie in der Datei bereitgestellten Nib benannt werden`AENotificationView.xib`
-   Unteransichten müssen dieselben Transponder als die in der bereitgestellten Datei Nib`AENotificationView.xib`

> [AZURE.TIP] Kopieren Sie einfach die bereitgestellten Nib-Datei mit dem Namen `AENotificationView.xib`, und dort arbeiten. Aber Vorsicht, die Ansicht in dieser Nib-Datei der Klasse zugeordnet ist `AENotificationView`. Diese Klasse definiert die Methode `layoutSubViews` und Größe der Unteransichten Kontext. Möchten Sie es Ersetzen einer `UIView` oder Sie benutzerdefinierte Ansicht.

Benötigen Sie tiefer Anpassung von Benachrichtigungen (soll beispielsweise die Ansicht direkt aus dem Code laden), empfiehlt sich, einen Blick auf die bereitgestellte Quelle und Dokumentation `Protocol ReferencesDefaultNotifier` und `AENotifier`.

Beachten Sie, dass Sie den gleichen Notifier für mehrere Kategorien.

Außerdem können neu definiert Notifier standardmäßig wie folgt:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Benachrichtigung behandeln

Verwendung die Standardkategorie nennt einige Lebenszyklusmethoden auf der `AEReachContent` -Objekt Statistiken und den Status der Kampagne aktualisieren:

-   Wenn die Benachrichtigung in der Anwendung angezeigt wird die `displayNotification` -Methode (die Statistik meldet) von `AEReachModule` Wenn `handleNotification:` gibt `YES`.
-   Wenn die Benachrichtigung geschlossen wird, die `exitNotification` -Methode aufgerufen wird, Statistik gemeldet und nächste Kampagnen können nun bearbeitet werden.
-   Wenn die Benachrichtigung geklickt haben, `actionNotification` ist, Statistik gemeldet und die zugeordnete Aktion wird ausgeführt.

Wenn die Implementierung von `AENotifier` das Standardverhalten umgeht diese Lebenszyklusmethoden selbst aufrufen müssen. Die folgenden Beispiele veranschaulichen einige Fälle, in denen das Standardverhalten umgangen wird:

-   Erweitern von nicht `AEDefaultNotifier`, z. B. Kategorie Behandlung völlig implementiert.
-   Sie überschrieben hat `prepareNotificationView:forContent:`, müssen Sie mindestens zugeordnet `onNotificationActioned` oder `onNotificationExited` zu der Benutzeroberfläche gesteuert.

> [AZURE.WARNING] Wenn `handleNotification:` löst eine Ausnahme der Inhalt gelöscht und `drop` ist, diese Statistiken gemeldet wird und nächste Kampagnen können nun bearbeitet werden.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Benachrichtigung als Teil einer vorhandenen Ansicht einschließen

Overlays sind für eine schnelle Integration aber können manchmal nicht praktisch oder unerwünschten Nebenwirkungen haben.

Wenn Sie nicht mit der Überlagerung in einigen Ansichten zufrieden sind, können Sie ihn für diese Ansichten anpassen.

Sie können unsere Benachrichtigung Layout in vorhandenen Ansichten enthalten. Hierzu gibt es zwei Implementierung Stile:

1.  Fügen Sie Benachrichtigung anzeigen mit Interface Builder hinzu

    -   *Interface Builder* öffnen
    -   Platzieren Sie 320 x 60 (oder 768 x 60 auf iPad) `UIView` , die Benachrichtigung angezeigt werden soll
    -   Legen Sie den Tag-Wert für diese Ansicht: **36822491**

2.  Benachrichtigung anzeigen programmgesteuert hinzufügen. Fügen Sie folgenden Code nur, wenn die Ansicht initialisiert wurde:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`Makro finden Sie unter `AEDefaultNotifier.h`.

> [AZURE.NOTE] Der standardmäßige Notifier erkennt automatisch, Layout Benachrichtigung in dieser Ansicht enthaltenen Overlay wird es nicht hinzugefügt.

### <a name="announcements-and-polls"></a>Ankündigungen und Umfragen

#### <a name="layouts"></a>Layouts

Sie können die Dateien `AEDefaultAnnouncementView.xib` und `AEDefaultPollView.xib` als die Variablenwerte und vorhandenen Unteransichten beibehalten.

#### <a name="categories"></a>Kategorien

##### <a name="alternate-layouts"></a>Alternative layouts

Wie Benachrichtigungen kann die Kampagne Kategorie alternative Layouts für Produkt- und Umfragen müssen verwendet werden.

Zum Erstellen einer Kategorie für eine Ankündigung müssen **AEAnnouncementViewController** erweitern und es nach das Reach-Modul initialisiert wurde:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Jedes Mal ein Benutzer auf eine Benachrichtigung für eine Ankündigung mit der Kategorie klickt "Mein\_Kategorie", Ihren registrierten View Controller (in diesem Fall `MyCustomAnnouncementViewController`) werden durch Aufrufen der Methode initialisiert `initWithAnnouncement:` und die Ansicht des aktuellen Anwendungsfensters hinzugefügt.

Die Implementierung der `AEAnnouncementViewController` Klasse Sie die Eigenschaft gelesen müssen `announcement` die Unteransichten initialisiert werden. Im Beispiel unten, wo zwei Etiketten mit initialisiert werden `title` und `body` Eigenschaften der `AEReachAnnouncement` Klasse:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Wenn Ansichten selbst laden möchten jedoch nur das Standardlayout Ankündigung Ansicht wiederverwenden möchten, können Sie einfach Gamecontrollers benutzerdefinierte Ansicht erweitert die bereitgestellte Klasse `AEDefaultAnnouncementViewController`. In diesem Fall doppelter Nib-Datei `AEDefaultAnnouncementView.xib` und benennen Sie sie so Ihre benutzerdefinierte Ansicht Controller geladen werden kann (für einen Controller mit dem Namen `CustomAnnouncementViewController`, rufen Sie die Nib-Datei `CustomAnnouncementView.xib`).

Ersetzen Sie die Standardkategorie Ankündigungen registrieren Controllers benutzerdefinierte Ansicht für die Kategorie gemäß `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Umfragen können genauso angepasst werden:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Dieses Mal sind die `MyCustomPollViewController` muss `AEPollViewController`. Oder Sie können vom Standard-Controller erweitern: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Vergessen Sie nicht, rufen Sie `action` (`submitAnswers:` für benutzerdefinierte Abfrage View Controller) oder `exit` Methode View Controller entlassen. Andernfalls Statistiken wird nicht gesendet werden (d. h. keine Analyse für die Kampagne) und weitere wichtiger nächsten Kampagnen nicht benachrichtigt erst nach einem der Anwendungsprozess Neustart.

##### <a name="implementation-example"></a>Beispiel für die Implementierung

In dieser Implementierung wird die Ankündigung benutzerdefinierte Ansicht aus einer externen Xib-Datei geladen.

Wie Voranmeldung Anpassung empfiehlt es sich den Quellcode der standard-Implementierung.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
