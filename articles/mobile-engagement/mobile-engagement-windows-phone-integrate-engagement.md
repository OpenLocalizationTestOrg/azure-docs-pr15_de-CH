<properties 
    pageTitle="Windows Phone Silverlight Engagement SDK-Integration" 
    description="Integration von Azure Mobile Engagement und Silverlight-Apps für Windows Phone"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight Engagement SDK-Integration

> [AZURE.SELECTOR] 
- [Universal Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Die einfachste Möglichkeit zum Aktivieren von Azure Mobile Engagement Analyse und Überwachung in der Windows Phone Silverlight-Anwendung beschrieben.

Die folgenden Schritte sind ausreichend Bericht alle Statistiken über Benutzer, Sitzung, Aktivitäten, Abstürze und technische berechnet erforderlichen Protokolle aktivieren. Bericht über Protokolle benötigt weitere Statistiken wie Ereignisse, Fehler und Aufträge berechnen erfolgt manuell mit Engagement-API (siehe unten [wie der erweiterte Mobile Engagement tagging-API in Ihre Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md) ) da sind anwendungsspezifisch.

##<a name="supported-versions"></a>Unterstützte Versionen

Mobile Engagement SDK für Windows Silverlight kann nur in Clientanwendungen auf integriert werden:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Wenn Sie Windows Phone 8.1 (nicht Silverlight) abzielen finden Sie [universelle Windows Integrationsprozess](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>Mobile Engagement Silverlight SDK installieren

Mobile Engagement SDK für Windows Silverlight ist als ein Nuget-Paket *MicrosoftAzure.MobileEngagement*aufgerufen. Sie können es von Visual Studio Nuget Paket-Manager. 

##<a name="add-the-capabilities"></a>Hinzufügen von Funktionen

Engagement-SDK benötigt einige Funktionen von Windows Phone Silverlight SDK einwandfrei funktioniert.

Öffnen der `WMAppManifest.xml` Datei und sicherzustellen, dass die folgenden Funktionen deklariert werden die `Capabilities` Bereich:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>Initialisieren des Projekts SDK

### <a name="engagement-configuration"></a>Engagement-Konfiguration

Engagement-Konfiguration zentral von der `Resources\EngagementConfiguration.xml` Datei des Projekts.

Bearbeiten Sie diese Datei an:

-   Die Verbindungszeichenfolge der Anwendung zwischen Tags `<connectionString>` und `<\connectionString>`.

Wenn Sie stattdessen zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung Agent Engagement aufrufen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Die Verbindungszeichenfolge für die Anwendung wird auf der Azure-Verwaltungsportal angezeigt.

### <a name="engagement-initialization"></a>Engagement-Initialisierung

Wenn Sie ein neues Projekt erstellen eine `App.xaml.cs` Datei wird generiert. Diese Klasse erbt von `Application` und enthält viele wichtige Methoden. Es wird auch verwendet, das Engagement SDK initialisieren.

Ändern der `App.xaml.cs`:

-   Hinzufügen der `using` Aussagen:

        using Microsoft.Azure.Engagement;

-   Legen Sie `EngagementAgent.Instance.Init` in der `Application_Launching` Methode:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Legen Sie `EngagementAgent.Instance.OnActivated` in der `Application_Activated` Methode:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] Wir raten Sie Initialisierung Engagement an einer anderen Stelle der Anwendung hinzufügen. Bedenken Sie jedoch, die `EngagementAgent.Instance.Init` -Methode auf einem dedizierten Thread und nicht im UI-Thread ausgeführt.

##<a name="basic-reporting"></a>Grundlegende Berichterstattung

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Empfohlen: Überladen der `PhoneApplicationPage` Klassen

Um Bericht über alle erforderlichen Engagement Benutzer Sessions, Aktivitäten, Abstürze und technische Statistiken berechnet Protokolle aktivieren einfach können Sie alle Ihre `PhoneApplicationPage` untergeordnete Klassen erben von der `EngagementPage` Klassen.

Hier ist ein Beispiel für eine Seite der Anwendung. Das gleiche gilt für alle Seiten der Anwendung möglich.

#### <a name="c-source-file"></a>C#-Quelldatei

Ändern die Seite `.xaml.cs` Datei:

-   Hinzufügen der `using` Aussagen:

        using Microsoft.Azure.Engagement;

-   Ersetzen Sie `PhoneApplicationPage` mit `EngagementPage` :

**Ohne:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Mit:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Wenn Ihre Seite erbt die `OnNavigatedTo` -Methode achten die `base.OnNavigatedTo(e)` aufrufen. Andernfalls wird die Aktivität nicht gemeldet. Die `EngagementPage` ist der Aufruf von `StartActivity` in der `OnNavigatedTo` Methode.

#### <a name="xaml-file"></a>XAML-Datei

Ändern die Seite `.xaml` Datei:

-   Fügen Sie Ihren Namespace-Deklarationen:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Ersetzen Sie `phone:PhoneApplicationPage` mit `engagement:EngagementPage` :

**Ohne:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Mit:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Das Standardverhalten überschreiben

Standardmäßig ist der Aktivitätsname ohne zusätzliche Klassennamen der Seite gemeldet. Wenn die Klasse "Page" Suffix verwendet, wird Sie Engagement auch entfernen.

Wenn Sie das Standardverhalten für den Namen überschreiben möchten, fügen Sie diese einfach Code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Wenn Sie zusätzliche Informationen mit Ihren Aktivitäten melden möchten, können Sie diese Code hinzufügen:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Diese Methoden werden aufgerufen, aus der `OnNavigatedTo` Methode der Seite.

### <a name="alternate-method-call-startactivity-manually"></a>Alternative Methode: Rufen Sie `StartActivity()` manuell

Wenn Sie nicht oder nicht überladen möchten Ihre `PhoneApplicationPage` Klassen können stattdessen starten Ihre Aktivitäten Aufrufen `EngagementAgent` Methoden direkt.

Aufrufen sollten `StartActivity` innerhalb der `OnNavigatedTo` Methode der PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Sicherzustellen Sie, dass Sie Ihre Sitzung korrekt.
>
> Das SDK ruft automatisch die `EndActivity` -Methode auf, wenn die Anwendung geschlossen wird. Daher ist es **sehr** empfehlenswert, rufen Sie die `StartActivity` Methode, wenn die Aktivität des Benutzers ändern und **nie** rufen die `EndActivity` Methode. Diese Methode sendet eine Nachricht an den Server Engagement, der aktuelle Benutzer die Anwendung verlassen hat, dies wirkt sich auf alle Anwendungsprotokolle.

##<a name="advanced-reporting"></a>Erweiterte Berichte

Optional Bericht anwendungsspezifische Ereignisse, Fehler und Aufträge empfiehlt dazu verwenden, andere Methoden in gefunden der `EngagementAgent` Klasse. Engagement-API können alle erweiterten Funktionen des Projekts verwenden.

Weitere Informationen finden Sie unter [Erweiterte Mobile Projekts tagging-API in Ihre Windows Phone Silverlight-app verwenden](mobile-engagement-windows-phone-use-engagement-api.md).

##<a name="advanced-configuration"></a>Erweiterte Konfiguration

### <a name="disable-automatic-crash-reporting"></a>Deaktivieren der automatischen Absturzberichte

Sie können den Absturz Berichterstellungsfunktion Engagement deaktivieren. Dann tritt ein Ausnahmefehler wird Engagement etwas nicht.

> [AZURE.WARNING] Wenn Sie dieses Feature deaktivieren möchten, beachten Sie, dass nicht behandelte in Ihrer app Absturz wird, Engagement senden nicht wird den Absturz **und** Sitzung und Aufträge nicht geschlossen wird.

Deaktivieren der automatischen Absturzberichte nur passen Sie Ihrer Konfiguration je nach es deklariert an:

#### <a name="from-engagementconfigurationxml-file"></a>Von `EngagementConfiguration.xml` Datei

Legen Sie Bericht Absturz auf `false` zwischen `<reportCrash>` und `</reportCrash>` Tags.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Von `EngagementConfiguration` zur Laufzeit

Setzen Sie Bericht Absturz auf False das EngagementConfiguration-Objekt verwenden.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst-Modus

Standardmäßig protokolliert der Engagement Berichte in Echtzeit. Meldet die Anwendung Protokolle häufig, empfiehlt sich die Protokolle Puffer und sie gleichzeitig auf einem regulären Zeitbasis (Dies wird "Burstmodus" bezeichnet).

Rufen Sie dazu die Methode:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Das Argument ist ein Wert in **Millisekunden**. Jederzeit Wenn Sie Echtzeit-Protokollierung aktivieren möchten, rufen Sie die Methode ohne Parameter oder mit dem Wert 0.

Der Burstmodus leicht die Akkulaufzeit erhöhen jedoch wirkt sich auf das Engagement: alle Sitzungen und Aufträge Dauer Burst-Schwellenwert (also Sessions und kürzer als Burst-Schwellenwert werden Aufträge) gerundet. Es wird empfohlen, einen Burst-Schwellenwert nicht mehr als 30000 (30) verwenden. Sie müssen bewusst, dass gespeicherte Protokolle 300 beschränkt. Lang ist kann einige Protokolle verloren gehen.

> [AZURE.WARNING] Burst-Schwellenwert kann nicht konfiguriert werden, auf weniger als eine Sekunde. Wenn Sie versuchen, und das SDK die Spur mit dem Fehler wird automatisch auf den Standardwert zurückgesetzt zeigt 0 Sekunden. Dies löst das SDK Protokolle in Echtzeit melden.
 
