<properties
   pageTitle="Eine Antwort mit einem einfachen Modell - Regression vorhergesagt | Microsoft Azure"
   description="Erstellen Sie ein einfache Regressionsmodell Preis im Daten für Anfänger video 4 Vorhersagen Enthält eine lineare Regression mit Zieldaten."                                  
   keywords="Erstellen Sie ein Modell, einfaches Modell preisvorhersage, einfache Regressionsmodell"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="predict-an-answer-with-a-simple-model"></a>Eine Antwort mit einem einfachen Modell vorhergesagt

## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Datenwissenschaft für Einsteiger-Serie

Enthält Informationen zum Erstellen eines einfachen Regressionsmodells Preis eine Raute im Daten für Anfänger video 4 Vorhersagen. Wir ziehen ein Regressionsmodells mit Zieldaten.

Um die Serie optimal sehen sie alle. [Die Liste der videos](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-predict-an-answer-with-a-simple-model]

## <a name="other-videos-in-this-series"></a>Andere Videos dieser Reihe

*Wissenschaftliche Daten für Anfänger* ist eine kurze Einführung in datenwissenschaft in fünf kurzen Videos.

  * Video 1: [5 Fragen Daten Wissenschaft Antworten](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 Minuten)*
  * Video 2: [kann Ihre Daten datenwissenschaft?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 56 Minuten)*
  * Video 3: [Frage Daten beantworten können](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 Minuten)*
  * Video 4: Eine Antwort mit einem einfachen Modell vorhergesagt
  * Video 5: [Die Arbeiten anderer datenwissenschaft dazu kopieren](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 Minuten)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Protokoll: Eine Antwort mit einem einfachen Modell vorhergesagt

Willkommen Sie bei des vierten Videos "Daten Wissenschaft für Anfänger" Serie. In diesem wir ein einfaches Modell und eine Vorhersage.

Ein *Modell* ist eine vereinfachte Geschichte über unsere Daten. Ich zeige Ihnen, was ich meine.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Sammeln relevante, präzise, verbunden, genügend Daten

Angenommen Sie, Shop für eine Raute möchte. Einen Ring Großmutter mit einer Einstellung für eine Raute 1,35 Karat gehört habe und wie viel kostet sehen möchte. Nehme einen Editor und einen Stift-Speicher Schmuck und den Preis aller Diamanten und wie viel sie Karat wiegen schreibe. Beginnend mit der erste Diamant - des 1.01 Karat und $7,366.

Ich durchlaufen und dies gilt für alle Diamanten im Speicher.

![Diamond Datenspalten](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Beachten Sie, dass unsere zwei Spalten. Jede Spalte besitzt ein anderes Attribut - wiegt Karat Preis - und jede Zeile einen einzelnen Datenpunkt, der eine Raute darstellt.

Wir haben wirklich ein kleines-Datensatz eine Tabelle. Beachten Sie, dass unsere für Qualität Kriterien:

* **Relevante** - Daten Gewicht definitiv bezieht sich auf
* **Genaue** - wir überprüft die Preise, die wir aufschreiben
* **Verbunden** - sind keine Leerzeichen in beiden Spalten
* Und wir sehen unsere Antwort **genug** Daten

## <a name="ask-a-sharp-question"></a>Scharfe Fragen

Nachdem wir unsere scharfe so Frage werden: "wie viel kostet es, 1,35 Karat Diamant kaufen?"

Die Liste ist eine Raute 1,35 Karat, müssen wir unsere Daten verwenden, um eine Antwort auf die Frage.

## <a name="plot-the-existing-data"></a>Zeichnen Sie die vorhandenen Daten

Das erste, was, das wir tun, ist eine horizontale Position, eine Achse, um das Gewicht Diagramm aufgerufen. So zeichnen wir eine Zeile umfasst, die den Bereich und Teilstriche für jede Hälfte Karat ist an die Gewichte 0 bis 2.

Als Nächstes erstellen wir eine vertikale Achse Preis und Gewicht horizontale Achse herstellen. Diese werden in Einheiten von Dollar. Jetzt haben wir eine Reihe von Koordinatenachsen.

![Preis- und Achsen](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Wir werden diese Daten jetzt und daraus ein *Punkt gezeichnet*. Dies ist eine hervorragende Möglichkeit, numerische Datensätze darstellen.

Für den ersten Datenpunkt erfassen wir eine vertikale Linie an 1.01 Karat. Anschließend erfassen wir eine horizontale Linie auf $7,366. Wo erreichen, zeichnen wir einen Punkt. Dies ist unsere erste Raute.

Wir jede Raute auf dieser Liste durchlaufen und das gleiche tun. Wenn wir damit fertig sind, dies ist, was wir erhalten: eine Reihe von Punkten, für jeden Raute.

![Punktdiagramm](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Ziehen Sie das Modell über die Datenpunkte

Nun betrachten Sie Punkte und schielen, sieht die Auflistung eine Zeile fett, unscharf. Wir können unsere Marke und zeichnen eine gerade Linie.

Durch eine Linie erstellt ein *Modell*. Stellen Sie sich als die reale Welt und eine vereinfachte Cartoon Version. Jetzt Cartoon ist falsch - die Zeile nicht alle Datenpunkte durchlaufen. Aber es ist eine nützliche Vereinfachung.

![Regressionsgeraden](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Die Tatsache, dass die Punkte genau über der Zeile nicht ist OK. Datenwissenschaftler erklären, daß - das der Zeile - Modell und dann jeden Punkt einige *Rauschen* oder *Varianz* zugeordnet. Zugrunde liegende perfekte Beziehung besteht, und gibt an, reale Welt, die Lärm und Unsicherheit hinzufügt.

Da wir versuchen, die Frage *wie viel?* ist dies eine *Regression*bezeichnet. Und da wir gerade Linie verwenden, wird eine *lineare Regression*.

## <a name="use-the-model-to-find-the-answer"></a>Verwenden Sie das Modell der Antwort

Jetzt wir ein Modell haben und fordern sie die Frage: wie viel kostet 1,35 Karat Diamanten?

Unsere Frage wir 1,35 Karat erfassen und eine vertikale Linie. Schnittpunkt der Linie Modell erfassen wir eine horizontale Linie an der Achse Dollar. Es trifft rechts 10.000. Boom! Die Antwort ist: 1,35 Karat Diamanten Kosten über $10.000.

![Finden Sie die Antwort im Modell](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Erstellen Sie ein Konfidenzintervall

Es ist natürlich Fragen, wie präzise Vorhersage. Es ist hilfreich zu wissen, ob 1,35 Karat Diamanten sehr nahe $10.000 oder viel höher oder niedriger. Um dies herauszufinden, zeichnen wir einen Umschlag in der Regressionsgeraden, die meisten Punkte enthält. Umschlag *Konfidenzintervall*aufgerufen: Wir sind sehr zuversichtlich, dass diesem Umschlag Preise fallen, weil in den letzten von ihnen haben. Können wir mehr Linie vom Schnittpunkt der 1,35 Karat Linie auf der oberen und unteren Rand, der Umschlag.

![Konfidenzintervall](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Jetzt können wir etwas über unsere Konfidenzintervall sagen: kann man getrost, ist eine Raute 1,35 Karat über $10.000 - jedoch möglicherweise so niedrig wie $8.000 möglicherweise bis zu 12.000.

## <a name="were-done-with-no-math-or-computers"></a>Mathematik keine Computer fertig

Wir haben welche datenwissenschaftler bezahlt werden und wir haben sie gerade zeichnen:

* Wir Frage eine, dass wir mit Daten beantworten
* Es erstellt ein *Modell* *lineare regression*
* Wir haben eine *Vorhersage*, mit *Konfidenzintervall*

Und wir haben mathematischen oder Computer dazu.

Nun hätten wir weitere Informationen wie...

* Ausschneiden des Diamanten
* Farbvariationen (wie nahe die Raute ist weiß)
* die Anzahl der Einschlüsse im Diamant

... dann hätten wir mehr Spalten. In diesem Fall wird die mathematische hilfreich. Wenn Sie mehr als zwei Spalten haben, ist es schwierig Punkte auf Papier zeichnen. Math können Sie sehr gut, Linie oder auf dieser Ebene passen.

Auch statt nur wenige Karo, wir hatten 2000 zwei Millionen, dann möglich, dass die Arbeit schneller mit einem Computer.

Heute haben wir wie linearen Regression gesprochen, und wir eine Vorhersage mit Daten.

Achten Sie darauf, dass Sie Videos "Daten Wissenschaft für Anfänger" von Microsoft Azure Computer überprüfen.



## <a name="next-steps"></a>Nächste Schritte

  * [Versuch einer ersten Daten Wissenschaft mit Computer lernen](machine-learning-create-experiment.md)
  * [Einführung in Computerlernen auf Microsoft Azure](machine-learning-what-is-machine-learning.md)
