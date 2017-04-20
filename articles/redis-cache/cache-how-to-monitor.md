<properties 
    pageTitle="Azure Redis Cache überwachen | Microsoft Azure" 
    description="Informationen Sie zum Status und Performance Azure Redis Cache Instanzen überwachen" 
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
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Azure Redis Cache überwachen

Azure Redis Cache bietet mehrere Optionen für die Überwachung der Cacheinstanzen. Sie können Daten anzeigen, Metriken Diagramme an das Startmenü anheften Datums- und Zeitbereich der Überwachung Diagramme anpassen, hinzufügen und entfernen Metriken aus den Diagrammen und Alarm festlegen, wenn bestimmte Bedingung zutrifft. Diese Tools ermöglichen die Überwachung des Status von Azure Redis Cache Instanzen und Zwischenspeichern Anwendung verwalten.

Bei Cache Diagnose aktivierten werden Metriken für Azure Redis Cacheinstanzen ungefähr alle 30 Sekunden erfasst und gespeichert werden in den Diagrammen Metriken angezeigt und von Warnregeln ausgewertet.

Cache Metriken sind mit den Redis gesammelt [Kommando](http://redis.io/commands/info) . Weitere Informationen zu anderen Informationen Werte für jede Metrik zwischenspeichern, [verfügbaren Metriken und Berichterstellungsintervalle](#available-metrics-and-reporting-intervals).

Cache-Metriken, die Cacheinstanz in [Azure-Portal](https://portal.azure.com) [Durchsuchen](cache-configure.md#configure-redis-cache-settings) an. Metriken für Azure Redis Cacheinstanzen erfolgt-Blade **Redis Metriken** .

![Redis Kennzahlen][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] Wenn die folgende Meldung in das Blade **Redis Metriken** angezeigt wird, führen Sie die Schritte im Abschnitt [Cache Diagnose aktivieren](#enable-cache-diagnostics) Cache Diagnose aktivieren.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

**Redis Metriken** Blade hat **Überwachung** -Diagramme mit Cache-Metriken. Jedes Diagramm kann durch Hinzufügen oder Entfernen von Metriken und ändern das Berichtsintervall angepasst werden. Anzeigen und Konfigurieren von Operationen und warnt, hat **Redis Cache** -Blade einen Abschnitt **Operationen** , der Cache **Ereignisse** und **Warnregeln**angezeigt.

## <a name="enable-cache-diagnostics"></a>Cache-Diagnose aktivieren

Azure Redis Cache ermöglicht Diagnosedaten Tools verwenden können möchten und bearbeiten die Daten direkt in ein Speicherkonto gespeichert haben. Reihenfolge für Cache-Diagnose gesammelt werden muss gespeichert und in Azure-Portal angezeigt ein Speicherkonto konfiguriert werden. Caches in derselben Region und Abonnement freigeben demselben Speicherkonto für die Diagnose und die Konfiguration geändert wird, gilt für alle Caches das Abonnement in diesem Bereich.

Navigieren Sie zum Aktivieren und Konfigurieren von Cache-Diagnose, **Redis Cache** -Blade für die Cacheinstanz. Wenn Diagnose noch nicht aktiviert sind, wird eine Meldung anstelle einer Diagnose angezeigt.

![Cache-Diagnose aktivieren][redis-cache-enable-diagnostics]

Klicken Sie auf das Blatt **Metrik** anzuzeigen und auf **Diagnose Einstellungen** aktivieren und konfigurieren die Diagnose für die Dienstinstanz Cache.

![Diagnose-Einstellung][redis-cache-diagnostic-settings]

![Diagnose konfigurieren][redis-cache-configure-diagnostics]

Klicken Sie **auf** Cache-Diagnose aktivieren und die Diagnose: Konfiguration anzeigen.

Klicken Sie auf den Pfeil rechts neben **Speicherkonto** ein Speicherkonto für diagnostische Daten auswählen. Für eine optimale Leistung wird wählen Sie ein Speicherkonto in derselben Region als Cache aus.

Sobald die Diagnose Einstellungen konfiguriert sind, klicken Sie auf **Speichern** , um die Konfiguration zu speichern. Beachten Sie, dass dauert einen Moment wirksam wird.

>[AZURE.IMPORTANT] Caches in derselben Region und Abonnement freigeben eingestellten Storage Diagnostics, und wenn die Konfiguration geändert (Diagnose aktivieren bzw. deaktivieren oder ändern das Speicherkonto) gilt für alle Caches das Abonnement in diesem Bereich.

Überprüfen Sie zum Anzeigen der gespeicherten Metriken Tabellen in das Speicherkonto mit Namen, die mit `WADMetrics`. Weitere Informationen zum Zugriff auf die gespeicherten Metriken außerhalb der Azure-Portal finden Sie unter Sample [Data Access Redis Cache überwachen](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .

>[AZURE.NOTE] Nur ausgewählte Speicherkonto auf Metriken werden in Azure-Portal angezeigt. Ändert Speicherkonten Daten im Speicherkonto konfigurierte bleibt heruntergeladen, aber nicht in Azure-Portal angezeigt.  

## <a name="available-metrics-and-reporting-intervals"></a>Verfügbare statistische Werte und Intervalle reporting

Cache-Metriken sind über mehrere reporting, u.a. **Stunde**, **heute** **Woche**und **benutzerdefinierte**gemeldet. **Metrik** Blade für jedes Diagramm Metrik zeigt die durchschnittliche, minimale und maximale Werte für jede Metrik im Diagramm und einige Messgrößen für das Berichtsintervall insgesamt angezeigt. 

Jede Metrik enthält zwei Versionen. Eine Metrik misst Performance für den gesamten Cache und Caches, die [Clusterbildung](cache-how-to-premium-clustering.md)eine zweite Version der Metrik verwenden, enthält `(Shard 0-9)` Namen Maßnahmen Performance für eine einzelne Splitter in einem Cache. Wenn beispielsweise ein Cache 4 Splitter `Cache Hits` ist der Gesamtbetrag für den gesamten Cache Treffer und `Cache Hits (Shard 3)` ist nur die Treffer, Splitter des Caches.

>[AZURE.NOTE] Auch wenn der Cache mit nicht verbundenen aktiven Clientanwendungen ist, sehen Sie möglicherweise einige Cacheaktivität verbundenen Clients Speicherverwendung und ausgeführten Vorgänge. Diese Aktivität ist normaler Betrieb auf Azure Redis Cache-Instanz.

| Metrik            | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cache-Treffer        | Die Anzahl der erfolgreichen Schlüssel suchen während der angegebenen Berichtsintervall. Zugeordnet `keyspace_hits` von Redis [INFO](http://redis.io/commands/info) Befehl.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Cachefehler      | Die Anzahl der fehlgeschlagenen Schlüssel suchen während der angegebenen Berichtsintervall. Zugeordnet `keyspace_misses` aus dem Befehl INFO Redis. Cachefehler bedeuten nicht unbedingt, dass ein Problem mit dem Cache. Beispielsweise sucht bei der Cache-Aside Programmiermuster eine Anwendung zunächst für ein Element im Cache. Ist das Element nicht vorhanden (Cachefehler), wird das Element aus der Datenbank abgerufen und im Cache für nächsten hinzugefügt. Cachefehler sind normal für Cache-Aside Programmiermuster. Ist die Anzahl der Cachefehler höher als erwartet, überprüfen Sie die Anwendungslogik, die füllt und aus dem Cache gelesen. Wenn Elemente aus dem Cache aufgrund ungenügenden Arbeitsspeichers vertrieben werden und einige Cachefehler aber ein besseres Maß für ungenügenden Arbeitsspeicher überwachen wäre `Used Memory` oder `Evicted Keys`. |
| Verbundene Clients | Die Anzahl der Clientverbindungen zum Cache im angegebenen Intervall reporting. Zugeordnet `connected_clients` aus dem Befehl INFO Redis. [Verbindungslimit](cache-configure.md#default-redis-server-configuration) erreicht werden weitere Verbindungsversuche zum Cache fehl. Beachten Sie, dass auch wenn keine aktiven Clientanwendung sind möglicherweise noch einige Instanzen des verbundenen Clients aufgrund interner Prozesse und Verbindungen.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Entfernten Schlüssel      | Die Anzahl der Elemente aus dem Cache entfernt werden, während der angegebenen Berichtsintervall aufgrund der `maxmemory` Limit. Zugeordnet `evicted_keys` aus dem Befehl INFO Redis.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Abgelaufener Schlüssel      | Die Anzahl der Elemente wurde aus dem Cache beim angegebenen Berichtsintervall. Dieser Wert entspricht `expired_keys` aus dem Befehl INFO Redis.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Ruft ab              | Die Anzahl der Get-Operationen aus dem Cache beim angegebenen Berichtsintervall. Dieser Wert ist die Summe der folgenden Werte aus den Redis Befehl: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, und `cmdstat_getrange`, und entspricht der Summe der Cachetreffer und Fehler während das Berichtsintervall.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Redis Serverlast | Der Prozentsatz der Zyklen in denen Redis-Server verarbeitet und nicht warten für Nachrichten beschäftigt. Wenn dieser Leistungsindikator erreicht 100 bedeutet, dass Redis-Server trifft eine Obergrenze Leistung und die CPU nicht verarbeitet arbeiten Sie schneller. Wenn man hohe Serverlast Redis Timeout Ausnahmen im Client angezeigt wird. In diesem Fall sollten Sie skalieren oder Partitionieren der Daten in mehrere Caches.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Legt              | Die Anzahl der Mengenoperationen Cache im angegebenen Intervall reporting. Dieser Wert ist die Summe der folgenden Werte aus den Redis Befehl: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, und `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Gesamtanzahl der Vorgänge  | Die Gesamtzahl der Befehle, die vom Cacheserver angegebenen Berichtsintervall verarbeitet. Dieser Wert entspricht `total_commands_processed` aus dem Befehl INFO Redis. Beachten Sie, dass bei Azure Redis Cache rein Pub-Sub keine Metriken für werden `Cache Hits`, `Cache Misses`, `Gets`, oder `Sets`, aber `Total Operations` Metriken, die Cache-Nutzung für Pub-Sub Vorgänge entsprechen.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Verwendeter Speicher       | Die den Cache Schlüssel-Wert-Paare im Cache in MB während der angegebenen Berichtsintervall verwendet. Dieser Wert entspricht `used_memory` aus dem Befehl INFO Redis. Dies schließt keine Metadaten oder Fragmentierung.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Verwendeter Speicher RSS   | Der Cache verwendete Speichermenge in MB während der angegebenen Berichtsintervall Fragmentierung sowie Metadaten. Dieser Wert entspricht `used_memory_rss` aus dem Befehl INFO Redis.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| CPU               | Die CPU-Auslastung des Servers Azure Redis Cache als Prozentsatz während der angegebenen Berichtsintervall. Dieser Wert wird für das Betriebssystem zugeordnet `\Processor(_Total)\% Processor Time` Leistungsindikator.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Cache-lesen        | Die Menge der Daten aus dem Cache gelesen in Megabyte pro Sekunde (MB/s) während der angegebenen Berichtsintervall. Dieser Wert wird von Netzwerk-Schnittstellenkarten abgeleitet, die der virtuelle Computer unterstützen, die den Cache und nicht bestimmten Redis. **Dieser Wert entspricht der Netzwerkbandbreite durch Cache. Wenn Sie Benachrichtigungen für Server Seite Netzwerk Bandbreitengrenzwerte festlegen möchten, erstellen sie mit diesem `Cache Read` Zähler. Siehe [Tabelle](cache-faq.md#cache-performance) für die überwachte Bandbreitengrenzwerte für verschiedene Ebenen und Größen Preise Cache.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Cache-Schreiben       | Die Datenmenge im Cache in Megabyte pro Sekunde (MB/s) während der angegebenen geschrieben Berichterstellungsintervall. Dieser Wert wird von Netzwerk-Schnittstellenkarten abgeleitet, die der virtuelle Computer unterstützen, die den Cache und nicht bestimmten Redis. Dieser Wert entspricht der Netzwerkbandbreite im Cache vom Client gesendeten Daten.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>Daten anzeigen und Anpassen von Diagrammen

Sie können einen Überblick über die Metriken für den Cache-Blade **Redis Kennzahlen** anzeigen. **Redis Metriken** auf Blade Einstellungen **Alle** > **Redis Metriken**.

![Redis Kennzahlen][redis-cache-redis-metrics]


Das Blade **Redis Metriken** enthält die folgenden Diagramme.

| Metriken Diagramm redis | Angezeigte Kennzahlen     |
|------------------|-------------------|
| Write Cache lesen und Cache | Cache-lesen |
|                            | Cache-Schreiben |
| Verbundene Clients      | Verbundene Clients |
| Erfolgreiche und erfolglose Zugriffe  | Cache-Treffer        |
|                  | Cachefehler      |
| Befehle gesamt   | Gesamtanzahl der Vorgänge  |
| Ruft und Sets    | Ruft ab              |
|                  | Legt              |
| CPU-Auslastung         | CPU           |
| Speicherverwendung      | Verwendeter Speicher   |
|                   | Verwendeter Speicher RSS |
| Redis Serverlast | Serverlast   |
| Anzahl der Schlüssel | Schlüssel insgesamt |
|           | Entfernten Schlüssel |
|           | Abgelaufener Schlüssel |


Eine detailliertere Ansicht der Metriken in einem bestimmten Diagramm und das Diagramm anzupassen klicken Sie auf das gewünschte Diagramm aus dem Blade **Redis Metriken** Blade **Metrik** für das Diagramm angezeigt.

![Blatt Metrik][redis-cache-metric-blade]

Alarme, die auf ein Diagramm angezeigten Metriken sind unten Blade **Metrik** für das Diagramm angezeigt.

Wählen Sie zum Hinzufügen oder Entfernen von Metriken, oder ändern Sie das Berichtsintervall, **Diagramm bearbeiten**.

Hinzufügen oder Entfernen von Metriken aus dem Diagramm, klicken Sie auf das Kontrollkästchen neben dem Namen der Metrik. Um das Berichtsintervall, klicken Sie auf das gewünschte Intervall. Um den **Diagrammtyp**zu ändern, klicken Sie auf das gewünschte Format. Sobald die gewünschte geändert wird, klicken Sie auf **Speichern**. 

![Diagramm bearbeiten][redis-cache-edit-chart]

Beim Klicken auf **Speichern** bleibt die Änderung bis zum Blade **Metrik** lassen. Wenn Sie später zurückkehren, sehen die ursprünglichen Metrik und Zeitraum erneut Sie. Weitere Informationen zum Anpassen von Diagrammen finden Sie unter [Monitor Dienstmetrik](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Um die Metriken für einen bestimmten Zeitraum in einem Diagramm anzuzeigen, den Mauszeiger über einem bestimmten Balken oder auf das Diagramm, das die gewünschte Zeit entspricht, und die Metrik für dieses Intervall angezeigt.

![Diagramm-Details anzeigen][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>Einen Premium-Cache mit clustering überwachen

Premium-Caches, die [Clusterunterstützung](cache-how-to-premium-clustering.md) aktiviert können bis zu 10 Splitter. Jeder Splitter verfügt über eigene Metriken und diese Metriken zu Metriken für den Cache insgesamt aggregiert. Jede Metrik enthält zwei Versionen. Eine Metrik misst Performance für den gesamten Cache und eine zweite Version der Metrik umfasst `(Shard 0-9)` Namen Maßnahmen Performance für eine einzelne Splitter in einem Cache. Wenn beispielsweise ein Cache 3 Splitter `Cache Hits` ist der Gesamtbetrag für den gesamten Cache Treffer und `Cache Hits (Shard 2)` ist nur die Treffer, Splitter des Caches.

Jedes Diagramm Überwachung wird oberster Ebene für den Cache mit Metriken für jeden Cache-Splitter.

![Monitor][redis-cache-premium-monitor]

Beim Bewegen der Maus über die Datenpunkte zeigt die Details für diesen Punkt rechtzeitig. 

![Monitor][redis-cache-premium-point-summary]

Größere Werte sind in der Regel die Aggregatwerte für den Cache zwar kleinere Werte der einzelnen Metriken für den Splitter. Beachten Sie, dass in diesem Beispiel drei Splitter werden und Cache-Treffer auf der Splitter gleichmäßig verteilt werden.

![Monitor][redis-cache-premium-point-shard]

Um weitere Details klicken Sie auf das Diagramm, um eine erweiterte Ansicht auf die **Metrik** anzuzeigen.

![Monitor][redis-cache-premium-chart-detail]

Standardmäßig enthält jedes Diagramm auf oberster Ebene Cache-Leistungsindikator sowie die Leistungsindikatoren für die einzelnen Splitter. Sie können diese auf dem **Diagramm bearbeiten** .

![Monitor][redis-cache-premium-edit]

Weitere Informationen über die verfügbaren Leistungsindikatoren finden Sie in der [verfügbaren Metriken und Berichterstellungsintervalle](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Vorgänge und Alarme

Abschnitt mit den **Vorgängen** auf dem **Redis Cache** hat **Ereignisse** und **Warnregeln** Abschnitte.

![Oeprations][redis-cache-operations-events]

Klicken Sie eine Liste der letzten Cachevorgänge **Ereignisse** Diagramm Blatt **Ereignisse** anzuzeigen. Operationen gehören abrufen und regenerieren Sie Zugriffstasten, und die Aktivierung und Auflösung von Warnregeln. Weitere Informationen zu jedem Ereignis klicken Sie auf das Ereignis im Blatt **Ereignisse** .

Weitere Informationen über Ereignisse finden Sie unter [Events und Überwachungsprotokolle](../monitoring-and-diagnostics/insights-debugging-with-events.md).

**Warnregeln** Abschnitt zeigt die Alarme für die Cacheinstanz. Warnregeln kann die Cacheinstanz überwachen und eine e-Mail erhalten, wenn Sie ein bestimmten Metrikwert in der Regel definierten Schwellenwert erreicht. 

Warnregeln werden ca. alle fünf Minuten ausgewertet und Aktivierung eine Warnregel konfigurierten Benachrichtigungen gesendet. Warnregel Aktivierungen und Benachrichtigungen werden nicht sofort verarbeitet. möglicherweise eine Verzögerung von mehreren Minuten eine Warnregel aktiviert und Benachrichtigungen.

Warnregeln können betrachtet und **Metrik** Blade für bestimmte Überwachung Diagramm oder Blade **Warnregeln** .

Haben die folgenden Eigenschaften.

| Regel-Eigenschaft                 | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ressource                            | Die Ressource, durch die Regel ausgewertet. Beim Erstellen einer Warnregel aus Redis-Cache ist der Cache der Ressource.                                                                                                                                                                                                                                                                                                                                                  |
| Name                                | Namen, die eindeutig die Regel innerhalb der aktuellen Cacheinstanz.                                                                                                                                                                                                                                                                                                                                                                                       |
| Beschreibung                         | Optionale Beschreibung der Regel.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Metrik                              | Die Metrik der Regel überwacht werden. Finden Sie eine Liste der Cache Metriken verfügbaren Metriken und Berichterstellungsintervalle.                                                                                                                                                                                                                                                                                                                                             |
| Bedingung                           | Der Bedingungsoperator für die Regel. Mögliche Angaben sind: größer als, größer oder gleich, kleiner als, kleiner als oder gleich                                                                                                                                                                                                                                                                                                                             |
| Schwellenwert                           | Der Wert zum Vergleichen mit der Metrik mithilfe des Operators durch die Condition-Eigenschaft angegeben. Je nach der Metrik dieser Wert möglicherweise Bytes/s, Bytes, % oder Anzahl.                                                                                                                                                                                                                                                                                     |
| Zeitraum                              | Gibt den Zeitraum über dem Durchschnittswert der Metrik Warnregel Vergleich verwendet wird. Beispielsweise ist beträgt in der letzten Stunde der durchschnittliche Wert der Metrik über vorherige Stunden für den Vergleich verwendet. Möchten Sie benachrichtigt werden, wenn durch eine Aktivität der Schwellenwert erreicht ist, eignet sich dann kürzer. Um benachrichtigt zu werden, wird über dem Schwellenwert andauernde Aktivität verwenden Sie einen längeren Zeitraum. |
| E-Mail-Dienst und Co-Administratoren | Wenn true, Dienstadministratoren und Co-Administrator per e-Mail aktiviert die Warnung.                                                                                                                                                                                                                                                                                                                                                                    |
| Zusätzliche Administrator e-Mail      | Optionale e-Mail-Adresse weitere Administrator benachrichtigt werden, wenn die Warnung aktiviert ist.                                                                                                                                                                                                                                                                                                                                                                    |

Pro Regel Aktivierung wird nur eine Benachrichtigung gesendet. Der Schwellenwert für eine Regel überschritten und eine Benachrichtigung gesendet, wird die Regel erneut erst ausgewertet Metrik Schwellenwert. Die Metrik anschließend den Schwellenwert überschreitet, die Warnung aktiviert und eine neue Benachrichtigung.

Klicken Sie alle Warnregeln für die Cacheinstanz **Warnregeln** an **Redis Cache** -Blade. Um nur die Warnungsregeln anzeigen, die einen bestimmten Metrikwert verwenden, navigieren Sie zu Blade **Metrik** für das Diagramm, die Metrik.

![Warnregeln][redis-cache-alert-rules]

Um eine Regel hinzuzufügen, klicken Sie auf **Warnung** oder Blade- **Metrik** **Warnregeln** Blade. 

Geben Sie die gewünschten Kriterien in dem Blatt **Hinzufügen eine Warnung** , und klicken Sie auf **OK**. 

![Regel hinzufügen][redis-cache-add-alert]

>[AZURE.NOTE] Wenn **Metrik** Blade **quot** auf eine Warnregel erstellt wird, werden im Diagramm auf diesem Blatt angezeigten Metriken in der Dropdownliste **Metrik** angezeigt. Wenn **Warnregeln** Blade **quot** auf eine Warnregel erstellt wird, stehen alle Cache-Vorgaben in der Dropdownliste **Metrik** .

Sobald eine Warnregel speichern wird Blade **Warnregeln** sowie auf die **Metrik** für Diagramme, die die Metrik in der Warnregel anzeigen. Zum Bearbeiten einer Regel klicken Sie auf die Regel das Blatt **Bearbeiten** angezeigt. Aus dem Blatt **Bearbeiten** können Sie bearbeiten Sie die Eigenschaften der Regel, löschen die Warnregel zu deaktivieren oder die Regel wieder aktivieren, wenn sie zuvor deaktiviert wurde.

>[AZURE.NOTE] Änderungen an den Eigenschaften der Regel dauert einen Moment, bevor sie **Warnregeln** Blade oder Blade- **Metrik** entsprechend.

Aktivierung eine Warnregel e-Mail ist abhängig von der Konfiguration der Warnregel und ein Warnsymbol in der **Warnregeln** -Blade **Redis Cache** angezeigt.

Eine Warnungsregel gilt aufgelöst werden, wenn die Warnung nicht mehr True ergibt. Sobald die regelbedingung behoben ist, wird das Warnsymbol mit einem Häkchen ersetzt. Ausführliche Informationen zum Alarm Aktivierungen und Auflösung klicken Sie auf die **Ereignisse** auf dem **Redis Cache** Anzeigen der Ereignisse auf die **Ereignisse** .

Weitere Informationen über Alarme in Azure finden Sie unter [Benachrichtigung empfangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="metrics-on-the-redis-cache-blade"></a>Metriken Redis Cache-Blade

**Redis Cache** -Blade zeigt folgende Messgrößen.

-   [Überwachen von Diagrammen](#monitoring-charts)
-   [Verwendung-Diagramme](#usage-charts)

### <a name="monitoring-charts"></a>Überwachen von Diagrammen

Abschnitt **Überwachung** hat **Treffer und vermisst**, **abgerufen und festgelegt**, **Anschlüsse**und **Befehle gesamt** Diagramme.

![Überwachen von Diagrammen][redis-cache-monitoring-part]

Die folgenden Metriken anzeigen **Überwachen** Diagramme

| Überwachung Diagramm | Cache-Metriken     |
|------------------|-------------------|
| Erfolgreiche und erfolglose Zugriffe  | Cache-Treffer        |
|                  | Cachefehler      |
| Ruft und Sets    | Ruft ab              |
|                  | Legt              |
| Anschlüsse      | Verbundene Clients |
| Befehle gesamt   | Gesamtanzahl der Vorgänge  |

Informationen zum Anzeigen der Metriken und Anpassen die einzelnen Diagramme in diesem Abschnitt finden Sie in Abschnitt [Daten anzeigen und Metriken Diagramme anpassen](#how-to-view-metrics-and-customize-charts) .

### <a name="usage-charts"></a>Verwendung-Diagramme

Im Abschnitt **Verwendung** hat **Serverlast Redis**, **Speicher**, **Netzwerkbandbreite**und **CPU-Auslastung** Diagramme und zeigt auch die **Preisstufe** für die Cacheinstanz.

![Verwendung-Diagramme][redis-cache-usage-part]

**Preisstufe** zeigt Cache Preise Stufen und kann mit der [Skalierung](cache-how-to-scale.md) der Cache einen anderen Tarif.

Die **Verwendung** Diagramme zeigen die folgenden Metriken.

| Diagramm der Anwendungsprotokollverwendung       | Cache-Metriken |
|-------------------|---------------|
| Redis Serverlast | Serverlast   |
| Speicherverwendung      | Verwendeter Speicher   |
| Netzwerk-Bandbreite | Cache-Schreiben   |
| CPU-Auslastung         | CPU           |

Informationen zum Anzeigen der Metriken und Anpassen die einzelnen Diagramme in diesem Abschnitt finden Sie in Abschnitt [Daten anzeigen und Metriken Diagramme anpassen](#how-to-view-metrics-and-customize-charts) .
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png



