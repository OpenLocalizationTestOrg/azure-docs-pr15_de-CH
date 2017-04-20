<properties 
   pageTitle="Azure Mobile Engagement-Benutzeroberfläche - Reichweite Kriterium" 
   description="Lernen Sie Zielkriterien Push-Kampagnen an eine ausgewählte Teilmenge der Benutzer mithilfe von Azure Mobile Engagement" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Wie Sie Zielkriterien Push-Kampagnen an eine ausgewählte Teilmenge der Benutzer senden

Zielgruppe nach bestimmten Kriterien mit der Schaltfläche "Neue Kriterien" ist eines der leistungsstärksten Konzepte in Azure Mobile Engagement, können Sie entsprechende senden Benachrichtigungen schieben, dass Kunden, statt alle Spam Antworten. Zielgruppe basierend auf Kriterien einzuschränken und simulieren Push zu ermitteln, wie viele Personen die Benachrichtigung erhalten.

**Siehe auch:**

- [UI - Reach - Dokumentation neue Pushkampagne][Link 27]

## <a name="audience-criteria-can-include"></a>Zielgruppe Kriterien zählen:
- **Technischen:** Sie können als Ziel basierend auf dieselben technischen Informationen Sie in den Abschnitten Analytics und Monitor sehen. **Siehe auch:** [Dokumentation von UI - Analysen] [ Link 15], [Dokumentation von UI - Monitor][Link 16]
- **Speicherort:** Verwenden "Echtzeit Standortangabe" mit Geo Standort als Kriterium können Sie eine Zielgruppe von GPS-Position. "Träge Bereich Position Berichterstattung" Aufruf auch verwendet werden, um eine Zielgruppe aus Handy ("Real Time Standortangabe" und "Träge Bereich Standortangabe" müssen aus dem SDK aktiviert werden). **Siehe auch:** [SDK-Dokumentation - iOS - Integration] [ Link 5], [SDK-Dokumentation - Android - Integration][Link 5]
- **Feedback erreichen:** Sie können basierend auf deren Feedback von vorherigen Reichweite Benachrichtigung Reichweite Feedback von Ankündigungen, Umfragen und Daten legt Zielgruppe abzielen. Dadurch können Sie besser ausrichten Zielgruppe nach zwei oder drei Kampagnen erreichen, als man beim ersten. Sie können auch Benutzer herausfiltern, die bereits eine Benachrichtigung mit ähnlichem Inhalt durch Festlegen einer Kampagne nicht an Benutzer gesendet werden, die eine bestimmte Kampagne vorherige bereits verwendet werden. Sie können auch Benutzer ausschließen enthalten, die eine bestimmte Kampagne erhalten neue Push noch aktiv ist. **Siehe auch:** [Dokumentation von UI - erreichen - Push Content][Link 29]
- **Tracking installieren:** Verfolgen Sie Informationen basierend auf dem Benutzer Ihre Anwendung installiert. **Siehe auch:** [Dokumentation zu UI - Einstellung][Link 20]
- **Profil:** Auf Standard Ziel Benutzerinformationen und Sie basiert auf dem benutzerdefinierten app Info, die Sie erstellt haben. Dies schließt Benutzer angemeldet sind und Benutzer, die Fragen beantwortet haben, sie in der Anwendung gebeten haben anstatt nur sie auf frühere Kampagnen Reaktion festlegen. Ihre App-Informationen definiert für Ihre app angezeigt in der Liste.
- Segmente: Sie können auch Ziel basierend auf Segmenten erstellt haben aufgrund bestimmter Benutzer mit mehreren Kriterien. Alle Segmente für Ihre Anwendung definiert auftauchen in der Liste. **Siehe auch:** [Dokumentation von UI - Segmente][Link 18]
- **App Info:** Benutzerdefinierte App Info Tags können aus Benutzerverhalten verfolgen "Settings" erstellt. **Siehe auch:** [Dokumentation zu UI - Einstellung][Link 20]

## <a name="example"></a>Beispiel: 
Möchten Sie eine Ankündigung, die Untergruppe der Benutzer ablegen, die Kaufs Aktion ausgeführt haben.

1. Zur Anwendung Einstellungen, wählen "App Info" und wählen Sie "Neue app Info"
2. Registrieren einer neuen booleschen app Infos namens "InAppPurchase"
3. Die Anwendung dieser app Info auf "true", wenn der Benutzer erfolgreich eine Kaufs festgelegt (SendAppInfo ("InAppPurchase",...) mit Funktion)
4. Möchten Sie die Anwendung dazu, möglich es aus der Back-End mit API-Gerät)
5. Nur müssen Sie ein Kriterium einschränken Zuschauer "InAppPurchase" festlegen dass Benutzer die Ankündigung erstellen "true")
 
> Hinweis: Auf Kriterien als app infotags erfordert Azure Mobile Engagement zu Informationen von Geräten vor der Push und so zu einer Verzögerung führen kann. Optionen (wie die Aktualisierung Plaketten) auch verzögern können komplexe Push Konfiguration legt. Verwenden eine Kampagne "eine Aufnahme" Push-API ist die absolut schnellste Push in Azure Mobile Engagement. Mit nur app infotags als Push Kriterien für eine Kampagne Reichweite (entweder-API erreicht oder die Benutzeroberfläche) ist die nächste schnellste Methode app infotags auf dem Server gespeichert werden. Mit anderen Zielkriterien für eine pushkampagne wird flexible aber langsamste Methode seit Azure Mobile Engagement Geräte Abfragen, um die Kampagne zu senden.
 
![Criterion1 erreichen][29] 

## <a name="criterion-options-apply-to"></a>Kriterium Optionen gelten:
- **Technische**     
- Firmware-Name: Name der Firmware
- Firmware-Version: Firmware-Version
- Modell: Modell
- Hersteller: Gerätehersteller
- Anwendung: Version der Anwendung
- Netzbetreiber-Namen: Carrier Name nicht definiert
- Carrier-Land: Carrier Land nicht definiert
- Netzwerktyp: Netzwerktyp
- Gebietsschema: Gebietsschema
- Größe: Größe
- **Speicherort**      
- Letzte bekannte Bereich: Land, Region, Ort
- Echtzeit-geofencing: Liste der POIs (Name, Aktionen), kreisförmige POI (Name, Latitude, Länge, Radius in Metern)
- **Feedback zu erreichen**     
- Ankündigung Feedback: Ankündigung Feedback
- Feedback Umfrage: Feedback-Umfrage
- Fragen Antwort Feedback: Fragen Antwort Feedback, Fragen, Auswahl
- Daten Push Feedback: Datenpush Feedback
- **Installieren Sie die Überwachungsdatenbank**     
- Speicher: Speicher nicht definiert
- Quelle: Quelle nicht definiert
- **Benutzerprofil**     
- Geschlecht: männlich oder weiblich, nicht definiert
- Geburtsdatum: Operator, Datum, nicht definiert
- Anmelden: WAHR oder falsch, nicht definiert
- **App Info**      
- Zeichenfolge: Zeichenfolge undefined
- Datum: Datum undefiniert, operator
- Ganzzahl: Operator, und nicht definiert
- Boolescher Wert: true oder false nicht definiert
- **Segment**    
- Name der Segmente (aus) Ausschluss (Zielbenutzer, die nicht Teil dieses Segment).

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
 
