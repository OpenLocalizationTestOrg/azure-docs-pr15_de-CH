<properties 
    pageTitle="Stream Analytics Azure Machine Learning Endpunkte konfigurieren | Microsoft Azure" 
    description="Sprache für benutzerdefinierte Funktionen in Stream Analytics"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Computer lernen Integration in Stream Analytics

Stream Analytics unterstützt benutzerdefinierte Funktionen Azure Machine Learning Endpunkte aufrufen. REST API-Unterstützung für dieses Feature wird in den [Stream Analytics REST API-Bibliothek](https://msdn.microsoft.com/library/azure/dn835031.aspx)beschrieben. Dieser Artikel enthält ergänzende Informationen zur erfolgreichen Implementierung dieser Funktion Stream Analytics. Ein Lernprogramm ebenfalls gebucht und steht [hier](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Übersicht: Azure Machine Learning-Terminologie

Microsoft Azure Machine Learning bietet eine Zusammenarbeit, Drag & Drop Tool, zu erstellen, testen und Bereitstellen von Vorhersageanalysen Lösungen für Ihre Daten. Dieses Tool wird der *Azure Machine Learning Studio*aufgerufen. Studio wird zum interagieren mit der Maschine Lernressourcen leicht erstellen, testen und durchlaufen den Entwurf. Diese Ressourcen und ihre Definitionen sind unten.

- **Arbeitsbereich**: der *Arbeitsbereich* ist ein Container, auf alle anderen Computer Lernressourcen in einem Container für die Verwaltung und Steuerung.
- **Experiment**: *Experimente* Daten Wissenschaftler nutzen Datasets und Machine Learning-Modell erstellt.
- **Endpunkt**: *Endpunkte* sind Azure Computer lernen zu Funktionen als Eingabe akzeptieren, einen angegebenen Computer lernen Modell erzielte Ausgabe verwendet.
- **Webservice bewerten**: ein *Webservice Bewertung* ist eine Sammlung von Endpunkten erwähnt.

Jeder Endpunkt verfügt über apis für Stapelverarbeitung und synchrone Ausführung. Stream Analytics verwendet synchrone Ausführung. Dienst heißt [Anforderung-Antwort-Dienst](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) in AzureML Studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Computer lernen die Stream Analytics-Aufträge

Für die Zwecke der Stream Analytics sind Verarbeitung von Aufträgen, einen Anforderung-Antwort-Endpunkt eine [Apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)und eine Definition stolz alle zur erfolgreichen Ausführung erforderlich. Stream Analytics ist einen zusätzlichen Endpunkt, der den Url stolz Endpunkt erstellt, sucht die Schnittstelle und eine Standarddefinition UDF an den Benutzer zurückgegeben.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Konfigurieren eines Stream Analytics und maschinelles lernen UDF über REST API

Mithilfe von REST-APIs können Sie Ihre Arbeit um Azure Sprache Funktionen konfigurieren. Die Schritte lauten:

1. Erstellen Sie einen Stream Analytics-Auftrag
2. Eingabe definieren
3. Definieren Sie eine Ausgabe
4. Erstellen Sie eine benutzerdefinierte Funktion (UDF)
5. Geschrieben Sie eine Stream Analytics-Transformation, die der UDF-Datei wird
6. Der Auftrag wird gestartet

## <a name="creating-a-udf-with-basic-properties"></a>Erstellen einer UDF mit grundlegenden Eigenschaften

Beispielsweise erstellt der folgende Code eine skalare UDF mit dem Namen *Newudf* , die an einen Endpunkt Azure Machine Learning bindet. Hinweis den *Endpunkt* (Dienst-URI) auf der API-Hilfeseite für den ausgewählten Dienst und *ApiKey* finden finden auf der Seite Dienste.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Beispiel-Anfragetext:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>RetrieveDefaultDefinition Endpunkt für Standard UDF aufrufen

Sobald das Skelett UDF erstellt wird die vollständige Definition der UDF-Datei benötigt. RetreiveDefaultDefinition Endpunkt hilft Ihnen die Standarddefinition für eine Skalarfunktion, die einen Endpunkt Azure Machine Learning gebunden ist. Die folgenden Nutzlast erfordert zu Azure Machine Learning-Endpunkt an die UDF-Standarddefinition für eine Skalarfunktion. Es angeben nicht den tatsächlichen Endpunkt wie bereits während PUT-Anforderung übermittelten. Stream Analytics Ruft den Endpunkt in der Anforderung bereitgestellte wenn explizit angegeben. Andernfalls verwendet sie die ursprünglich verwiesen. UDF wird einem string-Parameter (Satz) und gibt ein einzelnes vom Typ Zeichenfolge hier gibt die Bezeichnung "Stimmung" für den Satz an.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Beispiel-Anfragetext:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Ein Beispiel dieses suchen unten etwas wird ausgegeben.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>UDF mit Patch 

Nachdem der UDF-Datei wie folgt mit der vorherigen Antwort gepatcht werden muss.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Anfragetext (Ausgabe vom RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Implementieren von Stream Analytics Transformation UDF aufrufen

Jetzt fragen Sie UDF (hier ScoreTweet genannt ab) für jedes Eingabeereignis und Antwort für dieses Ereignis für eine Ausgabe.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
