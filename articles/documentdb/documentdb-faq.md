<properties 
    pageTitle="DocumentDB Datenbank Fragen - häufig gestellte Fragen | Microsoft Azure" 
    description="Enthält Antworten auf häufig gestellte Fragen zur Azure DocumentDB NoSQL-Dokumentdienst Datenbank JSON. Datenbank-Fragen über Kapazität, Leistung und Skalierung." 
    keywords="Häufig gestellte Fragen, Documentdb, Azure, Microsoft Azure Datenbank Fragen"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Häufig gestellte Fragen zu DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Datenbank-Fragen zu Microsoft Azure DocumentDB-Grundlagen

### <a name="what-is-microsoft-azure-documentdb"></a>Was ist Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB ist eine blitzschnell, weltweit operierenden NoSQL Dokument Datenbank-as-a-Service, die bietet Abfragen über Schema-Daten, konfigurierbar und zuverlässige Leistung, unterstützt und ermöglicht die rasche Entwicklung durch eine verwaltete Plattform unterstützt der Leistung und Microsoft Azure erreichen. DocumentDB ist die richtige Lösung für mobile Spiele Web und IoT Applikationen vorhersagbare Durchsatz, hohe Verfügbarkeit, geringe Latenz und ein Modell Schema Daten sind wichtige Vorschriften. DocumentDB bietet Flexibilität Schema und Rich Indizierung über systemeigene JSON-Datenmodell und unterstützt mehrere Dokumente transaktional mit integrierten JavaScript.  
  
Datenbank Fragen Antworten und Informationen zum Bereitstellen und verwenden diesen Dienst finden Sie unter [DocumentDB-Dokumentationsseite](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-kind-of-database-is-documentdb"></a>Welche Art von Datenbank ist DocumentDB?
DocumentDB ist eine NoSQL ausgerichteten Datenbank, die Daten im JSON-Format gespeichert.  DocumentDB unterstützt geschachtelte eingeschlossen Daten Strukturen, die über eine umfangreiche DocumentDB [SQL Query Grammatik](documentdb-sql-query.md)abgefragt werden können. DocumentDB bietet eine hohe Leistung transaktionale Verarbeitung von serverseitigen JavaScript durch [gespeicherte Prozeduren, Trigger und benutzerdefinierten Funktionen](documentdb-programming.md). Die Datenbank unterstützt auch Entwickler tunable Konsistenz mit zugeordneten [Leistung](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>Haben DocumentDB Datenbanken Tabellen einer relationalen Datenbank (RDBMS)?
Nein, DocumentDB Daten in Sammlungen von JSON-Dokumente gespeichert.  Informationen zu DocumentDB Ressourcen finden Sie unter [DocumentDB-Ressourcenmodell und Konzepte](documentdb-resources.md). Weitere Informationen über die Unterschiede zwischen NoSQL-Lösungen wie DocumentDB und relationale Solutions anzeigen Sie [NoSQL Vs SQL](documentdb-nosql-vs-sql.md)

### <a name="do-documentdb-databases-support-schema-free-data"></a>Unterstützen Datenbanken DocumentDB Schema-Daten?
Ja, ist die DocumentDB Anwendung beliebige JSON Dokumente ohne Schemadefinition oder Hinweise. Daten sind sofort Abfrage über die Schnittstelle DocumentDB SQL-Abfrage.   

### <a name="does-documentdb-support-acid-transactions"></a>Unterstützt DocumentDB ACID-Transaktionen?
Ja, unterstützt DocumentDB zwischen Dokumenten Transaktionen als JavaScript gespeicherte Prozeduren und Trigger. Transaktionen auf einer Partition innerhalb jeder Sammlung beschränkt und ACID-Semantik wie alle ausgeführt oder nichts isoliert von anderen gleichzeitig ausgeführten Code und Anfragen.  Durch Server Side Ausführung von Anwendungscode JavaScript Ausnahmen ausgelöst werden, wird die gesamte Transaktion zurückgesetzt. Weitere Informationen über Transaktionen finden Sie unter [Programm Datenbanktransaktionen](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Was sind die normalen für DocumentDB?  
DocumentDB ist eine gute Wahl für neue mobile Web-Gaming und IoT Applikationen, automatische Skalierung, vorhersagbare Leistung schnell Reihenfolge Millisekunde Reaktionszeiten und die Möglichkeit, die Abfrage über Schema-Daten, ist wichtig. DocumentDB eignet sich für die schnelle Entwicklung und kontinuierliche Iteration von Datenmodellen Anwendung unterstützt. Programme, die Benutzer erzeugte Inhalte und Daten zu verwalten sind [häufig für DocumentDB](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Wie bietet DocumentDB vorhersagbare Leistung?
Eine [Anforderung Einheit](documentdb-request-units.md) ist das Maß für den Durchsatz in DocumentDB. 1 RU Durchsatz Abrufen eines Dokuments 1KB entspricht. Jedem Vorgang in DocumentDB, einschließlich Lesevorgänge, Schreibvorgänge, SQL-Abfragen und gespeicherte Prozedur wie hat einen deterministischen RU Wert basierend auf den Durchsatz zum Abschließen des Vorgangs erforderlich. Statt denken CPU, e/a und Speicher und wie sie Ihre Anwendungsdurchsatz auswirken, können Sie ein einzelnes Measure RU denken.

Jede Auflistung DocumentDB kann mit bereitgestellten Durchsatz in RUs Durchsatz pro Sekunde reserviert werden. Anwendungsmöglichkeiten von Dezimalstellenanzahl können Wünsche ihrer RU Werte, und Bereitstellung Sammlungen mit der Summe der anforderungseinheiten über alle Anfragen benchmark. Sie vergrößern oder Verkleinern der Auflistung Durchsatz müssen Ihre Anwendung entwickeln. Weitere Informationen zu anforderungseinheiten mit und Hilfe muss Auflistung bestimmen, Bitte lesen [Verwalten von Leistung und Kapazität](documentdb-manage.md) und der [Durchsatz Rechner](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>Ist HIPAA DocumentDB kompatibel?
DocumentDB ist HIPAA kompatibel. HIPAA richtet bei Benutzung Offenlegung und persönlichen Gesundheitsdaten. Weitere Informationen finden Sie im [Microsoft-Sicherheitscenter](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Was sind die Speichergrenzwerte DocumentDB? 
Gibt es keine theoretische Grenze der Gesamtgröße der Daten, die in DocumentDB speichern können. Möchten Sie über 250 GB Daten in einer Auflistung speichern, bitte [wenden Sie sich](documentdb-increase-limits.md) Ihr Konto Quote erhöht. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Was die Durchsatz DocumentDB sind? 
Keine theoretische Grenze der Gesamtgröße des Durchsatzes, die eine Auflistung in DocumentDB unterstützen ist Workload ungefähr gleichmäßig eine ausreichend große Anzahl Partitionsschlüssel verteilt werden kann. Wenn Sie mehr als 250.000 Einheiten pro Sekunde pro Auflistung oder Konto anfordern möchten, geben Sie Ihr Konto Quote erhöht [wenden](documentdb-increase-limits.md) . 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Wie viel kostet Microsoft Azure DocumentDB?
Finden Sie auf der Seite [DocumentDB Preisangaben](https://azure.microsoft.com/pricing/details/documentdb/) Details. DocumentDB Nutzungsgebühren festgelegten Anzahl Sammlungen verwendet, die Anzahl der Stunden, die Sammlungen online sind, und der belegte Speicher und bereitgestellten Durchsatz für jede Auflistung. 

### <a name="is-there-a-free-account-available"></a>Gibt es ein kostenloses Konto?
Neue Azure hingegen können Sie sich für ein [kostenloses Konto Azure](https://azure.microsoft.com/free/)anmelden 30 Tage und $200 versuchen Azure Services ermöglicht. Oder wenn Sie ein Visual Studio-Abonnement haben, Sie für [150 kostenlose Azure Credits pro Monat](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) auf Azure Service.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Wie kann ich zusätzliche Hilfe zu DocumentDB?
Wenn Sie Hilfe benötigen, erreichen Sie uns auf [Stapelüberlauf](http://stackoverflow.com/questions/tagged/azure-documentdb) [Azure DocumentDB MSDN Developer Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)oder planen Sie eine [1:1-Chat mit dem DocumentDB-Entwicklungsteam](http://www.askdocdb.com/). Um auf DocumentDB Neuigkeiten und Features bleiben folgen Sie uns auf [Twitter](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB einrichten

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Wie kann ich Anmeldung für Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB steht in [Azure-Portal][azure-portal].  Zunächst müssen Sie für Microsoft Azure-Abonnement anmelden.  Nach der Anmeldung für Microsoft Azure-Abonnement können Sie Ihre Azure-Abonnement DocumentDB-Konto hinzufügen. Finden Sie Informationen zum Hinzufügen eines Kontos DocumentDB [DocumentDB Datenbankkonto erstellen](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Was ist ein Hauptschlüssel?
Ein Hauptschlüssel ist ein Sicherheitstoken Zugriff auf alle Ressourcen für eine Firma. Personen mit dem Schlüssel haben Lese- und Schreibzugriff auf alle Ressourcen in das Konto. Vorsicht Wenn Sie Hauptschlüssel verteilen. Hauptschlüssel primäre und sekundäre Hauptschlüssel stehen in **Schlüssel **Blade [Azure-Portal][azure-portal]. Weitere Informationen über Schlüssel finden Sie unter [anzeigen, kopieren und Regenerieren Zugriffstasten](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Erstellen eine Datenbank
Sie können Datenbanken mithilfe von [Azure-Portal]() unter [DocumentDB Datenbank erstellen](documentdb-create-database.md), [DocumentDB SDKs](documentdb-sdk-dotnet.md)oder über die [REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx)erstellen.  

### <a name="what-is-a-collection"></a>Was ist eine Auflistung?
Eine Auflistung ist ein Container für JSON-Dokumente und zugeordnete JavaScript Anwendungslogik. Eine Auflistung ist eine fakturierbare Entität, wobei die [Kosten](documentdb-performance-levels.md) anhand des Durchsatzes und storaged verwendet. Sammlungen können können ein oder mehrere Partitionen/Server umfassen und um Speicher oder Durchsatz praktisch unbegrenzte Datenmengen verarbeiten.

Sammlungen sind auch die Abrechnung Entitäten für DocumentDB. Jede Sammlung wird stündlich basierend auf den bereitgestellten Durchsatz und belegten Speicherplatzes abgerechnet. Weitere Informationen finden Sie in der [DocumentDB Preisgestaltung](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-set-up-users-and-permissions"></a>Wie kann ich Benutzer und Berechtigungen einrichten?
Sie können Benutzer und Berechtigungen mit [DocumentDB SDKs](documentdb-sdk-dotnet.md) oder [Anderen APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx)erstellen.   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Datenbank der Entwicklung mit Microsoft Azure DocumentDB Fragen

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Wie soll zunächst gegen DocumentDB entwickeln?
[SDKs](documentdb-sdk-dotnet.md) sind für .NET, Python, Node.js, JavaScript und Java.  Entwickler können auch [RESTful HTTP-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) Interaktion mit DocumentDB Ressourcen aus einer Vielzahl von Plattformen und Sprachen. 

Beispiele für DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)und [Python](documentdb-python-samples.md) -SDKs finden Sie auf GitHub.

### <a name="does-documentdb-support-sql"></a>Unterstützt DocumentDB SQL?
DocumentDB SQL-Abfragesprache ist eine erweiterte Teilmenge von Abfragefunktionen von SQL unterstützt. DocumentDB SQL-Abfragesprache stellt Rich hierarchische und relationale Operatoren und Erweiterbarkeit über JavaScript-basierte Benutzer definierten Funktionen (UDF). JSON-Grammatik ermöglicht die Modellierung JSON-Dokumente als Strukturen mit als Strukturknoten, die von sowohl die DocumentDB automatische Indizierung sowie SQL Query Dialekt des DocumentDB verwendet wird.  Informationen zur Verwendung von SQL-Grammatik finden Sie unter [Query DocumentDB] [ query] Artikel.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Was sind die Datentypen von DocumentDB unterstützt?
DocumentDB unterstützten die primitiven Datentypen entsprechen JSON. JSON ist ein einfacher Typ-System, das aus Zeichenfolgen, Zahlen (IEEE754 mit doppelter Genauigkeit) und Boolean - true, False und NULL.  Komplexere Datentypen DateTime, Guid, Int64 und Geometrie können durch geschachtelte Objekte mit {Operator und Arrays mit dem Operator [] in JSON und DocumentDB dargestellt werden. 

### <a name="how-does-documentdb-provide-concurrency"></a>Wie bietet DocumentDB Parallelität?
DocumentDB unterstützt die Steuerung durch vollständige Parallelität (OCC) über HTTP Entitäts-Tags oder Etags. Jede Ressource DocumentDB hat ein ETag-Header und das Etag jeder Aktualisierung ein Dokuments auf dem Server festgelegt ist. Der Etag-Header und den aktuellen Wert aller Antwortnachrichten gehören. Etags mit If-Match-Header lässt sich durch Server entscheiden, ob eine Ressource aktualisiert werden soll. Der If-Match-Wert ist der Etag-Wert verglichen werden. Das Etag Server Etag-Wert übereinstimmt, wird die Ressource aktualisiert. Das Etag nicht mehr aktuell ist, lehnt der Server den Vorgang mit einer "HTTP 412 Vorbedingung Fehler" Antwortcode. Die Clients müssen müssen Sie die Ressource für den aktuellen Etag-Wert für die Ressource abgerufen. Darüber hinaus können Etags mit If-None-Match-Header bestimmen ggf. erneut Abrufen einer Ressource verwendet werden. 

Parallelität in .NET zu verwenden, verwenden Sie die [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) -Klasse. Ein Beispiel .NET finden Sie in ["Program.cs"](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) im Beispiel Dokumentenverwaltung auf Github.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Wie werden Transaktionen in DocumentDB ausgeführt?
DocumentDB unterstützt LINQ-Transaktionen über JavaScript gespeicherte Prozeduren und Trigger. Gültigkeitsbereich der Auflistung ist eine Auflistung einer Partition oder Dokumente mit demselben Schlüsselwert Partition innerhalb einer Auflistung partitioniert die Auflistung ausgelassen werden alle Datenbankoperationen in Skripts ausgeführt. Ein Snapshot von Dokumentversionen (ETags) am Anfang der Transaktion und nur, wenn das Skript erfolgreich ausgeführt wurde. Wenn JavaScript Fehler auslöst, wird die Transaktion zurückgesetzt. Weitere Informationen finden Sie unter [DocumentDB serverseitige Programmierung](documentdb-programming.md) .

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Wie kann ich einfügen Dokumente in DocumentDB gebündelt? 
Es gibt drei Methoden zum Massenkopieren einfügen Dokumente in DocumentDB:

- Das Migrations-Tool unter [Daten importieren auf DocumentDB](documentdb-import-data.md).
- Document Explorer im Azure-Portal unter [Bulk Dokumente mit Document Explorer hinzufügen](documentdb-view-json-document-explorer.md#BulkAdd).
- Gespeicherte Prozeduren, wie in [DocumentDB serverseitige Programmierung](documentdb-programming.md)beschrieben.

### <a name="does-documentdb-support-resource-link-caching"></a>Unterstützt DocumentDB Zwischenspeichern der Ressource verknüpfen?
Ja, da DocumentDB eines Rest-Diensts, Ressourcenlinks sind unveränderlich und zwischengespeichert werden können. DocumentDB-Clients können einen Header "If-None-Match" Lesevorgänge für jede Ressource wie Dokument oder Auflistung aktualisieren ihre lokale Kopien hat nur wenn die Serverversion ändern. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Gibt eine lokale Instanz von DocumentDB?
Eine lokale Instanz von DocumentDB ist zurzeit nicht verfügbar. Sie können den Status der lokalen Emulator und Abstimmung dafür [Bewertungsportal](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance)verfolgen.


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
