<properties
   pageTitle="ExpressRoute Stromkreise von Classic Resource Manager verschieben | Microsoft Azure"
   description="Diese Seite beschreibt einen klassischen Circuit Bereitstellungsmodell Ressourcenmanager verschieben."
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


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Verschieben Sie ExpressRoute Stromkreise in der Standardansicht in Ressourcen-Manager-Bereitstellungsmodell

## <a name="configuration-prerequisites"></a>Erforderliche Konfiguration

- Sie benötigen die neueste Version von Azure PowerShell-Module (mindestens Version 1.0).
- Stellen Sie sicher, dass Sie [Komponenten](expressroute-prerequisites.md), [Arbeitsplan Vorschriften](expressroute-routing.md)und [Workflows](expressroute-workflows.md) überprüft haben, vor der Konfiguration.
- Überprüfen Sie vor vor weiteren Informationen unter [ExpressRoute-Verbindung von klassischen Resource Manager verschieben](expressroute-move.md). Stellen Sie sicher, dass Sie die Grenzen und Grenzen des vollständig verstanden haben.
- Wenn Sie eine Azure ExpressRoute-Verbindung von klassischen Bereitstellungsmodell in Azure-Ressourcen-Manager-Bereitstellungsmodell verschieben möchten, müssen Sie Verbindung vollständig konfiguriert und im klassischen Bereitstellungsmodell.
- Sicherstellen Sie, dass Sie eine Ressourcengruppe, die der Ressourcen-Manager-Bereitstellungsmodell erstellt wurde.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Verschieben Sie ExpressRoute-Verbindung in der Ressourcen-Manager-Bereitstellungsmodell

Damit Sie das klassische und Bereitstellungsmodelle Ressourcen-Manager verwenden zu können, müssen Sie eine ExpressRoute-Verbindung zum Ressourcen-Manager-Bereitstellungsmodell verschieben. Sie können hierzu die folgenden PowerShell Befehle ausführen.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Schritt 1: Sammeln von klassischen Bereitstellungsmodell Circuit details

Sie müssen zunächst Informationen über ExpressRoute-Verbindung zu sammeln.

Melden Sie sich bei Azure classic Umgebung und sammeln des Schlüssels. Der folgende Codeausschnitt PowerShell können Sie die Informationen:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Kopieren Sie den **Schlüssel** der Verbindung, die Sie in den Ressourcen-Manager-Bereitstellungsmodell übernehmen möchten.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Schritt 2: Ressourcen-Manager-Umgebung anmelden und eine neue Ressourcengruppe erstellen

Im folgenden Codeausschnitt verwenden können Sie eine neue Ressourcengruppe erstellen:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Sie können auch eine vorhandene Ressourcengruppe, haben Sie bereits eine.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Schritt 3: ExpressRoute-Verbindung in der Ressourcen-Manager-Bereitstellungsmodell verschieben

Sie können nun über ExpressRoute-Verbindung in der Standardansicht in der Ressourcen-Manager-Bereitstellungsmodell verschieben. Überprüfen Sie den Angaben unter [ExpressRoute-Verbindung in der Standardansicht auf der Ressourcen-Manager-Bereitstellungsmodell verschieben](expressroute-move.md) , bevor Sie fortfahren.

Hierzu mit den folgenden Codeausschnitt:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Nach Abschluss die Verschiebung wird der neue Name in der vorherigen Cmdlet aufgeführt an die Ressource verwendet. Die Verbindung wird im Wesentlichen umbenannt.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>ExpressRoute-Verbindung für beide Bereitstellungsmodelle aktivieren

Sie müssen ExpressRoute-Verbindung zum Ressourcen-Manager-Bereitstellungsmodell vor Steuern des Zugriffs auf das Bereitstellungsmodell verschieben.

Führen Sie das folgende Cmdlet auf beiden aktivieren:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Nachdem dieser Vorgang erfolgreich ausgeführt wurde, werden Sie die Leitung in das klassische Bereitstellungsmodell anzeigen.

Führen Sie Folgendes ein, um die Details des ExpressRoute-Verbindung:

    get-azurededicatedcircuit

Sie müssen der Service Schlüssel aufgeführt sein. Sie können nun Links zu mit Ihrem Modell Befehle standard klassische Bereitstellung für klassische VNets die Standardbefehle ARM für ARM-VNETs ExpressRoute-Verbindung verwalten. Die folgenden Artikel gehen Sie über Links auf ExpressRoute-Verbindung verwalten:

- [Verknüpfen Sie virtuelle Netzwerk mit ExpressRoute-Verbindung in der Ressourcen-Manager-Bereitstellungsmodell](expressroute-howto-linkvnet-arm.md)
- [Verknüpfen Sie virtuelle Netzwerk mit ExpressRoute-Verbindung im klassischen Bereitstellungsmodell](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>Deaktivieren Sie ExpressRoute-Verbindung zum klassischen Bereitstellungsmodell

Führen Sie das folgende Cmdlet auf dem klassischen Bereitstellungsmodell deaktivieren:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Nachdem dieser Vorgang erfolgreich ausgeführt wurde, können nicht an der Rennstrecke im klassischen Bereitstellungsmodell Sie.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen der Verbindung sicher, dass Sie Folgendes tun:

- [Erstellen und Ändern von routing für ExpressRoute-Verbindung](expressroute-howto-routing-arm.md)
- [Verknüpfen von virtuellen Netzwerk mit ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)
