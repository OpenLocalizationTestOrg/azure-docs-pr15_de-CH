<properties
    pageTitle="MapReduce Aufträge mit HDInsight .NET SDK | Microsoft Azure"
    description="Informationen Sie zum Azure HDInsight Hadoop mit HDInsight .NET SDK MapReduce Aufträge senden."
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
   ms.date="10/28/2016"
    ms.author="jgao"/>

# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Mit HDInsight .NET SDK MapReduce-Aufträge ausführen

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Erfahren Sie, wie mit HDInsight .NET SDK MapReduce Aufträge senden. HDInsight Cluster mit einer JAR-Datei mit Beispiele MapReduce kommen. Die JAR-Datei ist */example/jars/hadoop-mapreduce-examples.jar*.  Eines der Beispiele ist *Wordcount*. Sie entwickeln ein C#-Konsolenanwendungsprojekt Wordcount Auftrag senden.  Der Auftrag liest die */example/data/gutenberg/davinci.txt* und gibt die Ergebnisse an */example/data/davinciwordcount*.  Wenn Sie die Anwendung erneut ausführen möchten, müssen Sie den Ausgabeordner bereinigen.

> [AZURE.NOTE] Die Schritte in diesem Artikel müssen von einem Windows-Client ausgeführt werden. Informationen zur Verwendung einer Linux, OS X und Unix-Client Struktur arbeiten verwenden Sie die Tabstoppauswahl auf Artikel angezeigt.

##<a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Hadoop Cluster in HDInsight**. [Erste Schritte mit Linux-basierten Hadoop in HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database)anzeigen
- **Visual Studio 2012/2013/2015**.

##<a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>MapReduce Aufträge mit HDInsight .NET SDK

HDInsight .NET SDK stellt .NET Clientbibliotheken HDInsight Cluster aus .NET arbeiten erleichtert. 

**Aufträge senden**

1. Erstellen Sie eine C# in Visual Studio.
2. Über die Konsole Nuget Paket-Manager Befehl.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Verwenden Sie den folgenden Code:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        //Content = "wordcount  ",
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Drücken Sie **F5** , um die Anwendung auszuführen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie verschiedene HDInsight-Cluster erstellen. Weitere finden Sie in folgenden Artikeln:

- Erstellen eines Clusters und Übermitteln einer "Struktur" finden Sie in [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
- HDInsight-Cluster erstellen, finden Sie unter [Hadoop erstellen Linux-basierten Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
- HDInsight-Cluster verwalten, finden Sie unter [Verwalten Hadoop Cluster in HDInsight](hdinsight-administer-use-management-portal.md).
- Lernen die HDInsight .NET SDK, finden Sie unter [HDInsight .NET SDK-Referenz](https://msdn.microsoft.com/library/mt271028.aspx).
- Für nicht-interaktive Azure authentifizieren, siehe [nicht-interaktive Authentifizierung .NET HDInsight erstellen](hdinsight-create-non-interactive-authentication-dotnet-applications.md).




