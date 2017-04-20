<properties
   pageTitle="MapReduce und SSH-Verbindung Hadoop in HDInsight | Microsoft Azure"
   description="Erfahren Sie mehr über das SSH verwenden HDInsight Hadoop über MapReduce-Aufträge ausgeführt werden."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>MapReduce Hadoop auf HDInsight mit SSH verwenden

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel erfahren Sie, wie Sie Secure Shell (SSH) Hadoop auf HDInsight Cluster und MapReduce Aufträge Hadoop Befehle senden.

> [AZURE.NOTE] Wenn Sie bereits mit Hadoop Linux-basierte Server, aber sind neue HDInsight, anzeigen Sie [Linux-basierte HDInsight Tipps](hdinsight-hadoop-linux-information.md)

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Linux-basierte HDInsight (Hadoop auf HDInsight) cluster

* SSH-Client. Linux, Unix und Mac-Betriebssysteme sollten mit einem SSH-Client kommen. Windows-Benutzer müssen einen Client wie [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen.

##<a id="ssh"></a>Verbinden mit SSH

Eine Verbindung zu den vollqualifizierten Domänennamen (FQDN) des Clusters HDInsight Befehl SSH. Der FQDN werden dem Namen gefolgt von Cluster zugewiesen **. azurehdinsight.net**. Beispielsweise würde die folgenden zu einem Cluster mit dem Namen **Myhdinsight**verbinden:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Sofern Zertifikatschlüssel für SSH-Authentifizierung** während der HDInsight-Cluster erstellt, müssen Sie den Speicherort des privaten Schlüssels auf dem Clientsystem beispielsweise angeben:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Sofern ein Kennwort zur Authentifizierung SSH** beim Erstellen des HDInsight-Clusters müssen Sie geben das Kennwort ein.

Weitere Informationen über SSH mit HDInsight finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>Kitten (Windows-Clients)

Windows bietet keine integrierten SSH-Client. Wir empfehlen, **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen über kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Hadoop Befehle

1. Nach der Verbindung mit dem HDInsight-Cluster Befehl folgende **Hadoop** um MapReduce-Auftrag zu starten:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Dies startet die **Wordcount** -Klasse, die in der Datei **Hadoop Mapreduce examples.jar** enthalten. Verwendet als Eingabe **wasbs://example/data/gutenberg/davinci.txt** Dokument und Ausgabe gespeicherten **Wasbs: / / / Beispiel/Daten/WordCountOutput**.

    > [AZURE.NOTE] Weitere Informationen zu diesen Auftrag MapReduce und Beispiel-Daten finden Sie unter [Verwenden MapReduce in Hadoop auf HDInsight](hdinsight-use-mapreduce.md).

2. Das Projekt gibt Details verarbeitet, und es gibt Informationen ähnlich der folgenden Abschluss des Auftrags:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Bei Beendigung des Auftrags Befehl folgenden Ausgabedateien aufzulisten, die in **Wasbs://example/data/WordCountOutput**gespeichert sind:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    Dies sollte zwei Dateien, **_SUCCESS** und **Teil-R-00000**anzeigen. **Teil-R-00000** enthält die Ausgabe für diesen Auftrag.

    > [AZURE.NOTE] Einige MapReduce-Aufträge können die Ergebnisse auf mehrere **Teil R ###** Dateien aufgeteilt. Verwenden Sie ggf. die ### Suffix an die Reihenfolge der Dateien.

4. Um die Ausgabe anzuzeigen, verwenden Sie den folgenden Befehl ein:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Dies zeigt eine Liste von Wörtern, die in der Datei **wasbs://example/data/gutenberg/davinci.txt** und die Anzahl des Auftretens aller Wörter enthalten sind. Nachfolgend ein Beispiel in der Datei enthaltenen Daten:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Zusammenfassung

Wie Sie sehen können, erleichtern Hadoop Befehle MapReduce Aufträge in einem Cluster HDInsight und zeigen die Auftragsausgabe.

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce Aufträge in HDInsight:

* [HDInsight Hadoop MapReduce verwenden](hdinsight-use-mapreduce.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)
