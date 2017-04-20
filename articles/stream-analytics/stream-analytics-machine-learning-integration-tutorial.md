<properties
    pageTitle="Stimmungsanalyse mithilfe von Azure Stream Analytics und Azure Machine Learning | Microsoft Azure"
    description="Wie Sie eine benutzerdefinierte Funktion und maschinelles lernen in einem Stream Analytics-Auftrag"
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
    ms.date="10/04/2016" 
    ms.author="jeffstok"
/>

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Stimmungsanalyse mithilfe von Azure Stream Analytics und Azure maschinelles lernen #

Dieser Artikel soll Sie schnell eine einfache Azure Stream Analytics-Auftrag Azure Machine Learning-Integration einrichten. Wir verwenden ein Gefühl Analytics maschinelles lernen Modell aus der Galerie Cortana Intelligence Textdaten analysieren und ermitteln die Stimmung Bewertung in Echtzeit. Die Informationen in diesem Artikel können Sie die Szenarien wie Echtzeit Stimmung Analytics auf Twitter Daten vertraut, Datensätze Gespräche mit Kunden zu analysieren und auswerten Kommentare auf Foren, Blogs und Videos neben vielen anderen Echtzeit, vorhersehbaren Punktzahl Szenarien.

Dieser Artikel enthält eine Beispiel CSV-Datei mit Text als Eingabe in Azure BLOB-Speicher in der folgenden Abbildung dargestellt. Der Auftrag gilt Stimmung Analytics Modell als eine benutzerdefinierte Funktion (UDF) Beispiel Textdaten aus dem BLOB-Speicher. Das Ergebnis wird im gleichen BLOB-Speicher in einen anderen CSV-Datei platziert. 

![Stream Analytics maschinelles lernen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Die folgende Abbildung zeigt diese Konfiguration. Für einen realistischeren Szenario können Sie BLOB-Speicher mit Twitter Daten von Azure Ereignis Hubs Eingabe ersetzen. Darüber hinaus könnten Sie eine [Microsoft Power BI](https://powerbi.microsoft.com/) in Echtzeit Visualisierung der aggregate Stimmung erstellen.    

![Stream Analytics maschinelles lernen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Erforderliche Komponenten

Die erforderlichen Komponenten für die Aufgaben, die in diesem Artikel gezeigt lauten folgendermaßen:

-   Ein aktives Azure-Abonnement.
-   Eine CSV-Datei einige Daten. Von [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv)in Abbildung 1 gezeigte Datei herunterladen oder eigene Datei erstellen. In diesem Artikel wird angenommen, dass bereitgestellt auf GitHub verwenden.

Auf einer hohen Ebene werden zum Ausführen der Aufgaben in diesem Artikel veranschaulicht Folgendes:

1.  Hochladen der CSV-Datei in Azure BLOB-Speicher.
2.  Arbeitsbereich Azure Machine Learning ein Gefühl Analytics Modell aus Cortana Intelligence Gallery hinzufügen.
3.  Bereitstellen Sie dieses Modell als Webdienst im Arbeitsbereich Computer Learning.
4.  Erstellen Sie einen Stream Analytics-Auftrag, der eine Funktion eingegeben Gefühl bestimmt diesen Webdienst aufruft.
5.  Der Stream Analytics-Auftrag gestartet, und beobachten Sie die Ausgabe.

## <a name="upload-the-csv-input-file-to-blob-storage"></a>Hochladen der CSV-Datei in den BLOB-Speicher

Für diesen Schritt können Sie eine CSV-Datei wie bereits angegeben als GitHub heruntergeladen. Können [Azure Storage Explorer](http://storageexplorer.com/) oder Visual Studio die Datei hochzuladen oder benutzerdefinierten Code verwenden. Wir verwenden Visual Studio Beispiele.

1.  Klicken Sie in Visual Studio auf **Azure** > **Speicher** > **Externspeicher anfügen**. Geben Sie einen **Kontonamen** und ein **Kontoschlüssel**.  

    ![Stream Analytics maschinelles lernen, Server-Explorer](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  Erweitern Sie den Speicher, den Sie in Schritt 1 angeschlossen und geben Sie dann einen logischen Namen auf **Blob-Container erstellen**. Nach dem Erstellen des Containers öffnen Sie, um dessen Inhalt anzuzeigen. (Es wird zu diesem Zeitpunkt leer).  

    ![Stream Analytics maschinelles lernen, Blob erstellen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  Hochladen die CSV-Datei auf **Hochladen Blob**und klicken Sie dann auf **lokale Datei**.  

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Stimmung Analytics Modell aus Cortana Intelligence Galerie hinzufügen

1.  [Prädiktive Stimmung Analytics Modell](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) aus Cortana Intelligence Gallery herunterladen  
2.  Klicken Sie in Machine Learning Studio **Studio öffnen**.  

    ![Stream Analytics maschinelles lernen öffnen Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  Melden Sie zum Arbeitsbereich gehen an. Wählen Sie den Speicherort, der Ihren Standort am besten entspricht.
4.  Klicken Sie am unteren Rand der Seite **Ausführen** .  
5.  Nachdem der Prozess erfolgreich ausgeführt wird, klicken Sie auf **Webdienst bereitstellen**.
6.  Das Gefühl Analytics Modell ist einsatzbereit. Um zu überprüfen, klicken Sie auf die Schaltfläche **Test** und mitteilen Sie, Text, wie "Ich liebe Microsoft." Der Test sollte ein Ergebnis ähnlich der folgenden zurück:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Stream Analytics maschinelles lernen, Daten](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

Klicken Sie in der Spalte **Apps** für **Excel 2010 oder früher Arbeitsmappe** den API-Schlüssel und die URL, die Sie später den Stream Analytics-Auftrag einrichten müssen. (Dieser Schritt muss nur ein Computerlernen Modell aus einem anderen Arbeitsbereich Azure-Konto verwenden. Dieser Artikel setzt voraus, dass dies der Fall, dieses Szenario ist.)  

Beachten Sie die URL und Access Webdienstschlüssel aus der heruntergeladenen Excel-Datei wie folgt:  

![Stream Analytics maschinelles lernen Blick](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Erstellen Sie einen Stream Analytics-Auftrag, der das Computerlernen verwendet

1.  Zum [Azure-Portal](https://manage.windowsazure.com).  
2.  **Klicken Sie auf** > **Data Services** > **Stream Analytics** > **schnell erstellen**. Geben Sie einen Namen für den Auftrag in **Auftrag**Geben Sie die entsprechende Region für das Projekt im **Bereich**und wählen Sie das Konto in **Regionalen Überwachung Speicherkonto**.    
3.  Nachdem der Auftrag, auf der Registerkarte **Eingaben erstellt wurde** klicken Sie auf **Eingabe hinzufügen**.  

    ![Stream Analytics maschinelles lernen, maschinelles lernen Eingabe hinzufügen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  Auf der ersten Seite des Assistenten **Hinzufügen** **Datenstrom**auf und klicken Sie dann auf **Weiter**. Auf der nächsten Seite auf **BLOB-Speicher** als Eingabe und klicken Sie dann auf **Weiter**.  
5.  Bieten Sie auf **BLOB-Einstellungen** des Assistenten den Speicher BLOB-Container ein, zuvor beim Hochladen der Daten definierten. Klicken Sie auf **Weiter**. **Ereignis-Serialisierungsformat**klicken Sie auf **CSV**. Übernehmen Sie die Standardwerte für den Rest der **Serialisierung** Einstellungsseite. Klicken Sie auf **OK**.  
6.  Klicken Sie auf der Registerkarte **Ausgaben** **Hinzufügen eine Ausgabe**.  

    ![Stream Analytics maschinelles lernen, hinzufügen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  **BLOB-Speicher**auf und geben Sie die gleichen Parameter außer dem Container. Der Wert für die **Eingabe** wurde konfiguriert, Lesen aus dem Container mit dem Namen "Test" in die **CSV-** Datei hochgeladen wurde. **Ausgabe**Geben Sie "Testoutput". Containernamen müssen unterschiedlich sein. Stellen Sie sicher, dass dieser Container vorhanden ist.     
8.  Klicken Sie auf **Weiter** um die Ausgabe **Serialisierungseinstellungen**konfigurieren. Mit **Input**auf **CSV**und klicken Sie dann auf die Schaltfläche **OK** .
9.  Klicken Sie auf der Registerkarte **Funktionen** **Hinzufügen Lernfunktion Computer**.  

    ![Stream Analytics maschinelles lernen, fügen Sie Computer lernen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10. Suchen Sie auf der Seite **Computer Learning Web Service Settings** maschinelles lernen Workspace, Webdienst und Standardendpunkt. In diesem Artikel die Einstellungen Sie manuell zu Vertrautheit mit einen Webdienst keinem Arbeitsbereich kennen die URL und den API-Schlüssel konfigurieren. Geben Sie die Endpunkt- **URL** und **API-Schlüssel**. Klicken Sie auf **OK**.    

    ![Stream Analytics maschinelles lernen Machine Learning-Webdienst](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. Ändern Sie auf der Registerkarte **Abfrage** die Abfrage wie folgt:    

 ```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
 
    Select text, result.[Scored Labels]  
    Into output  
    From subquery  
 ```    
12. Klicken Sie auf **Speichern** , um die Abfrage zu speichern.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Der Stream Analytics-Auftrag gestartet und beobachten Sie die Ausgabe

1.  Klicken Sie am unteren Rand der Seite Auftrag **Starten** .
2.  **Abfragedialogfeld**klicken Sie auf **Benutzerdefiniert**, und wählen Sie eine Uhrzeit vor CSV auf BLOB-Speicher hochgeladen. Klicken Sie auf **OK**.  

    ![Stream Analytics Computerlernen, benutzerdefinierte Uhrzeit](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  BLOB-Speicher mithilfe des Tools die CSV-Datei, z. B. Visual Studio verwendet werden.
4.  Minuten nachdem der Auftrag gestartet wurde, Ausgabe Container erstellt und eine CSV-Datei werden hochgeladen.  
5.  Öffnen Sie die Datei im CSV-Standardeditor. Ähnlich der folgenden sollte angezeigt:  

    ![Stream Analytics maschinelles lernen, CSV anzeigen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Abschluss

Dieser Artikel beschreibt einen Stream Analytics-Auftrag erstellen, der Textdaten liest und Daten in Echtzeit für Stimmung Analytics. All dies erreichen ohne Komplexität der ein Gefühl Analytics Modell kümmern. Dies ist einer der Vorteile von Cortana Intelligence Suite.

Sie können auch Azure Machine Learning funktionsbezogene Metriken anzeigen. Klicken Sie dazu auf die Registerkarte **Monitor** . Drei funktionsbezogene Metriken werden angezeigt.  

- **Anfragen Funktion** gibt die Anzahl der Anfragen an einen Computer Learning-Webdienst.  
- **Funktion Ereignisse** gibt die Anzahl der Ereignisse in der Anforderung. Standardmäßig enthält jede Anforderung an einen Webdienst maschinelles lernen bis zu 1.000 Ereignisse.  
- **Fehler bei Funktion Anfragen** gibt die Anzahl der fehlgeschlagenen Anfragen an den Webdienst maschinelles lernen.  

    ![Stream Analytics maschinelles lernen, maschinelles lernen Monitor anzeigen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  
