<properties
    pageTitle="Azure Mobile Engagement iOS SDK Integration | Microsoft Azure"
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

#<a name="how-to-integrate-engagement-on-ios"></a>Wie integrieren Engagement für iOS

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Dieses Verfahren beschreibt die einfachsten Projekts Analyse und Überwachung der iOS-Anwendung aktivieren.

Engagement-SDK erfordert iOS6 und Xcode 8: das Deployment-Ziel der Anwendung muß mindestens iOS 6.

> [AZURE.NOTE]
> Wenn Sie wirklich XCode 7 abhängig können Sie [iOS v3.2.4 Engagement SDK](https://aka.ms/r6oouh)verwenden. Es ist ein bekanntes Problem des Moduls erreichen dieser vorherigen Version auf iOS 10 Geräte [Integration Modul Reichweite](mobile-engagement-ios-integrate-engagement-reach.md) für Weitere Informationen sehen. Möchten Sie v3.2.4 SDK verwenden und Überspringen der `UserNotifications.framework` im nächsten Schritt importieren.

Die folgenden Schritte sind ausreichend Bericht alle Statistiken über Benutzer, Sitzung, Aktivitäten, Abstürze und technische berechnet erforderlichen Protokolle aktivieren. Bericht über Protokolle benötigt weitere Statistiken wie Ereignisse, Fehler und Aufträge berechnen erfolgt manuell mit Engagement-API (siehe [wie der erweiterte Mobile Engagement tagging-API in iOS-app](mobile-engagement-ios-use-engagement-api.md) seit Anwendung abhängig sind.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>Engagement-SDK in iOS Projekt einbetten

- IOS SDK [hier](http://aka.ms/qk2rnj)herunterladen

- IOS-Projekt Engagement SDK hinzufügen: in Xcode, klicken Sie mit der rechten Maustaste auf das Projekt und wählen Sie **"Dateien hinzufügen..."** und wählen Sie die `EngagementSDK` Ordner.

- Engagement erfordert zusätzliche Frameworks arbeiten: im Projekt-Explorer öffnen Sie Ihr im Projektbereich, und wählen Sie das richtige Ziel. Dann öffnen Sie die Registerkarte **"Phasen erstellen"** , und fügen Sie im Menü **"Link Binary mit Libraries"** dieser Frameworks:

    -   `UserNotifications.framework`-Legen Sie den Link`Optional`
    -   `AdSupport.framework`-Legen Sie den Link`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] AdSupport Rahmen kann entfernt werden. Engagement benötigt dieses Framework, um die IDFA zu sammeln. IDFA Auflistung kann jedoch deaktiviert werden \<Ios-Sdk-Engagement-Idfa\> die neue Apple Richtlinie bezüglich dieser ID

##<a name="initialize-the-engagement-sdk"></a>Initialisieren des Projekts SDK

Sie müssen Ihre Anwendung Stellvertretung ändern:

-   Importieren Sie am Anfang der Implementierung Engagement-Agent:

        [...]
        #import "EngagementAgent.h"

-   Initialisieren von Engagement in der Methode '**ApplicationDidFinishLaunching:**"oder"**Anwendung: DidFinishLaunchingWithOptions:**":

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Grundlegende Berichterstattung

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Empfohlen: Überladen der `UIViewController` Klassen

Um Bericht über alle erforderlichen Engagement Benutzer Sessions, Aktivitäten, Abstürze und technische Statistiken berechnet Protokolle aktivieren einfach können Sie alle Ihre `UIViewController` untergeordnete Klassen erben von der `EngagementViewController` Klassen (dieselbe Regel für `UITableViewController`  - \> `EngagementTableViewController`).

**Ohne:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Mit:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternative Methode: Rufen Sie `startActivity()` manuell

Wenn nicht oder nicht überladen möchten Ihre `UIViewController` Klassen können stattdessen starten Ihre Aktivitäten Aufrufen `EngagementAgent`Methoden direkt.

> [AZURE.IMPORTANT] IOS SDK automatisch Ruft die `endActivity()` -Methode auf, wenn die Anwendung geschlossen wird. Daher ist es *sehr* empfehlenswert, rufen Sie die `startActivity` Methode, wenn die Aktivität des Benutzers ändern und *nie* Aufrufen der `endActivity` -Methode, da diese Methode aufzurufen die aktuelle Sitzung erzwingt beendet.

##<a name="location-reporting"></a>Standortangabe

Anwendung des Druckerstandorts Statistiken nur für Zweck zulassen Apple Vertragsbedingungen nicht. Daher wird empfohlen, Position Berichte nur aktivieren, wenn die Anwendung auch die Verfolgung des Druckerstandorts aus einem anderen Grund.

IOS 8 ab, müssen Sie eine Beschreibung für Ihre app Diensten Verwendung durch Festlegen einer Zeichenfolge für den Schlüssel [NSLocationWhenInUseUsageDescription] oder [NSLocationAlwaysUsageDescription] in Ihrer Anwendung Info.plist Datei angeben. Wenn Sie zum Speicherort des Berichts im Hintergrund mit, Hinzufügen des Schlüssels NSLocationAlwaysUsageDescription. Fügen Sie in allen anderen Fällen den Schlüssel NSLocationWhenInUseUsageDescription.

### <a name="lazy-area-location-reporting"></a>Träge Bereich Standortangabe

Träge Bereich Position reporting ermöglicht, Land, Region und Ort Geräte zugeordnet. Diese Standortangabe verwendet nur Netzwerkadressen (basierend auf Zelle ID oder WIFI). Bereich wird höchstens einmal pro Sitzung gemeldet. GPS wird niemals verwendet und daher dieser Position Bericht verfügt über wenige (nicht, Nein) Auswirkung auf den Akku.

Gemeldete Bereichen werden verwendet, um geografische Statistiken zu Benutzern, Sitzungen, Ereignissen und Fehlern berechnen. Sie können auch als Kriterium in Reach-Kampagnen verwendet werden. Letzte bekannte Bereich ein gemeldet kann dank der [Device-API]abgerufen werden.

Um träge Bereich Position Berichte aktivieren, fügen Sie die folgende Zeile nach der Initialisierung des Engagement-Agent:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Echtzeit-reporting Speicherort

Echtzeit Position reporting ermöglicht den Breiten- und Längengrad zugeordnete Geräte melden. Standardmäßig Standortangabe nur verwendet die Netzwerkadressen (basierend auf Zelle ID oder WIFI) und die Berichterstattung ist nur aktiv, wenn die Anwendung (z. B. während einer Sitzung) im Vordergrund ausgeführt wird.

Echtzeit sind *keine* Statistik berechnet. Ihr einziger Zweck ist die Verwendung von Echtzeit-geofencing \<erreichen Publikum Geofencing\> Kriterium in Reichweite Kampagnen.

Um Echtzeit Standortangabe zu aktivieren, fügen Sie die folgende Zeile nach der Initialisierung des Engagement-Agent:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS-basierte Berichte

Standardmäßig verwendet das Echtzeit Standortangabe nur Netzwerk Speicherorte. Aktivieren von GPS-basierten Standorten (die wesentlich genauere), fügen Sie Folgendes hinzu:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Hintergrund reporting

Standardmäßig ist Echtzeit Standortangabe nur aktiv beim Ausführen der Anwendung im Vordergrund (z. B. während einer Sitzung). Um die Berichte auch im Hintergrund aktivieren, fügen Sie Folgendes hinzu:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Wenn die Anwendung im Hintergrund ausgeführt wird nur Network Orten werden gemeldet, auch wenn das GPS aktiviert.

Implementierung dieser Funktion rufen [StartMonitoringSignificantLocationChanges] beim Wechsel der Anwendung in den Hintergrund. Beachten Sie, dass die Anwendung in den Hintergrund automatisch starten wird, kommt ein neues Standort-Ereignis.

##<a name="advanced-reporting"></a>Erweiterte Berichte

Optional, wenn Sie anwendungsspezifische Ereignisse, Fehler und Aufträge melden möchten, müssen Sie über die Methoden der Engagement-API verwenden die `EngagementAgent` Klasse. Ein Objekt dieser Klasse kann abgerufen werden durch Aufrufen der `[EngagementAgent shared]` statische Methode.

Engagement-API können alle Projekts erweiterten Funktionen und in der Engagement-API auf iOS verwenden (sowie in der technischen Dokumentation der `EngagementAgent` Klasse).

##<a name="disable-idfa-collection"></a>Deaktivieren der IDFA-Auflistung

Standardmäßig verwendet Engagement [IDFA] einen Benutzer eindeutig identifiziert. Jedoch wenn Sie Werbung an anderer Stelle in der Anwendung nicht verwenden, können Sie vom App Store-Überprüfung abgelehnt. IDFA Auflistung kann durch Hinzufügen des Präprozessor-Makros deaktiviert `ENGAGEMENT_DISABLE_IDFA` in der Pch-Datei (oder der `Build Settings` der Anwendung). Dadurch wird sichergestellt, dass gibt es keine Verweise auf `ASIdentifierManager`, `advertisingIdentifier` oder `isAdvertisingTrackingEnabled` in der Anwendung erstellen.

Integration in die Datei **prefix.pch** :

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Sie können überprüfen, ob IDFA Auflistung korrekt in der Anwendung deaktiviert ist Testprotokolle Engagement überprüfen. Siehe die Integrationstests\<Ios-Sdk-Engagement-Test-Idfa\> Netzwerkdokumentation für weitere Information.

##<a name="disable-log-reporting"></a>Protokoll melden deaktivieren

### <a name="using-a-method-call"></a>Ein Methodenaufruf

Engagement zu Protokolle senden möchten, können Sie aufrufen:

    [[EngagementAgent shared] setEnabled:NO];

Dieser Aufruf ist persistent: verwendet `NSUserDefaults` zum Speichern der Informationen.

Können Protokolldateien wieder dieselbe Funktion mit reporting `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integration in der Settings-bundle

Anstatt diese Funktion können Sie auch diese Einstellung integrieren, direkt in die vorhandene `Settings.bundle` Datei. Die Zeichenfolge `engagement_agent_enabled` verwendet werden eine Einstellung Bezeichner und müssen einen Umschalter zugeordnet werden (`PSToggleSwitchSpecifier`).

Im folgenden Beispiel `Settings.bundle` veranschaulicht die Implementierung:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Gerät API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
