<properties
    pageTitle="Ein Lastenausgleich mit IPv6 in Azure-Ressourcen-Manager mit der Azure-CLI mit Internetzugriff erstellen | Microsoft Azure"
    description="Erstellen Sie ein Lastenausgleich mit IPv6 in Azure-Ressourcen-Manager mit der Azure-CLI mit Internetzugriff"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6 Azure Lastenausgleich dual-Stack, öffentliche IP-Adresse, nativen IPv6-, Mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Erstellen Sie ein Lastenausgleich mit IPv6 in Azure-Ressourcen-Manager mit der Azure-CLI mit Internetzugriff

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Vorlage](./load-balancer-ipv6-internet-template.md)

Ein Azure Lastverteiler ist ein Layer-4 (TCP, UDP)-Lastenausgleich. Lastenausgleich bietet hohe Verfügbarkeit durch verteilen eingehenden Datenverkehr fehlerfrei Dienstinstanzen mit Cloud-Services oder virtuelle Computer in einer Load Balancer. Azure Lastenausgleich können auch diese Dienste auf mehrere Ports oder mehrere IP-Adressen darstellen.

## <a name="example-deployment-scenario"></a>Beispielszenario für die Bereitstellung

Das folgende Diagramm veranschaulicht den Lastenausgleich Lösung mithilfe der in diesem Artikel beschriebene Beispielvorlage bereitgestellt wird.

![Load Balancer Szenario](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

In diesem Szenario erstellen Sie Azure Folgendes:

- zwei virtuelle Maschinen (VMs)
- eine virtuelle Netzwerkschnittstelle für jede VM mit IPv4- und IPv6-Adressen
- ein Internetzugriff zum Lastenausgleich mit einer IPv4- und IPv6-öffentliche Adresse
- eine Verfügbarkeit, enthält zwei VMs
- zwei laden Netzwerklastenausgleich Regeln für private Endpunkte öffentliche VIPs zuordnen

## <a name="deploying-the-solution-using-the-azure-cli"></a>Bereitstellen der Lösung mit der Azure-CLI

Die folgenden Schritte zeigen eine CLI Azure Ressourcenmanager mit Lastenausgleich mit Internetzugriff erstellen. Azure Resource Manager jede Ressource erstellt und individuell konfiguriert und dann zu einer Ressource.

Um einen Lastenausgleich bereitzustellen, erstellen und konfigurieren Sie die folgenden Objekte:

- Front-End-IP-Konfiguration – enthält öffentliche IP-Adressen für den eingehenden Netzwerkverkehr.
- Back-End-Adresspool - enthält Netzwerkkarten (NICs) für virtuelle Maschinen von Lastenausgleich der Netzwerkdatenverkehr.
- Regeln des Lastenausgleichs - enthält Regeln Port im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.
- Eingehende NAT-Regeln - enthält einen Anschluss für einen bestimmten virtuellen Computer im Back-End-Adresspool mit einem öffentlichen Port auf dem Lastenausgleich zuordnen.
- Prüfpunkte - Gesundheit Prüfpunkte verwendet, um Instanzen von virtuellen Maschinen im Back-End-Adresspool Verfügbarkeit enthält.

Weitere Informationen finden Sie unter [Azure Resource Manager Unterstützung für Lastenausgleich](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Richten Sie Ihre Umgebung CLI Azure-Ressourcen-Manager

Für dieses Beispiel werden wir die CLI-Tools in einem Befehlsfenster PowerShell ausgeführt. Wir Azure PowerShell-Cmdlets nicht verwenden, aber wir PowerShell Skriptfunktionen Lesbarkeit und Wiederverwendung zu verwenden.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../../articles/xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** in Ressourcen-Manager-Modus wechseln.

        azure config mode arm

    Erwartete Ausgabe:

        info:    New mode is arm

3. Azure anmelden und eine Liste der Abonnements.

        azure login

    Anmeldeinformationen Sie Azure Aufforderung.

        azure account list

    Wählen Sie das Abonnement, das Sie verwenden möchten. Notieren Sie die Abonnement-Id für nächsten Schritt.

4. Einrichten von PowerShell-Variablen für die CLI-Befehle.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Erstellen Sie eine Ressourcengruppe, ein Lastenausgleich, ein virtuelles Netzwerk und Subnetze

1. Erstellen einer Ressourcengruppe

        azure group create $rgName $location

2. Erstellen Sie ein System zum Lastenausgleich

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Erstellen Sie ein virtuelles Netzwerk (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Erstellen Sie zwei Subnetzen in dieser VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Öffentliche IP-Adressen für den Front-End-Pool erstellen

1. Das PowerShell-Variablen

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Erstellen Sie eine öffentliche IP-Adresse des Front-End-IP-Adresspools.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Lastenausgleich verwendet Domänennamens des öffentlichen IP als seinen vollqualifizierten Domänennamen. Das Ändern von klassischen Bereitstellung Cloud-Dienst verwendet den Namen Lastenausgleich FQDN.
    >In diesem Beispiel ist der FQDN *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Front-End- und Back-End-Pools erstellen

Dieses Beispiel erstellt die Front-End-IP-Adresspool, der den eingehenden Netzwerkverkehr auf dem Lastenausgleich empfängt und der Back-End-IP-Adresspool, der Front-End-Pool den Lastenausgleich Datenverkehr sendet.

1. Das PowerShell-Variablen

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Verknüpfen die öffentliche IP-Adresse im vorherigen Schritt und Lastenausgleich erstellt einen Front-End-IP-Adresspool erstellen.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>Erstellen Sie Prüfpunkt, NAT-Regeln und LB Regeln

Dieses Beispiel erstellt die folgenden Elemente:

- ein Prüfpunkt Regel Verbindung zum TCP-Port 80 zu überprüfen
- eine NAT-Regel alle eingehenden Datenverkehr auf Port 3389 an Port 3389 für RDP<sup>1</sup> übersetzen
- eine NAT-Regel alle eingehenden Datenverkehr auf Port 3389 für RDP<sup>1</sup> Anschluss 3391 übersetzen
- Load Balancer Regel um alle Datenverkehr über Port 80 an Port 80 auf Adressen im Back-End-Pool verteilen.

<sup>1</sup> NAT-Regeln sind eine Instanz bestimmter virtueller Computer hinter den Lastenausgleich. Der Port 3389 eingehenden Netzwerkverkehr wird bestimmter virtueller Computer und NAT-Regel zugeordneten Port gesendet. Sie müssen ein Protokoll (UDP oder TCP) für eine NAT-Regel angeben. Beide Protokolle können nicht denselben Port zugewiesen werden.

1. Das PowerShell-Variablen

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. Den Prüfpunkt erstellen

    Das folgende Beispiel erzeugt einen TCP-Prüfpunkt, der überprüft Konnektivität mit Backend-TCP-Port 80 alle 15 Sekunden. Diese markiert Back-End-Ressourcen nicht verfügbar nach zwei aufeinander folgenden Fehler.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Erstellen Sie eingehende NAT-Regeln, die RDP-Verbindung zum Back-End-Ressourcen

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Erstellen Sie ein Lastenausgleich Regeln, die verschiedene Backend-Ports je nach der front-End die Anforderung empfangen Datenverkehr senden

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Überprüfen Sie die Einstellungen

        azure network lb show --resource-group $rgName --name $lbName

    Erwartete Ausgabe:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>Erstellen von NICs

Erstellen Sie Netzwerkkarten und NAT-Regeln Load Balancer Regeln und Prüfpunkte zuordnen.

1. Das PowerShell-Variablen

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Erstellen einer NIC jede Backend und eine IPv6-Konfiguration hinzufügen.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Backend-VM Ressourcen erstellen und jede NIC Anfügen

Erstellen Sie virtuelle Computer benötigen Sie ein Speicherkonto. Für den Lastenausgleich müssen die VMs Mitglieder eines Verfügbarkeit. Weitere Informationen zum Erstellen virtueller Computer finden Sie unter [Erstellen einer VM Azure mithilfe von PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)

1. Das PowerShell-Variablen

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] Dieses Beispiel verwendet den Benutzernamen und das Kennwort für die virtuellen Computer im Klartext. Entsprechende Vorsicht beim mit in Klartext Anmeldeinformationen. Eine sicherere Methode mit Anmeldeinformationen in PowerShell finden Sie unter [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) -Cmdlet.

2. Der Speicher und verfügbaren erstellen

    Ein vorhandenes Speicherkonto können beim Erstellen von VMs. Der folgende Befehl erstellt ein neues Speicherkonto.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Anschließend erstellen Sie die Verfügbarkeit.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. Erstellen Sie virtueller Computer mit zugeordneten NICs

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Nächste Schritte

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-cli.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)
