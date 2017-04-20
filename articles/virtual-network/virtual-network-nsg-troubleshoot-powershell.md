<properties 
   pageTitle="Problembehandlung bei Netzwerk-Sicherheitsgruppen - PowerShell | Microsoft Azure"
   description="Erfahren Sie mehr über das Netzwerk von Sicherheitsgruppen in der Azure-Ressourcen-Manager-Bereitstellungsmodell mit Azure PowerShell behandeln."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
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
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Problembehandlung bei Netzwerksicherheitsgruppen Azure PowerShell

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Netzwerk-Sicherheitsgruppen (NSGs) auf dem virtuellen Computer (VM) konfiguriert und VM-Verbindungsprobleme auftreten Überblick dieser Artikel einen Diagnosefunktionen für NSGs zu diagnostizieren.

NSGs können Sie den Datenverkehr steuern, fließen in die virtuellen Computer (VMs). NSGs können Subnetze in einer Azure Virtual Network (VNet) oder Netzwerkschnittstellen (NIC) zugewiesen werden. Die effektiven Regeln zu einer Netzwerkkarte sind eine Aggregation Subnetz verbunden, und die Regeln in der NSGs auf einen Netzwerkadapter angewendet. Regeln über diese NSGs können manchmal Konflikte verursachen und eine VM-Netzwerkkonnektivität auswirken.  

Sie können aus der NSGs auf die VM-Netzwerkkarten die effektive Regeln anzeigen. Dieser Artikel veranschaulicht das VM Verbindungsproblemen verwenden diese Regeln in der Azure-Ressourcen-Manager-Bereitstellungsmodell. Wenn Sie nicht mit VNet und NSG Konzepten vertraut sind, lesen Sie Artikel Übersicht über [virtuelle Netzwerk](virtual-networks-overview.md) und [Netzwerk-Sicherheitsgruppen](virtual-networks-nsg.md) .

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Verwendung von effektiven Sicherheitsregeln VM-Datenverkehr

Das folgende Szenario ist ein Beispiel für eine allgemeine Verbindung:

Ein virtueller Computer mit dem Namen *VM1* gehört ein Subnetz mit dem Namen *Subnet1* in einer VNet mit dem Namen *WestUS-VNet1*. Ein Verbindungsversuch an TCP-Port 3389 RDP über VM schlägt fehl. NSGs werden sowohl NIC *VM1 NIC1* Subnetz *Subnet1*angewendet. Datenverkehr an TCP-Port 3389 darf zugeordneten Netzwerkschnittstelle *VM1 NIC1*NSG jedoch TCP ping auf VM1s Port 3389 fehlschlägt.

Während dieses Beispiel TCP-Port 3389 verwendet, können folgendermaßen verwendet werden, eingehenden und ausgehenden Verbindungsfehlern über einen beliebigen Port bestimmen.

## <a name="detailed-troubleshooting-steps"></a>Detaillierte Schritte zur Fehlerbehebung
Gehen Sie folgendermaßen vor um NSGs für eine VM zu beheben:

1. Starten einer Azure PowerShell-Sitzung und bei Azure. Wenn Sie nicht mit Azure PowerShell vertraut sind, lesen Sie den Artikel [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) .

2. Geben Sie den folgenden Befehl alle NSG Regeln für eine Netzwerkkarte namens *VM1 NIC1* Ressourcengruppe *RG1*zurückgegeben:

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Wenn Sie den Namen einer NIC nicht kennen, geben Sie folgenden Befehl alle NICs in einer Ressourcengruppe abgerufen: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    Der folgende Text ist ein Beispiel für die Ausgabe effektive Regeln für *VM1 NIC1* NIC:

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Beachten Sie die folgende Informationen in der Ausgabe:

    - Es gibt zwei **NetworkSecurityGroup** Bereiche: eine mit einem Subnetz (*Subnet1*) und eine Netzwerkkarte (*VM1 NIC1*) zugeordnet. In diesem Beispiel wurde eine NSG jedes ausgeglichen.
    - **Zuordnung** zeigt der Ressource (Subnetz oder NIC) angegebene NSG zugeordnet ist. Wenn NSG Ressource verschoben/aufgehoben wird unmittelbar vor dem Ausführen dieses Befehls müssen Sie ein paar Sekunden Änderung entsprechend in der Ausgabe des Befehls. 
    - Die Namen, die mit *DefaultSecurityRules*beginnen: mehrere Standard-Sicherheitsregeln darin erstellten, wenn eine NSG erstellt. Standardregeln können nicht entfernt werden, jedoch mit höherer Prioritätsregeln überschrieben werden.
     Lesen Sie [NSG](virtual-networks-nsg.md#default-rules) Übersichtsartikel Informationen NSG Standardregeln Sicherheit.
    - **ExpandedAddressPrefix** erweitert die Adresspräfixe NSG Standard-Tags. Es stehen mehrere Adresspräfixe. Erweiterung der Tags hilfreich bei der Behandlung von VM-Verbindung nach bestimmten Präfixen. Beispielsweise erweitert mit peering VNET VIRTUAL_NETWORK Tag hervorragendem VNet Präfixe in der vorherigen Ausgabe angezeigt.

        >[AZURE.NOTE] Der Befehl nur zeigt effektive Regeln ist eine NSG mit einem Subnetz, eine Netzwerkkarte oder beide. Ein virtueller Computer möglicherweise mehrere NICs mit verschiedenen NSGs angewendet. Bei der Problembehandlung, führen Sie den Befehl für jede Netzwerkkarte.
        
3. Geben Sie die folgenden Befehle zur weiteren Fehlerbehebung, um über mehr NSG Regeln filtern zu erleichtern: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    Filter für RDP-Datenverkehr (TCP-Port 3389) wird der Rasteransicht angewendet, wie in der folgenden Abbildung dargestellt:

    ![Liste](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. Wie in der Rasteransicht angezeigt, gibt Verzeichnisdienstobjekt Regeln für RDP. Die Ausgabe aus Schritt 2 zeigt, dass die *DenyRDP* -Regel NSG im Subnetz zugewiesen wird. Für eingehende Regeln werden angewendet auf das Subnetz NSGs zuerst verarbeitet. Eine Übereinstimmung gefunden wird, wird auf die Schnittstelle angewendete NSG nicht verarbeitet. In diesem Fall blockiert *DenyRDP* Regel aus dem Subnetz RDP VM (**VM1**).

    >[AZURE.NOTE] Ein virtueller Computer möglicherweise mehrere Netzwerkkarten verbunden. Jeder kann in ein anderes Subnetz verbunden. Da eine Netzwerkkarte die Befehle in den vorherigen Schritten ausgeführt werden, ist es wichtig sicherzustellen, dass die Netzwerkkarte Geben Sie Verbindungsfehler zu haben. Wenn Sie nicht sicher sind, können Sie die Befehle immer gegen jede Netzwerkkarte an der VM ausführen.

5. Ändern Sie RDP in VM1 Regel *Verweigern RDP (Port 3389)* in *RDP(3389) können* in **Subnet1 NSG** NSG. Bestätigen Sie, dass TCP-Port 3389 durch Öffnen einer RDP-Verbindungs zur VM oder PsPing Werkzeug geöffnet ist. Sie können mehr über PsPing erfahren [PsPing download-Seite](https://technet.microsoft.com/sysinternals/psping.aspx)

    Sie können oder eine NSG mithilfe der Informationen in der Ausgabe des folgenden Befehls Regeln entfernen:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Hinweise

Berücksichtigen Sie Behandlung von Konnektivitätsproblemen sollten Sie folgende Punkte:

- NSG Standardregeln eingehenden Zugriff aus dem Internet blockiert und VNet nur zulassen eingehenden Datenverkehr. Regeln sollten eingehende Internet Bedarf Zugriff explizit hinzugefügt werden.
- Sind keine NSG Sicherheitsregeln verursacht eine VM-Netzwerkkonnektivität fehlschlägt, kann das Problem Ursachen haben:
    - Firewallsoftware Betriebssystem des virtuellen Computers
    - Routen für virtuelle Appliances oder lokalen Datenverkehr konfiguriert. Internet-Datenverkehr kann lokal über das erzwungene Tunneln umgeleitet werden. Eine RDP/SSH-Verbindung aus dem Internet die VM funktioniert möglicherweise nicht mit dieser Einstellung je nach die Netzwerkhardware lokalen Datenverkehr Behandlung. Die [Problembehandlung Routen](virtual-network-routes-troubleshoot-powershell.md) Artikel Informationen zum Arbeitsplan Probleme diagnostizieren, die den Fluss des Datenverkehrs und die VM behindern kann. 
- Wenn VNets, standardmäßig dies haben, wird das Tag VIRTUAL_NETWORK automatisch erweitert Präfixe bei hervorragendem VNets enthalten. Sie können diese Präfixe in der Liste **ExpandedAddressPrefix** beheben Sie alle Probleme im Zusammenhang mit VNet peering Konnektivität anzeigen. 
- Effektive Sicherheitsregeln werden nur angezeigt, wenn eine NSG der VMs NIC oder Subnetz zugeordnet ist. 
- Keine NSGs mit der Netzwerkkarte verknüpft oder Subnetz und eine öffentliche IP-Adresse der VM zugewiesen haben, werden alle Anschlüsse für eingehenden und ausgehenden Zugriff geöffnet. Hat die VM eine öffentliche IP-Adresse der Netzwerkkarte oder dem Subnetz NSGs zuweisen empfiehlt.  
