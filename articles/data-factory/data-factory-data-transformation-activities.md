<properties 
    pageTitle=": Daten und Transformation Transformationsdaten | Microsoft Azure" 
    description="Informationen Sie zum Transformieren von Daten oder Daten in Azure Data Factory Hadoop, Computerlernen oder in Azure Data Lake Analytics." 
    keywords="Datentransformationen, Daten, Data Transformation Aktivität transformieren"
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
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Transformieren von Daten in Azure Data Factory
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schweine](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten See Analytics U-SQL](data-factory-usql-activity.md)
[benutzerdefinierte .NET](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>Übersicht 
Dieser Artikel beschreibt Data Transformationsaktivitäten in Azure Data Factory können Sie transformieren und verarbeitet die Rohdaten Vorhersagen und Einblicke. In einer Umgebung Azure Batch oder Azure HDInsight Cluster führt eine Transformation Aktivität. Ausführliche Informationen zu jeder Transformation Aktivität bietet Hyperlinks zu Artikeln.
 
Data Factory unterstützt folgende Daten Transformation, die mit [Rohrleitungen](data-factory-create-pipelines.md) entweder einzeln hinzugefügt oder verkettet werden mit einer anderen Aktivität.

> [AZURE.NOTE] Eine exemplarische Vorgehensweise mit einer schrittweisen Anleitung finden Sie im Artikel [Erstellen Sie eine Rohrleitung mit Struktur](data-factory-build-your-first-pipeline.md) .  

## <a name="hdinsight-hive-activity"></a>HDInsight-Struktur-Aktivität
HDInsight Struktur Aktivität in einer Pipeline Data Factory führt Abfragen eigene Struktur oder bei Bedarf Windows, Linux-basierte HDInsight-Cluster. Siehe [Struktur Aktivität](data-factory-hive-activity.md) Artikel zu dieser Aktivität. 

## <a name="hdinsight-pig-activity"></a>HDInsight Schwein Aktivität
HDInsight Schwein Aktivität in einer Pipeline Data Factory führt Schwein Abfragen selbst oder bei Bedarf Windows, Linux-basierte HDInsight-Cluster. Siehe [Schwein Aktivität](data-factory-pig-activity.md) Artikel zu dieser Aktivität. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce-Aktivität
HDInsight MapReduce-Aktivität in einer Pipeline Data Factory MapReduce Programme selbst oder bei Bedarf Windows, Linux-basierte HDInsight-Cluster ausgeführt. Siehe [MapReduce Aktivität](data-factory-map-reduce.md) Artikel zu dieser Aktivität.

MapReduce-Aktivität können Sie Funken im Cluster HDInsight Spark Programme. Einzelheiten Sie [aufrufen Spark Programme Azure Data Factory](data-factory-spark.md) .

## <a name="hdinsight-streaming-activity"></a>Streaming-HDInsight Aktivität
HDInsight Streaming-Aktivität in einer Pipeline Data Factory Hadoop Streaming Programme selbst oder bei Bedarf Windows, Linux-basierte HDInsight-Cluster ausgeführt. Details zu dieser Aktivität finden Sie unter [HDInsight Streaming-Aktivität](data-factory-hadoop-streaming-activity.md) .

## <a name="machine-learning-activities"></a>Computer lernen
Azure Data Factory können Sie problemlos Pipelines erstellen, einen veröffentlichten Azure Machine Learning-Webdienst für Vorhersageanalysen. Die [Ausführung Aktivität](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) in Azure Data Factory-Pipeline verwenden, können Sie Computerlernen mit Webdiensten zum Vorhersagen auf den Daten im Stapel aufrufen.

Im Laufe der Zeit müssen Vorhersagemodelle Computer lernen bewerten Experimente mit neuen input Datasets einarbeiten. Nachdem Sie abschließend mit Umschulung möchten Sie Punktzahl Webdienst trainierte maschinelles lernen Modell aktualisieren. [Aktivitäten aktualisieren](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) können Sie den Webdienst neu ausgebildeten Modell aktualisieren.  

Finden Sie ausführliche Informationen über diesen Computer Lernaktivitäten [verwenden Computer lernen](data-factory-azure-ml-batch-execution-activity.md) . 

## <a name="stored-procedure-activity"></a>Gespeicherte Prozedur-Aktivität
Verwenden die SQL Server gespeicherte Prozedur Aktivität in einer Data Factory-Pipeline zum Aufrufen einer gespeicherten Prozedur in einem der folgenden Datenspeicher: Azure SQL-Datenbank Azure SQL Data Warehouse SQL Server-Datenbank in Ihrem Unternehmen oder einem Azure VM. Siehe [Gespeicherte Prozedur Aktivität](data-factory-stored-proc-activity.md) Artikel.  

## <a name="data-lake-analytics-u-sql-activity"></a>Daten See Analytics U-SQL-Aktivität
Daten See Analytics U-SQL-Aktivität führt U-SQL-Skript in einem Cluster Azure Data Lake Analytics. Siehe [Data Analytics U-SQL-Aktivität](data-factory-usql-activity.md) Artikel. 

## <a name="net-custom-activity"></a>Benutzerdefinierte Aktivität .NET
Benötigen Sie Daten transformieren, die Data Factory unterstützt wird, können Sie mit eigenen Verarbeitung eine benutzerdefinierte Aktivität erstellen und verwenden Sie die Aktivität in der Pipeline. Sie können benutzerdefinierte .NET Aktivität mit einer Azure Batch-Dienst oder ein Azure HDInsight Cluster konfigurieren. Siehe [benutzerdefinierte Aktivitäten](data-factory-use-custom-activities.md) Artikel. 

Sie können eine benutzerdefinierte Aktivität zum R im HDInsight Cluster mit installierten Skripte erstellen. Siehe [Verwenden von Azure Data Factory R-Skript ausführen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Berechnen der Umgebung
Sie verknüpften Dienst für die Compute-Umgebung erstellen und Festlegung eine Transformation Aktivität verknüpften Dienst verwenden. Es gibt zwei Arten von Compute-Umgebung von Data Factory unterstützt. 

1. **Auf Anforderung**: In diesem Fall die Umgebung vollständig von Data Factory verwaltet. Es ist Data Factory-Dienst automatisch ein Auftrag übermittelten Daten und entfernt, wenn der Auftrag abgeschlossen ist. Konfigurieren und präzise Einstellungen der Umgebung bei Bedarf Compute Auftragsausführungsergebnisse Clusterverwaltung und bootstrapping Aktionen. 
2. **Bring Your Own**: In diesem Fall eigene Umgebung (z. B. HDInsight Cluster) als verknüpfte Dienst in Data Factory registrieren. Die Umgebung von Ihnen verwalteten und der Data Factory verwendet zum Ausführen von Aktivitäten. 

Siehe [berechnen verknüpften](data-factory-compute-linked-services.md) Artikel Informationen berechnen von Data Factory unterstützten Dienste. 


## <a name="summary"></a>Zusammenfassung
Azure Data Factory unterstützt Data Transformationsaktivitäten und Compute-Umgebung für die Aktivitäten. Die Transformationsaktivitäten können in Rohrleitungen entweder einzeln hinzugefügt oder mit anderen Aktivitäten verkettet.

Data Transformation Aktivität |  Berechnen der Umgebung 
:----------------------- | :--------------------
[Struktur](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Schweine](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Maschine lernen: Batchausführung und Ressource](data-factory-azure-ml-batch-execution-activity.md) | Azure VM 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md) | SQL Azure, SQL Azure Datawarehouse oder SQL Server |
[Data Lake Analytics U-SQL](data-factory-usql-activity.md) | Azure Data Lake Analytics 
[DotNet](data-factory-use-custom-activities.md) | HDInsight [Hadoop] oder Azure Batch
   

