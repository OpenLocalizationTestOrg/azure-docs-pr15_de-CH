<properties
   pageTitle="Routing für eine ExpressRoute-Verbindung konfigurieren | Microsoft Azure"
   description="Dieser Artikel führt Sie durch die Schritte zum Erstellen und Bereitstellen der Private, öffentliche und Microsoft peering von ExpressRoute-Verbindung. In diesem Artikel auch veranschaulicht den Status aktualisieren oder Löschen von Peerings für Ihre Verbindung."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Erstellen und Ändern von routing für eine ExpressRoute-Verbindung


> [AZURE.SELECTOR]
[Azure-Portal - Ressourcen-Manager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - Ressourcen-Manager](expressroute-howto-routing-arm.md)
[PowerShell - Klassisch](expressroute-howto-routing-classic.md)



Dieser Artikel führt Sie durch die Schritte zum Erstellen und Verwalten von routing-Konfiguration für eine ExpressRoute-Verbindung mit PowerShell und Azure-Ressourcen-Manager-Bereitstellungsmodell.  In den nachfolgenden Schritten werden auch zeigen, wie Status, Update oder Delete und Entziehen von Peerings für eine ExpressRoute-Verbindung. 


**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Erforderliche Konfiguration

- Sie benötigen die neueste Version von Azure PowerShell-Module Version 1.0 oder höher. 
- Stellen Sie sicher, dass Sie der Seite [erforderliche Komponenten](expressroute-prerequisites.md) der Seite [routing von Vorschriften](expressroute-routing.md) und [der Seite](expressroute-workflows.md) überprüft haben, vor der Konfiguration.
- Sie benötigen eine aktive ExpressRoute-Verbindung. Gehen Sie zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) und haben Sie die Leitung von Ihrem Konnektivität aktiviert werden, bevor Sie fortfahren. ExpressRoute-Verbindung muss einen Zustand bereitgestellt und aktiviert Sie die Cmdlets beschrieben ausführen.

Diese Schritte gelten nur für Stromkreise mit Layer 2-Konnektivität Dienstleistungen erstellt. Wenn Sie ein Dienstleister mit Layer 3-Dienstleistungen (in der Regel ein IPVPN, wie MPLS) verwenden, wird Dienstanbieter Verbindung konfigurieren und verwalten Sie routing.

>[AZURE.IMPORTANT] Wir anzeigen derzeit nicht von Dienstanbietern über Service Management-Portal konfiguriert Peerings. Wir arbeiten schnell, diese Fähigkeit zu aktivieren. Überprüfen Sie vor dem Konfigurieren von BGP Peerings mit Ihrem Dienstanbieter.

Sie können eine, zwei oder alle drei Peerings (Azure private, Azure Public und Microsoft) für eine ExpressRoute-Verbindung konfigurieren. Sie können Peerings in beliebiger Reihenfolge wählen Sie. Sie müssen jedoch sicherstellen, dass die Konfiguration der einzelnen Peers gleichzeitig. 

## <a name="azure-private-peering"></a>Azure private peering

Dieser Abschnitt enthält Informationen zum Erstellen, abrufen, aktualisieren und Löschen von Azure private peering Konfiguration für ExpressRoute-Verbindung. 

### <a name="to-create-azure-private-peering"></a>Azure private peering erstellen

1. Importieren Sie das PowerShell-Modul ExpressRoute.
    
    Installieren Sie den neuesten PowerShell Installer von [PowerShell Gallery](http://www.powershellgallery.com/) , und Importieren der Azure-Ressourcen-Manager-Module in der PowerShell-Sitzung mit ExpressRoute-Cmdlets. Sie müssen PowerShell als Administrator ausführen.

        Install-Module AzureRM

        Install-AzureRM

    Alle Module AzureRM.* im Bereich bekannte semantische version

        Import-AzureRM

    Sie können auch ein select Modul im Bereich bekannte semantische version 
        
        Import-Module AzureRM.Network 

    Anmeldung bei Ihrem Konto

        Login-AzureRmAccount

    Wählen Sie den Dauerauftrag zu ExpressRoute-Verbindung erstellen
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Erstellen Sie eine ExpressRoute-Verbindung.
    
    Führen Sie die Schritte erstellen eine [ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) und der konnektivitätsanbieter bereitgestellt. 

    Wenn Ihre Verbindung Dienstleistungen Layer 3 Anbieter, können Sie Dienstanbieter Konnektivität ermöglichen Azure private peering Sie anfordern. In diesem Fall müssen Sie in den nächsten Abschnitten Anweisungen befolgen. Aber wenn Dienstanbieter Konnektivität nicht routing verwaltet, nach dem Erstellen der Verbindung gehen. 

3. Überprüfen Sie die ExpressRoute-Verbindung, um sicherzustellen, dass es bereitgestellt wird.

    Sie müssen zuerst, ob die ExpressRoute-Verbindung bereitgestellt und aktiviert. Siehe Beispiel unten.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Die Antwort ist ähnlich dem folgenden Beispiel:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. Konfigurieren Sie Azure private peering für die Verbindung.

    Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie mit den nächsten Schritten fortfahren:

    - Keine/30 Subnetz für die primäre Verknüpfung. Dies muss keine Adressraum für virtuelle Netzwerke gehören.
    - Keine/30 Subnetz für den zweiten Link. Dies muss keine Adressraum für virtuelle Netzwerke gehören.
    - Eine gültige VLAN-ID zu peering auf. Sicherstellen Sie, dass keine anderen peering in den Kreislauf die gleiche VLAN-ID verwendet
    - Anzahl Peering. Sie können 2-Byte- und 4-Byte-Zahlen. Sie können Private als Nummer für dieses peering. Stellen Sie sicher, dass Sie nicht 65515.
    - MD5-Hash verwenden möchten. **Dieses Feld ist optional**.
    
    Sie können das folgende Cmdlet konfigurieren Azure private peering für Ihre Verbindung ausführen.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Mit dem folgenden Cmdlet können MD5-Hash verwendet.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Sicherstellen Sie, dass die AS-Nummer als Peers ASN nicht ASN Customer angeben.

### <a name="to-view-azure-private-peering-details"></a>Azure private peering Details anzeigen

Sie können mit dem folgenden Cmdlet Konfigurationsdetails abrufen.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>Azure private peering Konfiguration aktualisieren

Sie können einen Teil der Konfiguration mit dem folgenden Cmdlet. Im folgenden Beispiel wird die VLAN-ID der Verbindung von 100 bis 500 aktualisiert.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>Azure private peering löschen

Das folgende Cmdlet ausführen, können Sie Ihre Peers Konfiguration entfernen.

>[AZURE.WARNING] Sie müssen sicherstellen, dass alle virtuellen Netzwerke ExpressRoute-Verbindung aufgehoben werden, bevor Sie dieses Cmdlet ausführen. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure öffentliche peering

Dieser Abschnitt enthält Informationen zum Erstellen, abrufen, aktualisieren und Löschen von Azure öffentliche peering Konfiguration für ExpressRoute-Verbindung.

### <a name="to-create-azure-public-peering"></a>Azure öffentliche peering erstellen

1. Importieren Sie das PowerShell-Modul ExpressRoute.
    
    Installieren Sie den neuesten PowerShell Installer von [PowerShell Gallery](http://www.powershellgallery.com/) , und Importieren der Azure-Ressourcen-Manager-Module in der PowerShell-Sitzung mit ExpressRoute-Cmdlets. Sie müssen PowerShell als Administrator ausführen.

        Install-Module AzureRM

        Install-AzureRM

    Alle Module AzureRM.* im Bereich bekannte semantische version

        Import-AzureRM

    Sie können auch ein select Modul im Bereich bekannte semantische version 
        
        Import-Module AzureRM.Network 

    Anmeldung bei Ihrem Konto

        Login-AzureRmAccount

    Wählen Sie den Dauerauftrag zu ExpressRoute-Verbindung erstellen
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Erstellen Sie eine ExpressRoute-Verbindung.
    
    Führen Sie die Schritte erstellen eine [ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) und der konnektivitätsanbieter bereitgestellt. 

    Wenn Ihre Verbindung Dienstleistungen Layer 3 Anbieter, können Sie Dienstanbieter Konnektivität ermöglichen Azure öffentliche peering Sie anfordern. In diesem Fall müssen Sie in den nächsten Abschnitten Anweisungen befolgen. Aber wenn Dienstanbieter Konnektivität nicht routing verwaltet, nach dem Erstellen der Verbindung gehen.

3. Überprüfen Sie die ExpressRoute-Verbindung, um sicherzustellen, dass es bereitgestellt wird.

    Sie müssen zuerst, ob die ExpressRoute-Verbindung bereitgestellt und aktiviert. Siehe Beispiel unten.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Die Antwort ist ähnlich dem folgenden Beispiel:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. Konfigurieren Sie Azure öffentliche peering für die Verbindung.

    Stellen Sie sicher, dass die folgenden Informationen verfügen, bevor Sie fortfahren.

    - Keine/30 Subnetz für die primäre Verknüpfung. Dies muss eine gültige öffentliche IPv4-Adresspräfix.
    - Keine/30 Subnetz für den zweiten Link. Dies muss eine gültige öffentliche IPv4-Adresspräfix.
    - Eine gültige VLAN-ID zu peering auf. Sicherstellen Sie, dass keine anderen peering in den Kreislauf die gleiche VLAN-ID verwendet
    - Anzahl Peering. Sie können 2-Byte- und 4-Byte-Zahlen.
    - MD5-Hash verwenden möchten. **Dieses Feld ist optional**.
    
    Sie können das folgende Cmdlet konfigurieren Azure öffentliche peering für Ihre Verbindung ausführen.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Das folgende Cmdlet können Sie wählen einen MD5-hash

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Sicherstellen Sie, dass die AS-Nummer als Peers ASN und nicht Kunde ASN angeben.

### <a name="to-view-azure-public-peering-details"></a>Azure peering Einzelheiten anzeigen

Sie können mit dem folgenden Cmdlet Konfigurationsdetails abrufen.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>Azure öffentliche peering Konfiguration aktualisieren

Sie können einen Teil der Konfiguration mit dem folgenden cmdlet

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Die VLAN-ID der Verbindung wird von 200 bis 600 im obigen Beispiel aktualisiert.

### <a name="to-delete-azure-public-peering"></a>Azure öffentliche peering löschen

Sie können Ihre Peers Konfiguration entfernen das folgende Cmdlet ausführen

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Microsoft peering

Dieser Abschnitt enthält Informationen zum Erstellen, abrufen, aktualisieren und Löschen von Microsoft peering Konfiguration für ExpressRoute-Verbindung. 

### <a name="to-create-microsoft-peering"></a>Microsoft peering erstellen

1. Importieren Sie das PowerShell-Modul ExpressRoute.
    
    Installieren Sie den neuesten PowerShell Installer von [PowerShell Gallery](http://www.powershellgallery.com/) , und Importieren der Azure-Ressourcen-Manager-Module in der PowerShell-Sitzung mit ExpressRoute-Cmdlets. Sie müssen PowerShell als Administrator ausführen.

        Install-Module AzureRM

        Install-AzureRM

    Alle Module AzureRM.* im Bereich bekannte semantische version

        Import-AzureRM

    Sie können auch ein select Modul im Bereich bekannte semantische version 
        
        Import-Module AzureRM.Network 

    Anmeldung bei Ihrem Konto

        Login-AzureRmAccount

    Wählen Sie den Dauerauftrag zu ExpressRoute-Verbindung erstellen
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Erstellen Sie eine ExpressRoute-Verbindung.
    
    Führen Sie die Schritte erstellen eine [ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) und der konnektivitätsanbieter bereitgestellt. 

    Wenn Ihre Verbindung Dienstleistungen Layer 3 Anbieter, können Sie Dienstanbieter Konnektivität ermöglichen Azure private peering Sie anfordern. In diesem Fall müssen Sie in den nächsten Abschnitten Anweisungen befolgen. Aber wenn Dienstanbieter Konnektivität nicht routing verwaltet, nach dem Erstellen der Verbindung gehen.

3. Überprüfen Sie die ExpressRoute-Verbindung, um sicherzustellen, dass es bereitgestellt wird.

    Sie müssen zuerst, ob die ExpressRoute-Verbindung bereitgestellt und aktiviert. Siehe Beispiel unten.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Die Antwort ist ähnlich dem folgenden Beispiel:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Konfigurieren Sie Microsoft peering für die Verbindung.

    Stellen Sie sicher, dass die folgenden Informationen verfügen, bevor Sie fortfahren.

    - Keine/30 Subnetz für die primäre Verknüpfung. Muss eine gültige öffentliche IPv4-Adresspräfix Besitz und in einer RIR registriert / IRR.
    - Keine/30 Subnetz für den zweiten Link. Muss eine gültige öffentliche IPv4-Adresspräfix Besitz und in einer RIR registriert / IRR.
    - Eine gültige VLAN-ID zu peering auf. Sicherstellen Sie, dass keine anderen peering in den Kreislauf die gleiche VLAN-ID verwendet
    - Anzahl Peering. Sie können 2-Byte- und 4-Byte-Zahlen.
    - Präfixe angekündigt: Geben Sie eine Übersicht über alle Präfixe über BGP-Sitzung angekündigt werden soll. Nur öffentliche IP-Adresspräfixe werden akzeptiert. Wenn Sie Präfixe senden möchten, können Sie eine kommagetrennte Liste senden. Diese Präfixe müssen Sie in einer RIR registriert / IRR.
    - ASN Customer: Sind Sie Werbung Präfixe, die peering Anzahl nicht registriert sind, können Sie die AS-Nummer angeben sie registriert. **Dieses Feld ist optional**.
    - Registrierungsname für das Datensatzrouting: Sie können die RIR / IRR gegen die wie Anzahl und Präfixe registriert sind.
    - Ein MD5-Hash verwenden möchten. **Dies ist optional.**
    
    Sie können das folgende Cmdlet konfiguriert Microsoft peering für Ihre Verbindung ausführen.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>Microsoft peering Details abrufen

Mit dem folgenden Cmdlet Konfigurationsdetails erhalten.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Microsoft peering Konfiguration aktualisieren

Sie können einen Teil der Konfiguration mit dem folgenden Cmdlet.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>Microsoft peering löschen

Das folgende Cmdlet ausführen, können Sie Ihre Peers Konfiguration entfernen.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Nächste Schritte

Nächstes [Link ein VNet ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md).

-  Weitere Informationen zu ExpressRoute Workflows anzeigen Sie [ExpressRoute workflows](expressroute-workflows.md)

-  Finden Sie weitere Informationen Verbindung peering [ExpressRoute Stromkreise und routing-Domänen](expressroute-circuit-peerings.md).

-  Weitere Informationen zum Arbeiten mit virtuellen Netzwerke Übersicht [virtuelle Netzwerk](../virtual-network/virtual-networks-overview.md).

