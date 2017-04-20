<properties
   pageTitle="MapReduce und Remotedesktop Hadoop in HDInsight | Microsoft Azure"
   description="Informationen Sie zum Verwenden von Remotedesktop Hadoop auf HDInsight und MapReduce Aufträge ausführen."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Verwenden Sie MapReduce Hadoop auf HDInsight mit Remotedesktop

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel erfahren Sie, wie Sie Hadoop auf HDInsight Cluster mithilfe von Remotedesktop Verbindung und führen Sie dann mithilfe des Befehls Hadoop MapReduce-Jobs.

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Eine Windows-basierte HDInsight (Hadoop auf HDInsight) cluster

* Ein Clientcomputer unter Windows 10, Windows 8 und Windows 7

##<a id="connect"></a>Verbinden mit Remotedesktop

Aktivieren von Remotedesktop für den HDInsight-Cluster und gemäß der Anleitung in [Verbindung mit HDInsight-Cluster mit RDP](hdinsight-administer-use-management-portal.md#rdp)herstellen.

##<a id="hadoop"></a>Verwenden Sie den Befehl Hadoop

Wenn Sie auf dem Desktop für den HDInsight-Cluster verbunden sind, gehen Sie folgendermaßen vor, um eine MapReduce Hadoop Befehl auszuführen:

1. Starten Sie vom Desktop HDInsight **Hadoop-Befehlszeile**. Daraufhin wird eine neue Befehlszeile in der **c:\apps\dist\hadoop-&lt;Versionsnummer >** Verzeichnis.

    > [AZURE.NOTE] Die Versionsnummer ändert Hadoop aktualisiert wird. Die Umgebungsvariable **HADOOP_HOME** kann verwendet werden, den Pfad zu. Beispielsweise `cd %HADOOP_HOME%` Verzeichnisse in das Verzeichnis Hadoop ohne die Versionsnummer ändert.

2. Um die **Hadoop** Befehl einen Beispiel MapReduce-Auftrag ausführen, verwenden Sie den folgenden Befehl ein:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Dies startet die **Wordcount** -Klasse, die die **Hadoop Mapreduce examples.jar** -Datei im aktuellen Verzeichnis befindet. Verwendet als Eingabe **wasbs://example/data/gutenberg/davinci.txt** Dokument und Ausgabe an gespeichert: **Wasbs: / / / Beispiel/Daten/WordCountOutput**.

    > [AZURE.NOTE] Weitere Informationen zu diesen Auftrag MapReduce und Beispiel-Daten finden Sie unter <a href="hdinsight-use-mapreduce.md">MapReduce in HDInsight Hadoop verwenden</a>.

2. Das Projekt gibt Details verarbeitet, und es gibt Informationen ähnlich der folgenden beim Abschluss des Auftrags:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wenn der Auftrag abgeschlossen ist, verwenden Sie folgenden Befehl an **Wasbs://example/data/WordCountOutput**gespeicherten Ausgabedateien aufgelistet:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Dies sollte zwei Dateien, **_SUCCESS** und **Teil-R-00000**anzeigen. **Teil-R-00000** enthält die Ausgabe für diesen Auftrag.

    > [AZURE.NOTE] Einige MapReduce-Aufträge können die Ergebnisse auf mehrere **Teil R ###** Dateien aufgeteilt. Verwenden Sie ggf. die ### Suffix an die Reihenfolge der Dateien.

4. Um die Ausgabe anzuzeigen, verwenden Sie den folgenden Befehl ein:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Eine Liste von Wörtern, die in der Datei **wasbs://example/data/gutenberg/davinci.txt** und die Anzahl der enthaltenen Worts aufgetreten. Nachfolgend ein Beispiel in der Datei enthaltenen Daten:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Zusammenfassung

Wie Sie sehen, bietet der Befehl Hadoop leicht MapReduce-Jobs auf einen HDInsight-Cluster und zeigen die Auftragsausgabe.

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce Aufträge in HDInsight:

* [HDInsight Hadoop MapReduce verwenden](hdinsight-use-mapreduce.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)
