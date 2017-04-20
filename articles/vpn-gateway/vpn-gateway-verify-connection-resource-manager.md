<properties
   pageTitle="Ein Gateway Verbindung | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie eine Gateway-Verbindung in der Ressourcen-Manager-Bereitstellungsmodell überprüfen"
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
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Überprüfen Sie eine gatewayverbindung

Die gatewayverbindung können Sie unterschiedliche Weise überprüfen. In diesem Artikel wird den Status einer gatewayverbindung Ressourcenmanager Azure-Portal und mithilfe von PowerShell überprüfen angezeigt.


## <a name="verify-using-powershell"></a>Überprüfen Sie mithilfe von PowerShell

Sie müssen die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets installieren. Informationen zum Installieren von PowerShell-Cmdlets [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)anzeigen Weitere Informationen über Ressourcen-Manager-Cmdlets finden Sie unter [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Schritt 1: Azure-Konto anmelden

1. Öffnen Sie die PowerShell-Konsole mit erhöhten und Ihr Konto verbinden.

        Login-AzureRmAccount

2. Überprüfen Sie die Abonnements für das Konto.

        Get-AzureRmSubscription 

3. Geben Sie das Abonnement, das Sie verwenden möchten.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Schritt 2: Überprüfen der Verbindungs


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Prüfen Sie mit Azure-portal

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Nächste Schritte

- Sie können virtuelle Maschinen virtuelle Netzwerke hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

