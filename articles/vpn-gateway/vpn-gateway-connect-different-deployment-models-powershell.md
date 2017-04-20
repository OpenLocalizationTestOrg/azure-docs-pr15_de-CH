<properties 
   pageTitle="Ressourcen-Manager virtuelle Netzwerke mit PowerShell klassische virtuelle Netzwerke Verbindung | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer VPN-Verbindungs zwischen classic VNets und Ressourcenmanager VNets VPN-Gateway mit PowerShell"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Verbinden Sie virtuelle Netzwerke aus verschiedenen Modellen mit PowerShell

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure hat derzeit zwei Management: classic und Resource Manager (RM). Wenn Sie Azure für einige Zeit verwendet haben, haben Sie wahrscheinlich Azure VMs und Instanz Rollen im klassischen VNet. Ihr neuer VMs und Rolleninstanzen möglicherweise ein Ressourcen-Manager erstellten VNet ausgeführt werden. Dieser Artikel führt Sie durch VNets Ressourcenmanager die Ressourcen in separaten Bereitstellungsmodelle kommunizieren miteinander über ein gatewayverbindung zu klassischen VNets herstellen.

Sie können eine Verbindung zwischen VNets erstellen, die in verschiedenen Subskriptionen und in anderen Regionen. Sie können auch VNets, die Verbindung mit lokalen Netzwerken bereits als dynamische oder Route-basierte Gateway mit konfiguriert ist. Weitere Informationen zu VNet VNet Verbindung finden Sie unter [VNet VNet FAQS](#faq) am Ende dieses Artikels.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Bereitstellungsmodelle und Methoden zum Verbinden von VNets in verschiedenen Modellen

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Wir aktualisieren in der folgenden Tabelle als neue Artikel und weitere Tools stehen für diese Konfiguration. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt aus der Tabelle.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Vor Beginn

Die folgenden Schritte durchlaufen müssen dynamische oder Route-basierte Gateway für jede VNet konfigurieren und eine VPN-Verbindung zwischen den Gateways Settings. Diese Konfiguration unterstützt keine statischen oder Policy-basierte Gateways.

### <a name="prerequisites"></a>Erforderliche Komponenten

 - Beide VNets wurden bereits erstellt.
 - Adressbereiche für die VNets nicht mit einander überlappen oder überschneiden sich mit anderen Bereichen für andere Verbindungen Gateways angeschlossen sind.
 - Installiert die neueste PowerShell-Cmdlets (1.0.2 oder höher). Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Weitere Informationen. Stellen Sie sicher, dass Service-Management (SM) und Resource Manager (RM)-Cmdlets installieren. 

### <a name="exampleref"></a>Konfigurieren

Beispieleinstellungen können Sie als Referenz.

**Klassisches VNet-Einstellung**

VNet Name = ClassicVNet <br>
Ort = Westen der USA <br>
Virtuelles Netzwerk Adressräume = 10.0.0.0/24 <br>
Subnetz 1 = 10.0.0.0/27 <br>
Subnetzname = 10.0.0.32/29 <br>
LAN-Name = RMVNetLocal <br>
GatewayType = DynamicRouting

**Ressourcenmanager VNet settings**

VNet Name = RMVNet <br>
Ressourcengruppe RG1 = <br>
Virtuelles Netzwerk IP-Adressbereiche = 192.168.0.0/16 <br>
Subnetz 1 = 192.168.1.0/24 <br>
Subnetzname = 192.168.0.0/26 <br>
Ort = East US <br>
Gateway öffentlicher IP-Name = Gwpip <br>
Lokales Netzwerk-Gateway = ClassicVNetLocal <br>
Virtuelle Netzwerk-Gateway-Name = RMGateway <br>
Gateway-IP-Adressierung Konfiguration = Gwipconfig


## <a name="createsmgw"></a>Abschnitt 1: Konfigurieren das klassische VNet

### <a name="part-1---download-your-network-configuration-file"></a>Teil 1: Download der Netzwerk-Konfigurationsdatei

1. Melden Sie sich bei Ihrem Azure-Konto in der PowerShell-Konsole mit erhöhten Rechten. Das folgende Cmdlet aufgefordert, die Anmeldeinformationen für Ihr Konto Azure. Nach der Anmeldung gedownloadet Ihrer Konteneinstellungen, damit sie in Azure PowerShell verfügbar sind. Sie werden SM-PowerShell-Cmdlets verwenden, um diesen Teil der Konfiguration abzuschließen.

        Add-AzureAccount

2. Exportieren der Konfigurationsdatei Azure-Netzwerk durch Ausführen des folgenden Befehls. Ändern Sie den Speicherort der Datei an einem anderen Speicherort bei Bedarf exportieren. Sie bearbeiten die Datei und importieren sie in Azure.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Öffnen Sie die XML-heruntergeladene Datei, um sie zu bearbeiten. Beispielsweise der Netzwerk-Konfigurationsdatei finden Sie unter [Network Configuration Schema](https://msdn.microsoft.com/library/jj157100.aspx).
 

### <a name="part-2--verify-the-gateway-subnet"></a>Teil 2 – überprüfen das gatewaysubnetz

Fügen Sie im **VirtualNetworkSites** -Element ein die VNet noch nicht erstellt wurde hinzu. Beim Arbeiten mit der Netzwerk-Konfigurationsdatei gatewaysubnetz muss heißen "Subnetzname" oder Azure kann nicht erkannt und als ein.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Beispiel:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Teil 3 - LAN-Site hinzufügen

LAN-Website hinzugefügten stellt RM VNet Sie eine Verbindung herstellen möchten. Möglicherweise fügen Sie ein **LocalNetworkSites** Element Datei, sofern nicht bereits vorhanden ist. An diesem Punkt kann die Konfiguration der VPNGatewayAddress gültige öffentliche IP-Adresse, weil wir noch das Gateway für Ressourcenmanager VNet erstellt haben. Sobald wir das Gateway erstellen, ersetzen wir diese Platzhalter IP-Adresse die richtige öffentliche IP-Adresse, die RM-Gateway zugeordnet.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Teil 4: Verknüpfen der VNet mit dem lokalen Netzwerk-Standort

In diesem Abschnitt geben wir LAN-Site, der gewünschte VNet an. In diesem Fall ist der Ressourcen-Manager-VNet, auf die zuvor verwiesen. Vergewissern Sie sich die Namen übereinstimmen. Dadurch wird ein Gateway nicht erstellt. Es gibt im lokale Netzwerk Gateway herstellen.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Teil 5: Speichern und Hochladen

Speichern Sie die Datei dann in Azure durch Ausführen des folgenden Befehls importieren. Stellen Sie sicher, dass Sie den Pfad entsprechend Ihrer Umgebung ändern.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Sie sollten sehen ähnlich dadurch, dass der Import erfolgreich war.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Teil 6 - Gateway erstellen 

Sie können mit dem Verwaltungsportal oder PowerShell VNet-Gateway erstellen.

Im folgenden Beispiel wird dynamisches routing-Gateway zu verwenden:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Überprüfen Sie den Status des Gateways mit der `Get-AzureVNetGateway` Cmdlet.


## <a name="creatermgw"></a>Abschnitt 2: Konfigurieren des Gateways RM VNet

Erstellen Sie eine VPN-Gateway für RM VNet Anweisungen. Starten Sie nicht die Schritte bis, nachdem Sie öffentliche IP-Adresse für klassische VNet Gateway abgerufen haben. 

1. **Melden Sie sich bei Ihrem Azure-Konto** in der PowerShell-Konsole Das folgende Cmdlet aufgefordert, die Anmeldeinformationen für Ihr Konto Azure. Nach der Anmeldung werden die Einstellungen heruntergeladen, sodass Azure PowerShell verfügbar sind.

        Login-AzureRmAccount 

    Abrufen Sie eine Liste der Azure-Abonnements haben mehrere Abonnements.

        Get-AzureRmSubscription

    Geben Sie das Abonnement, das Sie verwenden möchten. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Erstellen einer lokalen Netzwerk-Gateway**. In einem virtuellen Netzwerk bezieht sich normalerweise an lokalen LAN Gateway. In diesem Fall bezieht sich das lokale Netzwerk-Gateway auf klassische VNet. Geben sie einen Namen mit dem Azure kann darauf verweisen und auch das Adresspräfix Speicherplatz. Azure verwendet IP-Adresspräfix, welcher Datenverkehr an Ihren lokalen Standort angeben. Möchten Sie die Informationen später anpassen, bevor Sie Ihr Gateway erstellen können Sie die Werte ändern und führen Sie das Beispiel erneut.<br><br>
    - `-Name`ist der Name auf das lokale Netzwerk-Gateway zuordnen möchten.<br>
    - `-AddressPrefix`der Adressraum wird für das klassische VNet.<br>
    - `-GatewayIpAddress`ist die öffentliche IP-Adresse der klassischen VNet-Gateway. Achten Sie darauf, dass im folgende Beispiel die richtige IP-Adresse entsprechend ändern.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Anfordern eine öffentliche IP-Adresse** Gateway des virtuellen Netzwerks für Ressourcenmanager VNet zugewiesen werden. Sie können nicht die IP-Adresse angeben, die Sie verwenden möchten. Die IP-Adresse wird dem virtuellen Netzwerk-Gateway dynamisch zugewiesen. Dies bedeutet jedoch nicht, dass die IP-Adresse geändert wird. Nur virtuelles Netzwerk-Gateway IP-Adressänderungen wird das Gateway gelöscht und neu erstellt wird. Ändert nicht ändern, zurücksetzen oder andere interne Wartung und Upgrades des Gateways.<br>In diesem Schritt haben wir auch eine Variable, die in einem späteren Schritt verwendet wird.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Überprüfen, ob das virtuelle Netzwerk hat ein**. Kein gatewaysubnetz vorhanden ist, fügen Sie hinzu. Stellen Sie sicher, dass gatewaysubnetz *Subnetzname*heißt.

5. **Abrufen des Subnetzes** für das Gateway durch Ausführen des folgenden Befehls verwendet. In diesem Schritt haben wir auch eine Variable, die im nächsten Schritt verwendet werden.
    - `-Name`ist der Name der Ressourcen-Manager-VNet.
    - `-ResourceGroupName`ist die Ressourcengruppe, der das VNet zugeordnet ist. Gatewaysubnetz ist für dieses VNet und muss den Namen *Subnetzname* ordnungsgemäß funktioniert.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Gateway-IP-Adressierung Konfiguration erstellen**. Die Gateway-Konfiguration definiert das Subnetz und die öffentliche IP-Adresse verwenden. Verwenden Sie Folgendes Beispiel, um Ihre Gateway-Konfiguration zu erstellen.<br>In diesem Schritt der `-SubnetId` und `-PublicIpAddressId` übergeben werden müssen die ID-Eigenschaft aus dem Subnetz und IP-Adressobjekte bzw.. Sie können keine einfache Zeichenfolge verwenden. Diese Variablen werden im Schritt festgelegt, eine öffentliche IP-Adresse und der Schritt im Subnetz abrufen.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **Erstellen virtuelle Netzwerkgateways Ressourcenmanager** durch Ausführen des folgenden Befehls. Die `-VpnType` müssen *RouteBased*sein. Es dauert mindestens 45 Minuten Abschluss.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **Kopieren Sie die öffentliche IP-Adresse** einmal VPN-Gateway wurde erstellt. Sie verwenden, wenn Sie die LAN-für das klassische VNet Einstellungen. Das folgende Cmdlet können Sie die öffentliche IP-Adresse abrufen. Die öffentliche IP-Adresse wird in der als *IP-Adresse*aufgeführt.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Abschnitt 3: Ändern Sie das lokale Netzwerk für klassische VNet

Dieser Schritt möglich durch die Netzwerk-Konfigurationsdatei exportieren, bearbeiten, speichern und die Datei importieren Azure zurück. Oder Sie können diese Einstellung im klassischen Portal. 

### <a name="to-modify-in-the-portal"></a>Das Portal ändern

1. Öffnen Sie das [Verwaltungsportal](https://manage.windowsazure.com).

2. Im klassischen Portal links unten Sie und klicken Sie auf **Netzwerke**. Klicken Sie auf der Seite **Netzwerk** auf **Lokale Netzwerke** am oberen Rand der Seite. 

3. Klicken Sie auf der Seite **Lokale Netzwerke** auf das lokale Netzwerk, Teil 1 konfiguriert und dann an das Ende der Seite gehen und klicken Sie auf **Bearbeiten**.

4. Ersetzen Sie auf **Ihrem lokalen Netzwerk Angaben** Platzhalter IP-Adresse die öffentliche IP-Adresse für das Gateway Resource Manager, das Sie im vorherigen Abschnitt erstellt. Klicken Sie auf den Pfeil, um zum nächsten Abschnitt. Überprüfen Sie den **Adressraum** , und klicken Sie auf das Häkchen, um die Einstellung zu übernehmen.

### <a name="to-modify-using-the-network-configuration-file"></a>So ändern Sie die Netzwerk-Konfigurationsdatei verwenden

1. Exportieren Sie Netzwerk-Konfigurationsdatei.
2. Ändern Sie in einem Texteditor VPN-Gateway IP-Adresse.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Speichern Sie und importieren Sie die bearbeitete Datei in Azure.


## <a name="connect"></a>Abschnitt 4: Erstellen Sie eine Verbindung zwischen den gateways

Erstellen einer Verbindung zwischen den Gateways erfordert PowerShell. Sie müssen Ihre Azure Konto, um die klassische PowerShell-Cmdlets hinzufügen. Verwenden Sie hierzu das folgende Cmdlet: 
        
    Add-AzureAccount

1. Durch Ausführen des folgenden Befehls **gemeinsam genutzten Schlüssel festlegen** . In diesem Beispiel `-VNetName` ist der Name der klassischen VNet und `-LocalNetworkSiteName` ist der Name für das lokale Netzwerk angegeben Konfiguration im klassischen Portal. Die `-SharedKey` ist, Sie generieren und angeben können. Der hier angegebene Wert muss denselben Wert, den Sie im nächsten Schritt beim Erstellen der Verbindungs angeben. Die Rückgabe für dieses Beispiel weisen **Status: erfolgreich**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. Erstellen Sie die VPN-Verbindung durch Ausführen der folgenden Befehle.

    **Die Variablen**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Erstellen der Verbindung**<br> Beachten Sie, dass die `-ConnectionType` IPsec nicht Vnet2Vnet ist.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. Die Verbindung können entweder Portal oder mithilfe der `Get-AzureRmVirtualNetworkGatewayConnection` Cmdlet.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>VNet VNet-FAQ

Häufig gestellte Fragen zu Details Weitere VNet VNet Verbindung anzeigen.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


