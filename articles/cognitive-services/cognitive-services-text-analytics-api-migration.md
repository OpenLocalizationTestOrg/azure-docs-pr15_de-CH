<properties
    pageTitle="Aktualisierung auf Version 2 der Textanalyse API | Microsoft Azure"
    description="Azure maschinelles lernen Textanalyse - Upgrade auf Version 2"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Aktualisierung auf Version 2 der Textanalyse API #

Dieses Handbuch gelangen Sie über die Aktualisierung der Code mit der zweiten Version die [erste Version der API](../machine-learning/machine-learning-apps-text-analytics.md) verwenden. 

Wenn die API nicht verwendet und weitere möchten können Sie **[erfahren Sie mehr über die API](//go.microsoft.com/fwlink/?LinkID=759711)** oder **[Führen Sie Quick Start Guide](//go.microsoft.com/fwlink/?LinkID=760860)**. Technische Referenz finden Sie die **[API-Definition](//go.microsoft.com/fwlink/?LinkID=759346)**.

### <a name="part-1-get-a-new-key"></a>Teil 1. Abrufen eines neuen Schlüssels ###

Zuerst müssen Sie einen neuen API-Schlüssel aus dem **Azure-Portal**zu erhalten:

1. Navigieren Sie zur Textanalyse Service durch [Cortana Intelligence Gallery](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2). Hier finden Sie außerdem Links zu der Dokumentation und den Codebeispielen.

1. Klicken Sie auf **Anmelden**. Hier gelangen Sie zu der Azure-Verwaltungsportal, können Sie für den Dienst anmelden.

1. Wählen Sie einen Plan aus. Sie können **freie Tier für 5.000 Transaktionen pro Monat**auswählen. Als frei wird, werden Sie für den Dienst nicht berechnet. Sie müssen sich Ihre Azure-Abonnement anmelden. 

1. Nach der Anmeldung für die Textanalyse Sie einen **API-Schlüssel**erhalten. Kopieren Sie diesen Schlüssel, wie Sie sie benötigen, wenn Sie API-Dienste verwenden.

### <a name="part-2-update-the-headers"></a>Teil 2. Der Header aktualisieren ###

Aktualisieren Sie die gesendeten Headerwerte wie folgt. Beachten Sie, dass kontoschlüssel nicht codiert ist.

**Version 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Version 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Teil 3. Die base-URL aktualisieren ###

**Version 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Version 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Teil 4a. Aktualisieren Sie Formate für Stimmung, Schlüsselbegriffen und Sprachen ###

#### <a name="endpoints"></a>Endpunkte ####

GET Endpunkte sind jetzt veraltet, damit alle Eingaben als eine POST-Anforderung gesendet werden soll. Aktualisieren Sie die Endpunkte, unten.

| |Version 1 einzelnen Endpunkt|Version 1 Batch Endpunkt|Version 2-Endpunkt|
|---|---|---|---|
|Typ|Erhalten|Bereitstellen|Bereitstellen|
|Grüße|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Wichtige Begriffe|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Sprachen|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Eingabeformate ####

Hinweis dieses nur postformat nun akzeptiert, damit Eingaben formatieren soll die zuvor Einzeldokument Endpunkte entsprechend verwendet. Eingaben sind nicht Groß-/Kleinschreibung beachtet.

**Version 1 (Stapelverarbeitung)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Version 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Ausgabe von Stimmung ####

**Version 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Ausgabe von Schlüsselbegriffen ####

**Version 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Ausgabe von Sprachen ####


**Version 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Teil 4 b. Aktualisieren Sie die Formate für Themen ###

#### <a name="endpoints"></a>Endpunkte ####

| |Version 1-Endpunkt | Version 2-Endpunkt|
|---|---|---|
|Senden Sie für Thema erkennen (POST)|```StartTopicDetection```|```topics```|
|Abrufen von Ergebnissen Thema (GET)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Eingabeformate ####

**Version 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Version 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Sendeergebnisse ####

**Version 1 (POST)**

Früher Abschluss des Auftrags erhalten die folgende JSON-Ausgabe Sie, der JobId URL abrufen die Ausgabe angefügt wird.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Version 2 (POST)**

Die Antwort enthält jetzt einen Headerwert: wo `operation-location` als Endpunkt für die Ergebnisse Abfragen verwendet:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Ergebnisse ####

**Version 1 (erhalten)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2 (GET)**

Wie zuvor wird **daher in regelmäßigen Abständen die Ausgabe** (die vorgeschlagene Periode ist pro Minute), bis die Ausgabe zurückgegeben. 

Wenn Themen-API wurde gelesen, einen Status `succeeded` zurückgegeben. Dazu gehören der Ergebnisse dann im Format unten:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Teil 5. Testen! ###

Jetzt können loslegen. Testen Sie den Code mit einem kleinen Beispiel, um sicherzustellen, dass die Daten erfolgreich verarbeiten kann.
