<properties 
    pageTitle="Bridge iOS WebView mit systemeigenen Mobile Engagement iOS SDK" 
    description="Beschreibt, wie eine Brücke zwischen WebView mit Javascript und systemeigene Mobile Engagement iOS SDK"      
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
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Bridge iOS WebView mit systemeigenen Mobile Engagement iOS SDK

> [AZURE.SELECTOR]
- [Android-Brücke](mobile-engagement-bridge-webview-native-android.md)
- [iOS-Brücke](mobile-engagement-bridge-webview-native-ios.md)

Einige mobile apps dienen als hybride Anwendung, die Anwendung selbst systemeigene iOS Objective-C Entwicklung entwickelt, aber einige oder sogar alle Bildschirme in einem iOS WebView gerendert. Mobile Engagement iOS SDK diese Apps können weiterhin verwenden und dieses Lernprogramm beschreibt dabei gehen. 

Es gibt zwei Ansätze dazu jedoch nicht dokumentiert sind:

- Eine wird zuerst beschrieben auf diesem [Link](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) registrieren beinhaltet eine `UIWebViewDelegate` Ihr Web und Catch-und-sofort-Abbrechen einer Änderung in Javascript gemacht. 
- Zweitens eine basiert auf dieser [Sitzung WWDC 2013](https://developer.apple.com/videos/play/wwdc2013/615)Ansatz sauberer als das erste, das wir folgen dieses Handbuch. Beachten Sie, dass dieser Ansatz nur auf iOS7 oder höher. 

Gehen Sie für iOS-Beispiel Brücke:

1. Zunächst müssen Sie sicherstellen, dass Sie unsere [Erste Schritte-Lernprogramm](mobile-engagement-ios-get-started.md) Mobile Engagement iOS SDK in Hybrid app integrieren durchgegangen sind. Optional können Sie auch Protokollierung Test wie folgt, damit Sie sehen, wie wir aus der Webansicht auslösen SDK-Methoden. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Jetzt unbedingt Hybrid app einen Bildschirm mit einer Webansicht ist. Können sie die `Main.storyboard` der Anwendung. 

3. Der **ViewController** durch Klicken und Ziehen der Webansicht aus der Ansicht Controller auf diese Webview Zuordnen der `ViewController.h` bearbeiten, indem sie direkt unterhalb der `@interface` Linie. 

4. Wenn Sie dies tun, erscheint ein Dialogfeld fordert ein. Geben Sie den Namen als **Webansicht**. Die `ViewController.h` Datei sollte wie folgt aussehen:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. Wir aktualisieren die `ViewController.m` Datei später jedoch zunächst erstellen wir die Bridge-Datei erzeugt einen Wrapper über einige häufig verwendete Mobile Engagement iOS SDK-Methoden. Erstellen Sie eine neue Headerdatei **EngagementJsExports.h** verwendet die `JSExport` beschriebenen genannten [Sitzung](https://developer.apple.com/videos/play/wwdc2013/615) systemeigene iOS-Methoden verfügbar gemacht. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. Anschließend wird den zweiten Teil der Bridge-Datei erstellt. Erstellen Sie eine Datei namens **EngagementJsExports.m** erstellen die tatsächliche Wrapper die Mobile Engagement iOS SDK Methoden Implementierung enthalten. Beachten Sie auch, die wir analysieren die `extras` Webview JavaScript übergeben wird und dass in einem `NSMutableDictionary` Objekt mit Engagement SDK-Methode übergeben werden.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Jetzt wieder zum **ViewController.m** und durch den folgenden Code aktualisiert: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Beachten Sie folgende Punkte zur **ViewController.m** -Datei:

    - In die `loadWebView` -Methode laden wir eine lokale HTML-Datei namens **LocalPage.html** , deren Code weiter besprochen werden. 
    - In der `webViewDidFinishLoad` Methode, greifen wir die `JsContext` und unsere Wrapperklasse zuordnen. Dadurch können unsere Wrapper das Handle **EngagementJs** aus der Webansicht SDK-Methoden aufrufen. 

7. Erstellen Sie eine Datei namens **LocalPage.html** mit dem folgenden Code:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Beachten Sie folgende Punkte zur HTML-Datei:

    -   Es enthält eine Reihe von Eingabefeldern, in denen Sie Daten als Namen für Ihr Ereignis Auftrag Fehler, AppInfo bereitstellen können. Wenn Sie auf die Schaltfläche neben ist Javascript aufgerufen bezeichneten schließlich die Methoden aus der Bridge-Datei diese Mobile Engagement iOS SDK übergeben. 
    -   Wir werden Tags auf statische zusätzlichen Informationen an Ereignisse, Projekte und sogar Fehler zeigen, wie dies möglich ist. Diese zusätzlichen Informationen gesendet als eine JSON Zeichenfolge, die Sie suchen in der `EngagementJsExports.m` Datei analysiert und mit senden Ereignisse, Projekte, Fehler übergeben. 
    -   Eine Mobile Engagement Auftrag wird mit dem Namen 10 Sekunden und fahren Sie in das Eingabefeld geben begann. 
    -   Eine Mobile Engagement Appinfo oder Tag wird mit "Kundenname" als statische Schlüssel und die Eingabe als Wert für das Tag eingegebene Wert übergeben. 
 
9. Führen Sie die Anwendung, und Sie sehen die folgenden. Jetzt einige Namen für einen Testereignis wie folgt und daneben auf **Senden** . 

    ![][1]

10. Jetzt Wenn Sie auf der Registerkarte **Faxmonitor** Ihrer app und suchen unter **Ereignisse -> Details**wechseln, sehen Sie dieses Ereignis mit statischen app-Informationen anzeigen, die wir senden. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
