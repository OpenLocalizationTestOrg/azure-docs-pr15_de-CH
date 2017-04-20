<properties
   pageTitle="Windows-basierte Hadoop Cluster in HDInsight mit Azure PowerShell erstellen | Microsoft Azure"
    description="Informationen Sie zum Cluster mithilfe von Azure PowerShell für Azure HDInsight erstellen."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>Erstellen Sie Windows-basierte Hadoop Cluster in Azure PowerShell mit HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Erfahren Sie, wie mithilfe von Azure PowerShell HDInsight-Cluster erstellen. Azure PowerShell ist ein Modul, die zum Verwalten von Azure mithilfe von Windows PowerShell-Cmdlets bereitstellt. Andere Clustererstellung Tools und Features klicken Sie auf die Registerkarte wählen oben auf dieser Seite finden Sie oder [Cluster Methoden](hdinsight-provision-clusters.md#cluster-creation-methods).


##<a name="prerequisites"></a>Komponenten:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Vor diesem Artikel benötigen Sie Folgendes:

- Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Steuerelement erforderlich

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Erstellen von Clustern
Azure PowerShell ist ein leistungsfähiges Skriptingumgebung, mit denen Sie steuern und Automatisieren der Bereitstellung und Verwaltung der Arbeitslasten in Azure. Dieser Abschnitt beschreibt einen HDInsight-Cluster mithilfe von Azure PowerShell erstellen. Informationen zum Konfigurieren einer Arbeitsstation ausführen HDInsight Windows PowerShell-Cmdlets finden Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Weitere Informationen zum Verwenden von Azure PowerShell mit HDInsight finden Sie unter [Verwalten von HDInsight mithilfe von PowerShell](hdinsight-administer-use-powershell.md). Liste der HDInsight Windows PowerShell-Cmdlets finden Sie in [HDInsight Cmdlet Verweis](https://msdn.microsoft.com/library/azure/dn858087.aspx).


Die folgenden Verfahren müssen einen HDInsight-Cluster mithilfe von Azure PowerShell erstellen:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>Erstellen Sie Cluster mit ARM-Vorlage

Azure PowerShell können Sie einen HDInsight-Cluster erstellt eine ARM-Vorlage bereitgestellt.  Siehe [Vorlagen mithilfe von Azure PowerShell aufrufen](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell).

##<a name="customize-clusters"></a>Anpassen von Clustern

- Siehe [Anpassen HDInsight Cluster mit Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- [Anpassen von Windows-basierten HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell)anzeigen


##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie verschiedene HDInsight-Cluster erstellen. Weitere finden Sie in folgenden Artikeln:

* [Einstieg in Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) - Informationen von HDInsight-Cluster
* [Hadoop senden Aufträge programmgesteuert](hdinsight-submit-hadoop-jobs-programmatically.md) - erfahren Sie programmgesteuert Aufträge an HDInsight übermitteln
* [Hadoop verwalten Cluster HDInsight mithilfe von PowerShell](hdinsight-administer-use-powershell.md) - Lernen mit HDInsight mithilfe von Azure PowerShell
* [Azure HDInsight SDK-Dokumentation]  [ hdinsight-sdk-documentation] -SDK HDInsight ermitteln




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx
