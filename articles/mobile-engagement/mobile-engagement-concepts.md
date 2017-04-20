<properties
    pageTitle="Mobile Engagement Konzepte | Microsoft Azure"
    description="Azure Mobile Engagement-Konzepte"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile Engagement-Konzepte

Mobile Engagement definiert einige allgemeine Konzepte auf allen unterstützten Plattformen. Dieser Artikel beschreibt kurz die Konzepte.

Dieser Artikel ist ein guter Anfang, wenn Sie Mobile Engagement sind. Stellen Sie außerdem sicher, lesen Sie die Dokumentation für die Plattform, die Sie verwenden, die wie es in diesem Artikel mit mehr Details und Beispiele für mögliche Nachteile beschriebenen Konzepte verfeinern.

## <a name="devices-and-users"></a>Geräte und Benutzer
Mobile Engagement identifiziert Benutzer generiert einen eindeutigen Bezeichner für jedes Gerät. Dieser Bezeichner wird die Geräte-ID bezeichnet (oder `deviceid`). Es ist so, dass alle Programme ausführen desselben Geräts teilen die gleiche Geräte-ID generiert.

Implizit bedeutet dies, dass Mobile Engagement ein Gerät gehört zu genau einem Benutzer betrachtet und folglich Benutzer und Geräte entsprechende Konzepte sind.

## <a name="sessions-and-activities"></a>Sessions und Aktivitäten
Eine Sitzung ist eine Verwendung der Anwendung von einem Benutzer ausgeführt, vom Zeitpunkt der Benutzer beginnt mit, bis der Benutzer reagiert.

Eine Aktivität ist eine Verwendung des angegebenen untergeordneten Teil der Anwendung von einem Benutzer (ist normalerweise ein Bildschirm, aber es ist nichts für die Anwendung).

Ein Benutzer kann jeweils nur eine Aktivität ausführen.

Eine Aktivität wird durch ein (maximal 64 Zeichen) identifiziert und einbetten kann optional einige zusätzlichen Daten (in der maximal 1024 Bytes).

Sitzung werden von der Reihenfolge der Aktivitäten von Benutzern automatisch berechnet. Eine Sitzung beginnt, wenn der Benutzer seine erste Aktivität startet und stoppt, wenn er seine letzte Aktivität abgeschlossen ist. Dies bedeutet, dass eine Sitzung nicht explizit gestartet oder gestoppt werden muss. Aktivitäten werden stattdessen explizit gestartet oder angehalten. Wenn keine Aktivität gemeldet wird, wird keine Sitzung gemeldet.

## <a name="events"></a>Ereignisse
Ereignisse werden verwendet, um instant Aktionen (wie gedrückt oder Beiträge von Benutzern gelesen) melden.

Die aktuelle Sitzung ausgeführte Auftrag ein Ereignis verknüpft werden oder ein eigenständiges Ereignis sein.

Ein Ereignis durch ein (maximal 64 Zeichen) identifiziert und kann optional auch einige zusätzlichen Daten (in maximal 1024 Bytes) einbetten.

## <a name="error"></a>Fehler
Fehler melden die Anwendung (z. B. falsche Benutzeraktionen oder Fehler beim Aufrufen der API) erkannt wird.

Fehler der aktuellen Sitzung aktiven Auftrag verknüpft werden oder eine eigenständige Fehler sein.

Fehler durch ein (maximal 64 Zeichen) identifiziert und kann optional auch einige zusätzlichen Daten (in maximal 1024 Bytes) einbetten.

## <a name="job"></a>Auftrag
Aufträge werden verwendet, um Aktionen Dauer melden (wie Dauer der API-Aufrufe anzeigen anzeigen und Dauer von Hintergrundaufgaben oder Dauer Aktionen).

Ein Auftrag mit einer Sitzung bezieht sich nicht, da eine Aufgabe im Hintergrund ohne Benutzereingriff ausgeführt werden kann.

Ein Projekt durch ein (maximal 64 Zeichen) identifiziert und kann optional einige zusätzlichen Daten (in der maximal 1024 Bytes) einbetten.

## <a name="crash"></a>Absturz
Abstürze werden automatisch ausgestellt, Mobile Engagement SDK Anwendungsfehler, nicht von der Anwendung erkannte machen, Absturz melden.

## <a name="application-information"></a>Anwendungsinformationen
Informationen (oder app Info) wird zum tag-Benutzer, d. h. Daten, die Benutzer einer Anwendung zuordnen (Dies ist ähnlich wie Webcookies, app Info auf Azure Mobile Engagement-Plattform auf dem Server gespeichert ist).

App-Informationen kann mithilfe der Mobile Engagement-SDK-API oder mit dem Mobile Engagement Device-API registriert werden.

App Info ist ein Schlüssel-Wert-Paar ein Gerät zugeordnet. Der Schlüssel ist der Name des app Info (maximal 64 ASCII-Buchstaben [a-zA-Z], Ziffern [0-9 und Unterstrich [_]). Der Wert (begrenzt auf 1024 Zeichen) kann jede Zeichenfolge, ganze Zahl, Datum (JJJJ-MM-TT) oder boolescher Wert (true oder false).

Eine beliebige Anzahl von app Info kann ein Gerät im Rahmen von Mobile Engagement Preise Begriffe definiert zugeordnet werden. Für einen angegebenen Schlüssel registriert von Mobile Engagement nur neueste Wertesatz (kein Verlauf). Festlegen oder Ändern des Wertes einer app Info erzwingt Mobile Engagement Zielpublikum Überprüfung Kriterien auf diese app Info (falls vorhanden) bedeutet app Info verwendet werden kann Echtzeit Push auslösen.

## <a name="extra-data"></a>Zusätzliche Daten
Zusätzliche Daten ist (Extras) beliebigen Daten die Ereignisse, Fehler und Aufträge Aktivitäten zugeordnet werden können.

Extras sind ähnlich strukturiert, JSON-Objekte: sie besteht aus einer Struktur von Schlüssel-Wert-Paare. Schlüssel können höchstens 64 ASCII-Buchstaben [a-zA-Z], [0-9 und Unterstrich [_]) und die Gesamtgröße der Extras ist auf 1024 Zeichen (einmal in JSON Mobile Engagement-SDK).

Die gesamte Struktur des Schlüssel-Wert-Paare werden als JSON-Objekt gespeichert. Jedoch nur die erste Ebene der Schlüssel-Werte zerlegte einige erweiterten Funktionen wie Segmente direkt erreichbar ist (z. B. einfache können ein Segment namens "SciFi Lüfter" aus allen Benutzern gesendet mindestens 10 Mal das Ereignis mit dem Namen "Content_viewed" mit dem zusätzlichen Schlüssel "Content_type" den Wert "Scifi" im letzten Monat). Daher empfehlenswert, einfache Listen mit Schlüssel-Wert-Paare mit skalaren Werten (z. B. Zeichenfolgen, Datumsangaben, Zahlen oder Boolean) nur Extras aus senden.

## <a name="next-steps"></a>Nächste Schritte

- [Universelle Windows SDK-Übersicht für Azure Mobile Engagement](mobile-engagement-windows-store-sdk-overview.md)
- [Übersicht über die Windows Phone Silverlight SDK für Azure Mobile Engagement](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS SDK für Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
- [Android SDK für Azure Mobile Engagement](mobile-engagement-android-sdk-overview.md)
