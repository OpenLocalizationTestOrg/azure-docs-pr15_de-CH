<properties
   pageTitle="Skalierbare Anwendung | Azure Referenzarchitektur | Microsoft Azure"
   description="Verbesserung der Skalierbarkeit einer Web-Anwendung in Microsoft Azure ausgeführt."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>Verbesserung der Skalierbarkeit einer Web-Anwendung 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel zeigt eine empfohlene Architektur zur Skalierung und Leistung in einer Web-Anwendung, die auf Microsoft Azure ausgeführt. Die Architektur baut auf [Azure Referenzarchitektur: einfache Anwendung][basic-web-app]. Vorschläge und Hinweise aus diesem Artikel gelten für diese Architektur.

>[AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: Ressourcen-Manager und Classic. In diesem Artikel verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

## <a name="architecture-diagram"></a>Architekturdiagramm

![[0]][0]

Die Architektur besteht aus folgenden Komponenten:

- **Ressourcengruppe**. Eine [Ressourcengruppe] [ resource-group] ist ein logischer Container für Azure-Ressourcen. 

- ** [WebApp] [ app-service-web-app] ** und ** [API-app][app-service-api-app]**. Typisch modernen gehören sowohl eine Website und eine oder mehrere RESTful APIs. Web-API kann von Browserclients durch AJAX, systemeigenen Clientanwendungen oder serverseitige Anwendung genutzt werden. Aspekte Designing Web APIs finden Sie unter [API-Designrichtlinien][api-guidance].    

- **Webauftrag**. Verwenden von [Azure Webaufträge] [ webjobs] zeitintensiven Aufgaben im Hintergrund ausgeführt. Webaufträge läuft nach einem Zeitplan kontinuierlich, oder auf einen Trigger, wie eine Nachricht in einer Warteschlange ablegen. Ein Webauftrag wird im Kontext der App Service-app im Hintergrund ausgeführt. 

- **Warteschlange**. In der hier gezeigten Anwendungswarteschlangen Hintergrundaufgaben mit einer Nachricht auf einem [Azure Queue Storage] [ queue-storage] Warteschlange. Die Nachricht wird eine Funktion in der Webauftrag. 

    Alternativ können Sie Servicebuswarteschlangen. Einen Vergleich finden Sie in [Azure und Service Bus-Warteschlangen - verglichen und gegenübergestellt][queues-compared].

- **Cache**. Semistatische Datenspeicher in [Azure Redis Cache][azure-redis].  

- **CDN**. Verwenden von [Azure Content Delivery Network] [ azure-cdn] (CDN) an öffentlich zugänglichen Inhalt Zwischenspeichern für geringere Latenz und schnellere Bereitstellung von Inhalten.

- **Speicherung von Daten.** Verwenden von [Azure SQL-Datenbank] [ sql-db] für relationale Daten. Nicht-relationalen Daten sollten Sie einen Informationsspeicher NoSQL Azure-Tabellenspeicher oder [DocumentDB][documentdb].

- **Azure suchen**. [Azure] suchen[ azure-search] Suchfunktionen, einschließlich Vorschläge, fuzzy-Suche und sprachspezifische Suche hinzufügen. Azure Suche wird normalerweise in Verbindung mit einem anderen Datenspeicher verwendet, insbesondere, wenn der primäre Datenspeicher strikten Konsistenz erforderlich ist. Bei dieser Vorgehensweise würde die autorisierenden Daten im anderen Datenspeicher und Azure Search Suchindex gebracht. Azure Suche kann auch einen einzelnes Index aus mehreren Datenspeichern konsolidieren verwendet werden.  

- **E-Mail-SMS**. Wenn Ihre Anwendung e-Mail oder SMS senden, verwenden Sie einen Drittanbieterdienst wie SendGrid oder Twilio, diese Funktion direkt in der Anwendung erstellen.

## <a name="recommendations"></a>Empfehlung

### <a name="app-service-apps"></a>App Service apps 

Erstellen der Anwendung und Web-API als separate App Service apps empfohlen. Dadurch können Sie separate App Service-Pläne führen wiederum können Sie diese unabhängig voneinander zu skalieren. Wenn dieser Ebene Skalierbarkeit benötigen, können Sie Planes apps bereitstellen und verschieben sie in separaten Plänen später bei Bedarf. (Basic, Standard und Premium-Pläne sind die VM-Instanzen im Plan nicht pro Anwendung berechnet. [App Servicepreise]Siehe[app-service-pricing])

Wenn Sie *Einfache Tabellen* oder *Einfach APIs* Features der App Service Mobile Apps verwenden möchten, erstellen Sie eine separate App Service-app für diesen Zweck.  Diese Features beruhen auf spezifische Anwendung zu ermöglichen.

### <a name="webjobs"></a>Webaufträge

Sollten Sie der Webauftrag ressourcenintensiv in einem separaten App Service-Plan für den Webauftrag bestimmte Instanzen zu einer leeren App Service-Anwendung bereitstellen. [Hintergrund Aufträge Anleitung]finden Sie unter[webjobs-guidance].  

### <a name="cache"></a>Cache

Verbessern von Leistung und Erweiterbarkeit [Azure Redis Cache] mit[ azure-redis] einige Daten. Redis Cache in Betracht:

- Semistatische Transaktionsdaten.

- Sitzungsstatus.

- HTML-Ausgabe. Dies kann nützlich sein, die komplexe HTML-Ausgabe dargestellt. 

Ausführlichere Hinweise zum Entwerfen einer Zwischenspeicherungsstrategie Siehe [Anleitung Zwischenspeichern][caching-guidance].

### <a name="cdn"></a>CDN 

Verwenden von [Azure CDN] [ azure-cdn] um statische Inhalte zwischenzuspeichern. Der Hauptvorteil eines CDN werden Latenz für Benutzer, da Inhalt ein *Edge-Server* zwischengespeichert, geografisch in der Nähe der Benutzer. CDN kann auch Auslastung der Anwendung reduzieren, da der Datenverkehr nicht von der Anwendung behandelt wird.

- Wenn Ihre Anwendung hauptsächlich aus statischen Seiten besteht, sollten Sie CDN die gesamte Anwendung zwischengespeichert. Siehe [Verwenden von Azure CDN in Azure App Service][cdn-app-service].

- Andernfalls Dateien statische Inhalte wie Bilder, CSS und HTML-, in Azure Storage und CDN verwenden diese Dateien. Finden Sie unter [integrieren ein Speicherkonto mit EUR][cdn-storage-account].

> [AZURE.NOTE] Azure CDN kann keine Inhalte bereitstellen, die Authentifizierung erfordert.

Ausführliche Hinweise finden Sie unter [Anleitung Content Delivery Network (CDN)][cdn-guidance]. 

### <a name="storage"></a>Speicher

Moderne Bearbeitung häufig große Datenmengen. Um die Skalierung für die Cloud ist es wichtig, den richtigen Typ auswählen. Hier sind einige empfohlene Grundlinie.  Ausführliche Hinweise finden Sie unter [Bewertung Daten Funktionen für mehrsprachige Persistenz Solutions][polyglot-storage].

Was möchten Sie speichern | Beispiel | Empfohlene
--- | --- | ---
Dateien | Bilder, Dokumente, PDF-Dateien | Azure BLOB-Speicher
Schlüssel-Wert-Paare | Benutzerprofildaten gesucht nach Benutzer ID | Azure Table Storage
Kurze Nachrichten zur Weiterverarbeitung auslösen | Anfragen | Azure Queue Storage Service Bus-Warteschlange und Service Bus-Thema
Nicht-relationalen Daten mit einem flexiblen Schema erfordert grundlegende Abfragen | Produktkatalog | Datenbank, oder Azure DocumentDB, MongoDB, CouchDB Apache
Relationale Daten, umfangreichere Unterstützung, strenge Schema und starke Konsistenz | Warenbestand | SQL Azure-Datenbank

## <a name="scalability-considerations"></a>Aspekte der Skalierung

### <a name="app-service-app"></a>App Service-app

Enthält die Projektmappe mehrere App Service apps, erwägen Sie, diese separate App Service-Pläne. Dadurch können sie unabhängig voneinander skalieren auf separaten Instanzen durchgeführt. Weitere Informationen zur Skalierung finden Sie unter [Skalierbarkeit Aspekte] [ basic-web-app-scalability] Abschnitt der [Anwendungsarchitektur grundlegende Web][basic-web-app].

Ebenso erwägen Sie ein Webauftrag in einen eigenen Plan, Hintergrundaufgaben auf dieselben Instanzen auszuführen nicht, die HTTP-Anfragen.  

### <a name="sql-database"></a>SQL-Datenbank

Erhöhen der Skalierbarkeit einer SQL-Datenbank durch *Sharding* der Datenbank &mdash; also horizontal partitionieren der Datenbank. Sharding können Sie die Datenbank horizontal skalieren mit [elastische Datenbanktools][sql-elastic]. Vorteile von Sharding gehören:

- Bessere Transaktionsdurchsatz.

- Abfragen können über eine Teilmenge der Daten schneller ausgeführt. 

### <a name="azure-search"></a>Azure Suche

Azure Search entfernt den Aufwand für komplexe Suche aus dem primären Datenspeicher und skalierbar um zu handhaben. Siehe [Skalieren Ressource für Abfrage und Indizierung Arbeitslasten in Azure Suche][azure-search-scaling].

## <a name="security-considerations"></a>Sicherheitsaspekte

### <a name="cross-origin-resource-sharing-cors"></a>Cross-Ursprung Ressourcenfreigabe (CORS)

Wenn Sie eine Website und Web-API als separate apps erstellen, die Website clientseitige AJAX-Aufrufe von der API nicht möglich CORS aktivieren. 

> [AZURE.NOTE] Browsersicherheit wird verhindert, dass eine Webseite AJAX-Anfragen in eine andere Domäne. Diese Einschränkung heißt same Origin Policy und verhindert, dass eine böswillige Website wie Daten von einer anderen Website lesen. CORS ist ein W3C-Standard, mit dem einen Server die same Origin Policy und andere ablehnen können einige Anfragen Cross-Ursprung.

Anwendungsdienste bietet integrierte Unterstützung für CORS, ohne Anwendungscode schreiben. Siehe [verbrauchen eine API-app von JavaScript mit CORS][cors]. Hinzufügen der Website zur Liste der zulässigen Ursprung für die API. 

### <a name="sql-database-encryption"></a>SQL-Datenbank-Verschlüsselung

[Transparente Verschlüsselung] verwenden[ sql-encryption] Daten in der Datenbank verschlüsselt werden soll. Diese Funktion führt in Echtzeit Ver- und Entschlüsselung der gesamten Datenbank (einschließlich Backups und Transaktionsprotokolldateien), ohne Änderung der Anwendung. Verschlüsselung einiger Wartezeit hinzufügen sollten die Daten trennen, die in eine eigene Datenbank sicherer, und aktivieren Sie die Verschlüsselung für die Datenbank.  

## <a name="next-steps"></a>Nächste Schritte

- Für höhere Verfügbarkeit die Anwendung in mehreren Regionen bereitstellen und [Azure Traffic Manager] verwenden[ tm] für Failover. Weitere Informationen finden Sie unter [Azure Referenzarchitektur: Web-Anwendung mit hoher Verfügbarkeit][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Web-Anwendung in Azure verbesserte Skalierbarkeit"