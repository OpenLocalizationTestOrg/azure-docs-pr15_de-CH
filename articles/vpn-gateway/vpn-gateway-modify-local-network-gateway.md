<properties
   pageTitle="LAN Gateway IP-Adresspräfixe und Gateway-IP-ändern | Microsoft Azure"
   description="Dieser Artikel führt Sie durch Ändern der IP-Adresspräfixe für Ihr lokales Netzwerkgateway"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>Ändern Sie LAN Gateway mit PowerShell

Manchmal Einstellungen für Ihr LAN-Gateway, AddressPrefix oder GatewayIPAddress. Hinweise helfen Ihnen die lokalen Netzwerk-Gateway ändern. Sie können auch diese Einstellungen im Azure-Portal.

## <a name="before-you-begin"></a>Bevor Sie beginnen
    
Sie müssen die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.

## <a name="to-modify-ip-address-prefixes"></a>IP-Adresspräfixe ändern

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Die Gateway-IP-Adresse ändern

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können die gatewayverbindung überprüfen. Finden Sie unter [Überprüfen einer gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).

