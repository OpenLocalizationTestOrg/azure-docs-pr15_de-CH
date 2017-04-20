<properties
   pageTitle="VNet Peering mit Azure-Portal erstellen | Microsoft Azure"
   description="So erstellen Sie ein virtuelles Netzwerk mit der Azure-Portal im Ressourcen-Manager."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Erstellen Sie ein virtuelles Netzwerk mit der Azure-Portal peering

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Erstellen einer VNet peering entsprechend Szenario über Azure-Portal wie folgt.

1. Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Um VNET peering einzurichten, müssen Sie zwei Links für jede Richtung zwischen zwei VNets erstellen. Peeringverbindung für VNET1, VNET2 VNET können erstellen. Klicken Sie auf das Portal **Durchsuchen** > **virtuelle Netzwerke auswählen**

    ![Erstellen Sie VNet peering in Azure-portal](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. Virtuelle Netzwerke Blatt wählen Sie VNET1, Peerings klicken Sie auf Hinzufügen

    ![Wählen Sie peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. Blatt hinzufügen Peering benennen Sie peering Link LinkToVnet2, wählen Sie das Abonnement mit dem Peer virtuelle Netzwerk VNET2, klicken Sie auf OK.

    ![Link zu VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Sobald diese Peeringverbindung VNET erstellt. Sie sehen den Status wie folgt:

    ![Verbindungsstatus](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Als Nächstes erstellen Sie die Peeringverbindung VNET für VNET2, VNET1. Virtuelle Netzwerke Blatt wählen Sie VNET2, Peerings klicken Sie auf Hinzufügen

    ![Peer aus anderen VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. Blatt hinzufügen Peering benennen Sie peering Link LinkToVnet1, wählen Sie das Abonnement mit dem Peer virtuelles Netzwerk klicken Sie auf OK.

    ![Virtuelles Netzwerk Kachel erstellen](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Sobald diese Peeringverbindung VNET erstellt. Sie sehen den Status wie folgt:

    ![Endgültige Verbindungsstatus](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Überprüfen Sie den Status LinkToVnet2 und jetzt Änderungen auf verbunden.  

    ![Letzten Status 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] VNET peering wird nur hergestellt, wenn beide Links verbunden sind.

Es gibt einige konfigurierbare Eigenschaften für jede Verknüpfung:

|Option|Beschreibung|Standard|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Ob Adressraum des Peer-VNet Virtual_network-Tag enthalten sein|Ja|
|AllowForwardedTraffic|Ob Datenverkehr nicht von hervorragendem VNet akzeptiert oder verworfen|Nein|
|AllowGatewayTransit|Peer-VNet VNet-Gateway verwenden können|Nein|
|UseRemoteGateways|Verwenden des Peers VNet Gateway. Peer-VNet muss ein Gateway-Konfiguration und AllowGatewayTransit ausgewählt. Sie können diese Option verwenden, wenn Gateway konfiguriert haben|Nein|

Jeder Link VNet peering hat eine über Eigenschaften. Portal klicken Sie VNet Peering Link und alle verfügbaren Optionen zu ändern, klicken Sie auf die Auswirkung der Änderung zu speichern.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. In diesem Beispiel nutzen zwei Abonnements A und B und BenutzerA und BenutzerB zwei Benutzer mit Berechtigungen in Abonnements bzw. wir
3. Wählen Sie im Portal klicken Sie auf Durchsuchen, virtuelle Netzwerke. Klicken Sie auf das VNET, und klicken Sie auf Hinzufügen.

    ![Szenario 2 durchsuchen](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Klicken Sie auf Blatt Zugriff hinzufügen wählen Sie eine Rolle Netzwerk Mitwirkenden auswählen, klicken Sie auf Benutzer hinzufügen, geben Sie BenutzerB Zeichen ein und klicken Sie auf OK.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Dies ist nicht erforderlich, peering hergestellt werden auch wenn Benutzer einzeln peering Anfragen für ihre jeweiligen Vnets Anfragen übereinstimmen. Hinzufügen privilegierten Benutzer andere VNet als Benutzer in der lokalen VNet erleichtert das Portal einrichten.

5. Dann melden Sie Azure-Portal mit UserB Berechtigung Benutzer für SubscriptionB. Führen Sie die Schritte BenutzerA als Netzwerk hinzufügen.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] Abmelden, und melden Sie sich beide benutzersitzungen im Browser, um sicherzustellen, dass die Autorisierung erfolgreich aktiviert.

6. Anmeldung zum Portal als BenutzerA zum VNET3 Blade, Peering, klicken Sie auf überprüfen ' mir Meine Ressourcen-ID "Kontrollkästchen und geben die Ressource ID für VNET5 im folgenden Format.

    / subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![Ressourcen-ID](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Anmeldung zum Portal BenutzerB und folgen über Schritt Peeringverbindung von VNET5 zu VNet3 erstellen.

    ![Ressourcen-ID 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Peering hergestellt und alle virtuellen Computer in VNet3 sollte mit einer virtuellen Maschine in VNet5

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Als ersten Schritt Peeringverbindungen VNET aus HubVnet, VNET1. Beachten Sie, dass Option weitergeleitet Datenverkehr zulassen nicht für die Verbindung aktiviert ist.

    ![Grundlegende Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Als Nächstes können Peeringverbindungen VNET1 auf HubVnet erstellt werden. Beachten Sie, dass Allow weitergeleitet Datenverkehr gewählt.

    ![Grundlegende Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Nachdem peering eingerichtet wurde, können Sie finden Sie in diesem [Artikel](virtual-network-create-udr-arm-ps.md) und Benutzer definierten Route(UDR) Umleitung VNet1 Datenverkehr über ein virtuelles Gerät die Funktionen definieren. Wenn die nächste Hop Adresse Route angeben, können Sie die IP-Adresse des virtuellen Appliance Peer VNet HubVNet festlegen


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.

2. Zu VNET peering in diesem Szenario müssen Sie nur einen Link aus dem virtuellen Netzwerk in Azure Resource Manager in Classic erstellen. Das heißt, von **VNET1** zu **VNET2**. Klicken Sie im Portal auf **Durchsuchen** > **Virtuelle Netzwerke** auswählen

3. Wählen Sie das virtuelle Netzwerke Blade **VNET1**. Klicken Sie auf **Peerings**und dann auf **Hinzufügen**.

4. Blade Peering hinzufügen den Namen der Verknüpfung. Hier wird **LinkToVNet2**genannt. Wählen Sie unter Peer Details **Classic**.

5. Wählen Sie das Abonnement mit dem Peer virtuelles Netzwerk **VNET2**. Klicken Sie auf OK.

    ![Vnet 2 Vnet1 verknüpfen](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Erstellte dieses VNet Peeringverbindung virtuellen Netzwerken dies, und Sie Folgendes sehen:

    ![Peering Verbindung überprüfen](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>Entfernen VNet Peering

1.  Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2.  Virtuelles Netzwerk Blade gehen Sie auf Peerings, klicken Sie auf die Verknüpfung, die Sie entfernen möchten, klicken Sie auf Löschen.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Sobald Sie eine Verknüpfung in VNET peering entfernen, gehen Peer-Verbindungsstatus getrennt an.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. In diesem Zustand können nicht Sie die Verknüpfung erneut erstellen, bis der Verbindungsstatus Peer initiiert wird. Wir empfehlen, beide Links entfernen, bevor Sie VNET peering neu erstellen.
