<properties
    pageTitle="Erstellen eine VM mit einer Vorlage Ressourcenmanager | Microsoft Azure"
    description="Mit der Ressourcenmanager Vorlage und PowerShell einfach einen neuen Windows-Computer erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Erstellen einer virtuellen Windows-Maschine mit einer Ressourcen-Manager-Vorlage

Dieser Artikel stellt Ihnen eine Vorlage Azure Ressourcenmanager und zeigt, wie Sie PowerShell verwenden, um es bereitzustellen. Die Vorlage wird eine einzige virtuellen Maschine mit Windows Server in ein neues virtuelles Netzwerk mit nur einem Subnetz bereitgestellt.

Ungefähr 20 Minuten sollte dauert, führen Sie die Schritte in diesem Artikel.

> [AZURE.IMPORTANT] Soll die VM Teil eines Verfügbarkeit fügen Sie auf die beim Erstellen des virtuellen Computers hinzu. Derzeit keine Möglichkeit, einen virtuellen Computer zu einem Verfügbarkeitsbericht festgelegt, nachdem es erstellt wurde.

## <a name="step-1-create-the-template-file"></a>Schritt 1: Erstellen der Vorlagendatei

Sie können eine eigene Vorlage mithilfe der Informationen in [Azure Ressourcenmanager Authoring-Vorlagen](../resource-group-authoring-templates.md)erstellen. Sie können auch Vorlagen bereitstellen, die für Sie von [Azure Schnellstarts Vorlagen](https://azure.microsoft.com/documentation/templates/)erstellt wurden.

1. Öffnen Sie einen Texteditor und fügen Sie die erforderlichen Schemaelement und erforderliche ContentVersion-Element:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parameter](../resource-group-authoring-templates.md#parameters) sind nicht immer erforderlich, aber sie können Werte eingeben, wenn die Vorlage bereitgestellt wird. Fügen Sie Parameter-Element und seine untergeordneten Elemente nach ContentVersion-Element:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Variablen](../resource-group-authoring-templates.md#variables) können in einer Vorlage verwendet werden, an Werte, die sich häufig ändern können oder Werte, die aus einer Kombination von Parameterwerten erstellt werden. Variablen-Element nach Parameterabschnitt hinzufügen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Ressourcen](../resource-group-authoring-templates.md#resources) wie der virtuelle Computer, virtuelle Netzwerk und das Speicherkonto werden anschließend in der Vorlage definiert. Fügen Sie im Ressourcenabschnitt nach Variablenabschnitt:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] Legt einen virtuellen Computer eine Version von Windows Server-Betriebssystem. Erfahren Sie mehr über andere Bilder finden Sie unter [Navigieren und select Azure virtuellen Computerimages Azure-CLI mit Windows PowerShell](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Speichern der Vorlage als *VirtualMachineTemplate.json*.

## <a name="step-2-create-the-parameters-file"></a>Schritt 2: Erstellen der Datei

Um Werte für die Ressourcenparameter anzugeben, die in der Vorlage definiert wurden, erstellen Sie eine Datei mit den Werten, die verwendet werden, wenn die Vorlage bereitgestellt wird.

1. Kopieren Sie im Text-Editor diese JSON-Inhalte in eine neue Datei namens *Parameters.json*:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Mehr über [Benutzername und Kennwort](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Speichern Sie die Datei.

## <a name="step-3-install-azure-powershell"></a>Schritt 3: Installieren von Azure PowerShell

Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen und an Ihrem Konto anmelden.

## <a name="step-4-create-a-resource-group"></a>Schritt 4: Erstellen einer Ressourcengruppe

Alle Ressourcen müssen in einer [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md)bereitgestellt werden.

1. Enthält eine Liste der verfügbaren Speicherorte, in denen Ressourcen erstellt werden können.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Ersetzen Sie den Wert des **$locName** mit einem Speicherort aus der Liste, z. B. **USA**. Erstellen Sie die Variable.

        $locName = "location name"
        
3. Ersetzen Sie den Wert des **$rgName** mit dem Namen der neuen Ressource. Erstellen Sie die Variable und die Ressourcengruppe.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Wie in diesem Beispiel sollte angezeigt werden:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Schritt 5: Erstellen von Ressourcen mit der Vorlage Parameter

Ersetzen Sie den Wert des **$templateFile** mit dem Pfad und Name der Vorlagendatei. Ersetzen Sie den Wert des **$parameterFile** mit dem Pfad und Namen der Datei. Die Variablen erstellen und Bereitstellen der Vorlage. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Sie können Vorlagen und Parameter von Azure Storage-Konto bereitstellen. Mehr finden Sie unter [Verwenden von Azure PowerShell mit Azure-Speicher](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Nächste Schritte

- Es gab Probleme mit der Bereitstellung, wäre Nächstes sich [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-portal](../resource-manager-troubleshoot-deployments-portal.md)
- Erfahren Sie, wie der virtuelle Computer verwalten, die anhand der [Azure-Ressourcen-Manager mit PowerShell verwalten virtuelle Computer](virtual-machines-windows-ps-manage.md)erstellt.
