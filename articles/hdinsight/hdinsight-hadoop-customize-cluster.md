<properties
    pageTitle="HDInsight Cluster mit Skriptaktionen anpassen | Microsoft Azure"
    description="Weitere Informationen zum HDInsight Cluster mit Skriptaktion anpassen."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Passen Sie HDInsight Windows-basierten Cluster mit Skriptaktion an

**Skriptaktion** kann verwendet werden, während der Clustererstellung für zusätzliche Software auf einem Cluster installieren [benutzerdefinierter Skripts](hdinsight-hadoop-script-actions.md) aufrufen.

Die Informationen in diesem Artikel gilt für Windows-basierte HDInsight-Cluster. Linux-basierten Clustern finden Sie unter [Anpassen von Linux-basierten HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md). 

HDInsight-Cluster in verschiedene andere Arten, wie z. B. zusätzliche Azure-Speicherkonten angepasst werden können, die Hadoop Konfigurationsdateien (Core site.xml, Struktur site.xml usw.) oder Bibliotheken (Struktur, Oozie) in gängige Speicherorte im Cluster freigegebenen hinzufügen. Diese Anpassung erfolgt durch Azure PowerShell, Azure HDInsight .NET SDK und Azure-Portal. Weitere Informationen finden Sie unter [Erstellen Hadoop Cluster in HDInsight][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Skript für Aktion in der Clustererstellung

Skript für Aktion wird nur verwendet, während ein Cluster wird gerade erstellt. Das folgende Diagramm veranschaulicht, wenn Skriptaktion während des Erstellungsprozesses ausgeführt wird:

![HDInsight Cluster Anpassung und Phasen bei der Clustererstellung][img-hdi-cluster-states]

Wenn das Skript ausgeführt wird, gibt Cluster **ClusterCustomization** Phase. In dieser Phase wird das Skript unter dem Konto System Admin parallel auf den angegebenen Knoten im Cluster ausgeführt und bietet vollständige Administratorrechte auf den Knoten.

> [AZURE.NOTE] Da Sie Administratorrechte für die Clusterknoten Phase **ClusterCustomization** haben, können Sie das Skript, Operationen wie Dienste, einschließlich Hadoop-Dienste starten und beenden. Das Skript, so müssen Sie sicherstellen, dass die Ambari und andere Hadoop-bezogene Dienste ausgeführt werden, bevor das Skript beendet. Diese Dienste müssen den Zustand und Status des Clusters erfolgreich überprüfen, während sie erstellt wird. Wenn Sie eine Konfiguration des Clusters, die diese Dienste auswirkt ändern, müssen Sie die Hilfsfunktionen verwenden, die bereitgestellt werden. Informationen über Hilfsfunktionen finden Sie unter [entwickeln Skriptaktion Skripts für HDInsight][hdinsight-write-script].

Ausgabe und Fehlerprotokolle für das Skript werden im Speicher Standardkonto für den Cluster angegeben gespeichert. Die Protokolle werden in einer Tabelle mit dem Namen **u < \cluster-name-fragment >< \time-stamp > Setuplog**. Dies sind aggregate Protokolle vom Skript auf allen Knoten (Head-Knoten und Arbeitskraft Knoten) im Cluster ausführen.

Jeder Cluster kann mehrere Skriptaktionen akzeptieren, die in der Reihenfolge aufgerufen werden, in denen sie angegeben werden. Ein Skript kann auf dem Head-Knoten oder die Arbeitskraft Knoten ausgeführt werden.

HDInsight bietet mehrere Skripts die folgenden Komponenten auf HDInsight-Cluster installieren:

Name | Skript
----- | -----
**Spark installieren** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. [Installieren und verwenden Funken auf HDInsight-Cluster]finden Sie unter[hdinsight-install-spark].
**Installieren von R** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Siehe [Installieren und R mit HDInsight-Cluster][hdinsight-install-r].
**Solr installieren** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. [Installieren und verwenden Solr auf HDInsight Cluster](hdinsight-hadoop-solr-install.md)anzeigen
- **Giraph installieren** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. [Installieren und verwenden Giraph auf HDInsight Cluster](hdinsight-hadoop-giraph-install.md)anzeigen
| **Pre-Load Hive Bibliotheken** | https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Siehe [Bibliotheken für HDInsight Cluster Struktur hinzufügen](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Aufrufen von Skripts mithilfe der Azure-Portal

**Von Azure-Portal**

1. Erstellen Sie einen Cluster als [Cluster erstellen Hadoop in HDInsight](hdinsight-provision-clusters.md#portal)beschrieben.
2. Klicken Sie unter optionale Konfiguration Blade **Skriptaktionen** auf **Skriptaktion hinzufügen** , um Angaben über die Skriptaktion wie folgt:

    ![Skriptaktion für einen Cluster anpassen verwenden] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Skriptaktion für einen Cluster anpassen verwenden")

    <table border='1'>
        <tr><th>Eigenschaft</th><th>Wert</th></tr>
        <tr><td>Name</td>
            <td>Geben Sie einen Namen für die Skriptaktion.</td></tr>
        <tr><td>Skript URI</td>
            <td>Geben Sie den URI für das Skript aufgerufen wird, um den Cluster anpassen. s</td></tr>
        <tr><td>Head-Arbeitskraft</td>
            <td>Geben Sie Knoten (**Head** oder **Arbeitskraft an**) auf die Ausführung des Skripts Anpassung </b>.
        <tr><td>Parameter</td>
            <td>Geben Sie die Parameter ggf. vom Skript an.</td></tr>
    </table>

    Drücken Sie die EINGABETASTE mehrere Skriptaktion zum Installieren von mehreren Komponenten im Cluster hinzufügen.

3. Klicken Sie auf die Aktionskonfiguration speichern und mit der Clustererstellung Fortfahren **Wählen** .

## <a name="call-scripts-using-azure-powershell"></a>Rufen Sie mithilfe von Azure PowerShell Skripts

Dieses PowerShell veranschaulicht Spark auf Windows basierend HDInsight-Cluster installieren.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Um andere Software zu installieren, müssen Sie die Skriptdatei im Skript ersetzen:


Wenn Sie aufgefordert werden, geben Sie die Anmeldeinformationen für den Cluster. Es dauert einige Minuten, bevor der Cluster erstellt wird.

## <a name="call-scripts-using-net-sdk"></a>Anruf-Skripte mit .NET SDK 

Im folgende Beispiel veranschaulicht, wie Spark auf Windows basierend HDInsight-Cluster installieren. Um andere Software zu installieren, müssen Sie die Skriptdatei im Code ersetzen.

**Erstellt einen HDInsight-Cluster mit Funken** 

1. Erstellen Sie eine C# in Visual Studio.
2. Über die Konsole Nuget Paket-Manager Befehl.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Verwenden Sie die folgende Anweisung in der Datei Program.cs mit:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Fügen Sie den Code in der Klasse durch Folgendes:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
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


4. Drücken Sie **F5** , um die Anwendung auszuführen.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Unterstützung für Open-Source-Software, die auf HDInsight-Cluster
Microsoft Azure HDInsight-Dienst ist eine flexible Plattform, die mit Ökosystem Open Source-Technologien Hadoop gebildet big Data Applications in der Cloud erstellen kann. Microsoft Azure bietet eine allgemeine Unterstützung für Open-Source-Technologien wie **Support** -Bereich der <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure-Support-FAQ-Website</a>. HDInsight-Dienst bietet zusätzliche Unterstützung für die Komponenten wie unten beschrieben.

Es gibt zwei Arten der Open Source-Komponenten, die in der HDInsight-Dienst:

- **Integrierte Komponenten** – diese Komponenten sind vorinstalliert auf HDInsight-Cluster und Kernfunktionen des Clusters bereitstellen. Beispielsweise gehören GARN ResourceManager Abfragesprache Struktur (HiveQL) und Mahout-Bibliothek zu dieser Kategorie. Eine vollständige Liste der Komponenten von Serverclustern steht in [neuen Hadoop Cluster Versionen von HDInsight bereitgestellten?](hdinsight-component-versioning.md) </a>.
- **Benutzerdefinierte Komponenten** – Sie als Benutzer des Clusters können installieren oder jede Komponente in der Gemeinschaft oder von Ihnen erstellte in Workload verwenden.

Integrierte Komponenten werden unterstützt, und Microsoft Support hilft isolieren und Lösen von Problemen im Zusammenhang mit diesen Komponenten.

> [AZURE.WARNING] Komponenten mit HDInsight-Cluster vollständig unterstützt und Microsoft Support hilft isolieren und Lösen von Problemen im Zusammenhang mit diesen Komponenten.
>
> Benutzerdefinierte Komponenten erhalten angemessene Unterstützung helfen, das Problem zu beheben. Dadurch kann die Fehlerbehebung oder Aufforderung zu Kanälen für open-Source-Technologien, fundiertes Fachwissen für diese Technologie fand. Beispielsweise sind viele Community-Sites, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-Projekte verfügen Projektsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/), [Funken](http://spark.apache.org/).

HDInsight Service bietet verschiedene benutzerdefinierte Komponenten. Unabhängig davon, wie eine Komponente verwendet oder im Cluster installiert gilt die gleiche Unterstützung. Es folgt eine Liste der häufigsten angepassten Komponenten HDInsight-Cluster eingesetzt werden können:

1. Bewerbung - Hadoop oder andere Arten von Einzelvorgängen, die ausgeführt oder benutzerdefinierte Komponenten kann dem Cluster gesendet werden.
2. Cluster-Anpassung – während der Clustererstellung können Sie zusätzliche Einstellungen und angepassten Komponenten, die auf den Clusterknoten installiert werden.
3. Beispiele – häufig verwendete benutzerdefinierte Komponenten und Microsoft bieten Beispiele für die Verwendung dieser Komponenten in Clustern HDInsight. Diese Beispiele werden ohne Unterstützung bereitgestellt.

## <a name="develop-script-action-scripts"></a>Entwickeln Skriptaktion Skripts

[Skriptaktion entwickeln]Skripte für HDInsight[hdinsight-write-script].


## <a name="see-also"></a>Siehe auch

- [Hadoop Cluster in HDInsight erstellen] [ hdinsight-provision-cluster] beschreibt einen HDInsight Cluster mit anderen benutzerdefinierten Optionen erstellen.
- [Entwickeln von Skriptaktion Skripts für HDInsight][hdinsight-write-script]
- [Installieren und Verwenden von Spark auf HDInsight-Cluster][hdinsight-install-spark]
- [Installieren und Verwenden von R auf HDInsight-Cluster][hdinsight-install-r]
- [Installieren und verwenden Solr auf HDInsight Cluster](hdinsight-hadoop-solr-install.md).
- [Installieren und verwenden Giraph auf HDInsight Cluster](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Phasen bei der Clustererstellung"
