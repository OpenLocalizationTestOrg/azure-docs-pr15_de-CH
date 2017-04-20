<properties 
    pageTitle="Zeppelin Notebooks mit Spark HDInsight Linux verwenden | Microsoft Azure" 
    description="Anleitung zum Zeppelin Notebooks mit Spark HDInsight Linux verwenden." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Verwenden Sie Zeppelin Notebooks mit Apache Spark HDInsight Linux

HDInsight Spark Cluster enthalten Zeppelin Notebooks, mit denen Sie Spark Aufträge ausgeführt werden. In diesem Artikel erfahren Sie, wie Zeppelin Notebook auf einen HDInsight-Cluster verwenden.


**Komponenten:**

* Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Apache Spark-Cluster. Informationen finden Sie [in Azure HDInsight Cluster Apache Spark erstellen](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Starten Sie ein Zeppelin notebook

1. Blade Cluster Spark auf **Cluster-Dashboard**und klicken Sie dann auf **Zeppelin Notebook**. Wenn Sie aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster.

    > [AZURE.NOTE] Sie können Zeppelin Notebook für den Cluster erreichen, durch die folgende URL in Ihrem Browser öffnen. Der Name des Clusters ersetzen Sie __CLUSTERNAME__ :
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Erstellen Sie ein neues Notizbuch. Der Header-Bereich **Notebook**auf und klicken Sie dann auf **Neue Notiz erstellen**.

    ![Erstellen einer neuen Zeppelin notebook] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Erstellen einer neuen Zeppelin notebook")

    Geben Sie einen Namen für das Notizbuch, und klicken Sie dann auf **Notiz**.

3. Stellen Sie außerdem sicher, Notebook-Header zeigt Status verbunden. Es wird durch einen grünen Punkt in der rechten oberen Ecke gekennzeichnet.

    ![Zeppelin Notebook status] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Zeppelin Notebook status")

4. Laden Sie Beispieldaten in eine temporäre Tabelle. Beim Erstellen eines Clusters Spark in HDInsight wird dem Konto zugeordneten Speicher unter **\HdiSamples\SensorSampleData\hvac**Beispieldatendatei **hvac.csv**kopiert.

    Fügen Sie den leeren Absatz, der in der neuen Arbeitsmappe standardmäßig erstellt wird, im folgenden Codeausschnitt.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Drücken Sie **UMSCHALT + EINGABETASTE** , oder klicken Sie auf **die Wiedergabeschaltfläche für den Absatz den Ausschnitt ausführen** . Status des Absatzes rechts sollte Fortschreiten bereit, AUSSTEHENDE ausgeführt hat. Die Ausgabe wird unten im gleichen Absatz. Der Screenshot sieht folgendermaßen aus:

    ![Erstellen einer temporären Tabelle Rohdaten] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Erstellen einer temporären Tabelle Rohdaten")

    Sie können auch einen Titel zu jedem Absatz bereitstellen. Rechts klicken Sie auf **das Symbol** und dann auf **Titel anzeigen**.

5. Sie können jetzt Spark-SQL-Anweisungen der **HKL** -Tabelle ausführen. Fügen Sie die folgende Abfrage in einen neuen Absatz. Die Abfrage ruft Gebäude-ID sowie die Differenz zwischen dem Ziel und Temperaturen für jedes Gebäude zu einem bestimmten Zeitpunkt. Drücken Sie **UMSCHALT + EINGABETASTE**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    **% Sql** -Anweisung zu Beginn weist Notebook Livius Scala Interpreter verwendet.

    Der folgende Screenshot zeigt die Ausgabe.

    ![Führen Sie eine Spark-SQL-Anweisung des Notebooks] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Führen Sie eine Spark-SQL-Anweisung des Notebooks")

     Klicken Sie zum Wechseln zwischen unterschiedlichen Darstellungsweisen für dieselbe Ausgabe auf Anzeigeoptionen (Rechteck hervorgehoben). Klicken Sie auf **Einstellungen** wählen Sie welche stellt die Schlüssel und Werte in der Ausgabe. Die Bildschirmaufnahme über verwendet **BuildingID** als Schlüssel und den Durchschnitt der **Temp_diff** als Wert.

    
6. Sie können auch mit Variablen in der Abfrage Spark-SQL-Anweisung ausführen. Der nächste Ausschnitt zeigt wie eine Variable **Temp**in der Abfrage mit den möglichen Werten mit Abfragen möchten. Beim Ausführen der Abfrage wird eine Dropdownliste automatisch mit den Werten aufgefüllt, für die Variable angegeben.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Fügen Sie diesen Ausschnitt eines neuen Absatzes, und drücken Sie **UMSCHALT + EINGABETASTE**. Der folgende Screenshot zeigt die Ausgabe.

    ![Führen Sie eine Spark-SQL-Anweisung des Notebooks] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Führen Sie eine Spark-SQL-Anweisung des Notebooks")

    Für nachfolgende Abfragen können wählen einen neuen Wert aus der Dropdown-Liste und die Abfrage erneut auszuführen. Klicken Sie auf **Einstellungen** wählen Sie welche stellt die Schlüssel und Werte in der Ausgabe. Die Bildschirmaufnahme oben verwendet **BuildingID** als Schlüssel den Durchschnitt der **Temp_diff** als Wert und als Gruppe **Targettemp** .

7. Starten des Interpreters Livius zum Beenden der Anwendung. Hierzu öffnen Sie Interpreter Einstellungen angemeldeten Benutzernamen oben rechts klicken und dann auf **Interpreter**.

    ![Interpreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Struktur der Ausgabe")

2. Ans Livius Interpreter Einstellungen und dann auf **neu starten**.

    ![Livius Intepreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Zeppelin Intepreter starten")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Verwendung externe Pakete mit dem Notebook?

Sie können externe, Community beigetragen Pakete verwenden, die nicht enthalten Out-of-the-Box im Cluster Zeppelin Notebook in Apache Spark-Cluster auf HDInsight (Linux) konfigurieren. Sie können das [Maven Repository](http://search.maven.org/) für die vollständige Liste der Pakete suchen verfügbar. Sie können auch eine Liste der verfügbaren Paketen aus anderen Quellen abrufen. Beispielsweise ist eine vollständige Liste der Gemeinschaft beigetragen haben Pakete auf [Spark](http://spark-packages.org/)verfügbar.

In diesem Artikel sehen Sie, wie [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -Paket mit Jupyter Notebook.

1. Interpreter Einstellungen zu öffnen. Oben rechts auf den angemeldeten Benutzernamen und klicken Sie **Interpreter**.

    ![Interpreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Struktur der Ausgabe")

2. Ans Livius Interpreter Einstellungen und dann auf **Bearbeiten**.

    ![Interpreter Einstellungen ändern] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Interpreter Einstellungen ändern")

3. Fügen Sie einen neuen Schlüssel namens **livy.spark.jars.packages** und legen Sie seinen Wert im Format `group:id:version`. Also wenn Sie [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -Paket verwenden möchten, müssen festlegen den Wert des Schlüssels `com.databricks:spark-csv_2.10:1.4.0`.

    ![Interpreter Einstellungen ändern] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Interpreter Einstellungen ändern")

    Klicken Sie auf **Speichern** , und starten Sie den Interpreter Livius.

4. **Tipp**: wie ankommen soll oben eingegebenen Wert des Schlüssels ist hier wie.

    ein. Suchen Sie das Paket im Repository Maven. Für dieses Lernprogramm verwendet [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Aus dem Repository sammeln Sie Werte **GroupId** **ArtifactId**und **Version**.

    ![Externe Pakete mit Jupyter Notebook verwenden] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Externe Pakete mit Jupyter Notebook verwenden")

    c. Verketten von drei Werten, getrennt durch einen Doppelpunkt (****).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Speicherort von Zeppelin-notebooks

Zeppelin-Notebooks werden Cluster-Headnodes gespeichert. Also wenn Sie Cluster löschen, werden die Notebooks sowie gelöscht. Wenn Sie für die spätere Verwendung in anderen Clustern Notizbücher beibehalten möchten, müssen Sie sie nach Ausführung der Jobs exportieren. Zum Exportieren eines Notizbuchs klicken Sie **Exportieren** wie in der folgenden Abbildung dargestellt.

![Notizbuch herunterladen] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Downloaden Sie das Notizbuch")

Das Notizbuch wird als JSON-Datei an Ihrem Downloadspeicherort gespeichert.

## <a name="livy-session-management"></a>Livius Session-management

Beim Ausführen von Code Absatz im Notizbuch Zeppelin wird eine neue Sitzung Livius im HDInsight Spark-Cluster erstellt. Diese Sitzung ist in allen Notizbüchern Zeppelin freigegeben, die Sie später erstellen. Wenn aus irgendeinem Grund getötet Sitzung Livius (Cluster Neustart usw.), können nicht Zeppelin Notebook Aufträge aus Sie.

In diesem Fall führen Sie die folgenden Schritte vor Ausführen von Aufträgen Zeppelin Notebook. 

1. Starten des Interpreters Livius vom Zeppelin Notebook. Hierzu öffnen Sie Interpreter Einstellungen angemeldeten Benutzernamen oben rechts klicken und dann auf **Interpreter**.

    ![Interpreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Struktur der Ausgabe")

2. Ans Livius Interpreter Einstellungen und dann auf **neu starten**.

    ![Livius Intepreter starten] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Zeppelin Intepreter starten")

3. Führen Sie ein Code aus einer vorhandenen Zeppelin Notebook. Dies erstellt eine neue Livius Sitzung im HDInsight-Cluster.

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







