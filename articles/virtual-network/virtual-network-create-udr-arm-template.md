<properties 
   pageTitle="Routing steuern und virtuelle Appliances in Ressourcen-Manager mithilfe einer Vorlage verwenden | Microsoft Azure"
   description="Informationen Sie zu routing steuern virtuelle Appliances in Azure-Ressourcen-Manager mithilfe einer Vorlage"
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
   ms.date="02/23/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-resource-manager-by-using-a-template"></a>Erstellen Sie Benutzer definierten Routen (UDR) in den Ressourcen-Manager mithilfe einer Vorlage

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager. 

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

## <a name="udr-resources-in-a-template-file"></a>UDR-Ressourcen in einer Vorlagendatei

Anzeigen und downloaden Sie die [Vorlage](https://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR).

Abschnitt zeigt die Definition des front-Ends UDR in der **Azuredeploy Vnet-Nsg udr.json** basierend auf dem Szenario.

    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/routeTables",
    "name": "[parameters('frontEndRouteTableName')]",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "UDR - FrontEnd"
    },
    "properties": {
      "routes": [
        {
          "name": "RouteToBackEnd",
          "properties": {
            "addressPrefix": "[parameters('backEndSubnetPrefix')]",
            "nextHopType": "VirtualAppliance",
            "nextHopIpAddress": "[parameters('vmaIpAddress')]"
          }
        }
      ]


Zum Verknüpfen der UDR mit dem front-End-Subnetz müssen Subnetz in der Vorlage ändern und die Referenz-Id für die UDR.

    "subnets": [
        "name": "[parameters('frontEndSubnetName')]",
        "properties": {
          "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
          },
          "routeTable": {
              "id": "[resourceId('Microsoft.Network/routeTables', parameters('frontEndRouteTableName'))]"
          }
        },
        ...]

Beachten Sie für die Back-End-NSG und Back-End-Subnetz in der Vorlage identisch.

Sie müssen sicherstellen, dass die VM **FW1** hat die IP-Weiterleitung-Eigenschaft der Netzwerkkarte, die zum Empfangen und Weiterleiten von Paketen verwendet werden. Abschnitt zeigt die Definition der NIC für FW1 in der Datei Azuredeploy Nsg udr.json basierend auf dem Szenario.

    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DMZ"
    },
    "name": "[concat(variables('fwVMSettings').nicName, copyindex(1))]",
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('fwVMSettings').pipName, copyindex(1))]",
      "[concat('Microsoft.Resources/deployments/', 'vnetTemplate')]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "enableIPForwarding": true,
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat('192.168.0.',copyindex(4))]",
            "publicIPAddress": {
              "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('fwVMSettings').pipName, copyindex(1)))]"
            },
            "subnet": {
              "id": "[variables('dmzSubnetRef')]"
            }
          }
        }
      ]
    },
    "copy": {
      "name": "fwniccount",
      "count": "[parameters('fwCount')]"
    }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Bereitstellen Sie ARM-Vorlage mithilfe von auf Bereitstellung

Vorlage verfügbar im öffentlichen Repository verwendet Parameterdatei standardmäßig Werte mit das oben beschriebene Szenario generiert. Zum Bereitstellen dieser Vorlage auf bereitstellen, folgen [diesem Link](https://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR)auf **in Azure bereitstellen**, ersetzen die Standardparameterwerte ggf. und gehen in das Portal.

## <a name="deploy-the-arm-template-by-using-powershell"></a>Bereitstellen Sie ARM-Vorlage mithilfe von PowerShell

Gehen Sie wie folgt vor Bereitstellung mithilfe von PowerShell heruntergeladene ARM-Vorlage.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Wenn Azure PowerShell noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) und gehen Sie bis Ende Azure anmelden und Ihr Abonnement auswählen.

2. Führen Sie die `New-AzureRmResourceGroup` Cmdlet eine Ressourcengruppe erstellen.

        New-AzureRmResourceGroup -Name TestRG -Location westus

3. Führen Sie die `New-AzureRmResourceGroupDeployment` Cmdlet die Vorlage bereitstellen.

        New-AzureRmResourceGroupDeployment -Name DeployUDR -ResourceGroupName TestRG `
            -TemplateUri https://raw.githubusercontent.com/telmosampaio/azure-templates/master/IaaS-NSG-UDR/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/telmosampaio/azure-templates/master/IaaS-NSG-UDR/azuredeploy.parameters.json            

    Erwartete Ausgabe:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              : 
        Permissions       : 
                            Actions  NotActions
                            =======  ==========
                            *                  
                            
        Resources         : 
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            ASFW                Microsoft.Compute/availabilitySets       westus  
                            ASSQL               Microsoft.Compute/availabilitySets       westus  
                            ASWEB               Microsoft.Compute/availabilitySets       westus  
                            FW1                 Microsoft.Compute/virtualMachines        westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            WEB1                Microsoft.Compute/virtualMachines        westus  
                            WEB2                Microsoft.Compute/virtualMachines        westus  
                            NICFW1              Microsoft.Network/networkInterfaces      westus  
                            NICSQL1             Microsoft.Network/networkInterfaces      westus  
                            NICSQL2             Microsoft.Network/networkInterfaces      westus  
                            NICWEB1             Microsoft.Network/networkInterfaces      westus  
                            NICWEB2             Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            PIPFW1              Microsoft.Network/publicIPAddresses      westus  
                            PIPSQL1             Microsoft.Network/publicIPAddresses      westus  
                            PIPSQL2             Microsoft.Network/publicIPAddresses      westus  
                            PIPWEB1             Microsoft.Network/publicIPAddresses      westus  
                            PIPWEB2             Microsoft.Network/publicIPAddresses      westus  
                            UDR-BackEnd         Microsoft.Network/routeTables            westus  
                            UDR-FrontEnd        Microsoft.Network/routeTables            westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  
                            
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Bereitstellen Sie ARM-Vorlage mithilfe der Azure-CLI

Gehen Sie folgendermaßen vor um die ARM-Vorlage mithilfe der Azure-CLI bereitstellen.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie die `azure config mode` Befehl Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    New mode is arm

3. Navigieren Sie zu **https://raw.githubusercontent.com/telmosampaio/azure-templates/master/IaaS-NSG-UDR/azuredeploy.parameters.json**aus Ihrem Browser kopieren Sie den Inhalt der Json-Datei und fügen Sie eine neue Datei auf Ihrem Computer. In diesem Szenario würden Sie die Werte unter in einer Datei namens **c:\udr\azuredeploy.parameters.json**kopieren.

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "fwCount": {
              "value": 1
            },
            "webCount": {
              "value": 2
            },
            "sqlCount": {
              "value": 2
            }
          }
        }

4. Führen Sie das Cmdlet **Azure Gruppe** zum Bereitstellen neuen Vnets mithilfe der Vorlage und Parameter-Dateien heruntergeladen und über geändert. Die Liste nach die Ausgabe der Parameter erläutert.

        azure group create -n TestRG -l westus --template-uri 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/IaaS-NSG-UDR/azuredeploy.json' -e 'c:\udr\azuredeploy.parameters.json'

    Erwartete Ausgabe:

        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Updating resource group TestRG
        info:    Updated resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK

5. Führen Sie den Befehl **Azure Gruppe** an die Ressourcen in die neue Ressourcengruppe erstellt. 

        azure group show TestRG

    Erwartetes Ergebnis
        
        info:    Executing command group show
        info:    Listing resource groups
        info:    Listing resources for the group
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    Resources:
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/availabilitySets/ASFW
        data:      Name    : ASFW
        data:      Type    : availabilitySets
        data:      Location: westus
        data:      Tags    : displayName=AvailabilitySet - DMZ
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/availabilitySets/ASSQL
        data:      Name    : ASSQL
        data:      Type    : availabilitySets
        data:      Location: westus
        data:      Tags    : displayName=AvailabilitySet - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/availabilitySets/ASWEB
        data:      Name    : ASWEB
        data:      Type    : availabilitySets
        data:      Location: westus
        data:      Tags    : displayName=AvailabilitySet - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/FW1
        data:      Name    : FW1
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - DMZ
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/SQL1
        data:      Name    : SQL1
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/SQL2
        data:      Name    : SQL2
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WEB1
        data:      Name    : WEB1
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WEB2
        data:      Name    : WEB2
        data:      Type    : virtualMachines
        data:      Location: westus
        data:      Tags    : displayName=VMs - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1
        data:      Name    : NICFW1
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - DMZ
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL1
        data:      Name    : NICSQL1
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL2
        data:      Name    : NICSQL2
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1
        data:      Name    : NICWEB1
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2
        data:      Name    : NICWEB2
        data:      Type    : networkInterfaces
        data:      Location: westus
        data:      Tags    : displayName=NetworkInterfaces - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd
        data:      Name    : NSG-BackEnd
        data:      Type    : networkSecurityGroups
        data:      Location: westus
        data:      Tags    : displayName=NSG - Front End
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
        data:      Name    : NSG-FrontEnd
        data:      Type    : networkSecurityGroups
        data:      Location: westus
        data:      Tags    : displayName=NSG - Remote Access
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPFW1
        data:      Name    : PIPFW1
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - DMZ
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPSQL1
        data:      Name    : PIPSQL1
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPSQL2
        data:      Name    : PIPSQL2
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - SQL
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPWEB1
        data:      Name    : PIPWEB1
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPWEB2
        data:      Name    : PIPWEB2
        data:      Type    : publicIPAddresses
        data:      Location: westus
        data:      Tags    : displayName=PublicIPAddresses - Web
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd
        data:      Name    : UDR-BackEnd
        data:      Type    : routeTables
        data:      Location: westus
        data:      Tags    : displayName=Route Table - Back End
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd
        data:      Name    : UDR-FrontEnd
        data:      Type    : routeTables
        data:      Location: westus
        data:      Tags    : displayName=UDR - FrontEnd
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:      Name    : TestVNet
        data:      Type    : virtualNetworks
        data:      Location: westus
        data:      Tags    : displayName=VNet
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Storage/storageAccounts/testvnetstorageprm
        data:      Name    : testvnetstorageprm
        data:      Type    : storageAccounts
        data:      Location: westus
        data:      Tags    : displayName=Storage Account - Premium
        data:    
        data:      Id      : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Storage/storageAccounts/testvnetstoragestd
        data:      Name    : testvnetstoragestd
        data:      Type    : storageAccounts
        data:      Location: westus
        data:      Tags    : displayName=Storage Account - Simple
        data:    
        data:    Permissions:
        data:      Actions: *
        data:      NotActions: 
        data:    
        info:    group show command OK

>[AZURE.TIP] Wenn Sie nicht alle Ressourcen anzuzeigen, die `azure group deployment show` aus, um den Bereitstellungsstatus der Bereitstellung sicherzustellen ist *erfolgreich*.