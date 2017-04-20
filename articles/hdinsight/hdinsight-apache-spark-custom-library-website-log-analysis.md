<properties 
    pageTitle="Mit der benutzerdefinierten Bibliotheken mit einem HDInsight Spark Website Protokolle analysieren | Microsoft Azure" 
    description="Mit benutzerdefinierten Bibliotheken mit einem HDInsight Spark Cluster Protokolle Website analysieren" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Analysieren Sie Apache Spark Cluster HDInsight Linux eine benutzerdefinierte Bibliothek mit Website-Protokolle

Dieses Notizbuch zeigt das Spark auf HDInsight eine benutzerdefinierte Bibliothek mit Daten analysieren. Benutzerdefinierte Bibliothek verwenden wir ist eine Python-Bibliothek namens **iislogparser.py**.

> [AZURE.TIP] Dieses Lernprogramm ist auch als ein Jupyter Notebook in einem Cluster Spark (Linux), den im HDInsight erstellt. Notebook-Erfahrung können Sie die Python-Ausschnitte aus dem Notizbuch selbst ausführen. Gehen Sie die Einführung in ein Notizbuch Spark-Cluster erstellen, Jupyter Notebook starten (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), und führen Sie **Analyze meldet mit Spark mit benutzerdefinierten library.ipynb** Notebook im Ordner **PySpark** .

**Komponenten:**

Sie benötigen Folgendes:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ein HDInsight Linux Apache Spark-Cluster. Informationen finden Sie [in Azure HDInsight Cluster Apache Spark erstellen](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Als ein RDD Rohdaten speichern

In diesem Abschnitt verwenden wir [Jupyter](https://jupyter.org) Notebook eines Apache Spark-Clusters in HDInsight zugeordnete Einzelvorgänge ausführen, die RAW Beispieldaten verarbeiten und speichern als Tabelle Struktur. Die Beispieldaten ist eine CSV-Datei (hvac.csv) verfügbar auf allen Clustern standardmäßig.

Wenn Ihre Daten als Tabelle Struktur gespeichert, im nächsten Abschnitt Verbinden mit BI-Tools wie Power BI und Tableaus Hive-Tabelle wir.

1. [Azure-Portal](https://portal.azure.com/)aus dem Startmenü klicken Sie auf die Kachel Spark Cluster (Wenn Sie es an das Startmenü angeheftet). Sie können auch zum Cluster unter **Alle durchsuchen** > **HDInsight-Cluster**.   

2. Blade Cluster Spark auf **Cluster-Dashboard**und klicken Sie dann auf **Jupyter Notebook**. Wenn Sie aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster.

    > [AZURE.NOTE] Sie können Jupyter Notebook für den Cluster erreichen, durch die folgende URL in Ihrem Browser öffnen. Der Name des Clusters ersetzen Sie __CLUSTERNAME__ :
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen Sie ein neues Notizbuch. Klicken Sie auf **neu**, und klicken Sie auf **PySpark**.

    ![Erstellen einer neuen Jupyter notebook] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Erstellen einer neuen Jupyter notebook")

3. Ein neues Notizbuch erstellt und mit dem Namen Untitled.pynb geöffnet. Klicken Sie oben auf Notebook, und geben Sie einen Anzeigenamen.

    ![Geben Sie einen Namen für das Notizbuch] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Geben Sie einen Namen für das Notizbuch")

4. Da Sie ein Notebook mit dem PySpark Kernel erstellt, müssen Sie keine explizit erstellen. Kontexte Spark und Struktur werden zur Ausführung von Code Zelle automatisch erstellt. Sie können zunächst Importieren von Typen, die für dieses Szenario erforderlich sind. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **Umschalt + Eingabe**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Erstellen Sie ein RDD mit dem Beispieldaten bereits im Cluster. Sie können die Daten im Speicherkonto standardmäßig den Cluster auf **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**zugreifen.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Rufen Sie ein Beispiel Protokollsatz zu überprüfen, ob den vorherigen Schritt erfolgreich ausgeführt ab.

        logs.take(5)

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Analysieren Sie Daten mithilfe einer benutzerdefinierten Python-Bibliothek

7. In der Ausgabe die ersten paar Zeilen enthalten die Headerinformationen und jede verbleibende Linie entspricht das Schema in diesen Header. Analysieren dieser Protokolle kann kompliziert sein. So verwenden Sie eine benutzerdefinierte Python-Bibliothek (**iislogparser.py**), die Analyse dieser Protokolle einfacher macht. Diese Bibliothek ist standardmäßig mit Ihr Cluster Spark auf HDInsight **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Diese Bibliothek ist jedoch nicht in die `PYTHONPATH` verwendet kann nicht mit einer importanweisung wie `import iislogparser`. Um diese Bibliothek zu verwenden, müssen wir es an die Arbeitskraft Knoten verteilen. Führen Sie den folgenden Codeausschnitt.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`Stellt eine Funktion `parse_log_line` zurückgibt `None` Log-Zeile ist eine Kopfzeile und gibt eine Instanz der `LogLine` Klasse zeilenweise Protokoll findet. Verwenden der `LogLine` Klasse nur die Protokollzeilen aus der RDD extrahieren:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Rufen Sie ein paar extrahierten Journalzeilen überprüfen, ob den Schritt erfolgreich ausgeführt ab.

        logLines.take(2)

    Die Ausgabe sollte ähnlich der folgenden:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. Die `LogLine` Klasse wiederum enthält einige nützliche Methoden wie `is_error()`, gibt zurück, ob ein Fehlercode Protokolleintrag. Hiermit berechnen die Anzahl der Fehler in extrahierten Protokollzeilen, und melden Sie alle Fehler in einer anderen Datei.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. **Matplotlib** können Sie eine Visualisierung der Daten erstellen. Wenn die Ursache der Anfragen zu isolieren, die lange ausgeführt werden soll, sollten Sie die Dateien finden, die die meiste Zeit durchschnittlich dienen.
Der folgende Codeausschnitt ruft Top 25 Ressourcen, die meisten Zeit zu einer Anforderung ab.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. Sie können auch diese Informationen in Form von Plots darstellen. Zunächst einen Plot erstellen wir zunächst eine temporäre Tabelle **AverageTime**. Die Tabelle Gruppen Protokolle von Zeit zu sehen, ob es ungewöhnlich Wartezeit Spitzen jederzeit.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Sie können die folgende SQL-Abfrage alle Datensätze in der Tabelle **AverageTime** zu führen.

        %%sql -o averagetime
        SELECT * FROM AverageTime

    Die `%%sql` Magic gefolgt von `-o averagetime` wird sichergestellt, dass die Ausgabe der Abfrage lokal auf dem Server Jupyter (in der Regel den Hauptknoten des Clusters) beibehalten wird. Die Ausgabe wird als ein [Panda](http://pandas.pydata.org/) Datensatz mit den angegebenen Namen **Averagetime**beibehalten.

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

    ![Ausgabe der SQL-Abfrage] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "Ausgabe der SQL-Abfrage")

    Weitere Informationen zu den `%%sql` Magic sowie andere mit Kernel PySpark Magie finden Sie [auf Jupyter Notebooks mit Spark HDInsight Kernel](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. Matplotlib einer Bibliothek erstellt Visualisierung von Daten können nun einen Plot erstellen. Da die Handlung von lokal gespeicherten **Averagetime** Datensatz erstellt werden muss, muss der Codeausschnitt mit beginnen die `%%local` Magic. Dies stellt sicher, dass der Code lokal auf dem Server Jupyter ausgeführt wird.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

    ![Matplotlib-Ausgabe] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Matplotlib-Ausgabe")

16. Nach der Ausführung der Anwendung sollten Sie Herunterfahren des Notebooks um die Ressourcen freizugeben. Klicken Sie dazu im Menü **Datei** auf das Notizbuch **Schließen und Anhalten**. Diese wird geschlossen und das Notizbuch schließen.
    

## <a name="seealso"></a>Siehe auch


* [Übersicht: Apache Spark auf Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Spark BI: Datenanalyse interaktive BI-Tools Spark in HDInsight mit](hdinsight-apache-spark-use-bi-tools.md)

* [Spark mit Computer: Funken im HDInsight für die Analyse erstellen Temperatur HKL-Daten verwenden](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark mit Computer: Spark in HDInsight Lebensmittel Ergebnisse vorherzusagen verwenden](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Verwendung Funken im HDInsight zum Erstellen von Echtzeit-streaming](hdinsight-apache-spark-eventhub-streaming.md)

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
