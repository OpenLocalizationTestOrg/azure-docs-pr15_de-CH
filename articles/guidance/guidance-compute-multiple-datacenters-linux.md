<properties
   pageTitle="Unter Linux VMs in mehreren Regionen für hohe Verfügbarkeit | Referenzarchitektur | Microsoft Azure"
   description="Wie VMs in mehreren Regionen für hohe Verfügbarkeit und Stabilität auf Azure bereitgestellt."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-in-multiple-regions-for-high-availability"></a>Ausführen von Linux VMs in mehreren Regionen für hohe Verfügbarkeit

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Ausführen von Linux VMs in mehreren Regionen für hohe Verfügbarkeit](guidance-compute-multiple-datacenters-linux.md)
- [Windows-VMs in mehrere Bereiche für hohe Verfügbarkeit ausgeführt](guidance-compute-multiple-datacenters.md)

In diesem Artikel empfehlen wir eine Reihe von Vorgehensweisen Linux virtuellen Maschinen (VMs) in mehreren Regionen Azure Verfügbarkeit und stabile Disaster Recovery-Infrastruktur ausgeführt.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource groups] und classic. In diesem Artikel verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

Eine Architektur mit mehreren bieten höheren Verfügbarkeit als einen einzelnen Bereich bereitzustellen. Wenn ein regionaler Ausfall die primäre Region betrifft, können [Traffic Manager] [ traffic-manager] Failover auf den sekundären Bereich. Diese Architektur kann auch helfen, schlägt ein einzelner Subsystem der Anwendung.

Es gibt mehrere allgemeine Verfahren zum Erreichen einer hohen Verfügbarkeit Rechenzentren:   
  
- Aktiv/Passiv mit hot Standby. Netzwerkverkehr einen Bereich während der wartet auf Standby. VMs in der sekundären sind jederzeit zugewiesen und ausgeführt.

- Aktiv/Passiv mit kaltem. Die gleiche, jedoch VMs in der sekundären Region werden erst zugeordnet, für Failover erforderlich. Dieser Ansatz günstiger ausgeführt, aber im Allgemeinen mehr Zeit bei einem Ausfall.

- Aktiv/aktiv. Beide Bereiche sind aktiv und Anfragen erfolgt ein Lastenausgleich zwischen ihnen. Wenn ein Rechenzentrum nicht verfügbar ist, wird es aus der Rotation übernommen.

Diese Architektur konzentriert sich auf Aktiv/Passiv mit hot Standby mit Traffic Manager für Failover. Beachten Sie, dass Sie eine kleine Anzahl von VMs für hot Standby bereitstellen und dann Skalierung nach Bedarf.

## <a name="architecture-diagram"></a>Architekturdiagramm

Im folgende Diagramm baut auf der Architektur Zuverlässigkeit [einer N-Tier-Architektur auf Azure hinzufügen](guidance-compute-n-tier-vm-linux.md)angezeigt. 

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm ist "Compute - Seite des Multi-Region (Linux).

![[0]][0]

- **Primäre und sekundäre Bereiche**. Diese Architektur verwendet zwei Bereichen eine höheren Verfügbarkeit. Eine ist der primäre Region. Während des normalen Betriebs wird Netzwerkverkehr an die primäre Region weitergeleitet. Aber wenn, die ausfällt an die sekundäre Region Datenverkehr weitergeleitet.

- ** [Azure Traffic Manager] [ traffic-manager] ** leitet Anfragen an die primäre Region. Wenn dieser Region nicht verfügbar ist, Failover Traffic Manager auf sekundären Region. Weitere Informationen finden Sie im Abschnitt [Konfigurieren von Traffic Manager](#configuring-traffic-manager).

- **Ressourcengruppen**. Erstellen Sie separate [Ressourcengruppen] [ resource groups] für die primäre Region sekundäre Region und für Traffic Manager. Dies gibt Ihnen die Flexibilität, jede Region als eine einzelne Sammlung von Ressourcen verwalten. Sie können z. B. einen Bereich Abbau der erneut bereitstellen. [Verknüpfen Sie die Ressourcengruppen][resource-group-links], Ausführen eine Abfrage, um alle Ressourcen für die Anwendung aufgelistet.

- **VNets**. Erstellen Sie eine separate VNet für jeden Bereich. Stellen Sie sicher, dass die Adressräume nicht überlappen.

- **Apache Cassandra** bereitgestellt Daten Ressourcen Azure Regionen. Cassandra Rechenzentren werden in verschiedenen Azure-Regionen für hohe Verfügbarkeit bereitgestellt. Innerhalb jeder Region Knoten im Rack-fähige Modus mit konfiguriert und Domänen Robustheit innerhalb des Bereichs aktualisiert.

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure-Ressourcen-Manager um die Referenzarchitektur installieren, die diese Empfehlung folgt bereitgestellt. Wenn Sie erstellen eigene Referenzarchitektur folgen diese Empfehlung nur eine Anforderung, die Empfehlung nicht passen

### <a name="regional-pairing"></a>Regionale koppeln

Jede Azure-Region ist eine andere Region innerhalb derselben Region zugeordnet. Im Allgemeinen wählen Sie Bereiche gleichen regionalen Paar (z. B. USA 2 Ost und US Central). Dies bietet folgende Vorteile:

- Ist ein breiter Ausfall, Recovery mindestens eine Region aus jeder erhält.

- Geplante Azure Systemupdates werden paarweise Regionen sequenziell eingeführt Minimierung möglichen Ausfallzeiten.

- Paare befinden sich in demselben geografischen Daten vor-Ort-Anforderungen.

Stellen Sie jedoch sicher, dass beide Bereiche Azure für die Anwendung erforderlichen Dienste unterstützen (siehe [Dienste nach Region][services-by-region]). Weitere Informationen zu regionalen Paare finden Sie unter [Business Continuity und Disaster Recovery (BCDR): Azure kombiniert Bereiche][regional-pairs].

### <a name="traffic-manager-configuration"></a>Traffic Manager-Konfiguration

Folgendes beim Datenverkehr konfigurieren Manager für das Szenario:

- **Routing.** Traffic Manager unterstützt mehrere [Routingalgorithmen][tm-routing]. Verwenden Sie in diesem Artikel beschriebenen Szenario _Priorität_ routing (ehemals _Failover_ routing). Mit dieser Einstellung sendet Traffic Manager alle Anfragen an die primäre Region, wenn die primäre Region nicht erreicht wird. An diesem Punkt wird automatisch über die sekundäre Region. [Routing konfigurieren Failover-Methode]finden Sie unter[tm-configure-failover].

- **Integritätstest.** Traffic Manager verwendet eine HTTP-(oder HTTPS-) [Prüfpunkt] [ tm-monitoring] überwacht die Verfügbarkeit der einzelnen Regionen. Der Prüfpunkt überprüft eine HTTP 200 Antwort für einen angegebenen URL-Pfad. Als bewährte Methode einen Endpunkt, der den Gesamtzustand der Anwendung meldet erstellen und diesen Endpunkt für den Integritätstest verwenden. Andernfalls kann der Prüfpunkt "gesunden" Endpunkt melden wenn wichtige Teile der Anwendung fehlerhaft sind Weitere Informationen finden Sie unter [Health Endpunkt Überwachung Muster][health-endpoint-monitoring-pattern].

Beim Failover Traffic Manager ist eine Zeitspanne, wenn Clients die Anwendung erreichen kann deinstalliert werden. Zwei Faktoren wirken sich auf die gesamte Dauer:

- Der Integritätstest muss erkennen, dass das primäre Rechenzentrum nicht erreichbar ist.

- DNS-Server müssen die zwischengespeicherten DNS-Einträge für die IP-Adresse Aktualisieren der DNS-Time to live (TTL) abhängig. Die Standardgültigkeitsdauer beträgt 300 Sekunden (5 Minuten), aber Sie können diesen Wert beim Erstellen des Profils Traffic Manager.

Näheres [Über Traffic Manager überwachen][tm-monitoring].

Traffic Manager Fehler auftreten, sollten eine manuelle Failback durchführen, sondern automatisch Failback. Überprüfen Sie, ob alle anwendungssubsysteme zuerst fehlerfrei sind. Andernfalls können Sie eine Situation erstellen, wo die Anwendung hin und her zwischen Rechenzentren spiegelt.

Standardmäßig schlägt Traffic Manager automatisch zurück. Um dies zu verhindern, verringern Sie die Priorität der primäre Region manuell nach einem Failover. Beispielsweise die primäre Region Priorität 1 und die sekundäre Priorität 2. Nach einem Failover die primäre Region Priorität festgelegt, 3, um Automatisches Failback zu verhindern. Wenn Sie nicht mehr zurück wechseln möchten, aktualisieren Sie die Priorität 1.

Folgende Azure CLI-Befehl aktualisiert die Priorität:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Anders zu Flipflop ist den Endpunkt vorübergehend deaktivieren:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```    

Je nach Ursache eines Failovers zu Redploy Ressourcen innerhalb eines Bereichs möglicherweise. Führen Sie vor dem Fehler wieder einen Einsatzbereitschaft Test. Der Test sollte beispielsweise:

- VMs sind ordnungsgemäß konfiguriert. (Alle erforderliche Software installiert, IIS ausführen usw.)

- Anwendungssubsysteme sind.

- Funktionstests. (Beispielsweise ist die Datenbankebene der Webebene erreichbar.)

### <a name="cassandra-deployment-across-multiple-regions"></a>Cassandra Bereitstellung in mehreren Regionen

Cassandra Rechenzentren sind Bereiche von Arbeitslasten: eine Gruppe von verwandten Knoten innerhalb eines Clusters für Replikation und Arbeitslast Trennung miteinander konfiguriert sind.

Wir empfehlen [DataStax Enterprise] [ datastax] für die Produktion verwenden. Weitere Informationen zum Ausführen von DataStax in Azure finden Sie [DataStax Enterprise Deployment Guide für Azure][cassandra-in-azure]. Allgemein Folgendes gelten für alle Cassandra Edition.

- Jeder Knoten eine öffentliche IP-Adresse zuweisen. Cluster Regionen mithilfe der Azure-Backbone-Infrastruktur bietet hohen Durchsatz kostengünstig kommunizieren können.

- Sichern Sie Knoten mit den entsprechenden NSG-Konfigurationen Datenverkehr nur bekannten Hosts, einschließlich Kunden und anderen Clusterknoten. Beachten Sie, dass Cassandra verschiedene Ports für die Kommunikation, OpsCenter, Funken usw. verwendet. Anschlussverwendung in Cassandra finden Sie unter [Konfigurieren von Firewall Port][cassandra-ports].

- Verwenden Sie SSL-Verschlüsselung für alle [Client-zu-Knoten-] [ ssl-client-node] und [Knoten -] [ ssl-node-node] Kommunikation.

- Richtlinien Sie in einem Bereich der in [Cassandra Recommendations](guidance-compute-n-tier-vm-linux.md#cassandra-recommendations).

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Eine komplexe N-Tier-App müssen Sie nicht die gesamte Anwendung in der sekundären Region zu replizieren. Stattdessen können Sie nur eine wichtige Subsystem replizieren, die benötigt Business Continuity.

Traffic Manager ist ein möglicher Fehlerpunkt im System. Wenn der Dienst nicht können nicht Clients während der Ausfallzeit die Anwendung zugreifen. Überprüfen Sie die [Traffic Manager SLA][tm-sla], und bestimmen, ob Bedarf für hohe Verfügbarkeit mit Traffic Manager allein erfüllt werden. Falls nicht, anderen Datenverkehr Management-Lösung als ein Failback hinzufügen. Ändern Sie Azure Traffic Manager-Dienst nicht, Ihre CNAME-Einträge in DNS auf der Datenverkehr-Verwaltungsdienst. (Dieser Schritt muss manuell ausgeführt werden und die Anwendung nicht verfügbar, bis die DNS-Änderungen weitergegeben werden.)

Für den Cluster Cassandra hängen die konsistenzebenen der Anwendung sowie die Anzahl der Replikationen verwendet Failover-Szenarien zu berücksichtigen. Konsistenzebenen und Verwendung Cassandra finden Sie unter [Konfigurieren von Datenkonsistenz] [ cassandra-consistency] und [Cassandra: wie viele Knoten mit Quorum gesprochen werden?][cassandra-consistency-usage] Verfügbarkeit in Cassandra bestimmt die konsistenzebene der Anwendung und der Replikationsmechanismus. Replikation in Cassandra finden Sie unter [Datenreplikation im NoSQL Datenbanken erläutert][cassandra-replication].

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

Beim Aktualisieren der Bereitstellung Aktualisieren einer Region zu einem Zeitpunkt eines globalen Fehlers eine falsche Konfiguration oder ein Fehler in der Anwendung zu reduzieren.

Die Stabilität des Systems zu testen. Hier sind einige Ausfallszenarien zu testen:

- VM-Instanzen beenden.

- Ressourcen wie die CPU- und Druck.

- Trennen/Delay-Netzwerk.

- Crash Prozesse.

- Zertifikate ablaufen.

- Simulieren Sie Hardware-Fehlern.

- Fahren Sie den DNS-Dienst auf den Domänencontrollern.

Messen der Recovery-Zeiten und überprüfen sie Ihre Bedürfnisse erfüllen. Fehlermodi sowie Kombinationen zu testen.

## <a name="next-steps"></a>Nächste Schritte

Dieser Serie konzentriert sich auf reine Cloud-Bereitstellungen. Unternehmensszenarien erfordern häufig eine hybride Netzwerk Azure virtuelles Netzwerk mit einem lokalen Netzwerk. Wie diese ein Hybrid-Netzwerks finden Sie unter [Implementieren einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[cassandra-in-azure]: https://academy.datastax.com/resources/deployment-guide-azure
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[cassandra-consistency-usage]: https://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-ports]: http://docs.datastax.com/en/latest-dse/datastax_enterprise/sec/secConfFirePort.html
[datastax]: https://www.datastax.com/products/datastax-enterprise
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssl-client-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLClientToNode_t.html
[ssl-node-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLNodeToNode_t.html
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc-linux.png "Hochverfügbare Netzwerkarchitektur für Azure N-Tier applications"
