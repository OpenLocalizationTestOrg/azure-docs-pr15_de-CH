<properties
   pageTitle="Mehrere IP-Adressen für virtuelle Maschinen - PowerShell | Microsoft Azure"
   description="Erfahren Sie, wie eine virtuelle Maschine mit Azure PowerShell mehrere IP-Adressen zuweisen."
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
   ms.date="10/05/2016"
   ms.author="jdial;annahar" />


# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Virtuelle Computer mehrere IP-Adressen zuweisen

Ein Azure Virtual Machine (VM) können eine oder mehrere Netzwerkschnittstellen (NIC) zugeordnet. NICs können eine oder mehrere öffentliche oder private IP-Adressen zugewiesen. Wenn Sie nicht mit IP-Adressen in Azure vertraut sind, lesen Sie den Artikel [IP-Adressen in Azure](virtual-network-ip-addresses-overview-arm.md) mehr über sie erfahren können. Dieser Artikel erläutert das Azure PowerShell verwenden, um eine VM in Azure-Ressourcen-Manager-Bereitstellungsmodell mehrere IP-Adressen zuweisen.

Ein virtueller Computer mehrere IP-Adressen zuweisen, können die folgenden Funktionen:

- Mehrere Websites oder Dienste mit unterschiedlichen IP-Adressen und SSL-Zertifikate auf einem einzigen Server hosten.
- Als eine virtuelle Appliance, z. B. eine Firewall oder Lastenausgleich.
- Die Möglichkeit, eine privaten IP-Adressen für alle Netzwerkkarten ein Lastenausgleich Azure Backend-Pool hinzufügen. In der Vergangenheit konnte nur die primäre IP-Adresse für die primäre NIC an einen Back-End-Pool hinzugefügt werden.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Um die Vorschau zu registrieren, senden Sie eine e-Mail an [Mehrere IP -Adressen](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) mit Ihrem Abonnement-ID und verwenden.

## <a name="scenario"></a>Szenario

In diesem Artikel werden drei IP-Konfigurationen für eine Netzwerkschnittstelle zuordnen.
Die folgenden Beispielkonfigurationen werden erstellt und eine Netzwerkkarte, die drei privaten IP-Adressen und eine öffentliche IP-Adresse zugewiesen zugewiesen:

- IPConfig-1: Eine dynamische private IP-Adresse (Standard) und eine öffentliche IP-Adresse aus der öffentlichen IP-Adressenressource namens PIP1.
- IPConfig-2: Statische private IP-Adresse und keine öffentliche IP-Adresse.
- IPConfig-3: Dynamische private IP-Adresse und keine öffentliche IP-Adresse.

    ![ALT Bildtext](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

Dieses Szenario setzt voraus, haben Sie eine Ressourcengruppe *RG1* Frist gibt es ein VNet namens *VNet1* und eine Subnetzmaske namens *Subnet1*aufgerufen. Darüber hinaus wird davon haben eine VM, der sogenannten *VM1*eine Netzwerkschnittstelle bezeichnet *VM1 NIC1* zugeordnet und eine öffentliche IP-Adresse als *PIP1*bezeichnet.

[Dieser Artikel](./virtual-machines/virtual-machines-windows-ps-create.md ) führt durch Erstellen von Ressourcen erwähnt, falls sie nicht erstellt haben.

## <a name = "create"></a>Erstellen Sie einen virtuellen Computer mit mehreren IP-Adressen

1. Öffnen Sie eine PowerShell-Befehlszeile, und führen Sie die verbleibenden Schritte in diesem Abschnitt innerhalb einer PowerShell-Sitzung. Wenn Sie bereits über PowerShell installiert und konfiguriert haben, die Schritte [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) -Artikel.

2. Ändern Sie "Werte" von der folgenden $Variables virtuelle Netzwerk wird in den Namen der [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups), der VNet die Ressourcengruppe, Subnetz die NIC eine Verbindung herstellen möchten und den Namen der Netzwerkkarte Azure [Speicherort](https://azure.microsoft.com/regions) Führen Sie Schritte aus einer VM angefügte NICs mehrere IP-Adressen hinzufügen, wie erforderlich.

        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"

    Wenn Sie den Namen eines vorhandenen Azure Speicherort oder Ressourcengruppe nicht kennen, geben Sie die folgenden Befehle:

        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName

    <a name="subnet"></a>Ein Subnetz ein vorhandenes Azure Virtual Network (VNet) muss die Netzwerkkarte verbunden. Die drei Komponenten: NIC, Subnetz und VNet, müssen alle in derselben Region und [Abonnement](../azure-glossary-cloud-terminology.md#subscription)vorhanden.  Wenn Sie nicht mit VNets vertraut sind, lesen Sie den [virtuellen Netzwerk](virtual-networks-overview.md) Artikel zu erfahren sie Artikel [Erstellen Sie eine VNet](virtual-networks-create-vnet-arm-ps.md) Informationen zum Erstellen.

    Wenn Sie den Namen des vorhandenen VNet nicht kennen, geben Sie den folgenden Befehl, und Ersetzen Sie *VNet1* in vorigen Variablen mit dem Namen einer VNet:

        Get-AzureRmVirtualNetwork | Format-Table Name

    Die zurückgegebene Liste leer ist, müssen Sie ein VNet erstellen. Um zu erfahren, lesen Sie [ein virtuelles Netzwerk erstellen](virtual-networks-create-vnet-arm-ps.md) .

    Geben Sie die folgenden Befehle, die Namen der vorhandenen Subnetze innerhalb der VNet und *Subnet1* über durch den Namen eines Subnetzes:

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

4. Geben Sie den folgenden Befehl im Subnetz abgerufen und einer Variablen zuweisen.

        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. <a name="ipconfigs"></a>Definieren Sie die IP-Konfigurationen der Netzwerkkarte zuweisen möchten Jede Konfiguration kann eine statische oder dynamische private IP-Adresse und eine statische oder dynamische Adresse öffentlichen IP-Adressenressource zugeordnet.

    Fügen Sie hinzu oder entfernen Sie eine beliebige Anzahl von Konfigurationen, die folgen, je nachdem, wie viele IP-Adressen der Netzwerkkarte und dem zuordnen möchten, die Sie konfigurieren möchten.

    **IPConfig-1**

    Ändern Sie den Wert *PIP1* auf den Namen einer vorhandenen öffentlichen IP-Adressressource, die an der Erstellung die Netzwerkkarte in vorhanden und ist nicht mit einer anderen Netzwerkkarte verknüpft Änderung *RG1* auf den Namen der Ressourcengruppe die öffentliche IP-Adressenressource vorhanden ist. *IPConfig-1* , den Namen der ersten IP-Konfiguration zu ändern. Geben Sie die folgenden Befehle:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName

        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary

    Beachten Sie die *-primäre* wechseln. Wenn Sie mehrere IP-Konfigurationen zu einer Netzwerkkarte zuweisen, muss eine Konfiguration als *primäre*zugewiesen. Wenn Sie den Namen einer vorhandenen öffentlichen IP-Adressressource nicht kennen, geben Sie folgenden Befehl: Get-AzureRMPublicIPAddress | Format-Table Name IPKonfigurationsdateiAddress Lage, IP-Adresse

    Wenn **IPKonfigurationsdateiAddress** Spalte keinen Wert in die Ausgabe, die öffentliche IP-Adressenressource ist nicht mit einer vorhandenen Netzwerkkarte verknüpft und kann verwendet werden. Wenn die Liste leer ist oder keine verfügbare öffentliche IP-Adresse Ressourcen, können Sie eine **Neue AzureRmPublicIPAddress** Befehl erstellen.

    >[AZURE.NOTE] Öffentliche IP-Adressen haben eine geringe Gebühr. Erfahren Sie mehr über IP-Adresse Preise, lesen Sie die [IP-Adresse Preise](https://azure.microsoft.com/pricing/details/ip-addresses) .

    **IPConfig-2**

    Ändern Sie *IPConfig-2* auf den Namen der zweiten IP-Konfiguration und *10.0.0.5* nicht verwendete gültigen IP-Adressen für das Subnetz, das zuweisen die NIC ändern möchten. Geben Sie die folgenden Befehle:

        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5

        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress

    Geben Sie den folgenden Befehl aus, wenn Sie nicht, dass die IP wissen Adressbereich des Subnetzes zugewiesen:

        $VNet.Subnets | Format-Table Name, AddressPrefix

    **IPConfig-3**

    Die Namen der IP-Konfiguration und geben die folgenden Befehle ändern Sie *IPConfig 3* :

        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

    >[AZURE.NOTE] Sie können bis zu 250 private IP-Adresse einer Netzwerkkarte zuweisen. Gibt es eine Beschränkung für die Anzahl öffentlicher IP-Adressen, die innerhalb eines Abonnements verwendet werden können. Weitere Artikel der [Azure beschränkt](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) .

6. Erstellen Sie die NIC mit den im vorherigen Schritt definierten IP-Konfigurationen.

        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

7. Legen Sie die NIC Erstellung eine VM mithilfe der Schritte im Artikel [Erstellen einer VM](../virtual-machines/virtual-machines-windows-ps-create.md) . Wenn der Artikel einen virtuellen Computer mit Windows Server erstellt, sind die Schritte für eine Linux VM als ein anderes Betriebssystem auswählen. Schritte 1 bis 3 des Artikels. Überspringen Sie die Schritte 4 und 5 und führen Schritt 6 Erstellen einer VM-Artikel.

    >[AZURE.WARNING] Schritt 6 Erstellen einer VM Artikel schlägt fehl, wenn die Variable mit dem Namen $nic etwas anderes in Schritt 6 dieses Artikels geändert oder die vorherigen Schritte in diesem Artikel noch nicht abgeschlossen.

8. Anzeigen der private IP Adressen, Azure DHCP die NIC zugewiesen und die öffentliche IP-Adressenressource durch Eingabe des folgenden Befehls die NIC zugewiesen:

        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. <a name="os"></a>Der TCP/IP-Konfiguration des Betriebssystems fügen Sie alle die privaten IP-Adressen (einschließlich der primären hinzu) manuell. 

**Windows**

1. Geben Sie eine Befehlszeile *Ipconfig/all*.  Sie sehen nur die *primäre* private IP-Adresse (über DHCP).
2. Als Nächstes geben Sie *ncpa.cpl* in das Eingabeaufforderungsfenster. Dadurch wird ein neues Fenster geöffnet.
3. Öffnen Sie die Eigenschaften für **LAN-Verbindung**.
4. Doppelklick auf Internet Protocol Version 4 (IPv4)
5. Wählen Sie **folgende IP-Adresse verwenden** , und geben Sie die folgenden Werte:
    - **IP-Adresse**: die *primäre* private IP-Adresse
    - **Subnetzmaske**: auf Grundlage von Subnetz. Beispielsweise das Subnetz ist eine /24 Subnetz dann die Subnetzmaske ist 255.255.255.0.
    - **Standard-Gateway**: die erste IP-Adresse im Subnetz. Subnetz ist 10.0.0.0/24, die Gateway-IP-Adresse 10.0.0.1.
    - Klicken Sie auf **folgenden DNS-Serveradressen verwenden** , und geben Sie die folgenden Werte:
        - **Bevorzugter DNS-Server:** Geben Sie 168.63.129.16, wenn Sie eigene DNS-Server nicht verwenden.  Sind, geben Sie die IP-Adresse des DNS-Servers.
    - Klicken Sie auf die Schaltfläche **Erweitert** und fügen Sie zusätzliche IP-Adressen hinzu. Fügen Sie die sekundären privaten IP-Adressen in Schritt 8 die NIC mit demselben Subnetz für die primäre IP-Adresse angegeben.
    - Klicken Sie auf **OK** , um die TCP/IP-Einstellungen schließen **OK** wieder zu Einstellungen des Adapters. Dies wird dann die RDP-Verbindung wiederherzustellen.

6. Geben Sie eine Befehlszeile *Ipconfig/all*. Alle IP-Adressen, die Sie hinzugefügt werden und DHCP deaktiviert.


**Linux (Ubuntu)**

1. Öffnen Sie ein terminal-Fenster.
2. Stellen Sie sicher, dass der Root-Benutzer sind. Wenn Sie nicht sind, können Sie dazu mithilfe des folgenden Befehls:

            sudo -i
3. Aktualisieren der Konfigurationsdatei der Netzwerkschnittstelle (sofern eth0").
    - Beibehalten der vorhandenen Zeile Dhcp. Dadurch wird die primäre IP-Adresse konfiguriert, wie früher.
    - Hinzufügen einer Konfiguration zusätzliche statische IP-Adresse mit den folgenden Befehlen:

                cd /etc/network/interfaces.d/
                ls

        Cfg-Datei sollte angezeigt werden.
4. Öffnen Sie die Datei: vi *Dateiname*.

    Am Ende der Datei die folgenden Zeilen sollten angezeigt werden:

            auto eth0
            iface eth0 inet dhcp
5. Fügen Sie folgende Zeilen nach den Zeilen, die in dieser Datei vorhanden:

            iface eth0 inet static
            address <your private IP address here>
6. Speichern Sie die Datei mit dem folgenden Befehl:

            :wq
7.  Zurücksetzen der Schnittstelle mit dem folgenden Befehl:

            sudo ifdown eth0 && sudo ifup eth0

    >[AZURE.IMPORTANT] Führen Sie Ifdown und Ifup in derselben Zeile eine Remoteverbindung verwenden.
8. Überprüfen Sie, ob die IP-Adresse der Schnittstelle mit dem folgenden Befehl hinzugefügt wird:

            ip addr list eth0

    Die IP-Adresse sollte als Teil der Liste der hinzugefügten angezeigt werden.

**Linux (Redhat, CentOS und andere)**

1. Öffnen Sie ein terminal-Fenster.
2. Stellen Sie sicher, dass der Root-Benutzer sind. Wenn Sie nicht sind, können Sie dazu mithilfe des folgenden Befehls:

            sudo -i
3. Geben Sie Ihr Kennwort und wie der Anleitung. Sobald der Root-Benutzer sind, navigieren Sie zum Ordner Skripts Netzwerk mit dem folgenden Befehl:

            cd /etc/sysconfig/network-scripts
4. Listen Sie verwandte ifcfg-mit dem folgenden Befehl:

            ls ifcfg-*

    *Ifcfg eth0* sollte als eine der Dateien angezeigt werden.
5. Kopieren Sie die Datei *Ifcfg eth0* und nennen Sie sie *Ifcfg-Eth0:0* mit dem folgenden Befehl:

            cp ifcfg-eth0 ifcfg-eth0:0
6. Bearbeiten der *Ifcfg-Eth0:0* -Datei mit dem folgenden Befehl:

            vi ifcfg-eth1
7. Ändern Sie das Gerät auf den entsprechenden Namen in der Datei. *Eth0:0* in diesem Fall mit dem folgenden Befehl:

            DEVICE=eth0:0
8. Ändern der *IPADDR = YourPrivateIPAddress* Zeile der IP-Adresse.
9. Speichern Sie die Datei mit dem folgenden Befehl:

            :wq
10. Starten Sie Netzwerkdienste und sicherzustellen Sie, dass die Änderung erfolgreich durch Ausführen der folgenden Befehle:

            /etc/init.d/network restart
            Ipconfig

    Die IP-Adresse hinzugefügt, *Eth0:0*zurückgegebenen Liste sollte angezeigt werden.

## <a name="add"></a>IP-Adressen auf einem vorhandenen virtuellen Computer hinzufügen

Gehen Sie folgendermaßen vor um weitere IP-Adressen zu einer vorhandenen Netzwerkkarte hinzufügen:

1. Öffnen Sie eine PowerShell-Befehlszeile, und führen Sie die verbleibenden Schritte in diesem Abschnitt innerhalb einer PowerShell-Sitzung. Wenn Sie bereits über PowerShell installiert und konfiguriert haben, die Schritte [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) -Artikel.

2. Ändern Sie "Werte" von folgenden $Variables auf den Namen der NIC, IP-Adressen hinzufügen möchten, Ressourcengruppe und Speicherort in die Netzwerkkarte vorhanden ist:

        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"

    Wenn Sie den Namen der Netzwerkkarte, die Sie ändern möchten kennen, geben die folgenden Befehle, ändern Sie die Werte der vorherigen Variablen:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

3. Erstellen einer Variable und legen auf vorhandene Netzwerkkarte durch Eingabe des folgenden Befehls:

        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName

4. Rufen Sie die Teilnetz-ID die Netzwerkkarte an von [Schritt 3](#subnet) erstellen einen virtuellen Computer mit mehreren IP-Adressen in diesem Artikel.

5. Erstellen Sie IP-Konfigurationen zum Netzwerk hinzufügen, wie in [Schritt 5](#ipconfigs) erstellen möchten ein virtueller Computer mit mehreren IP-Adressen dieser Artikel.

6. *$IPConfigName4* auf den Namen der im vorherigen Schritt erstellten IP-Konfiguration zu ändern. Um die Konfiguration hinzuzufügen, geben Sie folgenden Befehl ein:

        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1

7. Legen Sie die NIC mit der IP-Konfiguration geben Sie den folgenden Befehl ein:

        Set-AzureRmNetworkInterface -NetworkInterface $nic

8. Fügen Sie die IP-Adressen hinzugefügte NIC VM-Betriebssystem wie in [Schritt 9](#os) erstellen einen virtuellen Computer mit mehreren IP-Adressen dieses Artikels hinzu.
