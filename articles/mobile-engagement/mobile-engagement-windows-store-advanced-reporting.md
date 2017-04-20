<properties
    pageTitle="Windows Universal Erweiterte Berichte mit MobileApps"
    description="Integration von Azure Mobile Engagement und universelle Windows-Apps"                  
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Erweiterte Berichte mit Windows Universal Apps Engagement SDK

> [AZURE.SELECTOR]
- [Universelle Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Weitere reporting-Szenarien in der Universal Windows Anwendung beschrieben. Diese Szenarien umfassen Optionen, die Sie im [Schnellstart](mobile-engagement-windows-store-dotnet-get-started.md) -Lernprogramm erstellte Anwendung anwenden können.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Bevor Sie dieses Lernprogramm starten, müssen Sie zunächst das [Erste Schritte](mobile-engagement-windows-store-dotnet-get-started.md) -Lernprogramm ausführen absichtlich direkt und einfach. Dieses Lernprogramm umfasst zusätzliche Optionen, denen Sie auswählen können.

## <a name="specifying-engagement-configuration-at-runtime"></a>Engagement-Konfiguration zur Laufzeit festlegen

Engagement-Konfiguration zentral von der `Resources\EngagementConfiguration.xml` Datei des Projekts, das sie im Thema [Einführung](mobile-engagement-windows-store-dotnet-get-started.md) festgelegt wurde.

Aber Sie können auch zur Laufzeit: Rufen Sie die folgende Methode vor der Initialisierung Agent Engagement:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Empfohlen: Überladen der `Page` Klassen

Aktivieren der Berichterstattung aller Protokolle zu Benutzern, Sitzung, Aktivitäten, Abstürze und technische Statistiken berechnen erforderlich machen die `Page` untergeordnete Klassen erben von der `EngagementPage` Klassen.

Hier ist ein Beispiel für eine Seite der Anwendung. Das gleiche gilt für alle Seiten der Anwendung möglich.

### <a name="c-source-file"></a>C#-Quelldatei

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

> [AZURE.IMPORTANT] Wenn Ihre Seite überschreibt die `OnNavigatedTo` -Methode rufen `base.OnNavigatedTo(e)`. Andernfalls die Aktivität wird nicht gemeldet (die `EngagementPage` Aufrufe `StartActivity` innerhalb der `OnNavigatedTo` Methode).

### <a name="xaml-file"></a>XAML-Datei

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

### <a name="override-the-default-behaviour"></a>Das Standardverhalten überschreiben

Standardmäßig ist der Aktivitätsname ohne zusätzliche Klassennamen der Seite gemeldet. Die Klasse "Page" Suffix verwendet, wird entfernt Engagement.

Fügen Sie diesen Code, um das Standardverhalten für den Namen überschreiben:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Zusätzliche Informationen mit Ihren Aktivitäten, fügen Sie diesen Code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Diese Methoden werden aufgerufen, aus der `OnNavigatedTo` Methode der Seite.

### <a name="alternate-method-call-startactivity-manually"></a>Alternative Methode: Rufen Sie `StartActivity()` manuell

Wenn Sie nicht oder nicht überladen möchten Ihre `Page` Klassen können stattdessen starten Ihre Aktivitäten Aufrufen `EngagementAgent` Methoden direkt.

Aufrufen sollten `StartActivity` innerhalb der `OnNavigatedTo` Methode der Seite.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Sicherzustellen Sie, dass Sie Ihre Sitzung korrekt.
>
> Universelle Windows SDK ruft automatisch die `EndActivity` -Methode auf, wenn die Anwendung geschlossen wird. Daher ist es **sehr** empfehlenswert, rufen Sie die `StartActivity` Methode, wenn die Aktivität des Benutzers ändern und **nie** rufen die `EndActivity` Methode. Diese Methode teilt dem Server Engagement, dass der aktuelle Benutzer die Anwendung verlassen hat die alle Anwendungsprotokolle auswirkt.

## <a name="advanced-reporting"></a>Erweiterte Berichte

Sie möchten gegebenenfalls Bericht anwendungsspezifische Ereignisse, Fehler und Aufträge dazu verwenden, andere Methoden in gefunden der `EngagementAgent` Klasse. Engagement-API ermöglicht die Verwendung erweiterter Funktionen alle Engagement.

Weitere Informationen finden Sie unter [Erweiterte tagging-API in Ihrer app Universal Windows Mobile Projekts verwenden](mobile-engagement-windows-store-use-engagement-api.md).
