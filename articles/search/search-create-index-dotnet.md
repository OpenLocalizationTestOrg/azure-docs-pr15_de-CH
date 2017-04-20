<properties
    pageTitle="Erstellen einer Azure Suchindex mit .NET SDK | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Erstellen Sie einen Index im Code das Azure .NET SDK."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Erstellen einer Azure Suchindex mit .NET SDK
> [AZURE.SELECTOR]
- [Übersicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


Dieser Artikel führt Sie schrittweise mit [Azure Suche .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)Azure Search [Index](https://msdn.microsoft.com/library/azure/dn798941.aspx) erstellen.

Diese Anleitung und Erstellen eines Indexes sollten Sie bereits [erstellt einen Azure-Suchdienst](search-create-service-portal.md)verfügen.

Beachten Sie, dass alle Beispielcode in diesem Artikel in C# geschrieben ist. Sie finden die vollständige Quelle code [auf GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>. Identifizieren der Azure-Suchdienst Admin-api-Schlüssel
Damit Sie Azure-Suchdienst bereitgestellt haben, können Sie fast gegen Ihr Dienstendpunkt mit .NET SDK ausstellen. Zuerst müssen Sie einen Admin-api-Schlüssel, die für den Suchdienst war, den Sie bereitgestellt bekommen. .NET SDK sendet diesen API-Schlüssel bei jeder Anforderung an den Dienst. Ein gültiger Schlüssel stellt Vertrauen auf pro Anfrage, zwischen dem Senden der Anforderung und der Dienst, der es behandelt.

1. Um Ihren Dienst-api-Schlüssel müssen Sie in [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu der Azure-Suchdienst blade
3. Klicken Sie auf das Symbol "Schlüssel"

Ihr Dienst haben *Administratorschlüssel* *Abfrage Schlüssel*.

  - Die primären und sekundären *Administratorschlüssel* gewähren Sie Vollzugriff auf alle Vorgänge, einschließlich der Möglichkeit zur Verwaltung des Dienstes erstellen und Löschen von Indizes, Indexer und Datenquellen. Gibt es zwei Schlüssel, damit weiterhin dem Sekundärschlüssel verwenden, möchten Sie den Primärschlüssel und umgekehrt zu regenerieren.
  - Die *Abfrage Schlüssel* gewährt schreibgeschützten Zugriff auf Indizes und Dokumente und normalerweise für Clientanwendungen, die Suchabfragen ausstellen verteilt.

Zum Erstellen eines Indexes können entweder primäre oder sekundäre Administratorschlüssel.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Erstellen Sie eine Instanz der SearchServiceClient-Klasse
Um zu das Azure .NET SDK verwenden, müssen eine Instanz der `SearchServiceClient` Klasse. Diese Klasse verfügt über mehrere Konstruktoren. Die gewünschte nimmt Dienstnamen suchen und `SearchCredentials` Objekt als Parameter. `SearchCredentials`umschließt die API-Schlüssel.

Der folgende Code erstellt ein neues `SearchServiceClient` mit Werten für Suche Dienstname und API-Schlüssel, die in der Anwendungskonfigurationsdatei gespeichert werden (`app.config` oder `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`hat ein `Indexes` Eigenschaft. Diese Eigenschaft enthält alle Methoden, die Sie erstellen Liste müssen aktualisieren oder Löschen von Azure Suchindizes.

> [AZURE.NOTE] Die `SearchServiceClient` Klasse verwaltet Verbindung zum Suchdienst. Um zu vermeiden, zu viele Verbindungen öffnen, sollten eine Instanz freigeben `SearchServiceClient` in Ihrer Anwendung, wenn möglich. Ihre Methoden sind threadsicher diese Freigabe aktivieren.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Definieren der Azure Search-Index mit dem `Index` Klasse
Ein einzelner Aufruf an die `Indexes.Create` -Methode den Index erstellt. Diese Methode übernimmt als Parameter eine `Index` Objekt, Azure Suchindex definiert. Müssen Sie erstellen ein `Index` Objekt, und initialisieren Sie es wie folgt:

1. Festlegen der `Name` -Eigenschaft des der `Index` -Objekt, der Name des Indexes.
2. Festlegen der `Fields` -Eigenschaft des der `Index` -Objekt, ein Array von `Field` Objekte. Jedes der `Field` Objekte definiert das Verhalten eines Felds im Index. Sie können den Namen des Feldes an den Konstruktor mit den Datentyp (oder Analyzer für Zeichenfolgenfelder) bereitstellen. Sie können auch andere Eigenschaften wie festlegen `IsSearchable`, `IsFilterable`usw..

Es ist wichtig, dass Sie Ihre Erfahrung und Bedürfnisse suchen Benutzer beim Entwerfen des Index jedes Bedenken `Field` muss die [entsprechenden Eigenschaften](https://msdn.microsoft.com/library/azure/dn798941.aspx)zugewiesen werden. Diese Eigenschaften steuern die Funktionen (filtern, facettierung sortieren Volltextsuche usw.) suchen gelten für die Felder. Für jede Eigenschaft nicht explizit festgelegt, die `Field` Klasse standardmäßig die entsprechende Suchfunktion ausdrücklich aktivieren deaktivieren.

In unserem Beispiel haben wir unsere "Hotels" bezeichnet und Felder wie folgt definiert:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Sorgfältig haben wir die Eigenschaftenwerte für jedes `Field` basierend auf wie wir diese in einer Anwendung verwendet werden. Beispielsweise ist wahrscheinlich Menschen Hotels Schlüsselwort entspricht im Interesse der `description` Feld, damit wir Volltextsuche für das Feld festlegen aktivieren `IsSearchable` , `true`.

Bitte beachten Sie, dass genau ein Feld im Index vom Typ `DataType.String` festlegen als _Feld_ angegebenen sein `IsKey` , `true` (finden Sie unter `hotelId` im obigen Beispiel).

Die Indexdefinition oben verwendet einen benutzerdefinierte Sprache Analyzer für die `description_fr` Feld da französischen Text gespeichert werden soll. Finden Sie [auf MSDN unterstützen Sprache](https://msdn.microsoft.com/library/azure/dn879793.aspx) sowie die entsprechenden [Blogbeitrag](https://azure.microsoft.com/blog/language-support-in-azure-search/) weitere Sprache Analysatoren.

> [AZURE.NOTE]  Beachten Sie, dass durch übergeben `AnalyzerName.FrLucene` im Konstruktor der `Field` werden automatisch vom Typ `DataType.String` und `IsSearchable` legen auf `true`.

## <a name="iv-create-the-index"></a>IV. Index erstellen
Jetzt haben Sie ein initialisiertes `Index` -Objekt können Sie den Index erstellen, einfach durch Aufrufen von `Indexes.Create` auf der `SearchServiceClient` Objekt:

```csharp
serviceClient.Indexes.Create(definition);
```

Für eine erfolgreiche Anforderung gibt normalerweise die Methode zurück. Liegt ein Problem mit der Anforderung wie ein ungültiger Parameter, löst die Methode `CloudException`.

Wenn Sie einen Index löschen, rufen die `Indexes.Delete` -Methode der `SearchServiceClient`. Dies ist z. B. wie wir "Hotels" Index löschen möchten:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] Der Beispielcode in diesem Artikel verwendet die synchronen Methoden das .NET SDK Azure Einfachheit. Asynchronen Methoden in der Anwendung verwenden, skalierbare und Reaktionsfähigkeit empfohlen. Beispielsweise in den obigen Beispielen können `CreateAsync` und `DeleteAsync` anstelle von `Create` und `Delete`.

## <a name="next"></a>Weiter
Nach dem Erstellen einer Azure Suchindex, werden Sie zum [Hochladen in den Index](search-what-is-data-import.md) so können Sie Ihre Daten suchen.
