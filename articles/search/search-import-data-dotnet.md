<properties
    pageTitle="Hochladen von Daten in Azure Suche mit .NET SDK | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Informationen Sie zum Hochladen von Daten auf einen Index in Azure Suche mit .NET SDK."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Upload von Daten mit .NET SDK Azure-Suche
> [AZURE.SELECTOR]
- [Übersicht](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)

In diesem Artikel werden wie [Azure Suche .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) Datenimport in Azure Search-Index anzeigen

Vor dieser exemplarischen Vorgehensweise sollten Sie bereits [ein Azure-Suchindex erstellt](search-what-is-an-index.md)haben. Außerdem wird vorausgesetzt, dass bereits erstellt ein `SearchServiceClient` Objekt, wie in [Erstellen einer Azure Suchindex mit .NET SDK](search-create-index-dotnet.md#CreateSearchServiceClient).

Beachten Sie, dass alle Beispielcode in diesem Artikel in C# geschrieben ist. Sie finden die vollständige Quelle code [auf GitHub](http://aka.ms/search-dotnet-howto).

Um den Index mit .NET SDK Dokumente abzulegen, müssen Sie:

  1. Erstellen einer `SearchIndexClient` Objekt Suchindex herstellen.
  2. Erstellen Sie ein `IndexBatch` mit den Dokumenten hinzugefügt, geändert oder gelöscht.
  3. Aufrufen der `Documents.Index` Methode der `SearchIndexClient` an der `IndexBatch` , Suchindex.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>. Erstellen Sie eine Instanz der SearchIndexClient-Klasse
Zum Importieren von Daten in den Index mit dem Azure Suche .NET SDK müssen eine Instanz der `SearchIndexClient` Klasse. Sie können diese Instanz erstellen selbst, aber es einfacher haben Sie bereits ein `SearchServiceClient` Instanz aufrufen seiner `Indexes.GetClient` Methode. Sieht z. B. wie Sie erhalten würde eine `SearchIndexClient` für den Index mit dem Namen "Hotels" aus einer `SearchServiceClient` mit dem Namen `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] In einer normalen Suchmaschine Index Management und Auffüllung erfolgt durch eine separate Komponente Suchabfragen. `Indexes.GetClient`eignet sich für einen Index auffüllen, da diese Probleme auf einem anderen gespeichert `SearchCredentials`. Dazu übergeben Sie den Administratorschlüssel, die zum Erstellen der `SearchServiceClient` zum neuen `SearchIndexClient`. Im Teil Ihrer Anwendung, das Abfragen ausführt, es ist jedoch besser erstellen die `SearchIndexClient` direkt damit Abfrageschlüssel statt einer Administratorschlüssel übergeben können. Dies entspricht dem [Prinzip der geringsten Rechte](https://en.wikipedia.org/wiki/Principle_of_least_privilege) und die Anwendung sicherer machen soll. Administratorschlüssel und Abfrage Schlüssel [Azure Suche REST-API-Referenz auf MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)finden Sie.

`SearchIndexClient`hat ein `Documents` Eigenschaft. Diese Eigenschaft stellt die Methoden zum Hinzufügen, ändern, löschen oder Dokumente in den Index Abfragen müssen.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Entscheiden Sie, welche Indizierung verwenden
Zum Importieren von Daten mit .NET SDK müssen Verpacken Sie Ihre Daten in ein `IndexBatch` Objekt. Ein `IndexBatch` kapselt eine Auflistung von `IndexAction` Objekte, von denen jede enthält ein Dokument und eine Eigenschaft, die Azure Search auszuführende Aktion an diesem Dokument (Upload, zusammenführen, löschen usw.) wird. Je nach der unter Aktionen Sie wählen, nur bestimmte Felder für jedes Dokument aufgenommen werden müssen:

Aktion | Beschreibung | Pflichtfelder für jedes Dokument | Notizen
--- | --- | --- | ---
`Upload` | Ein `Upload` Aktion ist vergleichbar mit einer "Upsert", wo das Dokument eingefügt, wenn es neu ist und aktualisiert oder ausgetauscht, wenn vorhanden. | Taste und andere Felder, die Sie definieren möchten, | Beim Aktualisieren und ein vorhandenes Dokument ersetzen, haben jedes Feld, das nicht in der Anforderung der Feldgruppe in `null`. Dies geschieht auch, wenn das Feld zuvor ein Wert ungleich Null festgelegt wurde.
`Merge` | Aktualisiert ein vorhandenes Dokument mit der angegebenen Felder. Wenn das Dokument nicht im Index vorhanden ist, wird die Zusammenführung fehl. | Taste und andere Felder, die Sie definieren möchten, | Jedes Feld, das an eine Zusammenführung ersetzt das vorhandene Feld im Dokument. Dies umfasst auch Felder vom Typ `DataType.Collection(DataType.String)`. Angenommen, das Dokument enthält ein Feld `tags` mit `["budget"]` und führen Sie einen Seriendruck mit `["economy", "pool"]` für `tags`, der Endwert von der `tags` Feld `["economy", "pool"]`. Nicht `["budget", "economy", "pool"]`.
`MergeOrUpload` | Dadurch verhält sich wie `Merge` ein Dokument mit dem angegebenen Schlüssel im Index bereits vorhanden. Wenn das Dokument nicht vorhanden ist, verhält er sich wie `Upload` mit einem neuen Dokument. | Taste und andere Felder, die Sie definieren möchten, | -
`Delete` | Der Index entfernt des angegebenen Dokuments. | nur Schlüssel | Alle Felder Geben Sie nicht das Schlüsselfeld ignoriert. Wenn Sie einzelnen Felder aus einem Dokument entfernen möchten, verwenden Sie `Merge` einfach und stattdessen das Feld explizit auf null festgelegt.

Sie können angeben, welche Aktion mit verschiedenen statischen Methoden von möchten die `IndexBatch` und `IndexAction` Klassen, wie im nächsten Abschnitt.

## <a name="iii-construct-your-indexbatch"></a>III. Erstellen der IndexBatch
Jetzt wissen Sie welche Aktionen auf Ihre Dokumente, sind Sie bereit zum Erstellen der `IndexBatch`. Das folgende Beispiel veranschaulicht, wie zu wenige unterschiedliche Aktionen erstellen. Beachten Sie, dass unser eine benutzerdefinierte Klasse namens Beispiel `Hotel` , der ein Dokument im Index "Hotels" zugeordnet.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

In diesem Fall verwenden wir `Upload`, `MergeOrUpload`, und `Delete` als unsere Aktionen suchen wie die Methoden in der `IndexAction` Klasse.

Angenommen Sie, dieses Beispiel "Hotels" Index bereits mehrere Dokumente enthält. Beachten Sie, wie wir nicht haben, alle möglichen Dokumentfelder Geben Sie bei Verwendung `MergeOrUpload` und wie wir nur die Dokumentschlüssel (`HotelId`) Verwendung `Delete`.

Beachten Sie, dass in einer Volltextindizierung Anforderung nur bis zu 1000 Dokumente enthalten können.

> [AZURE.NOTE] In diesem Beispiel werden wir verschiedene Dokumente unterschiedliche Aktionen zuweisen. Wenn Sie die gleichen Aktionen allen Dokumenten im Stapel anstatt `IndexBatch.New`, können Sie die statischen Methoden der `IndexBatch`. Sie können z. B. Batches erstellen, indem `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, oder `IndexBatch.Delete`. Diese Methoden nehmen eine Sammlung von Dokumenten (Objekte vom Typ `Hotel` in diesem Beispiel) anstelle von `IndexAction` Objekte.

## <a name="iv-import-data-to-the-index"></a>IV. Importieren von Daten in den index
Jetzt haben Sie ein initialisiertes `IndexBatch` Objekt durch Aufrufen der Index senden `Documents.Index` auf der `SearchIndexClient` Objekt. Das folgende Beispiel zeigt, wie `Index`, sowie einige zusätzliche Schritte ausgeführt werden sollen:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Hinweis Die `try` / `catch` um den Aufruf der `Index` Methode. Catch-Block behandelt wichtige Fehler beim Indizieren bei. Fällt der Azure-Suchdienst Dokumente im Batch index eine `IndexBatchException` wird von `Documents.Index`. Dies kann auftreten, wenn Sie Dateien indizieren, während der Dienst überlastet ist. **Wir empfehlen ausdrücklich hier im Code behandeln.** Verzögern und dann erneuter Fehler Dokumente oder protokollieren und weiter wie geschieht, oder Sie können etwas je nach der Anwendung Daten Konsistenz.

Zum Schluss den Code im Beispiel oben zwei Sekunden verzögert. Indizierung geschieht asynchron in der Azure-Suchdienst beispielanwendung muss warten Sie kurz, dass Dokumente verfügbar sind. Verzögert, so sind in der Regel nur in Demos, Tests und Anwendungsbeispiele erforderlich.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>Wie .NET SDK Dokumente verarbeitet

Sie Fragen wie die Azure .NET SDK Instanzen einer benutzerdefinierten Klasse wie Hochladen ist `Hotel` auf den Index. Um diese Frage zu beantworten, betrachten wir die `Hotel` Klasse definiert unter [Erstellen einer Azure Suchindex mit .NET SDK](search-create-index-dotnet.md#DefineIndex)IndexSchema zugeordnet ist:

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Zu beachten ist, dass jede öffentliche Eigenschaft der `Hotel` entspricht einem Feld in der Indexdefinition jedoch einen entscheidenden Unterschied: der Name der einzelnen beginnt mit Kleinbuchstaben ("Höckerschreibweise"), während der Name jeder öffentlichen Eigenschaft der `Hotel` beginnt mit einem Großbuchstaben ("Pascal-Schreibweise"). Dies ist ein allgemeines Szenario in .NET Applications, die Datenbindungsfunktionen ausführen ist das Zielschema außerhalb der Kontrolle des Anwendungsentwicklers. Anstatt gegen Benennungsrichtlinien mit Eigenschaft Namen Höckerschreibweise .NET kann man das SDK Höckerschreibweise automatisch mit die Namen zuordnen der `[SerializePropertyNamesAsCamelCase]` Attribut.

> [AZURE.NOTE] Das .NET SDK Azure verwendet die Bibliothek [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) , die benutzerdefinierte Modellkonfiguration Objekte in und aus JSON serialisiert und deserialisiert. Sie können diese Serialisierung ggf. anpassen. Weitere Informationen finden Sie unter [Upgrading to Azure Suche .NET SDK, Version 1.1](search-dotnet-sdk-migration.md#WhatsNew). Ein Beispiel dafür ist die Verwendung der `[JsonProperty]` -Attribut auf die `DescriptionFr` -Eigenschaft im obigen Code.

Die zweite wichtige an der `Hotel` Klasse sind die Datentypen der öffentlichen Eigenschaften. .NET Datentypen Eigenschaften zugeordnet, die entsprechenden Feldtypen in der Indexdefinition. Beispielsweise die `Category` string-Eigenschaft wird die `category` ist vom Typ `DataType.String`. Gibt es ähnliche Zuordnungen zwischen `bool?` und `DataType.Boolean`, `DateTimeOffset?` und `DataType.DateTimeOffset`usw.. Die Einzelheiten der Zuordnung sind dokumentiert die `Documents.Get` -Methode auf [MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Die Möglichkeit, eigene Klassen vollendet Dokumente; Sie können auch Suchergebnisse abgerufen und automatisch deserialisieren sie Ihrer Wahl ein SDK im [nächsten Artikel](search-query-dotnet.md)angezeigt.

> [AZURE.NOTE] Das Azure .NET SDK unterstützt auch dynamisch typisierte Dokumente mit den `Document` Klasse, die eine Schlüssel-Wert-Zuordnung von Feldnamen Feldwerte. Dies ist nützlich in Szenarien IndexSchema zur Entwurfszeit nicht kennen oder es wäre unpraktisch, Modell Klassen binden. Alle Methoden im SDK, die mit Dokumenten haben-Überladung, die mit der `Document` Klasse sowie stark typisierte Überladung, die einen generischen Typparameter akzeptieren. Im Beispielcode in diesem Artikel werden nur diese verwendet. Erfahren Sie mehr über die `Document` [auf der MSDN-](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx)Klasse.

**Wichtiger Hinweis zur Datentypen**

Entwerfen eigener Modellklassen ein Azure-Suchindex zuordnen empfehlen wir Deklarieren von Eigenschaften von Werttypen wie `bool` und `int` , NULL-Werte zulässt (z. B. `bool?` anstelle von `bool`). Wenn Sie eine NULL-Eigenschaft verwenden, müssen Sie enthalten keine Dokumente im Index einen null-Wert für das entsprechende Feld **garantieren** . Weder das SDK der Azure-Suchdienst hilft Ihnen dies zu erzwingen.

Dies ist nicht nur ein hypothetisches Problem: ein Szenario, in dem Sie einen vorhandenen Index vom Typ ein neues Feld hinzufügen `DataType.Int32`. Nach der Aktualisierung der Indexdefinition haben alle Dokumente einen Nullwert für das neue Feld (da alle nullable in Azure Suche sind). Verwenden Sie dann eine mit einem NULL- `int` -Eigenschaft für das Feld, erhalten Sie eine `JsonSerializationException` beim Dokumente folgendermaßen:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Aus diesem Grund empfiehlt Typen in Ihrem Modellklassen als bewährte Methode verwenden.

## <a name="next"></a>Weiter
Nach Auffüllen Azure Suchindex, werden Sie jetzt Abfragen nach Dokumenten suchen. Details finden Sie in der [Abfrage der Azure-Suchindex](search-query-overview.md) .
