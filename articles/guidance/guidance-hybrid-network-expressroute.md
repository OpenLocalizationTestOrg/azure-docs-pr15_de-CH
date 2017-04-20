<properties
   pageTitle="Implementieren einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute | Referenzarchitektur | Microsoft Azure"
   description="Wie implementieren eine sicheren Netzwerkarchitektur von Standort zu Standort umfasst Azure virtual Network und Azure ExpressRoute über ein lokales Netzwerk verbunden."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Implementieren einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt best Practices für virtuelle Netzwerke auf Azure ExpressRoute mit einem lokalen Netzwerk herstellen. ExpressRoute-Verbindung erfolgt über eine dedizierte private Verbindung von einem Drittanbieter-Konnektivität. Die private Verbindung erweitert lokalen Netzwerk in Azure Zugriff auf Ihre Infrastruktur IaaS in Azure, öffentliche Endpunkte PaaS-Dienste und Office365 SaaS-Dienste verwendet. Dieses Dokument befasst sich mit ExpressRoute eine Verbindung zu einem einzelnen Azure virtuellen Netzwerk (VNet) mit privaten peering nennt.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. Dieser Plan verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

Normale Anwendungsfälle für diese Architektur gehören:

- Hybrid Applications, Arbeitslasten zwischen einem lokalen Netzwerk und Azure verteilt werden.

- Applikationen große, geschäftskritischen Arbeitslasten, die hohe Skalierbarkeit erfordern.

- Sicherung und Wiederherstellung Großgeräte Daten extern gespeichert werden müssen.

- Behandeln von Big Data Arbeitslasten.

- Verwenden von Azure als Disaster Recovery-Standort.

> [AZURE.NOTE] [Technische Übersicht über ExpressRoute] [ expressroute-technical-overview] ExpressRoute vorgestellt.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm zeigt wichtigen Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm wird auf der Seite "Hybrid network - ER".

![[0]][0]

- **Azure virtuelle Netzwerke (VNets).** Jedes VNet befindet sich in einem einzigen Azure Bereich und mehrere Anwendungsebenen hosten kann. Anwendungsebenen mit Subnetzen in jeder VNet segmentiert werden bzw. Sicherheitsgruppen (NSGs). 

- **Azure öffentliche Dienste.** Dies sind Azure Dienste in eine hybridanwendung genutzt werden können. Diese Services stehen auch über das Internet, aber Zugriff über ExpressRoute-Verbindung ermöglicht niedrige Latenz und vorhersagbare Leistung da Datenverkehr nicht über das Internet geht. Anschlüsse sind Adressen, die im Besitz der Organisation oder vom Provider Konnektivität mit **öffentlichen peering**, durchgeführt. 

- **Office 365-Dienste.** Dies sind öffentlich zugängliche Office 365-Applikationen und von Microsoft. Anschlüsse werden mit **Microsoft peering**, Adressen, die im Besitz der Organisation oder der Konnektivität Anbieters ausgeführt.

>[AZURE.NOTE] Sie können auch direkt zu Microsoft CRM Online durch Microsoft peering verbinden.

- **Lokalen Unternehmensnetzwerk.** Dies ist ein Netzwerk von Computer und Geräte über ein privates LAN-Netzwerk in einer Organisation.

- **Lokalen Edge-Router.** Router, die vom Anbieter verwaltet Circuit im lokalen Netzwerk verbunden, sind. Je nachdem, wie die Verbindung bereitgestellt ist müssen Sie die öffentlichen IP-Adressen der Router. 

- **Microsoft Edge-Router.** Dies sind zwei Router in einer Aktiv / Aktiv-Konfiguration mit hoher Verfügbarkeit. Diese Router ermöglichen einen konnektivitätsanbieter ihre Schaltkreise direkt Rechenzentrum herstellen. Je nachdem, wie die Verbindung bereitgestellt ist müssen Sie die öffentlichen IP-Adressen der Router.

- **ExpressRoute-Verbindung.** Dies ist eine Schicht 2 oder Layer 3 Circuit angegeben vom konnektivitätsanbieter, Joins lokalen Netzwerk mit Azure über Edge-Router. Die Schaltung verwendet die Hardwareinfrastruktur konnektivitätsanbieter verwaltet.

- **Konnektivitätsanbieter.** Sind Unternehmen, die einer Verbindung mit Layer 2 und Layer 3-Konnektivität zwischen Rechenzentrum und Azure-Rechenzentrum.

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure-Ressourcen-Manager um die Referenzarchitektur installieren, die diese Empfehlung folgt bereitgestellt. Wenn Sie eigene Referenzarchitektur erstellen diese Empfehlung Sie befolgen nur eine bestimmte Anforderung, die Empfehlung nicht passen.

### <a name="connectivity-providers"></a>Konnektivitätsanbieter

Geeigneten ExpressRoute konnektivitätsanbieter für Ihren Standort auswählen. Verwenden Sie zum Abrufen einer Liste von an Ihrem Standort verfügbaren konnektivitätsanbieter Azure PowerShell Folgendes:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

ExpressRoute verbunden Verbindung Rechenzentrum an Microsoft folgendermaßen:

- **Gemeinsam am Cloud Exchange.** Wenn Sie gemeinsam in einer Einrichtung mit einer Cloud befinden, können Sie virtuelle Querverbindungen Microsoft Cloud über die Co-Location-Anbieter Ethernet-Exchange bestellen. Gemeinsam können Layer 2 Querverbindungen oder verwaltete Layer 3 Querverbindungen zwischen Ihrer Infrastruktur in der Co-Location und Microsoft Cloud Anbieter...

- **Punkt-Ethernet-Anschlüsse.** Sie können Ihre lokalen Datencenter stellen Microsoft Cloud über Point-Ethernet-Links verbinden. Point-Ethernet Layer 2-Verbindungen bieten oder verwaltete Layer 3-Verbindungen zwischen Ihrer Website und die Microsoft-Cloud.

- **Any-to-any (IPVPN)-Netzwerke.** Sie können Ihrem WAN mit Cloud von Microsoft integrieren. IPVPN Anbieter (in der Regel MPLS VPN) n: n-Konnektivität zwischen Zweigstellen und Rechenzentren. Microsoft-Cloud kann Ihre WAN zu genau wie andere Zweigstelle verbunden werden. WAN-Anbieter normalerweise verwaltete Layer 3-Konnektivität.

Weitere Informationen zu Konnektivität Anbietern finden Sie unter [ExpressRoute-Schaltkreise und routing-Domänen][connectivity-providers].

### <a name="expressroute-circuit"></a>ExpressRoute-Verbindung

Stellen Sie sicher, dass Ihre Organisation [ExpressRoute Grundvoraussetzungen] erfüllt[ expressroute-prereqs] für die Verbindung mit Azure.

Wenn Sie dies nicht bereits getan haben, fügen Sie ein Subnetz mit dem Namen `GatewaySubnet` auf der Azure-VNet und ein ExpressRoute virtuelles Netzwerk-Gateway mit dem Azure VPN-Gateway-Dienst erstellen. Weitere Informationen zu diesem Vorgang finden Sie unter [ExpressRoute Workflows Circuit provisioning und Circuit Mitgliedstaaten][ExpressRoute-provisioning].

Erstellen Sie eine ExpressRoute-Verbindung:

1. Führen Sie den folgenden PowerShell-Befehl:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Senden der `ServiceKey` für neue Verbindung zum Dienstanbieter.

3. Warten der Anbieter der Verbindung bereitstellen. Sie können den Bereitstellungsstatus eines mit dem folgenden PowerShell-Befehl überprüfen:

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

Die `Provisioning state` Feld der `Service Provider` Abschnitt der Ausgabe ändert sich von `NotProvisioned` , `Provisioned` Wenn der Circuit bereit ist.

>[AZURE.NOTE]Wenn eine Layer 3-Verbindung verwenden, sollte der Anbieter konfigurieren und verwalten Sie routing; zum Aktivieren des Anbieters implementiert die entsprechenden Routen erforderlichen Informationen bereitstellen.

Wenn Sie eine Layer 2-Verbindung verwenden, wie folgt:

1. Reservieren Sie zwei/30 Subnetzen bestehen gültige öffentliche IP-Adressen für jeden Typ von peering implementieren möchten. Diese/30 Subnetzen zu IP-Adressen für die Verbindung zum Router verwendet. Wenn Sie Private, öffentliche und Microsoft peering implementieren, benötigen Sie /30 6 Subnetze gültige öffentliche IP-Adressen.     

2. Konfigurieren Sie routing für ExpressRoute-Verbindung. Sie müssen den Befehl unten für jeden der peering (Private, Public und Microsoft) konfiguriert werden soll.

    >[AZURE.NOTE]Siehe [Erstellen und Ändern von routing für eine ExpressRoute-Verbindung] [ configure-expressroute-routing] Informationen. Verwenden Sie die folgenden PowerShell Befehle Datenverkehr peering Netzwerk hinzufügen:

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Reservieren Sie ein anderer Pool von gültigen IP-Adressbereich für NAT für öffentliche und Microsoft peering. Es wird empfohlen, einen anderen Pool für jede peering. Geben Sie den Pool Dienstanbieter Konnektivität sie BGP Werbung für diese Bereiche Konfiguration an

[Verknüpfen Sie Ihre private VNet(s) in der Cloud mit ExpressRoute-Verbindung][link-vnet-to-expressroute]. Verwenden Sie die folgenden PowerShell-Befehlen:

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Beachten Sie folgende Punkte:

- ExpressRoute verwendet Border Gateway Protocol (BGP) für den Austausch von Routinginformationen zwischen dem Netzwerk und Azure.

- Sie können mehrere VNets in verschiedenen Regionen ExpressRoute Stromkreis befindet wie alle VNets und der ExpressRoute im gleichen geopolitischen Regionen befinden.

## <a name="scalability-considerations"></a>Aspekte der Skalierung

ExpressRoute Stromkreise bieten einen hohe Bandbreite Pfad zwischen Netzwerken. In der Regel die höhere Bandbreite der höhere Kosten verursachen. Obwohl einige Anbieter können Sie Ihre Bandbreite, ändern Sie eine anfängliche Bandbreite auswählen, die Ihren Bedürfnissen übertrifft und bietet Platz für Wachstum. Falls Sie in Zukunft Bandbreite erhöhen müssen, verbleiben Sie mit zwei Optionen.

Erhöhen der Bandbreite. Beachten Sie, dass nicht alle Anbieter, die dynamisch ermöglichen. Und zu diesem möglichst vermeiden. Aber gegebenenfalls nach Rücksprache mit Ihrem Anbieter die folgenden Befehle.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Sie können die Bandbreite ohne Verlust der Konnektivität. Herabstufung der Bandbreite führt Unterbrechung der Verbindung. Sie müssen die Verbindung löschen und mit der neuen Konfiguration neu erstellen.

Verwenden Sie die Standard-SKU ExpressRoute und müssen auf Premium aktualisieren oder ändern Sie Ihre Preise sind planen (gemessenen oder unbegrenzt), führen Sie die folgenden Befehle. Die `Sku.Tier` -Eigenschaft kann `Standard` oder `Premium`; die `Sku.Name` -Eigenschaft kann `MeteredData` oder `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Stellen Sie sicher, dass die `Sku.Name` -Eigenschaft entspricht die `Sku.Tier` und `Sku.Family`. Wenn Sie Familie und Ebene jedoch nicht den Namen ändern, wird die Verbindung deaktiviert.

Die SKU unterbrechungsfrei aktualisieren können, Sie aber von unbegrenzte Zahlungsplan gemessen. Beim Herunterstufen SKU müssen der Bandbreitenverbrauch Standardlimit von standard SKU

> [AZURE.NOTE] ExpressRoute bietet zwei Tarife Kunden metering unbegrenzte Daten. Siehe [Preise ExpressRoute] [ expressroute-pricing] Weitere Informationen. Gebühren variieren je nach Bandbreite der Verbindung. Bandbreite wird wahrscheinlich von Anbieter zu Anbieter variieren. Verwenden der `Get-AzureRmExpressRouteServiceProvider` Cmdlet an die Anbieter in Ihrer Region und Bandbreiten, die sie anbieten.

Eine ExpressRoute-Verbindung unterstützen eine Reihe von Peerings und VNet Links. Siehe [ExpressRoute Grenzen] [ expressroute-limits] Weitere Informationen.

Gegen Aufpreis bietet ExpressRoute Premium Add-on:

- Bis zu 10.000 Routen pro Verbindung. Der Grenzwert ist ohne ExpressRoute Premium Add-on 4.000 Routen pro Verbindung.

- Globale Konnektivität ermöglicht eine ExpressRoute-Verbindung an den Zugriff auf Ressourcen in jeder Region als nur Bereiche in demselben Kontinent der Welt.

- Erhöhung der Anzahl der Links pro Verbindung von 10 auf einen höheren Wert je nach Bandbreite der Verbindung VNet.

ExpressRoute-Schaltkreise dienen zu temporäres Netzwerk Bursts bis zu zweimal die Bandbreite, die ohne zusätzliche Kosten beschafft. Dies geschieht über redundante Links. Nicht alle konnektivitätsanbieter jedoch unterstützt dieses Feature; Überprüfen Sie, ob Dienstanbieter Konnektivität dieser bevor dies ermöglicht.

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Konfigurieren hohen Verfügbarkeit für Ihre Azure Verbindung unterschiedlich je nach Anbieter die Verwendung und die Anzahl ExpressRoute Stromkreise und virtuelles Netzwerk-Gateway, das Sie konfigurieren möchten. Die folgenden Punkte fassen die Verfügbarkeitsoptionen:

- ExpressRoute unterstützt keine Redundanz Routerprotokolle wie HSRP VRRP hohen Verfügbarkeit zu implementieren. Stattdessen verwendet er eine redundante BGP Sitzungsanzahl pro peering. Um Hochverfügbarkeits-Verbindungen zu Ihrem Netzwerk zu erleichtern, stellt Microsoft Azure Sie mit zwei redundante Anschlüsse auf beiden Routern (Teil der Microsoft Edge) in einer Aktiv / Aktiv-Konfiguration.

- Wenn eine Layer-2-Verbindung verwenden, Bereitstellen Sie redundante Router im lokalen Netzwerk in einer Aktiv / Aktiv-Konfiguration. Primärkreislaufs einen Router und sekundäre Verbindung zueinander herstellen. Dadurch erhalten Sie eine hochverfügbare Verbindung an beiden Enden der Verbindung. Dies ist notwendig, die ExpressRoute SLA benötigen. Siehe [SLA für Azure ExpressRoute] [ sla-for-expressroute] Weitere Informationen.

Das folgende Diagramm zeigt eine Konfiguration mit redundanten lokalen Router angeschlossen an den primären und sekundären. Jede Verbindung verarbeitet den Datenverkehr für Öffentliche peering und private peering (jede peering bezeichnet ein /30 Adressräume, wie im vorherigen Abschnitt beschrieben).

![[1]][1]

Wenn eine Layer 3-Verbindung verwenden, überprüfen Sie redundante BGP bereit Sessions, die Verfügbarkeit behandelt.

Virtuelle Netzwerke an mehrere ExpressRoute Stromkreise angeschlossen werden können und jeder Stromkreise von verschiedenen Dienstanbietern bereitgestellt werden. Diese Strategie bietet zusätzliche Hochverfügbarkeits- und Disaster Recovery-Funktionen.

Konfigurieren Sie eine Standort-zu-Standort-VPN als Failover-Pfad für ExpressRoute. Dies gilt nur für private peering. Azure und Office 365 ist das Internet nur Failover-Pfad.

Standardmäßig verwenden BGP-Sitzung im Leerlauf Timeoutwert von 60 Sekunden. Sobald eine Sitzung 3 Mal überschritten ist, die Route als nicht verfügbar markiert und den gesamten verbleibenden Router weitergeleitet. Dieses Zeitlimit 180 Sekunden möglicherweise zu lang für kritische Applikationen. In diesem Fall können Sie Ihre BGP-Timeouteinstellungen auf dem lokalen Router auf einen kleineren Wert ändern.

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

[Azure Konnektivität Toolkit (AzureCT)] können[ azurect] Konnektivität zwischen lokalen Datencenter und Azure überwachen.

## <a name="security-considerations"></a>Sicherheitsaspekte

Sie können Sicherheitsoptionen für die Azure Verbindung unterschiedlich je nach Sicherheit und Einhaltung konfigurieren. Die folgenden Punkte fassen die Sicherheitsoptionen.

ExpressRoute arbeitet in Schicht 3. Risiken in der Anwendungsschicht können verhindert werden, mit einer Network Security Appliance das Datenverkehr auf legitime Ressourcen beschränkt. Darüber hinaus können ExpressRoute Verbindung über öffentliche peering nur lokal initiiert werden. Dies verhindert einen Rogue-Dienst zugreifen und lokalen Daten vom öffentlichen Internet gefährden.

Fügen Sie zur Optimierung der Sicherheit Netzwerksicherheitsgeräte zwischen dem lokalen Netzwerk und Provider-Edge-Router. Dadurch wird den Zufluss von nicht autorisiertem Datenverkehr von der VNet einschränken:

![[2]][2]

Überwachen und Compliance Zwecken ggf. Komponenten in den VNet mit dem Internet direkten Zugriff verhindern und [Erzwungene Tunnel]implementieren[forced-tuneling]. In diesem Fall sollte Internetdatenverkehr über einen Proxy lokal ausgeführt, wo es überwacht werden umgeleitet. Der Proxy kann blockieren unbefugten Datenverkehr fließt und potenziell bösartigen Datenverkehr filtern konfiguriert werden.

![[3]][3]

Standardmäßig setzen Azure VMs Endpunkte für Login-Zugang für Verwaltungszwecke - RDP und Remote Powershell Windows VMs und SSH für Linux-basierte VMs bei Azure Portal verwendet. Zur Optimierung der Sicherheit nicht aktivieren Sie öffentliche IP-Adresse für Ihre virtuellen Computer und verwenden Sie NSGs, um sicherzustellen, dass diese virtuellen Computer nicht zugänglich sind. VMs sollten nur die interne IP-Adresse zur Verfügung. Diese Adressen können über ExpressRoute Netzwerk aktivieren vor Ort DevOps Mitarbeiter alle erforderlichen Konfiguration oder Wartung zugänglich gemacht werden.

Wenn Sie Endpunkte Management für VMs mit einem externen Netzwerk verfügbar machen müssen, verwenden Sie NSGs oder Zugriffssteuerungslisten Sie beschränkt die Sichtbarkeit dieser Ports White-Liste von IP-Adressen oder Netzwerken.

## <a name="troubleshooting-considerations"></a>Aspekte zur Problembehandlung

Schlägt eine zuvor funktionierende ExpressRoute-Verbindung jetzt herstellen, ohne jede Konfiguration ändert sich lokal oder in Ihrem privaten VNet müssen Sie wenden Sie sich an die Verbindung und sie das Problem beheben. Außerdem können die folgenden Azure PowerShell Befehle begrenzt überprüfen und bestimmen, wo die Probleme liegen können:

- Überprüfen der Verbindung bereitgestellt wurde:

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Die Ausgabe dieses Befehls zeigt verschiedene Eigenschaften für die Verbindung mit `ProvisioningState`, `CircuitProvisioningState`, und `ServiceProviderProvisioningState` wie unten dargestellt.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Wenn die `ProvisioningState` ist nicht festgelegt, `Succeeded` Nachdem Sie eine neue Verbindung erstellen möchten, entfernen Sie die Verbindung mithilfe des folgenden Befehls und versuchen Sie es erneut erstellen.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Dienstanbieter die Verbindung bereits bereitgestellt haben und die `ProvisioningState` wird `Failed`, oder `CircuitProvisioningState` ist `Enabled`, wenden Sie sich an Ihren Anbieter für weitere Unterstützung.

## <a name="solution-deployment"></a>Bereitstellung der Lösung

Wenn Sie eine vorhandene lokale Infrastruktur mit geeigneten Network Appliance bereits konfiguriert haben, können Sie die Referenzarchitektur folgendermaßen bereitstellen:

Wenn Sie eine vorhandene lokale Infrastruktur bereits ein VPN-Gerät konfiguriert haben, können Sie Referenzarchitektur folgendermaßen bereitstellen:

1. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Warten auf die Verknüpfung zum Azure-Portal öffnen, und gehen Sie folgendermaßen vor: 
  - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-hybrid-er-rg` im Textfeld.
  - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
  - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie, bis die Bereitstellung abgeschlossen.

4. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Warten auf die Verknüpfung zum Azure-Portal öffnen, und gehen Sie folgendermaßen vor: 
  - Wählen Sie im Bereich **Ressourcengruppe** **bestehende verwenden** , und geben Sie `ra-hybrid-er-rg` im Textfeld.
  - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
  - Klicken Sie auf die Schaltfläche **Einkauf** .

6. Warten Sie, bis die Bereitstellung abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

- Finden Sie unter [Implementieren einer hochverfügbaren Hybrid-Netzwerkarchitektur] [ highly-available-network-architecture] Informationen zur Erhöhung der Verfügbarkeit eines Netzwerks Hybrid basierend auf ExpressRoute nicht über eine VPN-Verbindung.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Hybrid-Netzwerkarchitektur Azure ExpressRoute verwenden"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Redundante Router verwenden mit primären und sekundären ExpressRoute"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Sicherheitsgeräte mit dem lokalen Netzwerk hinzufügen"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Mit Erzwungene Tunnel zum Internet gerichtete Datenverkehr überwachen"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "Suchen von ServiceKey ExpressRoute-Verbindung"
