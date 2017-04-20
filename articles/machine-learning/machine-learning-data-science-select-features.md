<properties
    pageTitle="Featureauswahl beim Team wissenschaftliche Daten | Microsoft Azure" 
    description="Erläutert den Zweck von Featureauswahl und Beispiele für ihre Rolle bei der Verbesserung von Daten der computerlernen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Featureauswahl im Team Daten Wissenschaft Prozess (TDSP)

Dieser Artikel beschreibt die Zwecke Featureauswahl und seiner Rolle in der Erweiterung Daten Computer lernen Beispiele. Diese Beispiele stammen aus Azure Machine Learning Studio. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


In diesem Thema erläutert den Zweck von Featureauswahl und seiner Rolle in der Erweiterung Daten Computer lernen Beispiele. Diese Beispiele stammen aus Azure Machine Learning Studio. 

Engineering und Funktionen ist ein Teil der TDSP gemäß den [Neuigkeiten Team Daten Wissenschaft?](data-science-process-overview.md). Feature Engineering und Auswahl sind Bestandteile des **Features entwickeln** Teil der TDSP.

* **Featureentwicklung**: Dieser Vorgang versucht, zusätzliche relevante Features aus vorhandenen raw Features in den Daten erstellen, und vorbeugende Leistungssteigerung Learning-Algorithmus.

* **Featureauswahl**: dabei wählt wichtige Teilmenge der ursprünglichen Datenfeatures versucht zu Dimensionalität Training-Problem.

Normalerweise wird **Featureentwicklung** zuerst um zusätzliche Funktionen und der **Featureauswahl** Schritt wird beseitigen irrelevante, redundante oder stark korrelierte Funktionen ausgeführt.


## <a name="filtering-features-from-your-data---feature-selection"></a>Filterfunktionen aus den Daten - Feature-Auswahl 

Featureauswahl ist ein Vorgang, der häufig für den Bau Training Datasets Vorhersagemodellen Aufgaben wie Klassifizierung oder Regression Aufgaben angewendet wird. Das Ziel ist eine Teilmenge der Funktionen aus dem ursprünglichen Dataset, die die Dimensionen reduzieren mit einem minimalen Satz von Funktionen die maximale Abweichung in den Daten dargestellt auswählen. Dieser Teil der Features sind dann nur Funktionen zum Trainieren des Modells enthalten sein. Featureauswahl dient zwei Hauptfunktionen.

* Zunächst Featureauswahl häufig erhöht die Klassifizierung Genauigkeit durch Eliminierung irrelevant, redundante und Korrelation Funktionen.
* Zweitens verringert die Anzahl der Funktionen, die Trainingsvorgang Modell effizienter gestaltet. Dies ist besonders wichtig für Lernende teuer wie Support Vector trainieren.

Obwohl Featureauswahl versucht, weniger Features im Dataset zum Trainieren des Modells verwendet, wird es nicht in der Regel durch den Begriff "Dimensionalität Reduzierung" bezeichnet. Featureauswahlmethoden extrahieren einen Teil der ursprünglichen Features in den Daten zu.  Dimensionalität Methoden einsetzen entwickelte Features, die im ursprünglichen KE umwandeln und so ändern können. Dimensionalität Methoden gehören Hauptkomponentenanalyse, kanonische korrelationsanalyse und einzigartigen Wert Zersetzung.

Unter anderem wird eine häufig angewendete Kategorie featureauswahlmethoden betreute dabei "basierenden Filter Featureauswahl" aufgerufen. Auswerten der Korrelations zwischen jede Funktion und das Zielattribut gelten diese Methoden statistisches Maß um jede Funktion einen Wert zuzuweisen. Die Features sind dann die Bewertung Platz die Hilfe den Schwellenwert beibehalten oder entfernen ein bestimmtes Feature verwendet werden kann. Diese Methoden der statistischen Maßnahmen gehören Person Korrelation Informationen und Chi-Quadrat-Test.

In Azure Machine Learning Studio sind Module Featureauswahl. Wie in der folgenden Abbildung gezeigt, enthalten diese Module [filterbasierte Featureauswahl] [ filter-based-feature-selection] und [Fisher lineare Diskriminanzanalyse][fisher-linear-discriminant-analysis].

![Beispiel für KE-Auswahl](./media/machine-learning-data-science-select-features/feature-Selection.png)


Betrachten Sie zum Beispiel die Verwendung der [Filter - Funktionsauswahl] [ filter-based-feature-selection] Modul. Zur Vereinfachung weiterhin Text Mining Beispiel genannten. Wir möchten ein Regressionsmodell erstellen, nachdem eine Reihe von 256 Funktionen entstehen durch das [Feature Hashing] [ feature-hashing] Modul, und die Variable Response "SP1" und stellt ein Buch lesen Bewertung von 1 bis 5. Mit "Features Bewertungsmethode" "Pearson Korrelation" der Spalte"Ziel", "SP1" und die "Anzahl der gewünschten Features" 50. Dann das Modul [filterbasierte Featureauswahl] [ filter-based-feature-selection] ergibt ein Dataset mit 50 Funktionen mit Zielattribut "SP1". Die folgende Abbildung zeigt den Fluss dieses Experiments und Eingabeparameter genau beschrieben.

![Beispiel für KE-Auswahl](./media/machine-learning-data-science-select-features/feature-Selection1.png)

Die folgende Abbildung zeigt die resultierende Datasets. Jede Funktion ist Grundlage Pearson Korrelation zwischen sich selbst und das Zielattribut "SP1" bewertet. KEs mit Bestnoten bleiben.

![Beispiel für KE-Auswahl](./media/machine-learning-data-science-select-features/feature-Selection2.png)

Die entsprechenden Werte der ausgewählten Features sind in der folgenden Abbildung dargestellt.

![Beispiel für KE-Auswahl](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Durch Anwenden dieser [Filter Featureauswahl] [ filter-based-feature-selection] Modul 50 von 256 Features ausgewählt sind, da sie die korrelierten Funktionen mit dem Ziel "SP1" anhand der Bewertungsmethode "Pearson Korrelation".

## <a name="conclusion"></a>Abschluss
Feature Engineering und Featureauswahl sind zwei häufig entwickelt und ausgewählten Features Effizienz Trainingsprozess versucht, die Informationen extrahieren Daten enthielt. Sie verbessern auch macht diese Modelle genau die eingegebenen Daten klassifizieren und Ergebnisse an stabiler vorherzusagen. Feature Engineering und Auswahl können zu lernen rechenintensiver vermitteln auch kombinieren. Dies geschieht und reduzieren die Anzahl der Funktionen Kalibrieren oder ein Modell. Mathematisch sind die Funktionen zum Trainieren des Modells aktiviert einen Mindestsatz von unabhängigen Variablen, die Muster in den Daten erklären und Vorhersagen Ergebnisse erfolgreich.

Beachten Sie, dass es nicht immer unbedingt Featureauswahl Engineering oder Funktion ausführen. Oder nicht erforderlich ist, hängt davon ab Daten, die wir oder sammeln, Algorithmus wir holen und das Ziel des Experiments.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
 
