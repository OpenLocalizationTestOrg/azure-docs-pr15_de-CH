<properties 
   pageTitle="Verwenden Sie Apache Phoenix und Eichhörnchen in HDInsight | Microsoft Azure" 
   description="Erfahren Sie, wie Apache Phoenix in HDInsight und zum Installieren und Konfigurieren von Eichhörnchen auf Ihrer Arbeitsstation für die Verbindung zu einem Cluster HBase in HDInsight." 
   services="hdinsight" 
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Verwenden Sie Apache Phoenix mit Linux-basierten HBase in HDInsight  

Lernen Sie mit [Apache Phoenix](http://phoenix.apache.org/) in HDInsight und SQLLine verwenden. Finden Sie weitere Informationen über Phoenix [Phoenix in 15 Minuten oder weniger](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Phoenix-Grammatik finden Sie unter [Phoenix-Grammatik](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Phoenix Versionsinformationen im HDInsight finden Sie in [neuen Hadoop Cluster Versionen von HDInsight bereitgestellten?] [hdinsight-versions].

##<a name="use-sqlline"></a>SQLLine verwenden
[SQLLine](http://sqlline.sourceforge.net/) ist ein Befehlszeilen-Dienstprogramm SQL ausführen. 

###<a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie SQLLine verwenden können, benötigen Sie Folgendes:

- **Ein HBase-Cluster in HDInsight**. Informationen auf HBase cluster, finden Sie unter [Erste Schritte mit Apache HBase in HDInsight][hdinsight-hbase-get-started].
- **Verbindung zum Cluster HBase remote Desktop-Protokoll**. Informationen finden Sie [in HDInsight mit der Azure-Verwaltungsportal verwalten Hadoop Cluster][hdinsight-manage-portal].


Beim Herstellen einer zu einem Cluster HBase Verbindung müssen Sie die lassen zu verbinden. Jeder HDInsight-Cluster verfügt über 3 lassen. 

**Zu den Zookeeper-Hostnamen**

1. Wechseln Sie zu Ambari öffnen **https://<ClusterName>. azurehdinsight.net**.
2. HTTP (Cluster) Benutzernamen und Kennwort für die Anmeldung eingeben.
3. Klicken Sie im linken Menü auf **ZooKeeper** . 3 **ZooKeeper Server** aufgeführt sehen.
4. Klicken Sie auf eine der aufgelisteten **ZooKeeper Server** . Finden Sie in der Zusammenfassung der **Hostname**. Es ähnelt dem *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine verwenden**

1. Verbinden Sie mit SSH. Hinweise finden Sie je nach Ihrem Clientcomputer OS [Verwendet SSH mit Linux-basierten Hadoop auf Linux, Unix oder OS X HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) oder [Verwenden SSH Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) .

2. Führen Sie von SSH die folgenden Befehle SQLLine ausführen:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Führen Sie die folgenden Befehle HBase erstellen, und fügen Sie Daten:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Weitere Informationen finden Sie unter [Manuelle SQLLine](http://sqlline.sourceforge.net/#manual) und [Phoenix-Grammatik](http://phoenix.apache.org/language/index.html).


 
##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Apache Phoenix in HDInsight verwendet.  Weitere Informationen finden Sie unter

- [Übersicht über die HDInsight HBase][hdinsight-hbase-overview]: HBase ist eine Apache Open-Source NoSQL-Datenbank basierend auf Hadoop, das RAM und starke Konsistenz für große Mengen von unstrukturierten und teilstrukturierte Daten bereitstellt.
- [HBase Cluster virtuellen Netzwerk Azure bereitstellen][hdinsight-hbase-provision-vnet]: virtuelles Netzwerk Integration HBase Cluster mit demselben virtuellen Netzwerk als Anwendung bereitgestellt werden, damit Programme HBase direkt kommunizieren können.
- [Replikation in HDInsight HBase konfigurieren](hdinsight-hbase-geo-replication.md): erfahren Sie mehr über zwei Azure-Datencentern HBase Replikation konfigurieren. 
- [Analysieren von Twitter Stimmung mit HBase in HDInsight][hbase-twitter-sentiment]: Lernen in Echtzeit [stimmungsanalyse](http://en.wikipedia.org/wiki/Sentiment_analysis) großer Datenmengen mit HBase in einem Cluster Hadoop in HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
