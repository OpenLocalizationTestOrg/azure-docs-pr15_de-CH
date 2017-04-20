<properties
   pageTitle="Azure Search Service REST API-Version 2015-02-28-Vorschau | Microsoft Azure | Azure Suchvorschau API"
   description="Azure Search Service REST API-Version 2015-02-28-Vorschau enthält experimentelle Features wie natürlichsprachliche Analyzer und MoreLikeThis suchen."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure Search Service REST API: Version 2015-02-28-Vorschau

Dieser Artikel ist die Referenzdokumentation für `api-version=2015-02-28-Preview`. Diese Vorschau erweitert die aktuelle allgemein verfügbare Version [API-Version 2015-02-28 =](https://msdn.microsoft.com/library/dn798935.aspx), durch die folgenden experimentelle Features:

- `moreLikeThis`Fragen Sie ab, Parameter in [Dokumente durchsuchen](#SearchDocs) API. Andere Dokumente, die für einen anderen bestimmten Dokument gefundenen.

Einige zusätzliche Teile der `2015-02-28-Preview` REST-API-Dokumentation. Dazu gehören:

- [Bewertung von Profilen](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indexer](search-api-indexers-2015-02-28-preview.md)

Azure-Suchdienst ist in mehreren Versionen verfügbar. Einzelheiten finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

## <a name="apis-in-this-document"></a>APIs in diesem Dokument

Azure Search-Dienst-API unterstützt zwei URL-Syntax für API-Vorgänge: einfache und OData (Details finden Sie unter [Unterstützung für OData (Azure Such-API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) ). Die folgende Liste zeigt die einfache Syntax.

[Index erstellen](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Index aktualisieren](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Index abrufen](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Indizes auflisten](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Index-Statistiken](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Analyzers](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Löschen eines Indexes](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Hinzufügen, löschen und Aktualisieren von Daten in einem Index](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Dokumente durchsuchen](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Dokument suchen](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Anzahl Dokumente](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Vorschläge](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Indexoperationen

Sie können erstellen und Verwalten von Indizes in Azure-Suchdienst über einfaches HTTP (POST, GET, PUT, DELETE) für eine Ressource angegebenen Index. Um einen Index zu erstellen, Buchen Sie zuerst eine JSON-Dokument, das IndexSchema beschreibt. Das Schema definiert der Index, Datentypen und wie sie (beispielsweise in Volltextsuchen, filtern, sortieren oder facettierung) verwendet werden können. Außerdem definiert bewertet Profile, Suggesters und andere Attribute konfigurieren Sie das Verhalten des Indexes.

Das folgende Beispiel veranschaulicht eine Suche mit dem Feld Beschreibung in zwei Sprachen definiert Hotelinformationen zum Schema. Beachten Sie, wie Attribute steuern, wie das Feld verwendet wird. Beispielsweise die `hotelId` als Dokument-Taste verwendet wird (`"key": true`) und Volltextsuche ausgeschlossen (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Nachdem der Index erstellt ist, können Sie Dokumente hochladen, die den Index auffüllen. Diesen Schritt finden Sie unter [Hinzufügen oder Aktualisieren von Dokumenten](#AddOrUpdateDocuments) .

Ein Einführungsvideo zum Indizieren in Azure Suche finden Sie unter [Channel 9 Cloud Cover Episode Azure suchen](http://go.microsoft.com/fwlink/p/?LinkId=511509).


<a name="CreateIndex"></a>
## <a name="create-index"></a>Index erstellen

Ein Index ist die primäre organisieren und Durchsuchen von Dokumenten in Azure Suche, ähnlich wie eine Tabelle Datensätze in einer Datenbank verwaltet. Jeder Index hat eine Sammlung von Dokumenten alle IndexSchema (Namen, Datentypen und Eigenschaften) entsprechen, dass Indizes auch zusätzliche Konstrukte (Suggesters, bewertungsprofilen und CORS-Optionen), die andere Verhaltensweisen Suche definieren angeben.

Sie können einen neuen Index in einer Azure-Suchdienst mithilfe einer HTTP-POST oder PUT-Anforderung erstellen. Der Hauptteil der Anforderung ist ein JSON-Schemas, Index und Konfiguration Informationen angibt.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativ können Sie PUT verwenden und geben den Indexnamen den URI. Wenn der Index nicht vorhanden ist, wird es erstellt.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Erstellen eines Indexes bestimmt die Struktur der Dokumente gespeichert und in Suchvorgängen verwendet. Den Index aufgefüllt wird separat. Für diesen Schritt können Sie einen [Indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (unterstützten Datenquellen verfügbar) oder einen Vorgang [Hinzufügen, aktualisieren oder Löschen von Dokumenten](https://msdn.microsoft.com/library/azure/dn798930.aspx) . Der invertierte Index generiert, wenn die Belege gebucht werden.

**Hinweis**: die maximale Anzahl der zulässigen Indizes variiert je nach Tarif. Kostenlose Service kann bis zu 3 Indizes. Standard-Service ermöglicht 50 Indizes pro Search-Dienst. Einzelheiten finden Sie unter [Limits und Einschränkungen](http://msdn.microsoft.com/library/azure/dn798934.aspx) .

**Anforderung**

HTTPS ist für alle Serviceanfragen. Die **Create Index** -Anforderung kann eine POST- oder PUT-Methode erstellt werden. Beim Buchen verwenden, müssen Sie ein Indexname im Hauptteil Anforderung zusammen mit der Schemadefinition Index angeben. Bei PUT ist der Indexname Teil der URL. Wenn der Index nicht vorhanden ist, wird sie erstellt. Wenn sie bereits vorhanden ist, wird es mit der neuen Definition aktualisiert.

Der Indexname muss in Kleinbuchstaben angegeben werden, mit einem Buchstaben oder einer Zahl beginnen haben keine Bindestriche oder Punkte und weniger als 128 Zeichen sein. Nach dem Start der Indexname einen Buchstaben oder eine Zahl, zählen der Rest des Namens Buchstabe, Zahl und Bindestriche als Bindestriche nicht aufeinander folgen.

Die `api-version` ist erforderlich. Eine Liste der verfügbaren Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `Content-Type`: Erforderlich. Legen Sie den`application/json`
- `api-key`: Erforderlich. Die `api-key` wird mit
- die Anforderung zum Suchdienst zu authentifizieren. Ein Zeichenfolgenwert für den Dienst eindeutig ist. Die **Create Index** -Anforderung gehört ein `api-key` Header der Administratorschlüssel (im Gegensatz zu einer Abfrageschlüssel) festgelegt.

Sie benötigen auch den Namen die angeforderten URL erstellen. Der Dienstname erhalten und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

<a name="RequestData"></a>
**Request Body-Syntax**

Der Hauptteil der Anforderung enthält eine Schemadefinition, einschließlich die Liste der Datenfelder in Dokumenten, die in diesen Index, Datentypen, Attribute sowie eine optionale Liste von Profilen, die übereinstimmenden Dokumente zur Abfragezeit Punktzahl bewerten gespeist werden.

Beachten Sie, dass für eine POST-Anforderung der Indexname im Hauptteil Anforderung angeben müssen.

Es kann nur ein Schlüsselfeld im Index sein. Es muss ein Zeichenfolgenfeld. Dieses Feld stellt den eindeutigen Bezeichner für jedes Dokument im Index gespeichert.

Die folgenden: Hauptkomponenten eines Indexes

- `name`
- `fields`fließen in diesem Index, einschließlich Name, Typ und Eigenschaften, die für das Feld zulässige Aktionen definieren.
- `suggesters`zur Vervollständigung oder Typeahead-Abfragen.
- `scoringProfiles`für benutzerdefinierte Suche Ergebnis Rangfolge verwendet. Einzelheiten finden Sie unter [Punktzahl Profile hinzufügen](https://msdn.microsoft.com/library/azure/dn798928.aspx) .
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` definiert, wie Dokumente/Abfragen indiziert/durchsuchbare Token unterteilt werden. Details finden Sie in der [Analyse in Azure suchen](https://aka.ms//azsanalysis) .
- `defaultScoringProfile`zur Bewertung Verhalten Standard überschreiben.
- `corsOptions`Cross-Ursprung der Index Abfragen können.

Die Syntax für die Strukturierung der Anforderungsnutzlast lautet wie folgt. Eine Beispiel für eine Anforderung wird weiter unten in diesem Thema bereitgestellt.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Indexattribute**

Die folgenden Attribute können festgelegt werden, wenn Sie einen Index erstellen. Einzelheiten bewertet und bewerten Profile finden Sie unter [Punktzahl Profile hinzufügen](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Legt den Namen des Feldes.

`type`-Legt den Datentyp für das Feld. Eine Liste der unterstützten Datentypen finden Sie unter [Supported Data Types](#DataTypes) .

`searchable`-Markiert das Feld als Volltext-Suche können. Dies bedeutet, dass z. B. Worttrennung während der Indizierung unterzogen werden. Festlegen einer `searchable` Feld auf einen Wert wie "sonnigen Tag" intern es in einzelne Tokens Teilen, "sonnigen" und "Tag". Dadurch können Volltextsuchen für diese Begriffe. Felder vom Typ `Edm.String` oder `Collection(Edm.String)` sind `searchable` standardmäßig. Andere Feldtypen nicht `searchable`.

  - **Hinweis**: `searchable` Felder belegen zusätzlichen Speicherplatz im Index da Azure Suche zusätzliche Token Version der Feldwert für Suchvorgänge speichern. Wenn Sie Platz in Ihrem Index speichern möchten, und Sie brauchen ein Feld in Suchvorgänge einbezogen werden, `searchable` , `false`.

`filterable`– Ermöglicht das Feld verwiesen in `$filter` Abfragen. `filterable`unterscheidet sich von `searchable` Zeichenfolgen behandelt werden. Felder vom Typ `Edm.String` oder `Collection(Edm.String)` die `filterable` Worttrennung, nicht unterzogen, damit Vergleiche sind nur genau entspricht. Beispielsweise ein Feld festgelegt `f` "sonnigen Tag" `$filter=f eq 'sunny'` keine Übereinstimmung findet, aber `$filter=f eq 'sunny day'` wird. Alle Felder sind `filterable` standardmäßig.

`sortable`-Standardmäßig sortiert Ergebnisse nach Bewertung, aber viele Erfahrungen Benutzer möchten in den Feldern sortieren. Felder vom Typ `Collection(Edm.String)` nicht `sortable`. Alle anderen Felder sind `sortable` standardmäßig.

`facetable`In einer Präsentation der Suchergebnisse, die Trefferanzahl nach Kategorie (z. B. Digitalkameras und Siehe Treffer Marke, Megapixel, Preis usw. suchen.) enthält, in der Regel verwendet. Diese Option kann nicht mit Typ verwendet `Edm.GeographyPoint`. Alle anderen Felder sind `facetable` standardmäßig.

  - **Hinweis**: Felder vom Typ `Edm.String` die `filterable`, `sortable`, oder `facetable` kann höchstens 32 KB umfassen. Ist diese Felder werden als einzelne Suchbegriff und die maximale Länge eines Begriffs in Azure Suche beträgt 32KB. Benötigen Sie mehr Text als eine einzelne Zeichenfolge-Feld zu speichern, müssen explizit `filterable`, `sortable`, und `facetable` , `false` in die Definition des Indexes.

  - **Hinweis**: Wenn ein Feld keine der oben genannten Attribute festgelegt, um `true` (`searchable`, `filterable`, `sortable`, oder`facetable`) Feld effektiv invertierten Index ausgeschlossen. Diese Option eignet sich für Felder, die nicht in Abfragen verwendet werden, aber in den Suchergebnissen angezeigt werden. Solche Felder aus dem Index ausgeschlossen verbessert die Leistung.

`key`-Markiert das Feld eindeutige Bezeichner für Dokumente in den Index enthält. Genau ein Feld muss ausgewählt werden, als das `key` Feld und muss vom Typ `Edm.String`. Felder können Dokumente direkt über die [Lookup-API](#LookupAPI)Nachschlagen verwendet werden.

`retrievable`-Legt fest, ob das Feld in den Suchergebnissen zurückgegeben werden kann.  Dies ist nützlich, wenn Sie ein Feld (z. B. Rand) verwenden als Filter sortieren oder Bewertung Mechanismus jedoch nicht das Feld für den Endbenutzer sichtbar sein soll. Dieses Attribut muss `true` für `key` Felder.

`analyzer`-Legt den Namen des Analyzers Feld zur Suche und Indizierung verwenden. Die zulässige Menge von Werten finden Sie unter [Analyzer](https://msdn.microsoft.com/library/mt605304.aspx). Diese Option kann nur verwendet werden `searchable` Felder und nicht mit entweder festgelegt werden `searchAnalyzer` oder `indexAnalyzer`.  Nachdem der Analyzer ausgewählt wird, kann für das Feld geändert werden.

`searchAnalyzer`-Legt den Namen der Analyzer verwendet Suchen für das Feld. Die zulässige Menge von Werten finden Sie unter [Analyzer](https://msdn.microsoft.com/library/mt605304.aspx). Diese Option kann nur verwendet werden `searchable` Felder. Es muss festgelegt werden, zusammen mit `indexAnalyzer` und es nicht zusammen mit der `analyzer` Option. Diese Analyse kann für ein vorhandenes Feld aktualisiert werden.

`indexAnalyzer`-Legt den Namen der Analyzer verwendet Index für das Feld. Die zulässige Menge von Werten finden Sie unter [Analyzer](https://msdn.microsoft.com/library/mt605304.aspx). Diese Option kann nur verwendet werden `searchable` Felder. Es muss festgelegt werden, zusammen mit `searchAnalyzer` und es nicht zusammen mit der `analyzer` Option. Nachdem der Analyzer ausgewählt wird, kann für das Feld geändert werden.

`suggesters`-Suche und Felder, die die Quelle des Inhalts für Vorschläge sind festgelegt. Einzelheiten finden Sie unter [Suggesters](#Suggesters) .

`scoringProfiles`-Definiert benutzerdefinierte scoring Verhaltensweisen, die Sie beeinflussen können Elemente höher in den Suchergebnissen erscheinen. Bewertungsprofile bestehen aus Feld Gewicht und Funktionen. Weitere Informationen über ein Bewertungsprofil verwendeten Attribute finden Sie unter [Bewertung Profile hinzufügen](https://msdn.microsoft.com/library/azure/dn798928.aspx) .

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Unterstützung**

Durchsuchbare Felder Analyse unterziehen meisten umfasst häufig Trennfugen Text Normalisierung und Begriffe herausfiltern. Standardmäßig sind durchsuchbare Felder in Azure mit [Apache Lucene Standard Analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) analysiert die Text in folgenden Regeln["Unicode Text Segmentierung"](http://unicode.org/reports/tr29/) unterteilt. Außerdem konvertiert standard Analyzer alle Zeichen in Kleinbuchstaben Formular. Indizierte Dokumente und Suchbegriffen wechseln durch die Analyse während der Indizierung und Abfrage-Verarbeitung

Azure unterstützt eine Vielzahl von Sprachen. Jede Sprache muss einen nicht standardmäßigen Text-Analyzer Merkmale einer bestimmten Sprache ausmacht. Azure Suche bietet zwei Arten von Analyzer:

- 35 Analysatoren Lucene unterstützt.
- 50 Analysatoren Verarbeitung Technologie und Bing proprietären Microsoft natürlicher Sprache unterstützt.

Einige Entwickler bevorzugen Lucene vertraute, einfache und Open Source-Lösung. Lucene Analysatoren sind schneller haben, aber Microsoft Analyzer Funktionen Lemmatisierung Word (in Sprachen wie Deutsch, Dänisch, Niederländisch, Schwedisch, Norwegisch, Estnisch, Fertig stellen, Ungarisch, Slowakisch) kompositazerlegung und Entity Recognition (URLs, e-Mails, Datumsangaben, Zahlen). Wenn möglich sollten Sie Vergleiche von Microsoft und Lucene Analysatoren entscheiden, welches besser ausführen.

***Vergleich***

Lucene Analyzer für Englisch erweitert den standard-Analyzer. Wörter (nachfolgende des) Possessivformen entfernt, gilt nach [Porter Wortstamm Algorithmus](http://tartarus.org/~martin/PorterStemmer/)Wortstamm und englische [Stoppwörter](http://en.wikipedia.org/wiki/Stop_words)entfernt.

Demgegenüber führt Microsoft Analyzer Lemmatisierung statt Wortstamm. Es bedeutet, dass sie gebeugten und Unregelmäßige Formen besser die Ergebnisse mehr relevante Suchergebnisse (Uhr Modul 7 [Azure Suche MVA](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) -Präsentation weitere Details) verarbeiten kann.

Indizierung mit Microsoft Analysatoren ist durchschnittlich zwei-bis dreimal langsamer als die äquivalente Lucene je nach Sprache. Suche Leistung sollte deutlich Durchschnittsgröße Abfragen unberührt.

***Konfiguration***

Für jedes Feld in der Indexdefinition lassen sich die `analyzer` -Eigenschaft ein Analyzer Name, welche Sprache und Kreditor angibt. Gleiche Analyzer gelten beim Indizieren und suchen das Feld.
Sie können z. B. separate Felder für Englisch, Französisch und Spanisch hotelbeschreibung, die nebeneinander im selben Index vorhanden. [Abfrageparameter Suchfeldern'](#SearchQueryParameters) an die sprachspezifischen Feld gegen in Abfragen verwenden. Enthalten Abfragebeispiele überprüfen die `analyzer` -Eigenschaft [Dokumente durchsuchen](#SearchDocs). 

***Analyzer-Liste***

Unten ist die Liste der unterstützten Sprachen zusammen mit Lucene und Microsoft Analyzer Namen.

<table style="font-size:12">
    <tr>
        <th>Sprache</th>
        <th>Microsoft Analyzer name</th>
        <th>Lucene Analyzer name</th>
    </tr>
    <tr>
        <td>Arabisch</td>
        <td>ar.Microsoft</td>
        <td>ar.Lucene</td>      
    </tr>
    <tr>
        <td>Armenisch</td>
        <td></td>
        <td>Hy.Lucene</td>
    </tr>
    <tr>
        <td>Bangla</td>
        <td>Bn.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Baskisch</td>
        <td></td>
        <td>EU.Lucene</td>
    </tr>
    <tr>
        <td>Bulgarisch</td>
        <td>BG.Microsoft</td>
        <td>BG.Lucene</td>
    </tr>
    <tr>
        <td>Katalanisch</td>
        <td>ca.Microsoft</td>
        <td>ca.Lucene</td>          
    </tr>
    <tr>
        <td>Chinesisch (Vereinfacht)</td>
        <td>Zh-Hans.microsoft</td>
        <td>Zh-Hans.lucene</td>     
    </tr>
    <tr>
        <td>Chinesisch (Traditionell)</td>
        <td>Zh-Hant.microsoft</td>
        <td>Zh-Hant.lucene</td>     
    <tr>
    <tr>
        <td>Kroatisch</td>
        <td>HR.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Tschechisch</td>
        <td>cs.Microsoft</td>
        <td>cs.Lucene</td>      
    </tr>    
    <tr>
        <td>Dänisch</td>
        <td>Da.Microsoft</td>
        <td>Da.Lucene</td>      
    </tr>    
    <tr>
        <td>Holländisch</td>
        <td>NL.Microsoft</td>
        <td>NL.Lucene</td>  
    </tr>    
    <tr>
        <td>Englisch</td>        
        <td>en.Microsoft</td>
        <td>en.Lucene</td>      
    </tr>
    <tr>
        <td>Estnisch</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Finnisch</td>
        <td>Fi.Microsoft</td>
        <td>Fi.Lucene</td>      
    </tr>    
    <tr>
        <td>Französisch</td>
        <td>FR.Microsoft</td>
        <td>FR.Lucene</td>      
    </tr>
    <tr>
        <td>Galizisch</td>
        <td></td>
        <td>GL.Lucene</td>      
    </tr>
    <tr>
        <td>Deutsch</td>
        <td>de.Microsoft</td>
        <td>de.Lucene</td>      
    </tr>
    <tr>
        <td>Griechisch</td>
        <td>EL.Microsoft</td>
        <td>EL.Lucene</td>      
    </tr>
    <tr>
        <td>Gujarati</td>
        <td>gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebräisch</td>
        <td>HE.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.Lucene</td>      
    </tr>
    <tr>
        <td>Ungarisch</td>      
        <td>hu.Microsoft</td>
        <td>hu.Lucene</td>
    </tr>
    <tr>
        <td>Isländisch</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonesisch (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.Lucene</td>      
    </tr>
    <tr>
        <td>Irisch</td>
        <td></td>
        <td>GA.Lucene</td>
    </tr>
    <tr>
        <td>Italienisch</td>
        <td>IT.Microsoft</td>
        <td>IT.Lucene</td>      
    </tr>
    <tr>
        <td>Japanisch</td>
        <td>Ja.Microsoft</td>
        <td>Ja.Lucene</td>
        
    </tr>
    <tr>
        <td>Kannada</td>
        <td>KA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Koreanisch</td>
        <td>Ko.Microsoft</td>
        <td>Ko.Lucene</td>
    </tr>
    <tr>
        <td>Lettisch</td>        
        <td>LV.Microsoft</td>
        <td>LV.Lucene</td>  
    </tr>
    <tr>
        <td>Litauisch</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malayalam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malaiisch (Lateinisch)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>Mr.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Norwegisch</td>
        <td>NB.Microsoft</td>
        <td>No.Lucene</td>      
    </tr>
    <tr>
        <td>Persisch</td>
        <td></td>
        <td>FA.Lucene</td>      
    </tr>
    <tr>
        <td>Polnisch</td>
        <td>PL.Microsoft</td>
        <td>PL.Lucene</td>      
    </tr>
    <tr>
        <td>Portugiesisch (Brasilien)</td>
        <td>pt-Br.microsoft</td>
        <td>pt-Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugiesisch (Portugal)</td>
        <td>pt-Pt.microsoft</td>        
        <td>pt-Pt.lucene</td>
    </tr>
    <tr>
        <td>Punjabi</td>
        <td>PA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Rumänisch</td>
        <td>Ro.Microsoft</td>
        <td>Ro.Lucene</td>
    </tr>
    <tr>
        <td>Russisch</td>
        <td>ru.Microsoft</td>
        <td>ru.Lucene</td>  
    </tr>
    <tr>
        <td>Serbisch (Kyrillisch)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Serbisch (Lateinisch)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slowakisch</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slowenisch</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Spanisch</td>
        <td>es.Microsoft</td>
        <td>es.Lucene</td>
    </tr>
    <tr>
        <td>Schwedisch</td>
        <td>SV.Microsoft</td>
        <td>SV.Lucene</td>
    </tr>

    <tr>
        <td>Tamil</td>
        <td>TA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thailändisch</td>
        <td>th.Microsoft</td>
        <td>th.Lucene</td>
    </tr>
    <tr>
        <td>Türkisch</td>
        <td>tr.Microsoft</td>
        <td>tr.Lucene</td>      
    </tr>
    <tr>
        <td>Ukrainisch</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamesisch</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Darüber hinaus stellt Azure sprachunabhängig Analyzer Konfigurationen</td>
    <tr>
        <td>Der ASCII-Standardzeichensatz Falten</td>
        <td>standardasciifolding.Lucene</td>
        <td>
        <ul>
            <li>Unicode-Text Segmentierung (Standard Tokenizer)</li>
            <li>Aufklappbare Filter ASCII - konvertiert Unicode-Zeichen, die auf die ersten 127 ASCII-Zeichen in den jeweiligen ASCII-Zeichen gehören. Dies ist nützlich zum Entfernen diakritischer Zeichen.</li>
        </ul>
        </td>
    </tr>
</table>

[Apache Lucene Sprache Analysatoren](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html)sind alle Analyzer mit Namen versehen mit <i>Lucene</i> angetrieben. Weitere Informationen zu ASCII Faltung Filter finden Sie [hier](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

Ein `suggester` definiert, welche Felder in einem Index verwendet werden, AutoVervollständigen in Suchvorgängen unterstützt. Normalerweise werden teilweise Suchzeichenfolgen [Vorschläge API](#Suggestions) gesendet, und der Benutzer wird eine Suchabfrage eingeben die API gibt einen Satz von vorgeschlagenen Begriffe. Eine Suggester, die im Index definiert bestimmt, welche Felder die Typeahead-Suchbegriffe zu verwendet werden. Konfigurationsdetails finden Sie unter [Suggesters](#Suggesters) .

**Bewertung von Profilen**

Ein `scoringProfile` definiert benutzerdefinierte scoring Verhaltensweisen, die Sie beeinflussen können Elemente höher in den Suchergebnissen angezeigt. Bewertungsprofile bestehen aus Feld Gewicht und Funktionen. Um sie zu verwenden, geben Sie ein Profil nach Namen in der Abfragezeichenfolge.

Bewertungsprofil Standard arbeitet im Hintergrund berechnen eine suchbewertung für jedes Element in einem Resultset. Die interne können unbenannte Bewertungsprofil. Alternativ `defaultScoringProfile` mithilfe ein benutzerdefiniertes Profils standardmäßig aufgerufen wird, wenn ein benutzerdefiniertes Profil für die Abfragezeichenfolge angegeben ist.

Details finden Sie im [Suchindex (Azure Search Service-REST-API) bewertet Profile](search-api-scoring-profiles-2015-02-28-preview.md) .

**CORS-Optionen**

Clientseitiges Javascript kann nicht APIs standardmäßig aufrufen, da der Browser alle Cross-Ursprung Anfragen verhindert. CORS (Cross-Ursprung Ressourcenfreigabe) aktivieren, indem Sie die `corsOptions` Attribut Cross-Ursprung Abfragen zum Index. Hinweis nur Abfrage APIs Unterstützung CORS aus Sicherheitsgründen. Folgenden Optionen können für CORS festlegen:

- `allowedOrigins`(erforderlich): eine Liste Herkunft zum Index zugreifen können. Daher Javascript-Code aus diesen bedient dürfen Abfragen Index (vorausgesetzt, den richtigen API-Schlüssel enthält). Jedes Ursprungsland ist in der Regel der Form `protocol://fully-qualified-domain-name:port` zwar der Anschluss häufig fehlt. Finden Sie [in diesem Artikel](http://go.microsoft.com/fwlink/?LinkId=330822) Weitere Informationen.
 - Ggf. Zugriff auf alle Ursprünge gehören `*` als ein Element in der `allowedOrigins` Array. Beachten Sie, dass **Dies Praxis Produktion Suchdienste.** Allerdings kann es für Entwicklung und Debuggen nützlich.
- `maxAgeInSeconds`(optional): Browser verwenden diesen Wert bestimmt die Dauer (in Sekunden) Cache CORS Preflight-Antworten. Dies muss eine nicht Negative Ganzzahl sein. Je größer dieser Wert ist, bessere Leistung, aber desto länger dauert das CORS Änderungen wirksam. Wenn sie nicht festgelegt ist, wird standardmäßig über die Dauer von 5 Minuten verwendet.

<a name="CreateUpdateIndexExample"></a>
**Text wird anfordern**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Antwort**

Für eine erfolgreiche Anforderung: "201 Created".

Standardmäßig enthält der Antworttext JSON für die Indexdefinition, die erstellt wurde. Wenn die `Prefer` Anforderungsheader soll `return=minimal`, der Antworttext leer und Erfolg Status Code "204 kein Inhalt" statt "201 Created". Dies gilt unabhängig davon, ob PUT oder POST verwendet wurde, um den Index zu erstellen.

**Beschreibung**

Derzeit ist beschränkte Unterstützung für Index Schema-Updates. Schemaaktualisierungen, die Indizierung wie Feldtypen ändern benötigen werden derzeit nicht unterstützt. Vorhandene Felder können nicht geändert oder gelöscht, neue Felder können auf einen vorhandenen Index jederzeit hinzugefügt werden. Wenn ein neues Feld hinzugefügt wird, müssen alle vorhandenen Dokumente in den Index einen null-Wert für dieses Feld automatisch. Keine zusätzlichen Speicherplatz verbraucht, bis neue Dokumente in den Index aufgenommen werden.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

Vorschläge Feature in Azure Search ist eine Typeahead-Abfrage AutoVervollständigen-Funktion eine Liste der möglichen Suchbegriffe auf teilweise Eingaben in das Suchfeld eingegeben. Sie haben wahrscheinlich bemerkt Abfragevorschläge Verwendung kommerzieller Suchmaschinen: erzeugt eine Liste der Begriffe für Bing ".NET" eingeben ".NET 4.5" ".NET Framework 3.5" usw.. Verwendung den Suchdienst REST API erfordert Vorschläge in einer benutzerdefinierten Suche Azure-Anwendung implementieren:

- Aktivieren Sie Vorschläge **Suggester** Konstruktion in Ihrem Index hinzufügen geben den Namen, Suche und eine Liste der Felder für die Typeahead-aufgerufen wird. Beispielsweise wenn Sie teilweise Suchzeichenfolge "See" eingeben "Stadtname" als ein Quellfeld angeben führt "Seattle", "Meer" und "Seatac" (alle drei sind tatsächliche Städtenamen) für dem Benutzer als Abfragevorschläge angeboten.

- Rufen Sie Vorschläge [Vorschläge API](#Suggestions) im Anwendungscode aufrufen. Normalerweise werden teilweise Zeichenfolgen an den Dienst gesendet, und der Benutzer wird eine Suchabfrage eingeben dieser API gibt einen Satz von vorgeschlagenen Begriffe.

Erläutert, wie ein **Suggester**konfigurieren. Sie sollten auch [Vorschläge API](#Suggestions) Informationen über die Verwendung einer Suggester.

**Verwendung**

`Suggesters`Index und arbeiten am besten, wenn bestimmte Dokumente als lose Begriffen oder Sätzen vorschlagen verwendet entstehen. Die besten Kandidaten Felder sind Titel, Namen und anderen relativ kurze Sätze, die ein Element identifizieren können. Weniger effektiv sind sich wiederholende, Kategorien oder Tags oder sehr lange Felder wie Felder Beschreibung oder Kommentare.

Als Teil der Indexdefinition können Sie eine einzelne Suggester, die `suggesters` Auflistung. Die folgenden: Eigenschaften, die ein Suggester definieren

- `name`: Der Name der Suggester. Sie verwenden den Namen der Suggester, beim Aufrufen der `suggest` API.
- `searchMode`Die Strategie zum Kandidaten Ausdrücken suchen. Der einzige Modus unterstützt `analyzingInfixMatching`, die flexible Anpassung der Begriffe oder Sätze in führt.
- `sourceFields`: Eine Liste von einem oder mehreren Feldern, die die Quelle des Inhalts für Vorschläge. Nur Felder vom Typ `Edm.String` und `Collection(Edm.String)` möglicherweise Quellen für Vorschläge. Nur Felder, die einen benutzerdefinierte Sprache Analyzer festlegen können verwendet werden.

**Suggester-Beispiel**

Ein Suggester ist Teil des Indexes. Nur ein Suggester kann der `suggesters` -Auflistung in die aktuelle Version neben der Fields-Auflistung und `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Wenn die public Preview-Version von Azure Search verwendet `suggesters` ersetzt eine ältere boolesche Eigenschaft (`"suggestions": false`), Präfix Vorschläge nur für kurze Zeichenfolgen (3-25 Zeichen) unterstützt. Dem neuen `suggesters`, unterstützt infix entsprechen, die entsprechende Begriffe am Anfang oder in der Feldinhalt, bessere Fehlertoleranz in Suchzeichenfolgen findet. Ab Version erhältlich ist dies jetzt die einzige Implementierung der Vorschläge API. Das ältere `suggestions` eingeführte Eigenschaft `api-version=2014-07-31-Preview` weiterhin in dieser Version jedoch nicht im der `2015-02-28` oder höher der Azure-Suche.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Index aktualisieren

Sie können einen vorhandenen Index in Azure Suche eine HTTP PUT-Anforderung aktualisieren. Updates gehören das vorhandene Schema neue Felder hinzufügen, CORS-Optionen ändern und scoring Profile ändern. Weitere Informationen finden Sie unter [Profile Bewertung hinzufügen](https://msdn.microsoft.com/library/azure/dn798928.aspx) . Sie geben den Namen des Indexes, auf Anfrage-URI zu aktualisieren:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Wichtig:** Unterstützung für Index Schema-Updates beschränkt sich auf Vorgänge, die keine Suchindex neu. Schemaupdates, die erneute Indizierung, wie ändern Feldtypen, benötigen werden derzeit nicht unterstützt. Neue Felder können jederzeit hinzugefügt werden, obwohl vorhandene Felder geändert oder gelöscht werden können. Das gleiche gilt für `suggesters`. Neue Felder können hinzugefügt, Suggester zum Zeitpunkt der Felder hinzugefügt werden, aber Felder können nicht entfernt werden, von `suggesters` und vorhandene Felder hinzugefügt werden `suggesters`.

Wenn Index ein neues Feld hinzufügen, müssen alle vorhandenen Dokumente in den Index einen null-Wert für dieses Feld automatisch. Keine zusätzlichen Speicherplatz verbraucht, bis neue Dokumente in den Index aufgenommen werden.

**Anforderung**

HTTPS ist für alle Serviceanfragen. **Index aktualisieren** Anforderung wird mit HTTP PUT erstellt. Bei PUT ist der Indexname Teil der URL. Wenn der Index nicht vorhanden ist, wird sie erstellt. Wenn der Index bereits vorhanden ist, wird es mit der neuen Definition aktualisiert.

Der Indexname muss in Kleinbuchstaben angegeben werden, mit einem Buchstaben oder einer Zahl beginnen haben keine Bindestriche oder Punkte und weniger als 128 Zeichen sein. Nach dem Start der Indexname einen Buchstaben oder eine Zahl, zählen der Rest des Namens Buchstabe, Zahl und Bindestriche als Bindestriche nicht aufeinander folgen.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `Content-Type`: Erforderlich. Legen Sie den`application/json`
- `api-key`: Erforderlich. Die `api-key` zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für den Dienst eindeutig ist. **Index aktualisieren** Anforderung gehört ein `api-key` Header der Administratorschlüssel (im Gegensatz zu einer Abfrageschlüssel) festgelegt.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Request Body-Syntax**

Beim Aktualisieren eines vorhandenen Indexes muss Text beinhalten die ursprünglichen Schemadefinition neue Felder, die Sie hinzufügen, sowie geänderte Punktzahl Profile, Suggesters und CORS-Optionen. Wenn Sie Bewertungsprofile und CORS-Optionen nicht ändern, müssen Sie die originale, wenn der Index erstellt wurde einschließen. Im Allgemeinen das besten Muster für Updates die Indexdefinition mit GET abrufen, ändern und dann mit aktualisieren.

Die Schemasyntax zum Erstellen eines Indexes wird hier der Einfachheit reproduziert. Weitere Informationen finden Sie unter [Create Index](#CreateIndex) .

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Antwort**

Für eine erfolgreiche Anforderung: "204 kein Inhalt".

Standardmäßig wird der Antworttext leer sein. Wenn die `Prefer` Anforderungsheader soll `return=representation`, der Antworttext enthalten JSON für die Indexdefinition aktualisiert wurde. In diesem Fall werden der Erfolgsstatuscode "200 OK".

**Aktualisieren der Indexdefinition mit benutzerdefinierten Analyzer**

Ein, eine Tokenizer, token Filter oder Filter Char definiert ist, kann nicht geändert werden. Neue werden hinzugefügt, einem vorhandenen Index nur, wenn die `allowIndexDowntime` gesetzt ist in der Aktualisierungsanfrage Index: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Beachten Sie, dass dieser Vorgang den Index offline für einige Sekunden verursacht die Indizierung und Anfragen fehlschlagen wird. Performance und Schreiben der Verfügbarkeit des Indexes kann einige Minuten nach der Aktualisierung des Index wird beeinträchtigt oder für sehr große Indizes.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Liste Indizes

**Liste Indizes** Operation gibt eine Liste der Indizes derzeit in Azure Suchdienst.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Anforderung**

HTTPS ist für alle Serviceanfragen. Die **Liste Indizes** Anforderung kann mit der GET-Methode erstellt werden.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `api-key`: Erforderlich. Die `api-key` zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für den Dienst eindeutig ist. Die Anforderung **Liste Indizes** sind ein `api-key` Admin Schlüssel (im Gegensatz zu einer Abfrageschlüssel) festgelegt.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Hier ist ein Beispiel-Antworttext:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Beachten Sie, dass die Antwort nur Eigenschaften zu filtern, die Sie interessieren. Z. B. eine Liste von Indexnamen, verwenden Sie die OData `$select` Abfrageoption:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

In diesem Fall würde die Antwort aus dem obigen Beispiel wie folgt aussehen:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Dies ist eine nützliche Technik, um Bandbreite zu sparen, haben Sie viele Indizes im Suchdienst.

<a name="GetIndex"></a>
## <a name="get-index"></a>Index abrufen

Der Vorgang **Abrufen** Ruft die Indexdefinition Azure Suche ab.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Anforderung**

HTTPS ist für Serviceanfragen. **Index Get** -Anforderung kann mit der GET-Methode erstellt werden.

[Indexname] in der URI-Anforderung gibt an, welcher Index aus der Indexes-Auflistung zurückgeben.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `api-key`: Der `api-key` wird zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für den Dienst eindeutig ist. **Index Get** -Anforderung gehört ein `api-key` Admin Schlüssel (im Gegensatz zu einer Abfrageschlüssel) festgelegt.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Siehe das Beispiel JSON [Erstellen und Aktualisieren eines Indexes](#CreateUpdateIndexExample) ein Beispiel für die antwortnutzlast.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Index löschen

Der Vorgang **Löschen** entfernt einen Index und Dokumente aus Azure Suchdienst. Sie erhalten den Indexnamen Dashboard Service in Azure-Portal oder die API. Details finden Sie in der [Liste Indizes](#ListIndexes) .

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Anforderung**

HTTPS ist für Serviceanfragen. **Index löschen** -Anforderung kann mit der DELETE-Methode erstellt werden.

[Indexname] in der URI-Anforderung gibt an, welcher Index aus der Indexes-Auflistung gelöscht.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `api-key`: Erforderlich. Die `api-key` zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für die Dienst-URL eindeutig ist. **Index löschen** Anforderung gehört ein `api-key` Header der Administratorschlüssel (im Gegensatz zu einer Abfrageschlüssel) festgelegt.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

Keine.

**Antwort**

Statuscode: 204 wird kein Inhalt für eine erfolgreiche Antwort zurückgegeben.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Index-Statistiken

**Indexstatistiken Get** -Vorgang gibt Azure Suche Dokumentanzahl für den aktuellen Index und Speicherverbrauch.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Statistiken über Anzahl und Speicher Dokumentgröße werden alle paar Minuten nicht in Echtzeit erfasst. Daher spiegeln von dieser API zurückgegebenen Statistiken kürzlich Indizierungsvorgänge ändert sich aufgrund nicht.

**Anforderung**

HTTPS ist für alle Dienste Anfragen erforderlich. Die **Indexstatistiken Get** -Anforderung kann mit der GET-Methode erstellt werden.

[Indexname] in der URI-Anforderung weist den Dienst Indexstatistiken für den angegebenen Index zurückgibt.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .


**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `api-key`: Der `api-key` wird zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für den Dienst eindeutig ist. **Indexstatistiken Get** -Anforderung gehört ein `api-key` Admin Schlüssel (im Gegensatz zu einer Abfrageschlüssel) festgelegt.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Der Antworttext ist im folgenden Format:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Analyzers

**API Analyse** zeigt, wie ein Text in Token unterteilt.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Anforderung**

HTTPS ist für alle Dienste Anfragen erforderlich. **Analysieren von API** -Anforderung kann mit der POST-Methode erstellt werden.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .


**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `api-key`: Der `api-key` wird zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für den Dienst eindeutig ist. **Analysieren-API-** Anforderung gehört ein `api-key` Admin Schlüssel (im Gegensatz zu einer Abfrageschlüssel) festgelegt.

Sie benötigen auch den Indexnamen und den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

oder

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

Die `analyzer_name`, `tokenizer_name`, `token_filter_name` und `char_filter_name` müssen gültige Namen der vordefinierten oder benutzerdefinierten Analyzer, Tokenizer token Filter und Char Filter für den Index. Finden Sie mehr über den Prozess der lexikalischen Analyse [Analyse in Azure suchen](https://aka.ms/azsanalysis).

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Der Antworttext ist im folgenden Format:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**API-Beispiel analysieren**

**Anforderung**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Antwort**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Dokument-Operationen

In Azure Suche ein Index in der Cloud gespeichert und mithilfe von JSON-Dokumente, die mit dem Dienst geladen gefüllt. Alle Dokumente, die Sie hochladen umfassen Corpus Daten suchen. Dokumente enthalten Felder, von die einige in Suchbegriffe zerlegt werden sie hochladen. Die `/docs` URL-Segment in Azure Such-API stellt die Auflistung der Dokumente im Index. Alle Operationen auf die Auflistung wie hochladen, zusammenführen, löschen oder Abfragen Dokumente übernehmen platzieren im Kontext eines einzelnen Indexes so URLs für diese Vorgänge startet immer mit `/indexes/[index name]/docs` für eine gegebene Indexname.

Anwendungscode muss entweder nach Azure upload JSON-Dokumente generieren oder Verwenden eines [Indexers](https://msdn.microsoft.com/library/dn946891.aspx) Dokumente geladen, wenn die Datenquelle Azure SQL-Datenbank oder DocumentDB ist. Indizes werden in der Regel aus einem Dataset gefüllt, die Sie bereitstellen.

Sie sollten auf ein Dokument für jeden Artikel, den Sie suchen möchten. Eine Film Vermietung Anwendung möglicherweise ein Dokument pro Film Shop Anwendung möglicherweise ein Dokument pro SKU online Courseware-Anwendung möglicherweise ein Dokument pro Kurs, einem Forschungsunternehmen ein Dokument für jedes wissenschaftlichen Arbeit in ihrem Repository haben usw..

Dokumente bestehen aus einem oder mehreren Feldern. Felder können Text wird von Azure Suche in Suchbegriffe Token sowie nicht zerlegt oder nicht-Text-Werte in Filtern oder bewertungsprofilen enthalten. Das IndexSchema sind Namen, Datentypen und Funktionen unterstützt für jedes Feld bestimmt. Eines der Felder in jedem IndexSchema muss als ID festgelegt und jedes Dokument müssen einen Wert für das ID-Feld, das das Dokument im Index eindeutig identifiziert. Alle Dokumentfelder sind optional und werden standardmäßig auf einen null-Wert, wenn angegeben. Beachten Sie, dass null-Werte nicht im Suchindex Speicherplatz.

Bevor Sie Dokumente hochladen können, müssen Sie bereits Index für den Dienst erstellt haben. Details zu diesem ersten Schritt finden Sie unter [Create Index](#CreateIndex) .

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Hinzufügen, aktualisieren oder Löschen von Dokumenten

Sie können hochladen, Seriendruck, Seriendruck oder hochladen oder löschen Dokumente aus einem angegebenen Index unter Verwendung von HTTP POST. Große Anzahl von Updates wird von Dokumenten (1000 Dokumente pro Batch) bis ca. 16 MB pro Batch Batchverarbeitung empfohlen.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Anforderung**

HTTPS ist für alle Serviceanfragen. Sie können hochladen, Seriendruck, Seriendruck oder hochladen oder löschen Dokumente aus einem angegebenen Index unter Verwendung von HTTP POST.

Anfrage-URI enthält [Indexname], welcher Index um Dokumente angeben. Sie können nur Dokumente einen Index gleichzeitig zu buchen.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `Content-Type`: Erforderlich. Legen Sie den`application/json`
- `api-key`: Erforderlich. Die `api-key` zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für den Dienst eindeutig ist. Anforderung **Hinzufügen Dokumente** sind ein `api-key` Header der Administratorschlüssel (im Gegensatz zu einer Abfrageschlüssel) festgelegt.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

Der Hauptteil der Anforderung enthält ein oder mehrere Dokumente indiziert werden. Dokumente werden mit einem eindeutigen Schlüssel identifiziert. Jedes Dokument ist mit einer Aktion verknüpft: Upload, Verschmelzen, MergeOrUpload oder löschen. Upload-Anfragen müssen die Daten als Satz von Schlüssel-Wert-Paare enthalten.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Dokumentschlüssel darf nur Buchstaben, Zahlen, Bindestriche ("-"), Unterstriche ("_") und Gleichheitszeichen ("="). Weitere Informationen finden Sie unter [Naming Regeln](https://msdn.microsoft.com/library/azure/dn857353.aspx).

**Dokumentaktionen**

- `upload`Eine Uploadaktion ist vergleichbar mit "Upsert", wird das Dokument eingefügt, wenn es ist neu und aktualisiert ersetzt, falls vorhanden. Beachten Sie, dass alle Updates ersetzt werden.
- `merge`: Zusammenführung aktualisiert ein vorhandenes Dokument mit angegebenen Felder. Wenn das Dokument nicht vorhanden ist, wird die Zusammenführung fehl. Jedes Feld, das an eine Zusammenführung ersetzt das vorhandene Feld im Dokument. Dies umfasst auch Felder vom Typ `Collection(Edm.String)`. Angenommen, das Dokument enthält ein Feld "Tags" mit `["budget"]` und führen Sie einen Seriendruck mit `["economy", "pool"]` für "Tags" der letzte Wert im Feld "Tags" sein `["economy", "pool"]`. Es wird **nicht** sein `["budget", "economy", "pool"]`.
- `mergeOrUpload`: verhält sich wie `merge` ein Dokument mit dem angegebenen Schlüssel im Index bereits vorhanden. Wenn das Dokument nicht vorhanden ist verhält sich wie `upload` mit einem neuen Dokument.
- `delete`: Löschen entfernt des angegebenen Dokuments aus dem Index. Beachten Sie, dass alle Felder Sie in Geben einer `delete` Vorgang als das Feld ignoriert. Wenn Sie einzelnen Felder aus einem Dokument entfernen möchten, verwenden Sie `merge` legen Sie das Feld einfach und stattdessen explizit `null`.

**Antwort**

Statuscode 200 (OK) usw. für eine erfolgreiche Antwort bedeutet, dass alle Elemente erfolgreich indiziert wurden Ist die `status` -Eigenschaft festgelegt auf true für alle Elemente als die `statusCode` Eigenschaftensatz 201 (für neu hochgeladene Dokumente) oder 200 (für zusammengeführte oder gelöschte Dokumente):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Statuscode 207 (Multi-Status) wird zurückgegeben, wenn mindestens ein Element nicht erfolgreich indiziert wurde. Elemente, die nicht indiziert die `status` Feld auf False festgelegt. Die `errorMessage` und `statusCode` Eigenschaften geben den Grund für die Indizierung Fehler:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

Die folgende Tabelle erläutert die verschiedenen pro Dokument Statuscodes, die in der Antwort zurückgegeben werden. Beachten Sie, dass einige Probleme mit der Anforderung selbst anzugeben, während andere temporäre Fehler hinzuweisen. Letzteres Verzögerung wiederholen soll.

<table style="font-size:12">
    <tr>
        <th>Statuscode</th>
        <th>Bedeutung</th>
        <th>Wiederholbare</th>
        <th>Notizen</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokument wurde erfolgreich geändert oder gelöscht.</td>
        <td>n/a</td>
        <td>Löschvorgänge sind <a href="https://en.wikipedia.org/wiki/Idempotence">Idempotent</a>. Also auch, wenn ein Dokumentschlüssel im Index nicht vorhanden ist, führt versucht, einen Löschvorgang mit diesem Schlüssel einen Statuscode 200.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokument wurde erfolgreich erstellt.</td>
        <td>n/a</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Fehler im Dokument, das indiziert wird verhindert.</td>
        <td>Nein</td>
        <td>Die Fehlermeldung in der Antwort zeigt Fehlersuche mit dem Dokument.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Dokument konnte nicht zusammengeführt werden, weil der angegebene Schlüssel im Index vorhanden ist.</td>
        <td>Nein</td>
        <td>Dieser Fehler tritt nicht für Uploads, da sie neue Dokumente erstellen, und es nicht löschen tritt, da sie <a href="https://en.wikipedia.org/wiki/Idempotence">Idempotent</a>sind.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Ein Versionskonflikt aufgetreten beim Versuch, ein Dokument zu indizieren.</td>
        <td>Ja</td>
        <td>Dies kann auftreten, wenn Sie dasselbe Dokument mehr als einmal gleichzeitig indizieren.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Der Index ist vorübergehend nicht verfügbar, da 'AllowIndexDowntime'-Flag auf 'True' gesetzt aktualisiert wurde.</td>
        <td>Ja</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Suchdienst ist vorübergehend nicht verfügbar, möglicherweise ausgelastet.</td>
        <td>Ja</td>
        <td>Code warten soll, bevor Sie in diesem Fall wiederholen oder Risiko Verlängerung der Service nicht verfügbar.</td>
    </tr>
</table> 

**Hinweis**: Ihr Clientcode häufig 207 Antwort findet, mögliche Ursache ist das System ausgelastet ist. Sie können dies überprüfen, indem überprüft die `statusCode` -Eigenschaft 503. Wenn dies der Fall ist, wird empfohlen, ***Drosselung Indizierung Anfragen***. Andernfalls Wenn Indizierung Datenverkehr Datenträgerressourcen nicht, konnte das System starten alle Anfragen Fehler 503 ablehnen.

Statuscode 429 gibt an, dass Sie Ihr Kontingent für die Anzahl der Dokumente pro Index überschritten haben. Sie müssen einen neuen Index erstellen oder aktualisieren für höhere Grenzen.

**Beispiel:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Dokumente durchsuchen

**Ein Suchvorgang** als GET oder POST-Anforderung ausgegeben und gibt Parameter an, die die Kriterien für die Auswahl von übereinstimmenden Dokumente geben.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Verwendung von POST anstelle von GET**

Wenn HTTP GET verwenden die **Suche** API aufrufen, müssen Sie darauf achten, dass die Länge des angeforderten URL 8 KB höchstens. Dies ist ausreichend für die meisten Applikationen. Einige Programme führen jedoch sehr große Abfragen oder Filterausdrücke OData. Für diese Anwendung wird mithilfe von HTTP POST besser da größere filtern und Abfragen als abrufen können. Mit ist die Anzahl Klauseln in einer Abfrage oder die Beschränkung der unformatierten Abfrage seit der Limit der Anforderungsgröße Post ca. 16 MB Größe.

> [AZURE.NOTE] Obwohl die Anforderung Bereitstellungsgröße sehr groß ist, können nicht Suchabfragen und Filterausdrücke beliebig komplex sein. Finden Sie weitere Suche Abfrage- und Komplexität Einschränkungen [Lucene Abfragesyntax](https://msdn.microsoft.com/library/mt589323.aspx) und [OData-Ausdruckssyntax](https://msdn.microsoft.com/library/dn798921.aspx) .

**Anforderung**

HTTPS ist für Serviceanfragen. **Die Suchabfrage** kann mithilfe der Methoden GET oder POST erstellt werden.

Anfrage-URI Gibt an, welcher Index Abfrage für alle Dokumente, die den Parametern entsprechen. Parameter der Abfragezeichenfolge bei GET-Anfragen angegeben werden und die Anforderung Stelle POST fordert.

Als bewährte Methode beim Erstellen von GET-Anfragen müssen Sie [URL-Kodierung](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) angegebenen Parametern beim REST-API direkt aufrufen. Für **Suchvorgänge** dazu gehören:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

URL-Codierung wird nur auf die oben genannten Abfrageparameter empfohlen. Wenn Sie versehentlich URL-Kodierung die gesamte Abfragezeichenfolge (alles nach dem?), Anfragen unterbricht.

Außerdem muss URL-Codierung nur beim Aufrufen der REST-API direkt mit GET. Keine URL-Codierung ist erforderlich, beim Aufruf der **Suche** mithilfe von POST oder [.NET Clientbibliothek](https://msdn.microsoft.com/library/dn951165.aspx)mithilfe der URL-Codierung für Sie behandelt.

<a name="SearchQueryParameters"></a>
**Abfrageparameter**

**Suche** akzeptiert mehrere Parameter, die Abfragekriterien und auch Suchverhalten angeben. Sie bieten diese Parameter in der URL beim **Suchen** über abrufen und als JSON-Eigenschaften im Hauptteil Anforderung aufrufen, beim Aufrufen der **Suche** per POST Zeichenfolge Abfragen. Die Syntax für einige Parameter unterscheidet zwischen GET und POST. Diese Unterschiede sind gegebenenfalls unter:

`search=[string]`(optional) – der Text suchen. Alle `searchable` Felder werden standardmäßig durchsucht, wenn `searchFields` angegeben ist. Bei der Suche `searchable` Felder selbst Suchtext zerlegt, damit mehrere Begriffe durch Leerzeichen getrennt werden (Beispiel: `search=hello world`). Jeder Begriff, geben Sie `*` (Dies ist nützlich für Abfragen booleschen Filtern). Dieser Parameter ausgelassen hat die gleiche Auswirkung wie auf `*`. Einzelheiten zur Syntax Suche finden Sie unter [Einfache Abfragesyntax](https://msdn.microsoft.com/library/dn798920.aspx) .

  - **Hinweis**: die Ergebnisse kann überraschend beim Abfragen über `searchable` Felder. Die Tokenizer enthält Logik in Englisch Apostrophe, Kommas in usw. Fälle. Beispielsweise `search=123,456` entspricht einem Begriff 123,456 als einzelne Begriffe 123 und 456, da Kommas als tausend-Trennzeichen für große Zahlen in Englisch verwendet werden. Aus diesem Grund empfehlen wir Leerraum als Satzzeichen trennen Begriffe in der `search` Parameter.

`searchMode=any|all`(optional, standardmäßig `any`) -, ob einige oder alle der Suchbegriffe abgestimmt werden müssen, um das Dokument als Übereinstimmung zu zählen.

`searchFields=[string]`(optional) – die Liste der durch Trennzeichen getrennte Feldnamen für den angegebenen Text suchen. Zielfelder müssen markiert sein, als `searchable`.

`queryType=simple|full`(optional, standardmäßig `simple`) – Wenn diese Option auf "einfach" Suchtext interpretiert wird mithilfe einer Symbole ermöglicht einfache Abfragesprache +, * und "". Abfragen über alle durchsuchbaren Felder ausgewertet (oder Felder im `searchFields`) in jedem Dokument standardmäßig. Wenn der Abfragetyp soll `full` Suchtext mithilfe der Abfragesprache Lucene feldspezifischen und gewichtete Suche ermöglicht interpretiert. Einzelheiten über die Syntax der Suche finden Sie unter [Einfache Abfragesyntax](https://msdn.microsoft.com/library/dn798920.aspx) und [Lucene Abfragesyntax](https://msdn.microsoft.com/library/mt589323.aspx) . 
 
> [AZURE.NOTE] Bereich Suchen in Lucene Abfragesprache für $filter ähnlichen Funktionalität bietet, nicht unterstützt wird.

`moreLikeThis=[key]`(optional) **Wichtig:** Dieses Feature steht nur in `2015-02-28-Preview`. Diese Option kann nicht verwendet werden, in einer Abfrage, die Suchparameter Text enthält `search=[string]`. Die `moreLikeThis` Parameter findet Dokumente ähnlich durch den Dokument angegeben. Bei einer Suche angefordert wird mit `moreLikeThis`, eine Liste der Suchbegriffe basierend auf der Häufigkeit und Seltenheit Begriffe im Quelldokument generiert. Diese Begriffe werden dann für die Anforderung verwendet. Standardmäßig wird der Inhalt aller `searchable` Felder gelten, wenn `searchFields` wird verwendet, um einzuschränken, welche Felder durchsucht werden.  

`$skip=#`(optional) – die Anzahl der Suchergebnisse zu überspringen. Darf nicht größer als 100.000 sein. Wenn Sie Dokumente nacheinander scannen können keine `$skip` aufgrund dieser Einschränkung verwenden `$orderby` auf eine vollständig geordnete und `$filter` mit Abfragen statt.

> [AZURE.NOTE] Beim Aufruf der **Suche** mit POST wird dieser Parameter namens `skip` anstelle von `$skip`.

`$top=#`(optional) – die Anzahl der Suchergebnisse abzurufen. Hiermit kann in Verbindung mit `$skip` clientseitigen Auslagerung der Suchergebnisse zu implementieren.

> [AZURE.NOTE] Beim Aufruf der **Suche** mit POST wird dieser Parameter namens `top` anstelle von `$top`.

`$count=true|false`(optional, standardmäßig `false`)-gibt an, ob die Gesamtzahl der Ergebnisse abrufen. Dies ist die Anzahl aller Dokumente entsprechen den `search` und `$filter` Parameter ignoriert `$top` und `$skip`. Wenn dieser Wert auf `true` möglicherweise Leistungseinbußen. Die Anzahl die zurückgegebenen ist ein Näherungswert.

> [AZURE.NOTE] Beim Aufruf der **Suche** mit POST wird dieser Parameter namens `count` anstelle von `$count`.

`$orderby=[string]`(optional) – eine Liste von Ausdrücken zum Sortieren der Ergebnisse durch Kommas. Jeder Ausdruck kann ein Feld oder einen Aufruf der `geo.distance()` Funktion. Jeder Ausdruck folgt `asc` aufsteigend, gekennzeichnet und `desc` absteigend an. Standardmäßig wird die aufsteigende Reihenfolge. Übereinstimmung unzählige Dokumente werden TIES aufgeschlüsselt. Wenn kein `$orderby` angegeben ist, wird die Sortierreihenfolge ist absteigend nach Dokument Spielstand. Maximal 32 Klauseln für `$orderby`.

> [AZURE.NOTE] Beim Aufruf der **Suche** mit POST wird dieser Parameter namens `orderby` anstelle von `$orderby`.

`$select=[string]`(optional) – eine Liste von durch Trennzeichen getrennte Felder abzurufen. Falls nicht angegeben, sind alle Felder in dem Schema aufgerufen enthalten. Sie können alle Felder auch explizit anfordern, wenn dieser Parameter auf `*`.

> [AZURE.NOTE] Beim Aufruf der **Suche** mit POST wird dieser Parameter namens `select` anstelle von `$select`.

`facet=[string]`(null oder mehr) ein Feld Facetten durch. Optional kann die Zeichenfolge enthalten Parameter angepasst werden als durch Trennzeichen getrennte facettierung `name:value` Paare. Parameter sind gültig:

- `count`(max. Anzahl der Facetten, Standard ist 10). Kein Maximum vorhanden ist, aber höhere Werte Leistungseinbußen entsprechende, besonders wenn Feld facettierte zahlreiche eindeutige Begriffe enthält.
  - Beispiel: `facet=category,count:5` fünf Kategorien in Facet angezeigt wird.  
  - **Hinweis**: Wenn die `count` -Parameter ist kleiner als die Anzahl der eindeutigen, die Ergebnisse möglicherweise nicht korrekt. Dies ist aufgrund der Splitter facettierung Abfragen verteilt werden. Erhöhung `count` im Allgemeinen verbessert die Genauigkeit der Begriff zählt jedoch Leistungseinbußen.
- `sort`(eines `count` Zählung *Absteigend* sortieren `-count` Zählung *Aufsteigend* sortieren `value` Wert *Aufsteigend* sortieren oder `-value` Wert *Absteigend* sortieren)
  - Beispiel: `facet=category,count:3,sort:count` Ruft die drei Kategorien in facettenergebnisse in absteigender Reihenfolge nach Anzahl Dokumente mit jeder Ortsname. Z. B. wenn die ersten drei Kategorien Budget, Motels und Luxus sind 5 Treffer hat, Motels 6 hat und Luxus 4 hat, werden dann der Buckets in der Reihenfolge Motels, Budget, Luxus.
  - Beispiel: `facet=rating,sort:-value` Buckets für alle möglichen Bewertungen in absteigender Reihenfolge nach Wert erzeugt. Beispielsweise sind die Bewertung von 1 bis 5, werden die Buckets bestellt 5, 4, 3, 2, 1, unabhängig davon, wie viele Dokumente jedes entspricht.
- `values`(Pipe getrennte numerische oder `Edm.DateTimeOffset` Werte angibt dynamische Facetwerte)
  - Beispiel: `facet=baseRate,values:10|20` erzeugt drei Buckets: für Basissatz 0 bis, aber nicht 10 10 bis einschließlich, aber nicht einschließlich 20 und 20 oder höher.
  - Beispiel: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` erzeugt zwei Buckets: für Februar 2010 renoviert Hotels und für Hotels renoviert Februar 1. 2010 oder höher.
- `interval`(Ganzzahlintervall größer als 0 für Zahlen oder `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` Datum Uhrzeit Werte)
  - Beispiel: `facet=baseRate,interval:100` Buckets anhand Basissatz von Größe 100 erzeugt. Beispielsweise sind Basis Preise zwischen $60 und $600 werden Buckets für 0-100, 100-200, 200-300, 300 und 400, 400 bis 500 und 600 500.
  - Beispiel: `facet=lastRenovationDate,interval:year` ein Bucket für jedes Jahr bei Hotels wurden erstellt.
- `timeoffset`([+-] hh: mm [+-] Hhmm, oder [+-] Hh) `timeoffset` ist optional. Kann nur mit kombiniert werden die `interval` Option, wenn für ein Feld vom Typ `Edm.DateTimeOffset`. Der Wert gibt Offset der UTC auf Grenzwerte festlegen.
  - Beispiel: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` verwendet die Tag-Grenze, die beginnt am 01:00 Uhr (Mitternacht in der Ziel-Zeitzone)
- **Hinweis**: `count` und `sort` in der gleichen Spezifikation Facet kombiniert werden, aber sie kombiniert werden `interval` oder `values`, und `interval` und `values` können nicht miteinander kombiniert werden.
- **Hinweis**: Intervall Facetten Zeitpunkt berechnet basierend auf UTC-Zeit, wenn `timeoffset` wurde nicht angegeben. Beispiel: für `facet=lastRenovationDate,interval:day`, am Grenze ab 00:00 Uhr. 

> [AZURE.NOTE] Beim Aufruf der **Suche** mit POST wird dieser Parameter namens `facets` anstelle von `facet`. Außerdem geben Sie es als JSON-Array von Zeichenfolgen, in dem jede Zeichenfolge einen separaten Facetten Ausdruck ist.

`$filter=[string]`(optional) – strukturierte Suchbegriff in OData-Standardsyntax. Einzelheiten Sie [Ausdruckssyntax OData](#ODataExpressionSyntax) auf die Grammatik Ausdruck OData, die Azure unterstützt.

> [AZURE.NOTE] Beim Aufruf der **Suche** mit POST wird dieser Parameter namens `filter` anstelle von `$filter`.

`highlight=[string]`(optional) – markiert eine durch Trennzeichen getrennte Feldnamen Treffer verwendet. Nur `searchable` Felder für Suchtreffer markieren verwendet werden können.

`highlightPreTag=[string]`(optional, standardmäßig `<em>`) - Zeichenfolge Tag voran, um Höhepunkte verwiesen. Eingestellt werden `highlightPostTag`.

> [AZURE.NOTE] Beim **Suchen** mit GET reservierte Zeichen in der URL aufrufen Prozent (z.B. 23 % #) codierte muss werden.

`highlightPostTag=[string]`(optional, standardmäßig `</em>`)-String-Variable, die Höhepunkte verwiesen ergänzen. Eingestellt werden `highlightPreTag`.

> [AZURE.NOTE] Beim **Suchen** mit GET reservierte Zeichen in der URL aufrufen Prozent (z.B. 23 % #) codierte muss werden.

`scoringProfile=[string]`(optional) – der Name des Bewertungsprofil auswerten übereinstimmen Ergebnisse nach übereinstimmenden Dokumente, um die Ergebnisse zu sortieren.

`scoringParameter=[string]`(null oder mehr) - zeigt die Werte für jeden Parameter in einer Punktzahl definiert (z. B. `referencePointParameter`) im Format `name-value1,value2,...`.

- Beispielsweise das Bewertungsprofil definiert eine Funktion mit dem Parameter "meinStandort" Abfragezeichenfolgenoption wäre `&scoringParameter=mylocation--122.2,44.8`. Der erste Strich trennt den Namen aus der Liste während dem zweiten Bindestrich Teil der erste Wert (in diesem Beispiel Längengrad).
- Punktwertung Parameter wie Tags angehoben, die Kommas enthalten können entgeht Sie diese Werte in der Liste mit einfachen Anführungszeichen. Wenn die Werte selbst Apostrophe enthalten, können Sie durch die Verdopplung Escapezeichen versehen.
  - Wenn Sie einen Parameter namens "Mytag" Verstärkung Tag und das Tag erhöhen möchten beispielsweise Werte, "Hello, O'Brien" und "Schmidt", die Abfrage Zeichenfolge Option `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Anführungszeichen sind nur für Werte, die Kommas enthalten.

> [AZURE.NOTE] Beim Aufruf der **Suche** mit POST wird dieser Parameter namens `scoringParameters` anstelle von `scoringParameter`. Geben sie außerdem als JSON-Array von Zeichenfolgen, wobei jede Zeichenfolge eine Separate ist `name-values` Paar.

`minimumCoverage`(optional, standardmäßig 100)-eine Zahl zwischen 0 und 100 die Angabe des Prozentsatzes des Indexes, der eine Suchabfrage, damit die Abfrage als erfolgreich gemeldet werden abgedeckt werden muss. Standardmäßig muss der gesamte Index verfügbar sein oder `Search` HTTP-Statuscode 503 zurückgegeben wird. Festlegen `minimumCoverage` und `Search` erfolgreich, es wird HTTP 200 zurückgeben und einen `@search.coverage` Wert auf die Angabe des Prozentsatzes des Indexes, der in der Abfrage enthalten.

> [AZURE.NOTE] Wenn dieser Parameter auf einen Wert unter 100 hilfreich für schnelle Suche auch für Dienste mit nur einem Replikat. Allerdings sind nicht alle übereinstimmenden Dokumente unbedingt in den Suchergebnissen vorhanden. Wenn Rückruf Suche wichtiger anwendungsspezifische Verfügbarkeit, am besten lassen `minimumCoverage` der Standardwert 100.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

Hinweis: für diesen Vorgang die `api-version` als Abfrageparameter unabhängig davon, ob die **Suche** mit GET oder POST auf URL angegeben.

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `api-key`: Der `api-key` wird zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für die Dienst-URL eindeutig ist. **Die Suchanfrage** können entweder ein Administrator oder Abfrage Taste für `api-key`.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

Für GET: keine.

Post:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Fortsetzung der teilweise Antworten**

Azure Suche kann nicht die gewünschten Ergebnisse manchmal in einer einzelnen Suche Antwort zurück. Dies kann aus verschiedenen Gründen, z. B. wenn die Abfrage zu viele Dokumente anfordert, verzichten Sie `$top` oder einen Wert für `$top` zu groß. In solchen Fällen Azure Suche umfasst das `@odata.nextLink` Anmerkung in den Antworttext und `@search.nextPageParameters` wurde eine POST-Anforderung. Die Werte dieser Anmerkungen können formulieren Sie eine andere Suchabfrage zu den nächsten Teil der Suchantwort. Dies ist eine ***Fortsetzung*** des ursprünglichen Suchanfrage bezeichnet und die Kommentare werden im allgemeinen ***Fortsetzungstoken***bezeichnet. Siehe [Beispiel unten](#SearchResponse) für Details zur Syntax dieser Zeichen und wo sie im Antworttext erscheinen. 

Die Gründe, warum Azure Fortsetzungstoken Suche möglicherweise, sind implementierungsspezifisch und. Robuste Clients sollte immer bereit, Fälle zu behandeln, weniger Dokumente als erwartet zurückgegeben und ein Fortsetzungstoken ist weiterhin Abrufen von Dokumenten enthalten, sein. Beachten Sie, dass Sie dieselbe HTTP-Methode wie die ursprüngliche Anforderung verwenden müssen, um fortzufahren. Wenn eine GET-Anforderung gesendet, Anfragen Fortsetzung senden müssen auch z. B. GET (und für POST).

<a name="SearchResponse"></a>
**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Beispiele:**

Weitere Beispiele finden Sie auf [OData-Ausdruckssyntax Azure suchen](https://msdn.microsoft.com/library/azure/dn798921.aspx) .

1)  Suche Index in absteigender Reihenfolge nach Datum sortiert.


    GET /indexes/hotels/docs? Search = * & $orderby = LastRenovationDate Beschr & API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "*", "Orderby": "LastRenovationDate Desc"}

2)  Eine facettierte Suche im Index und Facets für Kategorien, Bewertung, Tags sowie mit BaseRate in bestimmten Bereichen abrufen:


    GET /indexes/hotels/docs? Search = Test & Facet = Kategorie & Facet = Bewertung & Facet = und Facetten = BaseRate Werte: 80 | 150 | 220 und API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "test", "Facets": ["Kategorie", "Bewertung", "Tags", "BaseRate Werte: 80 | 150 | 220"]}

3)  Mithilfe eines Filters eingrenzen facettierten früheren Abfrageergebnisse nach der Benutzer klickt auf 3 und die Kategorie "Hotel" Bewertung:


    GET /indexes/hotels/docs? Search = Test & Facet = und Facetten = BaseRate Werte: 80 | 150 | 220 & $filter = Bewertung Eq 3 Kategorie Eq "Hotel" und API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "test", "Facets": ["Tags", "BaseRate Werte: 80 | 150 | 220"], "Filter": "Bewertung Eq 3 und Category Eq"Hotel""}

4) Legen Sie eine facettierte Suche eine Obergrenze eindeutige Begriffe in einer Abfrage zurückgegeben. Der Standardwert ist 10 jedoch erhöhen oder verringern Sie diesen Wert mit der `count` Parameter in der `facet` Attribut:


    GET /indexes/hotels/docs? Search = Test & Facet = City, Anzahl: 5 und API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "test", "Facets": ["Stadt, Anzahl: 5"]}

5)  Suchen Sie den Index in bestimmten Feldern. Eine sprachspezifische Feld:


    GET /indexes/hotels/docs? Search = Hotel & Suchfeldern = Description_fr & API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "Hotel", "Suchfeldern": "Description_fr"}

6) Suchen Sie den Index in mehreren Feldern. Sie können z. B. speichern und durchsuchbare Felder in mehreren Sprachen in demselben Index abgefragt.  Beschreibung der englischen und französischen in dasselbe Dokument vorhanden sein, können einige oder alle in den Abfrageergebnissen zurückgegeben werden:


    GET /indexes/hotels/docs? Search = Hotel Suchfeldern = Beschreibung, Description_fr und API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "Hotel", "Suchfeldern": "Beschreibung, Description_fr"}

Beachten Sie, dass jeweils nur ein Index Abfragen kann. Erstellen Sie mehrere Indizes für jede Sprache, wenn Sie nacheinander abfragen möchten.

7)  Paging - Abrufen der 1. Seite (Größe beträgt 10):


    GET /indexes/hotels/docs? Search = * & $skip = 0 & $top = 10 & API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "*", "Überspringen": "Top" 0: 10}

8)  Paging - Abrufen der 2. Seite (Größe beträgt 10):


    GET /indexes/hotels/docs? Search = * & $skip = 10 und $top = 10 & API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "*", "Überspringen": "Top" 10: 10}

9)  Rufen Sie eine bestimmte Gruppe von Feldern:


    GET /indexes/hotels/docs? Search = * & $select = nennen, Beschreibung und API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "*", "select": "nennen, Beschreibung"}

10)  Abrufen von Dokumenten, die einen bestimmten Filterausdruck entsprechen:


    GET /indexes/hotels/docs? $filter = (BaseRate ge 60 und BaseRate 300) oder nennen Eq 'Mit Effekten bleiben' & API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Filter": "(BaseRate ge 60 und BaseRate 300) oder nennen Eq"Mit Effekten bleiben""}

11) Suchen Sie Index und Rückgabe Fragmente mit Hervorhebung von Treffern


    GET /indexes/hotels/docs? suchen etwas = & highlight = Beschreibung & API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "etwas", "highlight": "Beschreibung"}

12) Der Suchindex und Dokumente sortiert näher weiter entfernt einen Verweis aus


    GET /indexes/hotels/docs? suchen etwas = & $orderby=geo.distance (Position, geography'POINT(-122.12315 47.88121)') & API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "etwas", "Orderby": "geo.distance (Position, geography'POINT(-122.12315 47.88121)')"}

13) Vorausgesetzt, es gibt ein Bewertungsprofil namens "Geo" Index mit zwei Abstand Punktzahl suchen, einen Parameter definieren "CurrentLocation" und einen Parameter namens "LastLocation" definieren


    GET /indexes/hotels/docs? suchen etwas = & ScoringProfile = Geo & ScoringParameter = CurrentLocation - 122.123,44.77233 & ScoringParameter = LastLocation - 121.499,44.2113 und API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "etwas", "ScoringProfile": "Geo", "ScoringParameters": ["CurrentLocation - 122.123,44.77233", "LastLocation - 121.499,44.2113"]}

14) Suchen von Dokumenten im Index [einfache](https://msdn.microsoft.com/library/dn798920.aspx)Abfragesyntax. Diese Abfrage gibt Hotels, durchsuchbaren Felder die Begriffe "Komfort" und "Ort" aber nicht "Hotel" enthalten:


    GET /indexes/hotels/docs? Search = Komfort + Ort-Motels & SearchMode = all & API-Version = 2015-02-28-Vorschau

    POST /indexes/hotels/docs/search? API-Version 2015-02-28-Vorschau = {"Suche": "Komfort + Ort-Motels", "SearchMode": "alle"}

Beachten Sie die Verwendung des `searchMode=all` oben. Einschließlich dieser Parameter überschreibt die von `searchMode=any`, Gewährleistung, `-motel` "Und nicht" statt "Oder nicht" bedeutet. Ohne `searchMode=all`, erhalten Sie "OR NOT" Erweitert, anstatt Suchergebnisse beschränkt und kann für einige Benutzer intuitiv.

15) Suchen von Dokumenten im Index [Lucene](https://msdn.microsoft.com/library/mt589323.aspx)Abfragesyntax. Diese Abfrage gibt Hotels, enthält das Kategoriefeld den Begriff "Budget" und alle durchsuchbaren Felder mit dem Ausdruck "renoviert". Dokumente, die den Ausdruck "renoviert" werden durch den Begriff Boost Wert (3) höher eingestuft.

    Abrufen /indexes/hotels/docs?search = Kategorie: Budget und \"renoviert\"^ 3 & SearchMode = all & API-Version 2015-02-28-Vorschau = & Querytype = vollständig

    POST /indexes/hotels/docs/search?api-version 2015-02-28-Vorschau = {"Suche": "Kategorie: Budget und \"renoviert\"^ 3", "QueryType": "voll", "SearchMode": "alle"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>Dokument suchen

**Lookup Dokument** Vorgang ruft ein Dokument von Azure Suche ab. Dies ist nützlich, wenn ein Benutzer auf ein bestimmtes Suchergebnis klickt und Einzelheiten zu diesem Dokument gesucht werden soll.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Anforderung**

HTTPS ist für Serviceanfragen. Die **Suche** Abfrage kann wie folgt erstellt werden.

    GET /indexes/[index name]/docs/key?[query parameters]

Alternativ können Sie für die Suche nach Ressourcenschlüsseln herkömmliche OData-Syntax:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

Anfrage-URI enthält ein [Indexname] und [], welches Dokument zum Abrufen aus der Index angeben. Sie können nur ein Dokument gleichzeitig abrufen. **Suchen** einer Anforderung mehrere Dokumente zu verwenden.

**Abfrageparameter**

`$select=[string]`(optional) – eine Liste von durch Trennzeichen getrennte Felder abzurufen. Wenn nicht angegeben oder `*`, alle Felder im Schema als abrufbar sind in der Projektion enthalten.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

Hinweis: für diesen Vorgang die `api-version` als Abfrageparameter angegeben.

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `api-key`: Der `api-key` wird zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für die Dienst-URL eindeutig ist. Die Abfrage **Nachschlagen** können entweder ein Administrator oder Abfrage Taste für `api-key`.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Beispiel**

Suchen Sie das Dokument mit dem Schlüssel "2"

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Suchen Sie das Dokument mit Schlüssel '3' OData-Syntax verwenden:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Anzahl Dokumente

**Anzahl Dokumente** -Vorgang ruft die Anzahl der Dokumente in ein Index. Die `$count` Syntax ist das OData-Protokoll.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Anforderung**

HTTPS ist für Serviceanfragen. **Anzahl Dokumente** -Anforderung kann mit der GET-Methode erstellt werden.

[Indexname] in der URI-Anforderung weist den Dienst Anzahl aller Elemente in der Auflistung Dokumente des angegebenen Indexes zurück.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader.

- `Accept`: Dieser Wert muss auf `text/plain`.
- `api-key`: Der `api-key` wird zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für die Dienst-URL eindeutig ist. Die **Anzahl der Dokumente** Anforderung können entweder ein Administrator oder Abfrage Taste für `api-key`.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Der Antworttext enthält den Wert als ganze Zahl im nur-Text formatiert.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Vorschläge

Der **Vorschläge** Vorgang Ruft Vorschläge auf teilweise ab. Es wird normalerweise in Suchfelder zur Typeahead-Vorschlägen wie Benutzer Suchbegriffe eingeben.

Vorschlag Anfragen Ziel vorschlagen Zieldokumente so vorgeschlagene Text wiederholt werden, kann Wenn mehrere Kandidaten Dokumente die gleiche Suche Eingabe übereinstimmen. Verwenden Sie `$select` andere Dokumentfelder (einschließlich der Zugriffstaste Dokument) abrufen, damit Sie erkennen können, welches Dokument die Quelle für jeden einzelnen Vorschlag ist.

**Vorschläge** Vorgang wird als GET oder POST-Anforderung ausgegeben.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Verwendung von POST anstelle von GET**

Wenn HTTP GET **Vorschläge** API verwenden aufrufen, müssen Sie darauf achten, dass die Länge des angeforderten URL 8 KB höchstens. Dies ist ausreichend für die meisten Applikationen. Einige Programme führen jedoch sehr umfangreiche Abfragen speziell OData Filterausdrücke. Für diese Anwendung wird mithilfe von HTTP POST besser da größere Filter als abrufen können. Mit wird die Anzahl der Klauseln in einem Filter einschränkender Faktor raw Filterzeichenfolge seit der Limit der Anforderungsgröße Post ca. 16 MB Größe.

> [AZURE.NOTE] Obwohl die Anforderung Bereitstellungsgröße sehr groß ist, können nicht Filterausdrücke beliebig komplex sein. Weitere Informationen über Filter Komplexität Einschränkungen finden Sie unter [OData-Ausdruckssyntax](https://msdn.microsoft.com/library/dn798921.aspx) .

**Anforderung**

HTTPS ist für Serviceanfragen. Die **Vorschläge** Anforderung kann mithilfe der Methoden GET oder POST erstellt werden.

Anfrage-URI Gibt den Namen des Indexes, der Abfrage. Parameter wie den teilweise eingegebenen Suchbegriff in der Abfragezeichenfolge bei GET-Anfragen angegeben werden und in der Anforderung Stelle POST angefordert.

Als bewährte Methode beim Erstellen von GET-Anfragen müssen Sie [URL-Kodierung](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) angegebenen Parametern beim REST-API direkt aufrufen. **Vorschläge** Operationen sind dies:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

URL-Codierung wird nur auf die oben genannten Abfrageparameter empfohlen. Wenn Sie versehentlich URL-Kodierung die gesamte Abfragezeichenfolge (alles nach dem?), Anfragen unterbricht.

Außerdem muss URL-Codierung nur beim Aufrufen der REST-API direkt mit GET. Keine URL-Codierung ist erforderlich, wenn **Vorschläge** POST aufrufen oder [.NET Clientbibliothek](https://msdn.microsoft.com/library/dn951165.aspx)mithilfe der URL-Codierung für Sie behandelt.

**Abfrageparameter**

**Vorschläge** akzeptiert mehrere Parameter, die Abfragekriterien und auch Suchverhalten angeben. Sie bieten diese Parameter in der URL beim **Vorschläge** über abrufen und als JSON-Eigenschaften im Hauptteil Anforderung aufrufen, beim Aufrufen der **Vorschläge** über POST Zeichenfolge Abfragen. Die Syntax für einige Parameter unterscheidet zwischen GET und POST. Diese Unterschiede sind gegebenenfalls unter:

`search=[string]`-der Suchtext Abfragen vorgeschlagen. Mindestens 1 Zeichen und nicht mehr als 100 Zeichen muss sein.

`highlightPreTag=[string]`(optional) – eine Zeichenfolge markieren, um Treffer Suche voran. Eingestellt werden `highlightPostTag`.

> [AZURE.NOTE] Wenn **Vorschläge** haben reservierte Zeichen in der URL aufrufen Prozent (z.B. 23 % #) codierte muss werden.

`highlightPostTag=[string]`(optional) – eine Zeichenfolge markieren, fügt Treffer zu suchen. Eingestellt werden `highlightPreTag`.

> [AZURE.NOTE] Wenn **Vorschläge** haben reservierte Zeichen in der URL aufrufen Prozent (z.B. 23 % #) codierte muss werden.

`suggesterName=[string]`-der Name des Suggester gemäß der `suggesters` -Auflistung, die die Indexdefinition gehört. Ein `suggester` bestimmt, welche Felder für vorgeschlagene Abfragebegriffe gescannt werden. Einzelheiten finden Sie unter [Suggesters](#Suggesters) .

`fuzzy=[boolean]`(optional, Standardeinstellung = False)-Wenn diese API true Vorschläge finden auch im Suchtext ersetzte oder fehlende Zeichen besteht. Und diese optimal in einigen Szenarien kommt es Leistungseinbußen fuzzy Vorschlag sucht langsamer und weitere Ressourcen.

`searchFields=[string]`(optional) – die Liste der durch Trennzeichen getrennte Feldnamen für den angegebenen Text suchen. Zielfelder müssen Vorschläge aktiviert.

`$top=#`(optional, Standardeinstellung = 5)-die Anzahl der Vorschläge abzurufen. Eine Zahl zwischen 1 und 100 muss sein.

> [AZURE.NOTE] Beim Aufrufen von **Vorschlägen** POST heißt dieser Parameter `top` anstelle von `$top`.

`$filter=[string]`(optional) - Ausdruck, der die Dokumente filtert Vorschläge berücksichtigt.

> [AZURE.NOTE] Beim Aufrufen von **Vorschlägen** POST heißt dieser Parameter `filter` anstelle von `$filter`.

`$orderby=[string]`(optional) – eine Liste von Ausdrücken zum Sortieren der Ergebnisse durch Kommas. Jeder Ausdruck kann ein Feld oder einen Aufruf der `geo.distance()` Funktion. Jeder Ausdruck folgt `asc` aufsteigend, gekennzeichnet und `desc` absteigend an. Standardmäßig wird die aufsteigende Reihenfolge. Maximal 32 Klauseln für `$orderby`.

> [AZURE.NOTE] Beim Aufrufen von **Vorschlägen** POST heißt dieser Parameter `orderby` anstelle von `$orderby`.

`$select=[string]`(optional) – eine Liste von durch Trennzeichen getrennte Felder abzurufen. Falls nicht angegeben, wird nur die Schlüssel und Vorschlag Dokumenttext zurückgegeben. Sie können alle Felder explizit anfordern, indem Sie diesen Parameter auf `*`.

> [AZURE.NOTE] Beim Aufrufen von **Vorschlägen** POST heißt dieser Parameter `select` anstelle von `$select`.

`minimumCoverage`(optional, standardmäßig 80)-eine Zahl zwischen 0 und 100 die Angabe des Prozentsatzes des Indexes, der von einer Abfrage Vorschläge, damit die Abfrage als erfolgreich gemeldet werden abgedeckt werden muss. Standardmäßig muss mindestens 80 % des Indexes verfügbar sein oder `Suggest` HTTP-Statuscode 503 zurückgegeben wird. Festlegen `minimumCoverage` und `Suggest` erfolgreich, es wird HTTP 200 zurückgeben und einen `@search.coverage` Wert auf die Angabe des Prozentsatzes des Indexes, der in der Abfrage enthalten.

> [AZURE.NOTE] Wenn dieser Parameter auf einen Wert unter 100 hilfreich für schnelle Suche auch für Dienste mit nur einem Replikat. Allerdings sind nicht alle übereinstimmenden Vorschläge unbedingt in den Ergebnissen. Wenn Rückruf wichtiger anwendungsspezifische Verfügbarkeit, sollten keine niedrigere `minimumCoverage` unter den Standardwert 80.

`api-version=[string]`(erforderlich). Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternative Versionen finden Sie unter [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

Hinweis: für diesen Vorgang die `api-version` als Abfrageparameter in der URL unabhängig davon, ob **Vorschläge** mit GET oder POST aufrufen angegeben.

**Anfrage-Header**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderungsheader

- `api-key`: Der `api-key` wird zum Authentifizieren der Anforderung Suchdienst verwendet. Ein Zeichenfolgenwert für die Dienst-URL eindeutig ist. Die Anforderung **Vorschläge** können entweder ein Administrator oder Abfrage Taste als das `api-key`.

Sie benötigen auch den Namen die angeforderten URL erstellen. Sie erhalten den Namen und `api-key` Service Dashboard im Azure-Portal. Siehe [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anfragetext**

Für GET: keine.

Post:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Wenn die Option Projektion abzurufenden Felder enthalten in jedem Element des Arrays verwendet wird:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Beispiel**

Rufen Sie 5 Vorschläge teilweise suchen ist 'Lux ab'

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
