<properties
  pageTitle="Cloud-Dienst mit einem benutzerdefinierten Domänencontroller verbinden | Microsoft Azure"
  description="Erfahren Sie, wie Ihre Web-Worker-Rollen zu einer benutzerdefinierten AD-Domäne mit PowerShell und AD-Domäne herstellen"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Ein benutzerdefiniertes Active Directory-Domänencontroller in Azure gehostet Azure Cloud Services Rollen herstellen

Ein virtuelles Netzwerk (VNet) wird zunächst in Azure eingerichtet. Das VNet wird dann eine Active Directory-Domänencontroller (gehostet auf einem Azure Virtual Machine) hinzugefügt. Wir werden vorab erstellten VNet vorhandenen Cloud-Dienstverwaltungsrollen hinzufügen und anschließend mit dem Domänencontroller herstellen.

Bevor wir beginnen, einige Dinge zu beachten:

1.  Dieses Lernprogramms PowerShell, Bitte stellen Sie sicher, dass Sie Azure PowerShell installiert und einsatzbereit. Hilfe bei der Einrichtung von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

2.  Die Active Directory-Domänencontroller und Web-Worker-Rolle Instanzen müssen in der VNet.

Folgen Sie dieser Anleitung und wenn Probleme auftreten, lassen Sie uns einen Kommentar. Jemand melden Sie (Ja, wir lesen Kommentare).

3. Die Cloud verweist Netzwerk <mark>muss</mark> ein **Klassisches virtuelles Netzwerk**.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Sie können ein virtuelles Netzwerk in Azure mithilfe von Azure-Verwaltungsportal oder PowerShell erstellen. Für dieses Lernprogramm wird PowerShell verwendet. Erstellen Sie ein virtuelles Netzwerk mit klassischen Azure-Portal finden Sie unter [Virtuelles Netzwerk erstellen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Erstellen Sie einen virtuellen Computer

Abschluss Einrichten des virtuellen Netzwerks müssen Sie einen AD-Domänencontroller erstellen. In diesem Lernprogramm werden wir ein AD-Domänencontroller eine Azure Virtual Machine einrichten.

Hierzu erstellen Sie einen virtuellen Computer über PowerShell mithilfe der folgenden Befehle:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Die virtuellen Computer zu einem Domänencontroller heraufstufen
Zum Konfigurieren der virtuellen Computer als Active Directory-Domänencontroller müssen Sie den virtuellen Computer anmelden und konfigurieren.

Anmeldung für die VM können RDP über PowerShell abrufen, verwenden Sie die folgenden Befehle.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Nach Anmeldung die VM setup virtuellen Computers anhand der Anleitung zum [Einrichten von Ihren Kunden AD-Domänencontroller](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx)als Active Directory-Domänencontroller.

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Das virtuelle Netzwerk Cloud-Dienst hinzufügen

Als Nächstes müssen Sie die VNet die Cloud Service-Bereitstellung hinzufügen erstellten. Dazu Ändern der Cloud-Dienst mithilfe der Visual Studio oder den Editor Ihrer Wahl mithilfe Abschnitten hinzufügen.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Anschließend erstellen Sie das Projekt Cloud Services und Azure bereitstellen. Hilfe bei der Bereitstellung des Cloud Services-Pakets in Azure finden Sie unter [Erstellen und Bereitstellen einer Cloud-Dienst](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Ihre Web-Worker-Rollen in der Domäne herstellen

Nach der Bereitstellung Cloud-Dienstprojekt auf Azure Verbinden der Rolleninstanzen der benutzerdefinierten AD-Domäne mit der Erweiterung AD-Domäne. Hinzufügen der Erweiterung AD-Domäne zu der vorhandenen Bereitstellung von Cloud-Dienste und benutzerdefinierte Domäne führen Sie die folgenden Befehle in PowerShell:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

Und das ist es.

Cloud Services jetzt auf Ihren benutzerdefinierten Domänencontroller verbunden werden sollen. Möchten Sie mehr über die verschiedenen Optionen für die Erweiterung der AD-Domäne konfigurieren, verwenden Sie PowerShell Hilfe wie unten dargestellt.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
