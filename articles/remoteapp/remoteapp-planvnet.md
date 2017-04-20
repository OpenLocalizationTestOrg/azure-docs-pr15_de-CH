<properties
    pageTitle="Virtuelle Netzwerk für eine Auflistung Azure RemoteApp Planung | Microsoft Azure"
    description="Informationen Sie zum virtuelle Netzwerk für eine Auflistung Azure RemoteApp planen."
    services="remoteapp"
    documentationCenter="" 
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Virtuelle Netzwerk für Azure RemoteApp planen

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Dieses Dokument beschreibt die Azure virtuellen Netzwerk (VNET) und die Subnetzmaske für Azure RemoteApp einrichten. Wenn Sie Azure virtuellen Netzwerken vertraut sind, ist dies eine Funktion, die Ihre Netzwerkinfrastruktur zur Cloud Virtualisieren und Hybrid-Lösungen mit Azure und Ihre lokalen Ressourcen erstellen kann. Lesen Sie mehr dazu [hier](../virtual-network/virtual-networks-overview.md).

Wenn Sie Sicherheitsrichtlinien für Datenverkehr (eingehende und ausgehende) im virtuellen Netzwerk definieren, in dem Sie Azure RemoteApp bereitstellen, möchten, empfehlen wir ein separates Subnetz für Azure RemoteApp vom Rest der Bereitstellung Ihrer Azure virtuelles Netzwerk erstellen. Lesen Sie weitere Informationen zum Definieren von Sicherheitsrichtlinien im Subnetz Azure virtual Network [Neuigkeiten Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Typen von Azure RemoteApp-Sammlungen mit Azure virtuelle Netzwerke

Die folgenden Bilder zeigen zwei unterschiedliche Optionen, wenn Sie ein virtuelles Netzwerk verwenden möchten.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp Cloud-Auflistung vnet

 ![Azure RemoteApp - Cloud-Sammlung mit einem VNET](./media/remoteapp-planvpn/ra-cloudvpn.png)

Dies ist eine Azure RemoteApp-Sammlung, wo alle RemoteApp-Sitzung Hosts müssen den Zugriff auf Ressourcen in Azure bereitgestellt werden. Sie können die gleichen VNET wie RemoteApp VNET oder andere VNET in Azure.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure RemoteApp Hybrid-Auflistung vnet

![Azure RemoteApp - Hybrid-Auflistung ein vnet](./media/remoteapp-planvpn/ra-hybridvpn.png)

Dies stellt eine einige der RemoteApp Session Hosts müssen den Zugriff auf Ressourcen lokal bereitgestellte sind Azure RemoteApp-Auflistung dar. RemoteApp VNET ist mit dem lokalen Netzwerk Azure hybridtechnologien wie Standort-zu-Standort-VPN oder Express Route verknüpft.


## <a name="how-the-system-works"></a>Funktionsweise des

Hinter den Kulissen bereit Azure RemoteApp Azure virtuelle Computer (mit der hochgeladenen Bild) mit virtuellen Netzwerk-Subnetz, das während der Bereitstellung ausgewählt Bei einer Hybrid-Sammlung, versuchen wir den FQDN des Domänencontrollers auflösen eingegebene Bereitstellung Workflow mit der DNS-Server in das virtuelle Netzwerk bereitgestellt.  
Wenn Sie ein vorhandenes virtuelles Netzwerk herstellen, müssen Sie die erforderlichen Ports in Ihrem netzwerksicherheitsgruppen in Azure RemoteApp Subnetz verfügbar machen. 

Wir empfehlen [für Azure RemoteApp groß genug Subnetz](remoteapp-vnetsizing.md)verwenden. Azure virtuelle Netzwerk ist /8 (mit CIDR Subnetzdefinitionen). Subnetz sollte groß genug für alle Azure RemoteApp VMs beim Skalieren Wenn mehr Benutzer apps zugreifen. 

Folgende Dinge Sie aktivieren Ihr virtuelles Netzwerk-Subnetz müssen: 

2.  Ausgehender Datenverkehr aus dem Subnetz auf Port 10101 10175 dürfen von internen Azure RemoteApp-Diensten kommunizieren.
3.  Ausgehender Datenverkehr aus dem Subnetz darf Azure Storage Port 443 herstellen
4.  Haben Sie Active Directory in Azure gehostet, sicherzustellen Sie, dass jede VM Subnetz virtuelles Netzwerk für Azure RemoteApp Domänencontroller herstellen kann. DNS in das virtuelle Netzwerk sollte den FQDN dieses Domänencontrollers auflösen.


## <a name="virtual-network-with-forced-tunneling"></a>Virtuelles Netzwerk mit erzwungener Tunnel

[Erzwungene Tunnel](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) wird nun für alle neuen Azure RemoteApp-Sammlungen unterstützt. Wir unterstützen derzeit nicht die Migration einer vorhandenen Sammlung Erzwungene Tunnel unterstützt.  Sie müssen alle vorhandenen Sammlungen mit der Azure RemoteApp verknüpften VNET löschen und erstellen eine neue gezwungen erhalten tunneling Sammlungen aktiviert. 
