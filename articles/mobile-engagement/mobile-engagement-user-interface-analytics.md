<properties
   pageTitle="Azure Mobile Engagement-Benutzeroberfläche - Analysen"
   description="Informationen Sie zum Analysieren von Verlaufsdaten über Ihre Anwendung Azure Mobile Engagement"
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

# <a name="how-to-analyze-historical-data-about-your-application"></a>Analysieren von Verlaufsdaten zur Anwendung

Dieser Artikel beschreibt die **ANALYTICS** Registerkarte **Mobile Engagement** -Portal. Überwachen und Verwalten der mobilen apps verwenden Sie **Mobile Engagement** -Portal. Beachten Sie, dass zum Starten über das Portal Sie zunächst ein Konto **Azure Mobile Engagement** zu erstellen müssen.


Analytics Abschnitt der Benutzeroberfläche enthält zusammengefasste Informationen über die Anwendung basierend auf Verlaufsdaten, die täglich aktualisiert wird. Die Informationen werden auf anderen Dashboards Zeile/Bar/Kreisdiagramme, Rastern und Karten zusammen angezeigt. Die Daten können auch als CSV-Dateien heruntergeladen werden. Die meisten dieser Daten steht in Echtzeit Monitor-Abschnitt der Benutzeroberfläche und auch aus der Analytics-API zugegriffen werden kann.

>[AZURE.NOTE] In vielen Abschnitten der **Mobile Engagement** -Portal Benutzeroberfläche enthalten die Schaltfläche **Hilfe anzeigen** . Weitere Kontextinformationen über einen Abschnitt zu drücken.

## <a name="standard-and-custom-analytics"></a>Standardmäßige und benutzerdefinierte Analysen

Azure Mobile Engagement bietet grundlegende, standard analytischen Informationen über Ihre Anwendung grafisch dargestellt werden können, wie das SDK app integrieren. Azure Mobile Engagement ermöglicht auch zusätzliche benutzerdefinierte Analytics Informationen über Ihre Endbenutzer Verhalten zu sammeln. Sie erreichen dies durch Erstellen eines Plans Tag benutzerdefinierte "Tags (app Info)" **Einstellungen** erstellt, sodass Azure Mobile Engagement für Sie diese zusätzlichen Daten sammeln können.



## <a name="analytics"></a>Analytics
- Dashboard: Zeigt Informationen zu neuen und aktiven Benutzer und ihre Trends.
- Benutzer: Benutzer werden durch ihre Geräte-ID identifiziert: Diese Bezeichner ist eindeutig für jedes Gerät (ein neuer Benutzer ist tatsächlich ein neues Gerät). Ein Benutzer wird als neue in einem bestimmten Zeitintervall, wenn er seiner ersten Sitzung während dieses Zeitraums ausgeführt wurde. Ein Benutzer ist als beibehalten, wenn er mindestens eine Sitzung in den letzten 7 Tagen ausgeführt hat. Aktive Benutzer sind Benutzer, die mindestens eine in einem bestimmten Zeitraum Sitzung. Sie können, monatlich, wöchentlich, täglich oder stündlich Zeiträume sortieren. Alle Diagramme ermöglichen Ihnen Filtern nach verschiedenen Features wie die Version Ihrer Anwendung und dann auf Sortieren nach einiger Zeit jedoch ähnlich. Standard Informationen durch die Integration des SDK umfasst Folgendes: aktive Benutzer, neuer Benutzer, Anzahl von Länge Sitzung, technische Informationen über Land, lokal, Standort, Sprache Carrier, Geräte, Firmware, Netzwerk (WLAN), app und SDK-Versionen durch Kunden verwendet. Diese Informationen können in Echtzeit vom Abschnitt Monitor angezeigt.

> Hinweis: Der Zeitraum basiert auf das Datum aus dem geräteeinstellungen, damit ein Benutzer, dessen Telefonnummer falsch festgelegte Datum hat, falschen Zeitraum anzeigen kann.

- Aufbewahrung: Ein Benutzer ist als in einem bestimmten Zeitintervall beibehalten, wenn er seinen ersten Sitzung während dieses Zeitraums durchgeführt hat. Ändern der beibehaltenen Benutzer (und neue Benutzer) zählen Zeitintervalle in Stunden, Tage, Wochen oder Monate. Benutzer Aufbewahrung Analytics basiert auf Kohorten. Eine der alle neuen Benutzer für einen bestimmten Zeitraum erkannt wird (d. h. die Gruppe Benutzer ihrer erste Sitzung während dieses Zeitraums). Kohorten von 1 Tag, 2 Tage, 4 Tage, 7 Tage oder 1 Monat verwendet. Wenn ein, alle 1-Tag 2-4 Tage 7 Tage oder 1 Monat, Azure Mobile Engagement berechnet der alle Benutzer der Gruppe und sind weiterhin aktiv (d. h. die Gruppe der Benutzer mindestens eine Sitzung während des Zeitraums ausgeführt). Diese Gruppe von Benutzern ist eine Kohorte aufgerufen. (Azure Mobile Engagement zeigen Sie wie viele Benutzer immer noch Ihre app verwenden, aber nur der Plattform Speicher kann Ihnen sagen, wie viele Benutzer Ihre app - beispielsweise GooglePlay iTunes Windows Store deinstalliert usw..).
- Sessions: Eine Verwendung der Anwendung durch einen Benutzer. Sitzung zur Reihenfolge der Aktivitäten von Benutzern erstellt (eine Aktivität ist üblicherweise auf die Verwendung von nur einer Seite der Anwendung, aber dies kann je nach Art des SDK in die Anwendung integriert). Ein Benutzer kann jeweils nur eine Aktivität ausführen: eine Sitzung beginnt, sobald der Benutzer seine erste Aktivität startet und stoppt, wenn er seine letzte Aktivität abgeschlossen ist. Bleibt ein Benutzer mehr als ein paar Sekunden ohne Aktivitäten, ist seine Abfolge von Aktivitäten in zwei unterschiedlichen Teilen.
- Aktivitäten: Die Namen der einzelnen Bildschirme in der Anwendung und der Länge der Benutzer auf dem Bildschirm ausgeben. Aktivitäten sind eine benutzerdefinierte analytischen Option, die für Ihre Anwendung eingerichteten Tags "app Info" entspricht:
- Pfad: Zeigt, wie die Benutzer über die Aktivitäten Ihrer Anwendung (Bildschirme) navigieren. Sie können den Schieberegler Details einstellen. Blaue Knoten darstellen Aktivitäten Ihrer Anwendung. Ihre Größe ist proportional zu der Zeit Benutzer darin ausgegeben. Weiße Knoten darstellen Sitzung starten und beenden. Rote Knoten darstellen abstürzen. Links darstellen Übergänge zwischen der Anwendung (oder zwischen Aktivitäten und stürzt ab). Klicken Sie auf einen Knoten oder eine Verknüpfung eine QuickInfo mit Informationen zu Ihren Daten: die Zeit in einem bestimmten Bildschirm die Anzahl der Übergänge und den Prozentsatz der Übergänge aus der Quelle zum Zielaktivität. (Eine---60 % B bedeutet, der Benutzer auf eine Aktivität zu Aktivität B führt---> 60 % der Zeit.) Sie können das Diagramm neu anordnen, präzisieren möchten; jedes Mal, wenn Sie eine Änderung vornehmen, wird seine Position gespeichert. Sie können ein- oder Ausblenden von Abstürzen, um das Diagramm zu erleichtern.
- Ereignisse: Bestimmte Aktionen durch einen Benutzer in der Anwendung. Die Verteilung der Ereignisse wird als die Anzahl der Ereignisse pro Benutzer pro Sitzung angezeigt. Ein Ereignis stellt eine sofortige Aktion, z. B. Klicken auf eine Schaltfläche oder den Empfang einer Benachrichtigung. (Die Bedeutung von Ereignissen hängt wie das SDK in die Anwendung integriert wurde.) Ein Ereignis kann während einer Sitzung oder eines Auftrags auftreten oder kann nicht allein stehen.
- Aufträge: Ähnliche Ereignisse außer auf die Länge der Aktion konzentrieren. Beispielsweise könnten Sie Aufträge sagen, technische Informationen, Inhalte zu laden oder einen Aufruf zum Webdienst Dauer. Es kann auch anzeigen, wie lange einen Benutzer ein Formular ausfüllen, registrieren oder einkaufen. Ein Projekt stellt die Dauer eines Vorgangs, z. B. die Dauer einer Aufgabe und Banner wird auf dem Bildschirm angezeigt. (Die Bedeutung der Aufträge hängt wie das SDK in die Anwendung integriert wurde.) Aufträge werden mit Hintergrundaufgaben, die außerhalb einer Sitzung (d. h. ohne die Benutzeraktivität) ausgeführt werden.
- Technische: Technische Informationen über die Geräte der Benutzer Ihrer App überwachen, wie Gebietsschema Netzbetreiber, Netzwerk, Gerät, Firmware und Größe der Geräte sowie die Version Ihrer Anwendung und die SDK-Version in Ihrer Anwendung verwendet.
- Fehler: Informationen über technische Fehler in der Anwendung, die nicht zum Absturz die Anwendung führen. Fehler stellt instant Problem, z. B. ein Netzwerkfehler oder eine fehlerhafte Bearbeitung. (Die Bedeutung von Ereignissen hängt wie das SDK in die Anwendung integriert wurde.) Fehler kann während einer Sitzung oder einem Auftrag oder kann allein.
- Abstürze: Informationen, die Ihre Anwendung abstürzen. Ein Absturz ist eine unerwartete Bedingung, wo die Anwendung die erwarteten Funktionen beendet und muss beendet werden. Ein Absturz ist normalerweise aufgrund eines Fehlers in der Anwendung.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Übersicht über die Aufbewahrung zugreifen
![Analytics3][12]

Übersicht über die Aufbewahrung wird in der Mitte mehrere Karten jede Übersicht für einen bestimmten Zeitraum aufgeteilt. Im Beispiel wird die Aufbewahrungszeit von 2 Tagen angezeigt. Die Karten anzeigen 4 und 7 Tagen Aufbewahrungszeiten

## <a name="understanding-the-retention-overview-cards"></a>Grundlegendes zur Aufbewahrung Überblick über Karten
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Jede Karte besteht aus 3 Teilen:
1. 1: Kohorte und Periode berücksichtigt
2. 2 bis 4: Aufbewahrung für die aktuelle Periode
3. 5: eine Sparkline Verlauf

### <a name="here-is-detailed-information-about-each-element"></a>Hier finden Informationen über die einzelnen Elemente:
1.    Kohorte und Zeitraum: diese Überschrift gibt den Typ der Gruppe. "2 Tage" bedeutet hier, betrachten wir das Verhalten der Benutzer 2 Tage über einen Zeitraum von 2 Tagen und ob kam Benutzer sie in den folgenden Blöcken von 2 Tagen herstellen. Im obigen Beispiel berücksichtigt die Aktivitäten von Benutzern zwischen dem 21. und 22. November.
2.    Dadurch die Aufbewahrung Rate 21. und 22. November für die Benutzer im 19. und 20. November. Hier musste 1 aktiven Benutzer zwischen dem 21. und 22. über die neuen zwischen dem 19. und 20. Benutzer 3.
3.    Diese visuelle Indikator gibt dieselbe Informationen wie oben grafisch dargestellt. (Die dritte des Kreises ist von 33 % Zahl.) Die Farbe Gibt zusätzliche Informationen: Grün zeigt diese Zahl wächst aus der vorherigen Berechnung an. Gelb bedeutet stabil und Rot verringern.
4.    Dies zeigt die Werte für die Berechnung verwendet.
5.    Dies ist eine Sparkline den Verlauf von Werten Aufbewahrung. Dadurch sehen Sie die Werte in der Vergangenheit zu einer umfassenden Ansicht wie entwickelt.


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
