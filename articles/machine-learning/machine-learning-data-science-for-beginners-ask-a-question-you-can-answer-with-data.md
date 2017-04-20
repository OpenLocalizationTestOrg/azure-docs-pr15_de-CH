<properties
   pageTitle="Frage können Sie beantworten, mit Daten - formulieren Fragen | Microsoft Azure"
   description="Erfahren Sie, wie eine Frage Daten wissenschaftliche Datenwissenschaft für Anfänger video 3 zu formulieren. Enthält einen Vergleich der Klassifizierung und Regression Fragen."
   keywords="Daten wissenschaftliche Fragen formulieren Fragen, Regression Fragen, Fragen zur, scharfe Frage"
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

# <a name="ask-a-question-you-can-answer-with-data"></a>Stellen Sie eine Frage mit Daten beantworten

## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Datenwissenschaft für Einsteiger-Serie

Erfahren Sie, wie eine Frage Daten wissenschaftliche Datenwissenschaft für Anfänger video 3 zu formulieren. Dieses Video enthält einen Vergleich der Fragen für Algorithmen Klassifizierung und Regression.

Um die Serie optimal sehen sie alle. [Die Liste der videos](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-ask-a-question-you-can-answer-with-data]

## <a name="other-videos-in-this-series"></a>Andere Videos dieser Reihe

*Wissenschaftliche Daten für Anfänger* ist eine kurze Einführung in datenwissenschaft in fünf kurzen Videos.

  * Video 1: [5 Fragen Daten Wissenschaft Antworten](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 Minuten)*
  * Video 2: [kann Ihre Daten datenwissenschaft?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 56 Minuten)*
  * Video 3: Stellen Sie eine Frage mit Daten beantworten
  * Video 4: [Eine Antwort mit einem einfachen Modell vorhergesagt](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 Minuten)*
  * Video 5: [Die Arbeiten anderer datenwissenschaft dazu kopieren](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 Minuten)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Protokoll: Frage Daten beantworten

Willkommen Sie bei des dritten Videos in der Reihe "Datenwissenschaft für Anfänger".  

In diesem erhalten Sie einige Tipps für die Formulierung einer Frage mit Daten beantworten.

Sie erhalten mehr aus diesem Video Wenn Sie zunächst die zwei früheren in diesen Videos: "wissenschaftliche Daten 5 Fragen beantworten" und "Ist Ihre Daten Wissenschaft bereit?"

## <a name="ask-a-sharp-question"></a>Scharfe Fragen

Wir haben bereits wie datenwissenschaft mit Namen (auch als Kategorien oder Etiketten) und eine Antwort auf eine Frage vorherzusagen ist. Aber kein Frage; Es muss ein *scharfe Frage.*

Eine vage Frage muss mit einem Namen oder einer Nummer beantwortet werden. Eine scharfe Frage muss.

Angenommen Sie, Sie Wunderlampe mit einem Genie wahrheitsgemäß Fragen beantworten Sie Fragen gefunden. Aber es Makrosprachen Genie, und er versuchen, seine Antwort vage und verwirrend wie er durchkommen. Sie möchten zu ihm eine Frage so luftdicht, dass er nicht helfen Ihnen sagen, was Sie wissen möchten.

Würden Sie Fragen vage, wie "Was wird mit meiner?" das Genie beantworten kann, "der Preis ändert". Das ist eine ehrliche Antwort, aber ist nicht sehr hilfreich.

Aber wenn Sie eine Frage scharfe, wie "Was Meine Aktien Verkaufspreis nächste Woche werden?", das Genie Hilfe nicht geben einen bestimmten beantworten und einen Verkaufspreis vorherzusagen.

## <a name="examples-of-your-answer-target-data"></a>Beispiele für die Antwort: Zieldaten

Nach Sie Ihre Frage formulieren überprüfen Sie, ob Sie Beispiele für die Antwort in den Daten haben.

Ist die Frage "Was Meine Aktien Verkaufspreis nächste Woche werden?" dann müssen wir sicherstellen, dass unsere Daten Aktienkurs-Verlauf enthält.

Ist die Frage "welche Auto in meinem Flotte wird zuerst fehlschlagen?" dann müssen wir sicherstellen, dass unsere Daten Informationen zu vorherigen Fehler enthält.

![Zieldaten - Beispiele für Ihre Antwort. Frage wissenschaftliche Daten zu formulieren.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-target-data.png)

Diese Beispiele Antworten werden Ziel bezeichnet. Ein Ziel ist darum zu zukünftigen Datenpunkten vorhergesagt, ob eine Kategorie oder eine Zahl ist.

Wenn Sie Zieldaten haben, müssen Sie einige erhalten. Nicht werden ohne Ihre Frage beantwortet.

## <a name="reformulate-your-question"></a>Formulieren Sie Ihre Frage

Manchmal können Sie Ihre Frage ein nützlicher Antwort zu formulieren.

Die Frage "dieser Daten Punkt A oder B?" die Kategorie oder Namen oder Bezeichnung etwas prognostiziert. Beantworten, verwenden Sie eine *klassifizierungsalgorithmus*.

Die Frage "Wie viel?" oder "Wie viele?" einen Betrag geschätzt. Sie beantworten verwenden wir eine *Regression-Algorithmus*.

Um zu sehen, wie wir diese umwandeln können, sehen wir uns die Frage "welche Nachricht ist diese Leser?" Es fragt eine einzelne Auswahl von viele - Vorhersage heißt "Diese A oder B oder C oder D?" - und klassifizierungsalgorithmus.

Aber diese Frage ist möglicherweise leichter zu beantworten, wenn sie als formulieren "wie jede Story auf dieser Liste dieser Reader ist interessant?" Jetzt können alle Artikel einer Punktzahl und ist dann leicht zu identifizieren, den höchste Punktzahl Artikel. Das Formulieren der Frage Einstufung in eine Regression Frage ist oder wie viel?

![Formulieren Sie Ihre Frage ein. Frage Klassifizierung oder Regression Frage.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-classification-question-vs-regression-question.png)

Wie bitten, eine Frage einen Hinweis, welcher Algorithmus können Sie eine Antwort erhalten.

Sie werden feststellen, dass bestimmte Familien Algorithmen – wie in unserem Beispiel News Story - eng miteinander verbunden sind. Formulieren Sie Ihre Frage um Algorithmus verwenden, der besonders antwortet.

Aber am wichtigsten, scharfen Frage - die Frage, die Sie mit Daten beantworten können. Und sicher, dass Sie die richtigen Daten beantworten.

Wir haben einige Grundlagen für eine Frage beantworten können Sie Daten gesprochen.

Achten Sie darauf, dass Sie Videos "Daten Wissenschaft für Anfänger" von Microsoft Azure Computer überprüfen.


## <a name="next-steps"></a>Nächste Schritte

  * [Versuch einer ersten Daten Wissenschaft mit Computer lernen](machine-learning-create-experiment.md)
  * [Einführung in Computerlernen auf Microsoft Azure](machine-learning-what-is-machine-learning.md)
