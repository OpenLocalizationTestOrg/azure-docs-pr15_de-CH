<properties 
   pageTitle="Routing und virtuellen Geräte der Azure-CLI im klassischen Bereitstellungsmodell | Microsoft Azure"
   description="Erfahren Sie, wie routing in VNets mit der Azure-CLI im klassischen Bereitstellungsmodell steuern"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Routing und virtuelle Appliances (klassisch) mit der Azure-CLI

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell. Sie können auch das [Steuerelement routing und virtuellen Geräte in der Ressourcen-Manager-Bereitstellungsmodell](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Azure-CLI Beispielbefehle unten erwarten eine einfache Umgebung bereits auf das Szenario. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, erstellen Sie die Umgebung in [einer VNet (classic) mit der Azure-CLI erstellen](virtual-networks-create-vnet-classic-cli.md)angezeigt.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>UDR für das front-End-Subnetz erstellen
Gehen Sie wie folgt vor Erstellen der Routentabelle und Route für das front-End-Subnetz basierend auf dem Szenario benötigt.

1. Ausführen der **`azure config mode`** auf zur klassischen Ansicht wechseln.

        azure config mode asm

    Ausgabe:

        info:    New mode is asm

3. Führen Sie die **`azure network route-table create`** Befehl Route für das front-End-Subnetz erstellen.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Ausgabe:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Parameter:
    - **(oder -Speicherort)**. Azure-Region, in dem neue NSG erstellt werden. In diesem Szenario *Westus*.
    - **-n (bzw. -Namen)**. Name für neue NSG. In diesem Szenario *NSG FrontEnd*.

4. Führen Sie die **`azure network route-table route set`** Befehl zum Erstellen einer Route in der Routentabelle oben an allen Datenverkehr auf dem Back-End-Subnetz (192.168.2.0/24) **FW1** VM (192.168.0.4) erstellt.

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Ausgabe:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Parameter:
    - **-r (oder -Route Tabellenname)**. Name der Routentabelle an die Route hinzugefügt wird. In diesem Szenario *UDR-FrontEnd*.
    - **-a (oder -Adresse Präfix)**. Adresspräfix für das Subnetz, in Pakete bestimmt sind. In diesem Szenario *192.168.2.0/24*.
    - **-t (oder -weiter-Hop-Typ)**. Typ des Datenverkehrs Objekt erhalten. Mögliche Werte sind *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*oder *None*.
    - **-p (oder - Next-Hop-Ip-Adresse**). IP-Adresse für den nächsten Hop. In diesem Szenario *192.168.0.4*.

5. Führen Sie die **`azure network vnet subnet route-table add`** Befehl zuordnen die Routentabelle oben mit der **Front-End** -Subnetzmaske erstellt.

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Ausgabe:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Parameter:
    - **-t (oder -Vnet-Namen)**. Name des VNet befindet sich im Subnetz. In diesem Szenario *TestVNet*.
    - **- n (oder - Subnetz**. Name des Teilnetzes der Routentabelle hinzugefügt werden. In diesem Szenario *FrontEnd*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>UDR für das Back-End-Subnetz erstellen
Gehen Sie wie folgt vor Erstellen der Routentabelle und Route für das Back-End-Subnetz basierend auf dem Szenario benötigt.

3. Führen Sie die **`azure network route-table create`** Befehl eine Routentabelle für Back-End-Subnetz erstellen.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Führen Sie die **`azure network route-table route set`** Befehl zum Erstellen einer Route in der Routentabelle oben an allen Datenverkehr auf dem front-End-Subnetz (192.168.1.0/24) **FW1** VM (192.168.0.4) erstellt.

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Führen Sie die **`azure network vnet subnet route-table add`** Befehl die Routentabelle mit dem **Back-End-** Subnetz oben erstellten zuordnen.

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

