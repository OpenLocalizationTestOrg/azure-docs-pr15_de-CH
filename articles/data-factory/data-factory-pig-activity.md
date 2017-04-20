<properties 
    pageTitle="Schwein-Aktivität" 
    description="Erfahren Sie wie Schwein Aktivität in einer Azure Daten verwenden, einen auf-Anforderung/Ihre eigenen HDInsight-Cluster Schwein Skripts auszuführen." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Schwein-Aktivität
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schweine](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten See Analytics U-SQL](data-factory-usql-activity.md)
[benutzerdefinierte .NET](data-factory-use-custom-activities.md)

HDInsight Schwein Aktivität in einer Data Factory- [Pipeline](data-factory-create-pipelines.md) Schwein Abfragen auf [Eigene](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bei Bedarf](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-basierte HDInsight-Cluster ausgeführt. Dieser Artikel baut auf [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) Artikel stellt einen allgemeinen Überblick Datentransformations- und unterstützten Transformationsaktivitäten.

## <a name="syntax"></a>Syntax

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
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
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Syntax-details

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Name | Name der Aktivität | Ja
Beschreibung | Beschreibung der Verwendungszweck der Aktivität | Nein
Typ | HDinsightPig | Ja
Eingaben | Mindestens eine Aktivität Schwein verbrauchte Vorleistungen | Nein
Ausgaben | Eine oder mehrere Ausgaben von der Schweine Aktivität erzeugt | Ja
Nameverknüpfterdienst | Verweis auf HDInsight Cluster als verknüpfte Dienst in Data Factory registriert | Ja
Skript | Schwein Skript Inline angeben | Nein
Pfad | Speichern Sie das Skript Schweine in Azure Blob-Speicher und den Pfad zu der Datei. Eigenschaft "Script" oder "ScriptPath" verwenden. Beide können nicht zusammen verwendet werden. Der Dateiname beachtet werden. | Nein
definiert | Geben Sie Parameter als Schlüssel-Wert-Paare für Verweise innerhalb des Skripts Schwein | Nein

## <a name="example"></a>Beispiel

Betrachten wir ein Beispiel Spiel Protokolle Analytics für Ihre Zeit Spieler spielen gestartet von Ihrem Unternehmen identifizieren.
 
Das folgende Beispiel Spiel ist eine Komma (,) getrennt. Es enthält die folgenden Felder-ID SessionStart, Dauer, SrcIPAddress und GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Das **Schwein Skript** zum Verarbeiten:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Zum Ausführen dieses Skripts Schwein in einer Pipeline Data Factory folgendermaßen Sie vor:

1. Erstellen Sie verknüpften Dienst registrieren [Eigene HDInsight compute Cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [auf Anforderung HDInsight compute Cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)konfigurieren. Nennen Sie diesen Dienst verknüpften **HDInsightLinkedService**.
2.  [Verknüpfte Dienst](data-factory-azure-blob-connector.md) zur Konfiguration der Verbindung zu Azure BLOB-Speicher der Daten erstellen. Nennen Sie diesen Dienst verknüpften **StorageLinkedService**.
3.  [Datasets](data-factory-create-datasets.md) auf die Eingabe und die Ausgabe von Daten erstellen. Nennen Sie die Eingabedatasets **PigSampleIn** und das ausgabedataset **PigSampleOut**.
4.  Kopieren Sie die Schwein Abfrage in einer Datei Azure BLOB-Speicher in Schritt #2 konfiguriert. Ist der Azure-Speicher, der die Daten anders, die Datei hostet, erstellen Sie eine separate verknüpfte Azure Storage-Diensts. Zu verknüpften Aktivitätskonfiguration verweisen. Verwenden Sie **ScriptPath **an den Pfad zur Skriptdatei Schwein und **ScriptLinkedService**. 
    
    > [AZURE.NOTE] Schwein Skript Inline in der Aktivitätsdefinition bieten mit der **Skript** -Eigenschaft. Allerdings sollten nicht dadurch alle Sonderzeichen im Skript müssen mit Escapezeichen versehen werden und kann beim Debuggen von Problemen. Am besten ist Schritt #4.
5. Erstellen Sie die Pipeline mit der HDInsightPig. Diese Aktivität verarbeitet die Eingabedaten Schwein Skript auf HDInsight Cluster ausführen.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. Bereitstellen der Pipeline. Siehe [Erstellen von Pipelines](data-factory-create-pipelines.md) Artikel. 
7. Überwachen der Pipeline über die Überwachung und Verwaltung Datenansichten von Factory. Siehe [Überwachen und Verwalten von Data Factory Rohrleitungen](data-factory-monitor-manage-pipelines.md) Artikel.

## <a name="specifying-parameters-for-a-pig-script"></a>Parameter für ein Schwein Skript angeben 

Beispiel: Spiel Protokolle täglich in Azure BLOB-Speicher aufgenommen und in einem Ordner partitionierte basierend auf Datum und Uhrzeit gespeichert. Sie möchten parametrisieren Schwein Skript input Ordner dynamisch während der Laufzeit übergeben und auch mit Datum und Uhrzeit partitionierte erstellen.
 
Um parametrisierte Schwein Skript zu verwenden, führen Sie folgende Schritte aus:

- Definieren Sie die Parameter in **definiert**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- Finden Sie im Skript Schwein in den Parameter '**$parameterName**' verwenden, wie im folgenden Beispiel gezeigt:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Siehe auch
- [Hive-Aktivität](data-factory-hive-activity.md)
- [MapReduce-Aktivität](data-factory-map-reduce.md)
- [Hadoop Streaming-Aktivität](data-factory-hadoop-streaming-activity.md)
- [Spark Programme aufrufen](data-factory-spark.md)
- [R-Skripts aufrufen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


