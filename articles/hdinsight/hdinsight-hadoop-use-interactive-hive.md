<properties
    pageTitle="Verwenden Sie interaktive Struktur in HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie mit interaktiven Hive (Struktur auf LLAP) in HDInsight."
    keywords=""
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Verwenden Sie interaktive Struktur in HDInsight (Vorschau)

Interaktive Struktur (auch bekannt als [Live Long und Prozess]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) ist ein neuer HDInsight [Clustertyp]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interaktive Struktur kann im Arbeitsspeicher zwischenspeichern, die Hive-Abfragen wesentlich mehr interaktive und schneller machen. Neue dadurch HDInsight der weltweit die meisten leistungsfähige, flexible und Big Data Projektmappe öffnen, die Cloud Speicher (mit Struktur und Funken) speichert und Analysen durch eine Tiefe Integration mit R. 

Interaktive Struktur Cluster unterscheidet Hadoop-Cluster. Sie enthält nur den Hive-Dienst. 

> [AZURE.NOTE] MapReduce Schwein, Sqoop, Oozie und andere Dienste werden aus diesem Clustertyp bald entfernt.
Struktur Dienst im Cluster interaktive Struktur ist nur über die Ambari Struktur anzeigen Luftlinie und ODBC-Struktur. Es kann nicht über Konsole, Templeton, Azure-CLI und Azure PowerShell Struktur zugegriffen werden. 


 


## <a name="create-an-interactive-hive-cluster"></a>Erstellen eines interaktiven Struktur Clusters

Interaktive Struktur Cluster wird nur auf Linux-basierten Clustern unterstützt. Informationen zum Erstellen von HDInsight-Cluster finden Sie unter [Hadoop erstellen Linux-basierten Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Struktur von interaktiven Hive ausführen

Es gibt verschiedene Optionen wie Hive-Abfragen ausführen können:

- Führen Sie Struktur der Ambari Struktur Ansicht aus

    Informationen zur Verwendung der Struktur anzeigen finden Sie unter [Verwenden der Struktur Ansicht Hadoop in HDInsight]( hdinsight-hadoop-use-hive-ambari-view.md).

- Führen Sie Struktur mit Luftlinie aus

    Informationen mit Abstecher auf HDInsight finden Sie in der [Struktur mit Hadoop in HDInsight mit Luftlinie](hdinsight-hadoop-use-hive-beeline.md).

    Luftlinie wird der Hauptknoten oder einen leeren Kantenknoten verwenden.  Eine leere Kantenknoten Luftlinie aus wird empfohlen.  Informationen zum Erstellen eines Clusters HDInsight mit einem leeren Edgenode anzeigen Sie [mit leeren Randknoten im HDInsight](hdinsight-apps-use-edge-node.md)

- Führen Sie Struktur mit ODBC Hive aus

    Informationen zur Verwendung von ODBC Struktur finden Sie unter [Verbinden Excel Hadoop Microsoft Struktur-ODBC-Treiber](hdinsight-connect-excel-hive-odbc-driver.md).

**JDBC-Verbindungsstring finden:**

1.  Melden Sie sich mithilfe der folgenden URL Ambari: https://<ClusterName>. AzureHDInsight.net.
2.  Klicken Sie im linken Menü **Struktur** .
3.  Klicken Sie zum Kopieren der URL auf hervorgehoben:

    ![HDInsight Hadoop interaktive Struktur llap-JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Siehe auch
-   [Hadoop erstellen Linux-basierten Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): interaktive Struktur Cluster in HDInsight erstellen.
-   [Hadoop in HDInsight mit Luftlinie Struktur verwenden](hdinsight-hadoop-use-hive-beeline.md): Informationen zum Luftlinie Struktur Abfragen senden.
-   [Excel Hadoop Microsoft Struktur-ODBC-Treiber herstellen](hdinsight-connect-excel-hive-odbc-driver.md): erfahren Sie, wie Excel Struktur verbunden.
