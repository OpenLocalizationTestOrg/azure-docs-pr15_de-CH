<properties
   pageTitle="Für eine ExpressRoute-Verbindung mit PowerShell klassischen Bereitstellungsmodell routing konfigurieren | Microsoft Azure"
   description="Dieser Artikel führt Sie durch die Schritte zum Erstellen und Bereitstellen der Private, öffentliche und Microsoft peering von ExpressRoute-Verbindung. In diesem Artikel auch veranschaulicht den Status aktualisieren oder Löschen von Peerings für Ihre Verbindung."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Erstellen und Ändern von routing für eine ExpressRoute-Verbindung

> [AZURE.SELECTOR]
[Azure-Portal - Ressourcen-Manager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - Ressourcen-Manager](expressroute-howto-routing-arm.md)
[PowerShell - Klassisch](expressroute-howto-routing-classic.md)



Dieser Artikel führt Sie durch die Schritte zum Erstellen und Verwalten von routing-Konfiguration für eine ExpressRoute-Verbindung mit PowerShell und klassischen Bereitstellungsmodell. In den nachfolgenden Schritten werden auch zeigen, wie Status, Update oder Delete und Entziehen von Peerings für eine ExpressRoute-Verbindung.


**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Erforderliche Konfiguration

- Sie benötigen die neueste Version von Azure PowerShell-Module. Sie können aktuelle PowerShell-Modul von PowerShell Teil der [Azure-Downloadseite](https://azure.microsoft.com/downloads/). Führen Sie die Schritte [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Seite schrittweise Anleitung zum Konfigurieren Ihres Computers Azure PowerShell-Module. 
- Stellen Sie sicher, dass Sie der Seite [erforderliche Komponenten](expressroute-prerequisites.md) der Seite [routing von Vorschriften](expressroute-routing.md) und [der Seite](expressroute-workflows.md) überprüft haben, vor der Konfiguration.
- Sie benötigen eine aktive ExpressRoute-Verbindung. Gehen Sie zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) und haben Sie die Leitung von Ihrem Konnektivität aktiviert werden, bevor Sie fortfahren. ExpressRoute-Verbindung muss einen Zustand bereitgestellt und aktiviert Sie die Cmdlets beschrieben ausführen.

>[AZURE.IMPORTANT] Diese Schritte gelten nur für Stromkreise mit Layer 2-Konnektivität Dienstleistungen erstellt. Wenn Sie ein Dienstleister mit Layer 3-Dienstleistungen (in der Regel ein IPVPN, wie MPLS) verwenden, wird Dienstanbieter Verbindung konfigurieren und verwalten Sie routing.

Sie können eine, zwei oder alle drei Peerings (Azure private, Azure Public und Microsoft) für eine ExpressRoute-Verbindung konfigurieren. Sie können Peerings in beliebiger Reihenfolge wählen Sie. Sie müssen jedoch sicherstellen, dass die Konfiguration der einzelnen Peers gleichzeitig. 

## <a name="azure-private-peering"></a>Azure private peering

Dieser Abschnitt enthält Informationen zum Erstellen, abrufen, aktualisieren und Löschen von Azure private peering Konfiguration für ExpressRoute-Verbindung. 

### <a name="to-create-azure-private-peering"></a>Azure private peering erstellen

1. **Importieren Sie das PowerShell-Modul ExpressRoute.**
    
    Verwendung der ExpressRoute-Cmdlets müssen Sie Module Azure und ExpressRoute in der PowerShell-Sitzung importieren. Führen Sie die folgenden Befehle in der PowerShell-Sitzung Azure und ExpressRoute Module importieren.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Erstellen Sie eine ExpressRoute-Verbindung.**
    
    Führen Sie die Schritte erstellen eine [ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) und der konnektivitätsanbieter bereitgestellt. Wenn Ihre Verbindung Dienstleistungen Layer 3 Anbieter, können Sie Dienstanbieter Konnektivität ermöglichen Azure private peering Sie anfordern. In diesem Fall müssen Sie in den nächsten Abschnitten Anweisungen befolgen. Aber wenn Dienstanbieter Konnektivität nicht routing verwaltet, nach dem Erstellen der Verbindung gehen. 

3. **Überprüfen Sie die ExpressRoute-Verbindung, um sicherzustellen, dass es bereitgestellt wird.**

    Sie müssen zuerst, ob die ExpressRoute-Verbindung bereitgestellt und aktiviert. Siehe Beispiel unten.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Stellen Sie sicher, dass die Verbindung bereitgestellt und aktiviert wird. Wenn nicht, arbeiten Sie mit Dienstanbieter Konnektivität zu der Verbindung erforderlichen Zustand und Status.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfigurieren Sie Azure private peering für die Verbindung.**

    Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie mit den nächsten Schritten fortfahren:

    - Keine/30 Subnetz für die primäre Verknüpfung. Dies muss keine Adressraum für virtuelle Netzwerke gehören.
    - Keine/30 Subnetz für den zweiten Link. Dies muss keine Adressraum für virtuelle Netzwerke gehören.
    - Eine gültige VLAN-ID zu peering auf. Sicherstellen Sie, dass keine anderen peering in den Kreislauf die gleiche VLAN-ID verwendet
    - Anzahl Peering. Sie können 2-Byte- und 4-Byte-Zahlen. Sie können Private als Nummer für dieses peering. Stellen Sie sicher, dass Sie nicht 65515.
    - MD5-Hash verwenden möchten. **Dieses Feld ist optional**.
    
    Sie können das folgende Cmdlet konfigurieren Azure private peering für Ihre Verbindung ausführen.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    Mit dem folgenden Cmdlet können MD5-Hash verwendet.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Sicherstellen Sie, dass die AS-Nummer als Peers ASN nicht ASN Customer angeben.

### <a name="to-view-azure-private-peering-details"></a>Azure private peering Details anzeigen

Sie können mit dem folgenden Cmdlet Konfigurationsdetails abrufen.

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Azure private peering Konfiguration aktualisieren

Sie können einen Teil der Konfiguration mit dem folgenden Cmdlet. Im folgenden Beispiel wird die VLAN-ID der Verbindung von 100 bis 500 aktualisiert.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Azure private peering löschen

Das folgende Cmdlet ausführen, können Sie Ihre Peers Konfiguration entfernen.

>[AZURE.WARNING] Sie müssen sicherstellen, dass alle virtuellen Netzwerke ExpressRoute-Verbindung aufgehoben werden, bevor Sie dieses Cmdlet ausführen. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure öffentliche peering

Dieser Abschnitt enthält Informationen zum Erstellen, abrufen, aktualisieren und Löschen von Azure öffentliche peering Konfiguration für ExpressRoute-Verbindung.

### <a name="to-create-azure-public-peering"></a>Azure öffentliche peering erstellen

1. **Importieren Sie das PowerShell-Modul ExpressRoute.**
    
    Verwendung der ExpressRoute-Cmdlets müssen Sie Module Azure und ExpressRoute in der PowerShell-Sitzung importieren. Führen Sie die folgenden Befehle in der PowerShell-Sitzung Azure und ExpressRoute Module importieren. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Erstellen Sie eine ExpressRoute-Verbindung**
    
    Führen Sie die Schritte erstellen eine [ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) und der konnektivitätsanbieter bereitgestellt. Wenn Ihre Verbindung Dienstleistungen Layer 3 Anbieter, können Sie Dienstanbieter Konnektivität ermöglichen Azure öffentliche peering Sie anfordern. In diesem Fall müssen Sie in den nächsten Abschnitten Anweisungen befolgen. Aber wenn Dienstanbieter Konnektivität nicht routing verwaltet, nach dem Erstellen der Verbindung gehen.

3. **Überprüfen Sie ExpressRoute-Verbindung, um sicherzustellen, dass es bereitgestellt wird**

    Sie müssen zuerst, ob die ExpressRoute-Verbindung bereitgestellt und aktiviert. Siehe Beispiel unten.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Stellen Sie sicher, dass die Verbindung bereitgestellt und aktiviert wird. Wenn nicht, arbeiten Sie mit Dienstanbieter Konnektivität zu der Verbindung erforderlichen Zustand und Status.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Konfigurieren Sie Azure öffentliche peering für die Verbindung**

    Stellen Sie sicher, dass die folgenden Informationen verfügen, bevor Sie fortfahren.

    - Keine/30 Subnetz für die primäre Verknüpfung. Dies muss eine gültige öffentliche IPv4-Adresspräfix.
    - Keine/30 Subnetz für den zweiten Link. Dies muss eine gültige öffentliche IPv4-Adresspräfix.
    - Eine gültige VLAN-ID zu peering auf. Sicherstellen Sie, dass keine anderen peering in den Kreislauf die gleiche VLAN-ID verwendet
    - Anzahl Peering. Sie können 2-Byte- und 4-Byte-Zahlen.
    - MD5-Hash verwenden möchten. **Dieses Feld ist optional**.
    
    Sie können das folgende Cmdlet konfigurieren Azure öffentliche peering für Ihre Verbindung ausführen.

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    Das folgende Cmdlet können Sie wählen einen MD5-hash

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Sicherstellen Sie, dass die AS-Nummer als Peers ASN und nicht Kunde ASN angeben.

### <a name="to-view-azure-public-peering-details"></a>Azure peering Einzelheiten anzeigen

Sie können mit dem folgenden Cmdlet Konfigurationsdetails abrufen.

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Azure öffentliche peering Konfiguration aktualisieren

Sie können einen Teil der Konfiguration mit dem folgenden cmdlet

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

Die VLAN-ID der Verbindung wird von 200 bis 600 im obigen Beispiel aktualisiert.

### <a name="to-delete-azure-public-peering"></a>Azure öffentliche peering löschen

Sie können Ihre Peers Konfiguration entfernen das folgende Cmdlet ausführen

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft peering

Dieser Abschnitt enthält Informationen zum Erstellen, abrufen, aktualisieren und Löschen von Microsoft peering Konfiguration für ExpressRoute-Verbindung. 

### <a name="to-create-microsoft-peering"></a>Microsoft peering erstellen

1. **Importieren Sie das PowerShell-Modul ExpressRoute.**
    
    Verwendung der ExpressRoute-Cmdlets müssen Sie Module Azure und ExpressRoute in der PowerShell-Sitzung importieren. Führen Sie die folgenden Befehle in der PowerShell-Sitzung Azure und ExpressRoute Module importieren.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Erstellen Sie eine ExpressRoute-Verbindung**
    
    Führen Sie die Schritte erstellen eine [ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) und der konnektivitätsanbieter bereitgestellt. Wenn Ihre Verbindung Dienstleistungen Layer 3 Anbieter, können Sie Dienstanbieter Konnektivität ermöglichen Azure private peering Sie anfordern. In diesem Fall müssen Sie in den nächsten Abschnitten Anweisungen befolgen. Aber wenn Dienstanbieter Konnektivität nicht routing verwaltet, nach dem Erstellen der Verbindung gehen.

3. **Überprüfen Sie ExpressRoute-Verbindung, um sicherzustellen, dass es bereitgestellt wird**

    Sie müssen zuerst die ExpressRoute-Verbindung bereitgestellt und aktiviert ist.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Stellen Sie sicher, dass die Verbindung bereitgestellt und aktiviert wird. Wenn nicht, arbeiten Sie mit Dienstanbieter Konnektivität zu der Verbindung erforderlichen Zustand und Status.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfigurieren von Microsoft für das Circuit peering**

    Stellen Sie sicher, dass die folgenden Informationen verfügen, bevor Sie fortfahren.

    - Keine/30 Subnetz für die primäre Verknüpfung. Muss eine gültige öffentliche IPv4-Adresspräfix Besitz und in einer RIR registriert / IRR.
    - Keine/30 Subnetz für den zweiten Link. Muss eine gültige öffentliche IPv4-Adresspräfix Besitz und in einer RIR registriert / IRR.
    - Eine gültige VLAN-ID zu peering auf. Sicherstellen Sie, dass keine anderen peering in den Kreislauf die gleiche VLAN-ID verwendet
    - Anzahl Peering. Sie können 2-Byte- und 4-Byte-Zahlen.
    - Präfixe angekündigt: Geben Sie eine Übersicht über alle Präfixe über BGP-Sitzung angekündigt werden soll. Nur öffentliche IP-Adresspräfixe werden akzeptiert. Wenn Sie Präfixe senden möchten, können Sie eine kommagetrennte Liste senden. Diese Präfixe müssen Sie in einer RIR registriert / IRR.
    - ASN Customer: Sind Sie Werbung Präfixe, die peering Anzahl nicht registriert sind, können Sie die AS-Nummer angeben sie registriert. **Dieses Feld ist optional**.
    - Registrierungsname für das Datensatzrouting: Sie können die RIR / IRR gegen die wie Anzahl und Präfixe registriert sind.
    - Ein MD5-Hash, verwenden möchten. **Dies ist optional.**
    
    Sie können das folgende Cmdlet konfiguriert Microsoft Pering für die Verbindung ausführen.

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>Microsoft peering Details anzeigen

Mit dem folgenden Cmdlet Konfigurationsdetails erhalten.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Microsoft peering Konfiguration aktualisieren

Sie können einen Teil der Konfiguration mit dem folgenden Cmdlet.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Microsoft peering löschen

Das folgende Cmdlet ausführen, können Sie Ihre Peers Konfiguration entfernen.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Nächste Schritte

Nächsten [Link ein VNet ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md).


-  Weitere Informationen zu Workflows finden Sie unter [ExpressRoute Workflows](expressroute-workflows.md).
-  Finden Sie weitere Informationen Verbindung peering [ExpressRoute Stromkreise und routing-Domänen](expressroute-circuit-peerings.md).

