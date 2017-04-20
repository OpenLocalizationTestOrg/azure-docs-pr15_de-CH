<properties 
    pageTitle="Partitionierung und Skalierung in Azure DocumentDB | Microsoft Azure"      
    description="Erfahren Sie wie Partitionierung funktioniert in Azure DocumentDB Partitionierung konfigurieren und partitionieren Schlüssel und wie den richtige Partitionsschlüssel für Ihre Anwendung."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Partitionierung und Skalierung in Azure DocumentDB
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) soll Ihnen schnelle, zuverlässige Performance und Skalierung nahtlos mit der Anwendung wächst. Dieser Artikel bietet einen Überblick wie Partitionierung in DocumentDB und beschreibt die Konfiguration DocumentDB Sammlungen Anwendung effektiv skalieren.

Nach dem Lesen dieses Artikels werden Sie folgenden Fragen beantworten:   

- Wie funktioniert der Partitionierung in Azure DocumentDB?
- Wie konfiguriere ich die Partitionierung in DocumentDB
- Was sind Partitionsschlüssel und wie wähle ich den richtige Partitionsschlüssel für meine Anwendung?

Zunächst mit Code Laden Sie das Projekt aus [DocumentDB Leistung testen Treiber](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

## <a name="partitioning-in-documentdb"></a>DocumentDB Partitionierung

In DocumentDB speichern und Abfragen mit weniger Schemata JSON Dokumente Reihenfolge Millisekunden Reaktionszeiten bei einer beliebigen Skalierung. DocumentDB stellt Container zum Speichern von Daten als **Sammlungen**bezeichnet werden. Sammlungen sind logische Ressourcen und physische Partitionen oder Server erstrecken können. Die Anzahl der Partitionen bestimmt anhand der Größe und der bereitgestellten Durchsatz der Auflistung DocumentDB. Jede Partition in DocumentDB einen festen Betrag SSD Speicher zugeordnet wurde und für hohe Verfügbarkeit repliziert. Partition Management vollständig von Azure DocumentDB verwaltet, und Sie keinen komplexen Code schreiben oder Partitionen verwalten. DocumentDB-Sammlungen sind **praktisch unbegrenzte** Speicherung und Durchsatz. 

Partitionierung ist transparent für die Anwendung. DocumentDB unterstützt Lesevorgänge und Schreibvorgänge, SQL und LINQ-Abfragen JavaScript-basierte Transaktionslogik, konsistenzebenen und abstimmbare Kontrolle über REST-API-Aufrufe zu einer einzelnen Auflistung Ressource. Der Dienst behandelt Verteilen von Daten über Partitionen und routing Anfragen an die richtige Partition. 

Wie funktioniert das? Wenn Sie eine Auflistung in DocumentDB erstellen, sehen Sie, dass gibt eine **Schlüsseleigenschaft Partition** Konfiguration, die Sie angeben können. Dies ist JSON-Eigenschaft (oder Pfad) in Ihren Dokumenten von DocumentDB verwendet werden können, die Daten auf mehrere Server oder Partitionen verteilt. DocumentDB wird hash-Wert der Partition und das gehashte Ergebnis die Partition bestimmen in der JSON-Dokument gespeichert werden. Alle Dokumente mit der gleichen Partition werden in der Partition gespeichert werden. 

Angenommen Sie, eine Anwendung, die Daten zu Mitarbeitern und Abteilung in DocumentDB gespeichert. Wählen Sie `"department"` der Partition Key-Eigenschaft, um Daten nach Abteilung skalieren. Jedes Dokument in DocumentDB muss ein `"id"` -Eigenschaft z. B. für alle Dokumente mit der gleichen Partitionswert eindeutig sein muss `"Marketing`". Jedes Dokument in einer Sammlung gespeichert muss eine eindeutige Kombination aus Partitionsschlüssel und Id, z. B. `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, und `{ "Department": "Sales", "id": "0001" }`. Also die zusammengesetzte Eigenschaft (Partition Schlüssel-Id) ist der Primärschlüssel für die Auflistung.

### <a name="partition-keys"></a>Partitionsschlüssel
Die Auswahl der Partitionsschlüssel ist eine wichtige Entscheidung, die Sie zur Entwurfszeit. Sie müssen einen Eigenschaftsnamen JSON auswählen, der eine Vielzahl von Werten und wahrscheinlich Zugriffsmuster verteilt haben. Der Partitionsschlüssel wird z. B. als JSON-Pfad angegeben `/department` Abteilung Eigenschaft darstellt. 

Die folgende Tabelle zeigt Beispiele für Partitionsdefinitionen und entsprechenden JSON-Werte.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partition-Pfad</strong></p></td>
            <td valign="top"><p><strong>Beschreibung</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>JSON-Wert von doc.department Doc das Dokument ist entspricht.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Name/Eigenschaften</p></td>
            <td valign="top"><p>JSON-Wert von doc.properties.name Doc Document (geschachtelte Eigenschaft ist) entspricht.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ ID</p></td>
            <td valign="top"><p>Entspricht dem JSON-Wert von doc.id (Id und Partition sind dieselbe Eigenschaft).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "Abteilungsname"</p></td>
            <td valign="top"><p>Entspricht dem Wert JSON doc ["Fachbereich"] Doc das Dokument ist.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] Die Syntax für Partitionspfad ähnelt die Pfadangabe für die Indizierung Richtlinie Pfade mit der Hauptunterschied, die der Pfad der Eigenschaft anstelle des Werts entspricht, das heißt keine Platzhalter am Ende. Beispielsweise würden Sie/Abteilung angeben? um die Werte unter Abteilung zu indizieren, aber fest /department als Definition des partition Pfad der Partition implizit indiziert und kann nicht ausgeschlossen indizieren mit Volltextindizierung außer Kraft setzt.

Nehmen Sie sich wie die Auswahl der Partitionsschlüssel die Leistung der Anwendung auswirken.

### <a name="partitioning-and-provisioned-throughput"></a>Partitionierung und bereitgestellte Durchsatz
DocumentDB wurde für vorhersagbare Leistung entwickelt. Wenn Sie eine Auflistung erstellen, reservieren Sie Durchsatz ** [anforderungseinheiten](documentdb-request-units.md) (RU) pro Sekunde**. Jede Anforderung erhält einen Zuschlag pro Einheit Anforderung, der proportional zur Menge an Systemressourcen wie CPU- und e/a von der Operation verbraucht wird. Lesen eines Dokuments 1 kB-Session-Konsistenz verbraucht 1 Anforderung Einheit. Ein Lesevorgang wird 1 RU unabhängig von der Anzahl der gespeicherten Elemente oder die Anzahl der gleichzeitigen Anfragen gleichzeitig ausführen. Umfangreichere Dokumente werden höhere anforderungseinheiten je benötigt. Sollten Sie die Größe der Entitäten und die Anzahl der Lesevorgänge für die Anwendung unterstützen müssen, können Sie den genauen Betrag der Durchsatz für die Ihre Anwendung benötigt lesen bereitstellen. 

Wenn DocumentDB Dokumente speichert, verteilt sie gleichmäßig in Partitionen basierend auf der Partition Schlüsselwert. Der Durchsatz wird auch gleichmäßig verteilt die Partitionen also des Durchsatzes pro Partition = (gesamte Durchsatz pro Auflistung) (Anzahl der Partitionen). 

>[AZURE.NOTE] Um die vollständige Durchsatz der Auflistung müssen Sie Partitionsschlüssel auswählen, der Anfragen von verschiedenen unterschiedliche Partition Schlüsselwerte verteilen können.

## <a name="single-partition-and-partitioned-collections"></a>Einzelne Partition und partitionierte Sammlungen
DocumentDB unterstützt die Erstellung einer Partition und partitioniert. 

- **Partitionierte Sammlungen** können mehrere Partitionen erstrecken und Unterstützung sehr großer Speicher und Durchsatz. Geben Sie Partitionsschlüssel für die Auflistung ein.
- **Einer Partition Sammlungen** haben niedrigeren Preisoptionen und die Möglichkeit, Abfragen und zum Ausführen von Transaktionen über alle Daten. Skalierbarkeits- und Speicher maximal eine Partition haben. Sie müssen keinen Partitionsschlüssel für diese Sammlungen geben. 

![Partitionierte Sammlungen in DocumentDB][2] 

Für Szenarien, die große Mengen an Speicher oder Durchsatz nicht benötigen, sind einzelne Partition Sammlungen geeignet. Beachten Sie, dass einer Partition Sammlungen Skalierbarkeits- und Speichergrenzwerte einer einzelnen Partition d.h. bis zu 10 GB Speicher und bis zu 10.000 Anforderung pro Sekunde. 

Partitionierte Sammlungen können sehr große Mengen an Speicher und Durchsatz unterstützen. Die Standard-Angebote sind jedoch bis zu 250 GB Speicher und bis zu 250.000 anforderungseinheiten pro Sekunde skalieren konfiguriert. Höhere Speicher oder Durchsatz pro Auflistung benötigen, wenden Sie sich an [Azure unterstützt](documentdb-increase-limits.md) diese erhöhte für Ihr Konto haben.

Die folgende Tabelle zeigt die Unterschiede mit einer einzelnen Partition und partitionierte Sammlungen:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Einzelne Partition-Auflistung</strong></p></td>
            <td valign="top"><p><strong>Partitionierte Auflistung</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Partitionsschlüssel</p></td>
            <td valign="top"><p>Keine</p></td>
            <td valign="top"><p>Erforderlich</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Primärschlüssel für Dokument</p></td>
            <td valign="top"><p>"Id"</p></td>
            <td valign="top"><p>Zusammengesetzte Schlüssel &lt;Partitionsschlüssel&gt; und "Id"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimale Speicher</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximaler Speicher</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Unbegrenzt (standardmäßig 250 GB)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaler Durchsatz</p></td>
            <td valign="top"><p>400 Anfrage Einheiten pro Sekunde</p></td>
            <td valign="top"><p>10.000 anforderungseinheiten pro Sekunde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximaler Durchsatz</p></td>
            <td valign="top"><p>10.000 anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>Unbegrenzt (250.000 anforderungseinheiten pro Sekunde standardmäßig)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API-Versionen</p></td>
            <td valign="top"><p>Alle</p></td>
            <td valign="top"><p>API 2015-12-16 und höher</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>Arbeiten mit SDKs

Azure DocumentDB Unterstützung für automatische Partitionierung [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx)API Version 2015-12-16. Um partitionierte Sammlungen erstellen, müssen Sie SDK-Versionen 1.6.0 herunterladen oder auf das SDK Plattformen (.NET Node.js Java, Python) höher. 

### <a name="creating-partitioned-collections"></a>Erstellen von partitionierten Sammlungen

Das folgende Beispiel zeigt einen Ausschnitt für .NET eine Auflistung zum Speichern von Telemetriedaten 20.000 Anforderung Einheiten pro Sekunde Durchsatz Gerät erstellen. Das SDK wird der Wert OfferThroughput (wiederum setzt die `x-ms-offer-throughput` Anforderungsheader in die REST-API). Legen wir die `/deviceId` als Partitionsschlüssel. Die Wahl der Partitionsschlüssel wird zusammen mit dem Rest der Auflistung Metadaten wie Namen und Indizierung Richtlinie gespeichert.

Für dieses Beispiel wir nahmen `deviceId` da wir, dass wissen (a) Da gibt es eine große Anzahl von Geräten, schreibt gleichmäßig über Partitionen verteilt sein und zu skalieren der Datenbank große Datenmengen aufnehmen und (b) viele Anfragen wie aktuelle Daten ein abrufen auf einzelne DeviceId beschränkt aus einer einzigen Partition abgerufen werden.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Um partitionierte Sammlungen erstellen, müssen Sie den Durchsatzwert > 10.000 anforderungseinheiten pro Sekunde angeben. Da Durchsatz ein Vielfaches von 100 ist, ist dies 10.100 oder höher.

Diese Methode stellt eine REST-API aufrufen, um DocumentDB und der Dienst Bereitstellen einer Anzahl von Partitionen basierend auf den angeforderten Durchsatz. Sie können den Durchsatz einer Auflistung ändern, wie Ihre Leistung entwickeln. Weitere Informationen finden Sie unter [Leistung](documentdb-performance-levels.md) .

### <a name="reading-and-writing-documents"></a>Lesen und Schreiben von Dokumenten

Nun fügen Sie Daten in DocumentDB. Hier ist eine Beispielklasse mit einem Gerät lesen und ein Aufruf von CreateDocumentAsync ein neues Gerät lesen in eine Auflistung eingefügt.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Wir lesen Dokument-Id mit der Partitionsschlüssel aktualisieren und als letzten Schritt Partitionsschlüssel und Id löschen. Beachten Sie, dass die Lesevorgänge einen PartitionKey Wert (entsprechend der `x-ms-documentdb-partitionkey` Anforderungsheader in die REST-API).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Abfragen von partitionierten Sammlungen

Beim Abfragen von Daten in partitionierten Sammlungen leitet DocumentDB die Abfrage automatisch Partitionen für die Partition Schlüsselwerte im Filter angegebenen (sofern vorhanden). Diese Abfrage wird beispielsweise an nur die Partition mit der Partitionsschlüssel "XMS-0001" weitergeleitet.

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

Die folgende Abfrage verfügt nicht über einen Filter für den Partitionsschlüssel (DeviceId) und alle Partitionen, die Partition Index erfolgt, aufgefächert ist. Beachten Sie, dass Sie an der EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in die REST-API) SDK zum Ausführen einer Abfrage über Partitionen haben.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>Parallele Ausführung

DocumentDB-SDKs 1.9.0 und parallelen Ausführung Supportoptionen ermöglicht niedrige Latenz Abfragen in partitionierten Sammlungen durchführen, auch wenn sie eine große Anzahl von Partitionen zu berühren. Die folgende Abfrage ist beispielsweise konfiguriert partitionsübergreifend parallel ausgeführt werden.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

Optimieren die folgenden Parameter können Sie parallele Ausführung verwalten:

- Mit `MaxDegreeOfParallelism`, den Grad der Parallelität z.B. die maximale Anzahl gleichzeitiger Netzwerkanschlüsse der Auflistung Partitionen steuern. Wenn Sie dies festlegen-1, wird der Grad der Parallelität vom SDK. Wenn die `MaxDegreeOfParallelism` ist nicht angegeben oder auf 0 (null) der Standardwert ist, werden eine Netzwerkverbindung in der Auflistung Partitionen.
- Mit `MaxBufferedItemCount`, können Handel aus Abfrage Latenz und Client Side speicherauslastung. Wenn Sie diesen Parameter oder das-1, wird die Anzahl der Elemente, die während der parallelen Abfrage gepuffert vom SDK.

Bei demselben Zustand der Auflistung gibt eine parallele Abfrage Ergebnisse in der gleichen Reihenfolge wie serielle Ausführung zurück. Beim Durchführen einer Abfrage Cross-Partition, die sortieren (ORDER BY bzw. oben) umfasst, DocumentDB SDK stellt die Abfrage parallel über Partitionen und führt vorsortierten führt die Clientseite Global geordnete Ergebnisse.

### <a name="executing-stored-procedures"></a>Ausführen von gespeicherten Prozeduren

Sie können auch atomare Transaktionen für Dokumente mit derselben Geräte-ID ausführen, z. B. Wenn Sie Aggregate oder die neuesten ein Gerät in einem einzelnen Dokument verwalten. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

Im nächsten Abschnitt betrachten wir wie partitionierte Sammlungen aus einer Partition verschoben werden können.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Migration von einer Partition auf partitionierten Sammlungen
Wenn eine Anwendung eine Auflistung einer Partition benötigt höheren Durchsatz (> 10.000 RU-s) oder größere Daten (> 10GB), können Sie das [Migrationsprogramm DocumentDB](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) Auflistung einer Partition einer partitionierten Auflistung Daten migrieren. 

Migrieren aus einer Partition einer partitionierten Auflistung

1. Exportieren von Daten aus einer Partition Auflistung JSON. Weitere Informationen finden Sie in der [JSON-Datei exportieren](documentdb-import-data.md#export-to-json-file) .
2. Importieren Sie die Daten in einer partitionierten Kollektion mit Schlüssel Partitionsdefinition 10.000 Anforderung Datendurchsatz pro Sekunde, wie im folgenden Beispiel gezeigt. Weitere Informationen finden Sie [in DocumentDB importieren](documentdb-import-data.md#DocumentDBSeqTarget) .

![Migrieren von Daten eine partitionierte Auflistung in DocumentDB][3]  

>[AZURE.TIP] Erhöhen Sie für Import schneller die Anzahl der Parallel Anfragen auf 100 oder höher der höheren Durchsatz partitionierte Sammlungen nutzen. 

Da wir die Grundlagen abgeschlossen haben, betrachten einige wichtige Aspekte beim Arbeiten mit Partitionsschlüssel in DocumentDB.

## <a name="designing-for-partitioning"></a>Entwerfen für die Partitionierung
Die Auswahl der Partitionsschlüssel ist eine wichtige Entscheidung, die Sie zur Entwurfszeit. Dieser Abschnitt beschreibt einige Nachteile bei Partitionsschlüssel für die Auflistung.

### <a name="partition-key-as-the-transaction-boundary"></a>Partitionsschlüssel als die Transaktionsgrenze
Gewünschter Partitionsschlüssel sollten Transaktionen erforderlich, mehrere Partitionsschlüssel eine skalierbare Lösung Entitäten verteilt aktivieren müssen ausgeglichen. Eine extrem gleichen Partitionsschlüssel konnte für alle Dokumente festgelegt, jedoch kann dadurch Erweiterbarkeit der Lösung begrenzt. Im anderen Extremfall könnte Sie eindeutige Partitionsschlüssel für jedes Dokument zuweisen hochskalierbare wäre jedoch würde verhindern Sie Cross Dokument Transaktionen über gespeicherte Prozeduren und Trigger. Eine ideale Partitionsschlüssel gehört, ermöglicht effiziente Abfragen verwenden und mit ausreichenden Kardinalität um sicherzustellen, dass Ihre Lösung skalierbar ist. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Speicher und Performance-Engpässe vermeiden 
Es ist auch wichtig, eine Eigenschaft, die Schreibvorgänge über eine Anzahl von unterschiedlichen Werten verteilt werden können. Anfragen an den gleichen Partitionsschlüssel darf höchstens den Durchsatz einer einzelnen Partition und gedrosselt werden. So ist es wichtig, Partitionsschlüssel auszuwählen, der nicht **"Hotspots"** in der Anwendung führt. Die gesamte Speichergröße für Dokumente mit der gleichen Partitionsschlüssel darf auch nicht 10 GB im Speicher. 

### <a name="examples-of-good-partition-keys"></a>Beispiele für gute Partitionsschlüssel
Hier sind einige Beispiele für wie Partitionsschlüssel für Ihre Anwendung:

* Wenn Sie eine Benutzer Profil Backend implementieren, ist die Benutzer-ID eine gute Wahl für Partitionsschlüssel.
* Wenn Sie IoT Daten z. B. Gerätestatus speichern, wird eine Geräte-ID eine gute Wahl für Partitionsschlüssel.
* Sie DocumentDB für die Protokollierung Zeitreihen-Daten verwenden, ist die Hostname oder Prozess-ID eine gute Wahl für Partitionsschlüssel.
* Haben Sie eine Multi-Tenant-Architektur ist die Mandanten-ID eine gute Wahl für Partitionsschlüssel.

Beachten Sie, dass in einigen Fällen verwenden (wie die IoT und Benutzerprofilen beschriebenen) der Partitionsschlüssel Mitgliedsnamen (Dokumentschlüssel) identisch sein muss. In anderen wie der Serie Zeitdaten Partitionsschlüssel möglicherweise, der sich von der Id unterscheidet.

### <a name="partitioning-and-loggingtime-series-data"></a>Partitionierung und Protokollierung/Zeit-Serie Daten
Eines der wichtigsten Einsatzbereiche von DocumentDB ist für die Protokollierung und Telemetrie. Es ist wichtig, gute Partitionsschlüssel auswählen, da Sie möglicherweise große Datenmengen schreibgeschützt. Die Auswahl hängt lesen und Schreiben Sätze und Abfragen ausführen erwarten. Hier sind einige Tipps zum guten Partitionsschlüssel auswählen.

- Ihre Verwendung betrifft eine kleine Anzahl von Acculumating über längere Zeit geschrieben und müssen Abfrage reicht von Zeitstempeln und andere Filter verwenden einen Rollup der Zeitstempel z.B. Datum als Partitionsschlüssel ein guter Ansatz ist. Dadurch Abfrage die Daten für ein Datum aus einer einzigen Partition. 
- Ist Ihre Arbeitslast Schreibzugriff Heavy Allgemein ist, sollten Sie Partitionsschlüssel verwenden, der nicht auf Timestamp basieren, damit DocumentDB Schreibvorgänge über mehrere Partitionen verteilen kann. Hier ist ein Hostname, Prozess-ID, Aktivitäts-ID oder eine andere Eigenschaft mit hoher Kardinalität geeignet. 
- Eine dritte Methode ist einer hybriden, mehrere Sammlungen für jeden Tag/Monat und der Partitionsschlüssel ist eine detaillierte Eigenschaft wie Hostname. Dies hat den Vorteil, den unterschiedliche Leistungsmerkmale basierend auf das Zeitfenster festlegen können, z. B. die Auflistung für den aktuellen Monat zulässig ist mit höherem Durchsatz da Lese- und Schreibvorgänge dient die vorherigen Monate Durchsatz verringern, da nur Lesevorgänge dienen.

### <a name="partitioning-and-multi-tenancy"></a>Partitionierung und mehrere Mandanten
Wenn Sie eine Multi-Tenant-Anwendung DocumentDB implementieren, sind zwei wichtige Muster zum Implementieren von Mandanten mit DocumentDB-Schlüssel pro Mandant eine Partition und ein Einzug pro Mieter. Hier werden die vor- und Nachteile der einzelnen:

* Ein Partitionsschlüssel pro Mandant: In diesem Modell Mieter innerhalb einer einzigen Sammlung zusammengestellt werden. Jedoch Abfragen und fügt für Dokumente in einem einzelnen Mandanten mit einer einzelnen Partition ausgeführt werden. Sie können auch allen Dokumenten in Mieter Transaktionslogik implementieren. Da mehrere Mandanten eine Auflistung verwenden, können Sie Kosten für Speicher und Durchsatz bündeln Mieter innerhalb einer Auflistung anstatt Bereitstellung zusätzlicher Spielraum für jeden Mandanten speichern. Der Nachteil ist, dass Sie keine Performance-Isolierung pro Mandant. Performance-Durchsatz erhöht gelten für die gesamte Auflistung Vs gezielte erhöht Mieter.
* Eine Auflistung pro Mandant: jeder hat eine eigene Auflistung. In diesem Modell können Sie Leistung pro Mandant reservieren. Mit DocumentDBs neuen Energieverbrauchs Preismodell ist dieses Modell für Multi-Tenant-Applikationen mit einer kleinen Anzahl der Mandanten.

Sie können auch Kombination/mehrstufigen Ansatz, der kollokatoren kleine Mieter und größere Mieter eigene Auflistung migriert.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben wir beschrieben, wie Partitionierung funktioniert in Azure DocumentDB partitionierte Sammlungen erstellen und wie Sie gute Partitionsschlüssel für Ihre Anwendung auswählen können. 

-   Durchführen Sie Skalierung und Performance-Tests mit DocumentDB. Ein Beispiel finden Sie unter [Leistung und Skalierung mit Azure DocumentDB testen](documentdb-performance-testing.md) .
-   Erste Schritte mit [SDKs](documentdb-sdk-dotnet.md) oder die [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx)
-   Erfahren Sie mehr über [bereitgestellte Durchsatz in DocumentDB](documentdb-performance-levels.md)
-   Wenn Sie anpassen, wie Ihre Anwendung führt partitionieren möchten, können Sie eigene Partitionierung Clientimplementierung anschließen. [Client-seitige Unterstützung Partitionierung](documentdb-sharding.md)anzeigen

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
