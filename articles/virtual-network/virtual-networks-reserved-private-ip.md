<properties 
   pageTitle="Das interne private Adresse festlegen"
   description="Grundlegendes zu statischen internen IP-Adressen (DIPs) und Verwaltung"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Festlegen eine statische interne private IP-Adresse mithilfe von PowerShell (klassisch)
In den meisten Fällen müssen Sie eine statische interne IP-Adresse für den virtuellen Computer angeben. Virtuelle Computer in einem virtuellen Netzwerk erhalten automatisch eine interne IP-Adresse aus einem Bereich, die Sie angeben. Aber in bestimmten Fällen eine statische IP-Adresse für einen bestimmten virtuellen Computer angeben sinnvoll. Wenn z. B. die VM DNS ausgeführt wird oder ein Domänencontroller sein. Eine interne statische IP-Adresse bleibt mit der VM auch durch eine Stop-entziehen. 

> [AZURE.IMPORTANT] Azure hat zwei verschiedene Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und Classic](../resource-manager-deployment-model.md). Dieser Artikel behandelt das klassische Bereitstellungsmodell verwenden. Microsoft empfiehlt, die neue [Ressourcen-Manager-Bereitstellungsmodell](virtual-networks-static-private-ip-arm-ps.md)verwendet.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Wie Sie überprüfen, ob eine bestimmte IP-Adresse verfügbar ist
Überprüfen, ob die IP-Adresse *10.0.0.7* in einem Vnet namens *TestVnet*verfügbar ist, führen Sie den folgenden PowerShell-Befehl und überprüfen Sie den Wert für *steht*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Wenn Sie den Befehl über Richtlinien [erstellen ein virtuelles Netzwerk](virtual-networks-create-vnet-classic-portal.md) erstellt ein Vnet namens *TestVnet* und sicherzustellen, dass den Adressraum *10.0.0.0/8* verwendet in einem sicheren Umgebung testen möchten.

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Interne Adresse angeben, wenn Sie einen virtuellen Computer erstellen
Folgenden PowerShell-Skript erstellt einen neuen Clouddienst mit dem Namen *TestService*, dann Ruft ein Bild aus Azure und erstellt einen virtuellen Computer mit dem Namen *TestVM* im neuen Cloud-Dienst mit dem abgerufenen Bild legt die VM in einem Subnetz mit dem Namen *Subnetz 1*und legt *10.0.0.7* interne Adresse für den virtuellen Computer:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Abrufen von statischen internen IP-Informationen für eine VM
Zum Anzeigen der statischen internen IP-Informationen für die VM mit dem Beispielskript erstellt führen Sie folgenden PowerShell-Befehl aus und beobachten Sie die Werte für die *IP-Adresse*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Interne Adresse eines virtuellen Computers entfernen
Führen Sie folgenden PowerShell-Befehl Entfernen Sie die statische interne IP VM im obigen Skript hinzugefügt:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Interne Adresse auf einem vorhandenen virtuellen Computer hinzufügen
Hinzufügen eine statischen internen erstellt IP für die VM mit dem Skript über zwerg folgenden Befehl:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte

[Reservierte IP-Adresse](virtual-networks-reserved-public-ip.md)

[Auf Instanzebene öffentliche IP-Adresse (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
