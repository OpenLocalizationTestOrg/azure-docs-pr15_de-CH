<properties 
   pageTitle="Was sind Benutzer definierten Routen und IP-Weiterleitung"
   description="Erfahren Sie, wie Benutzer definierten Routen (UDR) und Weiterleiten von IP-Verkehr mit Netzwerk virtuelle Appliances in Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-are-user-defined-routes-and-ip-forwarding"></a>Was sind Benutzer definierten Routen und IP-Weiterleitung
Wenn Sie virtuelle Maschinen (VMs) zu einem virtuellen Netzwerk (VNet) in Azure hinzufügen, sehen Sie, dass die virtuellen Computer im Netzwerk automatisch kommunizieren können. Sie brauchen keinen Gateway angeben, obwohl VMs in unterschiedlichen Subnetzen befinden. Gilt auch für die Kommunikation von VMs mit dem öffentlichen Internet und sogar auf Ihrem lokalen Netzwerk eine hybridverbindung von Azure zu Ihrem eigenen Datencenter vorhanden ist.

Diese Kommunikationsfluss ist möglich, weil Azure System-Routen verwendet definieren, wie IP-Datenverkehr fließt. Systemrouten steuern die Kommunikation in den folgenden Szenarien:

- Aus demselben Subnetz.
- Ein Subnetz innerhalb einer VNet.
- Von VMs mit dem Internet.
- Von einem VNet zu einem anderen VNet über ein VPN-Gateway.
- Aus einem VNet mit dem lokalen Netzwerk über ein VPN-Gateway.

Die folgende Abbildung zeigt ein einfaches Setup mit einer VNet zwei Subnetzen und einige VMs sowie das System, das IP-Datenverkehr.

![Systemrouten in Azure](./media/virtual-networks-udr-overview/Figure1.png)

Obwohl die Verwendung von systemrouten Datenverkehr automatisch für die Bereitstellung vereinfacht, sind in dem Sie das routing von Paketen über ein virtuelles Gerät steuern möchten. Sie können dies durch Erstellen von benutzerdefinierten Routen nächsten Hop für Pakete auf einem bestimmten Subnetz die virtuelle Appliance stattdessen zu übertragen und Aktivieren der IP-Weiterleitung für die VM als virtuelle Appliance angeben.

Die nachfolgende Abbildung zeigt ein Beispiel eines benutzerdefinierten Routen und IP-Routing Pakete an ein Subnetz voneinander zu virtuelle Appliance in einem dritten Subnetz.

![Systemrouten in Azure](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] Benutzerdefinierte Routen gelten nur für Datenverkehr ein Subnetz. Sie erstellen keine Routen angeben, wie Datenverkehr in einem Subnetz aus dem Internet beispielsweise kommt. Auch das Weiterleiten von Datenverkehr an Gerät im selben Subnetz nicht des Datenverkehrs Ursprung. Erstellen Sie immer ein separates Subnetz für Ihre Geräte. 

## <a name="route-resource"></a>Route-Ressource
Pakete werden über ein TCP/IP-Netzwerk für eine Routentabelle auf jedem Knoten des physischen Netzwerks definiert weitergeleitet. Route-Tabelle ist eine Auflistung von Routen entscheiden, wohin Pakete basierend auf der IP-Zieladresse verwendet. Eine Route besteht aus folgendem:

|Eigenschaft|Beschreibung|Nebenbedingungen|Hinweise|
|---|---|---|---|
| Präfix | Die Ziel-CIDR, die Route, wie 10.1.0.0/16 gilt.|Muss einen CIDR-Adressen Internetzugang, Azure virtual Network oder lokalen Datencenter darstellt.|Stellen Sie sicher, dass das **Adresspräfix** nicht die Adresse für die **Adresse des nächsten Abschnitts**, andernfalls enthalten die Pakete in einer Schleife wird von der Quelle zum nächsten Hop ohne Ziel erreichen eingeben. |
| Nächste Hop-Typ | Typ der Azure-Hop sollte das Paket an gesendet. | Einer der folgenden Werte muss sein: <br/> **Virtuelles Netzwerk**. Das lokale virtuelle Netzwerk darstellt. Beispielsweise haben Sie zwei Subnetzen, 10.1.0.0/16 und 10.2.0.0/16 im gleichen virtuellen Netzwerk, haben die Route für jedes Subnetz in der Routentabelle nächsten Hop Wert *Virtuelles Netzwerk*. <br/> **Virtuelle Netzwerk-Gateway**. Ein Azure S2S VPN-Gateways darstellt. <br/> **Internet**. Stellt das Internet Standardgateway von der Azure-Infrastruktur bereitgestellt. <br/> **Virtuelle Appliance**. Stellt die virtuelle Appliance Azure virtuelle Netzwerk hinzugefügt. <br/> **Keine**. Stellt ein schwarzes Loch. Ein schwarzes Loch weitergeleitete Pakete werden überhaupt nicht weitergeleitet.| Verwenden Sie ein **keine** Pakete an ein bestimmtes Ziel zu. | 
| Adresse des nächsten Abschnitts | Die Adresse des nächste Abschnitts enthält die IP-Adresse Pakete weitergeleitet werden sollen. Nächste Hop-Werte dürfen nur in Arbeitsplänen, der nächste Hop *Virtuelle Appliance*ist.| Muss eine IP-Adresse, die im virtuellen Netzwerk erreichbar ist, benutzerdefinierte Route angewendet wird. | Wenn die IP-Adresse eine VM darstellt, unbedingt in Azure [IP-Weiterleitung](#IP-forwarding) für den virtuellen Computer aktivieren. |

In Azure PowerShell haben "NextHopType" Werte unterschiedliche Namen:
- Virtuelles Netzwerk ist VnetLocal
- Virtuelle Netzwerk-Gateway ist VirtualNetworkGateway
- Virtuelle Appliance ist VirtualAppliance
- Internet ist
- Keine ist None.

### <a name="system-routes"></a>Systemrouten
Jedes Subnetz in ein virtuelles Netzwerk erstellt ist automatisch eine Routentabelle mit System Route Regeln zugeordnet:

- **Lokale Vnet Regel**: Diese Regel automatisch für jedes Subnetz ein virtuelles Netzwerk erstellt. Es gibt eine direkte Verbindung zwischen den virtuellen Computern in der VNet gibt, gibt es keine temporären Nächster Hop.
- **Lokale Regel**: Diese Regel gilt für alle Datenverkehr an den lokalen Adressbereich und VPN-Gateway als der nächste Hop-Ziel verwendet.
- **Internet-Regel**: Diese Regel behandelt alle Datenverkehr an das öffentliche Internet (0.0.0.0/Adresse Präfix 0) bestimmt und Infrastruktur Internet-Gateway als Nächster Hop für den gesamten Datenverkehr verwendet zum Internet bestimmt.

### <a name="user-defined-routes"></a>Benutzerdefinierte Routen
Für die meisten Unternehmen benötigen Sie nur systemrouten von Azure bereits definiert. Allerdings müssen Sie eine Routentabelle erstellen und eine oder mehrere Routen in bestimmten Fällen wie hinzufügen:

- Erzwingen Sie mit dem Internet über das lokale Netzwerk tunneling.
- Mit virtuellen Appliances in der Azure-Umgebung.

In den obigen Szenarios müssen Sie eine Routentabelle erstellen und benutzerdefinierte Routen hinzufügen. Haben mehrere Routetabellen und derselben Routentabelle kann ein oder mehrere Subnetze zugeordnet werden. Und jedes Subnetz kann nur eine single-Route-Tabelle zugeordnet werden. Alle VMs und Cloud-Services in einem Subnetz der Routentabelle an Subnetz verbunden.

Subnetze basieren auf systemrouten, bis eine Route-Tabelle im Subnetz zugeordnet ist. Sobald eine Zuordnung vorhanden ist, routing am längsten Präfix Übereinstimmung (LPM) benutzerdefinierte Routen und systemrouten Grundlage erfolgt. Ist mehr als eine Route mit dem gleichen LPM wird eine Route basierend auf seinen Ursprung in der folgenden Reihenfolge ausgewählt:

1. Benutzer definierten route
1. BGP-Routen (wenn ExpressRoute verwendet)
1. System-route

Informationen zum Erstellen von benutzerdefinierten Routen finden Sie unter [Erstellen und aktivieren Sie IP-Weiterleitung in Azure](virtual-network-create-udr-arm-template.md).

>[AZURE.IMPORTANT] Benutzerdefinierte Routen gelten nur für Azure VMs und Cloud-Dienste. Wenn Sie die virtuelle Appliance eine Firewall zwischen dem lokalen Netzwerk und Azure hinzufügen möchten, müssen Sie beispielsweise eine benutzerdefinierte Route für Ihren Azure Routetabellen erstellen, die allen Datenverkehr zum lokalen Adressraum auf die virtuelle Appliance weitergeleitet. Subnetzname den gesamten lokal in Azure über die virtuelle Appliance weiterleiten können auch eine benutzerdefinierte Route (UDR) hinzufügen. Dies ist eine neue Ergänzung.

### <a name="bgp-routes"></a>BGP-Routen
Haben eine ExpressRoute-Verbindung zwischen dem lokalen Netzwerk und Azure können Sie BGP Routen aus dem lokalen Netzwerk in Azure übertragen. Diese BGP-Routen werden wie systemrouten und benutzerdefinierten Routen in jedem Subnet Azure verwendet. Weitere Informationen finden Sie unter [Einführung in ExpressRoute](../expressroute/expressroute-introduction.md).

>[AZURE.IMPORTANT] Konfigurieren Sie Ihre Azure-Umgebung Kraft tunneling über das lokale Netzwerk durch Erstellen einer Benutzer definierten Route für Subnetzmaske 0.0.0.0/0, der als Nächster Hop VPN-Gateway verwendet. Dies funktioniert jedoch nur bei einem VPN-Gateway nicht ExpressRoute. Für ExpressRoute wird das erzwungene Tunneln über BGP konfiguriert.

## <a name="ip-forwarding"></a>IP-Weiterleitung
Wie oben beschrieben, ist einer der Hauptgründe, warum eine Benutzer definierte Route Datenverkehr an virtuelle Appliance weiterleiten. Virtuelle Appliance ist nichts anderes als ein virtueller Computer, die eine Anwendung zur Behandlung von Netzwerkverkehr gewissermaßen eine Firewall oder ein NAT-Gerät verwendet.

Diese virtuelle Appliance VM muss eingehenden Datenverkehr empfangen, der sich nicht behandelt. Damit eine VM an andere Ziele gerichtete Datenverkehr empfangen können, müssen Sie die IP-Weiterleitung für den virtuellen Computer aktivieren. Dies ist ein Azure, keine Einstellung im Gastbetriebssystem.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Erstellen von Routen in der Ressourcen-Manager-Bereitstellungsmodell](virtual-network-create-udr-arm-template.md) und Subnetze zuzuordnen. 
- Informationen zum [Erstellen von Routen in der klassischen Bereitstellungsmodell](virtual-network-create-udr-classic-ps.md) und ordnen diese Subnetze.
