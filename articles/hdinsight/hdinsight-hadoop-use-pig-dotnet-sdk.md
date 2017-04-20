<properties
   pageTitle="Verwenden Sie Hadoop Schwein mit .NET in HDInsight | Microsoft Azure"
   description="Informationen Sie zum .NET SDK Hadoop Hadoop auf HDInsight Schwein Aufträge senden."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Aufträgen Sie Schwein mit .NET SDK für Hadoop in HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dieses Dokument enthält ein Beispiel für das .NET SDK für Hadoop Schwein Aufträge in Hadoop auf HDInsight Cluster senden.

HDInsight .NET SDK stellt .NET Client-Bibliotheken, die mit HDInsight aus .NET arbeiten erleichtert. Schweine kann MapReduce-Vorgänge erstellen, indem Sie eine Reihe von Datentransformationen modellieren. Wie Sie eine einfache C#-Anwendung einen Auftrag Schwein einen HDInsight-Cluster senden erfahren.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel wird Folgendes erforderlich.

* Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (Windows oder Linux-basierten).
* Visual Studio 2012 oder 2013 oder 2015.

## <a name="create-the-application"></a>Erstellen der Anwendung

HDInsight .NET SDK stellt .NET Clientbibliotheken HDInsight Cluster aus .NET arbeiten erleichtert. 


1. Öffnen Sie Visual Studio 2012 oder 2013
2. Im Menü **Datei** wählen Sie **neu** und wählen Sie dann **Projekt**.
3. Neues Projekt wählen Sie oder geben Sie die folgenden Werte.

    <table>
    <tr>
    <th>Eigenschaft</th>
    <th>Wert</th>
    </tr>
    <tr>
    <th>Kategorie</th>
    <th>Vorlagen/Visual C# / Windows</th>
    </tr>
    <tr>
    <th>Vorlage</th>
    <th>Konsolenanwendungsprojekt</th>
    </tr>
    <tr>
    <th>Name</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Klicken Sie auf **OK** , um das Projekt zu erstellen.
5. Klicken Sie im Menü **Extras** wählen Sie **Library Paket-Manager** oder **Nuget Paket-Manager**und wählen Sie **Paket-Manager-Konsole**.
6. Führen Sie den folgenden Befehl in der Konsole die Packages .NET SDK installieren.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Doppelklicken Sie im Projektmappen-Explorer auf **Program.cs** , um ihn zu öffnen. Ersetzen Sie den vorhandenen Code durch den folgenden Code.

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Drücken Sie **F5** , um die Anwendung zu starten.
8. Drücken Sie die **EINGABETASTE** zum Beenden der Anwendungdes.

## <a name="summary"></a>Zusammenfassung

Wie Sie sehen können, ermöglicht .NET SDK Hadoop erstellen .NET Applications, die Aufträge Schwein einen HDInsight-Cluster senden und den Status überwachen.

## <a name="next-steps"></a>Nächste Schritte

Allgemeine Informationen zum Schwein in HDInsight.

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

Weitere Informationen können Sie Hadoop auf HDInsight arbeiten.

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Mit MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md) [Vorschau-Portal]: https://portal.azure.com/
