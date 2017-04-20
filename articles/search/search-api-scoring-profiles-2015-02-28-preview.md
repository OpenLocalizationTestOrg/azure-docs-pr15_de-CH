<properties
    pageTitle="Bewertung der Profile (Azure REST API-Version 2015-02-28-Suchvorschau) | Microsoft Azure | Azure Suchvorschau API"
    description="Azure Suche ist eine gehostete Cloud-Suchdienst, der Optimierung der Ergebnisse basierend auf benutzerdefinierten Punktzahl Profile unterstützt."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Profile (Azure REST API-Version 2015-02-28-Suchvorschau) bewerten

> [AZURE.NOTE] Dieser Artikel beschreibt Punktzahl Profile [2015-02-28-Vorschau](search-api-2015-02-28-preview.md). Derzeit besteht kein Unterschied zwischen dem `2015-02-28` Version auf [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) dokumentiert und `2015-02-28-Preview` beschriebenen Version, aber wir dieses Dokument trotzdem um Dokument über die gesamte API abzudecken.

## <a name="overview"></a>Übersicht

Bewertung bezieht sich auf die Berechnung der suchbewertung für jedes Element in den Suchergebnissen zurückgegeben. Das Ergebnis ist ein Indikator Relevanz eines Elements im Kontext des aktuellen Suchvorgangs. Je höher die Bewertung, desto relevanter Elements. Elemente werden in Suchergebnissen Rang geordnet von hohen, basiert auf der Suche für jedes Element berechnet.

Azure Suche verwendet zum Berechnen einer ersten Bewertung bewerten, aber die Berechnung durch ein Bewertungsprofil anpassen. Punktwertung Profile geben größere Kontrolle über die Positionierung von Elementen in den Suchergebnissen angezeigt. Angenommen, möchten Sie Elemente basierend auf ihren Umsatz steigern, neuere Elemente oder vielleicht steigern Artikeln im Lager zu lang.

Ein Bewertungsprofil ist Teil der Indexdefinition besteht aus Feldern, Funktionen und Parameter.

Geben Sie einen Überblick darüber, wie ein Bewertungsprofil aussieht, werden im folgenden Beispiel wird ein einfaches Profil mit der Bezeichnung "Geo" Diese Elemente mit dem Suchbegriff in steigert die `hotelName` Feld. Außerdem wird das `distance` zugunsten Elemente, die innerhalb von zehn Kilometern der aktuellen Funktion. Wenn jemand nach dem Begriff "Hotel sucht" und "Hotel" ist Teil des Hotels werden Dokumente, die mit"" Hotels enthalten höher in den Suchergebnissen angezeigt.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Verwendung dieser Bewertungsprofil ist Ihre Abfrage formuliert, um das Profil in der Abfragezeichenfolge angegeben. Beachten Sie in der Abfrage den Abfrageparameter `scoringProfile=geo` in der Anforderung.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Diese Abfrage sucht nach dem Begriff "Hotel" und die aktuelle Position übergibt. Beachten Sie, dass diese Abfrage weitere Parameter, z. B. enthält `scoringParameter`. Parameter werden in [Dokumente durchsuchen (Azure Such-API)](search-api-2015-02-28-preview.md#SearchDocs)beschrieben.

Klicken Sie auf [Beispiel](#example) Ein ausführlicheres Beispiel ein Bewertungsprofil überprüfen.

## <a name="what-is-default-scoring"></a>Was ist standardmäßig bewerten?

Bewertung berechnet eine suchbewertung für jedes Element im Rang sortierten Resultset. Jedes Element in einem Suchergebnissatz Suche Bewertung zugewiesen und höchsten zum niedrigsten Rang. Elemente mit höheren Werten werden an die Anwendung zurückgegeben. Standardmäßig Top 50 zurückgegeben werden, aber Sie können die `$top` -Parameter auf eine kleinere oder größere Anzahl von Elementen (bis zu 1000 in einer einzelnen Antwort) zurückzugeben.

Standardmäßig wird eine suchbewertung statistische Eigenschaften der Daten und der Abfrage berechnet. Azure Suche sucht nach Dokumenten, die in der Abfragezeichenfolge Suchbegriffe enthalten (einige oder alle, je nach `searchMode`), vorzugsweise Dokumente, viele Instanzen des Suchbegriffs enthalten. Suche Ergebnis geht weiter, wenn der Begriff selten über Corpus Daten aber im Dokument. Die Grundlage dafür computing Relevanz TF IDF genannt oder (Begriff Häufigkeit Inverse Document Frequency).

Angenommen es kein benutzerdefiniertes Sortieren, Ergebnisse dann mit Suche rangieren bevor sie an die aufrufende Anwendung zurückgegeben werden. Wenn `$top` nicht angegeben wird, 50 Artikel der höchsten Suche Ergebnis zurückgegeben.

Bewertung von Suchwerten können in einem Resultset wiederholt werden. Z. B. möglicherweise 10 Elemente mit 1.2 20 mit 1.0 und 20 mit 0,5. Bei mehreren Treffern suchen Punktegleichstand dieselben bewertete Elemente sortieren nicht definiert und ist nicht stabil. Ausführen der Abfrage, und Sie sehen Elemente UMSCHALT-Position. Zwei Elemente mit identischen Wert angegeben, Sie besteht keine Garantie, welches zuerst angezeigt wird.

## <a name="when-to-use-custom-scoring"></a>Verwendung von benutzerdefinierten bewerten

Erstellen Sie Profile bewertet, wenn ranking Verhalten standardmäßig nicht weit genug in Erfüllung Ihrer geschäftlichen Ziele. Sie könnten beispielsweise, Suche Relevanz hinzugefügten Elemente zuerst. Ebenso müssen Sie ein Feld mit Gewinn oder ein anderes Feld Umsatzpotenzial angibt. Zugriffe, die Vorteile für Ihr Unternehmen steigern kann ein wichtiger Faktor bei der Entscheidung bewertet Profile verwenden.

Relevanz basierenden Sortierung erfolgt auch über Profile bewerten. Betrachten Sie Suchergebnisseiten verwendeten in der Vergangenheit, mit denen Sie nach Preis, Datum, Bewertung oder Relevanz sortieren können. In Azure Search Laufwerk Punktzahl Profile "Erheblichkeit"-Option. Die Definition der Relevanz wird Sie basiert auf Unternehmensziele und den Typ der Suchergebnisse zu liefern.

<a name="example"></a>
## <a name="example"></a>Beispiel

Durch Bewertung in einem IndexSchema definierte Profile angepassten Bewertung wie implementiert.

Dieses Beispiel zeigt das Schema eines Indexes mit zwei Punktzahl (`boostGenre`, `newAndHighlyRated`). Jede Abfrage, die entweder Profil enthält z. B. ein Abfrageparameter Profil erzielen Resultset Index.

[Dieses Beispiel](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Workflow

Um benutzerdefinierte scoring Verhalten implementieren, fügen Sie ein Bewertungsprofil an der Index definiert. Mehrere Werte Profile in einem Index haben, aber Sie können nur ein Profil in einer Abfrage angegebenen Zeitpunkt.

Beginnen Sie mit der [Vorlage](#bkmk_template) in diesem Thema.

Geben Sie einen Namen ein. Punktwertung Profile sind optional, aber der Name ist erforderlich, wenn eine hinzufügen. Achten Sie darauf, die Namenskonventionen für Felder (beginnt mit einem Buchstaben vermeidet Sonderzeichen und reservierte Wörter). Weitere Informationen finden Sie unter [Namenskonventionen](http://msdn.microsoft.com/library/azure/dn857353.aspx) .

Der Hauptteil der Bewertungsprofil ist aus gewichteten Felder und Funktionen erstellt.

### <a name="weights"></a>Gewicht ###

Die `weights` -Eigenschaft ein Bewertungsprofil gibt Name-Wert-Paaren, die ein Feld eine relative Gewichtung zugewiesen. Im [Beispiel](#example)werden die Felder AlbumTitle, Genre und artist stärkere 1,5, 5 und 2. Warum ist viel höher als die anderen Genre erhöht? Wenn Daten gesucht wird, die homogen ist (wie bei "Genre" "in der `musicstoreindex`), benötigen Sie eine größere Varianz in der relativen Gewichtung. In der `musicstoreindex`, "Rock" als ein Genre und Genre identisch formulierte Beschreibung angezeigt. Genre Genre Beschreibung überwiegen wird, Feld Genre eine höhere relative Gewichtung sollten.

### <a name="functions"></a>Funktionen ###

Funktionen werden verwendet, wenn zusätzliche Berechnungen für bestimmte Kontexte erforderlich sind. Gültige Funktion sind `freshness`, `magnitude`, `distance` und `tag`. Jede Funktion hat Parameter einzigartig.

  - `freshness`sollte verwendet werden, wenn Sie steigern möchten wie neue oder alte ein Element ist. Diese Funktion kann nur mit Datetime-Felder verwendet werden (`Edm.DataTimeOffset`). Hinweis Die `boostingDuration` -Attribut wird nur für frische-Funktion verwendet.
  - `magnitude`sollte verwendet werden, zu basierend auf wie hoch oder Niedrig ein numerischer Wert ist. Die für diese Funktion aufrufen Szenarien Gewinnspanne, Höchstpreis, Preis und Anzahl der Downloads erhöhen. Bereich, hoch, Niedrig, storniert werden können ggf. das inverse Muster (z. B. kostengünstige Elementen teureren Elemente mehr). Preise von $100 $1 gegeben, legen Sie `boostingRangeStart` 100 und `boostingRangeEnd` 1 zu günstigeren Elemente. Diese Funktion kann nur Double- und Integer-Felder verwendet werden.
  - `distance`sollte verwendet werden, um Nähe oder geographischen steigern möchten. Diese Funktion kann nur mit `Edm.GeographyPoint` Felder.
  - `tag`sollte verwendet werden, um Tags gemeinsam Dokumente und Suchabfragen erhöhen möchten. Diese Funktion kann nur mit `Edm.String` und `Collection(Edm.String)` Felder.
  
#### <a name="rules-for-using-functions"></a>Regeln für die Verwendung von Funktionen ####

  - Funktion muss (frische, Größe, Abstand, Tag) klein sein.
  - Funktionen können nicht null oder leere Werte enthalten. Insbesondere enthält Fieldname Ihnen so eingestellt.
  - Funktionen können nur in filterbaren Felder angewendet werden. Weitere Informationen zu Filtern finden Sie unter [Create Index](search-api-2015-02-28-preview.md#CreateIndex) .
  - Funktionen können nur auf Felder angewendet werden, die in der Fields-Auflistung eines Index definiert sind.

Nachdem der Index definiert ist, erstellen Sie den Index IndexSchema gefolgt von Dokumente hochladen. Weitere Informationen zu diesen Vorgängen finden Sie unter [Create Index](search-api-2015-02-28-preview.md#CreateIndex) und das [Update Dokumente](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) . Nachdem der Index erstellt ist, haben Sie eine funktionale Bewertungsprofil, die mit Ihrer Suche Daten.

<a name="bkmk_template"></a>
## <a name="template"></a>Vorlage
Dieser Abschnitt zeigt die Syntax und die Vorlage zur Bewertung der Profile. Verweisen Sie [auf Index](#bkmk_indexref) im nächsten Abschnitt eine Beschreibung der Attribute.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Referenz für Profil bewerten

**Hinweis** Eine Punktzahl Funktion kann nur auf Felder angewendet werden, die gefiltert werden.

| Eigenschaft | Beschreibung |
|----------|-------------|
| `name`   | Erforderlich. Dies ist der Name der Bewertungsprofil. Es folgt die gleichen Namenskonventionen eines Felds. Sie müssen mit einem Buchstaben beginnen, darf keine Punkte, Doppelpunkte enthalten oder @ Symbole und nicht mit dem Ausdruck "AzureSearch" (Groß-/Kleinschreibung) beginnen. |
| `text` | Die Gewichte-Eigenschaft enthält. |
| `weights` | Optional. Ein Name-Wert-Paar, das einen Feldnamen und Gewichtung angibt. Gewichtung muss eine positive ganze Zahl oder eine Gleitkommazahl sein. Sie können den Feldnamen ohne eine entsprechende Gewichtung. Gewicht wird die Bedeutung einer relativ zu einem anderen an. |
| `functions` | Optional. Beachten Sie, dass eine Punktzahl Funktion nur auf Felder angewendet werden kann, die gefiltert werden. |
| `type` | Zur Bewertung der Funktionen erforderlich. Gibt den Typ der Funktion. Gültige Werte sind `magnitude`, `freshness`, `distance` und `tag`. Sie können mehrere Funktionen in jeder Bewertungsprofil einschließen. Der Funktionsname muss klein sein. |
| `boost` | Zur Bewertung der Funktionen erforderlich. Eine positive Zahl als Multiplikator für Rohwert verwendet. Gleich 1 darf nicht sein. |
| `fieldName` | Zur Bewertung der Funktionen erforderlich. Eine Punktzahl Funktion kann nur auf Felder angewendet, Teil der feldauflistung des Indexes und gefiltert werden. Darüber hinaus führt jede Funktion Einschränkungen (frische mit Datetime-Felder Betrag mit Integer oder double Felder und mit Suchfelder mit Zeichenfolge oder Auflistung Zeichenfolgenfelder verwendet). Sie können nur ein einzelnes Feld pro Funktionsdefinition. Beispielsweise um Betrag zweimal in demselben Profil verwenden, müssten Sie zwei Definitionen Größe für jedes Feld enthalten. |
| `interpolation` | Zur Bewertung der Funktionen erforderlich. Definiert die Steigung für die Bewertung Verstärkung erhöht sich von Anfang an bis zum Ende des Bereichs. Gültige Werte sind `linear` (Standard), `constant`, `quadratic`, und `logarithmic`. Einzelheiten finden Sie unter [Festlegen der Interpolation](#bkmk_interpolation) . |
| `magnitude` | Bewertung Funktion Betrag wird verwendet, um basierend auf den Werten für ein numerisches Feld Platzierung ändern. Die häufigste Verwendungsbeispiele hierfür sind:<ul><li>Bewertungssterne: Bewertung basierend auf dem Wert im Feld "Star Rating" ändern. Wenn zwei Elemente sind, wird das Element mit der höheren Bewertung zuerst angezeigt.</li><li>Rand: Wenn zwei relevanten Dokumenten ein Einzelhändler können zu Dokumenten, die höhere Margen zuerst.</li><li>Klicken Sie auf Zähler: Applikationen verfolgen Aktionen zu Produkten oder Seiten klicken, können Sie Größe zu Elementen, die den Datenverkehr werden.</li><li>Downloads: Anwendungsmöglichkeiten, Elemente verfolgen Downloads Betrag-Funktion können Sie erhöhen, haben Sie die Downloads.</li></ul> |
| `magnitude:boostingRangeStart` | Den Startwert des Bereichs die Größe erzielt wird. Der Wert muss eine ganze Zahl oder eine Gleitkommazahl sein. Bewertungssterne 1 bis 4 wäre dies 1. Ränder über 50 % wäre 50. |
| `magnitude:boostingRangeEnd` | Den Endwert des Bereichs die Größe erzielt wird. Der Wert muss eine ganze Zahl oder eine Gleitkommazahl sein. Bewertungssterne 1 bis 4 wäre dies 4. |
| `magnitude:constantBoostBeyondRange` | Gültige Werte sind True oder False (Standard). Wenn true, vollständige Boost weiterhin auf Dokumente angewendet, die einen Wert für das Zielfeld, die höher ist als das obere Ende des Bereichs. False, wird Verstärkung dieser Funktion zu Dokumenten mit dem Wert für das Zielfeld des Bereichs außerhalb angewendet werden. |
| `freshness` | Frische Bewertung Funktion dient zum Ranking Faktoren für Elemente basierend auf Werten in DateTimeOffset Felder ändern. Beispielsweise kann ein Element mit einem neueren höher als ältere Objekte eingestuft. (Beachten Sie ist es auch möglich, Rang Elemente wie Kalenderereignisse mit einem zukünftigen Datum, Artikel näher am aktuellen zukünftig weitere höher Elemente eingestuft werden.) In den aktuellen Service Release wird derzeit das Bereichsende fest. Das andere Ende wird in der Vergangenheit auf der Grundlage der `boostingDuration`. Zu verwenden, ein Zeitbereich zukünftig eine Negative `boostingDuration`. Die Rate, mit der Förderung von maximal ändert und Mindestbereich anhand der Interpolation, auf das Bewertungsprofil angewendet (siehe Abbildung unten). Wählen Sie zum Umkehren des Steigerung Faktors angewendet Boost Faktor kleiner als 1. |
| `freshness:boostingDuration` | Legt fest, nach der Förderung eine Gültigkeitsdauer für ein bestimmtes Dokument beendet wird. Siehe [Set BoostingDuration] [#Bkmk_boostdur] im Abschnitt Syntax und Beispiele. |
| `distance` | Abstand Punktzahl Funktion verwendet wird, auf die Bewertung der Dokumente wie schließen oder weit sie gegenüber einem geografischen Referenzspeicherort. Referenzposition erhält als Teil der Abfrage einen Parameter (mit den `scoringParameter` Abfrage Parameter) als ein Lon Lat-Argument. |
| `distance:referencePointParameter` | Ein Parameter in Abfragen als Referenzposition verwendet übergeben werden. ScoringParameter ist eine Abfrage. Eine Beschreibung der Parameter finden Sie unter [Dokumente durchsuchen](search-api-2015-02-28-preview.md#SearchDocs) . |
| `distance:boostingDistance` | Eine Zahl, die Entfernung in Kilometern von Referenzposition angibt, Steigerung Bereich endet. |
| `tag` | Bewertung Funktion Tag wird auf die Bewertung der Dokumente anhand von Tags in Dokumenten und Suchabfragen verwendet. Dokumente mit Tags mit der Abfrage erhöht werden. Die Tags für die Suchabfrage dient als Punktzahl Parameter in jeder Suchanfrage (mit dem `scoringParameter` Abfrage Parameter). |
| `tag:tagsParameter` | Ein Parameter in Abfragen an Tags für eine bestimmte Anforderung übergeben werden. `scoringParameter`ist ein Abfrageparameter. Eine Beschreibung der Parameter finden Sie unter [Dokumente durchsuchen](search-api-2015-02-28-preview.md#SearchDocs) . |
| `functionAggregation` | Optional. Gilt nur bei Funktionen angegeben werden. Gültige Werte sind: `sum` (Standard), `average`, `minimum`, `maximum`, und `firstMatching`. Suche Ergebnis ist ein einzelner Wert, der aus mehreren Variablen einschließlich mehrere Funktionen berechnet wird. Diese Attribute gibt an, wie steigert die Funktionen in einer einzelnen aggregate steigern kombiniert werden, die dann zum Ergebnis Basisdokument angewendet wird. Die Bewertung basiert auf Tf Idf Wert berechnet aus dem Dokument und der Abfrage. |
| `defaultScoringProfile` | Beim Ausführen einer Suchabfrage keine Bewertungsprofil angegeben ist Standard Bewertung verwendet (Tf-Idf nur). Bewertung Profilname Standard kann hier verursacht Azure Suche auf dieses Profil verwendet, wenn kein bestimmtes Profil in der Suchanfrage angegeben festgelegt werden. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Set Interpolationen

Interpolationen können Sie die Steigung für die Bewertung Verstärkung erhöht sich von Anfang an bis zum Ende des Bereichs zu definieren. Die folgenden Interpolationen können verwendet werden:

  - `Linear`: Für Max und min Bereich Elemente auf das Element angewendeten steigern ständig Abnahme erfolgt in. Die standardmäßige Interpolation ein Bewertungsprofil ist linear.
  - `Constant`Für Elemente, die in den Anfangs- und Endbereich Konstante Verstärkung Rank Ergebnisse gelten.
  - `Quadratic`: Im Vergleich zu eine lineare Interpolation, die ständig abnehmenden steigern, quadratisch anfänglich kleinere Tempo verringert und dann dem Bereichsende nähert, vermindert deutlich Intervallen. Diese Interpolationsoption ist Tag bewerten Funktionen nicht zulässig.
  - `Logarithmic`: Gegenüber eine lineare Interpolation, die ständig abnehmenden steigern, logarithmische wird anfänglich mit höherer Geschwindigkeit verringern und dann dem Bereichsende nähert, vermindert Intervallen viel kleiner. Diese Interpolationsoption ist Tag bewerten Funktionen nicht zulässig.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>BoostingDuration festlegen

`boostingDuration`ist ein Attribut der frische-Funktion. Können sie ein Ablaufdatum festlegen Periode nach der Verstärkung für ein bestimmtes Dokument beendet wird. Förderung einer Produktlinie oder Marke für 10 Tage Werbung, angeben Sie z. B. die 10 Tage als "P10D" für diese Dokumente. Oder zu Veranstaltungen in der nächsten Woche "-P7D".

`boostingDuration`muss als XSD-Wert "DayTimeDuration" (eine eingeschränkte Teilmenge der Dauer ein ISO 8601) formatiert werden. Dieses Muster ist: `[-]P[nD][T[nH][nM][nS]]`.

Die folgende Tabelle enthält Beispiele.

| Dauer | boostingDuration |
|----------|------------------|
| 1 Tag | "P1D" |
| und 12 Stunden | "P2DT12H" |
| 15 Minuten | "PT15M" |
| 30 Tage, 5 Stunden, 10 Minuten und 6.334 Sekunden | "P30DT5H10M6.334S" |

Weitere Beispiele finden Sie unter [XML Schema: Datatypes (W3.org-Website)](http://www.w3.org/TR/xmlschema11-2/).

**Siehe auch**
[Azure Search Service REST-API](http://msdn.microsoft.com/library/azure/dn798935.aspx) auf MSDN <br/>
Auf der MSDN- [Index (Azure Such-API) erstellen](http://msdn.microsoft.com/library/azure/dn798941.aspx)<br/>
[Bewertungsprofil einen Suchindex hinzufügen](http://msdn.microsoft.com/library/azure/dn798928.aspx) auf MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
