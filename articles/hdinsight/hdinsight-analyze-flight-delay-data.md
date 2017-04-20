<properties
    pageTitle="Flug Verspätung Datenanalyse Hadoop in HDInsight | Microsoft Azure"
    description="Dazu verwenden Sie ein Windows PowerShell-Skript einen HDInsight-Cluster zu erstellen, führen Sie die Struktur des, Sqoop Stapelverarbeitung und Cluster löschen."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analysieren Sie Verzögerung Daten mit Struktur in HDInsight

Struktur der ausgeführten Hadoop MapReduce Aufträge durch eine SQL-ähnliche Skriptsprache ermöglicht * [HiveQL][hadoop-hiveql]*, die zusammenfassen, Abfragen und Analysieren von großen Datenmengen angewendet werden können.

> [AZURE.NOTE] Die Schritte in diesem Dokument ist einen Windows-basierte HDInsight-Cluster erforderlich. Mit einem Linux-basierten Cluster Arbeitsschritte finden Sie unter [Analysieren Flug Verzögerung mit Struktur in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Einer der Hauptvorteile von Azure HDInsight ist die Trennung der Speicherung von Daten und berechnen. HDInsight verwendet Azure BLOB-Speicher zum Speichern von Daten. Eine normale Arbeit umfasst drei Teile:

1. **Speichern Sie Daten in Azure BLOB-Speicher.**  Beispielsweise Wetter Daten, Daten, Web-Logs und bei Verzögerung Daten in Azure BLOB-Speicher gespeichert.
2. **Aufträgen.** Wenn die Daten verarbeitet werden, führen Sie ein Windows PowerShell-Skript oder eine Anwendung einen HDInsight-Cluster erstellen, Aufträgen und Cluster löschen. Die Einzelvorgänge speichern Ausgabedaten in Azure BLOB-Speicher. Die Ausgabedaten bleibt auch nach dem Löschen des Clusters. So bezahlen Sie nur was Sie aufgenommen haben.
3. **Abrufen der Ausgabe von Azure BLOB-Speicher**, oder die Daten in diesem Lernprogramm mit einer Azure SQL-Datenbank exportieren.

Das folgende Diagramm veranschaulicht das Szenario und die Struktur dieses Lernprogramms:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Hinweis**: die Zahlen im Diagramm entsprechen den Titeln der Abschnitte. **M** steht für den Hauptprozess. **A** steht für den Inhalt in der Anlage.

Hauptteil des Lernprogramms wird veranschaulicht, wie ein Windows PowerShell-Skript für folgende Aufgaben verwenden:

- Erstellen Sie einen HDInsight-Cluster.
- Stapelverarbeitung eine Struktur im Cluster Durchschnittliche Verzögerung Flughäfen berechnen. Verzögerung Flugdaten werden in einem Konto Azure BLOB-Speicher gespeichert.
- Führen Sie Sqoop des Auftragsausgabe Struktur mit einer Azure SQL-Datenbank zu exportieren.
- Löschen Sie HDInsight cluster

In den Anhängen finden Sie die Anleitung für Verzögerung Daten hochladen, eine Abfragezeichenfolge Struktur erstellen/hochladen und Vorbereiten der SQL Azure-Datenbank für das Projekt Sqoop.

> [AZURE.NOTE] Die Schritte in diesem Dokument sind spezifisch für Windows-basierte HDInsight-Cluster. Die mit Linux-basierten Cluster finden Sie unter [Analysieren Verzögerung Flugdaten mit Struktur in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Workstation mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**In diesem Lernprogramm verwendeten Dateien**

In diesem Lernprogramm wird die Leistung der Fluggesellschaft Flugdaten aus [Forschung und Innovative Technologie Verwaltung, Transport statistische oder RITA] [rita-website]. Eine Kopie der Daten wurde ein Azure BLOB-Speicher Container mit Öffentlichen Blob-Zugriffsberechtigung hochgeladen. Teil der PowerShell-Skript kopiert die Daten aus dem Blob für den öffentlichen Container zum Standardcontainer Blob des Clusters. Das HiveQL-Skript wird auch auf den gleichen Container Blob kopiert. Wenn Sie Get/eigene Speicherkonto Daten hochladen und zum Erstellen und Hochladen von Skriptdatei HiveQL erfahren möchten, finden Sie in [Anhang A](#appendix-a) und [Anhang B](#appendix-b).

In der folgenden Tabelle sind die in diesem Lernprogramm verwendeten Dateien aufgeführt:

<table border="1">
<tr><th>Dateien</th><th>Beschreibung</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Die HiveQL-Skriptdatei Mitarbeiterstelle Struktur verwendet. Dieses Skript wurde einem Konto Azure BLOB-Speicher mit öffentlichem Zugriff hochgeladen. <a href="#appendix-b">Anhang B</a> enthält Informationen zum Vorbereiten und Upload der Datei zu Ihrem Konto Azure BLOB-Speicher.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Eingabedaten für den Hive-Auftrag. Die Daten wurde ein Konto Azure BLOB-Speicher mit der Öffentlichkeit hochgeladen. <a href="#appendix-a">Anhang A</a> wurde zum Abrufen der Daten und Hochladen der Daten auf Ihr eigenes Konto Azure BLOB-Speicher.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Der Ausgabepfad für das Projekt Struktur. Der standardmäßige Container zum Speichern der Ausgabedaten.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Die Struktur Job Status Ordner für den Standardcontainer.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Erstellen Sie Cluster und Aufträgen Sie Hive-Sqoop

Hadoop MapReduce ist Stapelverarbeitung. Die kostengünstigste Möglichkeit zum Ausführen eines Auftrags Struktur ist ein Cluster für den Auftrag und löschen Sie den Auftrag aus, nachdem der Auftrag abgeschlossen ist. Das folgende Skript umfasst den gesamten Prozess. Informationen HDInsight-Cluster und Ausführen von Aufträgen Struktur finden Sie unter [Erstellen Hadoop Cluster in HDInsight] [ hdinsight-provision] und [Mit Struktur mit HDInsight][hdinsight-use-hive].

**Die Hive-Abfragen von Azure PowerShell ausgeführt**

1. Erstellen einer Azure SQL-Datenbank und die Tabelle für die Auftragsausgabe Sqoop mithilfe der Informationen in [Anhang C](#appendix-c).
3. Öffnen Sie Windows PowerShell ISE, und führen Sie das folgende Skript:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
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
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Verbinden mit der SQL-Datenbank und durchschnittliche Flug Verzögerungen Stadt in der AvgDelays-Tabelle:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Anhang A - Upload Verzögerung Flugdaten Azure Blob-Speicher
Hochladen der Datei und die HiveQL-Skriptdateien (siehe [Anhang B](#appendix-b)) erfordert einige Planung. Soll der Datendateien und die Datei HiveQL einen HDInsight-Cluster erstellen und Ausführen des Auftrags Struktur speichern. Sie haben zwei Optionen:

- **Verwenden Sie dasselbe Azure Storage-Konto, das als Standard-Dateisystem HDInsight-Cluster verwendet wird.** Da HDInsight Cluster Speicherschlüssel Konto Zugriff haben, müssen Sie weitere Änderungen vornehmen.
- **Verwenden Sie ein anderes Azure Storage-Konto von HDInsight Cluster Standard-Dateisystem.** Wenn dies der Fall ist, müssen Sie erstellen Teil des Windows PowerShell-Skript [Erstellen HDInsight-Cluster und zur Struktur-Sqoop Aufträge](#runjob) Speicherkonto als zusätzlicher Speicherkonto verknüpfen ändern. Informationen finden Sie in [Erstellen Hadoop Cluster in HDInsight][hdinsight-provision]. HDInsight-Cluster stellt dann die Tastenkombination für das Speicherkonto.

>[AZURE.NOTE] BLOB-Speicherpfad für die Datendatei ist schwer in der Skriptdatei HiveQL codiert. Sie müssen diese entsprechend aktualisieren.

**Flight-Daten herunterladen**

1. Wechseln Sie zu [Forschung und Technologie Administration Bureau of Transportation Statistics][rita-website].
2. Wählen Sie auf der Seite die folgenden Werte ein:

    <table border="1">
    <tr><th>Name</th><th>Wert</th></tr>
    <tr><td>Jahr filtern</td><td>2013 </td></tr>
    <tr><td>Zeitraum filtern</td><td>Januar</td></tr>
    <tr><td>Felder</td><td>*Jahr*, *FlightDate*, *UniqueCarrier*, *Netzbetreiber*, *FlightNum*, *OriginAirportID*, *Ursprung*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (Löschen aller Felder)</td></tr>
    </table>

3. Klicken Sie auf **Download**.
4. Extrahieren Sie die Datei in den Ordner **C:\Tutorials\FlightDelay\2013Data** . Jede Datei ist eine CSV-Datei und ca. 60 GB Größe.
5.  Benennen Sie die Datei auf den Namen des Monats, der Daten enthält. Beispielsweise würde die Datei mit den Daten Januar *January.csv*heißen.
6. Wiederholen Sie die Schritte 2 und 5 für jeden der 12 Monate 2013 herunterladen. Sie benötigen mindestens eine Datei des Lernprogramms.  

**Verzögerung Flugdaten Azure BLOB-Speicher hochladen**

1. Vorbereiten der Parameter:

    <table border="1">
    <tr><th>Name der Variablen</th><th>Notizen</th></tr>
    <tr><td>$storageAccountName</td><td>Das Azure Storage-Konto Daten hochgeladen werden soll.</td></tr>
    <tr><td>$blobContainerName</td><td>Der Blob-Container Daten hochgeladen werden soll.</td></tr>
    </table>
2. Öffnen Sie Azure PowerShell ISE.
3. Fügen Sie das folgende Skript in das Skriptfenster:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Drücken Sie **F5** , um das Skript auszuführen.

Möchten Sie eine andere Methode für das Hochladen von Dateien verwenden, stellen Sie sicher, dass der Dateipfad Lernprogramme, Flightdelay/Daten. Die Syntax für den Zugriff auf Dateien ist:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Die Lernprogramme/Flightdelay/Daten ist virtueller Ordner erstellten Dateien hochgeladen. Sicherzustellen Sie, dass 12-Dateien für jeden Monat.

>[AZURE.NOTE] Sie müssen die Hive-Abfrage Lesen vom neuen Speicherort aktualisieren.

> Sie müssen entweder die Berechtigung Container öffentlich sein oder HDInsight-Cluster an das Speicherkonto konfigurieren. Andernfalls können die Abfragezeichenfolge Struktur nicht auf Dateien zugreifen.

---
##<a id="appendix-b"></a>Anhang B - erstellen und Hochladen von Skripts HiveQL

Azure PowerShell verwenden, können Sie mehrere HiveQL-Anweisungen eine gleichzeitig oder package-HiveQL-Anweisung in einer Skriptdatei. In diesem Abschnitt wird veranschaulicht, wie ein HiveQL Skript und das Skript mithilfe von Azure PowerShell Azure Blob-Speicher hochladen. Struktur erfordert HiveQL Skripts in Azure BLOB-Speicher gespeichert werden.

Das HiveQL-Skript führt die folgenden Vorgänge:

1. **Delays_raw Tabelle löschen**, ist im Fall der Tabelle bereits vorhanden.
2. Der BLOB-Speicherort mit den Flug Verzögerung auf **Delays_raw externe Struktur Tabelle erstellen** . Diese Abfrage gibt an, dass Felder durch getrennt sind "," und "\n" Zeilen abgeschlossen werden. Dies stellt ein Problem dar, wenn Feldwerte Kommas enthalten, da Struktur unterscheiden kann, ob ein Komma ein Feldtrennzeichen und ein Teil des Feldwertes (die Feldwerte für Ursprungs gilt\_CITY\_NAME und Ziel\_Ort\_NAME). Dieses Problem umgehen, erstellt die Abfrage TEMP Spalten Daten enthalten, die nicht korrekt in Spalten aufgeteilt wird.  
3. **Verzögert die Tabelle löschen**, ist im Fall der Tabelle bereits vorhanden.
4. **Erstellen der verzögert**. Es empfiehlt sich vor der weiteren Verarbeitung die Daten bereinigen. Diese Abfrage erstellt eine neue Tabelle *Verzögerung*aus der Delays_raw-Tabelle. Hinweis Spalten TEMP (wie bereits erwähnt) nicht kopiert und die **Substring** -Funktion zum Entfernen von Anführungszeichen aus den Daten.
5. **Berechnen Sie die durchschnittliche Wetter Verzögerung und die Ergebnisse nach dem Ortsnamen.** Es gibt auch Ergebnisse Blob-Speicher. Beachten Sie, dass die Abfrage entfernen Apostrophe aus den Daten ausschließen Zeilen, in denen der Wert für **Weather_delay** null ist. Dies ist erforderlich, da Sqoop später in diesem Lernprogramm verwendeten Werte ordnungsgemäß standardmäßig nicht behandelt.

Eine vollständige Liste der HiveQL Befehle finden Sie unter [Struktur Datendefinitionssprache][hadoop-hiveql]. Jeder Befehl HiveQL muss mit einem Semikolon enden.

**Erstellen eine Skriptdatei HiveQL**

1. Vorbereiten der Parameter:

    <table border="1">
    <tr><th>Name der Variablen</th><th>Notizen</th></tr>
    <tr><td>$storageAccountName</td><td>Das Azure Storage-Konto das Skript HiveQL hochgeladen werden soll.</td></tr>
    <tr><td>$blobContainerName</td><td>Der Blob-Container das Skript HiveQL hochgeladen werden soll.</td></tr>
    </table>
2. Öffnen Sie Azure PowerShell ISE.

3. Kopieren Sie und fügen Sie das folgende Skript in den Skriptbereich:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Hier werden die Variablen im Skript:

    - **$hqlLocalFileName** - speichert das Skript die Datei HiveQL lokal vor dem Hochladen auf BLOB-Speicher. Dies ist der Dateiname. Der Standardwert ist <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** – ist dies HiveQL Namen der Skriptdatei Blob in Azure BLOB-Speicher verwendet. Der Standardwert ist tutorials/flightdelay/flightdelays.hql. Da die Datei direkt in Azure BLOB-Speicher geschrieben werden, ist es kein "/" am Anfang des Blob. Wenn Sie die Datei aus dem BLOB-Speicher zugreifen möchten, müssen Sie ein "/" am Anfang des Dateinamens hinzugefügt.
    - **$srcDataFolder** und **$dstDataFolder** - = "Lernprogramme/Flightdelay/Data" = "Lernprogramme/Flightdelay/Ausgabe"


---
##<a id="appendix-c"></a>Anhang C - Auftragsausgabe Sqoop eine SQL Azure-Datenbank vorbereiten
**Vorbereiten die SQL-Datenbank (das Skript Sqoop Zusammenführen)**

1. Vorbereiten der Parameter:

    <table border="1">
    <tr><th>Name der Variablen</th><th>Notizen</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Der Name des Datenbankservers SQL Azure. Geben Sie nichts zum Erstellen eines neuen Servers.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Der Anmeldename für den Azure SQL-Datenbankserver. Ist $sqlDatabaseServerName auf einem vorhandenen Server, werden Benutzername und Kennwort zur Authentifizierung mit dem Server. Andernfalls wird sie einen neuen Server zu erstellen.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Das Anmeldekennwort für den Azure SQL-Datenbankserver.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Dieser Wert wird nur beim Erstellen eines neuen Azure Datenbankservers verwendet.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>Die SQL-Datenbank verwendet, um AvgDelays für das Projekt Sqoop erstellen. Leer lassen, wird eine Datenbank namens HDISqoop erstellt. Der Tabellenname für die Auftragsausgabe Sqoop ist AvgDelays. </td></tr>
    </table>
2. Öffnen Sie Azure PowerShell ISE.
3. Kopieren Sie und fügen Sie das folgende Skript in den Skriptbereich:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Das Skript verwendet einen Dienst representational State Transfer (REST) http://bot.whatismyipaddress.com, um Ihre externe IP-Adresse abzurufen. Die IP-Adresse wird zum Erstellen einer Firewall für den SQL-Datenbankserver.  

    Hier sind einige Variablen im Skript:

    - **$ipAddressRestService** - der Standardwert ist http://bot.whatismyipaddress.com. Es ist eine öffentliche IP-Adresse REST-Dienst für die externe IP-Adresse. Andere Dienste können soll. Die externe IP-Adresse über den Dienst abgerufen dienen eine Firewallregel für den Azure SQL-Datenbankserver erstellen, damit Sie die Datenbank von Ihrer Arbeitsstation aus zugreifen können (mithilfe eines Windows PowerShell-Skripts).
    - **$fireWallRuleName** - Dies ist der Name der Firewallregel Azure SQL-Datenbankserver. Der Standardname lautet <u>FlightDelay</u>. Wenn Sie möchten, können Sie ihn umbenennen.
    - **$sqlDatabaseMaxSizeGB** - dieser Wert wird nur beim Erstellen eines neuen Azure SQL-Datenbankservers verwendet. Der Standardwert ist 10GB. 10GB reicht für dieses Lernprogramm.
    - **$sqlDatabaseName** - dieser Wert wird nur beim Erstellen einer neuen SQL Azure-Datenbank verwendet. Der Standardwert ist HDISqoop. Wenn Sie ihn umbenennen, müssen Sie das Sqoop Windows PowerShell-Skript entsprechend aktualisieren.

4. Drücken Sie **F5** , um das Skript auszuführen.
5. Überprüfen Sie die Ausgabe. Stellen Sie sicher, dass das Skript erfolgreich ausgeführt wurde.

##<a id="nextsteps"></a>Nächste Schritte
Sie verstehen jetzt uploaden eine Datei auf Azure BLOB-Speicher, wie eine Tabelle Struktur aufgefüllt mit den Daten aus dem Azure BLOB-Speicher Hive-Abfragen ausführen und wie mit Sqoop um Daten aus bietet mit einer Azure SQL-Datenbank zu exportieren. Weitere finden Sie in folgenden Artikeln:

* [Erste Schritte mit HDInsight][hdinsight-get-started]
* [Struktur mit HDInsight verwenden][hdinsight-use-hive]
* [Verwenden Sie Oozie mit HDInsight][hdinsight-use-oozie]
* [Verwenden Sie Sqoop mit HDInsight][hdinsight-use-sqoop]
* [Verwenden Sie Schwein mit HDInsight][hdinsight-use-pig]
* [Entwickeln von Java MapReduce Programme für HDInsight][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
