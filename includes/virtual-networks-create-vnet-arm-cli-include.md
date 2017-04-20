## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Erstellen einer VNet mithilfe der Azure-CLI

Azure-CLI können Sie über die Befehlszeile von jedem Computer unter Windows, Linux oder OSX Azure Ressourcen verwalten. Gehen Sie folgendermaßen vor um ein VNet zu erstellen, mithilfe der Azure-CLI.

1. Wenn der Azure-CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../articles/xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **Azure Config Modus** in Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    New mode is arm

3. Gegebenenfalls führen Sie **Azure Gruppe erstellt** eine neue Ressourcengruppe erstellen, wie unten dargestellt. Beachten Sie die Ausgabe des Befehls. Die Liste nach die Ausgabe der Parameter erläutert. Weitere Informationen zu Ressourcengruppen Überblick [Azure Ressourcen-Manager](../articles/virtual-network/resource-group-overview.md#resource-groups).

        azure group create -n TestRG -l centralus

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (bzw. -Namen)**. Name für neue Ressourcengruppe. In diesem Szenario *TestRG*.
    - **(oder -Speicherort)**. Azure-Region, in dem die neue Ressourcengruppe erstellt werden. In diesem Szenario *Centralus*.

4. Führen Sie den Befehl **Azure Netzwerk Vnet erstellen** ein VNet und ein Subnetz erstellen, wie unten dargestellt. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **g (oder -Ressourcengruppen)**. Name der Ressourcengruppe, in dem das VNet erstellt werden. In diesem Szenario *TestRG*.
    - **-n (bzw. -Namen)**. Name des VNet erstellt werden. In diesem Szenario *TestVNet*
    - **-a (oder -Adresspräfixe)**. Liste der CIDR-Blocks für den Adressraum VNet verwendet. In diesem Szenario *192.168.0.0/16*
    - **(oder -Speicherort)**. Azure-Region, in dem das VNet erstellt werden. In diesem Szenario *Centralus*.

5. Führen Sie den Befehl **Azure Netzwerksubnet Vnet erstellen** , erstellen Sie ein Subnetz wie unten dargestellt. Beachten Sie die Ausgabe des Befehls. Die Liste nach die Ausgabe der Parameter erläutert.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (oder - Vnet-**. Name des VNet in das Subnetz erstellt wird. In diesem Szenario *TestVNet*.
    - **-n (bzw. -Namen)**. Name des neuen Subnetzes. In diesem Szenario *FrontEnd*.
    - **-a (oder -Adresse Präfix)**. Subnetz CIDR-Block. Vier unserer Szenario, *192.168.1.0/24*.

6. Wiederholen Sie Schritt 5 oben andere Subnetze erstellen erforderlich. In diesem Szenario führen Sie folgenden Befehl an das *Back-End-* Subnetz erstellen.

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Führen Sie den Befehl **Azure Netzwerk Vnet anzeigen** zum Anzeigen der Eigenschaften der neuen Vnet, wie unten dargestellt.

        azure network vnet show -g TestRG -n TestVNet

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
