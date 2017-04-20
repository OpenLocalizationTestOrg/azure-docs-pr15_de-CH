<properties 
   pageTitle="Wie private Adresse im ARM-Modus mit der CLI | Microsoft Azure"
   description="Grundlegendes zu statischen IP-Adressen (DIPs) und wie sie ARM-Modus über die CLI verwaltet"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>Wie eine statische private IP-Adresse in Azure-CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager. Sie können auch [statische private IP-Adresse im klassischen Bereitstellungsmodell verwalten](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Azure-CLI Beispielbefehle unten erwarten eine einfache Umgebung bereits erstellt. Die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, zuerst Erstellen der Umgebung beschrieben in [ein Vnet erstellen](virtual-networks-create-vnet-arm-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Statische private IP-Adresse angeben, wenn Sie einen virtuellen Computer erstellen
Erstellen Sie einen virtuellen Computer mit dem Namen *DNS01* im *Front-End-* Subnetz ein VNet namens *TestVNet* mit privaten Adresse des *192.168.1.101*wie folgt:

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** in Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Erwartete Ausgabe:

        info:    New mode is arm

3. **Erstellen von Azure Netzwerk öffentliche Ip -** erstellen Sie eine öffentliche IP-Adresse für den virtuellen Computer ausgeführt. Die Liste nach die Ausgabe der Parameter erläutert.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Erwartete Ausgabe:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **g (oder -Ressourcengruppen)**. Name der Ressourcengruppe die öffentliche IP-Adresse wird in erstellt.
    - **-n (bzw. -Namen)**. Name der öffentliche IP-Adresse.
    - **(oder -Speicherort)**. Azure-Region, in dem die öffentliche IP-Adresse erstellt werden. In diesem Szenario *Centralus*.

3. Führen Sie den Befehl **Azure Netzwerk Nic erstellen** eine NIC mit privaten Adresse erstellen. Die Liste nach die Ausgabe der Parameter erläutert.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Erwartete Ausgabe:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **-a (oder -Private IP-Adresse)**. Statische private IP-Adresse für die Netzwerkkarte.
    - **m (oder -Vnet Teilnetznamen)**. Name des VNet, in dem die NIC erstellt werden.
    - **-k (oder -Subnetz)**. Name des Subnetzes die Netzwerkkarte Erstellungsort.

4. Führen Sie den Befehl **Azure Vm erstellen** mit der öffentlichen IP-NIC oben erstellten VM erstellt. Die Liste nach die Ausgabe der Parameter erläutert.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Erwartete Ausgabe:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **-y (oder os-Typ)**. Betriebssystem für den virtuellen Computer *Windows* oder *Linux*.
    - **-f (oder -Nic)**. Name der NIC VM verwendet.
    - **-i (oder - öffentliche IP-Name)**. Namen von öffentlichen IP-VM verwendet.
    - **-F (oder Vnet-Namen)**. Name des VNet, der virtuellen Computer erstellt werden.
    - **-j (oder Teilnetznamen-Vnet)**. Name des Subnetzes, in dem die VM erstellt werden.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Statische private IP-Adressinformationen für einen virtuellen Computer abrufen

Anzeige die statische private IP-Adressinformationen für die VM mit dem Beispielskript erstellt führen Sie folgenden Azure CLI-Befehl aus, und beobachten Sie die Werte für *Private IP-Zuordnung-Methode* und *Private IP-Adresse*:

    azure vm show -g TestRG -n DNS01

Erwartete Ausgabe:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Statische private IP-Adresse eines virtuellen Computers entfernen
Sie können nicht statische private IP-Adresse von NIC in Azure-CLI für Ressourcen-Manager entfernen. Sie müssen erstellen Sie eine neue Netzwerkkarte, die eine dynamische IP-Adresse verwendet die VM die vorherige Netzwerkkarte entfernen und fügen Sie die neue Netzwerkkarte für die VM. Ändern der NIC für die VM verwendet Int eh oben angeführten Befehle wie folgt.
    
1. Führen Sie den Befehl **Azure Netzwerk Nic erstellen** , erstellen Sie eine neue Netzwerkkarte mit dynamischen IP-Zuordnung. Beachten Sie, wie Sie nicht müssen die IP-Adresse angeben.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Erwartete Ausgabe:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. Führen Sie den Befehl **Azure Vm legen** die VM verwendete NIC ändern.

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Erwartete Ausgabe:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Wenn, führen Sie den Befehl **Azure Netzwerk Nic löschen** alte Netzwerkkarte löschen

        azure network nic delete -g TestRG -n TestNIC --quiet

    Erwartete Ausgabe:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Statische private IP-Adresse zu einem vorhandenen virtuellen Computer hinzufügen
Um statische private IP-Adresse der Netzwerkkarte mit dem Beispielskript erstellt VM verwendet hinzuzufügen, führen Sie den folgenden Befehl ein:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Erwartete Ausgabe:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Nächste Schritte

- [Reservierte öffentliche IP-](virtual-networks-reserved-public-ip.md) Adressen erfahren.
- Erfahren Sie mehr über [öffentliche IP (ILPIP) auf Instanzebene](virtual-networks-instance-level-public-ip.md) Adressen.
- Wenden Sie sich an die [reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).
