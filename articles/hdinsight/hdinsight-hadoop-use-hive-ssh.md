<properties
   pageTitle="Verwenden Sie Struktur-Shell in HDInsight (Hadoop) | Microsoft Azure"
   description="Informationen Sie zum Shell Struktur mit einer Linux-basierten HDInsight verwenden. Sie erfahren Sie, wie die Verbindung mit HDInsight SSh dann interaktiv Abfragen mithilfe der Shell Struktur."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Struktur mit Hadoop in HDInsight mit SSH verwenden

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Sie Secure Shell (SSH) Hadoop in Azure HDInsight Cluster und anschließend Hive-Abfragen interaktiv mit Struktur-Befehlszeilenschnittstelle (CLI).

> [AZURE.IMPORTANT] Während der Befehl Struktur auf Linux-basierten HDInsight-Cluster verfügbar ist, sollten Sie mit Luftlinie. Luftlinie ist eine neuere für die Arbeit mit Struktur und HDInsight Cluster gehört. Weitere Informationen über diese finden Sie unter [Verwenden Hadoop in HDInsight mit Luftlinie Struktur](hdinsight-hadoop-use-hive-beeline.md).

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Ein Linux-basiertes Hadoop auf HDInsight Cluster.

* SSH-Client. Linux, Unix und Mac OS sollte mit einem SSH-Client kommen. Windows-Benutzer müssen einen Client wie [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen.

##<a id="ssh"></a>Verbinden mit SSH

Eine Verbindung zu den vollqualifizierten Domänennamen (FQDN) des Clusters HDInsight Befehl SSH. Der vollqualifizierte Domänenname sein Name den Cluster dann gab **. azurehdinsight.net**. Beispielsweise würde die folgenden zu einem Cluster mit dem Namen **Myhdinsight**verbinden:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Sofern Zertifikatschlüssel für SSH-Authentifizierung** während der HDInsight-Cluster erstellt, müssen Sie den Speicherort des privaten Schlüssels auf dem Clientsystem angeben:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Sofern ein Kennwort zur Authentifizierung SSH** beim Erstellen des HDInsight-Clusters müssen Sie geben das Kennwort ein.

Weitere Informationen über SSH mit HDInsight finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kitten (Windows-basierte Clients)

Windows bietet keine integrierten SSH-Client. Wir empfehlen, **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen über kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>Verwenden Sie den Befehl Struktur

2. Sobald verbunden, starten Sie die CLI Struktur mit dem folgenden Befehl:

        hive

3. Geben Sie die CLI die folgende Anweisung zum Erstellen einer neuen Tabelle namens **log4jLogs** mit den Beispieldaten:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Diese Aussagen werden die folgenden Aktionen durchführen:

    * **DROP TABLE** - Löschen der Tabelle und der Datendatei, falls die Tabelle bereits vorhanden ist.
    * **Externe Tabelle erstellen** - erstellt eine neue 'externe' Tabelle Struktur. Externe Tabellen enthalten nur die Tabellendefinition in Struktur. Die Daten verbleiben am ursprünglichen Speicherort.
    * **FORMAT Zeile** - weist Struktur wie die Daten formatiert werden. In diesem Fall werden die Felder in jedem Protokoll durch ein Leerzeichen getrennt.
    * **Gespeichert als Textdatei Speicherort** - weist Struktur, in der Daten, gespeichert (Beispiel-Daten-Verzeichnis) und davon wird als Text gespeichert.
    * **Auswählen** - wählt alle Zeilen Spalte **t4** Wert **[Fehler]**enthält. Drei Zeilen, die diesen Wert enthalten sollte dadurch der Wert **3** zurückgegeben.
    * **INPUT__FILE__NAME wie '%.log'** - sagt Struktur, die wir nur Daten aus Dateien zurückgibt. Log. Dies beschränkt die Suche auf sample.log-Datei enthält die Daten und verhindert, dass Sie zurückgeben von Daten aus anderen Dateien, die nicht das Schema definiert.

    > [AZURE.NOTE] Externe Tabellen sollte verwendet werden, die zugrunde liegenden Daten von einer externen Quelle wie ein automatisches Uploadprozess oder anderen MapReduce Vorgang aktualisiert aber immer Hive-Abfragen mit den neuesten Daten erwartet.
    >
    > Löschen einer externen Tabelle ist **nicht** löschen die Daten nur die Tabellendefinition.

4. Mit der neue 'interne' Tabelle **Fehlerprotokolle von.**erstellt Folgendes:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Diese Aussagen werden die folgenden Aktionen durchführen:

    * **Erstellen der Tabelle IF NOT EXISTS** - erstellt eine Tabelle, wenn es nicht bereits vorhanden ist. Da **externe** Schlüsselwort nicht verwendet wird, ist eine interne Tabelle Struktur Data Warehouse gespeichert und wird vollständig von Struktur verwaltet.
    * **Gespeichert als ORK** - speichert die Daten optimiert Zeile Einspaltig (ORK)-Format. Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten der Struktur.
    * ÜBERSCHREIBEN von **Einfügen... Wählen Sie** - wählt Zeilen aus der **log4jLogs** -Tabelle, die enthalten **[Fehler]**und fügt die Daten in der Tabelle **ErrorLogs** .

    Um sicherzustellen, dass nur Zeilen mit **[Fehler]** in Spalte t4 **ErrorLogs** Tabelle gespeichert wurden, verwenden Sie die folgende Anweisung alle Zeilen aus **ErrorLogs**zurückgeben:

        SELECT * from errorLogs;

    Drei Zeilen mit Daten zurückgegeben werden soll alle enthalten **[Fehler]** Spalte t4.

    > [AZURE.NOTE] Im Gegensatz zu externen Tabellen löscht Ablegen einer internen Tabelle zugrunde liegenden Daten.

##<a id="summary"></a>Zusammenfassung

Wie Sie sehen, bietet der Befehl Struktur leicht interaktiv Abfragen Struktur auf einen HDInsight-Cluster, den Status überwachen und Abrufen der Ausgabe.

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zur Struktur in HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

Weitere Informationen können Sie mit Hadoop auf HDInsight arbeiten:

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

Bei Verwendung von Tez mit Struktur finden Sie in den folgenden Dokumenten Debuginformationen:

* [Mithilfe der Tez-Benutzeroberfläche auf Windows-basierten HDInsight](hdinsight-debug-tez-ui.md)

* [Ambari Tez Ansicht auf Linux-basierten HDInsight](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

