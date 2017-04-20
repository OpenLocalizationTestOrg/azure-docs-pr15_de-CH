<properties 
    pageTitle="Hadoop Streaming-Aktivität" 
    description="Erfahren Sie Hadoop Streaming-Aktivität in einer Azure Daten Verwendung Hadoop Streaming Programme auf einen auf-Anforderung/Ihre eigenen HDInsight-Cluster ausführen." 
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
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop Streaming-Aktivität
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schweine](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten See Analytics U-SQL](data-factory-usql-activity.md)
[benutzerdefinierte .NET](data-factory-use-custom-activities.md)

Verwenden Sie die HDInsightStreamingActivity Aktivität ein Auftrags Hadoop Streaming über eine Pipeline Azure Data Factory aufgerufen. Der folgende JSON-Ausschnitt zeigt die Syntax der HDInsightStreamingActivity in einer Pipeline JSON-Datei. 

HDInsight Streaming-Aktivität in einer Data Factory- [Pipeline](data-factory-create-pipelines.md) führt Programme Hadoop Streaming auf [Eigene](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bei Bedarf](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-basierte HDInsight-Cluster. Dieser Artikel baut auf [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) Artikel stellt einen allgemeinen Überblick Datentransformations- und unterstützten Transformationsaktivitäten.

## <a name="json-sample"></a>JSON-Beispiel
HDInsight-Cluster wird mit Beispiel (wc.exe und cat.exe) und (davinci.txt) automatisch ausgefüllt. Name der Container, der HDInsight-Cluster ist standardmäßig der Name des Clusters. Beispielsweise ist der Clustername Myhdicluster wäre Name des Containers Blob zugeordnete Myhdicluster. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Beachten Sie folgende Punkte:

1. Legen Sie **Nameverknüpfterdienst** auf den Namen des verknüpften Diensts auf HDInsight Cluster verweist auf dem streaming Mapreduce-Job ausgeführt wird.
2. Legen Sie den Typ der Aktivität auf **HDInsightStreaming**.
3. Geben Sie für **Mapper** -Eigenschaft den Namen der ausführbaren Mapper an Im Beispiel ist cat.exe der ausführbaren Zuordnung.
4. Geben Sie den Namen des Übergangsstücks ausführbare **Reducer** -Eigenschaft. Im Beispiel ist wc.exe der ausführbaren Datei.
5. Geben Sie für **input** Type-Eigenschaft die Eingabedatei (einschließlich der Position) für Zuordnung. Im Beispiel: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": Adfsample ist der BLOB-Container und Beispiel/Daten/Gutenberg ist der Ordner davinci.txt ist das Blob.
6. Geben Sie für **Output** Type-Eigenschaft die Ausgabedatei (einschließlich der Position) für des Reduzierstücks. Die Ausgabe des Einzelvorgangs Hadoop Streaming wird für diese Eigenschaft angegebene Speicherort geschrieben.
7. Geben Sie im Abschnitt **Dateipfade** für Mapper und Reduzierstücks ausführbare Pfade. Im Beispiel: "adfsample/example/apps/wc.exe" Adfsample ist der Container der BLOB-Beispiel-apps ist der Ordner und wc.exe ist die ausführbare Datei.
8. **FileLinkedService** -Eigenschaft festlegen Sie den Azure verknüpft Speicherdienst, der den Azure-Speicher darstellt, der die im Abschnitt "Dateipfade" enthält.
9. **Arguments** -Eigenschaft geben Sie die Argumente für den streaming-Auftrag.
10. **GetDebugInfo** -Eigenschaft ist ein optionales Element. Wenn Fehler festgelegt ist, werden die Protokolle nur bei heruntergeladen. Wenn auf All gesetzt ist, werden die Protokolle immer unabhängig von den Ausführungsstatus heruntergeladen.

> [AZURE.NOTE] Wie im Beispiel gezeigt, geben Sie ein ausgabedataset für die Hadoop Streaming für die Eigenschaft **gibt** . Dieses Dataset ist ein dummy Dataset, die Pipeline Zeitplan Laufwerk erforderlich ist. Sie müssen keine Eingabe-Dataset für die Aktivität für die **Eingaben** Eigenschaft angeben.  

    
## <a name="example"></a>Beispiel
Die Pipeline in dieser exemplarischen Vorgehensweise führt Word Count streaming Map/Reduce Programm auf Azure HDInsight Cluster. 

### <a name="linked-services"></a>Verknüpfte Dienste

#### <a name="azure-storage-linked-service"></a>Azure verknüpft Speicherdienst
Zunächst erstellen Sie einen verknüpften Dienst den Azure-Speicher zu verknüpfen, die von Azure HDInsight Cluster Azure Data Factory verwendet wird. Wenn Sie folgenden Code kopieren, vergessen Sie nicht Namen und kontoschlüssel mit dem Namen und den Schlüssel der Azure-Speicher ersetzen. 

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
Anschließend erstellen Sie einen verknüpften Dienst Azure HDInsight Cluster Azure Data Factory verknüpfen. Wenn Sie den folgenden Code kopieren, der Name Ihres Clusters HDInsight HDInsight Clustername ersetzen und Werte für Benutzernamen und Kennwort ändern. 
    
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
Die Pipeline in diesem Beispiel übernimmt alle Eingaben. Sie geben ein ausgabedataset für HDInsight Streaming-Aktivität. Dieses Dataset ist ein dummy Dataset, die Pipeline Zeitplan Laufwerk erforderlich ist. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Pipeline

Die Pipeline in diesem Beispiel hat nur eine Aktivität vom Typ ist: **HDInsightStreaming**. 

HDInsight-Cluster wird mit Beispiel (wc.exe und cat.exe) und (davinci.txt) automatisch ausgefüllt. Name der Container, der HDInsight-Cluster ist standardmäßig der Name des Clusters. Beispielsweise ist der Clustername Myhdicluster wäre Name des Containers Blob zugeordnete Myhdicluster.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Siehe auch
- [Hive-Aktivität](data-factory-hive-activity.md)
- [Schwein-Aktivität](data-factory-pig-activity.md)
- [MapReduce-Aktivität](data-factory-map-reduce.md)
- [Spark Programme aufrufen](data-factory-spark.md)
- [R-Skripts aufrufen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

