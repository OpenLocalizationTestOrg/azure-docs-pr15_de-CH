<properties 
    pageTitle="Aufträge starten streaming in Stream Analytics | Microsoft Azure" 
    description="Wie Stapelverarbeitung ein streaming in Azure Stream Analytics | Lernen Pfadsegment."
    keywords="Streaming-Aufträge"
    documentationCenter=""
    services="stream-analytics"
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

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>In Azure Stream Analytics streaming Job ausführen

Wenn ein Auftrag Eingabe, Abfrage und Ausgabe alle angegeben wurden, Stream Analytics-Auftrag zu starten.

Ihr Projekt starten:

1.  Im klassischen Azure Portal aus dem Dashboard Auftrag **beginnen** am unteren Rand der Seite klicken.

    ![Schaltfläche Auftrag starten](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Azure-Portal klicken Sie am oberen Rand der Seite Projekt **Starten** .

    ![Azure Portal Start Job Schaltfläche](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Geben Sie **Start** Ausgabewert zu bestimmen, wenn dieses Projekt startet Ausgabe. Die Standardeinstellung für Aufträge, die zuvor nicht gestartet ist **Auftragsstartzeit**bedeutet, dass der Auftrag sofort Datenverarbeitung. (Für Verlaufsdaten verbraucht) oder die Zukunft (Verarbeitung bis zu einem späteren Zeitpunkt verzögern) können Sie auch eine **benutzerdefinierte** Zeit angeben. Fällen wird ein Auftrag wurde bereits gestartet und beendet, die Option **Zuletzt beendet** um den Auftrag aus der letzten Ausgabe fortsetzen und Datenverlust zu vermeiden.  

    ![Starten Sie streaming-Auftrags](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure Portal starten streaming Auftrags](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Bestätigen Sie Ihre Auswahl. Der Status wechselt zu *Starten* und verschiebt in Kürze *ausgeführt* , sobald der Auftrag gestartet wurde. Sie können den Fortschritt der Operation **Starten** im **Notification Hub**überwachen:

    ![Streaming-Job-Status](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Azure-Portal streaming Job-Status](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
