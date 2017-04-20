<properties 
   pageTitle="Routing und virtuelle Appliances mit PowerShell im klassischen Bereitstellungsmodell | Microsoft Azure"
   description="Enthält Informationen zum routing VNets mithilfe von PowerShell im klassischen Bereitstellungsmodell steuern"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Routing und virtuelle Appliances (klassisch) mit PowerShell

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Im Beispiel oben beschriebenen Szenario Azure PowerShell Befehle unten eine einfache Umgebung bereits erwarten Grundlage. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, erstellen Sie die Umgebung in [einer VNet (classic) mit PowerShell erstellen](virtual-networks-create-vnet-classic-netcfg-ps.md)angezeigt.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>UDR für das front-End-Subnetz erstellen
Gehen Sie wie folgt vor Erstellen der Routentabelle und Route für das front-End-Subnetz basierend auf dem Szenario benötigt.

3. Ausführen der **`New-AzureRouteTable`** -Cmdlet zum Arbeitsplan für das front-End-Subnetz erstellen.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Ausgabe:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Ausführen der **`Set-AzureRoute`** -Cmdlet zum Erstellen einer Route in der Routentabelle oben erstellte alle Datenverkehr auf dem Back-End-Subnetz (192.168.2.0/24) **FW1** VM (192.168.0.4) senden.
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Ausgabe:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Führen Sie die **`Set-AzureSubnetRouteTable`** Cmdlet zuordnen die Routentabelle mit **Front-End** -Subnetz über erstellt.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>UDR für das Back-End-Subnetz erstellen
Gehen Sie wie folgt vor Erstellen der Routentabelle und Route für das Back-End-Subnetz basierend auf dem Szenario benötigt.

3. Führen Sie die **`New-AzureRouteTable`** Cmdlet eine Routentabelle für Back-End-Subnetz erstellen.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Ausführen der **`Set-AzureRoute`** -Cmdlet zum Erstellen einer Route in der Routentabelle oben erstellte alle Datenverkehr auf dem front-End-Subnetz (192.168.1.0/24) **FW1** VM (192.168.0.4) senden.

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Führen Sie die **`Set-AzureSubnetRouteTable`** Cmdlet zuordnen die Routentabelle oben mit dem **Back-End-** Subnetz erstellt.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>IP-Weiterleitung auf dem virtuellen Computer FW1 aktivieren
Gehen Sie folgendermaßen vor um IP-Weiterleitung in der FW1 VM zu aktivieren.

1. Ausführen der **`Get-AzureIPForwarding`** -Cmdlet zum Prüfen der Status der IP-Weiterleitung

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Ausgabe:

        Disabled

2. Führen Sie die **`Set-AzureIPForwarding`** Befehl IP-Weiterleitung für *FW1* VM aktivieren.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
