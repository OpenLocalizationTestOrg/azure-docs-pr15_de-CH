<properties
    pageTitle="Schnellstart-Handbuch: Computer lernen Text Analytics APIs | Microsoft Azure"
    description="Azure maschinelles lernen Textanalyse - Quick Start-Handbuch"
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

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Erste Schritte mit Text Analytics APIs Gefühl, Schlüsselbegriffen, Themen und Sprache

<a name="HOLTop"></a>

Dieses Dokument beschreibt, wie zu integrierten der Dienst oder die Anwendung [Text Analytics APIs](//go.microsoft.com/fwlink/?LinkID=759711)verwenden.
Diese APIs können Gefühl, Schlüsselbegriffen, Themen und Sprache aus dem Text erkennen. [Klicken Sie hier um eine interaktive Demo Erfahrung.](//go.microsoft.com/fwlink/?LinkID=759712)

Finden Sie die [API-Definitionen](//go.microsoft.com/fwlink/?LinkID=759346) für technische Dokumentation zu den APIs.

Dieses Handbuch ist für Version 2 der APIs. Einzelheiten auf Version 1 der APIs [finden Sie in diesem Dokument](../machine-learning/machine-learning-apps-text-analytics.md).

Am Ende dieser praktischen Einführung werden Sie programmgesteuert feststellen:

- **Stimmung** - positiv oder negativ ist?

- **Schlüssel-Sätze** - was in einem einzigen Artikel besprechen?

- **Themen** - was über viele Artikel besprechen?

- **Sprachen** - Sprache Text geschrieben?

Beachten Sie, dass diese API 1 Transaktion pro Dokument. Beispielsweise verlangen Stimmung für Dokumente in einem einzelnen Aufruf 1000 1000 Transaktionen abgezogen.



<a name="Overview"></a>
## <a name="general-overview"></a>Übersicht ##

Dieses Dokument ist eine schrittweise Anleitung. Unser Ziel ist, gehen Sie durch die Schritte zum Trainieren eines Modells und auf Ressourcen, die Sie in das Produktionssystem einfügen können. In dieser Übung dauert ca. 30 Minuten.

Für diese Aufgaben Sie benötigen einen Editor und rufen Sie die REST-Endpunkten in einer Sprache Ihrer Wahl.

Fangen wir an!

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>Aufgabe 1 - Signatur für Text Analytics-APIs ####

In dieser Aufgabe werden Sie für den Analytics-Dienst anmelden.

1. **Kognitive Dienste** in [Azure-Portal](//go.microsoft.com/fwlink/?LinkId=761108) navigieren Sie und sicherzustellen Sie, dass **Text Analytics** als "API-Typ ausgewählt ist.

1. Wählen Sie einen Plan aus. Sie können **freie Tier für 5.000 Transaktionen pro Monat**auswählen. Als frei wird, werden Sie für den Dienst nicht berechnet. Sie müssen sich Ihre Azure-Abonnement anmelden. 

1. Füllen Sie die Felder aus, und erstellen Sie Ihr Konto.

1. Nach der Anmeldung für die Textanalyse finden Sie den **API-Schlüssel**. Kopieren Sie den Primärschlüssel Bedarf wird bei Verwendung der API-Dienste.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Aufgabe 2 - Erkennung Gefühl, Schlüsselbegriffen und Sprachen ####

Es ist leicht Gefühl, Schlüsselbegriffen und Sprachen im Text erkannt. Sie erhalten dieselben Ergebnisse programmgesteuert [Demo Erfahrung](//go.microsoft.com/fwlink/?LinkID=759712) zurück.

>[AZURE.TIP] Stimmung Analyse empfiehlt Sätze Text aufgeteilt. Dies führt in der Regel eine höhere Genauigkeit in Stimmung Vorhersagen.

Beachten Sie, dass die Sprachen wie folgt:

| Funktion | Unterstützte Sprachen |
|:-----|:----|
| Grüße | `en`(Englisch) `es` (Spanisch) `fr` (Französisch), `pt` (Portugiesisch) |
| Wichtige Begriffe | `en`(Englisch) `es` (Spanisch) `de` (Deutsch) `ja` (Japanisch) |


1. Sie müssen die Header folgt gesetzt. Beachten Sie, dass JSON derzeit das einzige akzeptierte Format für APIs. XML wird nicht unterstützt.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Formatieren Sie dann die Eingabezeilen in JSON. Stimmung, Schlüsselbegriffen und Sprache entspricht das Format. Beachten Sie, dass jede ID eindeutig sein muss und die ID vom System zurückgegeben. Die maximale Größe eines einzelnen Dokuments gesendet werden können ist 10KB und die maximale Gesamtgröße der übermittelten Eingabe beträgt 1MB. Mehr als 1.000 Dokumente können in einem Aufruf eingereicht werden. Mit einer Geschwindigkeit von 100 Anrufen pro Minute vorhanden Bandbreitenkontrolle - deshalb wird empfohlen, dass Sie große Mengen von Dokumenten in einem einzigen Aufruf senden. Ist ein optionaler Parameter angegeben werden sollen Wenn nicht englischen Text analysieren. Beispiel für Eingabe zeigt, wo der optionale Parameter `language` für Stimmung Analyse oder Schlüssel Ausdruck Extraktion enthalten ist:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. Rufen Sie **POST** System mit Gefühl, Schlüsselbegriffen und Sprache an. Die URLs werden wie folgt aussehen:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. Dieser Aufruf liefert eine JSON formatiert mit IDs und Eigenschaften erkannt. Ein Beispiel der Ausgabe für Stimmung zeigt unten (mit Fehler ausgeschlossen). Ein Wert zwischen 0 und 1 wird bei Gefühl für jedes Dokument zurückgegeben:

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Task 3 - Themen im Korpus Text erkennen ####

Dies ist eine neu veröffentlichte API die oben eine Liste der Themen gefunden Gibt Datensätze gesendet. Identifizierte ein Thema mit einem Schlüsselsatz, eine oder mehrere verwandte Wörter. Die API sollte für kurze, Menschen geschriebenen Text Benutzerfeedback wie Berichte funktionieren.

Diese API erfordert **mindestens 100 Datensätze** vorzulegen, sondern ist zur Erkennung von Hunderten oder Tausenden von Datensätzen Themen. Nicht englische Datensätze oder Datensätze mit weniger als 3 Wörter verworfen und daher nicht zu Themen zugeordnet. Thema Erkennung die maximale Größe eines einzelnen Dokuments übermittelt werden kann beträgt 30KB und die maximale Gesamtgröße der übermittelten Eingabe ist 30MB. Thema-Erkennung ist 5 Anträgen auf 5 Minuten begrenzt.

Es gibt zwei weitere **optionale** Parameter, die Verbesserung die Qualität der Ergebnisse:

- **Stoppwörter.**  Diese Begriffe und ihre Formulare schließen (z. B. Plurale) werden die gesamte Thema Erkennung Pipeline ausgeschlossen. Dient für häufig verwendete Wörter (zum Beispiel, "Problem", "Fehler" und "Benutzer" entsprechende Optionen für Kundenbeschwerden Software möglicherweise). Jede Zeichenfolge sollte ein einzelnes Wort.
- **Beenden Sie Sätze** - diese Sätze aus der Liste der zurückgegebenen Themen ausgeschlossen. Können Sie allgemeine Themen ausschließen, die nicht in den Ergebnissen angezeigt werden sollen. Beispielsweise würde "Microsoft" und "Azure" entsprechenden Themen ausgeschlossen sein. Zeichenfolgen können mehrere Wörter enthalten.

Gehen Sie Themen in Ihrem Text erkennen.

1. Formatieren der Eingabe in JSON. Diesmal können Sie Stoppwörter definieren und Sätze beenden.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Treffen Sie identische Kopfzeilen in Aufgabe 2 definierten, ein **Beitrag** zum Endpunkt Themen aufrufen:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. Dies gibt eine `operation-location` als Header in der Antwort, der Wert der URL Abfrage resultierenden Themen ist:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. Abfrage zurückgegebenen `operation-location` regelmäßig mit einer **GET** -Anforderung. Einmal pro Minute wird empfohlen.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Endpunkt liefert eine Antwort einschließlich `{"status": "notstarted"}` vor der Verarbeitung `{"status": "running"}` beim Verarbeiten und `{"status": "succeeded"}` mit dem einmal abgeschlossen. Sie können dann die Ausgabe nutzen werden im folgenden Format (Hinweis Details wie Fehler und die Daten aus diesem Beispiel ausgeschlossen wurden):

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Beachten Sie, dass die erfolgreiche Antwort Themen aus der `operations` Endpunkt haben das folgende Schema:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Erklärung für jeden Teil dieser Antwort lauten wie folgt:

**Themen**

| Schlüssel | Beschreibung |
|:-----|:----|
| ID | Ein eindeutiger Bezeichner für jedes Thema. |
| Bewertung | Anzahl der Dokumente Thema zugeordnet. |
| Suchwort | Zusammenfassen von Wort oder Ausdruck für das Thema. |

**topicAssignments**

| Schlüssel | Beschreibung |
|:-----|:----|
| documentId | Der Bezeichner für das Dokument. Entspricht der ID in der Eingabe enthalten. |
| topicId | Die Thema-ID, die das Dokument zugeordnet ist. |
| Abstand | Dokument zum Thema Zugehörigkeit Punktzahl zwischen 0 und 1. Unten eine Entfernung Faktor, desto stärker ist das Thema. |

**Fehler**

| Schlüssel | Beschreibung |
|:-----|:----|
| ID | Eingabedokument Eindeutiger Bezeichner der Fehler bezieht. |
| Nachricht | Fehlermeldung. |

## <a name="next-steps"></a>Nächste Schritte ##

Herzlichen Glückwunsch! Sie haben jetzt Textanalyse Daten verwenden. Sie können jetzt mithilfe eines Tools wie [Power BI](//powerbi.microsoft.com) Visualisieren von Daten sowie das Automatisieren Ihre Erkenntnisse Geben Sie eine Echtzeitansicht Datenmenge Text suchen.

Verwendung Text Analysefunktionen wie Gefühl, als Teil einer Bot finden Sie unter Beispiel [Emotionale Bot](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) auf Bot Framework-Website.
