<properties 
    pageTitle="Verwenden Sie externe Pakete mit Jupyter Notebooks in Apache Spark-Clustern auf HDInsight | Azure"
    description="Anleitung zum Konfigurieren Jupyter Notebooks verfügbar mit HDInsight Spark externe Spark-Pakete verwenden." 
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
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="nitinme"/>


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Verwenden Sie externe Pakete mit Jupyter Notebooks in Apache Spark-Clustern auf HDInsight Linux

>[AZURE.NOTE] Dieser Artikel ist für Spark 1.5.2 auf HDInsight 3.3 Spark 1.6.2 auf HDInsight 3.4. 

Informationen zum Konfigurieren von Jupyter Notebook in Apache Spark-Cluster auf HDInsight (Linux), externe, Community beigetragen Pakete nicht enthalten Out of Box im Cluster. 

Sie können das [Maven Repository](http://search.maven.org/) für die vollständige Liste der Pakete suchen verfügbar. Sie können auch eine Liste der verfügbaren Paketen aus anderen Quellen abrufen. Beispielsweise ist eine vollständige Liste der Gemeinschaft beigetragen haben Pakete auf [Spark](http://spark-packages.org/)verfügbar.

In diesem Artikel erfahren Sie, wie [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -Paket mit Jupyter Notebook.

##<a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ein HDInsight Linux Apache Spark-Cluster. Informationen finden Sie [in Azure HDInsight Cluster Apache Spark erstellen](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Verwenden Sie externe Pakete mit Jupyter notebooks 

1. [Azure-Portal](https://portal.azure.com/)aus dem Startmenü klicken Sie auf die Kachel Spark Cluster (Wenn Sie es an das Startmenü angeheftet). Sie können auch zum Cluster unter **Alle durchsuchen** > **HDInsight-Cluster**.   

2. Blatt Cluster Spark klicken Sie **Quick Links**und dann Blatt **Cluster Dashboard** auf **Jupyter Notebook**. Wenn Sie aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster.

    > [AZURE.NOTE] Sie können Jupyter Notebook für den Cluster erreichen, durch die folgende URL in Ihrem Browser öffnen. Der Name des Clusters ersetzen Sie __CLUSTERNAME__ :
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen Sie ein neues Notizbuch. Klicken Sie auf **neu**und dann auf **Spark**.

    ![Erstellen einer neuen Jupyter notebook] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Erstellen einer neuen Jupyter notebook")

3. Ein neues Notizbuch erstellt und mit dem Namen Untitled.pynb geöffnet. Klicken Sie oben auf Notebook, und geben Sie einen Anzeigenamen.

    ![Geben Sie einen Namen für das Notizbuch] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Geben Sie einen Namen für das Notizbuch")

4. Verwenden Sie die `%%configure` Magic das Notebook, um eine externe Paket konfigurieren. Notebooks, die externe Pakete verwenden, vergewissern Sie sich, rufen Sie die `%%configure` in der ersten Zelle Code Magic. Dadurch Kernel konfiguriert ist das Paket vor die Sitzung.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Sollten Sie in der ersten Zelle den Kernel konfigurieren, können Sie die `%%configure` mit dem `-f` Parameter, jedoch wird die Sitzung neu starten und alle Status verloren.

5. Im obigen Codeausschnitt `packages` erwartet wird eine Liste von Koordinaten in zentralen Repository Maven Maven. In diesem Ausschnitt `com.databricks:spark-csv_2.10:1.4.0` ist die Maven-Koordinate **Spark-Csv** -Pakets. Hier ist wie die Koordinaten für ein Paket erstellen.

    ein. Suchen Sie das Paket im Repository Maven. Für dieses Lernprogramm verwenden wir [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Aus dem Repository sammeln Sie Werte **GroupId** **ArtifactId**und **Version**.

    ![Externe Pakete mit Jupyter Notebook verwenden] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Externe Pakete mit Jupyter Notebook verwenden")

    c. Verketten von drei Werten, getrennt durch einen Doppelpunkt (****).

        com.databricks:spark-csv_2.10:1.4.0

6. Ausführen von Code Zelle der `%%configure` Magic. Das konfigurieren die zugrunde liegenden Livius Sitzung bereitgestellten Paket verwenden. In den nachfolgenden Zellen in der Arbeitsmappe jetzt können das Paket Sie wie unten dargestellt.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. Anschließend können Sie die Ausschnitte ausführen, wie dargestellt, die Daten aus dem Datensatz anzeigen im vorherigen Schritt erstellt.

        df.show()

        df.select("Time").count()


## <a name="seealso"></a>Siehe auch


* [Übersicht: Apache Spark auf Azure HDInsight](hdinsight-apache-spark-overview.md)

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

* [Jupyter auf dem Computer installieren und Verbinden mit einem HDInsight Spark-cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Ressourcen Sie für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debug Aufträge in einem Apache Spark-Cluster HDInsight](hdinsight-apache-spark-job-debugging.md)
