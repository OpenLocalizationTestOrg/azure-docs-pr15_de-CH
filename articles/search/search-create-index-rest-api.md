<properties
    pageTitle="Die REST-API mit Azure Search Index erstellen | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Erstellen eines Indexes in der Azure Suche HTTP REST API-Code."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>Erstellen Sie die REST-API mit Azure Search index
> [AZURE.SELECTOR]
- [Übersicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


Dieser Artikel führt Sie durch einen Azure Search- [Index](https://msdn.microsoft.com/library/azure/dn798941.aspx) mit Azure Suche REST API erstellen.

Diese Anleitung und Erstellen eines Indexes sollten Sie bereits [erstellt einen Azure-Suchdienst](search-create-service-portal.md)verfügen.

Erstellen Sie eine Azure Suchindex mithilfe der REST-API werden Sie Ihre Azure-Suchdienst-URL Endpunkt eine HTTP POST-Anforderung ausgeben. Die Indexdefinition ist im Hauptteil Anforderung als wohlgeformt JSON-Inhalt enthalten.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>. Identifizieren der Azure-Suchdienst Admin-api-Schlüssel
Damit Sie einen Azure-Suchdienst bereitgestellt haben, können Sie HTTP-Anfragen an den Dienst URL Endpunkt mithilfe der REST-API ausstellen. Allerdings müssen *Alle* API api-Schlüssel enthalten, der für den Suchdienst generiert wurde, den Sie bereitgestellt. Ein gültiger Schlüssel stellt Vertrauen auf pro Anfrage, zwischen dem Senden der Anforderung und der Dienst, der es behandelt.

1. Um Ihren Dienst-api-Schlüssel müssen Sie in [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu der Azure-Suchdienst blade
3. Klicken Sie auf das Symbol "Schlüssel"

Ihr Dienst haben *Administratorschlüssel* *Abfrage Schlüssel*.

 - Die primären und sekundären *Administratorschlüssel* gewähren Sie Vollzugriff auf alle Vorgänge, einschließlich der Möglichkeit zur Verwaltung des Dienstes erstellen und Löschen von Indizes, Indexer und Datenquellen. Gibt es zwei Schlüssel, damit weiterhin dem Sekundärschlüssel verwenden, möchten Sie den Primärschlüssel und umgekehrt zu regenerieren.
 - Die *Abfrage Schlüssel* gewährt schreibgeschützten Zugriff auf Indizes und Dokumente und normalerweise für Clientanwendungen, die Suchabfragen ausstellen verteilt.

Zum Erstellen eines Indexes können entweder primäre oder sekundäre Administratorschlüssel.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. Verwendung von JSON wohlgeformt Azure Suchindex definieren
Eine einzelne HTTP POST-Anforderung an den Dienst erstellt den Index. Der Hauptteil der HTTP POST-Anforderung wird ein JSON-Objekt enthalten, das Azure Suchindex definiert.

1. Die erste Eigenschaft des JSON-Objekts ist der Name des Indexes.
2. Die zweite Eigenschaft des JSON-Objekts ist ein JSON-Array mit dem Namen `fields` ein separates JSON-Objekt für jedes Feld im Index enthält. Diese JSON-Objekte enthalten mehrere Name-Wert-Paare für jedes Feld Attribute einschließlich "name", "Typ" usw..

Es ist wichtig, dass Sie dabei Ihre Erfahrung und Bedürfnisse suchen Benutzer bei Index als jedes Feld die [richtigen Attribute](https://msdn.microsoft.com/library/azure/dn798941.aspx)zugewiesen werden muss. Diese Attribute welche Features (filtern, facettierung sortieren Volltextsuche usw.) anwenden, welche Felder. Für jedes Attribut nicht angeben, werden standardmäßig die entsprechende Suchfunktion aktivieren, wenn Sie ausdrücklich deaktivieren.

In unserem Beispiel haben wir unsere "Hotels" bezeichnet und Felder wie folgt definiert:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Wir haben sorgfältig Indexattribute für jedes Feld basierend auf wie wir glauben, dass sie in einer Anwendung verwendet werden. Beispielsweise `hotelId` ist ein eindeutiger Schlüssel suchen Hotels wahrscheinlich nicht wissen, damit wir mit Volltextsuche für dieses Feld deaktivieren Personen `searchable` , `false`, spart Platz im Index.

Bitte beachten Sie, dass genau ein Feld im Index vom Typ `Edm.String` festgelegte 'Key'-Feld sein.

Die Indexdefinition oben verwendet einen benutzerdefinierte Sprache Analyzer für die `description_fr` Feld da französischen Text gespeichert werden soll. Finden Sie [auf MSDN unterstützen Sprache](https://msdn.microsoft.com/library/azure/dn879793.aspx) sowie die entsprechenden [Blogbeitrag](https://azure.microsoft.com/blog/language-support-in-azure-search/) weitere Sprache Analysatoren.

## <a name="iii-issue-the-http-request"></a>III. Die HTTP-Anforderung
1. Erteilen Sie die Indexdefinition als Hauptteil der Anforderung verwenden, Ihre Azure Search Service-Endpunkt-URL eine HTTP POST-Anforderung. In der URL müssen Sie Dienstnamen als Hostname verwenden und die entsprechenden `api-version` als Abfrageparameter (aktuelle API-Version ist `2015-02-28` zum Zeitpunkt der Veröffentlichung dieses Dokuments).
2. Geben Sie im Header Anforderung der `Content-Type` als `application/json`. Müssen auch Ihren Dienst Admin Key angeben, die Sie in Schritt I ermittelt die `api-key` Header.


Sie müssen eigene Service Name und API-Schlüssel um die folgende Anforderung ausgeben:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Für eine erfolgreiche Anforderung sollte Statuscode 201 (erstellt) angezeigt werden. Weitere Informationen zum Erstellen eines Indexes über die REST-API finden Sie die API-Referenz auf [MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx). Weitere Informationen zu anderen HTTP-Statuscodes, die bei Fehler zurückgegeben werden kann, finden Sie unter [HTTP-Statuscodes (Azure suchen)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Wenn Sie einen Index löschen, nur ausgegeben Sie HTTP DELETE-Anforderung. Dies ist z. B. wie wir "Hotels" Index löschen möchten:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Weiter
Nach dem Erstellen einer Azure Suchindex, werden Sie zum [Hochladen in den Index](search-what-is-data-import.md) so können Sie Ihre Daten suchen.
