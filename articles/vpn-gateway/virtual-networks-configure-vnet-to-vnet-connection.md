<properties
   pageTitle="Konfigurieren einer Verbindung VNet VNet für das klassische Bereitstellungsmodell | Microsoft Azure"
   description="Wie Azure virtuelle Netzwerke mit PowerShell und klassischen Azure-Portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>Konfigurieren einer Verbindung VNet VNet für das klassische Bereitstellungsmodell

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Classic - Verwaltungsportal](virtual-networks-configure-vnet-to-vnet-connection.md)



Dieser Artikel führt Sie durch die Schritte zum Erstellen und verbinden virtuelle Netzwerke mit dem klassischen Bereitstellungsmodell (auch bekannt als Service Management). Die folgenden Schritte verwenden klassische Azure-Portal zu VNets und Gateways und PowerShell VNet-VNet-Verbindung konfigurieren. Sie können nicht im Portal die Verbindung konfigurieren.

![VNet VNet Konnektivität Diagramm](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)


### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Bereitstellungsmodelle und Methoden VNet VNet Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle zeigt die derzeit Bereitstellungsmodelle und Methoden VNet VNet Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>VNet VNet-Verbindungen

Ein virtuelles Netzwerk mit einem anderen virtuellen Netzwerk entspricht (VNet VNet) zu einem virtuellen Netzwerk an einem lokalen Standort. Beide Konnektivität verwenden eine VPN-Gateway für einen sicheren Tunnel mit IPsec-IKE. 

Verbindung VNets können verschiedene Abonnements und Regionen. Sie können VNet VNet Kommunikation mit mehreren Standorten kombinieren. Dadurch können Sie die Netzwerktopologien einzurichten, die standortübergreifende Konnektivität zwischen virtuellen Netzwerkkonnektivität kombinieren.


### <a name="why-connect-virtual-networks"></a>Warum verbinden virtuelle Netzwerke?

Möglicherweise möchten virtuelle Netzwerke aus den folgenden Gründen:

- **Cross-Region Geo-Redundanz und Geo-Präsenz**
    - Geo-Replikation oder Synchronisierung lassen mit sicheren sich ohne Internetzugriff Endpunkte.
    - Azure-Lastenausgleich und Microsoft oder Drittanbietern clustering-Technologie können Sie hochverfügbare Arbeitslast Geo-Redundanz über mehrere Azure Bereiche festlegen. Ein wichtiges Beispiel soll SQL ständig mit Verfügbarkeit über mehrere Azure-Regionen.

- **Regionale Multi-Tier-Anwendung mit starken Isolationsgrenze**
    - Innerhalb desselben Bereichs können Sie Multi-Tier-Anwendung mit mehreren VNets verbunden mit starken Isolierung und sichere Kommunikation zwischen Ebenen einrichten.

- **Cross-Kommunikation zwischen Organisationen in Azure-Abonnement**
    - Haben mehrere Azure-Abonnements können Sie Arbeitslasten aus verschiedenen Subskriptionen sicher zwischen virtuellen Netzwerken verbinden.
    - Für Unternehmen oder Dienstanbieter können Sie Unternehmens-Kommunikation mit sichere VPN-Technologie in Azure.

### <a name="vnet-to-vnet-faq-for-classic-vnets"></a>FAQ für klassische VNets VNet VNet

- Virtuelle Netzwerke können denselben oder anderen Abonnements.

- Virtuelle Netzwerke können denselben oder anderen Azure Regionen (Standorte).

- Cloud-Dienst oder ein Lastenausgleich Endpunkt kann nicht über virtuelle Netzwerke umfassen, auch wenn sie miteinander verbunden sind.

- Mehrere virtuelle Netzwerke verbinden erfordert keine VPN-Geräte.

- VNet VNet unterstützt verbinden Azure virtuelle Netzwerke. Nicht verbundenen virtuellen Maschinen unterstützen oder cloud-Dienste, die nicht zu einem virtuellen Netzwerk bereitgestellt werden.

- VNet VNet erfordert dynamische routing Gateways. Azure statischen routing Gateways werden nicht unterstützt.

- Virtuelle Netzwerkkonnektivität kann gleichzeitig mit mehreren Standorten VPNs verwendet werden. Es ist maximal 10 VPN-Tunnel für ein virtuelles Netzwerk VPN-Gateway andere virtuelle Netzwerke oder lokalen Standorten herstellen.

- Die Adressräume der virtuellen Netzwerke und lokalen LAN dürfen nicht überlappen. Sich überlappende Adressräume verursacht die Erstellung virtueller Netzwerke oder Hochladen von Netcfg Konfigurationsdateien fehlschlagen.

- Redundante Tunnel zwischen zwei virtuelle Netzwerke werden nicht unterstützt.

- Alle VPN-Tunnel für VNet einschließlich P2S VPNs teilen die verfügbare Bandbreite für das VPN-Gateway und die gleichen VPN-Gateway Betriebszeit SLA in Azure.

- VNet VNet Datenverkehr durchläuft in Azure Backbone.


## <a name="step1"></a>Schritt 1 - IP-Adressbereiche planen

Es ist wichtig, entscheiden die Bereiche, die Sie verwenden, um Ihre virtuelle Netzwerke konfigurieren. Bei dieser Konfiguration müssen Sie sicherstellen, dass keine VNet Bereiche überschneiden, oder mit den lokalen Netzwerken herstellen.

In der folgenden Tabelle zeigt ein Beispiel der VNets definieren. Verwenden Sie die Bereiche lediglich als Anhaltspunkt. Notieren Sie die Bereiche für virtuelle Netzwerke. Sie benötigen diese Informationen für spätere Schritte.

**Konfigurieren**

|Virtuelles Netzwerk  |Adressbereich               |Region      |Verbindung zum lokalen Netzwerk|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)         |US West     |VNet2Local (10.2.0.0/16)      |
|VNet2            |VNet2 (10.2.0.0/16)         |Japan OST  |VNet1Local (10.1.0.0/16)      |
  
## <a name="step-2---create-vnet1"></a>Schritt 2 - Erstellen von VNet1

In diesem Schritt erstellen wir VNet1. Bei den Beispielen verwendet, müssen Sie Ihre eigenen Werte ersetzen. Wenn das VNet bereits vorhanden ist, müssen Sie diesen Schritt. Aber Sie müssen überprüfen, ob die IP-Adressbereiche überlappen sich nicht mit den für das zweite VNet oder mit der VNets, der Sie eine Verbindung herstellen möchten.

1. Melden Sie sich im [klassischen Azure-Portal](https://manage.windowsazure.com). In diesem Artikel verwenden wir das Verwaltungsportal, da einige der erforderlichen Konfigurationsinformationen noch nicht im Azure-Portal verfügbar sind.

2. Klicken Sie in der linken unteren Ecke des Bildschirms auf **neu** > **Netzwerkdienste** > **Virtual Network** > zu den Konfigurations-Assistenten**Benutzerdefinierte** . Jede Seite navigieren durch den Assistenten fügen Sie angegebenen Werte hinzu.

### <a name="virtual-network-details"></a>Virtuelles Netzwerk-Details

Geben Sie die folgenden Informationen auf der Seite Virtuelles Netzwerk-Details:

  ![Virtuelles Netzwerk-Details](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **Name** - Name des virtuellen Netzwerks. Beispielsweise VNet1.
  - **Speicherort** – Wenn Sie ein virtuelles Netzwerk erstellen, ordnen Sie es mit einer Azure (Region). Soll Ihre VMs, die mit dem virtuellen Netzwerk im Westen der USA physisch bereitgestellt werden, wählen Sie diesen Speicherort ein. Sie können nicht ändern den virtuellen Netzwerk nach ihrer Erstellung zugeordnet.

### <a name="dns-servers-and-vpn-connectivity"></a>DNS-Server und VPN-Konnektivität

Geben Sie auf der Seite DNS-Server und VPN-Konnektivität die folgenden Informationen und klicken Sie dann auf den Pfeil rechts.

  ![DNS-Server und VPN-Konnektivität](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

- **DNS-Server** - DNS-Server-Namen und IP-Adresse eingeben oder wählen Sie einen zuvor registrierten DNS-Server aus. Diese Einstellung wird einen DNS-Server nicht erstellt. Dadurch werden die DNS-Server angeben, die für die Auflösung von Namen für dieses virtuelle Netzwerk verwenden möchten. Auflösung zwischen virtuellen Netzwerken sollen, müssen Sie konfigurieren, eigene DNS-Server, anstatt die Auflösung, die von Azure bereitgestellt.
- Wählen Sie eines der Kontrollkästchen für P2S oder S2S-Konnektivität. Klicken Sie auf den Pfeil rechts, um zum nächsten Bildschirm zu wechseln.

### <a name="virtual-network-address-spaces"></a>Virtuelles Netzwerk-Adressräume

Geben Sie auf der Seite Virtuelles Netzwerk Adressräume den Adressbereich für das virtuelle Netzwerk verwenden möchten. Dynamischen IP-Adressen (DIPS), die VMs und andere Instanzen, die in diesem virtuellen Netzwerk bereitstellen zugeordnet sind. 

Wenn Sie ein VNet, die auch eine Verbindung mit dem lokalen Netzwerk erstellen, ist besonders wichtig, die nicht überlappt mit anderen Bereichen ausgewählt, die für das lokale Netzwerk verwendet werden. In diesem Fall müssen Sie mit dem Netzwerkadministrator in Verbindung. Netzwerkadministrator müssen sich ein Bereich von IP-Adressen aus Ihrem lokalen Netzwerkadressbereich für das VNet verwenden.

  ![Virtuelle Netzwerk-Adressräume Seite](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - **Adressraum** - erste IP-sowie Zählung. Überprüfen Sie, ob der angegebene Adressräume mit die Adressräume sich nicht überschneiden, die Sie in Ihrem lokalen Netzwerk. In diesem Beispiel verwenden wir 10.1.0.0/16 für VNet1.
  - **Hinzufügen Subnetz** - erste IP-sowie Zählung. Weitere Subnetze sind nicht erforderlich, aber Sie möchten ein separates Subnetz für virtuelle Computer zu erstellen, die statische DIPS. Oder Sie möchten für Ihre virtuellen Computer in einem Subnetz getrennt von der anderen Instanzen.
 
**Klicken Sie auf das Kontrollkästchen** auf der rechten Seite und virtuellen Netzwerk beginnt zu erstellen. Wenn er abgeschlossen ist, sehen Sie "Erstellt" unter Status auf der Seite Netzwerk aufgeführt.

## <a name="step-3---create-vnet2"></a>Schritt 3 - VNet2 erstellen

Anschließend wiederholen Sie die vorhergehenden Schritte zum Erstellen einer anderen VNet. In späteren Schritten verbinden Sie die beiden VNets. Sie können zum [beispieleinstellungen](#step1) in Schritt 1 verweisen. Wenn das VNet bereits vorhanden ist, müssen Sie diesen Schritt. Allerdings müssen Sie überprüfen, ob die IP-Adressbereiche nicht mit den anderen VNets oder lokalen Netzwerken überlappen, die Sie herstellen möchten.

## <a name="step-4---add-the-local-network-sites"></a>Schritt 4 - die lokale Netzwerke

Beim Erstellen einer VNet VNet-Konfiguration müssen Sie lokale Netzwerke der **Lokalen Netzwerken** auf der Portalwebsite angezeigt. Azure verwendet die Einstellungen in jeder Website im lokalen Netzwerk, wie Datenverkehr zwischen den VNets. Sie bestimmen den Namen auf jeder Website im lokalen Netzwerk verwenden möchten. Es empfiehlt sich, verwenden Sie beschreibenden Text den Wert später eine Dropdown-Liste auswählen.

Beispielsweise verbindet VNet1 mit einer Website im lokalen Netzwerk, die Sie erstellen mit dem Namen "VNet2Local". Die Einstellungen für VNet2Local enthalten Adresspräfixe für VNet2 und eine öffentliche IP-Adresse des VNet2 Gateways. VNet2 Ruft ein LAN Website erstellten namens "VNet1Local", die die Adresspräfixe VNet1 und öffentliche IP-Adresse des VNet1 Gateways enthalten.

### <a name="localnet"></a>Hinzufügen der lokalen Netzwerk-Website VNet1Local

1. Klicken Sie in der linken unteren Ecke des Bildschirms auf **neu** > **Netzwerkdienste** > **Virtual Network** > **Lokalen Netzwerk hinzufügen**.

2. Auf **Ihrem lokalen Netzwerk Angaben** **Name**Geben Sie einen Namen, der im Netzwerk darstellen, dem Sie herstellen möchten. In diesem Beispiel können Sie "VNet1Local" für die IP-Adressbereiche und Gateway für VNet1.

3. **VPN-Geräte-IP-Adresse (optional)**Geben Sie eine beliebige gültige öffentliche IP-Adresse. Normalerweise verwenden Sie tatsächliche externe IP-Adresse für ein VPN-Gerät. VNet VNet Konfigurationen verwenden Sie öffentliche IP-Adresse, die Gateway für das VNet zugewiesen. Aber da das Gateway noch nicht erstellt haben, Sie können eine beliebige gültige öffentliche IP-Adresse als Platzhalter. Nicht nichts - ist nicht für diese Konfiguration optional. In einem späteren Schritt zurück in diese Einstellung und die entsprechende Gateway IP-Adressen konfigurieren, nachdem Azure generiert. Klicken Sie auf den Pfeil, um zum nächsten Bildschirm zu gelangen.

4. Geben Sie auf der **Adressseite angeben**den IP-Bereich und Adresse Zählung für VNet1. Dies muss der Bereich entsprechen, die für VNet1 konfiguriert. Azure verwendet die IP-Adressbereiche, die Sie angeben, dass der Datenverkehr für VNet1 vorgesehen. Klicken Sie auf das Häkchen, um das lokale Netzwerk erstellen.

### <a name="add-the-local-network-site-vnet2local"></a>Hinzufügen der lokalen Netzwerk-Website VNet2Local

Verwenden Sie die obigen Schritte LAN-Site "VNet2Local" erstellt. Sie können auf die Werte in den [beispieleinstellungen](#step1) in Schritt 1 ggf. verweisen.

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Konfigurieren Sie jedes VNet auf einem lokalen Netzwerk

Jedes VNet muss mit dem entsprechenden lokalen Netzwerk zeigen, die Datenverkehr an. 

#### <a name="for-vnet1"></a>Für VNet1

1. Navigieren Sie zu der Seite **Konfigurieren** für virtuelle Netzwerk **VNet1**. 
2. Wählen Sie unter Standort-zu-Standort-Verbindung "Verbindung mit dem lokalen Netzwerk", und wählen Sie **VNet2Local** als im lokalen Netzwerk aus. 
3. Speichern.

#### <a name="for-vnet2"></a>Für VNet2

1. Navigieren Sie zu der Seite **Konfigurieren** für virtuelle Netzwerk **VNet2**. 
2. Wählen Sie unter Standort-zu-Standort-Verbindung "Verbindung mit dem lokalen Netzwerk" und wählen Sie **VNet1Local** aus dem lokalen Netzwerk. 
3. Speichern.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Schritt 5 - Gateway für jede VNet konfigurieren

Konfigurieren Sie Gateway für jedes virtuelle Netzwerk dynamisches Routing. Statisches Routing-Gateways wird diese Konfiguration nicht unterstützt. Verwenden Sie VNets, zuvor konfiguriert wurden und bereits Gateways dynamisches Routing, müssen Sie diesen Schritt. Sind die Gateways statisches Routing, müssen Sie löschen und als dynamisches Routing Gateways erstellen. Löschen ein Gateways die öffentliche IP-Adresse zugewiesen wird veröffentlicht und müssen Sie zurückgehen und Ihre lokale Netzwerke und VPN-Geräte neue öffentliche IP-Adresse des neuen Gateways konfigurieren.

1. Überprüfen Sie auf der Seite **Netzwerk** die Statusspalte für das virtuelle Netzwerk **erstellt**.

2. Klicken Sie in der Spalte **Name** auf den Namen des virtuellen Netzwerks. In diesem Beispiel verwenden wir "VNet1".

3. Die Seite **Dashboard** feststellen Sie, dass diese VNet Gateway konfiguriert hat. Sie sehen Statuswechsel besprechen Ihres Gateways konfigurieren.

4. Klicken Sie am unteren Rand der Seite auf **Gateway erstellen** und **Dynamisches Routing**. Das System fordert Sie zum Bestätigen das Gateway erstellt, klicken Sie auf Ja.

    ![Gateway-Typ](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. Beachten Sie beim Erstellen Ihres Gateways Gateway-Grafik auf der Seite Gelb ändert und sagt "Gateway erstellen". Es dauert ca. 30 Minuten für das Gateway erstellen.

6. Wiederholen Sie die Schritte für VNet2. Sie brauchen nicht das erste VNet Gateway abzuschließen, bevor das Gateway für das VNet erstellen.

7. Gateway-Status in "Verbinden" ändert, wird die öffentliche IP-Adresse jedes Gateway im Dashboard angezeigt. Notieren Sie die IP-Adresse zu jeder VNet um zu mischen sie. Dies sind die IP-Adressen, die beim Bearbeiten der Platzhalter IP-Adressen für VPN-Gerät für jedes LAN verwendet werden.

## <a name="step-6---edit-the-local-network"></a>Schritt 6: Bearbeiten im lokalen Netzwerk

1. Auf der Seite **Lokale Netzwerke** den Namen des lokalen Netzwerks, das Sie bearbeiten möchten, klicken Sie am unteren Rand der Seite **Bearbeiten** . **VPN-Geräte-IP-Adresse**Geben Sie die IP-Adresse des Gateways, das dem VNet entspricht. Legen Sie z. B. für VNet1Local, die Gateway IP-Adresse VNet1 zugewiesen. Klicken Sie auf den Pfeil am unteren Rand der Seite.

2. Klicken Sie auf der Seite **Geben Sie den Adressraum** Häkchen unten rechts ohne Veränderung.

## <a name="step-7---create-the-vpn-connection"></a>Schritt 7 - VPN-Verbindung erstellen

Wenn die vorherigen Schritte abgeschlossen haben, die IPsec-IKE vorinstallierten Schlüssel und die Verbindung. Diese Schritte PowerShell verwendet und kann nicht im Portal konfiguriert werden. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von Azure PowerShell-Cmdlets. Stellen Sie sicher das Herunterladen Cmdlets Service-Management (SM). 

1. Windows PowerShell öffnen und sich anmelden.

        Add-AzureAccount

2. Wählen Sie das Abonnement in der VNets befinden.

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"

3. Die Verbindung zu erstellen. Beachten Sie in den Beispielen der freigegebene Schlüssel identisch ist. Der gemeinsame Schlüssel muss immer übereinstimmen.


    VNet1 VNet2-Verbindung

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2 VNet1-Verbindung

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Warten Sie auf Verbindung zu initialisieren. Sobald das Gateway initialisiert, sieht das Gateway in der folgenden Grafik.

    ![Gateway-Status - verbunden](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

    [AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Nächste Schritte

Sie können virtuelle Maschinen virtuelle Netzwerke hinzufügen. Finden Sie die [Dokumentation zu virtuellen Maschinen](https://azure.microsoft.com/documentation/services/virtual-machines/) .



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 
