<properties
    pageTitle="Hadoop MapReduce Ausführen von Beispielen für Linux-basierte HDInsight | Microsoft Azure"
    description="Erste Schritte mit HDInsight Linux-basierte MapReduce Beispiele. Verwenden von SSH zum Cluster herstellen und Befehl Hadoop Beispiel Aufträge ausgeführt."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>Hadoop Beispiele in HDInsight ausführen

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Linux-basierte HDInsight Cluster bieten MapReduce-Beispiele, in denen sich vertraut mit Hadoop MapReduce Aufträge verwendet werden kann. In diesem Dokument erfahren Sie mehr über die verfügbaren Beispiele und durchlaufen wenige ausgeführt.

##<a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**: finden Sie unter [Abrufen von Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **Ein Linux-basiertes HDInsight-Cluster**: finden Sie unter [Erste Schritte mit Hadoop mit Struktur in HDInsight unter Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Ein SSH-Client**: Informationen über SSH mit HDInsight finden Sie in folgenden Artikeln:

    - [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>Die Beispiele ##

**Ort**: Beispiele befinden sich im HDInsight-Cluster am **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar**

**Inhalt**: Beispiele sind in diesem Archiv enthalten:

- **Aggregatewordcount**: ein Aggregat Map/reduce-Programm, das zählt die Wörter in die Eingabedateien basiert
- **Aggregatewordhist**: ein Aggregat Map/reduce-Programm, das Histogramm Wörter Eingabedateien berechnet basierend
- **Bbp**: ein Map/reduce Programm, Peter Borwein Plouffe um exakte stellen von Pi zu berechnen
- **Dbcount**: ein Beispiel-Auftrag, der Seite Protokolle in gespeicherten zählt eine Datenbank
- **Distbbp**: ein Map/reduce Programm, BBP Typ Formel um genau Bits von Pi zu berechnen
- **GREP**: ein Map/reduce-Programm, das entspricht der Eingabe Regex zählt
- **Verknüpfung**: ein Projekt, das eine Verknüpfung Effekte über sortiert, gleichmäßig aufgeteilt Datasets
- **Multifilewc**: ein Auftrag, der Wörter aus mehreren Dateien zählt
- **Pentomino**: Map/reduce zur Anwendung Lösungen Pentomino anordnen
- **PI**: ein Map/reduce-Programm, das schätzt Pi mit quasi Monte Carlo-Methode
- **Randomtextwriter**: ein Map/reduce-Programm, das 10 GB pro Knoten Text Daten schreibt
- **Randomwriter**: ein Map/reduce-Programm, das 10 GB Daten pro Knoten schreibt
- **SecondarySort**: ein Beispiel definieren eine sekundäre Sortierung zu reduzieren
- **Sortieren**: ein Map/reduce-Programm, das vom zufällige Writer geschriebenen Daten sortiert
- **Sudoku**: Sudoku Solver
- **Teragen**: Generieren von Daten für den Terasort
- **Terasort**: die Terasort ausführen
- **Teravalidate**: Überprüfen der Ergebnisse der Terasort
- **WordCount**: ein Map/reduce-Programm, das zählt die Wörter in die Eingabedateien
- **Wordmean**: ein Map/reduce-Programm, das die durchschnittliche Länge der Wörter in den Eingabedateien zählt
- **Wordmedian**: ein Map/reduce-Programm, das die mittlere Länge der Wörter in den Eingabedateien zählt
- **Wordstandarddeviation**: ein Map/reduce-Programm, das die Standardabweichung der Länge der Wörter in den Eingabedateien zählt

**Quellcode**: Quellcode für diese Beispiele ist auf den HDInsight Cluster **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples** enthalten

> [AZURE.NOTE] Die `2.2.4.9-1` im Pfad ist die Version der Datenplattform Hortonworks für HDInsight-Cluster und aktualisiert HDInsight ändern kann.

## <a name="how-to-run-the-samples"></a>Die Beispiele ausführen ##

1. Verbinden Sie mit HDInsight über SSH, wie in den folgenden Artikeln beschrieben:

    - [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Aus der `username@#######:~$` dazu aufgefordert werden, verwenden Sie folgenden Befehl Listen Sie die Beispiele:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    Dies generiert eine Liste Beispiel im vorherigen Abschnitt dieses Dokuments.

3. Verwenden Sie den folgenden Befehl erhalten Sie Hilfe zu einem bestimmten Beispiel. In diesem Fall die **Wordcount** -Beispiel:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    Sie erhalten die folgende Meldung angezeigt:

        Usage: wordcount <in> [<in>...] <out>

    Dies bedeutet, dass Sie mehrere Eingabepfade für Herkunftsbelege bereitstellen können. Der Pfad ist, wo die Ausgabe (Anzahl der Wörter in den Herkunftsbelegen) gespeichert.

4. Anhand der folgenden Notebooks von Leonardo Da Vinci, alle Wörter zählen die Beispieldaten mit Cluster bereitgestellt werden:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Eingabe für dieses Projekt wird von **wasbs:///example/data/gutenberg/davinci.txt**lesen.

    Ausgabe für dieses Beispiel in gespeicherte **Wasbs: / / / Beispiel/Daten/Davinciwordcount**.

    > [AZURE.NOTE] Wie in der Hilfe für die Wordcount-Beispiel erwähnt, können Sie auch mehrere Eingabedateien angeben. Beispielsweise `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` davinci.txt und ulysses.txt Wörter zählen würde.

5. Sobald der Auftrag abgeschlossen ist, verwenden Sie folgenden Befehl zum Anzeigen der Ausgabe:

        hdfs dfs -cat /example/data/davinciwordcount/*

    Diese verkettet alle Ausgabedateien, die durch den Auftrag erstellt und angezeigt. Dieses einfache Beispiel gibt es nur eine Datei jedoch gäbe es mehrere dieser Befehl würden alle durchlaufen.

    Die Ausgabe ähnelt der folgenden:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Jede Zeile stellt ein Wort und wie oft die Eingabedaten aufgetreten ist.

## <a name="sudoku"></a>Sudoku

Sudoku-Beispiel hat etwas Gebrauchsanleitung; "Enthalten Sie ein Rätsel in der Befehlszeile."

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) ist ein logisches Puzzle besteht aus neun 3 x 3 Raster. Einige Zellen haben Zahlen, während andere leer, und die leeren Zellen lösen soll. Der oben stehenden Link Weitere Informationen über das Rätsel hat, aber dieses Beispiels soll die leeren Zellen zu lösen. So sollte Eingabe eine Datei, die im folgenden Format:

- Neun Zeilen mit neun Spalten

- Jede Spalte kann entweder eine Zahl enthalten oder `?` (wodurch eine leere Zelle)

- Zellen werden durch ein Leerzeichen getrennt.

Gibt es eine Möglichkeit zum Erstellen von Sudoku-Rätseln; Sie können keine Zahl in einer Spalte oder Zeile wiederholen. Es ist ein Beispiel der HDInsight-Cluster ordnungsgemäß erstellt wird. Es befindet sich unter **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** und enthält Folgendes:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] Die `2.2.4.9-1` HDInsight-Cluster aktualisiert werden kann Teil des Pfades ändern.

Führen Sie dieses Beispiel Sudoku Befehl:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

Das Ergebnis sollte ähnlich der folgenden angezeigt:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>PI (π)

Pi-Beispiel verwendet eine statistische (quasi Monte Carlo) Methode, um den Wert von Pi zu schätzen. Punkten nach dem Zufallsprinzip innerhalb einer Einheit fallen Platz auch einen Kreis mit einer Wahrscheinlichkeit gleich der Fläche eines Kreises, der in das Feld eingetragen Pi/4. Der Wert von Pi werden ab der 4R, abgeschätzt R das Verhältnis der Anzahl der Punkte ist, die in den Kreis, um die Gesamtzahl der Punkte in das Quadrat. Je größer die Stichprobe der Punkte ist die bessere Schätzung.

Zuordnung für dieses Beispiel generiert eine Reihe von Punkten nach dem Zufallsprinzip in einem Einheitsquadrat und zählt dann die Anzahl der Punkte, die innerhalb des Kreises.

Des Reduzierstücks sammelt Punkte der Mapper gezählt und schätzt den Wert von Pi aus der Formel 4R, R ist das Verhältnis der Anzahl der Punkte in den Kreis, um die Gesamtzahl der Punkte in das Quadrat gezählt.

Verwenden Sie den folgenden Befehl, um dieses Beispiel auszuführen. Dies wird 16 Karten mit 10.000.000 Beispiele schätzen den Wert von Pi verwendet:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

Der zurückgegebene Wert sollte ähnlich wie **3.14159155000000000000**. Verweise sind die ersten 10 Dezimalstellen Pi 3.1415926535.

##<a name="10gb-greysort"></a>10GB Greysort

GraySort ist ein Benchmark, deren Metrik sortieren (TB/Minute) ist, die beim Sortieren großer Datenmengen in der Regel ein 100TB minimale erreicht.

Dieses Beispiel verwendet eine bescheidene 10GB Daten relativ schnell ausgeführt werden kann. Entwickelt von Owen O'Malley und Arun Murthy, die jährliche allgemeine ("Daytona") Terabyte Art Benchmark 2009 mit 0,578 TB/Min. (100 TB in 173 Minuten) gewonnen MapReduce-Applikationen verwendet. Weitere Informationen zu dieser und anderen Sortierung Benchmarks finden Sie in der [Sortbenchmark](http://sortbenchmark.org/) -Website.

Dieses Beispiel verwendet drei Sätze von MapReduce Programme:

- **TeraGen**: ein MapReduce-Programm, das Datenzeilen sortiert wird

- **TeraSort**: Beispiele für die Eingabedaten und MapReduce zum Sortieren der Daten in einer Auftragssumme verwendet

    TeraSort ist eine standardmäßige MapReduce-Funktionen außer einem benutzerdefinierten Partitionierer, der eine sortierte Liste der Stichprobe n-1-Schlüssel verwendet, das den Schlüssel für jede reduzieren definiert. Insbesondere alle Schlüssel solche, die (Beispiel) [i-1] < = Schlüssel < Beispiel [i] i zu erhalten. Dies garantiert, dass die Ausgaben i reduzieren sind kleiner als die Ausgabe i + 1 verringern.

- **TeraValidate**: ein MapReduce-Programm, das überprüft, ob die Ausgabe Global sortiert

    Eine Karte pro Datei im Ausgabeverzeichnis erstellt und jede Karte wird sichergestellt, dass jeder Schlüssel kleiner oder gleich vorherigen. Die Funktion generiert auch Datensätze der ersten und letzten Schlüssel jeder Datei und die Funktion wird sichergestellt, dass der erste Schlüssel der Datei i größer als der letzte Teil i-Datei 1. Probleme werden als Ausgabe reduzieren mit Schlüsseln gemeldet, die sind.

Verwenden Sie die folgenden Schritte zum Generieren von Daten, Sortieren und überprüfen Sie das Ergebnis:

1. Generieren von 10GB Daten Standardspeicher am HDInsight Cluster gespeichert **Wasbs: / / / / Daten / 10GB-Art-Beispieleingabe**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    Die `-Dmapred.map.tasks` teilt Hadoop wie viele Karte Aufgaben für diesen Auftrag. Die letzten beiden Parameter weisen den Auftrag 10 GB Daten erstellen und speichern Sie es **Wasbs: / / / / Daten / 10GB-Art-Beispieleingabe**.

2. Verwenden Sie den folgenden Befehl zum Sortieren der Daten:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    Die `-Dmapred.reduce.tasks` teilt Hadoop wie viele Aufgaben für den Auftrag reduzieren. Die letzten beiden Parameter sind nur die Eingabe- und Ausgabeparameter für Daten.

3. Verwenden Sie folgende Daten sortieren zu überprüfen:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Nächste Schritte ##

In diesem Artikel haben Sie gelernt, die Linux-basierten HDInsight Cluster enthaltenen Beispiele ausführen. Lernprogramme zu HDInsight Schwein, Struktur und MapReduce mit finden Sie unter den folgenden Themen:

* [Hadoop auf HDInsight Schwein verwenden][hdinsight-use-pig]
* [Hadoop auf HDInsight Struktur verwenden][hdinsight-use-hive]
* [Verwenden Sie MapReduce Hadoop auf HDInsight] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
