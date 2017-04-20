<properties
    pageTitle="Hadoop, HBase, Sturm oder Funken Cluster unter Linux in HDInsight mit Azure PowerShell erstellen | Microsoft Azure"
    description="Erfahren Sie, wie mithilfe von Azure PowerShell Hadoop, HBase, Sturm oder Funken Cluster unter Linux für HDInsight erstellen."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Erstellen Sie Linux-basierten Clustern in HDInsight mit Azure PowerShell

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell ist ein leistungsfähiges Skriptingumgebung, mit denen Sie steuern und Automatisieren der Bereitstellung und Verwaltung der Arbeitslasten in Microsoft Azure. Dieses Dokument enthält Informationen zum Erstellen eines Linux-basierten HDInsight Clusters mithilfe von Azure PowerShell. Sie enthält außerdem ein Beispielskript.

> [AZURE.NOTE] Azure PowerShell steht nur auf Windows-Clients. Bei Verwendung von Linux, Unix und Mac OS X-Client finden Sie unter [HDInsight Linux-basierten Cluster mit Azure CLI erstellen](hdinsight-hadoop-create-linux-clusters-azure-cli.md) Informationen über CLI Azure einen Cluster erstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten
Sie müssen vor dem beginnen die folgenden:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure PowerShell.
    Weitere Informationen zum Verwenden von Azure PowerShell mit HDInsight finden Sie unter [Verwalten von HDInsight mithilfe von PowerShell](hdinsight-administer-use-powershell.md). Die Liste der HDInsight Windows PowerShell-Cmdlets finden Sie in [HDInsight-Cmdlet Reference](https://msdn.microsoft.com/library/azure/dn858087.aspx).

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Steuerelement erforderlich

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Erstellen von Clustern

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Erstellen ein HDInsight-Clusters mithilfe von Azure PowerShell müssen Sie die folgenden Verfahren ausführen:

- Eine Azure-Ressourcengruppe erstellen
- Ein Azure Storage-Konto erstellen
- Einen Azure BLOB-Container erstellen
- Erstellen Sie einen HDInsight-cluster

Zwei wichtige Parameter, die festgelegt werden müssen, Erstellen von Linux-Clustern sind, die das Betriebssystem und Benutzerdaten SSH angeben:

- Stellen Sie sicher, dass Sie **Linux** **OSType -** Parameter angeben.
- Um SSH für Remotesitzungen in Clustern verwenden, können Sie das Benutzerkennwort SSH oder der öffentliche SSH-Schlüssel angeben. Wenn Sie das Benutzerkennwort SSH und der öffentliche SSH-Schlüssel angeben, wird der Schlüssel ignoriert. Wenn Sie SSH-Schlüssel für Remotesitzungen verwenden möchten, müssen Sie ein leeres SSH Kennwort einem angeben. Weitere Informationen über SSH mit HDInsight finden Sie in den folgenden Artikeln:

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

Das folgende Skript veranschaulicht das Erstellen eines neuen Clusters:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Die Werte für die **$clusterCredentials** angegebene dienen Hadoop Benutzerkonto für den Cluster zu erstellen. Verwenden Sie dieses Konto für die Verbindung mit dem Cluster.

Die Werte für die **$sshCredentials** angegebene dienen der Benutzer SSH für den Cluster erstellen. Verwenden Sie dieses Konto zu einer SSH-Remotesitzung auf dem Cluster Aufträge ausführen.

> [AZURE.IMPORTANT] In diesem Skript müssen Sie die Anzahl der workerknoten angeben, die im Cluster. Wenn Sie mehr als 32 workerknoten (oder Erstellung des Clusters durch Skalierung des Clusters nach der Erstellung) verwenden möchten, müssen Sie eine Größe Head-Knoten mit mindestens 8 Kernen 14 GB RAM angeben.
>
> Weitere Informationen zu Knoten Größen und Kosten finden Sie unter [HDInsight Preisgestaltung](https://azure.microsoft.com/pricing/details/hdinsight/).

Es dauert bis zu 20 Minuten zum Erstellen eines Clusters.

Im folgende Beispiel veranschaulicht, wie ein Konto zusätzlichen Speicher hinzuzufügen:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Anpassen von Clustern

- Siehe [Anpassen HDInsight Cluster mit Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- [Anpassen von Windows-basierten HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell)anzeigen

## <a name="delete-the-cluster"></a>Cluster löschen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte

Nun, dass Sie einen HDInsight-Cluster erstellt haben, verwenden Sie Folgendes Informationen zum Cluster arbeiten.

### <a name="hadoop-clusters"></a>Hadoop-Cluster

* [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)
* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)
* [Verwenden Sie MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-Cluster

* [Erste Schritte mit HBase auf HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Java-Anwendungsentwicklung für HBase auf HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-Cluster

* [Entwickeln von Java-Topologien für auf HDInsight](hdinsight-storm-develop-java-topology.md)
* [Python-Komponenten in auf HDInsight verwenden](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen und Überwachen von Topologien mit auf HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark-Cluster

* [Erstellen Sie eine eigenständige Anwendung Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Führen Sie Aufträge auf einem Spark-Cluster mit Livius Remote aus](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark BI: Datenanalyse interaktive BI-Tools Spark in HDInsight mit](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Computer: Spark in HDInsight Lebensmittel Ergebnisse vorherzusagen verwenden](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Verwendung Funken im HDInsight zum Erstellen von Echtzeit-streaming](hdinsight-apache-spark-eventhub-streaming.md)
