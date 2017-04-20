<properties
    pageTitle="Schritt 2: Upload von Daten in einem Experiment maschinelles lernen | Microsoft Azure"
    description="Schritt 2 entwickeln eine vorbeugende Lösung Exemplarische Vorgehensweise: Upload Daten in Azure Machine Learning Studio gespeichert."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Schritt 2 der exemplarischen Vorgehensweise: Versuch Azure Machine Learning hochladen Sie vorhandene Daten

Dies ist der zweite Schritt der exemplarischen Vorgehensweise [entwickeln eine Lösung Vorhersageanalytik Azure maschinelles lernen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs maschinelles lernen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Vorhandene Daten**
3.  [Erstellen Sie einen neuen Test](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Schulen und Auswerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Den Web Service bereitstellen](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

----------

Entwickeln Sie ein Vorhersagemodell Kreditrisiko benötigen wir Daten, mit denen wir trainieren und Testen des Modells. Für diese exemplarische Vorgehensweise verwenden wir "UCI Statlog (deutsche Kreditdaten)-Datensatz" aus dem Repository UCI maschinelles lernen. Sie finden sie hier:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.uci.edu/ml/Datasets/Statlog+(German+Credit+Data)</a>

Wir verwenden die Datei mit dem Namen **german.data**. Downloaden Sie diese Datei auf Ihre Festplatte.  

Dieses Dataset enthält Zeilen mit 20 Variablen für 1000 letzten beantragt haben. Diese 20 Variablen darstellen des Datasets Feature Vektor, der Eigenschaften für jede Gutschrift Antragsteller bereitstellt. Eine zusätzliche Spalte in jeder Zeile stellt der Antragsteller berechneten Kreditrisiko mit 700 Bewerber ein geringes Kreditrisiko und 300 als hohes Risiko identifiziert.

UCI-Website enthält eine Beschreibung der Attribute des Vektors Feature Finanzdaten, Kredit-Geschichte, Beschäftigungsstatus und persönliche Informationen. Für jeden Anmelder eine binäre Bewertung wurde, ob sie einen niedrigen sind bestimmte oder hohe Kreditrisiko.  

Wir verwenden diese Daten zum Trainieren eines Modells Vorhersageanalytik. Wenn wir fertig sind, sollen unser Modell Informationen für neue Benutzer und ob sind niedrig oder hoch Kreditrisiko Vorhersagen.  

Hier ist eine interessante Drehung. Beschreibung des Datasets erklärt, dass eine Person sicherzustellen, wie ein geringes Kreditrisiko wird tatsächlich ein hohes Kreditrisiko 5 Mal teurer an die Bank als geringes Kreditrisiko hoch sicherzustellen. Eine einfache Möglichkeit, im Experiment berücksichtigen wird durch Duplizieren (5 Mal) die Einträge, die eine Person mit einem hohen Kreditrisiko darstellen. Dann, wenn das Modell ein hohe Kreditrisiko niedrig fälschlicherweise, es diese falschen Klassifizierung 5 Mal einmal für jeden doppelten. Dies erhöht die Kosten dieser Fehler in den trainingsergebnissen.  

##<a name="convert-the-dataset-format"></a>Dataset-Format konvertieren
Das ursprüngliche Dataset wird ein Leerzeichen als Trennzeichen verwendet. Machine Learning Studio funktioniert besser mit einer Datei durch Trennzeichen getrennten Werten (CSV), damit wir konvertieren Dataset Leerzeichen durch Kommas ersetzt werden.  

Es gibt viele Methoden, um diese Daten konvertieren. Eine Möglichkeit ist mit dem folgenden Windows PowerShell-Befehl:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Wird mit dem Unix-Sed-Befehl:  

    sed 's/ /,/g' german.data > german.csv  

In beiden Fällen haben wir eine durch Trennzeichen getrennte Daten in eine Datei namens **german.csv** , die wir im Experiment verwenden.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>Das Dataset auf Machine Learning Studio hochladen

Nach dem Konvertieren der Daten in das CSV-Format müssen wir lernen in Computer hochladen. Weitere Informationen zum Einstieg in Machine Learning Studio finden Sie unter [Microsoft Azure Machine Learning Studio-Startseite](https://studio.azureml.net/).

1.  Machine Learning Studio ([https://studio.azureml.net](https://studio.azureml.net)) zu öffnen. Gefragt Anmelden verwenden Sie als Besitzer des Arbeitsbereichs angegebene Konto.
1.  Klicken Sie auf der Registerkarte **Studio** am oberen Fensterrand.
1.  Klicken Sie auf **+ neu** am unteren Rand des Fensters.
1.  **DATASET**auswählen
1.  Wählen Sie **lokale Datei aus**.
1.  Im Dialogfeld **Uploaden Sie ein neues Dataset** auf **Durchsuchen** , und suchen Sie die Datei **german.csv** , die Sie erstellt.
1.  Geben Sie einen Namen für das Dataset. In dieser exemplarischen Vorgehensweise werden sie "UCI Deutsch Kreditkartendaten" aufgerufen werden.
1.  Wählen Sie für Daten **ohne Header generischer CSV-Datei (. nh.csv)**.
1.  Fügen Sie eine Beschreibung möchten hinzu.
1.  Klicken Sie auf **OK**.  

![Laden Sie das dataset][1]  


Dadurch lädt die Daten in ein Dataset-Modul, die wir in einem Experiment verwenden können.

> [AZURE.TIP] Um Studio hochladen Datasets zu verwalten, klicken Sie auf die Registerkarte **DATASETS** im Studio links.

Weitere Informationen zum Importieren von verschiedenen Datentypen in einem Experiment finden Sie unter [Import Trainingsdaten in Azure Machine Learning Studio](machine-learning-data-science-import-data.md).

**Weiter: [Erstellen Sie einen neuen Test](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
