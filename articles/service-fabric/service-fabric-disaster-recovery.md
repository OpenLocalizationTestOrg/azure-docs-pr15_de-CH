<properties
   pageTitle="Azure Service Fabric Disaster Recovery | Microsoft Azure"
   description="Azure Service Fabric bietet Funktionen müssen mit allen Arten von Katastrophen. Dieser Artikel beschreibt die Typen von Katastrophen auftreten können und wie Sie damit umgehen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Wiederherstellung in Azure Service Fabric

Ein wichtiger Bestandteil der Bereitstellung hoher Verfügbarkeit Cloudanwendung besteht darin, alle Arten von Fehlern überleben, einschließlich derjenigen, die außerhalb Ihrer Kontrolle. Dieser Artikel beschreibt das physische Layout eines Azure Service Fabric-Clusters im Kontext von möglichen Katastrophen und enthält Hinweise zum Umgang mit Katastrophen einschränken oder verhindern Ausfallzeiten oder Datenverluste.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Physisches Layout Service Fabric-Cluster in Azure

Um das Risiko von verschiedenen Arten von Fehlern zu verstehen, ist es nützlich zu wissen, wie Cluster physisch in Azure angeordnet werden.

Wenn Sie in Azure Service Fabric-Cluster erstellen, müssen Sie eine Region auswählen, in dem sie gehostet werden. Die Azure-Infrastruktur stellt dann die Ressourcen für diesen Cluster in der Region, insbesondere die Anzahl der virtuellen Maschinen (VMs) angefordert. Näher am wie und wo die VMs bereitgestellt werden.

### <a name="fault-domains"></a>Fehlerdomänen

Standardmäßig werden die virtuellen Computer im Cluster gleichmäßig in logische Gruppen Fehlerdomänen (FDs) der Computer basierend auf mögliche Fehler in der Server-Hardware segment. Insbesondere wenn zwei VMs in zwei unterschiedlichen FDs befinden, können Sie sicher sein, dass sie den gleichen Quelle oder Netzwerk Netzschalter nicht freigeben. Daher wirkt einem lokalen Netzwerk oder Stromausfall auf einer VM nicht anderen ermöglicht Service Fabric die Auslastung des Computers nicht reagiert innerhalb des Clusters neu.

Über Fehlerdomänen mithilfe der Clusterzuordnung gemäß [Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md)können Sie das Layout des Clusters darstellen:

![Knoten verteilt Fehlerdomänen Service Fabric-Explorer][sfx-cluster-map]

>[AZURE.NOTE] Die Achse in der Cluster-Karte zeigt Upgrade Domänen, die Knoten basierend auf geplante Wartungsaktivitäten logisch zu gruppieren. In Azure Service Fabric-Cluster werden immer über fünf Upgrade Domänen angelegt.

### <a name="geographic-distribution"></a>Geografische Verteilung

Derzeit gibt es [26 Azure Regionen der Welt][azure-regions], mit mehr angekündigt. Ein einzelner Bereich kann je nach Bedarf und Verfügbarkeit von geeigneten Standorten unter anderem eine oder mehrere physische Datenzentren enthalten. Beachten Sie jedoch, dass sogar in Bereiche, die mehrere physische Datenzentren enthalten, keine Garantie besteht, dass virtuelle Computer des Clusters gleichmäßig über die physischen Standorte verteilt. Derzeit werden, alle VMs für einen bestimmten Cluster in einem physischen Standort bereitgestellt.

## <a name="dealing-with-failures"></a>Umgang mit Fehlern

Es gibt verschiedene Arten von Fehlern, die dem Cluster mit eigenen Minderung auswirken können. Wir sehen Sie in der Reihenfolge der Wahrscheinlichkeit auftreten.

### <a name="individual-machine-failures"></a>Einzelne Computer-Fehler

Wie bereits erwähnt, Gefahr einzelne Computer Fehler innerhalb des virtuellen Computers oder in der Hardware oder Software-hosting in eine Fehlerdomäne keine selbst. Service Fabric normalerweise erkennt den Fehler innerhalb von Sekunden und reagieren entsprechend auf den Status des Clusters. Der Knoten der primäre Replikate für eine Partition hostet, wird beispielsweise eine neue primäre von sekundären Replikate der Partition gewählt. Wenn Azure ausgefallenen Computer sichern bringt, es treten Sie dem Cluster automatisch und wieder auf seinen Anteil an der Arbeitslast übernehmen.

### <a name="multiple-concurrent-machine-failures"></a>Mehrere Computer gleichzeitig Fehler

Fehlerdomänen deutlich das Risiko der gleichzeitigen Computer Fehler reduzieren, ist immer das Potenzial für mehrere zufällige Ausfälle auf mehreren Computern in einem Cluster gleichzeitig bringen.

Im Allgemeinen als eine Mehrzahl der Knoten stehen, weiterhin Cluster ausgeführt werden, allerdings bei niedriger Kapazität als statusbehaftete Replikate in einer kleineren Gruppe von Computern verpackt erhalten und weniger statusfreie Instanzen zur verteilen.

#### <a name="quorum-loss"></a>Quorumverlust

Wenn die Mehrheit der Replikationen für statusbehaftete Dienst Partition gehen, trägt dieser Partition Status "quorumverlust" An dieser Stelle beendet Service Fabric ermöglicht Schreibvorgänge auf dieser Partition Zustand konsistent und zuverlässig bleibt. Wählen Wir akzeptieren einen Zeitraum um sicherzustellen, dass Clients nicht gesagt werden, dass ihre Daten gespeichert wurde, wenn es nicht war nicht verfügbar. Beachten Sie, dass wenn Sie entschieden haben, in Lesen vom sekundären Replikate statusbehafteten Diensts zugelassen, Sie können die Lesevorgänge in diesem Zustand ausführen. Eine Partition bleibt Quorum verloren gehen bis eine ausreichende Anzahl von Replikaten zurückkehren oder die Clusterverwaltung System weiter mit der [Reparatur ServiceFabricPartition API]erzwingt[repair-partition-ps].

>[AZURE.WARNING] Während primäre Replikat ist eine Reparatur Aktion führt zu Datenverlust.

Systemdienste können auch Quorum, mit der für den betreffenden Dienst zu Schaden. Beispielsweise wirkt quorumverlust im naming Service Name Resolution, während quorumverlust der Failover-Manager-Dienst neuer Service und Failover blockiert wird. Beachten Sie, dass im Gegensatz zu für Ihre eigenen Dienste versucht, Systemdienste zu reparieren *nicht* empfohlen. Stattdessen empfiehlt es sich einfach warten, bis nach unten Replikate zurück.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>Minimiert das Risiko quorum

Vergrößern Ziel Replica Set für den Dienst, um das Risiko der Quorumressource Verlust zu minimieren. Es wird sich die Anzahl der Replikationen müssen Sie die Anzahl der nicht verfügbaren Knoten, denen Sie tolerieren können nach gleichzeitig verfügbar für Schreibvorgänge, dabei die Anwendung dagegen, oder Cluster-Upgrades können Knoten verfügbar neben Hardwarefehler.

Beispiele vorausgesetzt, Ihre Dienste zu einer MinReplicaSetSize von drei wenige empfohlen für Dienstleistungen konfiguriert haben. Mit einer TargetReplicaSetSize von drei einen und zwei sekundäre ein Hardwarefehler bei einem Upgrade (zwei Replikate unten) wird Quorum verloren gehen und der Dienst werden schreibgeschützt. Alternativ haben Sie fünf Replikate wäre Sie zwei während der Aktualisierung (drei Replikate unten) standhalten, die verbleibenden zwei Replikate weiterhin ein Quorum in mindestens Replikatsatz bilden können.

### <a name="data-center-outages-or-destruction"></a>Data Center Ausfälle oder Zerstörung

In seltenen Fällen werden physische Datenzentren Konnektivitätsverlust Strom- oder vorübergehend nicht verfügbar. In diesen Fällen Ihre Service Fabric-Cluster und ebenfalls nicht verfügbar, aber die Daten erhalten. Für Cluster in Azure ausgeführt, können Updates auf Ausfälle auf der [Statusseite Azure][azure-status-dashboard].

Im unwahrscheinlichen Fall einer gesamten Rechenzentrums zerstört wird verloren alle Service Fabric-Cluster gehostet es, zusammen mit ihren Status.

Zum Schutz gegen diese Möglichkeit ist extrem wichtig, regelmäßig Geo redundanten Speicher [Sichern des Systemstatus](service-fabric-reliable-services-backup-restore.md) und sicherstellen, dass es wiederherzustellen überprüft haben. Wie oft eine Sicherung werden Ihre Recovery Point Objective (RPO) abhängig. Auch wenn Sie nicht vollständig sichern und Wiederherstellen noch implementiert haben, soll, implementieren Sie einen Handler für das `OnDataLoss` Ereignis, damit Sie sich anmelden können, tritt als folgt:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Software-Ausfälle und andere Datenverlust

Als Ursache für Datenverluste sind Codefehler Dienste Betrieb Benutzerfehlern und Sicherheitslücken als verbreitet Center Datenfehler. In allen Fällen die Wiederherstellungsstrategie ist jedoch identisch: Nehmen Sie regelmäßige Sicherungskopien aller statusbehaftete Dienste und Übung zu diesem Zustand wiederherstellen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie verschiedene mit [Prüfbarkeit Framework](service-fabric-testability-overview.md) simulieren
- Lesen Sie andere Ressourcen Disaster Recovery und hohe Verfügbarkeit. Microsoft hat viel Anleitung zu diesen Themen veröffentlicht. Während einige Dokumente auf bestimmte Techniken für die Verwendung in anderen Produkten, enthalten sie viele allgemeine Vorgehensweisen in Service Fabric Zusammenhang tragen:
 - [Verfügbarkeit: Checkliste](../best-practices-availability-checklist.md)
 - [Durchführen einer Disaster Recovery-Bohrung](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Disaster Recovery und hohe Verfügbarkeit für Azure applications][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
