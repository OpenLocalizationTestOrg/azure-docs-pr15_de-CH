<properties 
    pageTitle="Was ist Azure Machine Learning Studio? | Microsoft Azure"
    description="Übersicht über Azure ML Studio ein Drag & Drop-Tool für das schnelle Erstellen Modelle aus einer Bibliothek bereit verwenden Algorithmen und Module."
    keywords="Azure maschinelles lernen, Azure ml ml studio"
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
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Was ist Azure Machine Learning Studio?

Microsoft Azure Machine Learning Studio ist eine Zusammenarbeit, Drag & Drop Tool, erstellen, testen und Bereitstellen Vorhersageanalytik Lösungen für Ihre Daten. Machine Learning Studio veröffentlicht Modelle als Webdienste einfach benutzerdefinierte apps oder BI-Tools wie Excel genutzt werden können.

Machine Learning Studio trifft sich datenwissenschaft, Vorhersageanalysen Cloudressourcen und Daten.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Interaktive Machine Learning Studio-Arbeitsbereich

Entwickeln Sie ein Modell Vorhersageanalysen Sie normalerweise verwenden von Daten aus einer oder mehreren Quellen transformieren und Analysieren dieser Daten über verschiedene Datenmanipulation und statistische Funktionen und Ergebnisse generieren. Entwickeln ein Modell ist ein iterativer Prozess. Verschiedene Funktionen und deren Parameter ändern, bis Sie zufrieden sind die Ergebnisse konvergieren geschulte, effektive Modell haben.

**Azure Machine Learning Studio** bietet einen interaktiven, visuellen Arbeitsbereich problemlos erstellen, testen und Vorhersageanalysen Modell durchlaufen. Sie ziehen und Ablegen ***Datasets*** und Analyse ***Module*** auf eine interaktive Leinwand verbinden zu einer ***experimentieren***, die in Machine Learning Studio ausgeführt. Um die Modellkonstruktion Durchlaufen des Experiments bearbeiten, eine Kopie ggf. speichern und erneut ausführen. Wenn Sie bereit sind, können Sie ***vorhersageexperiment***konvertieren Ihr ***Training experimentieren*** und dann als ***Webdienst*** veröffentlichen, sodass das Modell von anderen zugegriffen werden kann.

>[AZURE.TIP] Zum Downloaden und Drucken ein Diagramm, das eine über die Funktionen von Machine Learning Studio Übersicht anzeigen Sie [Übersichtsdiagramm Azure Machine Learning Studio Funktionen](machine-learning-studio-overview-diagram.md)

Es ist keine Programmierung erforderlich, nur optisch Verbinden von Datasets und Module Vorhersageanalysen Modell erstellen.

![Azure ML Studio Diagramm: Experimente erstellen, Daten für viele bewertete Daten schreiben, Schreiben Sie Modelle.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Erste Schritte mit Machine Learning Studio

Bei der Eingabe von [Machine Learning Studio](https://studio.azureml.net) **die Startseite** angezeigt. Hier können Sie anzeigen, Dokumentation, Videos, Webinare und weitere wertvollen Ressourcen finden.

Drei Registerkarten stehen oben: **Home** (wo Sie beginnen), **Studio**und **Galerie**.

### <a name="studio"></a>Studio

Klicken Sie auf die Registerkarte **Studio** und werden Sie aufgefordert, melden Sie sich mit Ihrem Microsoft-Konto oder Ihr Konto Arbeits- oder Schulcomputer. Nach der Anmeldung sehen Sie links die folgenden Registerkarten:

- **Projekte** - Sammlungen Versuche, Datasets, Notebooks und anderen Ressourcen eines einzelnen Projekts
- **EXPERIMENTE** - Experimente, die erstellt, ausgeführt und als Entwürfe gespeichert
- **Webdienste** - aus der bereitgestellten Webdienste
- **NOTEBOOKS** - Jupyter Notebooks, die Sie erstellt haben
- **DATASETS** - Datasets in Studio hochgeladen
- **GESCHULTE Modelle** - Modelle die Experimente geschult und im Studio gespeichert
- **SETTINGS** - Einstellungen, mit denen Sie Ihr Konto und Ressourcen konfigurieren.

### <a name="gallery"></a>Galerie

Klicken Sie auf die Registerkarte **Katalog** und Cortana Intelligence Katalog geleitet. Die Galerie ist ein Ort, wo eine Community von datenwissenschaftler und Entwickler Lösungen Komponenten Cortana Intelligence Suite kann.

Weitere Informationen über die Galerie finden Sie [freigeben und die Intelligenz Website Cortana](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Komponenten von Hypothesen

Versuch besteht aus Datasets, die Daten analytische Module bereitstellen verbinden um Vorhersageanalysen Modell zu konstruieren. Eine gültige Experiment hat insbesondere Merkmale:

- Das Experiment hat mindestens ein Dataset und einem Modul
- Datasets können nur mit Modulen verbunden werden
- Module können Datasets oder andere Module angeschlossen werden
- Alle Eingänge für Module muss eine Verbindung zu Daten
- Alle erforderlichen Parameter für jedes Modul müssen festgelegt werden

Erstellen ein Experiment völlig oder Versuch vorhandene Beispiel als Vorlage verwenden. Weitere Informationen finden Sie unter [verwenden Beispiel Experimente neue Tests erstellen](machine-learning-sample-experiments.md).

Ein Beispiel zum Erstellen eines einfachen Experiments finden Sie unter [Erstellen eines einfachen Experiments in Azure Machine Learning Studio](machine-learning-create-experiment.md).

Mehr umfassende Vorhersageanalysen Projektmappe erstellen finden Sie unter [entwickeln eine vorbeugende Lösung mit Azure maschinelles lernen](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Datasets

Ein Dataset ist Daten in Machine Learning Studio hochgeladen wurde, damit sie bei der Modellierung verwendet werden kann. Anzahl der Beispiel-Datasets gehören Machine Learning Studio Sie experimentieren und weitere Datensätze nach Bedarf laden. Hier sind einige Beispiele für Datasets enthalten:

- **Daten für verschiedene Autos MPG** - Meilen pro Gallone (MPG) für Automobile, die Anzahl der Zylinder, Leistung usw. identifiziert.
- **Brust Krebs Daten** - Brust Krebs Diagnosedaten.
- **Gesamtstruktur mit Daten** - Gesamtstruktur Feuer Größen im Nordosten Portugal.

Erstellen von Hypothesen können Sie aus der Liste der verfügbaren Datasets zum linken Rand der Leinwand.

Eine Liste der Beispiel-Datasets in Machine Learning Studio enthalten finden Sie unter [Beispieldatensätzen in Azure Machine Learning Studio verwenden](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Module

Ein Modul ist ein Algorithmus, den auf die Daten möglich. Machine Learning Studio hat eine Reihe von Modulen von Funktionen eingehende Schulung, Bewertung und validierungsprozesse. Einige Beispiele enthalten Module:

- [Konvertieren in ARFF] [ convert-to-arff] -Dataset .NET serialisiert, Attribut Beziehung Datei Format (ARFF) konvertiert.
- [Elementare Statistiken berechnen] [ elementary-statistics] -elementare Statistiken wie Mittelwert, Standardabweichung berechnet.
- [Lineare Regression] [ linear-regression] -ein Online-Farbverlauf Abstieg basierende lineare Regressionsmodell erstellt.
- [Bewertungspunktestand Modell] [ score-model] -eine ausgebildete Klassifizierung oder Regression Modell bewertet.

Erstellen von Hypothesen können Sie aus der Liste der verfügbaren Module zum linken Rand der Leinwand.  

Ein Modul möglicherweise eine Reihe von Parametern, mit denen Sie das Modul interner Algorithmen konfigurieren. Wenn Sie ein Modul auf der Leinwand auswählen, werden die Modulparameter im **Eigenschaftenfenster neben der Leinwand** angezeigt. Sie können die Parameter in diesem Bereich, das Modell zu optimieren.

Einige Hilfe navigieren durch die große Bibliothek verfügbaren Computer lernalgorithmen finden Sie unter [Algorithmen für Microsoft Azure Computer auswählen](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Einen Webdienst Vorhersageanalysen bereitstellen

Wenn Vorhersageanalytik Modell fertig ist, können Sie es als Webdienst von Machine Learning Studio bereitstellen. Weitere Informationen zu diesem Vorgang finden Sie unter [Bereitstellen einer Azure Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
