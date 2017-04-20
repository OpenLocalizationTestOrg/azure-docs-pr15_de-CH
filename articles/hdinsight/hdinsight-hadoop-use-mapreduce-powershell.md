<properties
   pageTitle="Hadoop MapReduce und PowerShell verwenden | Microsoft Azure"
   description="Dazu verwenden Sie PowerShell, MapReduce Aufträge Hadoop auf HDInsight auszuführen."
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
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Hadoop führen Sie MapReduce Aufträge über HDInsight mithilfe von PowerShell aus

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Dieses Dokument enthält ein Beispiel für Azure PowerShell MapReduce-Job in einer Hadoop auf HDInsight Cluster ausgeführt.

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

- **Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (Windows- oder Linux-basierten)**

- **Eine Workstation mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>Führen Sie einen MapReduce Druckauftrag Azure PowerShell

Azure PowerShell bietet *Cmdlets* , die MapReduce-Jobs auf HDInsight Remote ausgeführt werden. Dies erfolgt intern durch REST Aufrufe [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (ehemals Templeton) auf dem HDInsight-Cluster ausgeführt.

Die folgenden Cmdlets sind verwendet MapReduce Aufträge remote HDInsight-Cluster.

* **Login AzureRmAccount**: authentifiziert Azure PowerShell Azure-Abonnement

* **Neu AzureRmHDInsightMapReduceJobDefinition**: erstellt eine neue *Auftragsdefinition* mithilfe der angegebenen MapReduce

* **Start AzureRmHDInsightJob**: Auftragsdefinition an HDInsight gesendet, den Auftrag starten und gibt *ein Auftragsobjekt verwendet werden kann, überprüfen Sie den Status des Auftrags*

* **Wait-AzureRmHDInsightJob**: Auftragsobjekt Überprüfen des Status des Auftrags verwendet. Wartet, bis der Auftrag abgeschlossen ist oder die Wartezeit überschritten.

* **Get-AzureRmHDInsightJobOutput**: zum Abrufen der Ausgabe des Auftrags

Die folgenden Schritte zeigen, wie diese Cmdlets verwenden, um eine im HDInsight-Cluster auszuführen.

1. Mit einem Editor, den folgenden Code als **mapreducejob.ps1**speichern. Sie müssen mit dem Namen HDInsight Cluster **CLUSTERNAME** ersetzen.

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Öffnen Sie eine neue **Azure PowerShell** -Befehlszeile. Wechseln Sie zum Speicherort der Datei **mapreducejob.ps1** , und verwenden Sie den folgenden Befehl, um das Skript auszuführen:

        .\mapreducejob.ps1
    
    Beim Ausführen des Skripts können Sie Ihre Azure-Abonnement authentifizieren aufgefordert. Sie werden ebenfalls aufgefordert, den HTTPS-Admin-Kontonamen und das Kennwort für den HDInsight-Cluster bereitstellen.

3. Abschluss des Auftrags erhalten Sie eine Ausgabe ähnlich der folgenden:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Diese Ausgabe zeigt, dass der Auftrag erfolgreich abgeschlossen.

    > [AZURE.NOTE] Wenn der **ExitCode** ungleich 0 ist, finden Sie unter [Problembehandlung](#troubleshooting).

    In diesem Beispiel wird auch die heruntergeladenen Dateien in eine Datei **output.txt** im Verzeichnis speichern die Ausführung des Skripts aus.

###<a name="view-output"></a>Ausgabe anzeigen

Öffnen der Datei **output.txt** in einem Texteditor die Wörter und Zahlen den Auftrag erstellt.

> [AZURE.NOTE] Die Ausgabedateien MapReduce-Auftrags sind unveränderlich. Wenn Sie dieses Beispiel erneut ausführen, müssen Sie so ändern Sie den Namen der Ausgabedatei.

##<a id="troubleshooting"></a>Problembehandlung

Wenn keine Informationen zurückgegeben, wenn der Auftrag abgeschlossen ist, können während der Verarbeitung Fehler haben. Um Fehlerinformationen für dieses Projekt anzuzeigen, fügen Sie den folgenden Befehl am Ende der Datei **mapreducejob.ps1** speichern Sie und dann erneut ausführen.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Dies gibt Informationen, die auf dem Server beim Ausführen der Aufgabe in STDERR geschrieben wurde und es kann helfen, zu ermitteln, warum der Auftrag fehlgeschlagen ist.

##<a id="summary"></a>Zusammenfassung

Wie Sie sehen, bietet Azure PowerShell leicht MapReduce-Jobs auf einen HDInsight-Cluster ausgeführt und überwacht den Status Abrufen der Ausgabe.

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce Aufträge in HDInsight:

* [HDInsight Hadoop MapReduce verwenden](hdinsight-use-mapreduce.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)
