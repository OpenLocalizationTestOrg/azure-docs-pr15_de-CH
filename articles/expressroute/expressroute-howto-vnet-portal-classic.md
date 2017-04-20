<properties
   pageTitle="Konfigurieren eines virtuellen Netzwerks und Gateway für ExpressRoute im klassischen Portal | Microsoft Azure"
   description="Dieser Artikel führt Sie durch das Einrichten eines virtuellen Netzwerks für ExpressRoute klassischen Bereitstellungsmodell mit dem Verwaltungsportal."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Erstellen Sie ein virtuelles Netzwerk für ExpressRoute im klassischen portal

Die Schritte in diesem Artikel führt Sie durch die Konfiguration eines virtuellen Netzwerks und ein virtuelles Netzwerk-Gateways zur Verwendung mit dem klassischen Bereitstellungsmodell mit dem Verwaltungsportal ExpressRoute.

Wenn Sie Informationen zum Ressourcen-Manager-Bereitstellungsmodell suchen, können Sie die folgenden Artikel: [Erstellen Sie ein virtuelles Netzwerk mit PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) und [ein VPN-Gateway, ein Ressourcen-Manager VNet für ExpressRoute hinzufügen](expressroute-howto-add-gateway-resource-manager.md).

**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>Erstellen Sie eine klassische VNet und gateway

Die folgenden Schritte erstellen eine klassische VNet und ein virtuelles Netzwerk-Gateway. Wenn bereits ein klassisches VNet finden Sie im Abschnitt [Konfigurieren einer vorhandenen klassischen VNet](#config) in diesem Artikel.

1. Melden Sie sich im [klassischen Azure-Portal](http://manage.windowsazure.com).

2. Klicken Sie in der linken unteren Ecke des Bildschirms auf **neu**. Klicken Sie im Navigationsbereich auf **Netzwerkdienste**und klicken Sie dann auf **Virtuelles Netzwerk**. Klicken Sie auf **Benutzerdefinierte** , um den Konfigurations-Assistenten zu starten.

3. Geben Sie auf der Seite **Details der virtuellen Netzwerk** Folgendes ein:

    - **Name** – Name des virtuellen Netzwerks. Sie verwenden diese virtuellen Netzwerknamen, wenn Ihre VMs und PaaS-Instanzen bereitstellen, sollten Sie nicht den Namen kompliziert.
    - **Position** – die Position bezieht sich direkt auf den physischen Speicherort (Region) Ressourcen (VMs) befinden. Soll die VMs, die Bereitstellung dieses virtuellen Netzwerk physisch im südostasiatischen USA befinden, wählen Sie diesen Speicherort ein. Sie können die Region mit dem virtuellen Netzwerk nach der Erstellung nicht ändern.

4. Geben Sie auf der Seite **DNS-Server und VPN-Konnektivität** die folgenden Informationen und klicken Sie dann auf den Pfeil rechts. 

    - **DNS-Server** - DNS-Server-Namen und IP-Adresse eingeben oder einen zuvor registrierten DNS-Server aus dem Kontextmenü auswählen. Diese Einstellung wird einen DNS-Server nicht erstellt. Dadurch werden die DNS-Server angeben, die für die Auflösung von Namen für dieses virtuelle Netzwerk verwenden möchten.
    - **Standort-zu-Standort-Verbindung** – aktivieren Sie das Kontrollkästchen für **eine Standort-zu-Standort-VPN konfigurieren**.
    - **ExpressRoute** – aktivieren Sie das Kontrollkästchen **ExpressRoute verwenden**. Diese Option ist nur bei Auswahl **eines Standort-zu-Standort-VPN konfigurieren**.
    - **LAN** - LAN für ExpressRoute haben müssen. Jedoch werden bei einer Verbindung ExpressRoute für LAN-Website angegebene Adresspräfixe ignoriert. Stattdessen werden durch die ExpressRoute-Verbindung an Microsoft angekündigt Adresspräfixe für das routing verwendet.<BR>Haben Sie bereits ein lokales Netzwerk für die ExpressRoute-Verbindung erstellt wurde, können Sie sie aus der Dropdownliste auswählen. Wählen Sie andernfalls **ein neues lokales Netzwerk angeben**.

5. Die **Standort-zu-Standort-Verbindung** angezeigt Wenn Sie ein neues lokales Netzwerk im vorherigen Schritt angeben. Konfigurieren Sie das lokale Netzwerk geben Sie die folgenden Informationen, und klicken Sie auf den Pfeil. 

    - **Name** - Name zu Ihrem (lokalen) aufrufen network Website.
    - **Adressraum** - erste IP-sowie CIDR (Zählung). Sie können einen beliebigen Adressbereich angeben, wie mit dem Adressbereich für das virtuelle Netzwerk überlappt. Diese Adressbereiche für die lokale Netzwerke geben normalerweise bei ExpressRoute, diese Einstellung nicht verwendet. Diese Einstellung ist jedoch erforderlich, um das lokale Netzwerk erstellen, wenn Sie das klassische Portal verwenden.
    - **Adressraum hinzufügen** - diese Einstellung gilt nicht für ExpressRoute.


6. Geben Sie auf der Seite **Virtuelles Netzwerk Adressräume** die folgenden Informationen und klicken Sie auf das Häkchen unten rechts, Ihr Netzwerk zu konfigurieren. 

    - **Adressraum** - einschließlich IP und Adresse Anzahl ab. Überprüfen Sie ob Adressräume Angabe eines die Adressräume nicht überlappen, die Sie in Ihrem lokalen Netzwerk.
    - **Hinzufügen Subnetz** - erste IP-sowie Zählung. Weitere Subnetze sind nicht erforderlich.
    - **Gateway-Subnetz hinzufügen** - auf gatewaysubnetz hinzuzufügen. Gatewaysubnetz nur für das virtuelle Netzwerkgateway verwendet und ist für diese Konfiguration.<BR>Gatewaysubnetz CIDR (Zählung) für ExpressRoute muss /28 sein oder größer (/ 27, von 26 usw..). Dadurch können genügend IP-Adressen in diesem Subnetz zu konfigurieren. Wenn Sie das Kontrollkästchen ExpressRoute, verwenden ausgewählt haben, gibt das Portal im klassischen Portal ein mit /28.  Anzahl der CIDR-Adresse im klassischen Portal kann nicht angepasst werden. Gatewaysubnetz erscheint als **Gateway** im klassischen Portal zwar der tatsächlichen Namen des gatewaysubnetz, das erstellt wird tatsächlich **Subnetzname**. Sie können diesen Namen mithilfe von PowerShell oder im Azure-Portal anzeigen.

7. Klicken Sie auf das Häkchen am unteren Rand der Seite und Ihr virtuelle Netzwerk erstellen beginnt. Beendigung, sehen Sie auf der Seite **Netzwerk** im klassischen Portal **erstellt** unter **Status** aufgeführt.

## <a name="gw"></a>Erstellen Sie das gateway

1. Auf der Seite **Netzwerk** das virtuelle Netzwerk, das Sie gerade erstellt haben, klicken Sie **Dashboard** am oberen Rand der Seite.

2. Ende der **Dashboard** -Seite auf **Gateway erstellen** und aktivieren Sie **Dynamisches Routing**. Klicken Sie auf **Ja** , dass Sie ein Gateway erstellen möchten.

3. Beginnt das Gateway erstellen, sehen Sie eine Meldung lassen wissen Sie, dass das Gateway gestartet wurde. Es dauert bis zu 45 Minuten für das Gateway erstellen.

4. Verknüpfen Sie Ihr Netzwerk einer. Führen Sie die Schritte im Artikel [VNets an ExpressRoute verknüpfen](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Konfigurieren einer vorhandenen klassischen VNet für ExpressRoute

Haben Sie bereits eine klassische VNet, können Sie es im klassischen Portal eine Verbindung mit ExpressRoute konfigurieren. Die Einstellungen entsprechen den obigen Abschnitten so Abschnitte mit erforderlich zu lesen. Eine Koexistenz ExpressRoute/Standort-zu-Standort-Verbindung erstellen, finden Sie die Schritte [in diesem Artikel](expressroute-howto-coexist-classic.md) . Sie unterscheiden sich die Schritte in diesem Artikel.
 
1. Sie müssen im lokale Netzwerk erstellen, bevor Sie die restlichen Einstellungen VNet aktualisieren. Klicken Sie auf **neu** , erstellen ein neues lokales Netzwerk ist beim Konfigurieren von ExpressRoute über das klassische Portal **>** **Netzwerkdienste** **>** **Virtual Network** **>** **lokalen Netzwerk hinzufügen**. Führen Sie die Assistenten erstellen das lokale Netzwerk.

2. Seite **Konfigurieren** verwenden die Einstellung für die VNet zu aktualisieren und das VNet mit dem lokalen Netzwerk.

3. Gehen Sie nach konfigurieren zum Abschnitt [erstellen das Gateway](#gw) auf das Gateway zu erstellen.


## <a name="next-steps"></a>Nächste Schritte

- Virtuelle Maschinen virtuelle Netzwerk hinzufügen finden Sie in der [virtuellen Maschinen Lernpfade](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Weitere Informationen zu ExpressRoute, finden Sie unter [Übersicht über ExpressRoute](expressroute-introduction.md).


 
