<properties 
    pageTitle="Pipelines erstellen/Zeitplan, Data Factory Aktivitäten verkettet | Microsoft Azure" 
    description="Informationen Sie zum Erstellen einer Datenpipeline in Azure Data Factory und Transformieren von Daten. Erstellen eines datengesteuerten Workflows zu bereit, um Informationen zu verwenden." 
    keywords="Datenpipeline, datengesteuerten Workflows"
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

# <a name="pipelines-and-activities-in-azure-data-factory"></a>Pipelines und Aktivitäten in Azure Data Factory
Dieser Artikel hilft Ihnen zu Pipelines und Aktivitäten in Azure Data Factory und End-to-End datengesteuerten Workflows für Datentransfer und Datenverarbeitung Szenarien erstellen verwenden.  

> [AZURE.NOTE] Es wird vorausgesetzt, dass Sie [Einführung in Azure Data Factory](data-factory-introduction.md)durchgegangen sind. Wenn Sie keinen hands-on-Erfahrung mit Factorys durch [Erstellen Ihrer erste Data Factory](data-factory-build-your-first-pipeline.md) Lernprogramm dazu Daten erstellen verstehen dieser Artikel besser.  

## <a name="what-is-a-data-pipeline"></a>Was ist eine Datenpipeline?
**Pipeline** ist eine Gruppierung von logisch verwandten **Aktivitäten**. Es wird auf Gruppenaktivitäten in einer Einheit verwendet, die eine Aufgabe ausführt. Um Rohrleitungen besser zu verstehen, müssen Sie zunächst verstehen, eine Aktivität. 

## <a name="what-is-an-activity"></a>Was ist ein?
Aktivitäten definieren die Aktionen, die für Ihre Daten auszuführen. Jede Aktivität oder mehrere [Datasets](data-factory-create-datasets.md) als Eingabe akzeptiert und ein oder mehrere Datasets als Ausgabe erzeugt. 

Eine kopieraktivität können Sie orchestrieren von Daten aus einem Datenspeicher in einem anderen Datenspeicher. Eine HDInsight Struktur Aktivität können Sie ebenso Struktur auf einem Cluster Azure HDInsight Transformation die Daten abgefragt. Azure Data Factory bietet eine Vielzahl [Datentransformations-](data-factory-data-transformation-activities.md)und [Daten](data-factory-data-movement-activities.md) . Sie können auch eine benutzerdefinierte Aktivität .NET führen Sie eigenen Code erstellen. 

## <a name="sample-copy-pipeline"></a>Beispiel kopieren pipeline
In der folgenden Beispiel-Pipeline ist eine Aktivität vom Typ **Kopieren** im Abschnitt **Aktivitäten** . In diesem Beispiel kopiert [Projektvorgang kopieren](data-factory-data-movement-activities.md) Daten aus Azure Blob-Speicher mit einer Azure SQL-Datenbank. 

    {
      "name": "CopyPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    } 

Beachten Sie folgende Punkte:

- Im Abschnitt Aktivitäten ist nur eine Aktivität vom **Typ** **Kopieren**festgelegt ist.
- Eingabe für die Aktivität, **InputDataset** und Ausgabe für die Aktivität ist auf **OutputDataset**festgelegt.
- In Abschnitt **TypeProperties** **BlobSource** als Quelltyp angegeben und **SqlSink** Senke Dateityp angegeben.

Diese Pipeline erstellen umfassende, finden Sie unter [Einführung: Daten aus dem BLOB-Speicher mit SQL-Datenbank](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Beispiel Transformation pipeline
In der folgenden Beispiel-Pipeline ist eine Aktivität vom Typ **HDInsightHive** Abschnitt **Aktivitäten** . In diesem Beispiel transformiert [HDInsight Struktur Aktivität](data-factory-hive-activity.md) Daten von Azure Blob-Speicher durch eine Strukturdatei Skript in einem Cluster Azure HDInsight Hadoop ausführen. 

    {
        "name": "TransformPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }

Beachten Sie folgende Punkte: 

- Im Abschnitt Aktivitäten ist nur eine Aktivität vom **Typ** **HDInsightHive**festgelegt ist.
- Struktur-Skriptdatei **partitionweblogs.hql**ist in Azure Storage Konto (angegeben durch ScriptLinkedService als **AzureStorageLinkedService**bezeichnet) und im **Skript** -Ordner im Container **Adfgetstarted**gespeichert.
- Abschnitt **definiert** wird verwendet, um die Laufzeit Einstellungen, die Hive-Skript als Struktur Werte übergeben werden (z. B. ${Hiveconf: inputtable}, ${Hiveconf:partitionedtable}).

Diese Pipeline erstellen umfassende, finden Sie unter [Tutorial: Erstellen Sie Ihre erste Pipeline Hadoop Cluster mit Daten](data-factory-build-your-first-pipeline.md). 

## <a name="chaining-activities"></a>Verketten von Aktivitäten
Wenn Sie mehrere Aktivitäten in einer Pipeline und Ausgabe einer Aktivität nicht Eingabe einer anderen Aktivität, können die Aktivitäten Wenn Eingabedaten Segmente für Aktivitäten werden parallel ausgeführt. 

Mit ausgabedataset einer Aktivität als Eingabedatasets der Aktivität können Sie zwei Aktivitäten verkettet. Aktivitäten können in der gleichen Rohrleitung oder anderen Rohrleitungen werden. Die zweite Aktivität führt nur beim ersten erfolgreich abgeschlossen wurde. 

Betrachten Sie beispielsweise den folgenden Fall:
 
1.  Pipeline P1 hat Aktivität A1, der externen Eingabedatasets D1 erfordert und erzeugen **Ausgabe** -Dataset **D2**.
2.  Pipeline-P2 hat Aktivität A2, der erfordert **Eingabe** von Dataset **D2**und erzeugt ausgabedataset D3.
 
In diesem Szenario führt die Aktivität A1, wenn die externen Daten und die Häufigkeit des geplanten Verfügbarkeit erreicht.  Aktivität führt A2 beim geplanten Segmente aus D2 verfügbar und die geplante Verfügbarkeit Frequenz erreicht. Ist ein Fehler in einem Dataset D2 Segmente, läuft A2 nicht dieses bis verfügbar ist.

Ansicht:

![Verketten von zwei Aktivitäten](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Ansicht mit beide in derselben Rohrleitung: 

![Verketten von Aktivitäten in derselben Rohrleitung](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Weitere Informationen finden Sie unter [Planung und Ausführung](#chaining-activities). 

## <a name="scheduling-and-execution"></a>Planung und Ausführung
Bisher haben Sie verstanden, was Pipelines und Aktivitäten sind. Außerdem haben sie definierte und allgemeine sind der Aktivitäten in Azure Data Factory anzuzeigen. Wir betrachten Sie nun wie sie ausgeführt werden. 

Eine Pipeline ist zwischen der Start- und Endzeit. Vor dem Beginn oder nach Ende wird nicht ausgeführt. Wenn die Pipeline angehalten, ist es nicht unabhängig von der Start- und ausgeführt. Eine Pipeline ausführen sollten sie nicht angehalten werden. Tatsächlich ist es nicht die Pipeline ausgeführt wird. Die Aktivitäten in der Pipeline ausgeführt ist. Jedoch werden im Zusammenhang mit der Pipeline. 

Siehe [Planung und Ausführung](data-factory-scheduling-and-execution.md) Planung und Ausführung Funktionsweise in Azure Data Factory.

## <a name="create-pipelines"></a>Pipelines erstellen
Azure Data Factory bietet verschiedene Mechanismen zum Erstellen und Bereitstellen von Pipelines (die wiederum eine oder mehrere Aktivitäten darin enthalten). 

### <a name="using-azure-portal"></a>Mithilfe von Azure-portal
Im Azure-Portal können Data Factory-Editor Sie eine Pipeline erstellen. Eine End-to-End Exemplarische Vorgehensweise finden Sie unter [Erste Schritte mit Azure Data Factory (Data Factory-Editor)](data-factory-build-your-first-pipeline-using-editor.md) . 

### <a name="using-visual-studio"></a>Mithilfe von Visual Studio 
Können Sie Visual Studio und Rohrleitungen auf Azure Data Factory bereitstellen. Eine End-to-End Exemplarische Vorgehensweise finden Sie unter [Erste Schritte mit Azure Data Factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) . 

### <a name="using-azure-powershell"></a>Mithilfe von Azure PowerShell
Azure PowerShell können Rohrleitungen in Azure Data Factory erstellt werden. Die Pipeline JSON haben, in eine Datei c:\DPWikisample.json definiert werden. Sie können diese in Ihrer Azure Data Factory-Instanz hochladen, wie im folgenden Beispiel gezeigt:

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

Eine End-to-End Exemplarische Vorgehensweise zum Erstellen einer Data Factory mit einer Pipeline finden Sie unter [Erste Schritte mit Azure Data Factory (Azure PowerShell)](data-factory-build-your-first-pipeline-using-powershell.md) . 

### <a name="using-net-sdk"></a>.NET SDK verwenden
Sie erstellen und Bereitstellen der Pipeline über .NET SDK zu. Dieser Mechanismus kann verwendet werden, Pipelines programmgesteuert erstellen. Weitere Informationen finden Sie unter [erstellen, verwalten und überwachen Daten Factorys programmgesteuert](data-factory-create-data-factories-programmatically.md). 


### <a name="using-azure-resource-manager-template"></a>Azure Ressourcenmanager Vorlage
Erstellen und Bereitstellen Rohrleitung mit Hilfe einer Vorlage Azure-Ressourcen-Manager. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Data Factory (Azure Resource Manager)](data-factory-build-your-first-pipeline-using-arm.md). 

### <a name="using-rest-api"></a>Mithilfe von REST-API
Erstellen und Bereitstellen Pipeline REST-APIs zu verwenden. Dieser Mechanismus kann verwendet werden, Pipelines programmgesteuert erstellen. Weitere Informationen finden Sie unter [Erstellen oder Aktualisieren einer Rohrleitung](https://msdn.microsoft.com/library/azure/dn906741.aspx). 


## <a name="monitor-and-manage-pipelines"></a>Überwachen und Verwalten von pipelines  
Nach der Bereitstellung einer Pipeline können verwalten und Überwachen der Rohrleitungen, Segmente und führt. Weitere Informationen über hier: [Überwachen und Verwalten von Pipelines](data-factory-monitor-manage-pipelines.md).


## <a name="pipeline-json"></a>Pipeline JSON   
Wir näher auf die Definition einer Rohrleitung im JSON-Format. Die generische Struktur für eine Pipeline sieht wie folgt aus:

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

Abschnitt **Aktivitäten** können eine oder mehrere Aktivitäten definiert. Jede Aktivität hat die folgende Struktur auf oberster Ebene:

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

Folgende Tabelle beschreiben Sie die Eigenschaften in der Aktivität und Pipeline JSON-Definitionen:

Tag | Beschreibung | Erforderlich
--- | ----------- | --------
Name | Name der Aktivität oder der Pipeline. Geben Sie einen Namen, der die Aktion darstellt, die Tätigkeit oder die Pipeline konfiguriert ist<br/><ul><li>Maximale Anzahl Zeichen: 260</li><li>Muss mit einer Zahl Buchstaben oder einem Unterstrich (_) beginnen.</li><li>Folgende Zeichen sind nicht zulässig: ".", "+" und "?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> | Ja
Beschreibung | Beschreibung der Aktivität oder Pipeline Verwendungszweck | Ja
Typ | Gibt den Typ der Aktivität. Siehe Artikel [Datenaktivitäten](data-factory-data-movement-activities.md) und [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) für verschiedene Aktivitäten. | Ja
Eingaben | Von der Aktivität verwendete Tabellen<br/><br/>Tabelle mit einem Eingabefeld<br/>"Inputs": [{"Name": "inputtable1"}],<br/><br/>zwei Tabellen <br/>"Inputs": [{"Name": "inputtable1"}, {"Name": "inputtable2"}], | Ja
Ausgaben | Ausgabetabellen anhand der activity.// eine Ausgabetabelle<br/>"Ausgaben": [{"Name": "outputtable1"}],<br/><br/>zwei Ausgabetabellen<br/>"Ausgaben": [{"Name": "outputtable1"}, {"Name": "outputtable2"}], | Ja
Nameverknüpfterdienst | Name des verknüpften Serviceartikel Aktivität verwendet. <br/><br/>Eine Aktivität erfordern, dass Sie verknüpften Dienst angeben, der die Umgebung erforderlichen Compute verknüpft. | Ja, HDInsight und Azure maschinelles lernen Batch Aktivität bewerten <br/><br/>Nicht für alle anderen
typeProperties | Eigenschaften im Abschnitt TypeProperties hängen vom Typ der Aktivität ab. | Nein
Richtlinie | Richtlinien, die das Laufzeitverhalten der Aktivität auswirken. Wenn sie nicht angegeben wird, werden Standardrichtlinien verwendet. | Nein
Starten | Start Datum und Uhrzeit für die Pipeline. Muss im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601). Beispiel: 2014-10-14T16:32:41Z. <br/><br/>Es ist möglich, eine lokale Zeit, z. B. eine EST Zeit angeben. Beispiel: "2016-02-27T06:00:00**-05: 00**", also 6 AM EST.<br/><br/>Die Start- und End-Eigenschaften geben zusammen aktiven Zeitraum für die Pipeline. Ausgabe-Segmente werden nur in dieser active mit erzeugt. | Nein<br/><br/>Wenn Sie einen Wert für die End-Eigenschaft angeben, müssen Sie den Wert für die Start-Eigenschaft angeben.<br/><br/>Die Start- und Endzeiten können sowohl eine Pipeline erstellt leer. Geben Sie beide Werte zum Festlegen eines aktiven Zeitraums für die Pipeline ausgeführt. Wenn Sie keine Start- und Endzeiten angeben beim Erstellen einer Pipeline festlegen später mithilfe des Set-AzureRmDataFactoryPipelineActivePeriod-Cmdlet.
Ende | Ende Datum und Uhrzeit für die Pipeline. Wenn angegeben, muss im ISO-Format. Beispiel: 2014-10-14T17:32:41Z <br/><br/>Es ist möglich, eine lokale Zeit, z. B. eine EST Zeit angeben. Beispiel: "2016-02-27T06:00:00**-05: 00**", also 6 AM EST.<br/><br/>Führen Sie die Pipeline unbegrenzt Geben Sie 9999-09-09 als Wert für die End-Eigenschaft. | Nein <br/><br/>Wenn Sie einen Wert für die Start-Eigenschaft angeben, müssen Sie den Wert für die Endeigenschaft angeben.<br/><br/>Siehe Hinweise zur **start** -Eigenschaft.
isPaused | Wenn auf True die Pipeline nicht ausgeführt wird. Standardwert = False. Verwenden Sie diese Eigenschaft aktivieren oder deaktivieren. | Nein 
Planer | "Planer"-Eigenschaft wird zum Definieren der gewünschten Planung für die Aktivität verwendet. Untereigenschaften entsprechen die [Verfügbarkeit-Eigenschaft in einem Dataset](data-factory-create-datasets.md#Availability). | Nein |   
| pipelineMode | Die Methode für die Planung führt für die Pipeline. Zulässige Werte: geplant (Standard), einmalige.<br/><br/>"Geplant" bedeutet, dass die Pipeline in einem angegebenen Zeitintervall nach aktiven Zeitraums (Start- und Zeit) ausgeführt wird. "Einmalige" gibt an, dass die Pipeline nur einmal ausgeführt wird. Einmalige Rohrleitungen nach der Erstellung geändert bzw. aktualisiert zurzeit nicht möglich. Details ehemalige Einstellung finden Sie unter [Onetime-Pipeline](data-factory-scheduling-and-execution.md#onetime-pipeline) . | Nein | 
| expirationTime | Dauer nach dem für die Pipeline ist gültig und bleiben bereitgestellt. Keine aktiven Fehler oder ausstehende ausgeführt wird, die Pipeline automatisch erreicht die Ablaufzeit gelöscht. | Nein | 
| Datasets | Liste der Datasets von Aktivitäten in der Pipeline verwendet werden. Diese Eigenschaft kann verwendet werden, Datensätze definieren, die für diese Pipeline und nicht im Werk Daten definiert. Diese Pipeline definiert Datasets kann nur durch diese Pipeline verwendet und kann nicht freigegeben werden. Einzelheiten finden Sie unter [ausgelegte Datasets](data-factory-create-datasets.md#scoped-datasets) .| Nein |  
 

### <a name="policies"></a>Richtlinien
Richtlinien beeinflussen das Laufzeitverhalten einer Aktivität, insbesondere bei Teil einer Tabelle. Die folgende Tabelle enthält die Details.

Eigenschaft | Zulässige Werte | Standardwert | Beschreibung
-------- | ----------- | -------------- | ---------------
Parallelität | Ganze Zahl <br/><br/>Maximalwert: 10 | 1 | Anzahl der gleichzeitigen Ausführung der Aktivität.<br/><br/>Sie bestimmt die Anzahl der parallelen Aktivität Ausführung, die auf verschiedenen Bereichen auftreten können. Beispielsweise wenn eine Aktivität über muss beschleunigt eine Reihe von verfügbaren Daten mehr Parallelität Wert der Datenverarbeitung. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Bestimmt die Reihenfolge der Datenslices verarbeitet werden.<br/><br/>Beispielsweise haben Sie 2 Scheiben (eine passiert: 00 Uhr und eine weitere um 17 Uhr) und Ausführung sind. Festlegen von ExecutionPriorityOrder zu NewestFirst ist das Segment 5 Uhr zuerst verarbeitet. Entsprechend festlegen ExecutionPriorityORder zu OldestFIrst wird das Segment 4 Uhr verarbeitet. 
Wiederholen | Ganze Zahl<br/><br/>Max. kann 10 | 3 | Anzahl der Wiederholungsversuche vor der Verarbeitung für das Segment als Fehler gekennzeichnet. Ausführung der Aktivitäten für einen Datenslice wird wiederholt, bis die angegebene Anzahl. Die Wiederholung erfolgt so bald wie möglich nach dem Fehler.
Zeitlimit | TimeSpan | 00:00:00 | Timeout für die Aktivität. Beispiel: 00:10:00 (impliziert Timeout 10 Min.)<br/><br/>Wenn ein Wert nicht angegeben oder 0 ist, ist das Zeitlimit unendlich.<br/><br/>Datenverarbeitung Zeit auf den Timeoutwert überschreitet, er abgebrochen und das System versucht, die Verarbeitung zu wiederholen. Die Anzahl der Wiederholungsversuche hängt die Wiederholungseigenschaft. Timeout auftritt, wird der Status auf TimedOut festgelegt.
Verzögerung | TimeSpan | 00:00:00 | Geben Sie die Verzögerung vor der Verarbeitung der Speicherbereich beginnt.<br/><br/>Die Ausführung der Aktivität für einen Datenslice wird gestartet, nachdem die Verzögerung über die erwartete Ausführungszeit ist.<br/><br/>Beispiel: 00:10:00 (impliziert Verzögerung von 10 Minuten)
longRetry | Ganze Zahl<br/><br/>Maximalwert: 10 | 1 | Die Anzahl der lange Wiederholungsversuche vor dem Segment Ausführung fehlgeschlagen ist.<br/><br/>LongRetry Versuche werden durch LongRetryInterval verteilt. Wenn Sie eine Zeit zwischen den Verbindungsversuchen angeben müssen, verwenden Sie LongRetry. Wenn wiederholen und LongRetry angegeben sind, jeder Versuch LongRetry enthält Wiederholungsversuche und die maximale Anzahl der Versuche ist wiederholen * LongRetry.<br/><br/>Beispielsweise haben wir Folgendes in der Aktivität:<br/>Wiederholen: 3<br/>LongRetry: 2<br/>LongRetryInterval: 01:00:00<br/><br/>Angenommen, es ist nur ein Slice ausgeführt (Status warten) und die Ausführung der Aktivitäten jedes Mal. Es wäre zunächst 3 aufeinander folgende Ausführungsversuche. Nach jedem Versuch wäre der Slice-Status "Wiederholen". Nach der ersten 3 über sind, wäre der Slice-Status LongRetry.<br/><br/>Nach einer Stunde (d. h. Longretryinterval Wert) wäre eine andere Gruppe von 3 aufeinander folgende Ausführungsversuche. Danach würde der Slice-Status konnte und keine weitere Versuche werden würde. Daher wurden insgesamt 6 versucht.<br/><br/>Bei erfolgreicher Ausführung wird Segment Status bereit und keine weitere Versuche.<br/><br/>LongRetry kann verwendet werden, in Fällen, bei denen abhängige Daten ankommen, nicht deterministische Zeiten oder die gesamte Umgebung arbeitende unter die Verarbeitung erfolgt. In solchen Fällen Zeit Versuche nach nicht helfen können dabei und nach einem Intervall von Ergebnissen in der gewünschten Ausgabe.<br/><br/>Vorsicht: hohe Werte für LongRetry oder LongRetryInterval nicht festgelegt. Normalerweise bedeutet höhere Werte anderer systemische Probleme. 
longRetryInterval | TimeSpan | 00:00:00 | Die Verzögerung zwischen lange Wiederholungsversuche 


## <a name="next-steps"></a>Nächste Schritte

- Verstehen Sie [Planung und Ausführung in Azure Data Factory](data-factory-scheduling-and-execution.md).  
- Informieren Sie sich über [Daten](data-factory-data-movement-activities.md) und [Funktionen für die Transformation](data-factory-data-transformation-activities.md) von Azure Data Factory
- Verstehen Sie [Management und Überwachung von Azure Data Factory](data-factory-monitor-manage-pipelines.md).
- [Erstellen und Bereitstellen der Pipeline Faust](data-factory-build-your-first-pipeline.md). 
