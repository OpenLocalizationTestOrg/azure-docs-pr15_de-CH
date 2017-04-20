<properties
   pageTitle="Python MapReduce Arbeitsplätze mit HDInsight | Microsoft Azure"
   description="Informationen Sie zum Erstellen und Ausführen von Python MapReduce-Jobs auf Linux-basierten HDInsight-Cluster."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Entwickeln von Python streaming-Programme für HDInsight

Hadoop bereit MapReduce, mit dem Sie zu schreiben Karte Funktionen in anderen Sprachen als Java, eine streaming-API. In diesem Artikel erfahren Sie, wie Sie Python MapReduce-Operationen.

> [AZURE.NOTE] Während der Python-Code in diesem Dokument mit einem Windows-basierten HDInsight verwendet werden kann, sind die Schritte in diesem Dokument bestimmte Linux-basierten Clustern.

Dieser Artikel basiert auf Informationen und Beispiele von Michael Noll schreiben [Eines Programms Hadoop MapReduce in Python](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/)veröffentlicht.

##<a name="prerequisites"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Ein Linux-basiertes Hadoop auf HDInsight cluster

* Ein Text-editor

    > [AZURE.IMPORTANT] Texteditor verwenden LF als Zeile endet. CRLF verwendet, werden Fehler dadurch HDInsight Linux-basierten Clustern MapReduce-Auftrags unter. Wenn Sie nicht sicher sind, verwenden Sie optionalen Schritt im Abschnitt [Ausführen MapReduce](#run-mapreduce) LF alle CRLF konvertieren.

* Für Windows-Clients kitten und PSCP. Diese Dienstprogramme sind auf der <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">Downloadseite kitten</a>.

##<a name="word-count"></a>Wörter zählen

In diesem Beispiel implementieren Sie grundlegende Wortzahl mithilfe einer Mapper Reduzierstücks. Mapper Sätze in einzelne Wörter aufgeteilt und des Reduzierstücks sammelt die Wörter und zählt die Ausgabe.

Das folgende Flussdiagramm veranschaulicht, was geschieht bei der Zuordnung und Phasen reduzieren.

![Abbildung der Karte reduzieren](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Warum Python?

Python ist eine allgemeine Programmiersprache, die das express Konzepte weniger Codezeilen als viele andere Sprachen ermöglicht. Es wurde kürzlich populär mit Daten als Prototypen Sprache seiner Art interpretiert, dynamische eingeben und eleganten Syntax für schnelle Anwendungsentwicklung vor.

Python ist auf alle HDInsight-Cluster installiert.

##<a name="streaming-mapreduce"></a>Streaming MapReduce

Hadoop können Sie zu einer Datei mit der Zuordnung Logik, die von einem Projekt verwendet wird. Spezielle Vorschriften für die Zuordnung und Logik sind:

* **Eingabe**: Zuordnung zu Komponenten Daten von STDIN lesen müssen.

* **Ausgabe**: Zuordnung und Komponenten müssen Ausgabedaten an STDOUT zu schreiben.

* **Datenformat**: Daten verarbeitet und hergestellt ist ein Schlüssel-Wert-Paar durch ein Tabstoppzeichen voneinander getrennt.

Python können einfach diese decken von **Sys** -Modul von STDIN lesen und **Drucken** Drucken auf STDOUT. Bleibt die Aufgabe ist einfach mit einer Registerkarte Formatierung (`\t`) Zeichen zwischen Schlüssel und Wert.

##<a name="create-the-mapper-and-reducer"></a>Mapper und Reduzierstücks erstellen

Mapper und Reduzierstücks sind Textdateien, in diesem Fall **mapper.py** und **reducer.py** (klarstellen der Funktionsweise). Sie können diese mit dem Editor Ihrer Wahl erstellen.

###<a name="mapperpy"></a>Mapper.py

Erstellen Sie eine neue Datei mit dem Namen **mapper.py** , und verwenden Sie den folgenden Code als Inhalt:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

In Ruhe lesen verstehen, was im Code.

###<a name="reducerpy"></a>REDUCER.py

Erstellen Sie eine neue Datei mit dem Namen **reducer.py** , und verwenden Sie den folgenden Code als Inhalt:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>Dateien hochladen

**Mapper.py** und **reducer.py** müssen auf dem Head-Knoten des Clusters sein, bevor es ausgeführt werden kann. Die einfachste Methode zum Hochladen ist mit **scp** (**Pscp** verwenden einen Windows-Client).

Verwenden Sie vom Client im selben Verzeichnis wie **mapper.py** und **reducer.py**den folgenden Befehl ein. Ersetzen Sie **Username** SSH Benutzer und **Clustername** mit dem Namen des Clusters.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

Dies kopiert die Dateien aus dem lokalen System auf dem Head-Knoten.

> [AZURE.NOTE] Wenn Sie ein Kennwort zum Schützen von SSH-Konto verwendet, werden Sie aufgefordert, das Kennwort anzugeben. Verwendet einen SSH-Schlüssel möglicherweise mit den `-i` Parameter und den Pfad für den privaten Schlüssel `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>MapReduce ausführen

1. Mithilfe von SSH mit Cluster verbinden:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Wenn Sie ein Kennwort zum Schützen von SSH-Konto verwendet, werden Sie aufgefordert, das Kennwort anzugeben. Verwendet einen SSH-Schlüssel möglicherweise mit den `-i` Parameter und den Pfad für den privaten Schlüssel `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Optional) Wenn Sie einen Text-Editor, der CRLF Zeilenende beim Erstellen von Dateien mapper.py und reducer.py verwendet verwendet oder nicht wissen, was Zeilenende Editor verwendet, verwenden Sie die folgenden Befehle LF CRLF in mapper.py und reducer.py Vorkommen konvertieren.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. Verwenden Sie folgenden Befehl MapReduce-Auftrag zu starten.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    Dieser Befehl hat folgende Bestandteile:

    * **Hadoop streaming.jar**: beim streaming MapReduce-Vorgängen verwendet. Diese Schnittstellen Hadoop mit externen MapReduce-Code, die, den Sie bereitstellen.

    * **-Dateien**: teilt Hadoop, die die angegebenen Dateien MapReduce dafür benötigt werden, und sollte auf die Arbeitskraft Knoten kopiert werden.

    * **-Mapper**: Hadoop weist die Datei als Zuordnung.

    * **-Reduzierstücks**: Hadoop weist die Datei als des Reduzierstücks.

    * **-Eingabe**: Wörter wir zählen Eingabedatei aus.

    * **-Ausgabe**: das Verzeichnis, das die Ausgabe geschrieben wird.

        > [AZURE.NOTE] Dieses Verzeichnis wird durch den Auftrag erstellt.

Sie sollten sehen eine Reihe von Anweisungen **INFO** Auftrag starten und schließlich die Operation **zuordnen** und **verringern** als Prozentwerte angezeigt.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

Sie erhalten Informationen zum Projekt, Abschluss.

##<a name="view-the-output"></a>Die Ausgabe anzeigen

Wenn der Auftrag abgeschlossen ist, verwenden den folgenden Befehl zum Anzeigen der Ausgabe:

    hdfs dfs -text /example/wordcountout/part-00000

Dies sollte zeigt eine Liste von Wörtern und wie oft das Wort ist aufgetreten. Folgendes ist ein Beispiel für die Ausgabe von Daten:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Nächste Schritte

Nun verwenden Sie HDInsight streaming MapRedcue Aufträge mit gelernt haben, die folgenden Links zu anderen Methoden zum Arbeiten mit Azure HDInsight.

* [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)
* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)
* [Verwenden Sie MapReduce Aufträge mit HDInsight](hdinsight-use-mapreduce.md)
