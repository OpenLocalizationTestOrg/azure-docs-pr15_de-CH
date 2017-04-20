<properties 
    pageTitle="Azure Redis Cache konfigurieren | Microsoft Azure"
    description="Verstehen Sie Standardkonfiguration für Azure Redis Cache Redis und erfahren Sie, wie Ihre Azure Redis Cacheinstanzen konfigurieren"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Azure Redis Cache konfigurieren

Dieses Thema beschreibt überprüfen und aktualisieren die Konfiguration für Ihre Azure Redis Cache-Instanzen und deckt die standardmäßige Redis-Serverkonfiguration für Azure Redis Cache-Instanzen.

>[AZURE.NOTE] Weitere Informationen zur Konfiguration und Verwendung von Cache-Zusatzfunktionen finden Sie unter [clustering Premium Azure Redis Cache konfigurieren](cache-how-to-premium-clustering.md) [Dauerhaftigkeit Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md)und [Unterstützung für einen Premium Azure Redis virtuelles Netzwerk konfigurieren](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Redis Cache konfigurieren

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis Cache bietet folgenden Einstellungen auf die **Standardeinstellungen** .

![Redis Cache Settings](./media/cache-configure/redis-cache-settings.png)



-   [Support und Problembehandlung settings](#support-amp-troubleshooting-settings)
-   [Allgemeine Einstellung](#general-settings)
    -   [Eigenschaften](#properties)
    -   [Zugriffstasten](#access-keys)
    -   [Erweitert](#advanced-settings)
    -   [Redis Cache-Berater](#redis-cache-advisor)
-   [Einstellungen](#scale-settings)
    -   [Preisstufe](#pricing-tier)
    -   [Redis Größe](#cluster-size)
-   [Daten-Einstellung](#data-management-settings)
    -   [Dauerhaftigkeit von Daten redis](#redis-data-persistence)
    -   [Import/Export](#importexport)
-   [Verwaltung Standardeinstellungen](#administration-settings)
    -   [Neustart](#reboot)
    -   [Planen von updates](#schedule-updates)
-   [Diagnose-Einstellung](#diagnostics-settings)
-   [Netzwerk-Standardeinstellungen](#network-settings)
-   [Ressource-Einstellung](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Support und Problembehandlung settings

Die Einstellungen im Abschnitt **Support + Problembehandlung** bieten Optionen zum Lösen von Problemen mit den Cache.

![Support + Problembehandlung](./media/cache-configure/redis-cache-support-troubleshooting.png)

Klicken Sie auf **diagnostizieren und Lösen von Problemen** auf allgemeine Probleme und Strategien zur Fehlerbehebung.

Klicken Sie auf **Aktivitätsprotokoll** Cache ausgeführten Aktionen anzeigen. Sie können auch Filtern um dieser Ansicht auf andere Ressourcen zu erweitern. Weitere Informationen zum Arbeiten mit Überwachungsprotokollen finden Sie unter [Ereignisse anzeigen und Überwachungsprotokolle](../monitoring-and-diagnostics/insights-debugging-with-events.md) und [Operationen mit Ressourcen-Manager überwachen](../resource-group-audit.md). Weitere Informationen zum Überwachen von Azure Redis Cache-Ereignissen finden Sie unter [Vorgänge und Alarme](cache-how-to-monitor.md#operations-and-alerts).

**Zustand der Ressourcen** die Ressource überwacht und informiert Sie, ob sie wie erwartet ausgeführt wird. Weitere Informationen über das Gesundheitswesen Azure Ressource Übersicht [Azure Ressource Gesundheit](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] Zustand der Ressourcen kann derzeit nicht Bericht über den Zustand von Azure Redis Cacheinstanzen in einem virtuellen Netzwerk gehostet. Weitere Informationen finden Sie unter [Cache-Funktionen funktionieren beim Hosten von eines Caches in einem VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Klicken Sie auf **neue support-Anfragen** , Öffnen einer Anfrage für den Cache.

## <a name="general-settings"></a>Allgemeine Einstellung

Die im Abschnitt " **Allgemein** " können Sie zu folgenden Einstellungen für den Cache.

![Allgemeine Einstellung](./media/cache-configure/redis-cache-general-settings.png)

-   [Eigenschaften](#properties)
-   [Zugriffstasten](#access-keys)
-   [Erweitert](#advanced-settings)
-   [Redis Cache-Berater](#redis-cache-advisor)

### <a name="properties"></a>Eigenschaften

Klicken Sie auf **Eigenschaften** , um Informationen über den Cache sowie den cacheendpunkt Ports anzuzeigen.

![Redis Cacheeigenschaften](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Zugriffstasten

Klicken Sie auf **Tastenkombinationen** oder Zugriffstasten für den Cache regenerieren. Diese Tasten werden Hostnamen und Ports aus dem Blade **Eigenschaften** von Clients mit den Cache verwendet.

![Redis Cache Zugriffstasten](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Erweitert

Folgendes sind auf die **Erweiterte Einstellungen** konfigurieren.

-   [Zugriff auf Ports](#access-ports)
-   [Maxmemory-Richtlinie und Maxmemory reserviert](#maxmemory-policy-and-maxmemory-reserved)
-   [Erledigten Anträge (Erweiterte Einstellungen)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>Zugriff auf Ports

Nicht-SSL-Zugriff ist für neue Zwischenspeicher deaktiviert. **Um nicht-SSL-Port zu aktivieren auf **Nein** **Zugriff nur über SSL** auf **Blades erweitert** **

![Redis Cache Zugriff auf Ports](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Maxmemory-Richtlinie und Maxmemory reserviert

Die **Richtlinien Maxmemory** und **Maxmemory vorbehalten** auf die **Erweiterte Einstellung** Einstellungen Richtlinien Speicher für den Cache. Die **Maxmemory -** Einstellung konfiguriert Entfernungsrichtlinie für den Cache und **Maxmemory reserviert** für Cache-Prozesse reservierten Speicher konfiguriert.

![Redis Maxmemory Cacherichtlinie](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory-Richtlinie** können Sie die folgenden Auslagerungsrichtlinien aus.

-   Volatile-Lru - Dies ist die Standardeinstellung.
-   AllKeys lru
-   Volatile random
-   zufällige AllKeys
-   Volatile-ttl
-   noeviction

Weitere Informationen zu Maxmemory-Richtlinien finden Sie unter [Auslagerungsrichtlinien](http://redis.io/topics/lru-cache#eviction-policies).

Die Einstellung **Maxmemory reserviert** konfiguriert die Speichermenge in MB, die für Operationen wie Replikation Failover-Cache reserviert ist. Es kann auch verwendet werden, bei einem Verhältnis hohe Fragmentierung. Diese Einstellung ermöglicht eine konsistente Redis Server Erfahrung, wenn Ihre variiert. Dieser Wert sollte für Arbeitslasten, die hohe schreiben festgelegt werden. Beim Reservieren von Speicher für solche Operationen steht zwischengespeicherte Daten.

>[AZURE.IMPORTANT] Einstellung **Maxmemory reserviert** ist nur verfügbar für Standard- und Premium zwischengespeichert.

### <a name="keyspace-notifications-advanced-settings"></a>Erledigten Anträge (Erweiterte Einstellungen)

Redis erledigten Anträge auf die **Erweiterte Einstellungen** konfiguriert. Erledigten Anträge können Clients benachrichtigt, wenn bestimmte Ereignisse auftreten.

![Redis Cache Erweiterte Einstellungen](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Erledigten Anträge und die Einstellung **Benachrichtigen erledigten-Ereignisse** sind nur für Standard- und Premium zwischengespeichert.

Weitere Informationen finden Sie unter [Redis erledigten Anträge](http://redis.io/topics/notifications). Beispielcode finden Sie in der Datei [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) in das [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) -Beispiel.

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis Cache-Berater

**Empfohlene** Blade Zeigt Vorschläge für den Cache. Während des normalen Betriebs werden keine Vorschläge angezeigt. 

![Empfehlung](./media/cache-configure/redis-cache-no-recommendations.png)

Etwaige Auflagen bei Operationen wie Speicher, Bandbreite oder Serverlast Cache auftreten, wird eine Warnung auf dem **Redis Cache** angezeigt.

![Empfehlung](./media/cache-configure/redis-cache-recommendations-alert.png)

Weitere Informationen finden auf der **empfohlenen** .

![Empfehlung](./media/cache-configure/redis-cache-recommendations.png)

Sie können diese Metriken auf die [Überwachung](cache-how-to-monitor.md#monitoring-charts) und [Verwendung Charts](cache-how-to-monitor.md#usage-charts) -Blade **Redis Cache** überwachen.

Jeder Tarif hat unterschiedliche Grenzwerte für Clientverbindungen, Speicher und Bandbreite. Der Cache für diese Kennzahlen über einen längeren Zeitraum Kapazitätsgrenze, entsteht eine Empfehlung. Weitere Informationen über die Kennzahlen und Grenzen durch das **Empfohlene** Tool überprüft finden Sie in der folgenden Tabelle.

| Redis Cache Metrik      | Weitere Informationen finden Sie unter                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Nutzung der Netzwerkbandbreite | [Cache-Performance - Bandbreite](cache-faq.md#cache-performance) |
| Verbundene clients       | [Standardmäßige Redis-Serverkonfiguration - maxclients](#maxclients)            |
| Serverlast             | [Verwendung Charts - Redis-Server laden](cache-how-to-monitor.md#usage-charts)  |
| Speicherverwendung            | [Cache-Performance - Größe](cache-faq.md#cache-performance)                |

Um den Cache zu aktualisieren, klicken Sie auf **Jetzt aktualisieren** , [Tarif](#pricing-tier) und den Cache skaliert. Weitere Informationen zum Auswählen eines Tarifs finden Sie unter [welche Redis cacheangebot und Größe sollte ich verwenden?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Einstellungen

Der im Bereich " **Skalieren** " können Sie zu folgenden Einstellungen für den Cache.

![Netzwerk](./media/cache-configure/redis-cache-scale.png)

-   [Preisstufe](#pricing-tier)
-   [Redis Größe](#cluster-size)

### <a name="pricing-tier"></a>Preisstufe

Klicken Sie auf **Preisstufe** anzeigen oder ändern den Tarif für den Cache. Weitere Informationen zur Skalierung finden Sie unter [Skalierung Azure Redis Cache](cache-how-to-scale.md).

![Redis Cache Preisstufe](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Redis Größe

Klicken Sie auf **Redis-Clustergröße (Vorschau)** zum Ändern der Clustergröße für eine Premium-Cache mit Clusterunterstützung.

>[AZURE.NOTE] Beachten Sie, dass bei Azure Redis Cache der Stufen wurde für die allgemeine Verfügbarkeit, Clustergröße Redis Funktion ist zurzeit in der Vorschau.

![Redis Größe](./media/cache-configure/redis-cache-redis-cluster-size.png)

Um die Clustergröße zu ändern, verwenden Sie den Schieberegler oder geben Sie eine Zahl zwischen 1 und 10 im Textfeld **Anzahl der Splitter** und klicken Sie auf **OK** , speichern.

>[AZURE.IMPORTANT] Redis-clustering ist nur für Premium-Caches. Weitere Informationen finden Sie unter [Cluster Premium Azure Redis Cache konfigurieren](cache-how-to-premium-clustering.md).


## <a name="data-management-settings"></a>Daten-Einstellung

Die im Abschnitt **Verwaltung** können Sie zu der folgenden Einstellungen für den Cache.

![Datenmanagement](./media/cache-configure/redis-cache-data-management.png)

-   [Dauerhaftigkeit von Daten redis](#redis-data-persistence)
-   [Import/Export](#importexport)

### <a name="redis-data-persistence"></a>Dauerhaftigkeit von Daten redis

Klicken Sie auf aktivieren, deaktivieren oder konfigurieren Datenpersistenz für den Premium-Cache **Datenpersistenz Redis** .

![Dauerhaftigkeit von Daten redis](./media/cache-configure/redis-cache-persistence-settings.png)

Redis-Dauerhaftigkeit klicken Sie auf **aktiviert** , um RDB (Redis) Sicherung aktivieren. Um Redis Dauerhaftigkeit zu deaktivieren, klicken Sie auf **deaktiviert**.

Konfigurieren Sie das Sicherungsintervall wird wählen Sie eine **Sicherung** aus der Dropdown-Liste aus. Auswahlmöglichkeiten sind **15 Minuten**, **30 Minuten** **60 Minuten**, **6 Stunden**, **12 Stunden**und **24 Stunden**. Dieses Intervall beginnt nach der vorherigen Sicherung erfolgreich abgeschlossen und eine neue Sicherung, wenn es abläuft initiiert wird.

Auf **Speicherkonto** Speicherkonto verwenden aktivieren und wählen Sie die **Primärschlüssel** oder **Sekundärschlüssel** Dropdown **Speicherschlüssel** verwenden. Sie müssen ein Speicherkonto in derselben Region als Cache auswählen und **Premium** Speicherkonto wird empfohlen, da Storage Premium höheren Durchsatz. Jederzeit Speicherschlüssel für Ihr Konto Dauerhaftigkeit regeneriert wird, müssen Sie die gewünschte Taste **Speicherschlüssel** Dropdown erneut auswählen.

Klicken Sie auf **OK** , um die Persistenzkonfiguration zu speichern.

>[AZURE.IMPORTANT] Redis Datenpersistenz steht nur für Premium-Caches. Weitere Informationen finden Sie unter [Persistenz Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md).

### <a name="importexport"></a>Import/Export

Import/Export wird Azure Redis Cache Data Management ermöglicht Azure Redis Cache Daten importieren oder Exportieren von Daten aus dem Azure Redis Cache durch Importieren und Exportieren von Schnappschüssen Redis-Cache-Datenbank (RDB) aus einem Premium-Cache auf einer Seitenblob in einem Azure Storage-Konto. Dadurch können Sie zwischen verschiedenen Azure Redis Cacheinstanzen migrieren oder Auffüllen mit Daten verwenden.

Import kann verwendet werden, von einem Redis-Server in jeder Umgebung unter Linux, Windows oder Cloud-Anbieter wie Amazon Web Services und andere Redis oder mit Redis kompatibel RDB-Dateien zu. Importieren von Daten ist eine einfache Möglichkeit einen Cache mit vorab aufgefüllten Daten erstellt. Während des Importvorgangs Azure Redis Cache RDB-Dateien von Azure-Speicher in den Arbeitsspeicher geladen und die Schlüssel in den Cache eingefügt.

Können Sie Daten in Azure Redis Cache Redis kompatibel RDB-Dateien exportieren. Diese Funktion können Sie Daten von einer Azure Redis Cache-Instanz in eine andere oder einen anderen Redis-Server verschieben. Während des Exportvorgangs erstellt eine temporäre Datei auf dem virtuellen Computer, hostet Azure Redis Cache-Serverinstanz und die Datei wird dem designierten Speicher-Konto hochgeladen. Nach Abschluss des Exportvorgangs mit dem Status Erfolg, wird die temporäre Datei gelöscht.

>[AZURE.IMPORTANT] Import/Export steht nur für Premium-Tier-Caches. Weitere Informationen und Hinweise finden Sie unter [Importieren und Exportieren von Daten in Azure Redis Cache](cache-how-to-import-export-data.md).


## <a name="administration-settings"></a>Verwaltung Standardeinstellungen

Die im Abschnitt **Verwaltung** können Sie die folgenden Verwaltungsaufgaben für den Premium-Cache ausgeführt. 

![Verwaltung](./media/cache-configure/redis-cache-administration.png)

-   [Neustart](#reboot)
-   [Planen von updates](#schedule-updates)

>[AZURE.IMPORTANT] Die Einstellungen in diesem Abschnitt sind nur für Premium-Tier-Caches.

### <a name="reboot"></a>Neustart

Blade **Starten** können Sie einen oder mehrere Knoten des Cache neu starten. Dadurch können Sie der Anwendung Robustheit im Fehlerfall.

![Neustart](./media/cache-configure/redis-cache-reboot.png)

Haben Sie einen Premium-Cache mit Clusterunterstützung aktiviert, können Sie die Splitter des Caches neu starten auswählen.

![Neustart](./media/cache-configure/redis-cache-reboot-cluster.png)

Zu starten eine oder mehr Knoten des Cache, wählen Sie die gewünschten Knoten **neu gestartet**. Haben Sie einen Premium-Cache mit Clusterunterstützung, wählen Sie den Shards zu starten, und klicken Sie dann auf **neu starten**. Nach einigen Minuten Onlineschalten der ausgewählten Knoten Neustart sind und einige Minuten später.

>[AZURE.IMPORTANT] Neustart ist nur für Premium-Tier-Caches. Weitere Informationen und Hinweise finden Sie unter [Azure Redis Cache-Verwaltung - Neustart](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Planen von updates

**Planen von Updates** Blade können Sie ein Wartungsfenster Redis Server Updates für den Cache festlegen. 

>[AZURE.IMPORTANT] Beachten Sie, dass im Wartungsfenster gilt nur für Serverupdates Redis und nicht für jede Azure oder des Betriebssystems VMs, die den Cache hosten.

![Planen von updates](./media/cache-configure/redis-schedule-updates.png)

Um ein Wartungsfenster anzugeben, überprüfen Sie die gewünschten Tage Geben Sie Wartung Fenster Startstunde für jeden Tag an und klicken Sie auf **OK**. Beachten Sie, dass die Dauer der Wartung in UTC. 

>[AZURE.IMPORTANT] Planen von Updates ist nur für Premium-Tier-Caches. Weitere Informationen und Hinweise finden Sie unter [Azure Redis Cache-Verwaltung - Updates planen](cache-administration.md#schedule-updates).

## <a name="diagnostics-settings"></a>Diagnose-Einstellung

Abschnitt **Diagnose** können Sie bei einem Redis Diagnose konfigurieren.

![Diagnose](./media/cache-configure/redis-cache-diagnostics.png)

Klicken Sie zum Speichern von Cache-Diagnose [konfigurieren das Speicherkonto](cache-how-to-monitor.md#enable-cache-diagnostics) auf **Diagnose** .

![Redis Cache-Diagnose](./media/cache-configure/redis-cache-diagnostics-settings.png)

Klicken Sie auf **Redis Metriken** für Cache und **Warnungsregeln** [Warnungsregeln](cache-how-to-monitor.md#operations-and-alerts)einrichten [Daten anzeigen](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) .

Weitere Informationen zur Azure Redis Cache-Diagnose finden Sie in [Azure Redis Cache überwachen](cache-how-to-monitor.md).


## <a name="network-settings"></a>Netzwerk-Standardeinstellungen

Die im **Netzwerkabschnitt** können Sie zu der folgenden Einstellungen für den Cache.

![Netzwerk](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Virtuelles Netzwerkeinstellungen stehen nur für Premium-Caches, die konfiguriert wurden mit VNET Unterstützung während der Erstellung des Caches. Informationen auf ein Premium-Cache vnet unterstützen Sie und aktualisieren seine [virtuellen Netzwerk unterstützt Premium Azure Redis Cache konfigurieren](cache-how-to-premium-vnet.md).

## <a name="resource-management-settings"></a>Ressource-Einstellung

![Ressourcenmanagement](./media/cache-configure/redis-cache-resource-management.png)

Abschnitt " **Tags** " können Sie Ihre Ressourcen zu organisieren. Weitere Informationen finden Sie unter [Tags Azure Ressourcen zu verwenden](../resource-group-using-tags.md).

Abschnitt **gesperrt** können Sie ein Abonnement, Ressourcengruppe oder Ressource, um zu verhindern, dass andere Benutzer in Ihrer Organisation versehentlich löschen oder Ändern von kritischen Ressourcen sperren. Weitere Informationen finden Sie unter [Sperrenressourcen mit Azure-Ressourcen-Manager](../resource-group-lock-resources.md).

Abschnitt **Benutzer** bietet Unterstützung für rollenbasierte Zugriffskontrolle (RBAC) im Azure-Portal können Organisationen ihre Access Management einfach und genau entsprechen. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffskontrolle im Azure-Portal](../active-directory/role-based-access-control-configure.md).

Klicken Sie zum Erstellen und Exportieren einer Vorlage bereitgestellten Ressourcen für zukünftige Installationen **Vorlage exportieren** . Weitere Informationen zum Arbeiten mit Vorlagen finden Sie unter [Ressourcen Azure Ressourcenmanager Vorlagen bereitstellen](../resource-group-template-deploy.md).

## <a name="default-redis-server-configuration"></a>Standardmäßige Redis-Serverkonfiguration

Neue Instanzen von Azure Redis Cache sind folgende Redis Konfiguration Standardwerte konfiguriert.

>[AZURE.NOTE] Die Optionen in diesem Abschnitt können nicht geändert werden, mit den `StackExchange.Redis.IServer.ConfigSet` Methode. Wenn diese Methode mit einem der Befehle in diesem Abschnitt wird eine ähnlich der folgenden Ausnahme:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Alle Werte, die wie **Max-Speicher-Richtlinie**konfiguriert sind, sind über die Azure-Portal oder Befehlszeile Verwaltungstools Azure-CLI oder PowerShell konfiguriert.

|Einstellung|Standardwert|Beschreibung|
|---|---|---|
|Datenbanken|16|Die Standardanzahl von Datenbanken ist 16 verschiedene basierend auf den Tarif konfigurieren. <sup>1</sup> die Standarddatenbank 0 DB, wählen Sie auf einer Basis pro Verbindung mit `connection.GetDatabase(dbid)` , Dbid ist eine Zahl zwischen `0` und `databases - 1`.|
|MaxClients|Hängt die Preisgestaltung Stufe<sup>2</sup>|Dies ist die maximale Anzahl der verbundenen Clients gleichzeitig zulässig. Wenn der Grenzwert erreicht wird Redis alle neuen Verbindungen senden Fehler "maximale Anzahl von Clients erreicht" geschlossen.|
|Maxmemory-Richtlinie|flüchtige lru|Maxmemory Richtlinie ist die Einstellung wie Redis auswählen zu entfernen, wenn Maxmemory (die Größe des Cache bietet ausgewählt, wenn der Cache erstellt) erreicht wird. Die Standardeinstellung ist Azure Redis Cache Volatile-Lru, wodurch Schlüssel mit einem laufen unter Verwendung eines LRU-Algorithmus festlegen. Diese Einstellung kann im Azure-Portal konfiguriert. Weitere Informationen finden Sie unter [Maxmemory Politik und Maxmemory](#maxmemory-policy-and-maxmemory-reserved).|
|Maxmemory-Beispiele|3|LRU und minimale TTL-Algorithmen nicht präzise Algorithmen sondern angenähert Algorithmen (um Speicherplatz zu sparen) sowie die Größe der Stichprobe überprüfen auswählen können. Standardmäßige beispielsweise Redis Tasten überprüfen und auswählen, das weniger zuletzt verwendet wurde.|
|LUA-Zeitlimit|5.000|Maximale Ausführungszeit eines Lua-Skripts in Millisekunden. Erreicht die Höchstausführungszeit meldet Redis, die ein Skript noch in Ausführung die maximale Zeit und werden Antworten auf Abfragen mit einem Fehler.|
|LUA-Ereignis-limit|500|Dies ist die maximale Größe des Skript-Ereigniswarteschlange.|
|Client-Ausgabe-Puffer-Limit Normalclient Ausgabe Buffer Limit pubsub|0 0 032 8mb-60 mb|Trennung der Clientverbindung erzwungen, das keine Daten vom Server schnell genug aus irgendeinem Grund gelesen werden (eine häufige Ursache ist das Pub-Sub-Client kann so schnell wie der Verleger erzeugen kann Nachrichten verwenden) können die Leistungsgrenzen Puffer Client verwendet werden. Weitere Informationen finden Sie unter [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<a name="databases"></a>
<sup>1</sup> Das Limit für `databases` wird für jede Azure Redis Cache Tarif und können zur Erstellung des Caches festgelegt werden. Wenn kein `databases` angegebene Einstellung während der Erstellung des Caches, der Standardwert ist 16.

-   Grundlegende und Standard-Caches
    -   C0 (250 MB) Cache – bis zu 16 Datenbanken
    -   C1 (1 GB) Cache – bis zu 16 Datenbanken
    -   C2 (2,5 GB) Cache – bis zu 16 Datenbanken
    -   C3 (6 GB) Cache – bis zu 16 Datenbanken
    -   C4 (13 GB) Cache - bis zu 32 Datenbanken
    -   C5 (26 GB) Cache - bis zu 48 Datenbanken
    -   C6 (53 GB) Cache - bis 64-Datenbanken
-   Premium-caches
    -   P1 (6 GB - 60 GB) - bis zu 16 Datenbanken
    -   P2 (13 bis 130 GB) - bis zu 32 Datenbanken
    -   P3 (26 bis 260 GB) - bis zu 48 Datenbanken
    -   P4 (53 GB - 530 GB) - bis zu 64 Datenbanken
    -   Alle Premium-Caches mit Redis aktiviert - Redis Cluster nur Datenbank 0 unterstützt die `databases` für alle Premium-Cache mit Redis aktiviert 1 ist und der Befehl [Select](http://redis.io/commands/select) nicht darf einschränken. Weitere Informationen finden Sie unter [müssen meine Clientanwendung clustering verwenden ändern?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] Die `databases` Einstellung kann nur während der Erstellung des Caches konfiguriert ist und nur mit PowerShell, CLI oder anderen Management. Ein Beispiel für die Konfiguration von `databases` während der Erstellung des Caches mit PowerShell, finden Sie unter [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` wird für jede Azure Redis Cache Tarif.

-   Grundlegende und Standard-Caches
    -   C0 (250 MB) Cache - 256 Verbindungen
    -   C1 (1 GB) Cache - 1.000 Verbindungen
    -   C2 (2,5 GB) Cache - 2.000 Verbindungen
    -   C3 (6 GB) Cache - 5.000 Verbindungen
    -   C4 (13 GB) Cache - 10.000 Verbindungen
    -   C5 (26 GB) Cache - 15.000 Verbindungen
    -   C6 (53 GB) Cache - 20.000 Verbindungen
-   Premium-caches
    -   P1 (6 GB - 60 GB) - bis zu 7.500 Anschlüsse
    -   P2 (13 bis 130 GB) - bis zu 15.000 Verbindungen
    -   P3 (26 bis 260 GB) - 30.000 Verbindungen
    -   P4 (53 GB - 530 GB) - 40.000 Verbindungen

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis Befehle in Azure Redis Cache nicht unterstützt

>[AZURE.IMPORTANT] Da Konfiguration und Verwaltung von Azure Redis Cache Instanzen von Microsoft verwaltet werden die folgenden Befehle deaktiviert. Wenn Sie versuchen, sie aufzurufen erhalten Sie eine Fehlermeldung ähnlich `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  KONFIGURATION
>-  DEBUGGEN
>-  MIGRIEREN
>-  SPEICHERN
>-  HERUNTERFAHREN
>-  SLAVEOF
>-  CLUSTER – Cluster schreiben Befehle deaktiviert, aber schreibgeschützten Cluster Befehle zulässig sind.

Weitere Informationen zu Redis-Befehlen finden Sie unter [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Konsole redis

Sie können sicher Befehle zu Azure Redis Cache Instanzen der **Konsole Redis**für Standard steht und Premium zwischengespeichert.

>[AZURE.IMPORTANT] Redis-Konsole funktioniert nicht mit VNET-clustering und Datenbanken als 0. 
>
>-  [VNET](cache-how-to-premium-vnet.md) - der Teil einer VNET sind, können nur Clients in dem VNET Cache zugreifen. Redis-Konsole verwendet gehosteten virtuellen Computer, die nicht Teil der VNET Redis cli.exe-Client kann nicht mit Cache verbunden werden.
>-  [Clustering](cache-how-to-premium-clustering.md) - der Redis-Konsole verwendet Redis cli.exe-Client, der clustering zu diesem Zeitpunkt nicht unterstützt. Das Dienstprogramm Redis Cli in der [instabil](http://redis.io/download) Redis Repository bei GitHub implementiert grundlegende Support mit den `-c` wechseln. Weitere Informationen finden Sie auf [http://redis.io](http://redis.io) im [Cluster-Lernprogramm Redis](http://redis.io/topics/cluster-tutorial) [Spielen mit dem Cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) .
>-  Redis-Konsole stellt eine neue Verbindung 0 jeder Datenbank einen Befehl senden. Sie können keine der `SELECT` Befehl eine andere Datenbank auswählen, da die Datenbank auf 0 bei jedem Befehl zurücksetzen. Weitere Informationen zum Ausführen von Redis Befehle, wie z. B. in eine andere Datenbank ändern [wie können Sie Redis Befehle ausführen?](cache-faq.md#how-can-i-run-redis-commands)

Zugriff auf die Konsole Redis klicken Sie auf **Konsole** **Redis Cache** -Blade.

![Konsole redis](./media/cache-configure/redis-console-menu.png)

Um Befehle für die Cacheinstanz Geben Sie einfach den gewünschten Befehl in die Konsole.

![Konsole redis](./media/cache-configure/redis-console.png)

Liste Azure Redis Cache deaktiviert Redis-Befehle finden Sie in Abschnitt [Redis Befehle in Azure Redis Cache nicht unterstützt](#redis-commands-not-supported-in-azure-redis-cache) . Weitere Informationen zu Redis-Befehlen finden Sie unter [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>Verschieben Sie Ihren Cache in ein neues Abonnement

Auf **Verschieben**, um den Cache auf ein neues Abonnement zu verschieben.

![Redis Cache verschieben](./media/cache-configure/redis-cache-move.png)

Informationen zum Verschieben von Ressourcen aus einer Ressourcengruppe und von einem Abonnement zu einem anderen finden Sie unter [Ressourcen neue Ressourcengruppe oder Abonnement](../resource-group-move-resources.md).

## <a name="next-steps"></a>Nächste Schritte
-   Weitere Informationen zum Arbeiten mit Redis-Befehlen finden Sie unter [wie können Sie Redis Befehle ausführen?](cache-faq.md#how-can-i-run-redis-commands).
