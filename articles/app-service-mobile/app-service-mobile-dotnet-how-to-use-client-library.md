<properties
    pageTitle="Arbeiten mit der Bibliothek App Service Mobile Apps verwalteten Client (Windows | Xamarin) | Microsoft Azure"
    description="Erfahren Sie, wie Azure App Service Mobile Apps mit Windows und Xamarin apps mit ein."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Verwendung den verwalteten Client für Azure Mobile Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht die allgemeine Szenarien mit verwalteten Client-Bibliothek für Azure App Service Apps für Windows Mobile und Xamarin apps. Wenn Sie Mobile Apps sind, sollten Sie ausfüllen [Azure Mobile Apps Schnellstart] [ 1] Tutorial. In diesem Handbuch konzentrieren wir uns auf clientseitige verwalteten SDK. Erfahren Sie mehr über die serverseitige SDKs für Mobile Apps finden für [.NET Server SDK] [ 2] oder [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Dokumentation

Die Referenzdokumentation für den Client SDK befindet sich hier: [Azure Mobile Apps .NET Client Reference][4].
Finden Sie auch mehrere Client-Beispiele in der [Azure-Beispiele GitHub Repository][5].

## <a name="supported-platforms"></a>Unterstützte Plattformen

Der unterstützt die folgenden Betriebssysteme:

* Xamarin Android Versionen API 19 bis 24 (KitKat durch Nougat)
* Xamarin iOS-Versionen für iOS, Version 8.0 und höher
* Universelle Windows-Plattform
* Windows Phone 8.1
* Windows Phone 8.0 außer Silverlight applications

"Server-Flow" Authentifizierung verwendet eine Webansicht dargestellten Benutzeroberfläche.  Wenn das Gerät nicht vorlegen WebView UI ist, sind andere Methoden der Authentifizierung erforderlich.  Dieses SDK deshalb nicht für Art oder eingeschränkten Geräten.

##<a name="setup"></a>Setup und Komponenten

Davon ausgegangen, dass bereits erstellt und veröffentlicht Mobile App Backend-Projekt mindestens eine Tabelle enthält.  Im Code in diesem Thema wird die Tabelle namens `TodoItem` und enthält folgende Spalten: `Id`, `Text`, und `Complete`. Diese Tabelle ist dieselbe Tabelle Abschluss [Azure Mobile Apps Schnellstart] erstellt.

Der entsprechende typisierte clientseitige Typ in C# ist die folgende Klasse:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

[JsonPropertyAttribute] [ 6] *PropertyName* Zuordnung zwischen dem Client und dem definiert.

Informationen zum Erstellen von Tabellen im Backend Mobile Apps finden Sie unter dem [Thema .NET Server SDK] [ 7] oder [Node.js Server SDK-Thema][8]. Wenn Sie Backend Mobile-Anwendung in Azure-Portal mit den Schnellstart erstellt, können Sie auch die Einstellung **einfache Tabellen** in [Azure-Portal].

###<a name="how-to-install-the-managed-client-sdk-package"></a>Gewusst wie: verwalteten Client-SDK-Paket installieren

Verwenden Sie eine der folgenden Methoden verwalteten Client SDK für Mobile Apps von [NuGet]installiert[9]:

+ **Visual Studio** Projekt Maustaste auf **NuGet-Pakete verwalten**, suchen Sie nach dem `Microsoft.Azure.Mobile.Client` Paket, und klicken Sie auf **Installieren**.

+ **Xamarin Studio** Maustaste auf das Projekt, klicken Sie auf **Hinzufügen** > **NuGet-Pakete hinzufügen**, suchen Sie nach der `Microsoft.Azure.Mobile.Client `-Paket und klicken Sie dann auf **Paket hinzufügen**.

Beachten Sie in der Datei hauptsächlich die folgende **using** -Anweisung hinzufügen:

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>Gewusst wie: Arbeiten mit Debugsymbolen in Visual Studio

Die Symbole für den Microsoft.Azure.Mobile-Namespace sind auf [SymbolSource][10].  Die [Anleitung des SymbolSource] [ 11] SymbolSource mit Visual Studio integriert.

##<a name="create-client"></a>Die Mobile Apps erstellen

Der folgende Code erstellt die [MobileServiceClient] [ 12] Objekt, Backend Mobile-Anwendung zugreifen.

    var client = new MobileServiceClient("MOBILE_APP_URL");

Ersetzen Sie im obigen Code `MOBILE_APP_URL` mit dem URL der Mobile-Anwendung Backend befindet, Blatt für die Mobile Anwendung Backend in [Azure-Portal]. Das MobileServiceClient-Objekt sollte ein Singleton.

## <a name="work-with-tables"></a>Arbeiten mit Tabellen

Der folgende Abschnitt enthält Informationen zum Suchen und Datensätze abrufen und Ändern von Daten in der Tabelle.  Folgende Themen werden behandelt:

* [Erstellen einer](#instantiating)
* [Abfragen von Daten](#querying)
* [Zurückgegebene Daten filtern](#filtering)
* [Daten sortieren](#sorting)
* [Daten in Seiten zurück](#paging)
* [Bestimmte Spalten auswählen](#selecting)
* [Nachschlagen eines Datensatzes nach Id](#lookingup)
* [Umgang mit nicht typisierten Abfragen](#untypedqueries)
* [Einfügen von Daten](#inserting)
* [Aktualisieren von Daten](#updating)
* [Löschen von Daten](#deleting)
* [Lösung von Konflikten und Parallelität](#optimisticconcurrency)
* [Binden an eine Windows-Benutzeroberfläche](#binding)
* [Ändern des Papierformats](#pagesize)

###<a name="instantiating"></a>Gewusst wie: Erstellen einer

Ruft der Code die zugreift oder diese ändert Daten in einer Tabelle Back-End-Funktionen für die `MobileServiceTable` Objekt. Erhalten Sie einen Verweis auf die Tabelle durch [GetTable] -Methode wie folgt aufrufen:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

Das zurückgegebene Objekt verwendet das typisierte Serialisierung. Nicht typisierte Serialisierungsmodell wird ebenfalls unterstützt. Das folgende Beispiel [erstellt einen Verweis auf eine nicht typisierte Tabelle]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

Nicht typisierte Abfragen müssen Sie die zugrunde liegenden OData-Abfragezeichenfolge angeben.

###<a name="querying"></a>Gewusst wie: Abfragen von Daten aus der Mobile-App

Dieser Abschnitt beschreibt, wie Abfragen Backend Mobile-Anwendung umfasst die folgende Funktionen:

- [Zurückgegebene Daten filtern](#filtering)
- [Daten sortieren](#sorting)
- [Daten in Seiten zurück](#paging)
- [Bestimmte Spalten auswählen](#selecting)
- [Daten Sie nach ID](#lookingup)

>[AZURE.NOTE]Server basierende Seitengröße wird erzwungen, um zu verhindern, dass alle Zeilen zurückgegeben werden.  Paging wird verhindert, dass Standard-Anfragen für Daten des Dienstes beeinträchtigen.  Um mehr als 50 Zeilen zurückzugeben, verwenden Sie die `Skip` und `Take` Methode unter [Rückgabedaten Seiten] beschrieben.

###<a name="filtering"></a>Gewusst wie: Filter zurückgegebenen Daten

Der folgende Code veranschaulicht das Filtern von Daten mit einem `Where` -Klausel in einer Abfrage. Gibt alle Elemente von `todoTable` , deren `Complete` -Eigenschaft `false`. Funktion [,] wendet eine Zeile Prädikat der Abfrage für die Tabelle zu filtern.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

Den URI der Anforderung per Backend mit Nachricht Überprüfung [Fiddler]oder Browser Entwicklertools sehen. Betrachten Sie die Anfrage-URI, beachten Sie die Abfragezeichenfolge geändert wird.

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

Diese Anforderung OData ist eine SQL-Abfrage vom Server SDK übersetzt:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

Die Funktion, die an der `Where` Methode kann eine beliebige Anzahl von Vorschriften haben.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

In diesem Beispiel wird eine SQL-Abfrage vom Server SDK übersetzt werden:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

Diese Abfrage kann auch in mehreren Klauseln aufgeteilt werden:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

Die beiden Methoden entsprechen und können synonym verwendet werden.  Die erste Option&mdash;Verketten mehrerer Prädikate in einer Abfrage&mdash;ist kompakt und empfohlen.

Die `Where` -Klausel unterstützt werden Vorgänge OData Teilmenge übersetzt. Vorgänge umfassen:

* Relationale Operatoren (==,! =, <, < =, >, > =),
* Arithmetische Operatoren (+, -, / *, %),
* Zahl mit doppelter Genauigkeit (Math.Floor, Math.Ceiling)
* Zeichenfolgenfunktionen (Länge Teilzeichenfolge ersetzen, IndexOf, "StartsWith", "EndsWith"),
* Datumseigenschaften (Jahr, Monat, Tag, Stunde, Minute, Sekunde)
* Zugriff auf Eigenschaften eines Objekts und
* Ausdrücke kombinieren dieser Vorgänge.

Wenn Server SDK unterstützt, können Sie [OData v3 Dokumentation]betrachten.

###<a name="sorting"></a>Gewusst wie: Sortieren Daten zurückgegeben

Der folgende Code veranschaulicht, wie Daten einschließlich einer [OrderBy] oder [OrderByDescending] -Funktion in der Abfrage zu sortieren. Gibt Elemente von `todoTable` von aufsteigend sortiert die `Text` Feld.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>Gewusst wie: Daten in Seiten zurück

Standardmäßig gibt das Back-End nur die ersten 50 Zeilen. Die Anzahl der zurückgegebenen Zeilen können durch Aufrufen der Methode [nehmen] . Mit `Take` und [Skip] -Methode zum Anfordern eines bestimmtes "Seite" des gesamten Datasets von der Abfrage zurückgegeben. Die folgende Abfrage ausgeführt, zurückgegeben die drei Elemente in der Tabelle.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Die folgende überarbeitete Abfrage überspringt die ersten drei Ergebnisse und die nächsten drei Ergebnisse zurückgegeben. Zweite Seite"" der Daten ist das Seitenformat auf drei Elemente ergibt.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

[IncludeTotalCount] Methode fordert der Gesamtzahl _aller_ Datensätze wäre ignoriert alle Paging-Limit-Klausel angegeben zurückgegeben worden:

    query = query.IncludeTotalCount();

In einer realen Anwendung können Abfragen ähnlich wie im vorherigen Beispiel mit einem Pagersteuerelement oder vergleichbare Benutzeroberfläche Sie zwischen den Seiten wechseln.

>[AZURE.NOTE]Zum Überschreiben der 50 Zeilengröße in eine Mobile Anwendung Back-End-müssen auch [EnableQueryAttribute] auf Öffentliche GET-Methode anwenden und das Pagingverhalten angeben. Bei der Methode wird folgende maximal 1000 Zeilen zurückgegeben:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>Gewusst wie: auswählen bestimmte Spalten

Sie können angeben, welche Gruppe von Eigenschaften durch Hinzufügen einer [Select] -Klausel der Abfrage in den Ergebnissen enthalten. Im folgende Code veranschaulicht z. B. nur ein Feld auswählen und auswählen und Formatieren mehrerer Felder:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Alle bisher beschriebenen Funktionen sind additiv, damit wir diese Verkettung beibehalten können. Jedes verkettete Aufruf betrifft mehr die Abfrage. Ein weiteres Beispiel:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>Gewusst wie: Daten nach ID

[LookupAsync] -Funktion kann verwendet werden, um Objekte aus der Datenbank mit einer bestimmten ID suchen

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>Gewusst wie: Ausführen von nicht typisierten Abfragen

Beim Ausführen einer Abfrage mit einem Objekt nicht typisierte Tabelle müssen Sie die Abfragezeichenfolge OData explizit angeben, durch Aufrufen von [ReadAsync], wie im folgenden Beispiel:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

Sie erhalten JSON-Werte, die wie eine Eigenschaftensammlung verwendet werden können. Weitere Informationen zu JToken und Newtonsoft Json.NET finden Sie in der [Json.NET] -Website.

### <a name="inserting"></a>Gewusst wie: Einfügen von Daten in eine Mobile Anwendung Backend

Alle Clients müssen Mitglied mit dem Namen **Id**, standardmäßig eine Zeichenfolge enthalten. Diese **Id** ist erforderlich, um CRUD-Operationen und offline synchronisieren. Der folgende Code veranschaulicht die [InsertAsync] -Methode verwenden, um neue Zeilen in eine Tabelle einfügen. Der Parameter enthält die Daten als ein eingefügt werden.

    await todoTable.InsertAsync(todoItem);

Wenn eindeutiger benutzerdefinierter ID-Wert nicht in enthalten ist die `todoItem` beim Einfügen, eine GUID wird vom Server generiert werden.
Abrufen die generierte Id anhand des Objekts nach Beendigung des Aufrufs.

Zum Einfügen von nicht typisierter Daten können Sie Json.NET nutzen:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Hier ist ein Beispiel einer e-Mail-Adresse als eindeutige Zeichenfolgen-Id:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Arbeiten mit ID-Werten

Mobile Apps unterstützt eindeutige benutzerdefinierte Werte für die **ID-** Spalte der Tabelle. Ein String-Wert ist die Anwendung benutzerdefinierte Werte wie e-Mail-Adressen oder Benutzernamen für die ID.  Zeichenfolgen-IDs bieten folgende Vorteile:

* IDs werden generiert, ohne dass ein Roundtrip zur Datenbank.
* Datensätze sind leichter von anderen Tabellen oder Datenbanken zusammenführen.
* IDs-Werte können mit der Anwendungslogik besser integrieren.

Wenn ein Zeichenfolgenwert ID eingefügten Datensatz nicht festgelegt ist, generiert Backend Mobile-Anwendung einen eindeutigen Wert für die Kennung. [Guid.NewGuid] -Methode können Sie um eigene ID-Werte auf dem Client oder im Backend zu generieren.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>Gewusst wie: Ändern von Daten in eine Mobile Anwendung Backend

Der folgende Code veranschaulicht die [UpdateAsync] -Methode verwenden, um einen vorhandenen Datensatz mit der gleichen ID mit neuen Informationen aktualisieren. Der Parameter enthält die Daten als ein aktualisiert werden.

    await todoTable.UpdateAsync(todoItem);

Nicht typisierte Daten aktualisieren, können Sie [Json.NET] folgendermaßen nutzen:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

Ein `id` Feld muss angegeben werden, wenn ein Update. Backend verwendet die `id` Feld der zu aktualisierenden Zeile bestimmen. Die `id` Feld erhalten Sie vom Ergebnis der `InsertAsync` aufrufen. Ein `ArgumentException` wird ausgelöst, wenn Sie versuchen, ein Element ohne Aktualisieren der `id` Wert.

###<a name="deleting"></a>Gewusst wie: Löschen von Daten in eine Mobile Anwendung Backend

Der folgende Code veranschaulicht, wie mit der [DeleteAsync] -Methode löschen Sie eine vorhandene Instanz. Die Instanz wird anhand der `id` Feld Satz der `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Um nicht typisierten Daten löschen, können Sie Json.NET folgendermaßen nutzen:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Wenn Sie eine Löschanfrage vornehmen, eine ID anzugeben. Andere Eigenschaften werden nicht an den Dienst übergeben oder an den Dienst ignoriert. Das Ergebnis einer `DeleteAsync` Aufruf ist in der Regel `null`. ID übergeben erhalten Sie vom Ergebnis der `InsertAsync` aufrufen. Ein `MobileServiceInvalidOperationException` wird ausgelöst, wenn Sie versuchen, ein Element zu löschen, ohne die `id` Feld.

###<a name="optimisticconcurrency"></a>Gewusst wie: vollständige Parallelität für Konfliktbehebung verwenden

Zwei oder mehr Clients möglicherweise ändert gleichzeitig auf dasselbe Element schreiben. Ohne Konflikte überschreibt der letzte Schreibvorgang alle vorherigen Updates. **Steuerung durch vollständige Parallelität** geht davon aus, dass jede Transaktion kann und daher keine Ressourcensperre.  Bevor eine Transaktion festgeschrieben, überprüft die Steuerung durch vollständige Parallelität, dass keine andere Transaktion die Daten geändert hat. Wenn die Daten geändert wurden, ist verpflichtet Rollback der Transaktion ausgeführt.

Mobile Apps unterstützt Steuerung durch vollständige Parallelität Nachverfolgen von Änderungen an jedes Element mithilfe der `version` System Eigenschaft-Spalte für jede Tabelle im Backend Mobile-Anwendung definiert wird. Mobile Apps ein Datensatz aktualisiert wird, legt die `version` -Eigenschaft für diesen Datensatz einen neuen Wert ein. Bei jeder aktualisierungsanforderung die `version` -Eigenschaft des Datensatzes der Anforderung auf dieselbe Eigenschaft für den Datensatz auf dem Server verglichen. Wenn die Version übergeben die Anforderung entspricht nicht das Back-End und die Clientbibliothek löst eine `MobileServicePreconditionFailedException<T>` Ausnahme. Die Ausnahme enthaltene Typ ist die Backend Server-Version des Datensatzes enthält. Die Anwendung können Sie diese Informationen entscheiden, ob Sie das Update Ersuchen mit den richtigen `version` Wert von Back-End-Commits.

Definieren eine Spalte in der Tabellenklasse für die `version` Systemeigenschaft Parallelität zu aktivieren. Zum Beispiel:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Applikationen nicht typisierten Tabellen Parallelität aktivieren, indem Sie die `Version` flag für die `SystemProperties` die Tabelle wie folgt.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Ermöglicht vollständigen Parallelität, müssen Sie auch fangen die `MobileServicePreconditionFailedException<T>` Ausnahme im Code beim [UpdateAsync]aufrufen.  Lösen Sie den Konflikt durch Anwenden der richtigen `version` aktualisierten Datensatz und Aufruf [UpdateAsync] aufgelösten Datensatz. Im folgenden Codebeispiel wird die Behebung ein Schreibkonflikt einmal erkannt:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

Weitere Informationen finden Sie auf [Offline Daten synchronisieren in Azure Mobile Apps] .

###<a name="binding"></a>Gewusst wie: Binden Mobile Apps Daten einer Windows-Benutzeroberfläche

Dieser Abschnitt veranschaulicht das zurückgegebene Datenobjekte mit UI-Elemente in einer Windows-Anwendung angezeigt.  Im folgenden Codebeispiel wird an die Quelle mit einer Abfrage für unvollständige Elemente gebunden. [MobileServiceCollection] erstellt eine Mobile Apps-aware Bindung-Auflistung.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Einige Steuerelemente in die verwaltete Laufzeit unterstützen eine Schnittstelle namens [ISupportIncrementalLoading]. Diese Schnittstelle ermöglicht es Steuerelementen, zusätzliche Daten anfordern, wenn ein Bildlauf. Es gibt integrierter Unterstützung für diese Schnittstelle für universelle Windows-apps über [MobileServiceIncrementalLoadingCollection], die Aufrufe von Steuerelementen automatisch behandelt. Mit `MobileServiceIncrementalLoadingCollection` in Windows-apps wie folgt:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Die neue Auflistung Apps für Windows Phone 8 und "Silverlight", verwenden die `ToCollection` Erweiterungsmethoden für `IMobileServiceTableQuery<T>` und `IMobileServiceTable<T>`. Um Daten zu laden, rufen Sie `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

Verwendung die Auflistung erstellt durch Aufrufen von `ToCollectionAsync` oder `ToCollection`, erhalten Sie eine Auflistung, die UI-Steuerelemente gebunden werden kann.  Diese Auflistung ist Paging unterstützt.  Da die Auflistung Daten aus dem Netzwerk geladen wird, schlägt gelegentlich laden. Um solche Fehler behandeln, überschreiben die `OnException` Methode `MobileServiceIncrementalLoadingCollection` Aufrufe durch Ausnahmen behandelt `LoadMoreItemsAsync`.

Beachten Sie, wenn die Tabelle viele Felder jedoch nur einige davon im Steuerelement anzeigen möchten. Die Anleitung können im vorherigen Abschnitt "[bestimmte Spalten wählen](#selecting)" auf bestimmte Spalten in der Benutzeroberfläche angezeigt.

###<a name="pagesize"></a>Ändern des Seitenformats

Standardmäßig gibt Azure Mobile Apps maximal 50 Elemente pro Anforderung zurück.  Erhöhen die maximale Seitengröße auf Client und Server, um die Seitengröße zu ändern.  Um die Seite zu vergrößern, geben Sie `PullOptions` Verwendung `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Angenommen, Sie haben die `PageSize` gleich oder größer als 100 Server eine Anforderung bis zu 100 Elemente zurückgegeben.

##<a name="#offlinesync"></a>Arbeiten mit Offline-Tabellen

Offline Tabellen verwenden einen lokalen SQLite zu speichern Daten offline.  Alle Tabelle die Vorgänge auf den lokalen SQLite statt remote-Server-Speicher gespeichert.  Erstellen eine offline-Tabelle zuerst bereiten Sie das Projekt vor:

1. Klicken Sie in Visual Studio die Projektmappe > **NuGet-Pakete verwalten, für die Lösung**dann suchen und installieren **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-Paket für alle Projekte in der Projektmappe.

2. (Optional) Unterstützung von Windows-Geräten installieren eines SQLite Runtime Pakete:

    * **Windows 8.1 Runtime:** [SQLite für Windows 8.1]installieren[3].
    * **Windows Phone 8.1:** [SQLite für Windows Phone 8.1]installieren[4].
    * **Universelle Windows-Plattform** [SQLite universellen universelle Windows]installieren[5].

3. (Optional). Windows-Geräten, klicken Sie auf **Referenzen** > **Verweis hinzufügen**, erweitern Sie den Ordner **Windows** > **Extensions**und aktivieren die entsprechenden **SQLite für Windows** SDK mit **Visual C++ 2013 Runtime für Windows** SDK.
    SQLite SDK Namen abweichen mit jeder Windows-Plattform.

Bevor eine Referenz erstellt werden kann, muss der lokale Speicher vorbereitet:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Shop-Initialisierung erfolgt normalerweise, sobald der Client.  Der **OfflineDbPath** sollte ein Dateiname auf allen Plattformen, die Sie unterstützen.  Wenn der Pfad einen vollqualifizierten Pfad (d. h. es beginnt mit einem Schrägstrich), wird dieser Pfad verwendet.  Wenn der Pfad nicht voll qualifiziert ist, wird die Datei in einem plattformspezifischen Verzeichnis platziert.

* Für IOS- und Android-Geräte ist der Standardpfad der Ordner "Persönlichen Dateien".
* Der Standardpfad ist für Windows-Geräte Ordner "Anwendungsdaten" anwendungsspezifische.

Tabellenverweis erzielt werden mit der `GetSyncTable<>` Methode:

    var table = client.GetSyncTable<TodoItem>();

Sie müssen nicht authentifizieren, um eine offline zu verwenden.  Sie müssen bei der Kommunikation mit dem Back-End-Dienst zu authentifizieren.

###<a name="syncoffline"></a>Synchronisieren einer Offline-Tabelle

Offline-Tabellen werden nicht mit dem Back-End standardmäßig synchronisiert.  Synchronisierung ist in zwei Bereiche unterteilt.  Downloaden neuer Elemente können Sie Änderungen einzeln drücken.  Hier ist eine normale Sync-Methode:

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
                "allTodoItems",
                this.todoTable.CreateQuery());
        }
        catch (MobileServicePushFailedException exc)
        {
            if (exc.PushResult != null)
            {
                syncErrors = exc.PushResult.Errors;
            }
        }

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
        if (syncErrors != null)
        {
            foreach (var error in syncErrors)
            {
                if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                {
                    //Update failed, reverting to server's copy.
                    await error.CancelAndUpdateItemAsync(error.Result);
                }
                else
                {
                    // Discard local change.
                    await error.CancelAndDiscardItemAsync();
                }

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Wenn das erste Argument für `PullAsync` null ist, dann inkrementelle Synchronisierung nicht verwendet wird.  Jeder Synchronisierungsvorgang Ruft alle Datensätze ab.

Das SDK führt einen impliziten `PushAsync()` vor Datensätze.

Konfliktbehandlung geschieht auf einem `PullAsync()` Methode.  Konflikte können genauso wie online-Tabellen behandelt werden.  Der Konflikt entsteht bei `PullAsync()` statt während INSERT-, Update- oder Delete aufgerufen. Wenn mehrere Konflikte auftreten, werden sie in einer einzigen MobileServicePushFailedException gebündelt.  Behandeln Sie jeder Fehler einzeln.

##<a name="#customapi"></a>Arbeiten Sie mit benutzerdefinierten API

Eine benutzerdefinierte API können Sie benutzerdefinierte Endpunkte, die Server Funktionen definieren, die nicht zuordnen einer einfügen, aktualisieren, löschen oder Lesevorgang. Mithilfe einer benutzerdefinierten API haben mehr Kontrolle über messaging, Sie auch lesen und HTTP-Nachrichtenheader und definiert ein Nachrichtenformat Körper als JSON.

Sie rufen eine benutzerdefinierte API durch einen Aufruf der [InvokeApiAsync] -Methoden auf dem Client. Beispielsweise sendet die folgende Codezeile eine POST-Anforderung an die **CompleteAll** API auf dem Back-End:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Dieses Formular ist eine typisierte Methode und erfordert, dass der Rückgabetyp **MarkAllResult** definiert ist. Typisierte und nicht typisierte Methoden werden unterstützt.

##<a name="authentication"></a>Benutzer authentifizieren

Authentifizieren und Autorisieren von app Benutzer verschiedene externe Identitätsanbieter Mobile Apps unterstützt: Facebook, Google, Microsoft Account, Twitter und Azure Active Directory. Sie können Berechtigungen für Tabellen für bestimmte Operationen nur authentifizierten Benutzern Zugriff festlegen. Die Identität des authentifizierten Benutzern können Sie Autorisierungsregeln in Serverskripts implementieren. Weitere Informationen finden Sie im Lernprogramm [Authentifizierung für Ihre Anwendung hinzufügen].

Zwei Authentifizierung Abläufe werden unterstützt: Fluss _verwalteten Client_ und _Server verwaltet_ . Fluss Server verwaltet bietet die einfachste Authentifizierung wie der Provider Weboberfläche Authentifizierung abhängig. Der Client verwaltet ermöglicht tiefere Integration gerätespezifische Funktionen wie anbieterspezifische gerätespezifische SDKs beruht.

>[AZURE.NOTE] Wir empfehlen einen Fluss Client verwaltet in Ihrer Produktion apps.

Um Authentifizierung einzurichten, müssen Sie Ihre Anwendung mit mindestens Identitätsprovider registrieren.  Der Identitätsanbieter eine Client-ID und eines geheimen für Ihre Anwendung generiert.  Diese Werte werden im Backend festgelegt Azure App-Authentifizierung/Autorisierung aktivieren.  Weitere Informationen folgen Sie detaillierten Informationen im Lernprogramm [Authentifizierung für Ihre Anwendung hinzufügen].

In diesem Abschnitt werden die folgenden Themen behandelt:

+ [Verwaltete Client-Authentifizierung](#clientflow)
+ [Server verwaltet Authentifizierung](#serverflow)
+ [Das Authentifizierungstoken Zwischenspeichern](#caching)

###<a name="clientflow"></a>Verwaltete Client-Authentifizierung

Ihre app kann unabhängig wenden Sie sich an die Identität und geben das zurückgegebene Token bei der Anmeldung mit Ihrem Back-End. Diese Client-Fluss können Sie eine einzelne Anmeldung für Benutzer oder zusätzliche Datenabruf vom Identitätsanbieter. Fluss Clientauthentifizierung ist mit einem Server als Identitätsanbieter SDK sich mehr systemeigene UX bietet und weitere Anpassung ermöglicht.

Beispiele für die folgenden Client-Authentifizierung Muster:

+ [Active Directory-Authentifizierungsbibliothek](#adal)
+ [Facebook und Google](#client-facebook)
+ [Live SDK](#client-livesdk)

#### <a name="adal"></a>Authentifizieren von Benutzern mit der Active Directory-Authentifizierungsbibliothek

Active Directory Authentifizierung Library (ADAL) können initiieren Benutzerauthentifizierung vom Client Azure Active Directory-Authentifizierung verwenden.

1. Konfigurieren der mobilen Anwendung Backend AAD anmelden nach dem Tutorial [App Service für Active Directory-Konto konfigurieren] . Stellen Sie sicher, das optionale Schritt eine native Client-Anwendung registrieren.
2. In Visual Studio oder Xamarin Studio, öffnen Sie das Projekt, und fügen Sie einen Verweis auf die `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-Paket. Beim Durchsuchen von einschließen Sie Vorabversionen
3. Fügen Sie folgenden Code der Anwendung entsprechend der verwendeten Plattform. Stellen Sie, die folgenden Ersatzteile:

    * Ersetzen Sie **Einfügen Behörde hier** durch den Namen des Mandanten, in dem die Anwendung bereitgestellt. Das Format muss https://login.windows.net/contoso.onmicrosoft.com sein. Dieser Wert kann aus der Registerkarte Domänen Azure Active Directory in [Azure-Verwaltungsportal]kopiert.
    * Die Client-ID für die mobile Anwendung Backend-ersetzen Sie **Einfügen-Ressource-ID-hier** . Die Client-ID erhalten Sie auf der Registerkarte **Erweitert** unter **Azure Active Directory Einstellungen** im Portal.
    * Ersetzen Sie **INSERT-CLIENT-ID-hier** durch die Client-ID von der systemeigenen Clientanwendung kopiert.
    * Ersetzen Sie **INSERT-REDIRECT-URI-hier** mit Ihrer Site _/.auth/login/done_ Endpunkt mit HTTPS-Schema. Dieser Wert sollte ähnlich wie _https://contoso.azurewebsites.net/.auth/login/done_.

    Der Code für jede Plattform folgt:

    **Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>Einmaliges Anmelden mit einem Token von Facebook und Google

Client-Fluss können wie Facebook oder Google in diesem Codeausschnitt gezeigt.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>Einzelne anmelden Live SDK mit Microsoft Account

Um Benutzer zu authentifizieren, müssen Sie Ihre Anwendung der Firma Microsoft Developer Center registrieren. Konfigurieren der Registrierungsdaten auf Mobile App Backend. Erstellen einer Registrierung Microsoft Mobile App Backend herstellen und die Schritte [Registrieren Ihrer Anwendung um ein Microsoft-Konto anmelden]. Haben Sie Windows Store und Windows Phone 8-Silverlight-Versionen Ihrer Anwendung registrieren Sie Windows Store-Version.

Der folgende Code verwendet Live SDK und verwendet das zurückgegebene Token Backend Mobile-Anwendung anmelden.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

Weitere Informationen finden Sie auf der [Windows Live SDK] -Dokumentation.

###<a name="serverflow"></a>Server verwaltet Authentifizierung

Wenn der Identitätsanbieter registriert haben, rufen Sie die Methode [LoginAsync] [MobileServiceClient] mit dem [MobileServiceAuthenticationProvider] Wert des Anbieters. Beispielsweise startet der folgende Code ein Server Fluss Anmelden mit Facebook.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Wenn Identitätsanbieter als Facebook ändern Sie den Wert des [MobileServiceAuthenticationProvider] mit dem Wert für den Anbieter.

In einem Fluss Server verwaltet Azure App Service das OAuth Authentifizierung durch Anzeigen der Webseite des ausgewählten Anbieters.  Nachdem die Identität Provider gibt Azure App Service generiert ein Authentifizierungstoken App Service. Die [LoginAsync] -Methode gibt eine [MobileServiceUser], die sowohl die [Benutzer-ID] des authentifizierten Benutzers und [MobileServiceAuthenticationToken]als JSON Web Token (JWT). Dieses Token kann zwischengespeichert und wiederverwendet, bis es abläuft. Weitere Informationen finden Sie unter [das Authentifizierungstoken Zwischenspeichern](#caching).

###<a name="caching"></a>Das Authentifizierungstoken Zwischenspeichern

In einigen Fällen kann der Aufruf der Login-Methode nach dem ersten erfolgreichen Authentifizierung speichern das Authentifizierungstoken vom Anbieter vermieden werden.  Windows Store und UWP apps können [PasswordVault] aktuelle Authentifizierungstoken wie folgt nach einer erfolgreichen Anmeldung Zwischenspeichern:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

UserId-Wert wird als Benutzername der Anmeldeinformationen gespeichert und das Token als das Kennwort gespeichert wird. Nachfolgende Neugründungen können Sie **PasswordVault** für Anmeldeinformationen überprüfen. Im folgende Beispiel verwendet die zwischengespeicherten Anmeldeinformationen finden und andernfalls versucht erneut mit dem Back-End-Authentifizierung:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

Wenn Sie einen Benutzer abmelden, müssen Sie die gespeicherte Anmeldeinformationen wie folgt entfernen:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

Xamarin apps verwenden [Xamarin.Auth] APIs zum sicheren Speichern von Anmeldeinformationen in einem **Konto** -Objekt. Ein Beispiel zur Verwendung dieser APIs finden Sie in der Datei [AuthStore.cs] [ContosoMoments Foto Beispiel].

Wenn Sie verwaltete Client-Authentifizierung verwenden, können Sie das Zugriffstoken erhalten von Ihrem Dienstanbieter wie Facebook oder Twitter Zwischenspeichern. Dieses Token kann wie folgt Anfordern neuer Authentifizierungstoken aus dem Back-End bereitgestellt werden:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>Push-Benachrichtigung

Die folgenden Themen Pushbenachrichtigungen:

* [Registrieren Sie sich für Pushbenachrichtigungen](#register-for-push)
* [Windows Store-Pakets SID abrufen](#package-sid)
* [Plattformübergreifende Vorlagen registrieren](#register-xplat)

###<a name="register-for-push"></a>Gewusst wie: Registrieren für Pushbenachrichtigungen

Mobile Apps-Client können Sie für Pushbenachrichtigungen mit Azure Benachrichtigung registrieren. Bei der Registrierung erhalten Sie ein Handle von plattformspezifischen Push Notification Service (PNS) erhalten. Dann geben Sie diesen Wert zusammen mit Tags beim Erstellen der Registrierung. Der folgende Code registriert Ihre Windows-Anwendung für Pushbenachrichtigungen mit Windows Notification Service (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Wenn, WNS drücken müssen Sie [eine Windows Store-Pakets SID erhalten](#package-sid).  Finden Sie weitere Informationen zum Windows-apps, wie Vorlage Registrierung registrieren [Pushbenachrichtigungen für Ihre Anwendung hinzufügen].

Tags vom Client angefordert wird nicht unterstützt.  Tag-Anfragen werden automatisch aus der Registrierung gelöscht.
Möchten Sie Ihr Gerät mit Tags zu registrieren, erstellen Sie eine benutzerdefinierte API, die Benachrichtigung Hubs API verwendet, um die Registrierung in Ihrem Namen durchzuführen.  [Benutzerdefinierte API Aufrufen](#customapi) , statt die `RegisterNativeAsync()` Methode.

###<a name="package-sid"></a>Gewusst wie: Abrufen einer Windows Store-Pakets SID

Ein Pakets SID ist erforderlich für Pushbenachrichtigungen in Windows Store-apps aktivieren.  Um ein Paket SID zu erhalten, registrieren Sie die Anwendung Windows Store.

Um diesen Wert zu erhalten:

1. Im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste Windows Store-app-Projekt, klicken Sie auf **Speichern** > **App mit dem Speicher zuordnen**.
2. Im Assistenten **Weiter**, Ihr Microsoft-Konto melden Sie an, geben Sie einen Namen für Ihre Anwendung in **einer neuen Anwendungsname**, klicken auf **Reservieren**.
3. Nach dem Erstellen die app-Registrierung erfolgreich wählen Sie app, klicken Sie auf **Weiter**und dann auf **zuordnen**.
4. Melden Sie sich mit Ihrem Microsoft Account [Windows-Entwicklungscenter] . Klicken Sie unter **Meine apps**app-Registrierung, die Sie erstellt haben.
5. Klicken Sie auf **Anwendung** > **App-Identität**und einen Bildlauf dann zu Ihrem **Paket SID**gefunden.

Viele Verwendungen des Pakets SID behandeln als URI, in diesem Fall müssen Sie verwenden _ms-app: / /_ als Schema. Notieren Sie die Version des Pakets SID verketten diesen Wert als Präfix gebildet.

Xamarin-apps müssen zusätzlichen Code zum Registrieren eine app für iOS oder Android-Plattformen ausgeführt werden. Weitere Informationen finden Sie im Thema für Ihre Plattform:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>Gewusst wie: Registrieren Push Vorlagen plattformübergreifende Benachrichtigungen senden

Um Vorlagen zu registrieren, verwenden Sie die `RegisterAsync()` mit Vorlagen wie folgt:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

Vorlagen sollten `JObject` Typen und mehrere Vorlagen im JSON-Format enthalten:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

Die **RegisterAsync()** -Methode akzeptiert auch Sekundärkacheln:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Alle Tags werden Sie während der Registrierung aus Sicherheitsgründen entfernt. Tags für Anlagen oder Vorlagen in Installationen finden Sie unter [Arbeit mit der Back-End-Server SDK für Azure Mobile Apps].

Zum Senden von Benachrichtigungen mit registrierten Vorlagen finden Sie unter [Notification Hubs APIs].

##<a name="misc"></a>Sonstiges

###<a name="errors"></a>Gewusst wie: Behandeln von Fehlern

Bei einem im Backend Fehler Client SDK löst ein `MobileServiceInvalidOperationException`.  Im folgenden Beispiel wird veranschaulicht, wie eine Ausnahme behandelt, die vom Backend zurückgegeben wird:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Ein weiteres Beispiel für den Umgang mit Fehlern finden im [Beispiel für Mobile Apps Dateien]. [LoggingHandler] -Beispiel stellt einen Protokollierung Delegathandler (Fortsetzung) Anfragen an den Back-End-protokollieren.

###<a name="headers"></a>Gewusst wie: Anpassen Anfrage-Header

Unterstützung für bestimmte Anwendung Szenario müssen Sie Kommunikation mit Back-End Mobile-Anwendung anpassen. Angenommen, möchten Sie alle ausgehenden Anforderung einen benutzerdefinierten Header hinzufügen oder sogar Antworten des Codes ändern. Sie können eine benutzerdefinierte [DelegatingHandler], wie im folgenden Beispiel:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Authentifizierung für Ihre Anwendung hinzufügen]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Offline-Daten synchronisieren in Azure mobiler Apps]: app-service-mobile-offline-data-sync.md
[Push-Benachrichtigung für Ihre Anwendung hinzufügen]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Registrieren Sie Ihrer Anwendung um ein Microsoft-Konto anmelden]: app-service-mobile-how-to-configure-microsoft-authentication.md
[App Service für Active Directory-Konto konfigurieren]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[erstellt einen Verweis auf eine nicht typisierte Tabelle]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Übernehmen]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Wählen Sie]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Überspringen]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Benutzer-ID]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Wo]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure-portal]: https://portal.azure.com/
[Azure-Verwaltungsportal]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows-Entwicklungscenter]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Benachrichtigung Hubs APIs]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Beispiel für Mobile Apps-Dateien]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3-Dokumentation]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[ContosoMoments Foto-sharing-Beispiel]: https://github.com/azure-appservice-samples/ContosoMoments