## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>Ein VNet mit einer Netzwerk-Konfigurationsdatei von PowerShell erstellen

Azure mithilfe eine XML-Datei alle verfügbaren VNets an ein Abonnement definiert. Sie können diese Datei herunterladen bearbeiten zum Ändern oder löschen vorhandene VNets und neue erstellen. In diesem Dokument werden Sie lernen, genannt Network Configuration (oder Netcgf) Datei herunterladen und bearbeiten, um ein neues VNet erstellen. [Azure virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100.aspx) Weitere Informationen zu Netzwerk-Konfigurationsdatei zu überprüfen.

Gehen Sie folgendermaßen vor um ein VNet mit einer Netcfg Datei mit PowerShell zu erstellen.

1. Wenn Azure PowerShell noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../articles/powershell-install-configure.md) und gehen Sie bis Ende Azure anmelden und Ihr Abonnement auswählen.
2. Azure PowerShell-Konsole verwenden Sie das Cmdlet " **Get-AzureVnetConfig** " Netzwerk-Konfigurationsdatei durch Ausführen des folgenden Befehls herunterladen. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Erwartete Ausgabe:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Öffnen Sie die Datei, die Sie in Schritt 2 mit einer XML- oder Text-Editor-Anwendung gespeichert und nach der **<VirtualNetworkSites>** Element. Haben Sie alle Netzwerke bereits jedes Netzwerk erscheint als eigene **<VirtualNetworkSite>** Element.
4. Um das virtuelle Netzwerk in diesem Szenario beschriebene erstellen, fügen Sie das folgende XML direkt unter der **<VirtualNetworkSites>** Element:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

9.  Speichern Sie Konfiguration von Netzwerk.
10. Azure PowerShell-Konsole verwenden Sie das Cmdlet " **Set-AzureVnetConfig** " Netzwerk-Konfigurationsdatei durch Ausführen des folgenden Befehls hochladen. Beachten Sie die Ausgabe unter dem Befehl, sehen Sie **erfolgreich** unter **OperationStatus**. Wenn dies nicht der Fall, überprüfen Sie die XML-Datei Fehler.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Hier ist die erwartete Ausgabe für den Befehl oben:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. Azure PowerShell-Konsole verwenden Sie das Cmdlet " **Get-AzureVnetSite** " überprüfen, ob das neue Netzwerk mit dem folgenden Befehl hinzugefügt wurde. 

        Get-AzureVNetSite -VNetName TestVNet

    Hier ist die erwartete Ausgabe für den Befehl oben:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded