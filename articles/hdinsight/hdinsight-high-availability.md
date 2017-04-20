<properties
    pageTitle="Verfügbarkeit von Hadoop Clustern in HDInsight | Microsoft Azure"
    description="HDInsight bringt hoch verfügbarer und zuverlässiger Cluster mit zusätzlicher Head-Knoten."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Verfügbarkeit und Zuverlässigkeit von Windows-basierten Hadoop Cluster HDInsight


>[AZURE.NOTE] Die Schritte in diesem Dokument sind spezifisch für Windows-basierte HDInsight-Cluster. Bei Verwendung von Linux-basierten Cluster finden Sie unter [Verfügbarkeit und Zuverlässigkeit von Linux-basierten Hadoop Cluster HDInsight](hdinsight-high-availability-linux.md) Linux-spezifische Informationen.

HDInsight kann Kunden Cluster Datentypen für verschiedene Analysen Arbeitslasten. Heute Clustertypen sind Hadoop für Abfrage und Analyse Arbeitslasten, HBase Cluster NoSQL-Arbeitslasten und Storm-Cluster Echtzeit Ereignis Arbeitslasten. Innerhalb einer bestimmten Clustertyp sind unterschiedliche Rollen für verschiedene Knoten. Zum Beispiel:



- Hadoop Cluster HDInsight werden mit zwei Funktionen bereitgestellt:
    - Head-Knoten (2 Knoten)
    - Datenknoten (Knoten 1)

- HBase HDInsight-Cluster sind drei Rollen bereitgestellt:
    - Head-Server (2 Knoten)
    - Region-Server (mindestens 1 Knoten)
    - Master-Zookeeper Knoten (3 Knoten)

- Storm-Cluster HDInsight sind mit drei Funktionen bereitgestellt:
    - Nimbus-Knoten (2 Knoten)
    - Supervisor-Servern (mindestens 1 Knoten)
    - Zookeeper-Knoten (3 Knoten)

Standard-Implementierungen Hadoop Cluster haben in der Regel einen einzelnen Head-Knoten. HDInsight entfernt dieses einzelne Fehlerquelle mit einer sekundären Hauptknoten/Head Server-Nimbus Knoten die Verfügbarkeit und Zuverlässigkeit der erforderliche Arbeitslasten zu erhöhen. Diese Server-Nimbus Knoten Head-Knoten-Head für Arbeitskraft Knoten Fehler problemlos verwalten, aber würde Ausfälle von master Services auf dem Head-Knoten des Clusters zu arbeiten.


[ZooKeeper](http://zookeeper.apache.org/ ) -Knoten (ZKs) hinzugefügt wurden und werden zum Marktführer Wahl der Head-Knoten und um sicherzustellen, dass Arbeitnehmer Knoten und Gateways (GWs) beim Failover auf den sekundären Head-Knoten (Head Knoten1) Wenn aktive Headknoten (Head Node0) deaktiviert wird.

![Diagramm des zuverlässigen Hauptknoten HDInsight Hadoop-Implementierung.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Aktive Headknoten Servicestatus überprüfen
Bestimmen der Head-Knoten aktiv ist und den Status der Dienste auf Head-Knoten müssen Sie Hadoop-Cluster mit Remote Desktop Protocol (RDP) verbinden. Hinweise RDP finden Sie [in HDInsight mit Azure-Portal verwalten Hadoop Cluster](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Sobald Sie mit dem Cluster verbunden sind, doppelklicken Sie auf das Symbol auf dem Desktop zu Status der Head-Knoten den Namenode **Hadoop Service verfügbar** , Jobtracker, Templeton, Oozieservice, Metastore und Hiveserver2 Dienste ausgeführt werden, oder für HDI 3.0, Namenode, Ressourcenmanager, Verlauf Server, Templeton, Oozieservice, Metastore und Hiveserver2 Dienste.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Auf dem Screenshot ist der Head-Knoten active *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Access-Protokolldateien auf dem sekundären Hauptknoten

Auftrag Zugriff auf Protokolle auf der sekundären Head-Knoten, die aktive Headknoten JobTracker UI durchsuchen geworden funktioniert wie für den primären aktiven Knoten. Zugriff auf JobTracker müssen Hadoop Cluster herstellen über RDP wie im vorherigen Abschnitt beschrieben. Haben Sie dem Cluster Remote, doppelklicken Sie auf das Symbol **Hadoop Name den Status** auf dem Desktop und klicken auf **NameNode Protokolle** in das Verzeichnis der Protokolle auf der sekundären Head-Knoten abzurufen.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Konfigurieren der Head-Knoten-Größe
Die Head-Knoten werden als große virtuelle Maschinen (VMs) standardmäßig zugewiesen. Diese Größe ist für die Verwaltung der meisten Hadoop Aufträge im Cluster ausgeführt. Aber es gibt Szenarien, in denen Extragroß VMs für den Hauptknoten erfordern. Ein Beispiel ist der Cluster verfügt über eine große Anzahl von kleinen Oozie Aufträge verwalten.

Extra große VMs können Azure PowerShell-Cmdlets oder HDInsight SDK konfiguriert werden.

[Verwalten von HDInsight mithilfe von PowerShell](hdinsight-administer-use-powershell.md)ist die Erstellung und Bereitstellung eines Clusters mit Azure PowerShell dokumentiert. Konfiguration der Extragroß Headknoten muss zusätzlich die `-HeadNodeVMSize ExtraLarge` Parameter der `New-AzureRmHDInsightcluster` Cmdlet in diesem Code verwendet.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

Für das SDK ist ähnlich. Die Erstellung und Bereitstellung eines Clusters mit dem SDK dokumentiert [HDInsight .NET SDK verwenden](hdinsight-provision-clusters.md#sdk). Konfiguration der Extragroß Headknoten muss zusätzlich die `HeadNodeSize = NodeVMSize.ExtraLarge` Parameter der `ClusterCreateParameters()` Methode im Code.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Nächste Schritte

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Verbinden Sie mit HDInsight-Cluster über RDP](hdinsight-administer-use-management-portal.md#rdp)
- [HDInsight .NET SDK verwenden](hdinsight-provision-clusters.md#sdk)
