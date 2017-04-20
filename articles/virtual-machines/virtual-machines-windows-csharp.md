<properties
    pageTitle="Azure Ressourcen mit C# | Microsoft Azure"
    description="Erfahren Sie mit C# und Azure Resource Manager Microsoft Azure Ressourcen erstellen."
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
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Ressourcen Sie Azure mit# 

Dieser Artikel veranschaulicht das Azure Ressourcen mit C# erstellen.

Zuerst müssen sicherstellen, dass Sie diese Aufgaben abgeschlossen haben:

- Installieren Sie [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Überprüfen der Installation von [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Ein [Authentifizierungstoken](../resource-group-authenticate-service-principal.md) abrufen

Es dauert etwa 30 Minuten, führen Sie folgende Schritte aus.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Schritt 1: Ein Visual Studio-Projekt erstellen und Installieren der Bibliotheken

NuGet-Pakete sind am einfachsten Bibliotheken installieren dieses Lernprogramm abgeschlossen. Zu den Bibliotheken, die in Visual Studio benötigen, führen Sie diese Schritte:

1. Klicken Sie auf **Datei** > **neue** > **Projekt**.

2. In **Vorlagen** > **Visual C#** **Konsolenanwendungsprojekt**wählen, geben Sie Namen und Speicherort für das Projekt und klicken Sie auf **OK**.

3. Klicken Sie der Projektname im Projektmappen-Explorer und dann auf **NuGet-Pakete verwalten**.

4. Typ *Active Directory* in das Suchfeld für das Active Directory-Authentifizierung Library Paket auf **Installieren** und gehen Sie zur Installation des Pakets.

5. Wählen Sie am oberen Rand der Seite **Include Prerelease**. Typ *Microsoft.Azure.Management.Compute* in das Suchfeld für Bibliotheken .NET berechnen auf **Installieren** und gehen Sie zur Installation des Pakets.

6. Typ *Microsoft.Azure.Management.Network* in das Suchfeld für .NET Netzwerkbibliotheken auf **Installieren** und gehen Sie zur Installation des Pakets.

7. Typ *Microsoft.Azure.Management.Storage* in das Suchfeld für Bibliotheken .NET Speicher auf **Installieren** und gehen Sie zur Installation des Pakets.

8. Geben Sie *Microsoft.Azure.Management.ResourceManager* in das Suchfeld, klicken Sie auf **Installieren** , um Management Ressourcenbibliotheken.

Sie können nun die Bibliotheken verwenden zum Erstellen einer Anwendung.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Schritt 2: Erstellen Sie die Anmeldeinformationen zum Authentifizieren

Jetzt formatiert die Anwendungsinformationen zuvor in Anmeldeinformationen erstellt, die zum Authentifizieren an Azure-Ressourcen-Manager verwendet werden.

1. Öffnen Sie die Datei "Program.cs" für das Projekt, das Sie erstellt haben, und fügen Sie diese using-Anweisung am Anfang der Datei hinzu:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Der Program-Klasse zur Erstellung des Tokens, die erforderlich ist, fügen Sie diese Methode hinzu:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    Ersetzen Sie {Clientkennung} mit der ID von Azure Active Directory Anwendung {-Clientschlüssel} mit der Zugriffstaste Anwendung und {Tenant-Id} mit der Mieter für Ihr Abonnement. Die Mandanten-Id finden Sie Get-AzureRmSubscription ausführen. Die Zugriffstaste finden Sie mithilfe des Azure-Portals.

3. Um die Methode aufzurufen, die Sie zuvor hinzugefügt, fügen Sie diesen Code für die Main-Methode in der Datei Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Speichern Sie die Datei Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Schritt 3: Registrieren der Ressourcenanbieter und Erstellen von Ressourcen

### <a name="register-the-providers-and-create-a-resource-group"></a>Der Provider registrieren und Erstellen einer Ressourcengruppe

Alle Ressourcen müssen in einer Ressourcengruppe enthalten. Vor dem Hinzufügen von Ressourcen zu einer Gruppe muss Ihr Abonnement mit dem Ressourcenanbieter registriert.

1. Fügen Sie Variablen auf die Hauptmethode der Program-Klasse die Namen angeben, die für die Ressourcen verwendet werden soll:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    Ersetzen Sie die Variable Werte mit der Kennung, die Sie verwenden möchten. Abonnement-ID finden Sie Get-AzureRmSubscription ausführen.

2. Zum Erstellen der Ressourcengruppe und Anbieter registrieren der Program-Klasse diese Methode hinzufügen:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Um die Methode aufzurufen, die Sie zuvor hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen

Ein [Speicherkonto](../storage/storage-create-storage-account.md) muss für den virtuellen Computer erstellt wird VHD-Datei gespeichert.

1. Erstellen Sie das Speicherkonto der Program-Klasse fügen Sie diese Methode hinzu:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Um die Methode aufzurufen, die Sie zuvor hinzugefügt, fügen Sie diesen Code auf die Hauptmethode der Program-Klasse:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Erstellen Sie die öffentliche IP-Adresse

Eine öffentliche IP-Adresse muss mit dem virtuellen Computer kommunizieren.

1. Der Program-Klasse fügen Sie diese Methode hinzu, um die öffentliche IP-Adresse des virtuellen Computers zu erstellen:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Um die Methode aufzurufen, die Sie zuvor hinzugefügt, fügen Sie diesen Code auf die Hauptmethode der Program-Klasse:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Virtuelle Netzwerk erstellen

Ein virtuelles Netzwerk muss eine virtuelle Maschine, die mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt.

1. Der Program-Klasse eine Subnetzmaske und ein virtuelles Netzwerk erstellen, fügen Sie diese Methode hinzu:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Um die Methode aufzurufen, die Sie zuvor hinzugefügt, fügen Sie diesen Code auf die Hauptmethode der Program-Klasse:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>Erstellen der Netzwerkschnittstelle

Eine VM benötigt eine Netzwerkschnittstelle virtuellen Netzwerk kommunizieren.

1. Erstellen Sie eine Netzwerkschnittstelle hinzufügen dieser Methode der Program-Klasse:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Um die Methode aufzurufen, die Sie zuvor hinzugefügt, fügen Sie diesen Code auf die Hauptmethode der Program-Klasse:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Erstellen einer Verfügbarkeit

Verfügbarkeitsgruppen erleichtern die Verwaltung der von der Anwendung verwendete virtuelle Computer verwalten.

1. Der Program-Klasse zum Erstellen der Verfügbarkeit fügen Sie dieser Methode hinzu:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Um die Methode aufzurufen, die Sie zuvor hinzugefügt, fügen Sie diesen Code auf die Hauptmethode der Program-Klasse:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Erstellen Sie einen virtuellen Computer

Unterstützenden Ressourcen erstellt, können Sie einen virtuellen Computer erstellen.

1. Erstellen Sie den virtuellen Computer der Program-Klasse fügen Sie diese Methode hinzu:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] In diesem Lernprogramm erstellt einen virtuellen Computer eine Version von Windows Server-Betriebssystem. Erfahren Sie mehr über andere Bilder finden Sie unter [Navigieren und select Azure virtuellen Computerimages Azure-CLI mit Windows PowerShell](virtual-machines-linux-cli-ps-findimage.md).

2. Um die Methode aufzurufen, die Sie zuvor hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Schritt 4: Ressourcen löschen

Da in Azure verwendeten Ressourcen berechnet ist es immer empfohlen, nicht mehr benötigte, Ressourcen löschen. Wenn Sie virtuelle Computer und die unterstützenden Ressourcen löschen möchten, müssen Sie lediglich die Ressourcengruppe löschen.

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

2.  Um die Methode aufzurufen, die Sie zuvor hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Schritt 5: Führen Sie das Konsolenanwendungsprojekt

1. Führen Sie das Konsolenanwendungsprojekt auf in Visual Studio **Starten** und melden Sie sich bei Azure AD mit demselben Benutzernamen und Kennwort, die Sie mit Ihrem Abonnement verwenden.

2. **Drücken Sie jeden Statuscode zu jeder Ressource zurückgegeben wird.** Nach dem Erstellen der virtuellen Computer werden Sie im nächsten Schritt vor dem Drücken der EINGABETASTE zum Löschen der Ressourcen.

    Etwa fünf Minuten für diese Konsolenanwendung vollständig aus starten dauert es Fertig stellen. Löschen Ressourcen EINGABETASTE dauert einige Minuten in die Erstellung von Ressourcen in Azure-Portal überprüfen, bevor Sie sie löschen.

3. Um die Ressourcen anzuzeigen, besuchen Sie die Überwachungsprotokolle in Azure-Portal:

    ![Überwachungsprotokolle in Azure-Portal durchsuchen](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Nächste Schritte

- Verwenden einer Vorlage zum Erstellen eines virtuellen Computers mithilfe der [Bereitstellung Azure Virtual Machine](virtual-machines-windows-csharp-template.md)mit C# und Ressourcenmanager Vorlage nutzen.
- Erfahren Sie, wie der virtuelle Computer verwalten, die anhand der [Azure-Ressourcen-Manager mit PowerShell verwalten virtuelle Computer](virtual-machines-windows-csharp-manage.md)erstellt.
