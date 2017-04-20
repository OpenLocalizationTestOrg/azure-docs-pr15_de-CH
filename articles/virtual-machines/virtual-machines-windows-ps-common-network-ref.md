<properties
    pageTitle="Gemeinsames Netzwerk PowerShell Befehle VMs | Microsoft Azure"
    description="Allgemeine PowerShell-Befehlen zu begann ein virtuelles Netzwerk und die zugehörigen Ressourcen für virtuelle Computer."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Allgemeine Netzwerk Azure PowerShell-Befehlen für VMs

Wenn Sie einen virtuellen Computer erstellen möchten, müssen Sie ein [virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) oder über ein vorhandenes virtuelles Netzwerk VM hinzugefügt werden kann. Normalerweise Wenn Sie einen virtuellen Computer erstellen, müssen Sie auch erstellen, die in diesem Artikel beschriebenen Ressourcen.

Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen und an Ihrem Konto anmelden.

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen

Aufgabe | Befehl 
-------------- | -------------------------
Subnetz erstellen | $subnet1 = [New AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -Namen "Subnet_name" AddressPrefix - XX. X.X.X/XX<BR>$subnet2 = New AzureRmVirtualNetworkSubnetConfig-Namen "Subnet_name" AddressPrefix - XX. X.X.X/XX<BR><BR>Normale Netzwerk möglicherweise ein Subnetz für eine [Internet facing Lastenausgleich](../load-balancer/load-balancer-internet-overview.md) und ein separates Subnetz für eine [interne Lastenausgleich](../load-balancer/load-balancer-internal-overview.md). |
Erstellen eines virtuellen Netzwerks | $vnet = [New AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -Namen "Virtual_network_name" - ResourceGroupName "Resource_group_name"-Speicherort "Location_name" AddressPrefix - XX. X.X.X/XX-Subnetz $subnet1, $subnet2
Testen Sie einen eindeutigen Domänennamen | [Test-AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName "Domänenname"-Speicherort "Location_name"<BR><BR>Sie können einen DNS-Domänennamen einer [öffentlichen IP-Ressource](../virtual-network/virtual-network-ip-addresses-overview-arm.md), die eine Zuordnung für die öffentliche IP-Adresse domainname.location.cloudapp.azure.com in Azure verwaltet DNS-Server erstellt. Der Name kann nur Buchstaben, Zahlen und Bindestriche enthalten. Das erste und letzte Zeichen muss ein Buchstabe oder Zahl und der Domänenname muss innerhalb von Azure Standort. Wenn **True** zurückgegeben wird, ist der vorgeschlagene Name global eindeutig.
Erstellen Sie eine öffentliche IP-Adresse | $pip = [New AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -Namen "Ip_address_name" - ResourceGroupName "Resource_group_name" - DomainNameLabel "Domänenname"-Speicherort "Location_name" - AllocationMethod dynamisch<BR><BR>Die öffentliche IP-Adresse verwendet den Domänennamen, den Sie zuvor getestet und werden vom Front-End-Konfiguration von Lastenausgleich verwendet.
Erstellen Sie eine Front-End-IP-Konfiguration | $frontendIP = [New AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -Namen "Frontend_ip_name" - Öffentl.IP $pip<BR><BR>Front-End-Konfiguration umfasst die öffentliche IP-Adresse, die Sie für eingehenden Verkehr erstellt.
Einen Back-End-Adresspool erstellen | $beAddressPool = [New AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -Namen "Backend_pool_name"<BR><BR>Interne Adressen enthält für das Backend der Lastenausgleich erfolgt über eine Netzwerkschnittstelle.
Erstellen einer Abfrage | $healthProbe = [New AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) -Namen "Probe_name" - RequestPath 'HealthProbe.aspx'-Protokoll http-Port 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Gesundheit Prüfpunkte verwendet, um virtuelle Computer Instanzen im Back-End-Adresspool Verfügbarkeit enthält.
Erstellen einer Regel für Lastenausgleich | $lbRule = [New AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -Namen HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-$healthProbe Probe-Protokoll Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich an einen Port in der Back-End-Adresspool zuweisen.
Eine eingehende NAT-Regel erstellen | $inboundNATRule = [New AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -Namen "Rule_name" - FrontendIpConfiguration $frontendIP-Protokoll TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Enthält Regeln, die einen Anschluss für einen bestimmten virtuellen Computer in der Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.
Erstellen Sie ein System zum Lastenausgleich | $loadBalancer = "Resource_group_name" [Neu AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName-Namen "Load_balancer_name"-Speicherort "Location_name" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-Prüfpunkt $healthProbe
Erstellen einer Netzwerkschnittstelle | $nic1 = "Resource_group_name" [Neu AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName-Name der Netzwerkschnittstelle"Name"-Speicherort "Location_name" - Priv.IP-Adresse XX. X.X.X-Subnetz subnet2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] LoadBalancerInboundNatRule - $loadBalancer.InboundNatRules[0]<BR><BR>Erstellen Sie eine Netzwerkschnittstelle die öffentliche IP-Adresse und virtuelles Netzwerk-Subnetz, das Sie zuvor erstellt haben.
    
## <a name="get-information-about-network-resources"></a>Informationen Sie über Netzwerkressourcen

Aufgabe | Befehl 
-------------- | -------------------------
Liste virtuelle Netzwerke | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "Resource_group_name"<BR><BR>Listet alle virtuellen Netzwerke in der Ressourcengruppe.
Abrufen von Informationen über ein virtuelles Netzwerk | Get-AzureRmVirtualNetwork-Namen "Virtual_network_name" - ResourceGroupName "Resource_group_name"
Liste Subnetze in einem virtuellen Netzwerk | Get-AzureRmVirtualNetwork-Namen "Virtual_network_name" - ResourceGroupName "Resource_group_name" & #124; Wählen Sie Subnets
Informationen Sie zu einem Subnetz | [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -Namen "Subnet_name" - VirtualNetwork $vnet<BR><BR>Ruft Informationen über das Subnetz im angegebenen virtuellen Netzwerk. $Vnet Wert von Get-AzureRmVirtualNetwork zurückgegebene Objekt.
Liste IP-Adressen | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "Resource_group_name"<BR><BR>Listet die öffentlichen IP-Adressen in der Ressourcengruppe.
Liste zum Lastenausgleich | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "Resource_group_name"<BR><BR>Listet alle Lastenausgleich in der Ressourcengruppe.
Liste Netzwerkschnittstellen | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "Resource_group_name"<BR><BR>Listet alle Netzwerkschnittstellen in der Ressourcengruppe.
Informationen Sie über eine Netzwerkschnittstelle | Get-AzureRmNetworkInterface-Namen "Name der Netzwerkschnittstelle" - ResourceGroupName "Resource_group_name"<BR><BR>Ruft Informationen über eine bestimmte Netzwerkschnittstelle.
Die IP-Konfiguration einer Netzwerkschnittstelle | [Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -Namen "Ipconfiguration_name" NetworkInterface - $nic<BR><BR>Ruft Informationen über die IP-Konfiguration der angegebenen Schnittstelle. $Nic Wert von Get-AzureRmNetworkInterface zurückgegebene Objekt.

## <a name="manage-network-resources"></a>Verwalten von Netzwerkressourcen

Aufgabe | Befehl 
-------------- | -------------------------
Hinzufügen eines Subnetzes zu einem virtuellen Netzwerk | [Fügen Sie AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix-XX. X.X.X/XX-Name "Subnet_name" - VirtualNetwork $vnet<BR><BR>Fügt ein Subnetz ein vorhandenes virtuelles Netzwerk hinzu. $Vnet Wert von Get-AzureRmVirtualNetwork zurückgegebene Objekt.
Löschen eines virtuellen Netzwerks | [Entfernen AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -Namen "Virtual_network_name" - ResourceGroupName "Resource_group_name"<BR><BR>Entfernt das angegebene virtuelle Netzwerk aus der Ressourcengruppe.
Löschen einer Netzwerkschnittstelle | [Entfernen AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -Namen "Name der Netzwerkschnittstelle" - ResourceGroupName "Resource_group_name"<BR><BR>Entfernt die angegebene Schnittstelle aus der Ressourcengruppe.
Ein Lastenausgleich löschen | [Entfernen AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -Namen "Load_balancer_name" - ResourceGroupName "Resource_group_name"<BR><BR>Entfernt die angegebene Lastenausgleich aus der Ressourcengruppe.
Eine öffentliche IP-Adresse löschen | [Entfernen AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-Namen "Ip_address_name" - ResourceGroupName "Resource_group_name"<BR><BR>Entfernt die angegebene öffentliche IP-Adresse aus der Ressourcengruppe.

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie die Netzwerkschnittstelle, die Sie gerade erstellt [einen virtuellen Computer erstellen](virtual-machines-windows-ps-create.md).
- Erfahren Sie, wie [einen virtueller Computer mit mehreren Netzwerkschnittstellen erstellen](../virtual-network/virtual-networks-multiple-nics.md)können.
