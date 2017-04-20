<properties
   pageTitle="Windows-VMs in mehreren Regionen für hohe Verfügbarkeit mit | Referenzarchitektur | Microsoft Azure"
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

# <a name="running-windows-vms-in-multiple-regions-for-high-availability"></a>Windows-VMs in mehrere Bereiche für hohe Verfügbarkeit ausgeführt

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Ausführen von Linux VMs in mehreren Regionen für hohe Verfügbarkeit](guidance-compute-multiple-datacenters-linux.md)
- [Windows-VMs in mehrere Bereiche für hohe Verfügbarkeit ausgeführt](guidance-compute-multiple-datacenters.md)

In diesem Artikel werden Methoden zum Ausführen von Windows virtuelle Maschinen (VMs) in mehreren Regionen Azure Verfügbarkeit und stabile Disaster Recovery-Infrastruktur empfohlen.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource groups] und classic. In diesem Artikel verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

Eine Architektur mit mehreren bieten höheren Verfügbarkeit als einen einzelnen Bereich bereitzustellen. Wenn ein regionaler Ausfall die primäre Region betrifft, können [Traffic Manager] [ traffic-manager] Failover auf den sekundären Bereich. Diese Architektur kann auch helfen, schlägt ein einzelner Subsystem der Anwendung. 

Es gibt mehrere allgemeine Verfahren zum Erreichen einer hohen Verfügbarkeit Rechenzentren:

- Aktiv/Passiv mit hot Standby. Netzwerkverkehr einen Bereich während der wartet auf Standby. VMs in der sekundären sind jederzeit zugewiesen und ausgeführt.

- Aktiv/Passiv mit kaltem. Die gleiche, jedoch VMs in der sekundären Region werden erst zugeordnet, für Failover erforderlich. Dieser Ansatz günstiger ausgeführt, aber im Allgemeinen mehr Zeit bei einem Ausfall.

- Aktiv/aktiv. Beide Bereiche sind aktiv und Anfragen erfolgt ein Lastenausgleich zwischen ihnen. Wenn ein Rechenzentrum nicht verfügbar ist, wird es aus der Rotation übernommen. 

Diese Architektur konzentriert sich auf Aktiv/Passiv mit hot Standby mit Traffic Manager für Failover. Beachten Sie, dass Sie eine kleine Anzahl von VMs für hot Standby bereitstellen und dann Skalierung nach Bedarf.

## <a name="architecture-diagram"></a>Architekturdiagramm

Im folgende Diagramm baut auf der Architektur Zuverlässigkeit [einer N-Tier-Architektur auf Azure hinzufügen](guidance-compute-n-tier-vm.md)angezeigt.

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm ist "Compute - Multi-Bereich (Windows)-Seite.

[! [0]][0]

- **Primäre und sekundäre Bereiche**. Diese Architektur verwendet zwei Bereichen eine höheren Verfügbarkeit. Eine ist der primäre Region. Während des normalen Betriebs wird Netzwerkverkehr an die primäre Region weitergeleitet. Aber wenn, die ausfällt an die sekundäre Region Datenverkehr weitergeleitet.

- ** [Azure Traffic Manager] [ traffic-manager] ** leitet Anfragen an die primäre Region. Wenn dieser Region nicht verfügbar ist, Failover Traffic Manager auf sekundären Region. Weitere Informationen finden Sie im Abschnitt [Konfigurieren von Traffic Manager](#configuring-traffic-manager).

- **Ressourcengruppen**. Erstellen Sie separate [Ressourcengruppen] [ resource groups] für die primäre Region sekundäre Region und für Traffic Manager. Dies gibt Ihnen die Flexibilität, jede Region als eine einzelne Sammlung von Ressourcen verwalten. Sie können z. B. einen Bereich Abbau der erneut bereitstellen. [Verknüpfen Sie die Ressourcengruppen][resource-group-links], Ausführen eine Abfrage, um alle Ressourcen für die Anwendung aufgelistet.

- **VNets**. Erstellen Sie eine separate VNet für jeden Bereich. Stellen Sie sicher, dass die Adressräume nicht überlappen. 

- **SQL Server immer auf Availability Group**. Wenn Sie SQL Server verwenden, sollten Sie [SQL immer auf Verfügbarkeit Gruppen] [ sql-always-on] für hohe Verfügbarkeit. Erstellen einer einzelnen Verfügbarkeit, die SQL Server-Instanzen in beiden Regionen enthält. Weitere Informationen finden Sie im Abschnitt [konfigurieren die Gruppe SQL Server immer auf Verfügbarkeit](#configuring-the-sql-server-alwayson-availability-group ).

> [AZURE.NOTE] Berücksichtigen Sie [Azure SQL-Datenbank][azure-sql-db], die eine relationale Datenbank als Cloud-Dienst bereitstellt. Mit SQL-Datenbank müssen Sie eine Gruppe Verfügbarkeit konfigurieren oder verwalten Failover.  

- **VPN-Gateways**: Erstellen Sie ein [VPN-Gateway] [ vpn-gateway] in jeder VNet und Konfigurieren einer [Verbindung VNet VNet][vnet-to-vnet], Netzwerkverkehr zwischen zwei VNets. Dies ist erforderlich für die Availability Group SQL ständig.

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

### <a name="sql-server-always-on-configuration"></a>SQL Server immer auf Konfiguration

SQL Server immer auf erforderlich Verfügbarkeit einen Domänencontroller. Alle Knoten in der verfügbarkeitsgruppe muss sich in derselben Active Directory-Domäne. Die folgenden Punkte Unterstützung wie eine Gruppe SQL Server immer auf Verfügbarkeit konfigurieren:

- Platzieren Sie mindestens zwei Domänencontroller in jeder Region. 

- Geben Sie jedem Domänencontroller eine statische IP-Adresse.

- Erstellen einer Kommunikation zwischen dem VNets VNet VNet Verbindungs.

- Fügen Sie für jede VNet IP-Adressen der Domänencontroller (beide Bereiche), die DNS-Serverliste. Sie können den folgenden CLI-Befehl. Weitere Informationen finden Sie unter [Verwalten von DNS-Server von einem virtuellen Netzwerk (VNet)][vnet-dns].

```bat
azure network vnet set --resource-group dc01-rg --name dc01-vnet --dns-servers "10.0.0.4,10.0.0.6,172.16.0.4,172.16.0.6"
```

- Erstellen Sie eine [Windows Server Failover Clustering] [ wsfc] (WSFC)-Cluster, die SQL Server-Instanzen in beiden Regionen enthält. 

- Erstellen einer SQL Server immer auf Verfügbarkeit, die SQL Server-Instanzen in den primären und sekundären Regionen enthält. Die Schritte finden Sie unter [Erweitern immer auf Availability Group, Remote Azure Rechenzentren (PowerShell)](https://blogs.msdn.microsoft.com/sqlcat/2014/09/22/extending-alwayson-availability-group-to-remote-azure-datacenter-powershell/) . 

- Legen Sie im Bereich für primäre primäre Replikat.

- Replikationen Sie eine oder mehrere sekundäre primäre Region. Konfigurieren Sie diese Commit synchron mit automatischem Failover verwenden.

- Replikationen Sie eine oder mehrere sekundäre im zweiten Bereich. Konfigurieren Sie diesen *asynchronen* Commit aus Leistungsgründen verwenden. (Andernfalls müssen alle SQL-Transaktionen über das Netzwerk auf den zweiten Bereich in einer Schleife warten.) 

> [AZURE.NOTE] Automatisches Failover unterstützt Replikate der asynchronen Commit nicht. 

Weitere Informationen finden Sie unter [Windows-VMs für N-Tier-Architektur in Azure ausgeführt](guidance-compute-n-tier-vm.md#SQL-AlwaysOn-Availability-Group).

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Eine komplexe N-Tier-App müssen Sie nicht die gesamte Anwendung in der sekundären Region zu replizieren. Stattdessen können Sie nur eine wichtige Subsystem replizieren, die benötigt Business Continuity.

Traffic Manager ist ein möglicher Fehlerpunkt im System. Wenn der Dienst nicht können nicht Clients während der Ausfallzeit die Anwendung zugreifen. Überprüfen Sie die [Traffic Manager SLA][tm-sla], und bestimmen, ob Bedarf für hohe Verfügbarkeit mit Traffic Manager allein erfüllt werden. Falls nicht, anderen Datenverkehr Management-Lösung als ein Failback hinzufügen. Ändern Sie Azure Traffic Manager-Dienst nicht, Ihre CNAME-Einträge in DNS auf der Datenverkehr-Verwaltungsdienst. (Dieser Schritt muss manuell ausgeführt werden und die Anwendung nicht verfügbar, bis die DNS-Änderungen weitergegeben werden.) 

Für den SQL Server-Cluster sind zwei Failover-Szenarien zu berücksichtigen:

1. Die SQL-Replikate im Bereich für primäre fehl. Beispielsweise könnte bei einem regionalen Ausfall dies. In diesem Fall müssen Sie manuell Gruppe Verfügbarkeit SQL fehl, obwohl Traffic Manager automatisch auf den front-End Failover. Führen Sie die Schritte [Ausführen ein gezwungen manuellen Failover für einen SQL Server Availability Group](https://msdn.microsoft.com/library/ff877957.aspx)beschreibt einen erzwungene Failover mit SQL Server Management Studio, Transact-SQL oder PowerShell SQL Server 2016 durchführen. 

    > [AZURE.WARNING] Mit erzwungener Failover besteht ein Datenverlust. Wenn die primäre Region wieder online ist, eine Momentaufnahme der Datenbank und [Tablediff] Unterschiede zu verwenden.

2. Traffic Manager Failover auf den sekundären Bereich primäre SQL-Replikat ist jedoch noch verfügbar. Beispielsweise kann die Front-End-Ebene fehlgeschlagen ohne SQL-VMs. In diesem Fall Internet-Datenverkehr an die sekundäre Region weitergeleitet und Region noch eine Verbindung zum primären SQL-Replikat. Allerdings werden Latenzzeit, denn die SQL-Verbindungen Regionen. In diesem Fall sollten Sie ein manuelles Failover durchführen: 

    - Vorübergehend wechseln Sie ein SQL-Replikat in der sekundären Region auf *synchrone* commit Dadurch werden nicht Datenverlust während des Failovers.

    - Ein Failover zu diesem SQL-Replikat. 
    
    - Wenn nicht um primäre Region wiederherstellen Sie asynchronen Commit-Einstellung. 

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
[azure-sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[sql-always-on]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[TableDiff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
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
[0]: ./media/blueprints/compute-multi-dc.png "Hochverfügbare Netzwerkarchitektur für Azure N-Tier applications"