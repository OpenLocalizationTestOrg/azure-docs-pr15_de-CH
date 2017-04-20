<properties
pageTitle="CSV-Blobs mit Suche Azure Blob Indexer die Indizierung | Microsoft Azure"
description="Erfahren Sie, wie CSV-Blobs Azure Suche indizieren"
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
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>CSV-Blobs mit Suche Azure Blob Indexer die Indizierung 

Standardmäßig begrenzt [Suche Azure Blob Indexer](search-howto-indexing-azure-blob-storage.md) analysiert Text Blobs als einzelnes Stück Text. Mit Blobs mit CSV-Daten möchten Sie jedoch häufig jede Zeile im Blob als separates Dokument behandelt. Betrachten Sie die folgende Textdatei: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

in 2 Dokumente analysieren möchten jedes "Id", "DatePublished" und "Tags" Felder enthält.

In diesem Artikel erfahren Sie, wie CSV-Blobs mit Blob Azure Search Indexer analysiert. 

> [AZURE.IMPORTANT] Diese Funktion ist derzeit in der Vorschau. Sie steht nur in der Version **2015-02-28-Vorschau**REST-API. Bitte denken Sie daran, Vorschau APIs für Test- und Evaluierungszwecke vorgesehen und sollte nicht in der Produktion verwendet werden. 

## <a name="setting-up-csv-indexing"></a>Einrichten von CSV-Indizierung

Index CSV-Blobs, erstellen oder Aktualisieren einer Indexer Definition mit der `delimitedText` Modus analysieren:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Weitere Informationen zu API Indexer erstellen anzeigen Sie [Indexer erstellen](search-api-indexers-2015-02-28-preview.md#create-indexer)

`firstLineContainsHeaders`Gibt an, dass die erste Zeile (nicht leer) jedes BLOB-Headern.
Wenn Blobs Erste Kopfzeile enthalten, sollte die Header in der Indexer-Konfiguration angegeben werden: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Derzeit wird nur der UTF-8-Codierung unterstützt. Auch das Komma `','` Zeichen als Trennzeichen verwendet wird. Benötigen Sie Unterstützung für andere Codierung oder Trennzeichen, Bitte teilen Sie uns auf [unserer Website UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Wenn Sie getrennte Textanalyse Modus verwenden, nimmt Azure Suche werden alle Blobs aus CSV. Benötigen Sie aus CSV und -CSV Blobs in derselben Datenquelle unterstützen, Bitte teilen Sie uns auf [unserer Website UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="request-examples"></a>Anforderung-Beispiele

Indem diese alle zusammen Beispiele die komplette Nutzlast. 

Datenquelle: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns, Azure Suche zu verbessern.

Wenn Sie Wünsche oder Verbesserungsvorschläge haben, bitte erreichen Sie uns auf unserer [Website UserVoice](https://feedback.azure.com/forums/263029-azure-search/).