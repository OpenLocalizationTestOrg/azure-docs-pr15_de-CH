<properties
   pageTitle="Automatisierung der Anwendungsbereitstellung mit der virtuellen | Microsoft Azure"
   description="Azure Virtual Machine DotNet Core-Lernprogramm"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Bereitstellung von Azure-Ressourcen-Manager-Vorlagen

Nachdem alle Azure infrastrukturelle Anforderungen identifiziert und eine Bereitstellungsvorlage übersetzt wurden, muss die Bereitstellung der Anwendung behandelt werden. Bereitstellung Hier bezieht Anwendungsbinärdateien auf Azure-Ressourcen installiert. Für die Musik Store Sample .net Core und IIS installiert und auf jedem virtuellen Computer konfiguriert werden müssen. Musik Shop-Binärdateien auf dem virtuellen Computer installiert werden müssen, und die Musik-Datenbank bereits erstellt.

Dieses Dokument beschreibt, wie Virtual Machine Extensions Bereitstellung und Konfiguration in Azure virtuelle Computer automatisieren können. Alle Abhängigkeiten und eindeutige Konfigurationen werden hervorgehoben. Für optimale Ergebnisse bereits eine Instanz der Lösung der Azure-Abonnement und zusammen mit der Azure-Ressourcen-Manager bereitgestellt. Die vollständige Vorlage – [Musik Shop-Bereitstellung unter Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows)finden Sie hier.

## <a name="configuration-script"></a>Skript

Virtual Machine Extensions sind spezielle Programme für virtuelle Computer Konfiguration Automatisierung ausgeführt. Extensions werden für viele bestimmte Anti-Virus, Protokollierung und Andockfenster Konfiguration. Die benutzerdefinierten Skripts Erweiterung kann verwendet werden, ein Skript für einen virtuellen Computer ausgeführt. Mit Musik Store Sample obliegt es benutzerdefiniertes Skript Erweiterung Windows virtuelle Computer konfigurieren und die Musik Store-Anwendung installieren.

Überprüfen Sie bevor Details wie virtuellen Extensions in Azure-Ressourcen-Manager-Vorlage deklariert werden das Skript ausgeführt wird. Dieses Skript konfiguriert Windows Virtual Machine Musik Store-Anwendung hosten. Beim Ausführen des Skripts alle benötigt und Software installiert, Musik Store-Anwendung vom Datenquellen-Steuerelement zu installieren, Vorbereiten der Datenbank. 

> In diesem Beispiel wird zu Demonstrationszwecken.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>VM-Skript-Erweiterung

VM-Erweiterungen kann mit der erweiterungsressource in Azure-Ressourcen-Manager-Vorlage für einen virtuellen Computer zur Buildzeit ausgeführt werden. Die Erweiterung kann mit dem Assistenten für Visual Studio Ressource oder gültigen JSON in die Vorlage einfügen hinzugefügt werden. Skript-Erweiterung Ressource ist in die VM-Ressource geschachtelt. Dies ist im folgenden Beispiel ersichtlich.

Folgen Sie diesem Link zum Beispiel JSON innerhalb der Ressourcen-Manager-Vorlage- [VM Skript Erweiterung](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339)anzuzeigen. 

Beachten Sie in der folgenden JSON, in GitHub gespeichert ist. Dieses Skript kann auch in Azure BLOB-Speicher gespeichert werden. Azure Resource Manager Vorlagen ermöglichen auch Skript Ausführungszeichenfolge erstellt werden, so dass Vorlage Parameterwerte als Parameter für die Ausführung von Skripten verwendet werden können. In diesem Fall Daten bereitgestellt, wenn die Vorlagen bereitstellen und diese Werte können dann verwendet werden, wenn das Skript ausgeführt.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Weitere Informationen zur Verwendung von benutzerdefinierten Skripts Erweiterung finden Sie unter [benutzerdefinierte Skript Extensions Resource Manager Vorlagen](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Nächstes

<hr>

[Mehr Azure Ressourcenmanager Vorlagen durchsuchen](https://github.com/Azure/azure-quickstart-templates)
