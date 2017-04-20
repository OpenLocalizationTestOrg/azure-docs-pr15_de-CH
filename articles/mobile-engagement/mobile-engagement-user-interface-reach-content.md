<properties 
   pageTitle="Azure Mobile Engagement-Benutzeroberfläche - Inhalte erreichen" 
   description="Verwalten Sie einzigartige Inhalte verschiedener Push Notification Kampagnen in Azure Mobile Engagement" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>Einzigartige Inhalte verschiedener Push Notification Kampagnen verwalten
 
Im Abschnitt Inhalt eine neue Kampagne erreichen können Sie den Inhalt anzeigen, Umfragen, legt Daten und Kacheln (nur Windows Phone) ändern. Die Einstellung Inhalte Push-Kampagnen ist spezifisch für die Kampagne. 
 
### <a name="content-types"></a>Inhaltstypen:
- Anzeigen
- Umfragen
- Daten-Push
- Kacheln (nur Windows Phone)
 
## <a name="content-of-announcements"></a>Inhalt anzeigen
 ![Reichweite Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Wählen Sie den Typ der Ankündigung:
-    Nur Benachrichtigung: ist eine einfache standard Benachrichtigung. Das heißt, wenn ein Benutzer darauf klickt, keine zusätzliche Ansicht wird angezeigt, aber nur die zugehörige Aktion ausgeführt.
-    Text-Ankündigung: ist eine Benachrichtigung, die die Benutzer auffordert, zu betrachten angezeigt.
-    Web-Ankündigung: eine Benachrichtigung, der die Benutzer, sich eine Webansicht aktiviert ist.

### <a name="see-also"></a>Siehe auch
- [Reach - wie Tos - Ankündigung][Link 3] 

### <a name="about-web-view-announcements"></a>Über Web Ansicht anzeigen:
Vorkommen des Musters "{Deviceid}" in der HTML-Code oder JavaScript-Code, die Sie hier werden automatisch durch die ID des Geräts mit der Ankündigung ersetzt. Dies ist leicht Azure Mobile Engagement Gerätebezeichner in einen externen Webdienst auf Ihr Büro abrufen.
Wenn Sie eine Webansicht Vollbild (ohne die Aktion Exit Standardschaltflächen und wir bieten) möchten die folgenden Funktionen aus Ihrem Web View Ankündigung JavaScript-Code verwenden: 

-    die Ankündigung Aktion: ReachContent.actionContent()
-    Beenden Sie die Ankündigung: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Wählen Sie die Aktion:

### <a name="about-action-urls"></a>Über Action-URLs:
URLs ein, die von einem Zielgerät Betriebssystem interpretiert werden kann als eine Aktions-URL verwendet.
Eine URL, die die Anwendung unterstützt dedizierte (z.B. Benutzer zu einem bestimmten Bildschirm springen) kann auch als eine Aktions-URL verwendet werden.
Jedes Vorkommen von Pattern {Deviceid} wird automatisch durch die ID des Geräts, der Aktion ersetzt. Hiermit können Azure Mobile Engagement Gerätebezeichner über einen externen Webdienst auf Ihr Büro einfach abrufen.

- **Android + iOS Aktionen**
    - Öffnen einer Webseite
    - http://\[Web-Site-Domäne\] 
    - Beispiel: http://www.azure.com
    - E-mail senden
    - Mailto:\[e-Mail-Empfänger\]?subject =\[Thema\]& Body =\[Nachricht\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - SMS senden
    - SMS:\[Telefonnummer\] 
    - Beispiel: sms:2125551212
    - Rufnummer
    - Tel:\[Telefonnummer\] 
    - Beispiel: tel:2125551212
- **Nur die Aktionen für Android**
    - Eine Anwendung im Play Store herunterladen
    - Market://details?id=\[app-Paket\] 
    - Beispiel: market://details?id=com.microsoft.office.word
    - Geo-lokalisierte Suche starten
    - Geo:0, 0?q =\[Suche\] 
    - Beispiel: Geo:0, 0? Q = Starbucks, Paris
- **nur die Aktionen für iOS**
    - Eine Anwendung im App Store herunterladen
    - http://iTunes.Apple.com/ [Land] /app/ [Anwendungsname] / ID [app-Id]? m = 8 
    - Beispiel: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Windows-Aktionen
    - Öffnen einer Webseite
    - http://\[Web-Site-Domäne\] 
    - Beispiel: http://www.azure.com
    - E-mail senden
    - Mailto:\[e-Mail-Empfänger\]?subject =\[Thema\]& Body =\[Nachricht\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Senden Sie eine SMS (Skype App Store erforderlich)
    - SMS:\[Telefonnummer\] 
    - Beispiel: sms:2125551212
    - Rufnummer (Skype App Store erforderlich)
    - Tel:\[Telefonnummer\] 
    - Beispiel: tel:2125551212
    - Eine Anwendung im Play Store herunterladen
    - MS-Windows-Speicher: PDP?PFN =\[app-Paket-ID\] 
    - Beispiel: ms-Windows-Speicher: PDP? PFN = 4d91298a-07cb-40fb-Aecc-4cb5615d53c1
    - Bingmaps suchen
    - Bingmaps:?q =\[Suche\] 
    - Beispiel: Bingmaps:? Q = Starbucks, Paris
    - Ein benutzerdefiniertes Schema verwenden
    - \[benutzerdefiniertes Schema\]://\[benutzerdefiniertes Schema Params\] 
    - Beispiel: myCustomProtocol://myCustomParams
    - Verwenden Sie Paketdaten (Store-App für Erweiterung lesen erforderlich)
    - \[Ordner\]\[Daten\]. \[Erweiterung\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>Erstellen Sie eine Tracking-URL:
-    Abschnitt "Settings" die <UI Documentation> für die Anweisung auf einer Tracking-URL, der Benutzer eine andere Anwendung downloaden kann.
 
### <a name="define-the-texts-of-your-announcement"></a>Definieren Sie den Wortlaut der Mitteilung
Geben Sie Titel, Inhalt und Schaltfläche Text der Ankündigung ein. Sie können eine Benutzergruppe einer künftigen Kampagne basierend auf dem Feedback erreichen, wie Benutzer auf diese Kampagne reagiert abzielen. Zielgruppenadressierung kann basieren auf dem Feedback, ob dieser Kampagne nur, beantwortete verarbeitet oder beendeten abgelegt wurde.

### <a name="see-also"></a>Siehe auch
- [UI - Reach - Dokumentation neue Push Kriterium][Link 28]

## <a name="content-of-polls"></a>Inhalt von Umfragen
![Reichweite Content2][31] Geben Sie Titel, Beschreibung und Schaltfläche Text der Ankündigung. Fügen Sie die Optionen für die Antworten auf Ihre Fragen und Fragen an.
Sie können eine Benutzergruppe einer künftigen Kampagne basierend auf dem Feedback erreichen, wie Benutzer auf diese Kampagne reagiert abzielen. Zielgruppenadressierung kann Grundlage, ob dieser Kampagne nur, beantwortete verarbeitet oder beendeten abgelegt wurde. Zielgruppenadressierung kann auch Umfrage Antwort Feedback basieren, Frage und Antwort als Kriterien verwendet werden.

### <a name="see-also"></a>Siehe auch
- [UI - Reach - Dokumentation neue Push Kriterium][Link 28]
 
## <a name="content-of-data-pushes"></a>Inhalt der Daten legt
![Reichweite Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Wählen Sie den Typ der Daten:
- Text
- Binäre Daten
- Base64-Daten

### <a name="define-the-content-of-your-data"></a>Definieren Sie den Inhalt Ihrer Daten
- Wenn Sie Text Daten, kopieren Sie und fügen Sie den Text im Feld "Inhalt".
- Wenn Sie binäre oder base64-Daten, verwenden Sie die Schaltfläche "Hochladen Ihrer Datei" Sie Ihre Datei hochladen.
- Sie können eine Benutzergruppe einer künftigen Kampagne basierend auf dem Feedback erreichen, wie Benutzer auf diese Kampagne reagiert abzielen. Zielgruppenadressierung kann Grundlage, ob dieser Kampagne nur, beantwortete verarbeitet oder beendeten abgelegt wurde.

### <a name="see-also"></a>Siehe auch
- [UI - Reach - Dokumentation neue Push Kriterium][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Inhalt der Kacheln (nur Windows Phone)
![Content4 erreichen][33]

### <a name="define-the-content-of-your-tile"></a>Definieren Sie den Inhalt der Kachel
Kachel-Nutzlast ist der Text in der Kachel Ihrer App für Windows Phone-Geräten angezeigt werden.
Kachel-Push ist Microsoft Push Notification Service (MPNS) Version von systemeigenen Push für Windows Phone. Kachel Push-Typ ist der einzige Push, die keine Antwort und so das Publikum künftige Kampagnen kann nicht auf den Ergebnissen einer Kachel Push-Kampagne erstellt werden. 

### <a name="see-also"></a>Siehe auch
- [Systemeigene API - Reichweite API - Dokumentation-Push][Link 4]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
