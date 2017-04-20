<properties
    pageTitle="In HDInsight .NET SDK Hadoop Cluster verwalten | Microsoft Azure"
    description="Erfahren Sie, wie Verwaltungsaufgaben für Hadoop Cluster in HDInsight mit HDInsight .NET SDK."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    tags="azure-portal"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>Verwalten Sie in HDInsight Hadoop Cluster mit .NET SDK

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Informationen Sie zu HDInsight-Cluster mit [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).


**Erforderliche Komponenten**

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).


##<a name="connect-to-azure-hdinsight"></a>Verbinden mit Azure HDInsight

Sie benötigen die folgenden Nuget-Pakete:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

Das folgende Codebeispiel veranschaulicht die Azure herstellen, bevor HDInsight Cluster unter Azure-Abonnement verwalten können.

    using System;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    namespace HDInsightManagement
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // This is the GUID for the PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            static void Main(string[] args)
            {
                // Authenticate and get a token
                var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // insert code here

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
            }

            /// <summary>
            /// Authenticate to an Azure subscription and retrieve an authentication token
            /// </summary>
            /// <param name="TenantId">The AAD tenant ID</param>
            /// <param name="ClientId">The AAD client ID</param>
            /// <param name="SubscriptionId">The Azure subscription ID</param>
            /// <returns></returns>
            static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
                var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                    ClientId, 
                    new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                    PromptBehavior.Always, 
                    UserIdentifier.AnyUser);
                return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
            }
            /// <summary>
            /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
            /// </summary>
            /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
            /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
            /// <param name="authToken">An authentication token for your Azure subscription</param>
            static void EnableHDInsight(TokenCloudCredentials authToken)
            {
                // Create a client for the Resource manager and set the subscription ID
                var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
                resourceManagementClient.SubscriptionId = SubscriptionId;
                // Register the HDInsight provider
                var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
            }
        }
    }

Sie sehen eine Aufforderung beim Ausführen dieses Programms.  Wenn Sie gefragt werden, ob möchten, finden Sie unter [Erstellen nicht interaktive Authentifizierung .NET HDInsight Applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

##<a name="create-clusters"></a>Erstellen von Clustern

Siehe [Erstellen von Linux-basierten Clustern in HDInsight mit .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

##<a name="list-clusters"></a>Liste Cluster

Der folgende Codeausschnitt Clustern und einige Eigenschaften aufgeführt:

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

##<a name="delete-clusters"></a>Cluster löschen

Verwenden Sie den folgenden Codeausschnitt einen Cluster synchron oder asynchron löschen: 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");
            
##<a name="scale-clusters"></a>Clustern
Cluster Feature Skalierung können Sie die Anzahl der workerknoten eines Clusters in Azure HDInsight ist ohne erneuten Erstellen des Clusters verwendet.

>[AZURE.NOTE] Nur mit HDInsight Version 3.1.3 Cluster oder höher unterstützt. Wenn Sie die Version Ihres Clusters kennen, können Sie die Seite Eigenschaften überprüfen.  [Liste und zeigen Cluster](hdinsight-administer-use-portal-linux.md#list-and-show-clusters)anzeigen

Die Auswirkung der Ändern der Anzahl der Datenknoten für jeden Cluster HDInsight unterstützt:

- Hadoop

    Sie können problemlos die Anzahl der workerknoten in einem Cluster Hadoop erhöhen, die ohne Beeinträchtigung der ausstehenden oder ausgeführten Aufträge ausgeführt wird. Arbeitsplätze können auch gesendet werden, während der Vorgang ausgeführt wird. Fehler bei einer Skalierung werden ordnungsgemäß behandelt, sodass immer der Cluster funktionsfähig bleibt.

    Wenn Hadoop Cluster durch die Verringerung der Datenknoten verkleinert wird, werden einige Dienste im Cluster neu gestartet. Dadurch alle ausgeführten und ausstehenden Aufträge am Ende die Skalierungsoperation fehlschlägt. Sie können Einzelvorgänge jedoch erneut, nachdem der Vorgang abgeschlossen ist.

- HBase

    Sie können problemlos hinzufügen oder Entfernen von Knoten zum Cluster HBase während der Ausführung. Regionale Server werden innerhalb weniger Minuten Abschließen der Skalierung automatisch ausgeglichen. Sie können die regionalen Server jedoch auch manuell ausgleichen, indem Hauptknoten des Clusters anmelden und im Eingabeaufforderungsfenster die folgenden Befehle ausführen:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

- Sturm

    Sie können problemlos hinzufügen oder entfernen Datenknoten zum Cluster Sturm während er ausgeführt wird. Aber nach erfolgreichem Abschluss der Skalierung müssen die Topologie auszugleichen.

    Lastausgleich kann auf zwei Arten erfolgen:

    * Storm-Webbenutzeroberfläche
    * Befehlszeilenschnittstelle (CLI) tool

    Finden Sie die [Apache Storm-Dokumentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) .

    Storm-Webbenutzeroberfläche ist im HDInsight-Cluster verfügbar:

    ![Hdinsight Sturm Skalierung auszugleichen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Hier ist ein Beispiel zum verwenden den CLI-Befehl Storm-Topologie neu:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Der folgende Codeausschnitt zeigt, wie einen Cluster synchron oder asynchron Größe:

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    

##<a name="grantrevoke-access"></a>GRANT/Revoke Zugriff

HDInsight-Cluster haben die folgenden HTTP-Webdienste (alle diese Dienste müssen REST-Endpunkten):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Standardmäßig werden diese Dienste für den Zugriff erteilt. Sie können Sperren/den Zugriff erteilen. Widerrufen:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

Gewähren:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


>[AZURE.NOTE] Durch den Zugriff gewähren/aufheben, wird die Cluster-Benutzername und Kennwort zurückgesetzt.

Dies kann auch über das Portal erfolgen. [Verwalten HDInsight mithilfe der Azure-Portal]finden Sie unter[hdinsight-admin-portal].

##<a name="update-http-user-credentials"></a>HTTP-Anmeldeinformationen aktualisieren

Es ist wie [Grant/Revoke HTTP zugreifen](#grant/revoke-access). Wenn der Cluster den HTTP-Zugriff gewährt, müssen Sie zunächst widerrufen.  Und gewähren Sie den Zugriff mit neuen HTTP-Anmeldeinformationen.


##<a name="find-the-default-storage-account"></a>Suchen Sie das Standardkonto Speicher

Der folgende Codeausschnitt veranschaulicht, wie Name des Standardkontos Speicher und Standard Konto Speicherschlüssel für einen Cluster.

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


##<a name="submit-jobs"></a>Aufträge

**MapReduce Aufträge senden**

Finden Sie [in HDInsight Hadoop MapReduce ausführen](hdinsight-hadoop-run-samples-linux.md).

**Struktur Aufträge senden** 

Finden Sie unter [Struktur führen Sie Abfragen mit .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).

**Schwein Aufträge senden**

Finden Sie unter [Ausführen Schwein Aufträge mit .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).

**Sqoop Aufträge senden**

[Verwenden von Sqoop mit HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)anzeigen

**Oozie Aufträge senden**

[Mit Oozie Hadoop definieren und Ausführen eines Workflows in HDInsight](hdinsight-use-oozie-linux-mac.md)anzeigen

##<a name="upload-data-to-azure-blob-storage"></a>Upload von Daten in Azure BLOB-Speicher
[Daten zu HDInsight]finden Sie unter[hdinsight-upload-data].


## <a name="see-also"></a>Siehe auch
* [HDInsight .NET SDK-Referenzdokumentation](https://msdn.microsoft.com/library/mt271028.aspx)
* [Verwalten Sie HDInsight mithilfe der Azure-Portal][hdinsight-admin-portal]
* [Verwalten Sie HDInsight über eine Befehlszeilenschnittstelle][hdinsight-admin-cli]
* [HDInsight Cluster erstellen][hdinsight-provision]
* [Upload von Daten auf HDInsight][hdinsight-upload-data]
* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md


