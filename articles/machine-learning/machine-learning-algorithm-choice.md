<properties
    pageTitle="Algorithmen für maschinelles lernen auswählen | Microsoft Azure"
    description="Auswahl Azure Machine Learning Algorithmen für beaufsichtigten und unbeaufsichtigten lernen in Clustern, Klassifizierung oder Regression Experimente."
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Algorithmen für Microsoft Azure Machine Learning auswählen

Die Antwort auf die Frage "Welche Algorithmus für maschinelles lernen soll ich verwenden?" ist immer "es." Es hängt von der Größe, Qualität und Art der Daten. Es hängt davon ab, was soll mit der Antwort. Das hängt davon ab, wie die mathematischen Algorithmus Anleitung für den Computer wurde übersetzt verwendeten. Und wie viel Zeit Sie haben. Sogar die erfahrensten Datenanalysten Algorithmus am besten durchführt, bevor Sie versuchen, sie ist nicht erkennbar.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Spickzettel: Machine Learning-Algorithmus

**Microsoft Azure Machine Learning Algorithmus Spickzettel** hilft Ihnen, die richtigen Algorithmus für maschinelles lernen für Vorhersageanalysen Projektmappen aus der Bibliothek Microsoft Azure Machine Learning Algorithmen auszuwählen.
Dieser Artikel führt Sie durch Ihre Verwendung.

> [AZURE.NOTE] Spickzettel: herunterladen und in diesem Artikel folgen zu wechseln [maschinelles lernen Algorithmus Spickzettel für Microsoft Azure Machine Learning Studio](machine-learning-algorithm-cheat-sheet.md)

Dieses Blatts hat eine ganz bestimmte Zielgruppe beachten: Anfang datenwissenschaftler mit Studenten auf Computer, einen Algorithmus zu Azure Machine Learning Studio auswählen möchten. Dies bedeutet, dass es einige verallgemeinerungen und vereinfachungen, aber es wird Sie in eine sichere Richtung zeigen. Dies bedeutet auch, dass es viele Algorithmen, die hier nicht aufgeführt. Mit zunehmender Azure Machine Learning auf einen vollständigeren Satz der verfügbaren Methoden fügen wir sie.

Diese Konfigurationen sind kompilierte Feedback und Tipps von vielen Daten und Computer Learning Experten. Wir nicht einig, aber ich habe Meinung in groben Konsens zu harmonisieren. Die meisten Anweisungen Uneinigkeit beginnen mit "kommt es..."

### <a name="how-to-use-the-cheat-sheet"></a>Verwendung der Spickzettel

Lesen Sie die Etiketten Pfad und Algorithmus im Diagramm als "für * &lt;Bezeichnung&gt; * verwenden * &lt;Algorithmus&gt;*." "Z. B. *Geschwindigkeit* *zwei Klasse logistische Regression*." Manchmal mehr als eine Verzweigung gelten.
Manchmal werden keine perfekt. Sie sind soll der Regel empfohlen, so genau zu sorgen.
Datenwissenschaftler Sprach ich mit sagte, das ist des einzige Weg zu den besten Algorithmus versuchen alle.

Hier ist ein Beispiel aus der [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) ein Versuch, die versucht, mehrere Algorithmen für dieselben Daten und vergleicht die Ergebnisse: [Vergleichen mit mehreren Klasse Klassifizierer: Buchstabe Recognition](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

>[AZURE.TIP] Zum Downloaden und Drucken ein Diagramm, das eine über die Funktionen von Machine Learning Studio Übersicht anzeigen Sie [Übersichtsdiagramm Azure Machine Learning Studio Funktionen](machine-learning-studio-overview-diagram.md)

## <a name="flavors-of-machine-learning"></a>Arten von computerlernen

### <a name="supervised"></a>Überwacht

Beaufsichtigte lernalgorithmen Vorhersagen anhand von Beispielen. Beispielsweise können Aktienverlaufskurse, Gefahr geraten zukünftigen Preisen verwendet werden. Jedes Beispiel Ausbildung trägt den Wert an, in diesem Fall den Aktienkurs. Beaufsichtigte lernalgorithmus sucht nach Mustern in diesen Wert Etiketten. Können alle relevanten Informationen – den Tag der Woche der Saison Finanzdaten des Unternehmens, Typ der Branche, das Vorhandensein disruptive geopolitischen Ereignisse, und jeder Algorithmus für verschiedene Muster. Nachdem der Algorithmus das beste Muster gefunden hat kann, verwendet das Muster unbeschriftete Daten Vorhersagen – morgen Preise.

Dies ist eine gängige und nützliche maschinelles lernen. Mit einer Ausnahme sind alle Module in Azure Machine Learning lernen Algorithmen überwacht. Gibt bestimmte Arten der überwachten lernen, die in Azure Machine Learning dargestellt werden: Klassifizierung, Regression und Anomalien erkennen.

* **Klassifizierung**. Wenn die Daten eine Kategorie Vorhersagen verwendet werden, beaufsichtigte lernen Klassifizierung steht. Dies ist der Fall, wenn ein Bild als Bild oder eine Katze "Hund" zuweisen. Wenn nur zwei Auswahlmöglichkeiten vorhanden sind, wird diese **zwei Klassen** **binomial Klassifizierung**bezeichnet. Es sind weitere Kategorien wie bei der Vorhersage des Gewinner des Turniers NCAA March Madness wird **Multi-Klasse Klassifikation**dieses Problem genannt.

* **Regression**. Wenn ein Wert, mit Aktienkursen prognostiziert wird, heißt beaufsichtigte lernen Regression.

* **Erkennen von Netzwerkanomalien**. Manchmal soll Daten identifizieren, die einfach sind. In Betrugsversuche sind beispielsweise alle ungewöhnlichen Kreditkarte Ausgabegewohnheiten fehlerverdächtig. Die Varianten sind so zahlreich und so einige ist nicht möglich, die betrügerische Aktivitäten lernen Trainingsbeispiele aussieht. Der Ansatz Normalbetriebswerte ist zu einfach normaler Aktivitäten (mit einem Verlauf nicht betrügerische Transaktionen) sieht etwas wesentlich zu identifizieren.

### <a name="unsupervised"></a>Unbeaufsichtigt

Unbeaufsichtigte lernen enthalten Datenpunkte keine Etiketten zugeordnet. Stattdessen ist das Ziel eines Algorithmus unbeaufsichtigte lernen organisieren die Daten oder die Struktur. Dies kann bedeuten, in Clustern zu gruppieren oder andere Wege komplexe Daten betrachten, sodass einfacher und übersichtlicher angezeigt wird.

### <a name="reinforcement-learning"></a>Verstärkung lernen

Verstärkung lernen wird der Algorithmus zum Auswählen einer Aktion auf jeden Datenpunkt. Learning-Algorithmus erhält auch eine Belohnung Signal kurze Zeit später, der angibt, wie gut die Entscheidung.
Auf dieser Grundlage ändert der Algorithmus Strategie um höchste Rendite erzielen. Aktuell sind keine Verstärkung Algorithmus Module in Azure Machine Learning lernen. Verstärkung Lernen ist Robotics, wo Sensorwerte zu einem bestimmten Zeitpunkt ist der einen Datenpunkt und des Algorithmus muss der Robot weiter Aktion. Es ist auch eine natürliche Anwendung für das Internet der Dinge anpassen.

## <a name="considerations-when-choosing-an-algorithm"></a>Hinweise zum Auswählen eines Algorithmus

### <a name="accuracy"></a>Genauigkeit

Die genaue Antwort möglich ist nicht immer erforderlich.
Manchmal ist ein Näherungswert ausreichend, je nachdem was Sie verwenden möchten. Wenn dies der Fall ist, können Sie die Bearbeitungszeit drastisch von ungefähren Methoden an sein. Ein weiterer Vorteil der ungefähren Methoden ist, dass sie natürlich zu [überanpassung](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Zeit

Die Anzahl der Minuten oder Stunden muss ein Modell unterschiedlich viel Algorithmen. Zeit ist häufig eng mit Genauigkeit – einen anderen normalerweise begleitet. Darüber hinaus sind einige Algorithmen die Anzahl der Datenpunkte als andere.
Wenn Zeit begrenzt ist fahren es die Wahl des Algorithmus, besonders wenn die Daten groß ist.

### <a name="linearity"></a>Linearität

Viele Algorithmen für maschinelles lernen Linearität verwenden. Lineare Klassifizierungsalgorithmen davon ausgehen, dass Klassen von einer geraden Linie (oder höher dimensionale Analog) getrennt werden können. Dazu gehören logistische Regression und unterstützen Vektor Maschinen (in Azure Machine Learning implementiert).
Lineare regressionsalgorithmen angenommen, Datentrends einer geraden Linie folgt. Diese Annahmen nicht für Probleme, aber andere sie bringen Genauigkeit.

![Nicht-lineare Klasse bounday][1]

***Nicht-lineare klassengrenze*** *-auf eine lineare klassifizierungsalgorithmus Genauigkeit ergibt*

![Daten mit einem nicht linearen trend][2]

***Daten mit einem nicht linearen trend*** *-eine lineare Regression Methode erzeugt viel größere Fehler als erforderlich*

Trotz ihrer Gefahren sind lineare Algorithmen als erste Zeile des Angriffs. Sie sind algorithmisch einfach und schnell zu trainieren.

### <a name="number-of-parameters"></a>Anzahl von Parametern

Parameter sind Regler wird Data Scientist bei einem Algorithmus einrichten. Sind Zahlen, die den Algorithmus Verhalten Fehlertoleranz oder Anzahl von Iterationen oder zwischen Varianten des Algorithmus Verhalten beeinflussen. Die Zeit und die Genauigkeit des Algorithmus können manchmal sehr empfindlich zu rechten Einstellungen sein. Normalerweise benötigen Algorithmen mit vielen Parametern die meisten ausprobieren eine gute Kombination.

Auch gibt es ein Modulblock [Parameter kehren](machine-learning-algorithm-parameters-optimize.md) in Azure maschinelles lernen, die alle Parameterkombinationen was Granularität versucht automatisch gewählten. Ist eine hervorragende Möglichkeit, um sicherzustellen, dass der Parameterraum erstreckt haben, erhöht sich die Dauer zum Trainieren eines Modells exponentiell mit der Anzahl der Parameter

Der Vorteil ist, dass in der Regel müssen viele Parameter gibt an, dass ein Algorithmus Flexibilität. Sie können häufig sehr guten Genauigkeit erzielen. Finden Sie die richtige Kombination von Einstellungen bereitgestellt.

### <a name="number-of-features"></a>Anzahl der Funktionen

Für bestimmte Datentypen, die Anzahl der großen kann im Vergleich zur Anzahl der Datenpunkte. Dies ist häufig der Fall bei Genetik oder Textdaten. Viele Funktionen kann einige lernalgorithmen machen Training impraktikabel lange Zeit Einbußen. Vektor Maschinen eignen sich besonders gut für diese Anfrage (siehe unten).

### <a name="special-cases"></a>Sonderfälle

Einige lernalgorithmen stellen bestimmte Annahmen über die Struktur der Daten oder die gewünschten Ergebnisse. Wenn Sie eine, die Ihren Bedürfnissen entspricht finden, vermitteln es nützliche Ergebnisse erzielen, genauere Vorhersagen und kürzere Zeiten.

|**Algorithmus**|**Genauigkeit**|**Zeit**|**Linearität**|**Parameter**|**Notizen**|
|---|:---:|:---:|:---:|:---:|---|
|**Zwei Klassen Klassifizierung**| | | | | |
|[logistische regression](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[Entscheidung Gesamtstruktur](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[Dschungel Entscheidung](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|Geringen Speicherbedarf|
|[stärkere Entscheidungsstruktur](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|Große Speicherbedarf|
|[neuronales Netzwerk](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[Zusätzliche Anpassung ist möglich](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[Durchschnittliche perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[Vektor Maschine](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|Gut für große Mengen|
|[lokal Tiefe Vektor Maschine](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|Gut für große Mengen|
|[Bayes Maschine](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**Multi-Klasse Klassifikation**| | | | | |
|[logistische regression](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[Entscheidung Gesamtstruktur](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[Dschungel Entscheidung](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|Geringen Speicherbedarf|
|[neuronales Netzwerk](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[Zusätzliche Anpassung ist möglich](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[1 V alle](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|Eigenschaften des ausgewählten zwei Klassen-Methode|
|**Regression**| | | | | |
|[Linear](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[Bayes'sche linear](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[Entscheidung Gesamtstruktur](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[stärkere Entscheidungsstruktur](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|Große Speicherbedarf|
|[Schnelle Gesamtstruktur Quantil](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|Verteilung als Punkt Vorhersagen|
|[neuronales Netzwerk](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[Zusätzliche Anpassung ist möglich](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|Technisch Protokoll linear. Für die Vorhersage von Zahlen|
|[Ordnungszahl](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|Für die Vorhersage von Rang sortieren|
|**Erkennen von Netzwerkanomalien**| | | | | |
|[Vektor Maschine](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|Besonders gut für große Mengen|
|[Erkennen von Netzwerkanomalien PCA-basierte](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[K-means](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|Clustering-Algorithmus|


**Eigenschaften:**

**●** - zeigt ausgezeichnete Genauigkeit, schnelle Zeiten und die Verwendung der Linearität

**○** - zeigt gute Genauigkeit und mittlere Zeiten

## <a name="algorithm-notes"></a>Algorithmus-Notizen

### <a name="linear-regression"></a>Lineare regression

Wie bereits erwähnt, passt [lineare Regression](https://msdn.microsoft.com/library/azure/dn905978.aspx) eine Zeile (oder Flugzeug oder hyperfläche) auf den Datensatz. Ist eine leistungsfähige, einfach und schnell, kann es jedoch zu einfach einige Probleme.
Hier finden Sie eine [lineare Regression Tutorial](machine-learning-linear-regression-in-azure.md).

![Mit einem linearen Trend ergeben][3]

***Mit einem linearen Trend ergeben***

### <a name="logistic-regression"></a>Logistische regression

Obwohl es verwirrend "Regression" im Namen enthält, ist logistische Regression tatsächlich ein leistungsfähiges Tool für [zwei Klassen](https://msdn.microsoft.com/library/azure/dn905994.aspx) und [mehrfachklasse](https://msdn.microsoft.com/library/azure/dn905853.aspx) Klassifizierung. Es ist schnell und einfach. Daß die verwendet eine '-geformten Kurve anstelle einer geraden Linie ist es ideal für Daten in Gruppen aufzuteilen. Logistische Regression gibt lineare Klassengrenzen unbedingt, Verwendung so eine lineare Annäherung ist mit erleben.

![Logistische Regression Daten mit nur einem zwei-Klasse][4]

***Logistische Regression Daten mit nur einem zwei-Klasse*** *-klassengrenze ist der Punkt auf die logistische Kurve so nahe beide Klassen*

### <a name="trees-forests-and-jungles"></a>Strukturen, Gesamtstrukturen und Dschungel

Entscheidung-Gesamtstrukturen ([Regression](https://msdn.microsoft.com/library/azure/dn905862.aspx), [zwei Klassen](https://msdn.microsoft.com/library/azure/dn906008.aspx)und [mehrfachklasse](https://msdn.microsoft.com/library/azure/dn906015.aspx)), Entscheidung Dschungel ([zwei Klassen](https://msdn.microsoft.com/library/azure/dn905976.aspx) und [mehrfachklasse](https://msdn.microsoft.com/library/azure/dn905963.aspx)) und stärkere Entscheidungsbäume ([Regression](https://msdn.microsoft.com/library/azure/dn905801.aspx) und [zwei Klassen](https://msdn.microsoft.com/library/azure/dn906025.aspx)) alle Entscheidungsstrukturen, eine grundlegende Computer lernen Konzept basieren. Es gibt viele Varianten von Entscheidungsbäumen, aber alle das gleiche tun – Bereich Feature in Regionen mit größtenteils die gleichen unterteilen. Können Bereiche konsistent Kategorie oder von Konstantenwert, je nachdem, ob Sie Klassifizierung oder Regression tun.

![Entscheidungsstruktur unterteilt Funktion Leerzeichen][5]

***Eine Entscheidungsstruktur unterteilt Funktion Leerzeichen in Regionen annähernd einheitliche Werte***

Da willkürlich kleine Funktion Leerzeichen unterteilt werden kann, ist genug fein zu einem Datenpunkt pro Region Division vorstellen – ein extremes Beispiel für überanpassung. Um dies zu vermeiden, eine Reihe von Strukturen entstehen mit besondere mathematische Sorgfalt, die Strukturen nicht korreliert werden. Der Durchschnitt dieser Gesamtstruktur"Entscheidung" ist eine Struktur, die überanpassung vermieden werden. Entscheidung Gesamtstrukturen können sehr viel Speicher. Dschungel Entscheidung sind eine Variante, die weniger Speicher auf Kosten einer etwas mehr Zeit beansprucht.

Stärkere Entscheidungsstrukturen vermeiden überanpassung beschränken, wie oft sie unterteilen können und wie einige Datenpunkte in jeder Region zulässig sind. Der Algorithmus erstellt eine Sequenz von Strukturen lernt jeweils links von der Struktur vor Fehler auszugleichen. Das Ergebnis ist eine sehr präzise Teilnehmern tendenziell viel Arbeitsspeicher. Überprüfung auf vollständige technische Beschreibung [Friedmans ursprüngliche](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf)Papier.

[Schnelle Gesamtstruktur Quantil Regression](https://msdn.microsoft.com/library/azure/dn913093.aspx) ist eine Variante von Entscheidungsbäumen für den besonderen Fall soll nicht nur der normalen (Median) Wert der Daten in einer Region, sondern auch die Quantile aus.

### <a name="neural-networks-and-perceptrons"></a>Neuronale Netzwerke und perzeptrons

Neuronale Netzwerke sind Gehirn inspiriert lernen Algorithmen über [zwei Klassen](https://msdn.microsoft.com/library/azure/dn905947.aspx), [mehrfachklasse](https://msdn.microsoft.com/library/azure/dn906030.aspx)und [Regression](https://msdn.microsoft.com/library/azure/dn905924.aspx) Probleme. Neuronale Netzwerke in Azure Machine Learning sind aus azyklische gerichtete Diagramme, jedoch in unendlich vielen kommen. Dies bedeutet, dass Funktionen vorwärts (niemals nach hinten) eine Reihe von Ebenen übergeben werden vor zu Ausgaben. In jeder Ebene sind Eingaben in verschiedenen Kombinationen gewichtet, zusammengefasst und an die nächste Ebene. Diese Kombination von einfachen Berechnung führt komplexe Klasse Grenzen und Datentrends scheinbar von Magic lernen. Vielschichtige Netzwerke dieser Art führen das Tiefe "lernen", das so viel Tech reporting und Science-Fiction Brennstoffe.

Hohe Leistung kommen nicht, aber. Neuronale Netzwerke dauert lange, besonders bei großen Datasets mit vielen Features Schulen. Sie können mehr Parameter als die meisten Algorithmen, d.h. Parameter kehren die Zeit viel erweitert.
Und für die Streber [Eigene Netzwerkstruktur](http://go.microsoft.com/fwlink/?LinkId=402867)angeben möchten, den produktivsten.

<a name="boundaries-learned-by-neural-networks6"></a>![Grenzen durch das neuronale Netzwerke][6]
---------------------------

***Die Grenzen durch das neuronale Netzwerke können komplex und unregelmäßig wiedergegeben werden.***

[Zwei Klassen durchschnittlich Perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) antwortet neuronale Netzwerke zu Sommerferien Zeiten. Eine Netzwerkstruktur linearen Grenzen verwendet. Ist heute fast primitive, aber seit langem robust arbeiten und schnell groß ist.

### <a name="svms"></a>SVMs

Vektor Maschinen (SVMs) finden Sie die Berandung, die Klassen von als breiten Rand möglichst trennt. Bei beiden Klassen klar getrennt werden können, finden die Algorithmen beste Begrenzung können sie. Schreiben in Azure Machine Learning wird [zwei Klassen SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) mit einer geraden Linie. (SVM sprechen verwendet es einen linearen Kernel.) Da es diese linearen Annäherung vornimmt, kann relativ schnell ausgeführt. Wo es das beste ist mit Funktion intensiv wie Text oder Genomische Daten. In diesen Fällen können SVMs Klassen schneller und mit weniger überanpassung als die meisten anderen Algorithmen zusätzlich nur einen geringen Anteil Speicher getrennt.

![Unterstützung für Vector-Klasse Computergrenze][7]

***Ein Standard Support Vector-Klasse Computergrenze maximiert den Rand zwischen zwei Klassen***

Ein weiteres Produkt von Microsoft Research ist [lokal Tiefe SVM zwei Klassen](https://msdn.microsoft.com/library/azure/dn913070.aspx) eine nichtlineare Variante SVM, die meisten der Geschwindigkeit und Effizienz der linearen Version beibehalten. Es eignet sich für Fälle, in denen die lineare Methode genau genug Antworten nicht. Die Entwickler hielt es durch Aufteilung des Problems in eine Reihe von kleineren linearen SVM-Probleme. Lesen Sie die [Beschreibung](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) für Details darüber, wie sie diesen Trick zog.

Eine kluge Erweiterung nichtlineare SVMs verwenden, zeichnet [eine Klasse SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) eine Grenze, die das gesamte Dataset eng erläutert. Er eignet sich zum Erkennen von Netzwerkanomalien. Neue Datenpunkte, die weit außerhalb dieser Grenze sind ungewöhnlich bemerkenswert.

### <a name="bayesian-methods"></a>Bayes-Methoden

Bayes-Methoden sind wünschenswert verfügbar: vermeiden sie überanpassung. Dazu einige Annahmen über die Wahrscheinlichkeit Verteilung der Antwort im voraus. Ein Nebeneffekt dieses Ansatzes ist, dass sie nur sehr wenige Parameter. Azure Machine Learning hat beide Bayes-Algorithmen für Klassifizierung ([zwei Klassen Bayes Maschine](https://msdn.microsoft.com/library/azure/dn905930.aspx)) und Regression ([Bayes lineare Regression](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Beachten Sie, dass diese die Daten können geteilt oder eine gerade Linie entsprechen.

Eine Historie beachten wurden Bayes Point Maschinen bei Microsoft Research entwickelt. Sie haben einige wunderschöne theoretische Arbeit dahinter. Interessierte Studierende wird im [ursprünglichen Artikel in JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) und eine [aufschlussreiche Blog von Chris Bishop](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx)weitergeleitet.

### <a name="specialized-algorithms"></a>Spezielle Algorithmen

Haben Sie ein ganz bestimmtes Ziel können Sie Glück. In Azure Machine Learning-Auflistung werden Algorithmen Rank Vorhersage ([Ordnungszahl Regression](https://msdn.microsoft.com/library/azure/dn906029.aspx)), Anzahl Vorhersage ([Poisson-Regression](https://msdn.microsoft.com/library/azure/dn905988.aspx)) und Normalbetriebswerte (eine Grundlage [Hauptkomponenten Analyse](https://msdn.microsoft.com/library/azure/dn913102.aspx) und ein [Vektor Computer unterstützen](https://msdn.microsoft.com/library/azure/dn913103.aspx)s) spezialisiert.
Und eine einzelne Clusteringalgorithmus sowie ([K-Means](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)).

![Erkennen von Netzwerkanomalien PCA-basierte][8]

***Erkennen von Netzwerkanomalien PCA-basierte*** *-in Stereotypen Verteilung; Punkten abweichende erheblich sind Verteilung fehlerverdächtig fällt der Großteil der Daten*

![Gruppiert Daten mit K-means][9]

***Ein Dataset ist 5 K-Means mit Clustern gruppiert.***

Es gibt auch ein Ensemble [ein v alle multiclass Klassifizierung](https://msdn.microsoft.com/library/azure/dn905887.aspx), der Klasse N klassifizierungsproblem in n-1 zwei Klassen klassifizierungsprobleme unterteilt. Genauigkeit, Zeit und Linearität Eigenschaften ergeben sich zwei Klassen Klassifizierer verwendet.

![Zwei Klassen Klassifizierer zu kombiniert drei Klassen Klassifizierung][10]

***Ein Paar von zwei Klassen Klassifizierer bilden zusammen eine Klassifizierung drei Klassen***

Azure Machine Learning auch Zugriff auf einem leistungsstarken Computer Learning Framework unter dem Titel des [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
VW definiert, Kategorisierung, Klassifizierung und Regression Probleme erfahren und können sogar teilweise unbeschriftete Daten erfahren. Sie können eine Reihe von Algorithmen, Verlust Funktionen und Algorithmen verwenden. Es wurde von Grund auf neu entwickelt effiziente, parallele und extrem schnell. Es behandelt lächerlich große Mengen mit geringem Aufwand deutlich.
Gestartet und von Microsoft Research eigene John Langford geführt wird VW Formel einen Eintrag in einem Lager Auto-Algorithmen. Nicht jedes Problem VW passt, aber wenn Ihr jedoch möglicherweise lohnt steigen die Lernkurve für die Schnittstelle. Es ist auch als [eigenständige offenen Quellcode](https://github.com/JohnLangford/vowpal_wabbit) in mehreren Sprachen verfügbar.


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
