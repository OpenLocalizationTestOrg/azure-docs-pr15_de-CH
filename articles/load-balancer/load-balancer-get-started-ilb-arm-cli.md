<properties
   pageTitle="Mithilfe einer internen Lastenausgleich der Azure-CLI im Ressourcenmanager | Microsoft Azure"
   description="Informationen Sie zum internen Lastenausgleich mithilfe der Azure-CLI in Ressourcen-Manager"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Mithilfe der Azure-CLI einen internen Lastenausgleich

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Bereitstellen der Lösung mithilfe der Azure-CLI

Die folgenden Schritte zeigen ein Lastenausgleich Internetzugriff CLI mit Azure-Ressourcen-Manager erstellen. Azure Resource Manager jede Ressource erstellt und individuell konfiguriert und dann zu einer Ressource.

Sie müssen zum Erstellen und konfigurieren Sie die folgenden Objekte um einen Lastenausgleich bereitzustellen:

- **Front-End-IP-Konfiguration**: enthält öffentliche IP-Adressen für eingehenden Verkehr
- **Back-End-Adresspool**: enthält Netzwerkschnittstellen (NICs), die die virtuellen Maschinen Netzwerkverkehr vom Lastenausgleicher empfangen
- **Lastenausgleich Regeln**: enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich Port im Back-End-Adresspool zuordnen
- **Eingehende NAT-Regeln**: enthält Regeln, die einen Anschluss für einen bestimmten virtuellen Computer im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen
- **Prüfpunkte**: Health Prüfpunkte, mit denen die Verfügbarkeit der virtuellen Maschinen Instanzen im Back-End-Adresspool enthält

Weitere Informationen finden Sie unter [Azure Resource Manager Unterstützung für Lastenausgleich](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Richten Sie CLI Ressourcenmanager

1. Azure-Befehlszeilenschnittstelle noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../../articles/xplat-cli-install.md). Gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** wie folgt in den Ressourcen-Manager-Modus zu wechseln:

        azure config mode arm

    Erwartete Ausgabe:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Erstellen einer internen Lastenausgleich Schritt für Schritt

1. Melden Sie sich bei Azure.

        azure login

    Geben Sie bei Aufforderung Ihre Azure-Anmeldeinformationen.

2. Ändern Sie die Befehlszeilenprogrammen Azure-Ressourcen-Manager-Modus.

        azure config mode arm

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Alle Ressourcen in Azure-Ressourcen-Manager sind eine Ressourcengruppe zugeordnet. Wenn Sie dies noch nicht getan haben, erstellen Sie eine Ressourcengruppe.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Eine interne Load Balancer erstellen

1. Erstellen einer internen Lastenausgleich

    Im folgenden Szenario wird eine Ressourcengruppe mit dem Namen Nrprg im südostasiatischen USA erstellt.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Alle Ressourcen für eine interne Lastenausgleich virtuelle Netzwerke und virtuelle Netzwerk-Subnetze muss in derselben Ressourcengruppe und im selben Bereich.

2. Erstellen Sie eine Front-End-IP-Adresse der internen Lastenausgleich.

    Die IP-Adresse muss im Subnetzbereich des virtuellen Netzwerks sein.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. Erstellen Sie den Back-End-Adresspool.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Nachdem Sie eine Front-End- und Back-End-Adresspool definieren, Load Balancer Regeln eingehende NAT-Regeln erstellen und benutzerspezifische untersucht.

4. Erstellen Sie eine Load Balancer Regel für interne Lastenausgleich.

    Wenn Sie die vorherigen Schritte erstellt der Befehl Lastenausgleich Regel Port 1433 im Front-End-Pool und Netzwerkverkehr mit Lastenausgleich an den Back-End-Adresspool sendet, auch Port 1433 verwenden.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Erstellen Sie eingehende NAT-Regeln.

    Eingehende NAT-Regeln zum Erstellen von Endpunkten in einem Lastenausgleich, die auf einem bestimmten virtuellen Instanz. Die vorherigen Schritte erstellt zwei NAT-Regeln für Remotedesktop.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Erzeugen Sie Prüfpunkte für Lastenausgleich health

    Eine Integritätstest überprüft alle virtuellen Instanzen um sicherzustellen, dass sie den Netzwerkverkehr senden können. Die VM-Instanz mit fehlerhaften Prüfpunkt wird entfernt von Lastenausgleich geht wieder online und eine Prüfpunkt Überprüfung wird ermittelt, dass sie fehlerfrei ist.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]Microsoft Azure-Plattform verwendet eine statische, öffentlich routingfähige IPv4-Adresse für verschiedene administrative Szenarios. Die IP-Adresse ist 168.63.129.16. Diese IP-Adresse sollte nicht durch Firewalls, blockiert werden, da dies zu unerwartetes Verhalten führen kann.
    >Bei Azure internen Lastenausgleich diese IP-Adresse dient durch Überwachen von Lastenausgleich Prüfpunkte Zustand für virtuelle Computer in eine Gruppe mit Lastenausgleich fest. Sicherstellen Sie eine Netzwerk-Sicherheitsgruppe Azure virtuelle Computer in einem intern Lastenausgleich Verkehr eingeschränkt wird oder ein virtuelles Netzwerk-Subnetz zugewiesen wird, dass eine Netzwerkregel Sicherheit Datenverkehr von 168.63.129.16 hinzugefügt.

## <a name="create-nics"></a>Erstellen von NICs

Sie müssen NICs erstellen (oder vorhandene ändern) und NAT-Regeln Load Balancer Regeln und Prüfpunkte zuordnen.

1. Erstellen Sie eine Netzwerkkarte namens *lb nic1 sein*, und verknüpfen Sie es mit *rdp1* NAT-Regel und *Beilb* Back-End-Adresspool.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

    Erwartete Ausgabe:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Erstellen Sie eine Netzwerkkarte namens *lb nic2 werden*, und verknüpfen Sie es mit *rdp2* NAT-Regel und *Beilb* Back-End-Adresspool.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Erstellen Sie einen virtuellen Computer mit dem Namen *"db1"*und danach die NIC namens zugeordnet *lb nic1 sein*. Ein Speicherkonto namens *web1nrp* wird erstellt, bevor der folgende Befehl ausgeführt wird:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs in einem Lastenausgleich müssen in der gleichen Verfügbarkeit. Mit `azure availset create` eine Verfügbarkeit zu erstellen.

4. Eine virtuelle Maschine (VM) namens *DB2*und dann die Netzwerkkarte mit dem Namen zuordnen *lb nic2 werden*. Ein Speicherkonto namens *web1nrp* erstellt wurde, bevor Sie den folgenden Befehl ausführen.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Ein Lastenausgleich löschen

Verwenden Sie den folgenden Befehl, um einen Lastenausgleich zu entfernen:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren Sie einen Load Balancer Verteilung Modus mit Source IP-Affinität](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)
