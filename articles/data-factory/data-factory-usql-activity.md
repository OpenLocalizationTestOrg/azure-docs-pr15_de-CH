<properties 
    pageTitle="Skript U SQL Azure Data Lake Analytics von Azure Data Factory" 
    description="Informationen Sie zum Prozessdaten U-SQL-Skripts See Datenanalyse Azure Compute-Dienst ausführen." 
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
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>Skript U SQL Azure Data Lake Analytics von Azure Data Factory
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schweine](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten See Analytics U-SQL](data-factory-usql-activity.md)
[benutzerdefinierte .NET](data-factory-use-custom-activities.md)
 
Eine Pipeline in einer Azure Daten verarbeitet Daten im verknüpften Speicher-Services mit verknüpften Compute-Services. Es enthält eine Sequenz von Aktivitäten, wobei jede Aktivität eine Verarbeitung ausführt. Dieser Artikel beschreibt **Data Lake Analytics U-SQL-Aktivität** , die **U-SQL** -Skript **See Datenanalyse Azure** Compute verknüpfte Dienst ausgeführt wird. 

> [AZURE.NOTE] 
> Erstellen Sie ein Konto Azure Data Lake Analytics vor dem Erstellen einer Pipeline mit Daten See Analytics U-SQL-Aktivität. Informationen zu Azure Data Lake Analytics finden Sie unter [Erste Schritte mit Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Überprüfen Sie [erstellen Ihre erste Pipeline Lernprogramm](data-factory-build-your-first-pipeline.md) ausführliche Daten Factory, verknüpften Diensten, Datasets und einer Pipeline erstellen. Mit der JSON-Ausschnitte mit Data Factory-Editor, Visual Studio oder Azure PowerShell Data Factory Entitäten erstellt.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics verknüpft Service
Sie erstellen einen Dienst **Azure Data Lake Analytics** verknüpft um einen See Datenanalyse Azure Compute Dienst mit einer Azure Data Factory verknüpfen. Daten See Analytics U-SQL-Aktivität in der Pipeline bezieht sich auf diese verknüpften Serviceartikel. 

Im folgende Beispiel stellt JSON-Definition für einen Dienst Azure Data Lake Analytics verknüpft. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


Die folgende Tabelle enthält eine Beschreibung der Eigenschaften in der JSON-Definition. 

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Type-Eigenschaft fest auf: **AzureDataLakeAnalytics**. | Ja
Kontoname | Azure Data Lake Analytics-Kontoname. | Ja
dataLakeAnalyticsUri | Azure Data Lake Analytics URI. |  Nein 
Autorisierung | Autorisierungscode wird automatisch nach Schaltfläche **Autorisieren** im Werk-Editor und die Anmeldung OAuth abgerufen.  | Ja 
subscriptionId | Azure-Abonnement-id | Nein (wenn nicht angegeben, wird Abonnement Data Factory verwendet). 
resourceGroupName | Azure Namen |  Nein (wenn nicht angegeben, wird Ressourcengruppe Data Factory verwendet).
sessionId | Sitzungsbezeichner OAuth Autorisierung Sitzung. Jede Sitzung-Id ist eindeutig und darf nur einmal verwendet werden. Die Sitzung ist Id im Werk-Editor automatisch generiert. | Ja

Mithilfe der Schaltfläche **Autorisieren** erstellten Autorisierungscode läuft nach einer Weile. Siehe folgende Tabelle für die Ablaufzeiten für verschiedene Typen von Benutzerkonten. Folgende Fehlermeldung Meldung an, wenn die Authentifizierung **Sicherheitstoken abläuft**: Vorgang Anmeldefehler: Invalid_grant - AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS70008: Der angegebene Zugriff gewähren abgelaufen oder gesperrt. Trace-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Zeitstempel: 2015-12-15 21:09:31Z

 
| Benutzertyp | Läuft ab nach |
| :-------- | :----------- | 
| Benutzerkonten, die nicht von Azure Active Directory verwaltet (@hotmail.com, @live.com, etc..) | 12 Stunden |
| Benutzerkonten verwaltet von Azure Active Directory (AAD) | 14 Tage nach die letzte Schicht ausführen. <br/><br/>90 Tage, wenn ein Segment verknüpften Serviceartikel OAuth-basierte Grundlage mindestens alle 14 Tage ausgeführt wird. |

Um zu vermeiden, / diese Fehler auflösen, erneut autorisieren, **Autorisieren** mit Schaltfläche Wenn **Sicherheitstoken abläuft** und verknüpfte Dienst bereitstellen. Sie können auch Werte für **SessionId** und **Autorisierung** Eigenschaften programmgesteuert mithilfe von Code im folgenden Abschnitt. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>SessionId und Autorisierung Werte programmgesteuert generieren 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Finden Sie unter [AzureDataLakeStoreLinkedService-Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService-Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)und [AuthorizationSessionGetResponse Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) Informationen im Code verwendeten Data Factory-Klassen. Fügen Sie einen Verweis auf: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll für die WindowsFormsWebAuthenticationDialog-Klasse. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-Aktivität 

Der folgende Codeausschnitt JSON definiert eine Rohrleitung mit einer Data Lake Analytics U-SQL. Die Aktivitätsdefinition enthält einen Verweis auf Azure Data Lake Analytics verknüpft-Dienst, den Sie zuvor erstellt haben.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


Tabelle Namen und eine Beschreibung der Eigenschaften, die für diese Aktivität. 

Eigenschaft | Beschreibung | Erforderlich
:-------- | :----------- | :--------
Typ | **DataLakeAnalyticsU-SQL**muss die Type-Eigenschaft fest. | Ja
scriptPath | Pfad zum Ordner mit dem U-SQL-Skript. Name der Datei beachtet werden. | Nein (Wenn Sie Skripts verwenden)
scriptLinkedService | Verknüpfte Dienst, der Speicher verbindet mit dem Skript Daten Fabrik | Nein (Wenn Sie Skripts verwenden)
Skript | Geben Sie Inlineskript anstatt ScriptPath und ScriptLinkedService an. Beispiel: "Skript": "CREATE DATABASE Test". | Keine (verwenden Sie ScriptPath und ScriptLinkedService)
degreeOfParallelism | Die maximale Anzahl von Knoten gleichzeitig zum Ausführen des Auftrags verwendet. | Nein
Priorität | Bestimmt welche Einzelvorgänge aus, die in der Warteschlange ausgewählt werden zuerst ausgeführt. Je niedriger die Zahl, desto höher die Priorität. | Nein 
Parameter | Parameter für U-SQL-Skript | Nein 

Finden Sie die Skriptdefinition [Skriptdefinition SearchLogProcessing.txt](#script-definition) . 

## <a name="sample-input-and-output-datasets"></a>Beispiele für Eingabe und Ausgabe von datasets

### <a name="input-dataset"></a>Eingabe-dataset
In diesem Beispiel befindet sich die Eingabedaten in eine Azure-See Datenspeicher (SearchLog.tsv-Datei im Ordner Datalake-Eingabe). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Ausgabedataset
In diesem Beispiel werden die Ausgabedaten U-SQL-Skript in ein Azure-See Datenspeicher (Datalake-Ausgabe Ordner) gespeichert. 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Beispiel-See Datenspeicher verknüpft Service
Hier ist die Definition der Probe, die Azure See Datenspeicher von Eingabe/Ausgabe-Datasets verwendet verknüpft. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

[Verschieben von Daten in und aus dem Datenspeicher Azure](data-factory-azure-datalake-connector.md) finden Sie im Artikel eine Beschreibung der JSON-Eigenschaften. 

## <a name="sample-u-sql-script"></a>U-SQL-Beispielskript 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

Die Werte für **@in** und **@out** Parameter U-SQL-Skript übergeben dynamisch von ADF mithilfe des Abschnitts "Parameters". Siehe Parameterabschnitt "" in der Definition der Pipeline.

Sie können andere Eigenschaften wie DegreeOfParallelism und Prioritäten sowie in der Pipeline-Definition für die Aufträge angeben, Azure Data Lake Analytics-Dienst ausgeführt.

## <a name="dynamic-parameters"></a>Dynamische Parameter
Beispieldefinition Pipeline werden und Parameter mit hart codierten Werten zugeordnet. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Es ist möglich, dynamische Parameter verwenden. Zum Beispiel: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

In diesem Fall Dateien sind noch abgeholt /datalake/input Ordner und Dateien im Ordner /datalake/output erstellt. Die Dateinamen sind dynamisch basierend auf Slice Startzeit.  