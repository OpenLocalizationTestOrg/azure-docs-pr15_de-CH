<properties 
    pageTitle="Konfigurieren für Stream Analytics-Aufträge gibt | Microsoft Azure" 
    description="Ausgaben für Stream Analytics-Aufträge konfigurieren | Lernen Pfadsegment."
    keywords="Datenausgabe, Datentransfer"
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

# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Konfigurieren gibt für Stream Analytics Einzelvorgänge

Azure Stream Analytics-Aufträge können eine oder mehrere Daten Ausgaben verbunden werden eine Verbindung zu einer vorhandenen Datensenke definieren. Stream Analytics-Auftrag verarbeitet und transformiert Daten wird ein Stream Ausgabe Ereignisse des Auftrags Ausgabe geschrieben.

Stream Analytics Daten Ausgänge können Echtzeit-Dashboards oder Alarme, Quelle Daten Bewegung Workflows auslösen oder einfach Archivdaten für die Stapelverarbeitung später. Stream Analytics bietet erstklassige Integration mit mehrere Azure Services, hier ausführlich dokumentiert sind.

Stream Analytics-Auftrag eine Ausgabe hinzu:

1. In der Azure-Verwaltungsportal auf **Ausgaben** und dann auf **Ausgabe hinzufügen** im Stream Analytics-Auftrag.

    ![Ausgaben hinzufügen](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  

    Klicken Sie im Azure-Portal **Ausgaben** in Ihrem Auftrag Stream Analytics.

    ![Azure Porta Ausgaben hinzufügen](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Geben Sie die Ausgabe:

    ![Daten verschieben auswählen](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  

    ![Azure-Portal Bewegung Datentyp auswählen](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Geben Sie einen Anzeigenamen für diese Ausgabe im **Ausgabe-Alias** . Name des Auftrags Abfrage später dienen die Ausgabe auf.  
    
    Füllen Sie den Rest der erforderlichen Verbindungseigenschaften Verbindung mit der Ausgabe.  Diese Felder Ausgabetyp variieren und sind hier definiert.  

    ![Hinzufügen von Datenausgabeeigenschaften](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  

4. Je Ausgabe müssen Sie angeben, wie Daten serialisiert oder formatiert wurde. Hier werden bestimmte Serialisierungseinstellungen für jede Ausgabe dokumentiert.

    Füllen Sie den Rest der erforderlichen Verbindungseigenschaften für die Verbindung zur Datenquelle. Diese Felder unterscheiden sich je nach Typ des Typs von Eingang und Quelle und detailliert definiert [hier](stream-analytics-create-a-job.md).  

    ![Ereignis-Hub Daten hinzufügen](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  

    ![Azure Portal Datenausgabe an Event hub](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [Azure.Note] Ein Output-Element dem Auftrag hinzugefügt muss vorhanden sein, bevor der Auftrag gestartet und Ereignisse fließen. Beispielsweise verwenden Sie BLOB-Speicher als Ausgabe erstellt der Auftrag kein Speicherkonto automatisch. Sie muss vom Benutzer erstellt werden, bevor der ASA-Auftrag gestartet wird.

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
