<properties
    pageTitle="Verwenden Sie Hadoop Sqoop in HDInsight | Microsoft Azure"
    description="Informationen Sie zum HDInsight .NET SDK Sqoop Import und export zwischen Hadoop-Cluster und einer Azure SQL-Datenbank verwenden."
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

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Aufträgen Sie Sqoop mit .NET SDK für Hadoop in HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie mit HDInsight .NET SDK Sqoop Aufträgen in HDInsight importieren und Exportieren von HDInsight-Cluster und SQL Azure oder SQL Server-Datenbank.

> [AZURE.NOTE] Die Schritte in diesem Artikel können mit einem Windows- oder Linux-basierte HDInsight-Cluster verwendet werden. folgendermaßen funktioniert jedoch nur von einem Windows-Client. Wählen Sie andere Methoden mit Tabstoppauswahl oben in diesem Artikel.

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Hadoop Cluster in HDInsight**. Siehe [Cluster erstellen und SQL-Datenbank](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Führen Sie Sqoop mit .NET SDK

HDInsight .NET SDK stellt .NET Clientbibliotheken HDInsight Cluster aus .NET arbeiten erleichtert. In diesem Abschnitt erstellen Sie eine C#-Konsolenanwendung der Hivesampletable in SQL-Datenbanktabelle exportieren in diesen Lernprogrammen erstellte.

**Senden ein Auftrags Sqoop**

1. Erstellen Sie eine C# in Visual Studio.
2. Der Visual Studio-Paket-Manager-Konsole führen Sie folgenden Befehl Nuget Paket importieren.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. Verwenden Sie den folgenden Code in der Datei Program.cs:

        using System.Collections.Generic;
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
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Drücken Sie **F5** , um das Programm auszuführen. 

##<a name="limitations"></a>Grenzen

* Massenexport - mit Linux-basierte HDInsight, Sqoop-Connector verwendet, um Daten in Azure SQL-Datenbank oder Microsoft SQL Server exportieren unterstützt derzeit keine Bulk INSERT.

* Batchverarbeitung von-mit Linux-basierte HDInsight bei Verwendung der `-batch` fügt beim wechseln, wird Sqoop mehrere fügt anstelle von Batchvorgängen einfügen.

##<a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie gelernt, wie Sqoop verwenden. Weitere Informationen finden Sie unter:

- [Oozie mit HDInsight verwenden](hdinsight-use-oozie.md): mit Sqoop-Aktion in einem Oozie-Workflow.
- [Analyze Verzögerung Flugdaten mit HDInsight](hdinsight-analyze-flight-delay-data.md): Flug analysieren Struktur verwenden Daten verzögern und Sqoop können um Daten mit einer Azure SQL-Datenbank zu exportieren.
- [Daten zu HDInsight](hdinsight-upload-data.md): andere Methoden zum Hochladen von Daten zum HDInsight-Azure BLOB-Speicher.


