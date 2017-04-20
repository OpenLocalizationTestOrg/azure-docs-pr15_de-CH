<properties
   pageTitle="Öffentliche und private IP-Adressierung in Azure-Ressourcen-Manager | Microsoft Azure"
   description="Erfahren Sie mehr über öffentliche und private IP-Adressierung in Azure-Ressourcen-Manager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>IP-Adressen in Azure
Azure-Ressourcen mit anderen Azure Ressourcen, Ihrem lokalen Netzwerk und dem Internet kommunizieren können Sie IP-Adressen zuweisen. Es gibt zwei Arten von IP-Adressen in Azure verwenden:

- **Öffentliche IP-Adressen**: für die Kommunikation mit dem Internet, einschließlich Azure öffentliche Dienste
- **Private IP-Adressen**: für die Kommunikation innerhalb einer Azure virtual Network (VNet) und die lokalen Netzwerk, wenn ein VPN-Gateway oder ExpressRoute-Verbindung verwenden, um Ihr Netzwerk in Azure erweitern.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](virtual-network-ip-addresses-overview-classic.md).

Wenn Sie das klassische Bereitstellungsmodell vertraut sind, die [Unterschiede in IP-Adressen zwischen Classic und Resource Manager](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Öffentliche IP-Adressen
Öffentliche IP-Adressen können Azure Ressourcen Azure öffentliche Dienste wie [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-Datenbanken](../sql-database/sql-database-technical-overview.md)und [Azure-Speicher](../storage/storage-introduction.md)mit Internet kommunizieren.

Azure-Ressourcen-Manager ist eine [öffentliche IP-](resource-groups-networking.md#public-ip-address) Adresse einer Ressource, die über eigene Eigenschaften verfügt. Sie können eine öffentliche IP-Adressenressource folgende Ressourcen zuordnen:

- Virtuellen Maschinen (VM)
- Internetzugriff Lastenausgleich
- VPN-gateways
- Anwendungsgateways

### <a name="allocation-method"></a>Zuordnungsmethode
Es gibt zwei Methoden, die in denen *öffentlichen* IP-Ressource - *dynamische* oder *statische*IP-Adresse zugeordnet ist. Die Standard-Zuordnungsmethode ist *dynamische*, eine IP-Adresse **nicht** bei seiner Erstellung zugewiesen. Stattdessen wird die öffentliche IP-Adresse zugewiesen, beim Starten (oder erstellen) die zugeordnete Ressource (z. B. VM oder Load Balancer). Die IP-Adresse wird freigegeben, wenn Sie die Ressource beenden (oder löschen). Dadurch wird die IP-Adresse beim Beenden und eine Ressource ändern.

Um sicherzustellen, dass die IP-Adresse für die zugeordnete Ressource unverändert bleibt, können Sie die Zuordnungsmethode explizit *statische*fest. In diesem Fall wird sofort eine IP-Adresse zugewiesen. Es erscheint nur, wenn die Ressource löschen oder ändern die Zuordnungsmethode *dynamischen*.

>[AZURE.NOTE] Auch wenn die Zuordnungsmethode *statischen*festgelegt wird, kann nicht die tatsächliche IP-Adresse mit der *öffentlichen IP-Ressource*angeben Stattdessen ruft aus einem Pool verfügbarer IP-Adressen an Azure reserviert, erstellte Ressource in.

Statische öffentliche IP-Adressen werden häufig in den folgenden Szenarien verwendet:

- Endbenutzer müssen Firewallregeln Azure-Ressourcen zu aktualisieren.
- DNS-Auflösung, wo eine Änderung der IP-Adresse müsste ein Datensätze aktualisieren.
- Azure Ressourcen kommunizieren mit anderen apps oder Dienste, die eine IP-Adresse-Sicherheitsmodell verwenden.
- Sie verwenden Zertifikate in eine IP-Adresse verknüpft.

>[AZURE.NOTE] Die Liste der IP-Adressbereiche aus denen öffentliche IP-Adressen (Dynamic/statisch) Azure Ressourcen zugeordnet werden erscheint in [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>DNS-Auflösung von Hostnamen
Sie können eine DNS-Domäne Namen einer öffentlichen IP-Ressource angeben, die eine Zuordnung für *Domainnamelabel*erstellt. *Speicherort*. cloudapp.azure.com der öffentlichen IP-Adresse in Azure verwaltet DNS-Server. Beispielsweise wenn Sie eine öffentliche IP-Ressource mit **Contoso** als ein *Domainnamelabel* im **Westen der USA** Azure *Speicherort*lösen voll gekennzeichneten Domänennamen (FULLY Qualified Name) **contoso.westus.cloudapp.azure.com** der öffentlichen IP-Adresse der Ressource. Dieser FQDN können Sie eine benutzerdefinierte Domäne CNAME-Eintrag auf die öffentliche IP-Adresse in Azure erstellen.

>[AZURE.IMPORTANT] Bezeichnung für jede Domäne erstellt muss in Azure Standort eindeutig sein.  

### <a name="virtual-machines"></a>Virtuelle Computer
Sie können eine öffentliche IP-Adresse einer [Windows](../virtual-machines/virtual-machines-windows-about.md) oder [Linux](../virtual-machines/virtual-machines-linux-about.md) VM Zuordnen von der **Netzwerkschnittstelle**zugewiesen. Bei mehreren Netzwerkschnittstelle VM können Sie es nur die *primäre* Netzwerkschnittstelle zuweisen. Sie können einen virtuellen Computer eine dynamische oder statische öffentliche IP-Adresse zuweisen.

### <a name="internet-facing-load-balancers"></a>Internetzugriff Lastenausgleich
Load Balancer **Front-End** -Konfiguration zuweisen kann ein [Lastenausgleich Azure](../load-balancer/load-balancer-overview.md)eine öffentliche IP-Adresse zugeordnet werden. Diese öffentliche IP-Adresse dient als ein Lastenausgleich virtuelle IP-Adresse (VIP). Sie können einen Front-End-Lastenausgleich eine dynamische oder statische öffentliche IP-Adresse zuweisen. Sie können ein Lastenausgleich Front-End, [Multi-VIP](../load-balancer/load-balancer-multivip.md) -Szenarien wie eine Multi-Tenant-Umgebung mit SSL-basierte Websites ermöglicht, auch mehrere öffentliche IP-Adressen zuweisen.

### <a name="vpn-gateways"></a>VPN-gateways
[Azure VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) dient zur Azure virtuelles Netzwerk (VNet) andere Azure VNets oder einem lokalen Netzwerk herzustellen. Die **IP-Konfiguration** mit dem Remotenetzwerk Kommunikation eine öffentliche IP-Adresse zuweisen müssen. Derzeit können Sie nur eine *dynamische* öffentliche IP-Adresse ein VPN-Gateway zuweisen.

### <a name="application-gateways"></a>Anwendungsgateways
Die Gateway- **Front-End** -Konfiguration zuweisen können ein Azure [Application Gateway](../application-gateway/application-gateway-introduction.md)eine öffentliche IP-Adresse zugeordnet werden. Diese öffentliche IP-Adresse dient als VIP Lastenausgleich. Derzeit können Sie nur eine *dynamische* öffentliche IP-Adresse einer Anwendungskonfiguration Gateway Frontend zuweisen.

### <a name="at-a-glance"></a>Auf einen Blick
Folgende Tabelle zeigt die spezifische Eigenschaft über die öffentliche IP-Adresse möglich zugeordnete Ressource der obersten Ebene und der möglichen Zuteilung Methoden (dynamisch oder statisch) können.

|Ressource der obersten Ebene|Zuordnung der IP-Adresse|Dynamische|Statische|
|---|---|---|---|
|Virtual machine|Netzwerkschnittstelle|Ja|Ja|
|Lastenausgleich|Front-End-Konfiguration|Ja|Ja|
|VPN-gateway|Gateway-IP-Konfiguration|Ja|Nein|
|Application gateway|Front-End-Konfiguration|Ja|Nein|

## <a name="private-ip-addresses"></a>Private IP-Adressen
Private IP-Adressen können Azure-Ressourcen mit anderen Ressourcen in einem [virtuellen Netzwerk](virtual-networks-overview.md) oder einem lokalen Netzwerk über ein VPN-Gateway oder ExpressRoute-Verbindung kommunizieren ohne Internet erreichbaren IP-Adresse.

In Bereitstellungsmodell Azure-Ressourcen-Manager ist eine private IP-Adresse für die folgenden Typen von Azure-Ressourcen verknüpft:

- VMs
- Interne Lastenausgleich (ILBs)
- Anwendungsgateways

### <a name="allocation-method"></a>Zuordnungsmethode
Eine private IP-Adresse wird aus dem Adressbereich des Subnetzes zugewiesen, dem die Ressource zugeordnet ist. Adressbereich des Subnetzes selbst ist Teil der VNet-Adressbereich.

Es gibt zwei Methoden, die in der private IP-Adresse zugewiesen: *dynamisch* oder *statisch*. Zuordnungsmethode Standard ist *dynamische*, wo die IP-Adresse automatisch (mit DHCP) der Ressource Subnetz zugeordnet. Diese IP-Adresse kann beim Beenden und starten Sie die Ressource ändern.

Festlegen die Zuordnungsmethode *statische* um sicherzustellen, dass die IP-Adresse unverändert. In diesem Fall müssen Sie eine gültige IP-Adresse bereitstellen, die die Ressource Subnetz gehört.

Private Adressen werden für häufig verwendet:

- VMs, die als Domänencontroller oder DNS-Server fungieren.
- Ressourcen, die Firewall-Regeln, IP-Adressen erforderlich.
- Ressourcen von anderen apps/Ressourcen über eine IP-Adresse zugegriffen.

### <a name="virtual-machines"></a>Virtuelle Computer
Die **Schnittstelle** des [Windows](../virtual-machines/virtual-machines-windows-about.md) oder [Linux](../virtual-machines/virtual-machines-linux-about.md) VM eine private IP-Adresse zugewiesen. Bei mehreren Netzwerkschnittstelle VM wird jede Schnittstelle eine private IP-Adresse zugewiesen. Sie können die Methode der Verteilung als dynamische oder statische für eine Netzwerkschnittstelle.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Interne DNS-Hostname-Auflösung (für VMs)
Azure-VMs sind mit [Azure verwaltet DNS-Server](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) standardmäßig so konfiguriert, wenn Sie explizit benutzerdefinierte DNS-Server konfigurieren. Diese DNS-Server lösen internen Namen für virtuelle Computer in der gleichen VNet befinden.

Beim Erstellen einer VM Azure verwaltet DNS-Server eine Zuordnung für den Hostnamen in die private IP-Adresse hinzugefügt. Bei mehreren Netzwerkschnittstelle VM ist der Hostname der privaten IP-Adresse der primären Netzwerkschnittstelle zugeordnet.

VMs mit Azure verwaltet DNS-Server konfiguriert werden Hostnamen von VMs innerhalb ihrer VNet ihren privaten IP-Adressen auflösen.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interne Lastenausgleich (ILB) und Application-gateways
Sie können **front-End** -Konfiguration eine [Interne Lastenausgleich Azure](../load-balancer/load-balancer-internal-overview.md) (ILB) oder ein [Azure Application Gateway](../application-gateway/application-gateway-introduction.md)eine private IP-Adresse zuweisen. Diese private IP-Adresse dient als interne Endpunkt nur für die Ressourcen in einem virtuellen Netzwerk (VNet) und remote-Netzwerken mit der VNet verbunden. Die front-End-Konfiguration können Sie entweder eine dynamische oder statische private IP-Adresse zuweisen.

### <a name="at-a-glance"></a>Auf einen Blick
Folgende Tabelle zeigt die spezifische Eigenschaft, über die private IP-Adresse möglich zugeordnete Ressource der obersten Ebene und der möglichen Zuteilung Methoden (dynamisch oder statisch) können.

|Ressource der obersten Ebene|Zuordnung von IP-Adresse|Dynamische|Statische|
|---|---|---|---|
|Virtual machine|Netzwerkschnittstelle|Ja|Ja|
|Lastenausgleich|Front-End-Konfiguration|Ja|Ja|
|Application gateway|Front-End-Konfiguration|Ja|Ja|

## <a name="limits"></a>Grenzen

Grenzen für IP-Adressierung wird in der vollständigen in Azure [für Netzwerke beschränkt](azure-subscription-service-limits.md#networking-limits) . Diese Grenzwerte werden pro Region und Abonnements. Sie können erhöhen die Standardgrenzwerte maximalen Grenzen basierend auf Ihren Bedarf [wenden](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="pricing"></a>Preisgestaltung

Öffentliche IP-Adressen möglicherweise eine Gebühr. Erfahren Sie mehr über die IP-Adresse in Azure Preise überprüfen Sie die [IP-Adresse Preise](https://azure.microsoft.com/pricing/details/ip-addresses) Seite.

## <a name="next-steps"></a>Nächste Schritte
- [Bereitstellen einer VM mit öffentlichen Adresse mit Azure-portal](virtual-network-deploy-static-pip-arm-portal.md)
- [Bereitstellen einer VM mit öffentlichen Adresse mit einer Vorlage](virtual-network-deploy-static-pip-arm-template.md)
- [Bereitstellen einer VM eine statische private IP-Adresse über das Azure-portal](virtual-networks-static-private-ip-arm-pportal.md)
