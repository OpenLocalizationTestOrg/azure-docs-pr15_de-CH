<properties
   pageTitle="VNet Peering mit Powershell-Cmdlets erstellen | Microsoft Azure"
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
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Erstellen von VNet mithilfe von Powershell-Cmdlets Peering

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Erstellen einer VNet mithilfe von PowerShell peering führen Sie die folgenden Schritte aus:

1. Wenn Azure PowerShell noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) und gehen Sie bis Ende Azure anmelden und Ihr Abonnement auswählen.

> [AZURE.NOTE] PowerShell-Cmdlets für die Verwaltung von VNet peering Lieferumfang [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Lesen Sie virtuelle Objekte:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Um VNet peering einzurichten, müssen Sie zwei Links für jede Richtung erstellen. Im folgende Schritt erstellt eine Peeringverbindung VNet zuerst für VNet1, VNet2:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Die Ausgabe zeigt:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Dieser Schritt erstellt eine Peeringverbindung VNet für VNet2, VNet1:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Die Ausgabe zeigt:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Erstellte VNet Peeringverbindung den folgenden Status angezeigt:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Die Ausgabe zeigt:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Es gibt einige konfigurierbare Eigenschaften für peering VNet:

  	|Option|Beschreibung|Standard|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Ob Adressraum des Peer-VNet Virtual_network-Tag enthalten sein|Ja|
  	|AllowForwardedTraffic|Ob Datenverkehr nicht von hervorragendem VNet akzeptiert oder verworfen|Nein|
  	|AllowGatewayTransit|Peer-VNet VNet-Gateway verwenden können|Nein|
  	|UseRemoteGateways|Verwenden des Peers VNet Gateway. Peer-VNet muss ein Gateway-Konfiguration und AllowGatewayTransit ausgewählt. Sie können diese Option verwenden, wenn Gateway konfiguriert haben|Nein|

    Jeder Link VNet peering hat den Eigenschaften. Sie können beispielsweise AllowVirtualNetworkAccess True Peeringverbindung VNet VNet1 auf VNet2 festgelegt und für die Peeringverbindung VNet in die andere Richtung auf False festgelegt.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Sie können Get-AzureRmVirtualNetworkPeering, überprüfen den Eigenschaftswert nach der Änderung ausführen. Aus der Ausgabe finden Sie AllowForwardedTraffic auf True ändert nach Ausführung der oben genannten Cmdlets.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Nachdem peering in diesem Szenario besteht, sollten Sie die Verbindungen von jedem virtuellen Computer mit einem virtuellen Computer beide vnets sein. Standardmäßig gilt für AllowVirtualNetworkAccess und VNet peering Bereitstellen der richtigen ACLs der Kommunikation zwischen VNets. Netzwerkregeln für Group (NSG) Verbindung zwischen bestimmten Subnetzen oder virtuelle Computer detailliert Kontrolle des Zugriffs zwischen zwei virtuelle Netzwerke blockieren können weiterhin anwenden.  Weitere Informationen zum Erstellen von NSG Regeln finden Sie in diesem [Artikel](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Erstellen VNet über Abonnements PowerShell peering führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei Azure privilegierter Benutzer A ist für ein Abonnement und führen das folgende Cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Dies ist nicht erforderlich, peering hergestellt werden auch wenn Benutzer einzeln peering Anfragen für ihre jeweiligen VNets Anfragen übereinstimmen. Hinzufügen privilegierter andere VNet als Benutzer in der lokalen VNet erleichtert das Setup.

2. Anmeldung bei Azure privilegierte Benutzer B Konto B Abonnement und führen Sie das folgende Cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. Benutzer A ist in Sitzung, führen Sie das folgende Cmdlet:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. Führen Sie in Benutzer-B-Sitzung mit dem folgenden Cmdlet:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Nachdem peering hergestellt wurde, sollte jede virtuelle Maschine im VNet3 mit einer virtuellen Maschine in VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In diesem Szenario können Sie die PowerShell-Cmdlets zu VNet peering unten ausführen.  Sie müssen die AllowForwardedTraffic-Eigenschaft auf True festgelegt und VNET1, HubVNet, eingehenden Datenverkehr von außerhalb peering VNet Adressraum ermöglicht.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Nachdem peering eingerichtet wurde, können Sie finden Sie in diesem [Artikel](virtual-network-create-udr-arm-ps.md) und definieren Sie eine benutzerdefinierte Route (UDR) VNet1 Datenverkehr über ein virtuelles Gerät seine Funktionen umleiten. Wenn die Adresse des nächste Hop in der Route angeben, können Sie es der IP-Adresse die virtuelle Appliance Peer VNet HubVNet festlegen. Es folgt ein Beispiel:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Erstellen Sie ein VNet zwischen einem klassischen virtuellen und ein Azure-Ressourcen-Manager virtuelle Netzwerk in PowerShell peering wie folgt:

1. Virtuelles Netzwerkobjekt für **VNET1**, das virtuelle Netzwerk Azure-Ressourcen-Manager wie folgt:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Nur einen Link zu VNet peering in diesem Szenario ist erforderlich, insbesondere eine Verknüpfung zwischen **VNET1** und **VNET2**. Dieser Schritt erfordert Kenntnis der klassischen VNet Ressourcen-ID. Ressource Gruppe ID Format sieht folgendermaßen aus:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Achten Sie darauf, dass SubscriptionID, ResourceGroupName und VirtualNetworkName mit den entsprechenden Namen ersetzen.

    Dies kann wie folgt erfolgen:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Einmal die Peeringverbindung erstellt VNet Verbindungsstatus angezeigt wie in der folgenden Ausgabe gezeigt:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>Entfernen VNet Peering

1.  Um peering VNet zu entfernen, müssen Sie das folgende Cmdlet ausführen:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Wenn Sie eine Verknüpfung in einem VNET peering entfernen, gehen Peer-Verbindungsstatus getrennt an. In diesem Zustand können nicht Sie die Verknüpfung erneut erstellen, bis der Verbindungsstatus Peer initiiert wird. Wir empfehlen, beide Links entfernen, bevor Sie VNet peering neu erstellen.
