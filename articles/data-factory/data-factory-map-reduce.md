<properties 
    pageTitle="Aufrufen von Azure Data Factory MapReduce-Programm" 
    description="Erfahren Sie, wie Daten durch Programme MapReduce in Azure HDInsight-Cluster aus einer Fabrik Azure Daten verarbeiten." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Data Factory MapReduce Programme aufrufen
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schweine](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten See Analytics U-SQL](data-factory-usql-activity.md)
[benutzerdefinierte .NET](data-factory-use-custom-activities.md)

HDInsight MapReduce-Aktivität in einer Data Factory- [Pipeline](data-factory-create-pipelines.md) führt MapReduce Programme für [Eigene](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bei Bedarf](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-basierte HDInsight-Cluster. Dieser Artikel baut auf [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) Artikel stellt einen allgemeinen Überblick Datentransformations- und unterstützten Transformationsaktivitäten.

## <a name="introduction"></a>Einführung 
Eine Pipeline in einer Azure Daten verarbeitet Daten im verknüpften Speicher-Services mit verknüpften Compute-Services. Es enthält eine Sequenz von Aktivitäten, wobei jede Aktivität eine Verarbeitung ausführt. Dieser Artikel beschreibt die HDInsight MapReduce-Aktivitäten.
 
Anzeigen Sie [Schweine](data-factory-pig-activity.md) und [Struktur](data-factory-hive-activity.md) Informationen ausgeführt Schwein-Struktur Windows, Linux-basierte HDInsight-Cluster von einer Rohrleitung mit HDInsight Schwein und Struktur Aktivitäten 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON für HDInsight MapReduce-Aktivität 

In der JSON-Definition für die HDInsight: 
 
1. Legen Sie den **Typ** der **Aktivität** auf **HDInsight**.
3. Geben Sie den Namen der Klasse **ClassName** -Eigenschaft.
4. Geben Sie den Pfad der JAR-Datei einschließlich des Dateinamens für die **JarFilePath** -Eigenschaft.
5. Geben Sie den verknüpften Dienst Azure BLOB-Speicher bezieht, die die JAR-Datei für die **JarLinkedService** -Eigenschaft enthält.   
6. Geben Sie Argumente für MapReduce-Programm im **Argumente** -Abschnitt. Zur Laufzeit sehen Sie einige zusätzliche Argumente (z. B.: mapreduce.job.tags) von MapReduce-Framework. Um Ihre Argumente mit Argumenten MapReduce zu unterscheiden, sollten Sie Option und Wert als Argumente verwenden, wie im folgenden Beispiel dargestellt (- s, - Eingabe, -Ausgabe usw. sind Optionen, die unmittelbar nach ihrer Werte).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

HDInsight MapReduce Aktivität können alle MapReduce JAR-Datei auf einen HDInsight-Cluster ausgeführt. In der folgenden Beispiel JSON Definition einer Pipeline ist die HDInsight Aktivität konfiguriert Mahout JAR-Datei.

## <a name="sample-on-github"></a>Beispiel für GitHub
Sie können eine Beispiel für die Verwendung von HDInsight MapReduce Aktivität aus: [Factory Beispieldaten auf GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Ausführen des Programms Wörter zählen
Die Pipeline in diesem Beispiel führt das Programm Wortzahl Map/Reduce auf Azure HDInsight Cluster.   

### <a name="linked-services"></a>Verknüpfte Dienste
Zunächst erstellen Sie einen verknüpften Dienst den Azure-Speicher zu verknüpfen, die von Azure HDInsight Cluster Azure Data Factory verwendet wird. Wenn Sie folgenden Code kopieren, vergessen Sie nicht **Namen** und **kontoschlüssel** mit dem Namen und den Schlüssel der Azure-Speicher ersetzen. 

#### <a name="azure-storage-linked-service"></a>Azure verknüpft Speicherdienst

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure verknüpften HDInsight-Dienst
Anschließend erstellen Sie einen verknüpften Dienst Azure HDInsight Cluster Azure Data Factory verknüpfen. Wenn Sie den folgenden Code kopieren, der Name Ihres Clusters HDInsight **HDInsight Clustername** ersetzen und Werte für Benutzernamen und Kennwort ändern.   

    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Datasets

#### <a name="output-dataset"></a>Ausgabedataset
Die Pipeline in diesem Beispiel übernimmt alle Eingaben. Sie geben ein ausgabedataset für HDInsight MapReduce-Aktivität. Dieses Dataset ist ein dummy Dataset, die Pipeline Zeitplan Laufwerk erforderlich ist.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Pipeline
Die Pipeline in diesem Beispiel hat nur eine Aktivität vom Typ ist: HDInsightMapReduce. Einige wichtigen Eigenschaften in JSON sind: 

Eigenschaft | Notizen
:-------- | :-----
Typ | Der Typ muss auf **HDInsightMapReduce**festgelegt. 
Klassenname | Name der Klasse: **Wordcount**
jarFilePath | Pfad der JAR-Datei, die die Klasse enthält. Wenn Sie den folgenden Code kopieren, vergessen Sie nicht, den Namen des Clusters ändern. 
jarLinkedService | Azure verknüpfter Dienst, der die JAR-Datei enthält. Dieser verknüpfte Dienst verweist auf Speicher, der den HDInsight-Cluster zugeordnet ist. 
Argumente | Wordcount-Programm akzeptiert zwei Argumente: Eingabe und Ausgabe. Die Datei ist die Datei davinci.txt.
/ Intervall | Die Werte für diese Eigenschaften entsprechen das ausgabedataset. 
Nameverknüpfterdienst | bezieht sich auf den Dienst HDInsight verknüpft, die Sie zuvor erstellt haben.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Spark Programme ausführen
MapReduce-Aktivität können Sie Funken im Cluster HDInsight Spark Programme. Einzelheiten Sie [aufrufen Spark Programme Azure Data Factory](data-factory-spark.md) .  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Siehe auch
- [Hive-Aktivität](data-factory-hive-activity.md)
- [Schwein-Aktivität](data-factory-pig-activity.md)
- [Hadoop Streaming-Aktivität](data-factory-hadoop-streaming-activity.md)
- [Spark Programme aufrufen](data-factory-spark.md)
- [R-Skripts aufrufen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
