<properties 
    pageTitle="Universelle Windows-Apps erreichen SDK-Integration" 
    description="Integration von Azure Mobile Engagement erreichen und universelle Windows-Apps"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-reach-sdk-integration"></a>Universelle Windows-Apps erreichen SDK-Integration

Sie müssen die Integration [Windows Universal Engagement SDK Integration](mobile-engagement-windows-store-integrate-engagement.md) dieser Anleitung beschriebenen Verfahren.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Engagement erreichen SDK in universelle Windows Projekt einbetten

Sie haben keine Artikel hinzufügen. `EngagementReach`Verweise und Ressourcen sind bereits in Ihrem Projekt.

> [AZURE.TIP] Gespeichert sind, können die `Resources` Projektordner insbesondere das Markensymbol (diese Standardeinstellung Engagement Symbol). Universelle Apps können Sie auch Verschieben der `Resources` auf das freigegebene Projekt, dessen Inhalte zwischen apps müssen zu der `Resources\EngagementConfiguration.xml` -Datei in ihrem Standardspeicherort ist plattformabhängig.

## <a name="enable-the-windows-notification-service"></a>Aktivieren Sie den Windows-Benachrichtigungsdienst

### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x und Windows Phone 8.1 nur

Nutzung der **Windows-Benachrichtigungsdienst** (bezeichnet als WNS) in der `Package.appxmanifest` auf dem `Application UI` klicken Sie auf `All Image Assets` im Feld links Bot. Rechts im Feld `Notifications`, ändern `toast capable` von `(not set)` , `(Yes)`.

### <a name="all-platforms"></a>Alle Plattformen

Sie müssen Ihre Anwendung auf Ihr Microsoft-Konto und Engagement-Plattform synchronisieren. Hierfür müssen Sie ein Konto erstellen bzw. [Windows-Entwicklungscenter](https://dev.windows.com). Danach erstellen Sie eine neue Anwendung und suchen Sie der SID und dem geheimen Schlüssel. Auf Frontend Engagement gehen auf Ihre app in `native push` , und fügen Sie Ihre Anmeldeinformationen. Danach klicken Sie auf das Projekt, wählen `store` und `Associate App with the Store...`. Sie müssen nur die Anwendung Sie erstellen vor der Synchronisierung auswählen.

## <a name="initialize-the-engagement-reach-sdk"></a>Initialisieren Sie die Reichweite Engagement SDK

Ändern der `App.xaml.cs`:

-   Legen Sie `EngagementReach.Instance.Init` nach `EngagementAgent.Instance.Init` in der `InitEngagement` Methode:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    Die `EngagementReach.Instance.Init` in einem dedizierten Thread ausgeführt wird. Sie müssen nicht selbst ausführen.

> [AZURE.NOTE] Verwenden Sie Push-Benachrichtigung an anderer Stelle in der Anwendung müssen Sie [den Push-Kanal gemeinsam](#push-channel-sharing) mit Engagement zu erreichen.

## <a name="integration"></a>Integration

Angebot umfasst zwei Arten Reach-app Banner und interstitielles Ansichten für Ankündigungen und Umfragen in Ihrer Anwendung hinzufügen: Overlay-Integration und manuelle Ansichten Webintegration. Sie sollten nicht beide Ansatz auf derselben Seite kombinieren.

Zwischen zwei Integration kann folgendermaßen zusammengefasst werden:

-   Overlay-Integration können Wenn Ihre Seiten bereits erbt vom Agent `EngagementPage`, ist nur eine Frage des Ersetzens `EngagementPage` von `EngagementPageOverlay` und `xmlns:engagement="using:Microsoft.Azure.Engagement"` von `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in Ihren Seiten.
-   Sie können die Webansicht manuelle Integration soll genau die Benutzeroberfläche erreichen Ihre Seiten oder anderen Vererbungsebene zu Ihren Seiten hinzufügen möchten. 

### <a name="overlay-integration"></a>Overlay-integration

Engagement Overlay fügt dynamisch Benutzeroberflächenelemente Reach-Kampagnen in der Seite angezeigt. Wenn die Überlagerung Layout entspricht empfiehlt der Webansicht manuelle Integration stattdessen.

In der XAML-Datei ändern `EngagementPage` Verweis auf`EngagementPageOverlay`

-   Fügen Sie Ihren Namespace-Deklarationen:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Ersetzen Sie `engagement:EngagementPage` mit `engagement:EngagementPageOverlay`:

**Mit EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**Mit EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Markieren Sie anschließend in die CS-Datei die Seite `EngagementPageOverlay` anstelle von `EngagementPage` und `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Ersetzen Sie `EngagementPage` mit `EngagementPageOverlay`:

**Mit EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Mit EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Engagement Overlay Fügt eine `Grid` Element auf der Seite besteht, das Layout und die `WebView` Elemente für Banner und die andere für interstitial anzeigen.

Sie können die Überlagerung Elemente direkt in der `EngagementPageOverlay.cs` Datei.

### <a name="web-views-manual-integration"></a>Manuelle Ansichten Webintegration

Reichweite suchen die Seiten für die beiden `WebView` Elemente im Banner und interstitielles Ansicht anzeigen. Sie müssen lediglich die beiden hinzufügen `WebView` Elemente irgendwo in Ihren Seiten hier ist ein Beispiel:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


In diesem Beispiel die `WebView` Elemente werden gestreckt Container die automatisch sie bei Änderung der Bildschirm drehen oder Fenster geändert Größe.

> [AZURE.WARNING] Es ist wichtig, die gleiche Benennung `engagement_notification_content` und `engagement_announcement_content` für die `WebView` Elemente. Reichweite ist sie durch ihren Namen identifiziert. 

## <a name="handle-datapush-optional"></a>Behandeln von Datapush (optional)

Soll die Anwendung erreichen Daten Push zu erhalten, müssen Sie zwei Ereignisse der EngagementReach-Klasse implementieren:

Fügen Sie in "App.Xaml.cs" in der App()-Konstruktor:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };
            
            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Sie können sehen, dass der Rückruf jeder Methode einen booleschen Wert zurückgibt. Engagement sendet Feedback an die Back-End nach datenpush verteilen. Wenn der Rückruf false gibt der `exit` Feedback senden werden. Andernfalls, `action`. Wenn kein Rückruf für die Ereignisse festgelegt ist die `drop` Feedback zu zurückgegeben.

> [AZURE.WARNING] Engagement ist nicht Vielfache Feedbacks für einen datenpush empfangen. Möchten Sie mehrere Handler für ein Ereignis festlegen, Bedenken Sie, dass das Feedback zur letzten entspricht erhalten. In diesem Fall empfehlen wir gibt immer den gleichen Wert zu vermeiden verwirrend Feedback im Front-End.

## <a name="customize-ui-optional"></a>Passen Sie an (optional)

### <a name="first-step"></a>Erster Schritt

Wir können Sie die Reichweite Benutzeroberfläche anpassen.

Hierzu müssen Sie eine Unterklasse erstellen, die `EngagementReachHandler` Klasse.

**Beispielcode:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Legen den Inhalt der `EngagementReach.Instance.Handler` mit Ihrem benutzerdefinierten Objekt in Ihre `App.xaml.cs` Klasse innerhalb der `App()` Methode.

**Beispielcode:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Engagement verwendet eine eigene Implementierung von `EngagementReachHandler`.
> Sie müssen Ihre eigenen erstellen, und wenn Sie dies tun, müssen Sie jede Methode überschreiben. Das Standardverhalten ist das Basisobjekt Engagement auswählen.

### <a name="web-view"></a>Webansicht

Standardmäßig verwendet erreichen die eingebetteten Ressourcen der DLL die Benachrichtigung und Seiten angezeigt.

Um eine vollständige Anpassung bieten wir nur Webansicht verwenden. Wenn Sie Layouts anpassen möchten, überschreiben die Ressourcendateien direkt `EngagementAnnouncement.html` und `EngagementNotification.html`. Engagement benötigt der gesamte Code in `<body></body>` ordnungsgemäß ausgeführt. Aber äußeren Tag hinzufügen `engagement_webview_area`.

Sie können jedoch Ihre eigenen Ressourcen.

Überschreiben Sie `EngagementReachHandler` Methoden in der Unterklasse anzuweisen, Engagement Layouts, aber darauf achten, dass eingebettete Mechanismus Engagement:

**Beispielcode:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


AnnouncementHTML ist `ms-appx-web:///Resources/EngagementAnnouncement.html`. Es stellt die HTML-Datei, die den Inhalt einer Push-Nachricht (Text Ankündigung und Web Anoucement Umfrage Ankündigung) entwerfen. AnnouncementName ist `engagement_announcement_content`. Es ist der Name des Entwurfs Webansicht in die XAML-Seite.

NotfificationHTML ist `ms-appx-web:///Resources/EngagementNotification.html`. Es stellt die HTML-Datei, die die Benachrichtigung über eine Nachricht zu entwerfen. NotfificationName ist `engagement_notification_content`. Es ist der Name des Entwurfs Webansicht in die XAML-Seite.

### <a name="customization"></a>Anpassung

Sie können Notification und Ankündigung hat Webansicht Sie Engagement-Objekt beibehalten möchten. Vorsicht, Webview-Objekt wird dreimal - erstmals in Xaml, erneut in die CS-Datei in "setwebview()"-Methode und dritten Mal in der HTML-Datei beschrieben.

-   In Xaml beschreiben Sie aktuelle grafisch Layout Webview-Komponente.
-   In die CS-Datei können Sie "setwebview()" in der Dimension zwei Webview (Benachrichtigung, Ankündigung) festgelegt. Es ist sehr effektiv, beim Ändern der Größe der Anwendungdes.
-   Engagement HTML-Datei beschreibt Webview Inhalt, Design und die Positionen der Elemente untereinander.

### <a name="launch-message"></a>Angezeigte Meldung

Klickt ein Benutzer auf eine Benachrichtigung des Systems (Toast), startet die Anwendung Engagement, laden Sie den Inhalt von Nachrichten drücken und zeigen Sie die Seite für die entsprechende Kampagne.

Gibt es eine Verzögerung zwischen dem Start der Anwendung und der Anzeige der Seite (je nach Netzwerk).

Um dem Benutzer anzuzeigen, dass etwas geladen wird, sollten Sie visuelle Informationen, wie Sie eine Statusanzeige oder eine Statusanzeige bereitstellen. Engagement kann nicht behandeln, stellt aber einige Ereignishandler zur Verfügung.

Fügen Sie in "App.Xaml.cs" in "Public App() {}" um den Rückruf zu implementieren:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
            
            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
            
            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Festlegen des Rückrufs in der "Öffentlichen App() {}"-Methode von der `App.xaml.cs` Datei, vorzugsweise vor der `EngagementReach.Instance.Init()` aufrufen.

> [AZURE.TIP] Jeder Handler wird vom UI-Thread aufgerufen. Sie müssen keinen MessageBox oder etwas Ignorierung mit sorgen.

##<a id="push-channel-sharing"></a>Push Channel freigeben

Wenn Sie Push-Benachrichtigung für einen anderen Zweck in Ihrer Anwendung verwenden, müssen Sie Push Channel Freigabefunktion Engagement SDK verwenden. Dies ist zu fehlenden Push.

- Sie können eigene Push Channel Initialisierung Engagement zu erreichen. Das SDK wird statt einer neuen Anforderung verwendet.

Aktualisieren die Initialisierung Engagement erreichen mit der Push-Channel in der `InitEngagement` aus der `App.xaml.cs` Datei:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Möchten Sie nur Push-Channel belegen nach Initialisierung erreichen Sie einen Rückruf Engagement Push-Channel einmal zu erreichen festlegen können wird sie auch vom SDK erstellt.

Richten Sie dem Rückruf am Ort **nach** Initialisierung Reichweite

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Benutzerdefiniertes Schema Tipp

Wir bieten benutzerdefiniertes Schema verwenden. Sie können verschiedene URI aus Engagement Frontend Anwendungscode Engagement verwendet werden senden. Standardschema wie `http, ftp, ...` sind Verwalten von Windows-Fenster bleibt Aufforderung sind kein Standardprogramm auf Gerät installiert. Sie können auch Ihr eigenes benutzerdefinierte Schema für Ihre Anwendung erstellen.

Einfach in der Anwendung ein benutzerdefiniertes Schema festgelegt ist auf der `Package.appxmanifest` im `Declarations` Bereich. Wählen Sie `Protocol` in den verfügbaren Deklarationen Bildlauffeld und hinzufügen. Bearbeiten der `Name` Feld mit der neuen gewünschten Namen.

Jetzt bearbeiten, um dieses Protokoll zu verwenden, die `App.xaml.cs` mit den `OnActivated` -Methode und Engagement hier auch initialisiert:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
