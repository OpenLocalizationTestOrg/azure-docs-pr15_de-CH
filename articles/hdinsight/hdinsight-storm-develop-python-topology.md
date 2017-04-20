<properties
   pageTitle="Verwenden von Python-Komponenten in einer Topologie Sturm HDinsight | Microsoft Azure"
   description="Verwendung von Python-Komponenten mit Apache auf Azure HDInsight lernen. Sie lernen Python Komponenten aus beiden Java basiert und Makrosystem basierten Storm-Topologie."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Entwickeln von HDInsight werden Python mit Apache Storm-Topologien

Apache Storm unterstützt mehrere Sprachen, auch wenn Sie Komponenten in mehreren Sprachen in einer Topologie. In diesem Dokument erfahren Sie, wie Python Komponenten in Ihre Topologien Java und Sturm Makrosystem basiert auf HDInsight.

##<a name="prerequisites"></a>Erforderliche Komponenten

* Python 2.7 oder höher

* Java JDK 1.7 oder höher

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Unterstützung mehrerer Sprachen Sturm

Storm soll Komponenten mit jeder Programmiersprache geschrieben, aber dies erfordert, dass die Komponenten zum Arbeiten mit der [Sparsamkeit Definition Sturm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift)kennen. Ein Modul dient für Python Apache Storm-Projekt, das problemlos mit Benutzeroberfläche ermöglicht. Dort finden Sie unter [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Da Apache Storm Java-Prozess, der auf der Java Virtual Machine (JVM) ausgeführt wird ist, sind in anderen Sprachen geschriebene Komponenten als Unterprozesse ausgeführt. Storm Bits im JVM kommuniziert mit diese Unterprozesse JSON Nachrichten über Stdin-Stdout gesendet. Weitere Informationen zur Kommunikation zwischen Komponenten finden in der Dokumentation zu [Lang Multi-Protokoll](https://storm.apache.org/documentation/Multilang-protocol.html) .

###<a name="the-storm-module"></a>Storm-Modul

Storm-Modul (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py) bietet die Python-Komponenten erstellen, die mit musste.

Auf diese Weise beispielsweise `storm.emit` Tupel ausgeben und `storm.logInfo` die Protokolle schreiben. Ich empfehle Ihnen diese Datei lesen und verstehen, was bietet.

##<a name="challenges"></a>Herausforderung

__Storm.py__ -Modul können Sie Python Ausläufe erstellen, die Daten und Muttern, die Prozessdaten, beanspruchen, jedoch insgesamt Sturm Topologie Definition, die Kommunikation zwischen Komponenten verbindet mit Java oder Makrosystem noch geschrieben wird. Darüber hinaus verwenden Java müssen Sie auch Java-Komponenten erstellen, die als Schnittstelle zu den Python-Komponenten.

Auch da Sturm Cluster verteilt ausgeführt, müssen Sie sicherstellen, Python-Komponenten erforderlichen Module auf alle workerknoten im Cluster verfügbar sind. Storm bietet keine einfache Möglichkeit, dies für Multi-Lang Ressourcen - Sie entweder alle Abhängigkeiten als Teil der JAR-Datei für die Topologie, oder manuell Abhängigkeiten auf jede Arbeitskraft Knoten im Cluster installieren.

###<a name="java-vs-clojure-topology-definition"></a>Java im Vergleich zu Makrosystem Topologie definition

Die zwei Methoden zum Definieren einer Topologie ist Makrosystem bei weitem die einfachste/saubersten wie direkt wesentlich Python-Komponenten in der Definition der Topologie. Java-basierte Topologie Definitionen definieren Sie auch Java-Komponenten, die Dinge wie Felder in Tupel Deklarieren von Python-Komponenten behandelt.

Beide Methoden werden in diesem Dokument sowie Beispiele beschrieben.

##<a name="python-components-with-a-java-topology"></a>Python-Komponenten mit einer Java-Topologie

> [AZURE.NOTE] In diesem Beispiel steht unter [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) im Verzeichnis __JavaTopology__ . Dies ist ein Projekt Maven basiert. Wenn Sie mit Maven nicht vertraut sind, finden Sie weitere Informationen zum Erstellen eines Maven für eine Storm-Topologie [entwickeln Java-basierte Topologien mit Apache auf HDInsight](hdinsight-storm-develop-java-topology.md) .

Eine Java-basierte Topologie, die Python (oder andere Sprachkomponenten JVM) verwendet erscheint zunächst Java-Komponenten verwenden. aber wenn aller Java Ausläufe/Schrauben suchen sehen Sie ähnlichen Code wie den folgenden:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

Hier wird Java ruft Python und führt das Skript, das die tatsächliche Bolzen enthält. Java-Ausläufe/Schrauben (beispielsweise) deklarieren Sie einfach die Felder im Tupel von der zugrunde liegenden Python-Komponente ausgegeben werden.

Die Python-Dateien befinden sich der `/multilang/resources` in diesem Verzeichnis. Die `/multilang` in __pom.xml__Directory verwiesen wird:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${Basedir} / Mehrsprachig</directory>
    </resource>
</resources>

Dies schließt alle Dateien in den `/multilang` Ordner im Glas aus diesem Projekt.

> [AZURE.IMPORTANT] Hinweis nur gibt die `/multilang` Verzeichnis nicht `/multilang/resources`. Storm erwartet-JVM-Ressourcen in einem `resources` Verzeichnis so intern bereits gesucht. Platzieren von Komponenten in diesem Ordner können Sie nur Verweis namentlich im Java-Code. Z. B. `super("python", "countbolt.py");`. Davon werden Sturm sieht die `resources` Verzeichnis als Root (/), wenn mehrere Lang Ressourcen zugreifen.
>
> Für dieses Projekt wird die `storm.py` Modul befindet sich auf der `/multilang/resources` Verzeichnis.

###<a name="build-and-run-the-project"></a>Erstellen und Ausführen des Projekts

Führen Sie das Projekt lokal nur mit Maven Folgendes erstellen und im lokalen Modus ausführen:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Verwenden Sie STRG + c zum Beenden dieses Prozesses.

Um das Projekt auf einen HDInsight-Cluster unter Apache Storm bereitstellen, gehen Sie folgendermaßen vor:

1. Erstellen Sie ein Glas Uber:

        mvn package

    Dies erstellt eine Datei mit dem Namen __WordCount - 1.0-SNAPSHOT.jar__ in der `/target` für dieses Projekt.

2. Laden Sie die JAR-Datei Hadoop Cluster mithilfe einer der folgenden Methoden:

    * Für __Linux-basierte__ HDInsight Cluster: mit `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` Cluster Clustername HDInsight Benutzername mit Ihrem SSH-Benutzernamen CLUSTERNAME ersetzen die JAR-Datei kopieren.

        Nach Abschluss der Upload mit SSH verbinden und die Topologie verwenden`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * Für __Windows-basierte__ HDInsight Cluster: Storm-Dashboard wird HTTPS://CLUSTERNAME.azurehdinsight.net/ in Ihrem Browser verbinden. Der Clustername HDInsight CLUSTERNAME ersetzen und den Administratornamen und Kennwort ein.

        Mithilfe des Formulars die folgenden Aktionen durchführen:

        * __JAR-Datei__: Wählen Sie __Durchsuchen__, und wählen Sie die __WordCount-1.0-SNAPSHOT.jar__ -Datei
        * __Klassenname__: eingeben`com.microsoft.example.WordCount`
        * __Zusätzliche Parameter__: Geben Sie einen Namen wie `wordcount` zu der Topologie

        Wählen Sie die Topologie zu __Senden__ .

> [AZURE.NOTE] Nach dem Start führt Storm-Topologie weiterspielen (getötet.) Um die Topologie zu beenden, verwenden Sie die `storm kill TOPOLOGYNAME` Befehl über die Befehlszeile (SSH-Sitzung auf einem Linux-Cluster beispielsweise) Storm-Benutzeroberfläche verwenden, wählen Sie die Topologie und wählen Sie die Schaltfläche __Abbrechen__ .

##<a name="python-components-with-a-clojure-topology"></a>Python-Komponenten mit einer Makrosystem-Topologie

> [AZURE.NOTE] In diesem Beispiel steht unter [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) im Verzeichnis __ClojureTopology__ .

Diese Topologie wurde mit [Leiningen](http://leiningen.org) erstellt [ein neues Makrosystem-Projekt](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project)erstellt. Danach wurden die folgenden erstellten Projekt geändert:

* __Project.CLJ__: hinzugefügte Abhängigkeiten Sturm und Ausschlüsse für Elemente, die möglicherweise ein Problem mit dem HDInsight-Server bereitgestellt.
* __Ressourcen-Ressourcen__: Leiningen erstellt ein `resources` Verzeichnis die Dateien angezeigt, hinzugefügt zum Stamm der Jar-Datei dieses Projekts erstellt und Storm erwartet Dateien in einem Unterverzeichnis mit dem Namen `resources`. Ein Verzeichnis wurde hinzugefügt und die Python-Dateien befinden sich im `resources/resources`. Zur Laufzeit wird dies als Root (/) behandelt auf Python-Komponenten.
* __src/WordCount/Core.CLJ__: Diese Datei enthält die Definition der Topologie und der __project.clj__ -Datei verwiesen wird. Weitere Informationen über Makrosystem Storm-Topologie definieren finden Sie unter [Makrosystem DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

###<a name="build-and-run-the-project"></a>Erstellen und Ausführen des Projekts

__Erstellen und Ausführen des Projekts lokal__, verwenden Sie folgenden Befehl:

    lein clean, run

Um die Topologie zu beenden, verwenden Sie __STRG + C__.

__Erstellen einer Uberjar und HDInsight bereitstellen__, gehen Sie folgendermaßen vor:

1. Erstellen Sie eine Uberjar mit der Topologie erforderliche Abhängigkeit:

        lein uberjar

    Dadurch entsteht eine neue Datei namens `wordcount-1.0-SNAPSHOT.jar` in der `target\uberjar+uberjar` Verzeichnis.
    
2. Verwenden Sie eine der folgenden Methoden bereitstellen und Ausführen der Topologie mit einem HDInsight-Cluster:

    * __Linux-basierte HDInsight__
    
        1. Kopieren Sie die Datei HDInsight Cluster Head-Knoten mit `scp`. Zum Beispiel:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            Ein Benutzer SSH für Cluster und CLUSTERNAME mit der Clustername HDInsight ersetzen Sie Benutzernamen.
            
        2. Nach dem Kopieren der Datei auf dem Cluster verwenden SSH zum Cluster herstellen und den Auftrag. Informationen über SSH mit HDInsight finden Sie Folgendes:
        
            * [Verwenden von SSH mit Linux-basierte HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [Verwenden von SSH mit Linux-basierte HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Nachdem die Verbindung hergestellt ist, anhand der folgenden Topologie starten:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __Windows-basierte HDInsight__
    
        1. Eine Verbindung zu Storm-Dashboard wird HTTPS://CLUSTERNAME.azurehdinsight.net/ in Ihrem Browser. Der Clustername HDInsight CLUSTERNAME ersetzen und den Administratornamen und Kennwort ein.

        2. Mithilfe des Formulars die folgenden Aktionen durchführen:

            * __JAR-Datei__: Wählen Sie __Durchsuchen__, und wählen Sie die __Wordcount-1.0-SNAPSHOT.jar__ -Datei
            * __Klassenname__: eingeben`wordcount.core`
            * __Zusätzliche Parameter__: Geben Sie einen Namen wie `wordcount` zu der Topologie

            Wählen Sie die Topologie zu __Senden__ .

> [AZURE.NOTE] Nach dem Start führt Storm-Topologie weiterspielen (getötet.) Um die Topologie zu beenden, verwenden Sie die `storm kill TOPOLOGYNAME` Befehl über die Befehlszeile (SSH-Sitzung auf einem Linux-Cluster) Storm-Benutzeroberfläche verwenden, wählen Sie die Topologie und wählen Sie die Schaltfläche __Abbrechen__ .

##<a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie gelernt, wie Python-Komponenten von Storm-Topologie verwendet. Finden Sie die folgenden Dokumente für andere Verwendungsmöglichkeiten von Python mit HDInsight:

* [Wie Python für streaming MapReduce-jobs](hdinsight-hadoop-streaming-python.md)
* [Wie Python benutzerdefinierte Funktion (UDF) und Struktur](hdinsight-python.md)
