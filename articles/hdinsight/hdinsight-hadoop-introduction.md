 <properties
    pageTitle="Einführung in Hadoop - Was ist Hadoop auf HDInsight? | Microsoft Azure"
    description="Einführung in Hadoop, großen verteilten Datenverarbeitung und Analyse und die Komponenten des Ökosystems Hadoop in der Cloud HDInsight."
    keywords="große Datenanalyse Einführung Hadoop Was ist Hadoop Hadoop Technologie, Hadoop-Ökosystem"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Einführung in das Ökosystem Hadoop Azure HDInsight

 Dieser Artikel enthält eine Einführung in Hadoop auf Azure HDInsight, Ökosystem und big Data. Enthält Informationen Sie zu Komponenten Hadoop, allgemeine Terminologie und Szenarien für die Analyse großer Datenmengen.

## <a name="what-is-hadoop-on-hdinsight"></a>Was ist Hadoop auf HDInsight?

Hadoop bezeichnet Ökosystem Open Source-Software, die ein Framework für verteilte Verarbeitung, Speicherung und Analyse von großen Datasets auf Gruppen von Computern. Azure HDInsight ist Hadoop Komponenten aus der **Hortonworks Daten Plattform (HDP)** in der Cloud bereitgestellt und Vorschriften verwalteten Cluster mit hoher Zuverlässigkeit und Verfügbarkeit.  

Apache Hadoop war der ursprüngliche Open-Source-Projekt für große Datenverarbeitung. Folgende entwickelte verwandte Software und Dienstprogramme, die Teil der Hadoop-Technologie, einschließlich Apache Hive, Apache HBase Apache Spark und viele andere. Details finden Sie in der [Übersicht des Ökosystems Hadoop HDInsight](#overview) .

## <a name="what-is-big-data"></a>Was ist big Data?

Große Daten beschreiben alle umfangreiche digitale Informationen aus dem Text in einem Twitter-feed auf dem Sensor Industrieanlagen, Informationen durchsuchen Kunden und Verkäufe auf einer Website. Big Data kann Verlaufsdaten (d. h. Daten) oder in Echtzeit (d. h. direkt von der Quelle übertragen). Große Daten werden gesammelt in-Eskalation Volumes mit zunehmend höherer Geschwindigkeit und ein verschiedene Formate.

Für große Datenmengen funktionale Intelligenz oder Insight Sie Daten sammeln und die richtigen Fragen. Sie müssen außerdem sicherstellen, dass die Daten zugegriffen werden, bereinigt, analysiert und dann sinnvoll dargestellt. Das ist, große Datenanalyse auf Hadoop in HDInsight helfen.

## <a name="overview"></a>Übersicht über das Ökosystem Hadoop in HDInsight

HDInsight ist eine Cloud auf Microsoft Azure der schnell wachsenden Apache Hadoop-Technologie für große Datenanalyse. Implementierung von Apache Spark HBase, Sturm, Schweine, Struktur, Sqoop, Oozie, Ambari, und darüber. HDInsight kann auch mit Business Intelligence (BI) Tools wie Power BI, Excel, SQL Server Analysis Services und SQL Server Reporting Services integriert.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop, HBase Spark, Sturm und benutzerdefinierte Gruppen

HDInsight bietet Clusterkonfigurationen für Apache Hadoop, Funken, HBase oder Sturm. Oder Sie können [Cluster mit Skriptaktionen anpassen](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop**: bietet zuverlässige Datenspeicher und [bietet](#hdfs)ein einfaches Programmiermodell [MapReduce](#mapreduce) verarbeiten und Analysieren von Daten.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: eine parallele Verarbeitung, die im Arbeitsspeicher verarbeitet und steigern die Leistung der Anwendung Groß Datenanalyse unterstützt Funken Works für SQL streaming-Daten und maschinelles lernen. Siehe [Übersicht: Neuigkeiten Apache Spark in HDInsight?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: eine NoSQL Datenbank auf Hadoop, das RAM und starke Konsistenz für große Mengen von unstrukturierten und semistrukturierten Daten - möglicherweise Milliarden Zeilen bietet Mal Millionen von Spalten. Übersicht [der HBase auf HDInsight](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>**: ein verteiltes, Echtzeit-Berechnung System für große Datenströme schneller zu verarbeiten. Storm wird als verwalteten Cluster in HDInsight angeboten. [Analyse in Echtzeit Daten Sturm mit Hadoop](hdinsight-storm-sensor-data-analysis.md)anzeigen

### <a name="example-customization-scripts"></a>Anpassung von Beispielskripts

Skript-Aktionen sind Skripts, die während der Bereitstellung Cluster ausgeführt und können verwendet werden, um zusätzliche Komponenten auf dem Cluster installieren. Für Linux-basierten Clustern sind Skripts für Bash.

Die folgenden Beispielskripts werden vom HDInsight-Team bereitgestellt:

* [Farbton](hdinsight-hadoop-hue-linux.md): eine Gruppe von ASP.NET-Webanwendungen Interaktion mit einem Cluster. Nur für Linux-Clustern.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): Diagramm Modell Beziehungen Dinge oder Personen verarbeiten.

* [R](hdinsight-hadoop-r-scripts-linux.md): ein Open-Source-Sprache und Umgebung für statistische Berechnung verwendeten Computer lernen.

* [Solr](hdinsight-hadoop-solr-install-linux.md): eine unternehmensweite Suche Plattform, die Volltextsuche für Daten ermöglicht.

Informationen zur Entwicklung eigener Skriptaktionen anzeigen Sie [Skriptaktion Entwicklung mit HDInsight](hdinsight-hadoop-script-actions-linux.md)


## <a name="what-are-the-hadoop-components-and-utilities"></a>Was sind die Hadoop Komponenten und Dienstprogramme

Die folgenden Komponenten und Dienstprogrammen sind auf HDInsight Cluster enthalten.

* **[Ambari](#ambari)**: Cluster, Bereitstellung, Verwaltung, Überwachung und Dienstprogramme.

* **[Avro](#avro)** (Microsoft .NET Bibliothek Avro): Datenserialisierung für Microsoft .NET-Umgebung.

* **[Struktur & HCatalog](#hive)**: strukturierte Abfragesprache (SQL)-Abfragen und eine Tabelle und Storage Management-Ebene.

* **[Mahout](#mahout)**: für skalierbare Computer lernen Applications.

* **[MapReduce](#mapreduce)**: Legacy-Framework für Hadoop verteilten Verarbeitung und Ressourcenmanagement. [Aus](#yarn)der nächsten Generation Ressource Rahmen anzeigen

* **[Oozie](#oozie)**: Workflow-Management.

* **[Phoenix](#phoenix)**: relationale Datenbankebene über HBase.

* **[Schwein](#pig)**: einfacher Skripts für MapReduce Transformationen.

* **[Sqoop](#sqoop)**: Daten importieren und exportieren.

* **[Tez](#tez)**: datenintensive Prozesse effizient Ebene ausgeführt.

* **[GARN](#yarn)**: Hadoop Kernbibliothek und nächste Generation MapReduce Software Framework.

* **[ZooKeeper](#zookeeper)**: Koordinierung der Prozesse in verteilten Systemen.

> [AZURE.NOTE] Informationen über die Komponenten und Versionsinformationen finden Sie [Hadoop Komponenten, Versionierung und Serviceangebote in HDInsight][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari ist für Bereitstellung, Verwaltung und Überwachung von Apache Hadoop Cluster. Sie enthält eine intuitive Tools Operator und einen robusten Satz von APIs, die die Komplexität von Hadoop vereinfacht den Betrieb des Clusters. Linux-basierte HDInsight bieten Cluster sowohl die Ambari web-Benutzeroberfläche und Ambari REST API Windows-Cluster eine Teilmenge der REST API bereit. Ambari auf HDInsight-Cluster können Funktionen für plug-in.

Siehe [Verwalten HDInsight Cluster mit Ambari](hdinsight-hadoop-manage-ambari.md) (nur Linux), [Monitor Hadoop Cluster in HDInsight mit der Ambari-API](hdinsight-monitor-use-ambari-api.md)und <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari-API-Referenz</a>.

### <a name="avro"></a>Avro (Avro-Bibliothek Microsoft .NET)

Die Bibliothek Microsoft .NET Avro implementiert das Apache Avro compact Binärdaten Interchange-Format für die Serialisierung für die Umgebung Microsoft .NET. Verwendet <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> sprachunabhängig Schema definieren, die Sprachinteroperabilität abschließt, in eine andere Bedeutung in einer Sprache serialisierte Daten gelesen werden. Ausführliche Informationen zum Format finden Sie der < Ziel = _ "leer" Href = "http://avro.apache.org/docs/current/spec.html" > Apache Avro-Spezifikation</a>.
Das Format der Avro unterstützt verteilte MapReduce-Programmiermodell. Dateien sind "es", d. h. jederzeit in einer Datei suchen und aus einem bestimmten Block lesen. Wie finden Sie [Serialize Daten mit Microsoft .NET Bibliothek für Avro](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>BIETET

Hadoop verteilten Datei System bietet ist eine verteilte Dateisystem, MapReduce und GARN Core Hadoop-Ökosystem. BIETET ist der standard für Hadoop Cluster auf HDInsight.

### <a name="hive"></a>Struktur & HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> ist Data Warehouse Software auf Hadoop, mit dem Sie Abfragen und Verwalten von großen Datasets in dezentralen mithilfe einer SQL-ähnlichen Sprache namens HiveQL erstellt. Struktur wie Schwein, ist eine Abstraktion auf MapReduce. Beim Ausführen übersetzt Struktur Abfragen in einer Reihe von MapReduce-Jobs. Struktur wird konzeptionell näher zu einem Datenbank-Managementsystem Schwein und deshalb für strukturierter Daten. Unstrukturierte Daten ist Schwein besser geeignet. Finden Sie unter [Struktur Hadoop in HDInsight verwenden](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> stellt eine Tabelle und Speicher-Management für Hadoop, das eine relationale Ansicht der Daten bereitstellt. In HCatalog können Sie Dateien lesen und Schreiben in einem beliebigen Format für die Struktur SerDe (Serialisierungsprogramm Deserialisierungsprogramm) geschrieben werden können.

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> ist eine skalierbare Bibliothek Machine learning Algorithmen Hadoop ausführen. Anwendung von Statistiken mit unterrichten Computer Learning Applikationen Systeme zu Daten und nach Ergebnisse zukünftiges Verhalten bestimmen. Finden Sie unter [generieren Film Recommendations mit Mahout auf Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce ist das Framework älterer Software für Hadoop für Applikationen Prozess große Datensätze parallel Batch schreiben. MapReduce-Auftrags teilt große Datasets und organisiert Daten in Schlüssel-Wert-Paare zur Verarbeitung.

[GARN](#yarn) ist Hadoop nächsten Generation Ressource-Manager und Application Framework und MapReduce 2.0 genannt wird. MapReduce-Jobs ausführen auf aus.

Finden Sie weitere MapReduce <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> Hadoop Wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> ist ein Workflow Koordinierung, die Hadoop-Aufträge verwaltet. Es ist Hadoop Stapel integriert und unterstützt Hadoop Aufträge MapReduce, Schweine, Struktur und Sqoop. Sie können auch zum Planen von Aufträgen für ein System, wie Java-Programme oder Shell-Skripts verwendet werden. Siehe [Oozie Hadoop verwenden](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> ist eine relationale Datenbank über HBase. Phoenix enthält einen JDBC-Treiber, die Benutzern Abfragen und SQL-Tabellen direkt verwalten. Phoenix übersetzt Abfragen und anderen in systemeigenen NoSQL-API-Aufrufe - statt mit MapReduce - damit schnellere Applikationen auf NoSQL-Speicher. Finden Sie [mit Apache Phoenix und Eichhörnchen mit HBase](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Schweine
<a  target="_blank" href="http://pig.apache.org/">Apache Schwein</a> ist eine allgemeine Plattform, die komplexe MapReduce Transformationen bei großen Datasets mithilfe der einfache Skriptsprache Pig Latin ermöglicht. Schwein übersetzt Schwein lateinischen Schriften werden in Hadoop vor. Benutzerdefinierte Funktionen (UDFs) Pig Latin erweitern können. Siehe [Schwein Hadoop verwenden](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> dient, überträgt Daten zwischen Hadoop und relationalen Datenbanken einer SQL oder anderen strukturierten Datenspeicher so effizient wie möglich gebündelt. Siehe [Sqoop Hadoop verwenden](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> ist ein Anwendungsframework basiert auf Hadoop GARN, die komplexe, azyklische Diagramme allgemeine Datenverarbeitung ausführt. Es ist ein flexibler und leistungsfähiger Nachfolger MapReduce-Framework, die datenintensive Prozesse wie Struktur Ebene effizienter ausführen können. Siehe ["Mit Apache Tez für verbesserte Leistung" mit Struktur und HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>GARNE AUS
Apache GARN ist die nächste Generation von MapReduce (MapReduce 2.0 oder MRv2) und Datenverarbeitung Szenarien über MapReduce Stapelverarbeitung mit breiter skalierbar und Echtzeit-Verarbeitung unterstützt. GARN bietet Ressourcenmanagement und ein verteiltes Anwendungsframework. MapReduce-Jobs ausführen auf aus.

Finden Sie Informationen zu GARN <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (GARN)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordiniert Prozesse großer verteilter Systeme durch einen gemeinsam genutzten hierarchischen Namespace Datenregister (Znodes). Znodes enthält kleine Mengen von Metainformationen koordinieren Prozesse erforderlich: Status, Standort, Konfiguration usw..

## <a name="programming-languages-on-hdinsight"></a>Programmiersprachen auf HDInsight

HDInsight Cluster - Hadoop HBase, Sturm und Spark-Cluster - Programmiersprachen unterstützen jedoch einige nicht standardmäßig installiert. Verwenden Sie für Bibliotheken, Module oder Pakete, die nicht standardmäßig installiert Skript-Aktion, um die Komponente zu installieren. [Skript-Aktion-Entwicklung mit HDInsight](hdinsight-hadoop-script-actions-linux.md)anzeigen

### <a name="default-programming-language-support"></a>Standard programming Language support

HDInsight Cluster standardmäßig unterstützt:

* Java

* Python

Zusätzliche Sprachen können mit Skriptaktionen installiert: [Skript Aktion Entwicklung mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Java Virtual Machine (JVM) Sprachen

Viele Sprachen als Java können eine Java Virtual Machine (JVM), ausführen jedoch erfordern mit einigen dieser Sprachen im Cluster installierte zusätzliche Komponenten.

Diese JVM Sprachen werden auf HDInsight Cluster unterstützt:

* Makrosystem

* Jython (Python für Java)

* Scala

### <a name="hadoop-specific-languages"></a>Hadoop bestimmte Sprachen

HDInsight-Cluster unterstützt die folgenden Sprachen Hadoop-Ökosystem sind:

* Pig Latin Schwein Aufträge

* HiveQL Struktur und SparkSQL


## <a name="advantage"></a>Vorteile von Hadoop in der cloud

Als Teil der Azure-Cloud-Ökosystem bietet Hadoop in HDInsight eine Reihe von Vorteilen, darunter:

* Automatische Bereitstellung von Hadoop-Cluster. HDInsight-Cluster sind viel einfacher als das manuelle Konfigurieren Hadoop-Cluster erstellen. Details finden Sie unter [Bereitstellung Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

* Dem Stand Hadoop Komponenten. Näheres [Hadoop Komponenten, Versionierung und Serviceangebote in HDInsight][component-versioning].

* Hohe Verfügbarkeit und Zuverlässigkeit von Clustern. Einzelheiten finden Sie unter [Verfügbarkeit und Zuverlässigkeit von Hadoop Cluster in HDInsight](hdinsight-high-availability-linux.md) .

* Effiziente und kostengünstige Datenspeicher mit Azure BLOB-Speicher eine Option Hadoop kompatibel. Einzelheiten finden Sie unter [verwenden Azure BLOB-Speicher Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md) .

* Integration mit anderen Azure Services, einschließlich [webapps](https://azure.microsoft.com/documentation/services/app-service/web/) und [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/).

* Weitere VM-Größen und Typen für HDInsight-Cluster ausgeführt. Siehe [Hadoop Komponenten, Versionierung und Serviceangebote in HDInsight] [ component-versioning] Weitere Informationen.

* Skalierung des Clusters. Cluster-Skalierung können Sie ändern, die Anzahl der Knoten eines Clusters ausgeführt HDInsight ohne löschen oder neu erstellen.

* Virtuelle Netzwerk-Unterstützung. HDInsight-Cluster mit Azure Virtual Network dienen zur Unterstützung der Isolation Cloudressourcen oder hybridszenarien, die Cloudressourcen mit Rechenzentrum verknüpfen.

* Niedrigen Einstiegskosten. Sie [kostenlose Testversion](https://azure.microsoft.com/free/)oder wenden Sie sich an [HDInsight Preisangaben](https://azure.microsoft.com/pricing/details/hdinsight/).

Mehr Vorteile auf Hadoop in HDInsight Siehe [Seite Azure-Features für HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard und HDInsight Premium

HDInsight bietet Cloudlösungen Datenverlustvorfalls in zwei Kategorien Standard- und Premium. HDInsight Standard bietet einen unternehmensweite Cluster, die Organisationen Datenverlustvorfalls Arbeitslasten ausgeführt. HDInsight Premium, und erweiterte analytische und Sicherheitsfunktionen für HDInsight-Cluster. Weitere Informationen finden Sie unter [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>Ressourcen zu großen Datenanalyse Hadoop und HDInsight

In dieser Einführung Hadoop in der Cloud und Analyse großer Datenmengen mit Ressourcen erstellen.


### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop Dokumentation HDInsight

* [HDInsight Dokumentation](https://azure.microsoft.com/documentation/services/hdinsight/): Dokumentationsseite für Hadoop auf Azure HDInsight mit Links zu Artikeln, Videos und weitere Ressourcen.

* [Erste Schritte mit Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): ein Schnellstart-Lernprogramm HDInsight Hadoop-Cluster bereitstellen und Ausführen von Beispielabfragen Struktur.

* [Erste Schritte mit Spark in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): ein Schnellstart-Lernprogramm für Spark-Cluster erstellen und interaktive Spark SQL-Abfragen ausführen.

* [R-Server auf HDInsight verwenden](hdinsight-hadoop-r-server-get-started.md): R Server in HDInsight nutzen.

* [Bereitstellung HDInsight-Cluster](hdinsight-hadoop-provision-linux-clusters.md): erfahren Sie, wie einen HDInsight Hadoop Cluster Azure-Portal, Azure-CLI oder Azure PowerShell bereitstellen.


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: erfahren Sie mehr über Software Apache Hadoop Bibliothek ein Framework, das Gruppen von Computern für die verteilte Verarbeitung von großen Datasets ermöglicht.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">Bietet</a>: erfahren Sie mehr über Architektur und Entwurf der Hadoop DFS Primärspeicher Hadoop Applikationen verwendet.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce Tutorial</a>: erfahren Sie mehr über Programmierungsframework zum Schreiben von Hadoop Applikationen, die schnell große Datenmengen parallel in großen Clustern von Compute-Knoten zu verarbeiten.


### <a name="microsoft-business-intelligence"></a>Microsoft Business intelligence

Vertrauten Business Intelligence (BI) Tools - wie Excel, PowerPivot, SQL Server Analysis Services und SQL Server Reporting Services - abrufen, analysieren und Daten mithilfe der Add-in-Leistung-Abfrage oder Microsoft Struktur ODBC-Treiber HDInsight integriert.

Diese BI-Tools helfen bei der Analyse großer Datenmengen:

* [Verbinden Excel Hadoop Power Abfrage](hdinsight-connect-excel-power-query.md): Informationen zum Excel Azure Storage-Konto Verbinden der Daten HDInsight Cluster mit Microsoft Power Query für Excel zugeordnet. Windows Workstation erforderlich. Funktioniert mit Windows oder Linux-basierten Cluster.

* [Hadoop mit der Struktur ODBC-Treiber von Microsoft Excel verbinden](hdinsight-connect-excel-hive-odbc-driver.md): Informationen zum Importieren von Daten aus HDInsight mit Microsoft Struktur ODBC-Treiber. Windows Workstation erforderlich. Funktioniert mit Windows oder Linux-basierten Cluster.

* [Microsoft Cloud-Plattform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): erfahren Sie mehr über Power BI für Office 365, SQL Server-Testversion und SharePoint Server 2013 und SQL Server BI einrichten.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
