<properties
    pageTitle="Installieren und Verwenden von Giraph auf Hadoop Cluster in HDInsight | Microsoft Azure"
    description="Lernen Sie HDInsight Cluster mit Giraph anpassen und Giraph verwenden."
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
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-giraph-in-hdinsight"></a>Installieren und Verwenden von Giraph in HDInsight


Lernen Sie Windows basierend HDInsight Cluster mit Giraph mit Skriptaktion anpassen und zum Giraph umfangreiche Grafiken verarbeiten. Informationen zu Linux-basierten Cluster mit Giraph finden Sie unter [Giraph auf HDInsight Hadoop-Cluster (Linux) installiert](hdinsight-hadoop-giraph-install-linux.md).
 
Giraph für jeden Cluster (Hadoop Sturm, HBase, Funken) können Azure HDInsight mit *Skriptaktion*. Eine Beispielskript Giraph auf einen HDInsight-Cluster installieren ist ein Blob schreibgeschützt Azure-Speicher zur [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)ab. Das Beispielskript funktioniert nur mit HDInsight Clusterversion 3.1. Weitere Informationen über HDInsight Cluster Versionen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).

**Verwandte Artikel**

- [Installieren von Giraph auf HDInsight Hadoop-Cluster (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [In HDInsight Cluster Hadoop erstellen](hdinsight-provision-clusters.md): Allgemeine Informationen zum HDInsight-Cluster erstellen.
- [HDInsight Cluster mit Skriptaktion anpassen][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mit Skriptaktion.
- [Skripts für HDInsight Skriptaktion entwickeln](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Was ist Giraph?

<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> können Sie Graphen mit Hadoop Verarbeitung durchführen und mit Azure HDInsight verwendet werden kann. Diagramme Modell Beziehungen, wie zwischen Routern in einem großen Netzwerk wie das Internet oder Beziehungen Netzwerken (auch als soziale Diagramm bezeichnet). Diagramm-Verarbeitung können Sie Grund zu der Beziehung zwischen Objekten in einem Diagramm wie:

- Identifizieren potenzielle Freunde auf Ihre aktuelle Beziehung.
- Identifizieren die kürzeste Route zwischen zwei Computern in einem Netzwerk.
- Berechnung den seitenrang von Webseiten.


## <a name="install-giraph-using-portal"></a>Installieren Sie Giraph mit portal

1. Erstellen eines Clusters mithilfe der Option **Benutzerdefinierte** wie [Hadoop erstellen Cluster in HDInsight](hdinsight-provision-clusters.md#portal)beschrieben.
2. Der **Skript-Aktionen** des Assistenten klicken Sie auf **Skriptaktion hinzufügen** , um Angaben über die Skriptaktion wie folgt:

    ![Skriptaktion für einen Cluster anpassen verwenden] (./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Skriptaktion für einen Cluster anpassen verwenden")

    <table border='1'>
        <tr><th>Eigenschaft</th><th>Wert</th></tr>
        <tr><td>Name</td>
            <td>Geben Sie einen Namen für die Skriptaktion. Beispielsweise <b>Giraph installieren</b>.</td></tr>
        <tr><td>Skript URI</td>
            <td>Geben Sie den URI Uniform Resource Identifier () in das Skript aufgerufen wird, um den Cluster anpassen. Beispielsweise <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Knotentyp</td>
            <td>Geben Sie Knoten, auf denen das Skript Anpassung ausgeführt wird. Sie können <b>alle Knoten</b>, <b>Head-Knoten</b>oder <b>Worker-Knoten</b>.
        <tr><td>Parameter</td>
            <td>Geben Sie die Parameter ggf. vom Skript an. Das Skript Giraph installieren erfordert keine Parameter, damit Sie dieses Feld leer lassen können.</td></tr>
    </table>

    Sie können mehrere Skriptaktion mehrere Komponenten im Cluster installieren. Nachdem die Skripts hinzugefügt haben, klicken Sie auf das Häkchen, um den Cluster erstellen.

## <a name="use-giraph"></a>Giraph verwenden

Wir verwenden das SimpleShortestPathsComputation-Beispiel veranschaulichen die grundlegende <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> -Implementierung für den kürzesten Weg zwischen Objekten in einem Diagramm. Gehen Sie folgendermaßen vor, die Beispieldaten und JAR-Beispiel hochladen Stapelverarbeitung anhand SimpleShortestPathsComputation, und zeigen Sie die Ergebnisse.

1. Laden Sie eine Beispieldatei Azure Blob-Speicher. Erstellen Sie eine neue Datei namens **tiny_graph.txt**auf der lokalen Arbeitsstation. Sie sollten die folgenden Zeilen enthalten:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Laden Sie die Datei tiny_graph.txt in den primären Speicher für den HDInsight-Cluster. Informationen zum Hochladen von Daten finden Sie unter [Daten Hadoop Aufträge in HDInsight](hdinsight-upload-data.md).

    Diese Daten beschreiben eine Beziehung zwischen Objekten in einem gerichteten Diagramm im Format [Quelle\_-Id, Quelle\_Wert [[Dest\_Id], [Rand\_Wert],...]]. Jede Zeile stellt eine Beziehung zwischen einer **Quelle\_Id** Objekt und einem oder mehreren **Dest\_Id** Objekte. Der **Rand\_Wert** (oder) können als Stärke oder Entfernung der Verbindung zwischen **Source_id** vorstellen und **Dest\_Id**.

    Gezeichnet, und die Verwendung der (Gewicht) als Abstand zwischen Objekten, Daten sieht:

    ![tiny_graph.txt als Kreise mit unterschiedlichen Abstand](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)



4. Führen Sie das Beispiel SimpleShortestPathsComputation. Verwenden Sie die folgenden Azure PowerShell-Cmdlets zum Ausführen des Beispiels mit der tiny_graph.txt-Datei als Eingabe. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

        $clusterName = "clustername"
        # Giraph examples jar
        $jarFile = "wasbs:///example/jars/giraph-examples.jar"
        # Arguments for this job
        $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                        "-ca", "mapred.job.tracker=headnodehost:9010",
                        "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                        "-vip", "wasbs:///example/data/tiny_graph.txt",
                        "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                        "-op",  "wasbs:///example/output/shortestpaths",
                        "-w", "2"
        # Create the definition
        $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
          -JarFile $jarFile
          -ClassName "org.apache.giraph.GiraphRunner"
          -Arguments $jobArguments

        # Run the job, write output to the Azure PowerShell window
        $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureHDInsightJob -Job $job
        Write-Host "STDERR"
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput

    Ersetzen Sie im obigen Beispiel **Clustername** mit dem Namen HDInsight Cluster, der Giraph installiert ist.

5. Die Ergebnisse anzeigen. Sobald der Auftrag abgeschlossen ist, wird die Ergebnisse gespeichert in zwei Ausgabedateien in den __Wasbs: / / / Beispiel/Out/Shotestpaths__ Ordner. Die Dateien heißen __Teil m 00001__ und __Teil m 00002__. Führen Sie folgende Schritte zum Herunterladen und zeigen die Ausgabe:

        $subscriptionName = "<SubscriptionName>"       # Azure subscription name
        $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
        $containerName = "<ContainerName>"             # Blob storage container name

        # Select the current subscription
        Select-AzureSubscription $subscriptionName

        # Create the Storage account context object
        $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

        # Download the job output to the workstation
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force

    __Beispiel, Ausgabe/Shortestpaths__ -Verzeichnisstruktur im aktuellen Verzeichnis auf Ihrer Arbeitsstation erstellt, und zwei Ausgabe Dateien an diesen Speicherort.

    Verwenden Sie das Cmdlet __Cat__ , der Inhalt der Dateien angezeigt:

        Cat example/output/shortestpaths/part*

    Die Ausgabe sollte etwa wie folgt aussehen:


        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    SimpleShortestPathComputation ist Beispiel hart codierten zu Objekt-ID 1 und den kürzesten Weg zu anderen Objekten suchen. Damit die Ausgabe als lauten `destination_id distance`, wobei Abstand den Wert (oder Gewicht) der Kanten zwischen Objekt-ID 1 und Ziel-ID

    Diese Visualisierung, können Sie die Ergebnisse überprüfen, Anreise den kürzesten Pfad zwischen ID 1 und alle anderen Objekte. Beachten Sie, dass der kürzeste Weg zwischen ID 1 und ID 4 5. Dies ist der Gesamtabstand zwischen <span style="color:orange">ID 1 3</span>und <span style="color:red">ID 3 und 4</span>.

    ![Zeichnen von Objekten als Kreise mit kürzeste Pfade zwischen](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Installieren Sie Giraph mit Aure PowerShell

Siehe [Anpassen HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Das demonstriert Spark mit Azure PowerShell installieren. Sie müssen das Skript [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)Verwendung anpassen.

## <a name="install-giraph-using-net-sdk"></a>Installieren Sie Giraph mit .NET SDK

Siehe [Anpassen HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Das demonstriert Spark mit .NET SDK installieren. Sie müssen das Skript [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)Verwendung anpassen.


## <a name="see-also"></a>Siehe auch

- [Installieren von Giraph auf HDInsight Hadoop-Cluster (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [In HDInsight Cluster Hadoop erstellen](hdinsight-provision-clusters.md): Allgemeine Informationen zum HDInsight-Cluster erstellen.
- [HDInsight Cluster mit Skriptaktion anpassen][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mit Skriptaktion.
- [Skripts für HDInsight Skriptaktion entwickeln](hdinsight-hadoop-script-actions.md).
- [Installieren und Verwenden von Spark auf HDInsight Cluster][hdinsight-install-spark]: Skriptaktion Beispiel Spark installieren.
- [R auf HDInsight-Cluster installieren][hdinsight-install-r]: Aktionsbeispiel Skript zum Installieren von r
- [Solr auf HDInsight-Cluster installieren](hdinsight-hadoop-solr-install.md): Skriptaktion Beispiel Solr installieren.



[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: ../powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
