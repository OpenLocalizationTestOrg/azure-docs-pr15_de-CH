<properties
    pageTitle="R Installation auf Linux-basierten HDInsight | Microsoft Azure"
    description="Informationen Sie zum Installieren und Verwenden von R Hadoop Linux-basierten Clustern anpassen."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von R HDInsight Hadoop-Cluster

R können auf Cluster in Hadoop auf HDInsight mit **Skriptaktion** Cluster Anpassung. Dadurch datenwissenschaftler und Analysten R leistungsstarke MapReduce-GARN Programmierungsframework bereitstellen große Datenmengen auf Hadoop Cluster verarbeitet, die in HDInsight bereitgestellt werden.

> [AZURE.IMPORTANT] Die [Premium-Stufe](https://azure.microsoft.com/pricing/details/hdinsight/) für HDInsight enthält R Server als Teil des Clusters HDInsight. Dadurch R Skripts MapReduce und Spark verteilte Berechnung ausgeführt. Weitere Informationen finden Sie unter [Erste Schritte mit R Server HDInsight](hdinsight-hadoop-r-server-get-started.md). 


## <a name="what-is-r"></a>Was ist R?

<a href="http://www.r-project.org/" target="_blank">R-Projekt für die statistische Berechnung</a> ist ein open Source-Sprache und Umgebung für statistische Berechnung. R bietet Hunderte von integrierten statistischen Funktionen und eigene Programmiersprachen, die Aspekte der funktionalen und objektorientierte Programmierung kombiniert. Darüber hinaus umfangreiche Grafiken bereit. R ist der bevorzugte Programmierumgebung für professionelle Statistiker und Wissenschaftler in einer Vielzahl von Feldern.

R-Skripts können auf Hadoop Cluster in HDInsight ausführen die Aktion beim Skript R Umgebung installieren mit angepasst wurden. R ist kompatibel mit Azure BLOB-Speicher (WASB), sodass die dort gespeicherten Daten HDInsight R über verarbeitet werden können.

## <a name="what-the-script-does"></a>Das Skript führt

Die Skriptaktion R auf HDInsight Cluster installiert installiert die folgenden Ubuntu-Pakete, die Standardinstallation R bereitstellen:

* [R-Base](http://packages.ubuntu.com/precise/r-base): Basis GNU R-Paket
* [R-Base-Dev](http://packages.ubuntu.com/precise/r-base-dev): zusätzlichen GNU R-Pakete

Die folgenden RHadoop-Pakete werden auch installiert die bieten Integration MapReduce und bietet:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): R Entwickler Hadoop MapReduce verwenden
* [Rhdfs](https://github.com/RevolutionAnalytics/rhdfs): ermöglicht R Entwickler Hadoop HDFS (WASB für HDInsight)

Darüber hinaus werden die folgenden R-Pakete installiert:

| R-Paket | Es bietet |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Eine niedrige Sicherheitsstufe R Java-Schnittstelle. |
| [RCPP](https://cran.r-project.org/web/packages/Rcpp/index.html) | R und C++-Integration. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | Serialisieren/Deserialisieren von Objekten JSON R |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Funktionen für bitweise Operationen für ganzzahlige Vektoren. |
| [Übersicht](https://cran.r-project.org/web/packages/digest/index.html) | Erstellen Sie kryptografischer Hash Digests R Objekte. |
| [funktionale](https://cran.r-project.org/web/packages/functional/index.html) | Curry, erstellen und andere Funktionen höherer Ordnung |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Flexibel neu strukturieren und Aggregieren von Daten. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Einfache, konsistente Wrapper für allgemeine Zeichenfolgenoperationen. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Tools zum Aufteilen, Anwendung und Daten kombinieren. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Tools für den Umstieg Fenster Statistik, GIF, Base64 ROC AUC usw.. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Ungefähre Zeichenfolgenvergleichs und Entfernung Zeichenfolgenfunktionen. |

## <a name="install-r-using-script-actions"></a>Installieren Sie R mit Skriptaktionen

Die folgende Skriptaktion dient zum R auf einen HDInsight-Cluster installieren. https://hdiconfigactions.BLOB.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.sh
    
Dieser Abschnitt enthält Informationen zur Verwendung des Skripts beim Erstellen eines neuen Clusters mithilfe der Azure-Portal. 

> [AZURE.NOTE] Azure PowerShell, Azure-CLI, HDInsight .NET SDK oder Azure Resource Manager Vorlagen können auch verwendet werden, Skriptaktionen anwenden. Sie können auch Skriptaktionen auf Cluster bereits anwenden. Weitere Informationen finden Sie unter [Cluster mit Skriptaktionen HDInsight anpassen](hdinsight-hadoop-customize-cluster-linux.md).

1. Bereitstellen eines Clusters mithilfe der Schritte in [HDInsight Bestimmung Linux-basierten Clustern](hdinsight-hadoop-provision-linux-clusters.md#portal), aber werden nicht bereitgestellt.

2. **Optionale Konfiguration** Blade **Skriptaktionen**wählen und folgende Informationen:

    * __NAME__: Geben Sie einen Anzeigenamen für die Skriptaktion.
    * __Skript-URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __Kopf__: Aktivieren Sie diese Option
    * __ARBEITSKRAFT__: Aktivieren Sie diese Option
    * __ZOOKEEPER__: Aktivieren Sie diese Option auf der Zookeeper-Knoten installieren.
    * __Parameter__: lassen Sie dieses Feld leer

3. Am Ende der **Skriptaktionen**Schaltfläche **Wählen Sie** zum Speichern der Konfiguration. Schließlich Schaltfläche **Wählen Sie** unten die **Optionale Konfiguration** Blade optionale Konfigurationsinformationen speichern.

4. Bereitstellung des Clusters gemäß [HDInsight Bestimmung Linux-basierten Clustern](hdinsight-hadoop-provision-linux-clusters.md#portal)weiter.

## <a name="run-r-scripts"></a>R-Skripts ausführen

Nach der Cluster bereitstellen, gehen Sie mit R einen MapReduce-Vorgang auf dem Cluster ausgeführt.

1. Verbinden Sie mit HDInsight SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Weitere Informationen über SSH mit HDInsight finden Sie unter:

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Aus der `username@hn0-CLUSTERNAME:~$` dazu aufgefordert werden, geben Sie folgenden Befehl eine interaktive R-Sitzung zu starten:

        R

3. Geben Sie das folgende Programm R. Dies erzeugt die Zahlen 1 bis 100 und dann mit 2 multipliziert.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    Die erste Zeile ruft RHadoop Bibliothek rmr2, die für MapReduce-Operationen verwendet.

    Die zweite Zeile Werte 1-100, generiert und speichert sie auf Hadoop mithilfe `to.dfs`.

    Die dritte Zeile erstellt einen MapReduce Prozess rmr2 Funktionalität und beginnt die Verarbeitung. Finden Sie unter mehrere Zeilen nach Bildlauf beginnt die Verarbeitung.

4. Anschließend anhand der folgenden temporären Pfad angezeigt, dem die Ausgabe MapReduce gespeichert wurde:

        print(calc())

    Dies sollte ungefähr `/tmp/file5f615d870ad2`. Verwenden Sie zum Anzeigen der tatsächlichen Ausgabe Folgendes:

        print(from.dfs(calc))

    Die Ausgabe sollte wie folgt aussehen:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Zum Beenden R Geben Sie Folgendes ein:

        q()


## <a name="next-steps"></a>Nächste Schritte

- [Installieren und verwenden Farbton auf HDInsight Cluster](hdinsight-hadoop-hue-linux.md). Farbton ist eine Web-Benutzeroberfläche, das Erstellen, ausführen und speichern Schwein und Struktur Aufträge sowie Durchsuchen der Standardspeicher für Ihre HDInsight cluster erleichtert.

- [Giraph für HDInsight-Cluster installieren](hdinsight-hadoop-giraph-install.md). Verwenden Sie Cluster Anpassung Giraph auf HDInsight Hadoop-Cluster installieren. Giraph können Sie graphverarbeitung Hadoop und mit Azure HDInsight verwendet werden.

- [Solr auf HDInsight-Cluster installieren](hdinsight-hadoop-solr-install.md). Verwenden Sie Cluster Anpassung Solr auf HDInsight Hadoop-Cluster installieren. Solr können Sie leistungsstarke Suchvorgänge auf gespeicherte Daten.

- [Farbton für HDInsight-Cluster installieren](hdinsight-hadoop-hue-linux.md). Verwenden Sie Cluster Anpassung Farbton auf HDInsight Hadoop-Cluster installieren. Farbton ist eine Reihe von Web Applications, die Interaktion mit einem Hadoop-Cluster verwendet.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
