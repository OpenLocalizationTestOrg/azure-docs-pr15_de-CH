<properties
    pageTitle="HDInsight virtuelle Netzwerk erweitern | Microsoft Azure"  
    description="Erfahren Sie, wie mit Azure Virtual Network HDInsight Verbindung mit anderen Cloudressourcen oder Ressourcen im Rechenzentrum"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="larryfr"/>


#<a name="extend-hdinsight-capabilities-by-using-azure-virtual-network"></a>Mithilfe von Azure Virtual Network erweitern Sie HDInsight Funktionen

Azure Virtual Network ermöglicht Hadoop Projektmappen integrieren von lokalen Ressourcen wie SQL Server, kombinieren mehrere HDInsight Cluster oder sichere private Netzwerke zwischen Ressourcen in der Cloud zu erweitern.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-and-cli.md)]


##<a id="whatis"></a>Was ist virtuelles Netzwerk Azure?

[Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) ermöglicht eine dauerhafte Netzwerk mit den Ressourcen für die Projektmappe erstellen. Ein virtuelles Netzwerk können Sie:

* Verbinden Sie Cloud-Ressourcen in einem privaten Netzwerk (nur Cloud) miteinander.

    ![Diagramm der Cloud nur Konfiguration](media/hdinsight-extend-hadoop-virtual-network/cloud-only.png)

    Virtuelles Netzwerk Azure Services mit Azure HDInsight verknüpfen ermöglicht Folgendes:

    * **Aufrufen von HDInsight Services oder Aufträge** von Azure Websites oder Dienste in Azure virtuellen Maschinen ausgeführt.

    * **Direkte Datenübertragung** zwischen HDInsight und Azure SQL-Datenbank, SQL Server oder anderen Daten-Storage-Lösung, die auf einem virtuellen Computer ausgeführt.

    * In einer einzigen Lösung **kombiniert mehrere HDInsight Server** . HDInsight Cluster sind in verschiedenen Typen entsprechen den Arbeitslast oder Technologie Cluster abgestimmt ist. Es ist keine unterstützte Methode in einem Cluster, der mehrere Typen wie Sturm und HBase auf einem Cluster kombiniert. Ein virtuelles Netzwerk kann mehrere Cluster direkt miteinander kommunizieren.

* Verbinden Sie Ihre Cloud-Ressourcen zu Ihrem lokalen Datencenter Netzwerk (zwischen Standorten oder Punkt-zu-Standort) mit einem virtuellen privaten Netzwerk (VPN).

    Standort-zu-Standort-Konfiguration können Sie mehrere Ressourcen aus Ihrem Rechenzentrum Azure virtuelles Netzwerk Verbindung mit Hardware VPN oder der Routing- und RAS-Dienst.

    ![Diagramm der Standort-zu-Standort-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/site-to-site.png)

    Punkt-zu-Standort-Konfiguration können Sie eine bestimmte Ressource Azure virtual Network mit Software-VPN-Verbindung.

    ![Diagramm der Punkt-zu-Standort-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/point-to-site.png)

    Virtuelles Netzwerk können Cloud und Rechenzentrum ähnliche Szenarios nur Cloud-Konfiguration. Jedoch beschränkt auf Ressourcen in der Cloud arbeiten können Sie auch Ressourcen im Rechenzentrum.

    * **Direkte Datenübertragung** zwischen HDInsight und Rechenzentrum. Ein Beispiel ist Sqoop zum Übertragen von Daten von SQL Server oder Lesen von Daten von einer Line-of-Business (LOB) Anwendung verwenden.

    * **Aufrufen von HDInsight Services oder Projekte** aus einer LOB-Anwendung. Ein Beispiel ist HBase Java APIs zum Speichern und Abrufen von Daten aus einem HDInsight HBase-Cluster verwenden.

Weitere Informationen zu virtuellen Netzwerk Funktionen, Vorteile und Funktionen finden Sie unter [Azure Virtual Network Overview](../virtual-network/virtual-networks-overview.md).

> [AZURE.NOTE] Sie müssen Azure Virtual Network vor Bereitstellung einen HDInsight-Cluster erstellen. Weitere Informationen finden Sie unter [Konfigurationsaufgaben virtuelles Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="virtual-network-requirements"></a>Virtuelle Netzwerk-Vorschriften

> [AZURE.IMPORTANT] Erstellen einer HDInsight in einem virtuellen Netzwerk erfordert bestimmte virtuelle Netzwerkkonfigurationen, die in diesem Abschnitt beschrieben werden.

###<a name="location-based-virtual-networks"></a>Standortbasierte virtuelle Netzwerke

Azure HDInsight unterstützt nur standortbasierte virtuelle Netzwerke und funktioniert derzeit nicht mit virtuellen Netzwerken basierend auf Gruppe.

###<a name="classic-or-v2-virtual-network"></a>Classic oder v2 virtuelles Netzwerk

Windows-basierten Clustern erfordern eine klassische virtuelles Netzwerk während Linux-basierten Clustern ein Azure Ressourcenmanager virtuelles Netzwerk erwarten. Wenn Sie keinen richtigen Netzwerk werden verwendet beim Erstellen des Clusters.

Wenn Sie Ressourcen in einem virtuellen Netzwerk, das nicht vom Cluster verwendet werden kann, die Sie erstellen möchten, können Sie erstellen ein neues virtuelles Netzwerk, die vom Cluster verwendet werden und inkompatible virtuelles Netzwerk an. Dann können den Cluster in die Netzwerkversion erfordert und Ressourcen in dem Netzwerk zugreifen sein, da die beiden verbunden sind. Weitere Informationen zum Anschließen von classic und neue virtuelle Netzwerke finden Sie unter [Verbinden klassische VNets auf neue VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

###<a name="custom-dns"></a>Benutzerdefinierte DNS

Beim Erstellen eines virtuellen Netzwerks bietet Azure standardauflösung für Azure Services wie HDInsight, die im Netzwerk installiert sind. Jedoch müssen Sie Ihre eigenen Domain Name System (DNS) für Situationen wie cross-Auflösung des Domänennamens Netzwerk verwenden. Beispielsweise wenn zwischen Webdiensten in zwei virtuelle Netzwerke verbunden. HDInsight unterstützt sowohl die standardmäßige Azure Auflösung sowie benutzerdefinierte DNS mit Azure Virtual Network.

Weitere Informationen über Ihre eigenen DNS-Server mit Azure Virtual Network finden Sie im Abschnitt __Auflösung über eigene DNS-Server__ des Dokuments [Auflösung für VMs und Instanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) .

###<a name="secured-virtual-networks"></a>Geschützte virtuelle Netzwerke

HDInsight-Dienst ist ein verwalteter Dienst und Internetzugang erforderlich, während der Bereitstellung und während der Ausführung. Daher, Azure Cluster Überwachung kann Failover für Clusterressourcen zu initiieren, die Anzahl der Knoten in dem Cluster durch Skalieren und andere Verwaltungsaufgaben.

Benötigen Sie HDInsight in einem gesicherten virtuellen Netzwerk installieren, müssen Sie eingehenden Datenverkehr über Port 443 für die folgenden IP-Adressen zulassen Azure HDInsight Cluster zu ermöglichen.

* 168.61.49.99
* 23.99.5.239
* 168.61.48.131
* 138.91.141.162

Eingehenden Zugriffs von Port 443 für diese Adressen können Sie HDInsight in einem gesicherten virtuellen Netzwerk erfolgreich installiert.

> [AZURE.IMPORTANT] HDInsight unterstützt keine ausgehenden Datenverkehr, nur Datenverkehr einschränken. Beim Definieren von Netzwerk-Sicherheitsgruppe Regeln für das Subnetz, das HDInsight enthält nur verwenden Sie eingehende Regeln.

Die folgenden Beispiele veranschaulichen eine neue Netzwerk-Sicherheitsgruppe erstellen, die erforderlichen Adressen ermöglicht und ein Subnetz innerhalb des virtuellen Netzwerks für die Sicherheitsgruppe. Diese Schritte setzen voraus, dass ein virtuelles Netzwerk und Subnetz, denen HDInsight in installieren möchten, bereits erstellt haben.

__Mithilfe von Azure PowerShell__

    $vnetName = "Replace with your virtual network name"
    $resourceGroupName = "Replace with the resource group the virtual network is in"
    $subnetName = "Replace with the name of the subnet that HDInsight will be installed into"
    # Get the Virtual Network object
    $vnet = Get-AzureRmVirtualNetwork `
        -Name $vnetName `
        -ResourceGroupName $resourceGroupName
    # Get the region the Virtual network is in.
    $location = $vnet.Location
    # Get the subnet object
    $subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
    # Create a new Network Security Group.
    # And add exemptions for the HDInsight health and management services.
    $nsg = New-AzureRmNetworkSecurityGroup `
        -Name "hdisecure" `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -name "hdirule1" `
            -Description "HDI health and management address 168.61.49.99" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "168.61.49.99" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 300 `
            -Direction Inbound `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -Name "hdirule2" `
            -Description "HDI health and management 23.99.5.239" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "23.99.5.239" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 301 `
            -Direction Inbound `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -Name "hdirule3" `
            -Description "HDI health and management 168.61.48.131" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "168.61.48.131" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 302 `
            -Direction Inbound `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -Name "hdirule4" `
            -Description "HDI health and management 138.91.141.162" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "138.91.141.162" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 303 `
            -Direction Inbound
    # Set the changes to the security group
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    # Apply the NSG to the subnet
    Set-AzureRmVirtualNetworkSubnetConfig `
        -VirtualNetwork $vnet `
        -Name $subnetName `
        -AddressPrefix $subnet.AddressPrefix `
        -NetworkSecurityGroupId $nsg

__Verwendung der Azure-CLI__

1. Verwenden Sie den folgenden Befehl zum Erstellen einer neuen Netzwerk-Sicherheitsgruppe mit dem Namen `hdisecure`. Ersetzen Sie __RESOURCEGROUPNAME__ und __Speicherort__ die Ressourcengruppe, die enthält Azure Virtual Network und den Speicherort (Region), dem die Gruppe erstellt wurde.

        azure network nsg create RESOURCEGROUPNAME hdisecure LOCATION
    
    Nachdem die Gruppe erstellt wurde, erhalten Sie Informationen zu der neuen Gruppe. Suchen Sie nach einer Zeile ähnlich der folgenden, und speichern Sie die `/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure` Informationen. Er wird in einem späteren Schritt verwendet werden.
    
        data:    Id                              : /subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure

2. Anhand der folgenden Regeln der neuen Netzwerk-Sicherheitsgruppe, die eingehende Kommunikation auf Port 443 aus dem Azure HDInsight und Management-Dienst hinzufügen. Ersetzen Sie __RESOURCEGROUPNAME__ durch den Namen der Ressourcengruppe, die Azure Virtual Network enthält.

        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule1 -p "*" -o "*" -u "443" -f "168.61.49.99" -e "VirtualNetwork" -c "Allow" -y 300 -r "Inbound"
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule2 -p "*" -o "*" -u "443" -f "23.99.5.239" -e "VirtualNetwork" -c "Allow" -y 301 -r "Inbound"
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule3 -p "*" -o "*" -u "443" -f "168.61.48.131" -e "VirtualNetwork" -c "Allow" -y 302 -r "Inbound"
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule4 -p "*" -o "*" -u "443" -f "138.91.141.162" -e "VirtualNetwork" -c "Allow" -y 303 -r "Inbound"

3. Angelegten Regeln mithilfe der Folgendes neuen Netzwerk-Sicherheitsgruppe Subnetz. Ersetzen Sie __RESOURCEGROUPNAME__ durch den Namen der Ressourcengruppe, die Azure Virtual Network enthält. Ersetzen Sie __VNETNAME__ und __SUBNETNAME__ mit dem Namen der Azure virtuelles Netzwerk und Subnetz, das Sie bei der Installation von HDInsight verwenden.

        azure network vnet subnet set RESOURCEGROUPNAME VNETNAME SUBNETNAME -w "/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    
    Sobald dieser Befehl abgeschlossen ist, können Sie HDInsight in gesicherten virtuellen Netzwerk im Subnetz verwendet diese Schritte erfolgreich installieren.

> [AZURE.IMPORTANT] Mithilfe der obigen Schritte nur auf HDInsight und Management-Dienst auf der Azure-Cloud. Dadurch einen HDInsight-Cluster in das Subnetz erfolgreich installiert jedoch Zugriff auf HDInsight Cluster außerhalb des virtuellen Netzwerks standardmäßig gesperrt wird. Sie müssen zusätzliche Netzwerk-Sicherheitsgruppe Regeln hinzufügen möchten Sie Zugriff von außerhalb des virtuellen Netzwerks zu ermöglichen.
>
> Beispielsweise um SSH-Zugriff über das Internet zu ermöglichen, müssen Sie eine Regel ähnlich der folgenden hinzufügen: 
>
> * Azure PowerShell-```Add-AzureRmNetworkSecurityRuleConfig -Name "SSSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 304 -Direction Inbound```
> * Azure CLI-```azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule4 -p "*" -o "*" -u "22" -f "*" -e "VirtualNetwork" -c "Allow" -y 304 -r "Inbound"```

Weitere Informationen zu Netzwerk-Sicherheitsgruppen Übersicht [Netzwerk-Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md). Informationen zum Steuern einer Azure Virtual Network routing finden Sie unter [Benutzer definierten Routen und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md).

##<a id="tasks"></a>Aufgaben und Informationen

Dieser Abschnitt enthält Informationen zu allgemeinen Aufgaben und Informationen, die Sie benötigen, wenn ein virtuelles Netzwerk HDInsight mit.

###<a name="determine-the-fqdn"></a>Ermitteln Sie den vollqualifizierten Domänennamen

HDInsight-Cluster wird einen bestimmte vollqualifizierten Domänennamen (FQDN) für die virtuelle Netzwerkschnittstelle zugewiesen. Dies ist die Adresse ein, die Cluster von anderen Ressourcen im virtuellen Netzwerk herstellen. Ermitteln Sie den vollqualifizierten Domänennamen Ambari-Verwaltungsdienst Abfragen anhand der folgenden URL:

    https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/<servicename>/components/<componentname>

> [AZURE.NOTE] Weitere Informationen zur Verwendung von Ambari mit HDInsight finden Sie unter [Monitor Hadoop Cluster in HDInsight mit der Ambari-API](hdinsight-monitor-use-ambari-api.md).

Sie müssen den Clusternamen, Service und auf Cluster wie GARN Ressourcen-Manager-Komponente angeben.

> [AZURE.NOTE] Die zurückgegebenen Daten ist ein JavaScript Object Notation (JSON)-Dokument, das viele Informationen über die Komponente enthält. Extrahieren nur den FQDN verwenden Sie JSON-Parser zum Abrufen der `host_components[0].HostRoles.host_name` Wert.

Beispielsweise den FQDN eines Clusters HDInsight Hadoop zurückgegeben, können eine der folgenden Methoden Sie zum Abrufen der Daten für Ressourcenmanager aus:

* [Azure PowerShell](../powershell-install-configure.md)

        $ClusterDnsName = <clustername>
        $Username = <cluster admin username>
        $Password = <cluster admin password>
        $DnsSuffix = ".azurehdinsight.net"
        $ClusterFQDN = $ClusterDnsName + $DnsSuffix

        $webclient = new-object System.Net.WebClient
        $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

        $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/yarn/     components/resourcemanager"
        $Response = $webclient.DownloadString($Url)
        $JsonObject = $Response | ConvertFrom-Json
        $FQDN = $JsonObject.host_components[0].HostRoles.host_name
        Write-host $FQDN

* [Rollen](http://curl.haxx.se/) und [jq](http://stedolan.github.io/jq/)

        curl -G -u <username>:<password> https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/yarn/components/resourcemanager | jq .host_components[0].HostRoles.host_name

###<a name="connecting-to-hbase"></a>HBase verbinden

HBase Remote Verbindung mit der Java API, Sie bestimmen das ZooKeeper Quorumressource für den Cluster HBase und dies in der Anwendung anzugeben.

Um die ZooKeeper Quorum Adresse, verwenden Sie eine der folgenden Methoden Ambari-Verwaltungsdienst Abfragen:

* [Azure PowerShell](../powershell-install-configure.md)

        $ClusterDnsName = <clustername>
        $Username = <cluster admin username>
        $Password = <cluster admin password>
        $DnsSuffix = ".azurehdinsight.net"
        $ClusterFQDN = $ClusterDnsName + $DnsSuffix

        $webclient = new-object System.Net.WebClient
        $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

        $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
        $Response = $webclient.DownloadString($Url)
        $JsonObject = $Response | ConvertFrom-Json
        Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'

* [Rollen](http://curl.haxx.se/) und [jq](http://stedolan.github.io/jq/)

        curl -G -u <username>:<password> "https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum" | jq .items[0].properties[]

> [AZURE.NOTE] Weitere Informationen zur Verwendung von Ambari mit HDInsight finden Sie unter [Monitor Hadoop Cluster in HDInsight mit der Ambari-API](hdinsight-monitor-use-ambari-api.md).

Haben die Quoruminformationen in der Clientanwendung verwenden.

Z. B. für eine Java-Anwendung, die die HBase-API verwendet Sie eine **Hbase site.xml** -Datei zum Projekt hinzufügen und die Quoruminformationen in der Datei wie folgt angeben:

```
<configuration>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>zookeeper0.address,zookeeper1.address,zookeeper2.address</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.clientPort</name>
    <value>2181</value>
  </property>
</configuration>
```

###<a name="verify-network-connectivity"></a>Überprüfen Sie die Netzwerkkonnektivität

Einige Dienste, wie SQL Server können eingehende Netzwerkanschlüsse beschränken. Dadurch wird verhindert, dass HDInsight erfolgreich mit diesen Services arbeiten.

Zugreifen auf einen Dienst von HDInsight Problemen lesen Sie die Dokumentation für den Dienst, um sicherzustellen, dass Netzwerkzugriff aktiviert haben. Sie können auch Netzwerkzugriff durch Erstellen von Azure virtuellen Computer im gleichen virtuellen Netzwerk überprüfen und Clientdienstprogramme sicherstellen, dass die virtuellen Computer den Dienst über das virtuelle Netzwerk Verbindung verwenden.

##<a id="nextsteps"></a>Nächste Schritte

Die folgenden Beispiele veranschaulichen die Verwendung von HDInsight mit Azure Virtual Network:

* [Analyse Daten mit Sturm und HBase in HDInsight](hdinsight-storm-sensor-data-analysis.md) - zeigt Sturm und HBase Cluster virtuellen Netzwerk konfigurieren sowie Remote Schreiben von Daten auf HBase von Storm.

* [Bereitstellung Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) - Informationen über die Bereitstellung Hadoop Cluster, einschließlich Informationen über Azure Virtual Network.

* [Mit Sqoop in HDInsight Hadoop](hdinsight-use-sqoop-mac-linux.md) - enthält Informationen zur Verwendung von Sqoop Daten mit SQL Server über ein virtuelles Netzwerk übertragen.

Erfahren Sie mehr über Azure virtuelle Netzwerke finden Sie unter [Übersicht über Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
