<properties
   pageTitle="Abfrage mit Hadoop Tools für Visual Studio Struktur | Microsoft Azure"
   description="Informationen Sie zum Struktur Hadoop in HDInsight mit Visual Studio Hadoop Tools verwenden."
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

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Abfragen Sie Struktur mit HDInsight-Tools für Visual Studio

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Struktur Abfragen an einen HDInsight-Cluster mit HDInsight-Tools für Visual Studio übermittelt.

> [AZURE.NOTE] Dieses Dokument bietet keine ausführlich in den Beispielen verwendeten HiveQL-Anweisungen vorgehen. Informationen über HiveQL, die in diesem Beispiel verwendet wird, finden Sie unter [Verwenden Hadoop auf HDInsight Struktur](hdinsight-use-hive.md).

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel wird Folgendes erforderlich.

* Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (Linux oder Windows)

* Visual Studio (eine der folgenden Versionen):

    Visual Studio 2013 Community/Professional/Premium/Ultimate mit [Update 4](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (Community/Enterprise)

- HDInsight-Tools für Visual Studio. Informationen zur Installation und Konfiguration der Tools finden Sie unter [Erste Schritte mit Visual Studio Hadoop Tools für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) .

##<a id="run"></a>Abfragen Sie Struktur mit HDInsight-Tools für Visual Studio

1. Öffnen Sie **Visual Studio** , und wählen Sie **neu** > **Projekt** > **HDInsight** > **Anwendung Struktur**. Geben Sie einen Namen für dieses Projekt.

2. Die Datei **Script.hql** , die das Projekt erstellt wird, und fügen Sie die folgende HiveQL-Anweisung:

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
    * **Auswählen**: Wählen Sie alle Zeilen, Spalte **t4** Wert **[Fehler]**enthalten. Dies sollte der Wert **3** zurück, da drei Zeilen, die diesen Wert enthalten.
    * **INPUT__FILE__NAME wie '%.log'** - sagt Struktur, die wir nur Daten aus Dateien zurückgibt. Log. Dies beschränkt die Suche auf sample.log-Datei enthält die Daten und verhindert, dass Sie zurückgeben von Daten aus anderen Dateien, die nicht das Schema definiert.

3. Aus der Symbolleiste wählen Sie **HDInsight-Cluster** , die Sie für diese Abfrage verwenden möchten und dann **an WebHCat senden** , die Anweisungen als Struktur Auftrag mit WebHCat. Sie können auch Senden des Auftrags Schaltfläche __Ausführen über HiveServer2__ Wenn HiveServer2 in der Clusterversion verfügbar ist. Die **Jobübersicht Struktur** und zeigt Informationen über die ausgeführte Arbeit. Verwenden Sie den Link **Aktualisieren** die Auftragsinformationen aktualisieren, bis **Job Status** **abgeschlossen**wird.

4. Verwenden Sie den Link **Auftragsausgabe** zum Anzeigen der Ausgabe dieses Auftrags. Sie sollten anzeigen `[ERROR] 3`, den Wert von der SELECT-Anweisung zurückgegeben.

5. Sie können auch Hive-Abfragen ausführen, ohne ein Projekt zu erstellen. **Azure**mit **Server-Explorer**erweitern > **HDInsight**Servers HDInsight Maustaste, und wählen Sie **eine Struktur Abfrage schreiben**.

6. Fügen Sie im Dokument **temp.hql** HiveQL Folgendes ein:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Diese Aussagen werden die folgenden Aktionen durchführen:

    * **Erstellen der Tabelle IF NOT EXISTS**: eine Tabelle erstellt, wenn es nicht bereits vorhanden ist. Da **externe** Schlüsselwort nicht verwendet wird, ist dies eine interne Tabelle Struktur Data Warehouse gespeichert und wird vollständig von Struktur verwaltet.

        > [AZURE.NOTE] Im Gegensatz zu **EXTERNEN** Tabellen ablegen einer Tabelle auch die zugrunde liegenden Daten gelöscht.

    * **Gespeichert als ORK**: speichert die Daten in Zeile optimierte Spaltenformat (ORK). Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten der Struktur.
    * ÜBERSCHREIBEN von **Einfügen... Wählen Sie**: Wählt Zeilen aus der Tabelle **log4jLogs** mit **[Fehler]**fügt dann die Daten in die Tabelle **ErrorLogs** .

7. Wählen Sie auf der Symbolleiste **Senden** zum Ausführen des Auftrags. Verwenden Sie den **Auftragsstatus** ermitteln, ob der Auftrag erfolgreich abgeschlossen wurde.

8. Um sicherzustellen, dass der Auftrag abgeschlossen und eine neue Tabelle erstellt, verwenden Sie **Server-Explorer** , und erweitern Sie **Azure** > **HDInsight** > HDInsight Cluster > **Datenbanken Struktur** > und **Standard**. Die Tabellen **ErrorLogs** und **log4jLogs** sollte angezeigt werden.

##<a id="summary"></a>Zusammenfassung

Wie Sie sehen können, die die HDInsight-Tools für Visual Studio bieten eine einfache Möglichkeit Struktur Abfragen auf einem HDInsight-Cluster, den Status überwachen und Abrufen der Ausgabe.

##<a id="nextsteps"></a>Nächste Schritte

Weitere Informationen zu Struktur in HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

Weitere Informationen über die HDInsight-Tools für Visual Studio:

* [Erste Schritte mit HDInsight Tools für Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
