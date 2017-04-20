<properties 
   pageTitle="Azure Mobile Engagement-Benutzeroberfläche - Reichweite" 
   description="Erfahren Sie, wie Benutzer der Anwendung mit Pushbenachrichtigungen verwenden Azure Mobile Engagement erreichen" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Wie Sie den Benutzern Ihrer Anwendung Pushbenachrichtigungen erreichen

Dieser Artikel beschreibt die **erreichen** Registerkarte **Mobile Engagement** -Portal. Überwachen und Verwalten der mobilen apps verwenden Sie **Mobile Engagement** -Portal. Beachten Sie, dass zum Starten über das Portal Sie zunächst ein Konto **Azure Mobile Engagement** zu erstellen müssen. Weitere Informationen finden Sie unter [Azure Mobile Engagement Konto erstellen](mobile-engagement-create.md).

Reach-Abschnitt der Benutzeroberfläche ist das Push Kampagne Verwaltungstool erstellen/bearbeiten/aktivieren/Ende/Monitor können und Statistiken über Push Notification Kampagnen und Features, die auch über API erreichen (und einige Elemente des niedrigen Push-API) zugegriffen werden können. Denken Sie daran, ob die APIs oder die Benutzeroberfläche Sie integrieren müssen Azure Mobile Engagement und Reichweite in der Anwendung für jede Plattform SDK vor der Verwendung Kampagnen zu erreichen.

>[AZURE.NOTE] In vielen Abschnitten der **Mobile Engagement** -Portal Benutzeroberfläche enthalten die Schaltfläche **Hilfe anzeigen** . Weitere Kontextinformationen über einen Abschnitt zu drücken.

## <a name="four-types-of-push-notifications"></a>Vier Arten von Pushbenachrichtigungen
1.    Ankündigungen - können Sie Werbung an Benutzer senden, die sie zu einem anderen Speicherort in Ihrer Anwendung oder zu einer Webseite außerhalb Ihrer app Store zu senden. 
2.    Umfragen - können Sie Informationen von Benutzern sammeln, Fragen sie.
3.    Daten-Push - können Sie eine binäre oder base64-Datei. Die Informationen in einem datenpush wird die Anwendung die Benutzer Kenntnissen in Ihrer Anwendung ändern gesendet. Die Anwendung muss die Daten in einen datenpush verarbeiten.

## <a name="campaign-details"></a>Kampagnendetails

Sie bearbeiten, kopieren, löschen oder Aktivieren von Kampagnen, die noch nicht aktiviert haben durch ihre Namen bewegen oder klicken Sie auf, um sie zu öffnen. Kampagnen, die bereits aktiviert wurden, durch ihre Namen bewegen Klonen oder klicken Sie auf, um sie zu öffnen. Jedoch kann eine Kampagne nach Aktivierung geändert werden.
 
![Reach1][18]

## <a name="reach-feedback"></a>Feedback zu erreichen

Klicken Sie auf **Statistik** Reichweite Kampagne angezeigt. Die **einfache** Ansicht bietet eine visuelle Darstellung in Form eines Balkendiagramms Spalte was passiert nach dem Aktivieren einer Kampagne. Die **Erweiterte** Ansicht bietet detailliertere Informationen zur pushkampagne. Diese Informationen werden nicht zur Verfügung, wenn einer Testkampagne also einen Push ein Test gesendet senden. Hier ist die Interpretation dieser Informationen sollte:

1. **Pushed** - gibt die Anzahl der Nachrichten auf Geräte übertragen. Diese Zahl hängt der Zielgruppe beim Erstellen der pushkampagne angegeben. Wenn Sie keine gewünschte Zielgruppe angeben, wird dann dieser Push an alle registrierten Geräte gesendet werden. Wie alle anderen Push-Dienste wir nicht direkt mit den Geräten Pushbenachrichtigungen sondern stattdessen der jeweiligen Plattform push bestimmten Push Notification Services (PNS - GCM/APN/WNS), damit sie die Benachrichtigung Geräte liefern können. 

2.  **Übermittelte** - gibt die Anzahl der Nachrichten, die erfolgreich vom PNS an das Gerät gesendet und bestätigt als von Mobile Engagement SDK. 
        
    *Gründe für Lieferung zählen kleiner als abgelegten zählen:*
    
    1. Wenn der Benutzer hat die app vom Gerät deinstalliert, aber PNS nicht wissen zum Zeitpunkt den Push PNS wir senden wird die Nachricht gelöscht.
    2. Wenn das Gerät die app hat Geräte für längere Zeit offline waren jedoch fehl PNS übermitteln der Nachricht an das Gerät. 
    3. Die Nachricht zugestellt, das Gerät aber Mobile Engagement SDK in die Anwendung den Inhalt der Nachricht erkennt, löscht es die Nachricht. Dies kann passieren, wenn die Anpassung der Benachrichtigung in der Anwendung generiert, wir fangen, eine Ausnahme im SDK und Ablegen der Nachricht. Dies kann auch auftreten, wenn die Anwendung auf dem Gerät mit einer Version des Mobile Engagement-SDK ist nicht die neuere Version der Push-Nachricht von der Plattform verstehen dies jedoch nur, wenn die Anwendung aktualisiert wurde, nachdem die Benachrichtigung an den Dienst weitergeleitet wurde. Die Registerkarte **Erweitert** informiert, wie viele Nachrichten gelöscht wurden. 
    4. IOS-Geräte werden Nachrichten manchmal nicht zugestellt auf Batterie ist das Gerät oder die app viel Strom beim remote-Benachrichtigung verarbeiten verbraucht. Dies ist eine Einschränkung der iOS-Geräte.   

3.  **Angezeigte** - gibt die Anzahl der Nachrichten, die erfolgreich Anwendung auf dem Gerät aus eine System Push/Out des app-Benachrichtigung in der Mitte Benachrichtigung oder eine Benachrichtigung in app innerhalb der app angezeigt werden.  Die Registerkarte **Erweitert** erfahren Sie, wie viele systembenachrichtigungen wurden und wie viele in app-Benachrichtigungen. 
    
    *Gründe für die angezeigte zählen kleiner als übermittelte Count (warten angezeigt werden)*
    
    1. Wäre die Benachrichtigung Kampagne ein Enddatum auf ist möglich, dass die Benachrichtigung angekommen kam öffnen und app-Benutzer die Zeit war abgelaufen nie angezeigt wurde.   
    2. Wenn die Benachrichtigung in-app-Benachrichtigung die Benachrichtigung nur angezeigt wird, wenn der app-Benutzer die Anwendung öffnet. In Fällen, in denen app-Benutzer die Anwendung geöffnet noch nicht, meldet das SDK noch nicht angezeigt, bis die Anwendung geöffnet wird, dass die Benachrichtigung übermittelt wurde. 
    2. Wenn die Benachrichtigung eine Benachrichtigung in app ist und konfiguriert, eine bestimmte Aktivität/angezeigt und auch die Benachrichtigung gemeldet wird, aber noch nicht übermittelt bis öffnet der Benutzer die Anwendung auf einem bestimmten Bildschirm. 
    
4.  **Benutzerinteraktionen** - gibt die Anzahl der Nachrichten, die mit der Anwendung Benutzer interagiert hat und die Nachrichten verarbeitet oder beendet. 

    - *Der app-Benutzer können Aktion eine Benachrichtigung auf folgenden Arten:*
            
        1. Wenn die Benachrichtigung System/Out des app-Benachrichtigung oder eine in app Benachrichtigung als Benachrichtigung nur dann app-Benutzer auf die Benachrichtigung klicken.
        2. Die Benachrichtigung eine Benachrichtigung in app mit einem Web-Ansicht oder Umfragen ist klickt app Benutzer auf die interaktive Schaltfläche in der Benachrichtigung.
        3. Wenn die Benachrichtigung eine Benachrichtigung in app mit einer Webansicht ist, klickt der Benutzer app auf eine URL in der Webansicht Android nur]
    
    - *Der app-Benutzer kann eine Benachrichtigung in einer der folgenden Methoden beenden:*
    
        1. Schaltfläche Schließen die Benachrichtigung direkt. 
        2. Ein Lesegerät entfernt oder die Benachrichtigung löschen. 
        3. In app-Benachrichtigungen Umfragen mit Text-Web-Inhalte werden normalerweise der app-Benutzer in zwei Schritten angezeigt. Sie sehen eine Benachrichtigung zuerst und wenn sie darauf klicken, nachfolgenden Web/Text/Umfrage Inhalt. Der app-Benutzer kann eine Benachrichtigung entweder Schritte beenden und Details in der erweiterten Ansicht erfasst diese. 

5.  **Actioned** - gibt die Anzahl der Nachrichten, die explizit vom Benutzer app verarbeitet wurden. Die interessanteste Nummer ist wie dies wie viele app-Benutzer die Meldung interessierten, die in der Benachrichtigung verdrängt 
 
> [AZURE.NOTE] Plattformen, hat der Benutzer die Anwendung öffnen iOS und Windows die Kampagne wurde eine Kampagne "Überall" und kann als app und in app-Benachrichtigungen gleichzeitig angezeigt werden. Dadurch kann über die übermittelte angezeigte Anzahl. Der Benutzer interagiert oder Aktionen der Benachrichtigung, und auch die Anzahl der Benutzer Aktivitäten/Actioned größer als geliefert sein kann. 


![Reach2][19]

## <a name="see-also"></a>Siehe auch

- [Konzepte][Link 6]
- [TV-Programmdienst Problembehandlung][Link 24]

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
 
