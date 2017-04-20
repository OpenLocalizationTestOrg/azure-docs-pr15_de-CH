<properties
pageTitle="JSON-Blobs mit Suche Azure Blob Indexer die Indizierung"
description="JSON-Blobs mit Suche Azure Blob Indexer die Indizierung"
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
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>JSON-Blobs mit Suche Azure Blob Indexer die Indizierung 

Dieser Artikel beschreibt, wie Search Azure Blob Indexer um strukturierte Inhalte von Blobs extrahieren, JSON enthalten, konfigurieren.

## <a name="scenarios"></a>Szenarien

[Suche Azure Blob Indexer](search-howto-indexing-azure-blob-storage.md) standardmäßig analysiert JSON Blobs als einzelnes Stück Text. Oft möchten Sie die Struktur Ihrer Dokumente JSON. Betrachten Sie das JSON-Dokument 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Sie möchten in einem Dokument Azure Search "Text", "DatePublished" und "Tags" Felder zu analysieren.

Auch wenn die Blobs ein **Array von JSON-Objekten enthalten**empfiehlt jedes Element des Arrays zu einem separaten Dokument Azure Search. Betrachten Sie ein Blob mit diesem JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Sie können Azure Suchindex mit 3 separate Dokumente mit "Id" und "Text" Felder ausfüllen. 

> [AZURE.IMPORTANT] Diese Funktion ist derzeit in der Vorschau. Sie steht nur in der Version **2015-02-28-Vorschau**REST-API. Bitte denken Sie daran, Vorschau APIs für Test- und Evaluierungszwecke vorgesehen und sollte nicht in der Produktion verwendet werden. 

## <a name="setting-up-json-indexing"></a>Einrichten von JSON Indizierung

JSON-Blobs indizieren, legen Sie die `parsingMode` Konfigurationsparameter, `json` (auf jeden Blob als ein einziges Dokument zu indizieren) oder `jsonArray` (wenn der Blobs ein JSON-Array enthalten): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Verwenden Sie bei Bedarf **Feld Zuordnung** Eigenschaften aufgefüllt Suchindex Ziel JSON-Quelldokument auszuwählen.  Dies wird im folgenden ausführlich beschrieben. 

> [AZURE.IMPORTANT] Bei Verwendung `json` oder `jsonArray` Analyse Modus übernimmt Azure Suche werden alle Blobs in Ihrer Datenquelle JSON. Benötigen Sie aus JSON und nicht JSON Blobs in derselben Datenquelle unterstützen, Bitte teilen Sie uns auf [unserer Website UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="using-field-mappings-to-build-search-documents"></a>Mithilfe von Feld Zuordnung erstellen Dokumente durchsuchen 

Azure Search derzeit können keine beliebige JSON-Dokumente direkt indizieren nur primitive Datentypen und Zeichenfolgenarrays GeoJSON Punkte unterstützt. Allerdings können Sie **Feld Zuordnung** Teile Ihres Dokuments JSON und "heben" sie in Felder der obersten Ebene des Dokuments suchen. Feld Zuordnung Grundlagen finden Sie unter [Azure Search Indexer Feld Zuordnung Brücke Unterschiede zwischen Daten und Indizes durchsuchen](search-indexer-field-mappings.md).

Kommen zurück zu unserem Beispiel JSON-Dokument: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Angenommen, Sie haben einen Index mit den folgenden Feldern: `text` des Typs Edm.String, `date` vom Typ Edm.DateTimeOffset, und `tags` vom Typ Collection(Edm.String). Verwenden Sie zum Zuordnen der JSON in die gewünschte Form das folgende Feld Zuordnung: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

Die Quelle Feldnamen in der Zuordnung werden mit [JSON Zeiger](http://tools.ietf.org/html/rfc6901) -Notation angegeben. Sie beginnen mit einem Schrägstrich verweisen auf den Stamm des Dokuments JSON und dann die gewünschte Eigenschaft (auf beliebiger Ebene der Schachtelung) mit Schrägstrich getrennt Weiterleitungspfad Drillinto. 

Mit einem nullbasierten Index können Sie auch einzelne Arrayelemente verweisen. Um das erste Element des Arrays "Tags" aus dem obigen Beispiel auszuwählen, z. B. eine feldzuordnung wie folgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Wenn ein Name für das Quellfeld in einem Feld Zuordnung Pfad eine Eigenschaft, die in JSON nicht vorhanden ist bezieht, wird die Zuordnung ohne Fehler übersprungen. Dies erfolgt, damit Dokumente mit einem anderen Schema unterstützt werden können (die allgemeinen Anwendungsfall). Da keine Validierung ist müssen Sie Tippfehler in der Mappingspezifikation Feld vermeiden. 

JSON-Dokumente nur einfache Eigenschaften enthalten, können Sie nicht Feld Zuordnung benötigen. Beispielsweise wenn Ihre JSON so aussieht, werden Eigenschaften "Text", "DatePublished" und "Tags" direkt in die entsprechenden Felder in den Suchindex zuordnen: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Indizieren von geschachtelten JSON-arrays

Wenn Sie ein Array von JSON-Objekte, aber Arrays indexieren wollen darin irgendwo innerhalb des Dokuments? Sie können auswählen, welche Eigenschaft enthält das Array mit den `documentRoot` Eigenschaft. Angenommen, die Blobs wie folgt aussehen: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

Verwenden Sie diese Konfiguration zum Indizieren des Arrays in der Eigenschaft "level2" enthalten: 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Anforderung-Beispiele

Indem diese alle zusammen Beispiele die vollständige Nutzlasten. 

Datenquelle: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns, Azure Suche zu verbessern.

Wenn Sie Wünsche oder Verbesserungsvorschläge haben, bitte erreichen Sie uns auf unserer [Website UserVoice](https://feedback.azure.com/forums/263029-azure-search/).