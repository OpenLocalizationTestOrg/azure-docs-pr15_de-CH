##<a name="create-client"></a>Erstellen Sie eine Clientverbindung

Erstellen eine Clientverbindung durch Erstellen einer `WindowsAzure.MobileServiceClient` Objekt.  Ersetzen Sie `appUrl` mit dem URL der mobilen Anwendung.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Arbeiten mit Tabellen

Zugriff oder Update-Daten auf Back-End-Tabelle erstellen. Ersetzen Sie `tableName` mit dem Namen der Tabelle

```
var table = client.getTable(tableName);
```

Haben Sie eine Referenz, kann mit der Tabelle:

* [Eine Tabelle](#querying)
  * [Filtern von Daten](#table-filter)
  * [Paging von Daten](#table-paging)
  * [Sortieren von Daten](#sorting-data)
* [Einfügen von Daten](#inserting)
* [Ändern von Daten](#modifying)
* [Löschen von Daten](#deleting)

###<a name="querying"></a>Gewusst wie: Abfragen einer

Haben Sie eine Referenz, können Sie Daten auf dem Server abgefragt.  Abfragen werden in einer Sprache "LINQ-artige" gestellt.
Um alle Daten aus der Tabelle zurückzugeben, verwenden Sie Folgendes:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Die Erfolg mit der Funktion.   Verwenden Sie nicht `for (var i in results)` den Erfolg wie die Informationen durchlaufen wird, die in den Ergebnissen enthalten ist beim anderen Abfragefunktionen (wie `.includeTotalCount()`) verwendet werden.

Weitere Informationen über die Abfragesyntax finden Sie in [Abfrage Objektdokumentation].

####<a name="table-filter"></a>Filtern von Daten auf dem server

Sie können ein `where` -Klausel für die Referenz:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Sie können auch eine Funktion, die das Objekt filtert.  In diesem Fall die `this` Variable erhält das aktuelle Objekt, das gefiltert wird.  Folgendes ist funktional dem vorherigen Beispiel:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Paging von Daten

Verwenden Sie die Methoden take() und skip().  Wenn beispielsweise 100 Zeilen Datensätze die Tabelle aufgeteilt werden soll:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Die `.includeTotalCount()` Methode das Results-Objekt eine TotalCount-Feld hinzu.  Die TotalCount mit Datensätzen gefüllt wird, die zurückgegeben wird, wenn kein Paging verwendet wird.

Die Variable Seiten und Benutzeroberfläche Schaltflächen können dann eine Seite Liste. Verwenden Sie loadPage() die neuen Datensätze pro Seite zu laden.  Implementieren Sie eine Art des Zwischenspeicherns Geschwindigkeit Zugriff auf Datensätze, die bereits geladen wurde.


####<a name="sorting-data"></a>Gewusst wie: Zurückgeben von Daten

Verwenden Sie .orderBy() oder .orderByDescending() Abfrage-Methoden:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Weitere Informationen über das Abfrageobjekt finden Sie [Abfrage Objekt Documentation].

###<a name="inserting"></a>Gewusst wie: Einfügen von Daten

Erstellen Sie ein JavaScript-Objekt mit dem entsprechenden Datums- und rufen Sie table.insert() asynchron auf:

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Erfolgreich anschließen wird das eingefügte Element zusätzliche Felder zurückgegeben, die für die Synchronisierungsoperationen erforderlich sind.  Sie sollten Ihren eigenen Cache diese Informationen für spätere Updates aktualisieren.

Beachten Sie, dass Azure Mobile Apps Node.js Server SDK dynamische Schema für Entwicklungszwecke unterstützt.
Bei dynamischen Schema wird das Schema der Tabelle ermöglicht das Hinzufügen von Spalten zu der Tabelle durch Angabe in Insert oder update-Operation dynamisch aktualisiert.  Wir empfehlen Sie dynamische Schema deaktivieren, vor der Anwendung für die Produktion.

###<a name="modifying"></a>Gewusst wie: Ändern von Daten

.Insert()-Methode ähnlich, Sie erstellen ein Aktualisierungsobjekt und rufen Sie .update().  Update-Objekt muss die ID des Datensatzes aktualisiert werden: erhält beim Lesen des Datensatzes oder .insert() aufrufen.

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Gewusst wie: Löschen

Rufen Sie die Methode .del(), um einen Datensatz zu löschen.  Übergeben Sie die ID ein Objektverweis:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
