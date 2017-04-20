<properties 
   pageTitle="Ressourcen-Manager virtuelle Netzwerke im Portal klassische virtuelle Netzwerke Verbindung | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer VPN-Verbindungs zwischen classic VNets und Ressourcenmanager VNets VPN-Gateway mit dem portal"
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
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>Verbinden Sie virtuelle Netzwerke aus verschiedenen Modellen im portal

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure hat derzeit zwei Management: classic und Resource Manager (RM). Wenn Sie Azure für einige Zeit verwendet haben, haben Sie wahrscheinlich Azure VMs und Instanz Rollen im klassischen VNet. Ihr neuer VMs und Rolleninstanzen möglicherweise ein Ressourcen-Manager erstellten VNet ausgeführt werden. Dieser Artikel führt Sie durch VNets Ressourcenmanager die Ressourcen in separaten Bereitstellungsmodelle kommunizieren miteinander über ein gatewayverbindung zu klassischen VNets herstellen. 

Sie können eine Verbindung zwischen VNets erstellen, die in verschiedenen Subskriptionen und in anderen Regionen. Sie können auch VNets, die Verbindung mit lokalen Netzwerken bereits als dynamische oder Route-basierte Gateway mit konfiguriert ist. Weitere Informationen zu VNet VNet Verbindung finden Sie unter [VNet VNet FAQS](#faq) am Ende dieses Artikels.

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Bereitstellungsmodelle und Methoden VNet VNet Verbindung

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Wir aktualisieren in der folgenden Tabelle als neue Artikel und weitere Tools stehen für diese Konfiguration. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt aus der Tabelle.<br><br>

**VNet VNet**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Vor Beginn

Die folgenden Schritte durchlaufen müssen dynamische oder Route-basierte Gateway für jede VNet konfigurieren und eine VPN-Verbindung zwischen den Gateways Settings. Diese Konfiguration unterstützt keine statischen oder Policy-basierte Gateways. 

In diesem Artikel verwenden wir das Verwaltungsportal, Azure-Portal und PowerShell. Derzeit ist nicht möglich, diese Konfiguration nur Azure-Portal erstellen.

### <a name="prerequisites"></a>Erforderliche Komponenten

 - Beide VNets wurden bereits erstellt.
 - Adressbereiche für die VNets nicht mit einander überlappen oder überschneiden sich mit anderen Bereichen für andere Verbindungen Gateways angeschlossen sind.
 - Installiert die neueste PowerShell-Cmdlets (1.0.2 oder höher). Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Weitere Informationen. Stellen Sie sicher, dass Service-Management (SM) und Resource Manager (RM)-Cmdlets installieren. 

### <a name="values"></a>Konfigurieren

Beispieleinstellungen können Sie als Referenz.

**Klassisches VNet-Einstellung**

VNet Name = ClassicVNet <br>
Ort = Westen der USA <br>
Virtuelles Netzwerk Adressräume = 10.0.0.0/24 <br>
Subnetz 1 = 10.0.0.0/27 <br>
Subnetzname = 10.0.0.32/29 <br>
LAN-Name = RMVNetLocal <br>

**Ressourcenmanager VNet settings**

VNet Name = RMVNet <br>
Ressourcengruppe RG1 = <br>
Virtuelles Netzwerk IP-Adressbereiche = 192.168.0.0/16 <br>
Subnetz 1 = 192.168.1.0/24 <br>
Subnetzname = 192.168.0.0/26 <br>
Ort = East US <br>
Virtuelles Netzwerk-gatewayname = RMGateway <br>
Gateway öffentlicher IP-Name = Gwpip <br>
Gateway-Typ = VPN <br>
VPN-Typ = Route basiert <br>
Lokales Netzwerk-Gateway = ClassicVNetLocal <br>

## <a name="createsmgw"></a>Abschnitt 1: Konfigurieren Sie klassische VNet

In diesem Abschnitt werden im lokalen Netzwerk und das Gateway für das klassische VNet erstellen. Die Schritte in diesem Abschnitt verwenden Sie das Verwaltungsportal. Azure-Portal bietet derzeit nicht alle Einstellungen, die klassische VNet betreffen.

### <a name="part-1---create-a-new-local-network"></a>Teil 1: Erstellen eines neuen lokalen Netzwerks

Öffnen Sie das [Verwaltungsportal](https://manage.windowsazure.com) und mit der Azure-Konto anmelden.

1. Klicken Sie links unten auf dem Bildschirm auf **neu** > **Netzwerkdienste** > **Virtual Network** > **lokalen Netzwerk hinzufügen**.

2. Geben Sie im Fenster **der lokalen Netzwerk Angaben** ein RM VNet Sie möchten. Geben Sie im **VPN-Gerät IP-Adresse (optional)** eine gültige öffentliche IP-Adresse. Dies ist nur eine temporäre Platzhalter. Diese IP-Adresse ändern später. Klicken Sie rechts unten im Fenster auf die Taste.
 
3. Auf der Seite **Geben Sie den Adressbereich** im Feld **Erste IP-** Geben Sie den Netzwerk-Präfix und CIDR-Block für Ressourcenmanager VNet herstellen möchten. Diese Einstellung wird an den Adressraum an RM-VNet verwendet.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>Teil 2 – im lokale Netzwerk auf Ihre VNet zuordnen

1. Klicken Sie unter **Virtuelle Netzwerke** am oberen Rand der Seite auf dem Bildschirm virtuelle Netzwerke wechseln Klicken Sie das klassische VNet. Klicken Sie auf der Seite für das VNet **Konfigurieren** Navigieren zur Konfigurationsseite.

2. Abschnitt Verbindung **Standort-zu-Standort-Verbindung** das Kontrollkästchen Sie **Verbindung mit dem lokalen Netzwerk** . Wählen Sie im lokale Netzwerk, das Sie erstellt haben. Wenn Sie mehreren lokale Netzwerken, die Sie erstellt haben, müssen Sie auswählen, die Sie erstellt der Ressourcen-Manager-VNet aus.

3. Klicken Sie am unteren Rand der Seite **Speichern** .

### <a name="part-3---create-the-gateway"></a>Teil 3 - Gateway erstellen

1. Klicken Sie nach dem Speichern der Einstellungen auf **Dashboard** am oberen Rand der Seite auf die Seite Dashboard. Am Ende der Dashboardseite **Gateway erstellen**klicken **Dynamisches Routing**. Klicken Sie auf **Ja,** um zu Ihrem Gateway erstellen. Ein dynamisches Routing-Gateway ist für diese Konfiguration erforderlich.

2. Warten Sie auf das Gateway erstellt werden. Dadurch kann 45 Minuten oder länger dauern.

### <a name="ip"></a>Teil 4: Anzeigen der öffentlichen IP-Adresse des Gateways

Nach dem Erstellen das Gateway Gateway IP-Adresse auf **der Dashboardseite** angezeigt. Dies ist die öffentliche IP-Adresse Ihres Gateways. Notieren Sie oder kopieren Sie die öffentliche IP-Adresse. Sie verwenden sie später, wenn im lokale Netzwerk für Ihre Konfiguration Ressourcenmanager VNet erstellen.


## <a name="creatermgw"></a>Abschnitt 2: Ressourcen-Manager-VNet konfigurieren

In diesem Abschnitt werden das virtuelle Netzwerkgateway und das lokale Netzwerk für das Ressourcen-Manager-VNet erstellen. Starten Sie nicht folgende Schritte erst nachdem Sie öffentliche IP-Adresse für klassische VNet Gateway abgerufen haben.

Die Screenshots werden als Beispiele bereitgestellt. Achten Sie darauf, dass Sie die Werte ändern. Wenn Sie diese Konfiguration als Übung erstellen, finden Sie diese [Werte](#values).


### <a name="part-1---create-a-gateway-subnet"></a>Teil 1: Erstellen Sie ein

Vor dem Gateway virtuelle Netzwerk herstellen, müssen Sie für das virtuelle Netzwerk gatewaysubnetz erstellen Sie eine Verbindung herstellen möchten. Erstellen Sie ein mit CIDR /28 oder größer (/ 27, von 26, etc..)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

In einem Browser zu [Azure-Portal](http://portal.azure.com) und Azure-Konto anmelden.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Teil 2 – Erstellen Sie ein virtuelles Netzwerk-gateway


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Teil 3: Erstellen einer lokalen Netzwerk-gateway

'LAN-Gateway' verweist normalerweise auf die lokalen. Es teilt Azure die IP-Adresse zu den Speicherort und die öffentliche IP-Adresse des Geräts für diesen Standort reicht. Allerdings bezieht sich in diesem Fall sie Adressbereich und öffentliche IP-Adresse der klassischen VNet und virtuellen Netzwerk-Gateway zugeordnet.

Benennen Sie dem lokale Netzwerk-Gateway über den Azure darauf verweisen kann. LAN-Gateways können während der Erstellung der virtuellen Netzwerk-Gateway. Für diese Konfiguration verwenden Sie die öffentliche IP-Adresse, die das klassische VNet Gateway im [vorherigen Abschnitt](#ip)zugewiesen wurde.

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Teil 4: Kopieren der öffentlichen IP-Adresse

Nach der virtuellen Netzwerk-Gateway erstellen kopieren Sie die öffentliche IP-Adresse, die Gateway zugeordnet ist. Sie verwenden, wenn Sie die LAN-für das klassische VNet Einstellungen. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Abschnitt 3: Ändern Sie das lokale Netzwerk für klassische VNet

Öffnen Sie das [Verwaltungsportal](https://manage.windowsazure.com).

2. Im klassischen Portal links unten Sie und klicken Sie auf **Netzwerke**. Klicken Sie auf der Seite **Netzwerk** auf **Lokale Netzwerke** am oberen Rand der Seite. 

3. Klicken Sie auf das lokale Netzwerk, das Sie in Teil 1 konfiguriert. Klicken Sie am unteren Rand der Seite auf **Bearbeiten**.

4. Ersetzen Sie auf **Ihrem lokalen Netzwerk Angaben** Platzhalter IP-Adresse die öffentliche IP-Adresse für das Gateway Resource Manager, das Sie im vorherigen Abschnitt erstellt. Klicken Sie auf den Pfeil, um zum nächsten Abschnitt. Überprüfen Sie den **Adressraum** , und klicken Sie auf das Häkchen, um die Einstellung zu übernehmen.

## <a name="connect"></a>Abschnitt 4: Verbindung herstellen

In diesem Abschnitt erstellen Sie die Verbindung zwischen den VNets. Die Schritte für diese erfordern PowerShell. Verbindung kann keine entweder die Portale erstellen. Stellen Sie sicher, Sie heruntergeladen und installiert, Classic (SM) und Resource Manager (RM) PowerShell-Cmdlets.


1. Auf der Azure-Konto in der PowerShell-Konsole anmelden. Das folgende Cmdlet aufgefordert, die Anmeldeinformationen für Ihr Konto Azure. Nach der Anmeldung werden die Einstellungen heruntergeladen, sodass Azure PowerShell verfügbar sind.

        Login-AzureRmAccount 

    Abrufen Sie eine Liste der Azure-Abonnements haben mehrere Abonnements.

        Get-AzureRmSubscription

    Geben Sie das Abonnement, das Sie verwenden möchten. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. Fügen Sie Ihre Azure-Konto zum klassischen PowerShell-Cmdlets verwenden. Verwenden Sie hierzu den folgenden Befehl:

        Add-AzureAccount

3. Legen Sie Ihren freigegebenen Schlüssel durch Ausführen des folgenden Beispiels. In diesem Beispiel `-VNetName` ist der Name der klassischen VNet und `-LocalNetworkSiteName` ist der Name für das lokale Netzwerk angegeben Konfiguration im klassischen Portal. Die `-SharedKey` ist, Sie generieren und angeben können. Der hier angegebene Wert muss denselben Wert, den Sie im nächsten Schritt beim Erstellen der Verbindungs angeben.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. Erstellen Sie die VPN-Verbindung durch Ausführen der folgenden Befehle:
    
    **Die Variablen**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Erstellen der Verbindung**<br> Beachten Sie, dass die `-ConnectionType` ist IPsec, nicht 'Vnet2Vnet'. In diesem Beispiel `-Name` ist der Name die Verbindung aufgerufen. Das folgende Beispiel erstellt eine Verbindung mit dem Namen "*Rm Classic-Verbindung*".
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Überprüfen Sie die Verbindung

Sie können die Verbindung mit dem Verwaltungsportal, Azure-Portal oder PowerShell überprüfen. Die folgenden Schritte können Sie um Ihre Verbindung zu überprüfen. Ersetzen Sie die Werte durch Ihren eigenen.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="faq"></a>VNet VNet-FAQ

Häufig gestellte Fragen zu Details Weitere VNet VNet Verbindung anzeigen.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


