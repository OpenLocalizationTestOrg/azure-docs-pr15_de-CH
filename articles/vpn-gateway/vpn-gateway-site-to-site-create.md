<properties
   pageTitle="Erstellen Sie ein virtuelles Netzwerk mit einer Standort-zu-Standort-VPN-Gateway Verbindung mit klassischen Azure-Portal | Microsoft Azure"
   description="Erstellen Sie ein VNet mit einer Verbindung S2S VPN-Gateway für standortübergreifende und hybridkonfigurationen mit dem klassischen Bereitstellungsmodell."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Erstellen Sie ein VNet mit einer Standort-zu-Standort-Verbindung mit klassischen Azure-portal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Classic - Verwaltungsportal](vpn-gateway-site-to-site-create.md)

Dieser Artikel führt Sie durch das Erstellen eines virtuellen Netzwerks und eine Standort-zu-Standort-VPN-gatewayverbindung mit dem lokalen Netzwerk der klassischen Bereitstellungsmodell und dem Verwaltungsportal. Standort-zu-Standort-Verbindung können für standortübergreifende und Hybrid verwendet Konfigurationen.

![Standort-zu-Standort-Diagramm] (./media/vpn-gateway-site-to-site-create/site2site.png "zwischen Standorten")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Bereitstellung und-Modelle für Standort-zu-Standort-Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Die folgende Tabelle zeigt die derzeit Bereitstellungsmodelle und Methoden für Standort-zu-Standort-Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Zusätzliche Konfigurationen 

VNets verbinden, finden Sie unter [Konfigurieren einer Verbindung VNet VNet für das klassische Bereitstellungsmodell](virtual-networks-configure-vnet-to-vnet-connection.md). Eine Standort-zu-Standort-Verbindung zu einem VNet hinzufügen, die bereits eine Verbindung, finden Sie unter [Hinzufügen einer S2S-Verbindung zu einem VNet mit einer vorhandenen VPN-Gateway-Verbindung](vpn-gateway-multi-site.md).
 
## <a name="before-you-begin"></a>Bevor Sie beginnen

Überprüfen Sie Folgendes vor der Konfiguration.

- Ein kompatibles VPN-Gerät und wer konfigurieren kann. [VPN-Geräte](vpn-gateway-about-vpn-devices.md)finden Sie unter. Vertraut mit Ihrem VPN-Gerät konfiguriert oder sind nicht mit der IP-Adresse befindet sich Bereiche in Ihrem lokalen Netzwerk-Konfiguration, müssen Sie diese Angaben können Sie mit anderen Personen koordinieren.

- Eine nach außen gerichtete öffentliche IP-Adresse für das VPN-Gerät. Diese IP-Adresse kann nicht hinter einem NAT befinden

- Ein Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.


## <a name="CreateVNet"></a>Virtuelle Netzwerk erstellen

1. Melden Sie sich im [klassischen Azure-Portal](https://manage.windowsazure.com/).

2. Unten links auf dem Bildschirm klicken Sie auf **neu**. Klicken Sie im Navigationsbereich auf **Netzwerkdienste**und klicken Sie dann auf **Virtuelles Netzwerk**. Klicken Sie auf **Benutzerdefinierte** , um den Konfigurations-Assistenten zu starten.

3. Geben Sie zum Erstellen des Vnets der Konfigurationen auf den folgenden Seiten:

## <a name="Details"></a>Virtuelles Netzwerk Seite

Geben Sie Folgendes ein:

- **Name**: Namen für das virtuelle Netzwerk. Beispielsweise *EastUSVNet*. Sie verwenden diese virtuellen Netzwerknamen, wenn Ihre VMs und PaaS-Instanzen bereitstellen, sollten Sie nicht den Namen kompliziert.
- **Position**: die Position bezieht sich direkt auf den physischen Speicherort (Region) Ressourcen (VMs) befinden. Soll die VMs, die Bereitstellung dieses virtuellen Netzwerk physisch im *Südostasiatischen USA*befinden, wählen Sie diesen Speicherort ein. Sie können die Region mit dem virtuellen Netzwerk nach der Erstellung nicht ändern.

## <a name="DNS"></a>DNS-Server und VPN-Konnektivität Seite

Geben Sie Folgendes ein, und klicken Sie auf den Pfeil rechts.

- **DNS-Server**: Geben Sie den DNS-Servernamen und IP-Adresse oder einen zuvor registrierten DNS-Server aus dem Kontextmenü auswählen. Diese Einstellung wird einen DNS-Server nicht erstellt. Dadurch werden die DNS-Server angeben, die für die Auflösung von Namen für dieses virtuelle Netzwerk verwenden möchten.
- **Standort-zu-Standort-VPN konfigurieren**: Aktivieren Sie das Kontrollkästchen für **eine Standort-zu-Standort-VPN konfigurieren**.
- **Lokalen Netzwerk**: lokal Ihren lokalen physischen Standort darstellt. Wählen Sie ein lokales Netzwerk, das Sie zuvor erstellt haben, oder ein neues lokales Netzwerk erstellen. Jedoch wenn Sie lokal verwenden, die Sie zuvor erstellt haben, wechseln Sie zur Konfigurationsseite **Lokalen Netzwerken** und überprüfen Sie, ob die VPN-Gerät (Adresse öffentliche für IPv4) für das VPN-Gerät ist.

## <a name="Connectivity"></a>Standort-zu-Standort-Konnektivität

Wenn Sie ein neues lokales Netzwerk erstellen, sehen Sie die Seite **Site-zu-Standort-Verbindung** . Wenn soll ein lokales Netzwerk, das Sie zuvor erstellt haben, diese Seite nicht im Assistenten angezeigt und können Sie mit dem nächsten Abschnitt fortfahren.

Geben Sie Folgendes ein, und klicken Sie auf den Pfeil.

-   **Name**: Netzwerkstandort Namen lokale (lokal) aufgerufen.
-   **VPN-Geräte-IP-Adresse**: öffentliche mit IPv4-Adresse des lokalen VPN-Geräts, die Verbindung zu Azure verwenden. Das VPN-Gerät kann nicht hinter einem NAT befinden
-   **Adressraum**: Starten IP und CIDR (Zählung). Sie geben die Adresse range(s), die Sie über virtuelle Netzwerk-Gateway an lokale lokalen gesendet werden sollen. Liegt eine IP-Zieladresse innerhalb der Bereiche, die Sie hier angeben, wird es durch das virtuelle Netzwerkgateway weitergeleitet.
-   **Adressraum hinzufügen**: haben Sie mehrere Adressbereiche, die durch das virtuelle Netzwerkgateway gesendet werden soll, jedes zusätzliche Adressbereich angeben. Sie können hinzufügen oder Entfernen von Bereichen später auf das **Lokale Netzwerk** .

## <a name="Address"></a>Virtuelles Netzwerk Adressseite Leerzeichen

Geben Sie den Adressbereich für das virtuelle Netzwerk verwenden möchten. Dynamischen IP-Adressen (DIPS), die VMs und andere Instanzen, die in diesem virtuellen Netzwerk bereitstellen zugeordnet sind.

Es ist besonders wichtig, die nicht überlappt mit anderen Bereichen ausgewählt, die für das lokale Netzwerk verwendet werden. Sie müssen mit dem Netzwerkadministrator in Verbindung. Netzwerkadministrator müssen sich ein Bereich von IP-Adressen aus Ihrem lokalen Netzwerkadressbereich für das virtuelle Netzwerk verwenden.

Geben Sie die folgende Informationen und klicken Sie auf das Häkchen unten rechts, Ihr Netzwerk zu konfigurieren.

- **Adressraum**: Starten IP und Zählung. Überprüfen Sie ob Adressräume Angabe eines die Adressräume nicht überlappen, die Sie in Ihrem lokalen Netzwerk.
- **Subnetz hinzufügen**: Start-IP-enthalten und Zählung. Weitere Subnetze sind nicht erforderlich, aber Sie möchten ein separates Subnetz für virtuelle Computer zu erstellen, die statische DIPS. Oder Sie möchten für Ihre virtuellen Computer in einem Subnetz getrennt von der anderen Instanzen.
- **Gateway-Subnetz hinzufügen**: auf gatewaysubnetz hinzuzufügen. Gatewaysubnetz nur für das virtuelle Netzwerkgateway verwendet und ist für diese Konfiguration.

Klicken Sie auf das Häkchen am unteren Rand der Seite und Ihr virtuelle Netzwerk erstellen beginnt. Beendigung, sehen Sie auf der Seite **Netzwerk** der Azure-Verwaltungsportal **erstellt** unter **Status** aufgeführt. Nachdem das VNet erstellt wurde, können Sie Ihr virtuelles Netzwerk-Gateway konfigurieren.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="VNetGateway"></a>Konfigurieren Sie Ihr virtuelles Netzwerk-gateway

Konfigurieren Sie virtuelle Netzwerk-Gateway zum Erstellen einer sicheren Verbindungs zwischen Standorten. Finden Sie unter [konfigurieren ein virtuelles Netzwerk-Gateway im klassischen Azure-Portal](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss die Verbindung können Sie virtuelle Computer, virtuelle Netzwerke hinzufügen. Finden Sie [virtuellen Maschinen](https://azure.microsoft.com/documentation/services/virtual-machines/) Weitere Informationen.
