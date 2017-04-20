<properties 
    pageTitle="Mit der Apache Spark Machine Learning Anwendung auf HDInsight | Microsoft Azure" 
    description="Anleitung zur Verwendung von Notebooks mit Apache Spark Machine Learning Applikationen erstellen" 
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
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Anwendungsentwicklung für maschinelles Lernen auf Apache Spark-Clustern auf HDInsight Linux

Informationen Sie zum Erstellen eines Anwendung mit einem Apache Spark-Cluster in HDInsight Computers. Dieser Artikel veranschaulicht, wie mit der verfügbaren Jupyter Notebook mit dem Erstellen und Testen der Anwendung. Die Anwendung verwendet die Beispiel HVAC.csv Daten auf allen Clustern standardmäßig.

**Komponenten:**

Sie benötigen Folgendes:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ein HDInsight Linux Apache Spark-Cluster. Informationen finden Sie [in Azure HDInsight Cluster Apache Spark erstellen](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>Daten anzeigen

Vor dem Beginn der Anwendung verstehen Sie wir die Struktur der Daten und der Art der Analyse, die wir für die Daten durchführen. 

In diesem Artikel verwenden wir die **HVAC.csv** Daten Beispieldatei Azure Storage-Konto zur Verfügung, die Sie mit dem HDInsight verknüpft. In das Speicherkonto ist die Datei **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Downloaden Sie und öffnen Sie die CSV-Datei um eine Momentaufnahme der Daten.  

![HKL-Daten-snapshot] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "Snapshot der HKL-Daten")

Die Daten zeigen die gewünschte Temperatur und die Temperatur ein Gebäude Hvac-Systeme installiert. Angenommen **die Systemspalte** stellt die System-ID und **SystemAge** Spalte die Anzahl der Jahre HKL-System auf das Gebäude.

Wir verwenden diese Daten vorherzusehen, ob ein Gebäude wärmer oder kälter basierend auf die gewünschte Temperatur ein System-ID und System Alter werden.

##<a name="app"></a>Schreiben einer Maschine Learning Anwendung Spark MLlib

In dieser Anwendung verwenden wir eine Pipeline Spark ML eine Klassifizierung von Dokumenten ausführen. In der Pipeline wir Teilen des Dokuments in Wörter Wörter in einen Vektor numerische Funktion konvertieren und schließlich ein Vorhersagemodell mithilfe der Funktion Vektoren und Etiketten erstellen. Führen Sie die folgenden Schritte aus, um die Anwendung zu erstellen.

1. [Azure-Portal](https://portal.azure.com/)aus dem Startmenü klicken Sie auf die Kachel Spark Cluster (Wenn Sie es an das Startmenü angeheftet). Sie können auch zum Cluster unter **Alle durchsuchen** > **HDInsight-Cluster**.   

2. Blade Cluster Spark auf **Cluster-Dashboard**und klicken Sie dann auf **Jupyter Notebook**. Wenn Sie aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster.

    > [AZURE.NOTE] Sie können Jupyter Notebook für den Cluster erreichen, durch die folgende URL in Ihrem Browser öffnen. Der Name des Clusters ersetzen Sie __CLUSTERNAME__ :
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen Sie ein neues Notizbuch. Klicken Sie auf **neu**, und klicken Sie auf **PySpark**.

    ![Erstellen einer neuen Jupyter notebook] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Erstellen einer neuen Jupyter notebook")

3. Ein neues Notizbuch erstellt und mit dem Namen Untitled.pynb geöffnet. Klicken Sie oben auf Notebook, und geben Sie einen Anzeigenamen.

    ![Geben Sie einen Namen für das Notizbuch] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Geben Sie einen Namen für das Notizbuch")

3. Da Sie ein Notebook mit dem PySpark Kernel erstellt, müssen Sie keine explizit erstellen. Kontexte Spark und Struktur werden zur Ausführung von Code Zelle automatisch erstellt. Sie können zunächst Importieren von Typen, die für dieses Szenario erforderlich sind. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **Umschalt + Eingabe**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. Sie müssen jetzt Laden der Daten (hvac.csv), analysieren und zum Trainieren des Modells verwendet. Hierzu definieren Sie eine Funktion, die überprüft, ob die Temperatur des Gebäudes zieltemperatur größer ist. Die Temperatur ist größer, Gebäude heiß, gekennzeichnet durch den Wert **1.0**. Die Temperatur ist weniger das Gebäude kalt Wert **0,0**gekennzeichnet. 

    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Konfigurieren die Pipeline Spark Computer lernen, der drei Phasen besteht: Tokenizer, HashingTF und Lr. Weitere Informationen über Rohrleitung und ihre Funktionsweise finden Sie unter <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark maschinelles lernen Pipeline</a>.

    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Anpassen der Pipeline Training Dokument. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        model = pipeline.fit(training)

7. Überprüfen des Dokuments Training Checkpoint den Status der Anwendung. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        training.show()

    Diese geben die Ausgabe ähnelt der folgenden:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Zurück und überprüfen Sie die Ausgabe mit der unformatierten CSV-Datei. Beispielsweise enthält die erste Zeile der CSV-Datei diese Daten:

    ![HKL-Daten-snapshot] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "Snapshot der HKL-Daten")

    Beachten Sie, dass die Temperatur kleiner als das Ziel Temperatur hindeutet kalt ist. Deshalb Ausgabe Training **Beschriftung** in der ersten Zeile der Wert **0,0**bedeutet, dass das Gebäude nicht heiß ist.

8.  Vorbereiten einer Datengruppe ausgebildete Modell gegen ausführen. Dazu übergeben wir System-ID und System Alter (bezeichnet als **SystemInfo** Training Ausgabe) und Modell würde sagen, ob mit dieser System-ID und System Alter heißer wäre (gekennzeichnet durch 1.0) oder Kühler (gekennzeichnet durch 0.0).

    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Schließlich Vorhersagen Sie auf die Testdaten. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    Aus der ersten Zeile in der Vorhersage Sie können sehen, dass bei einem System der HKL-System Alter von 25 Jahren mit ID 20 Gebäude hot (**Vorhersage = 1,0**). Der erste Wert für DenseVector (0.49999) entspricht der Vorhersage 0,0 und Vorhersage 1.0 entspricht der zweite Wert (0.5001). In der Ausgabe, obwohl der zweite Wert nur unwesentlich höher ist das Modell zeigt **Vorhersage = 1,0**.

11. Nach der Ausführung der Anwendung sollten Sie Herunterfahren des Notebooks um die Ressourcen freizugeben. Klicken Sie dazu im Menü **Datei** auf das Notizbuch **Schließen und Anhalten**. Diese wird geschlossen und das Notizbuch schließen.
           

##<a name="anaconda"></a>Verwenden Sie Anaconda Scikit-Bibliothek für maschinelles lernen lernen

Apache Spark-Cluster auf HDInsight enthalten Bibliotheken Anaconda. Auch die **Scikit-Informationen** Bibliothek für maschinelles lernen. Die Bibliothek enthält auch verschiedene Datensätze, mit denen Sie Beispiel Anwendung direkt von einem Jupyter Notebook. Beispiele zum Verwenden der Scikit-Bibliothek erfahren, finden Sie unter [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Siehe auch

* [Übersicht: Apache Spark auf Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Spark BI: Datenanalyse interaktive BI-Tools Spark in HDInsight mit](hdinsight-apache-spark-use-bi-tools.md)

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


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
