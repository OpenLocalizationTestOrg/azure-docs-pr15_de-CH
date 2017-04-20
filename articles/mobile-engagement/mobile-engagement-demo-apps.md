<properties
    pageTitle="Azure Mobile Engagement demoApp | Microsoft Azure"
    description="Beschreibt, wo Sie herunterladen, verwenden und die Vorteile von Azure Mobile Engagement demoApp"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile Engagement demoApp

Wir haben eine Azure Mobile Engagement demoApp für **iOS**, **Android**und **Windows** -Plattformen können Sie nützliche Ressourcen und Weitere Informationen zu Mobile Engagement veröffentlicht.

Die Anwendung unterstützt Sie beim:

- Finden Sie Links zu Ressourcen wie Videos, Dokumentation und Support-Forum, wo Wünsche zu Mobile Engagement leicht.
- Erleben Sie Beispiel-Benachrichtigung, die von Mobile Engagement Ideen für eigene Dienste unterstützt.
- Verwenden Sie eine referenzimplementierung, wie Mobile Engagement in Ihre Anwendung implementieren. Sie können Folgendes lernen:

    - Erfassen Sie Analysedaten.
    - Erweiterte Benachrichtigungsszenarien Typen oder *Vollbild interstitial* *Popup-*implementieren.
    - Umfragen zu implementieren.
    - Automatische Push Daten und Push-Szenarios zu implementieren.   

## <a name="app-installation"></a>App-installation
Diese app ist in den folgenden app speichern:

- **Universal Windows demoApp**:

    - Die app [Windows App store](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2)herunterladen
    - Die Anwendung wurde als 10 Universal Windows Anwendung entwickelt. Der Quellcode ist auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows)verfügbar.

- **iOS demo app**:

    - Bei [Apple store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090)herunterladen.
    - Die Anwendung wurde in iOS Swift entwickelt. Der Quellcode ist auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios)verfügbar.

- **Android demoApp**:

    - Die App in [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
    - Der Quellcode ist auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android)verfügbar.

![Universal Windows demoApp][1]

![iOS demo app][2]
![Android demoApp][3]


## <a name="usage"></a>Verwendung

Diese Anwendung können Sie auf folgende Weise:

**Die Anwendung Speicher Links (oben) herunterladen Sie die Anwendung auf dem Gerät:**

>[AZURE.IMPORTANT] Sie brauchen kein Azure-Konto oder ein Back-End die Anwendung herstellen müssen. Die Anwendung funktioniert unabhängig.

- Nachdem Sie die Anwendung auf dem Gerät haben, können Sie über die Links im Menü links zu hilfreichen Ressourcen über Mobile Engagement gehen.
- Wir haben [Service RSS-feed](https://aka.ms/azmerssfeed) in der Anwendung hinzugefügt, damit Sie immer über die neuesten Produktupdates aktualisiert werden.
- Sie gelangen auch über die Benachrichtigung Beispielszenarios zu Benachrichtigungstypen, die von Mobile Engagement für jede Plattform unterstützt werden. Diese Notifizierungen erlebt lokal – das, klicken Sie die Schaltflächen auf dem Bildschirm angezeigt Erfahrung Benachrichtigungen senden der Benachrichtigung von Mobile Engagement Plattform identisch ist.

![Anw.-Menü für Windows][4]

![Menü App für iOS][5]
![App Menü für Android][6]

**Die GitHub Links (oben) herunterladen Sie den Quellcode:**

- Nachdem Sie den Quellcode gedownloadet haben, öffnen sie in der jeweiligen Umgebung XCode für iOS, Android Studio für Android und Visual Studio für Windows.
- Anschließend befolgen die [SDK-Integrationsschritte](mobile-engagement-windows-store-dotnet-get-started.md) Sie, damit Sie ihre eigene Mobile Engagement Backend-Instanz dieser Anwendung herstellen können.
    - Sie müssen eine Verbindungszeichenfolge in der Datei app.
    - Sie müssen auch die Push Notification Plattform für Ihre Anwendung.
- Sie werden feststellen, dass die Anwendung selbst mit Mobile instrumentiert. Daher beim Öffnen der Anwendung nach der Verbindung mit dem Back-End werden Sie Sitzung, Aktivitäten, Ereignisse und So weiter auf der Registerkarte **Monitor** angezeigt.
- Sie auch werden Benachrichtigungen App eigene Instanz Mobile Engagement anstatt lokale Benachrichtigungen senden.
    - Hier können Sie mithilfe des **Geräte-ID abrufen** Menü in der Anwendung das Gerät als Testgerät hinzufügen. Dadurch erhalten Sie eine Geräte-ID, die Sie dann als ein Test mit der Back-End-Plattform registrieren.

    ![Geräte-ID unter Windows][7]

    ![Geräte-ID in iOS][8]
    ![Geräte-ID für Android][9]

## <a name="key-features-of-the-demo-app"></a>Hauptmerkmale der demoApp

- Mit dieser Anwendung erwähnt, haben Sie alle wichtigen Ressourcen für Mobile in der Hand. Sie können über die Links im linken Menü auf wechseln.

- Erleben Sie Out-of-app-Benachrichtigung für jede Plattform. Diese Notifizierungen lieferbar **nur Benachrichtigung**, auf die Benachrichtigung einfach einen einheitlichen Bildschirm der Anwendung geöffnet wird (mithilfe von **deep links**) – oder als ein **Web-Ankündigung**, wo Sie liefern zusätzliche HTML-Inhalte von Mobile Engagement-Back-End angezeigt, wenn die Benachrichtigung geklickt wird.

    ![Out-of-app-Benachrichtigung][29]

- IOS müssen Sie die app-app oder System Push-Benachrichtigung finden Sie unter schließen. Sie können die Implementierung für **interaktive Schaltflächen**hinzufügen, wie diejenigen, die diese Benachrichtigung app für *Feedback* und *Freigabe* hinzugefügt werden (so dass der Benutzer Aktionen direkt aus der Meldung) betrachten.

    ![IOS-app Benachrichtigungen][11] ![Benachrichtigungsanzeige auf iOS-app][14]

- Android hinzufügen die unterstützten Optionen mehrzeiligen Text (**Big Text**) oder ein Bild Benachrichtigung (**Überblick**) Benachrichtigung mit **interaktiven Schaltflächen** (wie iOS).

    ![Anträge auf Android-app][12] ![Benachrichtigungsanzeige auf Android-app][15]

- Windows 10 können Sie sehen, wie die Anträge auf dem PC suchen. Diese Benachrichtigung wird auch in Windows 10 **Notification Center**. Es unterstützt keine **interaktive Schaltflächen** gleichzeitig im Windows SDK hinzufügen.

    ![Out-of-app-Benachrichtigung unter Windows][10] ![Out-of-app-Anzeige unter Windows][13]

- Erleben Sie standardmäßig "in Anwendung" Benachrichtigung für jede Plattform. Hier wird eine zweistufige Erfahrung **ein Benachrichtigungsfenster** zuerst angezeigt. Wenn Sie darauf klicken, öffnet es Vollbildmodus **Ankündigung**, wie im folgenden Screenshot angezeigt. Der Inhalt stammt aus der Mobile Engagement Backend-Instanz. Das SDK enthält Vorlagen für Benachrichtigungen und anzeigen. Sie können einfach wie in dieser Demo-Anwendung mit dem Logo und Farbe anpassen.  

    ![In app-Benachrichtigungen unter Windows][16]

    ![IOS-app Benachrichtigungen][17]  ![In app-Benachrichtigungen für Android][18]

    **iOS**, **Android**

- **Kategorie** -Funktion von Mobile Engagement können Sie diese Standardeinstellung anpassen. In der demoApp haben wir zwei gängige Methoden, die Erfahrung der Benachrichtigungen gezeigt. Beachten Sie, dass das Kategorie Feature noch nicht im Windows SDK unterstützt wird.

    **Vollbild-interstitial:**

    ![In app-Benachrichtigung - Interstitial Kategorie][30]

    ![Interstitial Kategorie iOS][21]  ![Interstitial Kategorie Android][22]

    **Popup-Benachrichtigung:**

    ![In app-Benachrichtigung - Popupmenü Kategorie][31]

    ![Popupbenachrichtigung IOS][19]   ![Popupbenachrichtigung für Android][20]

**iOS**, **Android**

- Mobile Engagement unterstützt auch einen speziellen Typ von in app-Benachrichtigung **Umfragen**aufgerufen. Dadurch werden Benutzer segmentierten app schnelle Umfragen übermitteln. Sie können Optionen für jede Frage wie in der folgenden Abbildung und Fragen hinzufügen. Dies wird dann als eine Benachrichtigung in Anwendung der app-Benutzer angezeigt.   

    ![Umfrage-Benachrichtigung][32]

    ![Umfrage unter Windows][26]

    ![Umfrage auf iOS][27]   ![Umfrage auf Android][28]

**iOS**, **Android**

- Mobile Engagement unterstützt die automatische **Daten** Benachrichtigung gesendet. Mit dieser Benachrichtigung senden Sie Daten von Ihrem Dienst (wie die JSON-Daten folgt), die in Ihrer Anwendung verarbeiten und eine bestimmte Aktion. Ein Beispiel ist, wie wir den Preis eines Elements selektiv ändern mit Daten Pushbenachrichtigungen.

    ![Daten-Push-Benachrichtigung][33]

    ![Daten-Push-Benachrichtigung unter Windows][23]

    ![Daten-Push-Benachrichtigung auf iOS][24]  ![Daten-Push-Benachrichtigung für Android][25]

**iOS**, **Android**

> [AZURE.NOTE] Benachrichtigung Beispielbildschirm auf **wie diese Benachrichtigungen von Mobile Engagement-Plattform hier** können Sie detaillierte Anleitung für alle Benachrichtigungen anzeigen.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
