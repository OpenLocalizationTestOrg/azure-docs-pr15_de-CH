<properties 
    pageTitle="Windows Phone Silverlight Reichweite SDK-Integration" 
    description="Wie Azure Mobile Engagement erreichen und Silverlight-Apps für Windows Phone integriert"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reichweite SDK-Integration

Sie müssen die Integration in [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) dieser Anleitung beschriebenen Verfahren.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Einbetten von Engagement erreichen SDK in Windows Phone Silverlight-Projekt

Sie haben keine Artikel hinzufügen. `EngagementReach`Verweise und Ressourcen sind bereits in Ihrem Projekt.

> [AZURE.TIP]  Gespeichert sind, können die `Resources` Projektordner insbesondere das Markensymbol (diese Standardeinstellung Engagement Symbol).

##<a name="add-the-capabilities"></a>Hinzufügen von Funktionen

Engagement erreichen SDK benötigt einige zusätzlichen Funktionen.

Öffnen der `WMAppManifest.xml` Datei und stellen Sie sicher, dass folgenden Funktionen deklariert werden:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

Zuerst wird vom MPNS-Dienst ermöglicht die Anzeige von Popupbenachrichtigung verwendet. Zweite wird verwendet, um das SDK eine Aufgabe Browser einzubetten.

Bearbeiten der `WMAppManifest.xml` und fügen in der `<Capabilities />` Tag:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>Microsoft Push Notification Service aktivieren

Um den **Microsoft Push Notification Service** (als MPNS bezeichnet) verwenden Ihre `WMAppManifest.xml` Datei muss ein `<App />` tag mit einer `Publisher` -Attribut auf den Namen des Projekts festgelegt.

##<a name="initialize-the-engagement-reach-sdk"></a>Initialisieren Sie die Reichweite Engagement SDK

### <a name="engagement-configuration"></a>Engagement-Konfiguration

Engagement-Konfiguration zentral von der `Resources\EngagementConfiguration.xml` Datei des Projekts.

Bearbeiten dieser Datei Reichweite Konfiguration angeben:

-   *Optional*angeben, ob der systemeigene Push (MPNS) aktiviert ist oder nicht zwischen `<enableNativePush>` und `</enableNativePush>` -Tags (`true` standardmäßig).
-   *Optional*den Namen des Push-Kanal zwischen `<channelName>` und `</channelName>` Tags geben dasselbe, Ihre Anwendung möglicherweise gerade oder leer lassen.

Wenn Sie stattdessen zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung Agent Engagement aufrufen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] Sie können den Namen des Kanals Push MPNS der Anwendung. Engagement erstellt standardmäßig ein AppId abhängig. Sie haben den Namen außer planen den Push-Channel außerhalb Engagement verwendet, nicht angegeben.

### <a name="engagement-initialization"></a>Engagement-Initialisierung

Ändern der `App.xaml.cs`:

-   Hinzufügen der `using` Aussagen:

        using Microsoft.Azure.Engagement;

-   Legen Sie `EngagementReach.Instance.Init` nach `EngagementAgent.Instance.Init` in `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Legen Sie `EngagementReach.Instance.OnActivated` in der `Application_Activated` Methode:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] Die `EngagementReach.Instance.Init` in einem dedizierten Thread ausgeführt wird. Sie müssen nicht selbst ausführen.

##<a name="app-store-submission-considerations"></a>App Store Vorlage Aspekte

Microsoft stellt einige Regeln, wenn Pushbenachrichtigungen verwenden:

Aus der Dokumentation Microsoft [Anwendungsrichtlinien] Abschnitt 2.9:

1) Fordern Sie den Benutzer zum Empfangen von Pushbenachrichtigungen akzeptieren. Fügen Sie dann in den Einstellungen der Push-Benachrichtigung deaktivieren.

Das EngagementReach-Objekt bietet zwei Methoden zum Verwalten der Opt-in/Opt-Out, `EnableNativePush()` und `DisableNativePush()`. Sie können z. B. eine Option erstellen, Weise mit einer Umschaltfläche MPNS aktivieren oder deaktivieren.

Sie können auch MPNS Engagement Konfiguration deaktivieren\<Windows Phone-Sdk-Reach-Konfiguration\>.

> 2.9.1) muss die Anwendung zunächst die Benachrichtigung bereitgestellt werden und **Erlaubnis des Benutzers express (Anmelden)**und **muss einen Mechanismus bereitstellen, über dem der Benutzer abbestellen kann Pushbenachrichtigungen,**beschreiben. Anträge mithilfe der Push-Benachrichtigungsdienst von Microsoft mit der Beschreibung für den Benutzer bereitgestellt werden und müssen alle [Anwendungsrichtlinien]  [ Content Policies] und [Zusätzliche Vorschriften für bestimmte Anwendungstypen].

2) Sie sollten nicht zu viele Pushbenachrichtigungen verwenden. Engagement wird zur Benachrichtigung behandelt.

> 2.9.2) die Anwendung und die Verwendung von Microsoft Push Notification Service müssen nicht übermäßig Netzwerkkapazität oder Bandbreite des Push-Benachrichtigungsdienst von Microsoft verwenden andernfalls übermäßig belastet Windows Phone oder anderen Microsoft-Gerät oder mit übermäßiger Pushbenachrichtigungen Microsoft seine Ermessen bestimmt und muss Schaden oder Microsoft-Netzwerke oder Server oder Servern Dritter oder verbundenen Microsoft Push Notification Service beeinträchtigen.

3) Nicht auf MPNS Partner Informationen senden. Diese Regel gilt auch für die Kampagnen innerhalb des Projekts Front-End-verwendet Projekts MPNS.

> 2.9.3) der Microsoft Push Notification Service möglicherweise nicht zum Senden der Benachrichtigung kritisch oder andernfalls sind könnte Fragen von Leben oder Tod, einschließlich ohne Einschränkung kritischen Benachrichtigungen im Zusammenhang mit ein Medizinprodukt oder Bedingung. MICROSOFT SCHLIESST ALLE GARANTIEN AUSDRÜCKLICH, DASS DIE VERWENDUNG VON MICROSOFT PUSH NOTIFICATION SERVICE ODER BEREITSTELLUNG DES MICROSOFT NOTIFICATION SERVICE PUSHBENACHRICHTIGUNGEN UNUNTERBROCHENEN FEHLERFREI ODER ANDERWEITIG GARANTIERT IN ECHTZEIT.

**Kann nicht garantiert werden, dass die Anwendung den Validierungsprozess übergeben werden, wenn Sie diese Empfehlung nicht berücksichtigt.**

##<a name="handle-data-push-optional"></a>Behandlung der Daten drücken (optional)

Soll die Anwendung erreichen Daten Push zu erhalten, müssen Sie zwei Ereignisse der EngagementReach-Klasse implementieren:

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

##<a name="customize-ui-optional"></a>Passen Sie an (optional)

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

Legen den Inhalt der `EngagementReach.Instance.Handler` mit Ihrem benutzerdefinierten Objekt in Ihre `App.xaml.cs` Klasse innerhalb der `Application_Launching` Methode.

**Beispielcode:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Engagement verwendet eine eigene Implementierung von `EngagementReachHandler`. Sie müssen Ihre eigenen erstellen, und wenn Sie dies tun, müssen Sie jede Methode überschreiben. Das Standardverhalten ist das Basisobjekt Engagement auswählen.

### <a name="layouts"></a>Layouts

Standardmäßig verwendet erreichen die eingebetteten Ressourcen der DLL die Benachrichtigung und Seiten angezeigt.

Allerdings können Sie Ihre eigenen Ressourcen entsprechend Ihrer Marke in diese Komponenten verwenden.

Überschreiben Sie `EngagementReachHandler` Methoden in der Unterklasse mitteilen zu Layouts verwenden:

**Beispielcode:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] Die `CreateNotification` -Methode kann null zurückgeben. Die Benachrichtigung wird nicht angezeigt, und Reichweite Kampagne gelöscht.

Zur Vereinfachung der Implementierung Layout bieten wir auch eigene Xaml dienen als Grundlage für Ihren Code kann. Sie befinden sich im Archiv Engagement SDK (/ Src/Reichweite).

> [AZURE.WARNING] Wir bieten Quellen sind genauen dieselben, die wir verwenden. So möchten Sie direkt ändern, vergessen Sie nicht, den Namespace und den Namen zu ändern.

### <a name="notification-position"></a>Benachrichtigungsposition

Standardmäßig wird eine Benachrichtigung in app unten links der Anwendung angezeigt. Sie können dieses Verhalten ändern, indem Sie überschreiben die `GetNotificationPosition` Methode der `EngagementReachHandler` Objekt.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Derzeit können Sie zwischen den `BOTTOM` (Standard) und `TOP` Positionen.

### <a name="launch-message"></a>Angezeigte Meldung

Klickt ein Benutzer auf eine Benachrichtigung des Systems (Toast), startet die Anwendung Engagement, den Inhalt der Nachrichten Push laden und Anzeigen der Seite für die entsprechende Kampagne.

Gibt es eine Verzögerung zwischen dem Start der Anwendung und der Anzeige der Seite (je nach Netzwerk).

Um dem Benutzer anzuzeigen, dass etwas geladen wird, sollten Sie visuelle Informationen, wie Sie eine Statusanzeige oder eine Statusanzeige bereitstellen. Engagement kann nicht behandeln, stellt aber einige Ereignishandler zur Verfügung.

Um den Rückruf zu implementieren, sind:

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

Festlegen des Rückrufs die `Application_Launching` Methode der `App.xaml.cs` Datei vorzugsweise vor der `EngagementReach.Instance.Init()` aufrufen.

> [AZURE.TIP] Jeder Handler wird vom UI-Thread aufgerufen. Sie müssen keinen MessageBox oder etwas Ignorierung mit sorgen.

[Anwendungsrichtlinien]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Zusätzliche Vorschriften für bestimmte Anwendungstypen]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
