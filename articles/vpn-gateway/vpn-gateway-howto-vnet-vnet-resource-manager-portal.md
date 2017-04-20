<properties
   pageTitle="Verbinden mit dem Ressourcen-Manager-Bereitstellungsmodell und Azure-Portal VNets | Microsoft Azure"
   description="Erstellen Sie ein VPN-Gateway zwischen VNets mit Ressourcen-Manager und Azure-Portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Konfigurieren einer VNet VNet Verbindung mit Azure-portal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Classic - Verwaltungsportal](virtual-networks-configure-vnet-to-vnet-connection.md)


Dieser Artikel führt Sie durch die Schritte zum Erstellen einer Verbindung zwischen VNets in der Ressourcen-Manager-Bereitstellungsmodell mit VPN-Gateway und Azure-Portal.

Wenn Sie Azure-Portal, die virtuelle Netzwerke verwenden, muss die VNets dieselbe Abonnement. Sind virtuelle Netzwerke in verschiedenen Subskriptionen, können Sie diese weiterhin [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) wie verbinden.

![V2V-Diagramm](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Bereitstellungsmodelle und Methoden VNet VNet Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle zeigt die derzeit Bereitstellungsmodelle und Methoden VNet VNet Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>VNet VNet-Verbindungen

Ein virtuelles Netzwerk mit einem anderen virtuellen Netzwerk ähnelt (VNet VNet) ein VNet mit einem lokalen Standort. Beide Konnektivität verwenden eine Azure VPN-Gateway für einen sicheren Tunnel mit IPsec-IKE. VNets die Verbindung kann in unterschiedlichen Regionen oder in verschiedenen Subskriptionen.

Sie können auch VNet VNet Kommunikation mit standortübergreifende Konfigurationen kombinieren. Diese können Sie kombinieren Netzwerktopologien standortübergreifende Konnektivität zwischen virtuellen Netzwerkkonnektivität herstellen, wie im folgenden Diagramm dargestellt:

![Über Verbindungen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Über Verbindungen")

### <a name="why-connect-virtual-networks"></a>Warum verbinden virtuelle Netzwerke?

Möglicherweise möchten virtuelle Netzwerke aus den folgenden Gründen:

- **Cross-Region Geo-Redundanz und Geo-Präsenz**
    - Geo-Replikation oder Synchronisierung lassen mit sicheren sich ohne Internetzugriff Endpunkte.
    - Azure Traffic Manager und Lastenausgleich können Sie hochverfügbare Arbeitslast Geo-Redundanz über mehrere Azure Bereiche festlegen. Ein wichtiges Beispiel soll SQL ständig mit Verfügbarkeit über mehrere Azure-Regionen.

- **Regionale Multi-Tier-Anwendung mit Isolation oder administrative Grenze**
    - Innerhalb desselben Bereichs können Sie mit mehreren virtuellen Netzwerken isoliert oder Verwaltungsaufwand verbunden Multi-Tier-Anwendung einrichten.

Weitere Informationen zu VNet VNet Verbindung finden Sie unter [VNet VNet FAQS](#faq) am Ende dieses Artikels.

### <a name="values"></a>Konfigurieren

Bei folgendermaßen als Übung können Sie die Beispiel-Konfigurationswerte. Zu Beispielzwecken verwenden wir mehrere Adressräume für jedes VNet. VNet VNet Konfigurationen erfordern jedoch nicht mehrere Adressräume.

**Werte für TestVNet1:**

- VNet Name: TestVNet1
- Adressraum: 10.11.0.0/16
    - Subnet-Name: Front-End
    - Subnetzadressbereichs: 10.11.0.0/24
- Ressourcengruppe: TestRG1
- Ort: USA OST
- Adressraum: 10.12.0.0/16
    - Subnet-Name: Back-End
    - Subnetzadressbereichs: 10.12.0.0/24
- Gateway Subnet-Name: Subnetzname (Dies wird Automatisches Ausfüllen im Portal)
    - Gateway-Adresse Teilnetzes: 10.11.255.0/27
- DNS-Server: Verwenden Sie die IP-Adresse des DNS-Servers
- Virtuelles Netzwerk-Gateway-Name: TestVNet1GW
- Gateway-Typ: VPN
- VPN-Typ: Route basiert
- SKU: Wählen Sie die gewünschte Gateway Lagerhaltungsdaten
- Namen von öffentlichen IP-Adresse: TestVNet1GWIP
- Verbindung-Werte:
    - Name: TestVNet1toTestVNet4
    - Gemeinsamer Schlüssel: den gemeinsamen Schlüssel selbst erstellen. In diesem Beispiel verwenden wir abc123. Wichtig ist, dass beim Erstellen der Verbindung zwischen den VNets der Wert entsprechen muss.

**Werte für TestVNet4:**

- VNet Name: TestVNet4
- Adressraum: 10.41.0.0/16
    - Subnet-Name: Front-End
    - Subnetzadressbereichs: 10.41.0.0/24
- Ressourcengruppe: TestRG1
- Lage: Westen der USA
- Adressraum: 10.42.0.0/16
    - Subnet-Name: Back-End
    - Subnetzadressbereichs: 10.42.0.0/24
- Subnetzname Name: Subnetzname (Dies wird Automatisches Ausfüllen im Portal)
    - Subnetzname Bereich: 10.41.255.0/27
- DNS-Server: Verwenden Sie die IP-Adresse des DNS-Servers
- Virtuelles Netzwerk-Gateway-Name: TestVNet4GW
- Gateway-Typ: VPN
- VPN-Typ: Route basiert
- SKU: Wählen Sie die gewünschte Gateway Lagerhaltungsdaten
- Namen von öffentlichen IP-Adresse: TestVNet4GWIP
- Verbindung-Werte:
    - Name: TestVNet4toTestVNet1
    - Gemeinsamer Schlüssel: den gemeinsamen Schlüssel selbst erstellen. In diesem Beispiel verwenden wir abc123. Wichtig ist, dass beim Erstellen der Verbindung zwischen den VNets der Wert entsprechen muss.


## <a name="CreatVNet"></a>1. erstellen und Konfigurieren von TestVNet1

Wenn Sie bereits ein VNet überprüfen, ob die mit dem VPN-Gateway kompatibel sind. Achten Sie auf Subnetze, die mit anderen Netzwerken überlappen können. Wenn Sie überlappende Subnetze haben, funktioniert die Verbindung nicht ordnungsgemäß. Wenn Ihr VNet mit korrekt konfiguriert ist, können Sie die Schritte im Abschnitt [Geben Sie einen DNS-Server](#dns) beginnen.

### <a name="to-create-a-virtual-network"></a>Ein virtuelles Netzwerk erstellen

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. Fügen Sie zusätzliche Adressraum und erstellen Subnetze

Sie können zusätzliche Adressraum hinzufügen und Subnetze nach das VNet erstellt wurde.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. erstellen Sie ein

Vor dem Gateway virtuelle Netzwerk herstellen, müssen Sie für das virtuelle Netzwerk gatewaysubnetz erstellen Sie eine Verbindung herstellen möchten. Gegebenenfalls empfiehlt es sich, ein gatewaysubnetz eines CIDR-Blocks von /28 oder /27 um genügend IP-Adressen um zusätzliche zukünftigen Konfiguration Anforderungen erstellen.

Wenn Sie diese Konfiguration als Übung erstellen, lesen Sie diese [Konfigurieren](#values) gatewaysubnetz erstellen.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Erstellen Sie ein

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. Geben Sie einen DNS-Server (optional)

Möchten Sie Auflösung für virtuelle Maschinen, die für die VNets bereitgestellt haben, sollten Sie einen DNS-Server angeben.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="VNetGateway"></a>5. erstellen Sie 5. ein virtuelles Netzwerk-gateway

In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway für das VNet. Dieser Schritt kann bis zu 45 Minuten. Wenn Sie diese Konfiguration als Übung erstellen, finden Sie den [Konfigurieren](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Erstellen Sie ein virtuelles Netzwerk-gateway

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="CreateTestVNet4"></a>6. erstellen und Konfigurieren von TestVNet4

Nach Konfiguration TestVNet1 erstellen Sie TestVNet4 durch Wiederholen der Schritte die TestVNet4 Werte ersetzen. Sie müssen warten, bis das virtuelle Netzwerkgateway für TestVNet1 wurde vor TestVNet4 konfigurieren. Wenn Sie Ihre eigenen Werte verwenden, stellen Sie sicher, dass die Adressräume nicht mit der VNets überlappen, die Sie herstellen möchten.


## <a name="TestVNet1Connection"></a>7. konfigurieren Sie die TestVNet1-Verbindung

Nach Abschluss der virtuellen Netzwerkgateways für TestVNet1 und TestVNet4 können Sie virtuelle Netzwerk Gateway Verbindungen erstellen. In diesem Abschnitt erstellen Sie eine Verbindung von VNet1 auf VNet4.

1. **Alle Ressourcen**navigieren Sie zu der virtuellen Netzwerk-Gateway für das VNet. Beispielsweise **TestVNet1GW**. Klicken Sie auf **TestVNet1GW** , um das virtuelle Netzwerk Gateway Blatt öffnen.

    ![Connections-blade] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Connections-blade")

2. Klicken Sie auf **+ Hinzufügen** öffnen Blade **Verbindung hinzufügen** .

3. Geben Sie das Blade **Verbindung hinzufügen** im Namensfeld einen Namen für die Verbindung ein. Beispielsweise **TestVNet1toTestVNet4**.

    ![Name der Verbindung] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Name der Verbindung")

4. Für die **Verbindung**. Wählen Sie **VNet VNet** aus.

5. Der **erste virtuelle Netzwerkgateway** -Feldwert wird automatisch ausgefüllt, da diese Verbindung angegebenen virtuellen Netzwerk-Gateway erstellen.

6. **Zweite virtuelle Netzwerk-Gateway** -Feld ist als virtuelles Netzwerk-Gateway VNet, die eine Verbindung zu erstellen. Klicken Sie auf **Wählen Sie ein anderes virtuelles Netzwerk-Gateway** öffnen **Wählen Sie virtuellen Netzwerk-Gateway** -Blade.

    ![Verbindung hinzufügen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Hinzufügen einer Verbindung")

7. Zeigen Sie die aufgelisteten virtuellen Netzwerkgateways auf diesem Blatt an Beachten Sie, dass nur virtuelle Netzwerkgateways, die in Ihrem Abonnement aufgeführt sind. Verwenden Sie möchten ein virtuelles Netzwerk-Gateway, die nicht in Ihrem Abonnement enthalten ist, die [PowerShell-Artikel](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Klicken Sie auf Virtuelles Netzwerk-Gateway, dem Sie möchten.
 
9. Geben Sie im Feld **Shared Key** einen gemeinsamen Schlüssel für Ihre Verbindung. Sie generieren oder diesen Schlüssel zu erstellen. In einer Standort-zu-Standort-Verbindung wäre verwendeten Schlüssel genau das lokale Gerät und Ihr virtuelles Netzwerk-Gateway-Verbindung. Das Konzept ähnelt, außer als VPN-Gerät mit Anschluss an ein anderes virtuelles Netzwerk-Gateway;

    ![Shared key] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Shared key")

10. Klicken Sie auf **OK** unten das Blade die speichern.

## <a name="TestVNet4Connection"></a>8. konfigurieren Sie die TestVNet4-Verbindung

Anschließend erstellen Sie eine Verbindung von TestVNet4, TestVNet1. Verwenden Sie dieselbe Methode, die zum Erstellen der Verbindungs von TestVNet1 zu TestVNet4 verwendet. Stellen Sie sicher, dass Sie den gleichen gemeinsamen Schlüssel verwenden.

## <a name="VerifyConnection"></a>9. Überprüfen Sie die Verbindung

Überprüfen Sie die Verbindung. Folgendermaßen Sie für jedes Gateway virtuelles Netzwerk vor:

1. Suchen Sie das Blade für das virtuelle Netzwerkgateway. Beispielsweise **TestVNet4GW**. 
2. Klicken Sie auf Blade Gateway virtuelles Netzwerk **Verbindung** Verbindungen Blade für das virtuelle Netzwerkgateway anzuzeigen.

Zeigen Sie der Verbindung an und überprüfen. Nachdem die Verbindung hergestellt ist, wird **erfolgreich** und **verbunden** als die Statuswerte angezeigt werden.

![Erfolgreich] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Erfolgreich")

Doppelklicken Sie auf jede Verbindung einzeln, um weitere Informationen über die Verbindung.

![Grundlagen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Grundlagen")

## <a name="faq"></a>VNet VNet-FAQ

Häufig gestellte Fragen zu Details Weitere VNet VNet Verbindung anzeigen.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss die Verbindung können Sie virtuelle Computer, virtuelle Netzwerke hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
