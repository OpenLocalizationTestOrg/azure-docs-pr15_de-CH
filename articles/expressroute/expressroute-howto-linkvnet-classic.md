<properties
   pageTitle="Verknüpfen ein virtuelles Netzwerks mit ExpressRoute-Verbindung mit dem klassischen Bereitstellungsmodell und PowerShell | Microsoft Azure"
   description="Dieses Dokument Überblick wie virtuelle Netzwerke (VNets) an ExpressRoute mithilfe der klassischen Bereitstellungsmodell und PowerShell."
   services="expressroute"
   documentationCenter="na"
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
   ms.author="ganesr" />

# <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>Verknüpfen Sie ein virtuelles Netzwerk mit ExpressRoute-Verbindung

> [AZURE.SELECTOR]
- [Azure-Portal - Ressourcen-Manager](expressroute-howto-linkvnet-portal-resource-manager.md)
- [PowerShell - Ressourcen-Manager](expressroute-howto-linkvnet-arm.md)
- [PowerShell - klassisch](expressroute-howto-linkvnet-classic.md)



Dieser Artikel hilft Ihnen mit der klassischen Bereitstellungsmodell PowerShell Azure ExpressRoute Stromkreise virtuelle Netzwerke (VNets) verknüpfen. Virtuelle Netzwerke können dieselbe Abonnement oder ein anderes Abonnement angehören.

**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Erforderliche Konfiguration

1. Sie benötigen die neueste Version von Azure PowerShell-Module. Sie können die neuesten PowerShell-Module von PowerShell Teil der [Azure-Downloadseite](https://azure.microsoft.com/downloads/). Führen Sie die Schritte [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) schrittweise Anleitung zum Konfigurieren Ihres Computers Azure PowerShell-Module.
2. Sie müssen vor der Konfiguration der [Komponenten](expressroute-prerequisites.md) [Routinganforderungen](expressroute-routing.md)und [Workflows](expressroute-workflows.md) überprüfen.
3. Sie benötigen eine aktive ExpressRoute-Verbindung.
    - Gehen Sie zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) und die konnektivitätsanbieter die Verbindung aktivieren.
    - Sicherstellen Sie, dass Sie Azure private peering für die Verbindung konfiguriert. Finden Sie Artikel [routing konfigurieren](expressroute-howto-routing-classic.md) Routinganweisungen.
    - Sicherstellen Sie, dass Azure private peering konfiguriert und BGP peering zwischen Ihrem Netzwerk und Microsoft sodass End-to-End-Konnektivität zu aktivieren.
    - Sie müssen ein virtuelles Netzwerk und ein virtuelles Netzwerk-Gateway erstellt und vollständig bereitgestellt. Folgen Sie zum [Konfigurieren eines virtuellen Netzwerks für ExpressRoute](expressroute-howto-vnet-portal-classic.md).

Sie können bis zu 10 virtuelle Netzwerke an einem ExpressRoute verknüpfen. Alle virtuellen Netzwerke muss im gleichen geopolitischen Regionen. Verknüpfen Sie eine größere Anzahl von virtuellen Netzwerken ExpressRoute-Verbindung oder Link virtuelle Netzwerke, die in anderen geopolitischen Regionen ExpressRoute Premium Add-on aktiviert. [FAQ](expressroute-faqs.md) für weitere Details zu Premium Add-on überprüfen.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Ein virtuelles Netzwerk in der gleichen Anmeldung einer Verbindung

Sie können eine ExpressRoute-Verbindung mit dem folgenden Cmdlet ein virtuelles Netzwerk verknüpfen. Stellen Sie sicher, dass virtuelle Netzwerk-Gateway erstellt und verknüpfen, bevor Sie das Cmdlet ausführen kann.

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Ein virtuelles Netzwerk in ein anderes Abonnement einer Verbindung

Sie können mehrere Abonnements ExpressRoute-Verbindung freigeben. Die folgende Abbildung zeigt ein einfaches Schema wie Teilen arbeiten für ExpressRoute Stromkreise über mehrere Abonnements.

Jede kleineren Wolken große Cloud dient Abonnements darstellen, die Abteilungen innerhalb einer Organisation angehören. Jede Abteilung innerhalb des Unternehmens können eigene Abonnement für Bereitstellung ihrer Dienste – aber die Abteilung einen Stromkreis ExpressRoute Verbindung zum lokalen Netzwerk freigeben kann. Eine einzelne Abteilung (in diesem Beispiel: IT) können eigene ExpressRoute-Verbindung. Andere Abonnements innerhalb der Organisation können die ExpressRoute-Verbindung.

>[AZURE.NOTE] Konnektivität und Bandbreite Zuschläge für den Stromkreis werden der Verbindungsbesitzer ExpressRoute angewendet. Alle virtuellen Netzwerke teilen die gleiche Bandbreite.

![Cross-Abonnement-Konnektivität](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Verwaltung

Der *Verbindungsbesitzer* ist der Administrator/Coadministrator des Abonnements in der ExpressRoute-Verbindung erstellt wird. Der Verbindungsbesitzer kann Administratoren/coadministratoren von anderen Abonnements genannte *Verbindungsbenutzer*Verwendung besitzen die Standleitung autorisieren. Circuit-Benutzer des Unternehmens ExpressRoute-Verbindung verwenden können ExpressRoute-Verbindung das virtuelle Netzwerk im Abonnement verknüpfen, nachdem sie autorisiert sind.

Der Verbindungsbesitzer versorgt ändern und Berechtigungen jederzeit widerrufen. Widerrufen einer Genehmigung führt alle Links aus der, deren Zugriff widerrufen wurde, gelöscht.

### <a name="circuit-owner-operations"></a>Circuit Besitzer Vorgänge

#### <a name="creating-an-authorization"></a>Erstellen einer Genehmigung

Der Verbindungsbesitzer autorisiert die Administratoren von anderen Abonnements mit der angegebenen Verbindung. Im folgenden Beispiel kann der Administrator des Stromkreises (Contoso IT) den Administrator ein weiteres Abonnement (Dev-Test) zwei virtuelle Netzwerke mit der Verbindung verknüpft. Contoso IT-Administrator ermöglicht dies durch Microsoft Dev-Test-ID anzugeben. Das Cmdlet wird die angegebene Microsoft-ID e-Mail Der Verbindungsbesitzer muss Ihrem Besitzer explizit zu benachrichtigen, dass die Autorisierung abgeschlossen ist.

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

#### <a name="reviewing-authorizations"></a>Berechtigungen überprüfen

Der Verbindungsbesitzer kann alle Berechtigungen überprüfen, die auf einer bestimmten Strecke mit dem folgenden Cmdlet ausgegeben werden:

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


#### <a name="updating-authorizations"></a>Berechtigungen aktualisieren

Der Verbindungsbesitzer kann mit dem folgenden Cmdlet Berechtigungen ändern:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


#### <a name="deleting-authorizations"></a>Löschen von Berechtigungen

Der Verbindungsbesitzer kann widerrufen/Berechtigungen für den Benutzer löschen durch das folgende Cmdlet ausführen:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a>Circuit-Benutzeraktionen

#### <a name="reviewing-authorizations"></a>Berechtigungen überprüfen

Circuit-Benutzer kann mit dem folgenden Cmdlet Berechtigungen überprüfen:

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

#### <a name="redeeming-link-authorizations"></a>Hyperlink-Bewilligung einlösen

Circuit-Benutzer kann das folgende Cmdlet Link Autorisierung einlösen ausführen:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu ExpressRoute finden Sie im [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md).
