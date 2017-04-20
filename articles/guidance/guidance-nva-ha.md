<properties
   pageTitle="Virtuelle Appliances hoher Verfügbarkeit bereitstellen | Microsoft Azure"
   description="Wie virtuelle Netzwerkgeräte hohe Verfügbarkeit bereitgestellt."
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
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Bereitstellen von virtuellen Appliances hoher Verfügbarkeit

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt verschiedene Methoden zum Bereitstellen von virtuellen Netzwerkgeräte (NVAs) in hoher Verfügbarkeit. Stellen Sie sicher, dass Sie verstehen, wie [benutzerdefinierte Routen (UDR)] [ udr-overview] und [Lastenausgleich] [ lb-overview] arbeiten in Azure.

Können andere NVAs verfügbar in Azure Marketplace zum Erweitern der Funktionalität von Azure genauso Appliances im lokalen Datencenter verwenden. Die folgende Abbildung zeigt eine beispielbereitstellung einer [einzigen NVA] [ nva-scenario] als Firewall-Appliance. 

![[0]][0]

Obwohl die vorherige Bereitstellung verwendet, stellt eine einzelne Fehlerquelle. Schlägt die virtuelle Appliance fließt kein Datenverkehr. Um dieses Problem zu beheben, müssen Sie mehrere NVAs verwenden. Jedoch, die auch andere Einstellungen und Ressourcen je nach Bedarf.

Einen der folgenden Schritte können Sie eine hochverfügbare NVA Umgebung bereitstellen.

|Lösung|Vorteile|Hinweise|
|---|---|---|
|Eindringen mit virtuellen Appliances Ebene 7|Alle sind Knoten aktiv|Erfordert eine NVA, die Verbindung zu beenden und SNAT verwenden können<br/>Erfordert einen separaten Satz von NVAs für Datenverkehr aus dem Internet und Azure<br/>Kann nur für Datenverkehr außerhalb Azure verwendet werden|
|Eindringen-Ausgang mit virtuellen Appliances Ebene 7|Alle sind Knoten aktiv<br/>Ursprung in Azure Datenverkehr verarbeiten |Erfordert eine NVA, die Verbindung zu beenden und SNAT verwenden können<br/>Erfordert einen separaten Satz von NVAs für Datenverkehr aus dem Internet und Azure|
|PIP-UDR wechseln|Einheitliche NVAs für den gesamten Datenverkehr<br/>Alle Datenverkehr (unbegrenzt Portregeln) können verarbeitet werden.|Aktiv / Passiv<br/>Erfordert einen failover|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Eindringen mit virtuellen Appliances Ebene 7
Eine Reihe von NVAs hinter Azure Lastenausgleich können Sie Konnektivität Azure Arbeitslasten hinter einer kleinen Gruppe von serverseitigen Ports (wie HTTP und HTTPS). Die folgende Abbildung zeigt wie hohe Verfügbarkeit in diesem Szenario Ebene NVA.

![[1]][1]

In diesem Szenario muss die virtuelle Netzwerk-Appliance verwendet beendet alle Verbindungen und Arbeitslast Subnetz übergeben. Arbeitslast virtuelle Maschinen (VMs) reagieren auf NVA Ersuchen und Verkehrswerte ohne Probleme erhalten. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Eindringen-Ausgang mit virtuellen Appliances Ebene 7
Die vorherige Architektur erweitert kann und hinzufügen NVAs den Verkehr aus Azure an NVAs, wie in der folgenden Abbildung dargestellt:

![[2]][2]

In diesem Szenario wird allen Datenverkehr in Azure internen Lastenausgleich weitergeleitet, die Last zwischen verschiedenen NVAs verteilt. Diese NVAs direkte Datenverkehr mit dem Internet, deren einzelne öffentliche IP-Adressen. 

## <a name="pip-udr-switch"></a>PIP-UDR wechseln
Sie können vermeiden, mehrere NVA Stapel mit zwei NVAs in Aktiv-Passiv-Modus erstellen. In diesem Szenario können Sie die öffentliche IP-Adresse (PIP) und benutzerdefinierte Routen (Decision) wechseln, stoppt der aktive Knoten.  

![[3]][3]

Dieses Szenario ähnelt dem Szenario der NVA. Der einzige Unterschied ist, dass PIP und Decision geändert werden müssen, um Datenverkehr zwischen den NVAs zu wechseln. Diese Änderung können manuell oder können Sie auch automatisieren. Um zu automatisieren, können Sie eine Anwendung beide NVAs bereitstellen, die den Zustand des aktiven Knotens überprüft. Sobald der aktive Knoten ausgefallen ist, kann die Anwendung PIP und Decision Verbindung auf den passiven Knoten ändern.

Eine mögliche Implementierung dieser Lösung ist die Verwendung einer [ZooKeeper] [ zookeeper] -Daemon auf NVAs mit Hinweislinie Wahl (entscheiden, welcher Knoten der aktive Knoten ist). Sobald eine Hinweislinie gewählt, wird Azure REST API PIP vom ausgefallenen Knoten entfernen und zur Hinweislinie. Sie sollten auch Decision neue Hinweislinie private IP-Adresse auf ändern.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Implementieren einer DMZ zwischen Azure und Rechenzentrum lokalen] [ dmz-on-prem] mit Layer 7 NVAs.
- Informationen zum [Implementieren einer DMZ zwischen Azure und dem Internet] [ dmz-internet] mit Layer 7 NVAs.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Einzelne NVA-Architektur"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Ebene 7 Eindringen"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "Ebene 7 Ingress- und egress"
[3]: ./media/guidance-nva-ha/active-passive.png "Aktiv / Passiv-cluster"