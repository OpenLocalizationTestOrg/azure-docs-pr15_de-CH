<properties 
   pageTitle="Bereitstellen eine VM mit öffentlichen Adresse mit der Azure-CLI im Ressourcenmanager | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von VMs mit öffentlichen Adresse mit der Azure-CLI in Ressourcen-Manager"
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

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-cli"></a>Bereitstellen einer VM mit öffentlichen Adresse mit der Azure-CLI

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="step-1---start-your-script"></a>Schritt 1 - Skript starten

Verwendet volle Skript herunterladen [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-cli.sh). Gehen Sie folgendermaßen vor, so ändern Sie das Skript in Ihrer Umgebung funktionieren.

1. Ändern Sie die Werte der folgenden Variablen basierend auf den Werten für die Bereitstellung verwenden möchten. Die Werte unten in diesem Dokument verwendete Szenario zugeordnet.

        # Set variables for the new resource group
        rgName="IaaSStory"
        location="westus"
        
        # Set variables for VNet
        vnetName="TestVNet"
        vnetPrefix="192.168.0.0/16"
        subnetName="FrontEnd"
        subnetPrefix="192.168.1.0/24"
        
        # Set variables for storage
        stdStorageAccountName="iaasstorystorage"
        
        # Set variables for VM
        vmSize="Standard_A1"
        diskSize=127
        publisher="Canonical"
        offer="UbuntuServer"
        sku="14.04.2-LTS"
        version="latest"
        vmName="WEB1"
        osDiskName="osdisk"
        nicName="NICWEB1"
        privateIPAddress="192.168.1.101"
        username='adminuser'
        password='adminP@ssw0rd'
        pipName="PIPWEB1"
        dnsName="iaasstoryws1"

## <a name="step-2---create-the-necessary-resources-for-your-vm"></a>Schritt 2 - Erstellen Sie die erforderlichen Ressourcen für die VM

Bevor Sie einen virtuellen Computer erstellen, benötigen Sie eine Ressourcengruppe, VNet, öffentliche IP-Adresse und NIC VM verwendet werden.

1. Erstellen Sie eine neue Ressourcengruppe.

        azure group create $rgName $location
        
2. Erstellen Sie VNet und Subnetz.
        
        azure network vnet create --resource-group $rgName \
            --name $vnetName \
            --address-prefixes $vnetPrefix \
            --location $location
        azure network vnet subnet create --resource-group $rgName \
            --vnet-name $vnetName \
            --name $subnetName \
            --address-prefix $subnetPrefix

3. Erstellen Sie die öffentliche IP-Adressenressource. 

        azure network public-ip create --resource-group $rgName \
            --name $pipName \
            --location $location \
            --allocation-method Static \
            --domain-name-label $dnsName 

4. Erstellen Sie die Netzwerkschnittstelle (NIC) für die VM erstellt, mit der öffentlichen IP-Subnetz. Beachten Sie den ersten Satz von Befehlen wird die **Id** des Subnetzes oben erstellt.

        subnetId="$(azure network vnet subnet show --resource-group $rgName \
                        --vnet-name $vnetName \
                        --name $subnetName|grep Id)"

        subnetId=${subnetId#*/}
        
        azure network nic create --name $nicName \
            --resource-group $rgName \
            --location $location \
            --private-ip-address $privateIPAddress \
            --subnet-id $subnetId \
            --public-ip-name $pipName

    >[AZURE.TIP] Der erste Befehl oben verwendet [Grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) und [Zeichenfolgen](http://tldp.org/LDP/abs/html/string-manipulation.html) (genauer gesagt Teilzeichenfolge Entfernung). 

5. Erstellen Sie ein Speicherkonto Betriebssystem virtuellen Laufwerk bereitstellen.

        azure storage account create $stdStorageAccountName \
            --resource-group $rgName \
            --location $location --type LRS 

## <a name="step-3---create-the-vm"></a>Schritt 3 - Erstellen der VM 

Alle notwendigen Ressourcen vorhanden sind, können Sie einen neuen virtuellen Computer erstellen.

1. Erstellen Sie den virtuellen Computer.

        azure vm create --resource-group $rgName \
            --name $vmName \
            --location $location \
            --vm-size $vmSize \
            --subnet-id $subnetId \
            --nic-names $nicName \
            --os-type linux \
            --image-urn $publisher:$offer:$sku:$version \
            --storage-account-name $stdStorageAccountName \
            --storage-account-container-name vhds \
            --os-disk-vhd $osDiskName.vhd \
            --admin-username $username \
            --admin-password $password

2. Speichern Sie die Skriptdatei.

## <a name="step-4---run-the-script"></a>Schritt 4: Führen Sie das Skript

Nach Bedarf ändern und das Verständnis das obigen Skript anzeigen das Skript. 

1. Führen Sie das obige Skript Bash-Konsole.

        sh myscript.sh

2. Nach einigen Minuten sollte die folgende Ausgabe angezeigt werden.

        info:    Executing command group create
        info:    Getting resource group IaaSStory
        info:    Creating resource group IaaSStory
        info:    Created resource group IaaSStory
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx/resourceGroups/IaaSStory
        data:    Name:                IaaSStory
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
        info:    Executing command network vnet create
        info:    Looking up virtual network "TestVNet"
        info:    Creating virtual network "TestVNet"
        info:    Loading virtual network state
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : westus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK
        info:    Executing command network vnet subnet create
        info:    Looking up the subnet "FrontEnd"
        info:    Creating subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK
        info:    Executing command network public-ip create
        info:    Looking up the public ip "PIPWEB1"
        info:    Creating public ip address "PIPWEB1"
        info:    Looking up the public ip "PIPWEB1"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/publicIPAddresses/PIPWEB1
        data:    Name                            : PIPWEB1
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Static
        data:    Idle timeout                    : 4
        data:    IP Address                      : 40.78.63.253
        data:    Domain name label               : iaasstoryws1
        data:    FQDN                            : iaasstoryws1.westus.cloudapp.azure.com
        info:    network public-ip create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICWEB1"
        info:    Looking up the public ip "PIPWEB1"
        info:    Creating network interface "NICWEB1"
        info:    Looking up the network interface "NICWEB1"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/networkInterfaces/NICWEB1
        data:    Name                            : NICWEB1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/publicIPAddresses/PIPWEB1
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx/resourceGroups/IaaSStory2/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up the VM "WEB1"
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account iaasstorystorage
        info:    Looking up the NIC "NICWEB1"
        info:    Creating VM "WEB1"
        info:    vm create command OK
