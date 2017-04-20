<properties
    pageTitle="Informationen Struktur und Verwendung HiveQL | Microsoft Azure"
    description="Lernen Sie Apache Struktur und Hadoop in HDInsight verwenden. Wählen Sie Ihre Struktur Stapelverarbeitung und mit HiveQL eine Beispieldatei Apache log4j analysieren."
    keywords="Hiveql, welche Struktur ist"
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
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Mit der Struktur und HiveQL in HDInsight Hadoop eine Beispieldatei Apache log4j analysieren

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


In diesem Lernprogramm werden Sie lernen Apache-Struktur in Hadoop auf HDInsight und Hive-Auftrag ausführen wählen. Sie lernen auch von HiveQL und eine Beispieldatei Apache log4j analysieren.

##<a id="why"></a>Was ist und warum?
[Apache Hive](http://hive.apache.org/) ist ein Datawarehouse-System für Hadoop ermöglicht Zusammenfassung Daten Abfragen und Analyse von Daten mithilfe von HiveQL (eine Abfragesprache wie SQL). Struktur kann interaktiv Daten oder wieder verwendbaren Verarbeitungsaufträge erstellen verwendet werden.

Struktur ermöglicht Projektstruktur weitgehend unstrukturierten Daten. Nach dem Definieren der Struktur können Struktur Sie Daten ohne Java oder MapReduce Abfragen. **HiveQL** (Struktur Query Language) können Sie mit Abfragen, die T-SQL ähnelt.

Struktur versteht arbeiten strukturierter und teilweise strukturierter Daten, wie z. B. Textdateien, in denen die Felder durch Zeichen getrennt sind. Struktur unterstützt auch benutzerdefinierte **Serialisierungsprogramm/Deserialisierer (SerDe)** für komplexen oder unregelmäßig strukturierte Daten. Weitere Informationen finden Sie unter [Verwendung einer benutzerdefinierten JSON-SerDe mit HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Benutzerdefinierte Funktion (UDF)

Struktur kann auch durch **benutzerdefinierte Funktion (UDF)**erweitert werden. Eine UDF-Datei können Sie Funktionen oder Logik einfach modelliert ist nicht in HiveQL implementieren. Beispielsweise Struktur mit UDFs finden Sie unter:

* [Mit Java Benutzer definierte Funktion mit Struktur](hdinsight-hadoop-hive-java-udf.md)

* [Python mit Struktur und HDInsight Schwein](hdinsight-python.md)

* [Verwenden von C# mit Struktur und Schweine in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Hinzufügen einer benutzerdefinierten Struktur UDF zum HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Benutzerdefinierte Struktur UDF Beispiel Struktur Timestamp Datums-und Zeitformate konvertieren](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Struktur von internen Tabellen Vs externen Tabellen

Es gibt einige Dinge, die Sie über die Tabelle Struktur und externe Tabelle:

- Der Befehl **CREATE TABLE** erstellt eine interne Tabelle. Die Datei muss im Standardcontainer befinden.
- Der Befehl **CREATE TABLE** verschiebt die Datendatei /hive/Warehouse /<TableName> Ordner.
- Der **Externe Tabelle erstellen** Befehl erstellt eine externe Tabelle. Die Datendatei kann außerhalb der standardmäßige Container befinden.
- Der **Externe Tabelle erstellen** Befehl wird die Datei nicht verschoben.
- Der **Externe Tabelle erstellen** Befehl zulässig keine Ordner im Pfad. Dies ist der Grund des Lernprogramms eine Kopie der Datei sample.log.

Weitere Informationen finden Sie unter [HDInsight: interne Struktur und externen Tabellen Einführung][cindygross-hive-tables].


##<a id="data"></a>Zu den Beispieldaten ein Apache log4j-Datei

Dieses Beispiel verwendet eine *log4j* -Beispieldatei, die **/example/data/sample.log** in den BLOB-Speicher-Container gespeichert werden. Jedes Protokoll in der Datei besteht aus einer Reihe von Feldern enthält eine `[LOG LEVEL]` um den Typ und Schweregrad, zum Beispiel anzuzeigen:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Im vorherigen Beispiel ist die Protokollierungsstufe Fehler.

> [AZURE.NOTE] Auch eine log4j mit [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) Protokollierungstool generieren, und laden die Datei in den BLOB-Container. Weitere Informationen finden Sie in der [Zu HDInsight Daten hochladen](hdinsight-upload-data.md) . Weitere Informationen zur Verwendung von Azure BLOB-Speicher mit HDInsight anzeigen Sie [Mit Azure BLOB-Speicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md)

Die Beispieldaten werden in Azure BLOB-Speicher gespeichert, HDInsight als Standard-Dateisystem verwendet. HDInsight kann mit dem Präfix **Wasb** in Blobs gespeicherte Dateien zugreifen. Zugriff auf die Datei sample.log verwenden Sie z. B. die folgende Syntax:

    wasbs:///example/data/sample.log

Da Azure BLOB-Speicher der Standardspeicher für HDInsight ist, können Sie auch die Datei mithilfe von **/example/data/sample.log** von HiveQL zugreifen.

> [AZURE.NOTE] Die Syntax **Wasbs: / /**, verwendet, um Dateien im Speicher Standardcontainer für HDInsight Cluster zugreifen. Wenn zusätzliche Speicherkonten wird angegeben, wenn Sie Cluster bereitgestellt und Dateien auf diese Konten zugreifen möchten, können Sie zugreifen Daten die Container und Kontoadresse anzugeben, z. B. **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Beispiele: getrennte Daten Spalten projizieren

HiveQL Folgendes Projekt Spalten auf getrennte in gespeicherten Daten die **Wasbs: / / / / Beispieldaten** Verzeichnis:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

Im vorherigen Beispiel durchführen die folgenden Aktionen HiveQL-Anweisung:

* __hive.execution.engine=tez, festlegen__: Legt das Ausführungsmodul Tez verwenden. Tez anstelle von MapReduce kann Abfrage-Leistung bereitstellen. Weitere Informationen zu Tez finden Sie im Abschnitt [Mit Apache Tez zur Verbesserung der Leistung](#usetez) .

    > [AZURE.NOTE] Dies ist nur erforderlich, wenn in einem Windows-basierten HDInsight Cluster; Tez ist das Ausführungsmodul Standard für Linux-basierte HDInsight.

* **DROP TABLE**: die Tabelle und die Datei löscht, wenn die Tabelle bereits vorhanden ist.
* **Externe Tabelle erstellen**: erstellt eine neue **externe** Tabelle Struktur. Externe Tabellen speichern nur die Tabellendefinition in Struktur. die Daten verbleiben am ursprünglichen Speicherort und im ursprünglichen Format.
* **ZEILENFORMAT**: weist Struktur wie die Daten formatiert. In diesem Fall werden die Felder in jedem Protokoll durch ein Leerzeichen getrennt.
* **Gespeichert als Textdatei Speicherort**: weist Struktur die Daten ist (Verzeichnis Beispiel-Daten) gespeichert und wird als Text gespeichert. Die Daten in eine Datei oder mehrere Dateien im Verzeichnis verteilt.
* **Auswählen**: Wählt alle Zeilen, Spalte **t4** Wert **[Fehler]**enthält. Dies sollte der Wert **3** zurück, da drei Zeilen, die diesen Wert enthalten.
* **INPUT__FILE__NAME wie '%.log'** - sagt Struktur, die wir nur Daten aus Dateien zurückgibt. Log. Dies beschränkt die Suche auf sample.log-Datei enthält die Daten und verhindert, dass Sie zurückgeben von Daten aus anderen Dateien, die nicht das Schema definiert.

> [AZURE.NOTE] Externe Tabellen sollte verwendet werden, wenn Sie erwarten, dass die zugrunde liegenden Daten von einer externen Quelle wie ein automatisches Uploadprozess oder anderen MapReduce Vorgang aktualisiert werden, und immer Hive-Abfragen mit den neuesten Daten.
>
> Löschen einer externen Tabelle enthält **nicht** die Daten gelöscht, sondern nur die Tabellendefinition.

Nach dem Erstellen einer externen Tabelle dienen die folgende Anweisung **eine Tabelle** erstellen.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Diese Aussagen werden die folgenden Aktionen durchführen:

* **Erstellen der Tabelle IF NOT EXISTS**: eine Tabelle erstellt, wenn es nicht bereits vorhanden ist. Da **externe** Schlüsselwort nicht verwendet wird, ist dies eine interne Tabelle Struktur Data Warehouse gespeichert und wird vollständig von Struktur verwaltet.
* **Gespeichert als ORK**: optimiert Zeile Einspaltig (ORK) Format speichert. Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten der Struktur.
* ÜBERSCHREIBEN von **Einfügen... Wählen Sie**: Wählt Zeilen aus **log4jLogs** -Tabelle enthält **[Fehler]**und fügt die Daten in der Tabelle **ErrorLogs** .

> [AZURE.NOTE] Im Gegensatz zu externen Tabellen ablegen einer Tabelle auch die zugrunde liegenden Daten gelöscht.

##<a id="usetez"></a>Verwenden Sie Apache Tez für verbesserte Leistung

[Apache Tez](http://tez.apache.org) ist ein Framework, mit dem Daten intensive Anwendung wie Struktur Ebene effizienter ausführen kann. In der neuesten Version von HDInsight unterstützt Struktur auf Tez. Tez ist für Linux-basierte HDInsight-Cluster standardmäßig aktiviert.

> [AZURE.NOTE] Tez ist im Moment deaktiviert standardmäßig für Windows-basierte HDInsight-Cluster und muss aktiviert werden. Um Tez nutzen zu können, muss der folgende Wert für eine Struktur Abfrage festgelegt werden:
>
> ```set hive.execution.engine=tez;```
>
>Diese kann jeweils pro Abfrage unter an den Anfang der Abfrage gesendet werden. Außerdem lassen sich dadurch auf werden standardmäßig in einem Cluster Konfigurationswert bei der Erstellung des Clusters. Weitere Informationen finden Sie im [HDInsight-Cluster bereitstellen](hdinsight-provision-clusters.md).

Der [in Entwurfsdokumenten Tez Struktur](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) enthalten Details zur Implementierung Auswahlmöglichkeiten und tuning-Konfigurationen.

Um Projekte Debuggen war mit Tez HDInsight bietet die folgende Webadresse von Benutzeroberflächen, die Details des Tez Einzelvorgänge anzeigen können:

* [Mithilfe der Tez-Benutzeroberfläche auf Windows-basierten HDInsight](hdinsight-debug-tez-ui.md)

* [Ambari Tez Ansicht auf Linux-basierten HDInsight](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Wählen Sie, wie zum Ausführen des Auftrags HiveQL

HDInsight kann mithilfe verschiedener Methoden HiveQL Aufträge ausgeführt werden. Verwenden Sie in der folgenden Tabelle entscheiden, welche Methode Sie, und folgen Sie dem Link eine exemplarische Vorgehensweise.

| **Verwenden Sie diese Option** , wenn Sie...                                                     | ... eine **interaktive** Shell | ... **Batch** -Verarbeitung | ... mit diesem **Cluster-Betriebssystem** | ... aus dem **Client-Betriebssystem** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Struktur anzeigen](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Alle (browserbasierte) |
| [Luftlinie Befehl (in einer SSH-Sitzung)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [Struktur-Befehl (in einer SSH-Sitzung)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [Aufrollen](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux oder Windows                          | Linux, Unix, Mac OS X oder Windows        |
| [Abfragekonsole](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | Alle (browserbasierte)                            |
| [HDInsight-Tools für Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows                                  |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows                                  |
| [Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Ausführen von Aufträgen Struktur auf Azure HDInsight mit lokalen SQL Server Integration Services

SQL Server Integration Services (SSIS) können Sie eine Stapelverarbeitung Struktur. Azure Feature Pack für SSIS bietet die folgenden Komponenten, die mit Hive-Aufträge HDInsight.


- [Azure HDInsight Struktur Aufgabe][hivetask]
- [Azure Abonnement Verbindungs-Manager][connectionmanager]


Weitere Informationen zu Azure Feature Pack für SSIS [hier][ssispack].


##<a id="nextsteps"></a>Nächste Schritte

Nachdem verwenden Sie gelernt, was Struktur und wie Hadoop in HDInsight verwendet, die folgenden Links zu anderen Methoden zum Arbeiten mit Azure HDInsight.


- [Upload von Daten auf HDInsight][hdinsight-upload-data]
- [Verwenden Sie Schwein mit HDInsight][hdinsight-use-pig]
- [Verwenden Sie Sqoop mit HDInsight](hdinsight-use-sqoop.md)
- [Verwenden Sie Oozie mit HDInsight](hdinsight-use-oozie.md)
- [Verwenden Sie MapReduce Aufträge mit HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
