<properties 
    pageTitle="Behandlung von Azure Redis Cache | Microsoft Azure" 
    description="Informationen Sie zum Beheben allgemeiner Probleme bei Azure Redis Cache." 
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
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Behandlung von Azure Redis Cache

Dieser Artikel enthält Hinweise zur Problembehandlung folgende Azure Redis Cache-Probleme.

-   [Client Side Problembehandlung](#client-side-troubleshooting) - dieser Abschnitt enthält Richtlinien zur Identifizierung und Lösung von Problemen aufgrund von Azure Redis Cache mit Anwendung.
-   [Server Seite troubleshooting](#server-side-troubleshooting) - dieser Abschnitt enthält Richtlinien zum Ermitteln und beheben Probleme auf der Serverseite Azure Redis Cache.
-   [StackExchange.Redis Timeout Ausnahmen](#stackexchangeredis-timeout-exceptions) - dieser Abschnitt enthält Informationen zur Problembehandlung bei Verwendung des StackExchange.Redis-Clients.


>[AZURE.NOTE] Einige Schritte zur Problembehandlung in diesem Handbuch enthält eine Anleitung zur Redis Befehle ausführen und unterschiedliche Performance-Werte überwachen. Weitere Informationen und Hinweise finden Sie im Artikel im Abschnitt [Weitere Informationen](#additional-information) .

## <a name="client-side-troubleshooting"></a>Client Side Problembehandlung


Dieser Abschnitt behandelt Probleme, die auftreten, wenn eine Bedingung für die Clientanwendung.

-   [Speicherdruck auf dem client](#memory-pressure-on-the-client)
-   [Burst Datenverkehr](#burst-of-traffic)
-   [Client hohe CPU-Auslastung](#high-client-cpu-usage)
-   [Client Side Bandbreite überschritten](#client-side-bandwidth-exceeded)
-   [Große Anforderung/Antwort-Größe](#large-requestresponse-size)
-   [Wo befinden sich meine Daten in Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Speicherdruck auf dem client

#### <a name="problem"></a>Problem

Speicherdruck auf dem Clientcomputer führt alle Arten von Leistungsproblemen, die Verarbeitung der Daten verzögert werden können, die von der Redis-Instanz ohne Verzögerung gesendet wurde. Trifft Speicherdruck muss das System normalerweise Daten aus dem physikalischen Speicher in den virtuellen Speicher auf der Festplatte ist. Diese *fehlerhafte Seite* führt das System erheblich verlangsamt.

#### <a name="measurement"></a>Messung 

1.  Überwachen Sie Speicherverwendung auf Computer, um sicherzustellen, dass der Arbeitsspeicher nicht überschritten wird. 
2.  Überwachen der `Page Faults/Sec` -Leistungsindikator. Die meisten Systeme haben einige Seitenfehler sogar während des normalen Betriebs, so sehen Spitzen in diesen Leistungsindikator Seitenfehler die Timeouts entsprechen.

#### <a name="resolution"></a>Auflösung

Aktualisieren Sie den Client an einen größeren Client virtueller Speicher mit mehr Speicher oder die Verwendungsmuster Arbeitsspeicher Speicher Verbrauch zu vertiefen.


### <a name="burst-of-traffic"></a>Burst Datenverkehr

#### <a name="problem"></a>Problem

Verkehrsspitzen zu schaffen kombiniert mit `ThreadPool` Settings kann verzögert sich die Daten bereits durch den Redis-Server gesendet, aber noch nicht auf dem Client verwendet.

#### <a name="measurement"></a>Messung 

Monitor wie Ihre `ThreadPool` Statistiken ändern Code [wie folgt](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Sie können auch hier die `TimeoutException` Nachricht von StackExchange.Redis. Hier ist ein Beispiel:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

In der obigen Nachricht werden mehrere Probleme sind:

 1. Beachten Sie, dass in der `IOCP` Abschnitt und `WORKER` Abschnitt Sie haben ein `Busy` Wert größer als die `Min` Wert. Dies bedeutet, dass die `ThreadPool` Einstellung benötigen anpassen.
 2. Außerdem sehen Sie `in: 64221`. Dies bedeutet, dass 64211 Bytes auf Socketebene Kernel empfangen wurden, aber nicht noch von der Anwendung (z.B. StackExchange.Redis) gelesen. Dies bedeutet normalerweise, dass die Anwendung Daten aus dem Netzwerk ist nicht so schnell liest wie der Server es Ihnen sendet.

#### <a name="resolution"></a>Auflösung

Konfigurieren der [ThreadPool Einstellungen](https://gist.github.com/JonCole/e65411214030f0d823cb) sicherstellen, dass der Threadpool Burst Szenarien schnell vergrößern wird.


### <a name="high-client-cpu-usage"></a>Client hohe CPU-Auslastung

#### <a name="problem"></a>Problem

Hoher CPU-Auslastung auf dem Client ist ein Hinweis, das System mit dem nicht Schritt, die gebeten halten, durchführen. Dies bedeutet, dass der Client nicht rechtzeitig eine Antwort von Redis verarbeiten, obwohl Redis sehr schnell Antwort gesendet.

#### <a name="measurement"></a>Messung

Überwachen Sie System große CPU-Auslastung über Azure-Portal oder über den zugeordneten Leistungsindikator. Achten Sie darauf, kein *Prozess* CPU zu überwachen, da ein einzelner Prozess gleichzeitig geringen CPU-Nutzung haben, das Gesamtsystem CPU hoch sein kann. Achten Sie auf Spitzen CPU-Auslastung, die Timeouts entsprechen. Aufgrund hoher CPU-Auslastung möglicherweise finden Sie auch hohe `in: XXX` Werte in `TimeoutException` Fehlermeldungen wie in Abschnitt [Burst Datenverkehr](#burst-of-traffic) beschrieben.

>[AZURE.NOTE] StackExchange.Redis 1.1.603 und später die `local-cpu` in metrische `TimeoutException` Fehlermeldungen. Gewährleisten Sie die neueste Version des [StackExchange.Redis NuGet-Paket](https://www.nuget.org/packages/StackExchange.Redis/). Es gibt ständig im Code robuster zu machen Timeouts so wichtig die neueste Version ist behobenen Fehler.

#### <a name="resolution"></a>Auflösung

VM vergrößern mit mehr CPU-Kapazität aktualisieren Sie oder Überprüfen Sie, was CPU-Auslastung verursacht. 



### <a name="client-side-bandwidth-exceeded"></a>Client Side Bandbreite überschritten

#### <a name="problem"></a>Problem

Andere Größe verfügen Grenzen wie viel Bandbreite im Netzwerk aufweisen. Wenn der Client die verfügbare Bandbreite überschreitet, werden dann Daten nicht auf dem Client verarbeitet werden schnell Server gesendet wird. Dies führt zu Timeouts.

#### <a name="measurement"></a>Messung

Überwachen Sie, wie die Bandbreite ändern Code [wie folgt](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Beachten Sie, dass dieser Code nicht erfolgreich in einigen Umgebungen mit eingeschränkten Berechtigungen (z. B. Azure Websites) kann.

#### <a name="resolution"></a>Auflösung 

Vergrößern Sie Client-VM oder verringern Sie der Netzwerkbandbreite.


### <a name="large-requestresponse-size"></a>Große Anforderung/Antwort-Größe

#### <a name="problem"></a>Problem

Eine große Anforderungsantwort kann Timeouts. Beispielsweise Angenommen Sie, die auf dem Client konfigurierten Timeoutwert ist 1 Sekunde. Die Anwendung fordert zwei Schlüssel ('A' und 'B') gleichzeitig (mit demselben physischen Netzwerkverbindung). Die meisten Clients unterstützen "Pipelining" Requests, daß beide 'A' und 'B' bei der Übertragung an den Server eine nach der anderen Anfragen werden ohne Antworten. Der Server sendet die Antworten in der Reihenfolge. Wenn 'A' große reagiert Speisen es fast Zeitlimit für nachfolgende Anforderungen. 

Im folgende Beispiel wird dieses Szenario veranschaulicht. In diesem Szenario 'A' und 'B' sind Anfrage schnell Serverstart Antworten 'A' und 'B' Schnelles Senden jedoch aufgrund Daten übertragen 'B' stecken hinter anderen Anforderung und Timeout, obwohl der Server schnell reagiert.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Messung

Dies ist ein schwer zu messen. Im Grunde müssen Instrumentieren Clientcode um umfangreiche Anfragen und Antworten verfolgen. 

#### <a name="resolution"></a>Auflösung

1.  Redis ist für zahlreiche kleine Werte anstatt wenige große Werte optimiert. Die bevorzugte Lösung ist Ihre Daten in kleinere Bezugswerte aufteilen. Sehen die [die ideale Größe für Wertebereich Redis? Ist 100KB groß?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Buchen Sie für Details Warum kleinere Werte empfohlen werden.
2.  Vergrößern Sie die VM (Client und Cacheserver Redis) höhere Bandbreite Funktionen Reduzierung Data Transfer für größere Antworten zu. Beachten Sie, dass immer mehr Bandbreite nur den Server oder auf der Client möglicherweise nicht ausreicht. Messen Sie der Bandbreite und die Funktionen der Größe der VM derzeit vergleichen.
3.  Die Zahl der `ConnectionMultiplexer` Objekte, die Sie verwenden und Roundrobin-Anfragen über verschiedene Anschlüsse.


### <a name="what-happened-to-my-data-in-redis"></a>Wo befinden sich meine Daten in Redis?

#### <a name="problem"></a>Problem

Für bestimmte Daten in meiner Azure Redis Cacheinstanz erwartet, aber es schien es.

##### <a name="resolution"></a>Auflösung

Siehe [Daten in Redis passierte?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) für mögliche Ursachen und Lösungsmöglichkeiten.


## <a name="server-side-troubleshooting"></a>Problembehandlung bei Server-Seite

Dieser Abschnitt behandelt Probleme, die auftreten, wenn eine Bedingung für die Cache-Server.

-   [Speicherdruck auf dem server](#memory-pressure-on-the-server)
-   [Hohe CPU-Auslastung / Server laden](#high-cpu-usage-server-load)
-   [Seite Serverbandbreite überschritten](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Speicherdruck auf dem server

#### <a name="problem"></a>Problem

Speicherdruck auf dem Server führt alle Arten von Leistungsproblemen, die Verarbeitung von Anforderungen verzögern können. Trifft Speicherdruck muss das System normalerweise Daten aus dem physikalischen Speicher in den virtuellen Speicher auf der Festplatte ist. Diese *fehlerhafte Seite* führt das System erheblich verlangsamt. Es gibt mehrere mögliche Ursachen für diese Speicherdruck: 

1.  Den Cache vollständig ausgelastet haben mit Daten gefüllt werden. 
2.  Redis sieht hohe Speicherfragmentierung - häufig verursacht durch Speichern von großen Objekten (Redis für kleine Objekte optimiert - finden Sie unter der [die ideale Größe für Wertebereich Redis? Ist 100KB groß?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Post-Informationen). 

#### <a name="measurement"></a>Messung

Redis stellt zwei Kriterien, mit denen Sie dieses Problem identifizieren können. Die erste `used_memory` und `used_memory_rss`. [Diese Metriken](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) sind im Azure-Portal oder über den Befehl [INFO Redis](http://redis.io/commands/info) verfügbar.

#### <a name="resolution"></a>Auflösung

Es gibt mehrere mögliche Änderungen können dazu Speicherverwendung fehlerfrei:

1. [Konfigurieren einer Richtlinie Speicher](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) und Ablaufzeiten für die Schlüssel. Beachten Sie, dass dies möglicherweise nicht ausreichend, wenn Fragmentierung aufweisen.
2. [Konfigurieren einer Maxmemory reservierten Wert](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) , die ausreicht, um die Fragmentierung des Speichers auszugleichen.
3. Unterteilen Sie Ihre großen zwischengespeicherte Objekte in kleinere verwandte Objekte.
4. [Skalierung](cache-how-to-scale.md) auf eine größere Cachegröße.
5. Verwenden Sie einen [Premium-Cache mit Redis aktiviert](cache-how-to-premium-clustering.md) können Sie [erhöhen die Anzahl der Splitter](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Hohe CPU-Auslastung / Server laden

#### <a name="problem"></a>Problem

Hoher CPU-Auslastung kann bedeuten, dass die Client-Seite nicht rechtzeitig eine Antwort von Redis verarbeiten, obwohl Redis sehr schnell Antwort gesendet.

#### <a name="measurement"></a>Messung

Überwachen Sie System große CPU-Auslastung über Azure-Portal oder über den zugeordneten Leistungsindikator. Achten Sie darauf, kein *Prozess* CPU zu überwachen, da ein einzelner Prozess gleichzeitig geringen CPU-Nutzung haben, das Gesamtsystem CPU hoch sein kann. Achten Sie auf Spitzen CPU-Auslastung, die Timeouts entsprechen.

#### <a name="resolution"></a>Auflösung

[Skalierung](cache-how-to-scale.md) ein größerer Cache tier mit mehr CPU-Kapazität oder was CPU-Spitzen untersuchen. 

### <a name="server-side-bandwidth-exceeded"></a>Seite Serverbandbreite überschritten

#### <a name="problem"></a>Problem

Verschiedener Größe Cacheinstanzen haben Grenzen wie viel Bandbreite im Netzwerk aufweisen. Überschreitet der Server die verfügbare Bandbreite, werden Daten nicht als schnell an den Client gesendet. Dies führt zu Timeouts.

#### <a name="measurement"></a>Messung

Überwachen Sie die `Cache Read` Metrik die Menge der Daten ist aus dem Cache gelesen in Megabyte pro Sekunde (MB/s) während der angegebenen Berichtsintervall. Dieser Wert entspricht der Netzwerkbandbreite durch Cache. Wenn Sie Benachrichtigungen für Server Seite Netzwerk Bandbreitengrenzwerte festlegen möchten, können sie mit diesem `Cache Read` Zähler. Vergleichen Sie die Werte mit den Werten in [dieser Tabelle](cache-faq.md#cache-performance) für die überwachte Bandbreitengrenzwerte für verschiedene Ebenen und Größen Preise Cache.

#### <a name="resolution"></a>Auflösung

Sollten Sie ständig in der Nähe der beobachteten maximale Bandbreite für Ihre Preiskalkulation Tier und Cache sind eine Preiskalkulation Tier oder Größe mehr Netzwerkbandbreite unter Verwendung der Werte in [dieser Tabelle](cache-faq.md#cache-performance) als Leitfaden [Skalieren](cache-how-to-scale.md) .


## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis Timeout Ausnahmen

StackExchange.Redis verwendet eine Konfiguration festlegen benannten `synctimeout` für synchrone Vorgänge hat einen Standardwert von 1000 ms. Wenn ein synchroner Aufruf in der vorgesehenen Zeit abgeschlossen ist, löst StackExchange.Redis Client einen Timeoutfehler ähnlich dem folgenden Beispiel.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Diese Fehlermeldung enthält Metriken, die helfen, die Ursache und mögliche Lösung des Problems zeigen. Die folgende Tabelle enthält Details zum Fehler Nachrichtenmetrik.

| Fehler Meldung Metrik | Details                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Inst       | In der letzten Zeit Segment: 0 Befehle ausgegeben wurden                                                                                                                                                                                              |
| Mgr        | Socket-Manager ausführen `socket.select` d. h. fragt das Betriebssystem an einen Socket, der etwas tun; Grundsätzlich: der Reader ist nicht aktiv Lesen aus dem Netzwerk da etwas denkt tun |
| Warteschlange      | 73 Vorgänge insgesamt angefangene                                                                                                                                                                                                        |
| qu         | 6 angefangene Vorgänge in der Warteschlange nicht gesendet sind und noch nicht auf ausgehenden Netzwerk geschrieben wurde                                                                                                                                                           |
| QS         | 67 He angefangene Vorgänge an den Server gesendet wurde, aber eine Antwort noch nicht verfügbar. Die Antwort könnte `Not yet sent by the server` oder`sent by the server but not yet processed by the client.`                                                   |
| QC         | 0 in Bearbeitung befindlichen Vorgänge Antworten habe aber noch nicht als vollständig durch das Warten auf den Abschluss markiert wurde                                                                                                                                      |
| WR         | Es ist ein Schriftsteller (d.h. 6 nicht gesendeten Anfragen nicht ignoriert werden) Bytes/activewriters                                                                                                                                                   |
| in         | Gibt es keine aktiven und NULL Bytes stehen auf NIC Bytes/activereaders                                                                                                                                               |


### <a name="steps-to-investigate"></a>Schritte zum Überprüfen

1. Als bewährte Methode sicherstellen verwenden Sie das folgende Muster Verbindung bei Verwendung des StackExchange.Redis-Clients.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    Weitere Informationen finden Sie unter [Verbinden mit StackExchange.Redis im Cache](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

2. Sicherstellen Sie, dass Ihre Azure Redis Cache und die Clientanwendung in derselben Region in Azure. Angenommen, Sie können immer Timeouts der Cache im südostasiatischen US jedoch der Client ist im Westen der USA und den Vorgang nicht innerhalb der `synctimeout` Intervall oder möglicherweise immer Timeouts beim Debuggen von Ihrem lokalen Entwicklungscomputer. 

    Es wird dringend empfohlen, der Cache und der Client im selben Azure Bereich. Wenn Sie ein Szenario, die Cross Region Aufrufe enthält haben, legen Sie die `synctimeout` Intervall auf einen Wert größer als 1000 ms Standardintervall mit einem `synctimeout` -Eigenschaft in der Verbindungszeichenfolge. Das folgende Beispiel zeigt einen Ausschnitt StackExchange.Redis Cache Connection String mit einem `synctimeout` 2000 ms.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Gewährleisten Sie die neueste Version des [StackExchange.Redis NuGet-Paket](https://www.nuget.org/packages/StackExchange.Redis/). Es gibt ständig im Code robuster zu machen Timeouts so wichtig die neueste Version ist behobenen Fehler.

4. Sind Bandbreite auf dem Server oder Client unterliegen immer Anfragen, dauert es länger dauern und bewirken Zeitlimits. Ist der Timeout aufgrund Bandbreite auf dem Server finden Sie unter [Seite Serverbandbreite überschritten](#server-side-bandwidth-exceeded). Ist der Timeout aufgrund Netzwerkbandbreite Client finden Sie unter [Client Side Bandbreite überschritten](#client-side-bandwidth-exceeded).

6. Sind Sie CPU auf dem Server oder auf dem Client gebunden?
    -   Überprüfen Sie, ob Sie CPU auf Ihrem Client führen konnte die Anforderung nicht unterliegen immer in der `synctimeout` Intervall, wodurch einen Timeout. Vergrößern Client verschieben oder Verteilen der Last können dies steuern. 
    -   Überprüfen Sie, ob Sie CPU erhalten gebunden auf dem Server durch die Überwachung der `CPU` [Leistungsmetrik Cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Anfragen Redis CPU-gebunden ist, kann diese Anfragen Timeout. Dabei können Sie verteilen die Last auf mehrere Splitter in einem Cache Premium oder zu einem größeren Tarif aktualisieren. Weitere Informationen finden Sie unter [Server Seite Bandbreite überschritten](#server-side-bandwidth-exceeded).

7. Gibt es auf dem Server lange Befehle? Befehle, die auf die Redis-Server lange sind mit langer kann Timeouts. Beispiele für Befehle langer `mget` mit einer großen Anzahl von Schlüsseln, `keys *` oder schlecht Lua-Skripts. Ihrer Azure Redis Cache-Instanz mit der Redis-Cli-Client herstellen oder die [Konsole Redis](cache-configure.md#redis-console) , und führen Sie den Befehl [SlowLog](http://redis.io/commands/slowlog) , um festzustellen, ob dauert länger Anfragen als erwartet. Redis-Server und StackExchange.Redis sind für viele kleine Anfragen als weniger umfangreiche Anfragen optimiert. Ihre Daten in kleinere Einheiten aufteilen kann hier verbessern. 

    Informationen zum Verbinden mit Redis Cli mit Stunnel Azure Redis Cache SSL-Endpunkt finden Sie unter Blogbeitrag [Ankündigung ASP.NET Sitzungszustandsanbieter für Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) . Weitere Informationen finden Sie unter [SlowLog](http://redis.io/commands/slowlog).

8. Hohe Redis Serverlast kann Timeouts. Die Serverlast überwachen, Überwachung der `Redis Server Load` [Leistungsmetrik Cache](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Serverlast 100 (Höchstwert) bedeutet, dass Redis-Server wurde auf Verarbeitung mit keine Leerlaufzeit beschäftigt. Um festzustellen, ob bestimmte Anfragen aller Serverfunktion beansprucht werden, führen Sie den Befehl SlowLog, wie im vorherigen Absatz beschrieben. Weitere Informationen finden Sie unter [hohe CPU-Auslastung / Server laden](#high-cpu-usage-server-load).

9. Wurde ein anderes Ereignis auf der Clientseite, die eine Netzwerk Blip verursacht haben? Auf dem Client (Web, Worker-Rolle oder eine VM Iaas) überprüfen, ob ein Ereignis wie Skalierung nach Anzahl der Client-Instanzen oder Bereitstellen einer neuen Version des Clients oder automatische Skalierung aktiviert ist? Unsere Tests haben wir festgestellt, dass dazu skalieren oder Skalierung nach oben/unten kann ausgehende Netzwerkkonnektivität für einige Sekunden verloren. StackExchange.Redis ist für solche Ereignisse und Verbindung erneut. Während dieser Zeit erneute Verbindung können Anfragen in der Warteschlange.

10. War eine große Anfrage mehrere kleine Anfragen zu Redis Cache, der Timeout vor? Der Parameter `qs` Fehler wird gemeldet, wie viele Anfragen vom Client an den Server gesendet wurden, aber noch keine Antwort verarbeitet. Dieser Wert kann wachsen, da StackExchange.Redis eine einzelne TCP-Verbindung verwendet und kann jeweils nur eine Antwort lesen. Obwohl der erste Vorgang hat das Zeitlimit überschritten, endet nicht nach dem Server gesendeten Daten und anderen Anfragen werden blockiert, bis Abschluss dieses Timeouts verursacht. Eine Lösung ist möglichst Timeouts sicherstellen, dass der Cache für Ihre Arbeitsbelastung ausreicht und große Werte in kleinere Einheiten aufteilen. Eine weitere mögliche Lösung ist die Verwendung ein Pools von `ConnectionMultiplexer` in Ihrem Client Objekte und wählen Sie zuletzt geladenen `ConnectionMultiplexer` Wenn eine neue Anforderung senden. Soll verhindern, dass einen Timeout andere Anforderungen auch Timeout verursacht.

11. Verwenden Sie `RedisSessionStateprovider`, gewährleisten Sie Retry Timeout richtig festgelegt haben. `retrytimeoutInMilliseconds`höher als `operationTimeoutinMilliseonds`, ansonsten erfolgt keine Versuche. Im folgenden Beispiel `retrytimeoutInMilliseconds` 3000 festgelegt ist. Weitere Informationen finden Sie unter [ASP.NET Sitzungszustandsanbieter Azure Redis Cache](cache-aspnet-session-state-provider.md) und [zum Konfigurationsparameter Sitzungszustandsanbieter und Ausgabecacheanbieter verwenden](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Speicherverwendung auf dem Server Azure Redis Cache [Überwachen](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) überprüfen `Used Memory RSS` und `Used Memory`. Ist ein Entfernungsrichtlinie an, Redis startet diesen Schlüssel bei `Used_Memory` die Cachegröße erreicht. Im Idealfall `Used Memory RSS` sollten nur geringfügig höher sein als `Used memory`. Ein großer Unterschied bedeutet Speicherfragmentierung (intern oder extern. Wenn `Used Memory RSS` ist kleiner als `Used Memory`, ist Teil des Caches wurde vom Betriebssystem ausgewechselt. In diesem Fall können Sie einige erheblichen Wartezeiten erwarten. Da Redis nicht steuern, wie die Umlagen hohe Speicherseiten zugeordnet werden `Used Memory RSS` ist häufig das Ergebnis einer Spitze im Speicher. Wenn Redis Speicher freigibt, der Zuweisung des Speichers zurückgegeben und die Zuweisung kann oder kann nicht Speicher wieder an das System. Möglicherweise liegt eine Diskrepanz zwischen der `Used Memory` -Wert und den Speicherverbrauch vom Betriebssystem gemeldet. Möglicherweise aufgrund der Tatsache Speicher verwendet und veröffentlicht Redis jedoch nicht an das System zurückgegeben wurde. Zum Verringern von Speicherproblemen führen Sie die folgenden Schritte aus.
    -   Aktualisieren Sie den Cache vergrößern, sodass nicht gegen Speicherlimit auf dem System ausführen.
    -   Legen Sie Ablaufzeiten Tasten, sodass ältere Werte proaktiv entfernt werden.
    -   Überwachen der der `used_memory_rss` cache-Metrik. Wenn dieser Wert die Größe des Cache nähert, werden Sie wahrscheinlich zu Performance-Problemen. Verteilen Sie die Daten über mehrere Splitter, wenn Sie einen Premium-Cache, oder aktualisieren Sie auf eine größere Cachegröße.

    Weitere Informationen finden Sie unter [Speicherdruck auf dem Server](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Weitere Informationen

-   [Welche Redis Cache Angebot und Größe sollte ich verwenden?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Wie bewerten und Testen Sie die Leistung meines Caches?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Wie können Redis Befehle ausführen?](cache-faq.md#how-can-i-run-redis-commands)
-   [Azure Redis Cache überwachen](cache-how-to-monitor.md)