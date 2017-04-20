<properties
   pageTitle="Verwenden Sie Hadoop Schwein mit PowerShell in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie Schwein Aufträge zu einem Cluster Hadoop mit Azure PowerShell HDInsight senden."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Aufträgen Sie Schwein mit PowerShell

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dieses Dokument enthält ein Beispiel für Azure PowerShell Schwein Aufträge in Hadoop auf HDInsight Cluster senden. Schweine können Sie MapReduce Aufträge mithilfe einer Sprache (Schwein Latein), die Modelle Datentransformationen Schreiben statt und Funktionen reduzieren.

> [AZURE.NOTE] Dieses Dokument bietet keine detaillierte Beschreibung der Aktionen in den Beispielen verwendeten Pig Latin-Anweisungen. Informationen Pig Latin verwendet in diesem Beispiel finden Sie unter [Verwenden Schwein mit Hadoop auf HDInsight](hdinsight-use-pig.md).

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel wird Folgendes erforderlich.

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Eine Workstation mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>Aufträgen Sie Schwein mit PowerShell

Azure PowerShell bietet *Cmdlets* , die Schweine Aufträge auf HDInsight Remote ausgeführt werden. Dies erfolgt intern durch REST Aufrufe [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (ehemals Templeton) auf dem HDInsight-Cluster ausgeführt.

Die folgenden Cmdlets werden verwendet, wenn auf einem Remotecluster HDInsight Schwein Jobs ausgeführt:

* **Login AzureRmAccount**: authentifiziert Azure PowerShell Azure-Abonnement

* **Neu AzureRmHDInsightPigJobDefinition**: erstellt eine neue *Auftragsdefinition* mit angegebenen Pig Latin-Anweisungen

* **Start AzureRmHDInsightJob**: Auftragsdefinition an HDInsight gesendet, den Auftrag starten und gibt *ein Auftragsobjekt verwendet werden kann, überprüfen Sie den Status des Auftrags*

* **Wait-AzureRmHDInsightJob**: Auftragsobjekt Überprüfen des Status des Auftrags verwendet. Es wartet, bis der Auftrag abgeschlossen oder die Wartezeit überschritten wurde.

* **Get-AzureRmHDInsightJobOutput**: zum Abrufen der Ausgabe des Auftrags

Die folgenden Schritte zeigen, wie diese Cmdlets verwenden, um eine auf dem HDInsight-Cluster auszuführen.

1. Mit einem Editor, den folgenden Code als **pigjob.ps1**speichern. Sie müssen mit dem Namen HDInsight Cluster **CLUSTERNAME** ersetzen.

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Öffnen Sie eine neue Windows PowerShell-Befehlszeile. Wechseln Sie zum Speicherort der Datei **pigjob.ps1** , und verwenden Sie den folgenden Befehl, um das Skript auszuführen:

        .\pigjob.ps1
        
    Sie werden zuerst aufgefordert, Ihre Azure-Abonnement anmelden. Dann werden Sie HTTPs-Admin-Kontoname und Kennwort für den Cluster HDInsight aufgefordert.

7. Bei Beendigung des Auftrags sollte Informationen wie die folgende zurückgegeben:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Problembehandlung

Wenn keine Informationen zurückgegeben, wenn der Auftrag abgeschlossen ist, können während der Verarbeitung Fehler haben. Um Fehlerinformationen für dieses Projekt anzuzeigen, fügen Sie den folgenden Befehl am Ende der Datei **pigjob.ps1** speichern Sie und dann erneut ausführen.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Erhalten die Informationen, die auf dem Server beim Ausführen der Aufgabe in STDERR geschrieben wurde, und es kann helfen, zu ermitteln, warum der Auftrag fehlgeschlagen ist.

##<a id="summary"></a>Zusammenfassung

Wie Sie sehen können, bietet Azure PowerShell leicht Schwein Aufträgen in einem Cluster HDInsight und Überwachen des Status Abrufen der Ausgabe.

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zum Schwein in HDInsight:

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)
