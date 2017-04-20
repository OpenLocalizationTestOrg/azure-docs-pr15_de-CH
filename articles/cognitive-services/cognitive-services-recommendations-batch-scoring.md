
<properties
    pageTitle="Erste Empfehlung stapelweise: maschinelles lernen empfohlene API | Microsoft Azure"
    description="Azure maschinelles lernen Recommendations - Empfehlung in Stapeln abrufen"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Lassen Sie sich in Stapeln

>[AZURE.NOTE] Empfehlung ist in Batches komplizierter als Empfehlung einer gleichzeitig abrufen. Überprüfen Sie die APIs für Informationen zu empfohlenen für eine einzelne Anforderung:

> [Artikel-Empfehlung](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Elements Benutzer empfohlen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Batch Bewertung ist nur für Builds, die nach dem 21. Juli 2016 erstellt wurden.


Es gibt Situationen empfohlenen für mehrere Elemente gleichzeitig abgerufen werden müssen. Beispielsweise Sie möglicherweise eine Empfehlung für den Zwischenspeicher oder sogar analysieren die Typen der Vorschläge, die Sie erhalten.

Punktwertung Batchoperationen, sind wie wir sie nennen asynchrone Vorgänge. Sie müssen den Antrag zum Abschluss des Vorgangs warten und erfassen die Ergebnisse.  

Um sind genau diese Schritte:

1.  Erstellen Sie einen Azure Storage Container haben Sie eine bereits.
2.  Hochladen einer Eingabedatei, die Ihre Empfehlung Anfragen Azure Blob-Speicher beschreiben.
3.  Die Punktwertung Stapelverarbeitung starten.
4.  Die asynchronen Vorgang warten.
5.  Nach Abschluss des Vorgangs Sammeln der Ergebnisse von BLOB-Speicher.

Gehen Sie durch die einzelnen Schritte.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Erstellen eines Falls Sie nicht bereits eine haben

Zum [Azure-Portal](https://portal.azure.com) , und erstellen Sie ein neues Speicherkonto ggf. noch nicht. Navigieren Sie zu diesem Zweck **neu** > **Daten** + **Speicher** > **Konto**.

Nachdem Sie ein Speicherkonto haben, müssen Sie Blob-Container erstellen, wo Sie die Eingabe und Ausgabe der Batchausführung speichern.

Hochladen einer Eingabedatei, die beschreibt jede Empfehlung fordert das BLOB-Speicher – nennen Datei input.json hier.
Wenn einen Container vorhanden ist, müssen Sie eine Datei hochzuladen, die jede Anforderung beschreibt, die vom Dienst Recommendations durchführen müssen.

Ein Stapel kann nur eine Anforderung aus einem bestimmten Build ausführen. Wir erklären, wie diese Informationen im nächsten Abschnitt. Jetzt nehmen wir an, wir empfohlenen Artikel aus einem bestimmten Build ausführen. Die Eingabedatei enthält für jede Anforderung die eingegebenen Informationen (in diesem Fall die Seed-Elemente).

Dies ist ein Beispiel für die input.json Datei aussieht:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Wie Sie sehen, ist die Datei eine JSON jede Anforderung Informationen, die hat senden eine Anforderung Vorschläge erforderlich ist. Erstellen Sie eine ähnliche JSON-Datei für Anfragen, die Sie erfüllen müssen, und Container die soeben im BLOB-Speicher erstellte kopieren.

## <a name="kick-start-the-batch-job"></a>Starten der Stapelverarbeitung

Im nächste Schritt wird eine neue Stapelverarbeitung übermitteln. Weitere Informationen finden Sie in der [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

Der Anforderungstext der API muss die Speicherorte festlegen, Eingabe-, Ausgabe- und Fehlerdateien gespeichert werden müssen. Es muss die Anmeldeinformationen definieren, die erforderlich sind, um diese Speicherorte zugreifen. Darüber hinaus müssen Sie einige Parameter für die gesamte Partie (die Art der auf Anforderung der Modellbuild verwenden gelten, die Anzahl der Ergebnisse pro Aufruf, usw.)

Dies ist ein Beispiel der Anforderungstext aussehen sollte:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Hier einige wichtige Dinge zu beachten:

-   Derzeit **Read** immer auf **PublicOrSas**fest.

-   Sie möchten einen Token Shared Access Signatur (SAS) ermöglichen die empfohlene API lesen und Schreiben von/das Speicherkonto Blob. Weitere Informationen zu SAS-Token finden auf [der Seite empfohlene API](../storage/storage-dotnet-shared-access-signature-part-1.md).

-   Derzeit unterstützt nur **ApiName** ist **ItemRecommend**, die für Artikel-Empfehlung verwendet wird. Batchverarbeitung unterstützt derzeit nicht Elements Benutzer empfohlen.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Warten Sie, bis des asynchronen Vorgangs abgeschlossen

Beim Starten des Batchvorgangs gibt die Antwort Operation Location-Header mit den Informationen, der zum Nachverfolgen der Operation erforderlich ist.
Den Vorgang wird mithilfe [Abrufen Vorgang Status API]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da)wie zum Nachverfolgen der Bedienung eines Buildvorgangs nachverfolgen.

## <a name="get-the-results"></a>Die Ergebnisse

Nachdem der Vorgang abgeschlossen wurde, vorausgesetzt, dass keine Fehler aufgetreten, können Sie die Ergebnisse aus der Ausgabe sammeln BLOB-Speicher.

Im folgenden Beispiel zeigen die Ausgabe aussehen könnte. In diesem Beispiel zeigen wir Ergebnisse für einen Stapel mit nur zwei (kurz).

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Erfahren Sie mehr über die Grenzen

-   Nur eine Stapelverarbeitung kann pro Abonnement nacheinander aufgerufen werden.
-   Eine Batch Job Eingabedatei darf nicht mehr als 2 MB sein.
