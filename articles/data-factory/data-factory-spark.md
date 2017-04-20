<properties 
    pageTitle="Azure Data Factory Spark Programme aufrufen" 
    description="Informationen Sie zum Spark Programme eine Azure Data Factory mithilfe der MapReduce-Aktivität aufgerufen." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Data Factory Spark Programme aufrufen
## <a name="introduction"></a>Einführung
In einer Pipeline Data Factory können MapReduce-Aktivität Sie Funken im Cluster HDInsight Spark Programme. Siehe [MapReduce Aktivität](data-factory-map-reduce.md) Weitere detaillierte Informationen über die Aktivität vor dem Lesen dieses Artikels. 

## <a name="spark-sample-on-github"></a>Spark-Beispiel GitHub
[Spark - Data Factory GitHub Beispiel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) veranschaulicht, wie MapReduce Aktivität Spark Programm aufrufen. Spark-Programm kopiert nur Daten aus einem Azure BLOB-Container in einen anderen. 

## <a name="data-factory-entities"></a>Factory Datenentitäten
**Spark-ADF/Src/ADFJsons** Ordner enthält Dateien für Data Factory Entitäten (verknüpften Diensten, Datasets Pipeline).  

In diesem Beispiel sind zwei **verknüpften Diensten** : Azure Storage und Azure HDInsight. Angeben der Azure-Speicher Name und Werte **StorageLinkedService.json** , ClusterUri, Benutzername und Kennwort in **HDInsightLinkedService.json**.

In diesem Beispiel sind zwei **Datasets** : **input.json** und **output.json**. Diese Dateien befinden sich im Ordner " **Datasets** ".  Diese Dateien stellen Eingabe- und Datasets MapReduce-Aktivität

Beispiel-Pipelines finden Sie im Ordner **ADFJsons-Pipeline** . Überprüfen Sie eine Rohrleitung zu verstehen, wie Spark-Programm mit MapReduce-Aktivität aufrufen. 

MapReduce Aktivität konfiguriert **com.adf.sparklauncher.jar** im Container **Adflibs** in der Azure-Speicher (angegeben in der StorageLinkedService.json) aufrufen. Der Quellcode für dieses Programm ist in Spark-ADF/Src/Main/Java/com/Adf/Ordner und ruft Spark übermitteln und Spark Aufträge ausführen. 

> [AZURE.IMPORTANT] 
> [Readme-Datei lesen. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) für die neuesten und zusätzliche Informationen vor mit dem Beispiel. 
>  
> Verwenden Sie HDInsight Spark-Cluster dabei Spark Programme mithilfe der MapReduce-Aktivität aufgerufen. Mit einer bedarfsgesteuerten HDInsight Cluster wird nicht unterstützt.   


## <a name="see-also"></a>Siehe auch
- [Hive-Aktivität](data-factory-hive-activity.md)
- [Schwein-Aktivität](data-factory-pig-activity.md)
- [MapReduce-Aktivität](data-factory-map-reduce.md)
- [Hadoop Streaming-Aktivität](data-factory-hadoop-streaming-activity.md)
- [R-Skripts aufrufen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
