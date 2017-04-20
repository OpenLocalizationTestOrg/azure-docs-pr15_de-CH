<properties
    pageTitle="Verwenden Sie R in HDInsight Cluster anpassen | Microsoft Azure"
    description="Informationen Sie zum Installieren von R mit Skriptaktion und verwenden Sie R HDInsight-Cluster."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von R HDInsight Hadoop-Cluster

Erfahren Sie anpassen basierenden HDInsight Cluster mit Skript Aktion und Clustern auf HDInsight R verwenden. Die [Premium-Stufe](https://azure.microsoft.com/pricing/details/hdinsight/) für HDInsight enthält R Server als Teil des Clusters HDInsight. Dadurch R Skripts MapReduce und Spark verteilte Berechnung ausgeführt. Weitere Informationen finden Sie unter [Erste Schritte mit R Server HDInsight](hdinsight-hadoop-r-server-get-started.md). Informationen zu Linux-basierten Cluster mit R finden Sie unter [Installieren und R mit HDinsight Hadoop-Cluster (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
R auf jedem Cluster (Hadoop Sturm, HBase, Funken) können Azure HDInsight mit *Skriptaktion*. Eine Beispielskript, R auf einen HDInsight-Cluster installieren ist ein Blob schreibgeschützt Azure-Speicher zur [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)ab. 

**Verwandte Artikel**

- [Installieren und Verwenden von R HDinsight Hadoop-Cluster (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [In HDInsight Cluster Hadoop erstellen](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight-Cluster
- [HDInsight Cluster mit Skriptaktion anpassen][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mit Skriptaktion
- [Entwickeln von Skriptaktion Skripts für HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Was ist R?

<a href="http://www.r-project.org/" target="_blank">R-Projekt für die statistische Berechnung</a> ist ein open Source-Sprache und Umgebung für statistische Berechnung. R bietet Hunderte von integrierten statistischen Funktionen und eigene Programmiersprachen, die Aspekte der funktionalen und objektorientierte Programmierung kombiniert. Darüber hinaus umfangreiche Grafiken bereit. R ist der bevorzugte Programmierumgebung für professionelle Statistiker und Wissenschaftler in einer Vielzahl von Feldern.

R ist kompatibel mit Azure BLOB-Speicher (WASB), sodass die dort gespeicherten Daten HDInsight R über verarbeitet werden können.  

## <a name="install-r"></a>Installieren von R

Ein [Beispielskript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) , R auf einen HDInsight-Cluster installieren ist ein nur-Lese-Blob in Azure-Speicher ab. Dieser Abschnitt enthält Informationen wie das Beispielskript beim Erstellen des Clusters mithilfe der Azure-Portal.

> [AZURE.NOTE] Das Beispielskript wurde mit HDInsight Clusterversion 3.1 eingeführt. Weitere Informationen zu HDInsight Cluster Versionen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).

1. Beim Erstellen eines HDInsight Clusters über das Portal auf **Optionale Konfiguration**und klicken Sie **Skriptaktionen**.
2. Auf der Seite **Aktionen Skript** eingeben:

    ![Skriptaktion für einen Cluster anpassen verwenden] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Skriptaktion für einen Cluster anpassen verwenden")

    <table border='1'>
        <tr><th>Eigenschaft</th><th>Wert</th></tr>
        <tr><td>Name</td>
            <td>Geben Sie einen Namen für die Skriptaktion z. B. <b>R installieren</b>.</td></tr>
        <tr><td>Skript URI</td>
            <td>Geben Sie den URI für das Skript aufgerufen wird, um den Cluster beispielsweise <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i> anpassen</td></tr>
        <tr><td>Knotentyp</td>
            <td>Geben Sie Knoten, auf denen das Skript Anpassung ausgeführt wird. Sie können <b>Alle Knoten</b>, <b>Head-Knoten</b>oder <b>Knoten Arbeitskraft</b> nur auswählen.
        <tr><td>Parameter</td>
            <td>Geben Sie die Parameter ggf. vom Skript an. Jedoch erfordert das Skript R installiert keine Parameter, damit Sie dieses Feld leer lassen können.</td></tr>
    </table>

    Sie können mehrere Skriptaktion mehrere Komponenten im Cluster installieren. Nachdem Sie die Skripts hinzugefügt haben, klicken Sie zum Starten des Clusters Verpackung.

Das Skript können Sie R auf HDInsight mithilfe von Azure PowerShell oder HDInsight .NET SDK installieren. Diese Verfahren werden in diesem Artikel Anweisungen.

## <a name="run-r-scripts"></a>R-Skripts ausführen
Dieser Abschnitt beschreibt, wie ein R-Skript im Cluster Hadoop mit HDInsight.

1. **Eine Remotedesktop Verbindung zum Cluster herstellen**: über das Portal aktivieren Sie den Remotedesktop für den Cluster mit installiert erstellt und mit Cluster verbinden. Eine Anleitung finden Sie [mit HDInsight-Cluster über RDP](hdinsight-administer-use-management-portal.md#rdp).

2. **Öffnen Sie die Konsole R**: die R Installation speichert eine Verknüpfung zur Konsole R auf dem Head-Knoten. Klicken sie zum Öffnen der Konsole R.

3. **Skript R**: das R-Skript direkt von R-Konsole ausgeführt werden kann, einfügen, auswählen und die EINGABETASTE drücken. Hier ist ein einfaches Beispiel-Skript, das die Zahlen 1 bis 100 und dann mit 2 multipliziert.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

Die ersten beiden Zeilen rufen Sie die RHadoop-Bibliotheken, die mit r installiert Die letzte Zeile zeigt die Ergebnisse an der Konsole. Die Ausgabe sollte wie folgt aussehen:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Installieren Sie mithilfe von Aure PowerShell R

Siehe [Anpassen HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Das demonstriert Spark mit Azure PowerShell installieren. Sie müssen das Skript [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)Verwendung anpassen.

## <a name="install-r-using-net-sdk"></a>Installieren Sie R mit .NET SDK

Siehe [Anpassen HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Das demonstriert Spark mit .NET SDK installieren. Sie müssen das Skript [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11)Verwendung anpassen.


## <a name="see-also"></a>Siehe auch

- [Installieren und Verwenden von R HDinsight Hadoop-Cluster (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [In HDInsight Cluster Hadoop erstellen](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight-Cluster
- [HDInsight Cluster mit Skriptaktion anpassen][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mit Skriptaktion
- [Entwickeln von Skriptaktion Skripts für HDInsight](hdinsight-hadoop-script-actions.md)
- [Installieren und Verwenden von Spark auf HDInsight Cluster][hdinsight-install-spark]: Skriptaktion Beispiel Spark installieren
- [Giraph für HDInsight-Cluster installieren](hdinsight-hadoop-giraph-install.md): Skriptaktion Beispiel zur Installation Giraph
- [Solr auf HDInsight-Cluster installieren](hdinsight-hadoop-solr-install-linux.md): Skriptaktion Beispiel Solr installieren.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
