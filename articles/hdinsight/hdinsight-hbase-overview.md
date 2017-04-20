<properties
    pageTitle="Was ist HBase in HDInsight? | Microsoft Azure"
    description="Einführung in Apache HBase in HDInsight auf Hadoop NoSQL-Datenbank erstellen. Erfahren Sie mehr über Anwendungsfälle und andere Hadoop Cluster HBase vergleichen."
    keywords="BigTable Nosql, was Hbase ist"
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
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Was ist HBase in HDInsight: ein NoSQL-Datenbank, die für Hadoop BigTable wie bereitstellt

Apache HBase ist eine Open-Source NoSQL-Datenbank, die auf Hadoop und Google BigTable nachgebildet ist. HBase bietet direkten Zugriff und starke Konsistenz für große Mengen von unstrukturierten und semistructured in einer schemalos Spalte Produktfamilien geordnet.

Daten in den Zeilen einer Tabelle und Daten innerhalb einer Zeile nach Spalte Familie gruppiert. HBase ist eine schemalos Datenbank insofern Spalten weder den Typ der gespeicherten Daten müssen vor der Verwendung definiert werden. Linear skaliert Open-Source-Code Petabyte auf Tausende von Knoten behandeln. Darauf beruhen, Datenredundanz, Batch-Verarbeitung und andere Features von verteilten Anwendung Hadoop-Ökosystem.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Wie wird HBase in Azure HDInsight implementiert?

HDInsight HBase bietet als verwalteten Cluster, die in der Azure-Umgebung integriert. Die Cluster werden zum Speichern von Daten auf Azure BLOB-Speicher mit geringer Latenz und höhere Elastizität in Leistung und Optionen konfiguriert. Dies ermöglicht Benutzern das interaktive Websites erstellen, die mit großen Datasets Services Sensor und Telemetrie Daten aus Millionen von Endpunkten zu speichern, und zum Analysieren der Daten mit Hadoop Aufträge. HBase und Hadoop sind gute Ausgangspunkte für big Data-Projekts in Azure; Insbesondere können sie Echtzeit-Applikationen mit großen Datenmengen arbeiten.

Die HDInsight-Implementierung nutzt Scale-Out-Architektur HBase automatische Sharding Tabellen starke Konsistenz für Lese- und Schreibvorgänge und automatisches Failover bereitstellen. Die Performance wird verbessert, indem im Arbeitsspeicher Zwischenspeichern für Lese- und hohem Durchsatz für Schreibvorgänge. Virtuelles Netzwerk Bereitstellung ist für HDInsight HBase. Details finden Sie unter [Bereitstellung HDInsight Cluster in Azure Virtual Network] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Wie werden Daten in HDInsight HBase verwaltet?

Daten können mit verwaltet werden in HBase die `create`, `get`, `put`, und `scan` Befehle über die Shell HBase. Daten in die Datenbank geschrieben, mit `put` und mit `get`. Die `scan` Befehl wird zum Abrufen von Daten aus mehreren Zeilen in einer Tabelle verwendet. Daten können auch mit der HBase C# API bietet eine Clientbibliothek auf HBase REST API verwaltet werden. HBase-Datenbank kann auch mit Struktur abgefragt werden. Eine Einführung in diese Programmiermodelle finden Sie unter [Erste Schritte mit HBase Hadoop in HDInsight][hbase-get-started]. Co-Prozessoren zur Verfügung stehen, ermöglichen Datenverarbeitung in den Knoten, die die Datenbank hosten.


## <a name="scenarios-use-cases-for-hbase"></a>Szenarien: Anwendungsfälle für HBase
Wurde der kanonische Anwendungsfall für die BigTable (und HBase) wurde die Suche im Web. Suchmaschinen erstellen Sie Indizes, die Webseiten Begriffe zuordnen, die sie enthalten. Aber es gibt viele andere HBase eignet Anwendungsfälle, die in diesem Abschnitt aufgeführt sind.

- Schlüssel-Wert-Speicher

    HBase kann als Schlüssel-Wert-Speicher verwendet und eignet sich für die Verwaltung von Nachrichtensysteme. Facebook HBase für ihr Messagingsystem verwendet und ist ideal für Speicherung und Management von Internetkommunikation. WebTable verwendet HBase suchen und Verwalten von Tabellen aus Webseiten.

- Sensordaten

    HBase eignet sich zum Erfassen von Daten inkrementell aus verschiedenen Quellen. Dazu gehören social Analytics Zeitreihe interaktive Dashboards Stand Trends und Indikatoren und Audit Log Systeme verwalten. Beispiele: Bloomberg Händler terminal und der geöffneten Zeitreihen-Datenbank (OpenTSDB) speichert und ermöglicht den Zugriff auf Metriken zum Zustand von Server-Systemen erfasst.

- Abfragen in Echtzeit

    [Phoenix](http://phoenix.apache.org/) ist eine SQL-Abfrage-Engine für Apache HBase. Erfolgt eine JDBC-Treiber und ermöglicht Abfragen und verwalten HBase Tabellen mit SQL.

- HBase als Plattform

    Programme können auf HBase als Datenspeicher mit ausführen. Beispiele: Phoenix, OpenTSDB, Kiji und Titan. Anträge können auch HBase integrieren. Beispiele Struktur Schwein, Solr, Sturm, Kanal, Impala, Funken, genehmigt und Drilldown.


##<a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit Hadoop in HDInsight HBase][hbase-get-started]
- [HDInsight-Cluster virtuellen Netzwerk Azure bereitstellen] [hbase-provision-vnet]
- [Konfigurieren der Replikation HBase in HDInsight](hdinsight-hbase-geo-replication.md)
- [Analysieren von Twitter Stimmung mit HBase in HDInsight][hbase-twitter-sentiment]
- [Mit der Java-Anwendung erstellen, die HDInsight (Hadoop) HBase mit Maven][hbase-build-java-maven]

##<a name="see-also"></a>Siehe auch

- [Apache HBase](https://hbase.apache.org/)
- [BigTable: Einem dezentralen System für strukturierte Daten](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
