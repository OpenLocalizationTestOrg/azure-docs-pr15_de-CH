<properties 
    pageTitle="Understanding Stream Analytics Projekt überwachen | Microsoft Azure" 
    description="Grundlegendes zu Stream Analytics Projekt überwachen" 
    keywords="Abfrage-monitor"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Verstehen Sie Stream Analytics Projekt überwachen und die Abfragen zu überwachen

## <a name="introduction-the-monitor-page"></a>Einführung: Die Seite "Monitor"

Azure-Verwaltungsportal und Azure-Portal Oberfläche wichtige Leistungsindikatoren zum Überwachen und behandeln die Abfrage- und Auftrag Leistung. 

Azure-Verwaltungsportal klicken Sie auf die Registerkarte **Monitor** ein ausgeführter Auftrag Stream Analytics zu diesen Metriken finden Sie unter. Es gibt eine Verzögerung von höchstens 1 Minute Leistungsmetriken auf Monitor angezeigt.  

  ![Überwachung der Job Dashboard](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

Suchen Sie in Azure-Verwaltungsportal Stream Analytics Projekts sehen Metriken für interessiert sind und **Überwachung** im Abschnitt.  

  ![Azure-Portal überwachen Auftrag Dashboard](./media/stream-analytics-monitoring/06-stream-analytics-monitoring.png)  

Beim ersten Erstellen eines Stream Analytics-Auftrags in einem Bereich müssen Sie für diese Region Diagnose konfigurieren. Hierzu wird überall auf die **Überwachung** und Blade- **Diagnose** angezeigt. Hier können Sie Diagnose und ein Speicherkonto zur Überwachung von Daten angeben.  

  ![Azure Portals Abfrage Diagnose](./media/stream-analytics-monitoring/07-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Metriken für Stream Analytics


| Metrik | Definition |
|--------|-------------|
| SU % Auslastung | Nutzung von Streaming Einheit(en) zugewiesen einen Auftrag auf der Registerkarte Skalierung des Auftrags. Dieser Indikator 80 % erreichen oder, mit hoher Wahrscheinlichkeit Verarbeitung verzögern oder Fortschritte beendet ist. |
| Ereignisse | Stream Analytics-Auftrags Anzahl Ereignisse empfangene Datenmenge. Dies lässt sich überprüfen, ob Ereignisse an die Datenquelle gesendet werden. |
| Ausgabeereignisse | Datenmenge per Stream Analytics-Auftrags auf Ausgabe in Anzahl der Ereignisse. |
| Ereignisse außerhalb der Reihenfolge | Anzahl der Ereignisse empfangen wurden, die gelöscht oder eine angepasste zeitstempelbasierte zur Veranstaltung Bestellung angegeben wurden. Dies kann durch die Konfiguration der Einstellung aus der Toleranzfenster beeinflusst werden. |
| Fehler bei der Konvertierung | Anzahl der Datenkonvertierungsfehler entstandenen Stream Analytics-Auftrag. |
| Laufzeitfehler | Anzahl der Fehler, die während der Ausführung des Auftrags Stream Analytics auftreten. |
| Spät Eingabeereignisse | Anzahl der Ereignisse, die aus der Quelle die entweder gelöscht oder deren Timestamp spät wurde korrigiert, basierend auf Bestellung Richtlinienkonfiguration Ereignis spät Ankunft Toleranzfenster festlegen. |

## <a name="customizing-monitoring-in-the-azure-management-portal"></a>Anpassen der Überwachung im Azure-Verwaltungsportal ##

Bis zu 6 Metriken können in einem Diagramm angezeigt werden.

Wählen Sie zum Wechseln zwischen der Anzeige relative Werte (Endwert für jede Metrik) und absolut (y-Achse angezeigt), relativen oder absoluten am oberen Rand des Diagramms.

  ![Abfrage Monitor relativen Absolute](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)  

Metriken können im Monitor Diagramm in Aggregationen von 1 Stunde, 12 Stunden, 24 Stunden oder 7 Tage angezeigt werden.

Ändern des Zeitraums auswählen Metriken Diagramm zeigt 1 Stunde, 24 Stunden und 7 Tage am oberen Rand des Diagramms

  ![Abfrage Monitor Zeitskala](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)  

Sie können Regeln festlegen, die Sie per e-Mail benachrichtigen können bei die Arbeit einen definierten Schwellenwert überschreitet. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Anpassen der Überwachung im Azure-Portal ##

Anpassen des Diagramms dargestellt, Metriken, und Uhrzeitbereich in den Einstellungen bearbeiten. Weitere Informationen finden Sie unter [Monitoring anpassen](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Azure Portal Abfrage Monitor-Zeitskala](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  

## <a name="job-status"></a>Auftragsstatus

Der Status der Stream Analytics-Aufträge kann in Azure-Verwaltungsportal Sie eine Liste der Aufträge finden angezeigt werden. Die Liste der Aufträge sehen in Azure-Verwaltungsportal Stream Analytics klicken.

| Status | Definition |
|--------|------------|
| Erstellt | Ein Auftrag erstellt wurde, jedoch wurde nicht gestartet. |
| Starten | Ein Klicken auf Start der Aufgabe und der Auftrag starten |
| Ausführen | Der Auftrag wird zugewiesen, Eingabe verarbeiten oder verarbeiten warten. Weist das Projekt einen aktiven Zustand ohne Ausgabe, dürfte das Zeitfenster Datenverarbeitung Groß oder die Abfragelogik kompliziert. Grund ist möglicherweise, dass derzeit keine Daten an den Auftrag. |
| Beenden | Ein Klicken auf Stop den Auftrag und Beenden des Auftrags. |
| Beendet | Der Auftrag wurde abgebrochen. |
| Heruntergestuft | Dieser Status zeigt an, dass ein Stream Analytics-Auftrag vorübergehender Fehler auftreten (für ex. Eingabe/Ausgabe-Fehler Verarbeitungsfehler, Konvertierungsfehler usw.). Der Auftrag wird noch ausgeführt, es gibt jedoch viele Fehler generiert. Dieser Auftrag benötigt Kunden und der Kunden sehen log Fehler. |
| Fehler | Dies bedeutet, dass der Auftrag aufgrund von Fehlern schlug und die Verarbeitung beendet hat. Der Kunde muss sich in der Log Fehler debuggen. |
| Löschen | Dies bedeutet, dass der Auftrag gelöscht wird. |

## <a name="diagnosis"></a>Diagnose

Im Azure-Verwaltungsportal Informationen Auftrag Dashboard zur sollten suchen Diagnose, d. h. Eingaben ausgibt oder Vorgänge protokollieren. Sie können auf den Link zu der entsprechenden Stelle der Diagnose betrachten klicken.

  ![Abfrage Monitor Fehler](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)  

Auf die Eingabe- oder Ressource bietet ausführlichen Diagnoseinformationen. Dies wird mit Diagnoseinformationen aktualisiert, während der Auftrag ausgeführt wird.

  ![Abfrage-Diagnose](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)  

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
