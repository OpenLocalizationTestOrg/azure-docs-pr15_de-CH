<properties 
    pageTitle="Azure Redis Cache verwalten | Microsoft Azure"
    description="Erfahren Sie, wie Verwaltungsaufgaben wie Neustart und Zeitplan Updates für Azure Redis Cache"
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

# <a name="how-to-administer-azure-redis-cache"></a>Azure Redis Cache verwalten

Dieses Thema beschreibt die Verwaltungsaufgaben wie Neustarten und Planen von Updates für Ihre Azure Redis Cache-Instanzen.

>[AZURE.IMPORTANT] Einstellungen und in diesem Artikel beschriebenen Features sind nur für Premium-Tier-Caches.


## <a name="administration-settings"></a>Verwaltung Standardeinstellungen

Die Azure Redis Cache- **Verwaltung** können Sie die folgenden Verwaltungsaufgaben für den Premium-Cache ausgeführt. Klicken Sie verwaltungseinstellungen **Einstellungen** oder **Alle** Redis Cache Blade und Scroll Abschnitt **Verwaltung** Blade- **Einstellungen** .

![Verwaltung](./media/cache-administration/redis-cache-administration.png)

-   [Neustart](#reboot)
-   [Planen von updates](#schedule-updates)

## <a name="reboot"></a>Neustart

Blade **Starten** können Sie einen oder mehrere Knoten des Cache neu starten. Dadurch können Sie der Anwendung Robustheit im Fehlerfall.

![Neustart](./media/cache-administration/redis-cache-reboot.png)

Haben Sie einen Premium-Cache mit Clusterunterstützung aktiviert, können Sie die Splitter des Caches neu starten auswählen.

![Neustart](./media/cache-administration/redis-cache-reboot-cluster.png)

Um einen oder mehrere Knoten des Cache neu zu starten, wählen Sie die gewünschten Knoten, und klicken Sie auf **neu starten**. Haben Sie einen Premium-Cache mit Clusterunterstützung, wählen Sie den Shards zu starten, und klicken Sie dann auf **neu starten**. Nach einigen Minuten Onlineschalten der ausgewählten Knoten Neustart sind und einige Minuten später.

Die Auswirkung auf die Clientanwendungen hängt die Knoten, die Sie neu starten.

-   **Master** - master-Knoten neu gestartet wird, Azure Redis Cache Replikat Knoten ein Failover und Master fördert. Bei diesem Failover möglicherweise eine kurze Zeit in der Verbindungen im Cache fehlschlagen.
-   **Slave** - beim Neustart des Slave-Knotens gibt normalerweise keine Auswirkung auf cacheclients.
-   **Beide master und slave** - beide Cache-Knoten neu gestartet werden, alle Daten verliert in den Cache und die Verbindung zu der Cache nicht bis primäre Knoten wieder online geschaltet. Wenn [Dauerhaftigkeit von Daten](cache-how-to-premium-persistence.md)konfiguriert haben, wird die aktuellste Sicherung wiederhergestellt, wenn der Cache wieder online ist. Beachten Sie, dass alle Cache-Schreibvorgänge, die nach der letzten Sicherung stattgefunden verloren.
-   **Knoten eines Premium-Caches mit Clusterunterstützung** - beim Neustart einen Premium-Cache-Knoten mit Clusterunterstützung, entspricht das Verhalten beim Neustart eines Caches nicht gruppierten Knoten.


>[AZURE.IMPORTANT] Neustart ist nur für Premium-Tier-Caches.

## <a name="reboot-faq"></a>Häufig gestellte Fragen zu starten

-   [Der Knoten sollte zum Testen meiner Anwendung starten?](#which-node-should-i-reboot-to-test-my-application)
-   [Kann den Cache deaktivieren Sie Clientverbindungen neu starten?](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Verliere Daten aus meinem Cache ich, wenn ich einen Neustart durchgeführt?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [Können meine Cache mit PowerShell, CLI oder anderen Verwaltungstools neu starten?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [Welche Preise leisten können die Neustart-Funktionen?](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Der Knoten sollte zum Testen meiner Anwendung starten?

Starten Sie zum Testen die Stabilität Ihrer Anwendung vor einem Ausfall des primären Knotens des Cache **Master** -Knoten. Testen Sie die Stabilität Ihrer Anwendung vor einem Ausfall des zweiten Knotens Neustart **Slave** -Knoten. Testen Sie die Stabilität Ihrer Anwendung gegen Totalausfall des Caches starten Sie **beide** Knoten.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Kann den Cache deaktivieren Sie Clientverbindungen neu starten?

Ja, Sie den Cache neu starten werden alle Clientverbindungen gelöscht. Dies kann sinnvoll sein, alle Clientverbindungen, beispielsweise durch einen logischen Fehler oder einen Fehler in der Clientanwendung verwendet werden. Jeder Tarif hat unterschiedliche [Client Verbindungslimits](cache-configure.md#default-redis-server-configuration) für die verschiedenen Größen und Grenzwerte erreicht werden, keine weitere Clientverbindungen akzeptiert. Neustart des Caches bietet eine Möglichkeit, alle Clientverbindungen zu deaktivieren.

>[AZURE.IMPORTANT] Wenn Ihre Clientverbindungen logischer Fehler oder Fehler im Clientcode verwendet werden, beachten Sie, dass StackExchange.Redis automatisch erneut verbunden wird, nachdem der Redis-Knoten wieder online ist. Wenn das zugrunde liegende Problem nicht behoben wird, weiterhin die Clientverbindungen verwendet werden.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Verliere Daten aus meinem Cache ich, wenn ich einen Neustart durchgeführt?

Neustart der **Master-** und **untergeordnete** Knoten alle Daten im Cache (oder in diesem Splitter verwenden Sie einen Premium-Cache mit Clusterunterstützung) ist verloren. Wenn [Dauerhaftigkeit von Daten](cache-how-to-premium-persistence.md)konfiguriert haben, werden die letzte Sicherung wiederhergestellt, wenn der Cache wieder online ist. Beachten Sie, dass alle Cache-Schreibvorgänge, die nach der Sicherung aufgetreten sind verloren.

Nur einer der Knoten nach einem Neustart Daten sind in der Regel verloren, aber dennoch möglicherweise. Zum Beispiel master-Knoten neu gestartet und Cache Schreibvorgänge ausgeführt, die Daten aus dem Cache schreiben verloren. Ein weiteres Szenario für Datenverlust wäre Neustart eines Knotens und der Knoten mit einem Fehler gleichzeitig ausfallen. Weitere Informationen zu möglichen Ursachen für Datenverlust finden Sie [Daten in Redis passierte?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Können meine Cache mit PowerShell, CLI oder anderen Verwaltungstools neu starten?

Ja, weitere PowerShell Anweisungen [Redis Cache neu starten](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Welche Preise leisten können die Neustart-Funktionen?

Neustart ist nur in der Premium Tarif.

## <a name="schedule-updates"></a>Planen von updates

**Planen von Updates** Blade können Sie ein Wartungsfenster für den Cache festlegen. Im Wartungsfenster angegeben, werden alle Redis Server in diesem aktualisiert. Beachten Sie, dass im Wartungsfenster gilt nur für Serverupdates Redis und nicht für jede Azure oder des Betriebssystems VMs, die den Cache hosten.

![Planen von updates](./media/cache-administration/redis-schedule-updates.png)

Um ein Wartungsfenster anzugeben, überprüfen Sie die gewünschten Tage Geben Sie Wartung Fenster Startstunde für jeden Tag an und klicken Sie auf **OK**. Beachten Sie, dass die Dauer der Wartung in UTC. 

>[AZURE.NOTE] Im Wartungsfenster Updates ist 5 Stunden. Dieser Wert lässt sich nicht aus dem Azure-Portal, aber mit PowerShell Konfigurieren der `MaintenanceWindow` Parameter des Cmdlets [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) . Weitere Informationen finden Sie unter [können geplante Updates mit PowerShell, CLI oder anderen Verwaltungstools verwaltet?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Planen von Updates häufig gestellte Fragen

-   [Wann treten Updates Wenn Zeitplan Aktualisierungsfunktion verwendet wird?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Welche Updates erfolgen im geplanten Wartungsfenster?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Kann ich geplante Updates mit PowerShell, CLI oder anderen Verwaltungstools verwaltet?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [Welche Preise leisten können Funktion Updates planen?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Wann treten Updates Wenn Zeitplan Aktualisierungsfunktion verwendet wird?

Wenn Sie ein Wartungsfenster angeben, können jederzeit aktualisiert werden.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Welche Updates erfolgen im geplanten Wartungsfenster?

Nur Redis Server im geplanten Wartungsfenster aktualisiert. Das Wartungsfenster gilt nicht Azure Updates oder Updates für die VM-Betriebssystem.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Kann ich geplante Updates mit PowerShell, CLI oder anderen Verwaltungstools verwaltet?

Ja, können Sie die geplante Updates mithilfe der folgenden PowerShell-Cmdlets verwalten.

-   [AzureRmRedisCachePatchSchedule abrufen](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Neue AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Neue AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [AzureRmRedisCachePatchSchedule entfernen](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Welche Preise leisten können Funktion Updates planen?

Planen von Updates steht nur in der Premium Tarif.

## <a name="next-steps"></a>Nächste Schritte

-   Untersuchen Sie weitere [Azure Redis Premium Cacheschicht](cache-premium-tier-intro.md) Funktionen.





