<properties
    pageTitle="ASP.NET Ausgabecacheanbieter Cache"
    description="Erfahren Sie, wie ASP.NET Seitenausgabe mit Azure Redis Cache zwischenspeichern"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>ASP.NET Ausgabecacheanbieter für Azure Redis Cache

Redis Ausgabecacheanbieter ist ein Out-of-Process Speichermechanismus für Cache-Ausgabedaten. Diese Daten werden für die vollständige HTTP-Antworten (Seite Zwischenspeichern der Ausgabe). Der Anbieter wird in die neue Ausgabe Cache Provider Erweiterbarkeitspunkt, der in ASP.NET 4 eingeführt wurde.

Redis Ausgabecacheanbieter, zunächst den Cache konfigurieren, und konfigurieren Sie die ASP.NET Anwendung mit Redis Ausgabe Cache Provider NuGet-Paket. Dieses Thema beschreibt die Anwendung Redis Ausgabecacheanbieter konfigurieren. Weitere Informationen zum Erstellen und Konfigurieren einer Azure Redis Cache-Instanz finden Sie unter [Create Cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>ASP.NET Seitenausgabe im Cache speichern

Um eine Clientanwendung in Visual Studio mit Redis Ausgabe Cache Provider NuGet-Paket konfigurieren, Maustaste auf das Projekt im **Projektmappen-Explorer** und **NuGet-Pakete verwalten**.

![Azure Redis Cache NuGet-Pakete verwalten](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

Geben Sie **RedisOutputCacheProvider** in das Textfeld Suchen, die Ergebnisse wählen Sie aus und auf **Installieren**.

![Azure Redis Cache Ausgabecacheanbieter](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Redis Ausgabe Cache Provider NuGet-Paket hat eine Abhängigkeit zu StackExchange.Redis.StrongName Paket. Wenn das StackExchange.Redis.StrongName-Paket nicht im Projekt vorhanden ist, wird es installiert. Beachten Sie, dass neben den starken Namen StackExchange.Redis.StrongName auch die StackExchange.Redis nicht-starkem Version ist. Das Projekt verwendet die nicht-starkem StackExchange.Redis Version, vor oder nach der Installation von Redis Ausgabe Cache Provider NuGet-Paket deinstallieren müssen, werden andernfalls Sie Konflikte im Projekt benennen erhalten. Weitere Informationen zu diesen Paketen finden Sie unter [Konfigurieren von .NET cacheclients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

NuGet-Paket heruntergeladen und die erforderlichen Assemblyverweise und hinzugefügt Abschnitt in der Datei web.config mit der erforderlichen Konfiguration für Ihre Anwendung ASP.NET Redis Ausgabecacheanbieter verwenden.

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

Kommentierte Abschnitt enthält ein Beispiel der Attribute und Beispiel für jedes Attribut.

Konfigurieren Sie die Attribute mit den Werten aus der Cache-Blade in Microsoft Azure-Portal, und konfigurieren Sie die Werte wie gewünscht. Informationen zum Zugriff auf die Cacheeigenschaften finden Sie unter [Konfigurieren Redis Cache Settings](cache-configure.md#configure-redis-cache-settings).

-   **Host** – Geben Sie den cacheendpunkt.
-   **Port** – die nicht-SSL-Port oder der SSL-port je nach den Ssl verwenden.
-   **AccessKey** -primären oder sekundären Schlüssel für den Cache verwenden.
-   **Ssl** -True Cache-Client-Kommunikation mit Ssl gesichert werden soll; andernfalls False. Achten Sie darauf, den richtigen Port.
    -   Nicht-SSL-Port ist standardmäßig für neue Zwischenspeicher deaktiviert. Geben Sie True für diese Einstellung den SSL-Anschluss verwenden. Weitere Informationen zu nicht-SSL-Anschluss finden Sie im Abschnitt [Zugriff auf Ports](cache-configure.md#access-ports) im Thema [Cache konfigurieren](cache-configure.md) .
-   **DatabaseId** – angegeben, welche Datenbank für Cache-Ausgabedaten. Wenn nicht angegeben, wird der Standardwert 0 verwendet.
-   **ApplicationName** -Schlüssel werden in Redis als <AppName>_<SessionId>_Data. Dadurch können mehrere Anträge für den gleichen gemeinsamen Schlüssels. Dieser Parameter ist optional und ein Standardwert wird verwendet, wenn Sie es nicht angeben.
-   **ConnectionTimeoutInMilliseconds** – diese Einstellung können Sie ConnectTimeout Einstellung im Client StackExchange.Redis überschreiben. Wenn nicht angegeben, wird die Standardeinstellung ConnectTimeout 5000 verwendet. Weitere Informationen finden Sie unter [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **OperationTimeoutInMilliseconds** – diese Einstellung können Sie die Einstellung SyncTimeout im Client StackExchange.Redis überschreiben. Wenn nicht angegeben, wird die Standardeinstellung für die SyncTimeout von 1000 verwendet. Weitere Informationen finden Sie unter [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).

Hinzufügen einer OutputCache-Direktive auf jeder Seite die Ausgabe zwischengespeichert werden soll.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

In diesem Beispiel die zwischengespeicherten Daten 60 Sekunden lang im Cache bleiben und eine andere Version der Seite für jede Parameterkombination zwischengespeichert. Weitere Informationen über die OutputCache-Direktive finden Sie unter [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Nachdem diese Schritte ausgeführt werden, wird die Anwendung konfiguriert Redis Ausgabecacheanbieter.

## <a name="next-steps"></a>Nächste Schritte

[ASP.NET Sitzungszustandsanbieter Azure Redis Cache](cache-aspnet-session-state-provider.md)anzeigen
