<properties 
    pageTitle="Arbeiten mit Geodaten in Azure DocumentDB | Microsoft Azure" 
    description="Erstellen, index und Abfrage räumliche Objekte mit Azure DocumentDB verstehen." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Arbeiten mit Geodaten in Azure DocumentDB

Dieser Artikel ist eine Einführung in die Funktionen Geodaten in [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Nach dem Lesen, werden Sie die folgenden Fragen beantworten:

- Wie speichere ich Geodaten in Azure DocumentDB?
- Wie kann ich Geodaten in Azure DocumentDB in SQL und LINQ-Abfragen?
- Wie aktivieren oder deaktivieren Sie die räumliche Indizierung in DocumentDB?

Dieses [Github Projekt](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) Codebeispiele anzeigen

## <a name="introduction-to-spatial-data"></a>Einführung in die räumliche Daten

Räumliche Daten beschreibt die Position und Form von Objekten im Raum. In den meisten Programmen entsprechen diese Objekte auf der Erde, d. h. Geodaten. Räumliche Daten können verwendet werden, um die Position einer Person, eine oder den Rand eine Stadt oder einen See. Einsatzbereiche umfassen häufig Nähe Abfragen "gefunden werden z. B. alle Cafés meine aktuelle Position". 

### <a name="geojson"></a>GeoJSON
DocumentDB unterstützt die Indizierung und Abfrage der geografische Daten, die der [Spezifikation GeoJSON](http://geojson.org/geojson-spec.html)dargestellt wird. GeoJSON Datenstrukturen sind immer gültige JSON-Objekten gespeichert werden können und mit DocumentDB ohne spezielle Tools Bibliotheken abgefragt. DocumentDB-SDKs bieten Klassen und Methoden, die Arbeit mit räumlichen Daten erleichtern. 

### <a name="points-linestrings-and-polygons"></a>Punkte, Linestrings und Polygone
Ein **Punkt** bezeichnet eine einzelne Position im Raum. In Geodaten stellt ein Punkt den genauen Speicherort die Adresse ein, einem Kiosk, ein Auto oder eine Stadt.  Ein Punkt wird in GeoJSON (und DocumentDB) die Koordinate Paar oder Längen- und dargestellt. Hier ist ein Beispiel JSON für einen Punkt.

**Punkte im DocumentDB**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] GeoJSON-Spezifikation gibt Länge erste und Latitude zweite. Wie in anderen Applikationen Zuordnung Längen- und Winkel und in Grad dargestellt. Länge vom Nullmeridian gemessen und zwischen-180 und 180,0 Grad und Latitude vom Äquator gemessen und zwischen-90.0 und 90,0 Grad. 
>
> DocumentDB interpretiert Koordinaten pro Referenzsystem WGS 84 dargestellt. Finden Sie unter Weitere Details zu koordinieren Systeme.

Dies kann in einem Dokument DocumentDB, wie in diesem Beispiel ein Benutzerprofil mit Daten eingebettet werden:

**Verwenden Sie Profil mit gespeichert in DocumentDB**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Neben unterstützt GeoJSON auch LineStrings und Polygone. **LineStrings** stellen eine Reihe von zwei oder mehr Punkte und Liniensegmente, die sie verbinden. In Geodaten werden Linestrings häufig Straßen oder Flüsse verwendet. Ein **Polygon** ist eine Grenze von miteinander verbundenen Punkten, die ein geschlossenes LineString bildet. Polygone sind meist natürlichen formationen wie Seen oder politischen Ländern Städte und Staaten darstellen. Hier ist ein Beispiel für ein Polygon in DocumentDB. 

**Polygone in DocumentDB**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] GeoJSON-Spezifikation erfordert, dass für gültige Polygone die letzten bereitgestellten Koordinaten der ersten ein geschlossenes Shape erstellen identisch sein sollte.
>
>Punkte innerhalb eines Polygons müssen gegen den Uhrzeigersinn nacheinander angegeben werden. Ein Polygon im Uhrzeigersinn angegeben stellt die Umkehrung der Bereich.

Zusätzlich zeigen LineString und Polygon gibt GeoJSON die Darstellung für mehrere geografische Standorte gruppiert, sowie wie Geolocation als **Funktion**beliebige Eigenschaften zugeordnet. Da diese Objekte gültigen JSON können alle werden gespeichert und in DocumentDB verarbeitet. DocumentDB unterstützt jedoch nur die automatische Indizierung der Punkte.

### <a name="coordinate-reference-systems"></a>Referenz-Koordinatensysteme

Da die Form der Erde unregelmäßig ist, werden Koordinaten von Geodaten in vielen-Koordinate Verweis (CRS) mit Bezugsrahmen und Maßeinheiten dargestellt. Beispielsweise "Nationalen Raster britischen" ist ein sehr genau für das Vereinigte Königreich, aber nicht außerhalb. 

Die beliebtesten CRS verwendet ist heute World Geodetic System [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). GPS-Geräten und viele Zuordnungsdienste Karte sowie Bing Maps APIs verwenden WGS 84. DocumentDB unterstützt die Indizierung und Abfrage von CRS WGS 84 nur mit Geodaten. 

## <a name="creating-documents-with-spatial-data"></a>Erstellen von Dokumenten mit räumlichen Daten
Wenn Sie Dokumente, die GeoJSON enthalten erstellen, werden sie automatisch mit einem räumlichen Index gemäß der Richtlinie Indizierung der Auflistung indiziert. Wenn Sie eine DocumentDB SDK eine dynamisch typisierte Sprache wie Python oder Node.js arbeiten, müssen Sie gültige GeoJSON erstellen.

**Dokument mit Geodaten in Node.js erstellen**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

Wenn Sie mit .NET (oder Java) SDKs arbeiten, können neuen Punkt und Polygon Klassen im Microsoft.Azure.Documents.Spatial-Namespace Sie Informationen in die Anwendungsobjekte einbetten. Diese Klassen vereinfachen die Serialisierung und Deserialisierung von Geodaten in GeoJSON.

**Dokument mit Geodaten in .NET erstellen**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Wenn Sie nicht Informationen Breiten- und Längengrad, aber die Adressen Standortnamen wie Ort oder Land oder, können Sie die aktuellen Koordinaten mithilfe eines Diensts Geocoding wie Bing Maps REST nachschlagen. Bing Maps Geocoding erfahren Sie [hier](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Räumliche Datentypen Abfragen

Jetzt, dass es sich wie Geodaten aufgenommen haben, werfen einen Blick auf diese Daten mit DocumentDB mit SQL und LINQ-Abfragen.

### <a name="spatial-sql-built-in-functions"></a>Räumliche integrierte SQL-Funktionen
DocumentDB unterstützt die folgenden Open Geospatial Consortium (OGC) integrierten Funktionen für Geodaten Abfragen. Weitere Informationen auf die integrierten Funktionen in der SQL-Sprache finden Sie [Abfrage DocumentDB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Verwendung</strong></td>
  <td><strong>Beschreibung</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (Point_expr, Point_expr)</td>
  <td>Gibt den Abstand zwischen den beiden GeoJSON Punkt Ausdrücken.</td>
</tr>
<tr>
  <td>ST_WITHIN (Point_expr, Polygon_expr)</td>
  <td>Gibt einen booleschen Ausdruck, ob im ersten Argument angegebene GeoJSON Punkt innerhalb des Polygons GeoJSON im zweiten Argument ist.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Gibt einen booleschen Wert, der angibt, ob der angegebene GeoJSON Punkt oder Polygon Ausdruck gültig ist.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Gibt eine JSON mit einem booleschen Wert Wenn der angegebene GeoJSON Punkt oder Polygon Ausdruck gültig ist und ungültige, außerdem der Grund als Zeichenfolgenwert.</td>
</tr>
</table>

Räumliche Funktionen dienen Nähe räumliche Daten abgefragt. Hier ist z. B. eine Abfrage, die alle Familie Dokumente zurückgibt, die innerhalb von 30 km von der angegebenen Position mit der integrierten ST_DISTANCE-Funktion. 

**Abfrage**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]

Enthält der räumliche Indizierung die Indizierung Richtlinie werden "Abstand Abfragen" effizient über den Index bereitgestellt. Weitere Informationen über räumliche Indizierung finden Sie im Abschnitt unten. Haben Sie einen räumlichen Index für die angegebenen Pfade, Sie können weiterhin raumbezogener Abfragen von ausführen `x-ms-documentdb-query-enable-scan` Anfrage-Header mit dem Wert "true". In .NET kann dies durch das optionale Argument **FeedOptions** Abfragen mit [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) auf True übergeben. 

ST_WITHIN kann verwendet werden, zu überprüfen, ob ein Punkt innerhalb eines Polygons liegt. Polygone dienen häufig Grenzen wie Postleitzahlen, Grenzen oder natürliche darstellen. Wieder enthält der räumliche Indizierung die Indizierung Richtlinie dann "in" Abfragen effizient über den Index servieren. 

Polygon Argumente im ST_WITHIN darf nur ein einziges Rufzeichen, Polygone müssen also keine Löcher. 

**Abfrage**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Ähnlich wie nicht übereinstimmende Typen arbeiten DocumentDB Abfrage, wenn der Speicherort im angegebene Argument ist fehlerhaft oder ungültig und bewertet **nicht definiert** und bewertet Dokument aus den Abfrageergebnissen übersprungen werden. Wenn die Abfrage keine Ergebnisse zurückgibt, ST_ISVALIDDETAILED ausführen, Debuggen, warum Spatail ist ungültig.     

DocumentDB unterstützt auch umgekehrte Abfragen, also können index Polygone oder Zeilen in DocumentDB Sie Abfragen für die Bereiche, die einen angegebenen Punkt enthalten. Dieses Muster wird in der Logistik meist identifizieren, z.B. ein LKW eingibt oder einen bestimmten Bereich verlässt. 

**Abfrage**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Ergebnisse**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID und ST_ISVALIDDETAILED kann verwendet werden, zu überprüfen, ob ein räumliches Objekt gültig ist. Die folgende Abfrage überprüft beispielsweise die Gültigkeit eines Punkts mit einem Latitude Bereichswert (-132.8). ST_ISVALID gibt einen booleschen Wert zurück und ST_ISVALIDDETAILED gibt den booleschen Wert und eine Zeichenfolge mit der Grund, warum er ungültig ist.

**Abfrage**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Ergebnisse**

    [{
      "$1": false
    }]

Diese Funktionen können auch verwendet werden, Polygone zu überprüfen. Beispielsweise verwenden Sie hier wir ST_ISVALIDDETAILED ein Polygon überprüfen, die nicht geschlossen ist. 

**Abfrage**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Ergebnisse**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>LINQ-Abfragen im .NET SDK

DocumentDB .NET SDK auch Anbieter stub-Methoden `Distance()` und `Within()` für die Verwendung in LINQ-Ausdrücke. DocumentDB-LINQ-Anbieter übersetzt diese Methodenaufrufe in die entsprechende integrierte SQL-Funktionsaufrufe (ST_DISTANCE und ST_WITHIN bzw.). 

Hier ist ein Beispiel für eine LINQ-Abfrage, die alle Dokumente in der DocumentDB-Auflistung, deren Wert "Location" ist innerhalb eines Radius von 30km des angegebenen zeigen mit LINQ, findet.

**LINQ-Abfrage für Abstand**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Dies ist auch eine Abfrage Suche alle Dokumente, die im angegebenen Feld/Polygon, deren "Location" ist. 

**LINQ-Abfragen für in**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Nun, wir wie Dokumente mithilfe von LINQ und SQL Abfrage aufgenommen haben, betrachten einer DocumentDB für die räumliche Indizierung konfigurieren.

## <a name="indexing"></a>Indizierung

Wie wir das [Schema unabhängig Indizierung mit Azure DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) Papier beschrieben, entwickelt wir DocumentDB Datenbank-Engine zu wirklich Schema unabhängig und JSON erster Klasse unterstützen. Das Datenbankmodul optimiert Schreiben von DocumentDB versteht nativ räumliche Daten (Punkte, Polygone und Linien) GeoJSON-Standard.

Kurz gesagt, Geometrie vom geodätischen Koordinaten auf einer 2D-Ebene wird schrittweise in Zellen mit **Quadtree**unterteilt. Diese Zellen werden basierend auf der Position der Zelle innerhalb einer **Hilbert Platz ausfüllen Kurve**mit Locality Punkte 1D zugeordnet. Außerdem bei indizierten Daten erfolgt über so **Tesselierung**, d. h. alle Zellen, die eine Position schneiden identifiziert und als Schlüssel im Index DocumentDB gespeichert. Zeitpunkt der Abfrage sind Argumente wie Punkten und Polygonen auch zum Extrahieren der relevanten ID Zellbereiche tesselierten und zum Abrufen von Daten aus dem Index.

Wenn Sie eine Indizierung Richtlinie angeben, die räumlichen Index enthält / * (alle Pfade) und effiziente räumliche Abfragen (ST_WITHIN und ST_DISTANCE) alle Punkte in der Auflistung indiziert sind. Räumliche Indizes nicht Genauigkeit Wert, und verwenden Sie immer einen Standardwert für die Genauigkeit.

>[AZURE.NOTE] DocumentDB unterstützt automatische Indizierung Punkten und Polygonen LineStrings

Folgende JSON-Ausschnitt zeigt eine Indizierung und räumliche Indizierung aktiviert, d. h. index beliebigen GeoJSON in Dokumenten für räumliche Abfragen gefunden. Wenn Sie die Volltextindizierung Richtlinie mithilfe von Azure-Portal ändern, können Sie folgende JSON für Volltextindizierung Richtlinie ermöglichen räumliche Indizierung der Auflistung angeben.

**Auflistung indizieren Richtlinie JSON mit räumlichen Punkten und Polygonen aktiviert**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Hier ist ein Codeausschnitt in .NET die Verwendung zum Erstellen einer Auflistung mit räumliche Indizierung für alle Pfade mit aktiviert. 

**Erstellen Sie eine Auflistung mit räumliche Indizierung**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

Und wie ändern eine vorhandene Auflistung um räumliche Indizierung über alle Punkte, die in Dateien gespeichert sind.

**Ändern einer vorhandenen Auflistung mit räumliche Indizierung**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] Wenn GeoJSON Lage im Dokument fehlerhaft oder ungültig ist, wird dann es nicht für räumliche Abfragen indiziert. Sie können Werte für ST_ISVALID mit ST_ISVALIDDETAILED überprüfen.
>
> Enthält die Auflistung Definition Partitionsschlüssel ist Indizierung Transformation Fortschritt nicht gemeldet. 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie zum Einstieg mit Geodaten Unterstützung in DocumentDB gelernt haben, können Sie:

- Codieren Sie mit [Geodaten .NET Codebeispiele für Github](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
- Hol Geodaten Abfragen [DocumentDB Abfrage Spielplatz](http://www.documentdb.com/sql/demo#geospatial)
- Erfahren Sie mehr über [DocumentDB-Abfrage](documentdb-sql-query.md)
- Erfahren Sie mehr über [DocumentDB Indizierung](documentdb-indexing-policies.md)
