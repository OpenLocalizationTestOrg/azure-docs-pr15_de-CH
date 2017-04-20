<properties
   pageTitle="Verwenden Sie Hadoop Registrierungsstruktur PowerShell in HDInsight | Microsoft Azure"
   description="Mithilfe von PowerShell Struktur in Hadoop auf HDInsight Abfragen."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>Abfragen Sie Struktur mithilfe von PowerShell

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Dieses Dokument enthält ein Beispiel für Azure PowerShell Azure-Ressourcengruppe können im Hive-Abfragen in einem Hadoop auf HDInsight Cluster ausgeführt.

> [AZURE.NOTE] Dieses Dokument bietet keine ausführlich was HiveQL-Anweisungen, die in den Beispielen verwendet werden. Informationen über HiveQL, die in diesem Beispiel verwendet wird, finden Sie unter [Verwenden Hadoop auf HDInsight Struktur](hdinsight-use-hive.md).


**Erforderliche Komponenten**

Die Schritte in diesem Artikel wird Folgendes erforderlich.

- **Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (Windows- oder Linux-basierten)**
- **Eine Workstation mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Abfragen Sie Struktur mithilfe von Azure PowerShell

Azure PowerShell bietet *Cmdlets* , die Hive-Abfragen auf HDInsight Remote ausgeführt werden. Dies erfolgt intern durch REST Aufrufe [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (ehemals Templeton) auf dem HDInsight-Cluster ausgeführt.

Die folgenden Cmdlets sind verwendet Struktur Abfragen remote HDInsight-Cluster:

* **AzureRmAccount hinzufügen**: der Azure-Abonnement Azure PowerShell authentifiziert

* **Neu AzureRmHDInsightHiveJobDefinition**: erstellt eine neue *Auftragsdefinition* mit der angegebenen HiveQL-Anweisung

* **Start AzureRmHDInsightJob**: Auftragsdefinition an HDInsight gesendet, den Auftrag starten und gibt *ein Auftragsobjekt verwendet werden kann, überprüfen Sie den Status des Auftrags*

* **Wait-AzureRmHDInsightJob**: Auftragsobjekt Überprüfen des Status des Auftrags verwendet. Es wartet, bis der Auftrag abgeschlossen ist oder die Wartezeit überschritten.

* **Get-AzureRmHDInsightJobOutput**: zum Abrufen der Ausgabe des Auftrags

* **Invoke-AzureRmHDInsightHiveJob**: HiveQL-Anweisungen ausgeführt. Dies blockiert die Abfrage abgeschlossen ist, und gibt die Ergebnisse zurück

* **Mit AzureRmHDInsightCluster**: Legt aktuelle Cluster für die **Invoke-AzureRmHDInsightHiveJob-** Befehl

Die folgenden Schritte zeigen, wie diese Cmdlets zum Ausführen eines Auftrags im HDInsight-Cluster:

1. Mit einem Editor, den folgenden Code als **hivejob.ps1**speichern. Sie müssen mit dem Namen HDInsight Cluster **CLUSTERNAME** ersetzen.

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Öffnen Sie eine neue **Azure PowerShell** -Befehlszeile. Wechseln Sie zum Speicherort der Datei **hivejob.ps1** , und verwenden Sie den folgenden Befehl, um das Skript auszuführen:

        .\hivejob.ps1

    Beim Ausführen des Skripts werden Sie aufgefordert, die HTTPS-Admin-Anmeldeinformationen für den Cluster eingeben. Sie können auch Ihre Azure-Abonnement anmelden aufgefordert.
    
7. Bei Beendigung des Auftrags sollte Informationen wie die folgende zurückgegeben:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Wie bereits erwähnt, **Invoke-Struktur** dienen zum Ausführen einer Abfrage und die Antwort wartet. Verwenden Sie die folgenden Befehle und den Namen des Clusters **CLUSTERNAME** durch:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    Die Ausgabe sieht wie folgt aus:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Für längere HiveQL Abfragen können Sie Azure PowerShell **Hier Zeichenfolgen** Cmdlet oder HiveQL-Skriptdateien. Im folgenden Codeausschnitt veranschaulicht das **Invoke-Hive** -Cmdlet verwenden, um eine HiveQL führen. HiveQL-Skriptdatei muss Wasbs hochgeladen werden: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > Für Weitere Informationen **Hier Zeichenfolgen**finden Sie unter <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell-hier-Strings</a>.

##<a name="troubleshooting"></a>Problembehandlung

Wenn keine Informationen zurückgegeben, wenn der Auftrag abgeschlossen ist, können während der Verarbeitung Fehler haben. Um Fehlerinformationen für dieses Projekt anzuzeigen, fügen Sie Folgendes zum Ende der Datei **hivejob.ps1** speichern Sie und dann erneut ausführen.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Die Informationen, die auf dem Server beim Ausführen der Aufgabe in STDERR geschrieben wird zurückgegeben und es kann helfen, zu ermitteln, warum der Auftrag fehlgeschlagen ist.

##<a name="summary"></a>Zusammenfassung

Wie Sie sehen, bietet Azure PowerShell leicht Hive-Abfragen in einem Cluster HDInsight und Überwachen des Status Abrufen der Ausgabe.

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Struktur in HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)
