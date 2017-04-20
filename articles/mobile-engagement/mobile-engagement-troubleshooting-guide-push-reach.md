<properties 
   pageTitle="Azure Mobile Engagement Fehlerbehebung - Push-Reichweite" 
   description="Problembehandlung bei der Interaktion und Benachrichtigung in Azure Mobile Engagement" 
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

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Problembehandlung bei Push und Probleme erreichen

Im folgenden werden mögliche Probleme können mit wie Azure Mobile Engagement Informationen an die Benutzer gesendet.
 
## <a name="push-failures"></a>Push-Fehler

### <a name="issue"></a>Problem
- Push funktionieren (in app aus app) nicht.

### <a name="causes"></a>Ursachen
- Oft ein Push-Fehler ist ein Hinweis, dass Azure Mobile Engagement, Reichweite und eine weitere fortschrittliche Funktion Azure Mobile Engagement nicht ordnungsgemäß integriert oder Aktualisierung im SDK erforderlich ist, um ein bekanntes Problem mit einer neuen Plattform Betriebssystem oder Gerät beheben.
- Testen Sie nur ein Push In App und nur eine App von Push um festzustellen, ob etwas In oder von App geht.
- Testen der Benutzeroberfläche und die API als Schritt zur Problembehandlung, was zusätzliche Fehlerinformationen verfügbar ist beides.
- App funktioniert drückt nur Azure Mobile Engagement und Reichweite im SDK integriert sind.
- Push funktioniert nicht, wenn Zertifikate ungültig sind oder werden Produkte im Vergleich zu DEV ordnungsgemäß (nur iOS). (**Hinweis:** "von app" Push-Benachrichtigung können nicht übermittelt werden zu iOS, wenn die Entwicklung (DEV) und (PROD) Versionen der Anwendung auf dem gleichen Gerät installiert werden, da das Sicherheitstoken mit dem Zertifikat verknüpft ist möglicherweise von Apple ungültig. Zum Beheben dieses Problems deinstallieren Sie die Entwicklung und Produktion Versionen der Anwendung zu, und installieren Sie nur die Version auf dem Gerät.)
- Aus App Push werden Zahlen in verschiedenen Plattformen unterschiedlich behandelt (iOS zeigt weniger Informationen als Android native Push auf einem Gerät deaktiviert, die API bieten mehr Informationen als die Benutzeroberfläche auf Push-Statistiken).
- App können Kunden OS (IOS- und Android) drückt blockiert.
- App Push erscheint in Azure Mobile Engagement Benutzeroberfläche deaktivieren, wenn sie sind nicht richtig integriert, aber möglicherweise automatisch von der API.
- In App funktioniert drückt nur Azure Mobile Engagement und Reichweite im SDK integriert sind.
- GCM und ADM Push funktioniert nur Azure Mobile Engagement und bestimmte Server SDK (nur Android) integriert sind.
- In oder von App sollten Push separat getestet werden, um festzustellen, ob ein Problem mit Push oder erreichen.
- In App benötigen schiebt die app empfangen werden können.
- In App sind Push häufig Setup eine Anmeldung oder Abmeldung app Info Tag gefiltert werden.
- Wenn Sie mithilfe einer benutzerdefinierten Kategorie in Reichweite in app-Benachrichtigungen anzeigen, müssen Sie den richtigen Lebenszyklus der Benachrichtigung, oder die Benachrichtigung kann nicht gelöscht Wenn Benutzer schließen.
- Starten einer Kampagne ohne Enddatum und ein Gerät erhält in app Benachrichtigung jedoch nicht anzeigen, sie jedoch die Benutzer weiterhin die Benachrichtigung der nächsten Anmeldung in der Anwendung auch wenn Sie manuell die Kampagne, ist.
- Probleme mit der Push-API bestätigen Sie, dass wirklich möchten-API erreicht (da die API erreichen häufig verwendet) der Push-API anstelle und die Parameter "Nutzlast" und "Antragsteller" nicht verwirrt werden.
- Beide Geräte über WIFI und 3G Netzwerkschnittstelle als mögliche Probleme testen Sie pushkampagne.

## <a name="push-testing"></a>Push testen

### <a name="issue"></a>Problem
- Push können ein bestimmtes Gerät basierend auf eine Geräte-ID gesendet werden

### <a name="causes"></a>Ursachen

- Geräte werden für jede Plattform, jedoch verursacht ein in Ihrer Anwendung auf einem Testgerät und der Geräte-ID im Portal suchen sollten Ihre Geräte-ID für alle Plattformen zu arbeiten.
- Geräte Funktionsweise mit IDFA und IDFV (nur iOS).


## <a name="push-customization"></a>Push-Anpassung

### <a name="issue"></a>Problem
- Erweiterte Push Content Element funktioniert nicht (Abzeichen, ring, Vibrieren, Bild usw.).
- Links von Push funktionieren (von app App auf einer Website an einem Speicherort in app) nicht.
- Push-Statistiken zeigen, dass ein Push an so viele Menschen wie erwartet (zu viele oder zu wenig) nicht gesendet wurde.
- Dupliziert und zweimal erhalten.
- Testgerät kann nicht für Azure Mobile Engagement legt (mit eigener Produkte oder DEV-app) registriert werden.

### <a name="causes"></a>Ursachen

- Um zu einer bestimmten Stelle in der Anwendung verknüpfen erfordert "Categories" (nur Android).
- Tiefe verknüpfen Schemas Benutzer nach dem Klicken auf eine Push-Benachrichtigung an einen anderen Speicherort umleiten müssen erstellt und direkt von der Anwendung und dem Betriebssystem des Geräts nicht durch Mobile Engagement verwaltet. (**Hinweis:** App Benachrichtigung können nicht verknüpft werden direkt an IOS-app wie Android.)
- Bild-Server müssen mit HTTP "GET" und "" für Überblick zu (nur Android) drückt.
- Im Code können Sie Azure Mobile Engagement-Agent deaktivieren, wenn die Tastatur geöffnet wird und Azure Mobile Engagement-Agent erneut zu aktivieren, sobald die Tastatur geschlossen wird, damit die Tastatur die Darstellung der Benachrichtigung (nur iOS) beeinträchtigt Code haben.
- Einige Arbeitsaufgaben nicht in Test Simulationen jedoch real Kampagnen (Abzeichen, ring, Vibrieren, Bild usw.).
- Bei Verwendung der Schaltfläche "Push testen" keine Serverdaten protokolliert. Daten werden nur für echte pushkampagnen protokolliert.
- Um das Problem zu isolieren, beheben: testen, simulieren und real seit sie etwas anders.
- Die Zeitdauer, die "app" und "jederzeit" Kampagnen geplant werden, kann Lieferung Zahlen auswirken, da eine Kampagne nur Benutzer "app" während die Kampagne ausgeführt wird (und Benutzer ihre geräteeinstellungen Benachrichtigungen "aus app") geliefert wird.
- Die Unterschiede zwischen Android und iOS aus app Benachrichtigung wie behandeln erschwert Push Statistiken zwischen Android und iOS-Version der Anwendung direkt zu vergleichen. Android bietet weitere OS Ebene Benachrichtigungsinformationen als iOS ist. Android meldet eine systemeigene Benachrichtigung empfangen, geklickt oder in der Mitte Benachrichtigung gelöscht, als iOS meldet diese Informationen nicht, wenn die Benachrichtigung geklickt wird. 
- Der Hauptgrund, die "geschoben" Zahlen anders sind als andere als "geliefert" für Kampagnen erreichen "in Anwendung" und "app" Benachrichtigung unterschiedlich gezählt. "In Anwendung" Benachrichtigung von Mobile Engagement behandelt werden, aber "von app" Benachrichtigung von Notification Center im Betriebssystem des Geräts verarbeitet werden.

## <a name="push-targeting"></a>Drücken Sie auf

### <a name="issue"></a>Problem
- Integrierte targeting funktioniert nicht wie erwartet.
- App Info Tag auf erwartungsgemäß nicht.
- Auf Standort erwartungsgemäß nicht.
- Sprachoptionen erwartungsgemäß nicht.

### <a name="causes"></a>Ursachen

- Stellen Sie sicher, dass app infotags Azure Mobile Engagement UI oder API hochgeladen.
- Drosselung Push Geschwindigkeit oder Push-Kontingent auf Anwendungsebene oder das Publikum auf Kampagnenebene begrenzen kann eine Person bestimmten Push erhalten, auch wenn sie die anderen Zielkriterien. 
- Einstellung "Sprache" unterscheidet sich auf Land Grundlage oder Gebietsschema auch auf basierend auf Standort unterscheidet auf einem Standort oder GPS-Position.
- Die "Standardsprache" Kunden Nachricht, der das Gerät eine alternative Sprache fest, die Sie angeben.


## <a name="push-scheduling"></a>Planen von Push

### <a name="issue"></a>Problem
- Planen von Push funktioniert nicht wie erwartet (zu früh oder verzögerte gesendet).

### <a name="causes"></a>Ursachen

- Zeitzonen können Probleme bei der Planung, vor allem, wenn die Endbenutzer Zeitzone.
- Erweiterte Push-Funktionen können Push verzögern.
- Zielgruppenadressierung basierend auf Phone (anstelle App Info Tags) verzögern können legt da Azure Mobile Engagement möglicherweise Daten vor dem Senden eines Push Echtzeit Telefon anfordern.
- Kampagnen ohne Enddatum den Push lokal auf dem Gerät speichern und zeigen das nächste Mal, das die Anwendung geöffnet wird, selbst wenn die Kampagne manuell beendet wird.
- Mehrere Kampagnen gleichzeitig starten dauert länger Benutzerbasis (versuchen, eine Kampagne jeweils maximal vier auch an Ihre aktive Benutzer erst damit alte Benutzer nicht gescannt werden) zu scannen.
- Verwenden Sie die Option "Ignorieren Publikum Push Benutzer API erhalten" im Abschnitt "Kampagne" eine Kampagne erreichen die Kampagne wird nicht automatisch senden, müssen Sie manuell API erreichen senden.
- Wenn Sie mithilfe einer benutzerdefinierten Kategorie in Reichweite in app-Benachrichtigungen anzeigen, müssen Sie den richtigen Lebenszyklus einer Benachrichtigung, oder die Benachrichtigung kann nicht gelöscht Wenn Benutzer schließen.

 
