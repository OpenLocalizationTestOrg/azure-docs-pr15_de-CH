Azure Data Factory unterstützt folgende Transformation, die mit Rohrleitungen entweder einzeln hinzugefügt oder verkettet werden mit einer anderen Aktivität.

Data Transformation Aktivität |  Berechnen der Umgebung 
:----------------------- | :--------------------
[Struktur](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Schweine](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop Streaming](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Maschine lernen: Batchausführung und Ressource](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure VM 
[Gespeicherte Prozedur](../articles/data-factory/data-factory-stored-proc-activity.md) | SQL Azure, SQL Azure Datawarehouse oder SQL Server |
[Data Lake Analytics U-SQL](../articles/data-factory/data-factory-usql-activity.md) | Azure Data Lake Analytics 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] oder Azure Batch
   
> [AZURE.NOTE] 
> MapReduce-Aktivität können Sie Funken im Cluster HDInsight Spark Programme. Einzelheiten Sie [aufrufen Spark Programme Azure Data Factory](../articles/data-factory/data-factory-spark.md) .
> Sie können eine benutzerdefinierte Aktivität zum R im HDInsight Cluster mit installierten Skripte erstellen. Siehe [Verwenden von Azure Data Factory R-Skript ausführen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).