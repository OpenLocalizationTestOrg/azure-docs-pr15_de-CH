<properties
   pageTitle="Ressourcenmanager Vorlage Walkthrough | Microsoft Azure"
   description="Eine schrittweise Anleitung Resource Manager Vorlage eine grundlegende Azure IaaS-Architektur bereitstellen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Ressourcenmanager Vorlage Exemplarische Vorgehensweise

Eine der ersten Fragen, die beim Erstellen einer Vorlage ist "von starten". Aus einer leeren Vorlage der grundlegenden Struktur gemäß [Artikel Vorlage erstellen](resource-group-authoring-templates.md#template-format), starten kann und Ressourcen und entsprechenden Parametern und Variablen hinzufügen. Eine gute Alternative wäre Durchlaufen der [Schnellstart-Galerie](https://github.com/Azure/azure-quickstart-templates) und Suchen nach ähnlichen Szenarien, die Sie erstellen möchten. Sie können mehrere Vorlagen zusammenführen oder Bearbeiten einer vorhandenen spezifische Szenario angepasst. 

Werfen Sie einen Blick auf eine gemeinsame Infrastruktur:

* Zwei virtuelle Computer, die das gleiche Speicher verwenden, werden in der gleichen Verfügbarkeit und im gleichen Subnetz des virtuellen Netzwerks
* Eine einzelne NIC und VM-IP Adresse für jeden virtuellen Computer.
* Ein Lastenausgleich mit einer Regel auf Port 80

![Architektur](./media/resource-group-overview/arm_arch.png)

Dieses Thema führt Sie schrittweise ein Ressourcenmanager Vorlage für die Infrastruktur. Die letzte Vorlage erstellten basiert auf Schnellstart Vorlage [2 VMs Lastenausgleich und Lastenausgleich Regeln](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/)aufgerufen.

Aber viele auf einmal erstellen, also ein Speicherkonto zuerst erstellen und bereitstellen. Nachdem Sie das Speicherkonto erstellen beherrschen, fügen Sie die Ressourcen und erneut bereitstellen, die Vorlage die Infrastruktur abgeschlossen.

>[AZURE.NOTE] Typ des Editors können beim Erstellen der Vorlage verwendet werden. Visual Studio bietet Tools, die Vorlage erleichtert jedoch Visual Studio zum Bearbeiten dieses Lernprogramms nicht erforderlich. Ein Lernprogramm zur Verwendung von Visual Studio zum Erstellen einer-Bereitstellung Web App und SQL-Datenbank finden Sie unter [Erstellen und Bereitstellen von Azure Ressourcengruppen über Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## <a name="create-the-resource-manager-template"></a>Ressourcen-Manager erstellen

Die Vorlage ist eine JSON-Datei, die alle Ressourcen definiert wird. Sie ermöglicht Ihnen, mit den angegebenen Parametern während der Bereitstellung Variablen definieren, die Werte und Ausdrücke und Ausgaben der Bereitstellung erstellt. 

Beginnen wir mit der einfachsten Vorlage:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Speichern Sie diese Datei als **azuredeploy.json** (Beachten Sie, dass die Vorlage können Sie alle Namen nur, dass es eine Json-Datei).

## <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen
Fügen Sie ein Objekt hinzu, das Speicherkonto definiert, werden im Abschnitt **Ressourcen** wie folgt. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Sie Fragen sich vielleicht, wo diese Eigenschaften und Werte stammen. Eigenschaften, **Typ**, **Name**, **Anforderungsparameter**und **Position** sind Standardelemente für alle Ressourcentypen verfügbar sind. Sie können die Standardelemente [Ressourcen](resource-group-authoring-templates.md#resources)kennen. **Name** ist ein Parameterwert fest, die während der Bereitstellung und **Speicherort** als Speicherort für die Ressourcengruppe verwendet übergeben. Wir werden Sie **Typ** und **Anforderungsparameter** in den folgenden Abschnitten ermitteln.

Abschnitt **Eigenschaften** enthält alle Eigenschaften, die für einen bestimmten Ressourcentyp eindeutig sind. Den PUT-Vorgang in die REST-API zum Erstellen von diesen Ressourcentyp entsprechen den Werten, die in diesem Abschnitt genau angeben. Wenn ein Speicherkonto erstellen, müssen Sie eine **AccountType**bereitstellen. Beachten Sie in der [REST-API zum Erstellen ein Speicherkonto](https://msdn.microsoft.com/library/azure/mt163564.aspx) Eigenschaftenabschnitt des anderen Vorgangs enthält auch eine Eigenschaft **AccountType** und die zulässigen Werte sind dokumentiert. In diesem Beispiel der Kontotyp auf **Standard_LRS**festgelegt ist, aber Sie geben einen anderen Wert oder Benutzer Kontotyp als Parameter übergeben.

Jetzt wir wechseln zurück zum Abschnitt **Parameter** und sehen, wie der Name des Speicherkontos definieren. Erfahren Sie mehr über die Verwendung von Parametern [Parameter](resource-group-authoring-templates.md#parameters). 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Hier definiert einen Parameter vom Typ String, der den Namen des Speicherkontos halten. Der Wert dieses Parameters wird während der Bereitstellung bereitgestellt.

## <a name="deploying-the-template"></a>Bereitstellen der Vorlage
Wir haben eine vollständige Vorlage für die Erstellung einer neuen Speicher. Wie bereits erwähnt, wurde die Vorlage im **azuredeploy.json** Datei gespeichert:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Es gibt einige Methoden zum Bereitstellen einer Vorlage Siehe [Ressourceneinsatz Artikel](resource-group-template-deploy.md). Bereitstellen die Vorlage mithilfe von Azure PowerShell verwenden:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Oder zum Bereitstellen der Vorlage mithilfe der Azure-Befehlszeilenschnittstelle verwenden:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Sie können nun die Klinik ein Speicherkonto.

Die nächsten Schritte werden zum Starten des Lernprogramms beschriebene Architektur bereitzustellende erforderlichen Ressourcen hinzufügen. Sie können diese Ressourcen in derselben Vorlage auf Arbeit hinzufügen.

## <a name="availability-set"></a>Festlegen der Verfügbarkeit
Fügen Sie nach der Definition der für das Speicherkonto einen für die virtuellen Computer festgelegten erhältlich hinzu. In diesem Fall gibt keine zusätzlichen Eigenschaften, damit seine Definition relativ einfach ist. Finden Sie die [REST-API zum Erstellen von Verfügbarkeit festlegen](https://msdn.microsoft.com/library/azure/mt163607.aspx) für Abschnitt vollständige Eigenschaften definieren Werte zählen Domäne zählen und Fehlertoleranz Domäne aktualisieren möchten.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Beachten Sie, dass der **Name** dem Wert einer Variablen. Für diese Vorlage wird der Name des Verfügbarkeit in verschiedenen benötigt. Problemlos können die Vorlage verwaltet Wert einmal definieren und mehrfach verwenden.

Der Wert für den **Typ** enthält sowohl der Ressourcenanbieter den Ressourcentyp. Bei Verfügbarkeit der Ressourcenanbieter ist **Microsoft.Compute** , und der Typ ist **AvailabilitySets**. Mit dem folgenden PowerShell-Befehl können Sie die Liste der verfügbaren Ressourcen abrufen:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Oder bei Verwendung von Azure CLI den folgenden Befehl ausführen:
```
    azure provider list
```
Da in diesem Thema mit Speicherkonten, virtuelle Computer, virtuelle Netzwerke erstellen, arbeiten Sie mit:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Um die Ressourcentypen für einen bestimmten Anbieter anzuzeigen, führen Sie den folgenden PowerShell-Befehl:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Oder Azure CLI der folgende Befehl Rückgabetypen im JSON-Format verfügbar, und in einer Datei speichern.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

**AvailabilitySets** sollte als einer der Typen in **Microsoft.Compute**angezeigt werden. Der vollständige Name des Typs ist **Microsoft.Compute/availabilitySets**. Sie können den Namen aller Ressourcen in Sie Vorlage bestimmen.

## <a name="public-ip"></a>Öffentliche IP-Adresse
Definieren Sie eine öffentliche IP-Adresse. Anzeigen, sehen Sie die [REST-API für öffentliche IP-Adressen](https://msdn.microsoft.com/library/azure/mt163590.aspx) für die Eigenschaften fest.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Die Methode der Verteilung auf **dynamische** jedoch legen fest, dass es Wert, den Sie müssen oder einen Parameterwert akzeptieren. Sie haben Benutzer der Vorlage übergeben einen Wert für den Namen der Domäne aktiviert.

Jetzt sehen wir uns **Anforderungsparameter ermitteln**. Der angegebene Wert entspricht einfach Version der REST-API, die Sie beim Erstellen der Ressource verwenden möchten. So sehen Sie die REST-API-Dokumentation für diesen Ressourcentyp. Oder führen Sie den folgenden PowerShell-Befehl für einen bestimmten Typ.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Die gibt folgende Werte zurück:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

API-Versionen mit Azure CLI finden Befehl die gleichen **Azure Anbieter anzeigen** gezeigt.

Beim Erstellen einer neuen Vorlage wählen Sie die neueste Version der API.

## <a name="virtual-network-and-subnet"></a>Virtuelles Netzwerk und Subnetz
Erstellen Sie ein virtuelles Netzwerk mit einem Subnetz. Sehen Sie sich die [REST API für virtuelle Netzwerke](https://msdn.microsoft.com/library/azure/mt163661.aspx) für alle Eigenschaften festgelegt.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>Lastenausgleich
Nun erstellen Sie eine externe mit Lastenausgleich. Da dieses System zum Lastenausgleich die öffentliche IP-Adresse verwendet, müssen Sie eine Abhängigkeit auf öffentliche IP-Adresse im Abschnitt **DependsOn** deklarieren. Dies bedeutet, dass das System zum Lastenausgleich nicht bereitgestellt wird, bis zum Abschluss der öffentlichen IP-Adresse bereitstellen. Ohne diese Abhängigkeit, erhalten Sie eine Fehlermeldung, da Ressourcen-Manager versucht, die Ressourcen parallel und versucht, Lastenausgleich öffentlichen IP-Adresse festlegen, die noch nicht vorhanden ist. 

Sie erstellt auch einen Backend-Adresspool, einige eingehende NAT-Regeln RDP in VMs und Load balancing Regel mit einem TCP-Port 80 in dieser Ressourcendefinition. Auschecken der [REST API für Lastenausgleich](https://msdn.microsoft.com/library/azure/mt163574.aspx) für alle Eigenschaften.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Netzwerkschnittstelle
2 Netzwerkschnittstellen für jede VM erstellt. Anstatt doppelte Einträge für die Netzwerkschnittstellen können die [copyIndex()-Funktion](resource-group-create-multiple.md) (als NicLoop bezeichnet) kopierschleife durchlaufen und Zahlen Netzwerkschnittstellen gemäß der `numberOfInstances` Variablen. Die Netzwerkschnittstelle hängt bei der Erstellung des virtuellen Netzwerks und Lastenausgleich. Virtuelles Netzwerk erstellen und Load Balancer Id definierten Subnetzes verwendet Load Balancer-Adresspool und die eingehenden NAT-Regeln.
Sehen Sie sich die [REST API für Netzwerkschnittstellen](https://msdn.microsoft.com/library/azure/mt163668.aspx) für alle Eigenschaften.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>Virtual machine
Erstellen Sie 2 virtuelle Computer mit copyIndex() Funktion wie Erstellen von [Netzwerkschnittstellen](#network-interface).
Die VM-Erstellung hängt das Speicherkonto network Interface und Verfügbarkeit festgelegt. Diese VM erstellt werden von einem Abbild Markt im Sinne der `storageProfile` -Eigenschaft - `imageReference` Bild Publisher, Angebot, Sku und Version definiert. Schließlich sieht Profil Diagnose um Diagnose für den virtuellen Computer zu aktivieren. 

Marketplace-Bild relevanten Eigenschaften suchen, folgen Sie [Wählen Sie virtuellen Computerimages Linux](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) oder [Windows virtuellen Computerimages](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) Artikel

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] Bilder von **3 Anbietern**veröffentlicht, müssen an eine andere Eigenschaft namens `plan`. Ein Beispiel finden in [dieser Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) aus der Galerie Schnellstart. 

Sie haben die Ressourcen für die Vorlage definieren.

## <a name="parameters"></a>Parameter

Definieren Sie im Parameterabschnitt die Werte, die angegeben werden können, wenn Sie die Vorlage bereitstellen. Nur legen Sie Parameter für Werte, die man während der Bereitstellung geändert werden soll. Einen Standardwert können für einen Parameter Sie, die verwendet wird, sofern nicht während der Bereitstellung. Die zulässigen Werte können auch definieren, wie für den **ImageSKU** -Parameter.

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Variablen

Im Variablenabschnitt können Sie Werte in mehr als einer Stelle in der Vorlage oder aus anderen Ausdrücke oder Variablen Werte definieren. Variablen werden häufig zur Vereinfachung der Syntax der Vorlage.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Sie haben die Vorlage! Sie können die Vorlage anhand der vollständigen Vorlage im [Schnellstart Gallery](https://github.com/Azure/azure-quickstart-templates) [2 VMs mit Lastenausgleich](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)und Lastenausgleich Regelvorlage laden. Die Vorlage geringfügig möglicherweise unterschiedliche Versionsnummern verwendet. 

Sie können die Vorlage erneut bereitstellen, die gleichen Befehle verwendet, wenn das Speicherkonto bereitstellen. Sie müssen nicht das Speicherkonto löschen, bevor Sie erneut bereitstellen, da Ressourcen-Manager neu erstellen Ressourcen überspringen, die bereits vorhanden und wurden nicht geändert.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Ressourcenmanager Vorlage Schnellansicht (ARMViz)](http://armviz.io/#/) ist ein hervorragendes Tool ARM Vorlagen visualisieren, wie groß verstehen von Json-Datei gelesen werden kann.
- Erfahren Sie mehr über die Struktur der Vorlage finden Sie unter [Erstellen von Azure-Ressourcen-Manager Vorlagen](resource-group-authoring-templates.md).
- Zum Bereitstellen einer Vorlage finden Sie unter [Bereitstellen einer Ressourcengruppe Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md)
