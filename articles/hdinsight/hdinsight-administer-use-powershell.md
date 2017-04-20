<properties
    pageTitle="In HDInsight mit PowerShell Hadoop Cluster verwalten | Microsoft Azure"
    description="Erfahren Sie, wie Verwaltungsaufgaben für Hadoop Cluster in Azure PowerShell mit HDInsight."
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
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Verwalten Sie in HDInsight Hadoop Cluster mithilfe von Azure PowerShell

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell ist ein leistungsfähiges Skriptingumgebung, mit denen Sie steuern und Automatisieren der Bereitstellung und Verwaltung der Arbeitslasten in Azure. In diesem Artikel lernen Sie in Azure HDInsight Hadoop Cluster mithilfe einer lokalen Azure PowerShell-Konsole durch Windows PowerShell verwalten. Liste von HDInsight-PowerShell-Cmdlets finden Sie in [HDInsight-Cmdlet Verweis][hdinsight-powershell-reference].



**Erforderliche Komponenten**

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

##<a name="install-azure-powershell"></a>Azure PowerShell installieren

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Bei der Installation von Azure PowerShell Version 0.9 X, deinstallieren Sie es bevor eine neuere Version installieren.

Die Version des installierten PowerShell überprüfen:

    Get-Module *azure*
    
Um die ältere Version zu deinstallieren, führen Sie Programme und Funktionen im Steuerungsbedienfeld. 


##<a name="create-clusters"></a>Erstellen von Clustern

Siehe [Erstellen von Linux-basierten Clustern in Azure PowerShell mit HDInsight](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

##<a name="list-clusters"></a>Liste Cluster
Verwenden Sie den folgenden Befehl alle Cluster in der aktuellen Anmeldung auflisten:

    Get-AzureRmHDInsightCluster

##<a name="show-cluster"></a>Cluster anzeigen

Befehl folgende Details eines bestimmten Clusters im aktuellen Abonnements anzuzeigen:

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

##<a name="delete-clusters"></a>Cluster löschen

Verwenden Sie den folgenden Befehl, um einen Cluster zu löschen:

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

Sie können einen Cluster auch durch Entfernen der Ressourcengruppe, die den Cluster enthält. Beachten Sie, dass alle Ressourcen in der Gruppe standardspeicherkonto einschließlich Löschvorgang.

    Remove-AzureRmResourceGroup -Name <Resource Group Name>
            
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

    ![HDInsight Sturm Skalierung auszugleichen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Hier ist ein Beispiel zum verwenden den CLI-Befehl Storm-Topologie neu:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Ändern die Clustergröße Hadoop mithilfe von Azure PowerShell, führen Sie den folgenden Befehl von einem Clientcomputer aus:

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    

##<a name="grantrevoke-access"></a>GRANT/Revoke Zugriff

HDInsight-Cluster haben die folgenden HTTP-Webdienste (alle diese Dienste müssen REST-Endpunkten):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Standardmäßig werden diese Dienste für den Zugriff erteilt. Sie können Sperren/den Zugriff erteilen. Widerrufen:

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

Gewähren:

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter the Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"
    
    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

>[AZURE.NOTE] Durch den Zugriff gewähren/aufheben, wird die Cluster-Benutzername und Kennwort zurückgesetzt.

Dies kann auch über das Portal erfolgen. [Verwalten HDInsight mithilfe des Azure-Portals]finden Sie unter[hdinsight-admin-portal].

##<a name="update-http-user-credentials"></a>HTTP-Anmeldeinformationen aktualisieren

Es ist wie [Grant/Revoke HTTP zugreifen](#grant/revoke-access). Wenn der Cluster den HTTP-Zugriff gewährt, müssen Sie zunächst widerrufen.  Und gewähren Sie den Zugriff mit neuen HTTP-Anmeldeinformationen.


##<a name="find-the-default-storage-account"></a>Suchen Sie das Standardkonto Speicher

Powershell-Skript veranschaulicht zu standardmäßigen speicherkontonamen Standard Konto Speicherschlüssel für einen Cluster.

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 

##<a name="find-the-resource-group"></a>Suchen der Ressourcengruppe

Im Ressourcen-Manager-Modus gehört jeder HDInsight Cluster Azure Ressourcengruppe.  Die Ressourcengruppe gefunden:

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


##<a name="submit-jobs"></a>Aufträge

**MapReduce Aufträge senden**

Finden Sie [in Windows-basierten HDInsight Hadoop MapReduce ausführen](hdinsight-run-samples.md).

**Struktur Aufträge senden** 

Finden Sie unter [Struktur führen Abfragen mithilfe von PowerShell](hdinsight-hadoop-use-hive-powershell.md).

**Schwein Aufträge senden**

[Führen Sie Schwein Aufträge mithilfe von PowerShell](hdinsight-hadoop-use-pig-powershell.md)anzeigen

**Sqoop Aufträge senden**

[Verwenden von Sqoop mit HDInsight](hdinsight-use-sqoop.md)anzeigen

**Oozie Aufträge senden**

[Mit Oozie Hadoop definieren und Ausführen eines Workflows in HDInsight](hdinsight-use-oozie.md)anzeigen

##<a name="upload-data-to-azure-blob-storage"></a>Upload von Daten in Azure BLOB-Speicher
[Daten zu HDInsight]finden Sie unter[hdinsight-upload-data].


## <a name="see-also"></a>Siehe auch
* [HDInsight-Cmdlet-Referenzdokumentation][hdinsight-powershell-reference]
* [Verwalten Sie HDInsight mithilfe der Azure-portal][hdinsight-admin-portal]
* [Verwalten Sie HDInsight über eine Befehlszeilenschnittstelle][hdinsight-admin-cli]
* [HDInsight Cluster erstellen][hdinsight-provision]
* [Upload von Daten auf HDInsight][hdinsight-upload-data]
* [Hadoop Aufträge programmgesteuert übermitteln][hdinsight-submit-jobs]
* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: powershell-install-configure.md

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
