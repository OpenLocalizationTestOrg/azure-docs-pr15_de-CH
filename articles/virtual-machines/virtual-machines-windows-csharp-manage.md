<properties
    pageTitle="Verwalten von virtuellen Computern mit Azure Ressourcenmanager C# | Microsoft Azure"
    description="Verwaltung virtueller Computer mithilfe von Azure-Ressourcen-Manager und C#."
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
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Verwalten von Azure Virtual Machines Azure Ressourcenmanager mit C#  

Die Aufgaben in diesem Artikel veranschaulicht das Verwalten virtueller Computer, z. B. starten, beenden und aktualisieren. Eine virtuelle Maschine müssen in einer Ressourcengruppe, die Aufgaben in diesem Artikel vorhanden.

Zum Ausführen der Aufgaben in diesem Artikel müssen wie folgt vor:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Ein Authentifizierungstoken](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Ein Visual Studio-Projekt erstellen und Installieren von Paketen

NuGet-Pakete sind am einfachsten Bibliotheken installieren, die Aufgaben in diesem Artikel. Bibliotheken, die in diesem Artikel installieren sind Azure Active Directory-Authentifizierung Library und die Anbieterbibliothek Ressource berechnet. Führen Sie diese Schritte, um die Bibliotheken in Visual Studio:

1. Klicken Sie auf **Datei** > **neue** > **Projekt**.

2. In **Vorlagen** > **Visual C#** **Konsolenanwendungsprojekt**wählen, geben Sie Namen und Speicherort für das Projekt und klicken Sie auf **OK**.

3. Klicken Sie der Projektname im Projektmappen-Explorer und dann auf **NuGet-Pakete verwalten**.

4. Typ *Active Directory* in das Suchfeld für das Active Directory-Authentifizierung Library Paket auf **Installieren** und gehen Sie zur Installation des Pakets.

5. Wählen Sie am oberen Rand der Seite **Include Prerelease**. Typ *Microsoft.Azure.Management.Compute* in das Suchfeld für Bibliotheken .NET berechnen auf **Installieren** und gehen Sie zur Installation des Pakets.

Sie können nun die Bibliotheken verwenden zum Verwalten der virtuellen Computer.

## <a name="set-up-the-project"></a>Einrichten des Projekts

Die Anwendung erstellt und die Bibliotheken werden installiert, erstellen Sie ein Token mithilfe der Anwendungsinformationen. Dieses Token zur Authentifizierung fordert, Azure-Ressourcen-Manager.

1. Öffnen Sie die Datei "Program.cs" für das Projekt, das Sie erstellt haben, und fügen Sie diese using-Anweisung am Anfang der Datei hinzu:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Fügen Sie Variablen für die Main-Methode der Program-Klasse den Namen der Ressourcengruppe und den Namen der virtuellen Computer und die Abonnement-ID an:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    Abonnement-ID finden Sie Get-AzureRmSubscription ausführen.
    
3. Abrufen eines Tokens, die benötigt wird, um die Anmeldeinformationen erstellen, diese Methode der Program-Klasse hinzufügen:

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
    
4. Um die Anmeldeinformationen zu erstellen, fügen Sie diesen Code der Main-Methode in Program.cs ein:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Speichern Sie die Datei Program.cs.

## <a name="display-information-about-a-virtual-machine"></a>Anzeigen von Informationen zu einem virtuellen Computer

1. Fügen Sie diese Methode der Klasse Programm im zuvor erstellten Projekt hinzu:

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Um die Methode aufzurufen, die Sie gerade hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Speichern Sie die Datei Program.cs.

4. Klicken Sie auf **Starten** in Visual Studio, und melden Sie sich Azure AD mit demselben Benutzernamen und Kennwort, die Sie mit Ihrem Abonnement verwenden.

    Beim Ausführen dieser Methode sollte etwa wie folgt angezeigt werden:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Eine virtuelle Maschine

Sie können einen virtuellen Computer auf zwei Arten beenden. Eine virtuelle Maschine und behalten alle Einstellungen aber weiterhin berechnet werden oder einen virtuellen Computer anhalten und ihn freigeben. Bei ein virtuellen Computer freigegeben ist, sind alle zugeordnete Ressourcen auch freigegeben und Rechnungsadressen enden dafür.

1. Kommentieren Sie Code der Main-Methode außer den Code, um die Anmeldeinformationen zuvor hinzugefügt.

2. Der Program-Klasse diese Methode hinzufügen:

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Wenn Sie den virtuellen Computer freigeben möchten, ändern Sie den Aufruf zum Herunterfahren in diesen Code:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Um die Methode aufzurufen, die Sie gerade hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf **Starten** in Visual Studio, und melden Sie sich Azure AD mit demselben Benutzernamen und Kennwort, die Sie mit Ihrem Abonnement verwenden.

    Den Status des virtuellen Computers geändert, so beendet sollte angezeigt werden. Wenn die Methode, die aufrufende Deallocate Status beendet (freigegeben) ausgeführt wurde.

## <a name="start-a-virtual-machine"></a>Starten eines virtuellen Computers

1. Kommentieren Sie Code der Main-Methode außer den Code, um die Anmeldeinformationen zuvor hinzugefügt.

2. Der Program-Klasse diese Methode hinzufügen:

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Um die Methode aufzurufen, die Sie gerade hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf **Starten** in Visual Studio, und melden Sie sich Azure AD mit demselben Benutzernamen und Kennwort, die Sie mit Ihrem Abonnement verwenden.

    Den Status des virtuellen Computers ausgeführt ändern sollte angezeigt werden.

## <a name="restart-a-running-virtual-machine"></a>Starten einer laufenden virtuellen Maschine

1. Kommentieren Sie Code der Main-Methode außer den Code, um die Anmeldeinformationen zuvor hinzugefügt.

2. Der Program-Klasse diese Methode hinzufügen:

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Um die Methode aufzurufen, die Sie gerade hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf **Starten** in Visual Studio, und melden Sie sich Azure AD mit demselben Benutzernamen und Kennwort, die Sie mit Ihrem Abonnement verwenden.

## <a name="resize-a-virtual-machine"></a>Ändern der Größe einer virtuellen Maschine

Dieses Beispiel zeigt die Größe eines ausgeführten virtuellen Computers zu ändern.

1. Kommentieren Sie Code der Main-Methode außer den Code, um die Anmeldeinformationen zuvor hinzugefügt.

2. Der Program-Klasse diese Methode hinzufügen:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Um die Methode aufzurufen, die Sie gerade hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf **Starten** in Visual Studio, und melden Sie sich Azure AD mit demselben Benutzernamen und Kennwort, die Sie mit Ihrem Abonnement verwenden.

    Die Größe des virtuellen Computers sich Standard_A1 sollte angezeigt werden.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Einem virtuellen Computer einen Datenträger hinzufügen

Dieses Beispiel zeigt, wie einem ausgeführten virtuellen Computer einen Datenträger hinzufügen.

1. Kommentieren Sie Code der Main-Methode außer den Code, um die Anmeldeinformationen zuvor hinzugefügt.

2. Der Program-Klasse diese Methode hinzufügen:

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Um die Methode aufzurufen, die Sie gerade hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf **Starten** in Visual Studio, und melden Sie sich Azure AD mit demselben Benutzernamen und Kennwort, die Sie mit Ihrem Abonnement verwenden.

## <a name="delete-a-virtual-machine"></a>Löschen Sie einen virtuellen Computer

1. Kommentieren Sie Code der Main-Methode außer den Code, um die Anmeldeinformationen zuvor hinzugefügt.

2. Der Program-Klasse diese Methode hinzufügen:

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Um die Methode aufzurufen, die Sie gerade hinzugefügt, fügen Sie diesen Code für die Main-Methode:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei Program.cs.

5. Klicken Sie auf **Starten** in Visual Studio, und melden Sie sich Azure AD mit demselben Benutzernamen und Kennwort, die Sie mit Ihrem Abonnement verwenden.

## <a name="next-steps"></a>Nächste Schritte

Würde Probleme eine Bereitstellung, können Sie [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) anzeigen
