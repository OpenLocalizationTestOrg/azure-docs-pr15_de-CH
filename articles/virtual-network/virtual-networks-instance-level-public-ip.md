<properties 
   pageTitle="Instanz auf öffentliche IP (ILPIP) | Microsoft Azure"
   description="Grundlegendes zu ILPIP (PIP) und deren Verwaltung"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="instance-level-public-ip-overview"></a>Instanz öffentlichen IP-Überblick
Eine Instanz auf öffentliche IP (ILPIP) ist eine öffentliche IP-Adresse, die Sie direkt auf die Instanz VM oder Rolle nicht die Instanz VM oder Rolle befinden sich im Cloud-Dienst zuweisen können. Dies statt nicht die VIP (virtual IP), die Cloud-Dienst zugeordnet ist. Vielmehr ist eine zusätzliche IP-Adresse, mit denen Sie direkt mit Ihrer Instanz VM oder Rolle verbunden.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](virtual-network-ip-addresses-overview-arm.md). 

Stellen Sie sicher, dass Sie verstehen [IP-Adressen](virtual-network-ip-addresses-overview-classic.md) in Azure.

>[AZURE.NOTE] In der Vergangenheit war ein ILPIP eines PIP genannt steht für öffentliche IP-Adresse. 

![Unterschied zwischen ILPIP und VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Wie in Abbildung 1 dargestellt, Cloud-Dienst erfolgt mithilfe einer VIP während der einzelnen VMs mit VIP normalerweise zugegriffen werden:&lt;port-Nummer&gt;. Durch Zuweisen einer ILPIP einer bestimmten VM VM direkt mit dieser IP-Adresse zugegriffen werden kann.

Wenn Sie in Azure Cloud-Dienst erstellen, werden entsprechende DNS A-Datensätze automatisch erstellt Zugriff auf den Dienst über einen vollqualifizierten Domänennamen (FQDN) anstelle der tatsächlichen VIP. Dabei geschieht ILPIP Zugang zu VM oder Rolle Instanz von FQDN anstelle der ILPIP. Beispielsweise wenn einen Cloud-Dienst mit dem Namen *Contosoadservice*erstellen und einer Webrolle namens *Contosoweb* mit zwei Instanzen konfigurieren Azure registriert die folgenden A-Datensätze für die Instanzen:

- Contosoweb\_IN_0.contosoadservice.cloudapp.net
- Contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] Sie können nur eine ILPIP für jede Instanz VM oder einer Rolle zuweisen. Sie können bis zu 5 ILPIP pro Abonnement. ILPIP wird zu diesem Zeitpunkt nicht für Multi-NIC VMs unterstützt.

## <a name="why-should-i-request-an-ilpip"></a>Warum sollte ein ILPIP anfordern?
Möchten Sie herstellen die Instanz VM oder Rolle einer IP-Adresse direkt zugewiesen werden, anstatt die Cloud-Dienst VIP:&lt;Portnummer&gt;, ein ILPIP für die VM oder die Rolleninstanz anfordern.
- **Passives FTP** - durch eine ILPIP auf die VM erhalten Sie Datenverkehr jeder Port Sie nicht haben, um einen Endpunkt empfangen zu öffnen. Dadurch können Szenarios wie Passives FTP, Ports dynamisch ausgewählt werden.
- **Ausgehendes IP** - ausgehender Datenverkehr von der VM geht mit ILPIP als Quelle und die VM auf externe Entitäten eindeutig identifiziert.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Wie eine ILPIP während der Erstellung virtueller Computer
Folgenden PowerShell-Skript erstellt einen neuen Clouddienst mit dem Namen *FTPService*, und ruft ein Bild aus Azure erstellt einen virtuellen Computer mit dem Namen *FTPInstance* abgerufene Bild verwenden, legt die VM mit einem ILPIP und fügt die VM auf den neuen Dienst:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>ILPIP-Informationen für eine VM abrufen
Zum Anzeigen von ILPIP Informationen für die VM mit dem Beispielskript erstellt führen Sie folgenden PowerShell-Befehl aus, und beobachten Sie die Werte für *Öffentl.IP* und *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Ein ILPIP eines virtuellen Computers entfernen
Führen Sie zum Entfernen der VM im obigen Skript hinzugefügt ILPIP folgenden PowerShell-Befehl:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Hinzufügen einer ILPIP zum vorhandenen VM
Um die VM mit dem Beispielskript erstellt eine ILPIP hinzuzufügen, führen Sie den folgenden Befehl ein:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>ILPIP einer VM mit einer Service-Konfigurationsdatei zuordnen
ILPIP einer VM stehen mit einer Service-Konfigurationsdatei (mithilfe). Die unten stehenden Beispiel zeigt einen Cloud-Dienst eine ILPIP namens *MyPublicIP* für eine Rolleninstanz Verwendung konfigurieren: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Nächste Schritte

- Verständnis der Funktionsweise von [IP-Adressen](virtual-network-ip-addresses-overview-classic.md) im klassischen Bereitstellungsmodell.

- Enthält Informationen Sie zu [reservierten IP-Adressen](virtual-networks-reserved-public-ip.md).
 