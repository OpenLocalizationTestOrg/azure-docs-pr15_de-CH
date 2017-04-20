<properties 
   pageTitle="Azure Mobile Engagement-Benutzeroberfläche - Reach-Kampagne" 
   description="Laern wie erstellen und Verwalten von Push-Benachrichtigung Kampagnen mit Azure Mobile Engagement" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Zum Erstellen und Verwalten von Push Notification Kampagnen
Reach-Abschnitt der Benutzeroberfläche können Sie eine neue pushkampagne mit einer komplexen Formel erstellen, indem die Informationen müssen Sie eine Push-Benachrichtigung senden. Die Optionen einer Push-Kampagne abweichen je nach vier Kampagne: Ankündigungen, Umfragen, legt Daten und Kacheln (nur Windows Phone).

### <a name="option-applies-to"></a>Option gilt für:
- Sprachen: Alle (Ankündigungen, Umfragen, Daten drückt, Fliesen)
- Kampagne: Alle (Ankündigungen, Umfragen, Daten drückt, Fliesen)
- Benachrichtigung: Ankündigungen, Umfragen
- Inhalt: Eindeutig für jede Kampagne
- Zielgruppe: Alle (Ankündigungen, Umfragen, Daten drückt, Fliesen)
- Zeitrahmen: Ankündigungen, Umfragen, Kacheln
- Test: Alle (Ankündigungen, Umfragen, Daten drückt, Fliesen)
 
![Campaign1 erreichen][20]

## <a name="languages"></a>Sprachen
Das Sprachen-Dropdown-Menü können Sprachen verwenden Geräte eine andere Version der Push an. Standardmäßig erhalten alle Geräte den gleichen Push unabhängig von der Sprache, die Sie verwenden soll. Benutzer mit einem Gerät eine andere Sprache erhalten die Standardsprache Version der Push-Installation. Viele Optionen Kampagne Push können alternativen Inhalt für jede zusätzliche Sprache angeben, die Sie auswählen. 
 
![Campaign2 erreichen][21]

### <a name="language-differences-apply-to"></a>Sprache gelten:
- Sprachen: Eindeutige Sprachen möglicherweise zusätzlich standardmäßig aktiviert
- Kampagne: Identisch für alle Sprachen
- Benachrichtigung: Für jede Sprache neben der Standardsprache
- Inhalt: Für jede Sprache neben der Standardsprache
- Zielgruppe: Möglicherweise nach einem Kriterium Sprache gefiltert werden
- Zeitrahmen: für alle Sprachen
- Test: Kann in jeder Sprache gleichzeitig gesendet werden
 
### <a name="supported-languages"></a>Unterstützte Sprachen:
- Arabisch (Ar) 
- Bulgarisch (bg) 
- Katalanisch (ca) 
- Chinesisch (Zh) 
- Kroatisch (hr) 
- Czech (cs) 
- Dänisch (da) 
- Niederländisch (nl) 
- Englisch (En) 
- Finnisch (Fi) 
- Französisch (fr) 
- Deutsch (de) 
- Griechisch (el) 
- Hebräisch (he) 
- Hindi (hi) 
- Ungarisch (Hu) 
- Indonesisch (Id) 
- Italienisch (it) 
- Japanisch (ja) 
- Koreanisch (Ko) 
- Lettland (lv) 
- Litauisch (Lt) 
- Malaiisch (Macrolanguage) (ms) 
- Norwegisch-Bokmål (nb) 
- Polnisch (pl) 
- Portugiesisch (pt) 
- Rumänisch (Ro) 
- Russisch (ru) 
- Serbisch (sr) 
- Slowakisch (sk) 
- Slowenien (sl) 
- Spanisch (es) 
- Schwedisch (Sv) 
- Tagalog (tl) 
- Thailändisch (th) 
- Türkisch (tr) 
- Ukrainisch (uk) 
- Vietnamesisch (vi) 
 
## <a name="campaign"></a>Kampagne
Abschnitt Kampagne können Sie den Namen und die Kategorie der Kampagne sowie festzulegen wie möchten Sie die Benutzergruppe einer Push-Kampagne ignorieren und stattdessen Senden dieser Kampagne über die API erreichen (und einige Elemente mit niedrigen Push-API). Kategorien können mit benutzerdefinierten Benachrichtigungsvorlage Steuerelement in app Benachrichtigungen basierend auf vordefinierten Einstellungen verwendet werden. Sie erhalten eine Liste der bestehenden "Kategorien" API erreichen.

> Warnung: Verwenden Sie die Option "Ignorieren Publikum Push Benutzer API erhalten" im Abschnitt "Kampagne" eine Kampagne erreichen die Kampagne wird nicht automatisch senden, müssen Sie manuell API erreichen senden.
 
![Campaign3 erreichen][22]
 
### <a name="option-applies-to"></a>Option gilt für:
- Name: alle
- Kategorie: Ankündigungen, Umfragen
- Ignorieren Publikum Push erhalten Benutzer API: alle
 
## <a name="notification"></a>Benachrichtigung
Können Sie Abschnitt Benachrichtigung Standardeinstellungen für die Push-einschließlich festgelegt: der Titel der Push Nachricht, ein Bild in app oder können. Viele Einstellungen sind spezifisch für die Plattform des Geräts. Sie können auswählen, ob die Push "in Anwendung" oder "app" gesendet werden oder beides. (Beachten Sie, dass Benutzer können "Anmelden" oder "opt-Out" "aus app" am Betriebssystem Ebene auf ihren Geräten legt und Azure Mobile Engagement dürfen diese Einstellung überschreiben. Auch daran-API erreicht behandelt "app" und "app aus" drückt. Die Push-API lässt sich "von app" Push zu verarbeiten.) Push können Bilder oder HTML-Inhalt, einschließlich deep Links zum Verknüpfen von außerhalb Ihrer App oder an eine andere Stelle in Ihrer App (Android SDK 2.1.0 oder höher beabsichtigte Kategorien erforderlich) angepasst werden. Sie können Symbol oder iOS Badge ändern und Text oder Web Inhalte (ein Popup-Fenster mit HTML-Inhalt an eine andere Position innerhalb oder außerhalb der app). Sie können Geräte Ring vornehmen oder mit schwingt. (Beachten Sie, dass Sie den richtigen benötigen Berechtigungen SDK für Android-Manifestdatei ring oder ein Vibrieren.) Es ist derzeit kein standard für Android "Überblick" Größen da Bildschirmgrößen auf jedes Gerät, aber fast jede Bildschirmgröße 400 x 100 Bilder bearbeiten.

### <a name="delivery-types"></a>Lieferarten:
-    Nur App: Benachrichtigung gesendet, wenn der Benutzer die Anwendung nicht verwendet.
- Außerhalb der Anwendung nur Benachrichtigung erfordert ein Zertifikat von Apple oder Google (APN GCM Zertifikat).
- Innerhalb der app nur: die Benachrichtigung angezeigt wird, wenn die Anwendung ausgeführt wird.
- Die Benachrichtigung verwendet das System Capptain Benutzer zu. Sie können die visuelle Layout/Anzeige der Push vollständig anpassen.
- Immer: Diese Option wird sichergestellt, dass eine Benachrichtigung zu senden, der die Anwendung ausgeführt wird.

 
![Campaign4 erreichen][23]

### <a name="option-applies-to"></a>Option gilt für:
- Benachrichtigung: Ankündigungen, Umfragen
 
## <a name="content"></a>Inhalt
Im Abschnitt Inhalt können Sie den Inhalt der Ankündigung, Umfragen legt Daten und Kacheln (nur Windows Phone) ändern. Die Einstellung Inhalt Push-Kampagnen ist spezifisch für die Kampagne. 

### <a name="see-also"></a>Siehe auch
- [Dokumentation von UI - erreichen - Push Inhalt][Link 29]
 
![Campaign5 erreichen][24]

## <a name="audience"></a>Zielgruppe
Die Benutzergruppe können Sie eine Liste aller Elemente beschränken Ihre Kampagne oder Grenzwerte die Kampagne auf benutzerdefinierten Kriterien basierend definieren. Der Standardsatz von Optionen zum Einschränken der Zielgruppe können Sie auf neue oder alte oder systemeigenen Push-Benutzer. Sie können auch festlegen, dass ein Kontingent auf die Anzahl der Benutzer begrenzen, die den Push zu erhalten. Sie können den Ausdruck wie Ihre Kampagne auf mindestens ein Kriterium für Zielgruppen gefiltert werden manuell bearbeiten. Sie können einen Publikum Ausdruck manuell eingeben. Ein Ausdruck muss die Beziehung zwischen explizit definieren. Ein Kriterium beschreibt Bezeichner, die mit einem Großbuchstaben beginnen und darf keine Leerzeichen enthalten. Die Beziehung zwischen den Kriterien mit beschrieben 'und', 'oder' keine' Operatoren sowie '(',')'. Beispiel: "Criterion1 oder (Criterion1 und nicht Criterion2)".

> Hinweis: Für eine große Zielgruppe in Kampagnen enthalten, kann Scan auf Serverseite langsam sein, insbesondere dann, wenn Sie versuchen, mehrere Kampagnen gleichzeitig starten.

- Wenn möglich, erst eine Kampagne gleichzeitig.
- Die nur starten Sie vier Kampagnen gleichzeitig.
- Drücken Sie, um die aktive Benutzer (Kontrollkästchen "führen nur Benutzer, die mit systemeigenen Push erreicht werden können" und "aktive Benutzer"), damit nur die Benutzer, die noch die Anwendung installiert und es gescannt werden müssen.
Nach der Definition der Zielgruppe können Schaltfläche simulieren Sie herausfinden, wie viele Benutzer diese Push erhalten. Dadurch wird die Anzahl der bekannten Benutzer möglicherweise diese Zielgruppe (Dies ist eine Schätzung anhand einer Stichprobe von Benutzern) berechnen. Werden Sie, dass Benutzer die Anwendung deinstalliert Bestandteil dieser Zielgruppe können nicht erreicht werden.

### <a name="see-also"></a>Siehe auch
- [UI - Reach - Dokumentation neue Push Kriterium][Link 28]

![Campaign6 erreichen][25]

### <a name="edit-expression"></a>Ausdruck bearbeiten
![Campaign7 erreichen][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Begrenzung die Zielgruppe Option gilt:
- Führen Sie nur eine Teilmenge der Benutzer: alle (Ankündigungen Umfragen Daten legt, Fliesen)
- Nur alte Benutzer: alle (Ankündigungen Umfragen Daten legt, Fliesen)
- Nur neue Benutzer: alle (Ankündigungen Umfragen Daten legt, Fliesen)
- Nur im Leerlauf Benutzer: Ankündigungen, Umfragen, Kacheln
- Nur aktive Benutzer: alle (Ankündigungen Umfragen Daten legt, Fliesen)
- Nur Benutzer, die mit systemeigenen Push erreicht werden kann: Ankündigungen, Umfragen
 
## <a name="time-frame"></a>Zeitrahmen
Abschnitt Zeitrahmen können fest, wenn der Push gesendet oder können Sie den Zeitrahmen die Kampagne sofort leer lassen. Beachten Sie, dass die Endbenutzer Zeitzone kann die Kampagne einen Tag früher als für Endbenutzer in Asien erwartet und Kleinserien Push nacheinander senden, bis alle Zeitzonen in der Welt des Zeitrahmens für die Kampagne festgelegt übereinstimmen. Die Endbenutzer Zeitzone kann auch Verzögerungen in Kampagnen seit es die Zeit vor dem Starten des Push vom Telefon anfordern.

> Hinweis: Kampagnen ohne Enddatum Zwischenspeichern kann Push lokal und noch sie Sie manuell abgeschlossen Kampagnen anzeigen. Zum Vermeiden des Problems, spezifische Endzeit für Kampagnen.

### <a name="see-also"></a>Siehe auch
- [Reach - wie Tos-Planung][Link 3] 
 
![Campaign8 erreichen][27]

### <a name="settings-apply-to"></a>Systemeinstellungen gelten:
- Zeitrahmen: Ankündigungen, Umfragen, Kacheln
 
## <a name="test"></a>Test
Im Abschnitt können an diesem Push eigene Testgerät vor dem Speichern der Kampagne. Wenn Sie alle benutzerdefinierten Sprachen für diese Kampagne konfiguriert haben, können Sie den Push in jeder Sprache testen. Sie können ein Testgerät von "Mein Konto" einrichten.
> Hinweis: Keine serverseitige Daten protokolliert, wenn Sie die Schaltfläche mithilfe "test" drückt, Daten nur für echte pushkampagnen protokolliert.

### <a name="see-also"></a>Siehe auch
- [UI-Dokumentation – Mein Konto][Link 14]
 
![Campaign9 erreichen][28]

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
 
