<properties
    pageTitle="Importieren von Daten in Machine Learning Studio aus einer lokalen Datei | Microsoft Azure"
    description="Zum Importieren von Trainingsdaten Azure Machine Learning Studio aus einer lokalen Datei."
    keywords="Importieren von Daten, Datenformat, Datentypen, Datenquellen, Daten"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Importieren von Trainingsdaten in Azure Machine Learning Studio aus einer lokalen Datei

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Um Ihre Daten in Machine Learning Studio verwenden, können Sie eine Datendatei vor der Zeit von der lokalen Festplatte ein Dataset Modul im Arbeitsbereich erstellt hochladen. 


## <a name="import-data-from-a-local-file"></a>Importieren von Daten aus einer lokalen Datei

Daten können von einer Festplatte wie folgt:

1. Klicken Sie auf **+ neu** am unteren Fensterrand Machine Learning Studio.
2. **DATASET** und **von lokale Datei**auswählen
3. Klicken Sie im Dialogfeld **Neues Dataset laden** suchen Sie die Datei hochladen möchten
4. Geben Sie einen Namen, den Datentyp und optional eine Beschreibung. Eine empfohlene - können alle Merkmale der Daten aufzeichnen, die Sie beachten, wenn die Daten verwenden möchten.
5. **Dies ist die neue Version eines vorhandenen Datasets** Kontrollkästchen können Sie ein vorhandenes Dataset mit neuen Daten aktualisieren. Markieren Sie diese Option, und geben Sie den Namen eines vorhandenen Datasets.

Beim Hochladen sehen Sie eine Meldung, dass die Datei hochgeladen wird. Uploaden Zeit hängt von der Größe Ihrer Daten und der Geschwindigkeit der Verbindung zum Dienst.
Wenn Sie, dass die Datei lange dauern wissen, möglich anderem Computer Learning Studio während Sie warten. Jedoch verursacht Schließen des Browsers Datenupload Fehler.

Nachdem die Daten in einem Dataset Modul gespeichert und steht für jedes Experiment im Arbeitsbereich.
Beim Versuch bearbeiten, finden Sie die Datensätze in der Liste **Eigene Datasets** in der Liste **Gespeicherte Datensätze** in der Modul-Palette erstellten. Sie können Drag & drop das Dataset auf der Leinwand Experiment um die Daten für weitere Analysen und maschinelles lernen verwenden möchten.



