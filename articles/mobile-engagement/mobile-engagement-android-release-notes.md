<properties
    pageTitle="Azure Mobile Engagement Android SDK-Integration"
    description="Neueste Updates und Verfahren für Android SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Versionshinweise

## <a name="423-08102016"></a>4.2.3 (10/08/2016)

- Keine weitere WIFI-Sperre.
- Beheben Sie einen Deadlock beim Aufrufen von GetDeviceId vor dem Init (Bug 4.2.0 eingeführt).

## <a name="422-05172016"></a>4.2.2 (17/05/2016)

- Stabilität.

## <a name="421-05102016"></a>4.2.1 (10/05/2016)

- Sicherheit: Deaktivieren Sie Web View lokalen Zugriff.
- Sicherheit: Entfernen `EngagementPreferenceActivity` Klasse, die veraltet und unsicheren erweitert `PreferenceActivity` Klasse.
- Sicherheit: Reichweite Aktivitäten jetzt Verwendung dokumentiert `exported="false"`, dieses Flag kann auch in früheren Versionen von SDK verwendet werden.

## <a name="420-03112016"></a>4.2.0 (11/03/2016)

- Das SDK ist jetzt unter MIT lizenziert.
- Lassen Sie angeben eines benutzerdefinierten Gerätebezeichners Initialisierungszeit SDK zu

## <a name="415-02012016"></a>4.1.5 (01/02/2016)

- Stabilität.

## <a name="414-01262016"></a>4.1.4 (26/01/2016)

- Stabilität.

## <a name="413-1292015"></a>4.1.3 (9/12/2015)

- Stabilität.

## <a name="412-11252015"></a>4.1.2 (25/11/2015)

- Stabilität.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)

- Stabilität.

## <a name="410-08252015"></a>4.1.0 (25/08/2015)

- Behandeln Sie neue Berechtigungsmodell für Android M.
- Können jetzt Speicherort Funktionen zur Laufzeit anstelle von `AndroidManifest.xml`.
- Korrigieren eines Fehlers Berechtigung: Verwenden Sie `ACCESS_FINE_LOCATION`, dann `ACCESS_COARSE_LOCATION` wird nicht mehr benötigt.
- Stabilität.

## <a name="400-07062015"></a>4.0.0 (06/07/2015)

-   Internes Protokoll Änderungen Analysen und Push zuverlässiger.
-   Native Push (GCM/ADM) wird jetzt auch für app-Benachrichtigung verwendet systemeigene Push Anmeldeinformationen für jede Art von pushkampagne konfigurieren müssen.
-   Überblick Notification beheben: sie wurden angezeigten nur 10 s gedrückt wird.
-   Korrigieren eines Fehlers in der Webansicht: einen Link auf die Standard-Aktions-URL auch ausgeführt wurde.
-   Beheben Sie einen seltenen Absturz Bezug auf lokalen Speicher-Management.
-   Beheben Sie Verwaltung von dynamischen Konfiguration.
-   EULA zu aktualisieren.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)

-   Erste Version von Azure Mobile Engagement
-   Konfiguration einer Verbindungszeichenfolge AppId Konfiguration Fassung.
-   API zum Senden und Empfangen von beliebigen XMPP-Nachrichten von beliebigen XMPP-Elemente entfernt.
-   API zum Senden und Empfangen von Nachrichten zwischen Geräten entfernt.
-   Verbesserte Sicherheit.
-   Google Play und SmartAd Tracking entfernt.
