<properties
   pageTitle="Erstellen Sie ein virtuelles Netzwerk mit einer Standort-zu-Standort-VPN-Verbindung Azure Resource Manager und Azure-Portal | Microsoft Azure"
   description="Das VNet mit dem Ressourcen-Manager-Bereitstellungsmodell erstellen und verbinden Sie es mit Ihrem lokalen Netzwerk eine Verbindung S2S VPN-Gateway."
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
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Erstellen Sie ein VNet mit einer Standort-zu-Standort-Verbindung mit Azure-portal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Classic - Verwaltungsportal](vpn-gateway-site-to-site-create.md)


Dieser Artikel führt Sie durch das Erstellen eines virtuellen Netzwerks und ein Standort-zu-Standort-VPN-Gateway mit dem lokalen Netzwerk Bereitstellungsmodell Azure Ressourcenmanager und Azure-Portal. Standort-zu-Standort-Verbindung können für standortübergreifende und Hybrid verwendet Konfigurationen.

![Diagramm](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Bereitstellung und-Modelle für Standort-zu-Standort-Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Die folgende Tabelle zeigt die derzeit Bereitstellungsmodelle und Methoden für Standort-zu-Standort-Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Zusätzliche Konfigurationen 

Wenn Sie VNets verbinden, aber keine Verbindung zu einem lokalen Speicherort erstellen, finden Sie unter [Konfigurieren einer Verbindung VNet VNet](vpn-gateway-vnet-vnet-rm-ps.md). Eine Standort-zu-Standort-Verbindung zu einem VNet hinzufügen, die bereits eine Verbindung, finden Sie unter [Hinzufügen einer S2S-Verbindung zu einem VNet mit einer vorhandenen VPN-Gateway-Verbindung](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Bevor Sie beginnen

Überprüfen Sie Folgendes vor der Konfiguration:

- Ein kompatibles VPN-Gerät und wer konfigurieren kann. [VPN-Geräte](vpn-gateway-about-vpn-devices.md)finden Sie unter. Vertraut mit Ihrem VPN-Gerät konfiguriert oder sind nicht mit der IP-Adresse befindet sich Bereiche in Ihrem lokalen Netzwerk-Konfiguration, müssen Sie diese Angaben können Sie mit anderen Personen koordinieren.

- Eine nach außen gerichtete öffentliche IP-Adresse für das VPN-Gerät. Diese IP-Adresse kann nicht hinter einem NAT befinden
    
- Ein Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](http://azure.microsoft.com/pricing/free-trial/)aktivieren.

### <a name="values"></a>Beispielwerte für diese Übung Konfiguration


Bei folgendermaßen als Übung können Sie die Beispiel-Konfigurationswerte:

- **VNet Name:** TestVNet1
- **Adressraum:** 10.11.0.0/16 und 10.12.0.0/16
- **Subnetze:**
    - FrontEnd: 10.11.0.0/24
    - Back-End: 10.12.0.0/24
    - Subnetzname: 10.12.255.0/27
- **Gruppe:** TestRG1
- **Speicherort:** Osten der USA
- **DNS-Server:** 8.8.8.8
- **Gateway-Namen:** VNet1GW
- **Öffentliche IP-Adresse:** VNet1GWIP
- **VPN-Typ:** Route-basierten
- **Verbindung:** Standort-zu-Standort (IPsec)
- **Gateway-Typ:** VPN
- **LAN gatewayname:** Site2
- **Name der Verbindung:** VNet1toSite2


## <a name="CreatVNet"></a>1. erstellen Sie 1. ein virtuelles Netzwerk 

Wenn Sie bereits ein VNet überprüfen, ob die mit dem VPN-Gateway kompatibel sind. Achten Sie auf Subnetze, die mit anderen Netzwerken überlappen können. Wenn Sie überlappende Subnetze haben, funktioniert die Verbindung nicht ordnungsgemäß. Wenn Ihr VNet mit korrekt konfiguriert ist, können Sie die Schritte im Abschnitt [Geben Sie einen DNS-Server](#dns) beginnen.

### <a name="to-create-a-virtual-network"></a>Ein virtuelles Netzwerk erstellen

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. Fügen Sie zusätzliche Adressraum und Subnetze

Sie können Hinzufügen zusätzlichen Adressraum und Subnetzen die VNet nach dem erstellen.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. Geben Sie einen DNS-server

### <a name="to-specify-a-dns-server"></a>Einen DNS-Server angeben

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. erstellen Sie ein

Vor dem Gateway virtuelle Netzwerk herstellen, müssen Sie für das virtuelle Netzwerk gatewaysubnetz erstellen Sie eine Verbindung herstellen möchten. Gegebenenfalls empfiehlt es sich, ein gatewaysubnetz eines CIDR-Blocks von /28 oder /27 um genügend IP-Adressen um zusätzliche zukünftigen Konfiguration Anforderungen erstellen.

Wenn Sie diese Konfiguration als Übung erstellen, finden Sie diese [Werte](#values) beim gatewaysubnetz erstellen.

### <a name="to-create-a-gateway-subnet"></a>Erstellen Sie ein


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. erstellen Sie 5. ein virtuelles Netzwerk-gateway

Wenn Sie diese Konfiguration als Übung erstellen, können Sie zum [Beispielwerte](#values)verweisen.

### <a name="to-create-a-virtual-network-gateway"></a>Erstellen Sie ein virtuelles Netzwerk-gateway

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. LAN Gateway erstellen

"LAN-Gateway" bezieht sich auf Ihren lokalen Standort. Benennen Sie dem lokale Netzwerk-Gateway über den Azure darauf verweisen kann. 

Wenn Sie diese Konfiguration als Übung erstellen, können Sie zum [Beispielwerte](#values)verweisen.

### <a name="to-create-a-local-network-gateway"></a>Ein lokales Netzwerk-Gateway erstellen

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. konfigurieren Sie das VPN-Gerät

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. erstellen Sie eine Standort-zu-Standort-VPN-Verbindung

Erstellen Sie Standort-zu-Standort-VPN-Verbindung zwischen virtuellen Netzwerkgateway und dem VPN-Gerät. Achten Sie darauf, dass Sie die Werte ändern. Der gemeinsame Schlüssel muss den Wert für die VPN-Konfiguration verwendet. 

Bevor Sie beginnen, sicher, dass Ihre virtuellen Netzwerk-Gateway und lokale Netzwerkgateways erstellt haben. Wenn Sie diese Konfiguration als Übung erstellen, finden Sie diese [Werte](#values) beim Erstellen der Verbindung.

### <a name="to-create-the-vpn-connection"></a>Die VPN-Verbindung


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. Überprüfen Sie die VPN-Verbindung

Sie können die VPN-Verbindung im Portal oder mit PowerShell überprüfen.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte

- Nach Abschluss die Verbindung können Sie virtuelle Computer, virtuelle Netzwerke hinzufügen. Sehen Sie der virtuelle Computer [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) für Weitere Informationen.

- Informationen über BGP finden Sie unter [Übersicht über BGP](vpn-gateway-bgp-overview.md) und [BGP konfigurieren](vpn-gateway-bgp-resource-manager-ps.md).
