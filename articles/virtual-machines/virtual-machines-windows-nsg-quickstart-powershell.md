<properties
   pageTitle="Öffnen Sie Ports einer VM mit PowerShell | Microsoft Azure"
   description="So öffnen Sie einen Port / Endpunkt der Windows-VM mit der Azure-Ressourcen-Manager-Bereitstellungsmodus Azure PowerShell erstellen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Öffnen von Anschlüssen und Endpunkte einer VM in Azure mithilfe von PowerShell
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Schnellzugriff
Erstellen einer Network Security und ACL Regeln benötigen Sie [die neueste Version von Azure PowerShell installiert](../powershell-install-configure.md). Sie können auch [gehen über das Azure-Portal](virtual-machines-windows-nsg-quickstart-portal.md).

Melden Sie sich bei Ihrem Azure-Konto:

```powershell
Login-AzureRmAccount
```

Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten `myResourceGroup`, `myNetworkSecurityGroup`, und `myVnet`.

Erstellen Sie eine Regel. Das folgende Beispiel erstellt eine Regel namens `myNetworkSecurityGroupRule` zu TCP-Datenverkehr an Port 80:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Anschließend erstellen Sie die Network Security-Gruppe und weisen Sie HTTP-Regel erstellten wie folgt. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Jetzt weisen wir die Netzwerk-Sicherheitsgruppe Subnetz. Im folgenden Beispiel wird ein vorhandenes virtuelles Netzwerk mit dem Namen `myVnet` der Variablen `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Ordnen Sie die Netzwerk-Sicherheitsgruppe Subnetz. Im folgende Beispiel ordnet das Subnetz mit dem Namen `mySubnet` mit der Netzwerk-Sicherheitsgruppe:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Schließlich aktualisieren Sie das virtuelle Netzwerk, damit die Änderungen wirksam:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerk-Sicherheitsgruppen
Die schnelle Befehle ermöglichen mit der VM-Datenverkehr zu kommen. Netzwerk-Sicherheitsgruppen bieten viele hervorragende Funktionen und Granularität zum Steuern des Zugriffs auf Ressourcen. Erfahren Sie mehr über das [Erstellen einer Sicherheitsgruppe Netzwerk und ACL Regeln](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Als Teil der Azure-Ressourcen-Manager-Vorlagen können Sie Netzwerk-Sicherheitsgruppen und ACL-Regeln definieren. Mehr über das [Erstellen von Sicherheitsgruppen mit Vorlagen Netzwerk](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Verwenden Sie müssen verwenden Port Weiterleitung zu einem internen Anschluss VM einen eindeutigen externen Anschluss zuordnen, ein Lastenausgleich und Network Address Translation (NAT) Regeln. Beispielsweise möchten Sie TCP-Port 8080 extern verfügbar machen und Datenverkehr an TCP-Port 80 auf einer VM geleitet. Sie erfahren [erstellen ein Lastenausgleich Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel wird eine einfache Regel HTTP-Datenverkehr erstellt. Finden Sie Informationen zum Erstellen von detaillierte Umgebung in den folgenden Artikeln:

- [Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md)
- [Was ist Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure Ressourcenmanager Übersicht für Lastenausgleich](../load-balancer/load-balancer-arm.md)