<properties
    pageTitle="Cache ASP.NET Session State Provider | Microsoft Azure"
    description="Erfahren Sie, wie ASP.NET Session State mit Azure Redis Cache speichern"
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
    ms.date="09/01/2016"
    ms.author="sdanie" />

# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>ASP.NET Session State Provider für Azure Redis Cache

Azure Redis Cache bietet einen Sitzungszustandsanbieter, mit denen Sie den Sitzungsstatus in einem Cache anstatt im Speicher oder in einer SQL Server-Datenbank speichern. Verwendung zwischenspeichern Sitzungszustandsanbieter zunächst konfigurieren Sie den Cache, und konfigurieren Sie Ihre Anwendung ASP.NET Redis Cache Sitzung Zustand NuGet-Paket-Cache.

Es ist häufig nicht praktikabel, in einer realen Cloud app zu eine Form der Status einer Aufbewahrung, aber einige Ansätze Auswirkungen Performance- und Skalierbarkeitslösungen mehr als andere. Haben Sie speichern, ist die beste Lösung den Anteil des Zustands klein und in Cookies gespeichert. Wenn das nicht möglich ist, ist die nächstbeste Lösung mit ASP.NET Sitzungsstatus bei einem Anbieter verteilten Cache im Arbeitsspeicher. Die schlechteste Lösung Leistung und Erweiterbarkeit Sicht ist einer Datenbank Sitzungszustandsanbieter gesichert. Dieses Thema beschreibt, mit der ASP.NET Sitzungszustandsanbieter Azure Redis Cache. Informationen zu weiteren Sitzung Zustand finden Sie unter [ASP.NET Session State-Optionen](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>ASP.NET Sitzungsstatus im Cache speichern

Um eine Clientanwendung in Visual Studio mit dem Redis Cache Sitzung Zustand NuGet-Paket konfigurieren, Maustaste auf das Projekt im **Projektmappen-Explorer** und **NuGet-Pakete verwalten**.

![Azure Redis Cache NuGet-Pakete verwalten](./media/cache-aspnet-session-state-provider/redis-cache-manage-nuget-menu.png)

Geben Sie **RedisSessionStateProvider** in das Textfeld Suchen, die Ergebnisse wählen Sie aus und auf **Installieren**.

>[AZURE.IMPORTANT] Verwenden Sie die Clusterfunktion von Premium-Tier, verwenden [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 oder höher oder eine Ausnahme ausgelöst. Dies ist eine unterbrechende Änderung. Weitere Informationen finden Sie unter [Version 2.0.0 brechen Details ändern](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

![Azure Redis Cache Sitzungszustand Anbieter](./media/cache-aspnet-session-state-provider/redis-cache-session-state-provider.png)

Redis Sitzung Zustand Anbieter NuGet-Paket hat eine Abhängigkeit zu StackExchange.Redis.StrongName Paket. Wenn das StackExchange.Redis.StrongName-Paket nicht im Projekt vorhanden ist, wird es installiert. Beachten Sie, dass neben den starken Namen StackExchange.Redis.StrongName auch die StackExchange.Redis nicht-starkem Version ist. Das Projekt verwendet die nicht-starkem StackExchange.Redis Version, vor oder nach der Installation von Redis Sitzung Zustand Anbieter NuGet-Paket deinstallieren müssen, werden andernfalls Sie Konflikte im Projekt benennen erhalten. Weitere Informationen zu diesen Paketen finden Sie unter [Konfigurieren von .NET cacheclients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

NuGet-Paket heruntergeladen und fügt die erforderliche Assembly verweist auf und fügt die folgenden Abschnitt in der Datei web.config hinzu, die die erforderliche Konfiguration für die ASP.NET Anwendung Redis Cache-Sitzungszustandsanbieter enthält.

    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
        <!--
        <add name="MySessionStateStore"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
        />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
    </sessionState>

Kommentierte Abschnitt enthält ein Beispiel der Attribute und Beispiel für jedes Attribut.

Konfigurieren Sie die Attribute mit den Werten aus der Cache-Blade in Microsoft Azure-Portal, und konfigurieren Sie die Werte wie gewünscht. Informationen zum Zugriff auf die Cacheeigenschaften finden Sie unter [Konfigurieren Redis Cache Settings](cache-configure.md#configure-redis-cache-settings).

-   **Host** – Geben Sie den cacheendpunkt.
-   **Port** – die nicht-SSL-Port oder der SSL-port je nach den Ssl verwenden.
-   **AccessKey** -primären oder sekundären Schlüssel für den Cache verwenden.
-   **Ssl** -True Cache-Client-Kommunikation mit Ssl gesichert werden soll; andernfalls False. Achten Sie darauf, den richtigen Port.
    -   Nicht-SSL-Port ist standardmäßig für neue Zwischenspeicher deaktiviert. Geben Sie True für diese Einstellung den SSL-Anschluss verwenden. Weitere Informationen zu nicht-SSL-Anschluss finden Sie im Abschnitt [Zugriff auf Ports](cache-configure.md#access-ports) im Thema [Cache konfigurieren](cache-configure.md) .
-   **ThrowOnError** -True, wenn eine Ausnahme bei Ausfällen oder False ggf. Augabe Vorgang soll. Überprüfen der statischen Microsoft.Web.Redis.RedisSessionStateProvider.LastException-Eigenschaft können Sie für einen Fehler überprüfen. Der Standardwert ist true.
-   **RetryTimeoutInMilliseconds** -Vorgänge fehlschlagen werden wiederholt während dieses Intervalls in Millisekunden angegeben. Die erste Wiederholung tritt nach 20 Millisekunden und dann Versuche pro Sekunde, bis das RetryTimeoutInMilliseconds Intervall auftreten. Unmittelbar nach dieser Zeitspanne wird der Vorgang ein letztes Mal wiederholt. Wenn der Vorgang weiterhin fehlschlägt, wird die Ausnahme zurück an den Aufrufer je nach ThrowOnError ausgelöst. Der Standardwert ist 0, d. keine Versuche h..
-   **DatabaseId** – gibt an, welche Datenbank für Cache, Daten ausgeben. Wenn nicht angegeben, wird der Standardwert 0 verwendet.
-   **ApplicationName** -Schlüssel werden in Redis als `{<Application Name>_<Session ID>}_Data`. Dadurch können mehrere Anträge für den gleichen gemeinsamen Schlüssels. Dieser Parameter ist optional und ein Standardwert wird verwendet, wenn Sie es nicht angeben.
-   **ConnectionTimeoutInMilliseconds** – diese Einstellung können Sie ConnectTimeout Einstellung im Client StackExchange.Redis überschreiben. Wenn nicht angegeben, wird die Standardeinstellung ConnectTimeout 5000 verwendet. Weitere Informationen finden Sie unter [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).
-   **OperationTimeoutInMilliseconds** – diese Einstellung können Sie die Einstellung SyncTimeout im Client StackExchange.Redis überschreiben. Wenn nicht angegeben, wird die Standardeinstellung für die SyncTimeout von 1000 verwendet. Weitere Informationen finden Sie unter [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).

Weitere Informationen zu diesen Eigenschaften finden Sie unter Post-Ankündigung für Blog zur [Ankündigung von ASP.NET Sitzungszustandsanbieter für Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Vergessen Sie nicht, die InProc Sitzung Zustand Anbieter Standardabschnitt in web.config auskommentieren.

    <!-- <sessionState mode="InProc"
         customProvider="DefaultSessionProvider">
         <providers>
            <add name="DefaultSessionProvider"
                  type="System.Web.Providers.DefaultSessionStateProvider,
                        System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                        PublicKeyToken=31bf3856ad364e35"
                  connectionStringName="DefaultConnection" />
          </providers>
    </sessionState> -->

Nachdem diese Schritte ausgeführt werden, wird die Anwendung konfiguriert Redis Cache-Sitzungszustandsanbieter. Bei Verwendung des Sitzungszustands in der Anwendung werden in einer Azure Redis Cache-Instanz gespeichert.

>[AZURE.NOTE] Beachten Sie, dass Daten im Cache müssen serialisierbar sein, im Gegensatz zu Daten, die in der standardmäßigen gespeichert werden im Arbeitsspeicher ASP.NET Session State Anbieter. Bei der Sitzungszustandsanbieter Redis Achten Sie die Datentypen, die im Sitzungsstatus gespeichert werden serialisierbar sind.

## <a name="aspnet-session-state-options"></a>ASP.NET Session State-Optionen

- Dieser Anbieter speichert Speicher Sitzungszustand Provider - Sitzungsstatus im Speicher. Der Vorteil dieser Anbieter ist einfach und schnell. Jedoch können Sie Web-Apps nicht skalieren, wenn Sie Speicher-Provider verwenden, da nicht verteilt.

- SQL Server-Sitzungsstatus Anbieter - dieser Anbieter speichert den Sitzungszustand in Sql Server. Wenn den Sitzungszustand in einem permanenten Speicher beibehalten werden sollen, sollten Sie diesen Anbieter verwenden. Ihrer Anwendung Skalieren mithilfe von Sql Server für Sitzung hat jedoch Leistungseinbußen auf Ihrer Anwendung.

- Verteilte im Speicher Sitzungszustandsanbieter wie Redis Sitzungszustandsanbieter von Cache - dieser Anbieter bietet das beste aus beiden Welten. Ihrer Anwendung haben eine einfache, schnelle und skalierbare Sitzungszustandsanbieter. Jedoch müssen Sie zu berücksichtigen, dass dieser Anbieter den Sitzungszustand in einem Cache speichert und Ihre app alle Eigenschaften verknüpft ist, wenn auf einen Cache im Speicher verteilt, wie vorübergehende Netzwerkausfälle achten muss. Best Practices zur Verwendung von Cache [Zwischenspeichern Leitfäden](../best-practices-caching.md) von Microsoft Patterns & Practices [Azure Cloud und Implementierungsleitfaden](https://github.com/mspnp/azure-guidance)anzeigen

Weitere Informationen zum Sitzungsstatus und andere empfohlene Vorgehensweisen finden Sie unter [Web Development Best Practices (Gebäude realen Cloud Apps mit Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Nächste Schritte

[ASP.NET Ausgabecacheanbieter für Azure Redis Cache](cache-aspnet-output-cache-provider.md)anzeigen
