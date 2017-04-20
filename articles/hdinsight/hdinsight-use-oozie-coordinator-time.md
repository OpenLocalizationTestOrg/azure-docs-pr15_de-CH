<properties
    pageTitle="Verwenden Sie zeitbasierte Hadoop Oozie Coordinator in HDInsight | Microsoft Azure"
    description="HDInsight, ein Service Datenverlustvorfalls zeitbasierte Hadoop Oozie Coordinator verwenden. Informationen Sie zum Oozie Workflows und Koordinatoren und Aufträge."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Verwenden Sie zeitbasierte Oozie Coordinator Hadoop in HDInsight Workflows definieren und koordinieren Aufträge

In diesem Artikel erfahren Sie, wie Workflows und Koordinatoren definieren und Auslösen der Koordinator Einzelvorgänge anhand. Es ist hilfreich zu [Verwenden Oozie mit HDInsight] [ hdinsight-use-oozie] bevor Sie diesen Artikel lesen. Neben Oozie können Sie auch mit Azure Data Factory planen. Azure Data Factory finden Sie unter [Verwendung und Struktur mit Daten](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] Dieser Artikel ist einen Windows-basierte HDInsight-Cluster erforderlich. Informationen über Oozie finden Sie unter einschließlich zeitbasierte Aufträge auf einem Linux-basierten Cluster [Mit Oozie Hadoop definieren und Ausführen eines Workflows auf Linux-basierten HDInsight](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Was ist Oozie

Apache Oozie ist ein Workflow-Koordinierung, die Hadoop-Aufträge verwaltet. Hadoop Stapel ist integriert und Hadoop Aufträge für Apache MapReduce Apache Schwein, Apache Struktur und Apache Sqoop unterstützt. Auch kann verwendet werden, um Projekte planen, die auf einem System Shellskripts oder Java-Programme beziehen.

Die folgende Abbildung zeigt den Workflow zu implementierende:

![Arbeitsablauf-Diagramm][img-workflow-diagram]

Der Workflow enthält zwei Aktionen:

1. Eine Struktur Aktion führt ein HiveQL Skript zum zählen der Vorkommen jedes Typs Ebene in einer Protokolldatei log4j. Jedes Protokoll log4j umfasst eine Zeile von Feldern mit einer [Ebene] Feld um den Typ und Schweregrad, beispielsweise:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Die Ausgabe Struktur ähnelt:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Weitere Informationen zur Struktur finden Sie unter [Verwenden Struktur mit HDInsight][hdinsight-use-hive].

2.  Sqoop Aktion exportiert HiveQL Aktion Ausgabe einer Tabelle in einer Azure SQL-Datenbank. Weitere Informationen zu Sqoop finden Sie unter [Verwenden Sqoop mit HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Unterstützte Oozie Versionen auf HDInsight-Cluster finden Sie unter [neuen Cluster Versionen von HDInsight bereitgestellten?] [hdinsight-versions].


##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Eine Workstation mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Ein HDInsight-Cluster**. Informationen zum Erstellen eines HDInsight-Clusters finden Sie unter [Erstellen HDInsight-Cluster][hdinsight-provision], oder [beginnen Sie mit HDInsight][hdinsight-get-started]. Sie benötigen die folgenden Daten in das Tutorial:

    <table border = "1">
    <tr><th>Clustereigenschaft</th><th>Windows PowerShell Variablennamen</th><th>Wert</th><th>Beschreibung</th></tr>
    <tr><td>HDInsight Clustername</td><td>$clusterName</td><td></td><td>Der HDInsight-Cluster, auf dem Sie dieses Lernprogramm ausführen.</td></tr>
    <tr><td>HDInsight Cluster-Benutzername</td><td>$clusterUsername</td><td></td><td>HDInsight Cluster Benutzername. </td></tr>
    <tr><td>HDInsight Cluster-Benutzerkennwort </td><td>$clusterPassword</td><td></td><td>Das HDInsight Cluster-Benutzerkennwort.</td></tr>
    <tr><td>Kontoname Azure-Speicher</td><td>$storageAccountName</td><td></td><td>Ein Azure Storage-Konto für den HDInsight-Cluster verfügbar. In diesem Lernprogramm verwenden Sie das Standardkonto für Speicher, das bei der Bereitstellung Cluster angegeben.</td></tr>
    <tr><td>Azure Blob Containername</td><td>$containerName</td><td></td><td>Verwenden Sie beispielsweise Azure Blob-Behälter, die für das standardmäßige HDInsight Cluster-Dateisystem verwendet wird. Standardmäßig hat er den gleichen Namen wie der HDInsight-Cluster.</td></tr>
    </table>

- **Ein Azure SQL-Datenbank**. Konfigurieren Sie eine Firewall-Regel für den SQL-Datenbankserver auf der Arbeitsstation zugreifen. Eine Anleitung zum Erstellen einer SQL Azure-Datenbank und Konfigurieren der Firewall finden Sie unter [Erste Schritte mit SQL Azure-Datenbank][sqldatabase-get-started]. Dieser Artikel enthält ein Windows PowerShell-Skript zum Erstellen von Azure SQL-Datenbanktabelle, die für dieses Lernprogramm erforderlich.

    <table border = "1">
    <tr><th>SQL Datenbank-Eigenschaft</th><th>Windows PowerShell Variablennamen</th><th>Wert</th><th>Beschreibung</th></tr>
    <tr><td>Name des SQL-Datenbankservers</td><td>$sqlDatabaseServer</td><td></td><td>Der SQL-Datenbankserver, Sqoop Daten exportieren. </td></tr>
    <tr><td>SQL Datenbank-Benutzername</td><td>$sqlDatabaseLogin</td><td></td><td>SQL Datenbank-Benutzername.</td></tr>
    <tr><td>SQL Datenbank-Passwort</td><td>$sqlDatabaseLoginPassword</td><td></td><td>SQL Datenbank-Anmeldekennwort.</td></tr>
    <tr><td>Name der SQL-Datenbank</td><td>$sqlDatabaseName</td><td></td><td>Der SQL Azure-Datenbank, Sqoop Daten exportieren. </td></tr>
    </table>

    > [AZURE.NOTE] Standardmäßig ermöglicht eine SQL Azure-Datenbank Verbindungen von Azure Services wie Azure HDInsight. Wenn diese firewalleinstellung deaktiviert ist, müssen Sie es aus dem Azure-Portal aktivieren. Anleitung zum Erstellen einer SQL-Datenbank und Konfigurieren von Firewall-Regeln finden Sie unter [Erstellen und Konfigurieren der SQL-Datenbank][sqldatabase-get-started].


> [AZURE.NOTE] Eingeben der Werte in den Tabellen. Für dieses Lernprogramm durchlaufen werden.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie Workflow und HiveQL Skripts definieren

Oozie Workflows Definitionen sind in hPDL (eine XML-Process Definition Language) geschrieben. Der Standardname der Workflow ist *workflow.xml*.  Sie die Workflowdatei lokal speichern und dann mithilfe von Azure PowerShell später in diesem Lernprogramm HDInsight Cluster bereitstellen.

Die Registrierungsstruktur Aktion im Workflow Ruft eine Skriptdatei HiveQL. Diese Datei enthält drei HiveQL-Anweisungen:

1. **Die DROP TABLE-Anweisung** löscht die log4j Struktur Tabelle, falls vorhanden.
2. **Die CREATE TABLE-Anweisung** erstellt eine log4j Struktur externe Tabelle, die den Speicherort der Protokolldatei log4j verweist.
3.  **Der Speicherort der Protokolldatei log4j**. Das Feldtrennzeichen ",". Zeile Standardtrennzeichen ist "\n". Eine externe Tabelle Struktur dient die Datei am ursprünglichen Speicherort entfernt zu vermeiden, falls der Oozie-Workflow mehrmals ausgeführt werden soll.
3. **Das Einfügen ÜBERSCHREIBEN Anweisung** zählt die Vorkommen jedes Typs Protokollebene Tabelle Struktur log4j und speichert die Ausgabe an einen Speicherort Azure Blob.

**Hinweis**: Es ist ein bekanntes Problem der Struktur Pfad. Sie werden beim Senden eines Auftrags Oozie dieses Problem auftreten. Die Schritte zum Beheben des Problems finden Sie im TechNet-Wiki: [HDInsight Struktur Fehler: konnte nicht umbenannt werden][technetwiki-hive-error].

**Definieren Sie die Skriptdatei HiveQL vom Workflow aufgerufen werden**

1. Erstellen Sie eine Textdatei mit folgendem Inhalt:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Es gibt drei Variablen im Skript:

    - ${HiveTableName}
    - ${HiveDataFolder}
    - ${HiveOutputFolder}

    Die Workflowdefinitionsdatei (workflow.xml in diesem Lernprogramm) übergibt diese Werte an dieses HiveQL zur Laufzeit.

2. Speichern Sie die Datei als **C:\Tutorials\UseOozie\useooziewf.hql** mit Codierung ANSI (ASCII). (Verwenden Sie Editor, wenn Text-Editor diese Option nicht.) Diese Datei wird später im Lernprogramm HDInsight Cluster bereitgestellt werden.



**Einen Workflow definiert**

1. Erstellen Sie eine Textdatei mit folgendem Inhalt:

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
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
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
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    Es gibt zwei Aktionen im Workflow definiert. Start, ist *RunHiveScript*. Wenn die Aktion *OK*ausgeführt, wird die nächste Aktion *RunSqoopExport*.

    Die RunHiveScript hat mehrere Variablen. Die Werte werden beim übermittelten Druckauftrag Oozie von Ihrer Arbeitsstation aus mit Azure PowerShell übergeben werden.

    <table border = "1">
    <tr><th>Workflow-Variablen</th><th>Beschreibung</th></tr>
    <tr><td>${JobTracker}</td><td>Geben Sie die URL des Hadoop Job Tracker. Verwenden Sie <strong>Jobtrackerhost:9010</strong> HDInsight Clusterversion 3.0 und 2.0.</td></tr>
    <tr><td>${NameNode}</td><td>Geben Sie die URL des Knotens Hadoop Name. Verwenden Sie die standardmäßige Datei System Wasbs: / / Adresse, z. B. <i>Wasbs: / /&lt;ContainerName&gt;@&lt;StorageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${QueueName}</td><td>Gibt den Warteschlangennamen der Auftrag übermittelt. <strong>Verwenden.</strong></td></tr>
    </table>


    <table border = "1">
    <tr><th>Struktur Aktionsvariable</th><th>Beschreibung</th></tr>
    <tr><td>${HiveDataFolder}</td><td>Das Quellverzeichnis für den Befehl Create Table Struktur.</td></tr>
    <tr><td>${HiveOutputFolder}</td><td>Der Ausgabeordner für die Anweisung ÜBERSCHREIBEN einfügen.</td></tr>
    <tr><td>${HiveTableName}</td><td>Der Name der Hive-Tabelle, die log4j Dateien verweist.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Sqoop Aktionsvariable</th><th>Beschreibung</th></tr>
    <tr><td>${SqlDatabaseConnectionString}</td><td>SQL Datenbank-Verbindungszeichenfolge.</td></tr>
    <tr><td>${SqlDatabaseTableName}</td><td>Die Azure SQL-Datenbanktabelle, in dem die Daten exportiert werden.</td></tr>
    <tr><td>${HiveOutputFolder}</td><td>Der Ausgabeordner für die Anweisung ÜBERSCHREIBEN Struktur einfügen. Dies ist die gleichen Ordner Sqoop exportieren (Export-Verzeichnis).</td></tr>
    </table>

    Weitere Informationen zu Oozie Workflow Workflow Aktionen Dokumentation [Apache Oozie 4.0] [ apache-oozie-400] (für HDInsight-Clusterversion 3.0) oder [Apache Oozie 3.3.2 Dokumentation] [ apache-oozie-332] (für HDInsight-Clusterversion 2.1).

2. Speichern Sie die Datei als **C:\Tutorials\UseOozie\workflow.xml** mit Codierung ANSI (ASCII). (Verwenden Sie Editor, wenn Text-Editor diese Option nicht.)

**Koordinator definieren**

1. Erstellen Sie eine Textdatei mit folgendem Inhalt:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Es gibt fünf Variablen in der Definitionsdatei:

  	| Variable          | Beschreibung |
  	| ------------------|------------ |
  	| ${CoordFrequency} | Auftrag der Verzögerung. Häufigkeit wird immer in Minuten ausgedrückt. |
  	| ${CoordStart}     | Startzeit des Auftrags. |
  	| ${CoordEnd}       | Endzeit eines Auftrags. |
  	| ${CoordTimezone}  | Oozie verarbeitet Koordinator Aufträge in einer festen Zeitzone keine Sommerzeit (in der Regel mit UTC dargestellt). Diese Zone bezeichnet als "Oozie Verarbeitung Timezone". |
  	| ${WfPath}         | Der Pfad für die Datei workflow.xml.  Ist der Dateiname Workflow nicht den Standardnamen (workflow.xml), müssen Sie diese angeben. |

2. Speichern Sie die Datei als **C:\Tutorials\UseOozie\coordinator.xml** mit Codierung ANSI (ASCII). (Verwenden Sie Editor, wenn Text-Editor diese Option nicht.)

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Bereitstellen Sie des Projekts Oozie und bereiten Sie des Lernprogramms vor

Sie werden ein Azure PowerShell Skript die folgenden Schritte ausführen:

- Kopieren Sie das Skript HiveQL (useoozie.hql) in Azure BLOB-Speicher wasbs:///tutorials/useoozie/useoozie.hql.
- Kopieren Sie "Workflow.xml" in wasbs:///tutorials/useoozie/workflow.xml.
- Kopieren Sie coordinator.xml in wasbs:///tutorials/useoozie/coordinator.xml.
- Kopieren Sie die Datei (/ example/data/sample.log) zu wasbs:///tutorials/useoozie/data/sample.log.
- Erstellen einer Datenbanktabelle SQL Azure Sqoop exportieren Daten gespeichert. Der Tabellenname ist *log4jLogCount*.

**HDInsight Speicher verstehen**

HDInsight verwendet Azure BLOB-Speicher zum Speichern von Daten. Wasbs: / / Microsoft Implementierung des Hadoop distributed File Systems (HDFS) in Azure BLOB-Speicher. Weitere Informationen finden Sie unter [verwenden Azure BLOB-Speicher mit HDInsight][hdinsight-storage].

Wenn Sie einen HDInsight-Cluster bereitstellen, ein Konto Azure BLOB-Speicher und einen bestimmten Container von diesem Konto bezeichnet als Standard-Dateisystem wie in bietet. Neben diesem Speicherkonto können Sie zusätzlichen Speicherkonten aus der gleichen Azure oder anderen Azure-Abonnements beim Bereitstellungsprozess hinzufügen. Informationen zum Hinzufügen von zusätzlichen Speicher-Konten finden Sie unter [Bereitstellung HDInsight-Cluster][hdinsight-provision]. Zur Vereinfachung dieses Lernprogramms verwendeten Azure PowerShell-Skripts werden alle Dateien in der Datei System Standardcontainer am */tutorials/useoozie*gespeichert. Dieser Container hat standardmäßig denselben Namen wie HDInsight Cluster.
Die Syntax lautet:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Nur die *Wasb: / /* Syntax HDInsight Cluster Version 3.0 unterstützt. Das ältere *Luftsauerstoff: / /* -Syntax in HDInsight 2.1 und 1,6 Cluster unterstützt, jedoch wird in HDInsight 3.0-Clustern nicht unterstützt.

> [AZURE.NOTE] Der Wasb: / / Pfad einen virtuellen Pfad. Weitere Informationen finden Sie unter [verwenden Azure BLOB-Speicher mit HDInsight][hdinsight-storage].

Eine Datei im Datei-System Standardcontainer möglich von HDInsight mithilfe der folgenden URIs (Ich bin workflow.xml als Beispiel verwendet):

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Wenn Sie die Datei direkt aus dem Storage-Konto zugreifen möchten, ist der BLOB-Namen für die Datei:

    tutorials/useoozie/workflow.xml

**Verstehen der Struktur interne und externe Tabellen**

Es gibt ein paar Dinge wissen über interne und externe Hive-Tabellen:

- Der Befehl CREATE TABLE erstellt eine interne, auch verwaltete Tabelle. Die Datei muss im Standardcontainer befinden.
- Der Befehl CREATE TABLE verschiebt die Datendatei /hive/Warehouse /<TableName> Ordner im Standardcontainer.
- Der externe Tabelle erstellen Befehl erstellt eine externe Tabelle. Die Datendatei kann außerhalb der standardmäßige Container befinden.
- Der externe Tabelle erstellen Befehl wird die Datei nicht verschoben.
- EXTERNE Tabelle erstellen Befehl nicht Unterordner unter dem Ordner zu, die in der LOCATION-Klausel angegeben ist. Dies ist der Grund des Lernprogramms eine Kopie der Datei sample.log.

Weitere Informationen finden Sie unter [HDInsight: interne Struktur und externen Tabellen Einführung][cindygross-hive-tables].

**Vorbereiten des Lernprogramms**

1. Windows PowerShell ISE öffnen (klicken Sie im Bildschirm Windows 8 Starten geben **PowerShell_ISE**und dann auf **Windows PowerShell ISE**. Weitere Informationen finden Sie unter [Starten von Windows PowerShell auf Windows 8 und Windows][powershell-start]).
2. Führen Sie den folgenden Befehl für die Verbindung zu Ihrem Azure-Abonnement im unteren Bereich:

        Add-AzureAccount

    Sie werden aufgefordert, Ihre Azure-Konto-Anmeldeinformationen eingeben. Diese Methode zum Hinzufügen von Abonnements Verbindung Timeout und nach 12 Stunden muss das Cmdlet erneut ausführen.

    > [AZURE.NOTE] Wenn Sie mehrere Azure-Abonnements und das Standardabonnement ist nicht die, die Sie verwenden möchten, verwenden Sie das Cmdlet <strong>Select AzureSubscription</strong> ein Abonnement auswählen.

3. Kopieren Sie das folgende Skript in das Skriptfenster, und legen Sie dann die ersten sechs Variablen:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Beschreibung der Variablen im Abschnitt [erforderliche Komponenten](#prerequisites) in diesem Lernprogramm.

3. Das Skript im Skriptfenster Folgendes hinzufügen:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Klicken Sie auf **Skript ausführen** oder drücken Sie **F5** , um das Skript auszuführen. Die Ausgabe ist ähnlich:

    ![Tutorial zur Vorbereitung Ausgabe][img-preparation-output]

##<a name="run-the-oozie-project"></a>Führen Sie das Projekt Oozie

Azure PowerShell bietet derzeit Cmdlets für Oozie Aufträge definieren. Das Cmdlet " **Invoke-RestMethod** " können Sie Oozie-Webdienste aufrufen. Oozie Web Services API ist eine HTTP-REST JSON-API. Weitere Informationen zu Webdiensten Oozie API-Dokumentation [Apache Oozie 4.0] [ apache-oozie-400] (für HDInsight-Clusterversion 3.0) oder [Apache Oozie 3.3.2 Dokumentation] [ apache-oozie-332] (für HDInsight-Clusterversion 2.1).

**Senden ein Auftrags Oozie**

1. Windows PowerShell ISE öffnen (klicken Sie in Windows 8 Startbildschirm, geben Sie **PowerShell_ISE**und dann auf **Windows PowerShell ISE**. Weitere Informationen finden Sie unter [Starten von Windows PowerShell auf Windows 8 und Windows][powershell-start]).

3. Kopieren Sie das folgende Skript in das Skriptfenster, und legen Sie die ersten vierzehn Variablen (überspringen, **$storageUri**).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Beschreibung der Variablen im Abschnitt [erforderliche Komponenten](#prerequisites) in diesem Lernprogramm.

    $coordstart und $coordend sind Workflow Start- und Endzeit. UTC-GMT-Zeit suchen Sie "utc Time" auf bing.com. Die $coordFrequency ist Häufigkeit in Minuten der Workflow ausgeführt werden soll.

3. Folgende an das Skript anhängen. Dieser Teil definiert die Nutzlast Oozie:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
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
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Der Hauptunterschied gegenüber die Übertragungsdatei Nutzlast Workflow wird die Variable **oozie.coord.application.path**. Wenn ein Workflowauftrag senden, verwenden Sie stattdessen **oozie.wf.application.path** .

4. Folgende an das Skript anhängen. Dieser Teil überprüft Oozie Web Service:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Folgende an das Skript anhängen. Dieses Webpart erstellt einen Oozie Auftrag:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Beim Senden eines Workflowauftrags stellen Sie einen anderen Webdienst aufrufen, um den Auftrag zu starten, nachdem der Auftrag erstellt wurde. In diesem Fall wird der Koordinator Auftrag Zeit ausgelöst. Der Auftrag wird automatisch gestartet.

6. Folgende an das Skript anhängen. Dieser Teil überprüft den Status Oozie:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Optional) Folgende an das Skript anhängen.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. Das Skript Folgendes hinzufügen:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    Entfernen Sie die #-Zeichen ggf. zusätzlichen Funktionen ausführen.

7. Ersetzen Sie HDinsight Cluster Version 2.1 "https://$clusterName.azurehdinsight.net:443/oozie/v2/" mit "https://$clusterName.azurehdinsight.net:443/oozie/v1/". HDInsight Clusterversion 2.1 wird nicht unterstützt, Version 2 der Webdienste.

7. Klicken Sie auf **Skript ausführen** oder drücken Sie **F5** , um das Skript auszuführen. Die Ausgabe ist ähnlich:

    ![Ausführen des Lernprogramms Workflow-Ausgabe][img-runworkflow-output]

8. Verbinden Sie mit der SQL-Datenbank die exportierten Daten.

**Überprüfen Sie das Fehlerprotokoll Auftrag**

Um einen Workflow zu beheben, die Protokolldatei Oozie am C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log von Cluster Hauptknoten finden. Informationen zu RDP finden Sie unter [Verwalten von HDInsight Cluster Azure-Portal mit][hdinsight-admin-portal].

**Das Lernprogramm erneut**

Um den Workflow erneut auszuführen, müssen Sie die folgenden Aufgaben ausführen:

- Löschen Sie die Skriptdatei Ausgabe Struktur
- Löschen der Daten in der Tabelle log4jLogsCount.

Hier ist ein Windows PowerShell-Beispielskript, das Sie verwenden können:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm haben Sie einen Workflow Oozie und Oozie-Koordinator definieren und einen Oozie Coordinator Auftrag mithilfe von Azure PowerShell ausführen. Weitere finden Sie in folgenden Artikeln:

- [Erste Schritte mit HDInsight][hdinsight-get-started]
- [Verwenden von Azure BLOB-Speicher mit HDInsight][hdinsight-storage]
- [Verwalten von HDInsight mithilfe von Azure PowerShell][hdinsight-admin-powershell]
- [Upload von Daten auf HDInsight][hdinsight-upload-data]
- [Verwenden Sie Sqoop mit HDInsight][hdinsight-use-sqoop]
- [Struktur mit HDInsight verwenden][hdinsight-use-hive]
- [Verwenden Sie Schwein mit HDInsight][hdinsight-use-pig]
- [Entwickeln von Java MapReduce Programme für HDInsight][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
