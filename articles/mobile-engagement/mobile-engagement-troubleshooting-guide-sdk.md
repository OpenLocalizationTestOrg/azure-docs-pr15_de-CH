<properties 
   pageTitle="Azure Mobile Engagement Fehlerbehebung - SDK" 
   description="Problembehandlung bei der SDK-Integration in Azure Mobile Engagement" 
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

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Problembehandlung bei der SDK-integration

Im folgenden werden mögliche Probleme mit von Azure Mobile Engagement in Ihrer Anwendung Integration auftreten können.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>SDK Probleme durch einen Fehler in einem anderen Bereich der Anwendung

### <a name="issue"></a>Problem
- Benutzeroberfläche Daten Auflistung Fehler (Analyse, Überwachung, Segmentierung oder Dashboards).
- Fehler Push (Push App aus app funktionieren nicht).
- Erweiterte KE-Fehler (Überwachung, Geolocation und Plattform bestimmte drückt funktionieren).
- API-Fehler (APIs Fail häufig im Hintergrund ohne Fehlermeldungen).
- Dienstfehlern (keine Azure Mobile Engagement kann für die Anwendung).

### <a name="causes"></a>Ursachen

- Die meisten Probleme, die in Azure Mobile Engagement SDK behoben werden durch einen Fehler in der Anwendung (wie einem UI-Auflistung fehlschlagen, Push-Fehler, erweiterte Funktion Fehler, API-Fehler, Anwendungsabstürze oder offensichtlich Dienstausfall) gefunden.  
- Eine Besonderheit von Azure Mobile Engagement in Ihrer Anwendung vor nie gearbeitet hat, müssen Sie die Integration abgeschlossen. 
- Wenn ein bestimmtes Feature von Azure Mobile Engagement beendet war, müssen Sie im Azure Mobile Engagement SDK-Version aktualisieren. Beachten Sie, dass es eine andere Version von Azure Mobile Engagement SDK für jede Plattform Azure Mobile Engagement (Android, iOS, Windows und Windows Phone).

#### <a name="sdk-integration"></a>SDK-Integration

- Azure Mobile Engagement integriert nicht richtig SDK (Analyse).
- Erreichen Sie nicht ordnungsgemäß im SDK (In App und der Anwendung drückt) integriert.
- Das Zertifikat abgelaufen oder ungültig PROD vs. DEV (nur iOS).
- GCM oder ADM nicht ordnungsgemäß im SDK integriert (Android-Dienst bestimmten drückt).
- Verfolgen nicht korrekt (Installation Speicher verfolgen) SDK integriert.
- Verzögerte oder GPS-Pfad nicht richtig integriert in SDK (Zielgruppenadressierung von Standort).


**Siehe auch:**

- [SDK-Dokumentation - Integration Hilfslinien][Link 5] 
- [Problembehandlung - Push][Link 23]

#### <a name="sdk-upgrade"></a>SDK-Aktualisierung

- SDK zur Behebung von Problemen mit älteren Versionen des SDK (häufig im Zusammenhang mit neueren Versionen des Geräts OS) aktualisieren müssen.
- Deinstallieren Sie alle frühere Versionen der Anwendung auf dem Gerät und installieren Sie der neuesten Version Ihrer Anwendung neu registrieren Ihre Geräte-ID von Azure Mobile Engagement Benutzeroberfläche zu bestätigen, dass Ihr die neueste Version der Anwendung Gerät.

**Siehe auch:**

- [SDK-Dokumentation - Versionsinformationen](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [SDK-Dokumentation - Upgrade Hilfslinien](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Andere SDK

- Fehler in "AndroidManifest.xml" Anwendungsmanifest kann Azure Mobile Engagement nicht zu (nur Android).
- Ein häufiges Problem mit SDK-Integration und API-Nutzung ist das SDK und API-Schlüssel zu verwechseln.

**Siehe auch:**

- [Konzepte - Glossar][Link 6]

## <a name="advanced-coding-issues"></a>Erweiterte Codeprobleme

### <a name="issue"></a>Problem
-  Plattform-spezifischen Code nicht unmittelbar Azure Mobile Engagement kann Probleme zu iOS, Android, Windows Phone.

### <a name="causes"></a>Ursachen

- Viele erweiterte Codierprobleme mit Azure Mobile Engagement entstehen durch falsch geschriebene bestimmte Plattformcode nicht unmittelbar Azure Mobile Engagement. Sie müssen in der Dokumentation für die Plattform entwickelten für neben Azure Mobile Engagement Dokumentation (Android, iOS, Web, Windows und Windows Phone).
- "Kategorien" nicht ordnungsgemäß konfigurieren, verhindert eine Benachrichtigung an eine andere Position innerhalb oder außerhalb der app (nur Android) verknüpfen. 
- Nicht festgelegt "UIKit.framework" "optional" in iOS Code zeigt eine "Symbol Fehler nicht gefunden" oder stürzt auf ältere iOS Geräte (nur iOS).
- Abgelaufene Zertifikate oder nicht korrekt mit der Entwickler oder FA Version des Zertifikats wird Push Probleme (nur iOS).
- Es sind einige Nachteile einer Plattform Azure Mobile Engagement steuern kann (z. B. System Center App Funktionsweise für Android und iOS drückt).
- Azure Mobile Engagement veröffentlicht eine vollständige Liste der internen Pakete von Azure Mobile Engagement für IOS- und Android Referenz verwendet. Denken Sie daran, dass einige Funktionen von Azure Mobile Engagement für die Plattform (Android, iOS, Web, Windows und Windows Phone) sind.

### <a name="see-also"></a>Siehe auch

 - [Problembehandlung - Push][Link 23] 
 - [SDK-Dokumentation - Versionsinformationen][Link 5]
 - [SDK-Dokumentation - Upgrade Hilfslinien][Link 5]

## <a name="application-crashes"></a>Programmabstürze

### <a name="issue"></a>Problem
- Die Anwendung stürzt ab, auf die Endbenutzer Gerät.

### <a name="causes"></a>Ursachen

- Informationen kann in *Analytics UI* oder *Analytics API* angezeigt werden
- Sie können die Geräte-ID des Testgeräts finden und dieselbe Aktion, die die Anwendung zum Absturz für Endbenutzer ermitteln die Ursache für den Absturz verursacht hat.
- Bekannte Probleme mit dem Azure Mobile Engagement-SDK, die Programme abstürzen werden manchmal durch ein Upgrade auf die neueste Version des SDK aufgelöst. Überprüfen Sie die Versionsinformationen zum Betriebssystem bei Abstürzen.

### <a name="see-also"></a>Siehe auch

- [SDK-Dokumentation - Versionsinformationen][Link 5]
- [SDK-Dokumentation - Upgrade Hilfslinien][Link 5]

## <a name="app-store-upload-failures"></a>App Store Upload-Fehler

### <a name="issue"></a>Problem
- Fehler im Zusammenhang mit die neueste Version der app Apple, Google oder Windows App Store hochladen.

### <a name="causes"></a>Ursachen

- App speichert Block apps manchmal bestimmte Features aktiviert (Apple Store verhindert die Verwendung von IDFV in apps im Speicher und Speicher GooglePlay verhindert die gemeinsame Nutzung von Informationen zwischen apps Anwendung). 
- Stellen Sie sicher, dass die Versionsinformationen der Plattform und die aktuelle Version des SDK überprüfen, sollten Sie eine Anwendung in den Speicher hochladen können.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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
 
