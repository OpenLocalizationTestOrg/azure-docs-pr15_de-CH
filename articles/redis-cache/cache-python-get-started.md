<properties
    pageTitle="Verwendung von Azure Redis Cache mit Python | Microsoft Azure"
    description="Erste Schritte mit Azure Redis Cache mit Python"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Verwendung von Azure Redis Cache mit Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

In diesem Thema veranschaulicht Einstieg in Azure Redis Cache mit Python.


## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie [Redis kopieren](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Redis Cache auf Azure erstellen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Die Host-Namen und Schlüssel abrufen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Den Endpunkt nicht SSL aktivieren

Einige Redis-Clients nicht unterstützt SSL und standardmäßig [nicht SSL-Anschluss für neue Instanzen von Azure Redis Cache deaktiviert ist](cache-configure.md#access-ports). Zum Zeitpunkt der Veröffentlichung dieses Dokuments unterstützen nicht [Redis Py](https://github.com/andymccurdy/redis-py) Client SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Fügen Sie zum Cache hinzu und Abrufen


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Ersetzen Sie `<name>` mit Ihrem Cachenamen und `key` und der Zugriffstaste.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
