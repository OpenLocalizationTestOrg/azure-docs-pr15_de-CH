<properties
    pageTitle="Linux-basierte HDInsight Hadoop Sqoop verwenden | Microsoft Azure"
    description="Erfahren Sie Sqoop importieren und Exportieren einer Azure SQL-Datenbank bis ein Linux-basiertes Hadoop auf HDInsight Cluster."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Verwenden Sie Sqoop mit Hadoop in HDInsight (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informationen Sie zur Verwendung von Sqoop zum import und export zwischen HDInsight Linux-basierten Cluster und Azure SQL-Datenbank oder SQL Server-Datenbank.

> [AZURE.NOTE] Die Schritte in diesem Artikel verwenden SSH Verbindung zu einem Linux-basierten HDInsight Cluster. Clients mit Windows können Azure PowerShell und HDInsight .NET SDK auch auf Linux-basierten Clustern mit Sqoop arbeiten. Verwenden Sie die Tabstoppauswahl Artikel öffnen.

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Hadoop Cluster HDInsight** und einer __Azure SQL-Datenbank__: die Schritte in diesem Dokument basieren auf den Cluster und die Datenbank mit dem [Cluster erstellen und SQL-Datenbank](hdinsight-use-sqoop.md#create-cluster-and-sql-database) erstellt. Wenn Sie bereits eine HDInsight-Cluster und SQL-Datenbank verfügen, können Sie für die in diesem Dokument verwendeten Werte ersetzen.
- **Arbeitsstation**: ein Computer mit einem SSH-Client.

##<a name="install-freetds"></a>FreeTDS installieren

1. Verwenden Sie SSH auf Linux-basierten HDInsight herstellen. Die Adresse beim Verbinden `CLUSTERNAME-ssh.azurehdinsight.net` und der Port ist `22`.

    Weitere Informationen über SSH Verbindung zu HDInsight finden Sie in folgenden Dokumenten:

    * **Linux, Unix oder OS X Clients**: finden Sie unter [Verbinden einer Linux-basierten HDInsight Cluster von Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windows-Clients**: finden Sie unter [Verbinden einer Linux-basierten HDInsight Cluster von Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Verwenden Sie den folgenden Befehl FreeTDS installiert:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS Schritte dienen zum SQL-Datenbank herstellen.

##<a name="create-the-table-in-sql-database"></a>Die Tabelle in der SQL-Datenbank erstellen

> [AZURE.IMPORTANT] Verwenden Sie ein HDInsight-Cluster und SQL-Datenbank mit den Schritten im [Cluster erstellen und SQL-Datenbank](hdinsight-use-sqoop.md)erstellt werden, ignorieren Sie die Schritte in diesem Abschnitt wie die Datenbank und die Tabelle als Teil der Schritte in diesem Dokument erstellt wurden.

1. Verwenden Sie aus der HDInsight Verbindung SSH folgenden Befehl Verbindung zum SQL-Datenbankserver und erstellen die Tabelle, die in nachfolgenden Schritten verwendet werden:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Sie erhalten eine Ausgabe ähnlich der folgenden:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Bei der `1>` dazu aufgefordert werden, geben Sie die folgenden Zeilen:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Wenn die `GO` Anweisung eingegeben wird, wird die vorherige Anweisung ausgewertet. Die **Mobiledata** -Tabelle wird erstellt zunächst, ein gruppierter Index hinzugefügt (SQL-Datenbank erforderlich.)

    Stellen Sie sicher, dass die Tabelle erstellt wurde anhand der folgenden:

        SELECT * FROM information_schema.tables
        GO

    Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Geben Sie `exit` an der `1>` Fragen Tsql-Dienstprogramm zu beenden.

##<a name="sqoop-export"></a>Sqoop exportieren

3. Der SSH-Verbindung zu HDInsight Se den folgenden Befehl, um sicherzustellen, dass Sqoop die SQL-Datenbank sehen:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Dies sollte eine Liste der Datenbanken, einschließlich der **Sqooptest** -Datenbank, die zuvor erstellte zurückgeben.

4. Befehl die folgenden Daten aus **Hivesampletable** in der **Mobiledata** Tabelle exportieren:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    Dies weist Sqoop SQL-Datenbank mit der Datenbank **Sqooptest** und Exportieren von Daten aus der **Wasbs: / / / Struktur/Warehouse/Hivesampletable** (physische Dateien für *Hivesampletable*) der **Mobiledata** -Tabelle.

5. Nach Abschluss des Befehls verwenden Sie folgende Verbindung zu der Datenbank TSQL:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Sobald verbunden, verwenden Sie die folgenden Aussagen um zu überprüfen, ob die Daten der **Mobiledata** -Tabelle wurde:

        SELECT * FROM mobiledata
        GO

    Sie sollten eine Liste der Daten in der Tabelle angezeigt. Typ `exit` Tsql-Dienstprogramm beenden.

##<a name="sqoop-import"></a>Sqoop importieren

1. Verwenden Sie die folgenden Daten aus der **Mobiledata** -Tabelle in SQL-Datenbank zu importieren der **Wasbs: / / / Tutorials/Usesqoop/Importeddata** Verzeichnis auf HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Die importierten Daten müssen durch ein Tabstoppzeichen getrennte Felder und Zeilen durch neue-Zeile-Zeichen beendet.

2. Sobald der Import abgeschlossen ist, verwenden Sie folgenden Befehl Liste Daten in das neue Verzeichnis:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>Mithilfe von SQL Server

Sie können auch Sqoop importieren und Exportieren von Daten aus SQL Server in Ihrem Rechenzentrum oder auf einem virtuellen Computer in Azure gehostet. Die Unterschiede zwischen SQL-Datenbank mit SQL Server sind:

* HDInsight und SQL Server muss auf demselben virtuellen Azure-Netzwerk

    > [AZURE.NOTE] HDInsight unterstützt nur standortbasierte virtuelle Netzwerke und es funktioniert derzeit nicht mit virtuellen Netzwerken Gruppe basiert.

    Bei Verwendung von SQL Server im Datencenter müssen Sie das virtuelle Netzwerk *zwischen Standorten* oder *Punkt-zu-Standort*konfigurieren.

    > [AZURE.NOTE] Für virtuelle **Punkt-zu-Standort** -Netzwerke muss SQL Server den VPN-Client Configuration Application ausgeführt werden aus dem **Dashboard** der Azure virtuelle Netzwerkkonfiguration.

    Weitere Informationen Azure Virtual Network finden Sie unter [Übersicht über Virtual Network](../virtual-network/virtual-networks-overview.md).

* SQL Server muss für SQL-Authentifizierung konfiguriert werden. Weitere Informationen finden Sie unter [Auswählen eines Authentifizierungsmodus](https://msdn.microsoft.com/ms144284.aspx)

* Sie müssen SQL Server um Remoteverbindungen konfigurieren. Weitere Informationen finden Sie [Problembehandlung bei Herstellen einer Verbindung mit der SQL Server-Datenbank-engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)

* Erstellen Sie die **Sqooptest** -Datenbank in SQL Server mithilfe eines Dienstprogramms wie **SQL Server Management Studio** oder **Tsql** - Schritte zur Verwendung der Azure-Befehlszeilenschnittstelle arbeiten nur Azure SQL-Datenbank

    TSQL-Anweisung zum Erstellen der Tabelle **Mobiledata** sind vergleichbar mit Ausnahme Erstellen einer stehen für SQL-Datenbank verwendet Index – Dies ist nicht erforderlich für SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Beim Verbinden mit SQL Server von HDInsight müssen Sie die IP-Adresse von SQL Server verwenden, wenn Sie eine Domäne Name System (DNS) zum Auflösen von Namen in Azure Virtual Network konfiguriert haben. Zum Beispiel:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Grenzen

* Massenexport - mit Linux-basierte HDInsight, Sqoop-Connector verwendet, um Daten in Azure SQL-Datenbank oder Microsoft SQL Server exportieren unterstützt derzeit keine Bulk INSERT.

* Batchverarbeitung von-mit Linux-basierte HDInsight bei Verwendung der `-batch` fügt beim wechseln, wird Sqoop mehrere fügt anstelle von Batchvorgängen einfügen.

##<a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie gelernt, wie Sqoop verwenden. Weitere Informationen finden Sie unter:

- [Verwenden Sie Oozie mit HDInsight][hdinsight-use-oozie]: mit Sqoop-Aktion in einem Oozie-Workflow.
- [Datenanalyse Flug Verzögerung mit HDInsight][hdinsight-analyze-flight-data]: Flug analysieren Struktur verwenden Daten verzögern und Sqoop können um Daten mit einer Azure SQL-Datenbank zu exportieren.
- [Upload von Daten auf HDInsight][hdinsight-upload-data]: andere Methoden zum Hochladen von Daten zum HDInsight-Azure BLOB-Speicher.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
