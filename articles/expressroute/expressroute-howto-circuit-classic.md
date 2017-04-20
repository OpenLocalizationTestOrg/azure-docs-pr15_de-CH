<properties
   pageTitle="Erstellen und Ändern von ExpressRoute-Verbindung mit dem klassischen Bereitstellungsmodell und PowerShell | Microsoft Azure"
   description="Dieser Artikel führt Sie durch die Schritte zum Erstellen und Bereitstellen einer ExpressRoute-Verbindung. Dieser Artikel beschreibt auch wie Status, Update oder Delete und Schaltung entziehen."
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
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Erstellen und Ändern von ExpressRoute-Verbindung

> [AZURE.SELECTOR]
[Azure-Portal - Ressourcen-Manager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Ressourcen-Manager](expressroute-howto-circuit-arm.md)
[PowerShell - Klassisch](expressroute-howto-circuit-classic.md)

Dieser Artikel führt Sie durch die Schritte zum Erstellen einer Azure ExpressRoute-Verbindung mithilfe von PowerShell-Cmdlets und das klassische Bereitstellungsmodell. In diesem Artikel wird auch zeigen, wie Status, Update oder Delete und Entziehen von ExpressRoute-Verbindung.

**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Bevor Sie beginnen

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1 Überprüfen Sie 1 die erforderlichen Komponenten und Workflow-Artikel

Stellen Sie sicher, dass Sie die [erforderlichen Komponenten](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md) überprüft haben, vor der Konfiguration.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2. installieren Sie die neuesten Versionen der Azure PowerShell-Module 

Führen Sie die Schritte [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) schrittweise Anleitung zum Konfigurieren Ihres Computers Azure PowerShell-Module.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. Melden Sie sich bei Ihrem Azure-Konto und wählen Sie ein Abonnement

1. Führen Sie das folgende Cmdlet mit einem erweiterten Windows PowerShell Prompt:

        Add-AzureAccount
2. Anmelden Bildschirm, der angezeigt wird, melden Sie sich bei Ihrem Konto an.

3. Enthält eine Liste der Abonnements.

        Get-AzureSubscription
4. Wählen Sie das Abonnement, das Sie verwenden möchten.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. importieren Sie die PowerShell-Module für ExpressRoute

 Wenn nicht bereits geschehen, müssen Sie die Module Azure und ExpressRoute in der PowerShell-Sitzung Verwendung ExpressRoute-Cmdlets importieren. Importieren Sie die Module aus, die sie auf dem lokalen Computer installiert wurden. Je nach verwendeten Module installieren möglicherweise der Speicherort das folgende Beispiel zeigt unterscheidet. Ändern Sie das Beispiel bei Bedarf.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Holen Sie die Liste der unterstützten Provider, Standorte und Bandbreiten

Bevor Sie eine ExpressRoute-Verbindung erstellen, benötigen Sie die Liste der konnektivitätsanbieter unterstützt, Speicherorten und Bandbreitenoptionen.

Das PowerShell-Cmdlet `Get-AzureDedicatedCircuitServiceProvider` gibt diese Informationen später verwenden:

    Get-AzureDedicatedCircuitServiceProvider

Überprüfen der konnektivitätsanbieter aufgeführt ist. Notieren Sie sich die folgenden Informationen, da Sie es später beim Erstellen einer Verbindung benötigen:

- Name

- PeeringLocations

- BandwidthsOffered

Sie nun können eine ExpressRoute-Verbindung erstellen.         

### <a name="3-create-an-expressroute-circuit"></a>3. erstellen Sie eine ExpressRoute-Verbindung

Im folgenden Beispiel wird veranschaulicht, wie zu 200 MBit / ExpressRoute-Verbindung durch Equinix im Silicon Valley. Wenn Sie einen anderen Anbieter und anderen Einstellungen verwenden, ersetzen Sie diese Informationen bei der Anforderung.

>[AZURE.IMPORTANT] ExpressRoute-Verbindung wird von dem Moment berechnet ein Dienstschlüssel ausgestellt. Stellen Sie sicher, dass Sie diesen Vorgang ausführen, wenn Connectivity-Anbieter die Verbindung bereitstellen kann.


Der folgende Code ist ein Beispiel für eine Anforderung für einen neuen Service-Schlüssel:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Oder eine ExpressRoute-Verbindung mit dem Premium erstellen, verwenden Sie das folgende Beispiel. [ExpressRoute FAQ](expressroute-faqs.md) für Weitere Informationen über das Premium Add-on finden.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Die Antwort wird den Schlüssel enthalten. Ausführliche Beschreibung aller Parameter erhalten ausgeführt werden:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. der ExpressRoute Circuits auflisten

Führen Sie die `Get-AzureDedicatedCircuit` Befehl, um eine Liste aller ExpressRoute Stromkreise, die Sie erstellt haben:


    Get-AzureDedicatedCircuit

Die Antwort ist ähnlich dem folgenden Beispiel:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Sie können diese Informationen jederzeit abrufen, mit dem `Get-AzureDedicatedCircuit` Cmdlet. Der Aufruf ohne Parameter listet alle Schaltkreise. Der Dienstschlüssel wird im Feld *ServiceKey* aufgeführt.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Ausführliche Beschreibung aller Parameter erhalten ausgeführt werden:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Senden des Schlüssels an Ihren konnektivitätsanbieter für für die Bereitstellung


*ServiceProviderProvisioningState* Informationen über den aktuellen Status der Bereitstellung auf Service-Anbieter. *Status* stellt den Status auf Microsoft. Weitere Informationen zu Bereitstellung Staaten Circuit finden Sie [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Beim Erstellen einer neuen ExpressRoute-Verbindung werden die Leitung in folgenden Status:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Die Leitung gehen folgende Status Wenn konnektivitätsanbieter gerade für Sie aktivieren:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

ExpressRoute-Verbindung muss der folgende Zustand zu verwenden:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. regelmäßig den Status und den Zustand der Taste circuit

Dies informiert Sie, wenn der Anbieter Ihre Verbindung aktiviert ist. Nach der Konfiguration der Verbindung zeigt *ServiceProviderProvisioningState* als *bereitgestellt* , wie im folgenden Beispiel gezeigt:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. erstellen Sie 7. Ihre routing-Konfiguration

Finden Sie die [ExpressRoute Schaltkreis Routingkonfiguration (erstellen und Ändern von Circuit Peerings)](expressroute-howto-routing-classic.md) Artikel schrittweise Anleitung.

>[AZURE.IMPORTANT] Diese Schritte gelten nur für Stromkreise mit erstellt werden, die Ebene 2 Connectivity Services anbieten. Verwenden Sie einen Dienstanbieter, der verwaltete bietet Schicht 3 Dienste (normalerweise eine IP VPN, wie MPLS), Dienstanbieter Verbindung konfigurieren und Verwalten von routing für Sie.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. verknüpfen Sie ein virtuelles Netzwerk mit ExpressRoute-Verbindung

Anschließend verknüpfen Sie ein virtuelles Netzwerk mit ExpressRoute-Verbindung. Weitere [Schaltkreise mit virtuellen Netzwerken verknüpfen ExpressRoute](expressroute-howto-linkvnet-classic.md) Anleitung. Benötigen Sie ein virtuelles Netzwerk mit klassischen Bereitstellungsmodell für ExpressRoute erstellen, finden Sie unter [Erstellen eines virtuellen Netzwerks für ExpressRoute](expressroute-howto-vnet-portal-classic.md) Anleitung.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Ermitteln des Status der ExpressRoute-Verbindung

Sie können diese Informationen jederzeit abrufen, mit dem `Get-AzureCircuit` Cmdlet. Der Aufruf ohne Parameter listet alle Schaltkreise.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Auf einer bestimmten ExpressRoute informieren den Schlüssel als Parameter an den Aufruf übergeben:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Ausführliche Beschreibung aller Parameter erhalten ausgeführt werden:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>ExpressRoute-Verbindung ändern

Sie können bestimmte Eigenschaften des ExpressRoute-Verbindung ohne Beeinträchtigung der Konnektivität.

Sie können Folgendes ohne Ausfallzeiten:

- Aktivieren Sie oder deaktivieren Sie die ExpressRoute Premium Add-on für ExpressRoute-Verbindung.
- Erhöhen der Bandbreite von ExpressRoute-Verbindung. Beachten Sie, dass Bandbreite eines Herabstufung nicht unterstützt wird.
- Ändern des Dosierung Plans aus gemessen in Daten. Beachten Sie, dass das Ändern der Messung von Daten gemessen Daten planen wird nicht unterstützt.
- Aktivieren und deaktivieren Sie *Klassische Vorgänge zulassen*.

Lesen Sie [ExpressRoute FAQ](expressroute-faqs.md) Weitere Grenzen und Grenzen.

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute Premium Add-on aktivieren

ExpressRoute Premium Add-on können für vorhandene Verbindung mit dem folgenden PowerShell-Cmdlet:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Die Schaltung haben nun ExpressRoute Premium Add-On-Funktionen aktiviert. Beachten Sie, dass wir beginnen Abrechnung Sie Premium Add-On-Funktion wie der Befehl erfolgreich ausgeführt wurde.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute Premium Add-on deaktivieren

>[AZURE.IMPORTANT] Dieser Vorgang kann fehlschlagen, wenn Sie Ressourcen, die größer verwenden als die erlaubten standard Schaltung.

Beachten Sie Folgendes:

- Sie müssen sicherstellen, dass die Anzahl von virtuellen Netzwerken an die weniger als 10 vor dem downgrade von Premium-Standard. Wenn Sie nicht die updateanforderung fehl schlägt, und Sie die Tarife in Rechnung gestellt.

- Sie müssen alle virtuellen Netzwerke in anderen geopolitischen Regionen aufheben. Wenn Sie nicht die updateanforderung fehl schlägt, und Sie die Tarife in Rechnung gestellt.

- Die Routingtabelle muss weniger als 4.000 Routen für private peering. Ist Ihre Route Größe größer als 4.000 Routen wird BGP-Sitzung und wird nicht wieder aktiviert werden, bis die Anzahl der angekündigten Präfixe unter 4.000 geht.

Mit dem folgenden PowerShell-Cmdlet können Sie das ExpressRoute Premium Add-on für vorhandene Verbindung deaktivieren:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute Schaltung Bandbreite aktualisieren

Überprüfen Sie [ExpressRoute FAQ](expressroute-faqs.md) für unterstützte Bandbreitenoptionen für den Anbieter. Sie können jeder Größe auswählen, die größer als die Größe der vorhandenen Verbindung als physische Anschluss (mit der Schaltkreis erstellt wird) ermöglicht.

>[AZURE.IMPORTANT] Sie können die Bandbreite von ExpressRoute-Verbindung ohne Unterbrechung nicht reduzieren. Herabstufung Bandbreite müssen Sie ExpressRoute-Verbindung Mail- und Basis eine neue ExpressRoute-Verbindung.

Nach Größe müssen entscheiden, können Sie den folgenden Befehl, Größe der Verbindung:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Die Verbindung wird auf Microsoft Größe wurde haben einrichten. Sie müssen die Konnektivität auf ihrer Seite entsprechend dieser Änderung aktualisieren wenden. Beachten Sie, dass wir beginnen Abrechnung Sie für die Option aktuelle Bandbreite von diesem Punkt.

Beim Circuit Bandbreite erhöhen die Fehlermeldung angezeigt wird, bedeutet es keine ausreichender Bandbreite am physischen Port bleibt in der vorhandenen Verbindung erstellt wird. Sie müssen diese Verbindung löschen und eine neue Verbindung der gewünschten Größe zu erstellen. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deaktivieren und Löschen von ExpressRoute-Verbindung

Beachten Sie Folgendes:

- Sie müssen alle virtuellen Netzwerke für diesen Vorgang aus der ExpressRoute aufheben. Überprüfen Sie bei jeder virtuelle Netzwerke, die mit der Verbindung verknüpft sind, schlägt dieser Vorgang.

- Die ExpressRoute Circuit Service Provider Bereitstellung **Bereitstellung** oder **bereitgestellt** ist müssen Sie beim Dienstanbieter zu entziehen Circuit auf ihrer Seite arbeiten. Wir werden weiterhin Ressourcen reservieren und Abrechnung bis Dienstanbieter Aufhebung der Verbindung abgeschlossen ist und uns benachrichtigt.

- Wenn der Dienstanbieter die Verbindung hat hat (Service Provider-Bereitstellungsstatus soll **nicht**bereitgestellt) können Sie die Verbindung löschen. Dies beendet die Abrechnung für die Verbindung.

Sie können die ExpressRoute-Verbindung durch den folgenden Befehl ausführen:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen der Verbindung sicher, dass Sie Folgendes tun:

- [Erstellen und Ändern von routing für ExpressRoute-Verbindung](expressroute-howto-routing-classic.md)
- [Verknüpfen von virtuellen Netzwerk mit ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)
