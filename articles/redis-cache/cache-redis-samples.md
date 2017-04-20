<properties 
    pageTitle="Azure Redis Cache-Beispiele | Microsoft Azure" 
    description="Informationen Sie zum Verwenden von Azure Redis Cache" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Azure Redis Cache-Beispiele 

Dieses Thema enthält eine Liste der Azure Redis Cache Beispiele für Szenarien wie Cache, lesen und Schreiben von Daten aus einem Cache Verbindung mithilfe des ASP.NET Redis Cache. Beispiele sind herunterladbare Projekte und einige bieten schrittweise und Einschließen von Codeausschnitten aber nicht link zu einem herunterladbaren Projekt.

## <a name="hello-world-samples"></a>Hello World-Beispiele

Die Beispiele in diesem Abschnitt zeigt die Grundlagen Herstellen einer Verbindung mit einer Azure Redis Cacheinstanz lesen und Schreiben von Daten in den Cache mit einer Vielzahl von Sprachen und Redis Clients.

[Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) -Beispiel veranschaulicht, wie verschiedene Cache mit dem [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET Client Operationen.

Dieses Beispiel zeigt, wie Sie:

-   Verwenden Sie verschiedene Verbindungsoptionen
-   Lesen und Schreiben von Cache über synchrone und asynchrone Operationen Objekte
-   Befehlen Sie Redis MGET-MSET angegebenen Schlüssel zurückzugeben
-   Führen Sie Transaktionsvorgänge Redis
-   Arbeiten Sie mit Listen Redis und sortierte Sätze
-   Speichern Sie .NET Objekte JsonConvert Serialisierungsprogramme
-   Verwenden Sie Redis Tags implementiert
-   Arbeiten mit Redis

Weitere Informationen finden Sie [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) auf Github und Weitere Szenarien finden Sie unter [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) -Komponententests.

[Wie Azure Redis Cache mit Python](cache-python-get-started.md) veranschaulicht Einstieg in Azure Redis Cache mit Python und Client [Redis kopieren](https://github.com/andymccurdy/redis-py) .

[Mit .NET Objekte im Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) wird eine Möglichkeit zum Schreiben und Lesen aus einer Azure Redis Cacheinstanz .NET Objekte serialisieren. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Redis Cache als dezentrales Rückwandplatine für ASP.NET SignalR verwenden

Das [Verwenden Redis Cache als dezentrales Rückwandplatine für ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) demonstriert wie wie SignalR Rückwandplatine Azure Redis Cache verwendet werden kann. Weitere Informationen über Backplane anzeigen Sie [SignalR Scaleout mit Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis)

## <a name="redis-cache-customer-query-sample"></a>Redis Cache Kunden Abfragebeispiel

Dieses Beispiel demonstriert vergleicht Leistung zwischen den Zugriff auf Daten aus einem Cache und Dauerhaftigkeit Speicher Daten zugreifen. Dieses Beispiel enthält zwei Projekte.

-   [Demo Redis Cache Performance Verbesserung durch Zwischenspeichern von Daten](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Seeding der Datenbank und des Cache für die demo](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET Session State und Zwischenspeichern der Ausgabe

[Verwenden Sie Azure Redis Cache zum Speichern von ASP.NET SessionState und OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) demonstriert wie Azure Redis Cache zum Speichern von ASP.NET Session und Ausgabecache SessionState und OutputCache Anbieter für Redis verwenden.

## <a name="manage-azure-redis-cache-with-maml"></a>Verwalten von Azure Redis Cache MAML

Das [Verwalten von Azure Redis Cache mithilfe von Azure Management Bibliotheken](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) demonstriert wie können Azure Management Bibliotheken verwalten - (erstellen / aktualisieren / löschen) den Cache. 

## <a name="custom-monitoring-sample"></a>Benutzerdefiniertes überwachen (Beispiel)

[Zugriff Redis Cache Überwachungsdaten](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) demonstriert wie Überwachungsdaten für Ihren Azure Redis Cache außerhalb der Azure-Portal zugreifen können.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Ein Twitter-Stil Klon geschriebene PHP und Redis

Das [Retwis](https://github.com/SyntaxC4-MSFT/retwis) Beispiel ist die Redis Hello World. Es ist ein minimal Twitter-Stil sozialen Netzwerk Klon mit Redis und [Predis](https://github.com/nrk/predis) -Client mit PHP geschrieben. Der Quellcode soll sehr einfach und gleichzeitig an verschiedene Redis Datenstrukturen.

## <a name="bandwidth-monitor"></a>Bandbreite monitor

Im Beispiel [Bandbreite Monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) können Sie auf dem Client verwendete Bandbreite überwachen. Messen der Bandbreite, führen Sie das Beispiel auf dem Clientcomputer Cache, Aufrufe an den Cache, und Bandbreite Bandbreite Monitor Beispiel gemeldet.
