<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Android Einheit Bereitstellung"
    description="Informationen Sie zum Verwenden von Azure Mobile Engagement Pushbenachrichtigungen mit Analytics Einheit Apps iOS Geräte bereitstellen."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Erste Schritte mit Azure Mobile Engagement für Android Einheit Bereitstellung

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird veranschaulicht, wie mit Azure Mobile Engagement die app Verwendung verstehen und Pushbenachrichtigungen zu senden segmentiert Benutzern einer Anwendung Einheit, wenn Android-Gerät bereitstellen.
Dieses Lernprogramms klassische Einheit Zurücksetzen einer Kugel Lernprogramm als Ausgangspunkt. Sie sollten die in diesem [Lernprogramm](mobile-engagement-unity-roll-a-ball.md) fortfahren Integration Mobile Engagement Schritte, die wir in der Anleitung unten zeigen. 

In diesem Lernprogramm ist Folgendes erforderlich:

+ [Einheit-Editor](http://unity3d.com/get-unity)
+ [Mobile Engagement Einheit SDK](https://aka.ms/azmeunitysdk)
+ Google Android SDK

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>Mobile Engagement für Ihre Android einrichten

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Mobile Engagement Backend Ihrer app herstellen

###<a name="import-the-unity-package"></a>Einheit-Paket importieren

1. [Mobile Engagement Einheit Paket](https://aka.ms/azmeunitysdk) herunterladen und auf Ihrem Computer speichern. 

2. Zu **Ressourcen-Paket importieren > -> benutzerdefinierte Paket** und heruntergeladenen Pakets im oben genannten Schritt auswählen. 

    ![][70] 

3. Stellen Sie sicher alle Dateien ausgewählt sind, und klicken Sie auf " **Importieren** ". 

    ![][71] 

4. Nach Import erfolgreich ist, sehen Sie die importierten SDK-Dateien im Projekt.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>Aktualisieren der EngagementConfiguration

1. Öffnen Sie die Skriptdatei **EngagementConfiguration** von SDK-Ordner und Update der **ANDROID\_Verbindung\_Zeichenfolge** mit der Verbindungszeichenfolge früher von Azure-Portal erhalten.  

    ![][73]

2. Speichern Sie die Datei 

3. Führen Sie **File-Engagement -> Android Manifest generieren**. Dies ist das Plug-in von Mobile Engagement SDK hinzugefügt und anklicken aktualisiert automatisch Ihre Einstellungen. 

    ![][74]

> [AZURE.IMPORTANT] Stellen Sie sicher, das bei jeder Änderung die **EngagementConfiguration** Datei sonst nicht reflektiert die Änderungen in der app ausgeführt. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfigurieren der Anwendung für einfache Überwachung

1. **PlayerController** Skript angefügt, das Player-Objekt zur Bearbeitung geöffnet. 

2. Fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Engagement.Unity;

3. Fügen Sie den folgenden, die `Start()` -Methode
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Bereitstellen und Ausführen der Anwendung
Stellen Sie sicher, dass Android SDK auf Ihrem Computer installiert ist, bevor Sie versuchen, diese Einheit Anwendung auf dem Gerät bereitgestellt haben. 

1. Android-Gerät an den Computer anschließen 

2. Öffnen Sie **File-Buildeinstellungen** 

    ![][40]

3. Wählen Sie **Android** , und klicken Sie auf **Switch-Plattform**

    ![][51]

    ![][52]

4. Klicken Sie auf **Einstellungen** und bieten Sie einen gültigen Bundle-Bezeichner. 

    ![][53]

5. Klicken Sie auf **Erstellen und Ausführen**

    ![][54]

6. Möglicherweise müssen Sie einen Ordnernamen zum Speichern von Android Paket bereitstellen. 

7. Wenn alles gut geht, das Paket auf das angeschlossene Gerät bereitgestellt und das Spiel Einheit sollte auf Ihrem Telefon angezeigt! 

##<a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-Benachrichtigung und messaging-Anwendung aktivieren

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>Aktualisieren der EngagementConfiguration

1. Öffnen Sie die Skriptdatei **EngagementConfiguration** von SDK-Ordner und Update der **ANDROID\_GOOGLE\_Anzahl** mit der **Projektnummer Google** bereits die Google Cloud-Entwicklerportal abgerufen. Dies ist eine Zeichenfolge Wert daher unbedingt in Anführungszeichen setzen. 

    ![][75]

2. Speichern Sie die Datei. 

3. Führen Sie **File-Engagement -> Android Manifest generieren**. Dies ist das Plug-in von Mobile Engagement SDK hinzugefügt und anklicken aktualisiert automatisch Ihre Einstellungen. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>Konfigurieren Sie die Anwendung benachrichtigt

1. **PlayerController** Skript angefügt, das Player-Objekt zur Bearbeitung geöffnet. 

2. Fügen Sie den folgenden, die `Start()` -Methode

        EngagementReachAgent.Initialize();

3. Nun, die Anwendung aktualisiert bereitstellen Sie, und führen Sie die Anwendung auf einem Gerät pro Anweisungen unten. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
