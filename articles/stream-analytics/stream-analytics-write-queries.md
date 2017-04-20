<properties 
    pageTitle="Wie Abfragen in Stream Analytics | Microsoft Azure" 
    description="Schreiben von Abfragen in Stream Analytics und Daten | Lernen Pfadsegment."
    keywords="wie Abfragen Daten schreiben, Schreiben Sie eine Abfrage Schreiben von Abfragen"
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

# <a name="how-to-write-queries-in-stream-analytics"></a>Wie Stream Analytics Abfragen

Schreiben von Abfragen für Verarbeitungslogik in Azure Stream Analytics Stream wird implementiert "stehende Abfrage", die vor ein Auftrag ausgeführt Daten erreichen den Auftrag definiert ist. Die Transformation wird in einer SQL-ähnlichen Abfragesprache ausgedrückt ist weitgehend eine Teilmenge der T-SQL mit einigen hinzugefügt wie [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) zeitlichen Semantik express.

## <a name="writing-queries"></a>Schreiben von Abfragen: ##

1. Stream Analytics Druckauftrag in Azure-Verwaltungsportal klicken Sie auf **Abfrage**.

    ![Auswahlabfrage](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Klicken Sie im Azure-Portal auf **Abfrage**.

    ![Wählen Sie Vorschau Abfragen](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Arbeitsplätze sind Abfragevorlage zum Einstieg. Die Abfragevorlage führt eine "Pass" Abfrage Felder Projekte von Eingabeereignissen in die Ausgabe.  

    - Wenn Sie mindestens eine Eingabe und Ausgabe für den Job definiert haben, ersetzen Sie den Platzhalter "[YourOutputAlias]" und "YourInputAlias" Felder mit den Aliasnamen der ein- und Ausgabe soll zuerst verwenden. Darüber hinaus können noch erstellen und Testen Sie Ihre Abfrage in der Azure-Verwaltungsportal ohne Eingaben und Ausgaben für das Projekt.
    - Wenn Sie mehr als eine einfache Pass verarbeiten möchten, können Sie die Abfragedefinition bearbeiten. Mit Abfrage erstellen zunächst betrachten einige allgemeine Muster erfasst Abfrage [hier](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Abfragedaten Fenster](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Überprüfen von Daten arbeiten: ##

Sie können testen, ob die Abfrage verhält sich, als über eine oder mehrere lokale JSON Dateien mit Daten im Browser ausführen. Dies startet den Auftrag oder nicht Abrechnung auswirken.

> [AZURE.NOTE] Derzeit wird im Browser Abfragetests im Azure-Portal nicht unterstützt.  

1.  Stellen Sie sicher, dass keine Fehler in der Abfrage (andernfalls die Test-Schaltfläche deaktiviert sein) und klicken Sie dann auf die Schaltfläche Test.  

    ![Abfragedaten Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  Sie werden aufgefordert, die Dateien für den in der Abfrage referenziert Eingaben an. In diesem Beispiel die Vorlage Abfrage als Links-ist, damit das Dialogfeld für die Eingabe mit dem Namen "Yourinputalias" aufgefordert.  

    ![Test-Datenabfrage](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Wechseln Sie zu einer Testdatei. Sind mehrere Beispieldateien auf [Github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) und Beispieldaten aus eigenen Stream Dateneingaben Beispieldaten Funktion auf der Registerkarte Eingaben abrufen.  

    ![Abfrage Eingang](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  Nach dem Schließen des Dialogfelds die Abfrage über die Testdaten auszuführen und die Ergebnisse am Ende der Seite Abfrage.  

    ![Abfrage-Zusammenfassung](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
