<properties
    pageTitle="Dateneingabe für Stream Analytics-Aufträge hinzufügen | Microsoft Azure"
    description="Erfahren Sie, wie eine Datenquelle Stream Analytics-Auftrag als Stream Dateneingabe aus Ereignis Hubs oder Verweis aus dem Blogspeicher einbinden."
    keywords="Daten, Daten"
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


# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Zum Hinzufügen eines Streaming-Daten ein- oder Verweis auf einen Stream Analytics-Auftrag

Informationen Sie zum Anschließen einer Datenquelle für Ihren Auftrag Stream Analytics als Stream Dateneingabe aus Ereignis Hubs oder Verweis Daten-BLOB-Speicher.

Azure Stream Analytics-Aufträge können verbunden werden, bis eine Eingabe, jeweils eine Verbindung mit einer vorhandenen Datenquelle definieren. Daten an die Datenquelle gesendet werden, ist von Stream Analytics-Auftrag verwendet und wie die Daten verarbeitet. Stream Analytics bietet erstklassige Integration mit [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/) und [Azure BLOB-Speicher](../storage/storage-dotnet-how-to-use-blobs.md) innerhalb und außerhalb des Auftrags Abonnement.

Dieser Artikel ist ein Schritt im [Stream Analytics Lernpfad](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Dateneingabe: Streaming-Daten und Daten

Es gibt zwei Typen von Eingaben im Stream Analytics: Datenströme und Referenzdaten.

- **Datenströme**: Stream Analytics-Aufträge sind mindestens ein Stream Dateneingabe verbraucht und dem Auftrag umgewandelt. Azure BLOB-Speicher und Azure Ereignis Hubs werden als input Stream Datenquellen unterstützt. Azure Ereignis Hubs dienen zum Erfassen von Ereignisstreams von verbundenen Geräte, Dienste und Programme. Azure BLOB-Speicher kann als eine Eingabequelle für Aufnahme Datenmengen als Stream verwendet werden.  
- **Referenzdaten**: Stream Analytics unterstützt eine zweite zusätzliche Eingabe als Daten.  Im Gegensatz zu Daten werden diese statische oder Verlangsamung ändern.  Sie wird normalerweise zum Suchen und Korrelationen mit Datenströmen umfangreichere Daten erstellen.  Azure BLOB-Speicher ist derzeit die einzige unterstützte Eingabequelle für Referenzdaten.  

Eingabe der Stream Analytics-Projekt hinzu

1. Im Azure-Portal auf **Eingaben** und klicken Sie **Eingabe** im Stream Analytics-Auftrag.

    ![Azure-Verwaltungsportal - Eingabe hinzufügen.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    Klicken Sie im Azure-Portal **Eingaben** im Stream Analytics-Auftrag.  

    ![Azure-Portal - Daten hinzufügen.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. Geben Sie die Eingabe: **Datenstrom** oder **Daten**.

    ![Fügen Sie die richtigen Daten eingeben, übertragen oder Verweis hinzu](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![Fügen Sie die richtigen Daten eingeben, übertragen oder Verweis hinzu](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. Erstellen eine Eingabe-Datenstrom Geben Sie den Quelle für die Eingabe.  Während der Erstellung als nur BLOB-Daten, Speicher unterstützt wird, kann dieser Schritt übersprungen werden.

    ![Dateneingabe Stream Daten hinzufügen](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![Dateneingabe Stream Daten hinzufügen](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. Geben Sie einen Anzeigenamen für diese Eingabe in das Feld Input Alias.  Name des Auftrags Abfrage später dienen die Eingabe auf.

    Füllen Sie den Rest der erforderlichen Verbindungseigenschaften für die Verbindung zur Datenquelle. Diese Felder unterscheiden sich je nach Typ des Typs von Eingang und Quelle und detailliert definiert [hier](stream-analytics-create-a-job.md).  

    ![Hinzufügen von Ereignis-Hub Dateneingabe](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. Festlegen der Serialisierung die Eingabedaten:
    - Geben Sie zu Abfragen arbeiten Sie erwarten, das **Ereignis Serialisierungsformat** eingehender Daten.  Unterstützte Serialisierungsformat sind JSON, CSV und Avro.
    - Überprüfen Sie die **Codierung** für die Daten.  UTF-8 ist derzeit das einzige unterstützte Codierungsformat.

    ![Daten-Serialisierungseinstellungen für die Dateneingabe](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![Daten-Serialisierungseinstellungen für die Dateneingabe](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. Nach Abschluss das Eingabe erstellen, prüft Stream Analytics, mit der Datenquelle verbinden können.  Sie können den Status des Vorgangs Verbindung testen im Notification Hub anzeigen.

    ![Verbindungstest streaming Dateneingabe](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![Verbindungstest streaming Dateneingabe](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Hilfe mit streaming-Daten
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
