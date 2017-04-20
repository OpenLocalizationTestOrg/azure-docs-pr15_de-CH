<properties 
    pageTitle="Universelle Windows-Apps SDK Upgrade-Verfahren" 
    description="Universelle Windows-Apps SDK Upgrade-Verfahren für Azure Mobile Engagement"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Universelle Windows-Apps SDK Upgrade-Verfahren

Wenn bereits eine ältere Version des Projekts in die Anwendung integriert haben, müssen Sie Punkte beim Aktualisieren des SDK.

Möglicherweise müssen mehrere Verfahren, wenn Sie mehrere Versionen des SDK verpasst. Zum Beispiel wenn Sie migrieren von 0.10.1, Sie zuerst die Prozedur "von 0.9.0 zu 0.10.1 müssen" 0.11.0 "von 0.10.1 zu 0.11.0" Verfahren.

##<a name="from-330-to-340"></a>Von 3.3.0 auf 3.4.0

### <a name="test-logs"></a>Testprotokolle

Konsolenprotokolle erzeugt vom SDK können jetzt aktiviert/deaktiviert/gefiltert werden. Um dies anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen Wert von der `EngagementTestLogLevel` -Enumeration, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Ressourcen

Die Reichweite Überlagerung verbessert. Es ist Teil des SDK NuGet-Paket.

Aktualisieren auf die neue Version des SDK kann wählen, ob vorhandenen Dateien aus dem Ordner Overlay Ressourcen oder nicht beibehalten werden sollen:

* Die vorherige Überlagerung für Sie arbeiten oder integrieren Sie die `WebView` Elemente manuell dann entscheiden, dass Sie Ihre bestehende Dateien weiterhin funktioniert. 
* Möchten Sie neue Overlay aktualisieren, ersetzen die gesamte `overlay` Ordner Ihrer Ressourcen durch das neue SDK-Paket (UWP-apps: nach der Aktualisierung erhalten Sie Overlay-Ordner von % USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Verwenden neue überschreibt Anpassungen in der vorherigen Version erstellt.

##<a name="from-320-to-330"></a>Von 3.2.0, 3.3.0

### <a name="resources"></a>Ressourcen
Dieser Schritt betrifft nur benutzerdefinierte Ressourcen. Wenn Sie die Ressourcen vom SDK (html, Bilder, Overlay) angepasst haben Sie vor der Aktualisierung sichern und Anwenden der Anpassung auf aktualisierte Ressourcen.

##<a name="from-310-to-320"></a>Von 3.1.0, 3.2.0

### <a name="resources"></a>Ressourcen
Dieser Schritt betrifft nur benutzerdefinierte Ressourcen. Wenn Sie die Ressourcen vom SDK (html, Bilder, Overlay) angepasst haben Sie vor der Aktualisierung sichern und Anwenden der Anpassung auf aktualisierte Ressourcen.

### <a name="webview-integration"></a>WebView-integration
Einige Verbesserungen mit verschiedenen Formfaktoren wurden in dieser Version. Stellen Sie sicher, dass die Integration der Webview folgt:

In der XAML-Seite:

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

Und die zugehörigen cs-Datei:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Von 2.0.0, 3.0.0

### <a name="resources"></a>Ressourcen
Dieser Schritt betrifft nur benutzerdefinierte Ressourcen. Wenn Sie die Ressourcen vom SDK (html, Bilder, Overlay) angepasst haben Sie vor der Aktualisierung sichern und Anwenden der Anpassung auf aktualisierte Ressourcen.

##<a name="from-111-to-200"></a>Von 1.1.1 zu 2.0.0

Die folgenden beschreibt der Capptain Service Capptain SAS in einer Anwendung bereitgestellt von Azure Mobile Engagement SDK Integration migrieren. 

> [Azure.IMPORTANT] Capptain und Mobile Engagement nicht dieselben Dienste und der unten beschriebenen Vorgehensweise zeigt nur die Clientanwendung zu migrieren. Migrieren das SDK in die Anwendung migrieren nicht Daten von Servern Capptain Mobile Engagement Server zu

Wenn Sie von einer früheren Version migrieren, finden Sie in der Capptain-Website migrieren, 1.1.1 zuerst dann folgenden Verfahren

### <a name="nuget-package"></a>NuGet-Paket

Ersetzen Sie **Capptain.WindowsPhone** durch **MicrosoftAzure.MobileEngagement** Nuget-Paket.

### <a name="applying-mobile-engagement"></a>Mobile Engagement anwenden

Das SDK verwendet den Begriff `Engagement`. Sie müssen das Projekt entsprechend dieser Änderung aktualisieren.

Sie müssen das aktuelle Capptain Nuget-Paket deinstallieren. Beachten Sie, dass alle Änderungen im Ordner Capptain Ressourcen entfernt werden. Möchten Sie diese Dateien dann eine Kopie davon.

Danach installieren Sie neue Microsoft Azure Engagement Nuget-Paket im Projekt. Sie finden direkt auf [Nuget-Website]. oder hier Index. Diese Aktion ersetzt alle Ressourcendateien Engagement und Ihre Projektverweise neue Engagement DLL hinzugefügt.

Sie müssen die Projektverweisen Capptain DLL-Verweise löschen säubern. Wenn Sie dies nicht vornehmen, Version Capptain Konflikte und Fehler auftreten.

Wenn Sie Capptain Ressourcen angepasst haben, kopieren Sie die alten Dateien Inhalte, und fügen sie im neuen Projekt Dateien. Beachten Sie, dass Xaml und Cs-Dateien aktualisiert werden.

Wenn diese Schritte müssen Sie nur alte Capptain Referenzen durch neuen Engagement Referenzen ersetzen.

1. Alle Capptain Namespaces müssen aktualisiert werden.

    Vor der Migration:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Nach der Migration:
    
        using Microsoft.Azure.Engagement;

2. Alle Capptain Klassen mit "Capptain" sollte "Projekt".

    Vor der Migration:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Nach der Migration:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Für XAML-Dateien ändern sich auch Capptain Namespace und Attribute.

    Vor der Migration:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Nach der Migration:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Seitenwechsel überlagern
    > [AZURE.IMPORTANT] Overlay wird auch geändert. Der neue Namespace ist `Microsoft.Azure.Engagement.Overlay`. In Xaml und Cs-Dateien verwendet werden muss. Darüber hinaus `CapptainGrid` genannt werden `EngagementGrid`, `capptain_notification_content` und `capptain_announcement_content` heißen `engagement_notification_content` und `engagement_announcement_content`.
    
    Überlagerung:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    Wird:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Andere Ressourcen wie Capptain Bilder und HTML-Dateien Beachten Sie bitte, dass sie auch umbenannt wurden, verwenden Sie "Projekt".

### <a name="project-declaration"></a>Projekt-Deklaration

Auf Package.appxmanifest `File Type Associations` wurden:

 -   Capptain\_erreicht\_Inhalt zu\_erreicht\_Inhalt
 -   Capptain\_Protokoll\_Datei zu\_Protokoll\_Datei

### <a name="application-id--sdk-key"></a>ID der Anwendung / SDK-Schlüssel

Engagement verwendet eine Verbindungszeichenfolge. Sie haben eine ID und einen Schlüssel SDK mit Mobile angeben, müssen Sie nur eine Verbindungszeichenfolge angeben. Sie können sie auf die Datei EngagementConfiguration einrichten.

Engagement Konfiguration legen Sie der `Resources\EngagementConfiguration.xml` Datei des Projekts.

Bearbeiten Sie diese Datei an:

-   Die Verbindungszeichenfolge der Anwendung zwischen Tags `<connectionString>` und `<\connectionString>`.

Wenn Sie stattdessen zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung Agent Engagement aufrufen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

Die Verbindungszeichenfolge für die Anwendung wird auf der Azure-Verwaltungsportal angezeigt.

### <a name="items-name-change"></a>Namen ändern

Alle Elemente mit dem Namen *Capptain* wurden *Engagement*benannt. Auch für *Capptain* *zu*.

Beispiele für häufig verwendete Capptain Artikel:

-   Nun den Namen EngagementConfiguration CapptainConfiguration
-   Nun den Namen EngagementAgent CapptainAgent
-   Nun den Namen EngagementReach CapptainReach
-   Nun den Namen EngagementHttpConfig CapptainHttpConfig
-   Nun den Namen GetEngagementPageName GetCapptainPageName

Beachten Sie diese umbenennen überschriebene Methoden beeinflusst.

 
