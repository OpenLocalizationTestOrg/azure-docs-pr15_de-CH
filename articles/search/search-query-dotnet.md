<properties
    pageTitle="Abfragen mit .NET SDK Azure Suchindex | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Erstellen einer Suchabfrage in Azure Suche und Suchparameter Suchergebnisse filtern und Sortieren verwenden."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Fragen Sie mit .NET SDK Azure Suchindex ab
> [AZURE.SELECTOR]
- [Übersicht](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

In diesem Artikel wird einen Index mit dem [Azure Suche .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)Abfragen anzeigen

Vor dieser exemplarischen Vorgehensweise sollten Sie bereits [ein Azure-Suchindex erstellt](search-what-is-an-index.md) und [mit Daten gefüllt](search-what-is-data-import.md)haben.

Beachten Sie, dass alle Beispielcode in diesem Artikel in C# geschrieben ist. Sie finden die vollständige Quelle code [auf GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>. Identifizieren der Azure-Suchdienst Abfrage API-Schlüssel
Erstellung einer Azure Suchindex können Sie fast Abfragen mit .NET SDK. Zuerst müssen Sie einen Abfrage-api-Schlüssel, die für den Suchdienst war, den Sie bereitgestellt bekommen. .NET SDK sendet diesen API-Schlüssel bei jeder Anforderung an den Dienst. Ein gültiger Schlüssel stellt Vertrauen auf pro Anfrage, zwischen dem Senden der Anforderung und der Dienst, der es behandelt.

1. Um Ihren Dienst-api-Schlüssel müssen Sie in [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu der Azure-Suchdienst blade
3. Klicken Sie auf das Symbol "Schlüssel"

Ihr Dienst haben *Administratorschlüssel* *Abfrage Schlüssel*.

  - Die primären und sekundären *Administratorschlüssel* gewähren Sie Vollzugriff auf alle Vorgänge, einschließlich der Möglichkeit zur Verwaltung des Dienstes erstellen und Löschen von Indizes, Indexer und Datenquellen. Gibt es zwei Schlüssel, damit weiterhin dem Sekundärschlüssel verwenden, möchten Sie den Primärschlüssel und umgekehrt zu regenerieren.
  - Die *Abfrage Schlüssel* gewährt schreibgeschützten Zugriff auf Indizes und Dokumente und normalerweise für Clientanwendungen, die Suchabfragen ausstellen verteilt.

Für die Zwecke der Abfrage eines Indexes können Sie eines der Abfrage. Der Administratorschlüssel können auch für Abfragen verwendet werden, aber verwenden Sie einen Abfrageschlüssel in Anwendungscode als besser das [Prinzip der niedrigsten Berechtigung](https://en.wikipedia.org/wiki/Principle_of_least_privilege)folgt.

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Erstellen Sie eine Instanz der SearchIndexClient-Klasse
Um Abfragen mit Azure Suche .NET SDK müssen eine Instanz der `SearchIndexClient` Klasse. Diese Klasse verfügt über mehrere Konstruktoren. Eine wird der Dienstname suchen, Indexnamen und `SearchCredentials` Objekt als Parameter. `SearchCredentials`umschließt die API-Schlüssel.

Der folgende Code erstellt ein neues `SearchIndexClient` "Hotels" Index (erstellt in [Erstellen einer Azure Suchindex mit .NET SDK](search-create-index-dotnet.md)) unter Verwendung Werte für Suche Dienstname und API-Schlüssel, die in der Anwendungskonfigurationsdatei gespeichert werden (`app.config` oder `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`hat ein `Documents` Eigenschaft. Diese Eigenschaft stellt die Methoden, die Azure Suchindizes Abfragen.

## <a name="iii-query-your-index"></a>III. Den Index Abfragen
Mit .NET SDK ist so einfach wie das Aufrufen der `Documents.Search` Methode für die `SearchIndexClient`. Diese Methode hat einige Parameter, mit dem Suchtext entlang einer `SearchParameters` -Objekt, um die Abfrage zu verfeinern.

#### <a name="types-of-queries"></a>Arten von Abfragen
Sind die zwei [Abfragetypen](search-query-overview.md#types-of-queries) verwenden Sie `search` und `filter`. Ein `search` Abfrage sucht einen oder mehrere Begriffe in alle _durchsuchbaren_ Felder im Index. Ein `filter` Abfrage ergibt einen booleschen Ausdruck über alle _filterbar_ Felder in einem Index.

Suchen und Filtern erfolgt mit der `Documents.Search` Methode. Eine Suchabfrage kann übergeben werden die `searchText` Parameter während ein Filterausdrucks in übergeben werden kann die `Filter` -Eigenschaft des der `SearchParameters` Klasse. Einfach Filtern ohne `"*"` für die `searchText` Parameter. Suche ohne Filterung lassen die `Filter` -Eigenschaft festlegen, oder übergeben Sie keiner `SearchParameters` Instanz zu.

#### <a name="example-queries"></a>Beispiel

Der folgende Beispielcode zeigt unterschiedliche Verwendungsmöglichkeiten von "Hotels" Index im [Erstellen einer Azure Suchindex mit .NET SDK](search-create-index-dotnet.md#DefineIndex)definierten Abfragen. Sind die Dokumente in den Suchergebnissen zurückgegeben Instanzen der `Hotel` -Klasse, die [Den Datenimport in Azure Suche mit .NET SDK](search-import-data-dotnet.md#HotelClass)definiert wurde. Der Beispielcode verwendet ein `WriteDocuments` Methode, um die Suchergebnisse auf der Konsole ausgegeben. Diese Methode wird im nächsten Abschnitt beschrieben.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Behandeln von Suchergebnissen
Die `Documents.Search` -Methode gibt ein `DocumentSearchResult` -Objekt, das die Ergebnisse der Abfrage enthält. Das Beispiel im vorherigen Abschnitt verwendet eine Methode namens `WriteDocuments` Suchergebnisse auf der Konsole ausgegeben:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Hier wird das Ergebnis Aussehen für Abfragen im vorherigen Abschnitt "Hotels" Index mit den Beispieldaten in [Datenimport in Azure Suche mit .NET SDK](search-import-data-dotnet.md)aufgefüllt wird vorausgesetzt:

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

Der obige Beispielcode verwendet die Konsole Suchergebnisse ausgegeben. Sie müssen ebenfalls Suchergebnisse in Ihrer Anwendung. Finden Sie [in diesem Beispiel auf GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) Beispiel Suchergebnisse in einer ASP.NET MVC-basierten Anwendung gerendert.
