<properties 
   pageTitle="HBase Replikation zwischen zwei Rechenzentren | Microsoft Azure" 
   description="Informationen Sie zum HBase-Replikation über zwei Datenzentren und Anwendungsfälle Clusterreplikation konfigurieren." 
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
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>HDInsight HBase Geo-Replikation konfigurieren

> [AZURE.SELECTOR]
- [Konfigurieren von VPN-Konnektivität](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurieren von DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase Replikation](hdinsight-hbase-geo-replication.md) 
 
Informationen Sie zum Konfigurieren der Replikation HBase zwei Rechenzentren. Einige Anwendungsfälle Clusterreplikation gehören:

- Backup und Disaster recovery
- Daten
- Geografische Verteilung
- Online-Daten-Erfassung mit Offlinedaten Analytics kombiniert

Clusterreplikation verwendet eine Quelle Push-Methode. Ein HBase-Cluster kann eine Quelle ein Ziel oder beide Rollen gleichzeitig erfüllen kann. Replikation ist asynchron und das Ziel der Replikation ist eventual Consistency. Empfängt die Quelle bearbeiten einer Spalte Familie mit Replikation aktiviert ist, dass bearbeiten alle Ziel-Cluster weitergegeben. Wenn Daten von einem Cluster auf einen anderen repliziert werden, werden Quell-Cluster und alle Cluster die Daten bereits verbraucht haben verfolgt, um Replikation verhindert. Weitere Informationen werden In diesem Lernprogramm Quelle-Ziel-Replikation konfigurieren.  Andere Clustertopologien finden Sie unter [Apache HBase-Referenzhandbuch](http://hbase.apache.org/book.html#_cluster_replication).

Dies ist der dritte Teil der Serie:

- [Konfigurieren einer VPN-Verbindung zwischen zwei virtuelle Netzwerke][hdinsight-hbase-replication-vnet]
- [Konfigurieren von DNS für virtuelle Netzwerke][hdinsight-hbase-replication-dns]
- HBase Geo Replikation (dieses Lernprogramm)

Die folgende Abbildung zeigt zwei virtuelle Netzwerke und die Netzwerkkonnektivität in [Konfigurieren einer VPN-Verbindung zwischen zwei virtuelle Netzwerke] erstellt[ hdinsight-hbase-geo-replication-vnet] und [Konfigurieren von DNS für die virtuellen Netzwerke][hdinsight-hbase-replication-dns]: 

![HDInsight HBase Replikation virtuelles Netzwerkdiagramm][img-vnet-diagram]

## <a id="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Workstation mit Azure PowerShell**.

    PowerShell-Skripts ausführen, müssen Sie Azure PowerShell als Administrator ausführen und die Ausführungsrichtlinie auf *RemoteSigned*festgelegt. Anzeigen Sie verwenden das Cmdlet "Set-ExecutionPolicy"
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Zwei Azure virtual Network VPN-Konnektivität und DNS konfiguriert**.  Eine Anleitung finden Sie [konfigurieren eine VPN-Verbindung zwischen zwei virtuellen Netzwerken Azure][hdinsight-hbase-replication-vnet], und [Konfigurieren Sie DNS zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-replication-dns].


    Stellen Sie bevor PowerShell-Skripts sicher, dass Sie Ihre Azure-Abonnement mit dem folgenden Cmdlet verbunden sind:

        Add-AzureAccount

    Haben mehrere Azure-Abonnements mit dem folgenden Cmdlet das aktuelle Abonnement fest:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>HBase in HDInsight Cluster bereitstellen

Unter [Konfigurieren einer VPN-Verbindung zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-replication-vnet], Sie haben ein virtuelles Netzwerk in einem Europa Rechenzentrum und ein virtuelles Netzwerk in einem US-Rechenzentrum. Zwei virtuelle Netzwerk sind über VPN verbunden sind. In dieser Sitzung wird einen HBase Cluster virtuellen Netzwerke bereitstellen. Später in diesem Lernprogramm werden Sie HBase Cluster HBase Cluster replizieren erstellen.

Azure-Verwaltungsportal unterstützt keine Bereitstellung HDInsight Cluster mit benutzerdefinierten Optionen. Beispielsweise *hbase.replication* auf *true*festgelegt. Wenn Sie den Wert in der Konfigurationsdatei festlegen, nach ein Cluster bereitgestellt wird, verloren die Einstellung nach Cluster versehen ist. Weitere Informationen finden Sie unter [Bereitstellung Hadoop Cluster in HDInsight][hdinsight-provision]. Optionen HDInsight Cluster mit benutzerdefinierten Optionen Bereitstellung verwendet Azure PowerShell.


**Bereitstellung eines HBase Clusters in Contoso-VNet-EU** 

1. Öffnen Sie Windows PowerShell ISE auf der Arbeitsstation.
2. Die Variablen am Anfang des Skripts festgelegt und dann das Skript ausführen.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**Bereitstellung eines HBase Clusters in Contoso-VNet-US** 

- Verwenden Sie dasselbe Skript mit den folgenden Werten:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Da Sie bereits ein Azure-Konto verbunden haben, müssen Sie die folgenden Comlets mehr ausführen:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>Konfigurieren von bedingter DNS-Weiterleitung

[Konfigurieren]von DNS für virtuelle Netzwerke[hdinsight-hbase-replication-dns], DNS-Server für zwei Netzwerke konfiguriert haben. HBase Cluster haben unterschiedliche Domänensuffixe. So müssen Sie zusätzliche DNS-bedingte Weiterleitung.

Zum Konfigurieren der bedingten Weiterleitung müssen Sie Domänensuffixe zwei HBase Cluster kennen. 

**Die Domänensuffixe zwei HBase Cluster gefunden**

1. RDP in **Contoso-HBase-EU**  Informationen finden Sie [in HDInsight mit der Azure-Verwaltungsportal verwalten Hadoop Cluster][hdinsight-manage-portal]. Tatsächlich headnode0 des Clusters ist.
2. Öffnen einer Windows PowerShell-Konsole oder eine Befehlszeile.
3. Führen Sie **Ipconfig**, und notieren Sie **verbindungsspezifische DNS-Suffix**.
4. Schließen Sie nicht die RDP-Sitzung.  Sie benötigen sie später zum Testen Auflösung des Domänennamens.
5. Wiederholen Sie die Schritte **verbindungsspezifischen DNS-Suffix** des **Contoso-HBase-US**zu.


**So konfigurieren Sie DNS-Weiterleitung**
 
1.  RDP in **Contoso-DNS-EU** 
2.  Klicken Sie auf die Windows-Taste unten links.
2.  Klicken Sie auf **Verwaltung**.
3.  Klicken Sie auf **DNS**.
4.  Erweitern Sie im linken Bereich **DSN** **Contoso-DNS-EU**.
5.  **Bedingte Weiterleitung**Maustaste, und klicken Sie dann auf **Neue bedingte Weiterleitung**. 
5.  Geben Sie Folgendes ein:
    - **DNS-Domäne**: Geben Sie das DNS-Suffix des Contoso-HBase-US. Beispiel: Contoso-HBase-US.f5.internal.cloudapp.net.
    - **IP-Adressen der Masterserver**: 10.2.0.4, die Contoso-DNS-US IP-Adresse eingeben. Überprüfen Sie die IP-Adresse. Der DNS-Server haben eine andere IP-Adresse.
6.  Drücken Sie die **EINGABETASTE**, und klicken Sie dann auf **OK**.  Nun werden Sie die Contoso-DNS-US IP-Adresse von Contoso-DNS-EU auflösen.
7.  Wiederholen Sie die Schritte zum Hinzufügen von bedingten DNS-Weiterleitung des DNS-Dienstes auf dem Contoso-DNS-US virtuellen Computer mit den folgenden Werten:
    - **DNS-Domäne**: Geben Sie das DNS-Suffix des Contoso-HBase-EU. 
    - **IP-Adressen der Masterserver**: 10.2.0.4, die Contoso-DNS-EU IP-Adresse eingeben.

**Auflösung des Domänennamens testen**

1. Wechseln Sie zum Fenster Contoso-HBase-EU RDP.
2. Öffnen Sie ein Eingabeaufforderungsfenster.
3. Führen Sie den Pingbefehl:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    Das ICM-Protokoll aktiviert Arbeitskraft Knoten der Cluster HBase

4. Schließen Sie die RDP-Sitzung nicht. Sie werden es später im Lernprogramm benötigen.
5. Wiederholen Sie dieselbe headnode0 der Contoso-HBase-EU von Contoso-HBase-US pingen.

>[AZURE.IMPORTANT] DNS muss funktionieren, bevor Sie mit den nächsten Schritten fortfahren können.

## <a name="enable-replication-between-hbase-tables"></a>Aktivieren der Replikation zwischen HBase Tabellen

Sie können nun eine Beispieltabelle HBase Replikation aktivieren und Testen Sie es mit Daten. Sie verwenden die Beispieltabelle hat zwei Spalte Familien: Personal und Office. 

Stellen Sie in diesem Lernprogramm Europa HBase Cluster Clusters Quelle und Ziel-Cluster Cluster US HBase.

Erstellen Sie HBase Tabellen mit der gleichen Spalte Familien Quelle und Ziel-Cluster, damit Zielcluster weiß, wo Daten gespeichert, die sie erhalten. Weitere Informationen über die HBase Shell finden Sie unter [Erste Schritte mit Apache HBase in HDInsight][hdinsight-hbase-get-started].

**Erstellen Sie eine HBase Tabelle Contoso-HBase-EU**

1. Wechseln Sie zum Fenster RDP **Contoso-HBase-EU** .
2. Klicken Sie auf dem Desktop auf **Hadoop Befehlszeile**.
2. Ändern Sie den Ordner zum Basisverzeichnis HBase:

        cd %HBASE_HOME%\bin
3. Öffnen Sie die HBase-Shell:

        hbase shell
4. Erstellen Sie HBase Tabelle:

        create 'Contacts', 'Personal', 'Office'
5. Schließen Sie die RDP-Sitzung noch Hadoop Befehlszeilenfenster an. Sie werden sie später im Lernprogramm benötigen.
    
**Erstellen Sie eine HBase-Tabelle auf Contoso-HBase-US**

- Wiederholen Sie die Schritte erstellen derselben Tabelle auf Contoso-HBase-US.


**Contoso-HBase-US als Peer Replikation hinzufügen**

1. Wechseln Sie zum Fenster RDP **Contso HBase_EU** .
2. Fügen Sie aus HBase Shell-Fenster Zielcluster (Contoso-HBase-US) beispielsweise als Peer, hinzu:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    Im Beispiel ist der Domäne *Contoso-Hbase-us.d4.internal.cloudapp.net*. Sie müssen Ihre Domänensuffix des Clusters uns HBase entsprechend aktualisieren. Stellen Sie sicher, dass keine Leerzeichen zwischen dem Hostnamen.

**Jede Spalte Familie auf dem Quell-Cluster repliziert werden sollen**

1. **Contso-HBase-EU** RDP-Sitzung Fenster Shell HBase konfigurieren Sie jeder Spalte Familie repliziert werden:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Zum Upload von Daten in HBase-Tabelle**

Eine Beispieldatei hat einen öffentlichen Azure BLOB-Container mit dem folgenden URL hochgeladen:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

Der Inhalt der Datei:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

HBase Cluster die Datendatei hochladen, und importieren Sie die Daten aus.

1. Wechseln Sie zum Fenster RDP **Contoso-HBase-EU** .
2. Klicken Sie auf dem Desktop auf **Hadoop Befehlszeile**.
3. Ändern Sie den Ordner zum Basisverzeichnis HBase:

        cd %HBASE_HOME%\bin

4. Hochladen der Daten:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Überprüfen Sie, ob die Datenreplikation stattfindet

Sie können überprüfen, dass die Replikation stattfindet Tabellen aus beiden HBase Shell Befehle scannen:

        Scan 'Contacts'


## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie HBase-Replikation über zwei Rechenzentren konfigurieren. Weitere Informationen zu HDInsight und HBase finden Sie unter:

- [HDInsight Seite](https://azure.microsoft.com/services/hdinsight/)
- [HDInsight-Dokumentation](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Erste Schritte mit Apache HBase in HDInsight][hdinsight-hbase-get-started]
- [HDInsight HBase (Übersicht)][hdinsight-hbase-overview]
- [HBase Cluster virtuellen Netzwerk Azure bereitstellen][hdinsight-hbase-provision-vnet]
- [Analyse in Echtzeit Twitter Stimmung mit HBase][hdinsight-hbase-twitter-sentiment]
- [Analysieren von Daten mit Sturm und HBase in HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
