<properties
    pageTitle="Erstellen ein Clusters Spark HDInsight Linux und Spark SQL aus Jupyter für interaktive Analyse | Microsoft Azure"
    description="Eine schrittweise Anleitung zum Erstellen einer Apache Spark HDInsight cluster und verwenden Sie Spark SQL Jupyter Notebooks interaktive Abfragen ausführen."
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
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Erste Schritte: Apache Spark-Cluster auf HDInsight Linux erstellen und interaktive Abfragen mit Spark SQL

Informationen Sie zu einen Apache Spark-Cluster in HDInsight erstellen [Jupyter](https://jupyter.org) Notebook interactive Spark SQL-Abfragen auf Spark-Cluster ausgeführt.

   ![Erste Schritte mit Apache Spark in HDInsight] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Erste Schritte mit Apache Spark in HDInsight-Lernprogramm. Dargestellten Schritte: erstellen ein Speicherkonto; Erstellen eines Clusters. Spark-SQL-Anweisungen ausgeführt")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Bevor Sie dieses Lernprogramm beginnen, müssen Sie ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Secure Shell (SSH)-Client**: Linux, Unix und Mac OS-Systemen angegeben ein SSH-Client über die `ssh` Befehl. Für Windows-Systeme sollten [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    
- **Secure Shell (SSH) Schlüssel (optional)**: das SSH-Konto zum Verbinden mit einem Kennwort oder einen öffentlichen Schlüssel sichern. Ein Kennwort wird Ihnen schnell und sollten verwenden Sie diese Option, wenn Sie schnell einen Cluster erstellen und einige Testaufträge ausführen möchten. Mit einem Schlüssel ist sicherer, aber zusätzlichen Setup erfordert. Möglicherweise möchten diesen Ansatz verwenden, wenn einen Produktionscluster erstellen. In diesem Artikel verwenden wir den Kennwort-Ansatz. Anleitung zum Erstellen und Verwenden von SSH-Schlüssel mit HDInsight finden Sie in den folgenden Artikeln:

    -  Von einem Linux-Computer - [Verwendung SSH mit Linux-basierte HDInsight (Hadoop) von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Von einem Windows Computer - [Verwendung SSH mit Linux-basierte HDInsight (Hadoop) von Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] In diesem Artikel verwendet eine Azure Resource Manager Vorlage Spark-Cluster erstellen, [Der Azure Storage Blobs als Clusterspeicher](hdinsight-hadoop-use-blob-storage.md)verwendet. Sie können auch Spark-Cluster erstellen, [Der Azure See Datenspeicher](../data-lake-store/data-lake-store-overview.md) als einen zusätzlichen Speicher neben Azure Storage Blobs als Standardspeicher verwendet. Informationen finden Sie unter [Erstellen eines Clusters HDInsight mit dem Datenspeicher](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### <a name="access-control-requirements"></a>Steuerelement erforderlich

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Spark-Cluster erstellen

In diesem Abschnitt erstellen Sie einen HDInsight Version 3.4 Cluster (Spark Version 1.6.1) mit einer Azure-Ressourcen-Manager-Vorlage. Informationen zu Versionen von HDInsight und ihre SLAs anzeigen Sie [HDInsight Komponenten Versionen](hdinsight-component-versioning.md) Andere Cluster erstellen finden Sie unter [Erstellen HDInsight-Cluster](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicken Sie zum Öffnen der Vorlage im Azure-Portal auf folgende.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Vorlage befindet sich in einem öffentlichen Blob-Container *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*. 
   
2. Blatt Parameter geben Sie Folgendes ein:

    - **ClusterName**: Geben Sie einen Namen für den Cluster Hadoop, die Sie erstellen.
    - **Cluster-Benutzername und Kennwort**: der Standard-Anmeldename ist Admin.
    - **SSH-Benutzernamen und ein Kennwort**.
    
    Bitte notieren Sie diese Werte.  Sie benötigen sie später im Lernprogramm.

    > [AZURE.NOTE] Remotezugriff über eine Befehlszeile HDInsight-Cluster wird SSH verwendet. Benutzername und Kennwort hier wird beim Verbinden mit dem Cluster über SSH verwendet. Die SSH-Benutzername muss eindeutig auch beim Erstellen eines Benutzerkontos auf allen Clusterknoten für HDInsight. Folgende sind einige Kontonamen vorbehalten Dienste im Cluster und nicht als Benutzername SSH verwendet werden:
    >
    > Stamm, Hdiuser, Sturm, Hbase, Ubuntu, Zookeeper, bietet, Garn, Mapred, Hbase, Struktur, Oozie, Falcon, Sqoop, Admin, Tez, Hcat, Hdinsight Zookeeper.

    > Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Artikeln:

    > * [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3. Klicken Sie auf **OK** , um die Parameter zu speichern.

4. **benutzerdefinierte Bereitstellung** Blade auf **Ressourcengruppe** Dropdownfeld, und klicken Sie auf **neu** , um eine neue Ressourcengruppe erstellen. Die Ressourcengruppe ist ein Container, der Cluster abhängige Speicherkonto und andere verknüpfte Ressource gruppiert.

5. auf **rechtlich**und klicken Sie dann auf **Erstellen**.

6. Klicken Sie auf **Erstellen**. Sie sehen eine neue Tile Titel Submitting Bereitstellung Bereitstellung. Dauert es etwa 20 Minuten Cluster und SQL-Datenbank erstellen.



## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Führen Sie ein Jupyter Notebook Spark SQL-Abfragen aus

In diesem Abschnitt verwenden Sie Jupyter Notebook Spark SQL-Abfragen für Spark-Cluster ausführen. HDInsight Spark Cluster bieten zwei Kerne mit Jupyter Notebook verwenden können. Diese sind:

* **PySpark** (für Applikationen in Python geschrieben)
* **Funken** (für Applikationen geschrieben in Scala)

In diesem Artikel verwenden Sie den PySpark-Kernel. In Artikel [Jupyter Notebooks mit Spark HDInsight zur Kernel](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) erfahren Sie ausführlich über die Vorteile von PySpark-Kernel. Jedoch sind zwei Hauptvorteile von PySpark Kernel:

* Sie müssen nicht die Kontexte Spark und Struktur festgelegt. Diese werden automatisch festgelegt.
* Können Sie Zelle Magie wie `%%sql`, SQL oder Struktur Abfragen ohne vorhergehenden Codeausschnitten direkt ausführen.
* Die Ausgabe für SQL oder Struktur Abfragen wird automatisch angezeigt.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Jupyter Notebook mit PySpark Kernel erstellen 

1. [Azure-Portal](https://portal.azure.com/)aus dem Startmenü klicken Sie auf die Kachel Spark Cluster (Wenn Sie es an das Startmenü angeheftet). Sie können auch zum Cluster unter **Alle durchsuchen** > **HDInsight-Cluster**.   

2. Blade Cluster Spark auf **Cluster-Dashboard**und klicken Sie dann auf **Jupyter Notebook**. Wenn Sie aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster.

    > [AZURE.NOTE] Sie können Jupyter Notebook für den Cluster erreichen, durch die folgende URL in Ihrem Browser öffnen. Der Name des Clusters ersetzen Sie __CLUSTERNAME__ :
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen Sie ein neues Notizbuch. Klicken Sie auf **neu**, und klicken Sie auf **PySpark**.

    ![Erstellen einer neuen Jupyter notebook] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Erstellen einer neuen Jupyter notebook")

3. Ein neues Notizbuch erstellt und mit dem Namen Untitled.pynb geöffnet. Klicken Sie oben auf Notebook, und geben Sie einen Anzeigenamen.

    ![Geben Sie einen Namen für das Notizbuch] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Geben Sie einen Namen für das Notizbuch")

4. Da Sie ein Notebook mit dem PySpark Kernel erstellt, müssen Sie keine explizit erstellen. Kontexte Spark und Struktur werden zur Ausführung von Code Zelle automatisch erstellt. Sie können in diesem Szenario erforderlichen Typen importieren starten. Dazu fügen Sie den folgenden Codeausschnitt in einer Zelle und drücken Sie **UMSCHALT + EINGABETASTE**.

        from pyspark.sql.types import *
        
    Bei jedem eines Auftrags im Jupyter ausführen zeigt Web Browser Fenstertitel Status **(belegt)** und Notebook-Titel. Sie sehen auch einen ausgefüllter Kreis neben dem Text **PySpark** in der oberen rechten Ecke. Nachdem der Auftrag abgeschlossen ist, ändert dies in einen Kreis.

     ![Status eines Auftrags Jupyter notebook] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Status eines Auftrags Jupyter notebook")

4. Laden Sie Beispieldaten in eine temporäre Tabelle. Beim Erstellen eines Clusters Spark in HDInsight wird dem Konto zugeordneten Speicher unter **\HdiSamples\HdiSamples\SensorSampleData\hvac**Beispieldatendatei **hvac.csv**kopiert.

    Fügen Sie in eine leere Zelle das folgende Codebeispiel, und drücken Sie **UMSCHALT + EINGABETASTE**. Dabei werden Daten in eine temporäre Tabelle namens **HKL**registriert.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Da PySpark Kernel verwendet, Sie können jetzt direkt ausführen eine SQL-Abfrage auf die temporäre Tabelle **HKL** , das Sie gerade erstellt die `%%sql` Magic. Weitere Informationen zu den `%%sql` Magic sowie andere mit Kernel PySpark Magie finden Sie [auf Jupyter Notebooks mit Spark HDInsight Kernel](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Sobald der Auftrag erfolgreich abgeschlossen ist, wird folgende Tabellenausgabe standardmäßig angezeigt.

    ![Tabellenausgabe des Abfrageergebnisses] (./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Tabellenausgabe des Abfrageergebnisses")

    Sie sehen auch die Ergebnisse sowie andere Visualisierung. Beispielsweise würde ein Flächendiagramm dieselbe Ausgabe wie folgt aussehen.

    ![Flächendiagramm Abfrageergebnisses] (./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Flächendiagramm Abfrageergebnisses")


6. Nach der Ausführung der Anwendung sollten Sie Herunterfahren des Notebooks um die Ressourcen freizugeben. Klicken Sie dazu im Menü **Datei** auf das Notizbuch **Schließen und Anhalten**. Diese wird geschlossen und das Notizbuch schließen.

##<a name="delete-the-cluster"></a>Cluster löschen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="see-also"></a>Siehe auch


* [Übersicht: Apache Spark auf Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Spark BI: Datenanalyse interaktive BI-Tools Spark in HDInsight mit](hdinsight-apache-spark-use-bi-tools.md)

* [Spark mit Computer: Funken im HDInsight für die Analyse erstellen Temperatur HKL-Daten verwenden](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark mit Computer: Spark in HDInsight Lebensmittel Ergebnisse vorherzusagen verwenden](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Verwendung Funken im HDInsight zum Erstellen von Echtzeit-streaming](hdinsight-apache-spark-eventhub-streaming.md)

* [Websiteanalyse mit Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Application Insight Telemetrie Analyse mit Spark in HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

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


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
