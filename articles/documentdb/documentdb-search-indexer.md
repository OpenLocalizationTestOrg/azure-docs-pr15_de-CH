<properties
    pageTitle="Azure Search Indexer mit DocumentDB mit | Microsoft Azure"
    description="Dieser Artikel veranschaulicht das Azure Search Indexer mit DocumentDB als Datenquelle verwenden."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Herstellen einer Verbindung mit Azure Search Indexer mit DocumentDB

Wenn Sie hervorragende Suchfunktionen über Ihre DocumentDB Daten implementieren suchen, verwenden Sie Azure Search Indexer für DocumentDB! In diesem Artikel zeigen wir Ihnen Azure DocumentDB in Azure Suche ohne Schreiben von Code zum Indizieren Infrastruktur integrieren.

Um dies einzurichten, müssen [Setup ein Konto Azure Search](../search/search-create-service-portal.md) (Sie müssen Standardsuche aktualisieren), und rufen Sie [Azure Suche REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) DocumentDB **Datenquelle** und einen **Indexer** für diese Datenquelle.

Im Auftrag senden Anfragen REST-APIs interagieren können Sie [Postbote](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)oder beliebiges Tool Ihrer Wahl.


##<a id="Concepts"></a>Azure Search Indexer-Konzepte

Azure Suche unterstützt die Erstellung und Verwaltung von Daten Quellen (einschließlich DocumentDB) und Indexer in diesen Datenquellen arbeiten.

Eine **Datenquelle** gibt an, welche Daten für die Indizierung, Anmeldeinformationen für den Zugriff auf Daten und Richtlinien Azure Suche ändert die Daten (z. B. geändert oder Dokumente in der Auflistung gelöscht) effizient zu aktivieren. Die Datenquelle wird als unabhängige Ressource definiert, sodass durch mehrere Indexer verwendet werden kann.

**Indexer** beschreibt, wie die Daten aus der Datenquelle einen Ziel-Suchindex fließt. Sie sollten auf einem Indexer für jede Kombination Ziel Index- und Quelle. Mehrere Indexer in demselben Index kann zwar, kann ein Indexer nur in einem einzigen Index schreiben. Indexer wird verwendet:

- Führen Sie eine einmalige Kopie der Daten zum Auffüllen des Indexes.
- Synchronisieren Sie einen Index mit der Datenquelle nach einem Zeitplan. Der Zeitplan ist Teil der Definition der Indexer.
- Rufen Sie Updates auf Anforderung einen Index nach Bedarf auf.

##<a id="CreateDataSource"></a>Schritt 1: Erstellen einer Datenquelle

Ausstellen einer HTTP POST-Anforderung zum Erstellen einer neuen Datenquelle in Azure Suchdienst, einschließlich der folgenden Anforderungsheader.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Die `api-version` ist erforderlich. Gültige Werte sind `2015-02-28` oder höher. Besuchen Sie die [API-Versionen in Azure Suche](../search/search-api-versions.md) alle unterstützten Versionen der Such-API finden Sie unter.

Die Anforderung enthält die Datenquellendefinition, die folgenden Felder umfaßt:

- **Name**: Wählen Sie einen beliebigen Namen für die DocumentDB-Datenbank.

- **Typ**: mit `documentdb`.

- **Anmeldeinformationen**:

    - **ConnectionString**: erforderlich. Geben Sie die Verbindungsinformationen zur Datenbank Azure DocumentDB im folgenden Format:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **Container**:

    - **Name**: erforderlich. Geben Sie die Id der DocumentDB Auflistung indiziert werden.

    - **Abfrage**: Optional. Sie können eine Abfrage, um ein beliebiges JSON-Dokument in ein vereinfachtes Schema zu reduzieren, die Azure Suche indiziert werden können.

- **DataChangeDetectionPolicy**: Optional. Finden Sie unter [Daten Erkennung Richtlinie ändern](#DataChangeDetectionPolicy) .

- **DataDeletionDetectionPolicy**: Optional. [Datenrichtlinien löschen Erkennung](#DataDeletionDetectionPolicy) unten anzeigen

Nachfolgend finden Sie ein [Beispiel-Anforderungstext](#CreateDataSourceExample).

###<a id="DataChangeDetectionPolicy"></a>Erfassen geänderte Dokumente

Änderung Erkennung Datenrichtlinien dient geänderte Daten effizient identifizieren. Derzeit ist die einzige unterstützte Richtlinie die `High Water Mark` mit dem `_ts` Zeitstempel letzte Änderung Eigenschaft DocumentDB - wie folgt angegeben:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Sie müssen auch hinzufügen `_ts` in der Projektion und `WHERE` -Klausel für die Abfrage. Zum Beispiel:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Gelöschte Dokumente erfassen

Wenn Zeilen aus der Tabelle gelöscht werden, sollten Sie die Zeilen aus dem Suchindex sowie löschen. Datenrichtlinien löschen Erkennung dient gelöschte Daten effizient identifizieren. Derzeit ist die einzige unterstützte Richtlinie der `Soft Delete` Richtlinie (Löschen wird mit einer Art markiert), der wie folgt angegeben:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] Sie müssen die SoftDeleteColumnName-Eigenschaft in der SELECT-Klausel einschließen, wenn Sie eine benutzerdefinierte Projektion verwenden.

###<a id="CreateDataSourceExample"></a>Text wird anfordern

Das folgende Beispiel erstellt eine Datenquelle mit einer benutzerdefinierten Abfrage und Richtlinien:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Antwort

Eine HTTP-201 erstellte Antwort erhalten, wenn die Datenquelle erstellt wurde.

##<a id="CreateIndex"></a>Schritt 2: Erstellen eines Indexes

Erstellen Sie einen Ziel Azure Index haben Sie eine bereits. Dies ist in der [Azure-Portal-Benutzeroberfläche](../search/search-create-index-portal.md) oder mit der [Index-API erstellen](https://msdn.microsoft.com/library/azure/dn798941.aspx)möglich.

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Sicherstellen Sie, dass das Schema der Zielindex Schema Herkunftsbelege JSON oder die Ausgabe des benutzerdefinierten Abfrageprojektion vereinbar ist.

>[AZURE.NOTE] Partitionierte Sammlungen wird Dokument Standardschlüssel des DocumentDB `_rid` -Eigenschaft in umbenannt wird `rid` in Azure suchen. Auch DocumentDB `_rid` Werte enthalten, die in Azure Suchschlüssel; ungültig daher die `_rid` Base64-codiert sind.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Abbildung a-Zuordnung zwischen JSON-Datentypen und Datentypen Azure suchen

| JSON-DATENTYP|   INDEX-FELDTYPEN KOMPATIBEL ZIEL|
|---|---|
|Bool|Edm.Boolean Edm.String|
|Zahlen, die wie Zahlen aussehen|Edm.Int32, Int64, Edm.String|
|Zahlen, die anscheinend Gleitkomma-Punkte|Edm.Double Edm.String|
|Zeichenfolge|Edm.String|
|Arrays von primitiven Typen, z. B. "a", "b", "C" |Collection(EDM.String)|
|Zeichenfolgen, die wie Datumsangaben aussehen| Edm.DateTimeOffset Edm.String|
|GeoJSON Objekte z. B. {"Typ": "Point", "Koordinaten": [lang Lat]} | Edm.GeographyPoint |
|JSON-Objekte|N/A|

###<a id="CreateIndexExample"></a>Text wird anfordern

Das folgende Beispiel erstellt einen Index mit einem Feld-Id und Beschreibung:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Antwort

Eine HTTP-201 erstellte Antwort erhalten, wenn der Index erstellt wurde.

##<a id="CreateIndexer"></a>Schritt 3: Erstellen eines Indexers

Einen neuen Indexer in Azure-Suchdienst können durch die folgenden Header einer HTTP POST-Anforderung mit.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Die Anforderung enthält die Indexer-Definition, die die folgenden Felder enthalten soll:

- **Name**: erforderlich. Der Name des Indexers.

- **DataSourceName**: erforderlich. Der Name einer vorhandenen Datenquelle.

- **TargetIndexName**: erforderlich. Der Name eines vorhandenen Indexes.

- **Zeitplan**: Optional. Die [Indizierung Zeitplan](#IndexingSchedule) unten anzeigen

###<a id="IndexingSchedule"></a>Indexer auf einem Zeitplan ausgeführt

Indexer kann optional auch einen Zeitplan angeben. Wenn ein Terminplan vorhanden ist, wird der Indexer regelmäßig nach Zeitplan ausgeführt. Zeitplan hat folgende Attribute:

- **Intervall**: erforderlich. Führt eine Dauer, die ein Intervall oder einen Zeitraum für Indexer angibt. Das kleinste zulässige Intervall ist 5 Minuten. die längste beträgt einen Tag. Es muss als XSD-Wert "DayTimeDuration" (eine eingeschränkte Teilmenge der eine Dauer von [ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ) formatiert. Dieses Muster ist: `P(nD)(T(nH)(nM))`. Beispiele: `PT15M` alle 15 Minuten `PT2H` für alle 2 Stunden.

- **Startzeit**: erforderlich. Eine UTC-Datetime, die angibt, wann der Indexer ausgeführt.

###<a id="CreateIndexerExample"></a>Text wird anfordern

Im folgenden Beispiel wird einen Indexer, der aus der Auflistung, auf die Daten kopiert die `myDocDbDataSource` Datenquellen die `mySearchIndex` Index nach einem Zeitplan, der beginnt am 1. Januar 2015 UTC und stündlich ausgeführt.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Antwort

Eine HTTP-201 erstellte Antwort erhalten, wenn der Index erstellt wurde.

##<a id="RunIndexer"></a>Schritt 4: Ausführen ein Indexers

Neben regelmäßig nach einem Zeitplan ausgeführt, kann ein Indexer auch bei Bedarf aufgerufen werden die folgenden HTTP POST-Anforderung:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Antwort

Eine HTTP 202 akzeptiert Antwort erhalten, wenn der Indexer erfolgreich aufgerufen wurde.

##<a name="GetIndexerStatus"></a>Schritt 5: Get Indizierungsstatus

Sie können eine HTTP GET-Anforderung zum Abrufen des aktuellen Status und Ausführung Verlaufs eines Indexers ausgeben:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Antwort

Sie sehen eine HTTP 200 OK-Antwort zusammen mit einer Antwort, die Informationen über Indexer ordnungsgemäßen Gesamtzustand der letzten Aufruf Index sowie den Verlauf der letzten Indexer Aufrufe (falls vorhanden) enthält.

Die Antwort sollte wie folgt aussehen:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Ausführungsverlauf enthält bis die 50 neuesten abgeschlossenen Einheiten, die in umgekehrter Reihenfolge sortiert werden (damit die neueste Ausführung in der Antwort zuerst eintritt).

##<a name="NextSteps"></a>Nächste Schritte

Herzlichen Glückwunsch! Gelernt wie Azure Search Indexer für DocumentDB mit Azure DocumentDB integriert

 - Mehr über Azure DocumentDB finden Sie unter der [DocumentDB-Seite](https://azure.microsoft.com/services/documentdb/).

 - Wie Azure Suche finden Sie unter [Service Suchseite](https://azure.microsoft.com/services/search/).
