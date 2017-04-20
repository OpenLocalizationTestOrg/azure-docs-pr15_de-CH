<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Xamarin.Android"
    description="Informationen Sie zum Azure Mobile Engagement für Xamarin.Android Apps mit Analysen und Pushbenachrichtigungen verwenden."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Erste Schritte mit Azure Mobile Engagement für Xamarin.Android Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird wie mit Azure Mobile Engagement die app Verwendung verstehen und Pushbenachrichtigungen segmentierten Benutzern Xamarin.Android-Anwendung zu senden.
Dieses Lernprogramm demonstriert das einfache Übertragung Szenario mit Mobile Engagement. Erstellen Sie eine leere Xamarin.Android app, die sammelt Daten und empfängt Pushbenachrichtigungen Google Cloud Messaging (GCM) verwenden.

In diesem Lernprogramm ist Folgendes erforderlich:

+ [Xamarin Studio](http://xamarin.com/studio). Sie können auch Visual Studio mit Xamarin, aber dieses Lernprogramms Xamarin Studio. Installationshinweise finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>Mobile Engagement für Ihre Android einrichten

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Mobile Engagement Backend Ihrer app herstellen

In diesem Lernprogramm wird eine "einfache Integration", ist minimal erforderlichen Daten sammeln und eine Push-Benachrichtigung zu senden. 

Wir erstellen eine einfache Anwendung mit Xamarin Studio Integration zu veranschaulichen.

###<a name="create-a-new-xamarinandroid-project"></a>Erstellen eines neuen Projekts Xamarin.Android

1. Einführung **Xamarin Studio** wechseln Sie zur **Datei** -> **neue** -> **Lösung** 

    ![][1]

2. Wählen Sie **Android aus** , stellen Sie sicher, dass die Sprache **C#** und klicken Sie auf **Weiter**.

    ![][2]

3. Der **Anwendungsname** und die **Organisations-ID**ausfüllen. Häkchen **Google Play Services** sicherstellen Sie und klicken Sie dann auf **Weiter**. 

    ![][3]
    
4. Aktualisieren Sie das **Projekt**, **Lösung** und **Speicherort** , falls erforderlich, und klicken Sie auf **Erstellen**.

    ![][4]
 
Xamarin Studio erstellt die Anwendung in der wir Mobile Engagement integrieren. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Mobile Engagement Backend Ihrer app herstellen

1. Rechts klicken Sie **Pakete** in die Lösung Windows und wählen **Pakete hinzufügen aus**

    ![][5]

2. **Microsoft Azure Mobile Engagement Xamarin SDK** gesucht und der Projektmappe hinzufügen.  

    ![][6]
   
3. **MainActivity.cs** öffnen, und fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. In der `OnCreate` Methode, fügen Sie Folgendes ein, um die Verbindung mit Mobile Engagement-Backend zu initialisieren. Stellen Sie Ihre **ConnectionString**hinzu. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Berechtigungen und Service-Deklaration hinzufügen

1. Öffnen Sie die Datei **Manifest.xml** unter dem Ordner Eigenschaften. Registerkarte Quelle, damit Sie die XML-Quelle direkt aktualisieren.
 
2. Des Projekts (die im Ordner **Eigenschaften** gefunden) Manifest.xml Berechtigungen hinzufügen, unmittelbar vor oder nach dem `<application>` Tag:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Fügen Sie Folgendes zwischen dem `<application>` und `</application>` Tags den Agent-Dienst zu deklarieren:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Ersetzen Sie im soeben eingefügten Code `"<Your application name>"` in der Bezeichnung. Dies erscheint im **Menü** Benutzer auf dem Gerät ausgeführte Dienste finden. Sie können das Wort "Service" in der Beschriftung hinzufügen.

###<a name="send-a-screen-to-mobile-engagement"></a>Senden Sie einen Bildschirm zu Mobile

Starten Sie Daten senden und sicherstellen, dass der Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm Mobile Engagement Backend senden. Dazu-sicherstellen, dass die `MainActivity` erbt von `EngagementActivity` anstelle von `Activity`.

    public class MainActivity : EngagementActivity
    
Auch beim Vererben können nicht `EngagementActivity` müssen Sie hinzufügen `.StartActivity` und `.EndActivity` Methoden `OnResume` und `OnPause` bzw..  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-Benachrichtigung und messaging-Anwendung aktivieren

Mobile Engagement können Sie arbeiten und Ihre Benutzer app im Rahmen von Kampagnen messaging mit Push-Benachrichtigung zu erreichen. Dieses Modul heißt REICHWEITE im Mobile Engagement-Portal.
In den folgenden Abschnitten wird die App erhalten.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
