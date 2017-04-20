<properties
    pageTitle="Engineering und in Azure Machine Learning Funktion | Microsoft Azure"
    description="Im Sinne der Featureauswahl und Featureentwicklung erläutert und Beispiele für ihre Rolle bei der Erweiterung Daten Computer lernen."
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
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Feature Engineering und in Azure maschinelles lernen

Dieses Thema erklärt Zwecke Feature und Featureauswahl dabei Daten Verbesserung der computerlernen. Worin diese Prozesse mithilfe von Azure Machine Learning Studio Beispielen veranschaulicht.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Die Trainingsdaten verwendet Computer lernen können oft durch Auswahl oder Extrahieren von Features aus unformatierten Daten erweitert werden. Ein Beispiel eines Engineering-KE im Kontext zu klassifizieren Bilder handgeschriebene Zeichen ist eine Bit-Dichte Zuordnung aus unformatierten Bit Verteilungsdaten erstellt. Diese Zuordnung kann Auffinden Kanten der Zeichen als raw Verteilung.

Engineering und ausgewählte Features Effizienz Trainingsprozess versucht, wichtige Informationen in den Daten zu extrahieren. Sie verbessern auch macht diese Modelle genau die eingegebenen Daten klassifizieren und Ergebnisse an stabiler vorherzusagen. Feature Engineering und Auswahl können zu lernen rechenintensiver vermitteln auch kombinieren. Dies geschieht und reduzieren die Anzahl der Funktionen Kalibrieren oder ein Modell. Mathematisch sind die Funktionen zum Trainieren des Modells aktiviert einen Mindestsatz von unabhängigen Variablen, die Muster in den Daten erklären und Vorhersagen Ergebnisse erfolgreich.

Engineering und Funktionen ist ein Teil eines größeren Prozesses besteht normalerweise aus vier Schritten:

* Datensammlung
* Daten-Erweiterung
* Erstellen
* Nachbearbeitung

Engineering und Auswahl machen der Erweiterung Schritt Computer lernen. Drei Aspekte dieses Prozesses können für unsere Zwecke unterschieden werden:

* **Vor Verarbeitung**: Dieser Vorgang versucht sicherzustellen, dass die Daten bereinigen und konsistent ist. Es umfasst Aufgaben wie Integration mehrerer Datensätze, Behandlung fehlender Daten, inkonsistente Daten behandeln und Konvertieren von Datentypen.
* **Featureentwicklung**: Dieser Vorgang versucht, zusätzliche relevante Features aus vorhandenen raw Features in den Daten erstellen und vorhersehbare Leistungssteigerung Learning-Algorithmus.
* **Featureauswahl**: Wählt dabei wichtige Teilmenge der ursprünglichen Datenfeatures Dimensionalität des Problems Schulung zu.

Dieses Thema behandelt nur die Featureentwicklung und Feature Selection Aspekte der Erweiterung Daten. Weitere Informationen zu der Schritt vor der Verarbeitung finden Sie unter [Daten in Azure Machine Learning Studio vorverarbeitet](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).


## <a name="creating-features-from-your-data--feature-engineering"></a>Erstellen von Features aus den Daten – Featureentwicklung

Die Trainingsdaten besteht aus einer Matrix bestehend aus Beispielen (Datensätzen bzw. Zeilen gespeichert), von denen jedes eine Reihe von Funktionen (Variablen oder Felder in Spalten gespeichert) hat. Die Versuchsanordnung angegebenen Funktionen sollen kennzeichnen die Muster in den Daten. Zwar viele Feld Ursprungsdaten direkt in den ausgewählten Funktion verwendet, um ein Modell berücksichtigt werden können, müssen zusätzliche entwickelte Features häufig Funktionen in die Rohdaten zu einer verbesserten Trainingsdataset erstellt werden.

Welche Funktionen erstellt werden soll, um das DataSet beim Trainieren eines Modells zu verbessern? Engineering Features, die zur Verbesserung der Schulung enthalten Informationen, die das Muster in den Daten besser unterscheidet. Sie erwarten neuen Funktionen für zusätzliche Informationen, die eindeutig nicht erfasst oder in der ursprünglichen oder vorhandene Funktionsumfang, aber dieser Vorgang leicht ersichtlich ist eine Kunst. Sound und produktive Entscheidungen erfordern häufig einige Fachwissen.

Beim Starten mit Azure Machine Learning ist es am einfachsten dabei konkret mithilfe der Beispiele in Machine Learning Studio erfassen. Hier sind zwei Beispiele vorgestellt:

* Ein Beispiel Regression ([Vorhersage der Anzahl Fahrräder](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) in einem kontrollierten, Zielwerte bekannt sind
* Eine Text-Mining Klassifizierung Beispiel [Feature Hashing][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Beispiel 1: Hinzufügen eines Regressionsmodells zeitlicher Funktionen ###

Verwendung auf Funktionen für eine regressionsaufgabe nehmen Experiment "Anforderung Prognosen Fahrräder" in Azure Machine Learning Studio. Dieses Experiment soll den Bedarf für die Fahrräder, die Anzahl der in einem bestimmten Monat, Tag oder Stunde Verleih vorherzusagen. Datensatz **Datensatz Fahrrad Vermietung UCI** dient als input Rohdaten.

Dieses Dataset basiert auf echten Daten vom Capital Bikeshare Unternehmen, die einer Fahrrad Vermietung Netzwerk in Washington DC in den USA verwaltet. Das Dataset stellt die Anzahl der Fahrräder innerhalb einer bestimmten Tageszeit von 2011 bis 2012 und 17379 Zeilen und 17 Spalten enthält. Der raw-Funktion enthält Wetter (Temperatur, Luftfeuchtigkeit, Geschwindigkeit) und Wochentag (Urlaub oder Wochentag). Vorhersagen ist **Cnt**, Anzahl der Verleih in eine bestimmte Stunde darstellt und dass 1 bis 977.

Zum Erstellen von effektiver Funktionen in den Trainingsdaten werden vier regressionsmodelle mit demselben Algorithmus jedoch mit vier verschiedenen erstellt. Die vier Datensätze dieselben eingegebenen Rohdaten, sondern mit mehr Funktionen. Diese Funktionen sind in vier Kategorien unterteilt:

1. A = Wetter Urlaub + Weekday + Wochenende Features für die vorhergesagte Tag
2. B = Anzahl der in jedem der letzten 12 Stunden vermietet wurden Fahrräder
3. C = Anzahl der Fahrräder aller vorherigen 12 Tage bei derselben Stunde vermietet wurden
4. D = Anzahl der Fahrräder aller vorherigen 12 Wochen bei derselben Stunde und am selben Tag vermietet wurden

Neben Feature Set A in den ursprünglichen unverarbeiteten Daten vorhanden ist, werden die drei Gruppen von Funktionen über das Feature engineering-Prozess erstellt. Funktionsumfang B erfasst die aktuelle Nachfrage der Fahrräder. Funktionsumfang C erfasst die Nachfrage nach Fahrrädern zu einer bestimmten Uhrzeit. Funktionsumfang D erfasst Nachfrage nach Fahrrädern Stunde und Tag der Woche. Jeder der vier Datensätze Schulung umfasst Features A, A + B, A + B + C und A + B + C + D bzw..

Im Experiment Azure maschinelles lernen werden diese vier Training Datensätze über vier Filialen von vorverarbeitete Eingabedatensatzes gebildet. Außer den linken Zweig enthält jede Filialen ein [R-Skript ausführen] [ execute-r-script] in dem eine Reihe von Features (Funktionsgruppen, B, C und D) abgeleitete Modul bzw. erstellt und importierten Datensatz angefügt. Die folgende Abbildung veranschaulicht R Skript zur Funktion B im Bereich Links erstellen.

![Erstellen Sie ein](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

In der folgenden Tabelle werden den Vergleich der Leistung der vier Modelle zusammengefasst. Die besten Ergebnisse werden nach Funktionen A + B + C angezeigt. Beachten Sie, dass die Fehlerrate verringert, wenn zusätzliche Funktionen in den Trainingsdaten enthalten sind. Unsere Vermutung Featuregruppen B und C bereit zusätzliche Informationen für den Vorgang Regression überprüft. Hinzufügen der Funktion D anscheinend keine zusätzlichen reduzieren die Fehlerquote.

![Vergleichen der Leistung](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Beispiel 2: Erstellen von Features in Text-mining  

Featureentwicklung wird häufig in textmining wie Dokumentanalyse Klassifizierung und Stimmung Aufgaben angewendet. Beispielsweise wird Dokumente in Kategorien klassifiziert werden soll, eine normale Annahme sind die Wörter oder Ausdrücke in einer Dokumentkategorie enthalten weniger wahrscheinlich in einem anderen Dokumentkategorie. Anders gesagt kann die Häufigkeit der Suchbegriff Verteilung kennzeichnen die verschiedenen Kategorien. Im Bergbau Text engineering-Prozess Funktion muss die KEs mit Suchbegriff Frequenzen, da einzelne Text-Inhalt normalerweise als Eingabedaten verwendet.

Für diese Aufgabe gilt genannte *Feature hashing* um beliebige Funktionen in Indizes effizient zu verwandeln. Anstatt jede Textfunktion (Wörter oder Sätze) an einem bestimmten Index dieser Funktionen durch Anwenden einer Hashfunktion auf die Funktionen und die Hashwerte als Indizes direkt zuordnen.

In Azure Machine Learning ist ein [Feature Hashing] [ feature-hashing] Modul, das diese Features Wort oder Ausdruck erstellt. Die folgende Abbildung zeigt ein Beispiel für dieses Modul. Eingabedataset enthält zwei Spalten: Buch Bewertung von 1 bis 5 und den tatsächlichen Inhalt überprüfen. Das Ziel dieses [Feature Hashing] [ feature-hashing] Modul neue Funktionen abgerufen, die die Häufigkeit Vorkommen der entsprechenden Wörter oder Ausdrücke in bestimmten Buchbesprechung angezeigt wird. Um dieses Modul zu verwenden, müssen Sie die folgenden Schritte ausführen:

1. Wählen Sie die Spalte mit dem eingegebenen Text (in diesem Beispiel**SP2** ).
2. Festgelegt *Hashing Bitsize* 8, 2 ^ 8 = 256-KEs erzeugt. Das Wort oder die Wortgruppe ist 256 Indizes berechnet. Der Parameter liegt *Hashing-Bitsize* zwischen 1 und 31. Wenn auf den Parameter, können Wörter oder Ausdrücke in den gleichen Index gehasht werden.
3. Den Parameter *N-Grams* auf 2 festgelegt. Dies ruft die Häufigkeit Vorkommen von unigramme (ein Feature für jedes Wort) und bigrame (ein Feature für jedes Paar von benachbarten Wörter) aus dem Eingabetext ab. Parameters zwischen *N-Grams* 0 und 10, womit maximal einzelne Wörter in einer Funktion enthalten sein.  

![Feature hashing-Modul](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Die folgende Abbildung zeigt, wie diese neuen Features aussehen.

![Feature hashing-Beispiel](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Filterfunktionen aus den Daten - Feature-Auswahl  ##

*Featureauswahl* ist ein Vorgang, der häufig auf Datensätze für Aufgaben wie Klassifizierung oder Regression Aufgaben Vorhersagemodellen Training angewendet wird. Das Ziel ist eine Teilmenge der Funktionen aus dem ursprünglichen Datensatz auswählen, der die Dimensionen verringert mit einem minimalen Satz von Funktionen die maximale Abweichung in den Daten dargestellt. Dieser Teil der Features enthält nur Funktionen, die zum Trainieren des Modells berücksichtigt werden. Featureauswahl erfüllt zwei Hauptfunktionen:

* Featureauswahl häufig erhöht die Klassifizierung Genauigkeit durch Eliminierung irrelevant, redundante und Korrelation Funktionen.
* Featureauswahl verringert die Anzahl von Features, macht das Modell Training zu effizienter. Dies ist besonders wichtig für Lernende teuer wie Support Vector trainieren.

Obwohl Featureauswahl Features in den Daten zum Trainieren des Modells verwendet weniger soll, wird nicht in der Regel bezeichnet der Begriff *Reduzierung der Dimensionalität.* Featureauswahlmethoden extrahieren einen Teil der ursprünglichen Features in den Daten zu.  Dimensionalität Methoden einsetzen entwickelte Features, die im ursprünglichen KE umwandeln und so ändern können. Dimensionalität Methoden zählen hauptkomponentenanalyse und kanonische korrelationsanalyse Wert Zerlegung.

Eine häufig angewendete Kategorie featureauswahlmethoden betreute dabei ist filterbasierte Featureauswahl. Auswerten der Korrelations zwischen jede Funktion und das Zielattribut gelten diese Methoden statistisches Maß um jede Funktion einen Wert zuzuweisen. Die Features sind dann die Bewertung Platz mit den Schwellenwert beibehalten oder ein bestimmtes Feature eliminieren. Diese Methoden der statistischen Maßnahmen gehören Pearson Korrelation, Informationen und der Chi-Quadrat-Test.

Azure Machine Learning Studio bereit Featureauswahl Module. Wie in der folgenden Abbildung gezeigt, enthalten diese Module [filterbasierte Featureauswahl] [ filter-based-feature-selection] und [Fisher lineare Diskriminanzanalyse][fisher-linear-discriminant-analysis].

![Beispiel für KE-Auswahl](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


Z. B. [filterbasierte Featureauswahl] [ filter-based-feature-selection] mit Text Mining Beispiel zuvor beschrieben. Angenommen, Sie möchten ein Regressionsmodell erstellen nach 256 Funktionen durch die [Funktion Hashing] erstellt wird[ feature-hashing] Modul und die antwortvariable **SP1** und stellt ein Buch lesen Bewertung im Bereich von 1 bis 5. Fest **Feature Methode bewerten** , **Pearson Korrelation** **Zielspalte** **SP1**und **Anzahl der gewünschten Features** **50** Das Modul [filterbasierte Featureauswahl] [ filter-based-feature-selection] ergibt ein Dataset mit 50 Funktionen mit Zielattribut **SP1**. Die folgende Abbildung zeigt den Fluss dieses Experiments und die Eingabeparameter.

![Beispiel für KE-Auswahl](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Die folgende Abbildung zeigt den resultierenden Datensätze. Jede Funktion ist Grundlage Pearson Korrelation zwischen sich selbst und das Zielattribut **Col1**bewertet. KEs mit Bestnoten bleiben.

![Filter-basierte Funktion Auswahl Datensätze](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Die folgende Abbildung zeigt die entsprechenden Werte der ausgewählten Features.

![Ausgewählte Feature Faktoren](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Durch Anwenden dieser [Filter Featureauswahl] [ filter-based-feature-selection] Modul 50 von 256 Features ausgewählt sind, da sie die Funktionen mit variabler **SP1** auf die Bewertungsmethode **Pearson Korrelation**korreliert.

## <a name="conclusion"></a>Abschluss
Feature Engineering und Featureauswahl sind zwei Schritte häufig um die Trainingsdaten beim Computer Lernmodell vorzubereiten. Normalerweise Featureentwicklung wird zuerst angewendet, um zusätzliche Funktionen zu generieren und dann Schritt Auswahl Funktion beseitigen irrelevant, redundante oder stark korrelierte Features erfolgt.

Es ist nicht immer unbedingt Featureauswahl Engineering oder Funktion durchzuführen. Ob Bedarf hängt Daten sammeln oder haben, holen Sie Algorithmus und das Ziel des Experiments.


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
