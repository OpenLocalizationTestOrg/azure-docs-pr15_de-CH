<properties
    pageTitle="Featureentwicklung dabei Analytics Cortana | Microsoft Azure" 
    description="Im Sinne der Featureentwicklung erläutert und Beispiele für seine Rolle in der Erweiterung Daten Computer lernen."
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


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>In der Cortana-Analyseprozess Featureentwicklung 

In diesem Thema im Sinne Featureentwicklung erläutert und Beispiele für seine Rolle in der Erweiterung Daten Computer lernen. Beispiele zur Veranschaulichung dieses Prozesses werden von Azure Machine Learning Studio gezeichnet. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Dieses **Menü** enthält Links zu Themen, in denen Funktionen für Daten in unterschiedlichen Umgebungen erstellen. Diese Aufgabe ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Sind technische Versuche zu vorhersehbaren macht lernen Algorithmen erstellen Features aus Rohdaten, das Lernen erleichtern. Engineering und Funktionen ist ein Teil der TDSP gemäß den [Neuigkeiten Team Daten Wissenschaft?](data-science-process-overview.md) Feature Engineering und Auswahl sind Bestandteile des **Features entwickeln** Teil der TDSP. 

* **Featureentwicklung**: Dieser Vorgang versucht, zusätzliche relevante Features aus vorhandenen raw Features in den Daten erstellen, und vorhersehbaren Leistungssteigerung Learning-Algorithmus.

* **Featureauswahl**: dabei wählt wichtige Teilmenge der ursprünglichen Datenfeatures versucht zu Dimensionalität Training-Problem.

Normalerweise wird **Featureentwicklung** zuerst um zusätzliche Funktionen und der **Featureauswahl** Schritt wird beseitigen irrelevante, redundante oder stark korrelierte Funktionen ausgeführt.

Die Trainingsdaten verwendet Computer lernen können oft durch Extraktion von Features aus unformatierten Daten erweitert werden. Ein Beispiel eines Engineering-KE im Kontext zu klassifizieren Bilder handgeschriebene Zeichen ist Schaffung etwas dichtekarte aus unformatierten Bit Verteilungsdaten erstellt. Diese Zuordnung kann Auffinden Kanten der Zeichen effizienter als einfach direkt mit raw Verteilung.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Erstellen von Features aus den Daten - Featureentwicklung

Die Trainingsdaten besteht aus einer Matrix bestehend aus Beispielen (Datensätzen bzw. Zeilen gespeichert), von denen jedes eine Reihe von Funktionen (Variablen oder Felder in Spalten gespeichert) hat. Die Versuchsanordnung angegebenen Funktionen sollen kennzeichnen die Muster in den Daten. Obwohl viele der Rohdaten Felder direkt in den ausgewählten Funktion verwendet, um ein Modell berücksichtigt werden können, ist es häufig der Fall Zusatzfunktionen (Engineering) Funktionen in die Rohdaten zu erweiterten Trainings-Dataset erstellt werden müssen.

Welche Funktionen erstellt werden soll, um das Dataset beim Trainieren eines Modells zu verbessern? Engineering Features, die zur Verbesserung der Schulung enthalten Informationen, die das Muster in den Daten besser unterscheidet. Wir erwarten, dass neuen Funktionen für zusätzliche Informationen, die nicht eindeutig erfasste oder in den ursprünglichen oder einer vorhandene Funktion leicht ersichtlich ist. Aber dabei ist eine Kunst. Sound und produktive Entscheidungen erfordern häufig einige Fachwissen.

Beim Starten mit Azure Machine Learning ist es am einfachsten dieser Prozess auch Beispiele Studio erfassen. Hier sind zwei Beispiele vorgestellt:

* Eine Regression Beispiel [Vorhersage der Anzahl Fahrräder](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) in einem überwachten, Zielwerte bekannt sind
* Ein Miningmodell Klassifizierung Beispieltext [Feature Hashing](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) verwenden

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Beispiel 1: Zeitliche Features für Regressionsmodell hinzufügen ###

Nehmen Experiment "Anforderung Prognosen Fahrräder" in Azure Machine Learning Studio auf Funktionen für eine regressionsaufgabe veranschaulichen. Dieses Experiment soll den Bedarf für die Fahrräder, die Anzahl der in einem bestimmten Monat/Tag/Stunde Verleih vorherzusagen. Das Dataset "Verleih UCI Dataset" als Eingaben Rohdaten verwendet wird. Dieses Dataset basiert auf echten Daten vom Capital Bikeshare Unternehmen, die einer Fahrrad Vermietung Netzwerk in Washington DC in den USA verwaltet. Das Dataset stellt die Anzahl der Fahrräder innerhalb einer bestimmten Stunde des Tages in den Jahren 2011 und 2012 und 17379 Zeilen und 17 Spalten enthält. Raw Feature Wetter (Temperatur, Luftfeuchtigkeit, Wind Geschwindigkeit) und enthalten den Wochentag (Urlaub/Wochentag). Vorhersagen ist "Cnt" Anzahl der Verleih in eine bestimmte Stunde darstellt und dem Bereich reicht von 1 bis 977.

Mit dem Ziel der effektive Funktionen in den Trainingsdaten vier Regression erstellte Modelle sind mit dem gleichen Algorithmus mit vier unterschiedlichen Datasets erstellen. Die vier Datasets dieselben Eingaben Rohdaten, sondern mit mehr Funktionen. Diese Funktionen sind in vier Kategorien unterteilt:

1. A = Wetter Urlaub + Weekday + Wochenende Features für die vorhergesagte Tag
2. B = Anzahl der in jedem der letzten 12 Stunden vermietet wurden Fahrräder
3. C = Anzahl der Fahrräder aller vorherigen 12 Tage bei derselben Stunde vermietet wurden
4. D = Anzahl der Fahrräder aller vorherigen 12 Wochen bei derselben Stunde und am selben Tag vermietet wurden

Neben Feature Set A in den ursprünglichen unverarbeiteten Daten vorhanden, werden die drei Gruppen von Funktionen durch die Funktion engineering-Prozess erstellt. Funktionsumfang B erfasst jüngsten Nachfrage für die Räder. Funktionsumfang C erfasst die Nachfrage nach Fahrrädern zu einer bestimmten Uhrzeit. Funktionsumfang D erfasst Nachfrage nach Fahrrädern Stunde und Tag der Woche. Die vier Training Datasets jedes enthält Feature Set A, A + B, A + B + C und A + B + C + D.

Im Experiment Azure Machine Learning bilden diese vier Training Datasets über vier Filialen von vorverarbeitete Eingabedatasets. Außer den meisten Zweig Filialen enthält Links ein [Ausführen R](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) Skriptmodul, in dem eine Reihe von Funktionen (Funktion B, C und D) abgeleitet werden erstellt bzw. importiert Dataset angefügt. Die folgende Abbildung veranschaulicht R Skript zur Funktion B im Bereich Links erstellen.

![KEs erzeugen](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

Vergleich der Leistung der vier Modelle sind in der folgenden Tabelle zusammengefasst. Die besten Ergebnisse werden nach Funktionen A + B + C angezeigt. Beachten Sie, dass die Fehlerrate Wenn reduziert zusätzliche Features sind in den Trainingsdaten enthalten. Er überprüft unsere Vermutung der Featuresatz B, C zusätzliche Informationen für die regressionsaufgabe bereitstellen. Hinzufügen der Funktion D nicht, scheint aber zusätzliche reduzieren die Fehlerquote.

![Vergleich](./media/machine-learning-data-science-create-features/result1.png)

### <a name="example2"></a>Beispiel 2: Erstellen von Features in Textmining  

Featureentwicklung wird häufig in textmining wie Dokumentanalyse Klassifizierung und Stimmung Aufgaben angewendet. Beispielsweise wird Dokumente in Kategorien klassifiziert werden soll, eine normale Annahme sind die Word/Sätze in einer Doc-Kategorie enthalten weniger wahrscheinlich in einer anderen Kategorie Doc. In anderen Worten ist die Häufigkeit von Wörtern und Satzteilen Verteilung unterschiedliche Dokumenttypen charakterisieren. Im Text Bergbau da einzelne Text-Inhalt normalerweise als Eingabedaten dienen Feature engineering-Prozess muss Wort oder Satzteil Frequenzen mit Features erstellen.

Für diese Aufgabe gilt genannte **Feature hashing** um beliebige Funktionen in Indizes effizient zu verwandeln. Anstatt jede Textfunktion (Wörter/Sätze) an einem bestimmten Index dieser Funktionen durch Anwenden einer Hashfunktion auf die Funktionen und die Hashwerte als Indizes direkt zuordnen.

In Azure Machine Learning ein [Feature Hashing](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) -Modul, das diese erstellt Wort oder Satzteil Funktionen bequem ist. Folgende Abbildung zeigt ein Beispiel für dieses Modul. Eingabe-Dataset enthält zwei Spalten: Buch Bewertung von 1 bis 5, und die tatsächliche überprüfen. Das Ziel dieses [Feature Hashing](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) ist ein paar neue abrufen Funktionen zeigen die Häufigkeit Vorkommen des entsprechenden Wörter / Sätze in bestimmten Buch lesen. Um dieses Modul zu verwenden, müssen die folgenden Schritte ausführen:

* Zunächst wählen Sie die Spalte mit dem eingegebenen Text (in diesem Beispiel "SP2").
* Zweitens gesetzt "Hashing-Bitsize" 8, 2 ^ 8 = 256-KEs erzeugt. Word/Phase im gesamten Text wird 256 Indizes gehasht. Der Parameter "Hashing-Bitsize" zwischen 1 und 31. Die Wörter / Sätze werden voraussichtlich in demselben Index gehasht werden, wenn sie eine größere Anzahl.
* Drittens setzen Sie die Parameter "N-Grams" auf 2. Eingabetext Ruft die Häufigkeit Vorkommen von unigramme (ein Feature für jedes Wort) und bigrame (ein Feature für jedes Paar von benachbarten Wörter) ab. Der Parameter Bereich "N-Grams" von 0 bis 10 gibt die maximale Anzahl sequenzieller Wörter in einer Funktion enthalten sein.  

![Modul "Feature Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

Die folgende Abbildung zeigt, wie diese neue Funktion aussehen wie.

![Beispiel "Feature Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Abschluss

Engineering und ausgewählte Features Effizienz Trainingsprozess versucht, wichtige Informationen in den Daten zu extrahieren. Sie verbessern auch macht diese Modelle genau die eingegebenen Daten klassifizieren und Ergebnisse an stabiler vorherzusagen. Feature Engineering und Auswahl können zu lernen rechenintensiver vermitteln auch kombinieren. Dies geschieht und reduzieren die Anzahl der Funktionen Kalibrieren oder ein Modell. Mathematisch sind die Funktionen zum Trainieren des Modells aktiviert einen Mindestsatz von unabhängigen Variablen, die Muster in den Daten erklären und Vorhersagen Ergebnisse erfolgreich.

Beachten Sie, dass es nicht immer unbedingt Featureauswahl Engineering oder Funktion ausführen. Oder nicht erforderlich ist, hängt davon ab Daten, die wir oder sammeln, Algorithmus wir holen und das Ziel des Experiments.
 
