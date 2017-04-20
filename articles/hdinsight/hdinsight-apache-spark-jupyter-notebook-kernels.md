<properties 
    pageTitle="Körner mit Jupyter Notebooks HDInsight Spark Linux Cluster | Microsoft Azure" 
    description="Enthält Informationen Sie zu zusätzlichen Jupyter Notebook Kernels mit Spark Cluster HDInsight Linux." 
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


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Für Jupyter Notebooks mit Apache Spark HDInsight Linux Kernels

Apache Spark-Cluster auf HDInsight (Linux) umfasst Jupyter Notebooks, mit denen Sie Ihre Anwendung testen. Ein Kernel ist ein Programm, das ausgeführt wird, und den Code interpretiert. HDInsight Spark Cluster bieten zwei Kerne mit Jupyter Notebook verwenden können. Diese sind:

1. **PySpark** (für Applikationen in Python geschrieben)
2. **Funken** (für Applikationen geschrieben in Scala)

In diesem Artikel lernen Sie wie diese Kernel und was sind die Vorteile, die aus der Verwendung.

**Komponenten:**

Sie benötigen Folgendes:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ein HDInsight Linux Apache Spark-Cluster. Informationen finden Sie [in Azure HDInsight Cluster Apache Spark erstellen](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-use-the-kernels"></a>Verwendung der Kernel 

1. [Azure-Portal](https://portal.azure.com/)aus dem Startmenü klicken Sie auf die Kachel Spark Cluster (Wenn Sie es an das Startmenü angeheftet). Sie können auch zum Cluster unter **Alle durchsuchen** > **HDInsight-Cluster**.   

2. Blade Cluster Spark auf **Cluster-Dashboard**und klicken Sie dann auf **Jupyter Notebook**. Wenn Sie aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster.

    > [AZURE.NOTE] Sie können Jupyter Notebook für den Cluster erreichen, durch die folgende URL in Ihrem Browser öffnen. Der Name des Clusters ersetzen Sie __CLUSTERNAME__ :
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen Sie ein neues Notizbuch mit neuen Kernel. Klicken Sie auf **neu**, und klicken Sie auf **Pyspark** oder **Funken**. Sie sollten Spark Kernel für Scala Applikationen und PySpark Kernel für Python-Applikationen verwenden.

    ![Erstellen einer neuen Jupyter notebook] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Erstellen einer neuen Jupyter notebook") 

3. Dies sollte ein neues Notizbuch mit dem Kernel ausgewählte öffnen.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Warum sollte PySpark oder Spark-Kernels verwenden?

Hier sind einige Vorteile der Verwendung der neuen Kernel.

1. **Voreingestellte Kontexte**. Mit **PySpark** oder mit Jupyter Notebooks **Spark** -Kernels müssen nicht die Kontexte Funken oder Struktur explizit festlegen, bevor Sie die Anwendung, die Sie entwickeln können. Diese sind standardmäßig verfügbar. Diese Kontexte sind:

    * **sc** - Spark-Kontext
    * **zur SqlContext** - Hive-Kontext


    Daher müssen Sie Anweisungen wie folgt die Kontexte festlegen:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    Stattdessen können Sie die vordefinierten Kontexte direkt in der Anwendung.
    
2. **Zelle Magie**. PySpark Kernel stellt einige vordefinierte "zaubert" sind spezielle Befehle aufrufen mit `%%` (z.B. `%%MAGIC` <args>). Magie Befehl muss das erste Wort in einer Zelle Code und können für mehrere Zeilen. Magic Word sollte das erste Wort in der Zelle. Vor der Magic auch Kommentare hinzufügen, verursachen einen Fehler.   Weitere Magie finden Sie [hier](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    Die folgende Tabelle enthält die verschiedenen Magie durch den Kernel.

  	| Magic     | Beispiel                         | Beschreibung  |
  	|-----------|---------------------------------|--------------|
  	| Hilfe      | `%%help`                            | Generiert eine Tabelle aller verfügbaren Magie Beispiel und Beschreibung |
  	| Info      | `%%info`                          | Informationen für den aktuellen Endpunkt Livius Ausgaben |
  	| Konfigurieren | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Konfiguriert die Parameter für das Erstellen einer Sitzung. Das Force-Flag (-f) ist erforderlich, wenn eine Sitzung bereits erstellt wurde und die Sitzung gelöscht und neu erstellt. Betrachten [Liviuss POST /sessions Anforderungstext](https://github.com/cloudera/livy#request-body) eine Liste gültiger Parameter. Parameter müssen als JSON-Zeichenfolge übergeben werden und müssen in der nächsten Zeile nach der Magic in der Spalte wird angezeigt. |
  	| SQL       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | Führt die SqlContext-Struktur Abfragen. Wenn die `-o` -Parameter übergeben wird, wird das Ergebnis der Abfrage im beibehalten der %% lokalen Python Kontext als [Panda](http://pandas.pydata.org/) Datensatz.   |
  	| lokale     |     `%%local`<br>`a=1`              | Der Code in den folgenden Zeilen wird lokal ausgeführt. Code muss gültige Python-Code. |
  	| Protokolle      | `%%logs`                        | Gibt die Protokolle für die aktuelle Sitzung Livius.  |
  	| Löschen    | `%%delete -f -s <session number>` | Löscht eine bestimmte Sitzung des aktuellen Livius Endpunkts. Beachten Sie, dass die Sitzung für den Kernel selbst initiiert wird gelöscht werden können. |
  	| Bereinigung   | `%%cleanup -f`                    | Löscht alle Sitzungen für den aktuellen Livius Endpunkt des Notizbuchs Sitzung. Force-Flag-f ist obligatorisch.  |

    >[AZURE.NOTE] Neben der Magie der Kernel PySpark hinzugefügt, können Sie auch [integrierte IPython Magie](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics)einschließlich `%%sh`. Sie können die `%%sh` Magic Skripts und Codeblock auf Hauptknoten Cluster ausführen. 

3. **Automatische Darstellung**. **Pyspark** Kernel visualisiert automatisch die Ausgabe Struktur und SQL-Abfragen. Sie können verschiedene einschließlich Tabelle Kreis Zeile, Bereich, Leiste Visualisierung wählen.

## <a name="parameters-supported-with-the-sql-magic"></a>Mit unterstützten Parametern der %% Magic Sql

Die %% Magic Sql unterstützt verschiedene Parameter, mit denen Sie Ausgabe steuern, die Sie erhalten, wenn Sie Abfragen ausführen. Die folgende Tabelle enthält die Ausgabe.

| Parameter     | Beispiel                         | Beschreibung  |
|-----------|---------------------------------|--------------|
| -o      | `-o <VARIABLE NAME>`                          | Mit diesem Parameter können Sie das Ergebnis der Abfrage beibehalten werden die %% lokalen Python-Kontext als [Panda](http://pandas.pydata.org/) Datensatz. Der Name der Variablen Datensatz ist der Name, den Sie angeben. |
| -q      | `-q`                          | Verwenden Sie diese Visualisierung für die Zelle deaktivieren. Möchten Sie den Inhalt einer Zelle automatisch darstellen und nur als ein Datensatz erfassen und dann `-q -o <VARIABLE>`. Visualisierung deaktivieren, ohne die Ergebnisse aufnehmen möchten (z. B. Ausführen einer SQL-Abfrage mit Nebenwirkungen wie eine `CREATE TABLE` Anweisung), verwenden `-q` ohne Angabe einer `-o` Argument. |
| -m       |  `-m <METHOD>`    | Ist der **Methode** **Übernehmen** oder **Beispiel** **(Standard ist)**. Wenn **die Methode ist,**wählt der Kernel Elemente vom Anfang der Daten Resultset MAXROWS (weiter unten in dieser Tabelle beschrieben) angegeben. Ist die Methode **Beispiel**Beispiel Kernel zufällig Elemente der Daten nach `-r` Parameter weiter in dieser Tabelle beschrieben.   |
| -r     |     `-r <FRACTION>`            | **Bruch** ist hier eine Gleitkommazahl zwischen 0,0 und 1,0. Beispielmethode für die SQL-Abfrage ist `sample`, und der Kernel zufällig angegebenen Bruchteil der Elemente des Resultsets Proben. z.B. Wenn Sie eine SQL-Abfrage mit Argumenten ausführen `-m sample -r 0.01`, dann 1 % Ergebniszeilen nach dem Zufallsprinzip beprobt werden. |
| -n      | `-n <MAXROWS>`                        | **MAXROWS** ist ein Integerwert. Der Kernel Anzahl Ausgabezeilen **MAXROWS**. Wenn **MAXROWS** negativ wie **-1**ist, wird die Anzahl der Zeilen im Resultset nicht eingeschränkt. |

**Beispiel:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

Die obige Anweisung führt Folgendes aus:

* Wählt alle Datensätze aus **Hivesampletable**.
* Da wir verwenden Sie - Q, deaktiviert es Auto-Visualisierung.
* Da wir verwenden `-m sample -r 0.1 -n 500` zufällig Beispiele für 10 % der Zeilen in der Hivesampletable und die Größe des Resultsets zu 500 Zeilen.
* Schließlich da verwendet `-o query2` speichert auch die Ausgabe in einem Datensatz **query2**aufgerufen.
    

## <a name="considerations-while-using-the-new-kernels"></a>Aspekte bei der Verwendung der neuen Kernel

Welche Kernel verwenden (PySpark oder Funken) verlassen Notebooks mit verbrauchen Clusterressourcen.  Mit diesen Kernel da Kontexte voreingestellt sind, einfach beenden die Notebooks nicht Töten den Kontext und der Clusterressourcen weiterhin verwendet werden. Sollten mit den Kerneln PySpark und Spark wäre das Notebook im Menü **Datei** die Option **Schließen und Anhalten** . Dies bricht den Kontext und beendet das Notizbuch.    


## <a name="show-me-some-examples"></a>Beispiele anzeigen

Wenn Sie ein Jupyter Notebook öffnen, sehen Sie zwei Ordner auf Stammebene.

* Der Ordner **PySpark** hat Beispielnotizbücher, mit dem neuen **Python** -Kernel.
* **Scala** Ordner hat Beispielnotizbücher, mit dem neuen **Spark** -Kernel.

**00 - [lesen Sie zuerst] Spark Magic Kernel Features** Notizbuch öffnen Sie im Ordner **PySpark** oder **Funken** erfahren die anderen Magie. Auch können andere verfügbare Beispielnotizbücher unter zwei Ordner Szenarien mit Jupyter Notebooks mit HDInsight Spark zu lernen.

## <a name="where-are-the-notebooks-stored"></a>Speicherort von den notebooks

Jupyter Notebooks werden das Speicherkonto den Cluster im Ordner **/HdiNotebooks** gespeichert.  Notebooks, Dateien und Ordner, die in Jupyter erstellen zugänglich von WASB.  Beispielsweise verwenden Sie Jupyter Ordner **Myfolder** und Notebook- **myfolder/mynotebook.ipynb**erstellen, Sie können zugreifen, Notizbuch `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  Umgekehrt gilt, d. h. wenn ein Notebook direkt zu Ihrem Speicherkonto hochladen `/HdiNotebooks/mynotebook1.ipynb`, das Notebook wird auch von Jupyter.  Notebooks bleibt im Speicherkonto auch wenn Cluster gelöscht wird.

Das Speicherkonto Notebooks gespeichert werden wie ist bietet vereinbar. Also, wenn Sie SSH im Cluster verwendeten Befehle wie folgt speichern:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


Bei Probleme beim Zugriff auf das Speicherkonto für Cluster, die Notebooks werden auch auf den Hauptknoten `/var/lib/jupyter`.

## <a name="supported-browser"></a>Unterstützte browser
Jupyter Notebooks gegen HDInsight Spark-Cluster sind nur auf Google Chrome unterstützt.

## <a name="feedback"></a>Feedback

Neuen Kernel in Phase entwickelt und werden mit der Zeit Reifen. Dies kann auch bedeuten, dass APIs wie diese Kernel Reifen ändern. Wir freuen uns Ihr Feedback, die Sie beim Verwenden dieser neuen Kernel. Dies wird bei der Gestaltung der endgültigen Version dieser Kernel sehr nützlich sein. Kommentare/Feedback lassen Sie am Ende dieses Artikels im Abschnitt **Kommentare** .


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

* [Verwenden Sie externe Pakete mit Jupyter notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter auf dem Computer installieren und Verbinden mit einem HDInsight Spark-cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Ressourcen Sie für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debug Aufträge in einem Apache Spark-Cluster HDInsight](hdinsight-apache-spark-job-debugging.md)
