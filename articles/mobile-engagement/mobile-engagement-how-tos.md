<properties 
   pageTitle="Azure Mobile Engagement-Benutzeroberfläche - Reichweite wie"
   description="User Interface Overview für Azure Mobile Engagement" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Erste Schritte, Verwendung und Verwaltung legt Ihre Endbenutzer erreichen

Wenn das SDK die app vollständig integriert ist, können Sie loslegen mit der Reichweite Abschnitt der Benutzeroberfläche Push-Benachrichtigung an die Benutzer der Anwendung.  

## <a name="do-your-first-push-notification-campaign"></a>Die erste Push Notification Kampagne
-    Bestätigen Sie, dass Ihre Reichweite Ihrer app SDK integriert. 
-    Wählen Sie Ihre Anwendung
 
![1][1]

-    Besuchen Sie die "Reichweite" und klicken Sie auf "Startseite"
 
![First2][2]

-    Erstellen Sie eine neue Kampagne, und nennen Sie es
 
 ![First3][3]

-    Wählen Sie, wie die Benachrichtigung übermittelt werden soll, als In-app nur
 
![First4][4]

-    Erstellen Sie die Nachricht, die Sie übertragen möchten
 
![First5][5]

-    Sie können einen Titel für die Benachrichtigung (Optional) schreiben.
-    Schreiben Sie Push Nachrichteninhalt.
-    Sie können ein Bild hochladen. Beachten Sie, dass die Größe der Datei 32.768 Byte nicht überschreiten darf.
-    Sie haben auch die Möglichkeit, weitere Optionen auswählen, aber für das Ziel dieses Lernprogramms wir sehen, später.

-    Wählen Sie nur den Inhaltstyp als Benachrichtigung
 
![First6][6]

-    Pushkampagne erstellen und erscheint in der Kampagnenliste.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>Testen Sie Ihre Kampagne Push Notification
![Test1][8]

-    Registrieren Sie Ihr Gerät.
-    Klicken Sie auf das Kontrollkästchen des Geräts, das Sie übertragen möchten.
-    Klicken Sie auf die Schaltfläche "Test" den Push an das Gerät senden.
 
![Test2][9]

-    Aktivieren Sie die Kampagne
 
![. 3][10]

-    Erstellung Ihrer Kampagne müssen Sie nur für die Benachrichtigung an die Benutzer abgelegt werden aktivieren.
 
## <a name="send-personalized-pushes"></a>Senden von personalisierten Push
-    Dieses Beispiel erstellt einen Push, Push-Benachrichtigung einen benutzerdefinierten Rückvergütungscode eingegangen wird.
 
![Personalize1][11]

Personalisierung funktioniert so eine Markierung durch eine app Info Tags ersetzen, müssen Sie sicherstellen, dass der Benutzer die richtige app Info zunächst definiert. In diesem Beispiel müssen die Zielgruppe einer app Info-Tag mit dem Namen Rebate_code definiert.
Siehe oben enthält den Inhalt der Push-Benachrichtigung Marke ${Rebate_code} die laut ist der tatsächliche Inhalt der app Info Tag ersetzt werden.

> Warnung: Wenn das app Info-Tag für den Benutzer nicht definiert ist, erhält der Benutzer nicht den Push.

-    Ergebnis
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>Sie können dem Text die Benachrichtigung persönlicher gestalten
![Personalize3][13]

-    Einschließlich des Titels der Benachrichtigung
-    Und den Inhalt der Nachricht.
-    Wählen Sie den Typ der Ankündigung (Text oder an)
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>Der Hauptteil einer Ankündigung kann auch angepasst werden:
-    Die Aktions-URL sollten Sie die Startseite anpassen
-    Der Titel
-    Der Text der Nachricht.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Unterscheiden der Push-Benachrichtigung (in oder App)
-    Wählen Sie die Art der Benachrichtigung Sie push, wählen Sie die Anwendung, wechseln Sie zum Abschnitt "Reach", auswählen oder Push Kampagnen und gehen Sie zum Abschnitt "Benachrichtigung".
 
-    Klicken Sie auf "Lieferart" werden soll.
-    Klicken Sie auf das Kontrollkästchen "Aktivitäten einschränken" möchten die Benachrichtigung tritt bei bestimmten Aktivitäten (Bildschirm).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Aus App" Lieferart
![Differentiate2][16]

"Aus App" Lieferart bietet Push-Benachrichtigung, wenn die Anwendung geschlossen wird. Dies ist die standard-Push-Benachrichtigung.
Bei Auswahl von "aus app" müssen Sie bereits die Zertifikate von der Plattform angegeben haben, die Erstellen der Anwendung (APN oder GCM).

### <a name="see-also"></a>Siehe auch
-  [Apple Push Notification Service-Zertifikate](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), Google Cloud Messaging-Zertifikat] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"in-App nur" Lieferart
![Differentiate3][17]

"In-App nur" Lieferart bietet Push-Benachrichtigung, wenn die Anwendung ausgeführt wird.
Für diese Benachrichtigung müssen nicht über das APN und GCM wechseln.
Das System innerhalb der app können die Endbenutzer erreichen.
Sie können die Benachrichtigung anpassen und entscheiden, in der Aktivität (Bildschirm) der Benachrichtigung angezeigt werden.

### <a name="anytime-delivery-mode"></a>"Überall" Lieferart
Sie können eine Lieferart "Jederzeit" wählen, wird gewährleistet, dass Ihre Endbenutzer erreichen, ob die Anwendung ausgeführt wird.
Wenn Sie "Immer" auswählen, müssen Sie bereits die Zertifikate von der Plattform angegeben haben, die Erstellen der Anwendung (APN oder GCM). 
 
## <a name="schedule-a-push-campaign"></a>Eine Pushkampagne planen
### <a name="plan-to-start-a-campaign"></a>Planen eine Kampagne
![Shedule1][18]

21. März und Sie haben eine Ankündigung zu für den 22. März um Mitternacht geplant. Sie müssen vor der Schnittstelle dazu einen Push bleiben! Sie können die genaue Uhrzeit planen Benachrichtigungen gesendet werden.
-    Deaktivieren Sie "None" Kontrollkästchen, und wählen Sie eine Anfangszeit 
-    Wählen Sie das Datum und die Uhrzeit die pushkampagne beginnen soll.

### <a name="plan-to-end-a-campaign"></a>Planen einer Kampagne
![Shedule2][19]

Die Kampagne am 25. März um 3 Uhr beenden soll, aber wissen, dass Sie es nicht zu tun.
Sie müssen vor der Schnittstelle müssen bleiben! Sie können die genaue Uhrzeit Planen Ihrer Kampagne halten.
-    Klicken Sie auf "None" auf das Kontrollkästchen oder eine Endzeit auswählen
-    Wählen Sie das Datum und die Uhrzeit pushkampagne beendet werden soll.

### <a name="end-a-campaign-manually"></a>Eine Kampagne manuell beenden
![Shedule3][20]

In der Standardeinstellung "Keine" Kontrollkästchen aktiviert sind.
Dies bedeutet, dass die Kampagne im Abschnitt Reichweite aktivieren und endet, wenn Sie im Abschnitt Reichweite beendet werden.
 
> Hinweis: Kampagnen ohne Enddatum den Push lokal auf dem Gerät speichern und zeigen sie das nächste Mal, das die Anwendung geöffnet wird, selbst wenn die Kampagne manuell beendet wird.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Erweitern Sie eine Push-Benachrichtigung angezeigt
### <a name="what-is-a-text-view"></a>Was wird angezeigt?
![TextView1][21]

Angezeigt wird ein Popupfenster mit Textinhalt. Dieses Popupfenster wird angezeigt, nachdem der Endbenutzer auf die Push-Benachrichtigung geklickt hat.
Eine Textansicht können Sie weitere Inhalte an den Endbenutzer. Dies ist auch die Möglichkeit, zu Aktionen wie das Wechseln zu einer Seite Ihrer Anwendung umleiten an einen Speicher Öffnen einer Webseite per e-Mail, Starten einer Suche Geo lokalisiert usw....

### <a name="example-text-view"></a>Beispiel: Ansicht
-    Im Abschnitt "Reichweite" erstellen Sie Push Notification Kampagne und benennen Sie Ihre Kampagne
 
![TextView2][22]

-    Schreiben der Nachricht, die angezeigt werden auf die Benachrichtigung.
-    Wählen Sie die Ankündigung Inhaltstyp "Text"
 
![TextView3][23]

> Hinweis: beim Drücken einer Textansicht erfolgt immer eine Benachrichtigung zuerst. 

- Definieren Sie den Text (nach Auswahl den Textinhalt Ankündigung Unterabschnitt erscheint, können Sie den Text fest, der angezeigt werden soll.)
 
![TextView4][24]

-    Schreiben Sie den Titel, der am oberen Rand der Nachricht angezeigt wird.
-    Schreiben Sie den Hauptinhalt der Textansicht.
-    Schreiben Sie den Inhalt, der angezeigt wird, auf Aktion (Aktionsschaltfläche kann die Anwendung eine bestimmte Aktion wie eine Seite der Anwendung umleiten auf appstore oder, die Sie bereitstellen können).
-    Schreiben Sie den Inhalt, die angezeigt werden auf die Schaltfläche Beenden (durch Klicken auf die Schaltfläche Beenden, die Textansicht verschwindet.)
 
-    Push Notification Kampagne erstellen und der Kampagnenliste erscheint.
 
![TextView5][25]

-    Aktivieren Sie Push Notification Kampagne Textansicht an die Benutzer senden.
 
![TextView6][26]

-    Ergebnis
 
![TextView7][27]

-    Der Benutzer erhält die Benachrichtigung und klicken.
-    Die Textansicht wird als einem der Benutzer damit interagieren.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Eine Push-Benachrichtigung mit einer Webansicht verbessern
### <a name="what-is-a-web-view"></a>Was ist eine Webansicht?
![WebView1][28]

Eine Webansicht ist ein Popupfenster mit Webinhalten. Dieses Popupfenster angezeigt wird, wenn der Endbenutzer auf die Push-Benachrichtigung geklickt hat.
Eine Webansicht ermöglicht mehr Interaktion mit dem Endbenutzer.
Dies ist auch die Möglichkeit, einen Aufruf wie Umleitung App Store usw. Öffnen einer Webseite per e-Mail, Geo-lokalisierte Suche starten...

### <a name="example-web-view"></a>Beispiel: Webansicht
-    Im Abschnitt "Reichweite" erstellen Sie pushkampagne und benennen Sie Ihre Kampagne.
 
![WebView2][29]

-    Schreiben der Nachricht, die angezeigt werden auf die Benachrichtigung.
-    Wählen Sie den Inhaltstyp Ankündigung als "Web"
 
![WebView3][30]

### <a name="about-announcement-types"></a>Zu Ankündigung:
- Nur Benachrichtigung: ist eine einfache standard Benachrichtigung. Das heißt, wenn ein Benutzer darauf klickt, keine zusätzliche Ansicht wird angezeigt, aber nur die zugehörige Aktion ausgeführt.
- Text-Ankündigung: ist eine Benachrichtigung, die die Benutzer auffordert, zu betrachten angezeigt.
- Web-Ankündigung: eine Benachrichtigung, der die Benutzer, sich eine Webansicht aktiviert ist.
Wählen Sie die "Web" Inhalt der Ankündigung.

> Hinweis: Wenn Sie eine Webansicht drücken, erfolgt immer eine Benachrichtigung zuerst.

- Definieren Sie Webinhalte (nach Auswahl Webinhalte Ankündigung im Unterabschnitt erscheint, können Sie die Webinhalte zu definieren, die angezeigt werden sollen.)

 
![WebView4][31]

-    Schreiben Sie den Titel, der am oberen Rand der Nachricht (optional) angezeigt wird.
-    Hier Ihren HTML-Code zu schreiben.
-    Klicken Sie auf die Bearbeiten-Schaltfläche um Edition und wie es aussieht.
-    Schreiben Sie den Inhalt, der angezeigt wird, auf Aktion (Aktionsschaltfläche kann die Anwendung eine bestimmte Aktion wie eine Seite der Anwendung umleiten an einen Informationsspeicher oder, die Sie bereitstellen können).
-    Schreiben Sie den Inhalt, die angezeigt werden auf die Schaltfläche Beenden (durch Klicken auf die Schaltfläche Beenden, die Webansicht verschwindet).
 
-    Ergebnis
 
![WebView5][32]

-    Der Benutzer der Benachrichtigung und klicken Sie darauf.
-    Die Textansicht wird als einem der Benutzer damit interagieren.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
