<properties
   pageTitle="Bereitstellen eine VM mit öffentlichen Adresse mit einer Vorlage im Ressourcenmanager | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von VMs mit öffentlichen Adresse mit einer Vorlage im Ressourcen-Manager"
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
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Bereitstellen einer VM mit öffentlichen Adresse mit einer Vorlage

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Öffentliche IP-Adressressourcen in einer Vorlagendatei

Anzeigen und downloaden Sie die [Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

Abschnitt wird die Definition der öffentlichen IP-Ressource basierend auf dem Szenario.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

Beachten Sie die **PublicIPAllocationMethod** -Eigenschaft auf *statische*festgelegt ist. Diese Eigenschaft kann *dynamisch* (Standardwert) oder *statisch*sein. Festlegen auf statische Garantien, die die öffentliche IP-Adresse nicht ändern.

Im folgenden Abschnitt wird die Zuordnung der öffentlichen IP-Adresse einer Netzwerkschnittstelle.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Beachten Sie die **Öffentl.IP** -Eigenschaft auf die **Id** einer Ressource mit dem Namen **Variables('webVMSetting').pipName**. Das ist der Name der öffentlichen IP-Ressource oben.

Schließlich ist in der **NetworkProfile** -Eigenschaft der erstellten VM Netzwerkschnittstelle oben aufgeführt.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Bereitstellen Sie die Vorlage mithilfe von auf Bereitstellung

Vorlage verfügbar im öffentlichen Repository verwendet Parameterdatei standardmäßig Werte mit das oben beschriebene Szenario generiert. Klicken Sie zum Bereitstellen dieser Vorlage auf Bereitstellung **Bereitstellen in Azure** in der Readme.md-Datei für die [VM mit statischen PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) -Vorlage. Ersetzen Sie ggf. die Standardparameterwerte und geben Sie Werte für die Parameter leer.  Führen Sie die Schritte im Portal zum Erstellen eines virtuellen Computers eine statische öffentliche IP-Adresse.

## <a name="deploy-the-template-by-using-powershell"></a>Bereitstellen der Vorlage mithilfe von PowerShell

Gehen Sie wie folgt vor Bereitstellung mithilfe von PowerShell heruntergeladene Vorlage.

1. Wenn Azure PowerShell noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) , und gehen Sie in den Schritten 1 bis 3.

2. In PowerShell-Konsole Ausführen des **Neu AzureRmResourceGroup** Cmdlets ggf. eine neue Ressourcengruppe erstellen. Wenn Sie bereits eine Ressourcengruppe erstellt haben, fahren Sie mit Schritt 3 fort.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Erwartete Ausgabe:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. Führen Sie im PowerShell-Konsole **Neu-AzureRmResourceGroupDeployment** -Cmdlet zum Bereitstellen der Vorlage.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Erwartete Ausgabe:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Bereitstellen der Vorlage mithilfe der Azure-CLI

Um die Vorlage mithilfe der Azure-CLI bereitstellen, gehen Sie die.

1. Wenn Azure CLI noch nie verwendet haben, führen Sie die Schritte im Artikel [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) und die Schritte für Ihr Abonnement [mit Azure-Abonnement aus der Azure-Befehlszeilenschnittstelle (CLI Azure)](../xplat-cli-connect.md) Artikel im Abschnitt "Azure Anmeldung interaktiv Authentifizierung verwendet" CLI herstellen.
2. Führen Sie den Befehl **Azure Config Modus** in Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    New mode is arm

3. Öffnen Sie die [Datei](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), wählen Sie seinen Inhalt und in einer Datei auf Ihrem Computer speichern. In diesem Beispiel werden die Parameter in einer Datei namens *parameters.json*gespeichert. Die Werte der Parameter in der Datei ggf. ändern, aber zumindest wird empfohlen, dass Sie den Wert für den Parameter AdminPassword eindeutig und komplexes Kennwort ändern.

4. Führen Sie das Cmdlet **Azure Bereitstellung erstellen** zum Bereitstellen neuen Vnets mithilfe der Vorlage und Parameter-Dateien heruntergeladen und über geändert. Im folgenden Befehl Ersetzen <path> durch den Pfad die Datei gespeichert. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Erwartete Ausgabe (Listen Parameterwerte verwendet):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
