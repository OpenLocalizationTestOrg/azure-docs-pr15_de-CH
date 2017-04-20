<properties
    pageTitle="Verwenden Sie Hadoop Oozie Workflows in Linux-basierten HDInsight | Microsoft Azure"
    description="Linux-basierte HDInsight verwenden Sie Hadoop Oozie. Erfahren Sie, wie einen Workflow Oozie und einen Oozie Auftrag senden."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a>Verwenden Sie Oozie mit Hadoop definieren und Ausführen eines Workflows auf Linux-basierten HDInsight

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Erfahren Sie, wie Apache Oozie Workflow definieren, der Struktur und Sqoop verwendet, und führen Sie den Workflow auf einem Linux-basierten HDInsight Cluster.

Apache Oozie ist ein Workflow-Koordinierung, die Hadoop-Aufträge verwaltet. Hadoop Stapel ist integriert und Hadoop Aufträge für Apache MapReduce Apache Schwein, Apache Struktur und Apache Sqoop unterstützt. Es kann auch verwendet werden, Projekte planen, die auf einem System wie Java-Programme oder Shell-Skripts beziehen

> [AZURE.NOTE] Eine weitere Option zum Definieren von Workflows mit HDInsight ist Azure Data Factory. Weitere Informationen zu Azure Data Factory finden Sie unter [Verwendung und Struktur mit Daten][azure-data-factory-pig-hive].

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**: finden Sie unter [Abrufen von Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI**: finden Sie unter [Installieren und konfigurieren die Azure-CLI](../xplat-cli-install.md)
    
    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- **Ein HDInsight-Cluster**: Einführung [zu HDInsight unter Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Ein Azure SQL-Datenbank**: Dies erstellt anhand der Schritte in diesem Dokument

##<a name="example-workflow"></a>Beispielsworkflow

Sie implementieren gemäß der Anleitung in diesem Workflow enthält zwei Aktionen. Aktionen sind Definitionen für Aufgaben wie Struktur, Sqoop, MapReduce oder anderen Prozess ausgeführt:

![Arbeitsablauf-Diagramm][img-workflow-diagram]

1. Eine Struktur Aktion führt ein Skript HiveQL zum Extrahieren von Datensätze aus der **Hivesampletable** in HDInsight enthalten. Jede Datenzeile beschreibt einen Besuch von einem bestimmten Gerät. Datensatzformat wird ähnlich der folgenden angezeigt:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    In diesem Dokument verwendete Struktur-Skript zählt die Besuche für jede Plattform (wie Android oder iPhone) und zählt zu einer neuen Struktur Tabelle speichert.

    Weitere Informationen zur Struktur finden Sie unter [Verwenden Struktur mit HDInsight][hdinsight-use-hive].

2.  Sqoop Aktion exportiert den Inhalt der neuen Tabelle Struktur einer Tabelle in einer Azure SQL-Datenbank. Weitere Informationen zu Sqoop finden Sie unter [Verwenden Hadoop Sqoop mit HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Unterstützte Oozie Versionen auf HDInsight-Cluster finden Sie unter [neuen Hadoop Cluster Versionen von HDInsight bereitgestellten?] [hdinsight-versions].

##<a name="create-the-working-directory"></a>Erstellen Sie das Verzeichnis

Oozie erwartet Ressourcen für ein Projekt im selben Verzeichnis gespeichert werden. Dieses Beispiel verwendet **Wasbs: / / / Tutorials/Useoozie**. Verwenden Sie den folgenden Befehl zum Erstellen dieses Verzeichnis und das Verzeichnis Data, das mit dem Workflow erstellte die neue Struktur-Tabelle enthält:

    hdfs dfs -mkdir -p /tutorials/useoozie/data

> [AZURE.NOTE] Die `-p` Parameter verursacht alle Verzeichnisse im Pfad erstellt werden, wenn sie nicht bereits vorhanden sind. **Das Datenverzeichnis** verwendet Daten vom **useooziewf.hql** -Skript verwendet.

Führen Sie den folgenden Befehl stellt sicher, dass Oozie Ihr Benutzerkonto aufzutreten, wenn Struktur und Sqoop Aufträge auch aus. Ersetzen Sie **USERNAME** durch Ihren Benutzernamen:

    sudo adduser USERNAME users

Wenn Sie eine Fehlermeldung, dass der Benutzer bereits Benutzer angehört, können Sie einfach ignorieren.

##<a name="add-a-database-driver"></a>Hinzufügen eines Datenbanktreibers

Da dieser Workflow Sqoop verwendet, um Daten in SQL-Datenbank exportieren, müssen Sie eine SQL-Datenbank mit verwendeten JDBC-Treibers bereitstellen. Verwenden Sie den folgenden Befehl in das Arbeitsverzeichnis kopieren:

    hdfs dfs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/

Wenn Ihr Workflow andere Ressourcen wie eine JAR-Datei mit einer MapReduce-Anwendung verwendet, müssten Sie diese hinzufügen.

##<a name="define-the-hive-query"></a>Die Hive-Abfrage definieren

Gehen Sie HiveQL-Skript erstellen, das eine Abfrage definiert, die in einem Workflow Oozie weiter unten in diesem Dokument verwendet wird.

1. SSH Verbindung mit Linux-basierte HDInsight-Cluster verwenden:

    * **Linux, Unix oder OS X Clients**: Siehe [Verwendet SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-Clients**: Siehe [Verwendet SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie den folgenden Befehl zum Erstellen einer neuen Datei:

        nano useooziewf.hql

1. Sobald Nano-Editor geöffnet wird, verwenden Sie folgende als Inhalt der Datei:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;

    Es gibt zwei Variablen im Skript:

    - **${HiveTableName}**: enthält den Namen der zu erstellenden Tabelle
    - **${HiveDataFolder}**: enthält den Speicherort die Datendateien für die Tabelle

    Die Workflowdefinitionsdatei (workflow.xml in diesem Lernprogramm) übergibt diese Werte zur Laufzeit an dieses HiveQL.

2. Drücken Sie STRG + X, um den Editor zu verlassen. Bei Aufforderung **Y** zum Speichern der Datei wählen verwenden Sie **EINGABETASTE** , um Dateinamen **useooziewf.hql** verwenden.

3. Verwenden Sie die folgenden Befehle, um **useooziewf.hql** **wasbs:///tutorials/useoozie/useooziewf.hql**zu kopieren:

        hdfs dfs -copyFromLocal useooziewf.hql /tutorials/useoozie/useooziewf.hql

    Diese Befehle speichern **useooziewf.hql** auf Azure Storage-Konto mit diesem Cluster die Datei erhalten, selbst wenn Cluster gelöscht wird. Dadurch können Sie sparen durch Cluster löschen, wenn sie nicht verwendet werden, die Projekte und Workflows.

##<a name="define-the-workflow"></a>Definieren des Workflows

Oozie Workflows Definitionen sind in hPDL (eine XML-Process Definition Language) geschrieben. Gehen Sie zum Definieren des Workflows:

1. Erstellen und Bearbeiten einer neuen Datei verwenden Sie die folgende Anweisung:

        nano workflow.xml

1. Sobald Nano-Editor geöffnet wird, geben Sie Folgendes als Inhalt der Datei:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>
            <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.job.queue.name</name>
                    <value>${queueName}</value>
                </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
            </action>
            <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.compress.map.output</name>
                    <value>true</value>
                </property>
                </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveDataFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\t"</arg>
                <archive>sqljdbc41.jar</archive>
                </sqoop>
            <ok to="end"/>
            <error to="fail"/>
            </action>
            <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>
            <end name="end"/>
        </workflow-app>

    Es gibt zwei Aktionen im Workflow definiert:

    - **RunHiveScript**: Dies ist die Startaktion und **useooziewf.hql** Struktur Skript ausgeführt

    - **RunSqoopExport**: exportiert die Daten aus dem Hive-Skript SQL-Datenbank mit Sqoop erstellt. Dies wird nur ausgeführt, wenn die **RunHiveScript** -Aktion erfolgreich ist.

        > [AZURE.NOTE] Weitere Informationen zu Oozie Workflows und Aktivitäten mit Dokumentation [Apache Oozie 4.0] [ apache-oozie-400] (für HDInsight Version 3.0) oder [Apache Oozie 3.3.2 Dokumentation] [ apache-oozie-332] (für HDInsight Version 2.1).

    Beachten Sie, dass der Workflow mehrere Einträge wie `${jobTracker}`, ersetzt werden durch Werte in der Auftragsdefinition weiter unten in diesem Dokument verwenden.

    Beachten Sie auch die `<archive>sqljdbc4.jar</arcive>` im Sqoop-Abschnitt. Dies weist Oozie dieses Archiv verfügbar machen für Sqoop bei dieser Aktion wird ausgeführt.

2. Verwenden Sie STRG + X und **Y** und **EINGABETASTE** zum Speichern der Datei.

3. Verwenden Sie den folgenden Befehl zum Kopieren der Datei **workflow.xml** , **wasbs:///tutorials/useoozie/workflow.xml**:

        hdfs dfs -copyFromLocal workflow.xml /tutorials/useoozie/workflow.xml

##<a name="create-the-database"></a>Datenbank erstellen

Folgen Sie den Schritten im Dokument [Erstellen einer SQL-Datenbank](../sql-database/sql-database-get-started.md) eine neue Datenbank erstellen. Beim Erstellen der Datenbank verwenden Sie __Oozietest__ als Datenbanknamen. Stellen Sie notieren Sie den Namen für den Datenbankserver auch dies im nächsten Abschnitt erforderlich.

###<a name="create-the-table"></a>Erstellen Sie die Tabelle

> [AZURE.NOTE] Es gibt vielfältige Verbindung zu SQL-Datenbank erstellen. Die folgenden Schritte verwenden [FreeTDS](http://www.freetds.org/) aus dem HDInsight-Cluster.

3. Verwenden Sie den folgenden Befehl FreeTDS im HDInsight-Cluster installieren:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Einmal FreeTDS installiert wurde, verwenden Sie den folgenden Befehl Verbindung mit der SQL Server zuvor erstellte:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest

    Sie erhalten eine Ausgabe ähnlich der folgenden:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

5. Bei der `1>` dazu aufgefordert werden, geben Sie die folgenden Zeilen:

        CREATE TABLE [dbo].[mobiledata](
        [deviceplatform] [nvarchar](50),
        [count] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
        GO

    Wenn die `GO` Anweisung eingegeben wird, wird die vorherige Anweisung ausgewertet. Dadurch wird eine neue Tabelle namens **Mobiledata** in Sqoop geschrieben wird.

    Stellen Sie sicher, dass die Tabelle erstellt wurde anhand der folgenden:

        SELECT * FROM information_schema.tables
        GO

    Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

8. Geben Sie `exit` an der `1>` Fragen Tsql-Dienstprogramm zu beenden.

##<a name="create-the-job-definition"></a>Auftragsdefinition erstellen

Die Auftragsdefinition beschreibt, wo Sie "Workflow.xml", sowie andere Dateien vom Workflow (z. B. useooziewf.hql.) Es definiert auch die Werte für Eigenschaften innerhalb des Workflows verwendet und Dateien.

1. Verwenden Sie den folgenden Befehl vollständigen WASB Standard Speicher abzurufen. Dies wird in Kürze in der Konfigurationsdatei verwendet werden:

        sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml

    Diese sollten Informationen ähnlich der folgenden zurück:

        <name>fs.defaultFS</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>

    Speichern Sie die **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** Wert, wie er in den nächsten Schritten verwendet werden.

2. Verwenden Sie den folgenden Befehl FQDN Hauptknoten Cluster zu. Dies wird für die JobTracker-Adresse für den Cluster verwendet werden. Dies wird in Kürze in der Konfigurationsdatei verwendet werden:

        hostname -f

    Dies gibt Informationen ähnlich der folgenden zurück:

        hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net

    Für die JobTracker verwendete Port ist 8050, die vollständige Adresse für die JobTracker **hn0 CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050**werden.

1. Verwenden Sie folgende Konfiguration Definition der Oozie erstellen:

        nano job.xml

2. Sobald Nano-Editor geöffnet wird, verwenden Sie folgende als Inhalt der Datei:

        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

          <property>
            <name>nameNode</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
          </property>

          <property>
            <name>jobTracker</name>
            <value>JOBTRACKERADDRESS</value>
          </property>

          <property>
            <name>queueName</name>
            <value>default</value>
          </property>

          <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
          </property>

          <property>
            <name>hiveScript</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
          </property>

          <property>
            <name>hiveTableName</name>
            <value>mobilecount</value>
          </property>

          <property>
            <name>hiveDataFolder</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
          </property>

          <property>
            <name>sqlDatabaseConnectionString</name>
            <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
          </property>

          <property>
            <name>sqlDatabaseTableName</name>
            <value>mobiledata</value>
          </property>

          <property>
            <name>user.name</name>
            <value>YourName</value>
          </property>

          <property>
            <name>oozie.wf.application.path</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
          </property>
        </configuration>

    * Ersetzen Sie alle Instanzen von **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** mit dem Wert bereits empfangen.

    > [AZURE.WARNING] Sie müssen den vollständigen Pfad der WASB mit dem Container und Speicher als Teil des Pfades verwenden. Verwenden das Kurzformat (Wasbs: / /) bewirkt, dass die RunHiveScript Aktion fehlschlägt, wenn der Einzelvorgang gestartet ist.

    * Ersetzen Sie **JOBTRACKERADDRESS** durch JobTracker-ResourceManager-Adresse erhalten Sie früher.

    * Ihr Benutzername für den HDInsight-Cluster ersetzen Sie **IhrName** .

    * Ersetzen Sie **ServerName**, **AdminLogin**und **AdminPassword** mit den Informationen zu Ihrem SQL Azure-Datenbank.

    Die Informationen in dieser Datei zum Füllen der Werte in der Datei workflow.xml oder ooziewf.hql (z. B. ${NameNode}.)

    > [AZURE.NOTE] Der Eintrag **oozie.wf.application.path** definiert, wo die Datei workflow.xml finden durch diesen Auftrag ausgeführt, das den Workflow enthält.

2. Verwenden Sie STRG + X und **Y** und **EINGABETASTE** zum Speichern der Datei.

##<a name="submit-and-manage-the-job"></a>Übermitteln und Verwalten der Arbeit

Die folgenden Schritte Befehl Oozie und Oozie Workflows auf dem Cluster zu verwalten. Oozie-Befehl ist eine benutzerfreundliche Oberfläche über [Oozie REST-API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [AZURE.IMPORTANT] Bei Verwendung des Oozie-Befehls müssen Sie den vollqualifizierten Domänennamen für den Hauptknoten HDInsight verwenden. Diesen FQDN ist nur aus dem Cluster oder Cluster in einem virtuellen Netzwerk Azure von anderen Computern im selben Netzwerk.

1. Abrufen der URL für den Oozie-Dienst anhand der folgenden:

        sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml

    Dies gibt den Wert ähnlich der folgenden zurück:

        <name>oozie.base.url</name>
        <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>

    **Http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000-Oozie** -Teil ist die URL mit dem Befehl Oozie.

2. Verwenden Sie folgende Umgebungsvariable für den URL erstellen, also Sie für jeden Befehl geben müssen:

        export OOZIE_URL=http://HOSTNAMEt:11000/oozie

    Ersetzen Sie die URL mit zuvor angezeigt wurde.

3. Anhand der folgenden den Auftrag senden:

        oozie job -config job.xml -submit

    Lädt die Auftragsinformationen aus **job.xml** und Oozie legt dies nicht ausgeführt.

    Nachdem der Befehl abgeschlossen ist, sollte die ID des Auftrags zurückgegeben. Z. B. `0000005-150622124850154-oozie-oozi-W`. Hiermit wird das Projekt verwalten.

4. Zeigt den Status des Auftrags mit dem folgenden Befehl. Geben Sie die Auftrags-ID des vorherigen Befehls zurückgegeben:

        oozie job -info <JOBID>

    Dies gibt Informationen ähnlich der folgenden zurück.

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasbs:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    Dieser Auftrag hat den Status `PREP`, was bedeutet, dass vorgelegt, sondern hat noch nicht begonnen.

4. Verwenden Sie folgende das Projekt starten:

        oozie job -start JOBID

    Überprüfen des Status nach dieser Befehl ausgeführt werden und Informationen für die Aktivitäten innerhalb des Projekts zurückgegeben.

5. Nach dem erfolgreichen Abschluss die Aufgabe können Sie überprüfen, dass Daten generiert und mithilfe der folgenden Befehle in die SQL-Datenbanktabelle exportiert wurden:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest

    Bei der `1>` dazu aufgefordert werden, geben Sie Folgendes ein:

        SELECT * FROM mobiledata
        GO

    Erhalten Sie Informationen, die der folgenden ähnelt:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Weitere Informationen zu den Oozie-Befehl finden Sie unter [Oozie-Befehlszeilentool](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

##<a name="oozie-rest-api"></a>REST-API Oozie

Oozie-REST-API können Sie eigene Tools erstellen, die mit Oozie arbeiten. Es folgen HDInsight bestimmte Informationen über die Oozie-REST-API:

* **URI**: die REST-API kann von außerhalb des Clusters auf zugegriffen werden`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Authentifizierung**: Sie müssen an die API mit dem Clusterdienstkonto HTTP (Admin) und Kennwort authentifizieren. Zum Beispiel:

        curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions

Weitere Informationen über die Oozie-REST-API finden Sie unter [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

##<a name="oozie-web-ui"></a>Oozie Web-Benutzeroberfläche

Die Webbenutzeroberfläche Oozie bietet eine webbasierte Ansicht der Status einer Oozie im Cluster. Dadurch werden Auftragsstatus Auftragsdefinition Konfiguration, ein Diagramm der Aktionen im Auftrag und Protokolle für den Auftrag anzeigen. Sie können auch Details zu Aktivitäten innerhalb eines Auftrags anzeigen.

Um die Webbenutzeroberfläche Oozie zuzugreifen, gehen Sie folgendermaßen vor:

1. SSH-Tunnel mit dem HDInsight-Cluster zu erstellen. Informationen dazu finden Sie unter [Verwenden SSH Tunneling auf Ambari Webbenutzeroberfläche ResourceManager, JobHistory, NameNode, Oozie, und andere Webbenutzeroberfläche](hdinsight-linux-ambari-ssh-tunnel.md).

2. Erstellte Tunnel öffnen Sie Ambari Webbenutzeroberfläche in Ihrem Webbrowser. Der URI für die Website Ambari ist **https://CLUSTERNAME.azurehdinsight.net**. Der Name des Clusters HDInsight Linux-basierten ersetzen Sie **CLUSTERNAME** .

3. Wählen Sie von der linken Seite der Seite **Oozie**und **Quick Links**und schließlich **Webbenutzeroberfläche Oozie**.

    ![Bild der Menüs](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. Die Webbenutzeroberfläche Oozie standardmäßig anzeigen Workflowaufträge ausgeführt. Um alle Workflowaufträge anzuzeigen, wählen Sie **Alle Aufträge**.

    ![Alle Aufträge angezeigt](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Wählen Sie einen Auftrag, der Weitere Informationen über das Projekt anzeigen.

    ![Job-Informationen](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Auf der Registerkarte Auftragsinformationen grundlegende Informationen sowie einzelnen Aktionen innerhalb des Auftrags angezeigt. Mithilfe der Registerkarten oben können Sie die Auftragsdefinition, Konfiguration, Zugriff Job-Protokoll oder eine gesteuerte azyklische Graph (so) des Auftrags anzeigen.

    * **Job-Protokoll**: Klicken Sie auf **GetLogs** , um alle Protokolle für das Projekt oder verwenden Sie das Feld **Geben Sie Suchfilter** Protokolle filtern

        ![Job-Protokoll](./media/hdinsight-use-oozie-linux-mac/joblog.png)

    * **JobDAG**: die so wird grafisch Datenpfade, die durch den Workflow

        ![Auftrag so](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Auswählen eine der Aktionen auf der Registerkarte **Auftrag Info** wird Informationen zur Aktion angezeigt. Beispiel: **RunHiveScript** -Aktion.

    ![Aktion info](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Sie können Details zur Aktivität, einschließlich Links zu **Console-URL**, die mit JobTracker Informationen für das Projekt anzeigen.

##<a name="scheduling-jobs"></a>Planen von Aufträgen

Der Koordinator können Sie ein Start, Ende und Häufigkeit Vorkommen für Aufträge angeben, damit sie für bestimmte Zeiten geplant werden können.

Gehen Sie folgendermaßen vor, um einen Zeitplan für den Workflow zu definieren:

1. Verwenden Sie folgende eine neue Datei namens **coordinator.xml**:

        nano coordinator.xml

    Verwenden Sie folgende als Inhalt der Datei:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
          <action>
            <workflow>
              <app-path>${workflowPath}</app-path>
            </workflow>
          </action>
        </coordinator-app>

    Hinweis verwendet `${...}` Variablen, die in der Auftragsdefinition Werte ersetzt werden. Die Variablen sind:

    * **${CoordFrequency}**: zwischen ausgeführten Instanzen des Auftrags
    * **${CoordStart}**: Startzeit für der Einzelvorgang
    * **${CoordEnd}**: die Endzeit eines Auftrags
    * **${CoordTimezone}**: Koordinator-Jobs werden in einer festen mit keine Sommerzeit (in der Regel mit UTC dargestellt). Diese Zone wird als "Oozie Verarbeitung Timezone" bezeichnet.
    * **${WfPath}**: der Pfad "Workflow.xml"

2. Verwenden Sie STRG + X und **Y** und **EINGABETASTE** zum Speichern der Datei.

3. Verwenden Sie folgende Arbeitsverzeichnis für diesen Auftrag kopieren:

        hadoop fs -copyFromLocal coordinator.xml /tutorials/useoozie/coordinator.xml

4. Verwenden Sie zum Ändern der Datei **job.xml** folgende:

        nano job.xml

    Ändern:

    * Ändern `<name>oozie.wf.application.path</name>` , `<name>oozie.coord.application.path</name>`. Dies weist Oozie Coordinator-Datei anstelle der Workflowdatei ausführen

    * Fügen Sie den folgenden Gruppen werden eine Variable in dem coordinator.xml darauf an "Workflow.xml":

            <property>
              <name>workflowPath</name>
              <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
            </property>

        Ersetzen Sie die Werte für **Mycontainer** und **Mystorageaccount** mit den Werten in anderen Einträge in der Datei job.xml verwendet.

    * Fügen Sie die folgenden, Start, Ende und Häufigkeit der coordinator.xml Datei definieren:

            <property>
              <name>coordStart</name>
              <value>2015-06-25T12:00Z</value>
            </property>

            <property>
              <name>coordEnd</name>
              <value>2015-06-27T12:00Z</value>
            </property>

            <property>
              <name>coordFrequency</name>
              <value>1440</value>
            </property>

            <property>
              <name>coordTimezone</name>
              <value>UTC</value>
            </property>

        Diese legen die Startzeit auf 12:00 Uhr am 25. Juni 2015, die Endzeit am 27. Juni 2015 und das Intervall für die Ausführung dieses Auftrags täglich (die Frequenz wird in Minuten, also 24 x 60 Minuten = 1440 Minuten.) Schließlich wird die Zeitzone auf UTC festgelegt.

5. Verwenden Sie STRG + X und **Y** und **EINGABETASTE** zum Speichern der Datei.

6. Um den Auftrag auszuführen, verwenden Sie den folgenden Befehl ein:

        oozie job -config job.xml -run

    Das Senden und den Auftrag starten.

7. Besuchen Sie die Webbenutzeroberfläche Oozie und Registerkarte **Koordinator Aufträge** sollten Informationen ähnlich der folgenden:

    ![Registerkarte Aufträge Koordinator](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    Beachten Sie den Eintrag **Weiter Materialisierung** . Dies ist, wenn der Auftrag weiter ausgeführt wird.

8. Wie frühere Workflowauftrag auswählen den Projektposten in der Web-Benutzeroberfläche für das Projekt zeigt Informationen:

    ![Koordinator Auftragsinformationen](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    Beachten Sie, dass dies nur erfolgreich ausgeführt wird der Auftrag nicht einzelne Aktionen innerhalb des geplanten Workflows. An, die eine der **Aktion** auswählen. Informationen, die für frühere Workflowauftrag ähnlich erscheint.

    ![Aktion info](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

##<a name="troubleshooting"></a>Problembehandlung

Problembehandlung mit Oozie Aufträge Oozie UI ist sehr hilfreich, Sie einfach beide Oozie Protokolle anzeigen sowie links zu JobTracker Protokolle MapReduce Aufgaben wie Hive-Abfragen. Im Allgemeinen sollte das Muster für die Problembehandlung:

1. Oozie Web-Benutzeroberfläche können Sie den Auftrag anzeigen.

2. Liegt ein Fehler oder Ausfall für eine bestimmte Aktion, wählen Sie die Aktion auf Feld **Fehlermeldung** Weitere Informationen zum Fehler enthält.

3. Verwenden Sie verfügbar ist, die URL der Aktion um weitere Details (z. B. JobTracker-Protokolle) für die Aktion.

Es folgen Fehlermeldungen, die auftreten können, und deren Behebung.

###<a name="ja009-cannot-initialize-cluster"></a>JA009: Cluster kann nicht initialisiert werden.

**Symptome**: der Status ändern in **angehalten**. Details für den Auftrag werden RunHiveScript-Status als **START_MANUAL**angezeigt. Auswählen der Aktion wird die folgende Fehlermeldung angezeigt:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Ursache**: der WASB-Adressen in der Datei **job.xml** enthalten keine Behälter oder Speicher-Kontonamen. WASB-Adressformat muss `wasbs://containername@storageaccountname.blob.core.windows.net`.

**Auflösung**: WASB Adressen der Auftrag ändern.

###<a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie darf keine Identität &lt;Benutzer >

**Symptome**: der Status ändern in **angehalten**. Details für den Auftrag werden RunHiveScript-Status als **START_MANUAL**angezeigt. Auswählen der Aktion wird die folgende Fehlermeldung angezeigt:

    JA002: User: oozie is not allowed to impersonate <USER>

**Ursache**: aktuelle Berechtigungen Oozie angegebene Benutzerkonto Identitätswechsel nicht zulassen.

**Auflösung**: Oozie können Benutzer in der Gruppe **Benutzer** imitieren. Verwenden der `groups USERNAME` der Gruppen an, denen das Benutzerkonto angehört. Ist der Benutzer nicht Mitglied der Gruppe **Benutzer** , verwenden Sie den folgenden Befehl der Gruppe Benutzer hinzu:

    sudo adduser USERNAME users

> [AZURE.NOTE] Es dauert einige Minuten, bevor HDInsight erkennt, dass der Benutzer der Gruppe hinzugefügt wurde.

###<a name="launcher-error-sqoop"></a>Startprogramm Fehler (Sqoop)

**Symptome**: der Status wird geändert, **GETÖTET**. Details für den Auftrag werden RunSqoopExport-Status als **Fehler**angezeigt. Auswählen der Aktion wird die folgende Fehlermeldung angezeigt:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Ursache**: Sqoop kann den Datenbanktreiber benötigt Zugriff auf die Datenbank zu laden.

**Auflösung**: bei Sqoop aus einem Oozie Projekt muss Datenbank verwendeten Treibers für Ressourcen (z. B. workflow.xml) der Auftrag enthalten.

Sie müssen das Archiv mit der Datenbanktreiber aus verweisen die `<sqoop>...</sqoop>` Abschnitt "Workflow.xml".

Beispielsweise würden Sie für den Auftrag in diesem Dokument folgendermaßen verwenden:

1. Kopieren Sie die Datei sqljdbc4.1.jar im Verzeichnis /tutorials/useoozie:

         hadoop fs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar

2. Ändern Sie "Workflow.xml" Fügen Sie Folgendes in eine neue Zeile oberhalb `</sqoop>`:

        <archive>sqljdbc41.jar</archive>

##<a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm haben Sie wie einen Oozie Workflow definieren und einen Oozie-Auftrag ausgeführt. Weitere Informationen zum Arbeiten mit HDInsight finden Sie in den folgenden Artikeln:

- [Zeitbasierte Oozie Koordinator verwenden mit HDInsight][hdinsight-oozie-coordinator-time]
- [Daten Sie für Projekte in HDInsight Hadoop][hdinsight-upload-data]
- [Verwenden Sie Sqoop mit Hadoop in HDInsight][hdinsight-use-sqoop]
- [Hadoop auf HDInsight Struktur verwenden][hdinsight-use-hive]
- [Hadoop auf HDInsight Schwein verwenden][hdinsight-use-pig]
- [Entwickeln von Java MapReduce Programme für HDInsight][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]: storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
