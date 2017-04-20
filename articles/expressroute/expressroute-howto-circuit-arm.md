<properties
   pageTitle="Erstellen und Ändern einer ExpressRoute-Verbindung mit Ressourcen-Manager und PowerShell | Microsoft Azure"
   description="Dieser Artikel beschreibt das Erstellen, bereitstellen, überprüfen, aktualisieren, löschen und Entziehen von ExpressRoute-Verbindung."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Erstellen und Ändern von ExpressRoute-Verbindung

> [AZURE.SELECTOR]
[Azure-Portal - Ressourcen-Manager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Ressourcen-Manager](expressroute-howto-circuit-arm.md)
[PowerShell - Klassisch](expressroute-howto-circuit-classic.md)


Dieser Artikel beschreibt, wie ein Azure ExpressRoute-Verbindung mithilfe von Windows PowerShell-Cmdlets und Azure-Ressourcen-Manager-Bereitstellungsmodell erstellen. In diesem Artikel wird auch zeigen, wie den Status der Verbindung überprüfen, aktualisieren oder löschen und entziehen sie.

**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Bevor Sie beginnen


- Die neueste Version von Azure PowerShell-Module (mindestens Version 1.0). Schrittweise Anleitung zum Konfigurieren Ihres Computers die PowerShell-Module verwenden führen Sie die Schritte [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

- Überprüfen Sie vor der Konfiguration [erforderlichen Komponenten](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md) .

## <a name="create-and-provision-an-expressroute-circuit"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Melden Sie sich bei Ihrem Azure-Konto und Ihr Abonnement auswählen

Ihre Konfiguration zunächst Ihre Azure Konto anmelden. Weitere Informationen zu PowerShell finden Sie unter [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md). Verwenden Sie die folgenden Beispiele zu verbinden:

    Login-AzureRmAccount

Überprüfen Sie die Abonnements des Kontos:

    Get-AzureRmSubscription

Wählen Sie das Abonnement für eine ExpressRoute-Verbindung erstellen möchten:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Holen Sie die Liste der unterstützten Provider, Standorte und Bandbreiten

Bevor Sie eine ExpressRoute-Verbindung erstellen, benötigen Sie die Liste der konnektivitätsanbieter unterstützt, Speicherorten und Bandbreitenoptionen.

Das PowerShell-Cmdlet `Get-AzureRmExpressRouteServiceProvider` gibt diese Informationen später verwenden:

    Get-AzureRmExpressRouteServiceProvider

Überprüfen der konnektivitätsanbieter aufgeführt ist. Notieren Sie sich die folgenden Informationen, da Sie es später beim Erstellen einer Verbindung benötigen:

- Name

- PeeringLocations

- BandwidthsOffered

Sie nun können eine ExpressRoute-Verbindung erstellen.   

### <a name="3-create-an-expressroute-circuit"></a>3. erstellen Sie eine ExpressRoute-Verbindung

Haben Sie bereits eine Ressourcengruppe, legen eine Sie vor dem Erstellen der ExpressRoute-Verbindung. Sie können dazu den folgenden Befehl ausführen:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


Im folgenden Beispiel wird veranschaulicht, wie zu 200 MBit / ExpressRoute-Verbindung durch Equinix im Silicon Valley. Wenn Sie einen anderen Anbieter und anderen Einstellungen verwenden, ersetzen Sie diese Informationen bei der Anforderung. Der folgende Code ist ein Beispiel für eine Anforderung für einen neuen Service-Schlüssel:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Stellen Sie sicher, dass die richtigen Tier-SKU und SKU-Familie angeben:

- SKU-Ebene bestimmt, ob ein ExpressRoute Standard oder ExpressRoute Premium Add-on aktiviert ist. Sie können *standardmäßige* standard SKU oder *Premium* für die zusätzliche Prämie erhalten.

- SKU-Familie bestimmt die Rechnungsadresse. Sie können *Metereddata* für einen Datenplan gemessenen und *Unlimiteddata* für einen Datentarif. Beachten Sie, dass Ändern des Rechnungsadressen Typs von *Metereddata* , *Unlimiteddata*, aber Sie können den Typ von *Unlimiteddata* in *Metereddata*ändern.


>[AZURE.IMPORTANT] ExpressRoute-Verbindung wird von dem Moment berechnet ein Dienstschlüssel ausgestellt. Stellen Sie sicher, dass Sie diesen Vorgang ausführen, wenn Connectivity-Anbieter die Verbindung bereitstellen kann.

Die Antwort enthält den Dienst. Ausführliche Beschreibung aller Parameter erhalten ausgeführt werden:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. Liste alle ExpressRoute circuits

Um eine Liste aller ExpressRoute Stromkreise, die Sie erstellt haben, führen Sie die `Get-AzureRmExpressRouteCircuit` Befehl:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Die Antwort wird im folgenden Beispiel aussehen:


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
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Sie können diese Informationen jederzeit abrufen, mit dem `Get-AzureRmExpressRouteCircuit` Cmdlet. Der Aufruf ohne Parameter listet alle Schaltkreise. Der Dienstschlüssel im Feld *ServiceKey* aufgelistet:


    Get-AzureRmExpressRouteCircuit


Die Antwort wird im folgenden Beispiel aussehen:


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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Ausführliche Beschreibung aller Parameter erhalten ausgeführt werden:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Senden des Schlüssels an Ihren konnektivitätsanbieter für für die Bereitstellung

*ServiceProviderProvisioningState* enthält Informationen über den aktuellen Status der Bereitstellung auf Service-Anbieter. Status stellt den Status auf Microsoft. Weitere Informationen zu Bereitstellung Staaten Circuit finden Sie [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Beim Erstellen einer neuen ExpressRoute-Verbindung werden die Leitung in folgenden Status:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Die Leitung wird folgende Status ändern, wenn konnektivitätsanbieter aktivieren Sie derzeit:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Zur ExpressRoute-Verbindung verwenden muss es folgende Status:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. regelmäßig den Status und den Zustand der Taste circuit

Überprüft den Status und den Zustand der Taste Circuit informiert Sie, wenn der Anbieter Ihre Verbindung aktiviert ist. Nach der Verbindung wurde konfiguriert *ServiceProviderProvisioningState* wird *bereitgestellt*, wie im folgenden Beispiel gezeigt:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Die Antwort wird im folgenden Beispiel aussehen:


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

### <a name="7-create-your-routing-configuration"></a>7. erstellen Sie 7. Ihre routing-Konfiguration

Anleitungsschritte finden Sie [ExpressRoute Circuit Routingkonfiguration](expressroute-howto-routing-arm.md) Circuit Peerings zu erstellen.


>[AZURE.IMPORTANT] Diese Schritte gelten nur für Stromkreise mit erstellt werden, die Ebene 2 Connectivity Services anbieten. Verwenden Sie einen Dienstanbieter, der verwaltete bietet Schicht 3 Dienste (normalerweise eine IP VPN, wie MPLS), Dienstanbieter Verbindung konfigurieren und Verwalten von routing für Sie.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. verknüpfen Sie ein virtuelles Netzwerk mit ExpressRoute-Verbindung

Anschließend verknüpfen Sie ein virtuelles Netzwerk mit ExpressRoute-Verbindung. Verwenden Sie beim Arbeiten mit dem Ressourcen-Manager-Bereitstellungsmodell Artikel [Verknüpfen virtuelle Netzwerke an ExpressRoute](expressroute-howto-linkvnet-arm.md) .

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Ermitteln des Status der ExpressRoute-Verbindung

Sie können diese Informationen jederzeit abrufen, mit dem `Get-AzureRmExpressRouteCircuit` Cmdlet. Der Aufruf ohne Parameter listet alle Schaltkreise.

    Get-AzureRmExpressRouteCircuit


Die Antwort wird im folgenden Beispiel ähneln:


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


Informationen auf einer bestimmten ExpressRoute erhalten den Namen und den Circuit Namen als Parameter an den Aufruf übergeben:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Die Antwort wird im folgenden Beispiel aussehen:


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


Ausführliche Beschreibung aller Parameter erhalten ausgeführt werden:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>ExpressRoute-Verbindung ändern

Sie können bestimmte Eigenschaften des ExpressRoute-Verbindung ohne Beeinträchtigung der Konnektivität.

Sie können Folgendes ohne Ausfallzeiten:

- Aktivieren Sie oder deaktivieren Sie die ExpressRoute Premium Add-on für ExpressRoute-Verbindung.
- Erhöhen der Bandbreite von ExpressRoute-Verbindung. Beachten Sie, dass Bandbreite eines Herabstufung nicht unterstützt wird.
- Ändern des Dosierung Plans aus gemessen in Daten. Beachten Sie, dass das Ändern der Messung von Daten gemessen Daten planen wird nicht unterstützt.
-  Aktivieren und deaktivieren Sie *Klassische Vorgänge zulassen*.

Weitere Informationen über Grenzwerte und Einschränkungen finden Sie [FAQ ExpressRoute](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute Premium Add-on aktivieren

ExpressRoute Premium Add-on können für vorhandene Verbindung mithilfe der folgenden PowerShell Ausschnitts:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Die Schaltung haben nun ExpressRoute Premium Add-On-Funktionen aktiviert. Beachten Sie, dass wir beginnen Abrechnung Sie Premium Add-On-Funktion wie der Befehl erfolgreich ausgeführt wurde.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute Premium Add-on deaktivieren

>[AZURE.IMPORTANT] Dieser Vorgang kann fehlschlagen, wenn Sie Ressourcen, die größer verwenden als die erlaubten standard Schaltung.

Beachten Sie Folgendes:

- Bevor downgrade von Premium-Standard müssen Sie sicherstellen, dass die Anzahl der virtuellen Netzwerke, die mit der Schaltkreis kleiner als 10 ist. Sollten nicht die updateanforderung fehlschlägt, und wir werden Sie zu Premium-Preisen Rechnung.

- Sie müssen alle virtuellen Netzwerke in anderen geopolitischen Regionen aufheben. Wenn Sie dies nicht tun, Ihre Aktualisierungsanfrage fehl und werden wir Sie zu Premium-Preisen Rechnung.

- Die Routingtabelle muss weniger als 4.000 Routen für private peering. Größe der Route größer als 4.000 Routen, BGP-Sitzung fällt und wird nicht wieder aktiviert werden, bis die Anzahl der angekündigten Präfixe unter 4.000 geht.

Mit dem folgenden PowerShell-Cmdlet können Sie das ExpressRoute Premium Add-on für vorhandene Verbindung deaktivieren:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute Schaltung Bandbreite aktualisieren

Unterstützte Bandbreitenoptionen für den Anbieter überprüfen Sie [ExpressRoute FAQ](expressroute-faqs.md). Sie können größer als die Größe der vorhandenen Verbindung auswählen.

>[AZURE.IMPORTANT] Sie können die Bandbreite von ExpressRoute-Verbindung ohne Unterbrechung nicht reduzieren. Herabstufung Bandbreite erfordert ExpressRoute-Verbindung Mail- und Basis eine neue ExpressRoute-Verbindung.

Nach Größe müssen entscheiden, verwenden Sie folgenden Befehl Größe der Verbindung:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Die Verbindung wird auf der Seite Microsoft reservierten. Dann müssen Sie Dienstanbieter Konnektivität auf ihrer Seite entsprechend dieser Änderung aktualisieren wenden. Nach dem vornehmen dieser Benachrichtigung beginnen wir Sie die aktualisierte Bandbreite Option Abrechnung.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Zum Verschieben der SKUs aus gemessen, unbegrenzt

Ändern Sie mithilfe der folgenden PowerShell Ausschnitts SKU ExpressRoute-Verbindung:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Zugriff auf die Classic und Ressourcenmanager Umgebung steuern  

Informationen Sie in [ExpressRoute verschieben Stromkreise in der Standardansicht auf der Ressourcen-Manager-Bereitstellungsmodell](expressroute-howto-move-arm.md).  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deaktivieren und Löschen von ExpressRoute-Verbindung

Beachten Sie Folgendes:

- Sie müssen alle virtuellen Netzwerke von ExpressRoute-Verbindung aufheben. Wenn dieser Vorgang fehlschlägt, überprüfen Sie, ob alle virtuellen Netzwerke mit der Verbindung verknüpft sind.

- Die ExpressRoute Circuit Service Provider Bereitstellung **Bereitstellung** oder **bereitgestellt** ist müssen Sie beim Dienstanbieter zu entziehen Circuit auf ihrer Seite arbeiten. Wir werden weiterhin Ressourcen reservieren und Abrechnung bis Dienstanbieter Aufhebung der Verbindung abgeschlossen ist und uns benachrichtigt.

- Wenn der Dienstanbieter die Verbindung hat hat (Service Provider-Bereitstellungsstatus soll **nicht**bereitgestellt) können Sie die Verbindung löschen. Dies beendet die Abrechnung für die Leitung

Sie können die ExpressRoute-Verbindung durch den folgenden Befehl ausführen:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen der Verbindung sicher, dass Sie Folgendes tun:

- [Erstellen und Ändern von routing für ExpressRoute-Verbindung](expressroute-howto-routing-arm.md)
- [Verknüpfen von virtuellen Netzwerk mit ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)
