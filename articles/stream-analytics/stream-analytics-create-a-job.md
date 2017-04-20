<properties 
    pageTitle="Erstellen einen Data Analytics verarbeitungsauftrag für Stream Analytics | Microsoft Azure" 
    description="Erstellen Sie einen Data Analytics verarbeitungsauftrag für Stream Analytics | Lernen Pfadsegment."
    keywords="Verarbeitung der Daten"
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

# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Erstellen einen Data Analytics verarbeitungsauftrag für Stream Analytics

Ressource die obersten Ebene in Azure Stream Analytics ist ein Stream Analytics-Auftrag.  Es besteht eine oder mehrere Datenquellen eine Abfrage die Datentransformation Ausdrücken und ein oder mehrere Ziele, denen Ergebnisse geschrieben werden. Zusammen ermöglichen diese Datenanalyse für streaming Szenarien bereit.

Um mit Stream Analytics, zunächst einen neuen Stream Analytics-Auftrag erstellen.  Beachten Sie, dass dadurch keine Rechnungsadressen Auswirkungen hat, bis der Einzelvorgang gestartet ist.

1.  Melden Sie sich auf die online [Azure-Verwaltungsportal](http://manage.windowsazure.com) oder [Azure-Portal](https://portal.azure.com/).
2.  Im Portal: **Neue klicken**, dann klicken Sie auf **Data Services** bzw. **Datenanalyse** Ihr Portal und dann auf **Azure Stream Analytics** oder **Stream Analytics** und **Schnell erstellen**.

    ![Verarbeitung der Daten-Assistenten](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    ![Erstellen Sie Datenanalyse Druckauftrags](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Geben Sie die gewünschte Konfiguration für den Stream Analytics-Auftrag.
    - Geben Sie im **Auftrag** einen Namen für den Stream Analytics-Auftrag. Wenn der **Auftragsname** überprüft wird, wird ein grünes Häkchen im Feld Auftrag angezeigt. Der **Auftragsname** darf nur alphanumerische Zeichen enthalten und '-' Zeichen und muss zwischen 3 und 63 Zeichen lang sein.
    - Verwenden Sie **Region** in Azure-Portal oder **Speicherort** im Azure-Portal an dem Standort, der Auftrag ausgeführt werden soll.
    - Verwenden das Azure-Portal wählen Sie oder erstellen Sie ein Speicherkonto **Regionalen Überwachung Speicherkonto**verwenden. Dieses Speicherkonto zum Speichern von Daten für alle Stream Analytics in diesem Bereich ausgeführt.
    - Wenn des Azure-Portals Geben Sie eine neue oder vorhandene **Ressourcengruppe** zu verwandten Ressourcen für die Anwendung.

4.  Sobald die neuen Stream Analytics Job Optionen konfiguriert sind, klicken Sie auf **Stream Analytics-Auftrag erstellen**. Es dauert ein paar Minuten dafür Stream Analytics erstellt werden. Zum Überprüfen des Status können Sie die im Hub Benachrichtigungen überwachen.

    ![Data Analytics Verarbeitung Job Benachrichtigungen hub](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure Portal Datenanalyse Druckauftrags erstellen Auftrag](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  Das neue Projekt erscheint mit dem Status **erstellt**. Beachten Sie, dass die Schaltfläche **Start** deaktiviert ist. Sie müssen Job Eingabe, Abfrage und Ausgabe zunächst den Auftrag konfigurieren.

    ![Verarbeitung der Daten Auftrag Status](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure Portal Datenanalyse Auftragsstatus Auftrag verarbeiten](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
