<properties
    pageTitle="Spark Aufträge über Livius | Microsoft Azure"
    description="Informationen Sie zum Livius mit HDInsight Spark Aufträge Remote senden."
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


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Spark Aufträge Remote auf einem Cluster Apache Spark HDInsight Linux mit Livius

Apache Spark-Cluster auf Azure HDInsight enthält Livius REST-Schnittstelle zum Senden von Aufträgen Remote zu einem Spark-Cluster. Eine ausführliche Dokumentation finden Sie unter [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Livius können interaktive Spark Schalen oder Stapelverarbeitungen auf Spark senden. Dieser Artikel spricht mit Livius Stapelverarbeitungsaufträge übermitteln. Die folgende Syntax verwendet Aufrollen REST Livius Endpunkt anrufen.

**Komponenten:**

Sie benötigen Folgendes:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ein HDInsight Linux Apache Spark-Cluster. Informationen finden Sie [in Azure HDInsight Cluster Apache Spark erstellen](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>Übermitteln eines Stapelverarbeitungsauftrags cluster

Vor dem Übermitteln eines Stapelverarbeitungsauftrags müssen Sie Anwendung Jar für den Cluster Clusterspeicher hochladen. [**AzCopy**](../storage/storage-use-azcopy.md), Befehlszeilen-Dienstprogramm können Sie tun. Es gibt viele andere Kunden, mit Daten. Sie können mehr über diese [Daten für Projekte in HDInsight Hadoop](hdinsight-upload-data.md)finden.

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Beispiele**:

* Die JAR-Datei ist für den Clusterspeicher (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Wenn Sie den JAR-Dateinamen und den Klassennamen als Teil einer Eingabedatei übergeben möchten (in diesem Beispiel input.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Informationen Sie zu im Cluster ausgeführten batches

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Beispiele**:

* Wenn Sie alle im Cluster ausgeführten Batches abrufen möchten:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Wenn Sie eine bestimmte Charge mit bestimmten BatchId abrufen möchten

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Löschen einer Stapelverarbeitung

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Beispiel**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Livius und hohe Verfügbarkeit

Livius bietet hohe Verfügbarkeit Spark Aufträge auf dem Cluster ausgeführt. Hier sind einige Beispiele.

* Der Auftrag weiterhin fällt Livius Service nach dem Absenden eines Auftrags Remote zu einem Cluster Spark, im Hintergrund ausgeführt. Livius wieder verfügbar ist, wiederhergestellt den Status des Auftrags und Berichte wieder.

* Jupyter Notebooks für HDInsight sind Livius im Backend angetrieben. Wenn ein Notebook Spark-Auftrag ausgeführt wird und Livius-Dienst neu gestartet wird, weiterhin das Notizbuch Zellen Code ausführen. 

## <a name="show-me-an-example"></a>Beispiel anzeigen

In diesem Abschnitt betrachten wir Beispiele zur Verwendung von Livius Spark Antrag, den Fortschritt der Anwendung überwachen und löschen Sie den Auftrag aus. Anwendung verwenden wir in diesem Beispiel wird im Artikel [Erstellen einer eigenständigen Anwendung Scala und auf HDInsight Spark Cluster](hdinsight-apache-spark-create-standalone-application.md)entwickelt. In den nachfolgenden Schritten wird Folgendes vorausgesetzt:

* Sie haben bereits Anwendung Jar auf den Cluster Speicherkonto kopiert.
* Sie haben CuRL installiert auf dem Computer, in dem Sie folgendermaßen versuchen.

Führen Sie die folgenden Schritte aus.

1. Lassen Sie uns zuerst, ob Livius im Cluster ausgeführt wird. Dazu können wir immer eine Liste der ausgeführten Batches. Ist dies das erste Mal einen Druckauftrag Livius arbeiten, sollte Null zurückgegeben werden.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Sie sollten eine Ausgabe ähnlich der folgenden erhalten:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Beachten Sie, wie die letzte Zeile in der Ausgabe **gesamt: 0**, sagt was keine Batches ausgeführt.

2. Lassen Sie uns jetzt übermitteln eines Stapelverarbeitungsauftrags. Der folgende Ausschnitt verwendet eine Eingabedatei (input.txt) der JAR-Name übergeben und den Klassennamen als Parameter. Dies wird empfohlen, wenn Sie Schritte aus einem Windows-Computer ausführen.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Die Parameter in der Datei **Eingabe.txt** sind wie folgt definiert:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Beachten Sie, wie die letzte Zeile der Ausgabe sagt **Zustand: ab**. Es sagt auch, **Id: 0**. ID der Stapelverarbeitung.

3. Sie können jetzt Abrufen der Status dieser bestimmten Stapel mit der Batch-ID.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Die Ausgabe zeigt jetzt **Zustand: Erfolg**anzeigt, dass der Auftrag erfolgreich abgeschlossen wurde.

4. Wenn Sie möchten, können Sie jetzt Stapel löschen.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Die letzte Zeile der Ausgabe zeigt, dass der Batch erfolgreich gelöscht wurde. Wenn Sie während der Ausführung ein Auftrags löschen, wird den Auftrag im Wesentlichen Töten. Wenn Sie einen Auftrag löschen, der andernfalls oder erfolgreich abgeschlossen hat werden die Auftragsinformationen vollständig gelöscht.

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
