<properties 
   pageTitle="Accelerated für einen virtuellen Computer - PowerShell | Microsoft Azure"
   description="Informationen Sie zum Netzwerk beschleunigt für eine Azure Virtual Machine mit PowerShell konfigurieren."
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
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Schnellere Netzwerke für eine virtuelle Maschine

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-accelerated-networking-portal.md)
- [PowerShell](virtual-network-accelerated-networking-powershell.md)

Schnellere Netzwerke kann einzelne Stamm e/a-Virtualisierung (SR-IOV) zu einer virtuellen Maschine (VM) die Netzwerke Leistung erheblich verbessert. Dieser leistungsfähige Pfad umgeht Server reduzieren Latenz, Jitter und CPU-Auslastung mit der anspruchsvollsten Arbeitslasten Netzwerk auf unterstützten VM Datapath. Erläutert, wie mit Azure PowerShell schnellere Netzwerke Bereitstellungsmodell Azure-Ressourcen-Manager konfigurieren. Beschleunigte Vernetzung mit Azure-Portal können Sie auch einen virtuellen Computer erstellen. Um zu erfahren, klicken Sie auf das Azure-Portal am Anfang dieses Artikels.

Die folgende Abbildung zeigt die Kommunikation zwischen zwei virtuellen Maschinen (VM) mit und ohne Netzwerk beschleunigt:

![Vergleich](./media/virtual-network-accelerated-networking-powershell/image1.png)

Ohne Netzwerk beschleunigt muss alle Netzwerkverkehrs und die VM Host und dem virtuellen Switch durchlaufen. Virtuelle Switch bietet alle durchsetzen, wie Netzwerk-Sicherheitsgruppen, Zugriffssteuerungslisten, Isolierung und andere virtualisiert Netzwerkdienste Netzwerkverkehr. Weitere Artikel der [Netzwerkvirtualisierung Hyper-V und virtuellen Switch](https://technet.microsoft.com/library/jj945275.aspx) .

Schnellere Netzwerke Netzwerkverkehr erreicht die Netzwerkkarte (NIC) mit der VM weitergeleitet. Alle virtuelle Switch ohne schnellere Netzwerke gilt Netzwerkrichtlinien sind ausgelagert und Hardware. Richtlinien Hardware kann NIC Netzwerk Verkehr direkt an die VM Host und dem virtuellen Switch Beibehaltung der Richtlinie im Host verwendete umgehen.

Die Vorteile von Accelerated gelten nur für den virtuellen Computer aktiviert ist. Optimale Ergebnisse bietet diese Funktion mindestens zwei VMs mit demselben VNet verbunden.  Bei der Kommunikation über VNets oder lokale Verbindung hat diese Funktion einen minimalen insgesamt Latenz.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Vorteile

- **Geringere Latenz / höhere Pakete pro Sekunde (Pps):** Die Datapath virtuellen Switch entfernt Zeit Pakete den Host für die Verarbeitung der Gruppenrichtlinie entfernt und erhöht die Anzahl der Pakete, die innerhalb des virtuellen Computers verarbeitet werden können.
- **Reduziert Zittern:** Virtueller Switch Verarbeitung abhängt und die Auslastung der CPU, die die Verarbeitung der Richtlinie, die angewendet werden muss. Entlastung der Durchsetzung der Hardware entfernt diese Variabilität durch die Bereitstellung von Paketen direkt an die VM Host zu VM Kommunikation und alle Software-Interrupts Kontextwechsel entfernen.
- **Verringert CPU-Auslastung:** Umgehen des virtuellen Switches im Host führt zu weniger CPU-Auslastung für die Verarbeitung von Netzwerkverkehr.

## <a name="limitations"></a>Grenzen

Die folgenden Nachteile vorhanden sein, wenn Sie diese Funktion verwenden:
 
- **Netzwerk-Schnittstelle erstellen:** Schnellere Netzwerke kann nur für eine neue Netzwerkschnittstelle aktiviert.  Es kann nicht auf eine vorhandene Netzwerkschnittstelle aktiviert.
- **VM erstellen:** Eine Netzwerkschnittstelle aktiviert schnellere Netzwerke kann nur einer VM angefügt werden, wenn die VM erstellt wird. Die Netzwerkschnittstelle kann eine vorhandene VM angefügt werden.
- **Regionen:** Im Westen zentralen USA und West Europa Azure nur angeboten. Der Satz von Bereichen wird in Zukunft erweitert.
- **Unterstützte Betriebssystem:** Microsoft Windows Server 2012 R2 und WindowsServer 2016 Technologievorschau 5. Unterstützung für Linux und Windows Server 2012 wird in Kürze.
- **Virtueller Speicher:** Standard_D15_v2 und Standard_DS15_v2 sind die einzigen VM instanzgrößen unterstützt. Weitere Informationen finden Sie im Artikel [Windows VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md) . Der Satz von unterstützten Größen von VM-Instanz wird in Zukunft erweitert.

Ändert diese Einschränkungen werden durch die Seite [Azure Virtual Networking Updates](https://azure.microsoft.com/updates/accelerated-networking-in-preview) bekannt.

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Erstellen eines Windows-VM mit schnellere Netzwerke

1. Öffnen Sie eine PowerShell-Befehlszeile, und führen Sie die verbleibenden Schritte in diesem Abschnitt innerhalb einer PowerShell-Sitzung. Wenn Sie bereits über PowerShell installiert und konfiguriert haben, die Schritte [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) -Artikel.
2. Um die Vorschau zu registrieren, senden Sie eine e-Mail an [Networking Abonnements beschleunigt](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) die Abonnement-ID und verwenden. Führen Sie die verbleibenden Schritte erst nachdem Sie per E-mail darüber informiert, dass Sie in der Vorschau angenommen haben.
3. Registrieren Sie die Funktion mit Ihrem Abonnement durch Eingabe der folgenden Befehle:

        Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. Ersetzen Sie *Westcentralus* durch einen anderen Standort dies gemäß Abschnitt [Einschränkungen](#limitations) (falls gewünscht) unterstützt. Geben Sie den folgenden Befehl zum Festlegen einer Variablen für den Standort:

        $locName = "westcentralus"

5. Einen Namen für die Ressourcengruppe, die neue Netzwerkschnittstelle enthält und geben Sie die folgenden Befehle erstellen ersetzen Sie *RG1* :

        $rgName = "RG1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

6. Ändern Sie *VM1 NIC1* soll die Netzwerkschnittstelle und geben Sie dann den folgenden Befehl:

        $NICName = "VM1-NIC1"

7. Netzwerkschnittstelle werden Subnetz innerhalb einer vorhandenen Azure Virtual Network (VNet) in derselben Position und [Abonnement](../azure-glossary-cloud-terminology.md#subscription) als Netzwerkschnittstelle angeschlossen. Erfahren Sie mehr über VNets den [virtuellen Netzwerk](virtual-networks-overview.md) Artikel lesen, wenn Sie nicht vertraut sind. Um ein VNet zu erstellen, die Schritte im Artikel [Erstellen einer VNet](virtual-networks-create-vnet-arm-ps.md) . Ändern Sie "Werte" von der folgenden $Variables auf den Namen des VNet und Subnetzmaske die Netzwerkschnittstelle verbinden möchten.

        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"

    Wenn Sie den Namen einer vorhandenen VNet an der kennen, die Sie in Schritt 3 ausgewählt haben, geben Sie die folgenden Befehle:
        
        $VirtualNetworks = Get-AzureRmVirtualNetwork
        $VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
        
    Die zurückgegebene Liste leer ist, müssen Sie ein VNet an dem Speicherort erstellt. Um ein VNet zu erstellen, die Schritte im Artikel [Erstellen eines virtuellen Netzwerks](virtual-networks-create-vnet-arm-ps.md) .

    Der Name der Subnetze innerhalb der VNet, geben Sie folgende Befehle und *Subnet1* oben mit dem Namen eines Subnetzes ersetzen:
        
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

8. Geben Sie die folgenden Befehle VNet und Subnetz abrufen und Variablen zuweisen.

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Identifizieren Sie eine vorhandene öffentliche IP-Adressressource, die Netzwerkschnittstelle zugeordnet werden können, damit Sie über das Internet herstellen können. Wenn Sie die VM mit der Schnittstelle zugreifen möchten, können Sie diesen Schritt überspringen. Ohne eine öffentliche IP-Adresse müssen Sie die VM aus anderen VM mit demselben VNet verbunden verbinden. 

    Ändern Sie den Namen einer vorhandenen öffentlichen IP-Adressressource, die am Speicherort vorhanden ist, erstellen Sie die Netzwerkschnittstelle in und, die nicht einer anderen Schnittstelle derzeit zugeordnet, *PIP1* . Ändern Sie ggf. $rgName auf den Namen der Ressourcengruppe die öffentliche IP-Adressressource vorhanden ist, und geben Sie den folgenden Befehl ein:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

    Geben Sie die folgenden Befehle aus, wenn Sie nicht den Namen einer vorhandenen öffentlichen IP-Adressressource kennen:

        $PublicIPAddresses = Get-AzureRmPublicIPAddress
        $PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

    Wenn **IPKonfigurationsdateiAddress** Spalte keinen Wert in der Ausgabe, die öffentliche IP-Adressenressource ist nicht mit einer vorhandenen Schnittstelle und kann verwendet werden. Wenn die Liste leer ist oder keine verfügbare öffentliche IP-Adresse Ressourcen, können Sie eine neue AzureRmPublicIPAddress Befehl erstellen.

    >[AZURE.NOTE] Öffentliche IP-Adressen haben eine geringe Gebühr. Erfahren Sie mehr über die [IP-Adresse Preise](https://azure.microsoft.com/pricing/details/ip-addresses) Seite Preise.
10. Wenn Sie nicht die Schnittstelle eine öffentliche IP-Adressenressource hinzugefügt haben, entfernen Sie *- Öffentl.IP $PIP1* am Ende des Befehls folgt. Erstellen Sie die Netzwerkschnittstelle mit schnellere Netzwerke durch Eingabe des folgenden Befehls:

        $nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. Weisen Sie Netzwerkschnittstelle eine VM beim die VM erstellen, wie in den Schritten 3 und 6 [Erstellen einer VM](../virtual-machines/virtual-machines-windows-ps-create.md) -Artikel zu. Ersetzen Sie in Schritt 6 2 *Standard_A1* VM Größen im Abschnitt [Einschränkungen](#limitations) aufgeführt.

    >[AZURE.NOTE] Ändert den *Namen* der $locName, $rgName oder $nic-Variablen in diesem Artikel wird Schritt 6 Erstellen eines Artikels VM fehl. Sie können jedoch die *Werte* der Variablen ändern.

12. Erstellte VM [Networking beschleunigte Treiber](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84)Herunterladen der VM herstellen und das Treiberinstallationsprogramm innerhalb des virtuellen Computers ausgeführt.

13. Windows rechten Maustaste, und klicken Sie auf **Geräte-Manager**. Überprüfen Sie die **Mellanox ConnectX 3 virtuellen Funktion Ethernet Adapter** unter **Netzwerk** die Option Wenn erweitert, wird angezeigt, wie in der folgenden Abbildung dargestellt:

    ![Geräte-Manager](./media/virtual-network-accelerated-networking-powershell/image2.png)