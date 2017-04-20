<properties 
    pageTitle="Cache Migrieren zu Redis | Microsoft Azure"
    description="Informationen Sie zum Cache Service Managed Applications Azure Redis Cache migrieren"
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
    ms.date="09/30/2016"
    ms.author="sdanie" />

# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Migrieren von verwalteten Cachedienst Azure Redis Cache

Migrieren einer Anwendung die Azure verwaltet Cache Dienstes Azure Redis Cache kann mit minimalem caching Anwendung Managed Cache Service-Funktionen der Anwendung erfolgen. Während der APIs nicht identisch sind ähnlich und viele der vorhandenen Code zum Managed Cache Service Cache zugreifen kann mit minimaler wiederverwendet werden. In diesem Thema veranschaulicht, wie die erforderlichen Konfigurationsschritte und Anwendung migrieren Cachedienst verwaltete Anwendung Azure Redis verwendet und veranschaulicht, wie einige der Features von Azure Redis Cache zum Implementieren der Funktionen des Managed Cache Service-Cache verwendet werden kann.

## <a name="migration-steps"></a>Schritte der Migration

Folgende Schritte müssen zur Migration einer Anwendung verwaltet Cachedienst Azure Redis verwendet.

-   Azure Redis Cache Cachedienst verwalteten Eigenschaften zuordnen
-   Wählen Sie eine Cacheangebot
-   Erstellen Sie einen Cache
-   Die Cacheclients konfigurieren
    -   Konfiguration der verwalteten Cache entfernen
    -   Konfigurieren Sie einen Cacheclient StackExchange.Redis NuGet-Paket
-   Migrieren von Code von Managed Cache Service
    -   Verbindung zum Cache mithilfe der ConnectionMultiplexer-Klasse
    -   Access-primitive Datentypen im cache
    -   Arbeiten Sie mit .NET Objekte im cache
-   Migrieren von ASP.NET Session State und Azure Redis Cache Zwischenspeichern der Ausgabe 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Azure Redis Cache Cachedienst verwalteten Eigenschaften zuordnen

Azure Managed Cache Service und Azure Redis Cache ähneln, aber einige Features unterschiedlich implementiert. Dieser Abschnitt beschreibt einige der Unterschiede und enthält Richtlinien zum Implementieren von Features von Managed Cache Service in Azure Redis Cache.

|Verwaltete Cache Service-Funktion|Verwaltete Cache Unterstützung|Azure Redis Cache-Unterstützung|
|---|---|---|
|Benannten caches|Ein Standardcache konfiguriert und Angebote bis zu neun zusätzliche benannten Caches können die Standard- und Premium-Cache bei Bedarf konfiguriert werden.|Azure Redis Cache haben eine konfigurierbare Anzahl von Datenbanken (Standardwert 16), die eine ähnliche Funktionalität benannten Caches verwendet werden können. Weitere Informationen finden Sie unter [Serverkonfiguration Standard Redis](cache-configure.md#default-redis-server-configuration).|
|Hohe Verfügbarkeit|Bietet hohe Verfügbarkeit für Elemente im Cache in Standard- und Premium-cacheangeboten. Wenn Elemente aufgrund eines Fehlers, sind Sicherungskopien der Elemente im Cache verfügbar. Sekundärer Cache schreibt erfolgt synchron.|Hoher Verfügbarkeit ist in Standard- und Premium-cacheangebote, die eine Konfiguration mit zwei Knoten primär-Replikat (jeder Splitter in einem Premium-Cache hat eine primäre-Replikat) verfügbar. Schreibt das Replikat erfolgen asynchron. Weitere Informationen finden Sie unter [Azure Redis Cache Preise](https://azure.microsoft.com/pricing/details/cache/).|
|Benachrichtigung|Ermöglicht Clients die asynchrone Benachrichtigung erhalten, wenn eine Vielzahl von Cache-Operationen auf einem benannten Cache auftreten.|Clientanwendungen können Redis Pub/Sub oder [erledigten Anträge](cache-configure.md#keyspace-notifications-advanced-settings) um eine ähnliche Funktionalität Benachrichtigungen zu erreichen.|
|Lokalen cache|Speichert eine Kopie der zwischengespeicherten Objekte lokal auf dem Client für extrem schnellen Zugriff.|Clientanwendungen müssen dies mit einem Wörterbuch oder ähnliche Datenstruktur zu implementieren.|
|Entfernungsrichtlinie|Keines LRU. Die Standardrichtlinie ist LRU.|Azure Redis Cache unterstützt die folgenden Auslagerungsrichtlinien: flüchtige Lru Allkeys Lru Volatile random, Allkeys Zufallszahlen Volatile-Ttl, Noeviction. Die Standardrichtlinie ist flüchtig Lru. Weitere Informationen finden Sie unter [Serverkonfiguration Standard Redis](cache-configure.md#default-redis-server-configuration).|
|Ablaufrichtlinie|Ist Absolute Ablaufrichtlinie Standard und das Ablaufintervall Standard ist 10 Minuten. Schieben und nie sind ebenfalls verfügbar.|Standardmäßig Elemente im Cache laufen nicht, aber eine anhand einer pro Write Cache Satz Overloads konfiguriert werden. Weitere Informationen finden Sie unter [Hinzufügen und Abrufen von Objekten aus dem Cache](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache).|
|Regionen und Tags|Bereiche sind Untergruppen für zwischengespeicherte Elemente. Bereiche unterstützt auch die Anmerkung von zwischengespeicherten Elementen mit zusätzlichen beschreibenden Tags bezeichnet. Bereiche unterstützen Suchzugriffe auf gekennzeichnete Elemente in diesem Bereich. Alle Elemente in einem Bereich befinden sich in einem einzelnen Knoten des Cacheclusters.|Redis Cache besteht aus einem einzelnen Knoten (außer Redis Cluster aktiviert ist) also nicht das Konzept der Cachedienst verwalteten Bereiche. Redis unterstützt die Suche und Platzhalter Vorgänge beim Schlüssel abrufen beschreibenden Tags eingebettet Schlüsselnamen und Elemente später abgerufen. Ein Beispiel zum Implementieren einer Tag-Lösung mit Redis finden Sie unter [Cache mit Redis tagging implementieren](http://stackify.com/implementing-cache-tagging-redis/).|
|Serialisierung|Verwaltete Cache unterstützt NetDataContractSerializer BinaryFormatter und die Verwendung benutzerdefinierter Serialisierungsprogramme. Der Standardwert ist NetDataContractSerializer.|Es obliegt der Clientanwendung .NET Objekte vor dem einordnen in den Cache mit der Wahl des Serialisierungsprogramms Entwickler Anwendung Client serialisiert. Weitere Informationen und Beispielcode finden Sie unter [Arbeiten mit .NET Objekte im Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).|
| Cache-emulator | Verwaltete Cache bietet einen lokalen Cache-Emulator. | Azure Redis Cache keinen Emulator können Sie [Build MSOpenTech von Redis-server.exe lokal ausführen](cache-faq.md#cache-emulator) , um einen Emulator bereitzustellen. |

## <a name="choose-a-cache-offering"></a>Wählen Sie eine Cacheangebot

Microsoft Azure Redis Cache ist in folgenden Kategorien verfügbar:

-   **Basic** -Knoten. Mehrere Größen bis zu 53 GB.
-   **Standard** – zwei Knoten primär/Replikat. Mehrere Größen bis zu 53 GB. 99,9 % SLA.
-   **Premium** -Knoten zwei primäre/Replikat mit bis zu 10. Mehrere Größen von 6 GB 530 GB (Kontakt mehr). Alle Standard-Tier-Funktionen und weitere mit Unterstützung für [Cluster Redis](cache-how-to-premium-clustering.md) [Redis Persistenz](cache-how-to-premium-persistence.md)und [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 % SLA.

Jede Ebene unterscheidet sich Funktionen und Preisen. Die Funktionen sind in diesem Handbuch und Weitere Informationen zu Preisen, siehe [Cache Preise](https://azure.microsoft.com/pricing/details/cache/).

Ausgangspunkt für die Migration ist die Größe, die der vorherigen Managed Cache Service Cache entspricht, und skalieren nach oben oder unten, je nach Ihrer Anwendung. Weitere Informationen über die Auswahl rechts Azure Redis Cache-Angebot finden Sie unter [welche Redis cacheangebot und Größe sollte ich verwenden?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Erstellen Sie einen Cache

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Die Cacheclients konfigurieren

Sobald der Cache erstellt und konfiguriert, Nächstes Dienstkonfiguration verwalteten Cache entfernen und Hinzufügen der Azure Redis Cache-Konfiguration hinzufügen und verweist, cacheclients auf den Cache zugreifen können.

-   Konfiguration der verwalteten Cache entfernen
-   Konfigurieren Sie einen Cacheclient StackExchange.Redis NuGet-Paket

### <a name="remove-the-managed-cache-service-configuration"></a>Konfiguration der verwalteten Cache entfernen

Clientanwendungen für Azure Redis Cache der vorhandenen verwalteten Cache Konfiguration konfiguriert werden und Assemblyverweise Managed Cache Service NuGet-Paket deinstallieren entfernt werden müssen.

Managed Cache Service NuGet-Paket deinstallieren Maustaste auf das Clientprojekt im **Projektmappen-Explorer** und **NuGet-Pakete verwalten**. Wählen Sie den Knoten **Pakete installiert** , und geben Sie W**indowsAzure.Caching** in das Suchfeld der installierten Pakete. Wählen Sie **Windows Azure** **Cache** (oder **Windows Azure** **Caching** je NuGet-Paket) und klicken Sie dann auf **Schließen**klicken Sie auf **Deinstallieren**.

![Azure verwalteten Cache Service NuGet-Paket deinstallieren](./media/cache-migrate-to-redis/IC757666.jpg)

Deinstallieren Managed Cache Service NuGet-Paket entfernt Cachedienst verwaltete Assemblys und die Einträge im Cache verwaltet in app.config oder web.config der Clientanwendung Da einige benutzerdefinierten Einstellungen nicht entfernt werden können als NuGet-Paket deinstallieren, öffnen Sie web.config oder app.config und sicherzustellen Sie, dass die folgenden Elemente entfernt werden.

Sicherstellen, dass die `dataCacheClients` Eintrag entfernt die `configSections` Element. Entfernen Sie nicht die gesamte `configSections` Element. Entfernen Sie die `dataCacheClients` Eintrag, falls vorhanden.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

Sicherstellen, dass die `dataCacheClients` Abschnitt entfernt. Die `dataCacheClients` Abschnitt werden ähnlich wie im folgenden Beispiel.

    <dataCacheClients>
      <dataCacheClientname="default">
        <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
        <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="true">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>

Managed Cache Service-Konfiguration entfernt, können Sie den Cacheclient konfigurieren, wie im folgenden Abschnitt beschrieben.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Konfigurieren Sie einen Cacheclient StackExchange.Redis NuGet-Paket

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Migrieren von Code von Managed Cache Service

Die API für den Cacheclient StackExchange.Redis ähnelt dem Cachedienst verwaltet. Dieser Abschnitt enthält eine Übersicht über die Unterschiede.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Verbindung zum Cache mithilfe der ConnectionMultiplexer-Klasse

Managed Cache Service Verbindungen im Cache von behandelt wurden die `DataCacheFactory` und `DataCache` Klassen. In Azure Redis Cache erfolgt die Verbindung über die `ConnectionMultiplexer` Klasse.

Fügen Sie die folgende Anweisung am Anfang einer Datei aus dem Cache zugreifen möchten.

    using StackExchange.Redis
                                
Sollten Sie dieser Namespace nicht behoben wird, StackExchange.Redis NuGet-Paket hinzugefügt haben, wie [die cacheclients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)konfigurieren.

>[AZURE.NOTE] Beachten Sie, dass der Client StackExchange.Redis.NET Framework 4 oder höher benötigt.

Rufen Sie die statische Verbindung mit einer Azure Redis Cacheinstanz `ConnectionMultiplexer.Connect` -Methode und übergeben den Endpunkt und Schlüssel. Ein Ansatz für die Freigabe einer `ConnectionMultiplexer` Instanz in der Anwendung ist eine statische Eigenschaft, die eine verbundene Instanz, wie im folgenden Beispiel gibt. Auf diese Weise threadsicher nur eine einzige Verbindung initialisiert `ConnectionMultiplexer` Instanz. In diesem Beispiel `abortConnect` ist auf False festgelegt, was bedeutet, dass der Aufruf erfolgreich ist, auch wenn keine Verbindung zum Cache hergestellt wird. Eines der wichtigsten Merkmale von `ConnectionMultiplexer` Neustart wird Konnektivität zum Cache einmal Netzwerkproblem oder andere Ursachen aufgelöst wird.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Cacheendpunkt, Schlüsseln und Ports **Redis Cache** -Blade für die Cacheinstanz entnehmen. Weitere Informationen finden Sie unter [Eigenschaften von Redis-Cache](cache-configure.md#properties).

Sobald die Verbindung hergestellt ist, einen Verweis auf die Redis-Cache-Datenbank zurückkehren, indem die `ConnectionMultiplexer.GetDatabase` Methode. Zurückgegebenes Objekt die `GetDatabase` -Methode ein einfaches Pass-Through-Objekt und müssen nicht gespeichert werden.

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Der StackExchange.Redis-Client verwendet die `RedisKey` und `RedisValue` Typen für den Zugriff auf und Speichern von Elementen im Cache. Diese Karte auf die Sprache Grundtypen, einschließlich, und häufig sind nicht direkt verwendet. Redis Zeichenfolgen sind die grundlegenden Redis Wert und viele Arten von Daten, einschließlich der serialisierten binäre Datenströme enthalten und den Typ nicht direkt verwenden können, verwenden Sie Methoden enthalten `String` im Namen. Für die meisten primitiven Datentypen speichern und Abrufen von Elementen aus dem Cache mithilfe der `StringSet` und `StringGet` Methoden, sofern Sie Sammlungen oder andere Redis Daten im Cache speichern. 

`StringSet`und `StringGet` ähneln dem Cachedienst verwaltet `Put` und `Get` mit einem wichtigen Unterschied, bevor Sie ein in den Cache abrufen Sie es zuerst serialisieren müssen. 

Beim Aufruf von `StringGet`existiert das Objekt zurückgegeben, und wenn nicht, wird Null zurückgegeben. In diesem Fall können Sie den Wert der gewünschten Datenquelle abzurufen und im Cache für spätere Verwendung speichern. Dies wird als Cache-Aside-Muster bezeichnet.

Verwenden Sie zum angeben die Ablaufzeit eines Elements im Cache der `TimeSpan` Parameter der `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure Redis Cache kann mit .NET Objekte primitive Datentypen, aber bevor ein zwischengespeichert werden kann er serialisiert werden muss. Dies ist der Verantwortung des Anwendungsentwicklers. Diese Flexibilität Entwickler bei der Auswahl des Serialisierungsprogramms. Weitere Informationen und Beispielcode finden Sie unter [Arbeiten mit .NET Objekte im Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>Migrieren von ASP.NET Session State und Azure Redis Cache Zwischenspeichern der Ausgabe

Azure Redis Cache enthält Anbieter für ASP.NET Sitzungsstatus und Seitenausgabe-caching. Um die Anwendung zu migrieren, die Cachedienst verwaltete Versionen dieser Anbieter verwendet vorhandene Abschnitte zuerst von web.config entfernen und Azure Redis Cache Versionen der Anbieter konfigurieren. Informationen mithilfe des Azure Redis Cache ASP.NET finden Sie unter [ASP.NET Sitzungszustandsanbieter Azure Redis Cache](cache-aspnet-session-state-provider.md) und [ASP.NET Ausgabecacheanbieter für Azure Redis Cache](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Nächste Schritte

Durchsuchen der [Dokumentation Azure Redis Cache](https://azure.microsoft.com/documentation/services/cache/) für Lernprogramme, Beispiele und Videos.

