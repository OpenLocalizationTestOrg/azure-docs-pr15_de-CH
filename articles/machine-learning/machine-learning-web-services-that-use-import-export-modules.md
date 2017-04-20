<properties
    pageTitle="Bereitstellen von Azure ML Webdienste, Datenimport- und Datenexport Module verwenden | Microsoft Azure"
    description="Erfahren Sie, wie die Module Daten importieren und Exportieren von Daten senden und Empfangen von Daten von einem Webdienst verwendet."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="v-donglo"/>



# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Bereitstellen von Azure ML Webdienste, die Datenimport- und Datenexport Module verwenden 

Wenn ein vorhersageexperiment erstellen, fügen Sie einen Web Service ein- und Ausgabe ein. Beim Bereitstellen des Experiments können Verbraucher senden und empfangen Daten vom Webdienst über die Eingaben und Ausgaben. Für einige Programme ein Consumer Daten über ein Datenfeed oder befinden sich bereits in einer externen Datenquelle wie Azure BLOB-Speicher. In diesen Fällen müssen sie nicht lesen und Schreiben von Daten mithilfe von Web Service-Eingaben und Ausgaben. Sie können stattdessen mit Batch Execution Service (BES) zum Lesen von Daten aus der Datenquelle mit Daten importieren-Modul und die Bewertungsergebnisse an einen anderen Speicherort mit Daten exportieren-Modul schreiben.

Daten importieren und Exportieren datenmodule können aus lesen und Schreiben auf Daten wie eine URL über HTTP-Hive Abfrage einer Azure SQL-Datenbank, Azure-Tabellenspeicher, Azure BLOB-Speicher Datenfeed oder eine lokale SQL-Datenbank.

Dieses Thema verwendet das "Beispiel 5: Zug Test auswerten binäre Klassifizierung: Erwachsene Dataset" Beispiel und übernimmt das Dataset in einer Azure SQL-Tabelle mit dem Namen Censusdata bereits geladen wurde.

## <a name="create-the-training-experiment"></a>Erstellen des Experiments Schulung 
 
Beim Öffnen der "Beispiel 5: Zug Test auswerten binäre Klassifizierung: Erwachsene Dataset" Beispiel Erwachsenen Erhebung Einnahmen binäre Klassifizierung Stichprobendataset verwendet. Und das Experiment im Bereich in der folgenden Abbildung ähneln.

![Erstkonfiguration des Experiments.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)
  

Zum Lesen der Daten aus der SQL Azure-Tabelle:

1.  Löschen Sie das Dataset-Modul.
2.  Geben Sie im Feld Suchen Komponenten importieren.
3.  Leinwand Experiment aus der Ergebnisliste fügen Sie ein *Datenimport* -Modul hinzu.
4.  Ausgabe des Moduls *Importdaten* Eingaben *säubern fehlt* Moduls anschließen
5.  Wählen Sie im Eigenschaftenbereich **Azure SQL-Datenbank** in der Dropdownliste **Datenquelle** .
6.  Geben Sie in **Servername**, **Datenbankname**, **Benutzername**und **Kennwort** die entsprechenden Informationen für Ihre Datenbank.
7.  Geben Sie im Feld Datenbank Abfrage die folgende Abfrage ein.

        select [age],
           [workclass],
           [fnlwgt],
           [education],
           [education-num],
           [marital-status],
           [occupation],
           [relationship],
           [race],
           [sex],
           [capital-gain],
           [capital-loss],
           [hours-per-week],
           [native-country],
           [income]
        from dbo.censusdata;

8.  Am Ende des Experiments Leinwand, klicken Sie auf **Ausführen**.

## <a name="create-the-predictive-experiment"></a>Vorhersageexperiment erstellen

Anschließend richten Sie vorhersageexperiment aus dem Sie den Web Service bereitstellen.

1.  Am unteren Rand der Leinwand Experiment auf **Web-Service** und wählen Sie **Vorhersehbare Webdienst [empfohlen]**.
2.  Entfernen Sie *Web Service ein-* und *Ausgabe von Modulen* aus der vorhersageexperiment 
3.  Geben Sie im Feld Suchen Komponenten exportieren.
4.  Aus der Ergebnisliste Experiment Leinwand Hinzufügen eines Moduls *Daten exportieren* .
5.  Verbinden Sie Ausgabe des Moduls *Score Model* die Eingabe des Moduls *Exportieren* . 
6.  Wählen Sie im Eigenschaftenbereich **Azure SQL-Datenbank** in der Dropdownliste Daten Ziel.
7.  **Servername**, **Datenbankname**, **Server Benutzerkontonamen**und **Server Benutzerkontokennwort** Felder Geben Sie die entsprechenden Informationen für Ihre Datenbank.
8.  Geben Sie in das Feld **durch Kommas getrennte Liste von Spalten gespeichert werden** Etiketten bewertet.
9.  Geben Sie im **Namensfeld der Datentabelle**Dbo. ScoredLabels. Wenn die Tabelle nicht vorhanden ist, entsteht beim Ausführen des Experiments oder der Webdienst aufgerufen.
10. Geben Sie im Feld **durch Kommas getrennte Liste von Datatable-Spalten** ScoredLabels.

Wenn Sie eine Anwendung, die den endgültigen Webdienst aufruft schreiben, möchten Sie eine andere Eingabeabfrage oder Zieltabelle zur Laufzeit angeben. Die Webdienstparameter-Funktion können Sie konfigurieren diese Eingaben und Ausgaben die Moduleigenschaft *Datenquelle* *Daten importieren* und *Exportieren von Daten* Modus Daten Ziel-Eigenschaft festgelegt.  Weitere Informationen zu Parametern für Web Service finden Sie im [AzureML Webdienstparameter Eintrag](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) Cortana Intelligence und Machine Learning Blog.

Web Service-Parameter für die Abfrage importieren und die Zieltabelle zu konfigurieren:

1.  Im Eigenschaftenbereich Moduls *Importieren* , klicken Sie im oberen Feld **Datenbankabfrage** und **als Web Service Parameter festlegen**.
2.  Im Eigenschaftenbereich Moduls *Exportieren* , klicken Sie oben in das Feld **Name der Tabelle mit Daten** und **als Web Service Parameter festlegen**.
3.  Unteren Eigenschaftenbereich *Daten exportieren* Modul im Abschnitt **Webdienstparameter** auf Datenbankabfrage und Umbenennen Sie Abfrage.
4.  Klicken Sie auf **Name der Datentabelle** und Umbenennen Sie **Tabelle**.

Wenn Sie fertig sind, sollte das Experiment in der folgenden Abbildung ähneln.
 
![Endgültige Version des Experiments.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Jetzt können Sie das Experiment als Web Service bereitstellen.

## <a name="deploy-the-web-service"></a>Den Web Service bereitstellen 
Sie können entweder einen und der neuen Webdienst bereitstellen.

### <a name="deploy-a-classic-web-service"></a>Bereitstellen eines klassischen Webdiensts

Als klassische Webdienst bereitstellen und Erstellen einer Anwendung zu verwenden:

1.  Klicken Sie am unteren Rand der Leinwand experimentieren.
2.  Nach Abschluss die Ausführung auf **Webdienst bereitstellen** und wählen Sie **Webdienst bereitstellen [Classic]**.
3.  Dienst-Dashboard suchen Sie den API-Schlüssel. Kopieren und zur späteren Verwendung speichern.
4.  Klicken Sie in der Tabelle **Standardmäßig Endpunkt** den **Batchausführung** Link API-Hilfeseite.
5.  Erstellen Sie in Visual Studio eine C#.
6.  Die API-Hilfeseite suchen Sie Abschnitt **Beispielcode** am unteren Rand der Seite.
7.  Kopieren Sie und fügen Sie C#-Beispielcode in die Datei Program.cs und entfernen Sie alle Verweise auf BLOB-Speicher.
8.  Aktualisieren Sie den Wert der Variablen *ApiKey* mit zuvor gespeicherten API-Schlüssel.
9.  Der Anforderung gesucht und Werte der Webdienstparameter Module *Daten importieren* und *Exportieren von Daten* an. In diesem Fall werden Sie die ursprüngliche Abfrage verwendet jedoch eine neue Tabelle namens definieren.

        var request = new BatchExecutionRequest() 
        {   
            GlobalParameters = new Dictionary<string, string>() {
            { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
            { "Table", "dbo.ScoredTable2" },
            }
        };

10. Führen Sie die Anwendung. 

Nach Abschluss der Ausführung wird eine neue Tabelle der Datenbank die Bewertungsergebnisse hinzugefügt.

### <a name="deploy-a-new-web-service"></a>Einen neuen Webdienst bereitstellen

Als neue Webdienst bereitstellen und Erstellen einer Anwendung zu verwenden:

1.  Am Ende des Experiments Leinwand, klicken Sie auf **Ausführen**.
2.  Nach Abschluss die Ausführung auf **Webdienst bereitstellen** , und wählen Sie **[New] Webdienst bereitstellen**.
3.  Geben Sie auf der Seite Bereitstellung Experiment einen Namen für den Webdienst und einen Zahlungsplan, klicken Sie auf **Bereitstellen**.
4.  Klicken Sie auf der Seite **Schnellstart** auf **verbrauchen**.
5.  Klicken Sie im Abschnitt **Beispielcode** auf **Stapel**.
6.  Erstellen Sie in Visual Studio eine C#.
7.  Kopieren Sie und fügen Sie den C#-Beispielcode in die Datei Program.cs.
8.  Aktualisieren Sie den Wert der Variablen *ApiKey* mit dem **Primärschlüssel** im Abschnitt **grundlegende Verbrauch** .
9.  Suchen Sie die Deklaration von *ScoreRequest* und Werte der Webdienstparameter Module *Daten importieren* und *Exportieren von Daten* an. In diesem Fall werden Sie die ursprüngliche Abfrage verwendet jedoch eine neue Tabelle namens definieren.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                 { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };

10. Führen Sie die Anwendung. 
 

