<properties
    pageTitle="Hochladen von Daten in Azure Suche mithilfe der REST-API | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Informationen Sie zum Hochladen von Daten auf einen Index in Azure Suche mithilfe der REST-API."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Upload von Daten nach Azure mithilfe der REST-API
> [AZURE.SELECTOR]
- [Übersicht](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)

In diesem Artikel werden wie [Azure Suche REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Datenimport in Azure Search-Index anzeigen

Vor dieser exemplarischen Vorgehensweise sollten Sie bereits [ein Azure-Suchindex erstellt](search-what-is-an-index.md)haben.

Um Dokumente in den Index mit der REST-API drücken, werden Sie Ihren Index URL Endpunkt HTTP POST-Anforderung ausgeben. Der Hauptteil der HTTP-Anforderung ist ein JSON-Objekt mit den Dokumenten hinzugefügt, geändert oder gelöscht werden.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>. Identifizieren der Azure-Suchdienst Admin-api-Schlüssel
Bei HTTP-Anfragen an den Dienst mithilfe der REST-API muss *jede* API Anforderung den api-Schlüssel enthalten, der für den Suchdienst generiert wurde, den Sie bereitgestellt. Ein gültiger Schlüssel stellt Vertrauen auf pro Anfrage, zwischen dem Senden der Anforderung und der Dienst, der es behandelt.

1. Um Ihren Dienst-api-Schlüssel müssen Sie in [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu der Azure-Suchdienst blade
3. Klicken Sie auf das Symbol "Schlüssel"

Ihr Dienst haben *Administratorschlüssel* *Abfrage Schlüssel*.

  - Die primären und sekundären *Administratorschlüssel* gewähren Sie Vollzugriff auf alle Vorgänge, einschließlich der Möglichkeit zur Verwaltung des Dienstes erstellen und Löschen von Indizes, Indexer und Datenquellen. Gibt es zwei Schlüssel, damit weiterhin dem Sekundärschlüssel verwenden, möchten Sie den Primärschlüssel und umgekehrt zu regenerieren.
  - Die *Abfrage Schlüssel* gewährt schreibgeschützten Zugriff auf Indizes und Dokumente und normalerweise für Clientanwendungen, die Suchabfragen ausstellen verteilt.

Für die Zwecke des Datenimports in einen Index können entweder primäre oder sekundäre Administratorschlüssel.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Entscheiden Sie, welche Indizierung verwenden
Wenn die REST-API verwenden, werden Sie HTTP POST-Anfragen mit JSON Anforderung der Azure-Suchindex Endpunkt-URL erteilen. Das JSON-Objekt im Hauptteil HTTP-Anforderung enthält ein einzelnes JSON-Array mit dem Namen "Value" mit JSON-Objekten Darstellung Dokumente Index, Update oder Delete hinzufügen möchten.

Jede JSON-Objekt im Array "Wert" stellt ein Dokument indiziert werden. Jedes dieser Objekte enthält das Dokument und gibt die gewünschte Indizierung (Upload, zusammenführen, löschen usw.). Je nach der unter Aktionen Sie wählen, nur bestimmte Felder für jedes Dokument aufgenommen werden müssen:

@search.action | Beschreibung | Pflichtfelder für jedes Dokument | Notizen
--- | --- | --- | ---
`upload` | Ein `upload` Aktion ist vergleichbar mit einer "Upsert", wo das Dokument eingefügt, wenn es neu ist und aktualisiert oder ausgetauscht, wenn vorhanden. | Taste und andere Felder, die Sie definieren möchten, | Beim Aktualisieren und ein vorhandenes Dokument ersetzen, haben jedes Feld, das nicht in der Anforderung der Feldgruppe in `null`. Dies geschieht auch, wenn das Feld zuvor ein Wert ungleich Null festgelegt wurde.
`merge` | Aktualisiert ein vorhandenes Dokument mit der angegebenen Felder. Wenn das Dokument nicht im Index vorhanden ist, wird die Zusammenführung fehl. | Taste und andere Felder, die Sie definieren möchten, | Jedes Feld, das an eine Zusammenführung ersetzt das vorhandene Feld im Dokument. Dies umfasst auch Felder vom Typ `Collection(Edm.String)`. Angenommen, das Dokument enthält ein Feld `tags` mit `["budget"]` und führen Sie einen Seriendruck mit `["economy", "pool"]` für `tags`, der Endwert von der `tags` Feld `["economy", "pool"]`. Nicht `["budget", "economy", "pool"]`.
`mergeOrUpload` | Dadurch verhält sich wie `merge` ein Dokument mit dem angegebenen Schlüssel im Index bereits vorhanden. Wenn das Dokument nicht vorhanden ist, verhält er sich wie `upload` mit einem neuen Dokument. | Taste und andere Felder, die Sie definieren möchten, | -
`delete` | Der Index entfernt des angegebenen Dokuments. | nur Schlüssel | Alle Felder Geben Sie nicht das Schlüsselfeld ignoriert. Wenn Sie einzelnen Felder aus einem Dokument entfernen möchten, verwenden Sie `merge` einfach und stattdessen das Feld explizit auf null festgelegt.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. Erstellen Sie die HTTP-Anforderung und Anforderungsinhalt
Nachdem die erforderlichen Werte für den Index Aktionen gesammelt haben, können Sie die aktuelle HTTP-Anforderung und JSON Anforderungstext den Datenimport.

#### <a name="request-and-request-headers"></a>Anforderung und Anfrage-Header
In der URL müssen Dienstnamen, Indexnamen (in diesem Fall "Hotels"), sowie die entsprechenden API-Version (die aktuelle Version der API ist `2015-02-28` zum Zeitpunkt der Veröffentlichung dieses Dokuments). Sie müssen definieren die `Content-Type` und `api-key` Anfrage-Header. Für das Verwenden eines Ihren Dienst Admin.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Anfragetext

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

In diesem Fall verwenden wir `upload`, `mergeOrUpload`, und `delete` als unsere Aktionen suchen.

Angenommen Sie, dieses Beispiel "Hotels" Index bereits mehrere Dokumente enthält. Beachten Sie, wie wir nicht haben, alle möglichen Dokumentfelder Geben Sie bei Verwendung `mergeOrUpload` und wie wir nur die Dokumentschlüssel (`hotelId`) Verwendung `delete`.

Beachten Sie, nur bis 1000 Dokumente (16 MB) können in einer Volltextindizierung Anforderung.

## <a name="iv-understand-your-http-response-code"></a>IV. Der HTTP-Antwortcode verstehen
#### <a name="200"></a>200
Eine erfolgreiche Indizierung Anfrage erhalten Sie eine HTTP-Antwort mit Statuscode `200 OK`. JSON-Text der HTTP-Antwort wird wie folgt festgesetzt:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Statuscode `207` zurückgegeben, wenn mindestens ein Element nicht erfolgreich indiziert wurde. JSON-Text der HTTP-Antwort enthält Informationen zu fehlgeschlagenen Dokumenten.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] Häufig bedeutet, dass Belastung Suchdienst einen Punkt erreicht, Indizierung Anfragen beginnt wieder `503` Antworten. In diesem Fall wir empfehlen wieder aus Ihrem Clientcode und gesendet. Dieses geben System einige Zeit wiederherstellen, die Chancen, die zukünftige Anfragen erfolgreich. Ihre Anfragen schnell erneut verlängert nur die Situation.

#### <a name="429"></a>429
Statuscode `429` zurückgegeben, wenn Sie Ihr Kontingent für die Anzahl der Dokumente pro Index überschritten haben.

#### <a name="503"></a>503
Statuscode `503` zurückgegeben, wenn keines der Elemente in der Anforderung erfolgreich indiziert wurden. Dieser Fehler bedeutet, dass das System stark ausgelastet ist und die Anforderung kann nicht zu diesem Zeitpunkt verarbeitet werden.

> [AZURE.NOTE] In diesem Fall wir empfehlen wieder aus Ihrem Clientcode und gesendet. Dieses geben System einige Zeit wiederherstellen, die Chancen, die zukünftige Anfragen erfolgreich. Ihre Anfragen schnell erneut verlängert nur die Situation.

Weitere Informationen zu Dokumentaktionen und Erfolg-Fehlermeldungen finden Sie unter [Hinzufügen, aktualisieren oder Löschen von Dokumenten](https://msdn.microsoft.com/library/azure/dn798930.aspx). Weitere Informationen zu anderen HTTP-Statuscodes, die bei Fehler zurückgegeben werden kann, finden Sie unter [HTTP-Statuscodes (Azure suchen)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Weiter
Nach Auffüllen Azure Suchindex, werden Sie jetzt Abfragen nach Dokumenten suchen. Details finden Sie in der [Abfrage der Azure-Suchindex](search-query-overview.md) .
