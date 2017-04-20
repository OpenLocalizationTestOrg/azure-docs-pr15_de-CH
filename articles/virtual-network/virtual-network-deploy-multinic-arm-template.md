<properties
   pageTitle="Multi-NIC VMs mit einer Vorlage im Ressourcen-Manager bereitstellen | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von Multi-NIC VMs mit einer Vorlage im Ressourcen-Manager"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Bereitstellen von Multi-NIC VMs mit einer Vorlage

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Da zu diesem Zeitpunkt Sie VMs mit einer NIC und VMs mit mehreren NIcs in derselben Ressourcengruppe haben können, implementieren Sie die Back-End-Server in einer Ressourcengruppe und anderen Komponenten in einer anderen Sicherheitsgruppe. Die folgenden Schritte verwenden eine Ressourcengruppe mit dem Namen *IaaSStory* für die wichtigsten Ressourcengruppe und *IaaSStory-Back-End-* Back-End-Server.

## <a name="prerequisites"></a>Erforderliche Komponenten

Vor der Bereitstellung von Back-End-Servern müssen Sie die wichtigsten Ressourcengruppe mit den erforderlichen Ressourcen für dieses Szenario bereitstellen. Gehen Sie folgendermaßen vor um diese Ressourcen bereitzustellen.

1. Navigieren Sie zur [Vorlagenseite](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klicken Sie auf der Vorlagenseite neben **übergeordneten Ressourcengruppe** **Bereitstellen in Azure**.
3. Ggf. ändern Sie Parameterwerte, und führen Sie die Schritte im vorschauportal Azure die Ressourcengruppe bereitstellen.

> [AZURE.IMPORTANT] Stellen Sie sicher, dass Ihre Storage Namen eindeutig sind. Sie keinen Kontonamen doppelter Speicher in Azure.

## <a name="understand-the-deployment-template"></a>Verstehen der Bereitstellungsvorlage

Vor dem Bereitstellen der Vorlage dieser Dokumentation unbedingt verstehen, was. Die nachstehenden Schritte geben einen Überblick der betreffenden Vorlage.

1. Navigieren Sie zur [Vorlagenseite](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klicken Sie auf **azuredeploy.json** zu öffnen.
3. Beachten Sie folgenden *OsType* -Parameter. Dieser Parameter verwendet, welche VM für die Datenbank verwenden auswählen, sowie mehrere Betriebssysteme verknüpfte Einstellungen.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Die Liste der Variablen scrollen Sie und überprüfen Sie die Definition für die unten aufgeführten Variablen **DbVMSetting** . Er erhält eine Arrayelemente in der Variablen **DbVMSettings** . Wenn Sie mit Software-Entwicklung vertraut sind, können Sie **DbVMSettings** Variable Hashtable oder einer Dictionay anzeigen.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Angenommen, Sie Windows-VMs mit SQL im Back-End bereitstellen möchten. Sein Wert **OSType** *Fenster*und Variable **DbVMSetting** enthält das unten aufgeführten Element entspricht den ersten Wert in der Variablen **DbVMSettings** .

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. Beachten Sie, dass der **VmSize** den Wert *Standard_DS3*enthält. Nur bestimmte VM-Größen ermöglichen die Verwendung von mehreren NICs. Sie können überprüfen, welche VM Multi NIC auf [mehrere Netzwerkkarten Überblick](virtual-networks-multiple-nics.md)aktiviert sind.
7. Scrollen Sie **Ressourcen** , und beachten Sie das erste Element. Ein Speicherkonto beschrieben. Dieses Speicherkonto verwendet jede VM-Datenbank verwendeten Datenträger verwalten. In diesem Szenario hat jede Datenbank VM ein Betriebssystemdatenträger im regulären Speicher gespeichert und zwei Datenträger im SSD (Premium) Speicher gespeichert.

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Bildlauf zur nächsten Ressource wie unten aufgeführt. Diese Ressource stellt die Netzwerkkarte für den Datenbankzugriff in jeder Datenbank VM verwendet. Beachten Sie die Verwendung der Funktion **Kopieren** in dieser Ressource. Die Vorlage können Sie beliebig viele VMs, die basierend auf dem **DbCount** -Parameter werden soll, bereitstellen. Daher müssen Sie die gleiche Netzwerkkarten für den Datenbankzugriff für jede VM erstellen.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Bildlauf zur nächsten Ressource wie unten aufgeführt. Diese Ressource stellt in jeder Datenbank VM verwendete NIC. Wieder benötigen Sie eine dieser NICs für jede Datenbank VM. Beachten Sie das **NetworkSecurityGroup** -Element verknüpfen eine NSG, die RDP/SSH diesen NIC zuzugreifen.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Bildlauf zur nächsten Ressource wie unten aufgeführt. Diese Ressource stellt eine Verfügbarkeit von VMs Datenbank verwendet werden soll. Auf diese Weise garantieren, dass werden einer VM in den Wartungsarbeiten ausführen.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Bildlauf zur nächsten Ressource. Diese Ressource steht für die Datenbank VMs, wie in der ersten aufgeführten Zeilen. Beachten Sie die Verwendung der Funktion **Kopieren** erneut, damit mehrere VMs basierend auf der **DbCount** -Parameter erstellt werden. Beachten Sie auch die **DependsOn** -Auflistung. Es listet zwei NICs erforderlich erstellt werden, bevor die VM und die Verfügbarkeit und das Speicherkonto bereitgestellt wird.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Einen Bildlauf in der VM-Ressource auf das Element **NetworkProfile** wie unten aufgeführt. Beachten Sie, dass zwei Netzwerkkarten Verweis für jeden virtuellen Computer. Beim Erstellen mehrerer Netzwerkkarten für einen virtuellen Computer müssen Sie die **Primärschlüssel** -Eigenschaft eines NICs auf *true*und den Rest auf *false*festlegen.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Bereitstellen Sie ARM-Vorlage mithilfe von auf Bereitstellung

> [AZURE.IMPORTANT] Stellen Sie sicher, dass Sie die [erforderlichen Komponenten](#Pre-requisites) Schritte vor wie folgt.

Vorlage verfügbar im öffentlichen Repository verwendet Parameterdatei standardmäßig Werte mit das oben beschriebene Szenario generiert. Zum Bereitstellen dieser Vorlage auf bereitstellen, folgen [diesem Link](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)rechts vom **Back-End-Ressourcengruppe (siehe Dokumentation)** auf **Azure bereitstellen**, ersetzen die Standardparameterwerte ggf. und gehen in das Portal.

Die nachfolgende Abbildung zeigt den Inhalt der neuen Ressourcengruppe nach der Bereitstellung.

![Back-End-Ressourcengruppe](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>Bereitstellen der Vorlage mithilfe von PowerShell

Gehen Sie wie folgt vor Bereitstellung mithilfe von PowerShell heruntergeladene Vorlage.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Führen Sie die **`New-AzureRmResourceGroup`** Cmdlet eine Ressourcengruppe mit der Vorlage erstellen.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Erwartete Ausgabe:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Bereitstellen der Vorlage mithilfe der Azure-CLI

Um die Vorlage mithilfe der Azure-CLI bereitstellen, gehen Sie die.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie die **`azure config mode`** Befehl Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    New mode is arm

3. Öffnen Sie die [Parameterdatei](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), seinen Inhalt und in einer Datei auf Ihrem Computer speichern. In diesem Beispiel werden die Datei in *parameters.json*gespeichert.

4. Ausführen der **`azure group deployment create`** -Cmdlet zum Bereitstellen neuen Vnets mithilfe der Vorlage und Parameter Dateien heruntergeladen und über geändert. Die Liste nach die Ausgabe der Parameter erläutert.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Erwartete Ausgabe:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
