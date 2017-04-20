<properties
pageTitle="Ports, die von HDInsight | Azure"
description="Eine Liste der Ports, die von Hadoop Dienste auf HDInsight."
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
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Ports und URIs von HDInsight verwendet

Dieses Dokument enthält eine Liste von Hadoop auf Linux-basierten HDInsight Cluster ausgeführten Dienste verwendeten Ports. Darüber hinaus Informationen Ports für die Verbindung mit SSH verwendet.

## <a name="public-ports-vs-non-public-ports"></a>Öffentliche Ports und nicht öffentliche ports

Linux-basierte HDInsight Cluster und macht nur drei Ports öffentlich im Internet verfügbar. 22, 23 und 443. Diese dienen sicher über SSH und Dienste verfügbar über das sichere HTTPS-Protokoll zugreifen.

HDInsight wird intern durch mehrere Azure Virtual Machines (die Knoten innerhalb des Clusters) implementiert ein virtuelles Netzwerk Azure ausgeführt. Innerhalb des virtuellen Netzwerks können Sie nicht über das Internet verfügbar gemachten Ports zugreifen. Beispielsweise wenn eine Hauptknoten über SSH Verbindung möglich aus dem Head-Knoten dann direkt Dienste auf den Clusterknoten.

> [AZURE.IMPORTANT] Wenn Sie HDInsight-Cluster erstellen und ein virtuelles Azure-Netzwerk nicht als eine Option angeben, wird einer erstellt. jedoch können nicht Sie anderen Computern (wie andere Azure-Computer oder dem Clientcomputer Entwicklung) automatisch erstellten virtuelles Netzwerk hinzufügen. 

Um weitere Computer an das virtuelle Netzwerk beizutreten, müssen Sie zunächst das virtuelle Netzwerk und dann angeben, wenn HDInsight Cluster erstellen. Weitere Informationen finden Sie unter [Funktionen über ein virtuelles Netzwerk Azure HDInsight erweitern](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Öffentliche ports

Alle Knoten in einem Cluster HDInsight in ein virtuelles Azure-Netzwerk befinden und können nicht direkt aus dem Internet zugegriffen werden. Eine öffentliche Gateway bietet Internetzugriff auf die folgenden Ports, die für alle Arten von HDInsight Cluster.

| Dienst | Anschluss | Protokoll | Beschreibung |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | Kunden verbunden mit Sshd auf primäre Hauptknoten. Siehe [SSH mit Linux-basierten HDInsight verwenden](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22 | SSH | Kunden verbunden mit Sshd auf dem edgeknoten (nur HDInsight Premium). Finden Sie unter [Erste Schritte mit R Server auf HDInsight](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | Kunden verbunden mit Sshd auf sekundären Hauptknoten. Siehe [SSH mit Linux-basierten HDInsight verwenden](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | HTTPS | Ambari Web-Benutzeroberfläche. Finden Sie unter [Verwalten von HDInsight mit Ambari Web-Benutzeroberfläche](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | HTTPS | Ambari-REST-API. Finden Sie unter [Verwalten von HDInsight mit Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | HTTPS | HCatalog-REST-API. Siehe [verwenden Struktur mit Kringel](hdinsight-hadoop-use-pig-curl.md) [Schwein mit Kringel verwenden](hdinsight-hadoop-use-pig-curl.md), [Verwenden Sie MapReduce mit Kringel](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ODBC | Struktur mit ODBC herstellt. Siehe [Verbindung Excel HDInsight mit Microsoft ODBC-Treiber](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Verbindung mit JDBC Struktur. Finden Sie unter [Verbinden Hive auf HDInsight Struktur JDBC-Treiber verwenden](hdinsight-connect-hive-jdbc-driver.md) |

Folgen für bestimmte Cluster verfügbar:

| Dienst | Anschluss | Protokoll |Clustertyp | Beschreibung |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | HTTPS | HBase | HBase-REST-API. Finden Sie unter [Erste Schritte mit HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livius | 443 | HTTPS |  Funken | Spark-REST-API. [Senden Spark](hdinsight-apache-spark-livy-rest-interface.md) Stellenangebote Livius über |
| Sturm | 443 | HTTPS | Sturm | Storm Webbenutzeroberfläche. Siehe [Bereitstellen und Sturm Topologien auf HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Authentifizierung

Alle Dienste im Internet veröffentlicht müssen authentifiziert werden:

| Anschluss | Anmeldeinformationen |
| ---- | ----------- |
| 22 und 23 | Bei der Clustererstellung angegebenen SSH Benutzeranmeldeinformationen |
| 443 | Der Benutzername (Standard: Admin) und das Kennwort während der Erstellung des Clusters festgelegt wurden |

## <a name="non-public-ports"></a>Nicht öffentliche ports

> [AZURE.NOTE] Einige Dienste stehen nur auf bestimmten Clustertyp. HBase ist z. B. nur auf HBase Cluster verfügbar.

### <a name="hdfs-ports"></a>BIETET ports

| Dienst | Knoten | Anschluss | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode Web-Benutzeroberfläche | Head-Knoten | 30070 | HTTPS | Webbasierte Benutzeroberfläche zum aktuellen Status anzeigen |
| NameNode Metadatendienst | Head-Knoten | 8020 | IPC | Dateisystem-Metadaten 
| DataNode | Alle workerknoten | 30075 | HTTPS | Webbenutzeroberfläche Anzeigestatus, Protokolle usw.. |
| DataNode | Alle workerknoten | 30010 | &nbsp; | Datenübertragung |
| DataNode | Alle workerknoten | 30020 | IPC | Metadaten-Operationen |
| Sekundärer NameNode | Head-Knoten | 50090 | HTTP | Checkpoint NameNode Metadaten |

### <a name="yarn-ports"></a>GARN-ports

| Dienst | Knoten | Anschluss | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Ressourcenmanager Webbenutzeroberfläche | Head-Knoten | 8088 | HTTP | Webbasierte Benutzeroberfläche für Ressourcenmanager |
| Ressourcenmanager Webbenutzeroberfläche | Head-Knoten | 8090 | HTTPS | Webbasierte Benutzeroberfläche für Ressourcenmanager |
| Ressourcen-Manager-Admin-Schnittstelle | Head-Knoten | 8141 | IPC | Für Anträge auf Zulassung Anwendung (Struktur Struktur Server Schwein, etc..) |
| Ressourcen-Manager scheduler | Head-Knoten | 8030 | HTTP | Administrative Schnittstelle |
| Ressourcenmanager-Anwendungsschnittstelle | Head-Knoten | 8050 | HTTP |Adresse des Manager-Benutzeroberfläche der Anwendung |
| NodeManager | Alle workerknoten | 30050 | &nbsp; | Die Adresse des Container-Managers |
| NodeManager Webbenutzeroberfläche | Alle workerknoten | 30060 | HTTP | Ressourcen-Manager-Schnittstelle |
| Zeitachse Adresse | Head-Knoten | 10200 | RPC | Die Zeitachse RPC-Dienst. |
| Zeitachse Webbenutzeroberfläche | Head-Knoten | 8181 | HTTP | Der Zeitachse Service Web-Benutzeroberfläche |

### <a name="hive-ports"></a>Struktur-ports

| Dienst | Knoten | Anschluss | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Head-Knoten | 10001 | Sparsamkeit | Service für programmgesteuert mit Struktur (Sparsamkeit/JDBC) |
| HiveServer | Head-Knoten | 10000 | Sparsamkeit | Service für programmgesteuert mit Struktur (Sparsamkeit/JDBC) |
| Struktur Metastore | Head-Knoten | 9083 | Sparsamkeit | Service für programmgesteuert mit Struktur Metadaten (Sparsamkeit/JDBC) |

### <a name="webhcat-ports"></a>WebHCat-Anschlüsse

| Dienst | Knoten | Anschluss | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat server | Head-Knoten | 30111 | HTTP | Web-API auf HCatalog und andere Hadoop-Dienste |

### <a name="mapreduce-ports"></a>MapReduce-ports

| Dienst | Knoten | Anschluss | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Head-Knoten | 19888 | HTTP | MapReduce JobHistory Web-Benutzeroberfläche |
| JobHistory | Head-Knoten | 10020 | &nbsp; | MapReduce JobHistory server |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Übertragung zwischen Karte gibt Reduzierstück angefordert |

### <a name="oozie"></a>Oozie

| Dienst | Knoten | Anschluss | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie-server | Head-Knoten | 11000 | HTTP | URL für den Oozie-Dienst |
| Oozie-server | Head-Knoten | 11001 | HTTP | Port für Oozie-admin |

### <a name="ambari-metrics"></a>Ambari Kennzahlen

| Dienst | Knoten | Anschluss | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Zeitachse (Anwendung History) | Head-Knoten | 6188 | HTTP | Der Zeitachse Service Web-Benutzeroberfläche |
| Zeitachse (Anwendung History) | Head-Knoten | 30200 | RPC | Der Zeitachse Service Web-Benutzeroberfläche |

### <a name="hbase-ports"></a>HBase-Anschlüsse

| Dienst | Knoten | Anschluss | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Head-Knoten | 16000 | &nbsp; | &nbsp; |
| HMaster Info Webbenutzeroberfläche | Head-Knoten | 16010 | HTTP | Der Port für die Webbenutzeroberfläche HBase Master |
| Region-server | Alle workerknoten | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | Ports, die Clients zum ZooKeeper herstellen verwenden |

### <a name="kafka-ports"></a>Kafka-Anschlüsse

| Dienst | Knoten | Anschluss | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Broker  | Arbeitskraft-Knoten | 9092 | [Kafka-Protokoll](http://kafka.apache.org/protocol.html) | Für die Clientkommunikation verwendet |
| &nbsp; | Zookeeper-Knoten | 2181 | &nbsp; | Ports, die Clients zum Zookeeper herstellen verwenden |
