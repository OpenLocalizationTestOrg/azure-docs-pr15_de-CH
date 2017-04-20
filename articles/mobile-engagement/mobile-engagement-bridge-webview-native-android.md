<properties 
    pageTitle="Bridge Android WebView mit systemeigenen Android Mobile Engagement-SDK" 
    description="Beschreibt, wie eine Brücke zwischen WebView mit Javascript und native Mobile Engagement Android SDK"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Bridge Android WebView mit systemeigenen Android Mobile Engagement-SDK

> [AZURE.SELECTOR]
- [Android-Brücke](mobile-engagement-bridge-webview-native-android.md)
- [iOS-Brücke](mobile-engagement-bridge-webview-native-ios.md)

Einige mobile apps dienen als hybride Anwendung, die Anwendung selbst mit systemeigenen Android Entwicklung entwickelt, aber einige oder sogar alle Bildschirme in Android WebView gerendert. Sie können solche Apps noch Mobile Engagement Android SDK verwenden und dieses Lernprogramm beschreibt, wie dies. Der Beispielcode basiert auf Android Dokumentation [hier](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Es beschreibt wie dokumentierte Vorgehensweise für Mobile Engagement Android SDK häufig verwendeten Methoden implementieren, Webview Hybrid-App auch Anfragen Ereignisse überwachen, Aufträge, Fehler app Info initiieren kann, während über unsere Android SDK Rohrleitungen verwendet werden kann. 

1. Zunächst müssen Sie sicherstellen, dass Sie unsere [Erste Schritte-Lernprogramm](mobile-engagement-android-get-started.md) Mobile Engagement Android SDK in Hybrid app integrieren durchgegangen sind. Danach, die `OnCreate` -Methode wird wie folgt aussehen.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Jetzt unbedingt Hybrid app einen Bildschirm mit einer Webansicht ist. Der Code werden ähnlich wie folgende, wir eine lokale HTML laden-Datei **Sample.html** in der Webansicht in die `onCreate` -Methode des Bildschirms. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Jetzt erstellen eine brückendatei **WebAppInterface** erzeugt einen Wrapper über einige häufig verwendete Mobile Engagement Android SDK mittels der `@JavascriptInterface` Ansatz in der [Android-Dokumentation](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript)beschrieben:

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Wenn wir über Bridge-Datei erstellt haben, müssen wir sicherstellen, dass unsere Webview zugeordnet ist. Dazu müssen bearbeiten Ihre `SetWebview` Methode, sodass wie folgt aussieht:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. In den oben stehenden Ausschnitt wir aufgerufen `addJavascriptInterface` unsere Webview Bridge Klasse zugeordnet und auch einen Handle aufgerufen **EngagementJs** rufen die Methoden aus der Bridge-Datei erstellt. 

6. Nun erstellen Sie die folgende Datei namens **Sample.html** in das Projekt in einem Ordner namens **Anlagen** in der Webansicht geladen und wo wir rufen die Methoden aus der Bridge-Datei.

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
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Beachten Sie folgende Punkte zur HTML-Datei:

    -   Es enthält eine Reihe von Eingabefeldern, in denen Sie Daten als Namen für Ihr Ereignis Auftrag Fehler, AppInfo bereitstellen können. Wenn Sie auf die Schaltfläche neben ist Javascript aufgerufen bezeichneten schließlich die Methoden aus der Bridge-Datei dieses Projekts Android Mobile SDK übergeben. 
    -   Wir werden Tags auf statische zusätzlichen Informationen an Ereignisse, Projekte und sogar Fehler zeigen, wie dies möglich ist. Diese zusätzlichen Informationen gesendet als eine JSON Zeichenfolge, die Sie suchen in der `WebAppInterface` Datei, ist analysiert und Android `Bundle` und mit Ereignisse, Projekte, Fehler senden. 
    -   Eine Mobile Engagement Auftrag wird mit dem Namen 10 Sekunden und fahren Sie in das Eingabefeld geben begann. 
    -   Eine Mobile Engagement Appinfo oder Tag wird mit "Kundenname" als statische Schlüssel und die Eingabe als Wert für das Tag eingegebene Wert übergeben. 
 
9. Führen Sie die Anwendung, und Sie sehen die folgenden. Jetzt einige Namen für einen Testereignis wie folgt und klicken Sie unten auf **Senden** . 

    ![][1]

10. Jetzt Wenn Sie auf der Registerkarte **Faxmonitor** Ihrer app und suchen unter **Ereignisse -> Details**wechseln, sehen Sie dieses Ereignis mit statischen app-Informationen anzeigen, die wir senden. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
