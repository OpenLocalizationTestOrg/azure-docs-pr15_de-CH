<properties 
   pageTitle="Azure Mobile Engagement-Benutzeroberfläche - Segmente" 
   description="Informationen Sie zum Erstellen und Verwalten von Segmenten von Benutzern mit Azure Mobile Engagement Verwendungsmuster zu identifizieren" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Zum Erstellen und Verwalten von Segmenten Benutzer Verwendungsmuster zu identifizieren

Dieser Artikel beschreibt die **Segmente** Registerkarte **Mobile Engagement** -Portal. Überwachen und Verwalten der mobilen apps verwenden Sie **Mobile Engagement** -Portal.

Der Abschnitt der Benutzeroberfläche können Sie arbeiten Segmentierung der Benutzer anders und Analysen, die Sie von der Anwendung und können auch über die Segmente-API zugreifen. Segmente sind 24 Stunden, nachdem sie erstellt und sie innerhalb von 24 Stunden anhand der neuesten Analytics Informationen berechnet zuerst berechnet. Sobald ein Segment berechnet, es "Tag Tage" zeigt ein Diagramm an jeden Tag.


>[AZURE.NOTE] In vielen Abschnitten der **Mobile Engagement** -Portal Benutzeroberfläche enthalten die Schaltfläche **Hilfe anzeigen** . Weitere Kontextinformationen über einen Abschnitt zu drücken.

## <a name="create-segments"></a>Erstellen von Segmenten
Sie können ein Segment anhand bis 10 auf einen bestimmten Zeitraum von 60 Tage in der Vergangenheit vom Abschnitt Analytics erstellen. Beispielsweise können Sie ein Segment basierend auf Personen, die bestimmte Seiten oder innerhalb Ihrer Anwendung innerhalb der letzten 10 Tage gesucht erstellen. Diese Informationen sind im Abschnitt Analytics. So können sie ein Segment erstellen und dann eine Push-Benachrichtigung auf diesen Teil der Benutzer zu der Anwendung wieder einrichten. 
 
> Hinweis: Sobald ein Segment berechnet, es nicht bearbeitet. Es kann nur geklont (kopiert) oder vernichtet (gelöscht). Ein Segment kann innerhalb derselben Anwendung (mit der gleichen AppID) kopiert werden, und kann auch in anderen Programmen (mit verschiedenen AppID) kopiert werden. 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Beispiele für Segmente
 ![segments2][36]

Segmente können Endbenutzer die Anwendung segmentieren.
Segmentierung der Benutzer ist ein wichtiger Marketingstrategie. Azure Mobile Engagement können Sie Daten und benutzerdefinierte Segmente. Dieses leistungsstarke Tool können Sie Informationen zu Ihren Kunden Erfahrung in der Anwendung. Sie können problemlos Segmente analysieren und Segmente als Push-Ziele.
Allgemeiner Anwendungsfall ist einem Push Endbenutzer die Anwendung im Speicher bewerten zu benachrichtigen möchten. Anstatt eine Benachrichtigung an alle Anwender können Sie ein Segment erstellen, die nur Benutzer, die Ihre Anwendung jeden Tag im letzten Monat verwendet und haben einen hervorragenden Benutzer angeben würde. Wenn Sie weniger, zielgerichtete Pushbenachrichtigungen senden, erhalten Sie einen besseren ROI.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Segmente können Sie basierend auf der Azure Mobile Engagement Hauptelemente:
- Ereignis: Erstellen Sie ein Segment ein bestimmte Ziele Ereignis der Anwendung mehr als zweimal pro Woche geschehen. 
- Sitzung: Erstellen Sie ein Segment von Benutzern, die die Anwendung letzte Woche mehr als 5 Mal verwendet haben.
- Aktivität: Erstellen Sie ein Segment von Benutzern, die eine Seite oder Inhalt mehr oder weniger als 10 Mal im letzten Monat verwendet haben.
- Auftrag: Erstellen Sie ein Segment von Benutzern, die ein Projekt mehr als zweimal täglich abgeschlossen haben.
- Absturz: Erstellen Sie ein Segment aller Benutzer, die einen Absturz letzte Woche mehr als 10 Mal aufweisen. (Sie können mit Entschuldigung oder sogar einen Gutschein drücken.)
- Fehler: Erstellen Sie ein Segment aller Benutzer, die einen Fehler mehr als 100 Mal 3 Tagen aufweisen.
- App Info: Erstellen Sie ein Segment mit der benutzerdefinierten App Info abzielt, die während der letzten 25 Tage passiert.
 
 ![segments4][38]

Um Segment zu verwenden, müssen Sie eine benutzerdefinierte Integration des SDK in der Anwendung mit einem Tag "App Info" Tags haben.
Dann an der Schnittstelle, wählen Sie die Anwendung, die und klicken Sie im Abschnitt "Segmente".

1. Wählen Sie "Segmente".
2. Klicken Sie auf "Neuer Bereich", um ein neues Segment zu erstellen.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Echte Beispiel: Erstellen einer einfachen Segment "Session" Angaben
Erstellen Sie ein Segment alle Anwender, die Ihre Anwendung in der letzten Woche mindestens 50 Mal verwendet haben. Dort finden Sie nur die Endbenutzer, die in Ihrer Anwendung pro Sitzung mindestens 30 Sekunden haben. Alle Endbenutzer zeigt eine positive Erfahrung in Ihrer Anwendung haben. Segment erstellt kann dann verwendet werden, müssen eine Benachrichtigung für diese Endbenutzer auffordern, bewerten Sie Ihre Anwendung im Speicher.
 
 ![segments5][39]

1. Benennen Sie Ihr Segment um schnell in der Liste "Segment" zu finden.
2. Klicken Sie auf die Schaltfläche "Erstellen".
 
 ![segments6][40]

Wählen Sie die Sitzung.
 
 ![segments7][41]

1. Wählen Sie "Letzte Woche" aus.
2. Klicken Sie auf Weiter.
 
 ![segments8][42]

1. Wählen Sie den Operator in der Liste ist: =; ≥, ≤.
2. Geben Sie die gewünschte Anzahl ein.
3. Wählen Sie das gewünschte vorkommen. 
4. Klicken Sie auf Weiter.
In diesem Beispiel wird festgelegt, sodass in der letzten Woche entsprechen Benutzer, die mindestens 50 Sessions vorgenommen haben.
 
 ![segments9][43]

Für die Segmentierung Sitzung können Sie die Dauer pro Sitzung als Kriterium.

1. Wählen Sie den Operator aus der Liste.
2. Geben Sie die Dauer pro Sitzung.
3. Klicken Sie auf Weiter.
In diesem Beispiel heißt es alle Sitzungen, die im Abschnitt Vorkommen unterteilt wurde, wählen Sie nur die Benutzer, die mehr als 30 Sekunden pro Sitzung haben.
 
 ![segments10][44]

Nennen Sie Kriterium im vollständigen Trichter abrufen, und klicken Sie auf Fertig stellen.
 
 ![segments11][45]

Wenn Sie Ihr Kriterium eingerichtet haben, wird er im Segment Trichter angezeigt.
Da ein Segment Analysedaten basiert, werden Segmente einmal pro Tag berechnet.
In diesem Beispiel 47,7 % der insgesamt Endbenutzer das Kriterium übereinstimmen. Sie sollen Benutzer eine gute Erfahrung und werden wahrscheinlich eine Bewertung drücken sie eine Benachrichtigung bitten, app im Speicher zu bewerten.


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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
