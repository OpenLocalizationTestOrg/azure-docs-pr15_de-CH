<properties 
    pageTitle="Hive-Aktivität" 
    description="Erfahren Sie die Hive-Aktivität in einer Azure Daten Verwendung Hive-Abfragen in einem auf-Anforderung/Ihrem eigenen HDInsight Cluster ausgeführt." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Hive-Aktivität
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schweine](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten See Analytics U-SQL](data-factory-usql-activity.md)
[benutzerdefinierte .NET](data-factory-use-custom-activities.md)

HDInsight-Struktur Aktivität in einer Data Factory- [Pipeline](data-factory-create-pipelines.md) Struktur Abfragen auf [Eigene](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bei Bedarf](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-basierte HDInsight-Cluster ausgeführt. Dieser Artikel baut auf [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) Artikel stellt einen allgemeinen Überblick Datentransformations- und unterstützten Transformationsaktivitäten.

## <a name="syntax"></a>Syntax

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
        "inputs": [
          {
            "name": "input tables"
          }
        ],
        "outputs": [
          {
            "name": "output tables"
          }
        ],
        "linkedServiceName": "MyHDInsightLinkedService",
        "typeProperties": {
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Syntax-details

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Name | Name der Aktivität | Ja
Beschreibung | Beschreibung der Verwendungszweck der Aktivität | Nein
Typ | HDinsightHive | Ja
Eingaben | Die Hive-Aktivität verbraucht | Nein
Ausgaben | Ausgaben von der Struktur Aktivität erzeugt | Ja 
Nameverknüpfterdienst | Verweis auf HDInsight Cluster als verknüpfte Dienst in Data Factory registriert | Ja 
Skript | Geben Sie Inline-Skript Struktur | Nein
Pfad | Speichern Sie das Skript Struktur in Azure Blob-Speicher und den Pfad zu der Datei. Eigenschaft "Script" oder "ScriptPath" verwenden. Beide können nicht zusammen verwendet werden. Der Dateiname beachtet werden. | Nein 
definiert | Geben Sie Parameter als Schlüssel-Wert-Paare für Verweise innerhalb des Struktur Skripts 'Hiveconf'  | Nein

## <a name="example"></a>Beispiel

Betrachten wir Beispiel Spiel Protokolle Analytics für Ihre Zeit gestartet von Ihrem Unternehmen spielen Benutzer identifizieren. 

Das folgende Protokoll ist ein Spiel Beispielprotokoll, das Komma (`,`) getrennt und enthält die folgenden Felder-ID SessionStart, Dauer, SrcIPAddress und GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Das **Skript Struktur** dieser Daten verarbeiten:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

Zum Ausführen dieses Skripts Struktur in einer Pipeline Data Factory müssen Sie Folgendes

1. Erstellen Sie verknüpften Dienst registrieren [Eigene HDInsight compute Cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [auf Anforderung HDInsight compute Cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)konfigurieren. Nennen Sie diesen verknüpften Dienst "HDInsightLinkedService".
2. [Verknüpfte Dienst](data-factory-azure-blob-connector.md) zur Konfiguration der Verbindung zu Azure BLOB-Speicher der Daten erstellen. Nennen Sie diesen verknüpften Dienst "StorageLinkedService"
3. [Datasets](data-factory-create-datasets.md) auf die Eingabe und die Ausgabe von Daten erstellen. Eingabe-Dataset "HiveSampleIn" nennen und das ausgabedataset "HiveSampleOut"
4. Kopie der Hive-Abfrage als Datei in Azure BLOB-Speicher auf Schritt #2 konfiguriert. ist der Speicher für die Daten anders Hosting-Abfragedatei erstellen Sie separaten Dienst Azure Storage verknüpft und in der Aktivität auf. Verwenden Sie **ScriptPath **an den Pfad zur Strukturdatei und **ScriptLinkedService** an den Azure-Speicher, der die Datei enthält. 

    > [AZURE.NOTE] Struktur Skript Inline in der Aktivitätsdefinition bieten mit der **Skript** -Eigenschaft. Wir empfehlen nicht dadurch alle Sonderzeichen in das Skript JSON-Dokument muss mit Escapezeichen versehen werden und kann beim Debuggen von Problemen. Am besten ist Schritt #4.
5.  Erstellen Sie eine Rohrleitung mit der HDInsightHive. Die Aktivität der Prozesse/Transformationen der Daten.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  Bereitstellen der Pipeline. Siehe [Erstellen von Pipelines](data-factory-create-pipelines.md) Artikel. 
7.  Überwachen der Pipeline über die Überwachung und Verwaltung Datenansichten von Factory. Siehe [Überwachen und Verwalten von Data Factory Rohrleitungen](data-factory-monitor-manage-pipelines.md) Artikel. 


## <a name="specifying-parameters-for-a-hive-script"></a>Parametereingabe für Hive-Skript  
Spiel Protokolle täglich in Azure BLOB-Speicher aufgenommen und in einem Ordner mit Datum und Uhrzeit partitioniert werden. Sie möchten parametrisieren Hive-Skript input Ordner dynamisch während der Laufzeit übergeben und auch mit Datum und Uhrzeit partitionierte erstellen.

Gehen Sie folgendermaßen vor, um parametrisierte Hive-Skript verwenden

- Definieren Sie die Parameter in **definiert**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- Finden Sie im Skript Struktur des Parameters **${Hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Siehe auch
- [Schwein-Aktivität](data-factory-pig-activity.md)
- [MapReduce-Aktivität](data-factory-map-reduce.md)
- [Hadoop Streaming-Aktivität](data-factory-hadoop-streaming-activity.md)
- [Spark Programme aufrufen](data-factory-spark.md)
- [R-Skripts aufrufen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









