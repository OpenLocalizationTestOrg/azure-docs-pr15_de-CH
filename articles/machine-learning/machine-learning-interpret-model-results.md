<properties
    pageTitle="Interpretieren der Ergebnisse für maschinelles lernen | Microsoft Azure"
    description="Auswählen den optimalen Parameter für Algorithmus verwendet und Visualisierung Bewertung Modell gibt."
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
    ms.date="09/12/2016"
    ms.author="bradsev" />


# <a name="interpret-model-results-in-azure-machine-learning"></a>Interpretieren der Ergebnisse in Azure Machine Learning

Dieses Thema erläutert visualisieren und Vorhersageergebnisse in Azure Machine Learning Studio interpretiert. Nachdem ein Modell geschult und Vorhersagen darüber ("erzielte das Modell") durchgeführt müssen Sie interpretieren Vorhersageergebnis.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Es gibt vier wichtige Arten Machine learning Modelle in Azure Machine Learning:

* Klassifizierung
* Clustering
* Regression
* Empfehlungssysteme

Die Module zur Vorhersage auf diese Modelle sind:

* [Bewertungspunktestand Modell] [ score-model] für Klassifizierung und Regression
* [Zuweisen von Clustern] [ assign-to-clusters] Modul für clustering
* [Bewertungspunktestand Matchbox Recommender] [ score-matchbox-recommender] Empfehlung Systeme

Dieses Dokument erläutert die Vorhersageergebnisse für jedes dieser Module interpretiert. Eine Übersicht über diese Module finden Sie unter [Parameter optimieren Sie die Algorithmen in Azure Machine Learning verwenden möchten](machine-learning-algorithm-parameters-optimize.md).

Dieses Thema behandelt Vorhersage Interpretation jedoch nicht auswerten. Weitere Informationen dazu, wie Sie die Auswertung finden Sie unter [modellleistung in Azure Machine Learning zu bewerten](machine-learning-evaluate-model-performance.md).

Wenn Sie neue Azure maschinelles lernen und Hilfe zum Erstellen eines einfachen Experiments Einstieg, finden Sie unter [Erstellen eines einfachen Experiments in Azure Machine Learning Studio](machine-learning-create-experiment.md) in Azure Machine Learning Studio.

## <a name="classification"></a>Klassifizierung ##
Es gibt zwei Unterkategorien Klassifizierung Probleme:

* Probleme mit nur zwei Klassen (zwei Klassen oder binäre Klassifizierung)
* Probleme mit mehr als zwei Klassen (Klassifizierung Multi-Klasse)

Azure Machine Learning hat unterschiedliche Module mit jeder dieser Typen der Einstufung jedoch Methoden interpretieren die Vorhersageergebnisse ähneln.

### <a name="two-class-classification"></a>Zwei Klassen Klassifizierung###
**Beispiel-experiment**

Ein Beispiel für eine Klasse zwei Klassifizierung ist die Klassifizierung des Iris Blumen. Die Aufgabe ist Iris Blumen anhand ihrer Features zu klassifizieren. Die Iris Daten bereitgestellt in Azure Machine Learning eine Teilmenge des gängigen [Iris DataSet](http://en.wikipedia.org/wiki/Iris_flower_data_set) Instanzen nur zwei Arten (Klassen 0 und 1) enthält. Es gibt vier Funktionen für jede Blume (kelchblatt Länge, kelchblatt Breite, sodass Länge und Breite des Blatts).

![Screenshot der Iris experiment](./media/machine-learning-interpret-model-results/1.png)

Abbildung 1. Iris zwei Klassen Klassifizierung Problem experiment

Versuch wurde zur Lösung dieses Problems ausgeführt, wie in Abbildung 1 dargestellt. Eine Klasse zwei stärkere Entscheidungsbaummodell hat geschult und bewertet. Jetzt die Vorhersageergebnisse [Bewertung Modell] visualisieren können[ score-model] Modul [Score Model] -Ausgangs-Port auf[ score-model] Modul und **visualisieren**klicken.

![Bewertung Modell Modul](./media/machine-learning-interpret-model-results/1_1.png)

Dadurch wird der Bewertungsergebnisse wie in Abbildung 2 dargestellt.

![Iris zwei Klassen Klassifizierung Versuchsergebnisse](./media/machine-learning-interpret-model-results/2.png)

Abbildung 2. Visualisieren Sie ein Faktor Modellergebnis in zwei Klassen Klassifizierung

**Ergebnisinterpretation**

Es gibt sechs Spalten in der Ergebnistabelle. Die linken vier Spalten sind vier Funktionen. Rechts zwei Spalten, Etiketten bewertet und erzielte Wahrscheinlichkeiten sind die Vorhersageergebnisse. Die Wahrscheinlichkeit bewertet Spalte zeigt die Wahrscheinlichkeit, die eine Blume positive Klasse (Klasse 1) gehört. Beispielsweise ist die erste Zahl in der Spalte (0,028571) bedeutet 0.028571 Wahrscheinlichkeit, dass die erste Blume Klasse 1 gehört. Die Etiketten erzielte Spalte zeigt die vorhergesagte Klasse für jede Blume. Dies basiert auf der Spalte Wahrscheinlichkeiten bewertet. Erzielte eine Blume größer als 0,5 ist, wird als Klasse 1 vorhergesagt. Andernfalls wird es als Klasse 0 vorhergesagt.

**Web Service-Publikation**

Nachdem die Vorhersageergebnisse verstanden und sound beurteilt, kann Experiments als Webdienst veröffentlicht werden, damit Sie in verschiedenen Programmen bereitstellen und Klasse Vorhersagen neuer Iris Blumen zu nennen. Das Ändern eines trainingsexperiments Punktzahl Experiment und als Webdienst veröffentlichen finden Sie unter [Azure Machine Learning-Webdienst veröffentlichen](machine-learning-walkthrough-5-publish-web-service.md). Dieses Verfahren bietet ein bewertungsexperiment wie in Abbildung 3 dargestellt.

![Screenshot des Experiments bewerten](./media/machine-learning-interpret-model-results/3.png)

Abbildung 3. Iris zwei Klassen Klassifizierung Problem Experiment bewerten

Jetzt müssen Sie die Eingabe und Ausgabe für den Webdienst festlegen. Die Eingabe ist rechts Eingabeanschluss [Bewertung Modell][score-model], ist die Iris Blume Eingabe bietet. Die Wahl der Ausgabe hängt davon ab, ob Sie die vorhergesagte Klasse (erzielte Label) oder der ausgewerteten Wahrscheinlichkeit interessiert sind. In diesem Beispiel wird davon ausgegangen, dass Sie beide interessiert sind. Wählen Sie die gewünschten Spalten verwenden, [Wählen Sie Spalten im DataSet] [ select-columns] Modul. [Wählen Sie]Spalten im DataSet[select-columns] **Spaltenauswahl starten**und auf **Etiketten bewertet** und **Erzielte Wahrscheinlichkeiten**auswählen. Nach dem Festlegen der Ausgabeanschluss der [Spalten im Dataset auswählen] [ select-columns] und erneut ausführen, Sie sollten das bewertungsexperiment als Webdienst veröffentlichen, indem Sie auf **Webdienst veröffentlichen**. Der letzte Versuch sieht wie in Abbildung 4.

![Iris zwei Klassen Klassifizierung experiment](./media/machine-learning-interpret-model-results/4.png)

Abbildung 4. Letzte bewertungsexperiment einer Iris zwei Klassen Klassifizierung Problem

Nach der Ausführung des Webdiensts und einige Features Werte für eine Testinstanz gibt das Ergebnis zwei Zahlen. Die erste Zahl ist die erzielte Bezeichnung und der zweite ausgewerteten Wahrscheinlichkeit. Diese Blume wird als Klasse 1 mit 0.9655 Wahrscheinlichkeit vorhergesagt.

![Interpretation Bewertung Modell](./media/machine-learning-interpret-model-results/4_1.png)

![Bewerten der Testergebnisse](./media/machine-learning-interpret-model-results/5.png)

Abbildung 5. Web Service aus Iris zwei Klassen Klassifizierung

### <a name="multi-class-classification"></a>Multi-Klasse Klassifikation
**Beispiel-experiment**

In diesem Versuch Aufgaben Sie Buchstaben Anerkennung als Beispiel multiklassenklassifizierung. Klassifizierung versucht, einen bestimmten Buchstaben (Klasse) basierend auf einige handgeschriebenen Attributwerte extrahiert aus handgeschriebenen vorherzusagen.

![Buchstabe Anerkennung wird](./media/machine-learning-interpret-model-results/5_1.png)

In den Trainingsdaten sind 16 Funktionen handgeschriebenen Buchstaben Bilder entnommen. 26 Buchstaben Formular 26 Klassen. Abbildung 6 zeigt ein Experiment, das ein multiklassenklassifizierung Modell Buchstaben Anerkennung trainieren und Vorhersagen auf dieselben Features auf einem Test.

![Buchstabe Recognition multiklassenklassifizierung experiment](./media/machine-learning-interpret-model-results/6.png)

Abbildung 6. Buchstabe Recognition multiklassenklassifizierung Problem experiment

Die Ergebnisse der [Bewertung Modell] visualisieren[ score-model] Modul auf die Ausgabe der [Bewertung Modell] [ score-model] Modul klicken und **visualisieren**, müsste Inhalt wie in Abbildung 7 dargestellt.

![Modell Resultat](./media/machine-learning-interpret-model-results/7.png)

Abbildung 7. Modell Resultat in einer Multi-Klasse Klassifikation visualisieren

**Ergebnisinterpretation**

Die linken 16 Spalten Funktionswerte festgelegten Test. Spalten mit Namen wie erzielte Wahrscheinlichkeiten für Klasse "XX" genau wie Spalte Wahrscheinlichkeiten erzielte bei zwei Klassen. Sie zeigen die Wahrscheinlichkeit, die der entsprechende Eintrag in einer bestimmten Klasse liegt. Für den ersten Eintrag ist beispielsweise 0,003571 Wahrscheinlichkeit ist ein "A" 0.000451 Wahrscheinlichkeit, dass es eine "B" und So weiter. Die letzte Spalte (erzielte Etiketten) entspricht der Etiketten erzielte bei zwei Klassen. Die Klasse mit der größten Wahrscheinlichkeit erzielte als vorhergesagte Klasse der entsprechende Eintrag ausgewählt. Beispielsweise wird für den ersten Eintrag erzielte Bezeichnung "F" seit die größte Wahrscheinlichkeit ein "F" (0.916995) sein.

**Web Service-Publikation**

Sie erhalten auch erzielte Bezeichnungsfeld für jeden Eintrag und die Wahrscheinlichkeit, dass die erzielte Bezeichnung. Die grundlegende Logik ist die größte Wahrscheinlichkeit unter erzielten Wahrscheinlichkeiten zu. Dazu müssen Sie das [R-Skript ausführen] [ execute-r-script] Modul. Der R-Code in Abbildung 8 dargestellt, und das Ergebnis des Versuchs in Abbildung 9 dargestellt.

![R-Codebeispiel](./media/machine-learning-interpret-model-results/8.png)

Abbildung 8. R Code Etiketten bewertet und die zugeordnete Wahrscheinlichkeit Etiketten

![Ergebnis des Versuchs](./media/machine-learning-interpret-model-results/9.png)

Abbildung 9. Endgültige bewertungsexperiment Buchstaben Recognition multiklassenklassifizierung Problem

Veröffentlichen führen Sie den Webdienst aus und geben Sie einige Funktionswerte input-zurückgegebene Ergebnis sieht Abbildung 10. Dieser Buchstabe handschriftlich mit Funktionsumfang 16 extrahierten voraussichtlich "T" mit 0.9715 Wahrscheinlichkeit sein.

![Interpretation Bewertung Testmoduls](./media/machine-learning-interpret-model-results/9_1.png)

![Testergebnis](./media/machine-learning-interpret-model-results/10.png)

Abbildung 10. Web Service aus multiklassenklassifizierung

## <a name="regression"></a>Regression

Regressionsprobleme unterscheiden klassifizierungsprobleme. Eine Klassifizierung Problem versuchen Sie separate Klassen vorherzusagen, welche Klasse eine Iris Blume gehört. Aber wie im folgenden Beispiel eine Regression Problem sehen können, versuchen, eine kontinuierliche Variable wie ein Auto vorherzusagen.

**Beispiel-experiment**

Verwenden Sie Automobile preisvorhersage als Ihr Beispiel für Regression. Sie versuchen, den Preis eines Autos basierend auf den Kamerafeatures einschließlich stellen, Kraftstofftyp Texttyp und Antriebsrad Vorhersagen. Der Versuch ist in Abbildung 11 dargestellt.

![Kfz-Preis Regression experiment](./media/machine-learning-interpret-model-results/11.png)

Abbildung 11. Kfz-Preis Regression Problem experiment

Visualisierung der [Bewertung Modell] [ score-model] Modul das Ergebnis sieht aus wie in Abbildung 12.

![Bewerten von Ergebnissen für Automobile Preis Vorhersage problem](./media/machine-learning-interpret-model-results/12.png)

Abbildung 12. Ergebnis für Automobile Preis Vorhersage problem

**Ergebnisinterpretation**

Erzielte Etiketten ist die Ergebnisspalte in diesem Ergebnis. Vorhergesagte Preis für jedes Fahrzeug sind.

**Web Service-Publikation**

Regression Versuchs einen Webdienst veröffentlichen, und genauso verwenden wie zwei Klassen Klassifizierung für Automobile preisvorhersage aufrufen.

![Bewertungsexperiment für Automobile Preis regressionsproblem](./media/machine-learning-interpret-model-results/13.png)

Abbildung 13. Bewertungsexperiment ein Auto Preis Regression Problem

Den Webdienst ausgeführt, sucht das zurückgegebene Ergebnis wie in Abbildung 14. Vorhergesagte kostet dieses Auto $15,085.52.

![Interpretation Punktzahl Testmoduls](./media/machine-learning-interpret-model-results/13_1.png)

![Modul Bewertungsergebnisse](./media/machine-learning-interpret-model-results/14.png)

Abbildung 14. Web Service Ursache ein Auto Preis regression

## <a name="clustering"></a>Clustering

**Beispiel-experiment**

Wir verwenden die Iris Daten erneut zu clustering Experiment. Hier können Sie Klasse Etiketten im DataSet filtern, sodass nur verfügt und für das clustering verwendet werden. In diesem Iris Anwendungsfall, geben Sie die Anzahl der Cluster zwei während des Trainings sein Blumen in zwei Klassen cluster würde. Der Versuch ist in Abbildung 15 dargestellt.

![Experimentieren Sie clustering Iris-problem](./media/machine-learning-interpret-model-results/15.png)

Abbildung 15. Experimentieren Sie clustering Iris-problem

Clustering unterscheidet sich von einer Trainingsdataset Boden Wahrheit Etiketten selbst hat. Clustering Gruppen in unterschiedliche Instanzen Daten Training. Während des Trainings Etiketten Modell Einträge lernen Sie die Unterschiede zwischen ihrer Funktionen. Danach ausgebildete Modell lässt sich zukünftige Einträge weiter zu klassifizieren. Es gibt zwei Teilen das Ergebnis, dem wir in einem Cluster Problem interessiert sind. Der erste Teil ist Trainingsdataset beschriften und die zweite Klassifizierung ein neues Dataset mit qualifizierten Modell.

Der erste Teil des Ergebnisses durch Klicken der linken Ausgabeanschluss [Zug Clusteringmodells] visualisiert werden[ train-clustering-model] **visualisieren**klicken. Abbildung 16 zeigt die Visualisierung.

![Ergebnis-Clustering](./media/machine-learning-interpret-model-results/16.png)

Abbildung 16. Visualisieren Sie clustering Ergebnis Trainingsdataset

Das Ergebnis des zweiten Teils clustering neue Einträge mit qualifizierten Clustermodell ist in Abbildung 17 dargestellt.

![Visualisieren Sie clustering Ergebnis](./media/machine-learning-interpret-model-results/17.png)

Abbildung 17. Visualisieren Sie Ergebnis auf einem neuen Cluster

**Ergebnisinterpretation**

Obwohl die Ergebnisse der beiden Teile aus verschiedenen Experiment stammen sie einheitlich und auf die gleiche Weise interpretiert werden. Die ersten vier Spalten sind Features. Die letzte Spalte Arbeitsaufträge ergibt Vorhersage. Die gleiche Nummer Einträge voraussichtlich im selben Cluster, d. h., sie Ähnlichkeit in irgendeiner Weise Teilen (dieses Experiment verwendet standardmäßig euklidischen Abstand Metrik). Da die Anzahl der Cluster 2 angegeben wird, werden Einträge in Aufgaben entweder 0 oder 1 bezeichnet.

**Web Service-Publikation**

Clustering Versuchs einen Webdienst veröffentlichen, und für Vorhersagen wie zwei Klassen Klassifizierung Anwendungsfall clustering aufrufen.

![Iris clustering Problem Experiment bewerten](./media/machine-learning-interpret-model-results/18.png)

Abbildung 18. Versuch ein clustering Iris-Problem bei der Bewertung

Abbildung 19 nach dem Ausführen des Webdiensts aussieht zurückgegebene Ergebnis. Diese Blume voraussichtlich Cluster 0.

![Test interpretieren Punktzahl Modul](./media/machine-learning-interpret-model-results/18_1.png)

![Modul Ergebnis](./media/machine-learning-interpret-model-results/19.png)

Abbildung 19. Web Service aus Iris zwei Klassen Klassifizierung

## <a name="recommender-system"></a>Recommender system
**Beispiel-experiment**

Für Recommender-Systeme können das Restaurant Empfehlung Problem als Beispiel: Restaurants können Kunden auf ihre Bewertung empfohlen. Die Eingabedaten besteht aus drei Teilen:

* Restaurant Bewertungen von Kunden
* Funktion von Kundendaten
* Restaurant Objektdaten

Gibt können wir mit dem [Zug Matchbox Recommender] [ train-matchbox-recommender] Modul in Azure maschinelles lernen:

- Bewertung für einen bestimmten Benutzer und Artikel Vorhersagen
- Empfehlen Sie Elemente an einen bestimmten Benutzer
- Benutzer mit einem bestimmten Benutzer verknüpft
- Suchen Sie Elemente im Zusammenhang mit einem bestimmten Element

Sie wollen die Optionen im Menü **Recommender Vorhersage Art** auswählen können. Hier können Sie alle vier Szenarien durchlaufen.

![Matchbox recommender](./media/machine-learning-interpret-model-results/19_1.png)

Ein normale Azure Machine Learning Experiment Recommender System sieht aus wie in Abbildung 20. Informationen zur Verwendung von Recommender Systemmodule finden Sie unter [Zug Matchbox Recommender] [ train-matchbox-recommender] und [Bewertung Matchbox Recommender][score-matchbox-recommender].

![Recommender System experiment](./media/machine-learning-interpret-model-results/20.png)

Abbildung 20. Recommender System experiment

**Ergebnisinterpretation**

**Bewertung für einen bestimmten Benutzer und Artikel Vorhersagen**

**Bewertung Vorhersagemodell** **Recommender Vorhersage**Art auswählen, fordern Sie Recommender System die Bewertung für einen bestimmten Benutzer und Artikel vorherzusagen. Die Visualisierung der [Faktor Matchbox Recommender] [ score-matchbox-recommender] Ausgabe sieht aus wie in Abbildung 21.

![Ergebnis aus Recommender System - Bewertung Vorhersage](./media/machine-learning-interpret-model-results/21.png)

Abbildung 21. Visualisieren Sie Ergebnis aus Recommender System - Bewertung Vorhersage

Die ersten beiden Spalten sind gemäß der Eingabedaten Benutzer-Element-Paare. Die dritte Spalte ist die vorhergesagte Bewertung eines Benutzers für ein bestimmtes Element. In der ersten Zeile beispielsweise wird Kunden U1048 Rate Restaurant 135026 2 vorhergesagt.

**Empfehlen Sie Elemente an einen bestimmten Benutzer**

**Artikel Empfehlung** **Recommender Vorhersage**Art auswählen, bitten Sie Recommender System Elemente an einen bestimmten Benutzer zu empfehlen. Der letzte Parameter wählen in diesem Szenario ist *Auswahl empfohlen*. Die Option ist hauptsächlich für auswerten während des Trainings **Aus bewertete Elemente (auswerten)** . Für diese vorhersagephase wählen wir **Alle Elemente**. Die Visualisierung der [Faktor Matchbox Recommender] [ score-matchbox-recommender] Ausgabe sieht Abbildung 22.

![Ergebnis aus Recommender System - Element Empfehlung](./media/machine-learning-interpret-model-results/22.png)

Abbildung 22. Visualisieren Sie Ergebnis aus Recommender System - Element Empfehlung

Die erste der sechs Spalten stellt die Benutzer IDs Elemente, empfehlen von Eingabedaten. Die fünf Spalten darstellen Elemente in absteigender Reihenfolge nach Relevanz Benutzer empfohlen. In der ersten Zeile ist das empfohlene Restaurant Kunden U1048 z. B. 134986, gefolgt von 135018, 134975, 135021 und 132862.

**Benutzer mit einem bestimmten Benutzer verknüpft**

**Verwandte Benutzer** **Recommender Vorhersage**Art auswählen, bitten Sie Recommender System verknüpfte Benutzer an einen angegebenen Benutzer. Verwandte Benutzer sind Benutzer, die ähnliche Voreinstellungen. Der letzte Parameter wählen in diesem Szenario ist *Verwandte Benutzerauswahl*. Die Option ist hauptsächlich für auswerten während des Trainings **Aus Benutzern, bewertete Elemente (auswerten)** . Wählen Sie **Alle Benutzer** für diese vorhersagephase. Die Visualisierung der [Faktor Matchbox Recommender] [ score-matchbox-recommender] Ausgabe sieht Abbildung 23.

![Ergebnis aus Recommender System Verwandte Benutzer](./media/machine-learning-interpret-model-results/23.png)

Abbildung 23. Visualisieren Sie Resultat Recommender System - Verwandte Benutzer

Die erste der sechs Spalten zeigt dem Benutzer IDs musste Verwandte Benutzer von Eingabedaten. Die fünf Spalten speichern vorhergesagter Verwandte Benutzer des Benutzers in absteigender Reihenfolge nach Relevanz. Beispielsweise ist in der ersten Zeile der wichtigsten Kunden für Kunden U1048: U1051, gefolgt von U1066, U1044: U1017 und: U1072.

**Suchen Sie Elemente im Zusammenhang mit einem bestimmten Element**

**Verwandte Elemente** **Recommender Vorhersage**Art auswählen, fordern Sie Recommender System verwandter Elemente zu einem bestimmten Element suchen. Verwandte Elemente sind Elemente wahrscheinlich durch denselben Benutzer gefallen. Der letzte Parameter wählen in diesem Szenario ist *Verwandte Auswahl*. Die Option ist hauptsächlich für auswerten während des Trainings **Aus bewertete Elemente (auswerten)** . Wir wählen **Alle Elemente** für diese vorhersagephase. Die Visualisierung der [Faktor Matchbox Recommender] [ score-matchbox-recommender] Ausgabe sieht Abbildung 24.

![Ergebnis aus Recommender System – zugehörige Artikel](./media/machine-learning-interpret-model-results/24.png)

Abbildung 24. Visualisieren Sie Resultat des Recommender Systems - verwandte Elemente

Die erste der sechs Spalten stellt das angegebene Element IDs musste verwandter Elemente von Eingabedaten. Die fünf Spalten speichern vorhergesagter verwandter Elemente des Elements in absteigender Reihenfolge nach ihrer Relevanz. In der ersten Zeile ist das wichtigste Element für Element 135026 beispielsweise 135074, gefolgt von 135035, 132875, 135055 und 134992.

**Web Service-Publikation**

Die Veröffentlichung dieser Experimente als Webdienste Vorhersagen ist für jedes der vier Szenarien. Wir nehmen das zweite Szenario (empfohlene Elemente an einen bestimmten Benutzer) als Beispiel. Sie können mit den anderen drei folgen.

Ausgebildeten Recommender System als geschulte Modell speichern und Eingabedaten für einen einzelnen Benutzer-ID-Spalte filtern, wie angefordert, können Experiment in Abbildung 25 einbinden und als Webdienst veröffentlichen.

![Versuch des Problems Empfehlung Restaurant bewerten](./media/machine-learning-interpret-model-results/25.png)

Abbildung 25. Versuch des Problems Empfehlung Restaurant bewerten

Den Webdienst ausgeführt, sucht das zurückgegebene Ergebnis wie in Abbildung 26. Die fünf empfohlenen Restaurants für Benutzer U1048 sind 134986, 135018, 134975, 135021 und 132862.

![Beispiel des Recommender-Dienstes](./media/machine-learning-interpret-model-results/25_1.png)

![Beispiel-Testergebnisse](./media/machine-learning-interpret-model-results/26.png)

In Abbildung 26. Web Service Ursache Restaurant Empfehlung


<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
