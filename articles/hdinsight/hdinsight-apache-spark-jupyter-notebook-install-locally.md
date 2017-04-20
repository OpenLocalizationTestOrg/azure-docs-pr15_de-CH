<properties 
    pageTitle="Jupyter Notebook auf dem Computer installieren und ein HDInsight Spark-Cluster an | Microsoft Azure" 
    description="Enthält Informationen zu Jupyter Notebook lokal auf Ihrem Computer zu einem Cluster Apache Spark Azure HDInsight verbinden." 
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
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Jupyter Notebook auf dem Computer installieren und Verbinden mit Apache Spark Cluster HDInsight Linux

In diesem Artikel erhalten Sie Informationen zu Jupyter Notebook mit benutzerdefinierten PySpark (für Python) und Kernel mit Spark Spark (für Scala), schließen Sie Ihr Notebook mit einem HDInsight-Cluster. Kann aus verschiedenen Gründen Jupyter auf dem lokalen Computer installiert und kann auch eine Herausforderung dar. Eine Liste der Gründe und Aufgaben finden Sie im Abschnitt am Ende dieses Artikels [Warum sollten Sie Jupyter auf dem Computer installieren](#why-should-i-install-jupyter-on-my-computer) .

Jupyter und Magic Spark auf Ihrem Computer installieren gibt drei Hauptschritte.

* Installieren Jupyter notebook
* Installieren Sie PySpark und Spark-Kernels mit Spark magic
* Konfigurieren von Funken Magie Spark-Cluster auf HDInsight zugreifen

Weitere Informationen zu benutzerdefinierten Kernel und die Spark Magic für Jupyter Notebooks mit HDInsight finden Sie unter [Kernels für Jupyter Notebooks mit Apache Spark Linux-Clustern auf HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Erforderliche Komponenten

Die hier aufgeführten Komponenten sind nicht für die Installation von Jupyter. Sind für einen HDInsight-Cluster das Notebook installierte Jupyter Notebook an.

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ein HDInsight Linux Apache Spark-Cluster. Informationen finden Sie [in Azure HDInsight Cluster Apache Spark erstellen](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter Notebook auf Ihrem Computer installieren

Python muss installiert werden, bevor Jupyter Notebooks zu installieren. Python und Jupyter stehen als Teil der [Ananconda Verteilung](https://www.continuum.io/downloads). Bei der Installation von Anaconda installieren Sie tatsächlich eine Verteilung Python. Installierte Anaconda fügen Sie Jupyter-Installation durch Ausführen eines Befehls. Dieser Abschnitt enthält Informationen, die Sie befolgen müssen.

1. Download des [Anaconda Installer](https://www.continuum.io/downloads) für Ihre Plattform, und führen Sie Setup. Sicherstellen Sie beim Ausführen des Assistenten, dass die Option die Pfadvariable Anaconda hinzu.

2. Führen Sie den folgenden Befehl Jupyter installieren.

        conda install jupyter

    Weitere Informationen zu Installting Jupyter finden Sie unter [Installieren von Jupyter mit Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Installieren Sie Kernel und Spark magic

Finden Sie Hinweise Spark Magic installiert PySpark und Spark-Kernels auf GitHub [Sparkmagic Dokumentation](https://github.com/jupyter-incubator/sparkmagic#installation) .

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Konfigurieren von Funken Magie HDInsight Spark-Cluster zugreifen

In diesem Abschnitt Konfigurieren Sie Spark-Magic, die zuvor installiert um einen Apache Spark-Cluster herstellen, die Sie in Azure HDInsight bereits erstellt sein müssen.

1. Jupyter-Konfigurationsinformationen werden normalerweise im Basisverzeichnis Benutzer gespeichert. Um jede Betriebssystemplattform Basisverzeichnis zu suchen, geben Sie die folgenden Befehle.

    Starten Sie Python-Shell. In einem Befehlsfenster Folgendes ein:

        python

    Geben Sie den folgenden Befehl Basisverzeichnis zu Python-Shell.

        import os
        print(os.path.expanduser('~'))

2. Navigieren an das Basisverzeichnis und erstellen Sie einen Ordner namens **.sparkmagic** , wenn es nicht bereits vorhanden ist.

3. Im Ordner eine Datei namens **config.json** und innerhalb die folgende JSON Ausschnitt hinzufügen.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. Ersetzen Sie **{Benutzername}**, **{CLUSTERDNSNAME}**und **{BASE64ENCODEDPASSWORD}** durch entsprechende Werte. Eine Reihe von Dienstprogrammen in Ihrer bevorzugten Programmiersprache oder online können Sie das Kennwort tatsächlich eine base64-codierte Kennwort generieren. Es wäre ein einfacher Python Ausschnitt aus der Befehlszeile ausführen:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Starten Sie Jupyter. Verwenden Sie den folgenden Befehl in der Befehlszeile.

        jupyter notebook

6. Überprüfen Sie mit Jupyter Notebook eine Verbindung herstellen können und die Verwendung der Magic Spark verfügbar mit dem Kernel. Führen Sie die folgenden Schritte aus.

    1. Erstellen Sie ein neues Notizbuch. Rechts klicken Sie auf **neu**. Standardkernel **Python2** und die zwei neuen Kernel, die Sie installieren, **PySpark** und **Funken**sollte angezeigt werden.

        ![Erstellen einer neuen Jupyter notebook] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Erstellen einer neuen Jupyter notebook")

    
        Klicken Sie auf **PySpark**.


    2. Führen Sie den folgenden Codeausschnitt.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Die Ausgabe erfolgreich abrufen kann, wird die Verbindung mit dem HDInsight-Cluster getestet.

    >[AZURE.TIP] Wenn Notebook-Konfiguration für die Verbindung zu einem anderen Cluster aktualisieren, aktualisieren Sie die config.json mit neuen Werten wie in Schritt 3 weiter oben dargestellt. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Warum sollte ich Jupyter auf meinem Computer installieren?

Zahlreiche Gründe, warum Sie auf Jupyter auf dem Computer installieren und dann zu einem Cluster Spark HDInsight möglich.

* Obwohl Jupyter Notebooks bereits in Azure HDInsight Cluster Spark verfügbar sind, bietet die Jupyter auf Ihrem Computer installieren Sie Notizbücher erstellen, testen Sie die Anwendung auf einem Cluster ausgeführt, und laden die Notebooks mit dem Cluster. Upload der Notebooks auf dem Cluster können Sie hochladen mit Jupyter Notebook, das ausgeführt wird oder im Cluster oder in den Ordner /HdiNotebooks in den Cluster Speicherkonto abspeichern. Weitere Informationen zum Speichern von Notebooks auf dem Cluster finden Sie unter [wo Jupyter Notebooks gespeichert](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Notebooks verfügbar können, basierend auf der Anwendung verschiedene Spark-Cluster herstellen.
* GitHub können ein Quellcodeverwaltungssystem implementieren und Versionskontrolle für die Notebooks. Auch möglich eine kollaborative Umgebung, mehrere Benutzer mit derselben Arbeitsmappe arbeiten können.
* Sie können mit Notebooks lokal arbeiten ohne einen Cluster einrichten. Sie benötigen nur ein Clusters Notizbücher, testen, nicht Ihre Notebooks oder eine manuell verwalten.
* Möglicherweise einfacher zu eigenen lokalen Umgebung konfigurieren, statt die Jupyter Installation konfigurieren.  Sie können die Software nutzen, die Sie lokal installiert haben, konfigurieren Sie einen oder mehrere remote-Cluster.

>[AZURE.WARNING] Mit Jupyter auf dem lokalen Computer installiert können mehrere Benutzer gleichzeitig derselben Arbeitsmappe im gleichen Spark-Cluster ausführen. In diesem Fall werden mehrere Livius Sitzung erstellt. Wenn Sie ein Problem stoßen, das Debuggen werden komplex Livius Sitzung verfolgen, welche Benutzer gehört.




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

* [Verwenden Sie externe Pakete mit Jupyter notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Ressourcen Sie für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debug Aufträge in einem Apache Spark-Cluster HDInsight](hdinsight-apache-spark-job-debugging.md)
