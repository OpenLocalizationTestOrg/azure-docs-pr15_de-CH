<properties
   pageTitle="Übergabe von Anmeldeinformationen mit DSC Azure | Microsoft Azure"
   description="Übersicht über sichere Übergabe von Anmeldeinformationen an Azure virtuelle Maschinen mit PowerShell gewünschten Konfiguration"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Übergabe von Anmeldeinformationen an Azure DSC Erweiterungshandler #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Dieser Artikel behandelt die Konfiguration des gewünschten Erweiterung für Azure. Überblick der DSC-Erweiterungshandler finden unter [Einführung in Ereignishandler Erweiterung Azure gewünschten Konfiguration](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Übergeben von Anmeldeinformationen
Als Teil der Konfiguration können Sie Benutzerkonten einrichten auf Dienste zugreifen, oder installieren Sie ein Programm in einem Benutzerkontext. Wenn Sie dies tun, müssen Sie Anmeldeinformationen. 

DSC kann parametrisierte Konfigurationen in denen Anmeldeinformationen in der Konfiguration übergeben und sicher in MOF-Dateien gespeichert. Azure-Erweiterungshandler vereinfacht Berechtigungsmanagement indem automatische Verwaltung von Zertifikaten. 

Betrachten Sie das folgende Skript für DSC, die ein lokales Benutzerkonto mit dem angegebenen Kennwort:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Es ist wichtig, *Knoten Localhost* als Teil der Konfiguration. Fehlt diese Anweisung funktionieren die folgenden Schritte nicht wie Dateierweiterungshandler speziell für die Knoten Localhost-Anweisung. Es ist auch wichtig Typumwandlung *[PsCredential]*als diese Art die Erweiterung um die Anmeldeinformationen zu verschlüsseln löst. 

Veröffentlichen Sie dieses BLOB-Speicher:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Azure DSC-Erweiterung und die Anmeldeinformationen:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Wie sind die Anmeldeinformationen geschützt
Ausführung dieses Codes aufgefordert, Anmeldeinformationen. Wenn es angegeben wird, wird es kurz im Arbeitsspeicher gespeichert. Wenn erscheint mit `Set-AzureVmDscExtension` Cmdlet, es werden über HTTPS für die VM, bei Azure speichert auf der Festplatte mit dem lokalen VM-Zertifikat verschlüsselt. Kurz wird im Speicher entschlüsselt und erneut verschlüsselt an DSC übergeben.

Dieses Verhalten unterscheidet sich [sichere Konfigurationen ohne Erweiterungshandler](https://msdn.microsoft.com/powershell/dsc/securemof). Die Azure-Umgebung kann Daten sicher über Zertifikate übertragen. Verwendung den DSC-Erweiterungshandler besteht keine Notwendigkeit, $CertificatePath oder ein $CertificateID / $Thumbprint-Eintrag im ConfigurationData.


## <a name="next-steps"></a>Nächste Schritte ##

Informationen auf Azure DSC Erweiterung Ereignishandler finden Sie unter [Einführung in Ereignishandler Erweiterung Azure gewünschten Konfiguration](virtual-machines-windows-extensions-dsc-overview.md). 

Überprüfen der [Azure-Ressourcen-Manager-Vorlage für DSC-Erweiterung](virtual-machines-windows-extensions-dsc-template.md).

Weitere Informationen über PowerShell DSC [finden Sie auf der Mitte der PowerShell-Dokumentation](https://msdn.microsoft.com/powershell/dsc/overview). 

Um zusätzliche Funktionalität können Sie PowerShell DSC, [Durchsuchen der Galerie PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) Weitere DSC-Ressourcen verwalten.
