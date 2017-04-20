<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Einheit iOS-Bereitstellung"
    description="Informationen Sie zum Verwenden von Azure Mobile Engagement Pushbenachrichtigungen mit Analytics Einheit Apps iOS Geräte bereitstellen."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Erste Schritte mit Azure Mobile Engagement für Einheit iOS-Bereitstellung

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird veranschaulicht, wie mit Azure Mobile Engagement die app Verwendung verstehen und Pushbenachrichtigungen zu senden Benutzern einer Anwendung Einheit aufgeteilt, bei der Bereitstellung für ein Gerät iOS.
Dieses Lernprogramms klassische Einheit Zurücksetzen einer Kugel Lernprogramm als Ausgangspunkt. Sie sollten die in diesem [Lernprogramm](mobile-engagement-unity-roll-a-ball.md) fortfahren Integration Mobile Engagement Schritte, die wir in der Anleitung unten zeigen. 

In diesem Lernprogramm ist Folgendes erforderlich:

+ [Einheit-Editor](http://unity3d.com/get-unity)
+ [Mobile Engagement Einheit SDK](https://aka.ms/azmeunitysdk)
+ XCode-Editor

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>Mobile Engagement für iOS-app einrichten

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

1. Öffnen Sie die Skriptdatei **EngagementConfiguration** von SDK-Ordner und Update der **IOS\_Verbindung\_Zeichenfolge** mit der Verbindungszeichenfolge früher von Azure-Portal erhalten.  

    ![][73]

2. Speichern Sie die Datei. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfigurieren der Anwendung für einfache Überwachung

1. **PlayerController** Skript angefügt, das Player-Objekt zur Bearbeitung geöffnet. 

2. Fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Engagement.Unity;

3. Fügen Sie den folgenden, die `Start()` -Methode
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Bereitstellen und Ausführen der Anwendung

1. IOS-Gerät an den Computer anschließen 

2. Öffnen Sie **File-Buildeinstellungen** 

    ![][40]

3. Wählen Sie **iOS aus** und dann auf **Switch-Plattform**

    ![][41]

    ![][42]

4. Klicken Sie auf **Einstellungen** und bieten Sie einen gültigen Bundle-Bezeichner. 

    ![][53]

5. Klicken Sie auf **Erstellen und Ausführen**

    ![][54]

6. Möglicherweise müssen Sie einen Ordnernamen zum Speichern des Pakets iOS bereitzustellen. 

    ![][43]

7. Wenn alles gut geht, das Projekt kompiliert und Sie sollten es für Ihre Anwendung XCode öffnen. 

8. Stellen Sie sicher, dass **bündeln Bezeichner** im Projekt korrekt ist.  

    ![][75]

10. Führen Sie die Anwendung jetzt in XCode, sodass das Paket an das angeschlossene Gerät bereitgestellt und Sie sollten Ihr Spiel Einheit auf Ihrem Telefon! 

##<a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-Benachrichtigung und messaging-Anwendung aktivieren

Mobile Engagement ermöglicht die Interaktion mit den Benutzern und app im Rahmen von Kampagnen messaging mit Push-Benachrichtigung. Dieses Modul heißt REICHWEITE im Mobile Engagement-Portal.
Sie müssen zusätzliche Konfiguration in Ihrer Anwendung verfügbar und kann bereits Setup für sie.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
