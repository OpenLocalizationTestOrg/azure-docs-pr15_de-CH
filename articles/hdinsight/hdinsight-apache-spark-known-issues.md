<properties 
    pageTitle="Bekannte Probleme von Apache in HDInsight | Microsoft Azure" 
    description="Bekannte Probleme in HDInsight von Apache." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Bekannte Probleme bei Apache Spark Cluster HDInsight Linux

Dieses Dokument verfolgt von alle bekannten Probleme für die HDInsight Spark public Preview.  

##<a name="livy-leaks-interactive-session"></a>Livius Speicherverluste interaktive Sitzung
 
Neustart Livius mit einer interaktiven Sitzung (Ambari oder durch Hauptknoten 0 virtuellen Neustart) noch aktiv ist, wird eine interaktive Arbeit Sitzung Verluste auftreten. Deshalb Arbeitsplätze können im Zustand angenommen fest und können nicht gestartet werden.

**Ausgleich:**

Gehen Sie zur Umgehung des Problems:

1. SSH in Hauptknoten. 
2. Führen Sie den folgenden Befehl zu Anwendung IDs interaktive Livius gestartete Einzelvorgänge. 

        yarn application –list

    Der Standard-Auftrag Namen Livius gestarteten Einzelvorgänge Livius interaktive Sitzung ohne explizite Namen angegebene Jupyter Notebook für Livius Sitzung gestartet werden, beginnt der Auftragsname mit Remotesparkmagics_ *. 

3. Führen Sie den folgenden Befehl die Aufträge zu. 

        yarn application –kill <Application ID>

Neue Aufträge werden gestartet. 

##<a name="spark-history-server-not-started"></a>Spark Geschichte Server wurde nicht gestartet 

Spark Geschichte Server wird nicht automatisch gestartet, nachdem ein Cluster erstellt wird.  

**Ausgleich:** 

Starten Sie aus Ambari Geschichte Server manuell.

## <a name="permission-issue-in-spark-log-directory"></a>Problem mit Benutzerberechtigungen im Protokollverzeichnis Funken 

Wenn Hdiuser bei Spark-submit sendet, wird ein Fehler java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Zugriff verweigert) sowie die Treiber nicht geschrieben. 

**Ausgleich:**
 
1. Hadoop Gruppe Hdiuser hinzufügen. 
2. Bieten Sie 777 Berechtigungen auf /var/log/spark nach Erstellung des Clusters. 
3. Aktualisieren der Spark-Standort mit Ambari ein Verzeichnis mit 777 Berechtigungen.  
4. Ausführen Spark-übermitteln als Sudo.  

## <a name="issues-related-to-jupyter-notebooks"></a>Probleme im Zusammenhang mit Jupyter notebooks

Es folgen einige bekannte Probleme im Zusammenhang mit Jupyter Notebooks.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Notebooks mit ASCII-Zeichen in Dateinamen

Spark HDInsight-Cluster verwendet werden können Jupyter-Notebooks müssen nicht nur ASCII-Zeichen in Dateinamen. Beim Hochladen einer Datei durch Jupyter UI mit nicht-ASCII-Dateinamen schlägt im Hintergrund (d. h. Jupyter lässt Sie die Datei hochladen, aber es wird nicht sichtbaren Fehler entweder werfen). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Fehler beim Laden des Notebooks größere

Fehler sehen **`Error loading notebook`** beim Laden des Notebooks größer sind.  

**Ausgleich:**

Wenn Sie diese Fehlermeldung erhalten, bedeutet dies nicht, dass Ihre Daten beschädigt oder verloren.  Notizbücher sind immer noch auf dem Datenträger in `/var/lib/jupyter`, und SSH im Cluster darauf zugreifen können. Sie können Notizbücher vom Cluster auf dem lokalen Computer (mit SCP oder WinSCP) als Sicherung für wichtigen Daten im Notizbuch verlieren. Anschließend können Sie SSH-Tunnel in der Hauptknoten Hafen 8001 Jupyter Zugriff auf das Gateway.  Dort können die Ausgabe des Notizbuchs deaktivieren und erneut speichern, um das Notizbuch zu minimieren.

Um diesen Fehler in Zukunft verhindern, führen Sie einige bewährte Methoden:

* Es ist wichtig, Notebook klein zu halten. Jede Ausgabe Ihre Spark-Aufträge, die an Jupyter gesendet wird in der Arbeitsmappe beibehalten.  Es empfiehlt sich mit Jupyter im Allgemeinen zu vermeiden `.collect()` auf große RDD oder Dataframes; Wenn ein RDD Inhalt einsehen möchten, stattdessen ausführen `.take()` oder `.sample()` damit das Ergebnis zu groß wird.
* Auch wenn Sie eine Arbeitsmappe speichern, Ausgabe deaktivieren alle Zellen, um die Größe zu verringern.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Notebook-Starts dauert länger als erwartet 

Erste Anweisung im Jupyter Notebook Funken Magie kann mehr als eine Minute dauern.  

**Erklärung:**
 
Dies geschieht, weil die Ausführung der ersten Code-Zelle. Im Hintergrund initiiert diese Sitzungskonfiguration Spark, SQL und Struktur Kontext festgelegt. Nach Kontexten festgelegt, die erste Anweisung ausführen und dadurch der Eindruck, dass entsteht die Anweisung eine lange gedauert abgeschlossen.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter Notebook-Timeout die Sitzung erstellen

Bei Spark Cluster Ressourcen werden Spark und Pyspark Kernel Jupyter Notebook Timeout die Sitzung erstellen möchten. 

**Gegenmaßnahmen:** 

1. Geben Sie einige Ressourcen im Cluster Spark durch:

    - Notebooks Spark beendet schließen und Anhalten im Menü, oder klicken Sie auf Herunterfahren im Notebook-Explorer.
    - Beenden andere Spark Anträge aus.

2. Starten Sie das Notizbuch, das Sie starten möchten. Genügend Ressourcen sollten Sie jetzt eine Sitzung erstellt werden.

##<a name="see-also"></a>Siehe auch

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

* [Verwenden Sie externe Pakete mit Jupyter notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter auf dem Computer installieren und Verbinden mit einem HDInsight Spark-cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Ressourcen Sie für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debug Aufträge in einem Apache Spark-Cluster HDInsight](hdinsight-apache-spark-job-debugging.md)
