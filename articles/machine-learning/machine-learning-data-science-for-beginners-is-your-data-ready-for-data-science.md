<properties
   pageTitle="Kann Ihre Daten datenwissenschaft? Auswertung | Microsoft Azure"
   description="4 Kriterien für Daten für wissenschaftliche Daten enthält. Datenwissenschaft für Anfänger video 2 hat Beispiele Basisdaten Bewertung zu."
   keywords="Daten Auswerten von Daten, Daten, Kriterien, Daten vorbereiten"
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


# <a name="is-your-data-ready-for-data-science"></a>Kann Ihre Daten datenwissenschaft?

## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Datenwissenschaft für Einsteiger-Serie

Erfahren Sie, wie Ihre Daten, um sicherzustellen, dass er grundlegende Kriterien für wissenschaftliche Daten erfüllt.

Um die Serie optimal sehen sie alle. [Die Liste der videos](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-is-your-data-ready-for-data-science]

## <a name="other-videos-in-this-series"></a>Andere Videos dieser Reihe

*Wissenschaftliche Daten für Anfänger* ist eine kurze Einführung in datenwissenschaft in fünf kurzen Videos.

  * Video 1: [5 Fragen Daten Wissenschaft Antworten](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 Minuten)*
  * Video 2: Sind Ihre Daten für wissenschaftliche Daten bereit?
  * Video 3: [Frage Daten beantworten können](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 Minuten)*
  * Video 4: [Eine Antwort mit einem einfachen Modell vorhergesagt](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 Minuten)*
  * Video 5: [Die Arbeiten anderer datenwissenschaft dazu kopieren](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 Minuten)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Protokoll: Sind Ihre Daten für wissenschaftliche Daten bereit?

Willkommen Sie bei "Kann Daten für wissenschaftliche Daten?" Das zweite Video Serie *Datenwissenschaft für Anfänger*.  

Bevor datenwissenschaft Antworten kann Ihnen gewünschte, müssen Sie einige hochwertige Materialien arbeiten geben. Genau wie einer Pizza besser Zutaten beginnen mit besser Endprodukt.

## <a name="criteria-for-data"></a>Kriterien für Daten

Bei datenwissenschaft gibt also einige Bestandteile, die wir gemeinsam.

Wir benötigen Daten:

  * Relevante
  * Verbunden
  * Genaue
  * Zu arbeiten

## <a name="is-your-data-relevant"></a>Gilt Ihre Daten?

Wir brauchen also die erste Zutat - Daten.

![Daten und irrelevante Daten - auswerten Daten](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-relevant-and-irrelevant-data.png)

Betrachten Sie die Tabelle auf der linken Seite. Wir sieben getroffen außerhalb Boston leisten, ihre blutalkoholspiegel, Red Sox-Trefferquote in ihrem letzten Spiel und den Preis der Milch in die nächste Supermarkt gemessen.

Dadurch werden alle legitim. Der einzige Fehler ist nicht relevant. Gibt es keine offensichtliche Beziehung zwischen diesen Werten. Wenn Sie den aktuellen Preis der Milch und Red Sox-Trefferquote habe, besteht keine Möglichkeit mein blutalkoholgehalt erraten können.

Betrachten Sie nun die Tabelle ein, auf der rechten Seite. Dieses Mal messen wir jede Person Masse und gezählt, Getränke haben.  Die Zahlen in jeder Zeile sind jetzt für einander. Wenn ich meine Stelle gab Masse und die Anzahl der Margaritas habe können eine Schätzung Blut Alkohol Inhalt.

## <a name="do-you-have-connected-data"></a>Verbundener Sie haben Daten?

Die nächsten angeschlossenen ist.

![Daten und getrennte Daten - Kriterien, Daten verbunden](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-connected-vs-disconnected-data.png)

Hier werden relevanten Daten über die Qualität des Hamburger: grill Temperatur, bereitet Gewichtung und Bewertung in der lokalen Magazin. Aber die Lücken in der Tabelle auf der linken Seite.

Die meisten Datensätze fehlen einige Werte. Häufig Löcher sieht und stehen diese zu umgehen. Aber ist viel fehlt, Daten wie Käse.

Wenn Sie die Tabelle auf der linken Seite betrachten, gibt es so viel Daten fehlen, um jede Art von Beziehung zwischen Grill Temperatur und bereitet ist. Dies ist ein Beispiel für getrennte Daten.

Die Tabelle auf der rechten Seite voll ist und abgeschlossen - Beispiel verbundenen Daten.

## <a name="is-your-data-accurate"></a>Sind die Daten?

Die nächste benötigten ist Genauigkeit. Hier sind vier Ziele wir mit Pfeilen drücken möchten.

![Daten im Vergleich zu ungenauen Daten - Kriterien](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-inaccurate-vs-accurate-data.png)

Sehen Sie sich das Ziel in der oberen rechten Ecke. Wir haben eine enge Gruppierung um schwarze. Das ist natürlich genau. Seltsamerweise in der Sprache der datenwissenschaft unserer Leistung darunter rechts Ziel genau gilt.

Würde sich die Mitte der Pfeile sehen Sie schwarze ist. Die Pfeile sind verteilt aller Ziel, sie sind als ungenau, aber sie sind schwarze, Mittelpunkt, so dass sie korrekt sind.

Betrachten Sie nun das linke obere Ziel. Hier treffen unsere Pfeile sehr eng nebeneinander enge Gruppierung. Sie sind genau, aber sie sind ungenau ist die Mitte Weg ins schwarze. Und natürlich die Pfeile im unteren linken Ziel ungenau und ungenau. Diese Archer benötigt mehr üben.

## <a name="do-you-have-enough-data-to-work-with"></a>Haben Sie genug Daten arbeiten?

Schließlich müssen Bestandteil #4 – wir genügend Daten.

![Haben Sie genügend Daten für die Analyse? Auswertung](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-barely-enough-data.png)

Betrachten Sie jeden Datenpunkt in der Tabelle als Pinselstrich in einer Zeichnung. Wenn nur wenige, Zeichnung kann ziemlich fuzzy - es schwer ist zu sagen, was.

Wenn Sie einige weitere Pinselstriche hinzufügen, wird die Zeichnung etwas schärfer zu gestartet.

Wenn kaum Striche sind, sehen Sie gerade genug Breite entscheiden. Ist es irgendwo besuchen möchte? Es sieht hell, sieht sauberes Wasser – Ja, das ist, wo ich in Urlaub.

Weitere Daten hinzufügen, wird das Bild klarer und weitere Entscheidungen. Ich kann die Hotels am linken schauen. Sie wissen, ich mag Architekturmerkmalen in den Vordergrund. Ich bleibe, im dritten Stock.

Mit Daten, relevante, präzise verbunden, alle Inhaltsstoffe müssen wir haben qualitativ hochwertige datenwissenschaft.

Achten Sie darauf, dass Sie vier Videos im *Wissenschaftlichen Daten für Anfänger* von Microsoft Azure Computer überprüfen.




## <a name="next-steps"></a>Nächste Schritte

  * [Versuch einer ersten Daten Wissenschaft mit Computer lernen](machine-learning-create-experiment.md)
  * [Einführung in Computerlernen auf Microsoft Azure](machine-learning-what-is-machine-learning.md)
