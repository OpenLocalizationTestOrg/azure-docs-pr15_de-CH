<properties 
   pageTitle="Azure Mobile Engagement Fehlerbehebung - Analysen" 
   description="Problembehandlung bei der Analyse, Überwachung, Segmentierung und Dashboard in Azure Mobile Engagement" 
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

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Fehlerbehebung für Analyse, Überwachung, Segmentierung und Dashboards

Im folgenden werden mögliche Probleme mit wie Azure Mobile Engagement Informationen über Ihre Anwendung, Geräten und Benutzern sammelt auftreten.

## <a name="missingdelayed-information"></a>Fehlende/verzögerte Informationen

### <a name="issue"></a>Problem
- Informationen verzögert Analytics, Segmentierung oder Dashboard angezeigt.
- Überwachung fehlen Informationen.
- Fehlen Informationen Analyse, Segmentierung oder Dashboard.
- Beschränkt auf Segmentierung.

### <a name="causes"></a>Ursachen

- Analytics-API Monitor API verwenden und Segmente API Daten nicht in der Benutzeroberfläche sind Sie durch die APIs sichtbar ist.
- Wenn Azure Mobile Engagement SDK nicht richtig in die Anwendung integriert ist wird nicht dann Sie Informationen in die Analyse, Segmentierung, Überwachung oder Dashboards können.
- Segmente nicht geändert, sobald sie erstellt sind, Segmente beschränkt "geklonte" (kopiert) oder "vernichtete" (gelöscht). Segmente können nur 10 Kriterien enthalten.
- Am besten testen Sie fehlende Informationen aus der Überwachung werden von setup ein Testgerät, deinstallieren und installieren die Anwendung auf Testgerät.
- Informationen ist für Analyse, Segmentierung oder Dashboards innerhalb von 24 Stunden aktualisiert.
- Informationen in neue Segmente können nicht bis 24 Stunden, nachdem sie erstellt wurden, auch wenn das Segment zuvor genannten Informationen angezeigt werden.
- Filtern Ihre Analysedaten in der Benutzeroberfläche wird unabhängig von der Version der Anwendung alle Beispiele anzeigen (z. B. "stürzt ab" gefiltert nach Namen von Version 1 und Version 2 der app zeigen)
- Zeitraum für Analysen basiert auf das Datum aus dem geräteeinstellungen, damit ein Benutzer, dessen Telefonnummer falsch festgelegte Datum hat, falschen Zeitraum anzeigen kann.
- Keine serverseitige Daten protokolliert, wenn Sie die Schaltfläche mithilfe "test" drückt, Daten nur für echte pushkampagnen protokolliert.

## <a name="cant-locate-items-in-ui"></a>Elemente in Benutzeroberfläche nicht gefunden werden.

### <a name="issue"></a>Problem
- Nicht Segmente basierend auf bestimmte integrierte oder benutzerdefinierte app Info tag-Kriterien.
- Nicht gefunden bestimmte integrierte oder benutzerdefinierte app Info tag-Kriterien Analyse, Überwachung oder Dashboards.
- Die Daten in die Analyse, Überwachung, Segmentierung oder Dashboards nicht interpretiert werden.

### <a name="causes"></a>Ursachen

- Einige Elemente integriert und app infotags stehen nur als Push Kriterien können jedoch nicht in einem Segment oder sichtbar aus Analyse, Überwachung oder Dashboard. 
- Integrierte Elemente und app-Info-Tags ein Segment hinzugefügt werden können, müssen Setup Liste auf Kriterien für jede Kampagne Sie dieselbe Funktion wie auf Grundlage eines Segments.
- Siehe Kontextmenüs im Abschnitt Analyse, Überwachung, Segmentierung und Dashboards Azure Mobile Engagement Benutzeroberfläche für weitere Hilfe und Informationen.

## <a name="crash-troubleshooting"></a>Problembehandlung bei Abstürzen

### <a name="issue"></a>Problem
- Anwendung stürzt in Analyse, Überwachung oder Dashboard angezeigt werden.

### <a name="causes"></a>Ursachen

- Beheben überprüfen Anwendung abstürzt angezeigt, Analyse, Überwachung oder Dashboard Sie die Versionshinweise für bekannte Probleme mit früheren Versionen des SDK.
- Um beheben führen Programmabstürze ein Ereignis Testgerät mit der Anwendung installiert und die Geräte-ID im Abschnitt "Überwachen von Ereignissen –" Azure Mobile Engagement Benutzeroberfläche zu suchen. Führen Sie dann das Ereignis, das die Anwendung zum Absturz gebracht und Weitere Informationen im Abschnitt "Überwachen – Crash" Azure Mobile Engagement Benutzeroberfläche verursacht. 

 
