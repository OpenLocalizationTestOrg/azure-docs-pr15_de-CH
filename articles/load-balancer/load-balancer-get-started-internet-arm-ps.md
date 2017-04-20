<properties
   pageTitle="Mithilfe einer Internetschnittstelle Lastenausgleich im Ressourcenmanager PowerShell | Microsoft Azure"
   description="Erstellen Sie ein Lastenausgleich Internetzugriff in Ressourcen-Manager mithilfe von PowerShell"
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

# <a name="get-started"></a>Erstellen einer Internetschnittstelle Lastenausgleich in Ressourcen-Manager mit PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager. Sie können auch [ein Lastenausgleich Internetzugriff mithilfe von klassischen Bereitstellungsmodell erstellen](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Bereitstellen der Lösung mit Azure PowerShell

Das folgende Verfahren wird ein Lastenausgleich Internetzugriff mit PowerShell Azure-Ressourcen-Manager erstellen. Azure Resource Manager jede Ressource erstellt und individuell konfiguriert und dann zu einem Lastenausgleich.

Sie müssen erstellen und konfigurieren Sie die folgenden Objekte um einen Lastenausgleich bereitzustellen:

- Front-End-IP-Konfiguration: enthält öffentliche IP (PIP) eingehenden Netzwerkverkehr.
- Back-End-Adresspool: Netzwerkkarten (NICs) für virtuelle Maschinen von Lastenausgleich der Netzwerkdatenverkehr enthält.
- Lastenausgleich Regeln: enthält Regeln, die einen Port im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.
- Eingehende NAT-Regeln: enthält Regeln, die einen Anschluss für einen bestimmten virtuellen Computer im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.
- Prüfpunkte: enthält Health Prüfpunkte Instanzen virtueller Computer im Back-End-Adresspool Verfügbarkeit verwendet.

Weitere Informationen finden Sie unter [Azure Resource Manager Unterstützung für Lastenausgleich](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Richten Sie PowerShell Ressourcenmanager

Stellen Sie sicher, dass Sie die neueste Version von Azure-Ressourcen-Manager-Modul für PowerShell:

1. Melden Sie sich bei Azure.

        Login-AzureRmAccount

    Geben Sie Ihre Anmeldeinformationen, wenn Sie aufgefordert werden.

2. Überprüfen Sie die Abonnements für das Konto.

        Get-AzureRmSubscription

3. Auswählen von Azure Abonnements verwenden.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Erstellen Sie eine Ressourcengruppe. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Erstellen Sie ein virtuelles Netzwerk und eine öffentliche IP-Adresse für den Front-End-IP-pool

1. Erstellen Sie ein Subnetz und ein virtuelles Netzwerk.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Erstellen einer Azure öffentlichen IP-Adressenressource, namens **PublicIP**ein Front-End-IP-Adresspool mit DNS-Namen **loadbalancernrp.westus.cloudapp.azure.com**verwendet werden. Der folgende Befehl verwendet die statische Zuordnung.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Lastenausgleich verwendet Domänennamens des öffentlichen IP als Präfix für seinen vollqualifizierten Domänennamen. Dies unterscheidet sich von der klassischen Bereitstellungsmodell, das Cloud-Dienst als Lastenausgleich FQDN verwendet.
    >In diesem Beispiel ist der FQDN **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Erstellen Sie ein Front-End-IP-Adresspool und Backend-Adresspool

1. Erstellen Sie einen Front-End-IP-Pool namens **LB-Frontend** , die **PublicIp** -Ressource verwendet.

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Erstellen Sie einen Back-End-Adresspool namens **LB-Backend**.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Erstellen Sie NAT-Regeln, Load Balancer Regel einen Prüfpunkt und ein Lastenausgleich

Dieses Beispiel erstellt die folgenden Elemente:

- Eine NAT-Regel alle eingehenden Datenverkehr auf Port 3389 Anschluss 3441 übersetzen
- Eine NAT-Regel alle eingehenden Datenverkehr auf Port 3389 Anschluss 3442 übersetzen
- Eine Prüfpunkt Regel überprüfen Sie den Status auf einer Seite mit dem Namen **HealthProbe.aspx**
- Load Balancer Regel Saldo aller Datenverkehr über Port 80 an Port 80 auf Adressen im Back-End-pool
- Ein Lastenausgleich, die diese Objekte verwendet.

Gehen Sie folgendermaßen vor:

1. Erstellen Sie die NAT-Regeln.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Erstellen Sie einen Integritätstest. Gibt es zwei Verfahren zum Konfigurieren einer Probe:

    HTTP-Prüfpunkt

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    TCP-Prüfpunkt

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Erstellen Sie eine Load Balancer Regel.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Erstellen Sie Lastenausgleich mithilfe der zuvor erstellten Objekte.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>Erstellen von NICs

Erstellen von Netzwerkschnittstellen (oder vorhandene ändern), und ordnen sie NAT-Regeln Load Balancer Regeln und Prüfpunkte:

1. Kommen Sie virtuelles Netzwerk und ein virtuelles Netzwerk-Subnetz Netzwerkkarten erstellt werden.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Erstellen Sie eine Netzwerkkarte namens **lb nic1 werden**, und die erste (und einzige) Backend-Adresspool mit der ersten Regel NAT zuordnen.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Erstellen Sie eine Netzwerkkarte namens **lb nic2 werden**, und die zweite NAT-Regel und die erste (und einzige) Backend-Adresspool zugeordnet.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Überprüfen Sie die Netzwerkkarten.

        $backendnic1

    Erwartete Ausgabe:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
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
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Verwenden der `Add-AzureRmVMNetworkInterface` Cmdlet verschiedene VMs Netzwerkkarten zuordnen.

## <a name="create-a-virtual-machine"></a>Erstellen Sie einen virtuellen Computer

Anleitung zum Erstellen eines virtuellen Computers und Zuweisen einer Netzwerkkarte finden Sie unter [Erstellen einer VM Azure mithilfe von PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Lastenausgleich Netzwerkschnittstelle hinzufügen

1. Lastenausgleich von Azure abrufen

    Laden Sie Load Balancer Ressource in eine Variable (falls Sie dies noch nicht getan haben). Die Variable wird **$lb**aufgerufen. Verwenden Sie denselben Namen Load Balancer Ressource, die Sie zuvor erstellt haben.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. Laden Sie die Back-End-Konfiguration eine Variable.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. Laden Sie die bereits erstellte Schnittstelle in eine Variable. Der Variablenname ist **$nic**. Die Schnittstelle wird derselbe aus dem früheren Beispiel.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. Ändern Sie die Back-End-Konfiguration auf der Schnittstelle.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Speichern Sie das Netzwerkschnittstellen-Objekt.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Nachdem eine Netzwerkschnittstelle Load Balancer Back-End-Pool hinzugefügt wird, wird basierend auf den Regeln des Lastenausgleichs für die Load-Balancer Ressource Netzwerkverkehr empfängt.

## <a name="update-an-existing-load-balancer"></a>Aktualisieren einer vorhandenen Lastenausgleich

1. Weisen Sie mit Lastenausgleich aus dem früheren Beispiel eine Load Balancer-Objekt an die Variable **$slb** `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. Im folgenden Beispiel wird eine eingehende NAT-Regel – mithilfe von Port 81 im Front-End-Pool und Port 8181 für Back-End-Pool--vorhandene Lastenausgleich hinzufügen.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. Speichern Sie die neue Konfiguration mit `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Ein Lastenausgleich entfernen

Verwenden Sie den Befehl `Remove-AzureLoadBalancer` löschen ein zuvor erstellten Lastenausgleich namens **NFP-LB** in einer Ressourcengruppe mit dem Namen **NFP RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Optionalen Schalter können **-Force** zu der Aufforderung zum Löschen.

## <a name="next-steps"></a>Nächste Schritte

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-ps.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)
