<properties 
    pageTitle="Simulierte Hybriden Cloudumgebung | Microsoft Azure" 
    description="Erstellen eine simulierten Hybriden Cloud-Umgebung für IT Pro- oder Entwicklungstests zwei Azure virtuelle Netzwerke mit einem VNet VNet-Verbindung." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Einrichten einer simulierten Hybriden Cloud-Umgebung für Tests

Dieser Artikel führt Sie durch eine simulierte Hybriden Cloud-Umgebung mit Microsoft Azure mit zwei Azure virtuelle Netzwerke erstellen. Hier ist die resultierende Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Dies simuliert eine hybride Cloud Produktionsumfeld und besteht aus:

- Eine simulierte und vereinfachte lokalen Netzwerk in Azure virtuelles Netzwerk (Testlabor virtuelles Netzwerk) gehostet.
- Ein simulierter standortübergreifende virtuelles Netzwerk in Azure (TestVNET) gehostet.
- Ein VNet VNet-Verbindung zwischen den beiden virtuellen Netzwerken.
- Sekundärer Domänencontroller im virtuellen Netzwerk TestVNET.

Dies bietet Basis und allgemeine Starten von denen zeigen:

- Entwickeln Sie und Testen Sie der Anwendung in einer simulierten Hybriden Cloud-Umgebung.
- Erstellen Sie der Computer, innerhalb des virtuellen Netzwerks Testlabor und innerhalb des virtuellen Netzwerks TestVNET Hybriden Cloud-basierten IT-Arbeitslasten simuliert Testkonfigurationen.

Es gibt vier Hauptphasen dieser hybriden Cloudumgebung einrichten:

1.  Konfigurieren Sie das virtuelle Netzwerk Testlabor.
2.  Erstellen Sie standortübergreifende virtuelles Netzwerk.
3.  VNet VNet VPN-Verbindung zu erstellen.
4.  Konfigurieren Sie DC2. 

Diese Konfiguration erfordert Azure-Abonnement. Haben Sie ein Abonnement MSDN oder Visual Studio finden Sie unter [monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Virtuelle Maschinen und virtuelle Netzwerkgateways in Azure entstehen einer laufenden Kosten beim Ausführen. Diese Kosten berechnet anhand der MSDN oder kostenpflichtiges Abonnement. Ein Azure VPN-Gateway wird als Satz von zwei virtuellen Maschinen Azure implementiert. Minimierung die Kosten die Umgebung erstellen und die benötigten und so schnell wie möglich ausführen.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Phase 1: Konfigurieren des virtuellen Netzwerks Testlabor

Verwenden der Anleitung im Thema [Base Konfiguration testen Umgebung](https://technet.microsoft.com/library/mt771177.aspx) Computer DC1, APP1 und CLIENT1 im Testlabor namens Azure virtuelle Netzwerk konfigurieren. 

Starten Sie nun eine Azure PowerShell Prompt.

> [AZURE.NOTE] Der folgende Befehl legt mit Azure PowerShell 1.0 und höher.

Melden Sie sich bei Ihrem Konto an.

    Login-AzureRMAccount

Abrufen der Namen mithilfe des folgenden Befehls.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Legen Sie Ihre Azure-Abonnement. Verwenden Sie dieselbe Abonnement, das Sie mit der Basiskonfiguration in Phase 1 erstellt. Alles innerhalb der Anführungszeichen, einschließlich der Zeichen mit dem richtigen Namen < und >.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Fügen Sie ein Testlabor virtuellen Netzwerk von der Grundkonfiguration Azure Gateway host verwendet wird.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Anschließend fordern Sie eine öffentliche IP-Adresse des Gateways für das Testlabor virtuelle Netzwerk zuweisen.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Erstellen Sie anschließend das Gateway.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Denken Sie daran, die neue Gateways mindestens 20 Minuten erstellen können.

Azure-Portal auf dem lokalen Computer verbinden Sie mit DC1 mit der CORP\User1. Um die CORP-Domäne so konfigurieren, dass Computer und Benutzer ihre lokalen Domänencontroller für die Authentifizierung verwenden, führen Sie diese Befehle ein auf Windows PowerShell-Befehlszeile auf DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Dies ist die aktuelle Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>Phase 2: TestVNET virtuelle Netzwerk erstellen

Zunächst TestVNET virtuelle Netzwerk erstellen und mit einer Network Security schützen.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Anschließend fordern Sie eine öffentliche IP-Adresse zum Gateway für das virtuelle Netzwerk TestVNET zugeteilt und das Gateway.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Dies ist die aktuelle Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>Phase 3: Erstellen Sie die VNet-VNet-Verbindung

Erstens erhalten eines vorinstallierten Schlüssels zufälligen kryptografisch, 32 Zeichen von Netzwerk- oder Systemadministrator. Anhand der Informationen auch Create [eine zufällige Zeichenfolge für einen vorinstallierten Schlüssel für IPsec](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) einen vorinstallierten Schlüssel.

Anschließend mit diesen Befehlen können VNet VNet VPN-Verbindung erstellen, die einige Zeit in Anspruch nehmen können.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Nach einigen Minuten sollte die Verbindung hergestellt werden.

Dies ist die aktuelle Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>Phase 4: Konfigurieren Sie DC2

Erstellen Sie zunächst einen virtuellen Computer für DC2. Diese Befehle an der Befehlszeile Azure PowerShell auf dem lokalen Computer ausführen.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Schließen Sie dann den neuen DC2 virtuellen Computer aus dem Azure-Portal.

Anschließend konfigurieren Sie eine Windows-Firewall-Regel zum Zulassen von Datenverkehr für grundlegende Konnektivität testen. Eine auf Windows PowerShell-Befehlszeile auf DC2 führen Sie diese Befehle aus.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

Der Pingbefehl sollten vier Antworten von IP-Adresse 10.0.0.4 führen. Dies ist ein Test der Datenverkehr über die Verbindung VNet VNet.

Fügen Sie zusätzliche Datenträger auf DC2 als neue Volume mit dem Laufwerkbuchstaben f.

1.  Klicken Sie im linken Bereich des Server-Managers auf **Datei und**klicken Sie dann auf **Datenträger**
2.  Klicken Sie im Inhaltsbereich in der **Festplatten** -Gruppe auf **Diskette 2** ( **Partition** **Unknown**festgelegt).
3.  Klicken Sie auf **Aufgaben**, und klicken Sie dann auf **Neues Volume**.
4.  Auf den Seite des Assistenten zu beginnen, klicken Sie auf **Weiter**.
5.  Wählen Sie auf der Seite Server und Datenträger **Datenträger 2**auf und klicken Sie auf **Weiter**. Wenn Sie aufgefordert werden, klicken Sie auf **OK**.
6.  Geben Sie die Größe der Seite Volume klicken Sie auf **Weiter**.
7.  Zuweisen einer Seite Laufwerk Buchstaben oder Ordner klicken Sie auf **Weiter**.
8.  Klicken Sie auf der Seite Wählen Sie Datei System Settings auf **Weiter**.
9.  Klicken Sie auf der Seite Auswahl bestätigen auf **Erstellen**.
10. Wenn Sie fertig sind, klicken Sie auf **Schließen**.

Anschließend konfigurieren Sie DC2 als Replikat-Domänencontroller für die Domäne "corp.contoso.com". Diese Befehle in der Befehlszeile von Windows PowerShell auf DC2 ausführen.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Beachten Sie, dass das Kennwort CORP\User1 und Directory Services-Dienstwiederherstellungsmodus (DSRM) Kennwort angeben und DC2 Neustart aufgefordert werden.

Das virtuelle Netzwerk TestVNET eigene DNS-Server (DC2) verfügt, müssen Sie das virtuelle Netzwerk TestVNET dieser DNS-Server konfigurieren.

1.  Klicken Sie im linken Bereich der Azure-Portal virtuelle Netzwerke und dann auf **TestVNET**.
2.  Klicken Sie auf **der Registerkarte** **DNS-Server**.
3.  Geben Sie im **primären DNS-Server** **192.168.0.4** 10.0.0.4 ersetzen.
4.  Klicken Sie auf **Speichern**.

Dies ist die aktuelle Konfiguration. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
Der simulierte Hybriden Cloud-Umgebung kann jetzt getestet.

## <a name="next-step"></a>Nächstes

- Richten Sie eine [Web-basierte branchenspezifische Anwendung](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) in dieser Umgebung.
