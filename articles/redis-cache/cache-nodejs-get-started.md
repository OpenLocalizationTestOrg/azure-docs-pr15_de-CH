<properties
    pageTitle="Verwendung von Azure Redis Cache mit Node.js | Microsoft Azure"
    description="Erste Schritte mit Azure Redis Cache Node.js mit Node_redis."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Verwendung von Azure Redis Cache mit Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache erhalten Sie Zugriff auf einen sicheren, dedizierten Redis Cache von Microsoft verwaltet. Der Cache kann jede Anwendung in Microsoft Azure zugegriffen werden.

In diesem Thema veranschaulicht Einstieg in Azure Redis Cache mit Node.js. Beispielsweise Node.js Azure Redis Cache mit finden Sie unter [Erstellen einer Node.js Chat-Anwendung mit Socket.IO Azure-Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie [Node_redis](https://github.com/mranney/node_redis):

    npm install redis

In diesem Lernprogramm wird [Node_redis](https://github.com/mranney/node_redis)verwendet. Beispiele mit anderen Node.js-Clients finden Sie im einzelnen zu Node.js Clients [Node.js Redis](http://redis.io/clients#nodejs)Clients aufgelistet.

## <a name="create-a-redis-cache-on-azure"></a>Redis Cache auf Azure erstellen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Die Host-Namen und Schlüssel abrufen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Cache sicher über SSL Verbinden

Die neuesten Builds von [Node_redis](https://github.com/mranney/node_redis) unterstützen Azure Redis Cache mit SSL verwenden. Im folgende Beispiel veranschaulicht Azure Redis Cache Verbindung mithilfe des SSL-Endpunktes des 6380. Ersetzen Sie `<name>` mit dem Namen des Cache und `<key>` mit der primären oder sekundären als beschriebenen vorangegangenen [Hostschlüssel Name und Abrufen](#retrieve-the-host-name-and-access-keys) .

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Fügen Sie zum Cache hinzu und Abrufen

Im folgende Beispiel wird das Verbinden mit einer Instanz Azure Redis Cache speichern und Abrufen eines Elements aus dem Cache veranschaulicht. Weitere Beispiele für die Redis mit der [Node_redis](https://github.com/mranney/node_redis) -Client finden Sie unter [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Ausgabe:

    OK
    value


## <a name="next-steps"></a>Nächste Schritte

- [Cache Diagnose aktivieren](cache-how-to-monitor.md#enable-cache-diagnostics) , können Sie [Überwachen](cache-how-to-monitor.md) den Zustand des Cache.
- Lesen Sie den offiziellen [Dokumentation Redis](http://redis.io/documentation).



