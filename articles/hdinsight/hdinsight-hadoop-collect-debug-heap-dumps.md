<properties
    pageTitle="Debuggen und Analysieren Hadoop Dienstleistungen Heapdumps | Microsoft Azure"
    description="Automatisch sammeln Sie Heapdumps für Hadoop Services und Azure BLOB-Speicher Konto zum Debuggen und Analyse."
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
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Collect Heap dumps im BLOB-Speicher zu debuggen Hadoop Services analysieren

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heapdumps enthalten einen Snapshot der Anwendung Speicher, einschließlich der Werte von Variablen Zeitpunkt das Speicherabbild erzeugt wurde. Sind sehr nützlich zum Diagnostizieren von Problemen, die bei der Ausführung auftreten. Heapdumps können automatisch Hadoop Dienste aufgenommen und Azure BLOB-Speicher-Konto eines Benutzers unter HDInsightHeapDumps /. 

Auflistung von Heapdumps für verschiedene Dienste muss für einzelne Cluster-Dienste aktiviert. Der Standardwert für dieses Feature ist für Cluster deaktiviert. Diese Heapdumps können groß, es empfiehlt sich, überwacht das Speicherkonto Blob sind Speicherort wird nach dem Aktivieren der Auflistung.

> [AZURE.NOTE] Die Informationen in diesem Artikel gilt nur für Windows-basierte HDInsight. Informationen zu Linux-basierten HDInsight finden Sie unter [Aktivieren von Heapdumps für Hadoop auf Linux-basierten HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>Dienste für Heapdumps

Sie können Heapdumps für die folgenden Dienste:

*  **Hcatalog** - tempelton
*  **Struktur** - hiveserver2, Metastore, derbyserver
*  **Mapreduce** - jobhistoryserver
*  **Garn** - Resourcemanager Nodemanager, timelineserver
*  **bietet** - Datanode, Secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfigurationselemente, die Heapdumps aktivieren

Um Heapdumps für einen Dienst zu aktivieren, müssen Sie entsprechende Konfigurationselemente im Abschnitt für diesen Dienst Festlegen der **Dienstname**angegeben wird.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Der Wert der **Dienstname** kann die oben aufgeführten Dienste: Tempelton, hiveserver2, Metastore, Derbyserver, Jobhistoryserver, Resourcemanager Nodemanager, Timelineserver, Datanode, Secondarynamenode, oder Namenode.

## <a name="enable-using-azure-powershell"></a>Mithilfe von Azure PowerShell aktivieren

Beispielsweise würde zum Heapdumps mithilfe von Azure PowerShell für Jobhistoryserver aktivieren Sie Folgendes:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Mit .NET SDK aktivieren

Beispielsweise würde zum Heapdumps mit Azure HDInsight .NET SDK für Jobhistoryserver aktivieren Sie Folgendes:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
