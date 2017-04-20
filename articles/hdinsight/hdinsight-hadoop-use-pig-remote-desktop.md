<properties
   pageTitle="Verwenden Sie Hadoop Schwein mit Remotedesktop in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie der Befehl Schwein eine Remotedesktopverbindung zu einem Windows-basierten Hadoop Cluster in HDInsight Pig Latin-Anweisungen aus."
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

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Aufträgen Sie Schwein über eine Remotedesktopverbindung

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dieses Dokument enthält eine exemplarische Vorgehensweise für Befehl Schwein eine Remotedesktopverbindung zu einem Windows-basierten HDInsight Cluster Pig Latin-Anweisungen aus. Pig Latin können Sie beschreiben Datentransformationen, MapReduce Applikationen erstellen statt und Funktionen reduzieren.

In diesem Dokument erfahren Sie, wie

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel wird Folgendes erforderlich.

* Eine Windows-basierte HDInsight (Hadoop auf HDInsight) cluster

* Ein Clientcomputer unter Windows 10, Windows 8 und Windows 7

##<a id="connect"></a>Verbinden mit Remotedesktop

Aktivieren von Remotedesktop für den HDInsight-Cluster und gemäß der Anleitung in [Verbindung mit HDInsight-Cluster mit RDP](hdinsight-administer-use-management-portal.md#rdp)herstellen.

##<a id="pig"></a>Verwenden Sie den Befehl Schwein

2. Nachdem Sie eine Remotedesktop-Verbindung haben, starten Sie **Hadoop Befehlszeile** mithilfe des Symbols auf dem Desktop.

2. Anhand der folgenden Schwein Befehl starten:

        %pig_home%\bin\pig

    Sie präsentiert werden mit einem `grunt>` aufgefordert.

3. Geben Sie Folgendes ein:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Dieser Befehl lädt den Inhalt der Datei sample.log in die Datei Protokolle. Mit dem folgenden Befehl können Sie den Inhalt der Datei anzeigen:

        DUMP LOGS;

4. Transformieren der Daten durch Anwendung eines regulären Ausdrucks nur die Protokollierungsstufe aus extrahieren:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **DUMP** können Sie die Daten nach der Transformation anzuzeigen. In diesem Fall `DUMP LEVELS;`.

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
    <td>Ergebnis = Bestellung FREQUENZEN von COUNT Desc;</td><td>Ordnet die Protokollebenen nach Anzahl (absteigend) und speichert in Ergebnis</td>
    </tr>
    </table>

6. Sie können auch die Ergebnisse einer Transformation speichern, mit der `STORE` Anweisung. Der folgende Befehl beispielsweise speichert die `RESULT` in das Verzeichnis **/example/data/pigout** im Standardcontainer für Ihren Speicher:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] Die Daten werden im angegebenen Verzeichnis Dateien **Teil Nnnnn**gespeichert. Wenn das Verzeichnis bereits vorhanden ist, erhalten Sie eine Fehlermeldung.

7. Geben Sie die folgende Anweisung, um schwierige Fragen zu beenden.

        QUIT;

###<a name="pig-latin-batch-files"></a>Pig Latin-Batchdateien

Schwein-Befehl können Sie Pig Latin ausführen, die in einer Datei enthalten ist.

3. Nach dem Beenden der schwierige Fragen, öffnen Sie den **Editor** und erstellt eine neue Datei namens **pigbatch.pig** im Verzeichnis **% PIG_HOME** .

4. Geben Sie oder fügen Sie die folgenden Zeilen in die Datei **pigbatch.pig** , und speichern Sie es:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Anhand der folgenden Befehl Schwein **pigbatch.pig** -Datei ausführen.

        pig %PIG_HOME%\pigbatch.pig

    Wenn die Stapelverarbeitung abgeschlossen ist, müsste die folgende Ausgabe, die gleiche als wenn verwendet `DUMP RESULT;` in den vorherigen Schritten:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Zusammenfassung

Wie Sie sehen können, können Schwein Befehl interaktiv ausgeführt, MapReduce oder Pig Latin-Aufträge, die in einer Batchdatei gespeichert sind.

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zum Schwein in HDInsight:

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)
