<properties
   pageTitle="Verwendung von Azure Redis Cache mit Java | Microsoft Azure"
    description="Erste Schritte mit Azure Redis Cache mit Java"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Verwendung von Azure Redis Cache mit Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache haben auf eine dedizierte Zugriff Redis Cache von Microsoft verwaltet. Der Cache kann jede Anwendung in Microsoft Azure zugegriffen werden.

In diesem Thema veranschaulicht Einstieg in Azure Redis Cache mit Java.

## <a name="prerequisites"></a>Erforderliche Komponenten

[Jedis](https://github.com/xetorthio/jedis) - Java-Client für Redis

Diese praktische Einführung verwendet Jedis, aber jeder aufgelisteten [http://redis.io/clients](http://redis.io/clients)Java-Client verwenden.

## <a name="create-a-redis-cache-on-azure"></a>Redis Cache auf Azure erstellen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Die Host-Namen und Schlüssel abrufen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Den Endpunkt nicht SSL aktivieren

Einige Redis-Clients nicht unterstützt SSL und standardmäßig [nicht SSL-Anschluss für neue Instanzen von Azure Redis Cache deaktiviert ist](cache-configure.md#access-ports). Zum Zeitpunkt der Veröffentlichung dieses Dokuments unterstützen nicht [Jedis](https://github.com/xetorthio/jedis) Client SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Fügen Sie zum Cache hinzu und Abrufen

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Nächste Schritte

- [Cache Diagnose aktivieren](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) , können Sie [Überwachen](https://msdn.microsoft.com/library/azure/dn763945.aspx) den Zustand des Cache.
- Lesen Sie den offiziellen [Dokumentation Redis](http://redis.io/documentation).

