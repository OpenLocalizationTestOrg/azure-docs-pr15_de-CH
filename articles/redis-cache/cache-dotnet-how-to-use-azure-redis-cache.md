<properties 
    pageTitle="Wie Azure Redis Cache | Microsoft Azure" 
    description="Erfahren Sie, wie Ihre Azure Applications Azure Redis Cache verbessern" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Wie Azure Redis Cache

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Dieses Handbuch veranschaulicht den Einstieg in **Azure Redis Cache**. Microsoft Azure Redis Cache anhand gängiger open Source Redis Cache. Sie erhalten Sie Zugriff auf einen sicheren, dedizierten Redis Cache von Microsoft verwaltet. Ein Cache mit Azure Redis Cache erstellt ist über jede Anwendung in Microsoft Azure.

Microsoft Azure Redis Cache ist in folgenden Kategorien verfügbar:

-   **Basic** -Knoten. Mehrere Größen bis zu 53 GB.
-   **Standard** – zwei Knoten primär/Replikat. Mehrere Größen bis zu 53 GB. 99,9 % SLA.
-   **Premium** -Knoten zwei primäre/Replikat mit bis zu 10. Mehrere Größen von 6 GB 530 GB (Kontakt mehr). Alle Standard-Tier-Funktionen und weitere mit Unterstützung für [Cluster Redis](cache-how-to-premium-clustering.md) [Redis Persistenz](cache-how-to-premium-persistence.md)und [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 % SLA.

Jede Ebene unterscheidet sich Funktionen und Preisen. Informationen zu Preisen finden Sie unter [Cache Preisdetails][].

Dieses Handbuch zeigt wie den [StackExchange.Redis][] Client C\# Code. Die fallen Szenarien **Erstellen und konfigurieren einen Cache**, **cacheclients konfigurieren**und **Hinzufügen und Entfernen von Objekten aus dem Cache**. Weitere Informationen zur Verwendung von Azure Redis Cache finden Sie im Abschnitt [Nächste Schritte][] . Ein schrittweises Lernprogramm erstellen Sie ein ASP.NET MVC WebApp mit Redis Cache finden Sie unter [Web App mit Redis Cache erstellen](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Erste Schritte mit Azure Redis Cache

Erste Schritte mit Azure Redis Cache ist einfach. Zunächst bereitgestellt und einen Cache konfigurieren. Konfigurieren Sie nun die cacheclients so, dass sie den Cache zugreifen können. Sobald die cacheclients konfiguriert sind, können Sie mit ihnen zu arbeiten beginnen.

-   [Cache erstellen][]
-   [Die cacheclients konfigurieren][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Erstellen Sie einen cache

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Zugriff auf den Cache dahinter wird erstellt

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Weitere Informationen zum Konfigurieren des Caches finden Sie unter [Azure Redis Cache konfigurieren](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Die cacheclients konfigurieren

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Nachdem das Clientprojekt zum Zwischenspeichern konfiguriert wurde, können Sie in den folgenden Abschnitten Arbeiten mit Ihren Cache beschriebenen Techniken verwenden.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Arbeiten mit Cache

Die Schritte in diesem Abschnitt beschrieben, wie häufige Aufgaben mit Cache.

-   [Verbinden mit cache][]
-   [Hinzufügen und Abrufen von Objekten aus dem cache][]
-   [Arbeiten Sie mit .NET Objekte im cache](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Verbinden mit cache

Programmgesteuert mit einem Cache arbeiten, benötigen Sie einen Verweis auf den Cache. Fügen Sie folgenden am Anfang einer Datei aus dem gewünschten StackExchange.Redis Client auf einen Azure Redis Cache.

    using StackExchange.Redis;

>[AZURE.NOTE] Der StackExchange.Redis-Client erfordert.NET Framework 4 oder höher.

Verbindung zu Azure Redis Cache wird von der `ConnectionMultiplexer` Klasse. Diese Klasse dient zur Freigabe und Wiederverwendung in der Clientanwendung und nicht pro Vorgang erstellt werden müssen. 

Ein Azure Redis Cache und eine Instanz eines zurückgegeben `ConnectionMultiplexer`, rufen Sie die statische `Connect` -Methode und übergeben das cacheendpunkt und den Schlüssel wie im folgende Beispiel. Verwenden von Azure-Verwaltungsportal als der Kennwortparameter generierte Schlüssel.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Warnung: Niemals Speicher Anmeldeinformationen im Quellcode. Um dieses Beispiel möglichst einfach zu halten, zeige sie im Quellcode ich. Informationen zum Speichern von Anmeldeinformationen finden Sie unter [Zeichenfolgen wie Anwendungszeichenfolgen und Verbindung][] .

Wenn Sie SSL verwenden möchten, legen Sie entweder `ssl=false` oder Auslassen der `ssl` Parameter.

>[AZURE.NOTE] Nicht-SSL-Port ist standardmäßig für neue Zwischenspeicher deaktiviert. Hinweise auf nicht-SSL-Anschluss finden Sie unter [Zugriff auf Ports](cache-configure.md#access-ports)...

Ein Ansatz für die Freigabe einer `ConnectionMultiplexer` Instanz in der Anwendung ist eine statische Eigenschaft, die eine verbundene Instanz, wie im folgenden Beispiel gibt. Auf diese Weise threadsicher nur eine einzige Verbindung initialisiert `ConnectionMultiplexer` Instanz. In diesen Beispielen `abortConnect` ist auf False festgelegt, was bedeutet, dass der Aufruf erfolgreich ist, auch wenn keine Verbindung zum Azure Redis Cache hergestellt wird. Eines der wichtigsten Merkmale von `ConnectionMultiplexer` Neustart wird Konnektivität zum Cache einmal Netzwerkproblem oder andere Ursachen aufgelöst wird.

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

Weitere Informationen zu erweiterten Verbindungsoptionen Konfiguration finden Sie unter [StackExchange.Redis Konfigurationsmodell][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Sobald die Verbindung hergestellt ist, einen Verweis auf die Redis-Cache-Datenbank zurückkehren, indem die `ConnectionMultiplexer.GetDatabase` Methode. Zurückgegebenes Objekt die `GetDatabase` -Methode ein einfaches Pass-Through-Objekt und müssen nicht gespeichert werden.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Damit Sie wissen, wie eine Instanz Azure Redis Cache und einen Verweis auf die Cache-Datenbank, sehen wir uns ein mit dem Cache.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Hinzufügen und Abrufen von Objekten aus dem cache

Elemente können in gespeichert und aus einem Cache abgerufen werden, indem die `StringSet` und `StringGet` Methoden.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis speichert die meisten Redis Zeichenfolgen, aber diese Zeichenfolgen Daten können viele Arten von Daten, einschließlich der serialisierte Binärdaten, die für die Speicherung .NET Objekte im Cache verwendet werden kann.

Beim Aufruf von `StringGet`, wenn das Objekt vorhanden ist, es wird zurückgegeben, und wenn nicht, `null` zurückgegeben. In diesem Fall können Sie den Wert der gewünschten Datenquelle abzurufen und im Cache für spätere Verwendung speichern. Dies wird als Cache-Aside-Muster bezeichnet.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Verwenden Sie zum angeben die Ablaufzeit eines Elements im Cache der `TimeSpan` Parameter der `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Arbeiten Sie mit .NET Objekte im cache

Azure Redis können cache .NET Objekte sowie primitive Datentypen, aber bevor ein zwischengespeichert werden kann er serialisiert werden muss. Dies liegt in der Verantwortung des Anwendungsentwicklers und Entwickler Flexibilität bei der Wahl des Serialisierungsprogramms.

Eine einfache Möglichkeit zum Serialisieren von Objekten ist die Verwendung der `JsonConvert` Serialisierungsmethoden in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) und in und aus JSON serialisiert. Das folgende Beispiel zeigt eine Get und Set mit einem `Employee` Objektinstanz.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Nächste Schritte

Da Sie die Grundlagen beherrschen, folgen Sie diesen Links erfahren Sie mehr über Azure Redis Cache.

-   ASP.NET Anbieter Azure Redis Cache anzeigen
    -   [Azure Redis Sitzungszustand Anbieter](cache-aspnet-session-state-provider.md)
    -   [Azure Redis Cache ASP.NET Ausgabecacheanbieter](cache-aspnet-output-cache-provider.md)
-   [Cache Diagnose aktivieren](cache-how-to-monitor.md#enable-cache-diagnostics) , können Sie [Überwachen](cache-how-to-monitor.md) den Zustand des Cache. Anzeigen der Metriken in Azure-Portal und können Sie [herunterladen und überprüfen](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) die Werkzeuge Ihrer Wahl.
-   [StackExchange.Redis Cache Kundendokumentation][]anzeigen
    -   Azure Redis Cache Redis Clients und Sprachen möglich. Weitere Informationen finden Sie unter [http://redis.io/clients][].
-   Azure Redis Cache kann auch mit Drittanbieter-Services und Tools wie Redsmin und Redis Desktop Manager verwendet werden.
    -   Weitere Informationen zu Redsmin finden Sie unter [Abrufen einer Verbindungszeichenfolge Azure Redis und mit Redsmin][].
    -   Zugriff auf und überprüfen Sie Ihre Daten in Azure Redis Cache mit einer GUI mithilfe von [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
-   [Redis][] Dokumentation und [redis Datentypen][] und [eine 15-Minuten-Datentypen Redis Einführung][]lesen.



<!-- INTRA-TOPIC LINKS -->
[Nächste Schritte]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Cache erstellen]: #create-cache
[Configure the cache]: #enable-caching
[Die cacheclients konfigurieren]: #NuGet
[Working with Caches]: #working-with-caches
[Verbinden mit cache]: #connect-to-cache
[Hinzufügen und Abrufen von Objekten aus dem cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/Clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Zum Abrufen einer Verbindungszeichenfolge Azure Redis und mit Redsmin verwenden]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis Konfigurationsmodell]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache-Preise]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis Cache Kundendokumentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis Datentypen]: http://redis.io/topics/data-types
[eine 15-Minuten-Einführung in Redis Datentypen]: http://redis.io/topics/data-types-intro

[Funktionsweise von Anwendungszeichenfolgen und Verbindungszeichenfolgen]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


