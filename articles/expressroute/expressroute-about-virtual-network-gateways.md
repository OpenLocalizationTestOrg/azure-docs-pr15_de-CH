<properties 
   pageTitle="Über virtuelle Netzwerkgateways ExpressRoute | Microsoft Azure"
   description="ExpressRoute erfahren Sie mehr über virtuelle Netzwerkgateways."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Über virtuelle Netzwerkgateways für ExpressRoute


Ein virtuelles Netzwerk-Gateway verwendet, um den Netzwerkverkehr zwischen Azure virtuelle Netzwerke senden und lokalen Speicherorten. Wenn Sie eine ExpressRoute-Verbindung konfigurieren, müssen Sie erstellen und konfigurieren ein virtuelles Netzwerk-Gateway und eine Gateway-Verbindung.

Wenn Sie ein virtuelles Netzwerk-Gateway erstellen, geben Sie mehrere Einstellungen. Eine erforderliche Einstellung gibt an, ob das Gateway für ExpressRoute oder Standort-zu-Standort-VPN-Datenverkehr verwendet werden. Bereitstellungsmodell Ressourcen-Manager wird die Einstellung '-GatewayType'.

Netzwerkverkehr über eine dedizierte private Verbindung gesendet wird, verwenden Sie den Gatewaytyp 'ExpressRoute'. Dies wird auch als ExpressRoute-Gateway bezeichnet. Netzwerkverkehr über das Internet verschlüsselt ist, verwenden Sie den Gatewaytyp "Vpn". Dies wird als ein VPN-Gateway bezeichnet. Standort zu Standort Punkt-zu-Standort- und VNet VNet Anschlüsse alle verwenden eine VPN-Gateway. 

Jedes virtuelles Netzwerk haben nur ein virtuelles Netzwerk-Gateway pro Gateway. Sie können z. B. ein virtuelles Netzwerk-Gateway, das GatewayType - Vpn verwendet und, GatewayType - ExpressRoute verwendet. Dieser Artikel konzentriert sich auf ExpressRoute virtuellen Netzwerk-Gateway.

## <a name="gwsku"></a>Gateway-SKUs

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Wenn Ihr Gateway ein leistungsfähiger Gateway SKU aktualisieren möchten, können Sie in den meisten Fällen "Größe AzureRmVirtualNetworkGateway" PowerShell-Cmdlet verwenden. Dies funktioniert für Upgrades auf Standard und leistungsstarker SKUs. Jedoch können die SKU UltraPerformance müssen Sie das Gateway neu erstellen.

###  <a name="aggthroughput"></a>Geschätzte Gesamtdurchsatz Gateway SKU


Die folgende Tabelle zeigt die Gateways und geschätzte aggregierte Durchsatz. Diese Tabelle gilt für den Ressourcen-Manager und klassischen Bereitstellungsmodelle.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>REST-APIs und PowerShell-cmdlets

Für zusätzliche technische Ressourcen und spezifische Syntax bei Verwendung von REST-APIs und PowerShell-Cmdlets für virtuelles Netzwerk-Gateway-Konfigurationen Siehe:

|**Classic** | **Ressourcen-Manager**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST-API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST-API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über verfügbare verbindungskonfigurationen finden Sie unter [Überblick ExpressRoute](expressroute-introduction.md) . 







 
