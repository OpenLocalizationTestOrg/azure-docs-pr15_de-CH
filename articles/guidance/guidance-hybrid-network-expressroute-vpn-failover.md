<properties
   pageTitle="Implementierung einer hochverfügbaren Hybrid-Netzwerkarchitektur | Microsoft Azure"
   description="Wie implementieren eine sicheren Netzwerkarchitektur von Standort zu Standort umfasst ein Azure virtuelles Netzwerk und einem lokalen Netzwerk verbunden mit ExpressRoute VPN-Gateway Failover."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Implementierung einer hochverfügbaren Hybrid-Netzwerkarchitektur

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Best Practices für virtuelle Netzwerke auf Azure ExpressRoute, eine Standort-zu-Standort-VPN (VPN) als Failover-Verbindung mit einem lokalen Netzwerk mit beschrieben. Der Datenfluss zwischen dem lokalen Netzwerk und Azure virtual Network (VNet) über eine ExpressRoute-Verbindung.  Ist ein Verlust der Konnektivität in die ExpressRoute-Verbindung, wird Datenverkehr durch einen Tunnel IPSec VPN weitergeleitet.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. Dieser Plan verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

Normale Anwendungsfälle für diese Architektur gehören:

- Hybrid Applications, Arbeitslasten zwischen einem lokalen Netzwerk und Azure verteilt werden.

- Applikationen große, geschäftskritischen Arbeitslasten, die hohe Skalierbarkeit erfordern.

- Sicherung und Wiederherstellung Großgeräte Daten extern gespeichert werden müssen.

- Behandeln von Big Data Arbeitslasten.

- Verwenden von Azure als Disaster Recovery-Standort.

Beachten Sie, dass die ExpressRoute-Verbindung nicht verfügbar ist, VPN-Route nur private peering Verbindungen behandelt. Öffentliche peering und Microsoft peering Verbindungen wird über das Internet geleitet.

## <a name="architecture-diagram"></a>Architekturdiagramm

>[AZURE.NOTE] [Azure VPN-Gateway Service] [ azure-vpn-gateway] implementiert zwei Typen von virtuellen Netzwerkgateways; VPN virtuelles Netzwerk-Gateways und ExpressRoute virtuelle Netzwerkgateways. In diesem Dokument der Begriff *VPN-Gateway* Azure Service beim Ausdrücken *VPN virtuelles Netzwerk-Gateway* und *ExpressRoute virtuelle Netzwerk-Gateway* bzw. auf die VPN- und ExpressRoute Implementierung des Gateways verwendet.

Das folgende Diagramm zeigt wichtigen Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm ist "Hybrid Netzwerk - ER VPN" Seite.

![[0]][0]

- **Azure Virtual Networks (VNets).** Jedes VNet befindet sich in einem einzigen Azure Bereich und mehrere Anwendungsebenen hosten kann. Anwendungsebenen mit Subnetzen in jeder VNet segmentiert werden bzw. Sicherheitsgruppen (NSGs).

- **Lokalen Unternehmensnetzwerk.** Dies ist ein Netzwerk von Computer und Geräte über ein privates LAN-Netzwerk in einer Organisation.

- **VPN-Anwendung.** Dies ist ein Gerät oder einen Dienst, die externe Konnektivität mit dem lokalen Netzwerk. Die VPN-Anwendung möglicherweise ein Hardwaregerät oder eine Software-Lösung wie der Routing- und RAS-Dienst (RRAS) in Windows Server 2012 sein.

> [AZURE.NOTE] Eine Liste der unterstützten VPN-Geräte und Informationen zum ausgewählten VPN-Geräte für die Verbindung mit einer Azure VPN-Gateway konfigurieren, finden Sie in der Anleitung für das entsprechende Gerät in der [Liste der von Azure unterstützten VPN-Geräte][vpn-appliance].

- **VPN virtuelles Netzwerk-Gateway.** Virtuelles Netzwerk VPN-Gateway ermöglicht VNet zum Herstellen der VPN-Anwendung im lokalen Netzwerk. Virtuelles Netzwerk VPN-Gateway konfiguriert Anfragen vom lokalen Netzwerk nur über die VPN-Anwendung akzeptieren. Weitere Informationen finden Sie in [einem lokalen Netzwerk mit einem virtuellen Microsoft Azure-Netzwerk verbinden][connect-to-an-Azure-vnet].

- **ExpressRoute virtuelles Netzwerk-Gateway.** ExpressRoute virtuelles Netzwerk-Gateway ermöglicht VNet Verbindung mit ExpressRoute-Verbindung für die Verbindung mit dem lokalen Netzwerk verwendet.

- **Gatewaysubnetz.** Virtuelles Netzwerk-Gateways werden in demselben Subnetz gehalten.

- **VPN-Verbindung.** Die Verbindung hat Eigenschaften, mit denen die Verbindung (IPSec) und der gemeinsam mit lokalen VPN-Anwendung zur Verschlüsselung des Verkehrs.

- **ExpressRoute-Verbindung.** Dies ist eine Schicht 2 oder Layer 3 Circuit angegeben vom konnektivitätsanbieter, Joins lokalen Netzwerk mit Azure über Edge-Router. Die Schaltung verwendet die Hardwareinfrastruktur konnektivitätsanbieter verwaltet.

- **Mehrstufige Cloudanwendung.** Dies ist die Anwendung in Azure. Es kann mehrere Ebenen mit mehreren Subnetzen über Azure Lastenausgleich verbundenen enthalten. Der Datenverkehr in jedem Subnet unterliegen Regeln [Azure Netzwerk]Sicherheitsgruppen[azure-network-security-group](NSGs). Weitere Informationen finden Sie unter [Erste Schritte mit Microsoft Azure Security][getting-started-with-azure-security].

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure-Ressourcen-Manager um die Referenzarchitektur installieren, die diese Empfehlung folgt bereitgestellt. Wenn Sie eigene Referenzarchitektur erstellen diese Empfehlung Sie befolgen nur eine bestimmte Anforderung, die Empfehlung nicht passen.

### <a name="vnet-and-gatewaysubnet"></a>VNet und Subnetzname

Erstellen Sie ExpressRoute virtuelles Netzwerk-Gateway und virtuelles Netzwerk VPN-Gateway in demselben VNet. Dies bedeutet, dass sie im selben Subnetz mit dem Namen **Subnetzname** verwendet werden soll

Das VNet enthält bereits ein Subnetz mit dem Namen **Subnetzname**sicher, dass sie ein /27 oder größeren Adressraum verfügt. Wenn vorhandene Subnetz zu klein ist, wie folgt entfernen Sie und erstellen Sie eine neue zu, wie unter dem nächsten Punkt dargestellt:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Wenn das VNet kein Subnetz mit dem Namen **Subnetzname**enthält, erstellen Sie eine neue folgendermaßen:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>VPN und ExpressRoute-gateways

Stellen Sie sicher, dass Ihre Organisation [ExpressRoute Grundvoraussetzungen] erfüllt[ expressroute-prereq] für die Verbindung mit Azure.

Haben Sie bereits eine virtuelles Netzwerk VPN-Gateway in Ihrem VNet Azure, entfernen Sie es, wie unten dargestellt.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Gehen Sie bei [einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute] [ implementing-expressroute] die ExpressRoute-Verbindung.

Gehen Sie bei [einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-] [ implementing-vpn] Ihr VPN virtuelles Netzwerk Gateway-Verbindung.

Nachdem der primären virtuellen Netzwerk-Gateway-Verbindungen testen die Umgebung wie folgt:

1. Sicherstellen Sie, dass Sie aus Ihrem lokalen Netzwerk Ihre Azure VNet verbinden können.

2. Wenden Sie sich an Ihren Anbieter, um ExpressRoute Konnektivität für Tests zu beenden.

3. Stellen Sie sicher, dass Sie noch aus dem lokalen Netzwerk der Azure-VNet über VPN-virtuelles Netzwerk-Gateway-Verbindung verbinden kann.

4. Wenden Sie sich an Ihren Anbieter, um Reestabilish ExpressRoute Konnektivität.

## <a name="considerations"></a>Hinweise

ExpressRoute Hinweise finden Sie unter [Implementieren einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute] [ guidance-expressroute] Anleitung.

Standort-zu-Standort-VPN-Aspekte finden Sie unter [Implementieren einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-] [ guidance-vpn] Anleitung.

Allgemeine Sicherheitsaspekte Azure finden Sie unter [Microsoft Cloud-Dienste und Sicherheit][best-practices-security].

## <a name="solution-deployment"></a>Bereitstellung der Lösung

Wenn Sie eine vorhandene lokale Infrastruktur mit geeigneten Network Appliance bereits konfiguriert haben, können Sie die Referenzarchitektur folgendermaßen bereitstellen:

1. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Warten auf die Verknüpfung zum Azure-Portal öffnen, und gehen Sie folgendermaßen vor: 
  - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-hybrid-vpn-er-rg` im Textfeld.
  - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
  - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie, bis die Bereitstellung abgeschlossen.

4. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Warten Sie auf den Link öffnen in Azure-Portal geben dann folgendermaßen: 
  - Wählen Sie im Bereich **Ressourcengruppe** **bestehende verwenden** , und geben Sie `ra-hybrid-vpn-er-rg` im Textfeld.
  - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
  - Klicken Sie auf die Schaltfläche **Einkauf** .

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Architektur einer hochverfügbaren Hybrid-Netzwerkarchitektur mit ExpressRoute und VPN-gateway"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
