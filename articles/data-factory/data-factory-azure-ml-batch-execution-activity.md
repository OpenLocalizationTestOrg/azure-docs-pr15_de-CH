<properties 
    pageTitle="Verwenden Sie Computer lernen | Microsoft Azure" 
    description="Beschreibt das Erstellen von Azure Data Factory mit Azure Machine Learning vorhersehbaren Pipelines erstellen" 
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
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Erstellen von vorhersehbaren Rohrleitungen Azure Computer lernen   
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schweine](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten See Analytics U-SQL](data-factory-usql-activity.md)
[benutzerdefinierte .NET](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Einführung

[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) ermöglicht das Erstellen, testen und Bereitstellen von Vorhersageanalysen Solutions. Aus allgemeinen Sicht geschieht dies in drei Schritten: 

1. **Erstellen eines trainingsexperiments**. Mithilfe von Azure ML Studio führen Sie diesen Schritt. ML Studio ist eine gemeinsame visuelle Entwicklung, die Sie verwenden und Vorhersageanalytik Modelle mit Hilfe von Trainingsdaten testen.
2. **Konvertieren einer vorhersageexperiment**. Modell mit vorhandenen Daten ausgebildet und Sie können es verwenden, um neue Daten vorbereiten und optimieren das Experiment zur Bewertung.
3. **Als Web Service bereitstellen**. Sie können Ihre Punktzahl Experiment als Azure Webdienst veröffentlichen. Sie können Ihr Modell über dieses Web Service-Endpunkt senden und empfangen Vorhersagen für Modell.  

Azure Data Factory können Sie problemlos Pipelines erstellen, veröffentlichten [Azure Machine Learning] [ azure-machine-learning] Webdienst für Vorhersageanalysen. Finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md) und [Erstellen Sie Ihre erste Pipeline](data-factory-build-your-first-pipeline.md) Artikel mit dem Azure Data Factory Schnelleinstieg. 

Die **Ausführung Aktivität** in Azure Data Factory-Pipeline verwenden, können Sie einen Webdienst Azure ML um vorhersagen auf den Daten im Stapel aufrufen. [Aufrufen von Azure ML Webdienst mithilfe der Aktivität Ausführung](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) Siehe Details.

Im Laufe der Zeit müssen Vorhersagemodelle in Azure ML Bewertung Experimente mit neuen input Datasets einarbeiten. Sie können ein Modell Azure ML Data Factory-Rohrleitung umschulen, führen Sie die folgenden Schritte aus: 

1. Veröffentlichen Sie trainingsexperiment (nicht vorhersageexperiment) als Webdienst. Wie Sie vorhersageexperiment als Webdienst im vorherigen Szenario verfügbar machen, wiederholen Sie diesen Schritt in Azure ML Studio.
2. Verwenden der Azure ML Ausführung Aktivität aufzurufende Webdienst für das trainingsexperiment. Azure ML Batchausführung Aktivität können Sie grundsätzlich Webdienst Schulung und bewertet Webdienst aufrufen. 
  
Nachdem Sie Umschulung abgeschlossen haben, möchten Sie Punktzahl Webdienst (predictive Experiment als Webdienst verfügbar gemacht) neu ausgebildeten Modell aktualisieren. Hier sind die Schritte: 

1. Punktwertung Webdienst einen nicht standardmäßigen Endpunkt hinzufügen. Der Standardendpunkt des Webdiensts kann aktualisiert werden, müssen Sie einen nicht standardmäßigen Endpunkt unter Verwendung des Azure-Portals erstellen. Die [Endpunkte erstellen](../machine-learning/machine-learning-create-endpoint.md) Siehe konzeptionelle Informationen und Verfahrensschritte.
2. Aktualisierung vorhandener Azure ML verknüpft, Dienste für die Verwendung der Standardendpunkt bewerten. Starten Sie mit den neuen Endpunkt der Webdienst, der aktualisiert wird.
3. Verwenden Sie **Azure ML aktualisieren Aktivitäten** den Webdienst neu ausgebildeten Modell aktualisieren.  

[Aktualisieren von Azure ML Modelle Ressource Aktualisierungsaktivitäten](#updating-azure-ml-models-using-the-update-resource-activity) Siehe Details. 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Aufrufen eines Webdienstes mit Ausführung Aktivität

Sie verwenden Azure Data Factory Datentransfer und Verarbeitung orchestrieren und führen Sie mit Azure Machine Learning Batchausführung. Hier sind Top-Level-Schritte:

1. Erstellen Sie einen Dienst Azure Machine Learning verknüpft. Sie benötigen Folgendes:
    1. **Request-URI** für die Batchausführung API. Request-URI finden Sie in der Web-Seite auf **Die BATCHAUSFÜHRUNG** .
    1. **API-Schlüssel** für den veröffentlichten Azure Machine Learning-Webdienst. Sie finden den API-Schlüssel des Webdienstes veröffentlicht haben. 
 2. Verwenden Sie die **AzureMLBatchExecution** Aktivität.

    ![Computer lernen Dashboard](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![Batch URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Szenario: Experimente mit Web Service ein-und Ausgänge, die Daten in Azure BLOB-Speicher
In diesem Szenario Azure Machine Learning-Webdienst macht Vorhersagen mit Daten aus einer Datei in eine Azure BLOB-Speicher und speichert die Vorhersageergebnisse im BLOB-Speicher. Die folgende JSON definiert eine Data Factory-Pipeline mit einer AzureMLBatchExecution-Aktivität. Die Aktivität ist das Dataset **DecisionTreeInputBlob** als Eingabe und Ausgabe **DecisionTreeResultBlob** . **DecisionTreeInputBlob** wird als Eingabe an den Webdienst übergeben, mit der **WebServiceInput** JSON-Eigenschaft. **DecisionTreeResultBlob** wird als Ausgabe an den Webdienst übergeben, mit der JSON-Eigenschaft **WebServiceOutputs** .  

> [AZURE.IMPORTANT] 
> Wenn der Webdienst mehrere Eingaben akzeptiert, verwenden Sie die **WebServiceInputs** -Eigenschaft anstelle von **WebServiceInput**. Siehe Abschnitt [Webdienst erfordert mehrere Eingaben](#web-service-requires-multiple-inputs) beispielsweise mithilfe der WebServiceInputs-Eigenschaft.
>  
> Datasets **WebServiceInput referenziert**/**WebServiceInputs** und **WebServiceOutputs** Eigenschaften ( **TypeProperties**) müssen auch in die Aktivität **Eingaben** und **Ausgaben**enthalten.
> 
> In das Experiment Azure ML haben Web Service Eingangs- und Ausgangsports und globale Parameter Standardnamen ("input1", "input2"), die Sie anpassen können. Die Namen für WebServiceInputs und WebServiceOutputs sowie GlobalParameters müssen exakt die Namen in den Experimenten. Sie können die Anforderungsnutzlast Beispiel auf Batch Execution-Hilfeseite für Ihren Azure ML Endpunkt erwartende Zuordnung überprüfen anzeigen. 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] Nur Eingaben und Ausgaben der AzureMLBatchExecution Aktivität können als Parameter an den Webdienst übergeben werden. In den oben stehenden Ausschnitt JSON ist DecisionTreeInputBlob z. B. Eingabe in der AzureMLBatchExecution-Aktivität ' WebServiceInput '-Parameter als Eingabe an den Webdienst übergeben wird.   

### <a name="example"></a>Beispiel

Dieses Beispiel verwendet Azure Storage für Eingabe- und Daten. 

Wir empfehlen Ihnen durch [Erstellen Sie Ihre erste Pipeline mit Daten] [ adf-build-1st-pipeline] vor diesem Beispiel Tutorial. Mit der Data Factory Artefakte (verknüpften Diensten, Datasets Pipeline) in diesem Beispiel erstellt der Factory-Editor.   
 

1. Erstellen eines **verknüpften Serviceartikel** für Ihren **Azure-Speicher**. Wenn die Eingabe- und Dateien in verschiedenen Konten, benötigen Sie zwei verknüpften Dienste. Hier ist ein JSON-Beispiel:

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. Erstellen der **Eingabe** Azure Data Factory **Dataset**. Im Gegensatz zu einigen anderen Data Factory-Datasets müssen diese Datasets **Ordnerpfad** und **Dateiname** Werte enthalten. Können die Partitionierung zu jeder Batchausführung (jede Datenslice) verarbeiten oder eindeutige Eingabe erzeugen und Ausgabedateien. Sie müssen einige upstream-Aktivität, um die Eingabe in das CSV-Dateiformat umwandeln und in das Speicherkonto für jedes Segment enthalten. In diesem Fall schließen Sie nicht die Einstellung **externe** und **ExternalData** im folgenden Beispiel gezeigt und Ihr DecisionTreeInputBlob wäre das ausgabedataset einer anderen Aktivität.

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }
    
    Ihre Eingabe CSV-Datei muss die Spaltenkopfzeile. Wenn Sie die **Kopieraktivität** um zu erstellen im CSV-Format in den blobspeicher verwenden, sollten Sink-Eigenschaft **BlobWriterAddHeader** auf **true**festgelegt werden. Zum Beispiel:
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    Haben die CSV-Datei nicht die Kopfzeile, möglicherweise folgende Fehlermeldung angezeigt: Fehler in Aktivität **: Fehler beim Lesen der Zeichenfolge. Unerwartetes Token: StartObject. Pfad '', Zeile 1, position 1**.
3. **Ausgabe** Azure Data Factory **Dataset**erstellen. Dieses Beispiel erstellt einen eindeutigen Ausgabepfad für jede Ausführung Slice verwendet Partitionierung. Ohne die Partitionierung wird die Aktivität die Datei überschrieben.

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. Erstellen Sie einen **verknüpften Dienst** vom Typ: **AzureMLLinkedService**, bietet die API Schlüssel und model Batch Execution URL.
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. Schließlich erstellen Sie eine Rohrleitung mit einer **AzureMLBatchExecution** -Aktivität. Zur Laufzeit Pipeline folgende Schritte ausgeführt:
    1. Ruft den Speicherort der Eingabedatei aus Ihrem Eingang Datasets ab.
    2. Ruft die Batchausführung Azure Machine Learning API
    3. Ausgabe der Batch kopiert in das ausgabedataset angegebenen BLOB. 

    > [AZURE.NOTE] AzureMLBatchExecution Aktivität können NULL oder mehrere Eingaben und eine oder mehrere Ausgaben.

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Start** und **Ende** Datumswerte müssen im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601)sein. Beispiel: 2014-10-14T16:32:41Z. **Die Endzeit** ist optional. Wenn kein Wert für die **End** -Eigenschaft angeben, wird es als "**Start + 48 Stunden.**" Führen Sie die Pipeline unbegrenzt Geben Sie **9999-09-09** als Wert für die **End** -Eigenschaft. Informationen zum JSON-Eigenschaften finden Sie in [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) .

    > [AZURE.NOTE] Festlegen Eingaben für die AzureMLBatchExecution ist Aktivität optional. 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Szenario: Experimente mit Leser-Schreiber-Module verweisen auf Daten in verschiedenen Speicher

Ein weiteres gängiges Szenario Erstellung Azure ML Experimente ist Reader und Writer Module verwendet werden. Reader-Modul wird verwendet, um Daten in einem Experiment zu laden und das Writer-Modul ist zum Speichern von Daten aus der. Einzelheiten zu den Reader und Writer finden Sie auf MSDN Library [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) und [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) Themen.     

Reader und Writer-Module verwenden, wird empfohlen einen Webdienstparameter für jede Eigenschaft dieser Reader/Writer-Module verwenden. Diese Web-Parameter können Sie die Werte zur Laufzeit zu konfigurieren. Sie können z. B. Versuch erstellen, mit einem Reader, der einer Azure SQL-Datenbank verwendet: XXX.database.windows.net. Nach der Bereitstellung des Webdiensts, der Verbraucher des Webdiensts an anderen Azure SQL Server YYY.database.windows.net aufgerufen werden soll. Einen Web Service-Parameter können Sie dieser Wert konfiguriert werden.

> [AZURE.NOTE] Webdienstparameter unterscheiden Web Service ein- und Ausgabe. Im ersten Szenario haben Sie gesehen, wie eine Eingabe und Ausgabe für eine Azure ML-Webdienst angegeben werden. In diesem Szenario übergeben Sie Parameter für einen Webdienst entsprechen Eigenschaften Reader-/Writer-Module. 

Betrachten Sie ein Szenario für die Verwendung von Web Service-Parameter. Ein bereitgestellter Azure Machine Learning Web Service, der Reader Modul verwendet zum Lesen von Daten aus Datenquellen von Azure Machine Learning unterstützt (z. B.: Azure SQL-Datenbank). Nachdem die Stapelverarbeitung ausgeführt wird, werden die Ergebnisse mit Writer-Modul (Azure SQL-Datenbank) geschrieben.  Keine Web Service Eingaben und Ausgaben werden in die Versuche definiert. In diesem Fall empfiehlt relevante Webdienstparameter der Reader und Writer-Module konfigurieren. Diese Konfiguration ermöglicht der Leser/Schreiber Module konfiguriert werden, wenn die AzureMLBatchExecution Aktivität. Sie geben Webdienstparameter im Abschnitt **GlobalParameters** Aktivität JSON wie folgt. 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

[Factory Funktionen](https://msdn.microsoft.com/library/dn835056.aspx) können Werte für die Webdienstparameter, wie im folgenden Beispiel übergeben:

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Die Web Service-Parameter beachtet, so sicherstellen, dass die Namen in der Aktivität angegebenen JSON die vom Webdienst verfügbar gemacht. 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Mithilfe eines Moduls Reader zum Lesen von Daten aus mehreren Dateien in Azure Blob
Big Data Rohrleitungen mit Schwein kann und Struktur eine oder mehrere Ausgabedateien ohne Erweiterung. Beispielsweise können die Daten für die externe Tabelle Struktur bei Angabe eine externe Tabelle Struktur in Azure BLOB-Speicher mit folgenden Namen 000000_0 gespeichert. Sie können mithilfe des Reader in einem Experiment mehrere Dateien lesen und für Vorhersagen verwenden. 

Bei Reader-Modul in einem Experiment Azure Machine Learning können Sie Azure Blob als Eingabe. Dateien in Azure BLOB-Speicher kann die Ausgabedateien (Beispiel: 000000_0) durch ein Skript Schwein und Struktur auf HDInsight entstehen. Reader-Modul können Sie zum Lesen von Dateien (ohne Erweiterung) durch Konfigurieren der **Pfad Container Verzeichnis-Blob**. **Container** Pfadpunkte auf Container und **Directory-Blob** verweist auf Ordner, die Dateien enthält, wie in der folgenden Abbildung dargestellt. Das Sternchen, also \*) **Gibt an, dass alle Dateien in der Containerordner (also Daten Aggregateddata Jahr 2014/Monat-6 = /\*)** im Rahmen des Experiments lesen.

![Azure BLOB-Eigenschaften](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>Beispiel 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Pipeline mit AzureMLBatchExecution mit Webdienstparametern

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
Im obigen JSON-Beispiel:

- Der bereitgestellte Azure Machine Learning-Webdienst verwendet einen Reader und Writer-Modul Daten aus und in einer Azure SQL-Datenbank schreibgeschützt. Dieser Webdienst macht die folgenden vier Parameter: Servername, Datenbankname, Server Benutzerkontonamen und Benutzerkennwort Server-Datenbank.  
- **Start** und **Ende** Datumswerte müssen im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601)sein. Beispiel: 2014-10-14T16:32:41Z. **Die Endzeit** ist optional. Wenn kein Wert für die **End** -Eigenschaft angeben, wird es als "**Start + 48 Stunden.**" Führen Sie die Pipeline unbegrenzt Geben Sie **9999-09-09** als Wert für die **End** -Eigenschaft. Informationen zum JSON-Eigenschaften finden Sie in [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) .

### <a name="other-scenarios"></a>Andere Szenarien

#### <a name="web-service-requires-multiple-inputs"></a>Webdienst erfordert mehrere Eingaben
Wenn der Webdienst mehrere Eingaben akzeptiert, verwenden Sie die **WebServiceInputs** -Eigenschaft anstelle von **WebServiceInput**. Datasets, die von der **WebServiceInputs** verwiesen wird, muss auch in Aktivität **Eingaben**enthalten.
 
In das Experiment Azure ML haben Web Service Eingangs- und Ausgangsports und globale Parameter Standardnamen ("input1", "input2"), die Sie anpassen können. Die Namen für WebServiceInputs und WebServiceOutputs sowie GlobalParameters müssen exakt die Namen in den Experimenten. Sie können die Anforderungsnutzlast Beispiel auf Batch Execution-Hilfeseite für Ihren Azure ML Endpunkt erwartende Zuordnung überprüfen anzeigen.


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Webdienst erfordert keine Eingabe

Alle Workflows für Beispiel R oder Python-Skripten ausführen, die keine Eingaben erfordern können Azure ML Batch Execution Webdienste verwendet werden. Oder Versuchs so konfiguriert ist, mit einem Reader-Modul, die keine GlobalParameters verfügbar macht. In diesem Fall würde die AzureMLBatchExecution Aktivität wie folgt konfiguriert:

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Webdienst erfordert ein Ausgang
Azure ML Batch Execution-Webdienst möglicherweise nicht Ausgabe Webdienst konfiguriert. In diesem Beispiel kein Webdienst Eingabe oder Ausgabe und alle GlobalParameters konfiguriert sind. Gibt es noch eine Ausgabe auf die Aktivität konfiguriert, aber nicht als ein WebServiceOutput angegeben.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web Service verwendet Leser und Schreiber und Aktivität führt nur bei anderen Aktivitäten gelungen

Die Azure ML Web Service Reader und Writer Module können konfiguriert werden mit oder ohne jede GlobalParameters. Sie möchten jedoch Service-Aufrufe in einer Pipeline einbetten, die Dataset Abhängigkeiten zum Aufrufen des Diensts erst einige upstream Verarbeitung verwendet. Nachdem die Stapelverarbeitung abgeschlossen ist mit diesem Ansatz können Sie auch eine andere Aktion auslösen. In diesem Fall können Sie die Abhängigkeit mit Aktivität Eingaben und Ausgaben, ohne diese als Webdienst Eingaben oder Ausgaben Ausdrücken.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

Die **Vorteile** sind:

-   Wenn Ihr Experiment Endpunkt einer WebServiceInput verwendet: BLOB-Dataset dargestellt und in die Aktivität Eingaben und die WebServiceInput-Eigenschaft enthalten. Andernfalls wird die WebServiceInput-Eigenschaft weggelassen. 
-   Wenn der Endpunkt Experiment WebServiceOutput(s) verwendet: sie werden BLOB-Datasets und sind in die Aktivität Ausgaben und die WebServiceOutputs-Eigenschaft. Die Aktivität gibt und WebServiceOutputs durch den Namen der einzelnen Ausgaben im Experiment zugeordnet. Andernfalls wird die Eigenschaft WebServiceOutputs ausgelassen.
-   Wenn Ihr Versuch GlobalParameter(s) Endpunkt, erhalten sie in der Aktivität GlobalParameters-Eigenschaft als Schlüssel-Wert-Paare. Andernfalls wird die Eigenschaft GlobalParameters ausgelassen. Die Schlüssel sind Groß-und Kleinschreibung berücksichtigt. [Azure Data Factory-Funktionen](data-factory-scheduling-and-execution.md#data-factory-functions-reference) können die Werte verwendet werden. 
- Zusätzliche Datensätze können in die Aktivitätseigenschaften Eingaben und Ausgaben ohne referenzierte TypeProperties Aktivität enthalten sein. Diese Datasets ausführen Slice Abhängigkeiten steuern aber durch die AzureMLBatchExecution ignoriert werden. 


## <a name="updating-models-using-update-resource-activity"></a>Aktualisieren von Modellen mit Aktivitäten aktualisieren
Im Laufe der Zeit müssen Vorhersagemodelle in Azure ML Bewertung Experimente mit neuen input Datasets einarbeiten. Nachdem Sie abschließend mit Umschulung möchten Sie Bewertungsmuster Webdienst trainierte ML Modell aktualisieren. Normale Vorgehensweise Umschulung und Aktualisierung Azure ML Modelle über Webdienste sind: 

1. Erstellen Sie Versuch in [Azure ML Studio](https://studio.azureml.net). 
2. Wenn Sie das Modell zufrieden, Azure ML Studio verwenden, um Webdienste für sowohl die **Schulung experimentieren** veröffentlichen und bewerten /**vorhersageexperiment**.

Die folgende Tabelle beschreibt die Webdienste in diesem Beispiel verwendet.  Einzelheiten finden Sie in der [Umschulung maschinelles lernen programmgesteuert modelliert](../machine-learning/machine-learning-retrain-models-programmatically.md) .

| Typ des Webdiensts | Beschreibung 
| :------------------ | :---------- 
| **Schulung-Webdienst** | Empfängt Daten und geschulten produziert. Die Ausgabe der Umschulung ist eine .ilearner-Datei in Azure Blob-Speicher.  Der **Standardendpunkt** wird automatisch erstellt, wenn trainingsexperiment als Webdienst veröffentlichen. Weitere Endpunkte erstellen, aber dabei nur der Standardendpunkt |
| **Webdienst bewerten** | Beispiele für unbezeichnete Daten empfängt und macht Vorhersagen. Die Ausgabe der Vorhersage möglicherweise verschiedene Formen Zeilen in SQL Azure-Datenbank je nach Konfiguration des Experiments oder eine CSV-Datei. Der Standardendpunkt wird automatisch erstellt, wenn Sie vorhersageexperiment als Webdienst veröffentlichen. Erstellen Sie den zweiten **Endpunkt nicht standardmäßigen und aktualisierbare** mithilfe der [Azure-Portal](https://manage.windowsazure.com). Weitere Endpunkte erstellen, aber dieses Beispiel nur einen nicht standardmäßigen Endpunkt aktualisiert. Die [Endpunkte erstellen](../machine-learning/machine-learning-create-endpoint.md) Siehe Schritte.       
 
Die folgende Abbildung zeigt die Beziehung zwischen Ausbildung und Bewertung von Endpunkten in Azure ML. 

![Webdienste](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


Zum Aufrufen der **Schulung Webdienst** mithilfe der **Azure ML Ausführung Aktivität**. Aufrufen eines Webdienstes Training entspricht einen Azure ML-Webdienst (scoring Webdienst) Punktzahl Daten aufrufen. In den vorherigen Abschnitten behandelt wie einen Azure ML-Webdienst aus einer Azure Data Factory-Pipeline im Detail aufrufen. 
  
Sie können **bewerten Webdienst** mithilfe von **Azure ML aktualisieren Aktivitäten** den Webdienst neu ausgebildeten Modell aktualisieren aufrufen. Wie in der obigen Tabelle müssen Sie erstellen und Verwenden von nicht-aktualisierbare Standardendpunkt. Darüber hinaus aktualisieren Sie vorhandene verknüpften Dienste im Werk Daten der Standardendpunkt verwenden, damit sie immer das neuesten trainierte Modell verwenden. 

Das folgende Szenario enthält weitere Informationen. Es wurde beispielsweise Umschulung und Azure ML Modelle von Azure Data Factory-Pipeline aktualisieren. 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Szenario: Umschulung und Azure ML-Modell aktualisieren
Dieser Abschnitt enthält eine Beispiel-Pipeline, die **Azure ML Batchausführung Aktivität** verwendet, um ein Modell trainieren. Die Pipeline verwendet auch **Azure ML aktualisieren Aktivitäten** zum Aktualisieren des Modells im Webdienst bewertet. Der Abschnitt enthält auch JSON Ausschnitte für alle verknüpften Diensten, Datasets und Pipeline im Beispiel. 

Hier ist die Diagrammansicht der Beispiel-Pipeline. Siehe, Azure ML Batch Execution Aktivität übernimmt die Schulung Eingabe und Ausgabe einer Schulung (iLearner Datei). Die Azure ML Ressource verwendet diese Schulung Ausgabe und das Modell Punktzahl Webdienstendpunkt aktualisiert. Das Update Aktivitäten erzeugt keine Ausgabe. Die PlaceholderBlob ist nur ein dummy ausgabedataset, das Azure Data Factory-Dienst erforderlich, die Pipeline ausgeführt. 

![Pipeline-Diagramm](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Azure BLOB-Speicher verknüpft Service:
Azure-Speicher enthält folgenden Daten:

- Trainingsdaten. Die Eingabedaten für den Webdienst Azure ML Training.  
- iLearner-Datei. Die Ausgabe von Azure ML Training-Webdienst. Diese Datei ist auch die Eingabe für die Ressource Aktivität.  
   
Hier ist die Definition Beispiel JSON verknüpfte Dienst: 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>Eingabedatasets Training:
Das folgende Dataset stellt die Eingabe Trainingsdaten für den Webdienst Azure ML Training. Die Aktivität Azure ML Batchausführung wird dieses Dataset als Eingabe verwendet. 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "policy": {          
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

#### <a name="training-output-dataset"></a>Trainingsdataset Ausgabe:
Das folgende Dataset stellt die Ausgabedatei iLearner von Azure ML Training-Webdienst dar. Die Aktivität Azure ML Ausführung erzeugt dieses Dataset. Dieses Dataset wird auch der Aktivität Azure ML Ressource.

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Verknüpfte Dienst für Azure ML Training Endpunkt 
Im folgenden Codeausschnitt JSON definiert eine Azure Machine Learning verknüpft, die der Standardendpunkt Training-Webdienst verweist. 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

Gehen Sie in **Azure ML Studio**zum Abrufen der Werte für **MlEndpoint** und **ApiKey**:

1. Klicken Sie im linken Menü auf **Webdienste** .
2. Klicken Sie auf **Webdienst Schulung** in der Liste der WebServices. 
3. Klicken Sie neben dem Textfeld **API-Schlüssel** kopieren. Fügen Sie den Schlüssel in der Zwischenablage Daten Factory JSON-Editor.
4. Klicken Sie in **Azure ML Studio** **BATCHAUSFÜHRUNG** auf.
5. **Request-URI** im Abschnitt **Anforderung** kopieren und Data Factory JSON-Editor einfügen.   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Verknüpfte Dienst für aktualisierbare Punktzahl Endpunkt Azure ML:
Der folgende Codeausschnitt JSON definiert eine Azure Machine Learning verknüpft, die auf nicht standardmäßige aktualisierbare Endpunkt der Punktzahl Webdienst verweist.  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


Vor dem Erstellen und Bereitstellen von Azure ML Dienst verknüpft, folgen Sie den Schritten [Endpunkte erstellen](../machine-learning/machine-learning-create-endpoint.md) Artikel zweite (nicht Standard und aktualisierbare) für die Punktwertung Webdienst erstellen.

Nach der Erstellung der nicht-aktualisierbare Standardendpunkt folgendermaßen Sie vor:

- Klicken Sie auf **Die BATCHAUSFÜHRUNG** um den URI-Wert für die Eigenschaft **MlEndpoint** JSON abzurufen.
- Klicken Sie **Ressource** , um den URI-Wert für die Eigenschaft **UpdateResourceEndpoint** JSON abzurufen. API-Schlüssel wird auf der Seite selbst (in der unteren rechten Ecke). 

![aktualisierbare Endpunkt](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>Ausgabe-Dataset Platzhalter:
Die Aktivität Azure ML Ressource generiert keine Ausgabe. Azure Data Factory erfordert jedoch ein ausgabedataset auf den Zeitplan der Rohrleitung. Verwenden Sie deshalb ein Dummy-Platzhalter Dataset in diesem Beispiel.  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>Pipeline
Die Pipeline hat zwei Aktivitäten: **AzureMLBatchExecution** und **AzureMLUpdateResource**. Azure ML Batchausführung Aktivität Daten als Eingabe akzeptiert und eine iLearner-Datei als Ausgabe erzeugt. Ruft den Webdienst Training (Schulung Experiment als Webdienst verfügbar gemacht) mit der eingegebenen Daten und der Webdienst empfängt die Ilearner-Datei. Die PlaceholderBlob ist nur ein dummy ausgabedataset, das Azure Data Factory-Dienst erforderlich, die Pipeline ausgeführt. 

![Pipeline-Diagramm](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>Reader und Writer-Module

Ein häufiges Szenario für die Verwendung von Web Service-Parameter ist die Verwendung von Azure SQL-Readern und Writern. Reader-Modul wird verwendet, um Daten in einem Experiment von Datenmanagement Services außerhalb Azure Machine Learning Studio laden. Das Writer-Modul ist in Data Management Services außerhalb Azure Machine Learning Studio Ihrer Tests speichern.  

Einzelheiten zu Azure Blob-Azure SQL-Leser/Schreiber finden Sie auf MSDN Library [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) und [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) Themen. Das Beispiel im vorherigen Abschnitt verwendet Azure BLOB-Reader und Writer Azure Blob. Dieser Abschnitt beschreibt mit SQL Azure Reader und Writer für SQL Azure.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Q:** Ich habe mehrere Dateien, die von meiner großen Datenpipelines generiert. Kann ich die AzureMLBatchExecution Aktivität alle Dateien zu verwenden?

**A:** Ja. Abschnitt **mit Modul Reader zum Lesen von Daten aus mehreren Dateien in Azure Blob** für Details. 

## <a name="azure-ml-batch-scoring-activity"></a>Azure ML Bewertung Aktivität
Verwenden Sie die **AzureMLBatchScoring** Aktivität Azure Machine Learning integrieren, empfehlen wir verwenden die neueste **AzureMLBatchExecution** Aktivität. 

Die AzureMLBatchExecution Aktivität wird in der Ausgabe August 2015 von Azure SDK und Azure PowerShell.

Wenn Sie weiterhin die AzureMLBatchScoring Aktivität verwenden möchten, lesen Sie diesen Abschnitt.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure ML Batch Bewertung Aktivität Azure Storage für Eingabe/Ausgabe 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>Webdienstparametern
Um Werte für Web Service-Parameter angeben, fügen Sie **TypeProperties** Abschnitt Abschnitt **AzureMLBatchScoringActivty** in der Pipeline JSON wie im folgenden Beispiel gezeigt: 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


[Factory Funktionen](https://msdn.microsoft.com/library/dn835056.aspx) können Werte für die Webdienstparameter, wie im folgenden Beispiel übergeben:

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Die Web Service-Parameter beachtet, so sicherstellen, dass die Namen in der Aktivität angegebenen JSON die vom Webdienst verfügbar gemacht. 

## <a name="see-also"></a>Siehe auch

- [Azure Blogbeitrag: Erste Schritte mit Azure Data Factory und Azure maschinelles lernen](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 
