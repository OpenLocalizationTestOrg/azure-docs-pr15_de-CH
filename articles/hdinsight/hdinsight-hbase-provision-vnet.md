<properties
    pageTitle="Erstellen HBase Cluster virtuellen Netzwerk | Microsoft Azure"
    description="Erste Schritte mit HBase in Azure HDInsight. Informationen Sie zum HDInsight HBase Cluster virtuellen Azure-Netzwerk erstellen."
    keywords=""
    services="hdinsight,virtual-network"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>HBase Cluster in Azure virtuelles Netzwerk erstellen 

Informationen zum Erstellen von Azure HDInsight HBase Cluster in einem [Azure Virtual Network][1].

Integration der virtuellen Netzwerk können HBase Cluster mit demselben virtuellen Netzwerk als Anwendung bereitgestellt, sodass Applikationen HBase direkt kommunizieren können. Die Vorteile:

- Direkte Verbindung der Website mit den Knoten des Clusters die Kommunikation über Remoteprozeduraufrufe HBase Java HBase (RPC)-APIs aufrufen.
- Verbesserte Leistung nicht mit Ihren Verkehr über mehrere Gateways und Lastenausgleich.
- Die Möglichkeit, vertrauliche Informationen auf sichere Weise verarbeiten, ohne einen öffentlichen Endpunkt.

###<a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Workstation mit Azure PowerShell**. [Installieren und Verwenden von Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)anzeigen 

## <a name="create-hbase-cluster-into-virtual-network"></a>HBase Cluster virtuellen Netzwerk erstellen

In diesem Abschnitt erstellen Sie einen HBase Linux-basierten Cluster mit abhängigen Azure Storage-Konto in ein Azure virtuelles Netzwerk mit einer [Azure-Ressourcen-Manager-Vorlage](../resource-group-template-deploy.md). Andere Methoden zur Erstellung und Verständnis die Standardeinstellungen finden Sie unter [Erstellen HDInsight-Cluster](hdinsight-hadoop-provision-linux-clusters.md). Weitere Informationen zum Verwenden einer Vorlage in HDInsight Hadoop Cluster erstellen finden Sie unter [Erstellen Hadoop Cluster in Azure Ressourcenmanager Vorlagen HDInsight](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Einige Eigenschaften wurden in der Vorlage fest codiert. Zum Beispiel:
>
> * __Ort__: East USA 2
> * __Cluster Version: 3.4
> * __Arbeitskraft Knoten Clusteranzahl__: 4
> * __Standard Konto__: &lt;Clustername > Speichern
> * __Name des virtuellen Netzwerks__: &lt;Clustername >-Vnet
> * __Virtuelle Netzwerk-Adressbereich__: 10.0.0.0/16
> * __Subnet-Name__: Standard
> * __Subnetzadressbereichs__: 10.0.0.0/24
>
> &lt;Cluster Name > mit dem Clusternamen angeben der Vorlage ersetzt.

1. Klicken Sie zum Öffnen der Vorlage im Azure-Portal auf folgende. Die Vorlage befindet sich in einem öffentlichen Blob-Container. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. **Benutzerdefinierte Bereitstellung** Blatt geben Sie Folgendes ein:

    - **Abonnement**: Wählen Sie zum Erstellen von HDInsight-Cluster, abhängige Speicherkonto und Azure virtual Network Azure-Abonnement.
    - **Gruppe**: Wählen Sie **neu erstellen**, und geben Sie einen neuen Ressource an.
    - **Ort**: Wählen Sie einen Speicherort für die Ressourcengruppe.
    - **ClusterName**: Geben Sie einen Namen für den Cluster Hadoop, die Sie erstellen.
    - **Cluster-Benutzername und Kennwort**: der Standard-Anmeldename ist **Admin**.
    - **SSH-Benutzername und Kennwort**: der Standard-Benutzername ist **Sshuser**.  Sie können sie umbenennen. 
    - **Ich stimme den allgemeinen Geschäftsbedingungen der genannten**: (Select)
    

3. Klicken Sie auf **kaufen**. Es dauert etwa 20 Minuten Erstellen eines Clusters. Nach Erstellung des Clusters können Sie Cluster Blade im Portal öffnen klicken.

Nach Abschluss des Lernprogramms sollten Sie Cluster löschen. Mit HDInsight Ihre Daten in Azure Storage gespeichert, einen Cluster sicher löschen können, wenn es nicht verwendet wird. Sie sind auch für einen HDInsight-Cluster berechnet, auch wenn es nicht verwendet wird. Da die Gebühren für den Cluster mehr als die Kosten für Speicher sind, ist es wirtschaftlich Cluster löschen, wenn sie nicht verwendet werden. Hinweise Löschen eines Clusters finden Sie [in HDInsight mit Azure-Portal verwalten Hadoop Cluster](hdinsight-administer-use-management-portal.md#delete-clusters).

Zum Arbeiten mit der neuen HBase können Sie die Prozeduren in [Erste Schritte mit HBase Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Verbinden Sie mit HBase HBase Java RPC-APIs

1.  Erstellen einer Infrastruktur als Service (IaaS) virtueller Computer in demselben Azure virtuelle Netzwerk und Subnetz. Informationen zum Erstellen eines neuen IaaS virtuellen Computers finden Sie unter [Erstellen einer virtuellen Windows Server ausgeführt wird](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Wenn Schritte in diesem Dokument verwenden Sie für die Konfiguration der folgenden:

    - __Virtual Network__: &lt;Clustername >-Vnet
    - __Subnetzmaske__: Standard

    > [AZURE.IMPORTANT] Ersetzen Sie &lt;Clustername > mit dem Namen beim Erstellen des HDInsight-Clusters in den vorherigen Schritten.

    Mit diesen Werten wird der virtuellen Computer verwenden den gleichen virtuellen Netzwerk und Subnetz HDInsight Cluster konfigurieren. Dadurch können sie direkt miteinander kommunizieren.

2.  Wenn eine Java-Anwendung Remoteverbindung zum HBase, müssen Sie den vollqualifizierten Domänennamen (FQDN) verwenden. Dazu müssen Sie das verbindungsspezifische DNS-Suffix des Clusters HBase abrufen. Dazu können Sie eine der folgenden Methoden:

    * Verwenden Sie einen Webbrowser Ambari aufrufen:
    
        Suchen Sie https://&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / Hosts?minimal_response = True. Es wird eine JSON-Datei mit DNS-Suffixe.

    * Verwenden der Ambari-Website:

        1. Suchen Sie https://&lt;ClusterName >. azurehdinsight.net.
        2. Klicken Sie im oberen Menü auf **Hosts** .

    * Aufrollen REST-Aufrufe zu verwenden:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        Finden Sie im zurückgegebenen Daten JavaScript Object Notation (JSON) den Eintrag "Hostname". Diese enthält den FQDN für den Knoten im Cluster. Zum Beispiel:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        Der Teil der Domäne beginnt mit dem Clusternamen ist das DNS-Suffix. Beispielsweise mycluster.b1.cloudapp.net.

    * Azure PowerShell verwenden
    
        Verwenden Sie folgende Azure PowerShell-Skript, um Funktion **Get-ClusterDetail** erfassen verwendet das DNS-Suffix zurückgegeben werden kann:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Azure PowerShell-Skript ausgeführt haben, verwenden Sie den folgenden Befehl das DNS-Suffix mithilfe der **Get-ClusterDetail** -Funktion zurückgegeben. Geben Sie Ihre HDInsight HBase Clustername Administratornamen und Admin-Kennwort beim Verwenden dieses Befehls.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Das DNS-Suffix wird zurückgegeben. Beispielsweise **yourclustername.b4.internal.cloudapp.net**.

    * RDP verwenden
    
        Sie können auch Remote Desktop HBase herstellen (Sie werden mit dem Head-Knoten) und führen Sie **Ipconfig** aus dem DNS-Suffix zu. Informationen zum Aktivieren von Remote Desktop Protocol (RDP) und Herstellen einer Verbindung zum Cluster über RDP finden Sie [Verwalten Hadoop Cluster in Azure-Portal mit HDInsight][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Um sicherzustellen, dass die virtuellen Computer mit den HBase kommunizieren können, verwenden Sie den Befehl `ping headnode0.<dns suffix>` virtuellen Computer. Zum Beispiel ping headnode0.mycluster.b1.cloudapp.net.

Um diese Informationen in einer Java-Anwendung verwenden, können Sie die in [Maven verwenden Java-Programme erstellen, die HBase mit HDInsight (Hadoop) verwenden,](hdinsight-hbase-build-java-maven.md) eine Anwendung erstellt Schritte. Um die Verbindung mit eines Remoteservers HBase Anwendung, bearbeiten Sie **Hbase site.xml** Datei in diesem Beispiel verwenden Sie den FQDN für Zookeeper Zum Beispiel:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Weitere Informationen zur namensauflösung von in Azure finden Sie unter virtuelle Netzwerke, wie Sie eigene DNS-Server verwenden [Name Resolution (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie ein HBase-Cluster erstellen. Weitere Informationen finden Sie unter:

- [Erste Schritte mit HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Konfigurieren der Replikation HBase in HDInsight](hdinsight-hbase-geo-replication.md)
- [Hadoop Cluster in HDInsight erstellen](hdinsight-provision-clusters.md)
- [Erste Schritte mit Hadoop in HDInsight HBase](hdinsight-hbase-tutorial-get-started.md)
- [Analysieren von Twitter Stimmung mit HBase in HDInsight](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Virtuelles Netzwerk (Übersicht)][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Bereitstellung Details für den neuen Cluster HBase"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Skriptaktion mit einen Cluster HBase anpassen"

[azure-preview-portal]: https://portal.azure.com
