<properties
   pageTitle="Verwenden Sie Hadoop Schwein mit SSH auf einem Cluster HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie eine Verbindung zu einem Linux-basierten Hadoop Cluster mit SSH und dann mit dem Befehl Schwein Pig Latin-Anweisungen interaktiv ausgeführt oder als Stapelverarbeitungsauftrag."
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

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Aufträgen Sie Schwein auf einem Linux-basierten Cluster mit dem Befehl Schwein (SSH)

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

In diesem Dokument führt Sie durch den Prozess des Verbindens Azure HDInsight Linux-basierten Cluster mit Secure Shell (SSH), dann Befehl geschlachtete Schweine Latin-Anweisungen interaktiv ausgeführt oder als Stapelverarbeitungsauftrag.

Pig Latin Programmiersprache können Sie Transformationen beschreiben, die die Eingabedaten für die gewünschte Ausgabe angewendet werden.

> [AZURE.NOTE] Wenn Sie bereits mit Hadoop Linux-basierte Server, aber neu HDInsight, anzeigen Sie [Linux-basierte HDInsight Tipps](hdinsight-hadoop-linux-information.md)

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel wird Folgendes erforderlich.

* Ein Linux-basiertes HDInsight (Hadoop auf HDInsight)-Cluster.

* SSH-Client. Linux, Unix und Mac OS sollte mit einem SSH-Client kommen. Windows-Benutzer müssen einen Client wie [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen.

##<a id="ssh"></a>Verbinden mit SSH

Eine Verbindung zu den vollqualifizierten Domänennamen (FQDN) des Clusters HDInsight Befehl SSH. Der vollqualifizierte Domänenname sein Name den Cluster dann gab **. azurehdinsight.net**. Beispielsweise würde die folgenden zu einem Cluster mit dem Namen **Myhdinsight**verbinden.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Sofern Zertifikatschlüssel für SSH-Authentifizierung** während der HDInsight-Cluster erstellt, müssen Sie den Speicherort des privaten Schlüssels auf dem Clientsystem angeben.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Sofern ein Kennwort zur Authentifizierung SSH** beim Erstellen des HDInsight-Clusters müssen Sie geben das Kennwort ein.

Weitere Informationen über SSH mit HDInsight finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kitten (Windows-basierte Clients)

Windows bietet keine integrierten SSH-Client. Wir empfehlen, **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen über kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="pig"></a>Verwenden Sie den Befehl Schwein

2. Schwein-Befehlszeilenschnittstelle (CLI) zunächst einmal verbunden, mit dem folgenden Befehl.

        pig

    Nach kurzer Zeit erscheint eine `grunt>` aufgefordert.

3. Geben Sie die folgende Anweisung.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Dieser Befehl lädt den Inhalt der Datei sample.log in PROTOKOLLEN. Sie können den Inhalt der Datei mit dem folgenden anzeigen.

        DUMP LOGS;

4. Transformieren von Daten anschließend durch Anwendung eines regulären Ausdrucks nur die Protokollierungsstufe aus mithilfe der folgenden extrahieren.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **DUMP** können Sie die Daten nach der Transformation anzuzeigen. In diesem Fall, `DUMP LEVELS;`.

5. Transformationen mithilfe der folgenden Anweisung fortgesetzt. Mit `DUMP` das Ergebnis der Transformation nach jedem Schritt anzeigen.

    <table>
    <tr>
    <th>Anweisung</th><th>Funktionsweise</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTEREBENEN durch LOGLEVEL ist nicht null.</td><td>Zeilen mit null-Wert für die Protokollebene entfernt und speichert die Ergebnisse in FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS Gruppe von LOGLEVEL;</td><td>Gruppiert die Zeilen von Ebene und speichert die Ergebnisse in GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREQUENZEN = Foreach erstellen GROUPEDLEVELS Gruppe als LOGLEVEL COUNT (FILTEREDLEVELS. LOGLEVEL) zählen;</td><td>Erstellt eine neue Gruppe, jede eindeutige Protokolldatei enthält, Wert und wie oft auftritt. Dieser wird in FREQUENZEN gespeichert.</td>
    </tr>
    <tr>
    <td>Ergebnis = Bestellung FREQUENZEN von COUNT Desc;</td><td>Ordnet die Protokollebenen nach Anzahl (absteigend) und Ergebnis speichert.</td>
    </tr>
    </table>

6. Sie können auch die Ergebnisse einer Transformation speichern, mit der `STORE` Anweisung. Beispielsweise die folgenden speichert die `RESULT` in das Verzeichnis **/example/data/pigout** Speicher Standardcontainer für den Cluster.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] Die Daten werden im angegebenen Verzeichnis Dateien **Teil Nnnnn**gespeichert. Wenn das Verzeichnis bereits vorhanden ist, wird eine Fehlermeldung.

7. Geben Sie die folgende Anweisung, um schwierige Fragen zu beenden.

        QUIT;

###<a name="pig-latin-batch-files"></a>Pig Latin-Batchdateien

Schwein-Befehl können Sie in einer Datei enthaltenen Pig Latin ausführen.

3. Nach dem Beenden der schwierige Fragen, verwenden Sie den folgenden Befehl Rohr STDIN in einer Datei namens **pigbatch.pig**. Diese Datei wird in das Basisverzeichnis für das Konto erstellt werden, die Sie für die SSH-Sitzung angemeldet sind.

        cat > ~/pigbatch.pig

4. Geben Sie oder fügen Sie die folgenden Zeilen, und verwenden Sie STRG + D Abschluss.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Anhand der folgenden Datei **pigbatch.pig** Schwein Befehl ausführen.

        pig ~/pigbatch.pig

    Nachdem die Stapelverarbeitung abgeschlossen ist, müsste die folgende Ausgabe, die gleiche als wenn verwendet `DUMP RESULT;` in den vorherigen Schritten.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Zusammenfassung

Wie Sie sehen, kann Befehls Schwein interaktiv ausgeführt, MapReduce mit Pig Latin sowie Ausführen von Anweisungen in einer Batch-Datei gespeichert.

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zum Schwein in HDInsight.

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

Weitere Informationen können Sie Hadoop auf HDInsight arbeiten.

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)
