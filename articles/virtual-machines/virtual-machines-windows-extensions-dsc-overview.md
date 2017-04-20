<properties
   pageTitle="Gewünschten Status Konfiguration Azure Übersicht | Microsoft Azure"
   description="Übersicht für mit Microsoft Azure Erweiterungshandler für PowerShell gewünschten Konfiguration. Komponenten, Architektur, Cmdlets einschließlich..."
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

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Einführung in Ereignishandler Erweiterung Azure gewünschten Konfiguration #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure VM Agent und zugeordnete Erweiterung gehören Microsoft Azure-Infrastrukturdienste. VM-Erweiterungen sind Softwarekomponenten, die VM-Funktionen erweitern und verschiedene Verwaltungsvorgänge VM zu vereinfachen. Z. B. die VMAccess verwendet werden, ein Administratorkennwort zurücksetzen oder die benutzerdefinierten Skripts verwendet werden, ein Skript auf dem virtuellen Computer auszuführen.

Dieser Artikel stellt die Erweiterung PowerShell gewünschten Zustand Konfiguration (DSC) für Azure VMs als Teil der Azure PowerShell SDK. Sie können neuen Cmdlets hochladen und PowerShell DSC-Konfiguration auf eine Azure-VM mit PowerShell DSC-Erweiterung aktiviert. Die PowerShell DSC Erweiterung Aufrufe in PowerShell DSC empfangene DSC-Konfiguration des virtuellen Computers zu erlassen. Diese Funktion ist auch über Azure-Portal.

## <a name="prerequisites"></a>Erforderliche Komponenten ##
**Lokalen Computer** Interaktion mit Azure VM-Erweiterung müssen Sie Azure-Portal oder Azure PowerShell SDK verwenden. 

**Gast-Agent** Der Azure-VM in der DSC konfiguriert werden muss ein Betriebssystem, das Windows Management Framework (WMF) 4.0 oder 5.0 unterstützt. Die vollständige Liste der unterstützten Betriebssystemversionen finden [DSC Erweiterung Versionsverlauf](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Begriffe und Konzepte ##
Dieses Handbuch setzt Kenntnisse der folgenden Konzepte voraus:

Konfiguration - ein DSC Dokument. 

Knoten - Ziel für DSC-Konfiguration. In diesem Dokument bezieht sich immer auf eine Azure-VM "Knoten".

Konfiguration - eine psd1 Datei mit Umgebungsdaten für eine Konfiguration

## <a name="architectural-overview"></a>Übersicht über die Architektur ##

Azure DSC-Erweiterung verwendet das Framework Azure VM-Agent zu liefern, erlassen und DSC-Konfigurationen auf Azure VMs melden. DSC-Erweiterung erwartet ZIP-Datei mindestens ein Dokument und mehrere Parameter durch Azure PowerShell SDK oder Azure-Portal.

Wenn die Erweiterung zum ersten Mal aufgerufen wird, wird eine Installation ausgeführt. Dieser Prozess installiert eine Version von Windows Management Framework (WMF) unter Verwendung der folgenden Logik:

1. Wenn die Azure VM Betriebssystem Windows Server 2016, wird keine Aktion ausgeführt. Windows Server 2016 bereits die neueste Version von PowerShell installiert.
2. Wenn die `wmfVersion` angegeben wird, werden diese Version des WMF ist installiert, wenn sie mit dem Betriebssystem des virtuellen Computers nicht kompatibel ist.
3. Wenn kein `wmfVersion` angegeben wird, werden die neueste Fassung der WMF installiert.

Installation von WMF erfordert einen Neustart. Nach dem Neustart die Erweiterung die ZIP-Datei im angegebenen downloads der `modulesUrl` Eigenschaft. Ist diese Platzierung in Azure BLOB-Speicher, kann ein SAS-Token angegeben der `sasToken` -Eigenschaft auf die Datei zugreifen. Nachdem heruntergeladen und entpackt ZIP der Konfigurationsfunktion gemäß `configurationFunction` ausführen, um die MOF-Datei zu generieren. Die Erweiterung wird dann `Start-DscConfiguration -Force` generierten MOF-Datei. Die Erweiterung zeichnet Ausgabe und zurückgeschrieben Azure Status Kanal. Ab diesem Punkt behandelt DSC LCM, Überwachung und Korrektur Normal. 

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets ##

PowerShell-Cmdlets können mit ARM oder ASM Verpacken, veröffentlichen und Überwachung DSC Erweiterung Bereitstellung verwendet werden. Die folgenden Cmdlets aufgelistet sind die ASM-Module "Azure" durch "AzureRm" der ARM Modell ersetzt werden Beispielsweise `Publish-AzureVMDscConfiguration` ASM verwendet, `Publish-AzureRmVMDscConfiguration` ARM verwendet. 

`Publish-AzureVMDscConfiguration`wird in einer Konfigurationsdatei und erstellt eine ZIP-Datei mit der Konfiguration und DSC Ressourcen darin, die Konfiguration DSC Ressourcen sucht. Sie können das Paket lokal mit dem `-ConfigurationArchivePath` Parameter. Andernfalls Azure BLOB-Speicher die ZIP-Datei veröffentlicht und Sichern Sie es mit einem SAS-Token.

Die ZIP-Datei erstellt dieses Cmdlet hat. ps1-Skript im Stammverzeichnis des Ordners. Ressourcen haben Modul Ordner im Archivordner gespeichert. 

`Set-AzureVMDscExtension`Fügt die Einstellungen von PowerShell DSC-Erweiterung in einer VM-Konfigurationsobjekt, ein Azure VM mit angewendet werden kann `Update-AzureVM`.

`Get-AzureVMDscExtension`Ruft den Status DSC-Erweiterung einer bestimmten VM. 

`Get-AzureVMDscExtensionStatus`Ruft den Status der vom Handler Erweiterung DSC DSC-Konfiguration. Diese Aktion kann auf einer einzelnen VM oder Gruppe.

`Remove-AzureVMDscExtension`Entfernt den Erweiterungshandler einen bestimmten virtuellen Computer. Dieses Cmdlet wird die Konfiguration **nicht** Entfernen deinstallieren WMF, oder angewendeten Einstellungen des virtuellen Computers. Nur entfernt den Erweiterung Ereignishandler. 

**Hauptunterschiede ASM und ARM Cmdlets**

- ARM-Cmdlets sind synchron. ASM Cmdlets sind asynchron.
- ResourceGroupName, VMName, ArchiveStorageAccountName, Version und Speicherort sind alle neuen erforderlichen Parameter.
- ArchiveResourceGroupName ist ein neuer optionaler Parameter für ARM. Sie können diesen Parameter angeben, wenn das Speicherkonto zu einer anderen Ressourcengruppe als gehört, in dem der virtuelle Computer erstellt.
- ConfigurationArchive heißt ArchiveBlobName in ARM
- ContainerName heißt ArchiveContainerName in ARM
- StorageEndpointSuffix heißt ArchiveStorageEndpointSuffix in ARM
- AutoUpdate-Schalter wurde hinzugefügt ARM aktivieren automatische Aktualisierung des Erweiterung Handlers auf die neueste Version und verfügbar ist. Inhaltsindexserver dieser Parameter verursachen hat Neustart des virtuellen Computers bei eine neue Version des WMF veröffentlicht. 


## <a name="azure-portal-functionality"></a>Azure Portalfunktionalität ##
Wechseln Sie zum klassischen VM. Unter Optionen -> Allgemein auf "Extensions". Ein neues Fenster wird erstellt. Klicken Sie auf "Hinzufügen" und wählen Sie PowerShell DSC.

Das Portal braucht.
**Konfiguration-Module oder Skript**: Dieses Feld ist obligatorisch. Erfordert eine. ps1-Datei mit einem Skript oder einer ZIP-Datei mit einer ps1-Konfigurationsskript, und alle abhängigen Ressourcen im Modul-Ordner in die ZIP. Erstellt werden, mit den `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` Cmdlet in Azure PowerShell SDK enthalten. Die ZIP-Datei wird in der Benutzer-BLOB-Speicher gesichert durch ein SAS-Token hochgeladen. 

**PSD1 Konfigurationsdatei**: Dieses Feld ist optional. Konfiguration der Konfigurationsdatei in psd1 erforderlich ist, verwenden dieses Feld und dem Benutzer BLOB-Speicher hochladen, wo sie ein SAS-Token gesichert. 
 
**Module-Qualified der Konfiguration**: ps1-Dateien können mehrere Konfigurationsfunktionen. Geben Sie das Skript für ps1 gefolgt von einer "\' und den Namen der Konfigurationsfunktion. Beispielsweise wenn Ihre. ps1-Skript hat den Namen "configuration.ps1" die Konfiguration ist "IisInstall", geben Sie:`configuration.ps1\IisInstall`

**Konfiguration Argumente**: Wenn der Konfigurationsfunktion Argumente geben Sie diese hier im Format `argumentName1=value1,argumentName2=value2`. Hinweis: dieses Format ein anderes Format als PowerShell-Cmdlets oder Ressourcenmanager Vorlagen wie Konfiguration Argumente akzeptiert werden. 

## <a name="getting-started"></a>Erste Schritte ##

Azure DSC-Erweiterung erhält DSC Konfigurationsdokumente und Azure VMs erlässt. Es folgt ein einfaches Beispiel für eine Konfiguration. Lokal speichern Sie als "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Die folgenden Schritte legen Sie das Skript IisInstall.ps1 auf dem angegebenen virtuellen Computer, führen Sie die Konfiguration und Status Berichten.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Protokollierung ##

Protokolle befinden:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[Versionsnummer]

## <a name="next-steps"></a>Nächste Schritte ##

Weitere Informationen über PowerShell DSC [finden Sie auf der Mitte der PowerShell-Dokumentation](https://msdn.microsoft.com/powershell/dsc/overview). 

Überprüfen der [Azure-Ressourcen-Manager-Vorlage für DSC-Erweiterung](virtual-machines-windows-extensions-dsc-template.md
). 

Um zusätzliche Funktionalität können Sie PowerShell DSC, [Durchsuchen der Galerie PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) Weitere DSC-Ressourcen verwalten.

Ausführliche vertrauliche Parameterübergabe in Konfigurationen finden Sie unter [Verwalten von Anmeldeinformationen mit DSC Erweiterungshandler](virtual-machines-windows-extensions-dsc-credentials.md).
