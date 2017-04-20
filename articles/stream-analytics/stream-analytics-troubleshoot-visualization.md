<properties
    pageTitle="Visualisieren und Problembehandlung Stream Analytics-Aufträge | Microsoft Azure"
    description="Erfahren Sie, wie eine Pipeline Stream Analytics Auftrag für Self-service Problembehandlung mithilfe der Diagnosefunktion Diagramm darstellen."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualisieren und Problembehandlung Stream Analytics-Aufträge

Stream Analytics mit anderen Techniken cloudbasierten Problembehandlung manchmal braucht zu untersuchen, warum ein Auftrag nicht die erwartete Ausgabe (oder darum Ausgabe) erzeugt. Vor diesem Hintergrund ermöglicht Stream Analytics für streaming Projekt visualisieren. Dies ist auch als ein Modellierungstool mit Nebeneffekt dieser die Dokumentation ihrer Arbeit.

Im Visualisierungsfenster werden Eingaben dann alle Ausgaben konfiguriert und die auszuführende Abfrage angezeigt. Konnektivität oder Konfigurationsprobleme können deutlicher und es kann auch hilfreich sein, eine visuelle Darstellung Ihrer Konfiguration finden Sie unter.

## <a name="using-the-diagnosis-diagram-tool"></a>Werkzeug für Diagnose-Diagramm

Um diese Schnellansicht zugreifen, klicken Sie auf die Schaltfläche "Diagnose Diagramm" Blatt "Einstellungen" des dem Stream Analytics-Auftrags.

![Stream-Analytics-Troubleshoot-Visualization-Diagnosis-Diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Alle Eingaben und Ausgaben ist farblich an die aktuelle Komponente, wie unten dargestellt.

![Stream-Analytics-Troubleshoot-Visualization-Input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Der Benutzer möchte Zwischenergebnisse von Abfragen Schritte Muster der Daten innerhalb eines Auftrags zu verstehen, bietet die Visualisierungstool eine Ansicht der Aufschlüsselung der Abfrage Komponente Schritte und die Ablaufreihenfolge. Auf jedem Abfrage zeigt den entsprechenden Abschnitt in einer Abfrage Bereich wie bearbeiten. 

![Stream-Analytics-Troubleshoot-Visualization-Intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
