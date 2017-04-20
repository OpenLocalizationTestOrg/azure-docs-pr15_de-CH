<properties
 pageTitle="VM-Erweiterungen und Funktionen | Microsoft Azure"
 description="Erfahren Sie, welche Erweiterungen für Azure virtuelle Computer gruppiert nach was sie oder verbessern."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>VM-Erweiterungen zu Funktionen

## <a name="azure-vm-extensions"></a>Azure VM Extensions

Azure Virtual Machine Extensions sind kleine Programme, die Konfiguration und Automatisierung Ausbringungstask des Post auf Azure Virtual Machines bieten. Beispielsweise erfordert einen virtuellen Computer Software installiert, Anti-Virus-Schutz oder Konfiguration Andockfenster Erweiterung VM dienen zum Ausführen dieser Aufgaben. Azure VM-Erweiterungen mit der Azure-CLI PowerShell ausgeführt werden können Vorlagen Ressourcen verwalten und Azure-Portal. Extensions können zusammen mit einer neuen VM-Bereitstellung oder vorhandenen System ausführen.

Dieses Dokument enthält Komponenten für Azure Virtual Machine-Erweiterung und Anleitung verfügbare VM-Erweiterungen zu erkennen. 

## <a name="azure-vm-agent"></a>Azure VM-Agent

Der Azure-VM-Agent verwaltet die Interaktion zwischen Azure Virtual Machine und Azure Fabric Controller. VM-Agent ist für viele funktionelle Aspekte der Bereitstellung und Verwaltung von Azure-Computer, einschließlich der Ausführung von VM-Erweiterungen verantwortlich. Azure VM Agent auf Azure Galeriebilder vorinstalliert und kann auf unterstützten Betriebssystemen installiert werden. 

Informationen zu unterstützten Betriebssystemen und Installationshinweise finden Sie in [Azure Virtual Machine-Agent](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>VM-Erweiterungen ermitteln

Viele andere VM-Erweiterungen stehen mit Azure Virtual Machines. Um eine vollständige Liste anzuzeigen, führen Sie den folgenden Befehl mit der Azure-CLI den Speicherort der gewünschten Position ersetzen.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>VM-Erweiterungen

|Name der Erweiterung   |Beschreibung   |Weitere Informationen   |
|---|---|---|
|Benutzerdefiniertes Skript-Erweiterung für Windows  | Führen Sie Skripts mit einer Azure Virtual Machine  |[Benutzerdefiniertes Skript-Erweiterung für Windows](./virtual-machines-windows-extensions-customscript.md)   |
|DSC-Erweiterung für Windows | PowerShell DSC (State Konfiguration) Erweiterung.  | [Andockfenster VM-Erweiterung](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Erweiterung der Azure-Diagnose | Verwalten von Azure Diagnostics |[Erweiterung der Azure-Diagnose](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
