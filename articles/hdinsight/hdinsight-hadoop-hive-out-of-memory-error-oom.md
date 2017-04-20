<properties
    pageTitle="Speicherfehler (OOM) - Hive-Einstellungen | Microsoft Azure"
    description="Befestigen Sie nicht genügend Arbeitsspeicher (OOM) aus einer Abfrage Struktur Hadoop in HDInsight. Das Szenario ist eine Abfrage über viele große Tabellen."
    keywords="aus Speicher Fehler OOM Hive-Einstellungen"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Korrigieren Sie Fehler von Speicher (OOM) mit Struktur Speicher in Hadoop in Azure HDInsight

Eines der häufigsten Probleme unserer Kunden Gesicht Fehler von Speicher (OOM) immer als Struktur verwenden. Dieser Artikel beschreibt ein Szenario und die Struktur Einstellungen empfohlen, um das Problem zu beheben.

## <a name="scenario-hive-query-across-large-tables"></a>Szenario: Hive-Abfrage über große Tabellen

Ein Kunde führte die Abfrage unter Verwendung der Struktur.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Bestimmte Aspekte der Abfrage:

* T1 ist ein Alias für eine große Tabelle Tabelle1 hat viele Zeichenfolge Spalte.
* Andere Tabellen sind nicht groß, aber eine große Anzahl von Spalten.
* Alle Tabellen sind, in einigen Fällen mit mehreren Spalten in Tabelle1 und verknüpfen.

Hat der Kunde MapReduce auf 24 Knoten A3 Cluster Struktur über Abfrage führte die Abfrage etwa 26 Minuten. Der Kunde bemerkt folgende Warnung beim Ausführen der Abfrage MapReduce Struktur auf:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Da etwa 26 Minuten Ausführung die Abfrage beendet, wird der Kunde diese Warnung ignoriert und stattdessen diese Verbesserung darauf gestartet die Abfrageperformance weiter.

Der Kunde konsultiert [Struktur optimieren Abfragen Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md)und Tez-Ausführungsmodul verwenden möchte. Wenn dieselbe Abfrage mit der Einstellung Tez ausgeführt wurde die Abfrage 15 Minuten lang und hat den folgenden Fehler:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Der Kunde nun mit einem VM (d. h. D12) eine größere VM denken müssten mehr Heapspeicher. Der Kunde weiterhin selbst dann die Fehlermeldung. Der Kunde erreicht das Team HDInsight Hilfe dieses Problem debuggen.

## <a name="debug-the-out-of-memory-oom-error"></a>Debuggen von Speicher (OOM) Fehler

Unser Support- und Entwicklungsteams gemeinsam von Speicher (OOM) Fehler verursacht Probleme gefunden wurde ein [bekanntes Problem in Apache JIRA beschrieben](https://issues.apache.org/jira/browse/HIVE-8306). Aus der Beschreibung in der JIRA:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

Wir bestätigt, **hive.auto.convert.join.noconditionaltask** tatsächlich auf **true** festgelegt wurde, unter Struktur site.xml Datei suchen:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Basierend auf die Warnung und die JIRA, war unsere Hypothese Karte beitreten die Java-Heap Speicherplatz OOM Fehlerursache. Damit wir dieses Problem tiefer GRUB.

Wie in den Blogbeitrag [Hadoop Garn speichereinstellungen in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), wenn Tez Execution Engine verwendet Heapspeicher verwendet gehört Tez Container. Bild unter Tez Container Speicher beschreiben.

![Tez Container Speicher Diagramm: Speicherfehler OOM Struktur](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Wie bereits im Blogbeitrag definieren zwei Folgendes für den Heap Speicher Container: **hive.tez.container.size** und **hive.tez.java.opts**. Erfahrungsgemäß bedeutet OOM-Ausnahme nicht die Größe des Containers zu klein ist. Es bedeutet, dass die Java-Heap-Größe (hive.tez.java.opts) zu klein ist. Also wenn Sie OOM sehen, können Sie versuchen, **hive.tez.java.opts**erhöhen. Bei Bedarf möglicherweise zu **hive.tez.container.size**. Die **java.opts** -Einstellung sollte ca. 80 % der **container.size**.

> [AZURE.NOTE]  Einstellung **hive.tez.java.opts** sein kleiner als **hive.tez.container.size**.

Da ein D12 28 GB Arbeitsspeicher besitzt, beschlossen wir Containergröße von 10GB (10240MB) und 80 % java.opts zuweisen. Dies erfolgte auf der Hive-Konsole mit der folgenden Einstellung:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Auf dieser Basis führte die Abfrage erfolgreich in weniger als zehn Minuten.

## <a name="conclusion-oom-errors-and-container-size"></a>Fazit: OOM Fehler und Containergröße

Eine OOM Fehlermeldung unbedingt nicht, dass die Größe des Containers zu klein ist. Stattdessen sollten Sie die Speicher konfigurieren, damit die Größe des Heaps und mindestens 80 % der Größe des Containers Speicher.
