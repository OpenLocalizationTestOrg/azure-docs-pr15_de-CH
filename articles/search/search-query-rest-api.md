<properties
    pageTitle="Abfragen über die REST-API Azure Suchindex | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Erstellen einer Suchabfrage in Azure Suche und Suchparameter Suchergebnisse filtern und Sortieren verwenden."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Fragen Sie verwenden die REST-API Azure Suchindex ab
> [AZURE.SELECTOR]
- [Übersicht](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Dieser Artikel wird mit [Azure Suche REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)Index Abfragen anzeigen

Vor dieser exemplarischen Vorgehensweise sollten Sie bereits [ein Azure-Suchindex erstellt](search-what-is-an-index.md) und [mit Daten gefüllt](search-what-is-data-import.md)haben.

## <a name="i-identify-your-azure-search-services-query-api-key"></a>. Identifizieren der Azure-Suchdienst Abfrage API-Schlüssel
Eine wichtige Komponente eines jeden Suchvorgangs gegen Azure Suche REST API ist der *API-Schlüssel* , der für den Dienst war, den Sie bereitgestellt. Ein gültiger Schlüssel stellt Vertrauen auf pro Anfrage, zwischen dem Senden der Anforderung und der Dienst, der es behandelt.

1. Um Ihren Dienst-api-Schlüssel müssen Sie in [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu der Azure-Suchdienst blade
3. Klicken Sie auf das Symbol "Schlüssel"

Ihr Dienst haben *Administratorschlüssel* *Abfrage Schlüssel*.

 - Die primären und sekundären *Administratorschlüssel* gewähren Sie Vollzugriff auf alle Vorgänge, einschließlich der Möglichkeit zur Verwaltung des Dienstes erstellen und Löschen von Indizes, Indexer und Datenquellen. Gibt es zwei Schlüssel, damit weiterhin dem Sekundärschlüssel verwenden, möchten Sie den Primärschlüssel und umgekehrt zu regenerieren.
 - Die *Abfrage Schlüssel* gewährt schreibgeschützten Zugriff auf Indizes und Dokumente und normalerweise für Clientanwendungen, die Suchabfragen ausstellen verteilt.

Für die Zwecke der Abfrage eines Indexes können Sie eines der Abfrage. Der Administratorschlüssel können auch für Abfragen verwendet werden, aber verwenden Sie einen Abfrageschlüssel in Anwendungscode als besser das [Prinzip der niedrigsten Berechtigung](https://en.wikipedia.org/wiki/Principle_of_least_privilege)folgt.

## <a name="ii-formulate-your-query"></a>II. Formulieren Sie die Abfrage
Gibt es zwei Arten [den REST-API mit Index](https://msdn.microsoft.com/library/azure/dn798927.aspx)durchsuchen. Eine Möglichkeit ist HTTP POST-Anforderung ausgeben, der Abfrageparameter in ein JSON-Objekt im Hauptteil Anforderung definiert werden. Umgekehrt werden eine HTTP GET-Anforderung, die Abfrageparameter innerhalb der angeforderten URL definiert werden. Beachten Sie, dass POST mehr [entspannt Grenzen](https://msdn.microsoft.com/library/azure/dn798927.aspx) auf die Größe der Abfrageparameter als GET. Aus diesem Grund empfehlen wir buchen, wenn besondere Umstände vorliegen, unter Verwendung von GET einfacher wäre.

POST und GET müssen die *Dienstnamen*, *Indexnamen*und die entsprechenden *API-Version* (die aktuelle Version der API ist `2015-02-28` zum Zeitpunkt der Veröffentlichung dieses Dokuments) im Request-URL. Für GET werden am Ende der URL *-Abfragezeichenfolge* , die Abfrageparameter angeben. Das URL-Format finden Sie unter:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

Das Format für POST ist die gleiche, aber nur api-Version in der Abfragezeichenfolgen-Parameter.



#### <a name="example-queries"></a>Beispiel

Hier sind einige Beispielabfragen einen Index namens "Hotels". Diese Abfragen sind GET und POST angezeigt.

Der Begriff 'Budget' den gesamten Index suchen und zurückgeben nur die `hotelName` Feld:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Filtern von Index suchen Hotels billiger als $150 pro Nacht und die `hotelId` und `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Den gesamten Index sortiert nach einem bestimmten Feld Suchen (`lastRenovationDate`) in absteigender Reihenfolge die oberen zwei Ergebnisse und zeigen nur `hotelName` und `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Die HTTP-Anfrage
Damit Sie als Teil der HTTP-Anforderung URL (GET) oder Text (POST) für Ihre Abfrage formuliert haben, können Sie Ihre Anfrage-Header definieren und Ihre Anfrage.

#### <a name="request-and-request-headers"></a>Anforderung und Anfrage-Header
Sie müssen zwei Anforderungsheader GET oder POST drei definieren:
1. Die `api-key` Header müssen Sie im Schritt oben habe Abfrageschlüssel festgelegt werden. Beachten Sie, dass Sie auch eine Administratorschlüssel als das `api-key` Header, aber es empfiehlt sich die Verwendung eines abfrageschlüssels ausschließlich Lesezugriff auf Indizes und Dokumente erteilt.
2. Die `Accept` Header muss festgelegt werden, um `application/json`.
3. POST, die `Content-Type` Header muss auch festgelegt werden, um `application/json`.

Eine HTTP GET-Anforderung suchen "Hotels" Index mit mit einer einfachen Abfrage nach dem Begriff "Hotel sucht" Azure Suche REST API finden Sie unter:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Hier ist dieselbe Beispielabfrage mit HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Eine erfolgreiche Abfrage führt Statuscode `200 OK` und die Suchergebnisse werden im Antworttext als JSON zurückgegeben. Hier ist was die Ergebnisse für die obige Abfrage aus, sofern "Hotels" Index mit den Beispieldaten in [Den Datenimport in Azure Suche mithilfe der REST-API](search-import-data-rest-api.md) (Beachten Sie, dass aus Gründen der Übersichtlichkeit JSON formatiert wurde) aufgefüllt.

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Weitere Informationen finden Sie im Abschnitt "Antwort" [Suche](https://msdn.microsoft.com/library/azure/dn798927.aspx)Dokumente. Weitere Informationen zu anderen HTTP-Statuscodes, die bei Fehler zurückgegeben werden kann, finden Sie unter [HTTP-Statuscodes (Azure suchen)](https://msdn.microsoft.com/library/azure/dn798925.aspx).
