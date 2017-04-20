<properties
    pageTitle="Struktur Abfragen mit HDInsight .NET SDK | Microsoft Azure"
    description="Erfahren Sie, wie Aufträge in Azure HDInsight Hadoop mit HDInsight .NET SDK Hadoop senden."
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
   ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Abfragen Sie Struktur mit HDInsight .NET SDK

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Erfahren Sie mehr über das Hive-Abfragen mit HDInsight .NET SDK senden.

> [AZURE.NOTE] Die Schritte in diesem Artikel müssen von einem Windows-Client ausgeführt werden. Informationen zur Verwendung einer Linux, OS X und Unix-Client Struktur arbeiten verwenden Sie die Tabstoppauswahl auf Artikel angezeigt.

##<a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Hadoop Cluster in HDInsight**. [Erste Schritte mit Linux-basierten Hadoop in HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database)anzeigen
- **Visual Studio 2012/2013/2015**.

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Anfragen Sie Struktur mit HDInsight .NET SDK

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

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
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

* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [Hadoop Cluster in HDInsight erstellen][hdinsight-provision]
* [Verwalten Sie in HDInsight Hadoop Cluster mithilfe von Azure-Portal](hdinsight-administer-use-management-portal.md)
* [HDInsight .NET SDK-Referenz](https://msdn.microsoft.com/library/mt271028.aspx)
* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)
* [Verwenden Sie Sqoop mit HDInsight](hdinsight-use-sqoop-mac-linux.md)
* [Erstellen Sie nicht interaktive Authentifizierung .NET HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


