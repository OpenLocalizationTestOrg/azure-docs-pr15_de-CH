<properties
    pageTitle="Wie mit Fiddler testen Azure Suche REST APIs | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Verwenden Sie Fiddler für einen Ansatz codefreien Azure Suche Verfügbarkeit überprüfen und versuchen, den REST-APIs."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Verwenden Sie Fiddler evaluieren und Testen von Azure Suche REST APIs
> [AZURE.SELECTOR]
- [Übersicht](search-query-overview.md)
- [Search Explorer](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Erläutert, wie mit Fiddler [kostenlos Telerik](http://www.telerik.com/fiddler), Problem HTTP-Anfragen und Antworten anzeigen mit Azure Suche REST API, ohne Code schreiben zu müssen. Azure Suche ist vollständig verwaltete, Suchdienst auf .NET und REST-APIs leichter programmierbare Microsoft Azure Cloud gehostet. Der Azure-Suchdienst REST-APIs sind auf [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)dokumentiert.

In den folgenden Schritten werden Sie einen Index erstellen, Dokumente hochladen, Index Abfragen und Abfragen des Systems für Informationen.

Um diese Schritte auszuführen, benötigen Sie einen Azure-Suchdienst und `api-key`. Anleitung für den Einstieg finden Sie unter [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) .

## <a name="create-an-index"></a>Erstellen eines Indexes

1. Starten Sie Fiddler. Deaktivieren Sie im Menü **Datei** **Datenverkehr erfassen** externen HTTP-Aktivität aus, die die aktuelle Aufgabe ist.

3. Auf der Registerkarte **Composer** formulieren Sie eine Anforderung, die der folgenden Abbildung aussieht.

    ![][1]

2. Wählen Sie **Einfügen**.

3. Geben Sie eine URL, die URL, Anforderung Attribute und api-Version angibt. Einige Hinweise zu beachten:
   + Verwenden Sie HTTPS als Präfix.
   + Anforderung-Attribut ist "/ Indizes Hotels". Dies weist Suche einen Index mit dem Namen "Hotels" erstellen.
   + API-Version ist als Kleinbuchstaben "? API-Version 2015-02-28 =". API-Versionen sind wichtig, da Azure Suche regelmäßig Updates bereitgestellt. In seltenen Fällen kann eine Update eine unterbrechende Änderung der API vorgestellt. Aus diesem Grund erfordert Azure Suche eine api-Version bei jeder Anforderung sind Sie vollständige Kontrolle über die Datenbank verwendet wird.

    Die vollständige URL sollte dem folgenden Beispiel ähneln.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Angeben des Anforderungsheaders gültigen Werte für den Dienst Host und API-Schlüssel ersetzen.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  Fügen Sie Anforderungstext im Bereich der Indexdefinition bilden.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Klicken Sie auf **Ausführen**.

In wenigen Sekunden erhalten Sie eine Antwort HTTP-201 in der Sitzungsliste, dass der Index erstellt wurde.

Get HTTP 504 sicher, dass die URL HTTPS gibt. Wenn Sie HTTP 400 oder 404, Überprüfen des Anforderungstexts um sicherzustellen, dass keine Fehler kopieren. HTTP 403 gibt in der Regel ein Problem mit dem api-Schlüssel (einen ungültigen Schlüssel oder ein Problem Syntax wie der api-Schlüssel angegeben wird).

## <a name="load-documents"></a>Dokumente

Auf der Registerkarte **Composer** wird Ihre Anforderung Dokumente wie folgt aussehen. Der Hauptteil der Anforderung enthält die Daten für 4 Hotels.

   ![][2]

1. Wählen Sie **Buchen**.

2.  Geben Sie eine URL beginnt mit HTTPS, gefolgt von den Dienst-URL folgen "/indexes/ < Indexname >/Docs Index? API-Version 2015-02-28 =". Die vollständige URL sollte dem folgenden Beispiel ähneln.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Anforderungsheader muss identisch sein. Beachten Sie, dass Sie den Host und den API-Schlüssel durch Werte ersetzt, die für den Dienst zulässig sind.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Der Anforderungstext enthält vier Dokumente Hotels Index hinzugefügt werden.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
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
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Klicken Sie auf **Ausführen**.

In ein paar Sekunden sollte eine HTTP 200 Antwort in der Sitzungsliste angezeigt werden. Dies bedeutet, dass die Dokumente erstellt wurden. Man eine 207 konnte mindestens ein Dokument hochladen. 404 erhalten, haben Sie einen Syntaxfehler in der Kopf- oder Text der Anforderung.

## <a name="query-the-index"></a>Abfrage der index

Da Index Dokumente geladen werden, können Sie diese Abfragen ausgeben.  Auf der Registerkarte **Composer** zeigt **GET** -Befehl, der den Dienst fragt folgende Abbildung.

   ![][3]

1.  Wählen Sie **Abrufen**.

2.  Geben Sie eine URL beginnt mit HTTPS, gefolgt von den Dienst-URL folgen "/indexes/ < Indexname > / Docs?", gefolgt von Abfrageparametern. Verwenden Sie beispielsweise die folgende URL für den Dienst ist der Hostname Beispiel ersetzen.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Diese Abfrage sucht nach dem Begriff "Hotel" und Facetten Kategorien für die Bewertung abgerufen.

3.  Anforderungsheader muss identisch sein. Beachten Sie, dass Sie den Host und den API-Schlüssel durch Werte ersetzt, die für den Dienst zulässig sind.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

Der Antwortcode sollte 200 und Antwort Ausgabe sollte ähnlich der folgenden Abbildung aussehen.

   ![][4]

Die folgende Beispielabfrage ist aus dem [Suchindex Vorgang (Azure Such-API)](http://msdn.microsoft.com/library/dn798927.aspx) auf MSDN. Viele der Beispielabfragen in diesem Thema enthalten Leerzeichen in Fiddler nicht zulässig sind. Jeder Raum ein +-Zeichen vor dem Einfügen in die Abfrage vor der Abfrage Fiddler Zeichenfolge ersetzen.

**Leerzeichen ersetzt werden:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Nachdem durch Leerzeichen ersetzt werden +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Abfragen des Systems

Sie können auch das System Dokument zählt und Speicherbedarf Abfragen. Auf der Registerkarte **Composer** Wunsch sieht etwa wie folgt und Zurückgeben der Antwort eine Anzahl von Dokumenten und Speicherplatz.

 ![][5]

1.  Wählen Sie **Abrufen**.

2.  Geben Sie eine URL mit Dienst-URL, gefolgt von "/ Hotels/Indizes/Statistiken? API-Version 2015-02-28 =":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Angeben des Anforderungsheaders gültigen Werte für den Dienst Host und API-Schlüssel ersetzen.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Den Anforderungstext leer.

5.  Klicken Sie auf **Ausführen**. Sie sollten einen HTTP 200 Statuscode in der Sitzungsliste sehen. Wählen Sie den Eintrag für den Befehl.

6.  Klicken Sie auf die Registerkarte **Prüfer** , klicken Sie auf die Registerkarte **Header** und wählen Sie das JSON-Format. Sie sollten die Dokumentgröße Count und Speicher (in KB) sehen.

## <a name="next-steps"></a>Nächste Schritte

Ohne Code [Verwalten Suchdienst auf Azure](search-manage.md) Weitere Ansatz zur Verwaltung und Verwendung von Azure suchen.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
