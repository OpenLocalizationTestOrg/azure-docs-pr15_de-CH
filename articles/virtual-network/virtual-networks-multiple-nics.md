<properties
   pageTitle="Erstellen Sie einen virtuellen Computer mit mehreren NICs"
   description="Informationen Sie zum Erstellen und Konfigurieren virtueller Computer mit mehreren Netzwerkkarten"
   services="virtual-network, virtual-machines"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management,azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="create-a-vm-with-multiple-nics"></a>Erstellen Sie einen virtuellen Computer mit mehreren NICs

Sie können virtuelle Maschinen (VMs) in Azure erstellen und jeder Ihrer virtuellen Computer mehrere Netzwerkkarten (NICs) zuordnen. Multi-NIC ist eine Voraussetzung für viele virtuelle Netzwerkgeräte, wie Bereitstellung und WAN-Optimierung Solutions. Multi-NIC bietet mehr Datenverkehr Funktionen für Netzwerkmanagement, einschließlich Isolierung von Datenverkehr zwischen einer Netzwerkkarte beenden und Ende NICs oder Trennung von Datenverkehr Ebene von Verwaltungsdatenverkehr Ebene zurück.

![Multi-NIC für VM](./media/virtual-networks-multiple-nics/IC757773.png)

Die obige Abbildung zeigt einem virtuellen Computer mit drei Netzwerkkarten jeweils in ein anderes Subnetz verbunden.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

- Internetzugriff VIP (klassische Bereitstellungen) wird nur "Standard"-Netzwerkkarte Es gibt nur eine IP-Adresse des Standard-NIC VIP
- Zu diesem Zeitpunkt werden Instanz auf öffentliche IP (LPIP) Adressen (klassische Bereitstellungen) für mehrere VMs NIC nicht unterstützt.
- Die Reihenfolge der Netzwerkkarten von innerhalb des virtuellen Computers werden zufällige und kann auch über Azure-Infrastruktur Updates ändern. Adressen, IP-Adressen und die entsprechenden Ethernet MAC werden unverändert. Angenommen Sie **Eth1** 10.1.0.100 und MAC-Adresse 00-0D-3A-B0-39-0D hat; ein Azure-Infrastruktur aktualisieren und Neustart **Eth2**jedoch die IP-Adresse geändert werden könnte und MAC Verbindung bleibt. Beim Neustart Kunden initiiert wird, wird die NIC-Reihenfolge unverändert.
- Adresse für jede Netzwerkkarte auf jedem virtuellen Computer in einem Subnetz befinden, mehrere Netzwerkkarten auf einer einzelnen VM jeder zugewiesen werden Adressen im selben Subnetz.
- Die VM-Größe bestimmt die Anzahl der Netzwerkkarten für einen virtuellen Computer erstellen können. Referenz [Windows Server](../virtual-machines/virtual-machines-windows-sizes.md) und [Linux](../virtual-machines/virtual-machines-linux-sizes.md) VM Größen Artikeln festzustellen, wie viele Netzwerkkarten unterstützt jede VM-Größe. 

## <a name="network-security-groups-nsgs"></a>Netzwerk-Sicherheitsgruppen (NSGs)
In einer Bereitstellung Ressourcenmanager NICs auf einem virtuellen Computer möglicherweise mit einem Netzwerk Sicherheit Gruppe (NSG), einschließlich NICs auf einer VM, die mehreren aktiviert Netzwerkkarten. Wenn eine Netzwerkkarte eine NSG zugeordnet die Subnetzmaske ist eine Adresse in einem Subnetz zugeordnet ist, gelten die Regeln in dem Subnetz NSG auch, NIC Neben Zuordnen von Subnetzen zu NSGs, können Sie auch eine Netzwerkkarte mit einer NSG zuordnen.

Wenn ein Subnetz zugeordnet ist eine NSG und eine Netzwerkkarte innerhalb eines Subnetzes einzeln eine NSG zugeordnet ist, die zugeordneten NSG regelbasiert **Fluss Reihenfolge** entsprechend der Richtung des Datenverkehrs in oder aus der NIC übergeben wird:

- **Eingehender Datenverkehr** , dessen Ziel die betreffenden NIC ist, verläuft zuerst im Subnetz im Subnetz NSG Regeln vor der Übergabe an die Netzwerkkarte und die NIC NSG Regeln auslösen auslösen.
- **Ausgehender Datenverkehr** , dessen Datenquelle betreffenden NIC ist, fließt zuerst von NIC, die NIC NSG Regeln vor passiert das Subnetz und die Subnetzmaske NSG Regeln auslösen auslösen.

Erfahren Sie mehr über [Netzwerk-Sicherheitsgruppen](virtual-networks-nsg.md) , und diese Anwendung basierend auf Subnetze VMs und Netzwerkkarten...

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Mehrere VM NIC in einer klassischen Bereitstellung konfigurieren

Hinweise helfen Ihnen mehrere NIC VM mit 3 NICs: Standard NIC und zwei zusätzliche Netzwerkkarten. Die Konfigurationsschritte erstellt einen virtuellen Computer, die nach dem Service Fragment einer Konfigurationsdatei unten konfiguriert werden:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Sie benötigen die folgenden Komponenten vor dem Ausführen von PowerShell-Befehlen im Beispiel.

- Ein Azure-Abonnement.
- Konfigurierte virtuelle Netzwerk. Weitere Informationen zu VNets finden Sie unter [Übersicht über Virtual Network](virtual-networks-overview.md) .
- Die neueste Version von Azure PowerShell heruntergeladen und installiert. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Gehen Sie folgendermaßen vor um einen virtuellen Computer mit mehreren NICs erstellen:

1. VM-Image von Azure VM-Bildergalerie auswählen Beachten Sie, dass Bilder häufig ändern und nach Region verfügbar sind. Im Beispiel unten angegebene Bild möglicherweise ändern oder möglicherweise nicht in Ihrer Region, werden müssen Sie das Bild an.

        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. Erstellen Sie VM-Konfiguration.

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. Standard-Administratorkonto zu erstellen.

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. Die VM-Konfiguration zusätzliche Netzwerkkarten hinzufügen.

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. Geben Sie das Subnetz und die IP-Adresse für Standard-NIC

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. Erstellen Sie den virtuellen Computer im virtuellen Netzwerk

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] Das hier angegebene VNet muss bereits vorhanden sein (gemäß der Komponenten). Im folgenden Beispiel gibt ein virtuelles Netzwerk mit dem Namen **MultiNIC-VNet**.

## <a name="limitations"></a>Grenzen

Die folgenden Nachteile gelten bei Multi-NIC-Funktion verwenden:

- Multi-NIC VMs müssen in Azure virtuelle Netzwerke (VNets) erstellt werden. Nicht VNet VMs kann mit mehreren Netzwerkkarten konfiguriert werden.
- Alle virtuellen Computer in einem Verfügbarkeit benötigen mehrere Netzwerkkarten oder einzelne Netzwerkkarte Aus mehreren NIC VMs und einzelne NIC VMs innerhalb einer Verfügbarkeit nicht. Dasselbe gilt für virtuelle Computer in einem Clouddienst.
- Ein virtueller Computer mit einzelner Netzwerkkarte kann nicht konfiguriert werden, mit mehreren NICs (und umgekehrt) nach der Bereitstellung ohne löschen und neu erstellen.


## <a name="secondary-nics-access-to-other-subnets"></a>Sekundäre NICs auf anderen Subnetzen

Standardmäßig werden sekundäre Netzwerkkarten nicht mit einem Standardgateway konfiguriert, Datenverkehr auf den sekundären NICs in demselben Subnetz beschränkt werden. Wenn Benutzer sekundäre Netzwerkkarten mit außerhalb der eigenen Subnetz aktivieren möchten, müssen sie einen Eintrag in der Routingtabelle Gateway konfigurieren, wie unten beschrieben.

>[AZURE.NOTE] VMs erstellt vor Juli 2015 ein Standardgateway für alle Netzwerkadapter konfiguriert haben. Das Standardgateway für sekundären Netzwerkkarten wird die VMs neu gestartet werden nicht entfernt. Betriebssysteme, die schwacher Host-routing-Modell wie Linux verwenden, kann Internetkonnektivität Ingress- und Egress-Datenverkehr verwenden andere Netzwerkkarten trennen.

### <a name="configure-windows-vms"></a>Konfigurieren von Windows-VMs

Angenommen Sie, ein Windows-VM mit zwei NICs wie folgt vor:

- Primäre NIC IP-Adresse: 192.168.1.4
- Sekundäre NIC IP-Adresse: 192.168.2.5

IPv4 Route-Tabelle für diese VM würde wie folgt aussehen:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Beachten Sie, dass die Standardroute (0.0.0.0) nur an die primäre Netzwerkkarte Nicht werden auf Ressourcen außerhalb des Subnetzes für die sekundären NIC wie folgt:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Gehen Sie folgendermaßen vor um eine Standardroute auf der sekundären NIC hinzuzufügen:

1. Führen Sie folgenden Befehl zum Identifizieren der Index der zweiten Netzwerkkarte von einer Befehlszeile aus:

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. Beachten Sie den zweiten Eintrag in der Tabelle mit einem Index von 27 (in diesem Beispiel).
3. Führen Sie über die Befehlszeile den Befehl **Route add** wie unten dargestellt aus. In diesem Beispiel sind Sie für die sekundären NIC 192.168.2.1 als Standardgateway angeben:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. Zum Testen der Konnektivität zurück zur Befehlszeile, und versuchen Sie zu einem anderen Subnetz als sekundärer NIC als Siehe Int eh Beispiel ping:

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. Sie können auch die Routingtabelle neu hinzugefügte Route überprüfen Überprüfen, wie unten dargestellt:

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Konfigurieren von Linux-VMs

Für Linux VMs standardmäßig schwachen Host routing verwendet empfiehlt die sekundären Netzwerkkarten Verkehrswerte im selben Subnetz beschränken. Jedoch verlangen bestimmte Szenarien Connectivity außerhalb des Subnetzes, sollten Benutzer können Policy-basierten routing, um sicherzustellen, dass der Datenverkehr Ingress- und Egress dieselbe NIC verwendet

## <a name="next-steps"></a>Nächste Schritte

- Bereitstellen Sie [MultiNIC VMs in einem Szenario 2-Tier-Anwendung in einer Bereitstellung Ressourcen-Manager](virtual-network-deploy-multinic-arm-template.md).
- Bereitstellen Sie [MultiNIC VMs in einem Szenario 2-Tier-Anwendung in einer klassischen Umgebung](virtual-network-deploy-multinic-classic-ps.md).
