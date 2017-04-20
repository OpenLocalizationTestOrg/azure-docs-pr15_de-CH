<properties
   pageTitle="Azure Virtual Network (VNet) Planung und Design Guide | Microsoft Azure"
   description="Informationen Sie zum Planen und Entwerfen von virtuellen Netzwerken in Azure Ihre Bedürfnisse Isolation, Konnektivität und Speicherort."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/08/2016"
   ms.author="jdial" />

# <a name="plan-and-design-azure-virtual-networks"></a>Planen und Entwerfen von Azure Virtual Networks

Erstellen VNet Experimentieren mit einfach ist, aber wahrscheinlich werden Sie mehrere VNets auf die Produktion Bedürfnisse Ihrer Organisation bereitstellen. Planen und Entwerfen werden Sie VNets bereitstellen und effektiver benötigten Ressourcen verbinden. Wenn Sie nicht mit VNets vertraut sind, sollten [VNets lernen](virtual-networks-overview.md) und [zum Bereitstellen](virtual-networks-create-vnet-arm-pportal.md) einer vor. 

## <a name="plan"></a>Planen

Verständnis der Azure-Abonnements, Bereiche und Netzwerkressourcen ist entscheidend für den Erfolg. Die Liste der Faktoren unter können Sie als Ausgangspunkt. Wenn Sie die Aspekte verstehen, können Sie die Vorschriften für den Netzwerkentwurf definieren.

### <a name="considerations"></a>Hinweise

Bevor unter beantworten der Planung Fragen die folgenden Punkte:

- Alles, was Sie in Azure erstellen besteht aus einer oder mehreren Ressourcen. Virtual Machine (VM) ist eine Netzwerkkarte (NIC) verwendete Netzwerkschnittstelle ein VM ist eine Ressource die öffentliche IP-Adresse einer Netzwerkkarte ist eine Ressource, an die Netzwerkkarte, VNet ist eine Ressource.
- Ressourcen in einem [Azure-Region](https://azure.microsoft.com/regions/#services) und Abonnements erstellen. Und Ressourcen können nur an ein VNet in derselben Region und Abonnements sind vorhanden. 
- Mit einem [VPN-Gateway](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)-Azure können Sie VNets miteinander verbinden. Sie können auch VNets und Abonnements So verbinden.
- VNets können mit dem lokalen Netzwerk mit einer der [Optionen für die Netzwerkkonnektivität](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) in Azure. 
- Verschiedene Ressourcen können in [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md#resource-groups)erleichtern die Ressource als Einheit verwalten gruppiert. Eine Ressourcengruppe kann Ressourcen über mehrere Bereiche enthalten, als die Ressourcen mit dem gleichen Abonnement gehören.

### <a name="define-requirements"></a>Definieren des Bedarfs

Verwenden Sie die folgenden Fragen als Ausgangspunkt für Ihre Azure-Netzwerk.  

1. Welche Azure Speicherorte verwenden Sie Host VNets?
2. Müssen Sie die Kommunikation zwischen diesen Azure?
3. Müssen Sie die Kommunikation zwischen der Azure-VNet(s) und die lokalen Datacenter(s)?
4. Wie viele Infrastruktur als eine VMs Service (IaaS) services Cloud-Funktionen und Web apps für Ihre Lösung?
5. Müssen Sie Verkehr auf VMs (d. h. front-End-Webservern und Back-End-Datenbankservern) zu isolieren?
6. Müssen Sie mit virtuellen Appliances Datenverkehr steuern?
7. Benötigen Benutzer unterschiedliche Berechtigungen für unterschiedliche Azure Ressourcen?

### <a name="understand-vnet-and-subnet-properties"></a>Eigenschaften VNet und Subnetz verstehen

VNet und Subnetze Ressourcen definieren eine Sicherheitsgrenze für Arbeitslasten in Azure ausgeführt. Ein VNet kennzeichnet eine Auflistung von Adressräumen als CIDR-Blocks. 

>[AZURE.NOTE] Administratoren sind vertraut mit CIDR-Notation. Wenn Sie nicht mit CIDR [erfahren Sie mehr über](http://whatismyipaddress.com/cidr)vertraut sind.

VNets enthalten die folgenden Eigenschaften.

|Eigenschaft|Beschreibung|Nebenbedingungen|
|---|---|---|
|**Name**|VNet Namen|Zeichenfolge mit bis zu 80 Zeichen. Buchstaben, Zahlen, Unterstriche, Punkte oder Bindestriche enthalten. Muss mit einem Buchstaben oder einer Zahl beginnen. Muss mit einem Buchstaben, Zahl oder Unterstrich enden. Groß- oder Kleinbuchstaben enthält.|  
|**Speicherort**|Azure Speicherort (auch als Bereich bezeichnet).|Muss eine gültige Azure Speicherorte.|
|**addressSpace**|Auflistung von Adresspräfixe in CIDR-Notation VNet bilden.|Muss ein Array von gültigen CIDR-Adressblöcke, einschließlich der öffentlichen IP-Adressbereiche.|
|**Subnetze**|Auflistung der Subnetze, die das VNet bilden|sehen Sie die Eigenschaften der unten stehenden Tabelle.||
|**dhcpOptions**|Objekt, das eine erforderliche Eigenschaft enthält den Namen **DnsServers**.||
|**dnsServers**|Array von DNS-Server von der VNet. Wenn kein Server angegeben ist, wird Azure interne Auflösung verwendet.|Muss ein Array von bis zu 10 DNS-Server nach IP-Adresse.| 

Ein Subnetz ist ein VNet untergeordnete Ressource und hilft Segmente Adressräume innerhalb eines CIDR-Blocks mit IP-Adresspräfixe definieren. NICs können Subnetze hinzugefügt und VMs bietet Konnektivität für unterschiedliche Workloads verbunden.

Subnetze enthält die folgenden Eigenschaften. 

|Eigenschaft|Beschreibung|Nebenbedingungen|
|---|---|---|
|**Name**|Subnet-name|Zeichenfolge mit bis zu 80 Zeichen. Buchstaben, Zahlen, Unterstriche, Punkte oder Bindestriche enthalten. Muss mit einem Buchstaben oder einer Zahl beginnen. Muss mit einem Buchstaben, Zahl oder Unterstrich enden. Groß- oder Kleinbuchstaben enthält.|
|**Speicherort**|Azure Speicherort (auch als Bereich bezeichnet).|Muss eine gültige Azure Speicherorte.|
|**addressPrefix**|Einzelnes Adresspräfix bilden das Subnetz in CIDR-notation|Muss eine CIDR-Blocks, die Teil der VNet-Adressräume.|
|**networkSecurityGroup**|NSG im Subnetz zugewiesen|Siehe [NSGs](resource-groups-networking.md#Network-Security-Group)|
|**routeTable**|Route-Tabelle auf dem Subnetz angewendet|Siehe [UDR](resource-groups-networking.md#Route-table)|
|**ipConfigurations**|Auflistung der IP-Konfigurationsobjekte Netzwerkkarten mit dem Subnetz verbunden|[IP-](../resource-groups-networking.md#IP-configurations) Konfiguration|

### <a name="name-resolution"></a>Auflösung

Standardmäßig verwendet das VNet [Azure bereitgestellte Auflösung.](virtual-networks-name-resolution-for-vms-and-role-instances.md#Azure-provided-name-resolution) zum Auflösen von Namen innerhalb der VNet und im Internet. Wenn Ihr VNets Ihre Rechenzentren lokale Verbindung müssen Sie jedoch [eigene DNS-Server](virtual-networks-name-resolution-for-vms-and-role-instances.md#Name-resolution-using-your-own-DNS-server) zum Auflösen von Namen zwischen dem bereitstellen.  

### <a name="limits"></a>Grenzen

Überprüfen Sie Netzwerk Grenzwerte in [Azure beschränkt](../azure-subscription-service-limits.md#networking-limits) Artikel um sicherzustellen, dass Ihr Design mit den Grenzen Konflikt nicht. Grenzen können durch Öffnen einer Support-Ticket erhöht werden.

### <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffskontrolle (RBAC)

[Azure RBAC](../active-directory/role-based-access-built-in-roles.md) können unterschiedliche Benutzer unterschiedliche Ressourcen in Azure möglicherweise die Zugriffsebene steuern. So kann die Arbeit der ihren Erfordernissen entsprechend zu trennen. 

Soweit virtuelle Netzwerken handelt, beherrschen Benutzer **Netzwerk** Teilnehmerrolle Azure Resource Manager virtuelle Netzwerkressourcen. Ebenso beherrschen Benutzer die Rolle **Eines Beitragenden klassische Netzwerk** klassische virtuelle Netzwerkressourcen.

>[AZURE.NOTE] Sie können auch separate Belieben administrativen [Rollen erstellen](../active-directory/role-based-access-control-configure.md) .

## <a name="design"></a>Design

Wenn Sie die Antworten auf die Fragen im Abschnitt [Planen](#Plan) kennen, prüfen Sie Folgendes vor dem Definieren der VNets.

### <a name="number-of-subscriptions-and-vnets"></a>Anzahl von Abonnements und VNets

Sie sollten mehrere VNets in den folgenden Szenarien:

- **Virtuelle Computer, die in Azure Standorten platziert werden müssen**. VNets in Azure sind regionale. Sie können keine Standorte erstrecken. Daher benötigen Sie mindestens eine VNet für jede Azure Host VMs in die gewünschte Position.
- **Arbeitslasten, die vollständig voneinander isoliert werden müssen**. Sie können separate VNets erstellen, auch die gleiche IP-Adressbereiche unterschiedliche Arbeitslasten voneinander isolieren. 

Denken Sie daran, die oben angezeigten Grenzwerte pro Gebiet und pro Abonnement. Das bedeutet, dass mehrere Abonnements mit werden können die Ressourcen erhöhen in Azure unterhalten. Ein Standort-zu-Standort-VPN oder ExpressRoute-Verbindung können Sie um VNets in verschiedenen Subskriptionen zu verbinden.

### <a name="subscription-and-vnet-design-patterns"></a>Abonnement und Entwurfsmuster VNet

Die nachstehende Tabelle zeigt einige allgemeine Entwurfsmuster für Abonnements mit VNets.

|Szenario|Diagramm|Profis|Nachteile|
|---|---|---|---|
|Einem Abonnement, zwei VNets pro Anwendung|![Abonnement](./media/virtual-network-vnet-plan-design-arm/figure1.png)|Nur ein Abonnement verwalten.|Maximale Anzahl von VNets pro Azure. Sie benötigen mehrere Abonnements danach. Überprüfen des Artikels [Azure beschränkt](../azure-subscription-service-limits.md#networking-limits) .|
|Ein Abonnement pro Anwendung, zwei VNets pro Anwendung|![Abonnement](./media/virtual-network-vnet-plan-design-arm/figure2.png)|Nur zwei VNets pro Abonnement verwendet.|Schwieriger zu verwalten, wenn es zu viele apps.|
|Ein Abonnement pro Geschäftsbereich, zwei VNets pro Anwendung.|![Abonnement](./media/virtual-network-vnet-plan-design-arm/figure3.png)|Anzahl von Abonnements und VNets zwischen.|Maximale Anzahl von VNets pro Geschäftsbereich (Abonnement). Überprüfen des Artikels [Azure beschränkt](../azure-subscription-service-limits.md#networking-limits) .|
|Ein Abonnement pro Geschäftsbereich, zwei VNets pro apps.|![Abonnement](./media/virtual-network-vnet-plan-design-arm/figure4.png)|Anzahl von Abonnements und VNets zwischen.|Apps müssen mit Subnetzen und NSGs isoliert werden.|


### <a name="number-of-subnets"></a>Anzahl von Teilnetzen

Sie sollten mehrere Subnetze in einem VNet in den folgenden Szenarien:

- **Nicht genügend privaten IP-Adressen für alle Netzwerkkarten in einem Subnetz**. Wenn Ihr Subnetz-Adressraum nicht genügend IP-Adressen für die Anzahl an NICs im Subnetz enthält, müssen Sie mehrere Subnetze erstellen. Denken Sie daran, das Azure 5 private IP-Adressen jedes Subnetz reserviert, die verwendet werden kann: die ersten und letzten Adressen der Adressraum (Subnetzadresse und Multicast) und 3 Adressen intern (für DHCP und DNS) verwendet werden. 
- **Sicherheit**. Sie können mit der um VMs voneinander für Arbeitslasten voneinander zu trennen, die eine Struktur mit mehreren Ebene Subnetze und unterschiedlichen [Netzwerk-Sicherheitsgruppen (NSGs)](virtual-networks-nsg.md#subnets) für diese Subnetze wenden.
- **Hybrid-Konnektivität**. VPN-Gateways und ExpressRoute-Schaltkreise [Verbindung](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) können der VNets untereinander und Ihre lokalen Daten auszusuchen. VPN-Gateways und ExpressRoute Stromkreise erfordern ein Subnetz selbst erstellt werden.
- **Virtuelle Appliances**. Virtuelle Appliance wie Firewall, WAN-Beschleuniger oder VPN-Gateway können in einer Azure-VNet. Wenn Sie dies tun, [Datenverkehr](virtual-networks-udr-overview.md) an andere Appliances müssen und in einem eigenen Subnetz isolieren.

### <a name="subnet-and-nsg-design-patterns"></a>Subnetz und Entwurfsmuster NSG

Die nachstehende Tabelle zeigt einige allgemeine Entwurfsmuster für die Verwendung von Subnetzen.

|Szenario|Diagramm|Profis|Nachteile|
|---|---|---|---|
|Einzelnes Subnetz NSGs pro Anwendungsschicht pro Anwendung|![Einzelnes Subnetz](./media/virtual-network-vnet-plan-design-arm/figure5.png)|Nur ein Subnetz verwalten.|Mehrere NSGs notwendig, jede Anwendung isolieren.|
|Ein Subnetz pro Anwendung NSGs pro Anwendungsschicht|![Subnetz pro Anwendung](./media/virtual-network-vnet-plan-design-arm/figure6.png)|Weniger NSGs verwalten.|Mehrere Subnetze verwalten.|
|Ein Subnetz pro Anwendungsschicht NSGs pro Anwendung.|![Subnetz pro Schicht](./media/virtual-network-vnet-plan-design-arm/figure7.png)|Anzahl der Subnetze und NSGs zwischen.|Maximale Anzahl von NSGs pro Abonnement. Überprüfen des Artikels [Azure beschränkt](../azure-subscription-service-limits.md#networking-limits) .|
|Ein Subnetz pro Anwendungsschicht pro Anwendung NSGs pro Subnetz|![Subnetz pro Schicht pro Anwendung](./media/virtual-network-vnet-plan-design-arm/figure8.png)|Möglicherweise weniger NSGs.|Mehrere Subnetze verwalten.|

## <a name="sample-design"></a>Beispielentwurf

Zur Veranschaulichung der Anwendung die Informationen in diesem Artikel Szenario.

Sie arbeiten für ein Unternehmen 2 Rechenzentren in Nordamerika und zwei Rechenzentren Europa. Gekennzeichnet mit 6 verschiedene Applikationen verwaltet von 2 verschiedenen Unternehmenseinheiten in Azure als Pilot migrieren möchten. Die grundlegende Architektur für die Anwendung sind:

- App1, App2, App3 und App4 sind Web auf Ubuntu Linux Servern gehostet. Jede Anwendung verbindet in einen separaten Application Server hostet RESTful-Dienste auf Linux-Servern. Rest-Dienste Verbinden mit einer Back-End MySQL Datenbank.
- App5 und App6 sind Web-Applikationen auf Windows-Servern mit Windows Server 2012 R2. Jede Anwendung verbindet eine Back-End-SQL Server-Datenbank.
- Alle apps sind derzeit in einem Unternehmens-Rechenzentren in Nordamerika.
- Lokale Rechenzentren verwenden 10.0.0.0/8-Adressraum.

Sie müssen ein virtuelles Netzwerk-Lösung, der folgenden Mindestanforderungen erfüllt:

- Jedes Geschäftsbereichs sollten Ressourcenverbrauch durch andere Unternehmenseinheiten nicht betroffen.
- Minimieren Sie die Anzahl VNets und Subnetze Verwaltung vereinfachen.
- Jede Unternehmenseinheit müssen eine einzelne Test/Entwicklung für alle verwendeten VNet.
- Jede Anwendung wird in 2 verschiedene Azure Rechenzentren pro Kontinent (Nordamerika und Europa) gehostet.
- Jede Anwendung ist vollständig isoliert.
- Jede Anwendung kann von Kunden im Internet über HTTP zugegriffen werden.
- Jede Anwendung kann vom Benutzer über einen verschlüsselten Tunnel mit lokalen Rechenzentren verbunden zugegriffen werden.
- Verbindung zum lokalen Rechenzentren verwenden vorhandene VPN-Geräte.
- Netzwerkgruppe des Unternehmens muss Vollzugriff auf die VNet-Konfiguration.
- Entwickler in jeder Unternehmenseinheit sollte nur VMs auf bestehenden Subnet ausbringen können.
- Alle Programme werden wie Azure (anheben und Schicht) migriert.
- Datenbanken in jeder Standort sollte täglich Orte Azure replizieren.
- Jede Anwendung sollte 5 front-End-Webservern, 2 Anwendungsserver (bei Bedarf) und 2 Datenbankserver verwenden.

### <a name="plan"></a>Planen

Starten Sie die Planung von Frage im Abschnitt [definieren Anforderungen](#Define-requirements) wie unten dargestellt.

1. Welche Azure Speicherorte verwenden Sie Host VNets?

    2 Standorte in Nordamerika und 2 Standorten in Europa. Wählen Sie auf den Standort Ihrer vorhandenen lokalen Rechenzentren. So haben die Verbindung der Standorte in Azure eine bessere Latenz.

2. Müssen Sie die Kommunikation zwischen diesen Azure?

    Ja. Da die Datenbanken an allen Standorten repliziert werden müssen.

3. Müssen Sie die Kommunikation zwischen der Azure-VNet(s) und Ihre lokalen Daten auszusuchen bereitstellen?

    Ja. Da Benutzer Verbindung muss lokale Rechenzentren auf die Anwendung über einen verschlüsselten Tunnel.
 
4. Wie viele IaaS VMs benötigen Sie für Ihre Lösung?

    200 IaaS VMs. App1 App2 und App3 erfordern 5 Webserver 2 Applikationen Server und 2 Datenbankserver jeder. Das ist insgesamt 9 IaaS VMs pro Anwendung oder 36 IaaS VMs. App5 und App6 erfordern 5 Webserver und Datenbankserver 2. Das ist insgesamt 7 IaaS VMs pro Anwendung oder 14 IaaS VMs. Daher benötigen Sie 50 IaaS VMs für alle einzelnen Azure-Region. Da wir 4 Regionen müssen, werden 200 IaaS VMs.

    Sie müssen DNS-Server in jeder VNet oder Ihrer lokalen Rechenzentren zwischen Ihrem Azure IaaS VMs und lokalen Netzwerk aufgelöst. 

5. Müssen Sie Verkehr auf VMs (d. h. front-End-Webservern und Back-End-Datenbankservern) zu isolieren?

    Ja. Jede Anwendung sollte vollständig voneinander und jeder Anwendungsebene sollte isolated. 

6. Müssen Sie mit virtuellen Appliances Datenverkehr steuern?

    Nein. Virtuelle Appliances können zu mehr Kontrolle über den Datenverkehr, einschließlich detailliertere Daten Ebene Protokollierung verwendet werden. 

7. Benötigen Benutzer unterschiedliche Berechtigungen für unterschiedliche Azure Ressourcen?

    Ja. Networking-Team benötigt Vollzugriff auf das virtuelle Netzwerk, während Entwickler nur ihre VMs auf vorhandene Subnetzen bereitstellen können. 

### <a name="design"></a>Design

Führen Sie die Angabe von Abonnements, VNets, Subnetze und NSGs Design. NSGs werden hier erläutert sollten, jedoch über [NSGs](virtual-networks-nsg.md) vor Ihrem Entwurf.

**Anzahl von Abonnements und VNets**

Folgende beziehen sich auf Abonnements und VNets:

- Jedes Geschäftsbereichs sollten Ressourcenverbrauch durch andere Unternehmenseinheiten nicht betroffen.
- Sie sollten die VNets und Subnetze minimieren.
- Jede Unternehmenseinheit müssen eine einzelne Test/Entwicklung für alle verwendeten VNet.
- Jede Anwendung wird in 2 verschiedene Azure Rechenzentren pro Kontinent (Nordamerika und Europa) gehostet.

Basierend auf diesen Vorschriften, benötigen Sie ein Abonnement für jeden Geschäftsbereich. Auf diese Weise Ressourcen eines Konzernmandanten Grenzen für andere Geschäftsbereiche gelten nicht. Und da die Anzahl der VNets minimieren möchten, erwägen Sie **ein Abonnement pro Geschäftsbereich, zwei VNets pro apps** Muster wie folgt.

![Abonnement](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Außerdem müssen den Adressraum für jede VNet festlegen. Da Sie benötigen Konnektivität zwischen den lokalen Daten Rechenzentren Azure Regionen Adressraum zur Azure-VNets kann nicht in Konflikt stehen mit dem lokalen Netzwerk und von jeder VNet verwendeten Adressraum sollte nicht im Konflikt mit anderen vorhandenen VNets. Adressräume in der folgenden Tabelle können Sie diese Anforderung erfüllen.  

|**Abonnement**|**VNet**|**Azure-region**|**Adressbereich**|
|---|---|---|---|
|BU1|ProdBU1US1|Westen der USA|172.16.0.0/16|
|BU1|ProdBU1US2|Osten der USA|172.17.0.0/16|
|BU1|ProdBU1EU1|Nordeuropa|172.18.0.0/16|
|BU1|ProdBU1EU2|Westeuropa|172.19.0.0/16|
|BU1|TestDevBU1|Westen der USA|172.20.0.0/16|
|BU2|TestDevBU2|Westen der USA|172.21.0.0/16|
|BU2|ProdBU2US1|Westen der USA|172.22.0.0/16|
|BU2|ProdBU2US2|Osten der USA|172.23.0.0/16|
|BU2|ProdBU2EU1|Nordeuropa|172.24.0.0/16|
|BU2|ProdBU2EU2|Westeuropa|172.25.0.0/16|

**Anzahl der Subnetze und NSGs**

Folgende beziehen, Subnetze und NSGs:

- Sie sollten die VNets und Subnetze minimieren.
- Jede Anwendung ist vollständig isoliert.
- Jede Anwendung kann von Kunden im Internet über HTTP zugegriffen werden.
- Jede Anwendung kann vom Benutzer über einen verschlüsselten Tunnel mit lokalen Rechenzentren verbunden zugegriffen werden.
- Verbindung zum lokalen Rechenzentren verwenden vorhandene VPN-Geräte.
- Datenbanken in jeder Standort sollte täglich Orte Azure replizieren.

Basierend auf diesen Vorschriften, konnte Sie verwenden ein Subnetz pro Anwendungsebene und NSGs Filter Datenverkehr pro Anwendung verwenden. Auf diese Weise müssen Sie nur 3 Subnetze in jeder VNet (front-End, Anwendungsebene und Datenebene) und eine NSG pro Anwendung pro Subnetz. In diesem Fall sollten Sie mit **einem Subnetz pro Anwendungsschicht NSGs pro Anwendung** Entwurfsmuster. Die nachfolgende Abbildung zeigt die Verwendung des Entwurfsmusters **ProdBU1US1** VNet darstellt.

![Ein Subnetz pro Schicht eine NSG pro Anwendung pro Schicht](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Sie müssen jedoch auch zusätzliche Subnetz für VPN-Konnektivität zwischen der VNets und lokale Rechenzentren erstellen. Und müssen Sie den Adressraum für jedes Subnetz angeben. Die folgende Abbildung zeigt eine Beispielprojektmappe für **ProdBU1US1** VNet. Dieses Szenario würde für jeden VNet repliziert werden. Jede Farbe stellt eine andere Anwendung.

![Beispiel VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Zugriffskontrolle**

Steuerelement auf Folgendes beziehen:

- Netzwerkgruppe des Unternehmens muss Vollzugriff auf die VNet-Konfiguration.
- Entwickler in jeder Unternehmenseinheit sollte nur VMs auf bestehenden Subnet ausbringen können.

Basierend auf diesen Vorschriften, konnte Sie Benutzer vom networking-Team das integrierte **Netzwerk** Teilnehmerrolle in jedem Abonnement hinzufügen. und erstellen Sie eine benutzerdefinierte Rolle für die Anwendungsentwickler in jedem Abonnement, erteilen ihm Rechte bestehenden Subnet VMs hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen eines virtuellen Netzwerks](virtual-networks-create-vnet-arm-template-click.md) auf der Grundlage eines Szenarios.
- Verstehen, wie [Lastenausgleich](../load-balancer/load-balancer-overview.md) IaaS VMs und [routing über mehrere Regionen Azure verwaltet](../traffic-manager/traffic-manager-overview.md).
- Erfahren Sie mehr über [NSGs und zum Planen und Entwerfen](virtual-networks-nsg.md) einer NSG Lösung.
- Erfahren Sie mehr über die [standortübergreifende und Konnektivitätsoptionen VNet](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site).
