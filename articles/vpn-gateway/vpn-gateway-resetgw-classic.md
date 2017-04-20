<properties
   pageTitle="Zurücksetzen einer Azure VPN-Gateway | Microsoft Azure"
   description="Dieser Artikel führt Sie durch das Azure VPN-Gateway zurücksetzen. Der Artikel gilt für VPN-Gateways in der Standardansicht und Ressourcenmanager Bereitstellungsmodelle."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Zurücksetzen Sie ein Azure VPN-Gateway mit PowerShell


Dieser Artikel führt Sie durch das PowerShell-Cmdlets mit Azure VPN-Gateway zurücksetzen. Dabei das klassische Bereitstellungsmodell und Ressourcenmanager Bereitstellungsmodell.

Zurücksetzen von Azure VPN-Gateway ist hilfreich, wenn standortübergreifende VPN-Verbindung auf ein oder mehrere S2S VPN-Tunnel verlieren. In diesem Fall Ihre lokalen VPN-Geräte arbeiten ordnungsgemäß, jedoch nicht IPSec-Tunnel mit Azure VPN-Gateways herstellen. 

Jede Azure VPN-Gateway besteht aus zwei VM-Instanzen in eine aktiv-Standby-Konfiguration. Wenn Sie das Gateway zurücksetzen das PowerShell-Cmdlet verwenden, startet das Gateway und wendet die standortübergreifende Konfigurationen zu. Das Gateway hält die öffentliche IP-Adresse bereits. Dies bedeutet, dass Sie die VPN-Routerkonfiguration neue öffentliche IP-Adresse für Azure VPN-Gateway aktualisieren müssen.  

Sobald der Befehl ausgeführt wird, wird die aktuelle aktive Instanz von Azure VPN-Gateway sofort neu gestartet. Werden eine kurze Lücke während des Failovers aktive Instanz (gerade neu gestartet), Standby-Instanz. Der Abstand sollte weniger als eine Minute.

Wenn die Verbindung nicht nach dem ersten Neustart wiederhergestellt wird, den Befehl dieselbe erneut, um die zweite Instanz VM (dem neuen aktiven Gateway) neu starten. Wenn zwei Neustarts, angefordert werden, werden etwas länger, beide Instanzen VM (aktive und standby) neu gestartet werden. Dies verursacht eine Lücke mehr VPN-Konnektivität bis 2 bis 4 Minuten VMs der Neustart abgeschlossen.

Nach zwei Neustarts weiterhin standortübergreifende Konnektivitätsprobleme Öffnen einer Support-Anforderung von Azure-Portal.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Bevor Sie Ihr Gateway zurücksetzen, überprüfen Sie die Schlüsselelemente unten für jeden IPsec-Standorten (S2S) VPN-Tunnel. Jede Abweichung Elemente führt die Trennung der Verbindung S2S VPN-Tunnel. Überprüfen und korrigieren die Konfigurationen für Ihren lokalen und Azure VPN-Gateways müssen Sie unnötige Neustarts und Störungen für die anderen arbeiten Verbindungen Gateways.

Überprüfen Sie Folgendes, bevor Sie Ihr Gateway zurücksetzen:

- IP-Adressen (VIPs) für Azure VPN-Gateway und lokalen VPN-Gateways werden in Azure und lokalen VPN-Richtlinien ordnungsgemäß konfiguriert.
- Der vorinstallierte Schlüssel muss auf Azure und lokalen VPN-Gateways.
- Wenn IPsec-IKE Konfiguration anwenden wie Verschlüsselung, hashing-Algorithmen und PFS (Perfect Forward Secrecy), sorgen Sie die Azure und lokalen VPN-Gateways dieselben Konfigurationen.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Zurücksetzen Sie ein VPN-Gateway mit dem Bereitstellungsmodell Ressourcenmanagement

Ressourcenmanager PowerShell-Cmdlet zum Gateway zurücksetzen ist `Reset-AzureRmVirtualNetworkGateway`. Das folgende Beispiel setzt das Azure VPN-Gateway "VNet1GW" Ressourcengruppe "TestRG1".

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Zurücksetzen Sie ein VPN-Gateway mit dem klassischen Bereitstellungsmodell

Das PowerShell-Cmdlet zum Zurücksetzen von Azure VPN-Gateway ist `Reset-AzureVNetGateway`. Das folgende Beispiel setzt Azure VPN-Gateway für das virtuelle Netzwerk namens "ContosoVNet".
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Ergebnis:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Nächste Schritte
    
[Servicemanagement PowerShell-Cmdlet Verweis](https://msdn.microsoft.com/library/azure/mt617104.aspx) und [Ressourcenmanager PowerShell-Cmdlet Verweis](http://go.microsoft.com/fwlink/?LinkId=828732) Informationen anzeigen






