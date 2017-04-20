<properties
   pageTitle="Stabilität und Recovery auf Elasticsearch in Azure konfigurieren"
   description="Aspekte im Zusammenhang mit Flexibilität und Recovery für Elasticsearch."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>
   
# <a name="configuring-resilience-and-recovery-on-elasticsearch-on-azure"></a>Stabilität und Recovery auf Elasticsearch in Azure konfigurieren

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie](guidance-elasticsearch.md). 

Eine Schlüsselfunktion der Elasticsearch ist die Unterstützung Robustheit bei Knotenausfälle oder Partition Ereignisse enthält. Die Replikation ist der offensichtliche Weg, der Sie zu die Stabilität eines Clusters verbessern, aktivieren Elasticsearch sicherstellen, dass mehrere Kopien der Datenelemente auf verschiedenen Knoten bei einem Knoten nicht zugegriffen werden. Wird ein Knoten verfügbar, dienen anderen Knoten mit Replikaten von Daten aus dem ausgefallenen Knoten die fehlenden Daten, bis das Problem behoben ist. Bei einer längeren Begriff Problem fehlende Knoten durch einen neuen ersetzt werden und Elasticsearch kann die Daten auf den neuen Knoten von den Replikaten wiederherzustellen.

Hier fassen wir die Flexibilität und Recovery Optionen mit Elasticsearch in Azure gehostet und einige wichtige Aspekte eines Elasticsearch-Clusters sollten Sie um die Gefahr von Datenverlusten und erweiterte Daten Recovery-Zeiten zu minimieren.

Dieser Artikel beschreibt auch Beispieltests, mit die die Effekte der verschiedenen Arten von Fehlern auf einem Cluster Elasticsearch und wie das System reagiert während der Wiederherstellung ausgeführt wurden.

Ein Elasticsearch Cluster verwendet Replikate zu Verfügbarkeit Lese-Performance. Replikate sollten auf andere VMs aus der primären Splitter gespeichert werden, die sie replizieren. Beabsichtigt ist, dass fällt die VM hostet einen Datenknoten oder verfügbar, das System kann funktioniert mit VMs mit Replikaten.

## <a name="using-dedicated-master-nodes"></a>Dedizierte Knoten master

Master-Knoten ist ein Knoten in einem Cluster Elasticsearch gewählt. Dieses Knotens soll Clustervorgänge wie Management ausgeführt werden:

- Erkennen von ausgefallenen Knoten und Replikate umschalten.

- Verschieben von Splitter Knoten Arbeitslast ausgleichen.

- Splitter wiederherstellen, wenn ein Knoten wieder online geschaltet.

Sie sollten erwägen, dedizierte master-Knoten wichtige Cluster, und 3 dedizierten Knoten, dessen einzige Funktion sein soll. Diese Konfiguration verringert Ressource intensiv, die diese Knoten durchführen (nicht speichern oder verarbeiten Abfragen) und Cluster Stabilität zu verbessern. Markierte Master sollte fehlschlagen, nur einen dieser Knoten gewählt, die andere enthält eine Kopie des Systemstatus und übernehmen.

## <a name="controlling-high-availability-with-azure--update-domains-and-fault-domains"></a>Hohen Verfügbarkeit mit Azure – Update und Fehlertoleranz Domänen steuern 

Verschiedene VMs können auf die gleiche physische Hardware freigeben. In einem Rechenzentrum Azure ein einzelnes Rack kann mehrere VMs hosten, und alle diese VMs häufige Quelle und Netzwerk Netzschalter. Der Ausfall ein einzelnes auf Rack-Ebene kann daher mehrere VMs auswirken. Azure verwendet das Konzept der Fehlerdomänen zu verbreiten dieses Risiko. Eine Fehlerdomäne entspricht ungefähr einer Gruppe, die einem Rack gemeinsam nutzen. Um sicherzustellen, dass Fehler auf Rack-Ebene ein und halten gleichzeitig alle zugehörigen Replikate Knoten nicht abstürzt, sollten die VMs Fehlerdomänen verteilt sind.

Ebenso können VMs [Azure Fabric Controller](https://azure.microsoft.com/documentation/videos/fabric-controller-internals-building-and-updating-high-availability-apps/) abgeschaltet werden geplante Wartung und Betriebssystem-Upgrades durchführen. Azure weist VMs Domänen aktualisieren. Tritt ein Ereignis Wartungsarbeiten erfolgt jeweils nur VMs in einer einzelnen updatedomäne. Virtuelle Computer in anderen aktualisierungsdomänen verbleiben bis zum die virtuellen Computern in der Domäne aktualisieren aktualisiert wieder online geschaltet werden. Daher müssen Sie sicherstellen, dass VMs hosten Knoten und ihre Replikationen möglichst unterschiedliche Domänen gehören.

> [AZURE.NOTE] Weitere Informationen über Fehler und updatedomänen finden Sie unter [Verwalten der Verfügbarkeit virtueller Maschinen][].

Einen virtueller Computer auf ein bestimmtes Update und Fehlertoleranz Domäne kann nicht explizit zuweisen. Diese Zuordnung wird von Azure Erstellung virtueller Computer gesteuert. Sie können virtuelle Computer als Teil einer Verfügbarkeit erstellt werden. Virtuelle Computer in der gleichen Verfügbarkeit werden Update und Fehlertoleranz Domänen verteilt. VMs manuell erstellt wird, erstellt Azure jedes Verfügbarkeit mit zwei Fehler und fünf updatedomänen festgelegt. VMs werden diese Fehlerdomänen und Domänen um durchlaufen, wie weitere virtuelle Computer, wie folgt bereitgestellt werden aktualisieren: 

| VM | Fehlerdomäne | Domäne aktualisieren |
|----|--------------|---------------|
|  1 |            0 |             0 |
|  2 |            1 |             1 |
|  3 |            0 |             2 |
|  4 |            1 |             3 |
|  5 |            0 |             4 |
|  6 |            1 |             0 |
|  7 |            0 |             1 |

> [AZURE.IMPORTANT] Erstellen virtueller Computer mithilfe der Azure-Ressourcen-Manager kann jede verfügbarkeitsgruppe Fehler bis zu 3 und 20 updatedomänen zugewiesen werden. Dies ist ein Grund für die Verwendung der Ressourcen-Manager.

Im Allgemeinen setzen Sie alle VMs, die demselben Verfügbarkeit dieselben Zweck, aber erstellen Sie unterschiedliche Verfügbarkeit für VMs, die verschiedene Funktionen. Setzt erstellen mindestens separate Verfügbarkeit für Elasticsearch bedeutet dies, dass Sie berücksichtigen sollten:

- VMs hosten Datenknoten.
- VMs hosten Clientknoten (Wenn verwendet werden).
- VMs hosten master-Knoten.

Darüber hinaus sollten Sie sicherstellen, dass jeder Knoten in einem Cluster ist die Domäne aktualisieren und Fehlerdomäne, der er angehört. Diese Informationen helfen, dass Elasticsearch nicht Splitter und sich in der gleichen Fehler erstellen und aktualisieren Domänen minimieren die Möglichkeit einen und die Replikationen zur gleichen Zeit außer Betrieb genommen wird. Sie können einen Knoten Elasticsearch Hardware Verteilung des Clusters Mirror [Splitter Allocation-Bewusstsein](https://www.elastic.co/guide/en/elasticsearch/reference/current/allocation-awareness.html#allocation-awareness)konfigurieren. Beispielsweise konnte ein Paar benutzerdefinierter Attribute namens *FaultDomain* und *UpdateDomain* in die Datei elasticsearch.yml wie folgt definieren:

```yaml
node.faultDomain: \${FAULTDOMAIN}
node.updateDomain: \${UPDATEDOMAIN}
```

In diesem Fall die Attribute setzen mithilfe der Werte der * \${FAULTDOMAIN}* und * \${UPDATEDOMAIN}* Umgebungsvariablen beim Starten von Elasticsearch. Sie müssen die folgenden Einträge in die Datei Elasticsearch.yml, *FaultDomain* und *UpdateDomain* Zuweisung Bewusstsein sind, und geben Sie die zulässigen Werte für diese Attribute:

```yaml
cluster.routing.allocation.awareness.force.updateDomain.values: 0,1,2,3,4
cluster.routing.allocation.awareness.force.faultDomain.values: 0,1
cluster.routing.allocation.awareness.attributes: updateDomain, faultDomain
```

Splitter Allocation-Bewusstsein können zusammen mit [Splitter Zuordnung filtern](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/shard-allocation-filtering.html#shard-allocation-filtering) Sie explizit angeben, welche Knoten für einen bestimmten Index Splitter hosten können.

Möchten Sie die Anzahl der Fehler und updatedomänen in einem Verfügbarkeit skalieren, können Sie zusätzliche Verfügbarkeit Mengen VMs erstellen. Sie müssen jedoch wissen, dass Knoten in unterschiedlichen Verfügbarkeit legt für Wartung gleichzeitig abgeschaltet werden können. Versuchen Sie sicherzustellen, dass jeder Splitter und eines seiner Replikate innerhalb Verfügbarkeit enthalten sind.

> [AZURE.NOTE] Es gibt maximal 100 VMs pro Verfügbarkeit. Weitere Informationen finden Sie unter [Azure-Abonnement und Service Grenzen, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

### <a name="backup-and-restore"></a>Sicherung und Wiederherstellung

Verwendung von Replikaten bieten keinen vollständigen Schutz von schwerwiegenden Fehler (z. B. versehentlich den gesamten Cluster). Sie sollten sicherstellen, dass die Daten in einem Cluster regelmäßig sichern und eine bewährte Strategie für das System aus dieser Sicherung wiederherstellen.

Elasticsearch [Snapshot und Wiederherstellen von APIs](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) verwenden: elastische nicht diese cap. >> Sicherung und Wiederherstellung von Indizes. Snapshots können auf einem freigegebenen Dateisystem gespeichert werden. Alternativ Plugins stehen schreiben, dass Snapshots Hadoop verteilten Dateisystem (HDFS) (die [bietet Plugin](https://github.com/elasticsearch/elasticsearch-hadoop/tree/master/repository-hdfs)) oder Azure-Speicher (der [Azure-Plug-in](https://github.com/elasticsearch/elasticsearch-cloud-azure#azure-repository)).

Berücksichtigen Sie Snapshot Speichermechanismus auswählen sollten Sie folgende Punkte:

- [Azure-Dateispeicher](https://azure.microsoft.com/services/storage/files/) können einem freigegebenen Dateisystem implementiert, die von allen Knoten.

- Verwenden Sie Plugin bietet nur, wenn Sie zusammen mit Hadoop Elasticsearch ausgeführt werden.

- Das Plug-in bietet erfordert Java Security Manager in Elasticsearch Instanz der Java Virtual Machine (JVM) deaktivieren.

- Das Plug-in bietet unterstützt alle bietet kompatible Dateisystem, die richtige Konfiguration Hadoop mit Elasticsearch verwendet wird.

  
## <a name="handling-intermittent-connectivity-between-nodes"></a>Unterbrochene Verbindung zwischen Knoten behandeln

Netzwerkprobleme Störungen VM startet nach Wartungsarbeiten im Rechenzentrum und ähnlichen Ereignissen kann Knoten nicht zugegriffen werden. In diesen Fällen das Ereignis kurzlebig sein ist, der Aufwand für die Splitter Ressourcenausgleich tritt zweimal in schnell hintereinander (wenn einmal der Fehler erkannt wird und wenn der Knoten Master sichtbar) einen erheblichen Mehraufwand die Leistung beeinträchtigt werden. Sie können verhindern, dass temporäre Knoten nicht zugegriffen werden kann verursacht Master neu Cluster durch Festlegen der *verzögert\_Timeout* -Eigenschaft eines Index oder alle Indizes. Im folgenden Beispiel wird die Verzögerung auf 5 Minuten:

```http
PUT /_all/settings
{
    "settings": {
    "index.unassigned.node_left.delayed_timeout": "5m"
    }
}
```

Weitere Informationen finden Sie unter [verzögern Zuweisung ein Knotens verlässt](https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html).

In einem Netzwerk unterbrochen ist, können Sie auch die Parameter ändern, die konfigurieren ein Master-Shape einem anderen Knoten nicht mehr verfügbar ist. Diese Parameter sind Teil der [Zen Discovery](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-zen.html#modules-discovery-zen) Modul mit Elasticsearch und Elasticsearch.yml-Datei festlegen. Der Parameter *discovery.zen.fd.ping.retries* gibt z. B. wie oft ein master-Knoten versucht, Pingen Sie einen anderen Knoten im Cluster vor der Entscheidung, den fehlgeschlagen ist. Dieser Parameter ist 3, aber wie folgt ändern:

```yaml
discovery.zen.fd.ping_retries: 6
```

## <a name="controlling-recovery"></a>Steuernde recovery

Bei Verbindung mit einem Knoten nach einem Ausfall wiederhergestellt wird, müssen Splitter auf diesem Knoten wiederhergestellt werden, um diese zu Stand. Standardmäßig stellt Elasticsearch Splitter in der folgenden Reihenfolge:

- Nach dem Erstellungsdatum reverse-Index. Neue Indizes werden vor älteren Indizes wiederhergestellt.

- Nach dem reverse-Index an. Indizes mit Namen, die alphanumerisch größer als andere zuerst wiederhergestellt werden.

Wenn einige Indizes sind wichtiger als andere Kriterien Sie Indizes Vorrang können durch Festlegen der Eigenschaft *index.priority* nicht übereinstimmen. Indizes mit einem höheren Wert für diese Eigenschaft werden vor Indizes wiederhergestellt, die einen niedrigeren Wert:

```http
PUT low_priority_index
{
    "settings": {
        "index.priority": 1
    }
}

PUT high_priority_index
{
    "settings": {
        "index.priority": 10
    }
}
```

Weitere Informationen finden Sie unter [Index Recovery Priorisierung](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/recovery-prioritization.html#recovery-prioritization).

Den Wiederherstellungsprozess für einen oder mehrere Indizes mit Überwachung der * \_Wiederherstellung* API:

```http
GET /high_priority_index/_recovery?pretty=true
```

Weitere Informationen finden Sie unter [Indizes Recovery](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-recovery.html#indices-recovery).

> [AZURE.NOTE] Ein Cluster mit, die Wiederherstellung erfordern haben den Status *Gelb* an, dass nicht alle Splitter derzeit verfügbar sind. Wird der Splitter, sollte der Cluster-Status auf *Grün*zurückgesetzt. Cluster mit Status *Rot* zeigt an, dass eine oder mehrere Splitter physisch fehlen möglicherweise Daten aus einer Sicherung wiederherstellen.

## <a name="preventing-split-brain"></a>Split Brain verhindern 

Split-Brain kann auftreten, wenn die Verbindungen zwischen Knoten fehl. Wird ein master-Knoten in den Cluster nicht erreichbar, eine Wahl statt im Netzwerksegment erreichbar ist und ein anderer Knoten werden Master. In einem Cluster nicht ordnungsgemäß konfiguriert kann jeder Teil des Clusters zu meistern zu Dateninkonsistenzen oder beschädigt. Dies wird als *Split Brain*bezeichnet.

Reduziert die Wahrscheinlichkeit von Split-Brain durch Konfigurieren der *mindestens\_master\_Knoten* Eigenschaft des Discovery-Moduls in der Datei elasticsearch.yml. Diese Eigenschaft bestimmt, wie viele Knoten zur Wahl eines Masters ermöglichen. Im folgende Beispiel wird den Wert dieser Eigenschaft auf 2:

```yaml
discovery.zen.minimum_master_nodes: 2
```

Dieser Wert niedrigste Großteil der Anzahl von Knoten fest, die die Funktion erfüllen. Wenn Cluster 3 master-Knoten hat beispielsweise *mindestens\_master\_Knoten* auf 2 festgelegt werden. Haben Sie 5 master-Knoten *mindestens\_master\_Knoten* auf 3 festgelegt werden. Im Idealfall sollten Sie eine ungerade Anzahl von master-Knoten verfügen.

> [AZURE.NOTE] Es kann ein Split Brain auftreten, wenn mehrere master-Knoten im selben Cluster gleichzeitig gestartet werden. Dieses Serienelement ist, zwar selten können Sie es verhindern Knoten nacheinander mit einer kurzen Verzögerung (5 Sekunden) gestartet jeweils. 

## <a name="handling-rolling-updates"></a>Behandeln von parallelen updates

Wenn Sie ein Softwareupgrade auf Knoten selbst durchführen (z.B. Migration auf eine neuere Version oder ein Update durchführen), müssen Sie arbeiten auf einzelne Knoten, die muss offline, ohne den Rest des Clusters verfügbar. Sollten Sie in diesem Fall den folgenden Prozess. 

1. Sicherstellen Sie, dass dieser Umverteilung Splitter ausreichend verzögert, um zu verhindern, dass den gewählten Master Ressourcenausgleich Splitter aus einem fehlenden Knoten in den Rest des Clusters. Standardmäßig Umverteilung Splitter 1 Minute verzögert, jedoch können Sie vergrößern die Dauer ein Knotens für einen längeren Zeitraum nicht verfügbar sein wird. Im folgenden Beispiel erhöht die Verzögerung auf 5 Minuten:

    ```http
    PUT /_all/_settings
    {
        "settings": {
            "index.unassigned.node_left.delayed_timeout": "5m"
        }
    }
    ```

    > [AZURE.IMPORTANT] Sie können Umverteilung Splitter auch vollständig deaktivieren, indem Sie *cluster.routing.allocation.enable* des Clusters *keine*. Allerdings sollten Sie mit diesem Ansatz sind neue Indizes, während der Knoten offline ist, dadurch Index Allocation fehlschlägt, was in einem Cluster mit roten Status können geschaffen werden.

2. Beenden Sie Elasticsearch auf dem Knoten verwaltet werden. Wenn Elasticsearch als Dienst ausgeführt wird, den Prozess mithilfe eines Betriebssystembefehls kontrolliert anhalten möglicherweise. Im folgenden Beispiel wird veranschaulicht, wie den Elasticsearch-Dienst auf einem einzigen Knoten unter Ubuntu angehalten:

    ```bash
    service elasticsearch stop
    ```

    Alternativ können Sie die Herunterfahren-API direkt auf dem Knoten:

    ```http
    POST /_cluster/nodes/_local/_shutdown
    ```

3. Wartung auf dem Knoten erforderlich

4. Starten Sie den Knoten neu, und warten sie, dem Cluster beizutreten.

5. Splitter Zuordnung aktivieren:

    ```http
    PUT /_cluster/settings
    {
        "transient": {
            "cluster.routing.allocation.enable": "all"
        }
    }
    ```

> [AZURE.NOTE] Benötigen Sie mehr als einen Knoten erhalten, wiederholen Sie Schritte 2&ndash;4 auf jedem Knoten Splitter Zuordnung wieder aktivieren.

Beenden Sie Sie können neue Daten dabei indizieren. Dadurch die Wiederherstellungszeit minimieren, wenn Knoten wieder online geschaltet, und treten Sie dem Cluster.

Vorsicht vor der automatischen Updates an die JVM (im Idealfall deaktiviert automatische Updates für diese Elemente), besonders wenn Elasticsearch unter Windows ausgeführt. Java Update-Agents automatisch die neueste Version von Java herunterladen kann jedoch erfordern Elasticsearch neu gestartet werden, damit das Update wirksam wird. Dadurch können unkoordiniert Funktionalitätsverlust Knoten je nach der Java-Update-Agent Konfiguration. Dadurch können unterschiedliche Instanzen von Elasticsearch im selben Cluster mit verschiedenen Versionen von JVM die Kompatibilitätsprobleme verursachen.

## <a name="testing-and-analyzing-elasticsearch-resilience-and-recovery"></a>Prüfung und Analyse von Elasticsearch Stabilität und recovery

Dieser Abschnitt beschreibt eine Reihe von Tests, die ausgeführt wurden, um die Stabilität zu bewerten und Recovery ein Elasticsearch Cluster mit drei Daten und drei master-Knoten.

Folgende Szenarien wurden getestet: 

- Der Ausfall eines Knotens und ohne Datenverlust neu starten. Ein Datenknoten beendet und nach 5 Minuten neu gestartet. Elasticsearch wurde nicht fehlen Splitter in diesem Intervall reservieren, damit keine zusätzliche e/a in Splitter verschieben entstanden ist konfiguriert. Beim Neustart des Knotens bringt der Wiederherstellungsvorgang Splittern auf diesem Knoten wieder dem.

- Der Ausfall eines Knotens mit schwerwiegenden Datenverlust. Ein Datenknoten beendet, und es enthält Daten simulieren schwerwiegender Datenträgerfehler gelöscht. Der Knoten wird neu gestartet (nach 5 Minuten), effektiv als Ersatz für den ursprünglichen Knoten. Der Wiederherstellungsprozess muss neu erstellt die fehlenden Daten für diesen Knoten und könnte Splitter am anderen Knoten verschieben.

- Der Ausfall eines Knotens und Neustart ohne Datenverlust ohne Umverteilung Splitter. Ein Datenknoten beendet und Splitter, die er enthält andere Knoten zugewiesen sind. Der Knoten neu gestartet und weitere Reallocation auftritt, um Cluster auszugleichen.

- Parallele Updates. Jeder Knoten im Cluster beendet und nach kurzer simuliert der Computer wird neu gestartet, nachdem ein Update gestartet. Nur ein Knoten ist jederzeit beendet. Splitter sind nicht zugewiesen, wenn ein Knoten ausfällt.

Jedes Szenario wurde die gleiche Arbeitslast einschließlich aus Daten Einnahme Aufgaben, Aggregationen und Filterabfragen Knoten offline und wiederhergestellt wurden. Das gleichzeitige Einfügevorgänge Arbeitslast jeweils 1000 Dokumente gespeichert und für einen Index und die Aggregationen ausgeführt wurden und Filterabfragen verwendet einen Index mit mehreren Millionen Dokumente. Dies war die Leistung von Abfragen von Bulk INSERT separat bewertet werden. Jeder Index enthalten fünf Splitter und ein Replikat.

Die folgenden Abschnitte fassen die Ergebnisse dieser Tests Leistungseinbußen beachten, während ein Knoten offline ist oder wiederhergestellten und Fehler gemeldet wurden. Die Ergebnisse werden grafisch dargestellt Hervorhebung mit einem oder mehreren Knoten fehlt und schätzen die Zeit für das System vollständig wiederherzustellen und eine ähnliche Leistung vor der Knoten offline geschaltet wurde.

> [AZURE.NOTE] Test Kabelbäume zur Durchführung dieser Tests sind online verfügbar. Sie können anpassen und verwenden diese Kabelbäume Stabilität und Wiederherstellung eigener Cluster-Konfigurationen überprüfen. Weitere Informationen finden Sie unter [Automatische Elasticsearch Stabilität Tests ausgeführt][].

## <a name="node-failure-and-restart-with-no-data-loss-results"></a>Der Ausfall eines Knotens und Neustart ohne Datenverlust: Ergebnisse

<!-- TODO; reformat this pdf for display inline --> 

Die Ergebnisse dieser Tests werden in der Datei [ElasticsearchRecoveryScenario1.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario1.pdf)angezeigt. Die Diagramme zeigen Leistungsprofil der Arbeitslast und physische Ressourcen für jeden Knoten im Cluster. Der erste Teil der Diagramme zeigen das System normalerweise für ungefähr 20 Minuten bei Knoten 0 5 Minuten neu gestartet wurde. Statistiken für 20 Minuten dargestellt werden; das System Minuten ungefähr 10 wiederherstellen und stabilisieren. Dies veranschaulicht die durch Transaktionsraten und Reaktionszeiten für unterschiedliche Workloads.

Beachten Sie folgende Punkte:

- Während der Prüfung wurden keine Fehler gemeldet. Kein Datenverlust, und alle Operationen abgeschlossen.

- Die Transaktion für alle drei (Bulk Insert Aggregatabfrage und Filterabfrage) gelöscht und die durchschnittlichen Antwortzeiten erhöht Knoten 0 offline war.

- Vorgänge wurden während der Wiederherstellung, die Transaktionsraten und Reaktionszeiten Aggregatabfrage und Filterabfrage allmählich wiederhergestellt. Die Leistung für Bulk Insert wiederhergestellt kurz bevor verringern. Jedoch wahrscheinlich verursacht Index zur Bulk Insert wachsen Datenmengen und Übertragungsraten für diesen Vorgang lässt sich noch 0 Knoten offline geschaltet wird verlangsamt.

- Das CPU-Auslastungsdiagramm für Knoten 0 zeigt reduzierte Aktivität Phase Recovery dieser durch erhöhte Datenträger und Netzwerkaktivität Wiederherstellungsmechanismus, der Knoten verursacht hat, den Offlinemodus verfehlt Daten und aktualisieren die Splitter enthält.

- Splitter für die Indizes nicht genau verteilt gleichmäßig über alle Knoten. Es gibt zwei Indizes 5 Splitter mit jeweils 1 Replikat insgesamt 20 Splitter. Zwei Knoten enthält daher 6 Splitter und die anderen beiden jeweils 7. Dies zeigt die CPU-Auslastung Diagramme 20-minütigen Zeitraum ist Knoten 0 als die anderen beiden. Nach Abschluss der Wiederherstellung scheint einige switching auftreten wie Knoten 2 Knoten leicht geladen werden.

    
## <a name="node-failure-with-catastrophic-data-loss-results"></a>Der Ausfall eines Knotens mit schwerwiegenden Datenverlust: Ergebnisse

<!-- TODO; reformat this pdf for display inline -->

Die Ergebnisse dieser Tests werden in der Datei [ElasticsearchRecoveryScenario2.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario2.pdf)dargestellt. Wie der erste Teil der Diagramme mit dem ersten Test System normalerweise für ungefähr 20 Minuten zeigt, an welchem Punkt Knoten 0 5 Minuten heruntergefahren wird. Während dieses Intervalls ist Elasticsearch Daten auf diesem Knoten entfernt simulieren schwerwiegenden Datenverlust vor neu gestartet wird. Vollständige Wiederherstellung wird 12 bis 15 Minuten vor der Test gesehen Leistungsniveau wiederhergestellt werden.

Beachten Sie folgende Punkte:

- Während der Prüfung wurden keine Fehler gemeldet. Kein Datenverlust, und alle Operationen abgeschlossen.

- Die Transaktion für alle drei (Bulk Insert Aggregatabfrage und Filterabfrage) gelöscht und die durchschnittlichen Antwortzeiten erhöht Knoten 0 offline war. An diesem Punkt ähnelt das Performance-Profil des Tests das erste Szenario. Dies ist nicht überraschend, darauf, die Szenarien sind identisch.

- Zeitraum Recovery wurden Transaktionsraten und Reaktionszeiten wiederhergestellt zwar während dieser Zeit mehr Volatilität Zahlen. Dies dürfte aufgrund der zusätzlichen Knoten im Cluster ausgeführt werden, die Daten zum Wiederherstellen der fehlenden Splitter. Diese zusätzliche Arbeit zeigt die CPU-Auslastung Datenträgeraktivität und Netzwerk Aktivitätsdiagrammen.

- CPU-Auslastungsdiagramm Knoten 0 und 1 zeigt reduzierte Aktivität Phase Wiederherstellung dieser erhöhten und Netzwerkaktivität durch den Wiederherstellungsprozess verursacht. Im ersten Szenario nur Knoten wiederhergestellt ausgestellt dies jedoch in diesem Szenario ist es wahrscheinlich, dass ein Großteil der fehlende Knoten 0 wird wiederhergestellt von Knoten 1.

- Die e/a-Aktivität für Knoten 0 ist tatsächlich das erste Szenario gegenüber reduziert. Dies könnte durch die i/o-Effizienz einfach kopieren von Daten für eine gesamte Splitter anstelle mehrerer kleiner e/a-Anfragen einer vorhandenen Splitter Stand bringen.

- Die Netzwerkaktivität für alle drei Knoten zeigen Aktivitätsschübe Daten übertragen und empfangen zwischen Knoten. In Szenario 1 schien nur Knoten 0 als viel Netzwerkaktivität, aber diese Aktivität für einen längeren Zeitraum aufrechterhalten werden. Dieser Unterschied möglicherweise erneut aufgrund der Effizienz der Übertragung der gesamten Daten für einen als eine einzelne Anforderung mehrerer kleiner Anfragen beim Wiederherstellen einen.

## <a name="node-failure-and-restart-with-shard-reallocation-results"></a>Der Ausfall eines Knotens und Neustart mit Umverteilung Splitter: Ergebnisse

<!-- TODO; reformat this pdf for display inline -->

Die Datei [ElasticsearchRecoveryScenario3.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario3.pdf) veranschaulicht die Ergebnisse dieses Tests. Mit dem ersten Test der erste Teil der Diagramme zeigen das System normalerweise für ungefähr 20 Minuten, an welchem Punkt Knoten 0 5 Minuten heruntergefahren wird. An dieser Stelle versucht Elasticsearch Cluster fehlt Splitter erstellen und den Splitter auf die verbleibenden Knoten neu. 0 wird nach 5 Minuten Knoten wieder online geschaltet und Cluster hat wieder die Splitter auszugleichen. Leistung wird nach 12-15 Minuten wiederhergestellt.

Beachten Sie folgende Punkte:

- Während der Prüfung wurden keine Fehler gemeldet. Kein Datenverlust, und alle Operationen abgeschlossen.

- Transaktionsraten für alle drei (Bulk Insert, Aggregatabfrage und Filterabfrage) gelöscht und die durchschnittlichen Antwortzeiten stiegen erheblich Knoten 0 offline war im Vergleich zu den vorherigen zwei Tests. Erhöhte Clusteraktivität Wiederherstellung fehlender Splitter und Lastausgleich Cluster wie die erhöhten Zahlen für Datenträger und Netzwerk für Knoten 1 und 2 in dieser Periode liegt.

- Während des Zeitraums nach 0 Knoten wieder online geschaltet bleiben die Transaktionsraten und Reaktionszeiten volatile.

- Die CPU-Nutzung und Datenträger Aktivität Graphen Knoten 0 zeigt sehr reduziert tätig Phase Recovery. Dies ist nun Knoten 0 nicht Daten dient. Nach einem Zeitraum von 5 Minuten bricht der Knoten in Aktion < RBC: mich laut Schnupfen. Nicht komme eine bessere Möglichkeit dazu aber einrichten.  >> wie der plötzliche Anstieg Netzwerk, Festplatte und CPU-Aktivität. Dies wird wahrscheinlich durch Umverteilung Splitter auf Knoten Cluster verursacht. Knoten 0 zeigt dann normalen Aktivität.
  
## <a name="rolling-updates-results"></a>Parallele Updates: Ergebnisse

<!-- TODO; reformat this pdf for display inline -->

Die Ergebnisse dieser Prüfung in der Datei [ElasticsearchRecoveryScenario4.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario4.pdf)anzeigen wie jeder Knoten offline und dann wieder wieder gebracht nacheinander. Jeder Knoten wird 5 Minuten neu gestartet wird heruntergefahren Zeitpunkt der nächste Knoten in Sequenz beendet wird.

Beachten Sie folgende Punkte:

- Während jeder Knoten neu gestartet wird, bleibt die Leistung im Hinblick auf Durchsatz und Antwortzeiten vernünftigerweise selbst.

- Datenträgeraktivität nimmt für jeden Knoten für kurze Zeit wieder online geschaltet wird. Dies ist wahrscheinlich durch den Wiederherstellungsprozess Rollforward Änderungen aufgetreten sind, während der Knoten heruntergefahren war.

- Wenn ein Knoten offline geschaltet wird, treten Aktivitätsspitzen Netzwerk in den verbleibenden Knoten. Spitzen treten beim Neustart eines Knotens.

- Nach der letzte Knoten wiederverwendet wird die Periode erhebliche Volatilität. Dies ist wahrscheinlich durch den Wiederherstellungsprozess zu jedem Knoten ändert synchronisieren sicherzustellen, dass alle Replikate und ihre entsprechenden Splitter. Dieser Aufwand Ursachen aufeinanderfolgenden Bulk irgendwann Einfügevorgänge Timeout und fehl. Fehler wurden jeweils:

```
Failure -- BulkDataInsertTest17(org.apache.jmeter.protocol.java.sampler.JUnitSampler$AnnotatedTestCase): java.lang.AssertionError: failure in bulk execution:
[1]: index [systwo], type [logs], id [AVEg0JwjRKxX_sVoNrte], message [UnavailableShardsException[[systwo][2] Primary shard is not active or isn't assigned to a known node. Timeout: [1m], request: org.elasticsearch.action.bulk.BulkShardRequest@787cc3cd]]

```

Nachfolgende Versuche ergab, dass Einführung in wenigen Minuten zwischen jedem Knoten durchlaufen dieser Fehler beseitigt, so war es wahrscheinlich verursachten Konflikte zwischen den Recovery-Prozess versucht, mehrere Knoten gleichzeitig wiederherstellen und das gleichzeitige Einfügevorgänge Tausende neuer Dokumente speichern möchten.


## <a name="summary"></a>Zusammenfassung

Die ausgeführten Tests angezeigt, die:

- Elasticsearch ist extrem flexibel auf die häufigsten Fehler wahrscheinlich in einem Cluster.

- Elasticsearch kann schnell wiederherstellen, wenn katastrophalem Datenverlust auf einem Knoten ein gut entworfener Cluster unterliegt. Dies ist Elasticsearch zum Speichern von Daten in den temporären Speicher konfigurieren und der Knoten dann nach einem Neustart ausgebessert. Diese Ergebnisse zeigen, dass auch in diesem Fall die flüchtigen Speicher wahrscheinlich sind, die diese Klasse Speicher bietet Vorteile aufgewogen.

- In den ersten drei Szenarien aufgetreten keine Fehler in gleichzeitigen Bulk Insert, Aggregation und Filter Abfrage Arbeitslasten ein Knoten offline und wiederhergestellte aufgenommen wurde.

- Das letzte Szenario angegeben Datenverluste und Verlust beeinflusst nur neue Daten hinzugefügt. Es wird empfohlen, in Clientanwendungen Daten Einnahme ausführen wiederholen Insert-Operationen, die fehlgeschlagen sind, als der Fehler wahrscheinlich vorübergehend ist diese Wahrscheinlichkeit minimieren.

- Die Ergebnisse des letzten Tests zufolge auch beim Ausführen von geplanten Wartung der Knoten in einem Cluster profitieren Leistung, einige Minuten zwischen Knoten sowie die nächste gestatten. In einem ungeplanten (wie das Rechenzentrum recycling Knoten nach dem Ausführen einer Aktualisierung des Betriebssystems) können Sie weniger Steuern wie und wann Knoten heruntergefahren und neu gestartet werden. Der Konflikt tritt auf, wenn Elasticsearch versucht, den Status der Cluster nach Ausfällen sequenzielle Knoten kann Timeouts und Fehlern führen. 

[Die Verfügbarkeit der virtuellen Computer verwalten]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[Ausführen von automatisierten Elasticsearch Stabilität Tests]: guidance-elasticsearch-running-automated-resilience-tests.md
