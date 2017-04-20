<properties 
    pageTitle="Modell in Azure Machine Learning Debuggen | Microsoft Azure" 
    description="Erläutert, wie Sie Ihr Modell in Azure Machine Learning Debuggen." 
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
    ms.date="09/09/2016" 
    ms.author="bradsev;garye" />

# <a name="debug-your-model-in-azure-machine-learning"></a>Debuggen Sie Ihr Modell in Azure maschinelles lernen

Erläutert, wie Sie Modelle in Microsoft Azure Machine Learning Debuggen. Insbesondere werden die Gründe, warum die folgenden zwei Szenarien Fehler auftreten wenn ein Modell, behandelt:

* [Zug Modell] [ train-model] Modul löst einen Fehler aus. 
* [Bewertung Modell] [ score-model] Modul falsche Ergebnisse liefert 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-throws-an-error"></a>Zug Modell Modul löst einen Fehler aus.

![Bild1](./media/machine-learning-debug-models/train_model-1.png)

[Zug Modell] [ train-model] Modul erwartet die folgenden 2 Eingaben:

1. Typ der Klassifizierung/Regressionsmodell aus der Auflistung von Modellen Azure maschinelles lernen
2. Die Daten mit einer angegebenen Beschriftung. Die Bezeichnungsspalte gibt die Variable vorherzusagen. Die übrigen Spalten enthalten sind Funktionen verwendet.

Dieses Modul löst einen Fehler in den folgenden Fällen:

1. Der Beschriftungsspalte wird entweder mehr als eine Spalte als Bezeichnung oder eine falsche Spaltenindex ausgewählt falsch angegeben. Z. B. gilt der zweite Fall, wenn ein Spaltenindex von 30 Eingabedatasets verwendet wurde die nur 25 Spalten.

2. Das Dataset enthält keine Funktion Spalten. Z. B. wäre Eingabedatasets nur 1 Spalte ist die Spalte Beschriftung gekennzeichnet ist, es keine Features, das Modell zu erstellen. In diesem Fall [Zug Modell] [ train-model] Modul wird ein Fehler ausgelöst.

3. Eingabedatasets (Funktionen oder Bezeichnung) unendlich als einen Wert enthalten.


## <a name="score-model-module-does-not-produce-correct-results"></a>Bewertung Modell Modul erzeugt keine korrekte Ergebnisse

![Bild2](./media/machine-learning-debug-models/train_test-2.png)

In einem normalen Diagramm Training und Tests für überwachte lernen die [Aufgeteilten Daten] [ split] Modul im ursprüngliche Dataset in zwei Teile unterteilt: Teil, die zum Trainieren des Modells verwendet und Bereich reserviert ausgebildete Modell auf Leistung erzielen nicht auf trainieren. Geschulte Modell wird dann zum Testdaten Faktor, nach denen die Ergebnisse ausgewertet werden, um die Genauigkeit des Modells zu bestimmen.

[Bewertung Modell] [ score-model] Modul benötigt zwei Eingaben:

1. Eine Ausgabe ausgebildeten Modell [Zug Modell] [ train-model] Modul
2. Ein Bewertungssystem Dataset nicht Trainieren des Modells auf nicht

Es kann vorkommen, dass, obwohl der Versuch erfolgreich ist, [Bewertung Modell] [ score-model] Modul falsche Ergebnisse liefert. Mehrere Szenarien können dazu führen:

1. Die angegebene Bezeichnung ist eindeutig ein Regressionsmodells Daten geschult und eine falsche Ausgabe ausgegeben, [Bewertung Modell] [ score-model] Modul. Dies ist da Regression kontinuierlicher antwortvariable erforderlich ist. In diesem Fall sollte eine Klassifizierung Modell besser geeignet sein. 
2. Ebenso kann ein klassifizierungsmodell in ein Dataset mit Gleitkommazahlen in der Beschriftungsspalte geschult, unerwünschte Ergebnissen führen. Deswegen Klassifizierung eine separate antwortvariable benötigt, die nur diesen Bereich über eine begrenzte und meist etwas kleine Klassen zulässt.
3. Enthält Punktzahl Dataset nicht alle Funktionen, die zum Trainieren des Modells [Bewertung Modell] [ score-model] wird einen Fehler erzeugt.
4. [Bewertung Modell] [ score-model] erzeugt keine Ausgabe einer Zeile in dem Bewertungsmuster Dataset mit einem fehlenden Wert oder einen unendlichen Wert für alle Funktionen.
5. [Bewertung Modell] [ score-model] möglicherweise erzeugen identische Ausgaben für alle Zeilen im Dataset bewertet. Dies kann beispielsweise in der versucht, über die Anzahl der Schulung Beispiele Einstufung Entscheidung Gesamtstrukturen wird die Mindestanzahl der Proben pro Endknoten ausgewählt werden.


<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 
