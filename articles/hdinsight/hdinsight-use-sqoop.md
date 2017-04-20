<properties
    pageTitle="Verwenden Sie Hadoop Sqoop in HDInsight | Microsoft Azure"
    description="Informationen Sie zum Verwenden von Azure PowerShell von einer Arbeitsstation Sqoop Import und export zwischen Hadoop-Cluster und einer Azure SQL-Datenbank."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>Verwenden Sie Sqoop mit Hadoop in HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informationen Sie zum Verwenden von Sqoop in HDInsight importieren und Exportieren von HDInsight-Cluster und SQL Azure oder SQL Server-Datenbank.

Hadoop ist eine gute Wahl für die Verarbeitung von unstrukturierter und teilstrukturierte Daten wie Protokolle und Dateien auch möglicherweise müssen strukturierte Daten verarbeiten, die in relationalen Datenbanken gespeichert sind.

[Sqoop] [ sqoop-user-guide-1.4.4] ist ein Tool zum Übertragen von Daten zwischen Hadoop-Cluster und relationalen Datenbanken. Sie können Daten aus einer relationalen Datenbank-Managementsystem (RDBMS) importieren, z. B. SQL Server, MySQL und Oracle Hadoop verteiltes Dateisystem (bietet) Transformieren der Daten in Hadoop MapReduce oder Struktur und exportieren Sie die Daten in einem RDBMS. In diesem Lernprogramm verwenden Sie eine SQL Server-Datenbank für relationale Datenbank.

Sqoop-Versionen, die auf HDInsight-Cluster unterstützt werden, finden Sie unter [neuen Cluster Versionen von HDInsight bereitgestellten?] [hdinsight-versions].

##<a name="understand-the-scenario"></a>Dieses Szenario verstehen

HDInsight-Cluster enthält einige Beispieldaten. Sie verwenden die folgenden zwei Beispiele:

- Eine Protokolldatei log4j, in */example/data/sample.log*. Folgende Protokolle werden aus der Datei extrahiert:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- Eine Struktur-Tabelle namens *Hivesampletable*, die am */hive/warehouse/hivesampletable*Datei verweist. Die Tabelle enthält Daten Mobilgerät. 

  	| Feld | Datentyp |
  	| ----- | --------- |
  	| ClientID | Zeichenfolge |
  	| querytime | Zeichenfolge |
  	| Markt | Zeichenfolge |
  	| deviceplatform | Zeichenfolge |
  	| devicemake | Zeichenfolge |
  	| devicemodel | Zeichenfolge |
  	| Zustand | Zeichenfolge |
  	| Land | Zeichenfolge |
  	| querydwelltime | Doppelte |
  	| SessionID | bigint |
  	| sessionpagevieworder | bigint |

Sie *sample.log* und *Hivesampletable* SQL Azure-Datenbank oder SQL Server zuerst exportieren und importieren Sie die Tabelle mit den mobilen Gerätedaten zu HDInsight mit dem folgenden Pfad:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Cluster und SQL-Datenbank erstellen

In diesem Abschnitt veranschaulicht das Erstellen eines Clusters und SQL Datenbankschemas für Azure-Portal mit einer Vorlage Azure Ressourcenmanager Lernprogramm ausführen.  Wenn Sie Azure PowerShell verwenden möchten, finden Sie in [Anhang A](#appendix-a---a-powershell-sample).

1. Klicken Sie zum Öffnen einer Ressource-Manager Vorlage im Azure-Portal auf folgende.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Ressourcen-Manager-Vorlage befindet sich in einem öffentlichen Blob-Container *https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*. 
    
    Die Ressourcen-Manager-Vorlage Ruft ein Bacpac Paket um die Tabellenschemas für SQL-Datenbank bereitzustellen.  Das Bacpac-Paket befindet sich auch in einem öffentlichen Blob-Container https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Privaten Container für Bacpac-Dateien verwenden, verwenden Sie die folgenden Werte in der Vorlage:
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. Blatt Parameter geben Sie Folgendes ein:

    - **ClusterName**: Geben Sie einen Namen für den Cluster Hadoop, die Sie erstellen.
    - **Cluster-Benutzername und Kennwort**: der Standard-Anmeldename ist Admin.
    - **SSH-Benutzernamen und ein Kennwort**.
    - **SQL-Datenbank-Server-Benutzername und Kennwort**.

    Folgende Werte sind im Variablenabschnitt:
    
  	|Name des Standardkontos Speicher|<CluterName>Speichern|
  	|----------------------------|-----------------|
  	|Azure SQL Servername|<ClusterName>dbserver|
  	|Azure SQL-Datenbankname|<ClusterName>DB|
    
    Bitte notieren Sie diese Werte.  Sie benötigen sie später im Lernprogramm.
    
3. Klicken Sie auf **OK** , um die Parameter zu speichern.

4. **benutzerdefinierte Bereitstellung** Blade auf **Ressourcengruppe** Dropdownfeld, und klicken Sie auf **neu** , um eine neue Ressourcengruppe erstellen. Die Ressourcengruppe ist ein Container, der Cluster abhängige Speicherkonto und andere verknüpfte Ressource gruppiert.

5. auf **rechtlich**und klicken Sie dann auf **Erstellen**.

6. Klicken Sie auf **Erstellen**. Sie sehen eine neue Tile Titel Submitting Bereitstellung Bereitstellung. Dauert es etwa 20 Minuten Cluster und SQL-Datenbank erstellen.

Wenn Sie SQL Azure-Datenbank oder Microsoft SQL Server verwenden

- **SQL Azure-Datenbank**: Konfigurieren Sie eine Firewall-Regel SQL Azure Database Server Zugriff auf der Arbeitsstation. Eine Anleitung zum Erstellen einer SQL Azure-Datenbank und Konfigurieren der Firewall finden Sie unter [Erste Schritte mit SQL Azure-Datenbank][sqldatabase-get-started]. 

    > [AZURE.NOTE] Standardmäßig ermöglicht eine SQL Azure-Datenbank Verbindungen von Azure Services wie Azure HDInsight. Wenn diese firewalleinstellung deaktiviert ist, müssen Sie aktiviert von Azure-Portal. Anweisung zum Azure SQL-Datenbank erstellen und Konfigurieren von Firewall-Regeln finden Sie unter [Erstellen und Konfigurieren der SQL-Datenbank][sqldatabase-create-configue].

- **SQL Server**: ist HDInsight Cluster im gleichen virtuellen Netzwerk in Azure als SQL Server, können Sie die Schritte in diesem Artikel importieren und Exportieren von Daten in einer SQL Server-Datenbank.

    > [AZURE.NOTE] HDInsight unterstützt nur standortbasierte virtuelle Netzwerke und es funktioniert derzeit nicht mit virtuellen Netzwerken Gruppe basiert.

    * Erstellen und Konfigurieren eines virtuellen Netzwerks finden Sie unter [Erstellen eines virtuellen Netzwerks mit Azure-Portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

        * Bei Verwendung von SQL Server im Datencenter müssen Sie das virtuelle Netzwerk *zwischen Standorten* oder *Punkt-zu-Standort*konfigurieren.

            > [AZURE.NOTE] Für virtuelle **Punkt-zu-Standort** -Netzwerke muss SQL Server den VPN-Client Configuration Application ausgeführt werden aus dem **Dashboard** der Azure virtuelle Netzwerkkonfiguration.

        * Bei Verwendung von SQL Server auf einem virtuellen Computer Azure kann virtuelle Netzwerkkonfiguration virtuellen Computers mit SQL Server ist Mitglied im gleichen virtuellen Netzwerk als HDInsight verwendet.

    * HDInsight-Cluster virtuellen Netzwerk finden Sie [in HDInsight mit benutzerdefinierten Optionen erstellen Hadoop Cluster](hdinsight-provision-clusters.md)

    > [AZURE.NOTE] SQL Server muss auch Authentifizierung zulassen. Die Schritte in diesem Artikel müssen Sie eine SQL Server-Anmeldung verwenden.


## <a name="run-sqoop-jobs"></a>Sqoop abgeschlossen

HDInsight kann mithilfe verschiedener Methoden Sqoop Aufträge ausführen. Verwenden Sie in der folgenden Tabelle entscheiden, welche Methode Sie, und folgen Sie dem Link eine exemplarische Vorgehensweise.

| **Verwenden Sie diese Option** , wenn Sie...                                   | ... eine **interaktive** Shell | ... **Batch** -Verarbeitung | ... mit diesem **Cluster-Betriebssystem** | ... aus dem **Client-Betriebssystem** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [.NET SDK für Hadoop](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows (vorläufig)                        |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows                                  |

##<a name="limitations"></a>Grenzen

* Massenexport - mit Linux-basierte HDInsight, Sqoop-Connector verwendet, um Daten in Azure SQL-Datenbank oder Microsoft SQL Server exportieren unterstützt derzeit keine Bulk INSERT.

* Batchverarbeitung von-mit Linux-basierte HDInsight bei Verwendung der `-batch` fügt beim wechseln, wird Sqoop mehrere fügt anstelle von Batchvorgängen einfügen.

##<a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie gelernt, wie Sqoop verwenden. Weitere Informationen finden Sie unter:

- [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)
- [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)
- [Verwenden Sie Oozie mit HDInsight][hdinsight-use-oozie]: mit Sqoop-Aktion in einem Oozie-Workflow.
- [Datenanalyse Flug Verzögerung mit HDInsight][hdinsight-analyze-flight-data]: Flug analysieren Struktur verwenden Daten verzögern und Sqoop können um Daten mit einer Azure SQL-Datenbank zu exportieren.
- [Upload von Daten auf HDInsight][hdinsight-upload-data]: andere Methoden zum Hochladen von Daten zum HDInsight-Azure BLOB-Speicher.


## <a name="appendix-a---a-powershell-sample"></a>Anhang A - PowerShell-Beispiel

Das PowerShell-Beispiel führt die folgenden Schritte aus:

1. Verbinden Sie mit Azure.
2. Erstellen Sie eine Azure-Ressourcengruppe. Weitere Informationen finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md)
3. Erstellen einer Azure SQL-Datenbankserver, einer Azure SQL-Datenbank und zwei Tabellen. 

    Verwenden von SQL Server verwenden Sie die folgenden Aussagen Tabellen erstellen:
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

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
         [sessionpagevieworder][bigint])

    Die einfachste Möglichkeit, die Datenbank und Tabellen ist mit Visual Studio. Der Datenbankserver und die Datenbank können das Azure-Portal untersucht werden.

4. Erstellen Sie einen HDInsight-Cluster.

    Untersuchen Sie den Cluster können Sie Azure-Portal oder Azure PowerShell.

5. Die Quelldatei vorverarbeitet.

    In diesem Lernprogramm wird eine Protokolldatei log4j (eine durch Trennzeichen getrennte Datei) und einer Tabelle Struktur mit einer Azure SQL-Datenbank exportieren. Durch Trennzeichen getrennte Datei heißt */example/data/sample.log*. Zuvor in diesem Lernprogramm haben Sie Beispiele der log4j Protokolle gesehen. In der Protokolldatei gibt einige leeren Zeilen und einige Zeilen ähnlich:
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    Dies ist für andere Beispiele, in denen diese Daten, aber wir müssen diese Ausnahmen entfernen, bevor wir in SQL Azure-Datenbank oder SQL Server importieren können. Sqoop Export fehl, wird eine leere Zeichenfolge oder eine Linie mit einer kleineren Anzahl von Elementen als die Anzahl der Felder in der Tabelle SQL Azure definiert. Die log4jlogs-Tabelle enthält 7 Zeichenfolgentyp Felder.

    Diese Prozedur erstellt eine neue Datei im Cluster: tutorials/usesqoop/data/sample.log. Untersuchen Sie die geänderte Datei können Sie Azure-Portal, ein Azure Storage Explorer-Tool oder Azure PowerShell. [Erste Schritte mit HDInsight] [ hdinsight-get-started] hat ein Codebeispiel für die Verwendung von Azure PowerShell Downloaden einer Datei und den Inhalt der Datei angezeigt.

6. Exportieren einer Datendatei in SQL Azure-Datenbank.

    Die Quelldatei ist tutorials/usesqoop/data/sample.log. Die Tabelle, in die Daten exportiert, wird log4jlogs aufgerufen.
    
    > [AZURE.NOTE] Die Schritte in diesem Abschnitt sollte als Verbindungszeichenfolge eine SQL Azure-Datenbank oder SQL Server funktionieren. Diese Schritte wurden mit der folgenden Konfiguration getestet:
    >
    > * **Azure virtuelle Punkt-zu-Standort-Netzwerkkonfiguration**: ein virtuelles Netzwerk HDInsight Cluster mit einem SQL Server in einem privaten Datencenter verbunden. Weitere Informationen finden Sie unter [Konfigurieren einer Punkt-zu-Standort-VPN im Verwaltungsportal](../vpn-gateway/vpn-gateway-point-to-site-create.md) .
    > * **Azure HDInsight 3.1**: Weitere Informationen zum Erstellen eines Clusters in einem virtuellen Netzwerk [Erstellen Hadoop Cluster in HDInsight mit benutzerdefinierten Optionen](hdinsight-provision-clusters.md) .
    > * **SQL Server 2014**: Authentifizierung und mit dem VPN-Client Paket für das sichere Verbinden mit dem virtuellen Netzwerk konfiguriert.

7. Exportieren einer Tabelle Struktur in Azure SQL-Datenbank.

8. Importieren Sie die Mobiledata-Tabelle in HDInsight Cluster.

    Untersuchen Sie die geänderte Datei können Sie Azure-Portal, ein Azure Storage Explorer-Tool oder Azure PowerShell.  [Erste Schritte mit HDInsight] [ hdinsight-get-started] ist ein Beispiel zur Verwendung von Azure PowerShell zum Downloaden einer Datei und den Inhalt der Datei anzuzeigen.


### <a name="the-powershell-sample"></a>Das PowerShell-Beispiel

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
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
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
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
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
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
    catch{Login-AzureRmAccount}
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
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
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
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
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
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
