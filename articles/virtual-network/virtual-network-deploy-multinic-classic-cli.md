<properties
   pageTitle="Bereitstellen von Multi-NIC VMs mit der Azure-CLI im klassischen Bereitstellungsmodell | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von Multi-NIC VMs mit der Azure-CLI im klassischen Bereitstellungsmodell"
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

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Bereitstellen von Multi NIC VMs (klassisch) mit der Azure-CLI

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Sie können virtuelle Maschinen (VMs) in Azure erstellen und jeder Ihrer virtuellen Computer mehrere Netzwerkkarten (NICs) zuordnen. Mehrere Netzwerkkarten ermöglichen Trennung der Datenverkehr über Netzwerkkarten. Beispielsweise könnte eine Netzwerkkarte mit dem Internet kommunizieren, während anderen nur mit internen Ressourcen nicht mit dem Internet verbunden kommuniziert. Trennen Sie den Netzwerkverkehr über mehrere Netzwerkkarten ist für viele virtuelle Netzwerkgeräte, wie Bereitstellung und WAN-optimierungslösungen erforderlich.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Derzeit sein nicht virtueller Computer mit einer NIC und VMs mit mehreren NICs im gleichen Clouddienst werden. Daher müssen Sie die Back-End-Server in eine andere Cloud-Dienst als und anderen Komponenten im Szenario implementieren. Die folgenden Schritte verwenden einen Cloud-Dienst mit dem Namen *IaaSStory* für die wichtigsten Ressourcen und *IaaSStory-Back-End-* Back-End-Server.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie die Back-End-Server bereitstellen können, müssen Sie main Cloud-Dienst mit den erforderlichen Ressourcen für dieses Szenario bereitstellen. Zumindest müssen Sie ein virtuelles Netzwerk mit einem Subnetz für das Backend erstellen. Besuchen Sie [erstellen ein virtuelles Netzwerk mit der Azure-CLI](virtual-networks-create-vnet-classic-cli.md) Informationen zum Bereitstellen eines virtuellen Netzwerks.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Bereitstellen der Back-End-VMs

Die Back-End-VMs hängt die Erstellung der unten aufgeführten Ressourcen.

- Das **Speicherkonto für Datenträger**. Für eine bessere Leistung verwendet die Datenträger auf den Datenbankservern solid State Drive (SSD)-Technologie erfordert ein Speicherkonto Premium. Vergewissern Sie sich Azure Speicherort bereitstellen, um Premium-Speicher unterstützen.
- **Netzwerkkarten**. Jede VM haben zwei Netzwerkkarten Datenbank zugreifen und Management.
- **Verfügbarkeit**. Alle Datenbankserver werden einzelne Verfügbarkeit festgelegt, um sicherzustellen, dass mindestens die VMs einrichten und Wartungsarbeiten ausführen hinzugefügt werden.

### <a name="step-1---start-your-script"></a>Schritt 1 - Skript starten

Verwendet volle Skript herunterladen [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Gehen Sie folgendermaßen vor, so ändern Sie das Skript in Ihrer Umgebung funktionieren.

1. Ändern Sie die Werte der folgenden Variablen anhand der vorhandenen Ressourcengruppe über [Komponenten](#Prerequisites)bereitgestellt.

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Ändern Sie die Werte der folgenden Variablen basierend auf den Werten für die Back-End-Bereitstellung verwenden möchten.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Schritt 2 - Erstellen der erforderlichen Ressourcen für Ihre VMs

1. Erstellen Sie einen neue Cloud-Dienst für alle Back-End-VMs. Beachten Sie die `$backendCSName` Variable für den Ressourcennamen Gruppe und `$location` Azure Region.

        azure service create --serviceName $backendCSName \
            --location $location

2. Erstellen Sie ein Speicherkonto Premium für Datenträger Betriebssystem und Daten von Ihrem virtuellen Computer verwendet werden.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Schritt 3 - VMs mit mehreren NICs erstellen

1. Starten Sie eine Schleife erstellen mehrere VMs auf der Grundlage der `numberOfVMs` Variablen.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Geben Sie für jeden VM den Namen und IP-Adressen der einzelnen zwei Netzwerkkarten.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Erstellen Sie den virtuellen Computer. Beachten Sie die Verwendung der `--nic-config` Parameter mit allen Netzwerkkarten mit Namen, die Subnetzmaske und IP-Adresse.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Erstellen Sie für jeden virtuellen Computer zwei Datenträger.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Schritt 4: Führen Sie das Skript

Führen Sie das Skript erstellen das Back-End Datenbank VMs mit mehreren NICs, heruntergeladen und Skript Ihren Bedürfnissen entsprechend geändert.

1. Speichern Sie das Skript und die **Bash** Terminal führen. Erste Ausgabe sehen, wie unten dargestellt.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Nach einigen Minuten die Ausführung beendet und Sie sehen die Ausgabe wie folgt.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
