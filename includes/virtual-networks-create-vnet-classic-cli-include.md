## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Erstellen Sie eine klassische VNet mit Azure-CLI

Azure-CLI können Sie über die Befehlszeile von jedem Computer unter Windows, Linux oder OSX Azure Ressourcen verwalten. Gehen Sie folgendermaßen vor um ein VNet zu erstellen, mithilfe der Azure-CLI.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../articles/xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **Azure Netzwerk Vnet erstellen** ein VNet und ein Subnetz erstellen, wie unten dargestellt. Die Liste nach die Ausgabe der Parameter erläutert.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Erwartete Ausgabe:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **-Vnet**. Name des VNet erstellt werden. In diesem Szenario *TestVNet*
    - **-e (oder -Adressraum)**. VNet-Adressraum. In diesem Szenario *192.168.0.0*
    - **-i (oder - Cidr)**. Netzwerkmaske im CIDR-Format. In diesem Szenario *16*.
    - **- n (oder - Subnetz**). Name des ersten Subnetz. In diesem Szenario *FrontEnd*.
    - **-p (oder -Ip-Start-Subnetz)**. IP-Startadresse für Subnetz oder Subnetz-Adressraum. In diesem Szenario *192.168.1.0*.
    - **-r (oder -Subnetz Cidr)**. Netzwerkmaske im CIDR-Format für Subnetz. In diesem Szenario *24*.
    - **(oder -Speicherort)**. Azure-Region, in dem das VNet erstellt werden. In diesem Szenario *USA*.

3. Führen Sie den Befehl **Azure Netzwerksubnet Vnet erstellen** , erstellen Sie ein Subnetz wie unten dargestellt. Die Liste nach die Ausgabe der Parameter erläutert.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Hier ist die erwartete Ausgabe für den Befehl oben:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (oder - Vnet-**. Name des VNet in das Subnetz erstellt wird. In diesem Szenario *TestVNet*.
    - **-n (bzw. -Namen)**. Name des neuen Subnetzes. In diesem Szenario *BackEnd*.
    - **-a (oder -Adresse Präfix)**. Subnetz CIDR-Block. Vier unserer Szenario, *192.168.2.0/24*.

4. Führen Sie den Befehl **Azure Netzwerk Vnet anzeigen** zum Anzeigen der Eigenschaften der neuen Vnet, wie unten dargestellt.

            azure network vnet show

    Hier ist die erwartete Ausgabe für den Befehl oben:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
