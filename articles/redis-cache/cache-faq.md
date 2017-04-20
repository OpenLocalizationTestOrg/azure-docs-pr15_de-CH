<properties 
    pageTitle="Azure Redis Cache FAQ | Microsoft Azure" 
    description="Erfahren Sie Antworten auf häufig gestellte Fragen, Muster und Vorgehensweisen für Azure Redis Cache" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Azure Redis Cache FAQ

Lernen Sie Antworten auf häufig gestellte Fragen, Muster und Vorgehensweisen für Azure Redis Cache. 


## <a name="what-if-my-question-isnt-answered-here"></a>Was geschieht, wenn meine Frage hier nicht beantwortet?

Wenn Ihre Frage hier nicht aufgeführt ist, bitte, und wir helfen Ihnen eine Antwort.

-   Sie können eine Frage im [Disqus-Thread](#comments) am Ende dieser FAQ und mit Azure Cache-Team und anderen Mitgliedern zu diesem Artikel.
-   Um ein breiteres Publikum erreichen können Frage [Azure Cache MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) und Azure Cache-Team mit anderen Mitgliedern der Community beteiligen.
-   Wenn Sie Vorschläge machen möchten, können Sie Ihre Anfragen und Ideen [Azure Redis Cache Benutzer Voicemail](https://feedback.azure.com/forums/169382-cache)senden.
-   Sie können auch eine e-Mail an uns [Azure Cache externe](mailto:azurecache@microsoft.com)Feedback senden.

## <a name="azure-redis-cache-basics"></a>Azure Redis Cache-Grundlagen

Häufig gestellte Fragen in diesem Abschnitt behandeln einige Grundlagen Azure Redis Cache.

-    [Was ist Azure Redis Cache?](#what-is-azure-redis-cache)
-    [Wie kann ich mit Azure Redis Cache starten?](#how-can-i-get-started-with-azure-redis-cache)

Die folgenden häufig gestellten Fragen Grundkonzepte und Fragen zur Azure Redis Cache behandelt und in einem anderen Abschnitt häufig gestellte Fragen beantwortet werden.

-   [Welche Redis Cache Angebot und Größe sollte ich verwenden?](#what-redis-cache-offering-and-size-should-i-use)
-   [Welche Redis Cache-Clients können werden verwendet?](#what-redis-cache-clients-can-i-use)
-   [Gibt es ein lokaler Emulator für Azure Redis Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Wie überwache ich den Zustand und die Leistung meines Caches?](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Planen der FAQs

-   [Welche Redis Cache Angebot und Größe sollte ich verwenden?](#what-redis-cache-offering-and-size-should-i-use)
-   [Azure Redis Cache-performance](#azure-redis-cache-performance)
-   [Welche Region soll ich meine Cache finden?](#in-what-region-should-i-locate-my-cache)
-   [Wie arbeite Azure Redis Cache in Rechnung?](#how-am-i-billed-for-azure-redis-cache)
-   [Kann ich mit Azure Government Cloud oder Azure China Cloud Azure Redis Cache verwenden?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Entwicklung häufig gestellte Fragen

-   [Was die Konfigurationsoptionen StackExchange.Redis?](#what-do-the-stackexchangeredis-configuration-options-do)
-   [Welche Redis Cache-Clients können werden verwendet?](#what-redis-cache-clients-can-i-use)
-   [Gibt es ein lokaler Emulator für Azure Redis Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Wie können Redis Befehle ausführen?](#how-can-i-run-redis-commands)
-   [Warum haben nicht Azure Redis Cache ein MSDN Klassenbibliotheksverweis wie Azure Dienste?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Kann ich als PHP-Sitzungscache Azure Redis Cache verwenden?](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>FAQs zum Thema Sicherheit

-   [Wann sollten den nicht-SSL-Anschluss für Redis aktivieren?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Produktion-FAQs

-   [Was sind einige bewährte Produktion?](#what-are-some-production-best-practices)
-   [Was sind einige der Aspekte mit gemeinsamen Redis](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Wie bewerten und Testen Sie die Leistung meines Caches?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Wichtige Angaben zum ThreadPool Wachstum](#important-details-about-threadpool-growth)
-   [Aktivieren Sie Server-GC zu höheren Durchsatz auf dem Client StackExchange.Redis Verwendung](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Überwachung und Problembehandlung häufig gestellte Fragen

Häufig gestellte Fragen in diesem Abschnitt behandelt allgemeine Überwachung und Fehlerbehebung Fragen. Weitere Informationen zur Überwachung und Problembehandlung von Azure Redis Cache Instanzen finden Sie unter [Azure Redis Cache überwachen](cache-how-to-monitor.md) und [Beheben von Azure Redis Cache](cache-how-to-troubleshoot.md).

-   [Wie überwache ich den Zustand und die Leistung meines Caches?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [Meine Cache Diagnose Storage kontoeinstellungen, was?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Warum ist die Diagnose einige neue Caches jedoch nicht aktiviert?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Warum sehe ich timeouts](#why-am-i-seeing-timeouts)
-   [Warum wurde mein Client aus dem Cache getrennt?](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Vorherige Cache mit häufig gestellten Fragen

-   [Welches Angebot Azure Cache ist richtig für mich?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Was ist Azure Redis Cache?

Azure Redis Cache basiert auf gängige Open-Source [Redis Cache](http://redis.io). Sie erhalten Sie Zugriff auf einen sicheren, dedizierten Redis Cache von Microsoft und von einer Anwendung in Azure. Ausführlichere Informationen finden Sie unter [Azure Redis Cache](https://azure.microsoft.com/services/cache/) -Produktseite auf Azure.com.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Wie kann ich mit Azure Redis Cache starten?

Es gibt verschiedene Arten, mit Azure Redis Cache beginnen können.

-    Sie können einen verfügbaren Lernprogramme für [.NET](cache-dotnet-how-to-use-azure-redis-cache.md) [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md)und [Python](cache-python-get-started.md)Auschecken. 
-    Sie sehen [wie hohe Leistung Apps mithilfe von Microsoft Azure Redis Cache anlegen](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
-    Kundendokumentation Clients finden, die die Programmiersprache des Projekts wie Redis entsprechen. Es gibt viele Redis-Clients, die mit Azure Redis Cache verwendet werden können. Eine Liste der Redis-Clients finden Sie unter [http://redis.io/clients](http://redis.io/clients).


Wenn Sie bereits ein Azure-Konto haben, können Sie:

-    [Öffnen Sie ein Azure-Konto kostenlos](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Sie erhalten ein Guthaben, die versuchen, bezahlte Azure Services verwendet werden können. Auch nach die Abspann verbraucht werden, können Sie das Konto und kostenlose Azure Dienste und Features verwenden.
-    [Abonnementvorteile Visual Studio aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Ihr MSDN-Abonnement erhalten Sie Credits monatlich für bezahlte Azure Services verwendet werden können.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Welche Redis Cache Angebot und Größe sollte ich verwenden?
Jede Azure Redis Cache bietet unterschiedliche **Größe**, **Bandbreite**, **hohe Verfügbarkeit**und **SLA** -Optionen.

Im folgenden sind Kriterien für die Auswahl von cacheangebot.

-   **Arbeitsspeicher**: die grundlegende und Standard-Ebenen bieten 250 MB – 53 GB. Die Premium-Stufe bietet bis zu 530 GB mehr [auf Anfrage](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Weitere Informationen finden Sie unter [Azure Redis Cache Preise](https://azure.microsoft.com/pricing/details/cache/).
-   **Netzwerk-Performance**: bei einer Arbeitslast, die hohen Durchsatz erfordert Premium-Ebene bietet mehr Bandbreite im Vergleich zu Standard oder Basic. Innerhalb jeder Kategorie haben größere Größen Caches auch mehr Bandbreite aufgrund der zugrunde liegenden VM, die den Cache hostet. Finden Sie in der [folgenden Tabelle](#cache-performance) Weitere Informationen.
-   **Durchsatz**: die Premium-Stufe bietet den maximalen Durchsatz verfügbaren. Cacheserver oder Client Bandbreitengrenzwerte erreicht, erhalten Sie Timeouts auf der Clientseite. Weitere Informationen finden Sie in der folgenden Tabelle.
-   **Hohe Verfügbarkeit-SLA**: Azure Redis Cache wird sichergestellt, dass ein Standard-Premium-Cache mindestens 99,9 % der Zeit zur Verfügung steht. Erfahren Sie mehr über unsere SLA finden Sie unter [Azure Redis Cache Preise](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). Die SLA umfasst nur Konnektivität mit Cache-Endpunkte. Schutz vor Datenverlust behandelt SLA nicht. Wir empfehlen die Funktion Redis Persistenz in der Premium Resiliency Datenverlust zu.
-   **Datenpersistenz redis**: die Premium-Stufe können Sie weiterhin die Cachedaten in Azure Storage-Konto. In einem Cache Basic-Standard werden die Daten nur im Arbeitsspeicher gespeichert. Bei der zugrunde liegenden Infrastruktur kann Probleme Datenverlust. Wir empfehlen die Funktion Redis Persistenz in der Premium Resiliency Datenverlust zu. Azure Redis Cache angeboten RDB und AOF (demnächst verfügbar) Redis Dauerhaftigkeit. Weitere Informationen finden Sie unter [Persistenz Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md).
-   **Redis Cluster**: speichert mehr als 53 GB erstellen oder Splitter Daten über mehrere Redis Knoten können Redis-clustering das Premium-Ebene verfügbar ist. Jeder Knoten besteht aus einem Primary-Replikat Cache für hohe Verfügbarkeit. Weitere Informationen finden Sie unter [Cluster Premium Azure Redis Cache konfigurieren](cache-how-to-premium-clustering.md).
-   **Verbesserte Sicherheit und Netzwerkisolation**: Azure Virtual Network (VNET) Bereitstellung bietet höhere Sicherheit und Isolation für Ihre Azure Redis Cache sowie Subnetze Zugriffsrichtlinien, und andere Funktionen zu den Zugriff einschränken. Weitere Informationen finden Sie unter [Unterstützung für einen Premium Azure Redis virtuelles Netzwerk konfigurieren](cache-how-to-premium-vnet.md).
-   **Redis konfigurieren**: In der Standard- und Premium-Ebenen konfigurieren Sie Redis für erledigten Anträge.
-   **Maximale Anzahl von Clientverbindungen**: die Premium-Stufe bietet die maximale Anzahl von Clients, die Redis mit einer höheren Anzahl der Verbindungen für größere Größe Caches herstellen können. [Siehe Preisseite Details](https://azure.microsoft.com/pricing/details/cache/).
-   **Dedizierte Core Server Redis**: Premium-Ebene alle Cachegrößen haben eine dedizierte Core Redis. Ebenen der C1 in der Basic-Standard Größe und höher haben eine dedizierte Core Redis Server.
-   **Redis ist Singlethreaded** so mehr als zwei Kerne stellt keinen zusätzlichen Vorteil gegenüber nur zwei Kerne, jedoch größere VM verfügen in der Regel mehr Bandbreite als kleinere. Cacheserver oder Client Bandbreitengrenzwerte erreicht, erhalten Timeouts auf der Clientseite Sie.
-   **Leistung**: Caches in der Premium-Hardware mit schnelleren Prozessoren bereitgestellt und bietet eine bessere Leistung im Vergleich zu Tier Basic oder Standard. Premium-Tier-Caches haben höhere Durchsatzraten und niedriger Latenz.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Azure Redis Cache-performance

Die folgende Tabelle zeigt die maximale Bandbreite Werten beim Testen verschiedener Größe Standard und Premium caches mit `redis-benchmark.exe` aus einer VM Iaas gegen Azure Redis Cache-Endpunkt. Beachten Sie, dass diese Werte nicht garantiert, und es kein DIENSTVERTRAG für diese besteht sollte Zahlen, jedoch typisch. Laden Sie Ihre eigene Anwendung bestimmen die richtige Cachegröße für Ihre Anwendung testen.

In dieser Tabelle können wir folgenden Schlussfolgerungen ziehen.

-   Durchsatz für die Caches, die dieselbe Größe ist höher als die Standard-Stufe in der Premium. Mit 6 GB Cache ist Durchsatz von P1 z. B. 140K RPS im Vergleich zu 49 K C3.
-   Mit Redis-clustering erhöht den Durchsatz linear die Anzahl der Splitter (Knoten) im Cluster. Beispielsweise Erstellen eines Clusters P4 10 Splitter dann der verfügbare Durchsatz ist 250K * 10 = 2,5 Millionen RPS.
-   Durchsatz für Schlüssel größer ist höher als die Standard-Stufe in der Premium.

| Preisstufe             | Größe   | CPU-Kerne | Verfügbare Bandbreite                                    | Schlüsselgröße 1 KB                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Standard-Cache-Größen** |        |           | **Megabits pro Sekunde (Mb/s) / Megabyte pro Sekunde (MB/s)** | **Anfragen pro Sekunde (RPS)**            |
| C0                       | 250 MB | Freigegeben    | 5 / 0,625                                              | 600                                      |
| C1                       | 1 GB   | 1         | 100 / 12,5                                             | 12200                                    |
| C2                       | 2.5 GB | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GB   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GB  | 2         | 500 / 62,5                                             | 61000                                    |
| C5                       | 26 GB  | 4         | 1000 / 125                                             | 115000                                   |
| C6                       | 53 GB  | 8         | 2000 / 250                                             | 150000                                   |
| **Premium-Cachegröße**  |        | **CPU-Kerne pro Splitter**  |                                         | **Anfragen pro Sekunde (RPS) Splitter** |
| P1                       | 6 GB   | 2         | 1000 / 125                                             | 140000                                   |
| P2                       | 13 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GB  | 8         | 4000 / 500                                             | 250000                                   |


Weitere Informationen Redis-Tools wie `redis-benchmark.exe`, finden Sie in der [wie können Sie Redis Befehle ausführen?](#cache-commands) Abschnitt.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>Welche Region soll ich meine Cache finden?

Suchen Sie für optimale Leistung und niedrigste Latenz Azure Redis Cache im selben Bereich wie die Cache-Clientanwendung.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Wie arbeite Azure Redis Cache in Rechnung?

Azure Redis Cache Preise [hier](https://azure.microsoft.com/pricing/details/cache/). Die Preisgestaltung Seite listet Preise als Stundensatz. Caches werden auf Minutenbasis ab berechnet, die der Cache bis zum Zeitpunkt erstellt wird, der ein Cache gelöscht wird. Es gibt keine Option für angehalten oder die Rechnung an einen Cache.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Kann ich mit Azure Government Cloud oder Azure China Cloud Azure Redis Cache verwenden?

Ja, steht Azure Redis Cache in Azure Government Cloud und Azure China Cloud. Beachten Sie, dass die URLs für den Zugriff auf und Verwaltung von Azure Redis Cache unterscheiden sich in Azure Government Cloud Azure China Cloud im Vergleich zu Azure öffentlichen Cloud. Finden Sie weitere Hinweise zum Azure Government Cloud und Azure China Cloud Azure Redis Cache mit [Azure Regierung Datenbanken – Azure Redis Cache](../azure-government/documentation-government-services-database.md#azure-redis-cache) und [Azure China Cloud - Azure Redis Cache](https://www.azure.cn/documentation/services/redis-cache/).

Informationen zur Verwendung von Azure Redis Cache mit PowerShell in Azure Government Cloud und Azure China Cloud [Azure Government Cloud oder Azure China Verbindung](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud)anzeigen


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Was die Konfigurationsoptionen StackExchange.Redis?

StackExchange.Redis hat viele Optionen. Erörtert einige allgemeinen Einstellungen. Weitere Informationen StackExchange.Redis Optionen finden Sie unter [StackExchange.Redis-Konfiguration](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

ConfigurationOptions|Beschreibung|Empfehlung
---|---|---
AbortOnConnectFail|Bei true wird die Verbindung nicht nach einem verbunden wird.|Auf False festgelegt und StackExchange.Redis automatisch wiederherstellen lassen.
ConnectRetry|Die Anzahl der Verbindungsversuche während der anfänglichen wiederholt verbinden.| Finden Sie unter den folgenden Hinweisen. |
ConnectTimeout|Timeout in Millisekunden für Vorgänge anschließen| Finden Sie unter den folgenden Hinweisen. |

In den meisten Fällen reichen die Standardwerte des Clients. Sie können die Optionen für Ihre Arbeitslast optimieren.

-   **Versuche**
    -   ConnectRetry und ConnectTimeout ist der Leitfaden schnell und wiederholen. Dieser Wert basiert auf der Arbeitslast und wie viel Zeit durchschnittlich nimmt der Client einen Redis Befehl und Antwort.
    -   Lassen Sie StackExchange.Redis automatisch statt Verbindungsstatus überprüft und sich erneut verbinden **Verwenden Sie die ConnectionMultiplexer.IsConnected-Eigenschaft**.
    -   Schneeballsystem - manchmal laufen in ein Problem Sie erneut das schneebälle und nicht wiederhergestellt. In diesem Fall sollten Sie sich mit einem exponentielle Backoff Retry-Algorithmus Siehe [Leitfaden wiederholen](best-practices-retry-general.md) von Microsoft Patterns & Practices-Gruppe veröffentlicht.
-   **Werte**
    -   Betrachten Sie Ihre Arbeitslast und die Werte entsprechend. Wenn Sie große Werte speichern, festzulegen Sie den Timeoutwert auf einen höheren Wert.
    -   Legen Sie `AbortOnConnectFail` mit StackExchange.Redis false und lassen Sie wiederherzustellen.
    -   Verwenden Sie eine ConnectionMultiplexer-Instanz für die Anwendung. Einen LazyConnection können Sie eine Connection-Eigenschaft zurückgegebene Instanz wie [mit Cache mithilfe der ConnectionMultiplexer-Klasse](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)erstellen.
    -   Legen Sie die `ConnectionMultiplexer.ClientName` -Eigenschaft app eindeutigen Instanznamen für Diagnosezwecke.
    -   Verwenden Sie mehrere `ConnectionMultiplexer` Instanzen für benutzerdefinierte Arbeitslasten.
    -   Sie können dieses Modell anwenden, haben Sie unterschiedliche Laden in Ihrer Anwendung. Zum Beispiel:
    -   Sie können einen multiplexer für den Umgang mit großen Schlüssel.
    -   Sie können eine für den Umgang mit kleine multiplexer.
    -   Sie können unterschiedliche Werte für Verbindungstimeouts und Wiederholungslogik für jede ConnectionMultiplexer, die Sie verwenden.
    -   Legen Sie die `ClientName` -Eigenschaft für jedes multiplexer Diagnose zu.
    -   Dies führt zu mehr optimierte Wartezeit pro `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Welche Redis Cache-Clients können werden verwendet?

Einer der großen Vorteile von Redis ist, dass viele Clients unterstützen viele verschiedene Sprachen. Eine aktuelle Liste der Clients finden Sie unter [Redis Kunden](http://redis.io/clients). Lernprogramme, die mehrere Sprachen und Clients finden Sie unter [Verwenden von Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md) , und klicken Sie auf die gewünschte Sprache aus Switcher Sprache am Anfang des Artikels.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Gibt es ein lokaler Emulator für Azure Redis Cache?

Es ist keine lokale Emulator für Azure Redis Cache, aber die MSOpenTech Version von Redis server.exe von [Befehlszeilentools Redis](https://github.com/MSOpenTech/redis/releases/) auf dem lokalen Computer ausführen und verbinden zu ähnlich einem lokalen Cache-Emulator wie im folgenden Beispiel gezeigt.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Sie können optional eine [redis.conf](http://redis.io/topics/config) mehr [Cache-Standardeinstellungen](cache-configure.md#default-redis-server-configuration) für Ihren Online-Azure Redis Cache weitestgehend bei Bedarf konfigurieren.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Wie können Redis Befehle ausführen?

Sie können die Befehle am [Redis Befehle](http://redis.io/commands#) mit Ausnahme der aufgelisteten [Befehle nicht in Azure Redis Cache unterstützt Redis](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache)Befehle aufgeführt. Führen Sie Redis Befehle haben Sie mehrere Optionen.

-   Haben Sie einen Standard oder Premium-Cache können Sie Redis Befehle [Redis Konsole](cache-configure.md#redis-console)ausführen. Auf diese Weise eine sichere Redis Befehle in Azure-Portal ausführen.
-   Sie können auch die Befehlszeilentools Redis. Um sie zu verwenden, führen Sie die folgenden Schritte.
-   Die [Befehlszeilentools Redis](https://github.com/MSOpenTech/redis/releases/)herunterladen
-   Verbinden mit Cache `redis-cli.exe`. Übergeben Sie den Cache Endpunkt -h-Switch und den Schlüssel mit-wie im folgenden Beispiel gezeigt.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Beachten Sie, dass die Befehlszeilentools Redis funktionieren nicht mit den SSL-Anschluss können Sie ein Dienstprogramm wie `stunnel` Verbindung sicher Tools mit den SSL-Anschluss gemäß der Anleitung im Blogbeitrag [Ankündigung ASP.NET Sitzungszustandsanbieter für Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Warum haben nicht Azure Redis Cache ein MSDN Klassenbibliotheksverweis wie Azure Dienste?

Microsoft Azure Redis Cache anhand gängiger open Source Redis Cache und eine Vielzahl von [Clients Redis](http://redis.io/clients) für viele Programmiersprachen zugreifen können. Jeder Client verfügt über eine eigene API, die Aufrufe an die Redis Cacheinstanz [Redis Befehle](http://redis.io/commands)verwenden.

Da jeder Client unterscheidet, gibt es nicht einen zentralen Klassenverweis auf MSDN; Stattdessen verwaltet jeder Client eine eigene Dokumentation. Neben der Referenzdokumentation gibt mehrere Lernprogramme zum Einstieg in Azure Redis Cache Sprachen mit cacheclients. Diese Lernprogramme finden Sie unter [Verwenden von Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md) und klicken Sie auf die gewünschte Sprache aus Switcher Sprache am Anfang des Artikels.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Kann ich als PHP-Sitzungscache Azure Redis Cache verwenden?

Ja, um Azure Redis Cache als PHP-Sitzungscache verwenden, werden die Verbindungszeichenfolge für Ihre Azure Redis Cacheinstanz in `session.save_path`.

>[AZURE.IMPORTANT] Bei Azure Redis Cache als PHP-Sitzungscache muss URL codieren Sicherheitsschlüssel zum Verbinden mit dem Cache, wie im folgenden Beispiel gezeigt.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Wenn der Schlüssel nicht URL-codiert ist, erhalten Sie eine ähnlich der folgenden Ausnahme:`Failed to parse session.save_path`

Weitere Informationen über Redis Cache als Cache PHP-Sitzung mit dem Client PhpRedis finden Sie unter [PHP-Sitzung](https://github.com/phpredis/phpredis#php-session-handler).



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Wann sollten den nicht-SSL-Anschluss für Redis aktivieren?

Redis Server unterstützt SSL nicht einsatzbereit, aber Azure Redis Cache verfügt. Anschluss an Azure Redis Cache und der Client unterstützt SSL wie StackExchange.Redis, sollten Sie SSL verwenden.

Beachten Sie, dass nicht-SSL-Anschluss für neue Instanzen von Azure Redis Cache deaktiviert ist. Wenn Ihr Client SSL nicht unterstützt, müssen Sie den nicht-SSL-Anschluss gemäß der Anleitung im Abschnitt [Zugriff auf Ports](cache-configure.md#access-ports) [konfigurieren einen Cache in Azure Redis Cache](cache-configure.md) Artikel aktivieren.

Tools wie redis `redis-cli` funktionieren nicht mit der SSL-Port, jedoch können Sie ein Dienstprogramm wie `stunnel` Verbindung sicher Tools mit den SSL-Anschluss gemäß der Anleitung im Blogbeitrag [Ankündigung ASP.NET Sitzungszustandsanbieter für Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

Informationen zum Herunterladen von Redis-Tools finden Sie in der [wie können Sie Redis Befehle ausführen?](#cache-commands) Abschnitt.



### <a name="what-are-some-production-best-practices"></a>Was sind einige bewährte Produktion?

-   [Bewährte StackExchange.Redis](#stackexchangeredis-best-practices)
-   [Konfiguration und Konzepte](#configuration-and-concepts)
-   [Leistungstests](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Bewährte StackExchange.Redis

-   Legen Sie `AbortConnect` auf False ConnectionMultiplexer automatisch wiederherstellen lassen. [Details finden Sie hier](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   Verwenden der ConnectionMultiplexer - nicht für jede Anforderung eine neue erstellen. Die `Lazy<ConnectionMultiplexer>` Muster [hier](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) wird empfohlen.
-   Redis funktioniert am besten mit kleineren Werten, so sollten Sie größere Daten in mehreren Schlüsseln Zerkleinern. In [dieser Diskussion Redis](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)gilt 100kb "Groß". Lesen Sie [in diesem Artikel](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) ein Beispiel-Problem, die durch große Werte verursacht werden kann.
-   Konfigurieren der [ThreadPool Einstellungen](#important-details-about-threadpool-growth) um Timeouts zu vermeiden.
-   Verwenden Sie mindestens ConnectTimeout Standardwert von 5 Sekunden. StackExchange.Redis genügend Zeit zum Herstellen der Verbindung bei einem Netzwerk Blip erneut würden.
-   Achten Sie auf Leistung Kosten im Zusammenhang mit anderen Operationen ausgeführt werden. Zum Beispiel die `KEYS` ist ein o(n)-Vorgang, und sollte vermieden werden. [Redis.io Site](http://redis.io/commands/) hat Details der Komplexität für jeden Vorgang unterstützt. Klicken Sie auf einen Befehl, um die Komplexität für jede Operation finden Sie unter.

#### <a name="configuration-and-concepts"></a>Konfiguration und Konzepte

-   Verwenden Sie Standard oder Premium-Ebene für Produktionssysteme. Die grundlegende Ebene ist ein einzelner Knotensystem keine Datenreplikation und kein SLA. Auch verwenden Sie einen C1 Cache. C0 Caches sollen wirklich einfache Test-Szenarien.
-   Beachten Sie, dass Redis einen Datenspeicher **Im Arbeitsspeicher** . [Diese](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) Artikel sind Sie über Szenarien, in denen Datenverlust auftreten kann.
-   Entwickeln Sie Ihr System, so dass Verbindung ahnen [patching und Failover](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md)verarbeiten kann.

#### <a name="performance-testing"></a>Leistungstests

-   Starten mit `redis-benchmark.exe` um ein Gefühl für Durchsatz vor dem Schreiben eigener Perf überprüft. Beachten Sie, dass die Redis-Benchmark SSL nicht unterstützt, so dass Sie [nicht-SSL-Port über Azure-Portal aktivieren](cache-configure.md#access-ports) , bevor Sie den Test haben. Beispiele finden Sie in [Wie bewerten und Testen Sie die Leistung meines Caches?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   VM zu Testzwecken verwendet der Client sollte im Bereich der Redis Cacheinstanz.
-   Wir empfehlen Dv2 VM-Serie für Ihre Kunden besseren Hardware und optimale Ergebnisse.
-   Vergewissern Sie sich Ihr Kunde VM Auswahl hat mindestens genauso viel computing und genutzt werden kann als Cache Sie testen. 
-   Aktivieren Sie VRSS auf dem Client, wenn Sie unter Windows sind. [Details finden Sie hier](https://technet.microsoft.com/library/dn383582.aspx).
-   Premium-Tier Redis Instanzen werden besser Latenz und der Durchsatz Netzwerk haben sie Hardwaregeräte für CPU und Netzwerk ausgeführt werden.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Was sind einige der Aspekte mit gemeinsamen Redis

-   Sie sollte nicht lange dauern, ohne den Einfluss dieser Befehle bestimmte Redis Befehle ausgeführt.
    -   Beispielsweise führen Sie nicht den Befehl [Schlüssel](http://redis.io/commands/keys) in der Produktion als dauert lange, je nach Schlüssel zurückgeben konnte. Redis ist eine Singlethread-Server und Befehle einzeln nacheinander verarbeitet. Haben Sie andere Befehle nach Schlüssel, werden sie bis Redis Schlüssel Befehl verarbeitet nicht verarbeitet. [Redis.io Site](http://redis.io/commands/) hat Details der Komplexität für jeden Vorgang unterstützt. Klicken Sie auf einen Befehl, um die Komplexität für jede Operation finden Sie unter.
-   Verwende Schlüsselgrößen - kleine Schlüsselwerte oder große Schlüsselwerte ich? Im Allgemeinen hängt das Szenario. Das Szenario erfordert größere Schlüssel können dann Sie Anpassen der ConnectionTimeout und Werten wiederholen und die Wiederholungslogik. Server im Hinblick auf Redis sind kleinere Werte beobachtet, eine bessere Leistung.
-   Dies bedeutet nicht, dass Sie größere Werte in Redis speichern können; Sie müssen Folgendes beachten. Wartezeiten können höher sein. Haben Sie einen Satz von Daten, die größer und kleiner ist, können Sie mehrere Instanzen von ConnectionMultiplexer, jeder konfiguriert mit anderen Timeout Retry-Werte wie im vorherigen Abschnitt [Was tun die StackExchange.Redis Konfigurationsoptionen](#cache-configuration) beschrieben.



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Wie bewerten und Testen Sie die Leistung meines Caches?

-   [Cache Diagnose aktivieren](cache-how-to-monitor.md#enable-cache-diagnostics) , können Sie [Überwachen](cache-how-to-monitor.md) den Zustand des Cache. Anzeigen der Metriken in Azure-Portal und können Sie [herunterladen und überprüfen](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) die Werkzeuge Ihrer Wahl.
-   Redis-benchmark.exe können Sie laden Test Redis-Server.
-   Sicherstellen Sie, dass die Auslastungstests Client- und Redis Cache im selben Bereich.
-   Redis cli.exe und monitor verwenden den Cache mit dem Befehl INFO.
-   Wenn Ihre hohe Speicherfragmentierung verursachen sollten Sie an eine größere Cachegröße skalieren.
-   Informationen zum Herunterladen von Redis-Tools finden Sie in der [wie können Sie Redis Befehle ausführen?](#cache-commands) Abschnitt.

Folgendes ist ein Beispiel für Redis-benchmark.exe. Führen Sie für genaue Ergebnisse diesen Befehl in einer VM in der gleichen Region wie Ihr Cache.

-   Test Pipelined SET-Anfragen mit 1 k Nutzlast

    Redis-benchmark.exe - h **Yourcache**. redis.cache.windows.net- **YourAccesskey** -t festgelegt - n 1000000 -d 1024 - P 50
    
-   Test Pipelined erhalten fordert mit 1 k Nutzlast. 
    Hinweis: Die Gruppe test ausführen obigen zuerst zum Cache Auffüllen
    
    Redis-benchmark.exe - h **Yourcache**. redis.cache.windows.net- **YourAccesskey** -t GET -n 1000000 - d 1024 - P 50

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Wichtige Angaben zum ThreadPool Wachstum

CLR-ThreadPool verfügt über zwei Arten von Threads - "Arbeitskraft" und "E/a-Completion Port" (auch bekannt als IOCP) Threads. 

-   Worker-Threads für Dinge wie Verarbeitung verwendet `Task.Run(…)` oder `ThreadPool.QueueUserWorkItem(…)` Methoden. Diese Threads werden auch von verschiedenen Komponenten in der CLR verwendet, wenn in einem Hintergrundthread auftreten muss.
-   IOCP-Threads werden verwendet, wenn asynchrone e/a erfolgt (z. B. Lesen aus dem Netzwerk). 

Der Threadpool stellt neue Arbeitsthreads oder e/a-Abschlussthreads bei Bedarf (ohne Einschränkung) bis "Minimum" Einstellung für jeden Thread erreicht. Standardmäßig ist die Anzahl der Prozessoren auf einem System die Mindestanzahl von Threads fest. 

Sobald die Anzahl der vorhandenen (beschäftigt) Treffer threads "minimum" Anzahl der ThreadPool-Threads wird injiziert Gas die Rate, mit der ist neue Threads einen Thread pro 500 Millisekunden. Dadurch wird Ihr System Burst Arbeit benötigen einen Thread IOCP Arbeit sehr schnell verarbeitet wird. Wenn die Arbeit über die konfigurierte Einstellung "Minimum" ist, werden einige Verzögerung verarbeiten die Arbeit während der ThreadPool auf zweierlei geschehen wartet.

1. Vorhandener Thread wird zur Bearbeitung frei.
1. Kein vorhandener Thread wird für 500 ms, ein neuer Thread erstellt wird.

Grundsätzlich Dadurch wird die Anzahl der ausgelasteten Threads größer als Min. Threads Sie wahrscheinlich eine Verzögerung von 500 ms Zahlen vor den Netzwerkverkehr von der Anwendung verarbeitet wird. Außerdem ist es wichtig darauf vorhandenen Threads bleibt für mehr als 15 Sekunden (basierend auf was weiß), werden diese bereinigt und dieses Wachstum und Schwund kann wiederholt.

Betrachten wir ein Beispiel Fehlermeldung von StackExchange.Redis (build 1.0.450 oder höher), Sie sehen, dass jetzt ThreadPool Statistiken ausgegeben (Siehe IOCP und ARBEITSKRAFT unten).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Im obigen Beispiel finden Sie unter IOCP Thread 6 ausgelasteten Threads Gibt und das System wird konfiguriert mindestens 4 Threads. In diesem Fall der Client würde wahrscheinlich habe zwei 500 ms verzögert da 6 > 4.

Beachten Sie, dass StackExchange.Redis Wachstum von IOCP oder WORKER Threads gedrosselt wird Timeouts treffen.

### <a name="recommendation"></a>Empfehlung

Mit diesen Informationen wird dringend empfohlen, Kunden die Mindestkonfiguration für IOCP und den ARBEITSTHREADS etwas größer als der Standardwert festgelegt. Wir können keine Stange Anleitung auf dieser Wert denn der richtige Wert für eine Anwendung zu hoher/niedriger für eine andere Anwendung geben. Diese Einstellung kann auch die Leistung der anderen komplizierten Programme auswirken, damit jeder Kunde diese Einstellung auf ihre speziellen Bedürfnisse anpassen. Ein guter Ausgangspunkt ist 200 oder 300, testen und nach Bedarf anpassen.

So konfigurieren Sie diese Einstellung:

-   In ASP.NET, verwenden Sie die [Einstellung "MinIoThreads"][] unter den `<processModel>` -Konfigurationselements in der Datei web.config. Ausführen in Azure WebSites ist diese Einstellung nicht über Konfigurationsoptionen verfügbar. Allerdings sollte man dies programmgesteuert festlegen (siehe unten) von der Application_Start-Methode in global.asax.cs.

> **Hinweis:** die Angabe dieses Konfigurationselement ist eine Einstellung *pro Kern* . Beispielsweise wenn 4 Core Maschine und Ihre MinIOThreads Einstellung 200 zur Laufzeit verwenden Sie `<processModel minIoThreads="50"/>`.

-   Verwenden Sie außerhalb von ASP.NET, [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) API.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Aktivieren Sie Server-GC zu höheren Durchsatz auf dem Client StackExchange.Redis Verwendung

Aktivieren der Server-GC Client optimieren und bessere Leistung und den Durchsatz StackExchange.Redis nutzen können. Weitere Informationen zu Server-GC und Aktivierung finden Sie in folgenden Artikeln.

-   [Server-GC aktivieren](https://msdn.microsoft.com/library/ms229357.aspx)
-   [Grundlagen der Garbagecollection](https://msdn.microsoft.com/library/ee787088.aspx)
-   [Garbagecollection und Leistung](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Wie überwache ich den Zustand und die Leistung meines Caches?

Microsoft Azure Redis Cache Instanzen können in [Azure-Portal](https://portal.azure.com)überwacht werden. Sie können Daten anzeigen, Metriken Diagramme an das Startmenü anheften Datums- und Zeitbereich der Überwachung Diagramme anpassen, hinzufügen und entfernen Metriken aus den Diagrammen und Alarm festlegen, wenn bestimmte Bedingung zutrifft. Weitere Informationen finden Sie unter [Monitor Azure Redis Cache](cache-how-to-monitor.md).

Abschnitt **Support + Problembehandlung** Blade Redis Cache **Settings** enthält auch mehrere Tools zur Überwachung und Problembehandlung der Caches. 

-   **Problembehandlung** enthält Informationen über häufige Probleme und Lösungsvorschläge.
-   **Überwachungsprotokolle** Informationen über Aktionen auf den Cache. Sie können auch Filtern um dieser Ansicht auf andere Ressourcen zu erweitern.
-   **Zustand der Ressourcen** die Ressource überwacht und informiert Sie, ob sie wie erwartet ausgeführt wird. Weitere Informationen über das Gesundheitswesen Azure Ressource Übersicht [Azure Ressource Gesundheit](../resource-health/resource-health-overview.md).
-   **Neue support-Anfragen** enthält Optionen zum Öffnen einer Anfrage für den Cache.

Diese Tools ermöglichen die Überwachung des Status von Azure Redis Cache Instanzen und Zwischenspeichern Anwendung verwalten. Weitere Informationen finden Sie unter [Support und Problembehandlung Settings](cache-configure.md#support-amp-troubleshooting-settings).

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Meine Cache Diagnose Storage kontoeinstellungen, was?

Caches in derselben Region und Abonnement freigeben eingestellten Storage Diagnostics, und ist die Konfiguration geändert (Diagnose aktivieren bzw. deaktivieren oder ändern das Speicherkonto) gilt für alle Caches das Abonnement in diesem Bereich. Wenn die Diagnose für den Cache geändert haben, überprüfen Sie, ob die Diagnose für einen anderen Cache in dieselbe Abonnement und Region geändert haben. Eine Möglichkeit zum Überprüfen ist an die Audit-Protokolle für den Cache für einen `Write DiagnosticSettings` Ereignis. Weitere Informationen zum Arbeiten mit Überwachungsprotokollen finden Sie unter [Ereignisse anzeigen und Überwachungsprotokolle](../monitoring-and-diagnostics/insights-debugging-with-events.md) und [Operationen mit Ressourcen-Manager überwachen](../resource-group-audit.md). Weitere Informationen zum Überwachen von Azure Redis Cache-Ereignissen finden Sie unter [Vorgänge und Alarme](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Warum ist die Diagnose einige neue Caches jedoch nicht aktiviert?

Caches in derselben Region und Abonnement freigeben eingestellten Storage Diagnostics. Beim Erstellen eines neuen Caches in derselben Region und Abonnement ein anderer Cache, die Diagnose aktiviert den neuen Cache mit den gleichen Einstellvorgaben Diagnose aktiviert.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Warum sehe ich timeouts

Timeouts auftreten im Client mit Redis verwenden. Redis-Server ist in den meisten Fällen nicht. Wenn ein an den Server Redis Befehl des Befehls in die Warteschlange und Redis Server schließlich nimmt der Befehl ausgeführt. Ist der Client können Timeout während dieses Vorgangs und eine Ausnahme wird jedoch auf der aufrufenden Seite ausgelöst. Weitere Informationen zur Problembehandlung bei Timeout finden Sie unter [Client Side Problembehandlung](cache-how-to-troubleshoot.md#client-side-troubleshooting) und [StackExchange.Redis Timeout Ausnahmen](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Warum wurde mein Client aus dem Cache getrennt?

Im folgenden werden einige häufige Ursache für einen Cache trennen.

-   Client Seite-
    -   Die Client-Anwendung bereitgestellt wurde.
    -   Die Clientanwendung einen Skalierung Vorgang ausgeführt.
        -   Cloud-Services oder Web Apps kann dies durch automatische Skalierung sein.
    -   Die Netzwerkebene auf der Clientseite geändert.
    -   Vorübergehende Fehler im Client oder in den Netzknoten zwischen dem Client und dem Server.
    -   Die Bandbreitengrenzwerte erreicht wurden.
    -   CPU-gebundene Vorgänge dauerte zu lange.
-   Server Seite-
    -   Azure Redis Cache-Dienst initiiert auf dem standardmäßigen Cache bietet ein Failover vom primären Knoten auf den zweiten Knoten.
    -   Azure wurde die Instanz patchen, Cache bereitgestellt wurde
        -   Dies ist Redis Serverupdates oder VM Wartung.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Welches Angebot Azure Cache ist richtig für mich?

>[AZURE.IMPORTANT]Nach [Ankündigung](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)des Vorjahres wird am 30. November 2016 Azure Managed Cache Service und Azure-Rolle Cache deaktiviert werden. Unsere Empfehlung ist [Azure Redis](https://azure.microsoft.com/services/cache/)verwendet werden. Informationen über das Migrieren finden Sie unter [Migrieren von Azure Redis Cache Cachedienst verwaltet](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Azure Redis Cache
Azure Redis Cache ist erhältlich in bis zu 53 GB Größe eine Verfügbarkeit von 99,9 % SLA. Neue [Premium-Stufe](cache-premium-tier-intro.md) bietet Größen bis zu 530 GB und Unterstützung für clustering, VNET und Dauerhaftigkeit mit einer SLA von 99,9 %.

Azure Redis Cache ermöglicht Kunden einen sicheren, dedizierten Redis Cache, von Microsoft verwaltet. Mit diesem Angebot können Sie die zahlreichen Features und Ökosystem von Redis, hosten und Überwachung von Microsoft bereitgestellten nutzen.

Im Gegensatz zu herkömmlichen Caches die nur mit Schlüssel-Wert-Paare ist Redis für seine äußerst leistungsfähigen Datentypen. Redis auch unterstützt die Ausführung von atomarer Operations mit diesen Typen wie String Anfügen; der Wert in einen Hashwert erhöht; Legt eine Liste; Computing Schnittmenge Union und Differenz; oder des Members mit höchste in einer sortierten. Unterstützung für Transaktionen, Pub-Sub scripting Schlüssel mit einem Time-to-live, Lua und Konfigurationen zu Redis wie herkömmliche Cache bietet.

Ein weiterer wichtiger Aspekt Redis Erfolg ist fehlerfrei, brillante Opensource Ökosystem gebaut. Dies spiegelt in unterschiedlichsten Redis Clients verfügbar in mehreren Sprachen. Dadurch können nahezu alle Arbeitslasten verwendet werden in Azure erstellen möchten. 

Weitere Informationen zum Einstieg in Azure Redis Cache finden Sie unter [verwenden Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md) und [Azure Redis Cache-Dokumentation](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="managed-cache-service"></a>Verwaltete Cachedienst
[Managed Cache Service 30 November 2016 zurückgezogen werden soll.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>Cache Funktion
[Cache Funktion soll 30 November 2016 zurückgezogen werden.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





[Einstellung "MinIoThreads"]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
