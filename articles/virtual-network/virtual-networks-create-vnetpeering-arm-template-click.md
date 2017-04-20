<properties
   pageTitle="VNet Peering Ressourcenmanager Vorlagen erstellen | Microsoft Azure"
   description="So erstellen Sie ein virtuelles Netzwerk peering mithilfe der Vorlagen im Ressourcen-Manager."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>Erstellen Sie VNet Peering Ressourcenmanager Vorlagen

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Erstellen Sie ein VNet mit Ressourcen-Manager Vorlagen peering führen Sie die folgenden Schritte aus:

1. Wenn Azure PowerShell noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) und gehen Sie bis Ende Azure anmelden und Ihr Abonnement auswählen.

    > [AZURE.NOTE] Das PowerShell-Cmdlet zum Verwalten von VNet peering Lieferumfang [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Nachfolgend wird die Definition einer Peeringverbindung VNet für VNet1, VNet2, basierend auf dem Szenario. Kopieren Sie den Inhalt und speichern in einer Datei namens VNetPeeringVNet1.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. Abschnitt zeigt die Definition einer Peeringverbindung VNet für VNet2 VNet1 basierend auf dem Szenario.  Kopieren Sie den Inhalt und speichern in einer Datei namens VNetPeeringVNet2.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Wie die oben genannte Vorlage sind einige konfigurierbare Eigenschaften für peering VNet:

  	|Option|Beschreibung|Standard|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Ob Adressraum eines Peers VNet Virtual_network Tag enthalten ist.|Ja|
  	|AllowForwardedTraffic|Ob Datenverkehr nicht von hervorragendem VNet akzeptiert oder verworfen.|Nein|
  	|AllowGatewayTransit|Peer-VNet VNet-Gateway verwenden können.|Nein|
  	|UseRemoteGateways|Verwenden des Peers VNet Gateway. Peer-VNet muss ein Gateway-Konfiguration und AllowGatewayTransit ausgewählt. Sie können diese Option verwenden, wenn ein Gateway konfiguriert haben.|Nein|

    Jeder Link VNet peering hat den Eigenschaften. Sie können beispielsweise AllowVirtualNetworkAccess True Peeringverbindung VNet VNet1 auf VNet2 festgelegt und für die Peeringverbindung VNet in die andere Richtung auf False festgelegt.


4. Bereitstellung die Vorlagendatei können Sie das Cmdlet AzureRmResourceGroupDeployment neu erstellen oder aktualisieren die Bereitstellung ausführen. Weitere Informationen über Ressourcen-Manager-Vorlagen finden Sie in diesem [Artikel](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Ersetzen Sie die Gruppe Namen und Ressourcendatei gegebenenfalls.

    Nachfolgend ein Beispiel dieses Szenario basiert:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Die Ausgabe zeigt:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Die Ausgabe zeigt:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Nachdem die Bereitstellung abgeschlossen ist, können Sie unten peering Ansichtszustand Cmdlet ausführen:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Die Ausgabe zeigt:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Nach peering in diesem Szenario besteht, sollten Sie die Verbindung von jedem virtuellen Computer mit einem virtuellen Computer in beiden VNets initiieren können. Standardmäßig gilt für AllowVirtualNetworkAccess und VNet peering Bereitstellen der richtigen ACLs der Kommunikation zwischen VNets. Netzwerkregeln für Group (NSG) Verbindung zwischen bestimmten Subnets oder virtuelle Computer die genaue Kontrolle auf zwischen zwei virtuelle Netzwerke blockieren können weiterhin anwenden.  Weitere Informationen erstellen NSG Regeln finden Sie in diesem [Artikel](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Erstellen Sie ein VNet über Abonnements peering führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei Azure privilegierter Benutzer ein Konto in ein Abonnement des und führen Sie das folgende Cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Dies ist nicht erforderlich, peering hergestellt werden auch wenn Benutzer einzeln peering Anfragen für ihre jeweiligen Vnets Anfragen übereinstimmen. Hinzufügen privilegierter andere VNet als Benutzer in der lokalen VNet erleichtert das Setup.

2. Anmeldung bei Azure privilegierte Benutzer B Konto B Abonnement und führen Sie das folgende Cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. Benutzer A ist in Sitzung dieses Cmdlet ausführen:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Hier ist wie die JSON-Datei definiert ist.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. Führen Sie das folgende Cmdlet in Benutzer-B-Sitzung:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Hier ist die Definition der JSON-Datei:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Nach peering in diesem Szenario besteht, sollten Sie die Verbindung von jedem virtuellen Computer mit einem virtuellen Computer beide vnets in verschiedenen Subskriptionen initiieren können.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In diesem Szenario können Sie unten zu VNet peering Vorlage bereitstellen.  Sie müssen die AllowForwardedTraffic-Eigenschaft auf True festzulegen, die virtuelle Netzwerkgerät in hervorragendem VNet senden und empfangen.

    Hier ist die Vorlage zum Erstellen einer peering von HubVNet VNet1 VNet. Beachten Sie, dass AllowForwardedTraffic auf False festgelegt ist.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Hier ist die Vorlage zum Erstellen einer peering von VNet1 HubVnet VNet. AllowForwardedTraffic festgelegt auf True.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Nachdem peering hergestellt wird, finden Sie auf diesen [Artikel](virtual-network-create-udr-arm-ps.md) benutzerdefinierte routes(UDR) Umleitung VNet1 Datenverkehr über ein virtuelles Gerät die Funktionen definieren. Wenn Sie die Adresse des nächste Abschnitts Route angeben, können Sie es an die IP-Adresse die virtuelle Appliance Peer VNet HubVNet festlegen.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Erstellen einer peering zwischen virtuellen Netzwerken aus verschiedenen Modellen wie folgt:
1. Der Text zeigt die Definition einer Peeringverbindung VNet für VNET1 VNET2 in diesem Szenario. Nur einen Link muss zu einem klassischen virtuellen Netzwerk Azure Resource Manager virtuelles Netzwerk.

    Legen Sie Ihre Abonnement-ID für klassische virtuelles Netzwerk oder VNET2 befindet und ändern Sie MyResouceGroup auf den Namen der entsprechenden Ressource.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "ContentVersion": "1.0.0.0", "Parameter": {}, "Variablen": {}, "Ressourcen": [{"Anforderungsparameter": "2016-06-01", "Typ": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "name": "VNET1/LinkToVNET2", "Location": "[ResourceGroup () .location]", "Eigenschaften": {"AllowVirtualNetworkAccess": true "AllowForwardedTraffic": false "AllowGatewayTransit": false "UseRemoteGateways": false "RemoteVirtualNetwork": {"Id": "[ResourceId (" Microsoft.ClassicNetwork/VirtualNetworks', 'VNET2')] "}}}]}

2. Führen Sie zum Bereitstellen der Vorlagendatei das folgende Cmdlet erstellen oder Aktualisieren der Bereitstellung.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Nach erfolgreicher Bereitstellung kann das folgende Cmdlet peering Ansichtszustand ausführen:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Nachdem peering zwischen einer klassischen VNet und Ressourcenmanager VNet eingerichtet wurde, sollten Sie Verbindungen von jedem virtuellen Computer in VNET1 zu jeder virtuellen Maschine VNET2 und umgekehrt werden.
