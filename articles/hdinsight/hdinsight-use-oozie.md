<properties
    pageTitle="Verwenden Sie Hadoop Oozie in HDInsight | Microsoft Azure"
    description="HDInsight, ein Service Datenverlustvorfalls verwenden Sie Hadoop Oozie. Erfahren Sie, wie einen Workflow Oozie und einen Oozie Auftrag senden."
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


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-in-hdinsight"></a>Verwenden Sie Oozie mit Hadoop definieren und Ausführen eines Workflows in HDInsight

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Informationen Sie zur Verwendung von Apache Oozie einen Workflow definieren und Ausführen des Workflows auf HDInsight. Über den Oozie Coordinator finden Sie [Zeitbasierte Hadoop Oozie Koordinator verwenden mit HDInsight][hdinsight-oozie-coordinator-time]. Azure Data Factory finden Sie unter [Verwendung und Struktur mit Daten][azure-data-factory-pig-hive].

Apache Oozie ist ein Workflow-Koordinierung, die Hadoop-Aufträge verwaltet. Hadoop Stapel ist integriert und Hadoop Aufträge für Apache MapReduce Apache Schwein, Apache Struktur und Apache Sqoop unterstützt. Sie können auch Aufträge planen, die spezifisch für ein System, wie Java-Programme oder Shell-Skripts verwendet werden.

Workflow, den Sie implementieren gemäß der Anleitung in diesem Lernprogramm, enthält zwei Aktionen:

![Arbeitsablauf-Diagramm][img-workflow-diagram]

1. Eine Struktur Aktion führt ein HiveQL Skript zum zählen der Vorkommen jedes Typs Ebene in einer log4j-Datei. Jede log4j Datei besteht aus einer Felder mit einem Feld [Ebene], die die Art und den Schweregrad z. B. zeigt:

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

2.  Sqoop Aktion exportiert HiveQL Ausgabe einer Tabelle in einer Azure SQL-Datenbank. Weitere Informationen zu Sqoop finden Sie unter [Verwenden Hadoop Sqoop mit HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Unterstützte Oozie Versionen auf HDInsight-Cluster finden Sie unter [neuen Hadoop Cluster Versionen von HDInsight bereitgestellten?] [hdinsight-versions].

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Eine Workstation mit Azure PowerShell**. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
    
    Windows PowerShell-Skripts ausführen, müssen Sie als Administrator ausführen und Ausführungsrichtlinie auf *RemoteSigned*festgelegt. Weitere Informationen finden Sie unter [Ausführen von Windows PowerShell-Skripts][powershell-script].

##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie Workflow und HiveQL Skripts definieren

Oozie Workflows Definitionen sind in hPDL (eine XML-Process Definition Language) geschrieben. Der Standardname der Workflow ist *workflow.xml*. Folgendes ist der Workflow-Datei in diesem Lernprogramm verwenden.

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

Es gibt zwei Aktionen im Workflow definiert. Start, ist *RunHiveScript*. Wenn die Aktion erfolgreich ausgeführt, wird die nächste Aktion *RunSqoopExport*.

Die RunHiveScript hat mehrere Variablen. Die Werte werden beim übermittelten Druckauftrag Oozie von Ihrer Arbeitsstation aus mit Azure PowerShell übergeben werden.

<table border = "1">
<tr><th>Workflow-Variablen</th><th>Beschreibung</th></tr>
<tr><td>${JobTracker}</td><td>Gibt die URL des Hadoop Job Tracker. Verwenden Sie <strong>Jobtrackerhost:9010</strong> HDInsight 3.0 und 2.1.</td></tr>
<tr><td>${NameNode}</td><td>Gibt die URL des Knotens Hadoop Name. Verwenden Sie beispielsweise die Datei System Standardadresse <i>Wasbs: / /&lt;ContainerName&gt;@&lt;StorageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
<tr><td>${QueueName}</td><td>Gibt den Warteschlangennamen der Auftrag übermittelt. Verwenden Sie die <strong>Standardeinstellung</strong>.</td></tr>
</table>

<table border = "1">
<tr><th>Struktur Aktionsvariable</th><th>Beschreibung</th></tr>
<tr><td>${HiveDataFolder}</td><td>Gibt das Quellverzeichnis für den Befehl Create Table Struktur.</td></tr>
<tr><td>${HiveOutputFolder}</td><td>Gibt den Ausgabeordner für die Anweisung ÜBERSCHREIBEN einfügen.</td></tr>
<tr><td>${HiveTableName}</td><td>Gibt den Namen der Struktur-Tabelle, die die Datendateien log4j verweist.</td></tr>
</table>

<table border = "1">
<tr><th>Sqoop Aktionsvariable</th><th>Beschreibung</th></tr>
<tr><td>${SqlDatabaseConnectionString}</td><td>Gibt die Verbindungszeichenfolge für die SQL Azure.</td></tr>
<tr><td>${SqlDatabaseTableName}</td><td>Gibt die Azure SQL-Datenbanktabelle, in dem die Daten exportiert werden.</td></tr>
<tr><td>${HiveOutputFolder}</td><td>Gibt den Ausgabeordner für die Anweisung ÜBERSCHREIBEN Struktur einfügen. Dies ist die gleichen Ordner Sqoop exportieren (Export-Verzeichnis).</td></tr>
</table>

Weitere Informationen zu Oozie Workflows und Aktivitäten mit Dokumentation [Apache Oozie 4.0] [ apache-oozie-400] (für HDInsight Version 3.0) oder [Apache Oozie 3.3.2 Dokumentation] [ apache-oozie-332] (für HDInsight Version 2.1).


Die Registrierungsstruktur Aktion im Workflow Ruft eine Skriptdatei HiveQL. Diese Datei enthält drei HiveQL-Anweisungen:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **Die DROP TABLE-Anweisung** löscht die log4j Struktur Tabelle, falls vorhanden.
2. **Die CREATE TABLE-Anweisung** erstellt eine externe log4j Hive-Tabelle, verweist auf den Speicherort der Protokolldatei log4j. Das Feldtrennzeichen ",". Zeile Standardtrennzeichen ist "\n". Eine externe Tabelle Struktur dient zu der Datendatei entfernt am ursprünglichen Speicherort Oozie Workflow mehrmals ausgeführt werden soll.
3. **Das Einfügen ÜBERSCHREIBEN Anweisung** zählt die Vorkommen jedes Typs Protokollebene Tabelle Struktur log4j und speichert die Ausgabe in ein Blob in Azure-Speicher.


Es gibt drei Variablen im Skript:

- ${HiveTableName}
- ${HiveDataFolder}
- ${HiveOutputFolder}

Die Workflowdefinitionsdatei (workflow.xml in diesem Lernprogramm) übergibt diese Werte zur Laufzeit an dieses HiveQL.

Der Workflow-Datei und die Datei HiveQL werden in einem BLOB-Container gespeichert.  PowerShell-Skript später in diesem Lernprogramm verwendeten wird das Standardkonto Speicher beide Dateien kopiert. 

##<a name="submit-oozie-jobs-using-powershell"></a>Oozie Aufträge mit PowerShell

Azure PowerShell bietet derzeit Cmdlets für Oozie Aufträge definieren. Das Cmdlet " **Invoke-RestMethod** " können Sie Oozie-Webdienste aufrufen. Oozie Web Services API ist eine HTTP-REST JSON-API. Weitere Informationen zu Webdiensten Oozie API-Dokumentation [Apache Oozie 4.0] [ apache-oozie-400] (für HDInsight Version 3.0) oder [Apache Oozie 3.3.2 Dokumentation] [ apache-oozie-332] (für HDInsight Version 2.1).

PowerShell-Skript in diesem Abschnitt werden folgende Schritte ausgeführt:

1. Verbinden Sie mit Azure.
2. Erstellen Sie eine Azure-Ressourcengruppe. Weitere Informationen finden Sie unter [Verwenden Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).
3. Erstellen einer Azure SQL-Datenbankserver, einer Azure SQL-Datenbank und zwei Tabellen. Diese werden durch die Sqoop-Aktion im Workflow verwendet.

    Der Tabellenname ist *log4jLogCount*.

4. Erstellen Sie einen HDInsight-Cluster Oozie Aufträge ausgeführt.

    Untersuchen Sie den Cluster können Sie Azure-Portal oder Azure PowerShell.

5. Kopieren Sie Oozie Workflow-Datei und die Datei HiveQL in das Standard-Dateisystem

    Beide Dateien werden in einem öffentlichen BLOB-Container gespeichert.
    
    - Kopieren Sie das Skript HiveQL (useoozie.hql) in den Azure-Speicher (wasbs:///tutorials/useoozie/useoozie.hql).
    - Kopieren Sie "Workflow.xml" in wasbs:///tutorials/useoozie/workflow.xml.
    - Kopieren Sie die Datei (/ example/data/sample.log) zu wasbs:///tutorials/useoozie/data/sample.log.
     
6. Senden eines Auftrags Oozie.

    Untersuchen Sie die Auftragsergebnisse OOzie verwenden Sie Visual Studio oder andere Tools zum Azure SQL-Datenbank herstellen.

Hier ist das Skript.  Sie können das Skript von Windows PowerShell ISE ausführen. Sie müssen nur die ersten 7 Variablen.

    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure subscription ID>"
    
    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"
    
    # HDInsight cluster HTTP user credential used for creating and connectin
    $httpUserName = "admin"  # The default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"
    
    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{
        Login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion
    
    #region - Create SQL database tables
    Write-Host "Creating the log4jlogs table  ..." -ForegroundColor Green
    
    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    #endregion
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - copy Oozie workflow and HiveQL files
    
    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green
    
    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    
    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml
    
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql
    
    #endregion
    
    #region - copy the sample.log file
    
    Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
    
    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log
    
    #endregion
    
    #region - submit Oozie job
    
    $storageUri="wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"
    
    $oozieJobName = $namePrefix + "OozieJob"
    
    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10
    
    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"
    
    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
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
        <value>$httpUserName</value>
    </property>
    
    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>
    
    </configuration>
    "@
    
    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerSatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerSatus."
    
    # create Oozie job
    Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."
    
    # start Oozie job
    Write-Host "Starting the Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug
    
    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
    
    Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")
    
    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }
    
    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green
    
    #endregion


**Das Lernprogramm erneut ausführen**

Um den Workflow erneut auszuführen, müssen Sie folgende löschen:

- Die Skriptdatei Ausgabe Struktur
- Die Daten in der Tabelle log4jLogsCount

Hier ist ein PowerShell-Beispielskript, das Sie verwenden können:

    $resourceGroupName = "<AzureResourceGroupName>"
    
    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()

##<a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm haben Sie einen Oozie Workflow definieren und Ausführen ein Auftrags Oozie mithilfe von PowerShell. Weitere finden Sie in folgenden Artikeln:

- [Zeitbasierte Oozie Koordinator verwenden mit HDInsight][hdinsight-oozie-coordinator-time]
- [Erste Schritte mit Hadoop Struktur in HDInsight analysieren Mobiltelefon verwenden][hdinsight-get-started]
- [Verwenden von Azure BLOB-Speicher mit HDInsight][hdinsight-storage]
- [Verwalten von HDInsight mithilfe von PowerShell][hdinsight-admin-powershell]
- [Daten Sie für Projekte in HDInsight Hadoop][hdinsight-upload-data]
- [Verwenden Sie Sqoop mit Hadoop in HDInsight][hdinsight-use-sqoop]
- [Hadoop auf HDInsight Struktur verwenden][hdinsight-use-hive]
- [Hadoop auf HDInsight Schwein verwenden][hdinsight-use-pig]
- [Entwickeln von Java MapReduce Programme für HDInsight][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: ../sql-database-create-configure.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
