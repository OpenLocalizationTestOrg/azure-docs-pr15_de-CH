<properties 
    pageTitle="Datasets in Azure Data Factory erstellen | Microsoft Azure" 
    description="Erfahren Sie, wie Datasets in Azure Data Factory Beispiele erstellen, wie Offset und AnchorDateTime."
    keywords="Erstellen von Dataset Dataset wird, versetzt wird"
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
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>Datasets in Azure Data Factory
Dieser Artikel beschreibt Datasets in Azure Data Factory und wie Offset, AnchorDateTime und Offset-Stil Datenbanken enthält.

Wenn Sie ein Dataset erstellen, erstellen Sie einen Zeiger auf die Daten, die Sie bearbeiten möchten. Daten (Eingabe/Ausgabe) in einer Aktivität und eine Aktivität in einer Pipeline befindet. Eingabedatasets Eingabe für eine Aktivität in der Pipeline und ein ausgabedataset darstellt die Ausgabe für die Aktivität.

Datasets identifiziert Daten in verschiedenen Datenspeichern wie Tabellen, Dateien, Ordner und Dokumente. Nachdem Sie ein Dataset erstellen, können Sie es Aktivitäten in einer Pipeline. Beispielsweise kann ein Dataset ein Eingabe/Ausgabe-Dataset eine Kopie oder eine HDInsightHive-Aktivität. Azure-Portal bietet eine visuelle Layout alle Rohrleitungen und Daten ein-und Ausgänge. Auf einen Blick sehen Sie die Beziehungen und Abhängigkeiten von Pipelines über alle Quellen hinweg damit Sie immer wissen, wo Daten kommt und wohin.

In Azure Data Factory können Sie Daten aus einem Dataset mit kopieraktivität in einer Pipeline abrufen.

> [AZURE.NOTE] Neue Azure Data Factory hingegen finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md) Überblick Azure Data Factory-Dienst. Finden Sie ein Lernprogramm zum Erstellen Ihrer ersten Data Factory [Erstellen Ihrer erste Data Factory](data-factory-build-your-first-pipeline.md) . Diese beiden Artikel bieten Sie Hintergrundinformationen benötigen Sie in diesem Artikel besser zu verstehen. 

## <a name="define-datasets"></a>Definieren von datasets
Ein Dataset in Azure Data Factory ist wie folgt definiert: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

Die folgende Tabelle beschreibt die Eigenschaften in der obigen JSON:   

| Eigenschaft | Beschreibung | Erforderlich | Standard |
| -------- | ----------- | -------- | ------- |
| Name | Name des Datasets. [Azure Data Factory - Benennungsregeln](data-factory-naming-rules.md) für Regeln anzeigen | Ja | NA |
| Typ | Typ des Datasets. Geben Sie die Typen von Azure Data Factory unterstützt (z. B.: AzureBlob, AzureSqlTable). <br/><br/>Einzelheiten finden Sie unter [Dataset-Typ](#Type) . | Ja | NA |
| Struktur | Schema des dataset<br/><br/>Weitere Informationen finden Sie unter [Dataset-Struktur](#Structure) Abschnitt. | Nein. | NA |
| typeProperties | Eigenschaften für den ausgewählten Typ. Siehe [Typ Dataset](#Type) Abschnitt unterstützten Typen und ihre Eigenschaften. | Ja | NA |
| externe | Boolesches Flag an, ob ein Dataset von einer Datenpipeline Factory oder nicht explizit erstellt wird.  | Nein | falsch | 
| Verfügbarkeit | Definiert das Verarbeitungsfenster oder slicing Modell für die Dataset-Produktion. <br/><br/>Weitere Informationen finden Sie unter [Dataset Verfügbarkeit](#Availability) Abschnitt. <br/><br/>Ausführliche Informationen zum Modell, [Planung und Ausführung](data-factory-scheduling-and-execution.md) finden Sie im Artikel schneiden Dataset. | Ja | NA
| Richtlinie | Definiert die Kriterien oder die Bedingung, die Dataset-Segmente erfüllen müssen. <br/><br/>Details finden Sie im [Dataset Richtlinie](#Policy) Abschnitt. | Nein | NA |

## <a name="dataset-example"></a>DataSet-Beispiel
Im folgenden Beispiel stellt das Dataset Tabelle **MyTable** in einer **Azure SQL-Datenbank**. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Beachten Sie folgende Punkte: 

- Typ wird auf AzureSqlTable festgelegt.
- MyTable TableName Typeigenschaft (Typ AzureSqlTable) fest.
- Nameverknüpfterdienst bezieht sich auf einen verknüpften Dienst vom Typ AzureSqlDatabase. Siehe die Definition der folgenden verknüpften Serviceartikel. 
- Verfügbarkeit auf Tag festgelegt ist und Intervall auf 1 bedeutet, dass das Segment täglich erzeugten festgelegt ist.  

AzureSqlLinkedService ist wie folgt definiert:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

In der obigen JSON: 

- Typ ist auf AzureSqlDatabase festgelegt.
- ConnectionString Type-Eigenschaft gibt Informationen zu einer Azure SQL-Datenbank herstellen.  


Wie Sie sehen können, definiert verknüpften Serviceartikel einer Azure SQL-Datenbank herstellen. Dataset definiert, welche Tabelle als ein Ausgang für die Aktivität in einer Pipeline verwendet wird. Abschnitt Aktivität in der [Pipeline](data-factory-create-pipelines.md) JSON gibt an, ob das Dataset als eine Eingabe- oder Dataset verwendet wird.


> [AZURE.IMPORTANT] Wenn ein Dataset durch Azure Data Factory entsteht sollte als **extern**gekennzeichnet werden. Diese Einstellung gilt im Allgemeinen für Eingaben der erste Aktivität in einer Pipeline.   

## <a name="Type"></a>DataSet-Typ
Die unterstützten Datenquellen und Dataset-Typen werden ausgerichtet. Finden Sie unter [Datenaktivitäten](data-factory-data-movement-activities.md#supported-data-stores) Weitere Informationen zu Typen und Konfiguration des Datasets verwiesen. Z. B. Wenn Sie Daten aus einer SQL Azure-Datenbank verwenden, klicken Sie auf Azure SQL-Datenbank in der Liste der unterstützten Datenspeicher, detaillierte Informationen anzuzeigen.  

## <a name="Structure"></a>DataSet-Struktur
Abschnitt **Struktur** definiert im Schema des Datasets. Sie enthält eine Auflistung von Namen und Datentypen der Spalten.  Im folgenden Beispiel wird das Dataset hat drei Spalten Slicetimestamp, Projektname und Seitenzugriffe und sie Typ: Zeichenfolge, Zeichenfolge und Decimal bzw..

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>DataSet-Verfügbarkeit
Abschnitt **Verfügbarkeit** in einem Dataset definiert im Verarbeitung (stündlich, täglich, wöchentlich etc.) oder das slicing Modell für das Dataset. Siehe [Planung und Ausführung](data-factory-scheduling-and-execution.md) Artikel auf dem Dataset schneiden und Abhängigkeit. 

Abschnitt Verfügbarkeit gibt an, dass das ausgabedataset erzeugten stündlich (oder) Eingabe Dataset stündlich verfügbar ist:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

Die folgende Tabelle beschreibt die Eigenschaften, die im Abschnitt Verfügbarkeit: 

| Eigenschaft | Beschreibung | Erforderlich | Standard |
| -------- | ----------- | -------- | ------- |
| Häufigkeit | Gibt die Zeiteinheit für die Dataset-Slice Produktion.<br/><br/>**Unterstützte Frequenz**: Minute, Stunde, Tag, Woche, Monat | Ja | NA |
| Intervall | Gibt einen Multiplikator für Frequenz<br/><br/>"Frequenzintervall X" bestimmt, wie oft das Segment erstellt wird.<br/><br/>Benötigen Sie das Dataset auf Stundenbasis geschnitten werden, legen Sie **Häufigkeit** **Stunde**und **Intervall** auf **1**.<br/><br/>**Hinweis:** Frequenz Minute angeben, sollten Sie das Intervall auf weniger als 15 festlegen | Ja | NA |
| Formatvorlage | Gibt an, ob das Segment am Anfang/Ende des Intervalls erstellt werden muss.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Wenn Häufigkeit auf Monat und Stil auf EndOfInterval festgelegt ist, wird das Segment am letzten Tag des Monats erstellt. Das Format StartOfInterval festgelegt ist, wird das Segment am ersten Tag des Monats erstellt.<br/><br/>Wenn Häufigkeit auf Tag und Stil auf EndOfInterval festgelegt ist, wird das Segment in der letzten Stunde des Tages erstellt.<br/><br/>Wenn Häufigkeit auf Stunde und Stil auf EndOfInterval festgelegt ist, wird das Segment am Ende der Stunde erzeugt. Das Segment wird für ein Segment 1 – 2 Uhr Zeitraum um 14 Uhr erstellt. | Nein | EndOfInterval |
| anchorDateTime | Definiert die absolute Position im Dataset Slice-Grenzen berechnet Planer verwendet. <br/><br/>**Hinweis:** Bei der AnchorDateTime Datumsteile mit präziser als die Häufigkeit werden genauere Teile ignoriert. <br/><br/>Z. B. wenn das **Intervall** **stündlich** (Häufigkeit: Stunde und Intervall: 1) **AnchorDateTime** enthält, **Minuten und Sekunden**und **Minuten und Sekunden** Teil der AnchorDateTime werden ignoriert. | Nein | 01/01/0001 |
| Offset | Zeitspanne, Anfang und Ende aller Segmente Dataset verschoben werden. <br/><br/>**Hinweis:** Wenn AnchorDateTime und Offset angegeben sind, ist das Ergebnis der Kombination Umschalt. | Nein | NA |

### <a name="offset-example"></a>versetzt wird

Tägliche slices, beginnen um 6 Uhr statt standardmäßig Mitternacht. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

Die **Häufigkeit** **Tag** und **Intervall** auf **1** (täglich) festgelegt: Wenn das Segment 6 Uhr statt zur Standardzeit erzeugt werden soll: 12 Uhr. Beachten Sie, dass dieses Mal eine UTC-Zeit. 

## <a name="anchordatetime-example"></a>AnchorDateTime-Beispiel

**Beispiel:** 23 Stunden Dataset Segmente auf 2007-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>Offset-Stil-Beispiel

Benötigen Sie Dataset monatlich auf Datum und Uhrzeit (angenommen am 3. monatlich um 8:00 Uhr), **Offset** -Tag, das Datum und Zeit verwenden sollten. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>DataSet-Richtlinie

Abschnitt **Richtlinie** Dataset-Definition definiert die Kriterien oder die Bedingung, die Dataset-Segmente erfüllen müssen.

### <a name="validation-policies"></a>Validierungsrichtlinien

| Richtlinienname | Beschreibung | Angewendet auf | Erforderlich | Standard |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Überprüft, dass die Daten in einer **Azure blob** die Mindestgröße (in Megabyte) erfüllt. | Azure Blob | Nein | NA |
|minimumRows | Überprüft, ob die Daten in einer **SQL Azure-Datenbank** oder einer **Tabelle Azure** die Mindestanzahl der Zeilen enthält. | <ul><li>SQL Azure-Datenbank</li><li>Azure-Tabelle</li></ul> | Nein | NA

#### <a name="examples"></a>Beispiele

**MinimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Externe Datensätze

Externe Datensätze sind, die nicht von einer laufenden Pipeline Data Factory erstellt werden. Wenn das Dataset als **extern**gekennzeichnet ist, kann **ExternalData** Richtlinie beeinflussen das Verhalten der Verfügbarkeit Slice Dataset definiert werden. 

Wenn ein Dataset durch Azure Data Factory entsteht sollte als **extern**gekennzeichnet werden. Diese Einstellung gelten für die Eingaben der erste Aktivität in einer Pipeline, wenn Aktivität oder Pipeline-Verkettung verwendet wird. 

| Name | Beschreibung | Erforderlich | Standardwert  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | Zeitpunkt die Überprüfung der Verfügbarkeit von externen Daten für das angegebene Segment verzögert. Beispielsweise soll die Daten stündlich zur Verfügung, kann die Überprüfung, die externen Daten und das entsprechende Segment ist verzögert werden mit DataDelay.<br/><br/>Gilt nur für die Zeit.  Zum Beispiel 13.00 Uhr jetzt und dieser Wert ist 10 Minuten, beginnt die Validierung 10 Uhr.<br/><br/>Diese Einstellung wirkt sich nicht Slices in der Vergangenheit (mit Slice Endzeit + DataDelay < jetzt) unverzüglich verarbeitet.<br/><br/>Zeit größer als 23:59 Stunden müssen im day.hours:minutes:seconds Format angegeben. Beispielsweise um 24 Stunden anzugeben, verwenden Sie nicht 24:00:00; Verwenden Sie stattdessen 1.00:00:00. Wenn Sie 24:00:00 verwenden, wird es als 24 Tage (24.00:00:00) behandelt. Geben Sie 1:04:00:00 für 1 Tag und 4 Stunden. | Nein | 0 |
| retryInterval | Die Wartezeit zwischen den Fehler Wiederholungsversuch. Gilt für Zeit. Wenn der fehlgeschlagenen Versuchen wir warten nach dem letzten Versuch. <br/><br/>Ist 1:00 Uhr jetzt beginnen wir beim ersten Versuch. Die Dauer die erste Überprüfung abgeschlossen 1 Minute und der Fehler die nächste Wiederholung ist auf 1:00 1 min (Dauer) + 1min (Wiederholungsintervall) = 02 Uhr. <br/><br/>Segmente in der Vergangenheit ist ohne Verzögerung. Die Wiederholung erfolgt unmittelbar. | Nein | 00:01:00 (1 Minute) | 
| "RetryTimeout" | Das Timeout für jeden Wiederholungsversuch.<br/><br/>Wenn diese Eigenschaft auf 10 Minuten festgelegt ist, muss die Überprüfung innerhalb von 10 Minuten abgeschlossen werden. Dauert länger als 10 Minuten zum Durchführen der Überprüfung wird der Retry Timeout.<br/><br/>Wenn alle Versuche für die Überprüfung ein Timeout, ist das Segment als TimedOut gekennzeichnet. | Nein | 00:10:00 (10 Minuten) |
| maximumRetry | Anzahl der externen Daten prüfen. Der zulässige Höchstwert ist 10. | Nein | 3 | 

## <a name="scoped-datasets"></a>Bewertete datasets
Sie können Datasets erstellen, die auf eine Pipeline mithilfe der Eigenschaft **Datensätze** beschränkt sind. Diese Datensätze können nur Aktivitäten in dieser Pipeline aber nicht Aktivitäten in anderen Rohrleitungen verwendet werden. Das folgende Beispiel definiert eine Rohrleitung mit zwei Datasets - Rdc InputDataset und OutputDataset-Rdc - in der Pipeline:  

> [AZURE.IMPORTANT] Bewertete Datasets werden nur einmal Pipelines (**PipelineMode** **OneTime**festgelegt) unterstützt. Einzelheiten finden Sie unter [Onetime-Pipeline](data-factory-scheduling-and-execution.md#onetime-pipeline) .

    {
        "name": "CopyPipeline-rdc",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }