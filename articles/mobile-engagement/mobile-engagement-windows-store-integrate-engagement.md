<properties 
    pageTitle="Windows Universal Apps Engagement SDK-Integration" 
    description="Integration von Azure Mobile Engagement und universelle Windows-Apps"                  
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

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Windows Universal Apps Engagement SDK-Integration

> [AZURE.SELECTOR] 
- [Universelle Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Dieses Verfahren beschreibt die einfachste Möglichkeit, Analyse und Überwachung der Universal Windows Anwendung Projekts aktivieren.

Die folgenden Schritte sind ausreichend Bericht alle Statistiken über Benutzer, Sitzung, Aktivitäten, Abstürze und technische berechnet erforderlichen Protokolle aktivieren. Bericht über Protokolle benötigt weitere Statistiken wie Ereignisse, Fehler und Aufträge berechnen erfolgt manuell mit Engagement-API (siehe [wie der erweiterte Mobile Engagement tagging-API in Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md) seit Anwendung abhängig sind.

## <a name="supported-versions"></a>Unterstützte Versionen

Mobile Engagement SDK für Windows Universal Apps kann nur in Windows-Runtime und für universelle Windows-Plattform-Anwendung integriert werden:

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1
-   Windows 10 (Desktop- und Familien)

> [AZURE.NOTE] Wenn Sie Windows Phone Silverlight Zielen finden Sie im [Windows Phone Silverlight Integration Verfahren](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>Mobile Engagement Universal Apps SDK installieren

### <a name="all-platforms"></a>Alle Plattformen

Die Mobile Engagement SDK für Windows Universal App ist als ein Nuget-Paket *MicrosoftAzure.MobileEngagement*aufgerufen. Sie können es von Visual Studio Nuget Paket-Manager.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x und Windows Phone 8.1

NuGet setzt automatisch die SDK-Ressourcen in die `Resources` Ordner im Stammverzeichnis hinzu.

### <a name="windows-10-universal-windows-platform-applications"></a>Windows 10 universelle Windows Plattform

NuGet wird die SDK-Ressourcen in der UWP-Anwendung noch nicht automatisch bereitgestellt. Sie müssen es manuell bis Ressourcen Bereitstellung in NuGet erneut:

1.  Öffnen Sie den Dateiexplorer.
2.  Navigieren Sie zum folgenden Speicherort (**x.x.x** ist die Version des Projekts installiert): *% USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Drag & drop **Ressourcenordner** im Datei-Explorer zum Stammverzeichnis des Projekts in Visual Studio.
4.  Wählen Sie in Visual Studio das Projekt, und aktivieren Sie das Symbol **Alle Dateien anzeigen** oben im **Projektmappen-Explorer**.
5.  Einige Dateien sind nicht im Projekt enthalten. Zum Importieren klicken sie gleichzeitig auf **Ressourcenordner **aus Projekt ausschließen** ** und anderen klicken Sie auf **den Ressourcenordner **Projekt** den gesamten Ordner erneut hinzufügen** . Alle Dateien aus dem Ordner **Ressourcen** sind jetzt im Projekt enthalten.

## <a name="add-the-capabilities"></a>Hinzufügen von Funktionen

Engagement-SDK benötigt einige Funktionen des Windows SDK einwandfrei funktioniert.

Öffnen der `Package.appxmanifest` Datei und stellen Sie sicher, dass folgenden Funktionen deklariert werden:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Initialisieren des Projekts SDK

### <a name="engagement-configuration"></a>Engagement-Konfiguration

Engagement-Konfiguration zentral von der `Resources\EngagementConfiguration.xml` Datei des Projekts.

Bearbeiten Sie diese Datei an:

-   Die Verbindungszeichenfolge der Anwendung zwischen Tags `<connectionString>` und `<\connectionString>`.

Wenn Sie stattdessen zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung Agent Engagement aufrufen:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Die Verbindungszeichenfolge für die Anwendung wird auf der Azure-Verwaltungsportal angezeigt.

### <a name="engagement-initialization"></a>Engagement-Initialisierung

Wenn Sie ein neues Projekt erstellen eine `App.xaml.cs` Datei wird generiert. Diese Klasse erbt von `Application` und enthält viele wichtige Methoden. Es wird auch verwendet, das Engagement SDK initialisieren.

Ändern der `App.xaml.cs`:

-   Hinzufügen der `using` Aussagen:

        using Microsoft.Azure.Engagement;

-   Definieren Sie eine Methode die Initialisierung Engagement für alle gemeinsam:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Rufen Sie `InitEngagement` in der `OnLaunched` Methode:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Beim Starten der Anwendung ein benutzerdefiniertes Schema, eine andere Anwendung oder in der Befehlszeile die `OnActivated` wird aufgerufen. Sie müssen das Engagement SDK initiieren, wenn Ihre app aktiviert ist. Dazu überschreiben `OnActivated` Methode:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] Wir raten Sie Initialisierung Engagement an einer anderen Stelle der Anwendung hinzufügen.

## <a name="basic-reporting"></a>Grundlegende Berichterstattung

### <a name="recommended-method-overload-your-page-classes"></a>Empfohlen: Überladen der `Page` Klassen

Um Bericht über alle erforderlichen Engagement Benutzer Sessions, Aktivitäten, Abstürze und technische Statistiken berechnet Protokolle aktivieren einfach können Sie alle Ihre `Page` untergeordnete Klassen erben von der `EngagementPage` Klassen.

Hier ist ein Beispiel für eine Seite der Anwendung. Das gleiche gilt für alle Seiten der Anwendung möglich.

#### <a name="c-source-file"></a>C#-Quelldatei

Ändern die Seite `.xaml.cs` Datei:

-   Hinzufügen der `using` Aussagen:

        using Microsoft.Azure.Engagement;

-   Ersetzen Sie `Page` mit `EngagementPage`:

**Ohne:**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Mit:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Wenn Ihre Seite überschreibt die `OnNavigatedTo` -Methode rufen `base.OnNavigatedTo(e)`. Andernfalls die Aktivität gemeldet werden (die `EngagementPage` Aufrufe `StartActivity` innerhalb der `OnNavigatedTo` Methode).

#### <a name="xaml-file"></a>XAML-Datei

Ändern die Seite `.xaml` Datei:

-   Fügen Sie Ihren Namespace-Deklarationen:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Ersetzen Sie `Page` mit `engagement:EngagementPage`:

**Ohne:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Mit:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Das Standardverhalten überschreiben

Standardmäßig ist der Aktivitätsname ohne zusätzliche Klassennamen der Seite gemeldet. Wenn die Klasse "Page" Suffix verwendet, wird Sie Engagement auch entfernen.

Wenn Sie standardmäßig den Namen überschreiben möchten, fügen Sie diese einfach Code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Wenn Sie zusätzlichen Informationen mit Ihren Aktivitäten melden möchten, können Sie diese Code hinzufügen:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Diese Methoden werden aufgerufen, aus der `OnNavigatedTo` Methode der Seite.

### <a name="alternate-method-call-startactivity-manually"></a>Alternative Methode: Rufen Sie `StartActivity()` manuell

Wenn Sie nicht oder nicht überladen möchten Ihre `Page` Klassen können stattdessen starten Ihre Aktivitäten Aufrufen `EngagementAgent` Methoden direkt.

Wir empfehlen Aufrufen `StartActivity` innerhalb der `OnNavigatedTo` Methode der Seite.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Sicherzustellen Sie, dass Sie Ihre Sitzung korrekt.
> 
> Universelle Windows SDK ruft automatisch die `EndActivity` -Methode auf, wenn die Anwendung geschlossen wird. Daher ist es **sehr** empfehlenswert, rufen Sie die `StartActivity` Methode, wenn die Aktivität des Benutzers ändern und **nie** Aufruf der `EndActivity` -Methode diese Methode schickt Engagement Server Aktueller Benutzer hat die Anwendung verlassen, dies wirkt sich auf alle Anwendungsprotokolle.

## <a name="advanced-reporting"></a>Erweiterte Berichte

Optional Bericht anwendungsspezifische Ereignisse, Fehler und Aufträge empfiehlt dazu verwenden, andere Methoden in gefunden der `EngagementAgent` Klasse. Engagement-API können alle erweiterten Funktionen des Projekts verwenden.

Weitere Informationen finden Sie unter [Erweiterte tagging-API in Ihrer app Universal Windows Mobile Projekts verwenden](mobile-engagement-windows-store-use-engagement-api.md).

##<a name="advanced-configuration"></a>Erweiterte Konfiguration

### <a name="disable-automatic-crash-reporting"></a>Deaktivieren der automatischen Absturzberichte

Sie können den Absturz Berichterstellungsfunktion Engagement deaktivieren. Dann tritt ein Ausnahmefehler wird Engagement etwas nicht.

> [AZURE.WARNING] Wenn Sie dieses Feature deaktivieren möchten, beachten Sie, dass bei nicht behandelten in Ihrer app Absturz wird, Engagement nicht senden Absturz **und** Sitzung und Aufträge nicht schließen.

Deaktivieren der automatischen Absturzberichte nur passen Sie Ihrer Konfiguration je nach es deklariert an:

#### <a name="from-engagementconfigurationxml-file"></a>Von `EngagementConfiguration.xml` Datei

Legen Sie Bericht Absturz auf `false` zwischen `<reportCrash>` und `</reportCrash>` Tags.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Von `EngagementConfiguration` zur Laufzeit

Setzen Sie Bericht Absturz auf False das EngagementConfiguration-Objekt verwenden.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst-Modus

Standardmäßig protokolliert der Engagement Berichte in Echtzeit. Meldet die Anwendung Protokolle häufig, empfiehlt sich die Protokolle Puffer und sie gleichzeitig auf einem regulären Zeitbasis (Dies wird "Burstmodus" bezeichnet).

Rufen Sie dazu die Methode:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Das Argument ist ein Wert in **Millisekunden**. Jederzeit Wenn Sie Echtzeit-Protokollierung aktivieren möchten, rufen Sie die Methode ohne Parameter oder mit dem Wert 0.

Der Burstmodus leicht die Akkulaufzeit erhöhen jedoch wirkt sich auf das Engagement: alle Sitzungen und Aufträge Dauer Burst-Schwellenwert (also Sessions und kürzer als Burst-Schwellenwert werden Aufträge) gerundet. Es wird empfohlen, einen Burst-Schwellenwert nicht mehr als 30000 (30) verwenden. Sie müssen bewusst, dass gespeicherte Protokolle 300 beschränkt. Lang ist kann einige Protokolle verloren gehen.

> [AZURE.WARNING] Burst-Schwellenwert kann nicht auf weniger als 1 s konfiguriert. Wenn Sie dies versuchen, das SDK ein Trace mit dem Fehler anzeigen und automatisch auf den Standardwert, d. h. 0 zurückgesetzt. Dies löst das SDK Protokolle in Echtzeit melden.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
