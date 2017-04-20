<properties 
    pageTitle="DocumentDB Konzepte und hierarchische Ressourcenmodell | Microsoft Azure" 
    description="Weitere Informationen Sie zu DocumentDBs hierarchische Modell Datenbanken Sammlungen benutzerdefinierte Funktion (UDF), Dokumente, Berechtigungen, Ressourcen verwalten."
    keywords="Hierarchisches Modell Documentdb, Azure, Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>Hierarchische Ressourcenmodell DocumentDB und Konzepte

Die DocumentDB verwaltet Datenbankentitäten werden als **Ressourcen**bezeichnet. Jede Ressource wird durch einen logischen URI eindeutig identifiziert. Sie können die Ressourcen mithilfe von standardmäßigen HTTP-Verben, Anforderung-Antwort-Header und Statuscodes interagieren. 

Anhand dieses Artikels werden Sie folgenden Fragen beantworten:

- Was ist DocumentDBs-Ressourcenmodell?
- Was sind Ressourcen benutzerdefinierte Ressourcen definiert?
- Wie wird angehen eine Ressource?
- Wie arbeite ich mit?
- Wie arbeite ich mit gespeicherten Prozeduren, Triggern und benutzerdefinierten Funktionen (UDFs)?

## <a name="hierarchical-resource-model"></a>Hierarchische Ressourcenmodell
Das folgende Diagramm zeigt, besteht aus DocumentDB hierarchische **Ressourcenmodell** Ressourcen ein Datenbankkonto jeweils über eine logische und stabile URI adressierbar. Eine Reihe von Ressourcen wird ein **feed** in diesem Artikel genannt werden. 

>[AZURE.NOTE] DocumentDB bietet ein höchst effizientes TCP-Protokoll RESTful seine Kommunikationsmodell [.NET Client SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)erhältlich ist.

![Hierarchische Ressourcenmodell DocumentDB][1]  
**Hierarchische Ressourcenmodell**   

Zum Arbeiten mit Ressourcen, müssen Sie Ihre Azure-Abonnement [Registrieren DocumentDB-Datenbank](documentdb-create-account.md) . Datenbankkonto besteht aus einer Reihe von **Datenbanken**, jeweils mehrere **Sammlungen**, von denen jede wiederum enthalten **gespeicherte Prozeduren, Trigger, UDFs, Dokumente** und **Anlagen** (Vorschaufunktion). Eine Datenbank hat auch **Benutzer**mit **Berechtigungen** für Sammlungen, gespeicherte Prozeduren, Trigger, UDFs, Dokumente oder Anlagen jeweils zugeordnet. Datenbanken, Benutzer, Berechtigungen und Sammlungen systemdefinierte Ressourcen mit bekannten Schemas, Dokumente und Anlagen enthalten beliebigen, benutzerdefinierte JSON-Inhalt.  

|Ressource   |Beschreibung
|-----------|-----------
|Konto   |Ein Konto ist eine Gruppe von Datenbanken und einen festen Betrag von BLOB-Speicher für Anlagen (Vorschaufunktion) zugeordnet. Sie können eine oder mehrere Datenbankkonten mithilfe der Azure-Abonnement erstellen. Weitere Informationen finden Sie auf unserer [Preisseite](https://azure.microsoft.com/pricing/details/documentdb/).
|Datenbank   |Eine Datenbank ist ein logischer Container Speicherung von Dokumenten über Sammlungen partitioniert. Es ist auch ein Benutzercontainer.
|Benutzer   |Die logischen Namespace für scoping Berechtigungen. 
|Berechtigung |Authentifizierungstoken eines Benutzers für den Zugriff auf eine bestimmte Ressource.
|Auflistung |Eine Auflistung ist ein Container für JSON-Dokumente und zugeordnete JavaScript Anwendungslogik. Eine Auflistung ist eine fakturierbare Entität, die [Kosten](documentdb-performance-levels.md) der Auflistung zugeordneter Leistungsstufe bestimmt. Sammlungen können können ein oder mehrere Partitionen/Server umfassen und um Speicher oder Durchsatz praktisch unbegrenzte Datenmengen verarbeiten.
|Gespeicherte Prozedur   |Die Anwendungslogik geschrieben in JavaScript mit registriert und abhängigem innerhalb des Datenbankmoduls ausgeführt.
|Trigger    |Anwendungslogik geschrieben in JavaScript vor oder nach einer Einfügung ersetzen oder löschen.
|UDF    |Die Logik in JavaScript geschrieben. UDFs können Sie einen benutzerdefinierten Abfrageoperator modellieren und erweitern damit Core DocumentDB Abfragesprache.
|Dokument   |Benutzerdefinierte (beliebigen) JSON-Inhalt. Standardmäßig kein Schema muss noch müssen sekundäre Indizes zu einer Auflistung hinzugefügten Dokumente zur Verfügung gestellt werden.
|(Vorschau) Anlage   |Anlage ist ein spezielles Dokument Verweise mit zugeordneten Metadaten für externe Blob-Medien. Entwickler können das Blob von DocumentDB verwaltet oder mit einem externen Blob Service Provider wie OneDrive, Ablage speichern. 


## <a name="system-vs-user-defined-resources"></a>System im Vergleich zu benutzerdefinierten Ressourcen
Ressourcen wie Datenbankkonten, Datenbanken, Sammlungen, Benutzer, Berechtigungen, gespeicherte Prozeduren, Trigger und UDF - alle festen Schema und werden als Ressourcen bezeichnet. Im Gegensatz dazu Ressourcen wie Dokumente und Anhänge haben keine Einschränkungen auf dem Schema und Beispiele für benutzerdefinierte Ressourcen. In DocumentDB, System und benutzerdefinierte Ressourcen dargestellt und als JSON-Standard kompatibel. Alle Ressourcen, System- oder benutzerdefinierten haben die folgenden allgemeinen Eigenschaften.

> [AZURE.NOTE] Beachten Sie, dass alle Eigenschaften in einer Ressource generiert werden durch einen Unterstrich (_) in die JSON-Repräsentation vorangestellt.

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Eigenschaft</strong></p></td>
            <td valign="top"><p><strong>Einstellbare Benutzer oder vom System generiert?</strong></p></td>
            <td valign="top"><p><strong>Zweck</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Vom System generiert</p></td>
            <td valign="top"><p>Vom System generiertes eindeutig und hierarchische Bezeichner für die Ressource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Vom System generiert</p></td>
            <td valign="top"><p>eTag der Ressource für die Steuerung der vollständigen Parallelität erforderlich</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Vom System generiert</p></td>
            <td valign="top"><p>Zuletzt aktualisierte Timestamp der Ressource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Vom System generiert</p></td>
            <td valign="top"><p>Eindeutige adressierbare URI der Ressource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>ID</p></td>
            <td valign="top"><p>Vom System generiert</p></td>
            <td valign="top"><p>Benutzerdefinierte eindeutiger Name der Ressource (mit derselben Partition Schlüsselwert). Wenn der Benutzer keine Id angeben, wird Id vom System generiert werden.</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Wire-Darstellung von Ressourcen
DocumentDB ist nicht proprietären Erweiterungen zu JSON Standard- oder Codierung verlangen; funktioniert mit kompatiblen JSON.  
 
### <a name="addressing-a-resource"></a>Adressieren einer Ressource
Alle Ressourcen sind URI adressierbar. Der Wert **_self** Eigenschaft einer Ressource stellt den relativen URI der Ressource. Das Format des URI umfasst die /\<feed\>/ {_rid} Pfadsegmente:  

|Wert der _self |Beschreibung
|-------------------|-----------
|/DBS   |Feed Datenbanken unter einem Konto
|/DBS/ {DbName}  |Datenbank mit dem Wert {DbName} id
|/DBS/ {DbName} /colls/   |Feed Sammlungen in einer Datenbank
|/DBS/ {DbName} /colls/ {CollName} |Mit dem Wert {CollName} id
|/DBS/ {DbName} /colls/ {CollName} / Docs    |Feed von Dokumenten unter einer Sammlung
|/DBS/ {DbName} /colls/ {CollName} /docs/ {DocId}    |Dokument mit dem Wert {Doc} id
|/DBS/ {DbName} / Users /   |Feed von Benutzern in einer Datenbank
|/DBS/ {DbName} / Users / {ID}   |Benutzer mit dem Wert {User} id
|/DBS/ {DbName} {ID} / Users / / Berechtigungen   |Feed Berechtigungen unter einem Benutzerkonto
|/DBS/ {DbName} / Users / {ID} /permissions/ {PermissionId}    |Mit dem Wert {Berechtigung} id
  
Jede Ressource weist einen eindeutigen definierten Namen über die ID-Eigenschaft verfügbar gemacht. Hinweis: für Dokumente, wenn der Benutzer eine Id nicht unsere unterstützten SDKs automatisch eine eindeutige Id für das Dokument generiert. Die Id ist eine benutzerdefinierte Zeichenfolge von bis zu 256 Zeichen innerhalb des Kontexts einer bestimmten übergeordnete Ressource eindeutig ist. 

Jede Ressource ist auch einen systemgenerierter hierarchische Ressourcenbezeichner (auch bezeichnet als ein RID), über die _rid-Eigenschaft. Die RID codiert die gesamte Hierarchie einer bestimmten Ressource und eine praktische interne Darstellung zum Erzwingen der referenziellen Integrität verteilt ist. Die RID wird in ein Konto eindeutig und wird intern von verwendet DocumentDB für effizientes routing ohne Cross Partition suchen. Die Werte der _self und _rid Eigenschaften sind alternative und kanonische Darstellung einer Ressource. 

DocumentDB übrigen APIs unterstützen Ressourcen Adressierung und Weiterleitung von Anfragen durch die Id und die _rid-Eigenschaften.

## <a name="database-accounts"></a>Datenbankkonten
Sie können eine oder mehrere DocumentDB Datenbankkonten mit Abonnements Azure bereitstellen.

Sie können [DocumentDB Datenbankkonten erstellt und verwaltet](documentdb-create-account.md) das Azure-Portal unter [http://portal.azure.com/](https://portal.azure.com/). Erstellen und Verwalten von einem Konto Administratorzugriff erforderlich und kann nur unter Abonnements Azure ausgeführt werden. 

### <a name="database-account-properties"></a>Eigenschaften von Benutzerkonten
Als Teil der Bereitstellung und Verwaltung von einem Konto Sie konfigurieren und Lesen Sie die folgenden Eigenschaften:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Eigenschaftenname</strong></p></td>
            <td valign="top"><p><strong>Beschreibung</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Konsistenz-Richtlinie</p></td>
            <td valign="top"><p>Legen Sie diese Eigenschaft so konfigurieren Sie die Standardstufe Konsistenz für alle Sammlungen unter Ihrem Konto. Sie können die konsistenzebene pro Anfrage mit [X-ms-Konsistenz-Ebene] Anforderungsheader überschreiben. <p><p>Beachten Sie, dass diese Eigenschaft nur für <i>benutzerdefinierte Ressourcen</i>. Alle System definierte Ressourcen für die Unterstützung konfiguriert sind Lesevorgänge/Abfragen mit starker Konsistenz.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Autorisierungsschlüssel</p></td>
            <td valign="top"><p>Dies sind die primären und sekundären Master und Readonly Schlüssel den administrativen Zugriff auf alle Ressourcen unter dem Konto.</p></td>
        </tr>
    </tbody>
</table>

Beachten Sie, dass neben bereitstellen, konfigurieren und verwalten Ihr Konto aus dem Azure-Portal, können Sie auch programmgesteuert erstellen und Konten DocumentDB Datenbank mithilfe von [Azure DocumentDB REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) und die [Client-SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx).  

## <a name="databases"></a>Datenbanken
DocumentDB-Datenbank ist ein logischer Container von einem Sammlungen und Benutzer, wie im folgenden Diagramm dargestellt. Sie können eine beliebige Anzahl von Datenbanken unter Konto ein DocumentDB Angebot beschränkt erstellen.  

![Konto und Sammlungen hierarchische Datenbankmodell][2]  
**Eine Datenbank ist ein logischer Container von Benutzern und Sammlungen**

Eine Datenbank kann nahezu unbegrenzte Speicherung partitioniert Sammlungen bilden die Transaktion Domänen für die darin enthaltenen Dokumente enthalten. 

### <a name="elastic-scale-of-a-documentdb-database"></a>Flexible Skalierung einer Datenbank DocumentDB
DocumentDB-Datenbank ist standardmäßig von ein paar GB bis Petabyte SSD unterstützt Speicherung und bereitgestellten Durchsatz elastisch. 

Im Gegensatz zu einer herkömmlichen RDBMS in ist eine Datenbank in DocumentDB nicht auf einen einzelnen Computer beschränkt. Mit DocumentDB wie Skalierung der Anwendung wächst, können Sie weitere Sammlungen oder Datenbanken erstellen. Verschiedenen erste Partei Anwendung Microsoft DocumentDB Ebene Consumer Verwenden von sehr umfangreichen DocumentDB jeder mit Tausenden von Sammlungen mit Terabyte Dokument erstellen. Sie vergrößern oder verkleinern eine Datenbank durch Hinzufügen oder Entfernen von Sammlungen der Anwendung skalieren Anforderungen. 

Sie können eine beliebige Anzahl von Sammlungen in einer Datenbank das Angebot erstellen. Jede Auflistung enthält SSD unterstützt Storage und Durchsatz für Sie je nach der ausgewählten Ebene bereitgestellt.

DocumentDB Datenbank ist ebenfalls ein Container Benutzer. Ein Benutzer wiederum, ist logischen Namespace für einen Satz von Berechtigungen, der eine fein abgestufte Autorisierung und Zugriff auf Sammlungen, Dokumente und Anhänge bietet.  
 
Mit anderen Ressourcen im Ressourcenmodell DocumentDB Datenbanken erstellt werden können, ersetzt, gelöscht, lesen oder einfach mit [Azure DocumentDB REST APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder den [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)aufgelistet. DocumentDB garantiert starken Konsistenz lesen oder Metadaten eine Datenbankressource Abfragen. Löschen einer Datenbank automatisch sichergestellt, dass Sammlungen oder darin enthaltenen Benutzer zugreifen kann.   

## <a name="collections"></a>Sammlungen
Eine DocumentDB-Auflistung ist ein Container für die JSON-Dokumente. Eine Auflistung ist auch eine Maßeinheit für Transaktionen und Abfragen. 

### <a name="elastic-ssd-backed-document-storage"></a>Elastische SSD unterstützt die Speicherung von Dokumenten
Eine Auflistung ist systembedingt elastische - automatisch vergrößert und verkleinert hinzufügen oder Entfernen von Dokumenten. Sammlungen sind logische Ressourcen und physische Partitionen oder Server erstrecken können. Die Anzahl der Partitionen in einer Auflistung bestimmt DocumentDB basierend auf der Größe und der bereitgestellten Durchsatz der Auflistung. Jede Partition in DocumentDB einen festen Betrag SSD Speicher zugeordnet wurde und für hohe Verfügbarkeit repliziert. Partition Management vollständig von Azure DocumentDB verwaltet, und Sie keinen komplexen Code schreiben oder Partitionen verwalten. DocumentDB-Sammlungen sind **praktisch unbegrenzte** Speicherung und Durchsatz. 

### <a name="automatic-indexing-of-collections"></a>Automatische Indizierung von Sammlungen
DocumentDB ist eine true-Schema frei. Nicht angenommen oder einem Schema für JSON-Dokumente benötigen. Hinzufügen von Dokumenten zu einer Auflistung DocumentDB automatisch Indexierung und für Abfragen verfügbar sind. Automatische Indizierung von Dokumenten ohne Schema oder sekundären Indizes ist eine wichtige Funktion von DocumentDB und durch Schreiben optimiert, ohne Sperren und Protokoll-strukturierten Index Wartung Techniken aktiviert ist. DocumentDB unterstützt nachhaltige Volume extrem schreibt und weiterhin konsistente Abfragen. Dokument und Index-Speicher wird jede Auflistung belegten Speicher berechnet. Sie können steuern, Speicherung und Leistung Kompromisse Indizierung durch Konfigurieren der Volltextindizierung für eine Auflistung zugeordnet. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Konfigurieren der Indizierung einer Auflistung
Indizierung Policy jeder Auflistung kann Leistung und Kompromisse Indizierung zugeordnet. Die folgenden Optionen stehen Ihnen im Rahmen der Indizierungskonfiguration:  

-   Wählen Sie aus, ob die Auflistung automatisch alle Dokumente oder nicht indiziert. Standardmäßig werden alle Dokumente automatisch indiziert. Sie können automatische Indizierung und selektiv nur bestimmte Dokumente zum Index hinzufügen. Umgekehrt können Sie gezielt bestimmte Dokumente auszuschließen. Sie können dies erreichen, indem die automatische-Eigenschaft auf true oder false für IndexingPolicy einer Auflistung und Anforderungsheader [X-ms-Indexingdirective] beim Einfügen, ersetzen oder Löschen eines Dokuments.  
-   Wahlweise ein- oder Ausschließen von bestimmten Pfaden oder Muster in Dokumenten aus dem Index. Sie können diese Einstellung IncludedPaths und ExcludedPaths auf die IndexingPolicy einer Auflistung bzw. erzielen. Sie können auch Speicherplatz und Leistung Kompromisse Bereich und Hash Abfragen nach bestimmten Pfad Mustern. 
-   Wählen Sie zwischen synchronen (konsistent) und asynchronen (lazy) Index aktualisiert. Standardmäßig ist der Index synchron auf jede einfügen, ersetzen oder Löschen eines Dokuments, das die Auflistung aktualisiert. Dadurch können Abfragen der Konsistenz Niveau des Dokuments lautet berücksichtigt. DocumentDB ist optimiert und anhaltende Volumes Dokument schreibt und synchrone Indexwartung mit konsistenten Abfragen unterstützt, können Sie bestimmte Sammlungen Index verzögert aktualisieren konfigurieren. Verzögerte Indizierung steigert die Schreib-Performance und ist ideal für Massenkopieren Einnahme Szenarien hauptsächlich Lesevorgänge Sammlungen.

Die Indizierung Richtlinie kann durch Ausführen von einem in der Auflistung geändert werden. Dies kann entweder durch die [Client-SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)der [Azure-Portal](https://portal.azure.com) oder [Azure DocumentDB REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### <a name="querying-a-collection"></a>Abfragen einer Auflistung
Die Dokumente innerhalb einer Auflistung können beliebige Schemata und Abfragen Dokumente innerhalb einer Auflistung ohne Schema oder sekundäre Indizes im voraus. Sie können Abfragen mit [DocumentDB-SQL-Syntax](https://msdn.microsoft.com/library/azure/dn782250.aspx)bietet Rich hierarchische, relationale und räumliche Operatoren und Erweiterbarkeit über JavaScript-basierte UDFs. JSON-Grammatik kann zum Modellieren von JSON-Dokumente als Strukturen mit als Strukturknoten. Dies ist von DocumentDBs automatische Indizierung sowie SQL-Dialekt des DocumentDB ausgenutzt. Die Abfragesprache DocumentDB besteht aus drei Aspekte:   

1.  Eine kleine Gruppe von Abfrageoperationen natürlich einschließlich hierarchische Abfragen und Projektionen Struktur zuordnen. 
2.  Eine Teilmenge der relationalen Operationen einschließlich Komposition, Filter, Projektionen, Aggregate und Self-Joins. 
3.  Reine JavaScript-basierte mit UDFs (1) und (2).  

DocumentDB Abfrage-Modell versucht, ein Gleichgewicht zwischen Funktionalität, Effizienz und Einfachheit. DocumentDB-Datenbank-Engine systemeigen kompiliert und die SQL-Abfrage-Anweisungen ausgeführt. Sie können eine Auflistung mit [Azure DocumentDB REST APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder eine [Client-SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)Abfragen. .NET SDK enthält ein LINQ-Anbieter.

> [AZURE.TIP] Sie können versuchen, DocumentDB und unser Dataset [Abfrage Spielplatz](https://www.documentdb.com/sql/demo)SQL-Abfragen ausführen.

### <a name="multi-document-transactions"></a>Multifunktions-Transaktionen
Datenbanken vorsehen mit gleichzeitiger Änderungen an den Daten sicher und vorhersagbaren Programmiermodell. In RDBMS ist die herkömmliche Methode zum Schreiben von Geschäftslogik zu schreiben, **gespeicherte Prozeduren** oder **Trigger** für die Ausführung von Transaktionen an die Datenbank geliefert. Der Anwendungsprogrammierer muss im RDBMS mit zwei unterschiedlichen Programmiersprachen: 

- (Nicht-transaktional) Anwendung Programmiersprache (z. B. JavaScript, Python, C#, Java, usw.)
- T-SQL, transaktionale Programmiersprache die direkt von der Datenbank ausgeführt wird

Aufgrund seiner Engagement zu JavaScript und JSON direkt innerhalb des Datenbankmoduls enthält DocumentDB eine intuitive Programmiermodell Anwendungslogik JavaScript basiert auf die Sammlungen in gespeicherten Prozeduren und Triggern ausgeführt. Dies ermöglicht Folgendes:

- Effiziente Implementierung Parallelität steuern, Wiederherstellung, automatische Indizierung Objektdiagramme JSON direkt in die Datenbank-engine
- Natürlich Ausdrücken Kontrollfluss, Variable scoping Zuordnung und Integration von primitiven mit Datenbanktransaktionen direkt in der Programmiersprache JavaScript-Ausnahmebehandlung

Auf einer Ebene registrierte JavaScript-Logik kann dann Datenbankoperationen in den Dokumenten der angegebenen Auflistung ausstellen. DocumentDB schließt implizit das JavaScript-basierte gespeicherte Prozeduren und Trigger in einer ambient ACID-Transaktionen mit Snapshot-Isolation Dokumenten innerhalb einer Auflistung. Verlauf der Ausführung Wenn JavaScript eine Ausnahme auslöst, wird so die gesamte Transaktion abgebrochen. Das resultierende Programmiermodell ist eine sehr einfache leistungsstark. JavaScript-Entwickler erhalten ein "dauerhaftes" Programmiermodell gleichzeitiger Verwendung ihrer vertrauten Konstrukte und Bibliothek primitiven.   

Auszuführende JavaScript direkt innerhalb des Datenbankmoduls im gleichen Adressbereich wie Pufferpool ermöglicht leistungsfähige und Ausführung von Transaktionen von Datenbankoperationen für die Dokumente einer Auflistung. Darüber hinaus DocumentDB-Datenbank-Engine wird Engagement der JSON und JavaScript eliminiert alle Impedance Mismatch zwischen Typ der Anwendung und der Datenbank.   

Nach dem Erstellen einer Sammlung können Sie gespeicherte Prozeduren, Trigger und UDFs mit Nutzung der [Azure DocumentDB REST APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)registrieren. Nach der Registrierung können Sie verweisen und ausführen. Betrachten Sie die folgende gespeicherte Prozedur in JavaScript geschrieben, folgenden Code nimmt zwei Argumente (Buch und Autorennamen) erstellt ein neues Dokument für ein Dokument Abfragen und aktualisiert es alle impliziten ACID-Transaktion. Wenn JavaScript-Ausnahme ausgelöst wird, wird Sie jederzeit während der Ausführung die gesamte Transaktion abgebrochen.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

Der Client versenden"über JavaScript-Logik in der Datenbank für die Ausführung von Transaktionen über HTTP POST". Weitere Informationen über HTTP-Methoden finden Sie unter [Rest Interaktionen mit DocumentDB](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Beachten Sie, dass, da die Datenbank direkt JSON und JavaScript kennt, kein System Typkonflikt, "Oder Zuordnung" oder Code Generation Magic erforderlich.   

Gespeicherte Prozeduren und Trigger interagieren mit einer Auflistung und Dokumente in einer Auflistung über einen definierten Objektmodell der aktuellen Auflistung Kontext verfügbar macht.  

Sammlungen in DocumentDB können erstellt, gelöscht, lesen oder [Azure DocumentDB REST APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder eine [Client-SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)aufgelistet einfach mit. DocumentDB bietet immer Sicheres Lesen oder Abfragen der Metadaten einer Auflistung. Löschen einer Auflistung automatisch sichergestellt, dass kann nicht auf Dokumente, Anlagen, gespeicherte Prozeduren, Trigger und UDF-Dateien darin enthalten.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktion (UDF)
Wie im vorherigen Abschnitt beschrieben, können Sie direkt innerhalb einer Transaktion in die Datenbank-Engine die Logik schreiben. Die Anwendungslogik kann vollständig in JavaScript geschrieben und als eine gespeicherte Prozedur, Trigger oder eine UDF modelliert werden kann. JavaScript-Code in eine gespeicherte Prozedur oder ein Trigger kann einfügen, ersetzen, löschen, lesen oder Abfrage innerhalb einer Auflistung. Auf der anderen Seite kann JavaScript innerhalb einer UDF-Datei einfügen, ersetzen oder löschen. UDFs aufgelistet werden die Dokumente von einer Abfrage und ein weiteres Resultset erzeugen. Für mehrere Mandanten erzwingt DocumentDB eine strikte Reservierung Ressource Governance. Jede gespeicherte Prozedur, Trigger oder eine UDF wird eine feste Quantum Betriebssystemressourcen arbeiten. Darüber hinaus gespeicherte Prozeduren, Trigger oder UDF nicht gegen externe JavaScript-Bibliotheken verknüpfen und überschreiten den ihnen zugewiesenen Ressource Budgets gesperrt werden. Sie registrieren, Registrierung gespeicherte Prozeduren, Trigger oder UDF mit einer Auflistung mithilfe der REST-APIs.  Bei der Registrierung ist eine gespeicherte Prozedur, Trigger oder eine UDF vorkompiliert und als Byte-Code später ausgeführt wird. Im folgenden Abschnitt beschreiben, wie Sie DocumentDB JavaScript SDK zu erfassen, ausführen und Aufheben der Registrierung einer gespeicherten Prozedur, Trigger und einer UDF. JavaScript-SDK ist ein einfacher Wrapper über die [DocumentDB REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

### <a name="registering-a-stored-procedure"></a>Registrieren einer gespeicherten Prozedur
Registrierung einer gespeicherten Prozedur erstellt eine neue gespeicherte Prozedur Ressource in einer Auflistung über HTTP POST.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Ausführen einer gespeicherten Prozedur
Die Ausführung einer gespeicherten Prozedur erfolgt mit HTTP POST für eine vorhandene gespeicherte Prozedur Ressource durch Übergabe von Parametern an die Prozedur im Hauptteil Anforderung.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Aufheben der Registrierung einer gespeicherten Prozedur
Aufheben der Registrierung einer gespeicherten Prozedur erfolgt einfach durch die Ausgabe einer HTTP DELETE für eine vorhandene gespeicherte Prozedur Ressource.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Vor Trigger registrieren
Registrierung eines Triggers erfolgt durch Erstellen einer neuen Trigger-Ressource in einer Auflistung über HTTP POST. Sie können angeben, wenn der Trigger ist ein oder ein posttrigger und den Typ des Vorgangs kann (z. B. erstellen, ersetzen, löschen oder alle) zugeordnet werden.   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Vor Trigger ausführen
Ausführung eines Triggers erfolgt durch Angabe eines vorhandenen Triggers bei der POST, PUT/DELETE Anforderung einer Ressource Dokument über die Anforderungsheader.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Aufheben der Registrierung vor trigger
Aufheben der Registrierung eines Triggers erfolgt einfach über Ausstellen einer HTTP DELETE für eine vorhandene Trigger-Ressource.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Registrieren einer UDF-Datei
Registrierung einer UDF-Datei erfolgt durch Erstellen einer neuen UDF-Ressource in einer Auflistung über HTTP POST.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Ausführen einer UDF-Datei als Teil der Abfrage
Eine UDF-Datei kann als Teil der SQL-Abfrage angegeben und dient als zentrale [SQL-Abfragesprache der DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx)erweitern können.

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Aufheben der Registrierung einer UDF-Datei 
Aufheben der Registrierung einer UDFs erfolgt einfach durch die Ausgabe einer HTTP DELETE für eine vorhandene UDF-Ressource.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Trotz der obigen Codeausschnitt Registrierung (POST), Abmeldung (PUT), Read List (GET) und Ausführung (POST) über [DocumentDB JavaScript-SDK](https://github.com/Azure/azure-documentdb-js)können Sie auch den [REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder anderen [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx). 

## <a name="documents"></a>Dokumente
Sie können einfügen, ersetzen, löschen, lesen, auflisten und beliebige JSON-Dokumente in einer Auflistung Abfragen. DocumentDB Schemas nicht Mandat und erfordert keine sekundären Indizes zur Unterstützung von Abfragen von Dokumenten in einer Auflistung.   

Wird ein Dienst wirklich öffnen, DocumentDB wird nicht dazu gezwungen spezielle Datentypen (z. B. Datum Uhrzeit) oder bestimmte Codierung für JSON-Dokumente. Beachten Sie, dass DocumentDB keine speziellen JSON Konventionen Beziehungen zwischen verschiedenen Dokumenten zu kodifizieren. die SQL-Syntax DocumentDB bietet leistungsstarke hierarchische und relationale Abfrage auf Abfrage und Projektdokumente ohne spezielle Kommentare müssen Beziehungen zwischen Dokumenten mit Zusätzen Eigenschaften unterschieden.  
 
Als können mit anderen Ressourcen Dokumente, ersetzt, gelöschte lesen, aufgelisteten und abgefragten problemlos mit anderen APIs oder den [Client-SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)erstellt werden. Löschen ein Dokument sofort frei das Kontingent für alle geschachtelten Anlagen. Lesen Sie Konsistenz auf Dokumente gelten Richtlinien Konsistenz auf das Konto. Diese Richtlinie kann auf pro-Anfrage je nach Daten Konsistenz Ihrer Anwendung überschrieben werden. Beim Abfragen von Dokumenten folgt Lese Konsistenz der Indexierungsmodus für die Auflistung festgelegt. Für "konsistent" folgt Kontorichtlinie Konsistenz. 

## <a name="attachments-and-media"></a>Anlagen und Medien
>[AZURE.NOTE] Anlage und Medien sind Vorschaufunktionen.
 
DocumentDB können Sie binäre Blobs/Medien mit DocumentDB oder eigene Speicher Remotemedien speichern. Es können Sie Metadaten Medien in Dokument Anlage darstellen. Eine Anlage in DocumentDB ist ein besondere (JSON)-Dokument, das das Media-Blob-gespeichertes verweist. Eine Anlage ist einfach ein Dokument, das die Metadaten eingelagerten Remotemedien gespeicherte Medien (z. B. Speicherort, Autor usw.) erfasst. 

Einer sozialen lesen Anwendung der DocumentDB verwendet, um speichern ihandanmerkungen und Metadaten, einschließlich der Kommentare hervorgehoben, Lesezeichen, Bewertung, mag/abneigungen usw. für ein e-Book von einem bestimmten Benutzer verknüpft ist.   

-   Der Inhalt selbst wird entweder in der Speichermedien gespeichert als Teil des DocumentDB Konto oder einen remote-Media verfügbar. 
-   Eine Anwendung kann jeder Benutzer Metadaten als unterschiedliche Dokument speichern – z.B. Wilhelms Metadaten für Mappe1 in ein Dokument von /colls/joe/docs/book1 gespeichert. 
-   Anlagen auf den Inhaltsseiten eines bestimmten Buches eines Benutzers befinden sich unter dem entsprechenden Dokument z.B. /colls/joe/docs/book1/chapter1 /colls/joe/docs/book1/chapter2 usw.. 

Beachten Sie, dass die oben aufgeführten Beispiele angezeigten Ids die Ressourcenhierarchie vermitteln. Ressourcen erfolgt über die REST-APIs über eindeutige Ressourcen-Ids. 

Für das Medium DocumentDB verwaltet, wird die Eigenschaft _media der Anlage Medien durch den URI verweisen. DocumentDB gewährleistet Müll sammeln die Medien, wenn alle ausstehenden Verweise gelöscht. DocumentDB automatisch die Anlage beim Hochladen der neuen Medien und füllt _media auf neue Medien. Wenn Sie Medien in einem von Ihnen (z. B. OneDrive, Azure Storage DropBox etc.) verwalteten remote-BLOB-Speicher speichern, weiterhin können Anlagen Sie die Medien. In diesem Fall Sie die Anlage selbst erstellen und füllen die zugehörige _media-Eigenschaft.   

Mit anderen Ressourcen Anlagen erstellt werden können, ersetzt, gelöscht, lesen oder einfach mit anderen APIs oder eine Client-SDKs aufgelistet. Bei Dokumenten folgt lesen Konsistenz Anlagen Konsistenz Richtlinie auf das Konto. Diese Richtlinie kann auf pro-Anfrage je nach Daten Konsistenz Ihrer Anwendung überschrieben werden. Beim Abfragen der Anlagen folgt Lese Konsistenz der Indexierungsmodus für die Auflistung festgelegt. Für "konsistent" folgt Kontorichtlinie Konsistenz. 
 
## <a name="users"></a>Benutzer
DocumentDB Benutzer stellt einen logischen Namespace zum Gruppieren von Berechtigungen. DocumentDB Benutzer kann ein Benutzer ein Identity Managementsystem oder einer vordefinierten Anwendung entsprechen. Für DocumentDB stellt ein Benutzer einfach eine Abstraktion gruppiert einen Satz von Berechtigungen in einer Datenbank.   

Für mehrere Mandanten in der Anwendung implementieren, erstellen Sie Benutzer in DocumentDB entspricht der tatsächliche Benutzer zu den Mandanten der Anwendung. Sie können dann Berechtigungen für einen bestimmten Benutzer erstellen, die den Zugriff steuern verschiedene Sammlungen, Dokumente, Anlagen usw. entsprechen.   

Bedarf mit Ihrem Wachstum der Anwendung können Sie verschiedene Methoden zum Splitter Daten übernehmen. Sie können jeden Benutzer wie folgt Modell:   

-   Jeder Benutzer einer Datenbank zugeordnet.
-   Jeder Benutzer ordnet eine Auflistung. 
-   Dokumente mit mehreren Benutzern werden dedizierte Auflistung. 
-   Dokumente mit mehreren Benutzern werden eine Reihe von Sammlungen.   

Unabhängig von der gewählten Strategie bestimmte Sharding können die tatsächlichen Benutzer als Benutzer in DocumentDB Datenbank Modell und feinkörnige Berechtigungen für jeden Benutzer.  

![Benutzersammlungen][3]  
**Sharding-Strategien und Modellierung Benutzer**

Wie alle anderen Ressourcen Benutzer in DocumentDB erstellt werden können, ersetzt, gelöscht, lesen oder einfach mit anderen APIs oder eine Client-SDKs aufgelistet. DocumentDB bietet immer Sicheres Lesen oder die Metadaten eines benutzerressource Abfragen. Es ist erwähnenswert, dass automatisch Löschen eines Benutzers wird sichergestellt, dass der darin enthaltenen Berechtigungen zugreifen kann. Obwohl die DocumentDB als Teil des gelöschten Benutzers im Hintergrund das Kontingent der Berechtigungen freigegeben, gelöschten Berechtigungen steht sofort wieder verwenden.  

## <a name="permissions"></a>Berechtigungen
Aus Access Control Sicht Ressourcen wie Datenbank Datenbanken, Benutzer und Berechtigungen *administrativen* Ressourcen gelten, da diese Administratorrechte erforderlich sind. Auf der anderen Seite sind Ressourcen einschließlich Sammlungen, Dokumente, Anlagen, gespeicherte Prozeduren, Trigger und UDFs unter eine bestimmte Datenbank begrenzt und als *Anwendungsressourcen*. Für zwei Typen von Ressourcen und die Rollen, die sie (Administrator und Benutzer) Zugriff auf das Autorisierungsmodell definiert zwei Arten von *Zugriffstasten*: *Hauptschlüssel* und *Ressourcenschlüssel*. Der Hauptschlüssel ist ein Teil des Datenbankkontos und sollen die Entwickler (oder Administrator) ist die Konto der Bereitstellung. Dieser Hauptschlüssel hat Administrator Semantik, zum Autorisieren des Zugriffs auf administrative und Anwendung verwendet werden kann. Dagegen ist ein Ressourcenschlüssel präzise Zugriffstaste, die einer *bestimmten* Anwendungsressource zuzugreifen. So erfasst die Beziehung zwischen dem Benutzer einer Datenbank und die Berechtigungen der Benutzer für eine bestimmte Ressource (z. B. Auflistung, Dokument, Anlage, gespeicherte Prozedur, Trigger oder UDF).   

Die einzige Möglichkeit, einen Ressourcenschlüssel erhalten ist eine Ressource unter einem bestimmten Benutzerkonto erstellen. Beachten Sie, dass erstellen oder Abrufen einer Berechtigung, ein Hauptschlüssel in der Authorization-Header vorgelegt. Eine Ressource verbindet die Ressource und den Zugriff der Benutzer. Nachdem eine Ressource erstellt, muss der Benutzer nur Schlüssel zugeordnete Ressource vorhanden, um die relevante Ressource zugreifen. Ein Ressourcenschlüssel kann daher als logische und kompakten Darstellung der Ressource angezeigt werden.  

Wie bei allen anderen Ressourcen Berechtigungen in DocumentDB erstellt werden können, ersetzt, gelöscht, lesen oder problemlos mit anderen APIs oder eine Client-SDKs aufgelistet. DocumentDB bietet immer Sicheres Lesen oder Abfragen der Metadaten einer Berechtigung. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Arbeiten mit Ressourcen mithilfe von HTTP-Befehlen in [Rest Interaktionen mit DocumentDB](https://msdn.microsoft.com/library/azure/mt622086.aspx).


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

