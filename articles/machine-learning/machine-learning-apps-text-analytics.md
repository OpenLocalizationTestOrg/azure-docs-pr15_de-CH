<properties
    pageTitle="Computer lernen APIs: Textanalyse | Microsoft Azure"
    description="Microsoft Computer lernen Text Analytics APIs können verwendet werden, unstrukturierten Text stimmungsanalyse und Schlüsselsatz Extraktion, Spracherkennung Thema Erkennung analysieren."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Computer lernen APIs: Schlüssel Textanalyse Stimmung Ausdruck extrahieren, Erkennung und Thema Erkennung

>[AZURE.NOTE] Dieses Handbuch ist für Version 1 der API. Version 2 [**Dieses Dokument**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Version 2 ist nun die bevorzugte Version dieser API.

## <a name="overview"></a>Übersicht

Text Analytics-API ist eine Suite mit Azure Computer integrierten Text Analytics [Webdienste](https://datamarket.azure.com/dataset/amla/text-analytics) . Die API kann verwendet werden, unstrukturierten Text für Aufgaben wie stimmungsanalyse, Schlüsselsatz Extraktion Spracherkennung und Thema Erkennung analysieren. Keine Daten erforderlich, diese API verwenden: die Textdaten bringen. Diese API verwendet erweiterte Techniken natürlichsprachliche zu Best in Class Vorhersagen.

Textanalyse in Aktion sehen in unserer [Demosite](https://text-analytics-demo.azurewebsites.net/), Sie auch [Beispiele](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) zur Textanalyse in C# und Python Implementierung finden Sie.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Stimmungsanalyse

Die API gibt einen numerischen Wert zwischen 0 und 1 zurück. Werte nahe 1 zeigen positive Stimmung Werte nahe 0 negative Stimmung anzuzeigen. Klassifizierung mit Gefühl Wert entsteht. Die Eingabe der Klassifizierung Merkmale n-Grams Features von Wortart Tags und eingebetteten Word generiert. Englisch ist derzeit die einzige unterstützte Sprache.
 
## <a name="key-phrase-extraction"></a>Schlüsselsatz Extraktion

Die API gibt eine Liste von Zeichenfolgen für die wichtigsten – zu besprechende Punkte im Eingabetext. Wir beschäftigen aus Microsoft Office komplexe Verarbeitung natürlicher Sprache Toolkit. Englisch ist derzeit die einzige unterstützte Sprache.

## <a name="language-detection"></a>Spracherkennung

Die API gibt die erkannte Sprache und einen numerischen Wert zwischen 0 und 1 zurück. Werte nahe 1 angeben 100 % Sicherheit identifizierte Sprache gilt 120 Sprachen werden unterstützt.

## <a name="topic-detection"></a>Thema-Erkennung

Dies ist eine neu veröffentlichte API die oben eine Liste der Themen gefunden Gibt Datensätze gesendet. Identifizierte ein Thema mit einem Schlüsselsatz, eine oder mehrere verwandte Wörter. Diese API erfordert mindestens 100 Datensätze vorzulegen, sondern ist zur Erkennung von Hunderten oder Tausenden von Datensätzen Themen. Beachten Sie, dass diese API 1 Transaktion pro Text übermittelt. Die API sollte für kurze, Menschen geschriebenen Text Benutzerfeedback wie Berichte funktionieren.

---

## <a name="api-definition"></a>API-Definition

### <a name="headers"></a>Kopfzeilen

Sicherstellen Sie, dass enthalten die richtigen Header in der Anforderung die wie folgt:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Ihr kontoschlüssel finden von Ihrem Konto auf dem [Markt Azure](https://datamarket.azure.com/account/keys)Sie. Beachten Sie, dass derzeit nur JSON für Eingabe- und Formate akzeptiert wird. XML wird nicht unterstützt.

---

## <a name="single-response-apis"></a>Antwort-APIs

### <a name="getsentiment"></a>GetSentiment

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Beispiel für eine Anforderung**

Im folgenden Aufruf anfordern stimmungsanalyse für den Ausdruck "Hello World":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Eine Antwort erhalten wie folgt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Beispiel für eine Anforderung**

Im folgenden Aufruf fordern wir wichtige Begriffe im Text gefunden "Ein wundervolles Hotel, einzigartige Ausstattung und war":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Eine Antwort erhalten wie folgt:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Beispiel für eine Anforderung**

Im folgenden GET-Aufruf fordern für die Stimmung Satzteile in den Text *Hello World* wir

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Eine Antwort erhalten wie folgt:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Optionale Parameter**

`NumberOfLanguagesToDetect`ist ein optionaler Parameter. Der Standardwert ist 1.

---

## <a name="batch-apis"></a>Batch-APIs

Text Analytics-Dienst können Sie Stimmung und Schlüssel-Satz Extraktionen im Batch-Modus. Beachten Sie, dass alle Datensätze zählt als eine Transaktion bewertet. Beispielsweise verlangen Stimmung 1000 Datensätzen in einem einzelnen Aufruf 1000 Transaktionen abgezogen.

Beachten Sie, dass in das System eingegeben IDs vom System zurückgegebenen IDs. Der Webdienst überprüft nicht, dass diese IDs eindeutig sind. Es obliegt dem Aufrufer Eindeutigkeit überprüfen. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Beispiel für eine Anforderung**

Im folgenden Aufruf POST fordern wir Gefühle Ausdruck "Hello World", "Hello"Foo "World" und "Hello My World" im Hauptteil der Anforderung:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Anfragetext:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

In der Antwort erhalten Sie die Liste der Faktoren, die den Text Ids zugeordnet:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Beispiel für eine Anforderung**

In diesem Beispiel fordern wir eine Liste der Ansichten für Satzteile in folgenden Texten: 

* "Es war eine wunderbare Hotel, einzigartige Ausstattung und"
* "Es war erstaunlich Build-Konferenz mit sehr interessante"
* "Der Verkehr war, ich drei Stunden zum Flughafen"

Diese Anforderung als POST-Aufruf an den Endpunkt:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Anfragetext:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

In der Antwort erhalten Sie die Liste von Schlüsselbegriffen Text Ids zugeordnet:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "Englisch",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "Französisch",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>Thema Erkennung APIs

Dies ist eine neu veröffentlichte API die oben eine Liste der Themen gefunden Gibt Datensätze gesendet. Identifizierte ein Thema mit einem Schlüsselsatz, eine oder mehrere verwandte Wörter. Beachten Sie, dass diese API 1 Transaktion pro Text übermittelt.

Diese API erfordert mindestens 100 Datensätze vorzulegen, sondern ist zur Erkennung von Hunderten oder Tausenden von Datensätzen Themen.


### <a name="topics--submit-job"></a>Themen-Auftrag senden

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Beispiel für eine Anforderung**


Im folgenden POST-Aufruf fordern wir Themen für einen Satz von 100 Artikeln, die erste und letzte Eingabe Artikel angezeigt und sind zwei StopPhrases enthalten.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Anfragetext:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

In der Antwort erhalten Sie die JobId für den gesendeten Auftrag:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Eine Liste von Wort oder mehreren Word-Sätze nicht als Themen zurückgegeben werden soll. Kann verwendet werden, sehr allgemeine Themen herausfiltern. Beispielsweise können in einem Dataset zu Bewertungen "Hotel" und "hostel" sinnvolle Stop Sätze sein.  

### <a name="topics--poll-for-job-results"></a>Themen – Umfrage für Auftragsergebnisse

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Beispiel für eine Anforderung**

Übergeben Sie JobId von ' Absenden ' Auftragsschrittes zurückgegebenen Ergebnisse abgerufen werden sollen. Es wird empfohlen, diesen Endpunkt pro Minute bis Status aufrufen = 'Vollständig' in der Antwort. Es dauert etwa 10 Minuten für einen Auftrag abgeschlossen oder mehr Aufträge mit Tausenden von Datensätzen.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Während es verarbeitet wird, werden die Antwort wie folgt:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


Die API sieht im JSON-Format im folgenden Format:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


Die Eigenschaften für jeden Teil der Antwort sind wie folgt:

**TopicInfo Eigenschaften**

| Schlüssel | Beschreibung |
|:-----|:----|
| TopicId | Ein eindeutiger Bezeichner für jedes Thema. |
| Bewertung | Anzahl der Datensätze Thema zugeordnet. |
| Suchwort | Zusammenfassen von Wort oder Ausdruck für das Thema. Kann 1 oder mehrere Wörter. |

**TopicAssignment Eigenschaften**

| Schlüssel | Beschreibung |
|:-----|:----|
| ID | Kennung für den Datensatz. Entspricht der ID in der Eingabe enthalten. |
| TopicId | Die Thema-ID, die der Datensatz zugeordnet ist. |
| Abstand | Vertrauen, dass der Datensatz zum Thema gehört. Geringer Entfernung NULL gibt mehr vertrauen. |
