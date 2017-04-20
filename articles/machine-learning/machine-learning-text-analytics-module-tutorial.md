<properties
    pageTitle="Erstellen von Text Analytics Modelle in Azure Machine Learning Studio | Microsoft Azure"
    description="Text in Azure Machine Learning Studio mit Modulen für Text vorverarbeiten, N-Grams oder Feature hashing Analytics Modelle erstellen"
    services="machine-learning"
    documentationCenter=""
    authors="rastala"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="roastala" />


#<a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Erstellen von Text Analytics Modelle in Azure Machine Learning Studio

Sie können Azure Machine Learning erstellen und durchsetzen Text Analytics Modelle. Diese Modelle können, z. B. Dokument Klassifizierung oder Stimmung Analyse Problemen helfen.

In einem Experiment Text Analytics würden Sie normalerweise:

 1. Reinigen und Text Dataset vorverarbeiten
 2. Numerische Funktion Vektoren vorverarbeitete Text extrahieren
 3. Zug Klassifizierung oder Regression Modell
 4. Bewerten und Validieren des Modells
 5. Bereitstellen des Modells zur Produktion

In diesem Lernprogramm lernen Sie Schritte gehen über eine Stimmung Modell mit Amazon Buchbesprechungen Dataset (Siehe diese Forschungsarbeit "Biographien, Bollywood, Boom Felder und Mixer: Domäne Anpassung Stimmung Klassifizierung" John Blitzer Mark Dredze und Fernando Pereira; Zuordnung von Computerlinguistik (ACL) 2007.) Dieses Dataset besteht aus Überprüfung Faktoren (1-2 oder 4 und 5) und ein freier Text. Soll das Ergebnis überprüfen Vorhersagen: Niedrig (1 - 2) oder hoch (4-5).

In diesem Lernprogramm Cortana Intelligence Galerie versuchen finden:

[Vorhersagen Sie Buchbesprechungen] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Vorhersagen Sie Buchbesprechungen - Vorhersageexperiment] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Schritt 1: Reinigen und Text Dataset vorverarbeiten

Beginn des Experiments Ergebnisse überprüfen in kategorisierten niedrigen und hohen Buckets formulieren Sie das Problem als zwei Klassen Klassifizierung aufteilen. Wir verwenden [Metadaten bearbeiten] (https://msdn.microsoft.com/library/azure/dn905986.aspx) und [Gruppe kategorischen Werten] (https://msdn.microsoft.com/library/azure/dn906014.aspx).

![Beschriftung erstellen](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Dann Reinigen Sie den Text mit Modul [vorverarbeiten Text] (https://msdn.microsoft.com/library/azure/mt762915.aspx). Die Reinigung Reduzierung des Rauschens im Dataset, helfen Ihnen die wichtigsten Funktionen und verbessern die Genauigkeit der Modell. Wir entfernen Stoppwörter - allgemeine Wörter wie "" oder "a" - und Zahlen, Sonderzeichen, duplizierten Zeichen, e-Mail-Adressen und URLs. Wir Umwandeln von Text in Kleinbuchstaben, Wörter kindgerechte und Satz Berandungen, die dann vom angegebenen erkennen "|||" Symbol vorverarbeitete Text.

![Vorverarbeiten von Text](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Wenn Sie eine benutzerdefinierte Liste von Stoppwörtern verwenden möchten? Sie können es als optionale Eingabe übergeben. Sie können auch benutzerdefinierten C# regulären Ausdruck verwenden, um Teilzeichenfolgen ersetzen oder entfernen Wörter Wortart: Substantive, Verben und Adjektiven.

Nach Abschluss der preprocessing wir Schulen die Daten aufgeteilt und legt testen.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Schritt 2: Extrahieren Sie numerische Funktion Vektoren aus bereits verarbeiteten text

Zum Erstellen eines Modells für Textdaten müssen Sie i. d. r. Freitext in numerische Funktion Vektoren zu konvertieren. In diesem Beispiel verwenden wir [N-Gram-Features aus Text extrahieren] Modul Textdaten in solchen Format transformieren (https://msdn.microsoft.com/library/azure/mt762916.aspx). Dieses Modul hat eine Spalte mit Leerzeichen getrennte Wörter und berechnet ein Wörterbuch mit Wörtern oder N-Grams der Wörter, im Dataset. Dann zählt es zählt, wie viele Male jedes Wort oder N-Gram in jedem Datensatz angezeigt wird und die Funktion Vektoren erstellt. In diesem Lernprogramm festlegen wir N-Gram-Größe auf 2, damit unsere Feature Vektoren einzelne Wörter und zwei nachfolgenden Wortkombinationen enthalten.

![N-Grams extrahieren](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

Wir übernehmen TF * N-Gram Gewichtung IDF (Begriff Häufigkeit Inverse Document Frequency) zählt. Diese Methode fügt Gewicht von Wörtern, die häufig in einem einzelnen Datensatz angezeigt aber selten über das gesamte Dataset. Andere Optionen enthalten Binary TF und graph wiegen.

Textfunktionen haben oft hohe Dimensionalität. Beispielsweise verfügt die Corpus 100.000 eindeutigen Wörter, hätte Space Feature 100.000 Dimensionen oder mehr N-Grams verwendet werden. Extrahieren der N-Gram-Features-Modul bietet eine Reihe von Optionen zum Reduzieren der Anzahl der Dimensionen. Sie können Wörter ausschließen, die kurz oder lang zu selten oder zu häufig zu erheblichen Vorhersagewert sind. In diesem Lernprogramm schließen wir N-Grams, die weniger als 5 Datensätze oder in mehr als 80 % der Datensätze angezeigt werden.

Auswahl von Features können Sie auch nur die Features auszuwählen, die korrelierte mit der Vorhersage. Wir verwenden Chi Featureauswahl 1000 Features auswählen. Durch Klicken auf die korrekte Ausgabe extrahieren N-Grams Modul können Sie das Vokabular von ausgewählten Wörtern oder N-Grams anzeigen.

Als Alternative zum Extrahieren N-Gram Funktionen können Sie Feature Hashing-Modul. Beachten Sie jedoch, [Feature Hashing] (https://msdn.microsoft.com/library/azure/dn906018.aspx) keine integrierte Funktion Auswahlfunktionen oder TF * IDF wiegen.

## <a name="step-3-train-classification-or-regression-model"></a>Schritt 3: Klassifizierung oder Regression Modell trainieren

Der Text wurde nun numerische Funktion Spalten transformiert. Das Dataset enthält noch Zeichenfolgenspalten aus früheren Phasen, so wählen Sie Spalten im Dataset wir verwenden, sie auszuschließen.

Wir verwenden dann [zwei Klassen logistische Regression] (https://msdn.microsoft.com/library/azure/dn905994.aspx) Vorhersagen Ziel: Hoch oder niedrig Bewertungsergebnis. An diesem Punkt wurde Text Analytics Problem in regulären klassifizierungsproblem transformiert. In Azure Machine Learning verfügbaren Tools können Sie das Modell verbessern. Beispielsweise können Sie experimentieren mit verschiedenen Klassifizierer zu wie präzise Ergebnisse geben sie oder verwenden Hyperparameter angepasst werden, um die Genauigkeit zu verbessern.

![Zug und Bewertung](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Schritt 4: Bewerten Sie und validieren Sie des Modells

Wie prüfen Sie geschulte Modell? Wir gegen das Test-Dataset Punktzahl und die Genauigkeit auswerten. Allerdings haben das Modell des Vokabulars der N-Grams und ihrem jeweiligen Gewicht von Trainings-Dataset. Daher sollten wir diese Begriffe und die Gewichte verwenden Funktionen von Testdaten, anstatt das Vokabular neu extrahieren. Daher wir scoring Zweig des Experiments extrahieren N-Gram Features Modul hinzufügen verbinden Ausgabe Vokabular aus Training und Vokabular Modus auf schreibgeschützt festgelegt. Wir Deaktivieren der Filterung von N-Grams Frequenz von mindestens 1 Instanz und auf 100 % festlegen und die Auswahl von Features deaktivieren.

Nach der Transformation der Textspalte im Testdaten numerische Funktion Spalten Zeichenfolgenspalten aus früheren Phasen wie Training Zweig ausgeschlossen. Wir verwenden dann Modul Score Model Vorhersagen und Modul Modell evaluieren die Genauigkeit auswerten.

## <a name="step-5-deploy-the-model-to-production"></a>Schritt 5: Bereitstellen des Modells zur Produktion

Das Modell ist fast fertig für die Produktion bereitgestellt werden. Wenn als Webdienst bereitgestellt, es formfreie Zeichenfolge als Eingabe und zurückgeben eine Vorhersage "Hoch" oder "Niedrig". Das Gelernte N-Gram Vokabular verwendet umzuwandelnde Text Funktionen und geschulten logistische Regressionsmodell Vorhersagen von diesen Funktionen. 

Zum Einrichten der vorhersageexperiment speichern wir zunächst der N-Gram Vokabular als Dataset und geschulten logistische Regressionsmodell Schulung Teil des Experiments. Anschließend speichern wir mit "Speichern unter" ein Experiment Diagramm für vorhersageexperiment zu experimentieren. Wir entfernen Moduls aufteilen und die Schulung aus dem Experiment. Wir verbinden dann gespeicherte N-Gram-Vokabular und Modell mit N-Gram-Funktionen extrahieren und Bewertung Modell Module bzw.. Wir entfernen auch Modul Modell evaluieren.

Wir legen Sie Spalten auswählen im Dataset Modul vor vorverarbeiten Textbaustein Beschriftungsspalte entfernen und deaktivieren "Append Ergebnisspalte Dataset" Option Bewertung darüber. So fordert der Webdienst nicht, die Beschriftung, die sie vorhersagen, jedoch nicht die Eingabe auf Funktionen Echo.

![Vorhersageexperiment](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Jetzt haben wir ein Experiment, das als Webdienst veröffentlicht und mit Anforderung-Antwort oder Batch Execution APIs aufgerufen werden kann.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Text Analytics Module [MSDN-Dokumentation] (https://msdn.microsoft.com/library/azure/dn905886.aspx).
