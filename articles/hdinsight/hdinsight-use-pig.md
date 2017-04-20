<properties
   pageTitle="Verwenden Hadoop Schweine in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie Hadoop auf HDInsight Schwein mit."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Hadoop auf HDInsight Schwein verwenden

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Apache Schwein](http://pig.apache.org/) ist eine Plattform zum Erstellen von Programmen für Hadoop mit prozedurale Sprache *Schwein Latein*genannt. Schwein ist eine Alternative zur Java *MapReduce* Lösungen und Azure HDInsight enthalten ist.

In diesem Artikel erfahren Sie, wie Sie Schwein mit HDInsight verwenden.

##<a id="why"></a>Warum verwenden Schwein?

Eines der Probleme der Datenverarbeitung mit MapReduce in Hadoop setzt die Verarbeitungslogik mithilfe einer Zuordnung und eine Funktion. Komplexe Verarbeitung häufig müssen Sie verkettet sind mehrere MapReduce-Vorgänge verarbeiten aufteilen um das gewünschte Ergebnis zu erzielen.

Anstatt nur Karte und Funktionen reduzieren, ermöglicht Schwein als eine Reihe von Transformationen definieren, die die Daten durchläuft, um die gewünschte Ausgabe zu erzeugen.

Pig Latin-Sprache können Sie den Datenfluss von Rohdaten durch eine oder mehrere Transformationen für die gewünschte Ausgabe beschreiben. Pig Latin Programme Folgen dieses Muster:

- **Laden**: Lesen von Daten aus dem Dateisystem bearbeitet werden
- **Transformieren**: die Daten bearbeiten
- **Dump oder Shop**: Daten auf dem Bildschirm ausgegeben oder zur Verarbeitung speichern

Pig Latin unterstützt auch benutzerdefinierte Funktion (UDF), können Sie externe Komponenten aufrufen, die Logik implementieren Modell Schwein Latin schwierig.

Finden Sie weitere Informationen Pig Latin [Pig Latin Reference Manual 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) und [Pig Latin Reference Manual 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Beispielsweise Schwein mit UDFs finden Sie in folgenden Dokumenten:

* [Mit DataFu Schwein in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu ist eine Sammlung von nützlich UDFs verwaltet von Apache

* [Python Schwein mit Struktur in HDInsight verwenden](hdinsight-python.md)

* [Verwenden von C# mit Struktur und Schweine in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>Zu den Beispieldaten

Dieses Beispiel verwendet eine *log4j* -Beispieldatei, die **/example/data/sample.log** in den BLOB-Speicher-Container gespeichert werden. Jedes Protokoll in der Datei besteht aus einer Reihe von Feldern enthält eine `[LOG LEVEL]` um den Typ und Schweregrad, zum Beispiel anzuzeigen:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Im vorherigen Beispiel ist die Protokollierungsstufe Fehler.

> [AZURE.NOTE] Auch eine log4j mit [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) Protokollierungstool generieren, und Laden Sie diese Datei auf das Blob. Weitere Informationen finden Sie in der [Zu HDInsight Daten hochladen](hdinsight-upload-data.md) . Weitere Informationen zur Verwendung von Blobs in Azure-Speicher mit HDInsight finden Sie unter [Verwenden Azure BLOB-Speicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md).

Beispieldaten werden in Azure BLOB-Speicher gespeichert, die HDInsight als das Standarddateisystem für Hadoop-Cluster verwendet. HDInsight kann mit dem Präfix **Wasb** in Blobs gespeicherte Dateien zugreifen. Zugriff auf die Datei sample.log verwenden Sie z. B. die folgende Syntax:

    wasbs:///example/data/sample.log

Ist WASB der Standardspeicher für HDInsight können Sie auch die Datei mit **/example/data/sample.log** Schwein Latein zugreifen.

> [AZURE.NOTE] Die Syntax **Wasbs: / /**, verwendet, um Dateien im Speicher Standardcontainer für HDInsight Cluster zugreifen. Zusätzliche Speicherkonten wird angegeben, wenn Sie den Cluster bereitgestellt und Dateien auf diese Konten zugreifen möchten, können Sie Daten zugreifen, indem Container Kontoadresse und beispielsweise angeben: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>Zum Beispiel-Projekt

Pig Latin Einzelvorgang lädt die Datei **sample.log** aus der Standardspeicher für Ihren HDInsight. Dann führt es eine Reihe von Transformationen, die Anzahl, wie oft führen jede Ebene in den Eingabedaten aufgetreten. Die Ergebnisse werden in STDOUT ausgegeben.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Die folgende Abbildung zeigt eine Aufschlüsselung der Transformation der Daten.

![Grafisch Transformationen][image-hdi-pig-data-transformation]

##<a id="run"></a>Die Stapelverarbeitung Pig Latin

HDInsight kann mithilfe verschiedener Methoden Pig Latin Aufträge ausführen. Verwenden Sie in der folgenden Tabelle entscheiden, welche Methode Sie, und folgen Sie dem Link eine exemplarische Vorgehensweise.

| **Verwenden Sie diese Option** , wenn Sie...                                   | ... eine **interaktive** Shell | ... **Batch** -Verarbeitung | ... mit diesem **Cluster-Betriebssystem** | ... aus dem **Client-Betriebssystem** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [Aufrollen](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux oder Windows                          | Linux, Unix, Mac OS X oder Windows        |
| [.NET SDK für Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows (vorläufig)                        |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows                                  |
| [Remote Desktop](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Windows                                   | Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Ausführen von Aufträgen Schwein auf Azure HDInsight mit lokalen SQL Server Integration Services

SQL Server Integration Services (SSIS) können Sie ein Schwein Auftrag ausgeführt. Azure Feature Pack für SSIS bietet die folgenden Komponenten, die mit Schwein Aufträge HDInsight.


- [Azure HDInsight Schwein Aufgabe][pigtask]
- [Azure Abonnement Verbindungs-Manager][connectionmanager]


Weitere Informationen zu Azure Feature Pack für SSIS [hier][ssispack].


##<a id="nextsteps"></a>Nächste Schritte

Nun verwenden Sie HDInsight Schwein mit gelernt haben, die folgenden Links zu anderen Methoden zum Arbeiten mit Azure HDInsight.

* [Upload von Daten auf HDInsight][hdinsight-upload-data]
* [Struktur mit HDInsight verwenden][hdinsight-use-hive]
* [Verwenden Sie Sqoop mit HDInsight](hdinsight-use-sqoop.md)
* [Verwenden Sie Oozie mit HDInsight](hdinsight-use-oozie.md)
* [Verwenden Sie MapReduce Aufträge mit HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
