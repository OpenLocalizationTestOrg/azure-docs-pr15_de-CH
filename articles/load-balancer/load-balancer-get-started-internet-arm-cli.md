<properties
   pageTitle="Erstellen Sie ein System zum Lastenausgleich im Ressourcen-Manager mit der Azure-CLI mit Internetzugriff | Microsoft Azure"
   description="Erstellen Sie ein System zum Lastenausgleich im Ressourcen-Manager mit der Azure-CLI mit Internetzugriff"
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

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Erstellen einer internen Lastenausgleich mithilfe der Azure-CLI

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager. Sie können auch [ein Lastenausgleich klassische Bereitstellung mit Internetzugriff erstellen](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Bereitstellen der Lösung mit der Azure-CLI

Die folgenden Schritte zeigen eine CLI Azure Ressourcenmanager mit Lastenausgleich mit Internetzugriff erstellen. Jede Ressource mit Azure-Ressourcen-Manager erstellt und konfiguriert, dann setzen zu einer Ressource.

Sie müssen erstellen und konfigurieren Sie die folgenden Objekte um einen Lastenausgleich bereitzustellen:

- Front-End-IP-Konfiguration – enthält öffentliche IP-Adressen für den eingehenden Netzwerkverkehr.
- Back-End-Adresspool - enthält Netzwerkkarten (NICs) für virtuelle Maschinen von Lastenausgleich der Netzwerkdatenverkehr.
- Regeln des Lastenausgleichs - enthält Regeln Port im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.
- Eingehende NAT-Regeln - enthält einen Anschluss für einen bestimmten virtuellen Computer im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.
- Prüfpunkte - Gesundheit Prüfpunkte verwendet, um Instanzen von virtuellen Maschinen im Back-End-Adresspool Verfügbarkeit enthält.

Weitere Informationen finden Sie unter [Azure Resource Manager Unterstützung für Lastenausgleich](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Richten Sie CLI Ressourcenmanager

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../../articles/xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** in Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Erwartete Ausgabe:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Erstellen Sie ein virtuelles Netzwerk und eine öffentliche IP-Adresse für den Front-End-IP-pool

1. Erstellen Sie ein virtuelles Netzwerk (VNet) mit dem Namen *NRPVnet* in ostasiatischen US-amerikanischen Standort mithilfe einer Ressourcengruppe mit dem Namen *NRPRG*.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Erstellen Sie ein Subnetz mit dem Namen *NRPVnetSubnet* mit einem CIDR 10.0.0.0/24 in *NRPVnet*.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Erstellen Sie eine öffentliche IP-Adresse mit dem Namen *NRPPublicIP* ein Front-End-IP-Adresspool mit DNS-Namen *loadbalancernrp.eastus.cloudapp.azure.com*verwendet werden. Geben Sie folgenden Befehl verwendet die statische Verteilung und Leerlaufzeitlimit 4 Minuten.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Lastenausgleich wird Domänennamens des öffentlichen IP des FQDN verwendet. Das Ändern von klassischen Bereitstellung Cloud verwendet service Lastenausgleich vollständig qualifizierten Domänennamen (FQDN).
    >In diesem Beispiel ist der FQDN *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Erstellen Sie ein System zum Lastenausgleich

Der folgende Befehl erstellt ein Lastenausgleich mit dem Namen *NRPlb* in der *NRPRG* -Ressourcengruppe im *Südostasiatischen US* Azure Speicherort.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Erstellen Sie ein Front-End-IP-Pool und eine Back-End-Adresspool

In diesem Beispiel wird veranschaulicht, wie der Front-End-IP-Adresspool, der den eingehenden Netzwerkverkehr auf dem Lastenausgleich empfängt und der Back-End-IP-Adresspool, der Front-End-Pool den Lastenausgleich Datenverkehr sendet.

1. Verknüpfen die öffentliche IP-Adresse im vorherigen Schritt und Lastenausgleich erstellt einen Front-End-IP-Adresspool erstellen.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Richten Sie eine Backend-Adresspool für eingehenden Datenverkehr vom Front-End-IP-Pool erhalten.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>Erstellen Sie LB Regeln, NAT-Regeln und probe

Dieses Beispiel erstellt die folgenden Elemente.

- eine NAT-Regel alle eingehenden Datenverkehr auf Port 21 Port 22<sup>1</sup> übersetzen
- eine NAT-Regel alle eingehenden Datenverkehr an Anschluss 23 Port 22 übersetzen
- Load Balancer Regel um alle Datenverkehr über Port 80 an Port 80 auf Adressen im Back-End-Pool verteilen.
- eine Prüfpunkt Regel überprüfen Sie den Status auf einer Seite mit dem Namen *HealthProbe.aspx*.

<sup>1</sup> NAT-Regeln sind eine Instanz bestimmter virtueller Computer hinter den Lastenausgleich. Der Datenverkehr auf Port 21 wird ein bestimmter virtueller Computer auf Port 22 dieser NAT-Regel zugeordneten gesendet. Sie müssen ein Protokoll (UDP oder TCP) für eine NAT-Regel angeben. Beide Protokolle können nicht denselben Port zugewiesen werden.

1. Erstellen Sie die NAT-Regeln.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Erstellen Sie eine Load Balancer Regel.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Erstellen Sie einen Integritätstest.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Überprüfen Sie die Einstellungen.

        azure network lb show nrprg nrplb

    Erwartete Ausgabe:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Erstellen von NICs

Sie müssen NICs erstellen (oder vorhandene ändern) und NAT-Regeln Load Balancer Regeln und Prüfpunkte zuordnen.

1. Erstellen Sie eine Netzwerkkarte namens *lb nic1 werden*, und die *rdp1* NAT-Regel und *NRPbackendpool* Back-End-Adresspool zugeordnet.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

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

2. Erstellen Sie eine Netzwerkkarte namens *lb nic2 werden*, und die *rdp2* NAT-Regel und *NRPbackendpool* Back-End-Adresspool zugeordnet.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Eine virtuelle Maschine (VM) mit dem Namen *web1*und NIC mit dem Namen zuordnen *lb nic1 sein*. Ein Speicherkonto namens *web1nrp* erstellt wurde, bevor Sie den folgenden Befehl ausführen.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] VMs in einem Lastenausgleich müssen in der gleichen Verfügbarkeit. Mit `azure availset create` eine Verfügbarkeit zu erstellen.

    Die Ausgabe sollte ähnlich der folgenden:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] Die Meldung **sieht eine Netzwerkkarte ohne PublicIP konfiguriert** wird seit erstellt Internet mit Load Balancer öffentliche IP-Adresse mit Lastenausgleich NIC erwartet.

    Da die *lb nic1 werden* NIC *rdp1* NAT-Regel zugeordnet ist, können mit *web1* mit RDP über Port 3441 Lastenausgleich.

4. Eine virtuelle Maschine (VM) mit dem Namen *web2*und NIC mit dem Namen zuordnen *lb nic2 werden*. Ein Speicherkonto namens *web1nrp* erstellt wurde, bevor Sie den folgenden Befehl ausführen.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Aktualisieren einer vorhandenen Lastenausgleich

Sie können Regeln verweisen auf vorhandene Lastenausgleich hinzufügen. Im nächsten Beispiel wird eine neue Load Balancer Regel einer vorhandenen Lastenausgleich **NRPlb** hinzugefügt

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Ein Lastenausgleich löschen

Verwenden Sie den folgenden Befehl, um einen Lastenausgleich zu entfernen:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Nächste Schritte

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-cli.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)
