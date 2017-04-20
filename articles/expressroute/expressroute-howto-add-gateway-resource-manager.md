<properties
   pageTitle="VNet-Gateway ein Ressourcenmanager mit PowerShell ExpressRoute hinzufügen | Microsoft Azure"
   description="Dieser Artikel führt Sie durch ein Vnet-Gateway zu einer bereits erstellten Ressourcenmanager VNet für ExpressRoute hinzufügen"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
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
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Konfigurieren Sie ein virtuelles Netzwerk-Gateway für Ressourcenmanager mit PowerShell ExpressRoute


> [AZURE.SELECTOR]
- [PowerShell - Ressourcen-Manager](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell - klassisch](expressroute-howto-add-gateway-classic.md)


Dieser Artikel führt Sie durch die Schritte zum Hinzufügen, ändern und entfernen Gateway virtuelles Netzwerk (VNet) für eine vorhandene VNet. Die Schritte werden werden für diese Konfiguration für VNets erstellten verwenden **Ressourcenmanager Bereitstellungsmodell** verwendet in einer ExpressRoute. 

**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Vor Beginn

Überprüfen der Installation der Azure-PowerShell-Cmdlets für diese Konfiguration benötigt (1.0.2 oder höher). Wenn Sie die Cmdlets installiert haben, müssen Sie vor der Konfigurationsschritte dazu. Weitere Informationen zum Installieren von Azure PowerShell Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie VNet-Gateway erstellt haben, können Sie das VNet ExpressRoute-Verbindung verknüpfen. [Verknüpfen eines virtuellen Netzwerks auf ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)anzeigen
