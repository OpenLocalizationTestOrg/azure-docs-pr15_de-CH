<properties 
    pageTitle="Einführung in JSON-Datenbank DocumentDB | Microsoft Azure" 
    description="Erfahren Sie mehr über Azure DocumentDB JSON NoSQL-Datenbank. Diese Datenbank wurde für big Data, flexible Skalierbarkeit und hohe Verfügbarkeit entwickelt." 
    keywords="JSON-Datenbank Datenbank"
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
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Einführung in DocumentDB: JSON NoSQL-Datenbank

##<a name="what-is-documentdb"></a>Was ist DocumentDB?

DocumentDB ist ein vollständig verwalteter NoSQL-Datenbank für schnelle und vorhersehbare Leistung, hohe Verfügbarkeit, flexible Skalierung, globale Verteilerlisten und einfache Entwicklung erstellt. Als Schema frei NoSQL-Datenbank bietet DocumentDB reiche und vertraute SQL Abfragefunktionen mit konsistenten niedrige Wartezeiten auf JSON Daten - 99 % der Lesevorgänge unter 10 Millisekunden und 99 % der Schreibvorgänge bereitgestellt werden 15 Millisekunden bereitgestellt werden. Diese Vorteile machen DocumentDB sehr fit für mobile Web spielen und IoT und vielen anderen Programmen, die nahtlose Skalierung und globale Replikation.

## <a name="how-can-i-learn-about-documentdb"></a>Wie kann ich über DocumentDB erfahren? 

Schnell DocumentDB lernen und erleben sie ist diese drei Schritte: 

1. Sehen Sie sich zwei Minuten [Was ist DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) Video, die Vorteile von DocumentDB vorgestellt.
2. Sehen Sie drei Minuten [DocumentDB auf Azure erstellen](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) video, wodurch mit DocumentDB Schritte mit Azure-Verwaltungsportal hervorgehoben.
3. Besuchen Sie die [Abfrage Spielplatz](http://www.documentdb.com/sql/demo), wo durchlaufen verschiedene Aktivitäten umfangreiche Abfragen Funktionen in DocumentDB Informationen. Dann gehen Sie zur Registerkarte Sandbox führen Sie eigene benutzerdefinierten SQL-Abfragen aus und experimentieren Sie mit DocumentDB.

Dann zurück zu diesem Artikel, werden wir tiefer.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Welche Funktionen und die wichtigsten Features bietet DocumentDB?  

Azure DocumentDB bietet die folgenden wichtigsten Funktionen und Vorteile:

-   **Elastisch skalierbare Durchsatz und Speicher:** Einfach vergrößern oder Verkleinern der Datenbank DocumentDB JSON Anwendung benötigen. Solid-State-Laufwerke (SSD) für niedrige vorhersagbare Wartezeiten auf Ihre Daten. DocumentDB unterstützt Container zum Speichern von JSON-Daten praktisch unbegrenzten Speichergröße und bereitgestellten Durchsatz skalieren können Sammlungen bezeichnet. Sie können elastisch DocumentDB vorhersagbare Leistung nahtlos skalieren Wachstum die Anwendung. 

-   **Mit mehreren Replikation:** DocumentDB repliziert Daten transparent für alle Bereiche, die Sie Ihr Konto DocumentDB können Clientanwendungen entwickeln, die globalen Daten zugreifen zugeordnet haben und Kompromisse zwischen Konsistenz, Verfügbarkeit und Leistung mit entsprechenden Garantien bietet. DocumentDB bietet transparente regionale Failover mit Multi-homing APIs und elastisch Durchsatz und Speicher weltweit skalieren. Erfahren Sie mehr in [Daten mit DocumentDB](documentdb-distribute-data-globally.md).

-   **Ad-hoc-Abfragen mit vertrauten SQL-Syntax:** Heterogene JSON-Dokumente in DocumentDB speichern und diese Dokumente über eine vertraute SQL-Syntax Abfragen. DocumentDB verwendet eine hochgradig parallele Sperre frei, Protokoll strukturiert Indizierung Technologie alle Dokumentinhalt indiziert. Dadurch werden umfassende Echtzeit-Abfragen ohne Schema Hinweise, sekundäre Indizes oder Ansichten angeben. Erfahren Sie mehr in [Abfrage DocumentDB](documentdb-sql-query.md). 

-   **Ausführung von JavaScript in der Datenbank:** Gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen (UDFs) mit standard-JavaScript drücken Sie Anwendungslogik. Dadurch wird die Anwendungslogik Daten arbeiten, ohne den Konflikt zwischen der Anwendung und das Datenbankschema. DocumentDB bietet vollständige transaktionale Ausführung von JavaScript Anwendungslogik direkt in die Datenbank-Engine. Tiefe Integration von JavaScript ermöglicht die Ausführung von einfügen, ersetzen, löschen und wählen Sie Vorgänge in ein JavaScript-Programm als isolierte Transaktion. Weitere Informationen finden Sie in [DocumentDB serverseitige Programmierung](documentdb-programming.md).

-   **Einstellbaren konsistenzebenen:** Vier konsistenzebenen von gut definierten zu optimalen Kompromiss zwischen Konsistenz und Leistung auswählen. Für Abfragen und Lesevorgänge DocumentDB bietet vier verschiedene Konsistenz: starke, begrenzt Veraltung Sitzung und. Diese detaillierte und konsistenzebenen können Sie sound Kompromisse zwischen Konsistenz, Verfügbarkeit und Wartezeit. Weitere [mit Konsistenz zum Maximieren der Verfügbarkeit und Leistung in DocumentDB](documentdb-consistency-levels.md).

-   **Vollständig verwaltet:** Entfällt die Datenbank auch Computer verwalten. Als vollständig verwaltet Microsoft Azure-Dienst Sie nicht brauchen, virtuelle Computer verwalten, bereitstellen und Konfigurieren der Software skalieren zu verwalten, oder komplexe Datenebene Upgrades beschäftigen. Jede Datenbank wird automatisch gesichert und regionale Ausfälle geschützt. Sie können ein DocumentDB-Konto hinzufügen und Kapazität nach Bedarf, können Sie sich für Ihre Anwendung anstelle der Betrieb und die Verwaltung Ihrer Datenbank bereitstellen. 

-   **Entwurf geöffnet:** Erste Schritte mit Tools und Kenntnisse. Programmieren mit DocumentDB ist einfach, zugänglich und erfordert keinen neuen Tools oder benutzerdefinierte Erweiterung JSON oder JavaScript entsprechen. Sie können alle Datenbankfunktionen einschließlich CRUD, Abfrage und JavaScript Verarbeitung über eine einfache Schnittstelle RESTful HTTP zugreifen. DocumentDB umfasst Formaten, Sprachen und Standards und bietet hochwertige Datenbankfunktionen auf.

-   **Automatische Indizierung:** In der Standardeinstellung DocumentDB [automatisch indiziert](documentdb-indexing.md) alle Dokumente in der Datenbank und nicht erwarten oder Schema oder Erstellung von sekundären Indizes benötigt. Möchten Sie alles indizieren? Keine Sorge, können Sie [Pfade in den JSON-Dateien deaktivieren](documentdb-indexing-policies.md) zu.

##<a name="data-management"></a>Wie verwaltet DocumentDB Daten?

Azure DocumentDB verwaltet JSON-Daten über klar definierte Ressourcen. Diese Ressourcen für hohe Verfügbarkeit repliziert und ihre logischen URI eindeutig adressiert werden. DocumentDB bietet eine einfache HTTP Rest Programmiermodell für alle Ressourcen. 

Konto der DocumentDB ist ein eindeutiger Namespace, der Azure DocumentDB enthält. Bevor Sie ein Konto erstellen können, müssen Sie ein Azure-Abonnement Sie an Azure Services zugreifen. 

Alle Ressourcen in DocumentDB werden modelliert und als JSON-Dokumente gespeichert. Verwaltete Ressourcen werden Elemente mit Metadaten sind JSON dokumentiert und als feeds sind Sammlungen von Elementen. Elemente sind in ihren jeweiligen Feeds enthalten.

Die Abbildung unten zeigt die Beziehung zwischen DocumentDB Ressourcen:

![Die hierarchische Beziehung zwischen Ressourcen in DocumentDB JSON NoSQL-Datenbank][1] 

Datenbankkonto besteht aus einer Reihe von Datenbanken, jeweils mehrere Sammlungen, die jeweils gespeicherte Prozeduren, Trigger, UDFs, Dokumente und zugehörigen Anlagen enthalten kann. Eine Datenbank hat auch Benutzer mit Zugriffsberechtigungen für verschiedene andere Sammlungen, gespeicherte Prozeduren, Trigger, UDFs, Dokumente oder Anlagen zugeordnet. Datenbanken, Benutzer, Berechtigungen und Sammlungen vom System definierte Ressourcen mit bekannten Schemas werden-Dokumente, gespeicherte Prozeduren, Trigger, UDFs und Anhänge beliebigen enthalten definierten Benutzer JSON-Inhalt.  

##<a name="develop"></a>Wie können apps mit DocumentDB entwickeln?

Azure DocumentDB stellt Ressourcen über eine REST-API, die von jeder Sprache, die HTTP/HTTPS-Anforderungen aufgerufen werden kann. Darüber hinaus bietet DocumentDB Programmierbibliotheken für verschiedene gängige Sprachen. Diese Bibliotheken vereinfachen viele Aspekte der Arbeit mit Azure DocumentDB von Details wie Adresse Zwischenspeichern ausnahmemanagement, automatische Wiederholungsversuche und behandeln. Bibliotheken sind derzeit für die folgenden Sprachen und Plattformen:  

Herunterladen | Dokumentation
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [.NET library](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js-SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [Node.js-Bibliothek](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Java-Bibliothek](http://azure.github.io/azure-documentdb-java/)
[JavaScript-SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [JavaScript-Bibliothek](http://azure.github.io/azure-documentdb-js/)
n/a | [Serverseitige JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Python-Bibliothek](http://azure.github.io/azure-documentdb-python/)

Darüber hinaus grundlegende erstellen, lesen, aktualisieren und Löschen von Operationen DocumentDB bietet eine umfangreiche SQL-Abfrageschnittstelle für JSON-Dokumente abrufen und serverseitige Unterstützung für Transaktions-Execution JavaScript Anwendungslogik. Die Abfrage und Skript Ausführung sind über alle Plattformen sowie die übrigen APIs verfügbar. 

### <a name="sql-query"></a>SQL-Abfrage
Azure DocumentDB unterstützt Dokumente mit einer SQL-Sprache JavaScript-Typsystem und Ausdrücke mit Unterstützung für relationale hierarchische und räumliche Abfragen WURZELT Abfragen. DocumentDB-Abfragesprache ist eine einfache, aber leistungsstarke Benutzeroberfläche für Abfrage JSON-Dokumente. Die Sprache unterstützt eine Teilmenge der ANSI SQL-Grammatik und umfassende Integration von JavaScript-Objekt, Arrays Objektkonstruktion und Funktionsaufruf hinzugefügt. DocumentDB stellt die Abfrage ohne explizite Schema oder Index-Hints vom Entwickler.

Benutzer definierten Funktionen (UDF) mit DocumentDB registriert und wird als Teil einer SQL-Abfrage und erweitert die Grammatik zur Unterstützung der benutzerdefinierten Anwendungslogik. Diese UDF als JavaScript-Programme geschrieben und in der Datenbank ausgeführt. 

Für .NET Entwickler bietet DocumentDB einen LINQ-Abfrageanbieter als Teil des [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx). 

### <a name="transactions-and-javascript-execution"></a>Transaktionen und JavaScript-Ausführung
DocumentDB können Sie Logik als benannte vollständig in JavaScript geschriebene Programme schreiben. Diese Programme sind für eine Auflistung registriert und Datenbankoperationen auf Dokumente in einer bestimmten Auflistung ausstellen können. JavaScript kann für die Ausführung als Trigger, gespeicherte Prozedur oder benutzerdefinierte Funktion registriert werden. Trigger und gespeicherte Prozeduren können erstellen, lesen, aktualisieren und löschen die benutzerdefinierten Funktionen als Teil der Abfrage Ausführung ohne Schreibzugriff auf die Auflistung ausgeführt.

Die Konzepte von relationalen Datenbanksystemen mit JavaScript als moderner Ersatz für Transact-SQL unterstützt JavaScript-Ausführung in DocumentDB nachempfunden. Alle JavaScript-Logik wird innerhalb einer ambient ACID-Transaktion mit Snapshot-Isolation ausgeführt. Verlauf der Ausführung Wenn JavaScript eine Ausnahme auslöst, wird so die gesamte Transaktion abgebrochen.

## <a name="next-steps"></a>Nächste Schritte
Haben Sie bereits ein Azure-Konto? Dann kann mit DocumentDB in [Azure-Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB) erstellen Sie [ein Konto DocumentDB](documentdb-create-account.md)Einstieg.

Sie haben kein Azure-Konto? Sie können:

- Melden Sie sich für ein [Azure Testversion](https://azure.microsoft.com/free/)30 Tage und $200 versuchen Azure Services ermöglicht. 
- Wenn Sie ein MSDN-Abonnement verfügen, können Sie für [150 kostenlose Azure Credits pro Monat](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) auf Azure Service verwenden. 

Wenn Sie mehr erfahren möchten, besuchen Sie dann unseren [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) alle verfügbaren Lernressourcen zu navigieren. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
