<properties
   pageTitle="Erstellen einer internen Lastenausgleich mit PowerShell im Ressourcenmanager | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer internen Lastenausgleich mithilfe von PowerShell in Ressourcen-Manager"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-powershell"></a>Erstellen einer internen Lastenausgleich mithilfe von PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Die folgenden Schritte erläutert erstellen Sie eine interne Lastenausgleich PowerShell Azure-Ressourcen-Manager mit. Azure Resource Manager Elemente zu einem internen Lastenausgleich werden individuell konfiguriert und dann um ein Lastenausgleich.

Sie müssen zum Erstellen und konfigurieren Sie die folgenden Objekte um einen Lastenausgleich bereitzustellen:

- Front-End-IP-Konfiguration - konfiguriert die private IP-Adresse für eingehenden Verkehr
- Back-End-Adresspool - konfigurieren die Netzwerkschnittstellen Lastenausgleich Datenverkehr vom front-End-IP-Pool erhalten
- Lastenausgleich Regeln - Quelle und lokale Port-Konfiguration für den Lastenausgleich.
- Prüfpunkte - Status Integritätstest für Instanzen virtueller Computer konfiguriert.
- Eingehende NAT-Regeln - Portregeln um eine VM-Instanzen direkt konfiguriert.

Sie erhalten weitere Informationen zum Load Balancer Komponenten mit Azure Ressourcenmanager bei [Azure Resource Manager Unterstützung für Lastenausgleich](load-balancer-arm.md).

Die folgenden Schritte erläutert, wie ein Lastenausgleich zwischen zwei virtuellen Maschinen konfiguriert.


## <a name="setup-powershell-to-use-resource-manager"></a>PowerShell Setup Resource Manager verwenden

Sicher haben die neueste Version des Moduls Azure PowerShell und PowerShell Setup ordnungsgemäß auf der Azure-Abonnement haben.

### <a name="step-1"></a>Schritt 1

        Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto

        Get-AzureRmSubscription

Sie werden authentifizieren mit Ihren Anmeldeinformationen aufgefordert.<BR>

### <a name="step-3"></a>Schritt 3

Auswählen von Azure Abonnements verwenden. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Erstellen Sie Ressourcengruppen für Lastenausgleich

### <a name="step-4"></a>Schritt 4

Erstellen Sie eine neue Ressourcengruppe (überspringen Sie diesen Schritt, wenn eine vorhandene Ressourcengruppe verwenden)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure Ressourcen-Manager muss alle Ressourcengruppen einen Speicherort angeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle erstellen ein Lastenausgleich die gleichen Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens "NFP RG" und "West US" Position.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Erstellen Sie virtuelles Netzwerk und eine private IP-Adresse für front-End-IP-pool


### <a name="step-1"></a>Schritt 1

Erstellt ein Subnetz für das virtuelle Netzwerk und Variablen $backendSubnet zugewiesen

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Erstellen eines virtuellen Netzwerks:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Das virtuelle Netzwerk erstellt und das virtuelle Netzwerk NRPVNet lb-Subnetz sein Subnetz hinzugefügt und Variablen $vnet zugewiesen



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Erstellen Sie Front End-IP-Pool und Back-End-Adresspool

Einrichten eines front-End-IP-Adresspools Verkehr, für der eingehende Load Balancer Netzwerk Datenverkehr und Back-End-Adresspool erhalten die Last ausgeglichen.

### <a name="step-1"></a>Schritt 1

Erstellen Sie einen front-End-IP-Adresspool Subnetz 10.0.2.0/24 die Endpunkt des eingehenden Datenverkehr über die private IP-Adresse 10.0.2.5.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>Schritt 2

Richten Sie ein Back-End-Adresspool für eingehenden Datenverkehr vom front-End-IP-Pool empfangen:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>LB, NAT-Regeln, Prüfpunkt erstellen und Lastenausgleich

Nach dem Pool IP-front-End und Back-End-Adresspool erstellen, müssen Sie Regeln erstellen, die Load Balancer Ressource gehören:

### <a name="step-1"></a>Schritt 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


Im obigen Beispiel wird Folgendes erstellt:

- NAT-Regel die gesamte eingehender Datenverkehr an Port 3441 auf Port 3389 geht.
- eine zweite NAT-Regel die gesamte eingehender Datenverkehr an Port 3442 auf Port 3389 geht.
- eine Load Balancer Regel laden Saldo aller eingehenden Datenverkehr an Port 80 an lokalen Anschluss 80 im Back-End-Adresspool öffentlichen.
- eine Probe Regel den Status für den Pfad "HealthProbe.aspx überprüft"



### <a name="step-2"></a>Schritt 2

Addieren alle Objekte (NAT, Load Balancer Regeln, Prüfpunkt Konfigurationen) Lastenausgleich zu erstellen:

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Erstellen von Netzwerk-Schnittstellen

Nach dem Erstellen der internen Lastenausgleich, müssen Sie definieren, welche Netzwerkschnittstellen eingehenden Netzwerkverkehr Lastenausgleich NAT-Regeln und Probe erhalten. Die Netzwerkschnittstelle ist in diesem Fall einzeln konfiguriert und später zu einer virtuellen Maschine zugewiesen werden kann.


### <a name="step-1"></a>Schritt 1


Abrufen der Ressource virtuelles Netzwerk und Subnetz Netzwerkschnittstellen erstellen:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Dieser Schritt erstellt eine Netzwerkschnittstelle Load Balancer Back-End Pool angehören und die erste NAT-Regel für diese Netzwerkschnittstelle für RDP zuordnen:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Schritt 2

Erstellen Sie eine zweite Netzwerkschnittstelle bezeichnet LB Nic2 werden:

Dieser Schritt erstellt eine zweite Netzwerkschnittstelle für RDP Load Balancer-Back-End dieselben zuweisen und Zuordnen der zweiten NAT-Regel erstellt:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Das Endergebnis wird Folgendes angezeigt:

    $backendnic1

Erwartete Ausgabe:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Schritt 3

Verwenden Sie den Befehl Add-AzureRmVMNetworkInterface die Netzwerkkarte zu einer virtuellen Maschine zuweisen.

Finden Sie die Anleitung einen virtuellen Computer erstellen und Zuweisen einer Netzwerkkarte nach der Dokumentation: [Erstellen einer VM Azure mithilfe von PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

Wenn Sie bereits einen virtuellen Computer erstellt haben, können Sie die Netzwerkschnittstelle mit den folgenden Schritten hinzufügen:

#### <a name="step-1"></a>Schritt 1

Laden Sie Load Balancer Ressource in eine Variable (falls Sie dies noch nicht getan haben). Die Variable heißt $lb und Verwenden von oben erstellte Load Balancer Ressource.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Schritt 2

Laden Sie die Back-End-Konfiguration einer Variablen.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Schritt 3

Laden Sie die bereits erstellte Schnittstelle in eine Variable. Der Variablenname ist $ NIC. Der Netzwerk-Schnittstellenname wird aus dem obigen Beispiel identisch.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Schritt 4

Ändern Sie die Back-End-Konfiguration auf der Schnittstelle.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Schritt 5

Speichern Sie das Netzwerkschnittstellen-Objekt.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Nachdem eine Netzwerkschnittstelle Load Balancer Back-End-Pool hinzugefügt wird, wird basierend auf den Lastenausgleich Regeln für diese Ressource Load Balancer Netzwerkverkehr empfängt.

## <a name="update-an-existing-load-balancer"></a>Aktualisieren einer vorhandenen Lastenausgleich


### <a name="step-1"></a>Schritt 1

Variable $slb mit Get-AzureRmLoadBalancer mit Lastenausgleich aus dem obigen Beispiel, Load Balancer-Objekt zuweisen

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Schritt 2

Im folgenden Beispiel wird eine neue NAT eingehende Regel 81 im front-End-Port und Port 8181 für den Back-End-Pool vorhandenen Lastenausgleich hinzufügen

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Schritt 3

Speichern Sie die neue Konfiguration mit Set-AzureLoadBalancer

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Ein Lastenausgleich entfernen

Verwenden Sie den Befehl AzureRmLoadBalancer entfernen ein zuvor erstellten Lastenausgleich namens "NFP-LB" in einer Ressourcengruppe namens "NFP RG" löschen

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Sie können die optionale switch - Kraft zu der Aufforderung zum Löschen.



## <a name="next-steps"></a>Nächste Schritte

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)