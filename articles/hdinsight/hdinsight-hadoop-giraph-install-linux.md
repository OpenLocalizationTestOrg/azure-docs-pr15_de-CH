<properties
    pageTitle="Installieren und Verwenden von Giraph auf Linux-basierten HDInsight (Hadoop) | Microsoft Azure"
    description="Informationen Sie zum Installieren von Giraph auf HDInsight Linux-basierten Cluster mit Skriptaktionen. Skript-Aktionen können Sie Cluster während der Erstellung anpassen Cluster ändern oder Dienste und Dienstprogramme installieren."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="larryfr"/>

# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Giraph auf HDInsight Hadoop-Cluster installieren und Giraph umfangreiche Grafiken zu verwenden

Giraph können für jeden Cluster Hadoop auf Azure HDInsight mit **Skriptaktion** ein Clusters anpassen.

In diesem Thema lernen Sie das Giraph mit Skriptaktion installieren. Installierte Giraph erfahren Sie auch, wie Sie Giraph für häufigste Applikationen die umfangreiche Grafiken verarbeiten.

> [AZURE.NOTE] Die Informationen in diesem Artikel gilt für Linux-basierte HDInsight-Cluster. Informationen zum Arbeiten mit Windows-basierten Clustern finden Sie unter [Installieren von Giraph auf HDInsight Hadoop-Cluster (Windows)](hdinsight-hadoop-giraph-install.md)

## <a name="whatis"></a>Was ist Giraph?

[Apache Giraph](http://giraph.apache.org/) können Sie Graphen mit Hadoop Verarbeitung durchführen und mit Azure HDInsight verwendet werden kann. Diagramme Modell Beziehungen, wie zwischen Routern in einem großen Netzwerk wie das Internet oder Beziehungen Netzwerken (auch als soziale Diagramm bezeichnet). Diagramm-Verarbeitung können Sie Grund zu der Beziehung zwischen Objekten in einem Diagramm wie:

- Identifizieren potenzielle Freunde auf Ihre aktuelle Beziehung.
- Identifizieren die kürzeste Route zwischen zwei Computern in einem Netzwerk.
- Berechnung den seitenrang von Webseiten.

> [AZURE.WARNING] Komponenten mit HDInsight-Cluster vollständig unterstützt und Microsoft Support hilft isolieren und Lösen von Problemen im Zusammenhang mit diesen Komponenten.
>
> Benutzerdefinierte Komponenten wie Giraph, erhalten angemessene Unterstützung helfen, das Problem zu beheben. Dadurch kann die Fehlerbehebung oder Aufforderung zu Kanälen für open-Source-Technologien, fundiertes Fachwissen für diese Technologie fand. Beispielsweise sind viele Community-Sites, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-Projekte verfügen Projektsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/).

##<a name="what-the-script-does"></a>Das Skript führt

Dieses Skript führt die folgenden Aktionen aus:

* Giraph, installiert`/usr/hdp/current/giraph`
* Kopien der `giraph-examples.jar` Datei Standardspeicher (WASB) für den Cluster:`/example/jars/giraph-examples.jar`

## <a name="install"></a>Installieren Sie Giraph mit Skripts

Eine Beispielskript Giraph auf einen HDInsight-Cluster installieren ist am folgenden Speicherort verfügbar.

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Dieser Abschnitt beschreibt das Beispielskript beim Erstellen des Clusters mithilfe der Azure-Portal verwenden. 

> [AZURE.NOTE] Azure PowerShell, Azure-CLI, HDInsight .NET SDK oder Azure Resource Manager Vorlagen können auch verwendet werden, Skriptaktionen anwenden. Sie können auch Skriptaktionen auf Cluster bereits anwenden. Weitere Informationen finden Sie unter [Cluster mit Skriptaktionen HDInsight anpassen](hdinsight-hadoop-customize-cluster-linux.md).

1. Erstellen eines Clusters mithilfe der Schritte in [HDInsight erstellen Linux-basierten Clustern](hdinsight-hadoop-create-linux-clusters-portal.md), aber führen Sie nicht erstellen.

2. **Optionale Konfiguration** Blade **Skriptaktionen**wählen und folgende Informationen:

    * __NAME__: Geben Sie einen Anzeigenamen für die Skriptaktion.
    * __Skript-URI__: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    * __Kopf__: Aktivieren Sie diese Option
    * __ARBEITSKRAFT__: lassen Sie diese deaktiviert
    * __ZOOKEEPER__: lassen Sie diese deaktiviert
    * __Parameter__: lassen Sie dieses Feld leer

3. Am Ende der **Skriptaktionen**Schaltfläche **Wählen Sie** zum Speichern der Konfiguration. Schließlich Schaltfläche **Wählen Sie** unten die **Optionale Konfiguration** Blade optionale Konfigurationsinformationen speichern.

4. Erstellen des Clusters unter [HDInsight Erstellen von Linux-basierten Clustern](hdinsight-hadoop-create-linux-clusters-portal.md)weiter.

## <a name="usegiraph"></a>Wie verwende ich Giraph in HDInsight?

Wenn Cluster erstellt wurde, gehen Sie SimpleShortestPathsComputation Beispiel enthaltene Giraph ausgeführt. Implementiert die grundlegende <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> -Implementierung für den kürzesten Weg zwischen Objekten in einem Diagramm.

1. Verbinden Sie mit HDInsight SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Weitere Informationen über SSH mit HDInsight finden Sie unter:

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

1. Verwenden Sie folgende eine neue Datei namens __tiny_graph.txt__:

        nano tiny_graph.txt

    Verwenden Sie die folgenden Inhalt dieser Datei:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Diese Daten beschreiben eine Beziehung zwischen Objekten in einem gerichteten Diagramm im Format [Quelle\_-Id, Quelle\_Wert [[Dest\_Id], [Rand\_Wert],...]]. Jede Zeile stellt eine Beziehung zwischen einer **Quelle\_Id** Objekt und einem oder mehreren **Dest\_Id** Objekte. Der **Rand\_Wert** (oder) können als Stärke oder Entfernung der Verbindung zwischen **Source_id** vorstellen und **Dest\_Id**.

    Gezeichnet, und die Verwendung der (Gewicht) als Abstand zwischen Objekten, Daten sieht:

    ![tiny_graph.txt als Kreise mit unterschiedlichen Abstand](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

2. Um die Datei zu speichern, verwenden Sie __STRG + X__und __Y__und schließlich __Geben__ den Dateinamen akzeptiert.

3. Verwenden Sie zum Speichern der Daten in der primären Speicher für HDInsight Cluster folgende:

        hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt

4. Führen Sie SimpleShortestPathsComputation wird mit dem folgenden Befehl aus.

         yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2

    Mit diesem Befehl verwendeten Parameter werden in der folgenden Tabelle beschrieben.

  	| Parameter | Funktionsweise |
  	| --------- | ------------ |
  	| `jar /usr/hdp/current/giraph/giraph-examples.jar` | JAR-Datei mit den Beispielen. |
  	| `org.apache.giraph.GiraphRunner` | Zu den Beispielen verwendete Klasse. |
  	| `org.apache.giraph.examples.SimpleShortestPathsCoputation` | Das Beispiel ausgeführt wird. In diesem Fall berechnen sie den kürzesten Weg zwischen ID 1 und alle anderen IDs im Diagramm. |
  	| `-ca mapred.job.tracker=headnodehost:9010` | Der Hauptknoten für den Cluster. |
  	| `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` | Das Eingabeformat mit für die eingegebenen Daten. |
  	| `-vip /example/data/tiny_graph.txt` | Die eingegebenen Daten-Datei. |
  	| `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` | Das Ausgabeformat. In diesem Fall-ID und Wert als Text. |
  	| `-op /example/output/shortestpaths` | Der Ausgabespeicherort. |
  	| `-w 2` | Die Anzahl der Arbeitskräfte verwendet. In diesem Fall 2. |

    Weitere Informationen über diese und andere Parameter mit Giraph [Giraph Schnellstart](http://giraph.apache.org/quick_start.html)anzeigen

5. Sobald der Auftrag abgeschlossen ist, wird die Ergebnisse gespeichert der __Wasbs: / / / Beispiel/Out/Shotestpaths__ Verzeichnis. Die erstellten Dateien beginnen mit __Teil-m -__ und enden mit einer Zahl die erste, zweite, usw. Datei. Verwenden Sie zum Anzeigen der Ausgabe Folgendes:

        hdfs dfs -text /example/output/shortestpaths/*

    Die Ausgabe sollte etwa wie folgt aussehen:

        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    SimpleShortestPathComputation ist Beispiel hart codierten zu Objekt-ID 1 und den kürzesten Weg zu anderen Objekten suchen. Damit die Ausgabe als lauten `destination_id distance`, wobei Abstand den Wert (oder Gewicht) der Kanten zwischen Objekt-ID 1 und Ziel-ID

    Diese Visualisierung, können Sie die Ergebnisse überprüfen, Anreise den kürzesten Pfad zwischen ID 1 und alle anderen Objekte. Beachten Sie, dass der kürzeste Weg zwischen ID 1 und ID 4 5. Dies ist der Gesamtabstand zwischen <span style="color:orange">ID 1 3</span>und <span style="color:red">ID 3 und 4</span>.

    ![Zeichnen von Objekten als Kreise mit kürzeste Pfade zwischen](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)


## <a name="next-steps"></a>Nächste Schritte

- [Installieren und verwenden Farbton auf HDInsight Cluster](hdinsight-hadoop-hue-linux.md). Farbton ist eine Web-Benutzeroberfläche, das Erstellen, ausführen und speichern Schwein und Struktur Aufträge sowie Durchsuchen der Standardspeicher für Ihre HDInsight cluster erleichtert.

- [R auf HDInsight-Cluster installieren](hdinsight-hadoop-r-scripts-linux.md): Anleitung zur Verwendung von cluster-Anpassung zur Installation und Verwendung von R auf HDInsight Hadoop-Cluster. R ist ein Open-Source-Sprache und Umgebung für statistische Datenverarbeitung. Es enthält Hunderte von integrierten statistischen Funktionen und eigene Programmiersprachen, die Aspekte der funktionalen und objektorientierte Programmierung kombiniert. Darüber hinaus umfangreiche Grafiken bereit.

- [Solr auf HDInsight-Cluster installieren](hdinsight-hadoop-solr-install-linux.md). Verwenden Sie Cluster Anpassung Solr auf HDInsight Hadoop-Cluster installieren. Solr können Sie leistungsstarke Suchvorgänge auf Daten.
