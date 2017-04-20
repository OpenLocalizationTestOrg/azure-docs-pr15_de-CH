<properties
   pageTitle="Verwenden Sie Hadoop Struktur und Remotedesktop im HDInsight | Microsoft Azure"
   description="Informationen Sie zum HDInsight Hadoop Cluster mithilfe von Remotedesktop herstellen und dann Abfragen Sie Struktur mithilfe der Befehlszeilenschnittstelle Struktur."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Verwenden Sie Struktur mit Hadoop auf HDInsight mit Remotedesktop

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel wird so ein HDInsight-Cluster mithilfe von Remotedesktop herstellen und dann Abfragen Struktur mithilfe der Struktur Befehlszeilenschnittstelle (CLI).

> [AZURE.NOTE] Dieses Dokument bietet keine ausführlich was HiveQL-Anweisungen, die in den Beispielen verwendet werden. Informationen über HiveQL, die in diesem Beispiel verwendet wird, finden Sie unter [Verwenden Hadoop auf HDInsight Struktur](hdinsight-use-hive.md).

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Eine Windows-basierte HDInsight (Hadoop auf HDInsight) cluster

* Ein Clientcomputer unter Windows 10, Windows 8 oder Windows 7

##<a id="connect"></a>Verbinden mit Remotedesktop

Aktivieren von Remotedesktop für den HDInsight-Cluster und gemäß der Anleitung in [Verbindung mit HDInsight-Cluster mit RDP](hdinsight-administer-use-management-portal.md#rdp)herstellen.

##<a id="hive"></a>Verwenden Sie den Befehl Struktur

Verbindung der Desktop HDInsight Cluster gehen Sie Struktur arbeiten:

1. Starten Sie vom Desktop HDInsight **Hadoop-Befehlszeile**.

2. Geben Sie den folgenden Befehl an die Struktur CLI:

        %hive_home%\bin\hive

    Nach dem Start der CLI wird die Struktur CLI-Aufforderung angezeigt: `hive>`.

3. Geben Sie die CLI die folgende Anweisung zum Erstellen einer neuen Tabelle mit dem Namen **log4jLogs** mit Beispieldaten:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Diese Aussagen werden die folgenden Aktionen durchführen:

    * **DROP TABLE**: die Tabelle und die Datei löscht, wenn die Tabelle bereits vorhanden ist.

    * **Externe Tabelle erstellen**: erstellt eine neue 'externe' Tabelle Struktur. Externe Tabellen enthalten nur die Tabellendefinition in Struktur (die Daten am ursprünglichen Speicherort verbleiben).

        > [AZURE.NOTE] Externe Tabellen sollte verwendet werden, wenn Sie erwarten, dass die zugrunde liegenden Daten von einer externen Quelle (z. B. eine automatisierte Upload von Daten) oder eine andere Operation MapReduce aktualisiert werden, aber immer soll Hive-Abfragen mit den neuesten Daten.
        >
        > Löschen einer externen Tabelle ist **nicht** löschen die Daten nur die Tabellendefinition.

    * **ZEILENFORMAT**: weist Struktur wie die Daten formatiert. In diesem Fall werden die Felder in jedem Protokoll durch ein Leerzeichen getrennt.

    * **Gespeichert als Textdatei Speicherort**: weist Struktur die Daten ist (Verzeichnis Beispiel-Daten) gespeichert und wird als Text gespeichert.

    * **Auswählen**: Wählt alle Zeilen, Spalte **t4** Wert **[Fehler]**enthält. Dies sollte der Wert **3** zurück, da drei Zeilen, die diesen Wert enthalten.

    * **INPUT__FILE__NAME wie '%.log'** - sagt Struktur, die wir nur Daten aus Dateien zurückgibt. Log. Dies beschränkt die Suche auf sample.log-Datei enthält die Daten und verhindert, dass Sie zurückgeben von Daten aus anderen Dateien, die nicht das Schema definiert.


4. Mit der neue 'interne' Tabelle **Fehlerprotokolle von.**erstellt Folgendes:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Diese Aussagen werden die folgenden Aktionen durchführen:

    * **Erstellen der Tabelle IF NOT EXISTS**: eine Tabelle erstellt, wenn es nicht bereits vorhanden ist. Da **externe** Schlüsselwort nicht verwendet wird, ist dies eine interne Tabelle Struktur Data Warehouse gespeichert und wird vollständig von Struktur verwaltet.

        > [AZURE.NOTE] Im Gegensatz zu **EXTERNEN** Tabellen ablegen einer Tabelle auch die zugrunde liegenden Daten gelöscht.

    * **Gespeichert als ORK**: speichert die Daten in Zeile optimierte Spaltenformat (ORK). Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten der Struktur.

    * ÜBERSCHREIBEN von **Einfügen... Wählen Sie**: Wählt Zeilen aus der Tabelle **log4jLogs** mit **[Fehler]**fügt dann die Daten in die Tabelle **ErrorLogs** .

    Um sicherzustellen, dass nur Zeilen mit **[Fehler]** in Spalte t4 **ErrorLogs** Tabelle gespeichert wurden, verwenden Sie die folgende Anweisung alle Zeilen aus **ErrorLogs**zurückgeben:

        SELECT * from errorLogs;

    Drei Zeilen mit Daten zurückgegeben werden soll alle enthalten **[Fehler]** Spalte t4.

##<a id="summary"></a>Zusammenfassung

Wie Sie sehen können, die der Befehl Struktur einfache Weise interaktiv Abfragen Struktur auf einen HDInsight-Cluster, den Status überwachen und Abrufen der Ausgabe.

##<a id="nextsteps"></a>Nächste Schritte

Weitere Informationen zu Struktur in HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

Bei Verwendung von Tez mit Struktur finden Sie in den folgenden Dokumenten Debuginformationen:

* [Mithilfe der Tez-Benutzeroberfläche auf Windows-basierten HDInsight](hdinsight-debug-tez-ui.md)

* [Ambari Tez Ansicht auf Linux-basierten HDInsight](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

