<properties
   pageTitle="Das Azure .NET SDK Version 1.1 aktualisieren | Microsoft Azure | Gehostete Cloud-Suchdienst"
   description="Das Azure .NET SDK Version 1.1 aktualisieren"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Azure Suche .NET SDK Version 2.0-Vorschau aktualisieren

Verwendung von [Azure Suche .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)Version 1.1 hilft dieser Artikel Ihnen die Anwendung für die nächste Hauptversion 2.0 Vorschau aktualisieren.

Eine allgemeine Anleitung SDK sowie Beispiele finden Sie unter [Azure Suche eine verwenden](search-howto-dotnet-sdk.md).

Version 2.0-Vorschau von Azure Suche .NET SDK enthält aus früheren Versionen geändert. Sind meist gering, so ändern den Code nur minimalen Aufwand erfordert. Siehe [Schritte zur Aktualisierung](#UpgradeSteps) Informationen den Code ändern, neue SDK-Version verwenden.

> [AZURE.NOTE] Wenn Sie Version 0,13 Vorschau oder älter verwenden, sollten Sie Version 1.1 erste und dann ein Upgrade auf 2.0 Vorschau aktualisieren. Siehe [Anhang: Schritte auf Version 1.1 aktualisiert](#UpgradeStepsV1) Informationen.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Was ist neu in Version 2.0-Vorschau

Version 2.0-Vorschau ist die erste Version von Azure Suche .NET SDK auf eine Vorschauversion von Azure Search REST API, insbesondere 2015-02-28-Vorschau. Dadurch viele Vorschau Features von Azure Suche von einer, einschließlich der folgenden:

- [Benutzerdefinierte Analyzer](https://aka.ms/customanalyzers)
- Unterstützung von [Azure BLOB-Speicher](search-howto-indexing-azure-blob-storage.md) und [Azure-Tabellenspeicher](search-howto-indexing-azure-tables.md) indexer
- Indexer Anpassung über [Feld Zuordnung](search-indexer-field-mappings.md)
- ETags Unterstützung aktivieren sicherer gleichzeitige Aktualisierung Indexdefinitionen Indexer und Datenquellen
- Unterstützung für .NET Core und .NET tragbaren 111

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Schritte zur Aktualisierung

Aktualisieren Sie zunächst die NuGet-Referenz für `Microsoft.Azure.Search` mithilfe der NuGet Paket-Manager-Konsole oder mit der rechten Maustaste auf die Projektverweise und "Verwalten von NuGet-Paketen..." in Visual Studio auswählen. Sicher aktivieren Pre-Release-Pakete in NuGet "Pakete verwalten" auswählen "Include Prerelease" Fenster in Visual Studio oder mithilfe der `-IncludePrerelease` verwenden Sie NuGet Paket-Manager Console switch.

Nachdem NuGet die neuen Pakete und deren abhängigen Dateien heruntergeladen, erstellen Sie Ihr Projekt neu. Je nach Code Struktur kann es erfolgreich neu erstellt. In diesem Fall können Sie gehen!

Wenn der Build fehlschlägt, sollte einen Buildfehler wie folgt angezeigt werden:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Der nächste Schritt ist Buildfehler zu beheben. Einzelheiten Sie [Breaking Changes in Version 2.0-Vorschau](#ListOfChanges) Ursachen den Fehler und beheben.

Möglicherweise werden zusätzliche Warnungen veralteter Methoden oder Eigenschaften angezeigt. Die Warnung enthält Hinweise zu veralteten Funktionen anstelle. Wenn Ihre Anwendung z. B. die `SearchRequestOptions.RequestId` Eigenschaft, Sie erhalten eine Warnung, die besagt,`"This property is deprecated. Please use ClientRequestId instead."`

Nachdem Sie alle Buildfehler behoben haben, können Sie Änderungen der Anwendung neue Funktionalität nutzen möchten. Neue Features im SDK sind [neu in der Version 2.0-Vorschau](#WhatsNew)ist aufgeführt.

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Grundlegend geändert in Version 2.0-Vorschau

Es ist nur eine unterbrechende Änderung in Version 2.0-Vorschau, die neben Ihrer Anwendung neu zu gelangen.

### <a name="indexesgetclient-return-type"></a>Rückgabetyp erhalten

Die `Indexes.GetClient` -Methode hat einen neuen Rückgabetyp. Früher zurückgegeben `SearchIndexClient`, aber dies wurde zu `ISearchIndexClient` in Version 2.0-Vorschau. Dies ist für Kunden, die simulieren möchten die `GetClient` -Methode für Komponententests durch Rückgabe eine simulierte Implementierung des `ISearchIndexClient`.

#### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Es ist zu ändern, um Buildfehler zu beheben:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Abschluss
Weitere Informationen zur Verwendung von Azure Suche .NET SDK, finden Sie unter unserer kürzlich aktualisierte- [Vorgehensweisen](search-howto-dotnet-sdk.md).

Wir begrüßen Ihr Feedback zum SDK. Wenn Probleme auftreten, können Sie Fragen zu [Azure Search MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch). Wenn Sie einen Fehler finden, können Sie ein Problem im [Azure .NET SDK GitHub Repository](https://github.com/Azure/azure-sdk-for-net/issues)-Datei. Sicherstellen der Problemtitel mit Präfix "Search SDK:".

Vielen Dank für die Verwendung von Azure Search!

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Anhang: Schritte auf Version 1.1 aktualisieren 

> [AZURE.NOTE] Dieser Abschnitt gilt nur für Benutzer von Azure Suche .NET SDK Version 0,13 Vorschau und ältere.

Aktualisieren Sie zunächst die NuGet-Referenz für `Microsoft.Azure.Search` mithilfe der NuGet Paket-Manager-Konsole oder mit der rechten Maustaste auf die Projektverweise und "Verwalten von NuGet-Paketen..." in Visual Studio auswählen.

Nachdem NuGet die neuen Pakete und deren abhängigen Dateien heruntergeladen, erstellen Sie Ihr Projekt neu.

Wenn Sie zuvor Version 1.0.0-preview, 1.0.1-preview oder 1.0.2-preview verwenden, sollte der Buildvorgang erfolgreich und Sie fertig!

Wenn Sie zuvor Version 0.13.0-preview verwenden oder älter, sollte Fehler wie den folgenden erstellen:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Der nächste Schritt ist die Buildfehler nacheinander beheben. Die meisten erfordern einige Klassen- und Namen, die im SDK umbenannt wurde. [Liste wichtiger Änderung in Version 1.1](#ListOfChangesV1) enthält eine Liste dieser ändern.

Wenn Sie benutzerdefinierte Klassen zum Modellieren von Dokumenten verwenden und diese Klassen Eigenschaften des NULL-primitive Typen (z. B. `int` oder `bool` in C#), gibt es eine Fehlerkorrektur in Version 1.1 des SDK die beachten. Weitere Informationen finden Sie unter [Fehlerbehebungen in Version 1.1](#BugFixesV1) .

Schließlich, wenn Buildfehler behoben haben, können Sie Änderungen neuen Funktionen nutzen möchten Sie die Anwendung.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Liste wichtiger Änderung in Version 1.1

Folgende ist sortiert die Wahrscheinlichkeit, dass die Änderung den Anwendungscode auswirkt.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch und IndexAction ändern

`IndexBatch.Create`umbenannt wurde `IndexBatch.New` und keine `params` Argument. Sie können `IndexBatch.New` für Batches, die Aktionen (führt, löschen usw.) mischen. Darüber hinaus gibt es neue statische Methoden zum Erstellen von Stapeln, die Aktionen entsprechen: `Delete`, `Merge`, `MergeOrUpload`, und `Upload`.

`IndexAction`verfügt über keine öffentlichen Konstruktoren und seine Eigenschaften jetzt unveränderlich. Verwenden Sie die neuen statischen Methoden zum Erstellen von Aktionen für verschiedene Zwecke: `Delete`, `Merge`, `MergeOrUpload`, und `Upload`. `IndexAction.Create`wurde entfernt. Wenn die Überladung verwendet wird, die ein Dokument akzeptiert, müssen Sie verwenden `Upload` statt.

##### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Es ist zu ändern, um Buildfehler zu beheben:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Wenn Sie möchten, können Sie weitere zu vereinfachen:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException ändern

Die `IndexBatchException.IndexResponse` -Eigenschaft wurde umbenannt `IndexingResults`, und der Typ ist nun `IList<IndexingResult>`.

##### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Es ist zu ändern, um Buildfehler zu beheben:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Vorgang ändert sich

Jede Operation in Azure Suche .NET SDK wie überladene Methoden für synchrone und asynchrone Aufrufer verfügbar gemacht. Signaturen und Verarbeitung von diese überladene Version 1.1 geändert.

Der Vorgang "Abrufen von Indexstatistiken" in älteren Versionen des SDK ausgesetzt z. B. diese Signaturen:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Die Methodensignaturen für den gleichen Vorgang in Version 1.1 wie folgt aussehen:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Ab Version 1.1 organisiert das .NET SDK Azure Methoden unterschiedlich:

 - Optionale Parameter werden als Parameter stattdessen Standard jetzt modelliert als zusätzliche überladene. Dies reduziert die Anzahl der überladene Methoden manchmal.
 - Die Erweiterungsmethoden ausblenden jetzt viel überflüssige Details zum HTTP-Aufrufer. Z. B. ältere Versionen des SDK ein Antwortobjekt mit einem HTTP-Statuscode, müssen häufig nicht überprüfen, da Methoden lösen zurückgegeben `CloudException` Status Code, der einen Fehler angibt. Die neue Erweiterungsmethoden zurückgeben nur Modellobjekte speichern die Mühe sie in Ihrem Code aufgelöst werden.
 - Der Kern Schnittstellen hingegen jetzt Methoden verfügbar machen, die Ihnen mehr Kontrolle auf HTTP-Ebene bei Bedarf. Können Sie jetzt benutzerdefinierte HTTP-Header enthalten Anfragen und neuen übergeben `AzureOperationResponse<T>` Rückgabetyp bietet direkten Zugriff auf die `HttpRequestMessage` und `HttpResponseMessage` für den Vorgang. `AzureOperationResponse`wird gemäß der `Microsoft.Rest.Azure` Namespace und ersetzt `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters ändern

Eine neue Klasse mit dem Namen `ScoringParameter` hat in das neueste SDK Parameter zum Bewerten von Profilen in einer Suchabfrage erleichtert. Zuvor die `ScoringProfiles` -Eigenschaft eines der `SearchParameters` Klasse wurde als typisierte `IList<string>`; Jetzt als angegeben `IList<ScoringParameter>`.

##### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Es ist zu ändern, um Buildfehler zu beheben: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Modelländerungen

Durch die Signatur wird im [Vorgang Methode ändert](#OperationMethodChanges), viele Klassen in der `Microsoft.Azure.Search.Models` Namespace umbenannt oder entfernt. Zum Beispiel:

 - `IndexDefinitionResponse`wurde ersetzt durch`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`wurde umbenannt.`DocumentSearchResult`
 - `IndexResult`wurde umbenannt.`IndexingResult`
 - `Documents.Count()`Jetzt gibt eine `long` mit dem Dokument statt einer`DocumentCountResponse`
 - `IndexGetStatisticsResponse`wurde umbenannt.`IndexGetStatisticsResult`
 - `IndexListResponse`wurde umbenannt.`IndexListResult`

Zusammenfassen, `OperationResponse`-abgeleitete Klassen, die nur umbrochen wurden ein Modellobjekt entfernt. Die übrigen Klassen wurden dem Suffix geändert `Response` , `Result`.

##### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Es ist zu ändern, um Buildfehler zu beheben:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Antwortklassen und IEnumerable

Eine weitere Änderung, die Code auswirken wird Antwortklassen, Sammlungen enthalten, mehr implementieren `IEnumerable<T>`. Stattdessen können Sie die Collection-Eigenschaft direkt zugreifen. Wenn beispielsweise der Code wie folgt aussieht:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Es ist zu ändern, um Buildfehler zu beheben:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Wichtiger Hinweis für ASP.NET-Webanwendungen

Wenn Sie eine Anwendung haben, die serialisiert `DocumentSearchResponse` direkt um Suchergebnisse an den Browser senden, Sie den Code ändern müssen oder das Ergebnis nicht ordnungsgemäß serialisiert. Wenn beispielsweise der Code wie folgt aussieht:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Immer ändern die `.Results` -Eigenschaft der Suche auf Suche Ergebnis Rendering zu beheben:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Sie müssen diesen Code selbst suchen in. **Der Compiler wird warnen** da `JsonResult.Data` vom Typ `object`.

#### <a name="cloudexception-changes"></a>CloudException ändern

Die `CloudException` Klasse wurde von der `Hyak.Common` Namespace der `Microsoft.Rest.Azure` Namespace. Auch die `Error` -Eigenschaft wurde umbenannt `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient und SearchIndexClient ändern

Der Typ der `Credentials` -Eigenschaft geändert hat `SearchCredentials` zu deren Basisklasse `ServiceClientCredentials`. Benötigen Sie Zugriff auf die `SearchCredentials` von einer `SearchIndexClient` oder `SearchServiceClient`, verwenden Sie das neue `SearchCredentials` -Eigenschaft.

In älteren Versionen des SDK `SearchServiceClient` und `SearchIndexClient` hatte Konstruktoren hat ein `HttpClient` Parameter. Diese wurden ausgetauscht mit Konstruktoren, die einen `HttpClientHandler` und `DelegatingHandler` Objekte. Dies erleichtert die benutzerdefinierte Handler für HTTP-Anfragen bei Bedarf vor der Verarbeitung zu installieren.

Schließlich hat Konstruktoren eine `Uri` und `SearchCredentials` haben. Angenommen, Sie über Code verfügen, die folgendermaßen aussieht:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Es ist zu ändern, um Buildfehler zu beheben:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Beachten Sie auch, die der Typ des Parameters Anmeldeinformationen geändert wurde `ServiceClientCredentials`. Dies ist wahrscheinlich auf Code seit `SearchCredentials` stammt von `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Kennung übergeben

In älteren Versionen des SDK konnte festlegen Kennung auf der `SearchServiceClient` oder `SearchIndexClient` und wird bei jeder Anforderung die REST-API enthalten sein. Dies empfiehlt sich zur Behandlung von Problemen mit Suchdienst benötigen Support wenden. Es ist jedoch sinnvoller, eine eindeutige Kennung für jeden Vorgang festlegen, anstatt dieselbe ID für alle Vorgänge verwendet. Aus diesem Grund die `SetClientRequestId` Methoden `SearchServiceClient` und `SearchIndexClient` wurden entfernt. Sie können stattdessen Kennung für jede operationsmethode übergeben, über die optionale `SearchRequestOptions` Parameter.

> [AZURE.NOTE] In einer zukünftigen Version des SDK werden wir einen neuen Mechanismus für Kennung Global auf dem Client Objekte hinzufügen mit der Vorgehensweise von anderen Azure SDKs.

#### <a name="example"></a>Beispiel

Wenn Sie über Code, die folgendermaßen aussieht verfügen:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Es ist zu ändern, um Buildfehler zu beheben:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Schnittstelle ändern

Die Operation Schnittstelle Gruppennamen haben alle an ihre entsprechenden Eigenschaftennamen:

 - Typ des `ISearchServiceClient.Indexes` wurde umbenannt in `IIndexOperations` , `IIndexesOperations`.
 - Typ des `ISearchServiceClient.Indexers` wurde umbenannt in `IIndexerOperations` , `IIndexersOperations`.
 - Typ des `ISearchServiceClient.DataSources` wurde umbenannt in `IDataSourceOperations` , `IDataSourcesOperations`.
 - Typ des `ISearchIndexClient.Documents` wurde umbenannt in `IDocumentOperations` , `IDocumentsOperations`.

Diese Änderung ist wahrscheinlich nicht auf Ihren Code auswirken, sofern dieser Schnittstellen für Testzwecke Mocks erstellt.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Fehlerbehebungen in Version 1.1

In älteren Versionen von Azure Suche .NET SDK zur Serialisierung von benutzerdefinierten Modellklassen ist ein Fehler. Der Fehler kann auftreten, wenn eine benutzerdefinierte Modellkonfiguration Klasse mit einer Eigenschaft ein NULL-Wert erstellt.

#### <a name="steps-to-reproduce"></a>Schritte zum Reproduzieren

Erstellen Sie eine benutzerdefinierte Modellkonfiguration Klasse mit einer Eigenschaft vom Typ NULL-Werte. Beispielsweise fügen Sie eine öffentliche `UnitCount` -Eigenschaft des Typs `int` anstelle von `int?`.

Wenn Sie ein Dokument mit dem Standardwert des Typs index (z. B. 0 für `int`), das Feld nicht null in Azure Suche. Wenn Sie das Dokument später suchen die `Search` Aufruf löst `JsonSerializationException` Klagen, die konvertiert werden können `null` , `int`.

Außerdem können Filter nicht erwartungsgemäß seit dem Index anstelle der beabsichtigten Wert Null.

#### <a name="fix-details"></a>Details Retuschieren

Wir haben dieses Problem im SDK-Version 1.1 behoben. Jetzt haben Sie eine Modellklasse wie folgt:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

und `IntValue` 0 der Wert jetzt ordnungsgemäß serialisiert 0 bei der Übertragung und 0 im Index gespeichert. Round-tripping funktioniert wie erwartet.

Gibt es ein potenzielles Problem dabei beachten: Wenn Sie ein Modell mit einer NULL-Eigenschaft verwenden, müssen Sie **gewährleisten** enthalten keine Dokumente im Index einen null-Wert für das entsprechende Feld. Das SDK weder die REST-API von Azure Suche hilft Ihnen dies zu erzwingen.

Dies ist nicht nur ein hypothetisches Problem: ein Szenario, in dem Sie einen vorhandenen Index vom Typ ein neues Feld hinzufügen `Edm.Int32`. Nach der Aktualisierung der Indexdefinition haben alle Dokumente einen Nullwert für das neue Feld (da alle nullable in Azure Suche sind). Verwenden Sie dann eine mit einem NULL- `int` -Eigenschaft für das Feld, erhalten Sie eine `JsonSerializationException` beim Dokumente folgendermaßen:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Aus diesem Grund empfohlen, Typen in Ihrem Modellklassen als bewährte Methode verwenden.

Weitere Informationen zu diesem Fehler und den finden Sie [dieses Problem auf GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).
