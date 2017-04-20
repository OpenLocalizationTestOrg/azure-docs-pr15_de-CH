<properties
    pageTitle="Mobile Export-API Projektübersicht"
    description="Grundlegendes zum Exportieren der Rohdaten Endgeräte in Ihre eigenen Tools nutzen"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Mobile Export-API Projektübersicht

## <a name="introduction"></a>Einführung

In diesem Dokument lernen Sie die Grundlagen zum Exportieren der Rohdaten Endgeräte in Ihre eigenen Tools nutzen.

## <a name="pre-requisites"></a>Erforderliche Komponenten

Exportieren die Rohdaten aus Mobile erfordert:

- API-Authentifizierung Setup können die APIs verwenden (siehe [Manuelles Einrichten der Authentifizierung](mobile-engagement-api-authentication-manual.md))
- Verwenden Sie die REST-APIs oder [.net SDK](mobile-engagement-dotnet-sdk-service-api.md),
- Ein Azure Storage-Konto.

>[AZURE.NOTE] Wir empfehlen auch hervorragende [Microsoft Azure Storage Explorer](http://storageexplorer.com/)mindestens während der Entwicklungsphase bietet eine benutzerfreundliche Benutzeroberfläche für die Interaktion mit Azure-Speicher.

## <a name="what-can-be-exported"></a>Was können exportiert werden?

Mobile Engagement hat daher verschiedene Export für diese Datentypen geeignet und ermöglicht die viele Arten von Daten zu sammeln.
Es gibt 2 wichtige Arten exportieren:

- Snapshot: in der Regel zum Exportieren von Daten, die einen Zustand darstellt und für die Mobile Engagement keinen Verlauf. Hierzu zählen beispielsweise Tags (app Info), Token oder Push Kampagne Feedback. Daher beziehen sich diese Art von Export nicht auf ein Datum.
- Historisch: dieser Export wird beispielsweise mit der Zeit Aktivitäten oder Ereignisse sammelt Daten verwendet.

Die Tabelle unten beschreibt ausführlich alle möglichen Exporte:

| Exporttyp | Datentyp | Beschreibung                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Snapshot    | Push      | Generiert einen Export Push Kampagnen Feedbacks jeweils pro Deviceid/Benutzer-ID                                                              |
| Snapshot    | Tag       | Generiert einen Export der jeweils zugeordneten Tags (app-Info)                                                                       |
| Snapshot    | Gerät    | Erzeugt die meisten Daten über Geräte technischen (Modell, Gebietsschema, Zeitzone,...), Tags, erstmals gesehen exportieren... |
| Snapshot    | Token     | Exportieren der gültigen Token generiert                                                                                                 |
| Versionsgeschichte  | Aktivität  | Generiert einen Export aller Aktivitäten für die einzelnen Geräte in einem bestimmten Zeitraum                                                           |
| Versionsgeschichte  | Ereignis     | Generiert einen Export aller Aktivitäten für die einzelnen Geräte in einem bestimmten Zeitraum                                                           |
| Versionsgeschichte  | Auftrag       | Generiert einen Export aller Aufträge für die einzelnen Geräte in einem bestimmten Zeitraum                                                                 |
| Versionsgeschichte  | Fehler     | Generiert einen Export alle Fehler für die einzelnen Geräte in einem bestimmten Zeitraum                                                               |

## <a name="how-does-it-work"></a>Wie funktioniert es?

Export lang sind Aufgaben mit großer Datendateien erzeugen kann. Aus diesem Grund können sie eine Datei herunterladen sofort zurückgeben aufgerufen werden.
Um Datenexport aus Mobile müssen Sie ein **Projekt exportieren** über API erstellen, wenn Sie im Allgemeinen angeben:

- Der Typ des Exports (Snapshot oder Verlaufsdaten)
- Den Datentyp
- Der **Azure-Behälter** (einschließlich einer gültigen SAS mit Schreibzugriff), in dem das Ergebnis des Exports geschrieben.

Beachten Sie, dass dauert möglicherweise einige Minuten den Sicherungsauftrag gestartet werden und es einige Sekunden für kleine apps mehrere Stunden für apps mit vielen Benutzern oder Aktivität führen kann.

Nachdem das Projekt erstellt wurde, ist möglich, prüfen Sie den Status, wenn es noch ausgeführt wird oder abgeschlossen wurde.

Wenn der Auftrag erfolgreich ausgeführt, steht die resultierende Datei auf bereitgestellten Behälter.
