<properties
   pageTitle="VNet-Gateway für ExpressRoute mithilfe von PowerShell konfigurieren | Microsoft Azure"
   description="VNet-Gateway für eine klassische Bereitstellung konfigurieren Modell VNet mithilfe von PowerShell für eine ExpressRoute-Konfiguration."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-classic-deployment-model-and-powershell"></a>Konfigurieren Sie ein virtuelles Netzwerk-Gateway für ExpressRoute mit der klassischen Bereitstellungsmodell PowerShell

> [AZURE.SELECTOR]
- [PowerShell - Ressourcen-Manager](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell - klassisch](expressroute-howto-add-gateway-classic.md)

Dieser Artikel führt Sie durch die Schritte zum Hinzufügen, ändern und entfernen Gateway virtuelles Netzwerk (VNet) für eine vorhandene VNet. Die Schritte werden werden für diese Konfiguration für VNets erstellten verwenden **klassische Bereitstellungsmodell** verwendet in einer ExpressRoute. 

**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Vor Beginn

Überprüfen der Installation der Azure-PowerShell-Cmdlets für diese Konfiguration benötigt (1.0.2 oder höher). Wenn Sie die Cmdlets installiert haben, müssen Sie vor der Konfigurationsschritte dazu. Weitere Informationen zum Installieren von Azure PowerShell Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

    
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie VNet-Gateway erstellt haben, können Sie das VNet ExpressRoute-Verbindung verknüpfen. [Verknüpfen eines virtuellen Netzwerks auf ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)anzeigen
