<properties
   pageTitle="Fehleranalyse Modus | Microsoft Azure"
   description="Richtlinien für die Analyse Fehler Modus für Azure Cloudlösungen."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Azure Resiliency Anleitung: Modus Fehleranalyse

Modus Fehleranalyse (FMA) ist ein Prozess zum Erstellen von Stabilität in einem System mögliche Fehlerquellen im System identifiziert. FMA sollte Bestandteil der Architektur und Design-Phasen, sodass erstellen Wiederherstellung in das System ab.

Hier ist der allgemeine Prozess ein FMA durchzuführen: 

1. Identifizieren Sie aller Komponenten im System. Können Sie externe Faktoren wie als Identitätsanbieter Fremdanbieterdienste, und.   

2. Identifizieren Sie für jede Komponente potenzielle Fehler auftreten. Eine einzelne Komponente möglicherweise mehrere Fehlermodus. Beispielsweise Sie sollten Sie lesen Fehler und Ausfälle da Auswirkung und mögliche Gegenmaßnahmen werden separat schreiben.

3. Bewerten Sie jeden Fehlermodus nach dessen Gesamtrisiko. Berücksichtigen Sie folgende Faktoren:  

    - Was ist die Wahrscheinlichkeit des Fehlers. Ist es relativ häufig? Selten Extrememly? Sie brauchen keine genaue Zahlen; soll Hilfe die Priorität zu bewerten.

    - Was ist die Auswirkung auf die Anwendung hinsichtlich Verfügbarkeit, Datenverlust, Kosten und Geschäftsbetriebs? 

4. Bestimmen Sie bei wie Reaktion und Wiederherstellung die Anwendung. Berücksichtigen Sie Kompromisse Kosten und Anwendung komplexer.   

Als Ausgangspunkt für den Prozess FMA enthält dieser Artikel einen Katalog von möglichen Fehlermodi und ihre Schutzmaßnahmen. Technologie oder Azure Service plus eine allgemeine Kategorie für die Anwendungsebene Design Katalog organisiert. Der Katalog ist nicht vollständig, aber umfasst viele der Azure Core Services. 


## <a name="app-service"></a>App Service

### <a name="app-service-app-shuts-down"></a>App Service-app wird beendet.

**Erkennung**. Mögliche Ursachen:

- Erwartete Herunterfahren
    
    - Ein Operator schließt die Anwendung; verwenden z. B. Azure-Portal.

    - Die Anwendung wurde entladen, da im Leerlauf war. (Nur, wenn die `Always On` ist deaktiviert.)

- Unerwartetes Herunterfahren

    - Die Anwendung stürzt ab.

    - Eine App Service VM-Instanz verfügbar.


Application_End Protokollierung fängt die Anwendung schließen (soft Prozessabsturz) und ist die einzige Möglichkeit zu Application Domain Herunterfahren.

**Wiederherstellung**

- Erwartete Herunterfahren verwenden Sie Shutdown-Ereignis der Anwendung ordnungsgemäß heruntergefahren. In ASP.NET, z. B. die `Application_End` Methode.

- Wenn die Anwendung im Leerlauf wurde, wird sie automatisch bei der nächsten Anforderung gestartet. Allerdings wird den "Kaltstart" Kosten entstehen. 

- Damit wird die Anwendung im Leerlauf entladen, Aktivieren der `Always On` im Web app. [Konfigurieren webapps in Azure App Service]finden Sie unter[app-service-configure].

- Vermittlung der Anwendung heruntergefahren festgelegt Ressourcensperre mit `ReadOnly` auf. [Sperrenressourcen mit Azure-Ressourcen-Manager]finden Sie unter[rm-locks].

- Wenn die Anwendung zum Absturz oder eine App Service VM ausfällt, App Dienst startet die Anwendung automatisch neu. 


**Diagnose**. Anwendung und Server Webprotokollen. [Aktivieren das Diagnoseprotokoll für Web-apps in Azure App Service]finden Sie unter[app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Ein bestimmten Benutzer wiederholt falsche fordert oder das System überlastet. 

**Erkennung**. Authentifizieren von Benutzern und Benutzer-ID in Anwendungsprotokolle.

**Wiederherstellung**

- Verwenden von [Azure API Management] [ api-management] Gas Anfragen vom Benutzer. Siehe [Erweiterte Anforderung mit Azure API Management Drosselung][api-management-throttling]

- Blockieren Sie den Benutzer.

**Diagnose**. Protokollieren Sie alle Authentifizierungsanfragen.


### <a name="a-bad-update-was-deployed"></a>Fehlerhafte Aktualisierung wurde bereitgestellt.

**Erkennung**. Überprüfen der Azure-Portal Anwendung ( [Monitor Azure app]Webleistungstest[app-insights-web-apps]) oder den [Health Endpunkt Überwachung Muster][health-endpoint-monitoring-pattern].

**Recovery**. Verwenden Sie mehrere [Bereitstellung Steckplätze] [ app-service-slots] und der letzten als funktionierend bekannten Bereitstellung. Weitere Informationen finden Sie unter [grundlegende Web Application-Referenzarchitektur][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>Authentifizierungsfehler OpenID verbinden (OIDC).

**Erkennung**. Mögliche Fehlerquellen sind:

1. Azure AD ist nicht verfügbar oder kann aufgrund eines Netzwerkproblems nicht erreicht werden. Umleitung zum Endpunkt Authentifizierung fehlschlägt und Middleware OIDC löst eine Ausnahme aus.
2. Azure AD-Mandanten ist nicht vorhanden. Umleitung zum Endpunkt Authentifizierung gibt einen HTTP-Fehlercode und Middleware OIDC löst eine Ausnahme aus.
3. Benutzer kann nicht authentifiziert werden. Keine Erkennung Strategie ist erforderlich. Azure AD behandelt Anmeldefehler.

**Wiederherstellung**

1. Die Middleware nicht behandelte Ausnahmen abfangen.
2. Behandlung `AuthenticationFailed` Ereignisse.
3. Umleiten Sie den Benutzer an eine Fehlerseite.
4. Versuche des Benutzers.


## <a name="azure-search"></a>Azure Suche

### <a name="writing-data-to-azure-search-fails"></a>Schreiben von Daten in Azure Suche fehlschlägt.

**Erkennung**. Catch `Microsoft.Rest.Azure.CloudException` Fehler.

**Wiederherstellung**

[Suche .NET SDK] [ search-sdk] nach vorübergehender Fehler automatisch wiederholt. Das Client-SDK ausgelöste Ausnahmen sollten nicht flüchtigen Fehler behandelt werden.

Die Standardrichtlinie wiederholen verwendet exponentielle Backoff. Um eine andere wiederholungsrichtlinie verwenden, rufen `SetRetryPolicy` auf der `SearchIndexClient` oder `SearchServiceClient` Klasse. Weitere Informationen finden Sie unter [Automatische Wiederholungsversuche][auto-rest-client-retry].

**Diagnose**. [Suche Datenverkehr Analytics]verwenden[search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Lesen von Daten aus Azure Suche fehlschlägt.

**Erkennung**. Catch `Microsoft.Rest.Azure.CloudException` Fehler.

**Wiederherstellung**

[Suche .NET SDK] [ search-sdk] nach vorübergehender Fehler automatisch wiederholt. Das Client-SDK ausgelöste Ausnahmen sollten nicht flüchtigen Fehler behandelt werden.

Die Standardrichtlinie wiederholen verwendet exponentielle Backoff. Um eine andere wiederholungsrichtlinie verwenden, rufen `SetRetryPolicy` auf der `SearchIndexClient` oder `SearchServiceClient` Klasse. Weitere Informationen finden Sie unter [Automatische Wiederholungsversuche][auto-rest-client-retry].

**Diagnose**. [Suche Datenverkehr Analytics]verwenden[search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Lesen oder Schreiben auf einen Knoten fehlschlägt.

**Erkennung**. Die Ausnahme abfangen. Für .NET Clients in der Regel werden `System.Web.HttpException`. Anderer Client möglicherweise andere Ausnahmetypen.  Weitere Informationen finden Sie unter [Fehlerbehandlung Cassandra richtig](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right).

**Wiederherstellung**

- Jeder [Cassandra Client](https://wiki.apache.org/cassandra/ClientOptions) verfügt über eigene wiederholungsrichtlinien und Funktionen. Weitere Informationen finden Sie unter [Fehlerbehandlung Cassandra richtig][cassandra-error-handling].
- Verwenden Sie eine Rack-fähige Bereitstellung mit Daten über die Fehlerdomänen verteilt.
- Mehrere Bereiche mit lokales Quorum Konsistenz bereitstellen. Bei einem nicht flüchtigen Fehler ein Failover auf einen anderen Bereich.

**Diagnose**. Anwendungsprotokolle


## <a name="cloud-service"></a>Cloud-Dienst

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Web-oder Workerrollen werden unerwartet heruntergefahren.

**Erkennung**. [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] wird ausgelöst.

**Recovery**. Überschreiben Sie [RoleEntryPoint.OnStop] [ RoleEntryPoint.OnStop] Methode ordnungsgemäß bereinigen. Weitere Informationen finden Sie in [Der richtig behandeln Azure OnStop Ereignisse] [ onstop-events] (Blog).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Lesen von Daten aus DocumentDB schlägt fehl.

**Erkennung**. Catch `System.Net.Http.HttpRequestException` oder `Microsoft.Azure.Documents.DocumentClientException`. 

**Wiederherstellung**

- Das SDK versucht automatisch Versuche. Zum Festlegen der Anzahl der Wiederholungsversuche und der maximalen Wartezeit konfigurieren `ConnectionPolicy.RetryOptions`. Der Client löst Ausnahmen sind entweder über die wiederholungsrichtlinie oder nicht vorübergehender Fehler. 

- Wenn DocumentDB Client drosselt, wird einen Fehler 429 HTTP. Überprüfen Sie den Statuscode der `DocumentClientException`. Wenn Sie Fehler 429 konsistent erhalten, erhöhen Sie Durchsatzwert der DocumentDB-Auflistung.

- Replizieren Sie DocumentDB-Datenbank mindestens zwei Regionen. Alle Replikate sind lesbar. Client-SDKs geben die `PreferredLocations` Parameter. Dies ist eine geordnete Liste von Azure-Regionen. Alle Lesevorgänge werden an den ersten verfügbaren Bereich in der Liste gesendet. Wenn die Anforderung fehlschlägt, versucht der Client die Bereiche in der Liste in der Reihenfolge. Weitere Informationen finden Sie unter [Entwickeln mit DocumentDB mit mehreren][docdb-multi-region].

**Diagnose**. Protokollieren Sie alle Fehler auf dem Client.


### <a name="writing-data-to-documentdb-fails"></a>Schreiben von Daten in DocumentDB fehlschlägt.

**Erkennung**. Catch `System.Net.Http.HttpRequestException` oder `Microsoft.Azure.Documents.DocumentClientException`. 

**Wiederherstellung**

- Das SDK versucht automatisch Versuche. Zum Festlegen der Anzahl der Wiederholungsversuche und der maximalen Wartezeit konfigurieren `ConnectionPolicy.RetryOptions`. Der Client löst Ausnahmen sind entweder über die wiederholungsrichtlinie oder nicht vorübergehender Fehler. 

- Wenn DocumentDB Client drosselt, wird einen Fehler 429 HTTP. Überprüfen Sie den Statuscode der `DocumentClientException`. Wenn Sie Fehler 429 konsistent erhalten, erhöhen Sie Durchsatzwert der DocumentDB-Auflistung.

- Replizieren Sie DocumentDB-Datenbank mindestens zwei Regionen. Fällt die primäre Region werden anderen Region gefördert schreiben. Einen Failover kann auch manuell ausgelöst werden. Das SDK tut automatische Erkennung und routing Anwendungscode weiterhin nach einem Failover. Zeitraum (normalerweise Minuten) müssen Schreibvorgänge höheren Latenz wie das SDK den neue Bereich schreiben. Weitere Informationen finden Sie unter [Entwickeln mit DocumentDB mit mehreren][docdb-multi-region].

- Als Fallback Dokument backup Warteschlange anhalten und später verarbeiten der Warteschlange.

**Diagnose**. Protokollieren Sie alle Fehler auf dem Client.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Lesen von Daten aus Elasticsearch schlägt fehl.

**Erkennung**. Die entsprechende Ausnahme für bestimmte [Elasticsearch Client] [ elasticsearch-client] verwendet wird. 

**Wiederherstellung**

- Verwenden Sie einen Wiederholungsmechanismus. Jeder Client verfügt über eine eigene wiederholungsrichtlinien. 

- Bereitstellen mehrerer Elasticsearch Knoten und Verwendung der Replikation für hohe Verfügbarkeit.

Weitere Informationen finden Sie unter [Elasticsearch in Azure ausgeführt][elasticsearch-azure].

**Diagnose**. Sie können Überwachungstools für Elasticsearch verwenden bzw. alle Fehler auf dem Client mit der Nutzlast. Siehe Abschnitt "Überwachung" in [Elasticsearch in Azure ausgeführt][elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Schreiben von Daten in Elasticsearch fehlschlägt.

**Erkennung**. Die entsprechende Ausnahme für bestimmte [Elasticsearch Client] [ elasticsearch-client] verwendet wird.  

**Wiederherstellung**

- Verwenden Sie einen Wiederholungsmechanismus. Jeder Client verfügt über eine eigene wiederholungsrichtlinien. 
 
- Wenn die Anwendung eine geringere konsistenzebene tolerieren kann, sollten Sie mit `write_consistency` Einstellung des `quorum`.

Weitere Informationen finden Sie unter [Elasticsearch in Azure ausgeführt][elasticsearch-azure].

**Diagnose**. Sie können Überwachungstools für Elasticsearch verwenden bzw. alle Fehler auf dem Client mit der Nutzlast. Siehe Abschnitt "Überwachung" in [Elasticsearch in Azure ausgeführt][elasticsearch-azure].


## <a name="queue-storage"></a>Warteschlangenspeicher

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Schreiben eine Nachricht an Azure-Warteschlange Speicher fehl konsistent.

**Erkennung**. Nach *N* Wiederholungsversuche, fällt der Schreibvorgang weiter.

**Wiederherstellung**

- Speichern Sie die Daten in einem lokalen Cache und weiterzuleiten Sie Schreibvorgänge in den Speicher später, wenn der Dienst verfügbar ist.
- Erstellen einer sekundären Warteschlange und Schreiben an die Warteschlange, wenn die primäre Warteschlange nicht verfügbar ist.

**Diagnose**. Verwenden Sie [Storage Metriken][storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Die Anwendung kann keine bestimmte Meldung aus der Warteschlange verarbeiten. 

**Erkennung**. Anwendungsspezifisch. Beispielsweise die Nachricht enthält ungültige Daten oder Geschäftslogik aus irgendeinem Grund fehlschlägt. 

**Wiederherstellung**

Verschieben der Nachricht in einer separaten Warteschlange. Überprüfen Sie die Nachrichten in der Warteschlange einen separaten Prozess ausgeführt.

Verwenden Sie Warteschlangen Azure Service Bus Messaging bietet eine [Warteschlange] [ sb-dead-letter-queue] Funktionen für diesen Zweck.

Hinweis: Bei Verwendung von speicherwarteschlangen mit Webaufträge das WebJobs SDK umfasst integrierte unzustellbare Nachrichten. Finden Sie unter [Warteschlangenspeicher Azure SDK Webaufträge verwenden][sb-poison-message].

**Diagnose**. Verwenden Sie die anwendungsprotokollierung.


## <a name="redis-cache"></a>Redis Cache

### <a name="reading-from-the-cache-fails"></a>Lesen vom Cache schlägt fehl.

**Erkennung**. Catch `StackExchange.Redis.RedisConnectionException`.

**Wiederherstellung**

1. Vorübergehende Fehler wiederholen. Azure Redis Cache unterstützt integrierte wiederholen Siehe [Cache Redis wiederholen Richtlinien][redis-retry].
2. Nicht vorübergehende Fehler wie ein fehlgeschlagener Cachezugriff behandelt und zurückgreifen auf die ursprüngliche Datenquelle.

**Diagnose**. [Redis Cache-Diagnose]verwenden[redis-monitor].


### <a name="writing-to-the-cache-fails"></a>In den Cache schreiben kann.

**Erkennung**. Catch `StackExchange.Redis.RedisConnectionException`.

**Wiederherstellung**

1. Vorübergehende Fehler wiederholen. Azure Redis Cache unterstützt integrierte wiederholen Siehe [Cache Redis wiederholen Richtlinien][redis-retry].
2. Ist der Fehler nicht flüchtigen ignorieren und andere Transaktionen, die später in den Cache schreiben lassen.

**Diagnose**. [Redis Cache-Diagnose]verwenden[redis-monitor].


## <a name="sql-database"></a>SQL-Datenbank

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>Keine Verbindung zur Datenbank in der primären Region.

**Erkennung**. Verbindung schlägt fehl.

**Wiederherstellung**

Voraussetzung: Die Datenbank muss für aktive Geo-Replikation konfiguriert. [SQL Datenbank aktive Geo-Replikation]finden Sie unter[sql-db-replication].

- Abfragen Lesen von einem sekundären Replikat. 
- Für Inserts und Updates manuell Failover zu einem sekundären Replikat. [Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank]finden Sie unter[sql-db-failover].

Das Replikat verwendet eine andere Verbindungszeichenfolge müssen Sie die Verbindungszeichenfolge in der Anwendung.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Läuft aus Verbindung im Verbindungspool.

**Erkennung**. Catch `System.InvalidOperationException` Fehler. 

**Wiederherstellung**

- Wiederholen Sie den Vorgang.
- Als ein Reduzierungsplan isolieren Sie Verbindungspools für jeden Anwendungsfall ein Anwendungsfall kann nicht alle Verbindungen dominieren.
- Erhöhen Sie die maximale Verbindungspools.

**Diagnose**. Anwendung anmeldet.


### <a name="database-connection-limit-is-reached"></a>Datenbank ist-Verbindung erreicht. 

**Erkennung**. Azure SQL-Datenbank begrenzt die Anzahl der gleichzeitigen Arbeitskräfte, Benutzernamen und Sessions. Die Grenzen hängen von der Dienstebene. Weitere Informationen finden Sie unter [Azure SQL-Datenbank Ressourcengrenzen][sql-db-limits].

Um diese Fehler zu erkennen, catch `System.Data.SqlClient.SqlException` und den Wert der `SqlException.Number` für den SQL-Fehlercode. Eine Liste der entsprechenden Fehlercodes finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbank-Verbindungsfehler und andere Probleme][sql-db-errors].

**Recovery**. Diese Fehler werden vorübergehend betrachtet wiederholt das Problem beheben kann. Wenn Sie diese Fehler konsistent erreicht, sollten Sie Skalierung der Datenbank.

**Diagnose**. - [Sys.event_log] [ sys.event_log] Abfrage gibt erfolgreiche Datenbankverbindungen, Verbindungsfehler und Deadlocks.

- Erstellen einer [Warnregel] [ azure-alerts] Verbindung fehlgeschlagen.

- Überwachen der [SQL-Datenbank] [ sql-db-audit] und fehlgeschlagener überprüfen.


## <a name="service-bus-messaging"></a>Service Bus Messaging

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Lesen einer Nachricht aus einer Warteschlange Service Bus ausfällt.

**Erkennung**. Fangen Sie Ausnahmen von der Client-SDK. Die Basisklasse für Ausnahmen Service Bus ist [MessagingException][sb-messagingexception-class]. Wenn der Fehler vorübergehend ist die `IsTransient` -Eigenschaft true ist. 

Weitere Informationen finden Sie unter [Service Bus messaging Ausnahmen][sb-messaging-exceptions].

**Wiederherstellung**

1. Vorübergehende Fehler wiederholen. Siehe [Service Bus Richtlinien erneut][sb-retry].

2. Nachrichten, die an alle Empfänger übermittelt werden können werden in einer *Warteschlange*platziert. Mithilfe dieser Warteschlange, die Nachrichten nicht empfangen werden konnte. Es gibt keine automatische Bereinigung Dead Letter-Warteschlange. Nachrichten bleiben dort, bis Sie explizit abrufen. Siehe [Übersicht der unzustellbare Servicebuswarteschlangen][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Schreiben eine Nachricht mit einem Service schlägt Warteschlange.

**Erkennung**. Fangen Sie Ausnahmen von der Client-SDK. Die Basisklasse für Ausnahmen Service Bus ist [MessagingException][sb-messagingexception-class]. Wenn der Fehler vorübergehend ist die `IsTransient` -Eigenschaft true ist. 

Weitere Informationen finden Sie unter [Service Bus messaging Ausnahmen][sb-messaging-exceptions].

**Wiederherstellung**

1. Der Service Bus-Client versucht automatisch nach vorübergehender Fehler. Standardmäßig verwendet es exponentielle Backoff. Maximale Wiederholungsanzahl oder maximalen Fehlerwartezeit Ausnahme Client eine. Weitere Informationen finden Sie unter [Service Bus Richtlinien erneut][sb-retry].

2. Wenn das Warteschlangenkontingent überschritten wird, löst der Client [QuotaExceededException][QuotaExceededException]. Die Meldung der Ausnahme enthält weitere Informationen. Abzuleiten Sie einige Nachrichten aus der Warteschlange vor der Wiederholung und verwenden Sie Schutzschalter Muster weitere Versuche zu, während das Kontingent überschritten wird. Stellen Sie außerdem sicher, dass die Eigenschaft [BrokeredMessage.TimeToLive] nicht zu hoch ist. 

3. Innerhalb eines Bereichs Stabilität verbessert werden [partitionierte Warteschlangen]oder Themen[sb-partition]. Eine nicht partitionierte Warteschlange oder ein Thema ein Nachrichtenspeicher erhält. Wenn dieser Nachrichtenspeicher nicht verfügbar ist, werden alle Vorgänge in dieser Warteschlange oder ein Thema fehl. Eine partitionierte Warteschlange oder ein Thema wird über mehrere messaging Informationsspeicher partitioniert. 

4. Für zusätzliche Stabilität zwei Service Bus Namespaces in unterschiedlichen Regionen erstellen und Replizieren von Nachrichten. Sie können aktive Replikation oder passive Replikation.

    - Aktive Replikation: der Client sendet alle Nachrichten an beide Warteschlangen. Der Empfänger überwacht sowohl Warteschlangen. Markieren von Nachrichten mit einem eindeutigen Bezeichner der Client doppelte Nachrichten verwerfen kann.

    - Passive Replikation: der Client sendet die Nachricht an eine Warteschlange. Wenn ein Fehler vorliegt, greift der Client wieder in die Warteschlange. Der Empfänger überwacht sowohl Warteschlangen. Dieser Ansatz reduziert die Anzahl der doppelten Nachrichten. Der Empfänger muss jedoch weiterhin doppelte Nachrichten behandeln.

    Weitere Informationen finden Sie im [Beispiel GeoReplication] [ sb-georeplication-sample] und [Best Practices für isolierende Anwendung Service Bus Ausfällen und Katastrophen][sb-outages].


### <a name="duplicate-message"></a>Doppelte Nachricht.

**Erkennung**. Überprüfen Sie die `MessageId` und `DeliveryCount` der Meldung.

**Wiederherstellung**

- Wenn möglich, Entwerfen Sie Ihre Nachricht Verarbeitung Idempotent sein. Speichern Sie Nachrichten-IDs der Nachrichten, die bereits verarbeitet und die Kennung vor der Verarbeitung einer Nachricht überprüfen.

- Erkennung doppelter, Erstellen der Warteschlange mit `RequiresDuplicateDetection` auf True festgelegt. Mit dieser Einstellung löscht Service Bus automatisch jede Nachricht mit derselben `MessageId` als eine Nachricht.  Beachten Sie Folgendes:

    -  Diese Einstellung verhindert, dass doppelte Nachrichten in die Warteschlange gestellt. Es verhindern nicht, dass Empfänger dieselbe Nachricht mehrmals verarbeitet.

    - Duplikaterkennungsregeln hat ein. Wenn ein Duplikat über dieses Fenster gesendet wird, wird nicht sie erkannt werden.

**Diagnose**. Protokollieren Sie doppelte Nachrichten.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Die Anwendung kann keine bestimmte Meldung aus der Warteschlange verarbeiten. 

**Erkennung**. Anwendungsspezifisch. Beispielsweise die Nachricht enthält ungültige Daten oder Geschäftslogik aus irgendeinem Grund fehlschlägt. 

**Wiederherstellung**

Es gibt zwei Fehlermodi zu berücksichtigen. 

- Der Empfänger erkennt den Ausfall. In diesem Fall Verschieben der Nachricht in der Warteschlange. Später führen Sie einen separaten Vorgang untersuchen die Nachrichten in der Warteschlange.

- Der Empfänger der Verarbeitung der Nachricht fehlschlägt &mdash; beispielsweise aufgrund einer Ausnahme. Dabei verwenden Sie `PeekLock` Modus. In diesem Modus läuft die Sperre steht die Nachricht an andere Empfänger. Überschreitet die Nachricht die maximale Lieferung Anzahl oder die Time-to-live, wird die Nachricht automatisch an die Dead Letter-Warteschlange verschoben.

Weitere Informationen finden Sie unter [Übersicht über Service Bus unzustellbare Warteschlangen][sb-dead-letter-queue].

**Diagnose**. Wenn die Anwendung eine Meldung an die Dead Letter-Warteschlange verschiebt, Schreiben Sie ein Ereignis in die Anwendungsprotokolle.


## <a name="service-fabric"></a>Service Fabric

### <a name="a-request-to-a-service-fails"></a>Eine Anforderung an einen Dienst fehlschlägt.

**Erkennung**. Der Dienst gibt einen Fehler zurück.

**Wiederherstellung**

- Suchen Sie erneut einen Proxy (`ServiceProxy` oder `ActorProxy`) und die Service-Schauspieler-Methode erneut aufrufen. 

- **Statusbehaftete Dienst**. Umschließen Sie auf zuverlässige Sammlungen in einer Transaktion. Wenn ein Fehler vorliegt, wird die Transaktion zurückgesetzt. Anforderung, werden wenn aus einer Warteschlange erneut verarbeitet. 
 
- **Statusfreie Service**. Wenn der Dienst auf einem externen Speicher erhalten bleiben, müssen alle Vorgänge Idempotent sein.


**Diagnose**. Anwendungsprotokoll

### <a name="service-fabric-node-is-shut-down"></a>Service Fabric-Knoten heruntergefahren.

**Erkennung**. Ein Abbruchtoken an den Dienst übergeben `RunAsync` Methode. Service Fabric bricht die Aufgabe vor dem Herunterfahren des Knotens ab.

**Recovery**. Verwenden Sie das Abbruchtoken Herunterfahren erkennen. Wenn Service Fabric Abbruch anfordert, Arbeit beenden und verlassen Sie `RunAsync` so schnell wie möglich.

**Diagnose**. Anwendungsprotokolle


## <a name="storage"></a>Speicher

### <a name="writing-data-to-azure-storage-fails"></a>Schreiben von Daten in Azure Storage schlägt fehl

**Erkennung**. Der Client empfängt Fehler beim Schreiben.

**Wiederherstellung**

1. Wiederholen Sie den Vorgang wieder von Ausfällen. Die [Richtlinie erneut] [ Storage.RetryPolicies] im Client SDK behandelt dies automatisch.
2. Implementieren des Überlastungsschalters Muster um überwältigend Speicher zu vermeiden.
3. Wenn N Versuche fehlschlagen, durchführen Sie ein ordnungsgemäßes Fallback. Zum Beispiel:

    - Speichern Sie die Daten in einem lokalen Cache und weiterzuleiten Sie Schreibvorgänge in den Speicher später, wenn der Dienst verfügbar ist.
    - Wurde die Schreibaktion in einen Transaktionsbereich ersetzen Sie die Transaktion.

**Diagnose**. Verwenden Sie [Storage Metriken][storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Lesen von Daten aus dem Azure-Speicher schlägt fehl.

**Erkennung**. Fehler bekommt beim Lesen von.

**Wiederherstellung**

1. Wiederholen Sie den Vorgang wieder von Ausfällen. Die [Richtlinie erneut] [ Storage.RetryPolicies] im Client SDK behandelt dies automatisch.
2. RA-GRS Storage Ausfall der primären lesen versuchen Sie Lesen vom sekundären Endpunkt. Das Client-SDK kann dies automatisch behandelt. [Azure Storage-Replikation]finden Sie unter[storage-replication].
3. *N* Versuche fehlschlagen, Aktion ein fallback diskret. Ein Produktbild aus dem Speicher abgerufen werden kann, zeigen Sie ein generische Platzhalter ein.

**Diagnose**. Verwenden Sie [Storage Metriken][storage-metrics].


## <a name="virtual-machine"></a>Virtual Machine

### <a name="connection-to-a-backend-vm-fails"></a>Verbindung zum Back-End-VM schlägt fehl.

**Erkennung**. Netzwerkverbindungsfehler.

**Wiederherstellung**

- Zwei Back-End-VMs in einem Satz Verfügbarkeit hinter einem Lastenausgleich bereitstellen.

- Wenn der Verbindungsfehler flüchtig ist, wiederholt manchmal TCP erfolgreich Senden der Nachricht. 

- Implementieren Sie eine Richtlinie "Wiederholen" in der Anwendung. 

- Implementieren Sie persistent oder nicht flüchtigen Fehler [Schutzschalter] [ circuit-breaker] Muster.

- Aufrufende VM seine Netzwerk Ausgang überschreitet, wird die ausgehende Warteschlange füllen. Sollten Sie die ausgehende Warteschlange ständig vollständigen skalieren. 

**Diagnose**. Ereignisse an Service.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>VM-Instanz ist nicht verfügbar oder fehlerhaft.

**Erkennung**. Konfigurieren Sie ein System zum Lastenausgleich [Integritätstest] [ lb-probe] Signale, ob die Instanz VM fehlerfrei ist. Der Prüfpunkt überprüfen, ob kritische Funktionen richtig sind. 

**Recovery**. Für jede Anwendungsebene mehrere VM-Instanzen dieselben Verfügbarkeit gebracht und Lastenausgleich vor VMs platzieren. Fällt der Integritätstest beendet Lastenausgleich neue Verbindungen auf die fehlerhafte Instanz senden.

**Diagnose**. -Verwenden Sie Lastenausgleich [Protokollanalyse][lb-monitor].
- Konfigurieren Sie Ihr Überwachungssystem Systemüberwachungsereignissen Endpunkte zu überwachen.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Operator fährt versehentlich eine VM.

**Erkennung**. N/A

**Recovery**. Legen Sie eine Ressourcensperre mit `ReadOnly` auf. [Sperrenressourcen mit Azure-Ressourcen-Manager]finden Sie unter[rm-locks].

**Diagnose**. Verwenden Sie [Azure Aktivitätsprotokollen][azure-activity-logs].


## <a name="webjobs"></a>Webaufträge

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Fortlaufende Aufgabe beendet wird, sobald der SCM-Host sich im Leerlauf befindet.

**Erkennung**. Übergeben eines Abbruchtokens Webauftrags Funktion. Weitere Informationen finden Sie unter [ordnungsgemäßes Herunterfahren][web-jobs-shutdown]. 

**Recovery**. Aktivieren der `Always On` im Web app. Weitere Informationen finden Sie unter [Ausführen Hintergrundaufgaben mit Webaufträge][web-jobs].


## <a name="application-design"></a>Anwendungsentwurf

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Anwendung kann einen Anstieg der Anfragen nicht bearbeiten.

**Erkennung**. Abhängig. Normale Symptome:

- Die Website überhaupt HTTP-Fehlercodes 5xx.
- Abhängige Dienste, wie Datenbank oder Speichergruppe, beginnen, Anfragen zu drosseln. HTTP-Fehler wie HTTP 429 (zu viele Anfragen) je suchen.
- HTTP-Warteschlangenlänge wächst.

**Wiederherstellung**

- Erhöhte Last Dezentrales Skalieren. 

- Minimieren Sie Fehler cascading stören die gesamte Anwendung Fehler zu vermeiden. Strategien zur Risikominderung umfassen:

    - Das [Muster Drosselung] [ throttling-pattern] überwältigend Backend-Systeme zu vermeiden.
    - [Queue-basierten laden Abgleich] verwenden[ queue-based-load-leveling] Puffer Anfragen und entsprechenden Tempo verarbeiten.
    - Priorisieren Sie bestimmte Clients. Beispielsweise verfügt die Anwendung kostenlose und kostenpflichtige Ebenen drosseln Sie Kunden kostenlose Ebene jedoch nicht Kunden. [Priorität Warteschlange Muster]finden Sie unter[priority-queue-pattern].

**Diagnose**. [App Service Diagnose Protokollierung]verwenden,[app-service-logging]. Einen Dienst wie [Azure Protokollanalyse][azure-log-analytics], [Application Insights][app-insights], oder [Neue Relikt] [ new-relic] zu den Diagnoseprotokollen.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Einer der Vorgänge in einem Workflow oder verteilte Transaktion schlägt fehl.

**Erkennung**. Nach *N* Wiederholungsversuche, es noch nicht.

**Wiederherstellung**

- Implementieren Sie als Ausgleichsplan, der [Planer Agent Supervisor] [ scheduler-agent-supervisor] Muster den gesamten Workflow verwalten. 
- Wiederholen Sie nicht Timeouts. Gibt eine niedrige Erfolg für diesen Fehler. 
- Warteschlange arbeiten, um später wiederholen.

**Diagnose**. Protokollieren Sie alle Vorgänge (erfolgreiche und fehlgeschlagene), einschließlich kompensierende Aktionen Verwenden Sie Korrelations-IDs, sodass Sie alle Vorgänge innerhalb derselben Transaktion verfolgen können.


### <a name="a-call-to-a-remote-service-fails"></a>Ein Aufruf an einen Remotedienst fehlschlägt.

**Erkennung**. HTTP-Fehlercode.

**Wiederherstellung**

1. Vorübergehende Fehler wiederholen. 
2. Wenn der Aufruf fehlschlägt *N* versucht, eine alternative Aktion. (Anwendungsspezifisch.)
3. Die [Leistungsschalter Muster] [ circuit-breaker] cascading Fehler vermeiden. 

**Diagnose**. Protokollieren Sie alle Remoteaufrufe Fehler


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über den Prozess FMA finden Sie unter [Stabilität von Design für Cloud-Services] [ resilience-by-design-pdf] (PDF-Download).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

