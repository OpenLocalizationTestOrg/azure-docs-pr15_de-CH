<properties
   pageTitle="Öffentliche und private Adressierung IP (klassisch) in Azure | Microsoft Azure"
   description="Erfahren Sie mehr über öffentliche und private IP-Adressierung in Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>IP-Adressen (klassisch) in Azure
Azure-Ressourcen mit anderen Azure Ressourcen, Ihrem lokalen Netzwerk und dem Internet kommunizieren können Sie IP-Adressen zuweisen. Es gibt zwei Arten von IP-Adressen in Azure können: öffentliche und private.

Öffentliche IP-Adressen werden für die Kommunikation mit dem Internet Azure öffentliche Dienste verwendet.

Private IP-Adressen werden für die Kommunikation innerhalb einer Azure virtual Network (VNet), einen Cloud-Dienst und lokalen Netzwerk verwendet, wenn ein VPN-Gateway oder ExpressRoute-Verbindung verwenden, um Ihr Netzwerk in Azure erweitern.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Bereitstellungsmodell](virtual-network-ip-addresses-overview-arm.md).

## <a name="public-ip-addresses"></a>Öffentliche IP-Adressen
Öffentliche IP-Adressen können Azure Ressourcen Azure öffentliche Dienste wie [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-Datenbanken](../sql-database/sql-database-technical-overview.md)und [Azure-Speicher](../storage/storage-introduction.md)mit Internet kommunizieren.

Eine öffentliche IP-Adresse gibt es die folgenden Ressourcentypen:

- Cloud-Dienste
- IaaS virtuelle Maschinen (VMs)
- PaaS-Instanzen
- VPN-gateways
- Anwendungsgateways

### <a name="allocation-method"></a>Zuordnungsmethode
Eine öffentliche IP-Adresse einer Azure Ressource zugewiesen werden soll, wird *dynamisch* aus verfügbaren öffentlichen IP-Adresse am Standort erstellte Ressource zugeordnet. Diese Adresse wird freigegeben, wenn die Ressource beendet wird. Bei der Cloud-Dienst dies geschieht, wenn alle Rolleninstanzen stehen, die vermieden werden mit *statischen* (reserviert) (siehe [Cloud-Dienste](#Cloud-services)).

>[AZURE.NOTE] Die Liste der IP-Adressbereiche aus dem Azure-Ressourcen öffentliche IP-Adressen zugewiesen werden erscheint in [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>DNS-Auflösung von Hostnamen
Beim Erstellen einer Cloud-Dienst oder eine VM IaaS müssen Sie alle Ressourcen in Azure eindeutig ist ein DNS-Cloud-Dienst bereitstellen. Dies erstellt eine Zuordnung in Azure verwaltet DNS-Server für *DNS-Name*. cloudapp.net der öffentlichen IP-Adresse der Ressource. Beim Erstellen eines Cloud-Diensts einen Cloud-Dienst DNS-Namen von **Contoso**wird vollständig qualifizierten Domain Name (FQDN) **contoso.cloudapp.net** beispielsweise in eine öffentliche IP-Adresse (VIP) Cloud-Dienst aufgelöst. Dieser FQDN können Sie eine benutzerdefinierte Domäne CNAME-Eintrag auf die öffentliche IP-Adresse in Azure erstellen.

### <a name="cloud-services"></a>Cloud-Dienste
Cloud-Dienst hat immer eine öffentliche IP-Adresse als eine virtuelle IP-Adresse (VIP). Sie können Endpunkte in einem Clouddienst verschiedene Ports VIP an interne Ports VMs und Instanzen in der Cloud-Dienst zu erstellen. 

Cloud-Dienst kann mehrere IaaS VMs oder PaaS Rolleninstanzen, über die gleiche Cloud Service VIP ausgesetzt enthalten. Sie können auch [mehrere VIPs auf einen Clouddienst](../load-balancer/load-balancer-multivip.md)ermöglicht Multi-VIP-Szenarien wie Multi-Tenant-Umgebung mit SSL-basierten Websites.

Sie können sicherstellen, dass die öffentliche IP-Adresse eines Cloud-Diensts unverändert bleibt, auch wenn die Rolleninstanzen stehen mit eine *statische* öffentliche IP-Adresse als [Reservierte IP](virtual-networks-reserved-public-ip.md). Sie können eine statische (reservierte) IP-Ressource in einem bestimmten Verzeichnis erstellen und alle Cloud-Dienst an diesem Speicherort zuweisen. Die tatsächliche IP-Adresse kann nicht für die reservierten IP-Adresse angeben, wird von Pool verfügbarer IP-Adressen an der Erstellung zugeordnet. Diese IP-Adresse wird nicht freigegeben, bis Sie explizit löschen.

Statische (reservierte) öffentliche IP-Adressen werden in den Szenarien bei einem Clouddienst häufig verwendet:

- Firewall-Regeln werden von Endbenutzern benötigt.
- externe DNS-namensauflösung hängt und eine dynamische IP-Adresse eine Datensätze aktualisieren.
- nimmt externe Webdienste die IP-basierte Sicherheitsmodell verwenden.
- verwendet SSL-Zertifikate eine IP-Adresse verknüpft.

>[AZURE.NOTE] Beim Erstellen einer klassischen VM entsteht ein Container *Cloud-Dienst* von Azure hat eine virtuelle IP-Adresse (VIP). Nach Abschluss die Erstellung Portal, ist eine benutzerdefinierte RDP oder SSH *Endpunkt* vom Portal konfiguriert, Verbindung zu VM durch die Cloud Service VIP. Diese Cloud Service VIP kann die bietet effektiv einer reservierten IP-Adresse für die Verbindung zur VM reserviert. Weitere Endpunkte konfigurieren, um weitere Ports zu öffnen.

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs und PaaS-Instanzen
Sie können eine öffentliche IP-Adresse an eine IaaS [VM](../virtual-machines/virtual-machines-linux-about.md) oder PaaS Rolleninstanz in einem Clouddienst zuweisen. Dies wird auf Instanzebene öffentliche IP-Adresse ([ILPIP](virtual-networks-instance-level-public-ip.md)) bezeichnet. Diese öffentliche IP-Adresse kann nur dynamisch sein.

>[AZURE.NOTE] Dies unterscheidet sich von VIP ist ein Container für IaaS VMs oder PaaS Rolleninstanzen, da ein Cloud-Dienst enthalten mehrere IaaS VMs oder PaaS-Instanzen, alle im selben Cloud Service VIP offen gelegten Cloud-Dienst.

### <a name="vpn-gateways"></a>VPN-gateways
Ein [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) lässt sich ein Azure-VNet anderen Azure VNets oder lokalen Netzwerken herstellen. Eine VPN-Gateway ist eine öffentliche IP-Adresse *dynamisch*zugewiesen Kommunikation mit remote-Netzwerk ermöglicht.

### <a name="application-gateways"></a>Anwendungsgateways
Ein Azure [Application Gateway](../application-gateway/application-gateway-introduction.md) kann für Layer7 Lastenausgleich HTTP-basierte Netzwerk-Datenverkehr verwendet werden. Application Gateway ist eine öffentliche IP-Adresse *dynamisch*dient als Lastenausgleich VIP zugewiesen.

### <a name="at-a-glance"></a>Auf einen Blick
Die Tabelle zeigt jeden Ressourcentyp mit möglichen Zuteilung Methoden (dynamische/statische) und mehrere öffentliche IP-Adressen zuweisen.

|Ressource|Dynamische|Statische|Mehrere IP-Adressen|
|---|---|---|---|
|Cloud-Dienst|Ja|Ja|Ja|
|IaaS VM oder PaaS Rolleninstanz|Ja|Nein|Nein|
|VPN-gateway|Ja|Nein|Nein|
|Application gateway|Ja|Nein|Nein|

## <a name="private-ip-addresses"></a>Private IP-Adressen
Private IP-Adressen können Azure-Ressourcen mit anderen Ressourcen in einem Clouddienst oder [virtuellen Netzwerk](virtual-networks-overview.md)(VNet) oder lokalen Netzwerk (über ein VPN-Gateway oder ExpressRoute-Verbindung) kommunizieren ohne Internet erreichbaren IP-Adresse.

In Azure klassischen Bereitstellungsmodell kann Folgendes Azure eine private IP-Adresse zugewiesen werden:

- IaaS VMs und PaaS-Instanzen
- Interne Lastenausgleich
- Application gateway

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs und PaaS-Instanzen
Virtuelle Maschinen (VMs) mit dem klassischen Bereitstellungsmodell erstellt befinden sich immer in einem Clouddienst PaaS Rolleninstanzen ähnelt. Das Verhalten der privaten IP-Adressen sind daher für diese Ressourcen.

Es ist wichtig zu beachten, dass ein Cloud-Dienst auf zwei Arten bereitgestellt werden:

- Als *eigenständige* Cloud-Dienst, nicht in einem virtuellen Netzwerk ist.
- Als Teil eines virtuellen Netzwerks.

#### <a name="allocation-method"></a>Zuordnungsmethode
Bei einem *eigenständigen* Clouddienst erhalten Ressourcen eine private IP-Adresse reserviert *dynamisch* Azure-Rechenzentrum privaten IP-Adressbereich. Es kann nur für die Kommunikation mit anderen virtuellen Computern im selben Clouddienst verwendet werden. Diese IP-Adresse kann ändern, wenn die Ressource beendet und gestartet wird.

Bei einer Cloud innerhalb eines virtuellen Netzwerks bereitgestellt erhalten Ressourcen private IP-Adressen aus dem Adressbereich des zugeordneten Subnetze (gemäß der Netzwerkkonfiguration) zugeordnet. Diese privaten IP-Adressen können für die Kommunikation zwischen VMs innerhalb der VNet verwendet werden.

Bei Cloud-Services in einem VNet eine private IP-Adresse außerdem *dynamisch* (mit DHCP) standardmäßig zugewiesen. Wenn die Ressource beendet und gestartet wird, können ändern. Um sicherzustellen, dass die IP-Adresse übereinstimmt, müssen Sie die Methode der Verteilung auf *statische*und eine gültige IP-Adresse im entsprechenden Adressbereich.

Private Adressen werden für häufig verwendet:

 - VMs, die als Domänencontroller oder DNS-Server fungieren.
 - VMs, die Firewall-Regeln, IP-Adressen erfordern.
 - VMs mit Services von anderen apps durch eine IP-Adresse zugegriffen.

#### <a name="internal-dns-hostname-resolution"></a>Interne DNS-Hostname-Auflösung
Alle Azure VMs und PaaS-Instanzen sind mit [Azure verwaltet DNS-Server](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) standardmäßig konfiguriert, wenn Sie explizit benutzerdefinierte DNS-Server konfigurieren. Diese DNS-Server lösen internen Namen für virtuelle Computer und Instanzen, die im selben VNet oder Cloud-Dienst befinden.

Beim Erstellen einer VM Azure verwaltet DNS-Server eine Zuordnung für den Hostnamen in die private IP-Adresse hinzugefügt. Bei einer VM Multi-NIC ist der Hostname private IP-Adresse des primären NIC zugeordnet. Allerdings ist diese Zuordnungsinformationen auf Ressourcen innerhalb der gleichen Cloud-Dienst oder VNet.

Bei einem *eigenständigen* Cloud-Dienst können zum Auflösen von Hostnamen aller Instanzen von VMs-Rolle innerhalb der gleichen Cloud-Dienst Sie. Bei einer Cloud-Dienst in einem VNet werden Sie Hostnamen aller Instanzen VMs-Rolle innerhalb der VNet lösen.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne Lastenausgleich (ILB) und Application-gateways
Sie können **front-End** -Konfiguration eine [Interne Lastenausgleich Azure](../load-balancer/load-balancer-internal-overview.md) (ILB) oder ein [Azure Application Gateway](../application-gateway/application-gateway-introduction.md)eine private IP-Adresse zuweisen. Diese private IP-Adresse dient als interne Endpunkt nur für die Ressourcen in einem virtuellen Netzwerk (VNet) und remote-Netzwerken mit der VNet verbunden. Die front-End-Konfiguration können Sie entweder eine dynamische oder statische private IP-Adresse zuweisen. Sie können auch mehrere private IP-Adressen, Multi-vip-Szenarien zu ermöglichen.

### <a name="at-a-glance"></a>Auf einen Blick
Die Tabelle zeigt jeden Ressourcentyp mit möglichen Zuteilung Methoden (dynamische/statische) und mehrere private IP-Adressen zuweisen.

|Ressource|Dynamische|Statische|Mehrere IP-Adressen|
|---|---|---|---|
|VM (in einem *eigenständigen* Cloud-Dienst)|Ja|Ja|Ja|
|PaaS Rolleninstanz (in einem *eigenständigen* Cloud-Dienst)|Ja|Nein|Ja|
|VM oder PaaS Rolleninstanz (in VNet)|Ja|Ja|Ja|
|Interne Load Balancer front-end|Ja|Ja|Ja|
|Application Gateway-front-end|Ja|Ja|Ja|

## <a name="limits"></a>Grenzen

In der folgenden Tabelle zeigt Grenzen IP-Adressierung in Azure pro Abonnement. Sie können erhöhen die Standardgrenzwerte maximalen Grenzen basierend auf Ihren Bedarf [wenden](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

||Standardlimit|Maximale Anzahl|
|---|---|---|
|Öffentliche IP-Adressen (dynamisch)|5|Kontakt zum support|
|Reservierte öffentliche IP Adressen|20|Kontakt zum support|
|Öffentliche VIP pro Bereitstellung (Cloud-Dienst)|5|Kontakt zum support|
|Private VIP (ILB) pro Bereitstellung (Cloud-Dienst)|1|1|

Stellen Sie sicher, lesen Sie den vollständigen Satz von Azure [für Netzwerke beschränkt](azure-subscription-service-limits.md#networking-limits) .

## <a name="pricing"></a>Preisgestaltung

In den meisten Fällen können öffentliche IP-Adressen. Es gibt eine Schutzgebühr zusätzliche oder statische öffentliche IP-Adressen verwenden. Stellen Sie sicher, Sie verstehen die [Preisstruktur für öffentliche IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Unterschiede zwischen Ressourcen-Manager und klassischen Bereitstellung
Es folgt ein IP-Adressierung Funktionen Ressourcenmanager und klassischen Bereitstellungsmodell.

||Ressource|Classic|Ressourcen-Manager|
|---|---|---|---|
|**Öffentliche IP-Adresse**|VM|Als ein ILPIP (nur dynamisch)|Als eine öffentliche IP-Adresse (dynamisch oder statisch)|
|||Eine VM IaaS oder eine PaaS Rolleninstanz zugewiesen|Die VM-NIC zugeordnet|
||Internet mit Lastenausgleich|Als VIP (dynamisch) oder reservierten IP-Adresse (statisch)|Als eine öffentliche IP-Adresse (dynamisch oder statisch)|
|||Cloud-Dienst zugewiesen|Der Lastenausgleich front-End-Konfiguration zugeordnet|
||||
|**Private IP-Adresse**|VM|Als sich|Als eine private IP-Adresse|
|||Eine VM IaaS oder eine PaaS Rolleninstanz zugewiesen|Die VM-Netzwerkkarte zugewiesen|
||Interne Lastenausgleich (ILB)|ILB (dynamisch oder statisch) zugewiesen|Die ILB-front-End-Konfiguration (dynamisch oder statisch) zugewiesen|

## <a name="next-steps"></a>Nächste Schritte
- [Bereitstellen einer VM statische private IP-Adresse](virtual-networks-static-private-ip-classic-pportal.md) mit dem Verwaltungsportal.
