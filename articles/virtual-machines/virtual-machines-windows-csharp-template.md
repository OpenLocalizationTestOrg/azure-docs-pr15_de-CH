<properties
    pageTitle="Einen virtueller Computer mit C# Ressourcenmanager Vorlage bereitstellen | Microsoft Azure"
    description="Erfahren Sie, wie C# und Ressourcenmanager Vorlage verwenden, um eine VM Azure bereitstellen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Bereitstellen Sie ein Azure Virtual Machine mit C# Ressourcenmanager Vorlage

Ressourcengruppen und Vorlagen verwenden, können Sie alle zusammen Ressourcen verwalten, die die Anwendung unterstützt. Dieser Artikel beschreibt, wie Visual Studio und C#-Authentifizierung einrichten, erstellen Sie eine Vorlage verwenden und Bereitstellen von Azure-Ressourcen mit der, die von Ihnen erstellten Vorlage.

Zuerst müssen sicherstellen, dass dies folgendermaßen einrichten:

- Installieren Sie [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Überprüfen der Installation von [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Ein [Authentifizierungstoken](../resource-group-authenticate-service-principal.md) abrufen
- Erstellen einer Ressourcengruppe [Azure PowerShell](../resource-group-template-deploy.md), [Azure-CLI](../resource-group-template-deploy-cli.md)oder in [Azure-Portal](../resource-group-template-deploy-portal.md).

Es dauert etwa 30 Minuten, führen Sie folgende Schritte aus.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Schritt 1: Erstellen der Visual Studio-Projekt der Vorlagendatei und der Datei

### <a name="create-the-template-file"></a>Die Vorlagendatei erstellen

Eine Azure-Ressourcen-Manager-Vorlage ermöglicht bereitstellen und Verwalten von Azure Ressourcen zusammen. Die Vorlage ist eine JSON Ressourcen und zugeordnete Bereitstellungsparameter.

Führen Sie diese Schritte in Visual Studio:

1. Klicken Sie auf **Datei** > **neue** > **Projekt**.

2. In **Vorlagen** > **Visual C#** **Konsolenanwendungsprojekt**wählen, geben Sie Namen und Speicherort für das Projekt und klicken Sie auf **OK**.

3. Maustaste auf den Projektnamen im Projektmappen-Explorer, klicken Sie auf **Hinzufügen** > **Neu**.

4. Klicken Sie auf Web wählen Sie JSON-Datei aus, geben Sie *VirtualMachineTemplate.json* ein und klicken Sie dann auf **Hinzufügen**.

5. Fügen Sie in den öffnenden und schließenden Klammern der VirtualMachineTemplate.json Datei erforderlichen Schemaelement und erforderliche ContentVersion-Element:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parameter](../resource-group-authoring-templates.md#parameters) sind nicht immer erforderlich, aber sie können Werte eingeben, wenn die Vorlage bereitgestellt wird. Fügen Sie Parameter-Element und seine untergeordneten Elemente nach ContentVersion-Element:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Variablen](../resource-group-authoring-templates.md#variables) können in einer Vorlage verwendet werden, an Werte, die sich häufig ändern können oder Werte, die aus einer Kombination von Parameterwerten erstellt werden. Variablen-Element nach Parameterabschnitt hinzufügen:

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

8. [Ressourcen](../resource-group-authoring-templates.md#resources) wie der virtuelle Computer, virtuelle Netzwerk und das Speicherkonto werden anschließend in der Vorlage definiert. Fügen Sie im Ressourcenabschnitt nach Variablenabschnitt:

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
              "name": "myvnet1",
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
      
9. Speichern Sie Vorlagendatei, die Sie erstellt haben.

### <a name="create-the-parameters-file"></a>Erstellen der Datei

Um Werte für die Ressourcenparameter anzugeben, die in der Vorlage definiert wurden, erstellen Sie eine Datei mit den Werten, die verwendet werden, wenn die Vorlage bereitgestellt wird. Führen Sie diese Schritte in Visual Studio:

1. Maustaste auf den Projektnamen im Projektmappen-Explorer, klicken Sie auf **Hinzufügen** > **Neu**.

2. Klicken Sie auf Web wählen Sie JSON-Datei aus, geben Sie *Parameters.json* ein und klicken Sie dann auf **Hinzufügen**.

3. Öffnen Sie die Datei parameters.json, und fügen Sie diese JSON-Inhalte:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Legt einen virtuellen Computer eine Version von Windows Server-Betriebssystem. Erfahren Sie mehr über andere Bilder finden Sie unter [Navigieren und select Azure virtuellen Computerimages Azure-CLI mit Windows PowerShell](virtual-machines-linux-cli-ps-findimage.md).

4. Speichern Sie die Datei, die Sie erstellt haben.

## <a name="step-2-install-the-libraries"></a>Schritt 2: Installieren der Bibliotheken

NuGet-Pakete sind am einfachsten Bibliotheken installieren dieses Lernprogramm abgeschlossen. Sie benötigen Azure Resource Management Library und Azure Active Directory-Authentifizierungsbibliothek Ressourcen erstellen. Führen Sie diese Schritte, um diese Bibliotheken in Visual Studio zu erhalten:

1. Mit der rechten Maustaste im Projektmappen-Explorer des Projektnamen, und klicken Sie auf Durchsuchen auf **NuGet-Pakete verwalten**.

2. Typ *Active Directory* in das Suchfeld für das Active Directory-Authentifizierung Library Paket auf **Installieren** und gehen Sie zur Installation des Pakets.

4. Wählen Sie am oberen Rand der Seite **Include Prerelease**. Typ *Microsoft.Azure.Management.ResourceManager* in das Suchfeld für Microsoft Azure Management Ressourcenbibliotheken auf **Installieren** und gehen Sie zur Installation des Pakets.

Sie können nun die Bibliotheken verwenden zum Erstellen einer Anwendung.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Schritt 3: Erstellen Sie die Anmeldeinformationen zum Authentifizieren

Active Directory Azure-Anwendung erstellt und die Authentifizierungsbibliothek installiert. Jetzt formatiert die Anwendungsinformationen in Anmeldeinformationen zum Authentifizieren an Azure-Ressourcen-Manager verwendet werden.

1. Öffnen Sie die Datei "Program.cs" für das Projekt, das Sie erstellt haben, und fügen Sie diese using-Anweisung am Anfang der Datei hinzu:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Fügen Sie diese Methode der Klasse Programm das Token abrufen, die für die Anmeldeinformationen hinzu:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    Ersetzen Sie {Clientkennung} mit der ID von Azure Active Directory Anwendung {-Clientschlüssel} mit der Zugriffstaste Anwendung und {Tenant-Id} mit der Mieter für Ihr Abonnement. Die Mandanten-Id finden Sie Get-AzureRmSubscription ausführen. Die Zugriffstaste finden Sie mithilfe des Azure-Portals.

3. Um die Anmeldeinformationen zu erstellen, fügen Sie diesen Code für die Main-Methode in der Datei Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Speichern Sie die Datei Program.cs.

## <a name="step-4-deploy-the-template"></a>Schritt 4: Bereitstellen der Vorlage

In diesem Schritt verwenden die Ressourcengruppe, die Sie zuvor erstellt haben, aber Sie können auch eine Ressourcengruppe [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) mit [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) -Klassen erstellen.

1. Die Hauptmethode der Anwendung Klasse geben Sie die Namen der Ressourcen, die Sie zuvor erstellt haben, den Namen und die Abonnement-ID fügen Sie Variablen hinzu:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    Ersetzen Sie den Wert der GroupName mit den Namen der Ressourcengruppe ein. Ersetzen Sie den Wert des DeploymentName mit dem Namen für die Bereitstellung verwenden möchten. Abonnement-ID finden Sie Get-AzureRmSubscription ausführen.

2. Der Program-Klasse zum Bereitstellen von Ressourcen der Ressourcengruppe mithilfe der Vorlage, die Sie definiert fügen Sie diese Methode hinzu:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Wenn Sie die Vorlage ein Speicherkonto bereitstellen, können Sie die Template-Eigenschaft mit der Eigenschaft TemplateLink ersetzen.

3. Um die Methode aufzurufen, die Sie gerade hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Schritt 5: Ressourcen löschen

Da in Azure verwendeten Ressourcen berechnet ist es immer empfohlen, nicht mehr benötigte, Ressourcen löschen. Sie müssen nicht jede Ressource aus einer Ressourcengruppe einzeln löschen. Löschen Sie die Ressourcengruppe und alle Ressourcen automatisch gelöscht.

1.  Zum Löschen der Ressourcengruppe der Program-Klasse fügen Sie diese Methode hinzu:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Um die Methode aufzurufen, die Sie gerade hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Schritt 6: Führen Sie das Konsolenanwendungsprojekt

1.  Führen Sie das Konsolenanwendungsprojekt auf in Visual Studio **Starten** und melden Sie sich bei Azure AD mithilfe der Anmeldeinformationen, die Sie mit Ihrem Abonnement verwenden.

2.  Drücken Sie die **EINGABETASTE** , nachdem der Status akzeptiert wird.

    Etwa fünf Minuten für diese Konsolenanwendung vollständig aus starten dauert es Fertig stellen. Löschen Ressourcen EINGABETASTE dauert einige Minuten in die Erstellung von Ressourcen in Azure-Portal überprüfen, bevor Sie sie löschen.

3. Um die Ressourcen anzuzeigen, besuchen Sie die Überwachungsprotokolle in Azure-Portal:

    ![Überwachungsprotokolle in Azure-Portal durchsuchen](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Nächste Schritte

- Es gab Probleme mit der Bereitstellung, wäre Nächstes [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)betrachten.
- Erfahren Sie, wie der virtuelle Computer verwalten, die anhand der [Azure-Ressourcen-Manager mit PowerShell verwalten virtuelle Computer](virtual-machines-windows-csharp-manage.md)erstellt.
