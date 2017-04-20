<properties 
    pageTitle="Eine Übersicht von Apache in HDInsight | Microsoft Azure" 
    description="Eine Einführung in Apache Spark in HDInsight und Szenarien, in denen Spark HDInsight in Ihrer Anwendung verwenden." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Übersicht: Apache Spark HDInsight Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a> ist ein Open Source-parallele Verarbeitung Rahmen, die Verarbeitung im Speicher zur Leistungssteigerung big Data analytische Applikationen unterstützt. Spark Verarbeitungsmodul ist für einfachen und komplexen Analytics erstellt. Spark Berechnung speicherresidenten Funktionen machen es eine gute Wahl für iterative Algorithmen Computer lernen und Graph berechnet. Spark ist auch kompatibel mit Azure BLOB-Speicher (WASB), sodass die vorhandenen Daten in Azure Spark problemlos verarbeitet werden können.

Beim Erstellen eines Clusters Spark in HDInsight erstellen Sie Azure computeressourcen mit Spark installiert und konfiguriert. Es dauert nur etwa zehn Minuten in HDInsight Spark-Cluster erstellen. Die Daten werden in Azure BLOB-Speicher gespeichert. [Mit Azure BLOB-Speicher mit HDInsight]finden Sie unter[hdinsight-storage].

![Apache Spark auf Azure HDInsight] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache Spark auf Azure HDInsight")


**Einstieg in Apache Spark auf Azure HDInsight möchten?** Siehe [Schnellstart: erstellen ein Clusters Spark auf HDInsight Linux und führen Anwendungsbeispiele mit Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Eine Liste der bekannten Probleme und mit der aktuellen Version finden Sie unter [bekannte Probleme von Apache in Azure HDInsight (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="why-use-spark-on-azure-hdinsight"></a>Wozu Spark auf Azure HDInsight? 

Azure HDInsight bietet eine vollständig verwaltete Spark. HDInsight mit Spark bietet folgende Vorteile:

| Funktion                             | Beschreibung       |
|-------------------------------------|-------------------|
| Einfache Erstellung von Clustern            | Sie können einen neuen Spark-Cluster auf HDInsight innerhalb von Minuten Azure-Verwaltungsportal, Azure PowerShell oder HDInsight .NET SDK erstellen. Einführung [in HDInsight mit Funken](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Einfache Bedienung                     | Spark in HDInsight-Cluster enthält vorkonfigurierte Jupyter Notebooks. Sie können diese interaktive Datenverarbeitung und Visualisierung. Die URLs für die https://CLUSTERNAME.azurehdinsight.net/jupyter ist. Der Name des Clusters Spark HDInsight ersetzen Sie __CLUSTERNAME__ .|
| REST-APIs                       | Spark in HDInsight enthält [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), REST-API basiert Spark Auftragsserver Remote senden und ausgeführten Aufträge zu überwachen. |
| Unterstützung für Azure See Datenspeicher | Spark auf HDInsight kann konfiguriert werden Azure See Datenspeicher als zusätzlichen Speicher. Informationen auf See Datenspeicher finden Sie unter [Übersicht über der Azure-See Datenspeicher](../data-lake-store/data-lake-store-overview.md).
| Integration in Azure | Spark auf HDInsight verfügt über einen Connector an Azure Ereignis Hubs. Kunden können Anwendungsentwicklung streaming mit Event Hubs neben [Kafka](http://kafka.apache.org/), die bereits als Teil des Spark. |
| Unterstützung für R Server  | R Server Cluster HDInsight Spark lassen verteilte R Berechnungen mit der zugesagten Spark-Cluster ausgeführt. Weitere Informationen finden Sie unter [Erste Schritte mit R Server HDInsight](hdinsight-hadoop-r-server-get-started.md).   |
| Integration mit IntelliJ IDEE  | HDInsight Plugin für IntelliJ können zu HDInsight Spark-Cluster beantragen. Weitere Informationen finden Sie unter [Verwenden HDInsight Tools-Plugin für IntelliJ IDEA Spark Anträge HDInsight Spark Linux Cluster erstellen](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Gleichzeitige Abfragen              | Spark in HDInsight unterstützt die gleichzeitige Abfragen. Dadurch können mehrere Abfragen von einem Benutzer oder mehrere Abfragen aus unterschiedlichen Benutzer und Programme die gleichen Clusterressourcen gemeinsam nutzen. |
| SSDs Zwischenspeichern                 | Sie können zum Zwischenspeichern von Daten im Speicher oder in SSDs mit den Clusterknoten verbunden. Im Arbeitsspeicher Zwischenspeichern bietet die beste abfrageleistung jedoch teuer sein kann. Zwischenspeichern in SSDs bietet eine hervorragende Option für die Verbesserung der Abfrageperformance ohne Erstellen eines Clusters mit einer Größe, die erforderlich ist, um das gesamte Dataset in den Speicher passen.|
| Integration von BI-Tools       | Spark für HDInsight bietet Connectors für BI-Tools [Macht BI](http://www.powerbi.com/) wie [Tableaus](http://www.tableau.com/products/desktop) für Datenanalyse.|
| Vorinstallierte Anaconda-Bibliotheken        | Spark auf HDInsight Cluster mit Anaconda-Bibliotheken installiert sind. [Anaconda](http://docs.continuum.io/anaconda/) bietet nahe 200 Bibliotheken für maschinelles lernen, Analyse, Visualisierung.|
| Skalierung                     | Obwohl die Anzahl der Knoten im Cluster während der Erstellung angeben können, sollten Sie vergrößert oder verkleinert den Cluster Auslastung angepasst. HDInsight-Cluster können Sie die Anzahl der Knoten im Cluster. Außerdem können Spark-Cluster gelöscht ohne Datenverlust, da alle Daten in Azure BLOB-Speicher gespeichert. |
| 24/7-Unterstützung                    | Spark auf HDInsight ist auf Unternehmensebene 24/7 Support mit einer SLA von 99,9 % Betriebszeit.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Was sind das für Spark auf HDInsight?

Apache Spark in HDInsight können die folgenden Szenarien.

### <a name="interactive-data-analysis-and-bi"></a>Interaktive Datenanalyse und BI

[Sehen Sie sich ein Lernprogramm](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark in HDInsight speichert Daten in Azure-Blobs. Business-Experten können Entscheidungsträger analysieren und Berichte über diese Daten erstellen und mit Microsoft Power BI interaktive Berichte aus den analysierten Daten erstellen. Analysten können aus unstrukturierten/halbstrukturierter Daten in Azure-Speicher gestartet, definieren Sie ein Schema für Notebooks mit Daten und Datenmodelle mit Microsoft Power BI erstellen. Spark in HDInsight unterstützt auch eine Reihe von Tools von Drittanbietern BI Tableaus, Qlikview und somit eine ideale Plattform für Datenanalysten, Business-Experten und Entscheidungsträger SAP Lumira.

### <a name="iterative-machine-learning"></a>Iterative maschinelles lernen

[Sehen Sie sich ein Lernprogramm: Vorhersagen Temperaturen Einzelhandelsabonnements HKL-Daten erstellen](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Sehen Sie sich ein Lernprogramm: Lebensmittel Ergebnisse vorherzusagen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark kommt mit [MLlib](http://spark.apache.org/mllib/), einem Computer lernen Bibliothek Spark aufbaut. Darüber hinaus auch Spark auf HDInsight Anaconda, eine Python-Verteilung mit einer Vielzahl von Paketen für maschinelles lernen. Dies mit integrierter Unterstützung für Jupyter Notebooks, und Sie haben eine der erstklassige Umgebung zum Computer Learning Applications.  

### <a name="streaming-and-real-time-data-analysis"></a>Streaming und Echtzeit-Datenanalysen

[Sehen Sie sich ein Lernprogramm](hdinsight-apache-spark-eventhub-streaming.md)

Echtzeit-Analyse ist für Szenarios von verringert die Daten Einblick durch Verarbeitung von Daten er landet echtes streaming-Lösung erstellen, verwendet. Spark in HDInsight bietet eine umfassende Unterstützung für Echtzeitanalysen Lösungen. Während Spark Connectors zum Einlesen von Daten aus zahlreichen Quellen wie Kafka, Kanal, Twitter, ZeroMQ oder TCP-Sockets bereits unterstützt Spark in HDInsight erstklassige Einnahme Daten von Azure-Ereignis. Ereignis-Hubs sind die am häufigsten verwendete Warteschlangendienst auf Azure. Eine Unterstützung für Ereignis Hubs mit Funken stellt in HDInsight eine ideale Plattform für Echtzeit Analytics Pipeline erstellen.

##<a name="next-steps"></a>Welche Komponenten sind Bestandteil des Spark-Cluster?

Spark in HDInsight umfasst die folgenden Komponenten, die auf den Clustern standardmäßig verfügbar sind.

- [Spark Core](https://spark.apache.org/docs/1.5.1/). Spark Core Spark SQL, Spark streaming-APIs, GraphX und MLlib enthält.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter Notebook](https://jupyter.org)

Spark in HDInsight bietet einen [ODBC-Treiber](http://go.microsoft.com/fwlink/?LinkId=616229) für Konnektivität Spark-Clustern in HDInsight BI-Tools wie Microsoft Power BI und Tableaus.

## <a name="where-do-i-start"></a>Wo beginne ich?

Beginnen Sie mit HDInsight Linux Spark-Cluster erstellen. Siehe [Schnellstart: erstellen ein Clusters Spark auf HDInsight Linux und führen Anwendungsbeispiele mit Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Nächste Schritte

### <a name="scenarios"></a>Szenarien

* [Spark BI: Datenanalyse interaktive BI-Tools Spark in HDInsight mit](hdinsight-apache-spark-use-bi-tools.md)

* [Spark mit Computer: Funken im HDInsight für die Analyse erstellen Temperatur HKL-Daten verwenden](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark mit Computer: Spark in HDInsight Lebensmittel Ergebnisse vorherzusagen verwenden](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Verwendung Funken im HDInsight zum Erstellen von Echtzeit-streaming](hdinsight-apache-spark-eventhub-streaming.md)

* [Websiteanalyse mit Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen der Anwendung

* [Erstellen Sie eine eigenständige Anwendung Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Führen Sie Aufträge auf einem Spark-Cluster mit Livius Remote aus](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterung

* [Verwenden Sie HDInsight Tools Plugin für IntelliJ IDEA erstellen und übermitteln Spark Scala Programme](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Mit HDInsight Tools Plugin IntelliJ Idee Remotedebugging Spark-Applikationen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Verwenden Sie Zeppelin Notebooks mit einem Cluster Spark HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Cluster-Kernels für Jupyter Notebook Spark für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Verwenden Sie externe Pakete mit Jupyter notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter auf dem Computer installieren und Verbinden mit einem HDInsight Spark-cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Ressourcen Sie für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debug Aufträge in einem Apache Spark-Cluster HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
