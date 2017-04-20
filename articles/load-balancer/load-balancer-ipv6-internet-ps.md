<properties
    pageTitle="Ein Lastenausgleich mit IPv6 mit PowerShell für Ressourcenmanager mit Internetzugriff erstellen | Microsoft Azure"
    description="Erstellen Sie ein Lastenausgleich mit IPv6 mit PowerShell für Ressourcenmanager mit Internetzugriff"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6 Azure Lastenausgleich dual-Stack, öffentliche IP-Adresse, nativen IPv6-, Mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Erste Schritte beim Erstellen ein Lastenausgleich mit IPv6 mit PowerShell für Ressourcenmanager mit Internetzugriff

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Vorlage](./load-balancer-ipv6-internet-template.md)

Ein Azure Lastverteiler ist ein Layer-4 (TCP, UDP)-Lastenausgleich. Lastenausgleich bietet hohe Verfügbarkeit durch verteilen eingehenden Datenverkehr fehlerfrei Dienstinstanzen mit Cloud-Services oder virtuelle Computer in einer Load Balancer. Azure Lastenausgleich können auch diese Dienste auf mehrere Ports oder mehrere IP-Adressen darstellen.

## <a name="example-deployment-scenario"></a>Beispielszenario für die Bereitstellung

Das folgende Diagramm veranschaulicht den Lastenausgleich Lösung in diesem Artikel bereitgestellt wird.

![Load Balancer Szenario](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

In diesem Szenario erstellen Sie Azure Folgendes:

- ein Internetzugriff zum Lastenausgleich mit einer IPv4- und IPv6-öffentliche Adresse
- zwei laden Netzwerklastenausgleich Regeln für private Endpunkte öffentliche VIPs zuordnen
- eine Verfügbarkeit, enthält zwei VMs
- zwei virtuelle Maschinen (VMs)
- eine virtuelle Netzwerkschnittstelle für jede VM mit IPv4- und IPv6-Adressen

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Bereitstellen der Lösung mit PowerShell Azure

Die folgenden Schritte anzeigen zum Erstellen einer PowerShell Azure Ressourcenmanager mit Lastenausgleich mit Internetzugriff Jede Ressource mit Azure-Ressourcen-Manager erstellt und konfiguriert, dann setzen zu einer Ressource.

Um einen Lastenausgleich bereitzustellen, erstellen und konfigurieren Sie die folgenden Objekte:

- Front-End-IP-Konfiguration – enthält öffentliche IP-Adressen für den eingehenden Netzwerkverkehr.
- Back-End-Adresspool - enthält Netzwerkkarten (NICs) für virtuelle Maschinen von Lastenausgleich der Netzwerkdatenverkehr.
- Regeln des Lastenausgleichs - enthält Regeln Port im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.
- Eingehende NAT-Regeln - enthält einen Anschluss für einen bestimmten virtuellen Computer im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.
- Prüfpunkte - Gesundheit Prüfpunkte verwendet, um Instanzen von virtuellen Maschinen im Back-End-Adresspool Verfügbarkeit enthält.

Weitere Informationen finden Sie unter [Azure Resource Manager Unterstützung für Lastenausgleich](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Richten Sie PowerShell Ressourcenmanager

Stellen Sie sicher, dass Sie die neueste Version von Azure-Ressourcen-Manager-Modul für PowerShell.

1. Melden Sie sich bei Azure

        Login-AzureRmAccount

    Geben Sie Ihre Anmeldeinformationen, wenn Sie aufgefordert werden.

2. Überprüfen Sie die Abonnements für das Konto

        Get-AzureRmSubscription

3. Auswählen von Azure Abonnements verwenden.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Erstellen einer Ressourcengruppe (überspringen Sie diesen Schritt, wenn eine vorhandene Ressourcengruppe verwenden)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Erstellen Sie ein virtuelles Netzwerk und eine öffentliche IP-Adresse für den Front-End-IP-pool

1. Erstellen Sie ein virtuelles Netzwerk mit einem Subnetz.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Erstellen Sie Azure öffentliche IP-Adresse (PIP) Ressourcen für die Front-End-IP-Adresspool.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Lastenausgleich verwendet Domänennamens des öffentlichen IP als Präfix für seinen vollqualifizierten Domänennamen. In diesem Beispiel sind die FQDNs, *lbnrpipv4.westus.cloudapp.azure.com* und *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Erstellen Sie ein Front-End-IP-Konfigurationen und einem Back-End-Adresspool

1. Erstellen Sie Front-End-Adresskonfiguration, der die erstellte öffentliche IP-Adressen verwendet.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Erstellen Sie Back-End-Adresspools.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>Erstellen Sie LB Regeln, NAT-Regeln einen Prüfpunkt und ein Lastenausgleich

Dieses Beispiel erstellt die folgenden Elemente:

- eine NAT-Regel alle Datenverkehr über Port 443 an Port 4443 übersetzen
- Load Balancer Regel um alle Datenverkehr über Port 80 an Port 80 auf Adressen im Back-End-Pool verteilen.
- Load Balancer Regel RDP-Verbindung auf die VMs auf Port 3389 zu.
- Überprüfen Sie den Status auf eine Seite mit dem Namen *HealthProbe.aspx* oder ein Dienst auf Port 8080 Prüfpunkt Regel
- ein Lastenausgleich, die diese Objekte verwendet.

1. Erstellen Sie die NAT-Regeln.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Erstellen Sie einen Integritätstest. Gibt es zwei Verfahren zum Konfigurieren einer Probe:

    HTTP-Prüfpunkt

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    oder TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    In diesem Beispiel werden wir die Prüfpunkte TCP verwenden.

3. Erstellen Sie eine Load Balancer Regel.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Lastenausgleich mithilfe der zuvor erstellten Objekte zu erstellen.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>Netzwerkkarten für Back-End-VMs erstellen

1. Erhalten Sie virtuelles Netzwerk und virtuellen Netzwerksubnet Netzwerkkarten sollten erstellt werden.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Erstellen Sie IP-Konfigurationen und Netzwerkkarten für die virtuellen Computer.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Erstellen Sie virtuelle Computer und weisen Sie der neu erstellten NICs zu

Weitere Informationen zum Erstellen eines virtuellen Computers finden Sie unter [Erstellen und einem Windows-Computer mit Ressourcen-Manager und Azure PowerShell vorkonfigurieren](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Verfügbarkeit und Speicher registrieren

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Erstellen Sie jede VM und weisen Sie der NICs erstellt zu

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Nächste Schritte

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-ps.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)
