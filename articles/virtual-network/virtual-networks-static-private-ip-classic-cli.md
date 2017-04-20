<properties 
   pageTitle="Wie privaten Adresse im klassischen tagesleistung CLI | Microsoft Azure"
   description="Grundlegendes zu statische private IP-Adressen (DIPs) und wie sie im klassischen Modus über die CLI verwaltet"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Wie eine statische private IP-Adresse in Azure-CLI (klassisch)

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell. Sie können auch [statische private IP-Adresse in der Ressourcen-Manager-Bereitstellungsmodell verwalten](virtual-networks-static-private-ip-arm-cli.md).

Azure-CLI Beispielbefehle unten erwarten eine einfache Umgebung bereits erstellt. Die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, zuerst Erstellen der Umgebung beschrieben in [ein Vnet erstellen](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Statische private IP-Adresse angeben, wenn Sie einen virtuellen Computer erstellen
Erstellen Sie einen neuen virtuellen Computer mit dem Namen *DNS01* in eine neue Cloud-Dienst mit dem Namen *TestService* basierend auf dem Szenario folgendermaßen Sie vor:

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
1. Führen Sie den Befehl **Azure Service erstellen** Cloud-Dienst erstellen.

        azure service create TestService --location uscentral

    Erwartete Ausgabe:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. Führen Sie den Befehl **Azure Vm erstellt** die VM erstellt. Beachten Sie den Wert für eine statische private IP-Adresse. Die Liste nach die Ausgabe der Parameter erläutert.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Erwartete Ausgabe:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **(oder -Speicherort)**. Azure-Region, in dem die VM erstellt werden. In diesem Szenario *Centralus*.
    - **-n (oder Vm-Namen)**. Name des virtuellen Computers erstellt werden.
    - **-w (oder – virtuelle Netzwerknamen)**. Name des VNet, der virtuellen Computer erstellt werden. 
    - **-S (oder -statische Ip)**. Statische private IP-Adresse für den virtuellen Computer.
    - **TestService**. Name der Cloud-Dienst, der virtuellen Computer erstellt werden.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-X64-v14.2**. Bild die VM erstellt.
    - **Administrator**. Lokaler Administrator für den Windows-VM.
    - **AdminP@ssw0rd**. Lokale Administratorkennwort für die Windows-VM.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Statische private IP-Adressinformationen für einen virtuellen Computer abrufen
Zum Anzeigen der statischen privaten IP-Adressinformationen für die VM mit dem Beispielskript erstellt führen Sie folgenden Azure CLI-Befehl aus, und beobachten Sie den Wert für *Netzwerk-StaticIP*:

    azure vm static-ip show DNS01

Erwartete Ausgabe:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Statische private IP-Adresse eines virtuellen Computers entfernen
Entfernen Sie die statische private IP-Adresse hinzugefügt VM im obigen Skript folgenden Azure CLI-Befehl ausführen:
    
    azure vm static-ip remove DNS01

Erwartete Ausgabe:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Private Adresse zu einem vorhandenen virtuellen Computer hinzufügen
Hinzufügen eine statischen privaten IP-Adresse für die VM erstellt das Skript über zwerg folgenden Befehl:

    azure vm static-ip set DNS01 192.168.1.101

Erwartete Ausgabe:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Nächste Schritte

- [Reservierte öffentliche IP-](virtual-networks-reserved-public-ip.md) Adressen erfahren.
- Erfahren Sie mehr über [öffentliche IP (ILPIP) auf Instanzebene](virtual-networks-instance-level-public-ip.md) Adressen.
- Wenden Sie sich an die [reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).
