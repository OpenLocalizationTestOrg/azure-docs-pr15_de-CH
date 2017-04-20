
<properties
   pageTitle="Azure virtual Network peering | Microsoft Azure"
   description="Lernen Sie in Azure peering VNet."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="narayan" />

# <a name="vnet-peering"></a>VNet peering

VNet peering ist ein Mechanismus, der zwei virtuelle Netzwerke (VNets) im selben Bereich Azure Backbone-Netzwerk verbunden. Sobald dies, werden zwei virtuelle Netzwerke als Gründen Konnektivität angezeigt. Sie werden weiterhin als separate Ressourcen verwaltet, aber virtuellen Computer in dieser virtuellen Netzwerke direkt mithilfe von privaten IP-Adressen miteinander kommunizieren können.

Der Datenverkehr zwischen virtuellen Maschinen in hervorragendem virtuelle Netzwerke wird über der Azure-Infrastruktur weitergeleitet, wie Datenverkehr zwischen virtuellen Computern im gleichen virtuellen Netzwerk weitergeleitet wird. Die Vorteile von VNet peering gehören:

- Eine niedrige Latenz und hoher Bandbreite Verbindung zwischen Ressourcen in verschiedenen virtuellen Netzwerken.
- Die Möglichkeit, Ressourcen wie Netzwerkgeräte und VPN-Gateways als Transitländer hervorragendem VNet verwenden.
- Die Möglichkeit, ein virtuelles Netzwerk herstellen, das Azure Ressourcenmanager zu einem virtuellen Netzwerk verwendet, das das klassische Bereitstellungsmodell und vollständige Konnektivität zwischen Ressourcen in dieser virtuellen Netzwerke aktivieren.

Vorschriften und Schwerpunkte peering VNet:

- Zwei virtuelle Netzwerke, die dies sind sollte in derselben Azure-Region.
- Die virtuellen Netzwerke sind dies müssen nicht überlappende IP-Adressbereiche.
- VNet peering zwischen virtuellen Netzwerken ist und keine abgeleiteten transitive Beziehung vorhanden ist. Beispielsweise wenn ein virtuelles Netzwerk mit virtuellen Netzwerk B dies und wenn virtuelles Netzwerk B mit virtuellen Netzwerk C dies, nicht auf virtuelle übersetzt Netzwerk einem wird dies mit virtuellen Netzwerk c
- Peering kann zwischen virtuellen Netzwerken in zwei verschiedenen Subskriptionen hergestellt werden lange ein privilegierter Benutzer beider Subskriptionen autorisiert die peering und Abonnements gehören demselben Active Directory-Mandanten. 
- Peering zwischen virtuellen Netzwerk Ressourcen-Manager-Modell und klassischen Bereitstellungsmodell erfordert, dass die VNets im gleichen Abonnement sein soll.
- Ein virtuelles Netzwerk, das Ressourcen-Manager Bereitstellung verwendet, dies mit einem anderen virtuellen Netzwerk, das dieses Modell verwendet oder mit einem virtuellen Netzwerk, das klassische Bereitstellungsmodell verwendet. Allerdings können nicht virtuelle Netzwerke, die das klassische Bereitstellungsmodell sich dies.
- Wenn die Kommunikation zwischen virtuellen Computern in hervorragendem virtuelle Netzwerke keine zusätzliche Bandbreite eingeschränkt ist, gilt Bandbreite Gap VM-Größe.


![Grundlegende VNet peering](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Konnektivität
Nach zwei virtuelle Netzwerke Dies sind, können ein virtuellen Computer (Web-Worker-Rolle) in das virtuelle Netzwerk direkt mit anderen virtuellen Computern im hervorragendem virtuellen Netzwerk verbinden. Diese zwei Netzwerke haben vollständigen IP-Konnektivität.

Die Netzwerkwartezeit einen Roundtrip zwischen zwei virtuellen Maschinen in hervorragendem virtuellen Netzwerken entspricht der Schleife in einem virtuellen LAN. Der Netzwerkdurchsatz basiert auf die Bandbreite, die für den virtuellen Computer, die proportional zu ihrer Größe zulässig ist. Eine weitere Einschränkung Bandbreite nicht.

Der Datenverkehr zwischen den virtuellen Computern in hervorragendem virtuelle Netzwerke wird direkt über Azure Back-End-Infrastruktur und nicht über ein Gateway weitergeleitet.

Virtuelle Computer in einem virtuellen Netzwerk können die internen Lastenausgleich (ILB) Endpunkte in hervorragendem virtuelles Netzwerk zugreifen. Netzwerk-Sicherheitsgruppen (NSGs) können entweder virtuellen Netzwerk den Zugriff auf andere virtuelle Netzwerke oder Subnetze blockieren bei Bedarf angewendet werden.

Konfigurieren von peering können sie öffnen oder schließen NSG Regeln zwischen virtuellen Netzwerken. Wählt der Benutzer vollständige Konnektivität zwischen hervorragendem virtuelle Netzwerke (ist die Standardoption) öffnen, können sie NSGs auf bestimmte Subnetze oder virtuellen Computern zu blockieren bestimmte Zugriff verweigern.

Azure bereitgestellten internen DNS-Auflösung für virtuelle Computer hinweg nicht hervorragendem virtuelle Netzwerke. Virtuelle Maschinen müssen interne DNS-Namen, die nur im lokalen virtuellen Netzwerk aufgelöst werden. Benutzer können jedoch virtuelle Computer konfigurieren, die als DNS-Server für ein virtuelles Netzwerk hervorragendem virtuelle Netzwerke ausgeführt werden.

## <a name="service-chaining"></a>Service verketten
Benutzer können benutzerdefinierte Routentabelle, die auf virtuellen Computern in hervorragendem virtuelle Netzwerke als "Nächster Hop" IP-Adresse konfigurieren, wie in der Abbildung weiter unten in diesem Artikel dargestellt. Dadurch können Benutzer zu verketten von Diensten über denen Datenverkehr von einem virtuellen Netzwerk virtuelle Appliance steuern können, die in hervorragendem virtuelles Netzwerk mit benutzerdefinierten Routentabelle ausgeführt wird.

Benutzer können auch Hub and Spoke-Umgebungen erstellen, Hub Infrastrukturkomponenten wie eine virtuelle Appliance hosten kann. Alle virtuellen Sprach-Netzwerke können dann peer mit es sowie eine Teilmenge des Datenverkehrs an Einheiten im Hub virtuelles Netzwerk ausgeführt werden. Also VNet peering nächste Hop IP-Adresse auf "benutzerdefinierte der Routentabelle" ermöglicht, die IP-Adresse eines virtuellen Computers in hervorragendem virtuelles Netzwerk.

## <a name="gateways-and-on-premises-connectivity"></a>Gateways und lokale Konnektivität
Jede virtuelles Netzwerk mit einem anderen virtuellen Netzwerk Dies ist haben eigene Gateway und Verbindung zu lokalen verwenden noch. Benutzer können auch [VNet VNet Verbindung](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) konfigurieren mithilfe von Gateways, obwohl dies virtuelle Netzwerke sind.

Wenn beide Optionen für virtuelles Netzwerk Vernetzung konfiguriert sind, verläuft der Datenverkehr zwischen virtuellen Netzwerken peering Konfiguration (d. h. durch Azure Backbone).

Bei virtuelle Netzwerken Dies sind, können Benutzer auch Gateway in hervorragendem virtuelles Netzwerk als Transitland lokal konfigurieren. In diesem Fall keinen virtuelle Netzwerk, das mit einem Remotegateway eigene Gateway. Ein virtuelles Netzwerk können nur einem Gateway. Es kann ein lokales Gateway oder einem Remotegateway (im hervorragendem virtuellen Netzwerk) sein wie in der folgenden Abbildung dargestellt.

Peering Beziehung zwischen virtuelle Netzwerke mit dem Ressourcen-Manager-Modell die klassischen Bereitstellungsmodell nicht Gateway übertragen werden. Beide virtuelle Netzwerke peering Beziehung müssen Ressourcen-Manager-Bereitstellungsmodell für eine Gateway zu verwenden.

Wenn virtuelle Netzwerke, die eine einzelne Azure ExpressRoute Verbindung teilen dies sind durchläuft der Datenverkehr zwischen Peers Beziehung (d. h. über das Azure Backbonenetzwerk). Benutzer noch können lokalen Gateways in jedem virtuellen Netzwerk Sie die lokale Verbindung herstellen. Alternativ können sie freigegebene Gateway verwenden und Übertragung lokale Verbindung konfigurieren.

![VNet peering Übertragung](./media/virtual-networks-peering-overview/figure02.png)

## <a name="provisioning"></a>Bereitstellung
VNet peering ist ein privilegierter Vorgang. Es ist eine separate Funktion unter dem VirtualNetworks-Namespace. Ein Benutzer kann bestimmte Rechte autorisieren peering zugewiesen werden. Ein Benutzer mit Lese-/ Schreibzugriff auf das virtuelle Netzwerk erbt diese Rechte automatisch.

Ein Benutzer entweder Administrator oder einen privilegierten Benutzer peering Fähigkeit kann peering Vorgang in einem anderen VNet initiieren. Wird eine übereinstimmende Anforderung auf der anderen Seite Peering und anderen Vorschriften eingehalten, wird die peering eingerichtet.

Finden Sie im Abschnitt "Nächste Schritte" um weitere Informationen zu VNet zwischen zwei virtuelle Netzwerke peering Artikel.

## <a name="limits"></a>Grenzen
Ist begrenzt auf die Anzahl der Peerings für ein virtuelles Netzwerk zulässig sind. Weitere Informationen finden Sie in [Azure networking Grenzen](../azure-subscription-service-limits.md#networking-limits) .

## <a name="pricing"></a>Preisgestaltung
VNet peering werden kostenlos Berichtszeitraum. Nach der Veröffentlichung, werden eine Schutzgebühr Ingress- und Egress-Datenverkehr, der die peering verwendet. Weitere Informationen finden Sie auf der [Seite Preise](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten von virtuellen Netzwerken peering](virtual-networks-create-vnetpeering-arm-portal.md).
- Lernen Sie [NSGs](virtual-networks-nsg.md).
- Informationen Sie zu [benutzerdefinierten Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).
