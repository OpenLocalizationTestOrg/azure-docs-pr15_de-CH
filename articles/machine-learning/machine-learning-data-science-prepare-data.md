<properties
    pageTitle="Vorgänge zum Vorbereiten von Daten für maschinelles lernen | Microsoft Azure"
    description="Vor der Verarbeitung und Bereinigen von Daten für maschinelles lernen vorbereiten."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Vorgänge zum Vorbereiten von Daten für maschinelles lernen

Vorbereitung und Bereinigen von Daten sind wichtige Aufgaben, die in der Regel durchgeführt werden müssen, bevor Dataset für maschinelles lernen effektiv verwendet werden kann. Rohdaten ist oft laut und unzuverlässig und Werte fehlen. Mithilfe dieser Daten für die Modellierung kann irreführende Ergebnisse. Diese Aufgaben sind Teil des Team Daten Wissenschaft Prozess (TDSP) und folgen normalerweise eine erste Untersuchung des Datasets zum Erkennen und Planen der Vorbehandlung erforderlich. Nähere Informationen über den Vorgang des TDSP finden Sie die Schritte im [Team wissenschaftliche Daten](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Vorbehandlung und Reinigung Aufgabe Untersuchung Daten können in eine Vielzahl von Umgebungen, wie SQL oder Struktur Azure Machine Learning Studio und mit verschiedenen Tools und Sprachen wie R oder Python, je nachdem, wo die Daten gespeichert sind, und wie formatiert durchgeführt werden. Da TDSP iterative Natur ist erfolgt diese Aufgaben verschiedene Schritte im Workflow des Prozesses.

Dieser Artikel stellt verschiedene Datenverarbeitung Konzepte und Aufgaben, die vor oder nach der Einnahme Daten in Azure Computer durchgeführt werden.

Beispielsweise Daten und vor Verarbeitung Azure Machine Learning Studio finden Sie unter [Daten in Azure Machine Learning Studio vorverarbeitet](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) Video.


## <a name="why-pre-process-and-clean-data"></a>Warum vorverarbeitet und Bereinigen von Daten?

Reale Daten aus verschiedenen Quellen stammen und Prozesse und möglicherweise Verstöße oder beschädigte Daten beeinträchtigen die Qualität des Datasets. Die Regel Qualitätsprobleme sind:

* **Unvollständig**: Daten fehlen, Attribute oder fehlende Werte enthält.
* **Laut**: fehlerhafte Datensätze oder Ausreißer Daten enthält.
* **Inkonsistent**: Konflikt verursachenden Datensätze oder Diskrepanzen Daten enthält.

Daten ist eine Voraussetzung für Qualität Vorhersagemodelle. "Garbage in Müll" vermeiden und Verbesserung der Datenqualität daher Modell Leistung und unbedingt einen Health Datenbildschirm um Probleme frühzeitig zu erkennen und die entsprechenden Datenverarbeitung und Reinigung Schritte durchführen.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Was sind einige normalen Zustand Bildschirme, die eingesetzt werden?

Wir können die allgemeine Qualität der Daten überprüfen Überprüfen:

* Die Anzahl der **Datensätze**.
* Die Anzahl der **Attribute** (oder **Eigenschaften**).
* Die Attribut **-Datentypen** (Nennwert, Ordnungszahl oder kontinuierliche).
* Die Anzahl der **fehlenden Werte**.
* **Wohlgeformt** Daten.
    * Daten im TSV oder CSV-Format sicher, dass die Spalte Trennzeichen Trennlinien immer ordnungsgemäß Spalten und Zeilen trennen.
    * Wenn die Daten im HTML- oder XML-Format, überprüfen Sie, ob die Daten basierend auf ihren jeweiligen Standards wohlgeformt ist.
    * Analyse kann auch um strukturierte Informationen teilweise strukturierten oder unstrukturierten Daten erforderlich.
* **Inkonsistente Datensätze**. Überprüfen Sie der Wertebereich sind zulässig. Beispielsweise enthält die Daten Student GPA, überprüfen, ob das GPA im festgelegten Bereich sagen 0 ~ 4.

Wenn Sie Probleme mit Daten, **Verarbeitungsschritte** werden die beinhaltet häufig Reinigung Werte fehlen datennormalisierung Diskretisierung, Textverarbeitung zum Entfernen oder Ersetzen Sie eingebettete Zeichen die Ausrichtung der Daten beeinträchtigt, gemischte Datentypen gemeinsame Felder und andere.

**Azure Machine Learning verbraucht wohlgeformte Tabellendaten**.  Wenn die Daten bereits in Tabellenform, vor Verarbeitung direkt mit Azure Machine Learning Studio Learning Computer erfolgt.  Wenn Daten nicht tabellarisch sagen in XML, Analyse erforderlich, um die Daten in tabellarischer Form konvertieren möglicherweise.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Was sind die wichtigsten Aufgaben vor Verarbeitung?

* **Daten bereinigen**: ausfüllen oder fehlende Werte erkennen und laut Daten und Ausreißer zu entfernen.
* **Datentransformation**: Normalisieren von Daten, um Dimensionen und Rauschen reduzieren.
* **Reduzierung der Daten**: Beispiel Datensätze oder Attribute für einfachere Datenverarbeitung.
* **Daten Diskretisierung**: kontinuierliche Attribute kategorische Attribute für einfache Bedienung mit Methoden bestimmte Computer konvertieren.
* **Text Reinigung**: z. B. eingebettete Registerkarten eine tabulatorgetrennte Datei, neue Zeilen die Datensätze usw. unterbrechen können eingebettet eingebettete Daten Versatz verursachen Zeichen entfernen.

Die folgenden Abschnitte beschreiben einige Schritte Datenverarbeitung.

## <a name="how-to-deal-with-missing-values"></a>Umgang mit fehlenden Werten?

Fehlende Werte behandeln, empfiehlt es sich, zunächst dem Grund für die fehlenden Werte besser verarbeiten des Problems ermitteln. Normale fehlenden Wert Methoden sind:

* **Löschen**: Entfernen Sie die Datensätze mit fehlenden Werten
* **Dummy-Ersetzung**: Ersetzen Sie fehlende Werte mit dem Dummywert: z.B., _Unbekannte_ Kategorieliste oder 0 für numerische Werte.
* **Ersetzung bedeuten**: fehlenden Daten numerische ersetzen Sie fehlende Werte mit dem.
* **Häufige Ersetzung**: fehlenden Daten kategorischen ersetzen Sie fehlende Werte mit den häufigsten
* **Regression Ersetzung**: eine Regression-Methode verwenden, um fehlende Werte durch Regression ersetzen.  

## <a name="how-to-normalize-data"></a>Wie Daten normalisieren?

Datennormalisierung skaliert neu numerische Werte in einem angegebenen Bereich. Gängige Methoden für die Normalisierung gehören:

* **Min-Max-Normalisierung**: linear Transformieren der Daten auf einen Bereich z.B. zwischen 0 und 1, der Mindestwert 0 und der maximale Wert 1 skaliert.
* **Z-Wert Normalisierung**: Skalieren anhand Mittelwert und Standardabweichung: aufteilen den Unterschied zwischen den Daten und den Mittelwert Standardabweichung.
* **Decimal skalieren**: Skalierung die Daten durch Verschieben der Punkt des Attributwerts.  

## <a name="how-to-discretize-data"></a>Wie Daten Diskretisieren?

Daten können durch kontinuierliche Werte umwandeln nominale Attribute oder Intervallen diskretisiert werden. Einige dabei sind:

* **Gleiche Breite Binning**: Bereich aller möglichen Werte eines Attributs in N Gruppen dieselbe Größe und die Werte in einem Lagerplatz mit der Ablage zuweisen.
* **Gleiche Höhe Binning**: unterteilen den Bereich aller möglichen Werte eines Attributs N Gruppen jeweils die gleiche Anzahl von Instanzen und dann die Werte in einem Lagerplatz mit dem Wert zuweisen.  

## <a name="how-to-reduce-data"></a>Wie kann ich Daten?

Es gibt verschiedene Methoden zum Reduzieren der Größe für einfacher Daten. Je nach Größe und Bereich können folgendermaßen angewendet:

* **Datensatz Probenahme**: Beispiel Datensätze, und wählen Sie nur repräsentative Teilmenge von Daten.
* **Attribut Probenahme**: Daten nur eine Teilmenge der wichtigsten Attribute auswählen.  
* **Aggregation**: Daten in Gruppen zu unterteilen und für jede Gruppe speichern. Beispielsweise können die täglichen Umsatzzahlen Restaurantkette seit 20 Jahren monatlichen Einnahmen zum Reduzieren der Größe der Daten aggregiert werden.  

## <a name="how-to-clean-text-data"></a>Bereinigen von Textdaten wie?

**Textfelder in Tabellendaten** kann Zeichen enthalten Spalten Ausrichtung oder Datensatz Grenzen betreffen. Für z. B. Registerkarten in eine Tabstoppgetrennte Datei Ursache Spalte Versatz eingebettete und eingebettete Zeilenumbrüche Zeilenumbruchzeichen aufzeichnen. Falscher Text encoding Behandlung beim Schreiben/Lesen Text führt zum Verlust von Informationen, unbeabsichtigten Einführung nicht lesbare Zeichen, z. B. Null und kann auch auf Textanalyse. Sorgfältige Analyse und Bearbeitung möglicherweise um Textfelder für korrekt ausgerichtet oder strukturiert extrahieren Daten aus unstrukturierten oder halbstrukturierten Daten bereinigen.

**Durchsuchen von Daten** bietet einen ersten Einblick in die Daten. Eine Reihe von Problemen kann dabei aufgedeckt und entsprechende Methoden können angewendet werden, um diese Probleme zu beheben.  Es ist wichtig zu Fragen, was die Ursache des Problems ist und wie das Problem eingeschleppt worden sein könnte. Dadurch können Sie die Verarbeitungsschritte Daten entscheiden, die aufgelöst werden müssen. Welche Erkenntnisse will eine Ableitung von Daten kann auch verwendet werden, um die Datenverarbeitung Prioritäten festlegen.

## <a name="references"></a>Referenzen

>*Datamining: Konzepte und Techniken*, 3. Ausgabe, Morgan Kaufmann Jiawei Han, Micheline Kamber und Jian Pei 2011
