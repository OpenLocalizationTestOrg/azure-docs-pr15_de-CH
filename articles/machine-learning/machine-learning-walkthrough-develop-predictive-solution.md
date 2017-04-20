<properties
    pageTitle="Eine vorbeugende Lösung für Kreditrisiko mit Computer | Microsoft Azure"
    description="Eine ausführliche exemplarische Vorgehensweise zum Erstellen einer Lösung Vorhersageanalytik für Gutschrift Risikoanalyse in Azure Machine Learning Studio."
    keywords="Kreditrisiko, Vorhersageanalytik Lösung, Risikoanalyse"
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
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Exemplarische Vorgehensweise: Entwickeln einer Lösung Vorhersageanalytik für Gutschrift Risikoanalyse in Azure Machine Learning

Angenommen Sie müssen basierend auf den Informationen in einer Anwendung geben sie eine einzelne Kreditrisiko Vorhersagen.  

Einer Risikoanalyse ist ein komplexes Problem, aber wir etwas vereinfachen die Parameter der Frage. Dann können wir so wie Microsoft Azure Machine Learning Machine Learning Studio mit Machine Learning-Webdienst verwenden, um eine solche Vorhersageanalytik Lösung erstellen.  

Im folgenden detaillierten folgen wir der Vorhersageanalytik Modelle in Machine Learning Studio entwickeln und dann als Azure Machine Learning Web Service bereitstellen. Wir beginnen mit Daten öffentlich verfügbare Kredit, entwickeln und Schulen ein Vorhersagemodell anhand der Daten, und dann Bereitstellen des Modells als Webdienst für Gutschrift Bewertung von anderen verwendet werden kann.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

>[AZURE.TIP] Zum Downloaden und Drucken ein Diagramm, das eine über die Funktionen von Machine Learning Studio Übersicht anzeigen Sie [Übersichtsdiagramm Azure Machine Learning Studio Funktionen](machine-learning-studio-overview-diagram.md)

Erstellen Sie eine Gutschrift Risiko Lösung werden wir die folgenden Schritte ausführen:  

1.  [Erstellen eines Arbeitsbereichs maschinelles lernen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Vorhandene Daten](machine-learning-walkthrough-2-upload-data.md)
3.  [Erstellen Sie einen neuen Test](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Schulen und Auswerten der Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Den Web Service bereitstellen](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

Dieser exemplarischen Vorgehensweise basiert auf eine vereinfachte Version der [binäre klassifiziert: Gutschrift Risiko Vorhersage](http://go.microsoft.com/fwlink/?LinkID=525270) Beispiel Experiment [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).
