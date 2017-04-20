<properties 
   pageTitle="Wie private Adresse im klassischen Modus mit PowerShell | Microsoft Azure"
   description="Grundlegendes zu statische private IP-Adressen (DIPs) und im klassischen Modus und PowerShell Verwaltung"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-powershell"></a>Wie eine statische private IP-Adresse (classic) in PowerShell

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell. Sie können auch [statische private IP-Adresse in der Ressourcen-Manager-Bereitstellungsmodell verwalten](virtual-networks-static-private-ip-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Die folgenden Beispiel PowerShell Befehle erwarten eine einfache Umgebung bereits erstellt. Erstellen Sie Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, zunächst Testmodus [erstellen ein VNet](virtual-networks-create-vnet-classic-netcfg-ps.md)beschrieben.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Wie Sie überprüfen, ob eine bestimmte IP-Adresse verfügbar ist
Überprüfen, ob die IP-Adresse *192.168.1.101* in einem VNet namens *TestVNet*verfügbar ist, führen Sie den folgenden PowerShell-Befehl und überprüfen Sie den Wert für *steht*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Erwartete Ausgabe:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Statische private IP-Adresse angeben, wenn Sie einen virtuellen Computer erstellen
Folgenden PowerShell-Skript erstellt einen neuen Clouddienst mit dem Namen *TestService*und Azure Ruft ein Bild ab, erstellt einen virtuellen Computer mit dem Namen *DNS01* im neuen Cloud-Dienst mit dem abgerufenen Bild legt die VM in einem Subnetz mit dem Namen *FrontEnd*und legt *192.168.1.7* als statische private IP-Adresse für den virtuellen Computer:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Erwartete Ausgabe:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Statische private IP-Adressinformationen für einen virtuellen Computer abrufen
Um die statische private IP-Adressinformationen für die VM mit dem Beispielskript erstellt anzuzeigen, führen Sie den folgenden PowerShell-Befehl und beachten Sie die Werte für die *IP-Adresse*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Erwartete Ausgabe:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Statische private IP-Adresse eines virtuellen Computers entfernen
Entfernen Sie die statische private IP-Adresse hinzugefügt VM im obigen Skript folgenden PowerShell-Befehl ausführen:
    
    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Erwartete Ausgabe:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Statische private IP-Adresse zu einem vorhandenen virtuellen Computer hinzufügen
Hinzufügen eine statischen privaten IP-Adresse für die VM erstellt das Skript über zwerg folgenden Befehl:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Erwartete Ausgabe:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a>Nächste Schritte

- [Reservierte öffentliche IP-](virtual-networks-reserved-public-ip.md) Adressen erfahren.
- Erfahren Sie mehr über [öffentliche IP (ILPIP) auf Instanzebene](virtual-networks-instance-level-public-ip.md) Adressen.
- Wenden Sie sich an die [reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).
