<properties
   pageTitle="Mehrere Gateways Standort-zu-Standort-VPN-Verbindungen zu einem virtuellen Netzwerk für Azure-Portal mit Ressourcen-Manager-Bereitstellungsmodell hinzufügen | Microsoft Azure"
   description="Ein VPN-Gateway, die eine Verbindung fügen Sie mit mehreren S2S Standortconnectors hinzu"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Ein VNet mit einer vorhandenen VPN-Gateway-Verbindung eine Standort-zu-Standort-Verbindung hinzufügen

> [AZURE.SELECTOR]
- [Ressourcenmanager - Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Classic - PowerShell](vpn-gateway-multi-site.md)

Dieser Artikel führt Sie mithilfe des Azure-Portals-Standorten (S2S) Verbindung zu einem VPN-Gateway mit hinzufügen eine Verbindung. Dieser Verbindungstyp wird häufig als "Multi-Site" Konfiguration bezeichnet. 

In diesem Artikel können Sie eine Verbindung S2S ein VNet hinzufügen, die bereits eine S2S Verbindung Punkt-zu-Standort-Verbindung oder VNet VNet Verbindung. Es gibt einige Einschränkungen beim Verbindungen hinzufügen. Überprüfen Sie [vor](#before) Abschnitt in diesem Artikel überprüfen Sie die Konfiguration vor. 

Dieser Artikel gilt für VNets mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt, die ein RouteBased VPN-Gateway. Diese Schritte gelten nicht für ExpressRoute /-Standorten Koexistenz verbindungskonfigurationen. Informationen über Koexistenz finden Sie unter [ExpressRoute-S2S Koexistenz Connections](../expressroute/expressroute-howto-coexist-resource-manager.md) .

### <a name="deployment-models-and-methods"></a>Bereitstellung und-Modelle

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Wir aktualisieren diese Tabelle als neue Artikel und weitere Tools werden für diese Konfiguration. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Bevor Sie beginnen

Überprüfen Sie Folgendes:

- Sie erstellen eine Koexistenz ExpressRoute-S2S-Verbindung nicht.
- Sie haben ein virtuelles Netzwerk erstellt wurde, eine Verbindung mit dem Ressourcen-Manager-Bereitstellungsmodell.
- Virtuelle Netzwerk-Gateway für das VNet ist RouteBased. Haben PolicyBased VPN-Gateway müssen Sie das virtuelle Netzwerkgateway löschen und einen neuen VPN-Gateway RoutBased erstellen.
- Keine Adressbereiche überschneiden für eines der Verbindung dieses VNet VNets.
- Sie haben VPN-Gerät und wer konfigurieren kann. [VPN-Geräte](vpn-gateway-about-vpn-devices.md)finden Sie unter. Vertraut mit Ihrem VPN-Gerät konfiguriert oder sind nicht mit der IP-Adresse befindet sich Bereiche in Ihrem lokalen Netzwerk-Konfiguration, müssen Sie diese Angaben können Sie mit anderen Personen koordinieren.
- Sie haben eine nach außen gerichtete öffentliche IP-Adresse für das VPN-Gerät. Diese IP-Adresse kann nicht hinter einem NAT befinden


## <a name="part1"></a>Teil 1: Konfigurieren einer Verbindung

1. In einem Browser navigieren Sie zum [Azure-Portal](http://portal.azure.com) und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Klicken Sie auf **alle Ressourcen** und suchen Sie Ihr **virtuelles Netzwerk-Gateway** in der Liste der Ressourcen, und klicken Sie auf.
3. Klicken Sie auf **Verbindungen**auf dem **virtuellen Netzwerk-Gateway** -Blade.

    ![Connections-blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Klicken Sie auf die **Verbindung** auf **+ Hinzufügen**.

    ![Schaltfläche Verbindung hinzufügen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. -Blade **Verbindung hinzufügen** die folgenden Felder ausfüllen:
    - **Name:** Den Namen zu der Website, die Sie die Verbindung zu erstellen.
    - **Verbindung:** Wählen Sie **Standort zu Standort (IPsec)**.

    ![Verbindung Blade hinzufügen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Teil 2: Hinzufügen einer lokalen Netzwerk-gateway

1. Klicken Sie auf **Lokales Netzwerk-Gateway** ***Wählen Sie ein lokales Netzwerk-Gateway***. **Wählen Sie LAN Gateway** Blade wird geöffnet.

    ![Wählen Sie LAN-gateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Klicken Sie auf **neu erstellen** , um das **lokale Netzwerk-Gateway erstellen** Blade öffnen.

    ![Erstellen Sie LAN Gateway blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. Auf dem Blatt **Erstellen lokale Netzwerk-Gateway** die folgenden Felder ausfüllen:
    - **Name:** Der Name der lokalen Gateway Netzwerkressource zuweisen möchten.
    - **IP-Adresse:** Die öffentliche IP-Adresse des VPN-Geräts auf der Website, der Sie herstellen möchten.
    - **Adressraum:** Der Adressbereich, die an den neuen Standort des lokalen Netzwerks weitergeleitet werden soll.
4. Klicken Sie auf **OK** , auf das **lokale Netzwerk-Gateway erstellen** zu speichern.

## <a name="part3"></a>Teil 3 - freigegebenen Schlüssel hinzufügen und die Verbindung

1. Das Blade **Verbindung hinzufügen** Hinzufügen des freigegebenen Schlüssels, den zum Herstellen die Verbindung verwenden möchten. Sie erhalten den gemeinsamen Schlüssel vom VPN-Gerät oder stellen einen aus, und konfigurieren Sie das VPN-Gerät um den gleichen gemeinsamen Schlüssel verwenden. Wichtig ist, dass die Schlüssel identisch sind.

    ![Gemeinsamer Schlüssel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Klicken Sie am unteren Rand das Blade auf **OK** , um die Verbindung zu erstellen.

## <a name="part4"></a>Teil 4: Überprüfen Sie die VPN-Verbindung

Sie können die VPN-Verbindung im Portal oder mit PowerShell überprüfen.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Nächste Schritte

- Nach Abschluss die Verbindung können Sie virtuelle Computer, virtuelle Netzwerke hinzufügen. Sehen Sie der virtuelle Computer [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) für Weitere Informationen.
