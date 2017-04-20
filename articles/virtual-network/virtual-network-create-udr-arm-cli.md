<properties 
   pageTitle="Routing steuern und virtuelle Appliances in Ressourcen-Manager mit der Azure-CLI verwenden | Microsoft Azure"
   description="Enthält Informationen zum routing und virtuellen Geräte der Azure-CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Erstellen benutzerdefinierter Routen (UDR) in Azure CLI

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager. Sie können auch [im klassischen Bereitstellungsmodell Decision erstellen](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Azure-CLI Beispielbefehle unten erwarten eine einfache Umgebung bereits auf das Szenario. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, zuerst die Umgebung durch Bereitstellung [dieser Vorlage](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before)erstellen **in Azure bereitstellen**, Standardparameterwerte ggf. ersetzen und auf gehen in das Portal.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>UDR für das front-End-Subnetz erstellen
Gehen Sie wie folgt vor Erstellen der Routentabelle und Route für das front-End-Subnetz basierend auf dem Szenario benötigt.

3. Führen Sie die **`azure network route-table create`** Befehl Route für das front-End-Subnetz erstellen.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Ausgabe:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Parameter:
    - **g (oder -Ressourcengruppen)**. Name der Ressourcengruppe, in dem die UDR erstellt werden. In diesem Szenario *TestRG*.
    - **(oder -Speicherort)**. Azure-Region, in dem die neue UDR erstellt werden. In diesem Szenario *Westus*.
    - **-n (bzw. -Namen)**. Name für neue UDR. In diesem Szenario *UDR-FrontEnd*.

4. Führen Sie die **`azure network route-table route create`** Befehl zum Erstellen einer Route in der Routentabelle oben an allen Datenverkehr auf dem Back-End-Subnetz (192.168.2.0/24) **FW1** VM (192.168.0.4) erstellt.

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Ausgabe:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Parameter:
    - **-r (oder -Route Tabellenname)**. Name der Routentabelle an die Route hinzugefügt wird. In diesem Szenario *UDR-FrontEnd*.
    - **-a (oder -Adresse Präfix)**. Adresspräfix für das Subnetz, in Pakete bestimmt sind. In diesem Szenario *192.168.2.0/24*.
    - **-y (oder -weiter-Hop-Typ)**. Typ des Datenverkehrs Objekt erhalten. Mögliche Werte sind *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*oder *None*.
    - **-p (oder - Next-Hop-Ip-Adresse**). IP-Adresse für den nächsten Hop. In diesem Szenario *192.168.0.4*.

5. Führen Sie die **`azure network vnet subnet set`** Befehl zuordnen die Routentabelle oben mit der **Front-End** -Subnetzmaske erstellt.

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Ausgabe:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Parameter:
    - **-e (oder -Vnet-Namen)**. Name des VNet befindet sich im Subnetz. In diesem Szenario *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>UDR für das Back-End-Subnetz erstellen
Gehen Sie wie folgt vor Erstellen der Routentabelle und Route für das Back-End-Subnetz basierend auf dem Szenario benötigt.

1. Führen Sie die **`azure network route-table create`** Befehl eine Routentabelle für Back-End-Subnetz erstellen.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Führen Sie die **`azure network route-table route create`** Befehl zum Erstellen einer Route in der Routentabelle oben an allen Datenverkehr auf dem front-End-Subnetz (192.168.1.0/24) **FW1** VM (192.168.0.4) erstellt.

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Führen Sie die **`azure network vnet subnet set`** Befehl die Routentabelle mit dem **Back-End-** Subnetz oben erstellten zuordnen.

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>IP-Weiterleitung auf FW1 aktivieren
Gehen Sie folgendermaßen vor um IP-Weiterleitung in **FW1**verwendete NIC zu aktivieren.

1. Führen Sie die **`azure network nic show`** Befehl, und beachten Sie den Wert für die **Weiterleitung von IP aktivieren**. Er sollte auf *false*festgelegt.

        azure network nic show -g TestRG -n NICFW1

    Ausgabe:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Führen Sie die **`azure network nic set`** Befehl IP-Weiterleitung aktivieren.

        azure network nic set -g TestRG -n NICFW1 -f true

    Ausgabe:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Parameter:

    - **-f (oder -aktivieren IP-Weiterleitung)**. *true* oder *false*.
