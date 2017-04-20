<properties
   pageTitle="Mehrere VIPs für einen Clouddienst"
   description="Übersicht über MultiVIP und wie mehrere VIPs auf einen Cloud-Dienst"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Mehrere VIPs für einen Clouddienst konfigurieren

Azure-Cloud-Dienste möglich über das Internet mithilfe der IP-Adresse von Azure. Diese öffentliche IP-Adresse wird als VIP (virtual IP) mit Azure Lastenausgleich verknüpft ist und Instanzen der virtuellen Computer (VM) im Cloud-Dienst. Sie können jede VM-Instanz in einem Clouddienst mit einer VIP zugreifen.

Es gibt jedoch Szenarien, in denen möglicherweise mehrere VIP als Eintrag auf denselben Clouddienst. Cloud-Dienst kann beispielsweise mehrere Websites hosten, die SSL-Verbindung mit den Standardport 443 wie jedem Standort für einen anderen Debitor befindet, oder Mieter. In diesem Szenario müssen Sie eine andere öffentliche nach IP-Adresse für jede Website. Das folgende Diagramm zeigt normalerweise mehrinstanzenfähige Webhosting mit mehrere Zertifikate auf dem gleichen öffentlichen Port.

![Szenario mit mehreren VIP SSL](./media/load-balancer-multivip/Figure1.png)

Im obigen Beispiel alle VIPs verwenden denselben öffentlichen Port (443) Datenverkehr zu umgeleitet, oder weitere Lastenausgleich VMs auf einer privaten eindeutige interne IP-Adresse des Cloud-Dienst alle Websites hosten.

>[AZURE.NOTE] Eine andere Situation die Verwendung mehrere VIPs hostet mehrere SQL AlwaysOn Availability Group Listener auf virtuellen Maschinen.

VIPs sind dynamisch standardmäßig bedeutet, dass die tatsächliche IP-Adresse zum Cloud-Dienst mit der Zeit ändern kann. Um das zu verhindern, können Sie eine VIP-Adresse für Ihren Dienst reservieren. Weitere Informationen zu reservierten VIPs finden Sie unter [Reservierte öffentliche IP-Adresse](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Informationen zu Preisen für VIPs reservierte IPs [Preise IP-Adresse](https://azure.microsoft.com/pricing/details/ip-addresses/) finden.

Sie können mithilfe von PowerShell von Cloud-Diensten verwendeten VIPs überprüfen sowie hinzufügen und entfernen VIPs, VIP zu einem Endpunkt zuordnen und Lastenausgleich auf einer bestimmten VIP konfigurieren.

## <a name="limitations"></a>Grenzen

Zu diesem Zeitpunkt ist Multi-VIP-Funktionen die folgenden Szenarien auf:

- **Nur IaaS**. Sie können nur mehrere VIP für Cloud-Services, die VMs enthalten. Sie können nicht mehrere VIP PaaS-Szenarios mit Instanzen verwenden.
- **PowerShell nur**. Sie können mehrere VIP nur mithilfe von PowerShell verwalten.

Diese Einschränkungen sind temporär und können jederzeit ändern. Vergewissern Sie sich diese Seite überprüfen Veränderungen erneut.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Hinzufügen eine VIP zum Cloud-Dienst

Um ein VIP zu Ihrem Dienst hinzuzufügen, führen Sie den folgenden PowerShell-Befehl:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

Dieser Befehl zeigt ein Ergebnis ähnlich dem folgenden Beispiel:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Entfernen eine VIP aus einem Clouddienst

Führen Sie zum Entfernen der Dienst im obigen Beispiel hinzugefügten VIP folgenden PowerShell-Befehl:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] Sie können nur eine VIP entfernen hat keine Endpunkte zugeordnet.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>VIP-Informationen von einem Cloud-Dienst abrufen

Rufen Sie einem Clouddienst zugeordneten VIPs führen Sie das folgende PowerShell-Skript:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Das Skript zeigt ein Ergebnis ähnlich dem folgenden Beispiel:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

In diesem Beispiel hat der Cloud-Dienst 3 VIPs:

- **Vip1** ist die Standard-VIP, Sie kennen, da der Wert für IsDnsProgrammedName auf True.
- **Vip2** und **Vip3** werden nicht verwendet, da sie keine IP-Adressen. Sie werden nur verwendet, wenn Sie einen Endpunkt an die VIP-Adresse zuordnen.

>[AZURE.NOTE] Ihr Abonnement nur für zusätzliche VIPs berechnet nach einem Endpunkt zugeordnet sind. Weitere Informationen zu Preisen finden Sie unter [IP-Adresse Preise](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>VIP zu einem Endpunkt zuordnen

Um VIP auf einen Cloud-Dienst zu einem Endpunkt zuzuordnen, führen Sie folgenden PowerShell-Befehl:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

Der Befehl erstellt einen Endpunkt aufgerufen *Vip2* auf Port *80*und damit die VM *myVM1* im Cloud-Dienst namens *"MyService"* *TCP* über Port *8080*namens Links VIP verknüpft.

Führen Sie zum Überprüfen der Konfiguration des folgenden PowerShell-Befehls:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Die Ausgabe ähnelt dem folgenden Beispiel:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Zum Aktivieren von Lastenausgleich auf einer bestimmten VIP

Sie können eine einzelne VIP mit mehreren virtuellen Computern für Lastenausgleichs zuordnen. Beispielsweise haben Sie einen Cloud-Dienst namens *"MyService"*und zwei virtuellen Computer *myVM1* und *myVM2*. Mit der Cloud-Dienst mehrere VIPs eines Namens *Vip2*. Möchten Sie sicherstellen, dass wird allen Datenverkehr auf Port *81* auf *Vip2* zwischen *myVM1* und *myVM2* auf Port *8181*, führen Sie das folgende PowerShell-Skript verteilt:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

Sie können auch Ihre Lastverteiler, um einer anderen VIP aktualisieren. Wenn Sie folgenden PowerShell-Befehl ausführen, werden Sie den Lastenausgleich auf einer VIP namens Vip1 ändern:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Nächste Schritte

[Protokollanalyse Azure-Lastenausgleich](load-balancer-monitor-log.md)

[Mit Load Balancer Übersicht über Internet](load-balancer-internet-overview.md)

[Schritte zum Lastenausgleich mit Internetzugriff](load-balancer-get-started-internet-arm-ps.md)

[Virtuelles Netzwerk (Übersicht)](../virtual-network/virtual-networks-overview.md)

[Reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx)