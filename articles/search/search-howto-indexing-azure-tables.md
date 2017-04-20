<properties
pageTitle="Indizierung von Azure Table Storage Azure Suche"
description="Erfahren Sie, wie Daten in Azure-Tabellen mit Azure Suche indizieren"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Indizierung von Azure Table Storage Azure Suche

Dieser Artikel beschreibt, wie Sie Azure Search Indexdaten in Azure-Tabellenspeicher gespeichert. Der neue Tabelle Azure Search Indexer kann dabei nahtlos. 

> [AZURE.IMPORTANT] Diese Funktion ist derzeit in der Vorschau. Es ist nur in der Version **2015-02-28-Vorschau** REST-API und in Version 2.0-Vorschau des .NET SDK verfügbar. Bitte denken Sie daran, Vorschau APIs für Test- und Evaluierungszwecke vorgesehen und sollte nicht in der Produktion verwendet werden.

## <a name="setting-up-azure-table-indexing"></a>Einrichten von Azure Tabelle indizieren

Und Indexer Azure Tabelle konfigurieren, können Sie die REST-API von Azure Suche erstellen und Verwalten von **Indexern** und **Datenquellen** beschriebenen [Indizierungsvorgänge](https://msdn.microsoft.com/library/azure/dn946891.aspx). Sie können auch .NET SDK, [Version 2.0-Vorschau](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) . In Zukunft Unterstützung für Tabelle indizieren Azure-Portal hinzugefügt werden.

Eine Datenquelle gibt die Daten für den index, Anmeldeinformationen Zugriff auf Daten und Richtlinien, die Azure Suche ändert Daten (neue geändert oder gelöschte Zeilen) effizient zu aktivieren.

Indexer liest Daten aus einer Datenquelle und lädt sie in ein Zielindex.

Tabelle indizieren einrichten:

1. Erstellen einer Datenquelle
    - Legen Sie die `type` Parameter`azuretable`
    - Ihre Storage-Konto Verbindungszeichenfolge übergeben der `credentials.connectionString` Parameter
    - Geben Sie die Tabelle mit den `container.name` Parameter
    - Geben Sie optional eine Abfrage mit der `container.query` Parameter. Verwenden Sie nach Möglichkeit einen Filter auf PartitionKey für optimale Leistung; Jede Abfrage führt einen vollständigen Tabellenscan Leistungseinbußen bei großen Tabellen führen kann.
2. Erstellen Sie einen Index mit dem Schema, das die Spalten in der Tabelle entspricht, die Sie indizieren möchten. 
3. Erstellen des Indexers Suchindex Datenquelle herstellen.

### <a name="create-data-source"></a>Datenquelle erstellen

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Weitere API Datasource erstellen finden Sie unter [Datenquelle erstellen](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Index erstellen 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Weitere API Index erstellen finden Sie unter [Create Index](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Index erstellen 

Abschließend erstellen Sie, der die Datenquelle und das Zielindex verweist Indexer. Zum Beispiel:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Weitere Informationen zu API Indexer erstellen anzeigen Sie [Indexer erstellen](search-api-indexers-2015-02-28-preview.md#create-indexer)

Das ist es - Indizierung zufrieden!

## <a name="dealing-with-different-field-names"></a>Umgang mit anderen Feldnamen

Häufig werden die Feldnamen in den vorhandenen Index von Eigenschaftennamen in der Tabelle. **Feld Zuordnung** können Sie die Namen aus der Tabelle auf den Feldnamen im Suchindex zuordnen. Informationen zum Feld Zuordnung finden Sie unter [Azure Search Indexer Feld Zuordnung Brücke Unterschiede zwischen Daten und Indizes durchsuchen](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Behandeln Dokumentschlüssel

In Azure Search identifiziert die Dokumentschlüssel eindeutig ein Dokument. Jede Suchindex muss genau ein Feld vom Typ `Edm.String`. Das Feld muss für jedes Dokument, das dem Index hinzugefügt wird (tatsächlich ist das einzige erforderliche Feld).

Da Zeilen einen zusammengesetzten Schlüssel haben, generiert Azure Search synthetische Feld `Key` ist eine Verkettung der Partition Schlüssel und Zeile Werte. Beispielsweise wenn eine Zeile PartitionKey ist `PK1` und RowKey `RK1`, dann `Key` wird der Wert `PK1RK1`. 

> [AZURE.NOTE] Die `Key` Wert enthält Zeichen, die im Dokument Tasten, z. B. Bindestriche ungültig sind. Ungültige Zeichen beschäftigen, mit dem `base64Encode` [Zuordnungsfunktion](search-indexer-field-mappings.md#base64EncodeFunction). Wenn Sie dies tun, müssen Sie auch URL-sichere Base64-Codierung beim Aufrufen wie Lookup Dokumentschlüssel API übergeben.

## <a name="incremental-indexing-and-deletion-detection"></a>Inkrementell indizieren und löschen-Erkennung
 
Beim Einrichten eines Indexers Tabelle nach einem Zeitplan ausgeführt, indiziert nur neue oder aktualisierte Zeilen gemäß einer Zeile `Timestamp` Wert. Müssen eine Erkennung Politik angeben – inkrementell indizieren Sie automatisch aktiviert. 

Um anzugeben, dass bestimmte Dokumente aus dem Index entfernt werden müssen, können Sie anstelle eine Strategie weichen löschen – Löschen einer Zeile, eine Eigenschaft, um anzugeben, dass es gelöscht, und eine weiche löschen Erkennung für die Datenquelle Richtlinie hinzufügen. Nachfolgend aufgeführten Richtlinien wird beispielsweise, dass eine Zeile gelöscht wird, wenn es eine Eigenschaft `IsDeleted` mit dem Wert `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns, Azure Suche zu verbessern.

Wenn Sie Wünsche oder Verbesserungsvorschläge haben, bitte erreichen Sie uns auf unserer [Website UserVoice](https://feedback.azure.com/forums/263029-azure-search/).