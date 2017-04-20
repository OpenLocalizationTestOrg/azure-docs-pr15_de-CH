<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Windows Phone Silverlight-apps"
    description="Informationen Sie zum Azure Mobile Engagement mit Analysen und Push Notifications für Windows Phone Silverlight-apps zu verwenden."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Erste Schritte mit Azure Mobile Engagement für Windows Phone Silverlight-apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird veranschaulicht, wie mit Azure Mobile Engagement kennen Ihre app-Nutzung und Pushbenachrichtigungen an segmentierte Windows Phone Silverlight-Anwendung.
Dieses Lernprogramm demonstriert das einfache Übertragung Szenario mit Mobile Engagement. Erstellen Sie eine leere Windows Phone Silverlight-app, die sammelt Daten und empfängt Pushbenachrichtigungen verwenden Microsoft Push Notification Service (MPNS).

> [AZURE.NOTE] Wenn Sie Windows Phone 8.1 (nicht Silverlight) abzielen, finden Sie das [universelle Windows-Lernprogramm](mobile-engagement-windows-store-dotnet-get-started.md).

In diesem Lernprogramm ist Folgendes erforderlich:

+ Visual Studio 2013
+ Nuget-Paket [MicrosoftAzure.MobileEngagement]

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>Einrichten mobiler Engagement für Ihre Windows Phone-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Mobile Engagement Backend Ihrer app herstellen

In diesem Lernprogramm wird eine "einfache Integration", ist minimal erforderlichen Daten sammeln und eine Push-Benachrichtigung zu senden. Die vollständige Integration-Dokumentation finden im [Mobile Engagement Windows Phone SDK-integration](mobile-engagement-windows-phone-sdk-overview.md)

Wir erstellen eine einfache Anwendung mit Visual Studio Integration zu veranschaulichen.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Erstellen Sie ein neues Windows Phone Silverlight-Projekt

Die folgenden Schritte gehen die Verwendung von Visual Studio 2015 Schritte in früheren Versionen von Visual Studio ähnlich sind. 

1. Starten Sie Visual Studio, und wählen Sie im **Startbildschirm,** **Neues Projekt**.

2. Wählen Sie im Popupfenster **Windows 8** -> **Windows Phone** -> **Leere App (Windows Phone Silverlight)**. App **Name** und **Projektmappenname**füllen Sie aus, und klicken Sie dann auf **OK**.

    ![][1]

3. Sie können **Windows Phone 8.0** oder **Windows Phone 8.1**Ziel.

Sie haben jetzt eine neue Windows Phone Silverlight-app erstellt in der wir Azure Mobile Engagement SDK integrieren.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Mobile Engagement Backend Ihrer app herstellen

1. Installieren Sie [MicrosoftAzure.MobileEngagement] Nuget-Paket im Projekt.

2. Open `WMAppManifest.xml` (unter dem Ordner Eigenschaften) und folgenden deklariert (addieren sind) in der `<Capabilities />` Tag:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Jetzt fügen Sie die Verbindungszeichenfolge, die Sie zuvor für Ihre Mobile Engagement kopiert und fügen Sie ihn in den `Resources\EngagementConfiguration.xml` Datei zwischen den `<connectionString>` und `</connectionString>` Tags:

    ![][3]

4. In der `App.xaml.cs` Datei:

    ein. Hinzufügen der `using` Anweisung:

            using Microsoft.Azure.Engagement;

    b. Initialisieren des SDK in die `Application_Launching` Methode:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. Fügen Sie in der `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Echtzeit-Überwachung aktivieren

Starten Sie Daten senden und sicherstellen, dass der Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm (Aktivität) Mobile Engagement Backend senden.

1. Die MainPage.xaml.cs Hinzufügen der `using` Anweisung:

        using Microsoft.Azure.Engagement;

2. Ersetzen Sie die Basisklasse der **MainPage**ist vor **PhoneApplicationPage**mit **EngagementPage**.

        class MainPage : EngagementPage 
    
3. In der `MainPage.xml` Datei:

    ein. Fügen Sie Ihren Namespace-Deklarationen:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Ersetzen Sie `phone:PhoneApplicationPage` in XML-Tagnamen mit `engagement:EngagementPage`.

##<a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-Benachrichtigung und messaging-Anwendung aktivieren

Mobile Engagement können Sie arbeiten und Ihre Benutzer Pushbenachrichtigungen mit Messaging-app im Rahmen von Kampagnen. Dieses Modul heißt REICHWEITE im Mobile Engagement-Portal.
In den folgenden Abschnitten richten Sie Ihre app erhalten.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Aktivieren Sie Ihre app MPNS Push Benachrichtigungen

Hinzufügen neuer Funktionen zu Ihrem `WMAppManifest.xml` Datei:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>Initialisieren des SDK REICHWEITE

1. In `App.xaml.cs`, rufen Sie `EngagementReach.Instance.Init();` in der Funktion **Application_Launching** direkt nach Agent-Initialisierung:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. In `App.xaml.cs`, rufen Sie `EngagementReach.Instance.OnActivated(e);` in der Funktion **Application_Activated** direkt nach Agent-Initialisierung:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Losgehen. Nun überprüfen wir, ob ordnungsgemäß dieses einfache Integration rief.

##<a id="send"></a>Sendet eine Benachrichtigung an Ihre Anwendung

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Sie sollten jetzt sehen eine Benachrichtigung auf Ihrem Gerät angezeigt wird als Benachrichtigung innerhalb der app ist die Anwendung sonst als Popupbenachrichtigung wie folgt: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
