<properties
   pageTitle="Luftlinie Struktur auf HDInsight (Hadoop) arbeiten verwenden | Microsoft Azure"
   description="Informationen Sie zur Verwendung von SSH zu einem Cluster Hadoop in HDInsight, anschließend Hive-Abfragen interaktiv mit Luftlinie. Luftlinie ist ein Dienstprogramm für die Arbeit mit HiveServer2 über JDBC."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Struktur mit Hadoop in HDInsight mit Luftlinie verwenden

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Sie Secure Shell (SSH) mit einer Linux-basierten HDInsight Cluster verbinden und anschließend Hive-Abfragen interaktiv mithilfe des Befehlszeilenprogramms [Luftlinie](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) .

> [AZURE.NOTE] Luftlinie verwendet JDBC Verbindung Struktur. Weitere Informationen zur Verwendung von JDBC mit Struktur finden Sie unter [Verbinden Hive auf Azure HDInsight Struktur JDBC-Treiber verwenden](hdinsight-connect-hive-jdbc-driver.md).

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Ein Linux-basiertes Hadoop auf HDInsight Cluster.

* SSH-Client. Linux, Unix und Mac OS sollte mit einem SSH-Client kommen. Windows-Benutzer müssen einen Client wie [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen.

##<a id="ssh"></a>Verbinden mit SSH

Eine Verbindung zu den vollqualifizierten Domänennamen (FQDN) des Clusters HDInsight Befehl SSH. Der vollqualifizierte Domänenname sein Name den Cluster dann gab **. azurehdinsight.net**. Beispielsweise würde die folgenden zu einem Cluster mit dem Namen **Myhdinsight**verbinden:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Sofern Zertifikatschlüssel für SSH-Authentifizierung** während der HDInsight-Cluster erstellt, müssen Sie den Speicherort des privaten Schlüssels auf dem Clientsystem angeben:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Sofern ein Kennwort zur Authentifizierung SSH** beim Erstellen des HDInsight-Clusters müssen Sie geben das Kennwort ein.

Weitere Informationen über SSH mit HDInsight finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kitten (Windows-basierte Clients)

Windows bietet keine integrierten SSH-Client. Wir empfehlen, **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen über kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>Verwenden Sie den Befehl Luftlinie

1. Nachdem die Verbindung hergestellt ist, verwenden Sie folgende Abstecher zu:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Dies wird den Luftlinie Client starten und JDBC-Url an. Hier `localhost` wird verwendet, da HiveServer2 auf beide Head-Knoten im Cluster ausgeführt, und der Luftlinie direkt auf dem primären Hauptknoten.
    
    Nach Abschluss des Befehls eintreffen an einem `jdbc:hive2://localhost:10001/>` aufgefordert.

3. Luftlinie Befehle beginnen normalerweise mit einem `!` Zeichen, z. B. `!help` Zeigt Hilfe. Die `!` oft weggelassen werden. Beispielsweise `help` kann auch verwendet werden.

    Wenn Sie Hilfe anzeigen, sehen Sie `!sql`, die HiveQL-Anweisungen ausgeführt wird. HiveQL wird jedoch so häufig verwendet, können Sie die obige weglassen `!sql`. Die folgenden beiden Aussagen haben dieselben Ergebnisse. derzeit über Struktur Tabellen angezeigt:
    
        !sql show tables;
        show tables;
    
    Einen neuen Cluster sollte nur eine Tabelle aufgeführt werden: __Hivesampletable__.

4. Anzeigen des Schemas für die Hivesampletable anhand der folgenden:

        describe hivesampletable;
        
    Dadurch wird Folgendes zurückgegeben:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Dies zeigt die Spalten in der Tabelle. Während wir einige Abfragen für diese Daten ausführen konnte, sondern erstellen eine neue Tabelle, um Daten zu laden und Anwenden eines Schemas veranschaulicht.
    
5. Geben Sie die folgende Anweisung zum Erstellen einer neuen Tabelle namens **log4jLogs** mit Beispieldaten mit dem HDInsight-Cluster:

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
    * **INPUT__FILE__NAME wie '%.log'** - sagt Struktur, die wir nur Daten aus Dateien zurückgibt. Log. Normalerweise nur müssen Daten mit dem Schema innerhalb desselben Ordners Sie beim Abfragen mit Struktur, jedoch mit anderen Datenformaten wird die Protokolldatei gespeichert ist.

    > [AZURE.NOTE] Externe Tabellen sollte verwendet werden, die zugrunde liegenden Daten von einer externen Quelle wie ein automatisches Uploadprozess oder anderen MapReduce Vorgang aktualisiert aber immer Hive-Abfragen mit den neuesten Daten erwartet.
    >
    > Löschen einer externen Tabelle ist **nicht** löschen die Daten nur die Tabellendefinition.
    
    Die Ausgabe dieses Befehls sollte ähnlich der folgenden:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Um Abstecher zu beenden, verwenden Sie `!quit`.

##<a id="file"></a>Führen Sie eine HiveQL-Datei

Luftlinie kann auch verwendet werden, eine Datei auszuführen, die HiveQL enthält. Gehen Sie zum Erstellen einer Datei Luftlinie ausführen.

1. Verwenden Sie folgenden Befehl, um eine neue Datei namens __query.hql__:

        nano query.hql
        
2. Sobald der Editor geöffnet wird, verwenden Sie die folgenden als Inhalt der Datei. Diese Abfrage erstellt eine neue 'interne' Tabelle namens **ErrorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Diese Aussagen werden die folgenden Aktionen durchführen:

    * **Erstellen der Tabelle IF NOT EXISTS** - erstellt eine Tabelle, wenn es nicht bereits vorhanden ist. Da **externe** Schlüsselwort nicht verwendet wird, ist eine interne Tabelle Struktur Data Warehouse gespeichert und wird vollständig von Struktur verwaltet.
    * **Gespeichert als ORK** - speichert die Daten optimiert Zeile Einspaltig (ORK)-Format. Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten der Struktur.
    * ÜBERSCHREIBEN von **Einfügen... Wählen Sie** - wählt Zeilen aus der **log4jLogs** -Tabelle, die enthalten **[Fehler]**und fügt die Daten in der Tabelle **ErrorLogs** .
    
    > [AZURE.NOTE] Im Gegensatz zu externen Tabellen löscht Ablegen einer internen Tabelle zugrunde liegenden Daten.
    
3. Zum Speichern der Datei __STRG__+ ___X__und __Y__und schließlich __Geben__eingeben.

4. Anhand der folgenden Luftlinie mit ausführen. Ersetzen Sie __HOSTNAMEN__ mit dem Namen eingegebenen Head-Knoten und __Kennwort__ mit dem Kennwort für das Administratorkonto:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] Die `-i` Parameter Luftlinie startet, führt die Anweisung in der Datei query.hql und bleibt im Luftlinie an der `jdbc:hive2://localhost:10001/>` aufgefordert. Sie können auch ausführen, eine Datei mit der `-f` Parameter wieder zum Bash, nachdem die Datei verarbeitet wurde.

5. Um sicherzustellen, dass die **ErrorLogs** -Tabelle erstellt wurde, verwenden Sie die folgende Anweisung alle Zeilen aus **ErrorLogs**zurückgeben:

        SELECT * from errorLogs;

    Drei Zeilen mit Daten zurückgegeben werden soll, alle mit **[Fehler]** Spalte t4:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Weitere Informationen über Luftlinie Konnektivität

Die Schritte in diesem Dokument verwenden `localhost` Verbindung mit HiveServer2 auf den Hauptknoten Cluster ausgeführt. Können Sie auch den Hostnamen oder den vollqualifizierten Domänennamen der Hauptknoten müssen zusätzliche Schritte für den Prozess (Schritte Hostnamen oder FQDN zu suchen). Mit `localhost` reicht bei Luftlinie den Hauptknoten.

Wenn Sie einen Kantenknoten in Ihrem Cluster mit Luftlinie installiert haben, müssen Sie den Hostnamen oder FQDN der Hauptknoten Verbindung verwenden.

Wenn Sie Abstecher auf einem Client außerhalb des Clusters installiert haben, können Sie mit dem folgenden Befehl verbinden. Der Name des Clusters HDInsight ersetzen Sie __CLUSTERNAME__ . Ersetzen Sie __PASSWORD__ durch das Kennwort für das Administratorkonto (HTTP-Anmeldung).

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Beachten Sie, dass der Parameter-URI beim Ausführen auf einem Hauptknoten oder von einem Rand Knoten innerhalb des Clusters unterscheidet. Deswegen Cluster aus dem Internet an öffentlichen Gateway verwendet, die Datenverkehr über Port 443 leitet. Außerdem werden verschiedene andere Dienste verfügbar gemacht über öffentliche Gateway Port 443 so URI anders als beim Verbindungsaufbau direkt. Verbindung aus dem Internet müssen Sie die Sitzung durch das Kennwort authentifizieren.

##<a id="summary"></a><a id="nextsteps"></a>Nächste Schritte

Wie Sie sehen, bietet der Befehl Luftlinie leicht Hive-Abfragen interaktiv in einem HDInsight-Cluster ausgeführt.

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

