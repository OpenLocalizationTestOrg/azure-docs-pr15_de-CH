<properties 
   pageTitle="Verbinden Sie ein virtuelles Netzwerk VPN-Gateway mit PowerShell Standorten | Microsoft Azure"
   description="Dieser Artikel führt Sie durch ein virtuelles Netzwerk über ein VPN-Gateway für das klassische Bereitstellungsmodell Lokal lokalen Standorten herstellen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Ein VNet mit einer vorhandenen VPN-Gateway-Verbindung eine Standort-zu-Standort-Verbindung hinzufügen

> [AZURE.SELECTOR]
- [Ressourcenmanager - Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Classic - PowerShell](vpn-gateway-multi-site.md)

Dieser Artikel führt Sie durch mithilfe von PowerShell-Standorten (S2S) Verbindung zu einem VPN-Gateway hinzufügen eine vorhandene Verbindung. Dieser Verbindungstyp wird häufig als "Multi-Site" Konfiguration bezeichnet. 

Dieser Artikel gilt für virtuelle Netzwerke mit dem klassischen Bereitstellungsmodell (auch bekannt als Service Management) erstellt. Diese Schritte gelten nicht für ExpressRoute /-Standorten Koexistenz verbindungskonfigurationen. Informationen über Koexistenz finden Sie unter [ExpressRoute-S2S Koexistenz Connections](../expressroute/expressroute-howto-coexist-classic.md) .

### <a name="deployment-models-and-methods"></a>Bereitstellung und-Modelle

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Wir aktualisieren diese Tabelle als neue Artikel und weitere Tools werden für diese Konfiguration. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>Zum Herstellen einer Verbindung

Sie können mehrere lokale Standorte zu einem virtuellen Netzwerk verbinden. Dies ist besonders interessant für Hybriden Cloud Lösungen. Erstellen einer Multi-Site Verbindungs mit Azure virtuelles Netzwerk-Gateway ähnelt einer anderen Verbindung zwischen Standorten. Tatsächlich können Sie ein vorhandenes Azure VPN-Gateway als Gateway dynamisch ist (Route-basiert).

Haben Sie bereits eine statische Gateway mit dem virtuellen Netzwerk verbunden, können Sie Gatewaytyp dynamischen ändern, ohne das virtuelle Netzwerk neu erstellen, um mehrere Standorte aufzunehmen. Ändern den Routingtyp, sicherstellen Sie, dass das lokale VPN-Gateway Route-basierte VPN-Konfigurationen unterstützt. 

![Multi-Site-Diagramm] (./media/vpn-gateway-multi-site/multisite.png "mehrere Standorte")

## <a name="points-to-consider"></a>Zu berücksichtigende Aspekte

**Sie wird nicht mit der Azure-Verwaltungsportal in diesem virtuellen Netzwerk ändern können.** In dieser Version müssen Sie die Netzwerk-Konfigurationsdatei anstelle der Azure-Verwaltungsportal zu ändern. In der Azure-Verwaltungsportal Änderungen werden Einstellungen Multisite-Referenz für diese virtuelle Netzwerk überschreiben. 

Sie sollten fühlen ziemlich mithilfe der Netzwerk-Konfigurationsdatei Multisite-Prozedur beendet haben. Haben Sie mehrere Mitarbeiter Ihre Netzwerkkonfiguration müssen Sie sicherstellen, dass jeder diese Einschränkung kennt. Dies bedeutet, dass der Azure-Verwaltungsportal überhaupt verwenden zu können. Sie können für alles außer Ändern der Konfiguration in diesem bestimmten virtuellen Netzwerk.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Bevor Sie Konfiguration beginnen, überprüfen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.

- Kompatible VPN-Hardware für jeden lokalen Speicherort. Überprüfen Sie [Über VPN-Geräte virtuelle Netzwerkkonnektivität](vpn-gateway-about-vpn-devices.md) überprüfen, ob das Gerät verwendet wird, die kompatibel ist.

- Nach außen gerichtete öffentlichen IPv4-IP-Adresse jedes VPN-Gerät. Die IP-Adresse kann nicht hinter einem NAT befinden Dies ist erforderlich.

- Sie müssen die neueste Version von Azure PowerShell-Cmdlets installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.

- Jemand, Helpdeskmitarbeiter das Konfigurieren der VPN-Hardware. Nicht werden mit automatisch generierten VPN-Skripts von Azure-Verwaltungsportal VPN-Geräte konfiguriert. Dies bedeutet, dass Sie starke Kenntnisse über das VPN-Gerät konfigurieren oder jemanden arbeiten müssen.

- Die IP-Adressbereiche, die für das virtuelle Netzwerk verwenden (Wenn Sie bereits eine erstellt haben). 

- Die IP-Adressbereiche für LAN-Sites, denen eine Verbindung herstellen. Sie müssen sicherstellen, dass die IP-Adresse für jeden lokalen Netzwerkstandort reicht die gewünschte Verbindung nicht überlappen. Andernfalls zurück der Azure-Verwaltungsportal oder die REST-API hochgeladen Konfiguration. 

    Beispielsweise haben Sie zwei LAN-Sites enthalten die IP-Adresse Bereich 10.2.3.0/24, Sie haben ein Paket mit einer Zieladresse 10.2.3.3 würde nicht Azure Site stellen das Paket nicht senden, da sich Adressbereiche überlappende. Um routing-Probleme zu vermeiden, lässt Azure keine Datei hochladen, die überlappende Bereiche.



## <a name="1-create-a-site-to-site-vpn"></a>1. erstellen Sie ein Standort-zu-Standort-VPN

Wenn Sie bereits ein Standort-zu-Standort-VPN mit dynamischen routing-Gateway, hervorragend! Sie können [virtuelle Netzwerk-Konfigurationen](#export)Exportieren fortfahren. Wenn dies nicht der Fall ist, gehen Sie folgendermaßen vor:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Wenn bereits ein virtuelles Netzwerk zwischen Standorten ist statisch (Policy-basiert) routing-Gateway:

1. Ändern der Gateway in dynamisches routing. Eine standortübergreifende VPN erfordert dynamische (auch bekannt als Route-basiert) routing-Gateway. Um Ihre Gateway ändern, müssen Sie zunächst vorhandene Gateway löschen und dann einen neuen erstellen. Eine Anleitung finden Sie unter [den VPN-routing für Ihr Gateway ändern](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Den neuen Gateway konfigurieren und Ihre VPN-Tunnel erstellen. Eine Anleitung finden Sie unter [konfigurieren ein VPN-Gateway in der Azure-Verwaltungsportal](vpn-gateway-configure-vpn-gateway-mp.md). Zuerst ändern Sie Ihr Gateway in dynamisches routing. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Wenn Sie ein virtuelles Netzwerk Standorten haben:

1. Zwischen Standorten virtuellen Netzwerks anhand der folgenden Schritte erstellen: [Erstellen Sie ein virtuelles Netzwerk mit einer Standort-zu-Standort-VPN-Verbindung in Azure-Verwaltungsportal](vpn-gateway-site-to-site-create.md).  

2. Dynamisches routing-Gateway anhand der folgenden Schritte konfigurieren: [ein VPN-Gateway konfigurieren](vpn-gateway-configure-vpn-gateway-mp.md). Achten Sie darauf, dass Sie für Ihren Gateway **Dynamisches routing** aktivieren.

## <a name="export"></a>2. die Netzwerk Konfigurationsdatei exportieren 

Exportieren Sie die Netzwerk-Konfigurationsdatei. Die Datei, die Sie exportieren, verwendet die neue Multi-Site konfigurieren. Benötigen Sie Informationen zum Exportieren einer Datei finden Sie im Abschnitt im Artikel: [ein VNet mit einer Netzwerk-Konfigurationsdatei im Azure-Portal erstellen](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. Öffnen Sie die Netzwerk-Konfigurationsdatei

Öffnen Sie im letzten Schritt der Netzwerk-Konfigurationsdatei, die Sie heruntergeladen haben. Verwenden Sie einen XML-Editor, den Ihnen gefällt. Die Datei sollte wie folgt aussehen:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Fügen Sie Verweise auf mehrere Websites

Beim Hinzufügen oder Websiteinformationen entfernen werden Sie Konfiguration ConnectionsToLocalNetwork/LocalNetworkSiteRef ändern. Hinzufügen einer neue Ort Referenz werden Azure einen neuen Tunnel erstellen. Im folgenden Beispiel wird die Konfiguration für eine einzelne Website-Verbindung. Speichern Sie die Datei nach der Änderung.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. die Netzwerk-Konfigurationsdatei importieren

Importieren Sie Netzwerk-Konfigurationsdatei. Beim Importieren dieser Datei mit den neuen Tunnel hinzukommen. Der Tunnel werden dynamische Gateway verwenden, das Sie zuvor erstellt haben. Benötigen Sie Informationen zum Importieren der Datei finden Sie im Abschnitt im Artikel: [ein VNet mit einer Netzwerk-Konfigurationsdatei im Azure-Portal erstellen](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. Schlüssel herunterladen

Sobald der neue Tunnel hinzugefügt wurden, verwenden das PowerShell-Cmdlet `Get-AzureVNetGatewayKey` IPsec-IKE vorinstallierte Schlüssel für jeden Tunnel zu.

Zum Beispiel:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Auf Wunsch auch *Erhalten virtuelle Netzwerk Gateway Shared Key* REST-API können Sie die vorinstallierten Schlüssel.

## <a name="7-verify-your-connections"></a>7. Überprüfen Sie Ihre Verbindung

Überprüfen Sie Multi-Site-Tunnel. Nach dem Download der Schlüssel für jeden Tunnel sollten Sie Anschlüsse überprüfen. Mit `Get-AzureVnetConnection` um eine Liste der virtuelle Tunnel, wie im folgenden Beispiel gezeigt. VNet1 ist der Name der VNet.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu VPN-Gateways finden Sie unter [VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md).

