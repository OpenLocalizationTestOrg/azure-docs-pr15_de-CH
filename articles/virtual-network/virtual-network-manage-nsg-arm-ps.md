<properties 
   pageTitle="NSGs mithilfe von PowerShell in Ressourcen-Manager verwalten | Microsoft Azure"
   description="Erfahren Sie, wie vorhandene NSGs mithilfe von PowerShell in Ressourcen-Manager verwalten"
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
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>NSGs mithilfe von PowerShell verwalten

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Informationen abrufen

Sie können Ihre vorhandenen NSGs anzeigen, Regeln für eine vorhandene NSG abrufen und herauszufinden, welche Ressourcen eine NSG zugeordnet ist.

### <a name="view-existing-nsgs"></a>Vorhandene NSGs anzeigen
Um alle vorhandenen NSGs in einem Abonnement anzuzeigen, führen Sie die `Get-AzureRmNetworkSecurityGroup` Cmdlets wie unten dargestellt.

    Get-AzureRmNetworkSecurityGroup

Ergebnis:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Führen Sie zum Anzeigen der Liste der NSGs in einer Ressourcengruppe der `Get-AzureRmNetworkSecurityGroup` Cmdlets wie unten dargestellt. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Erwartete Ausgabe:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>Liste aller Regeln für eine NSG

Um Regeln eine NSG namens **NSG FrontEnd**anzuzeigen, führen Sie die `Get-AzureRmNetworkSecurityGroup` Cmdlets wie unten dargestellt. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Erwartete Ausgabe:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] Sie können auch `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` die Standardregeln von **NSG FrontEnd** NSG aufgelistet.

### <a name="view-nsgs-associations"></a>NSGs Zuordnung anzeigen

Führen Sie zum Anzeigen der Ressourcen **NSG FrontEnd** NSG Zuordnen der `Get-AzureRmNetworkSecurityGroup` Cmdlets wie unten dargestellt.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Für die Eigenschaften **NetworkInterfaces** und **Subnetze** wie folgt aussehen:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

Im obigen Beispiel die NSG ist keine Netzwerkschnittstellen (NICs) und namens **FrontEnd**Subnetz zugeordnet ist.

## <a name="manage-rules"></a>Regeln verwalten

Sie können eine vorhandene NSG Regeln hinzufügen, bestehende Regeln bearbeiten und entfernen.

### <a name="add-a-rule"></a>Hinzufügen einer Regel

Gehen Sie folgendermaßen vor um eine Regel **eingehenden** Datenverkehr auf Port **443** von jedem Computer **NSG FrontEnd** NSG hinzuzufügen.

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum vorhandene NSG abrufen und in einer Variablen speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Ausführen der `Add-AzureRmNetworkSecurityRuleConfig` -Cmdlet, wie unten dargestellt.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Das Speichern der NSG zur Ausführung der `Set-AzureRmNetworkSecurityGroup` Cmdlets wie unten dargestellt.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Zeigt nur die Sicherheitsregeln erwartete Ausgabe:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Ändern einer Regel

Gehen Sie folgendermaßen vor um eingehenden Datenverkehr aus dem **Internet** nur oben erstellte Regel ändern.

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum vorhandene NSG abrufen und in einer Variablen speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Ausführen der `Set-AzureRmNetworkSecurityRuleConfig` -Cmdlet, wie unten dargestellt.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Das Speichern der NSG zur Ausführung der `Set-AzureRmNetworkSecurityGroup` Cmdlets wie unten dargestellt.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Zeigt nur die Sicherheitsregeln erwartete Ausgabe:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Löschen einer Regel

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum vorhandene NSG abrufen und in einer Variablen speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Ausführen der `Remove-AzureRmNetworkSecurityRuleConfig` -Cmdlet, wie unten dargestellt.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. Das Speichern der NSG zur Ausführung der `Set-AzureRmNetworkSecurityGroup` Cmdlets wie unten dargestellt.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Erwartete Ausgabe zeigt nur die Sicherheitsregeln Beachten Sie **Https-Regel** nicht aufgeführt ist:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Verwalten von Assoziationen

Sie können eine NSG, Subnetze und Netzwerkkarten zuordnen. Sie können auch eine NSG aus beliebigen Ressourcen trennen, zugeordnet ist.

### <a name="associate-an-nsg-to-a-nic"></a>NSG zu einer Netzwerkkarte zuordnen

Gehen Sie folgendermaßen vor um **NSG FrontEnd** NSG, **TestNICWeb1** NIC zuzuordnen.

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum vorhandene NSG abrufen und in einer Variablen speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Führen Sie die `Get-AzureRmNetworkInterface` Cmdlet vorhandene Netzwerkschnittstellenkarte abgerufen und in einer Variablen speichern, wie unten dargestellt.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Einstellen der **NetworkSecurityGroup** -Eigenschaft der **NIC** -Variable auf den Wert der Variablen **NSG** wie folgt

        $nic.NetworkSecurityGroup = $nsg

4. Führen Sie zum Speichern der Änderungen an der Netzwerkkarte der `Set-AzureRmNetworkInterface` Cmdlets wie unten dargestellt.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Erwartete Ausgabe zeigt nur die **NetworkSecurityGroup** -Eigenschaft:

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>NSG aus einer Netzwerkkarte trennen

Gehen Sie folgendermaßen vor um **NSG FrontEnd** NSG aus **TestNICWeb1** NIC trennen.

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum vorhandene NSG abrufen und in einer Variablen speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Führen Sie die `Get-AzureRmNetworkInterface` Cmdlet vorhandene Netzwerkschnittstellenkarte abgerufen und in einer Variablen speichern, wie unten dargestellt.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. **NetworkSecurityGroup** -Eigenschaft der **NIC** -Variable auf **$null**festgelegt, wie unten dargestellt.

        $nic.NetworkSecurityGroup = $null

4. Führen Sie zum Speichern der Änderungen an der Netzwerkkarte der `Set-AzureRmNetworkInterface` Cmdlets wie unten dargestellt.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Erwartete Ausgabe zeigt nur die **NetworkSecurityGroup** -Eigenschaft:

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>NSG in einem Subnetz trennen

Gehen Sie folgendermaßen vor um **NSG FrontEnd** NSG vom **Front-End** -Subnetz zu trennen.

1. Führen Sie die `Get-AzureRmVirtualNetwork` Cmdlet vorhandene VNet abgerufen und in einer Variablen speichern, wie unten dargestellt.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Ausführen der `Get-AzureRmVirtualNetworkSubnetConfig` -Cmdlet zum **Front-End** -Subnetz abrufen und in einer Variablen speichern, wie unten dargestellt.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. **NetworkSecurityGroup** -Eigenschaft der **Subnetz** -Variable auf **$null**festgelegt, wie unten dargestellt.

        $subnet.NetworkSecurityGroup = $null

4. Führen Sie zum Speichern der Änderungen an das Subnetz der `Set-AzureRmVirtualNetwork` Cmdlets wie unten dargestellt.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Mit den Eigenschaften des Subnetzes **FrontEnd** erwartete Ausgabe. Beachten Sie, dass keine Eigenschaft für **NetworkSecurityGroup**:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>NSG Subnetz zuordnen

Gehen Sie folgendermaßen vor um **NSG FrontEnd** NSG Subnetz **FronEnd** zuzuordnen.

1. Führen Sie die `Get-AzureRmVirtualNetwork` Cmdlet vorhandene VNet abgerufen und in einer Variablen speichern, wie unten dargestellt.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Ausführen der `Get-AzureRmVirtualNetworkSubnetConfig` -Cmdlet zum **Front-End** -Subnetz abrufen und in einer Variablen speichern, wie unten dargestellt.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum vorhandene NSG abrufen und in einer Variablen speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. **NetworkSecurityGroup** -Eigenschaft der **Subnetz** -Variable auf **$null**festgelegt, wie unten dargestellt.

        $subnet.NetworkSecurityGroup = $nsg

4. Führen Sie zum Speichern der Änderungen an das Subnetz der `Set-AzureRmVirtualNetwork` Cmdlets wie unten dargestellt.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Erwartete Ausgabe zeigt nur die **NetworkSecurityGroup** -Eigenschaft des **Front-End** -Subnetz:

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Eine NSG löschen

Wenn keine Ressourcen zugeordnet sind, können Sie nur eine NSG löschen. Gehen Sie folgendermaßen vor um eine NSG zu löschen.

1. Um eine NSG zugeordneten Ressourcen zu überprüfen, führen die `azure network nsg show` Siehe [Ansicht NSGs Assoziationen](#View-NSGs-associations).
2. Wenn die NSG alle NICs zugeordnet ist, die `azure network nic set` Siehe [lg NSG aus einer Netzwerkkarte](#Dissociate-an-NSG-from-a-NIC) für jede Netzwerkkarte. 
3. Wenn die NSG ein Subnetz zugeordnet ist, die `azure network vnet subnet set` Siehe [lg NSG ein Subnetz](#Dissociate-an-NSG-from-a-subnet) für jedes Subnetz.
4. Führen Sie zum Löschen der NSG der `Remove-AzureRmNetworkSecurityGroup` Cmdlets wie unten dargestellt.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] Die **-Force** Parameter stellt sicher, Sie müssen das Löschen bestätigen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren Sie die Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.