<properties
 pageTitle="VM-Erweiterungen und Funktionen | Microsoft Azure"
 description="Erfahren Sie, welche Erweiterungen für Azure virtuelle Computer gruppiert nach was sie oder verbessern."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>VM-Erweiterungen zu Funktionen

## <a name="azure-vm-extensions"></a>Azure VM Extensions

Azure Virtual Machine Extensions sind kleine Programme, die Konfiguration und Automatisierung Ausbringungstask des Post auf Azure Virtual Machines bieten. Beispielsweise erfordert einen virtuellen Computer Software installiert, Anti-Virus-Schutz oder Konfiguration Andockfenster Erweiterung VM dienen zum Ausführen dieser Aufgaben. Azure VM-Erweiterungen mit der Azure-CLI PowerShell ausgeführt werden können Vorlagen Ressourcen verwalten und Azure-Portal. Extensions können zusammen mit einer neuen VM-Bereitstellung oder vorhandenen System ausführen.

Dieses Dokument enthält Komponenten für Azure Virtual Machine-Erweiterung und Anleitung verfügbare VM-Erweiterungen zu erkennen. 

## <a name="azure-vm-agent"></a>Azure VM-Agent

Der Azure-VM-Agent verwaltet die Interaktion zwischen Azure Virtual Machine und Azure Fabric Controller. VM-Agent ist für viele funktionelle Aspekte der Bereitstellung und Verwaltung von Azure-Computer, einschließlich der Ausführung von VM-Erweiterungen verantwortlich. Azure VM Agent auf Azure Galeriebilder vorinstalliert und kann auf unterstützten Betriebssystemen installiert werden. 

Informationen zu unterstützten Betriebssystemen und Installationshinweise finden Sie in [Azure Linux Agent-Benutzerhandbuch](./virtual-machines-linux-agent-user-guide.md).

## <a name="discover-vm-extensions"></a>VM-Erweiterungen ermitteln

Viele andere VM-Erweiterungen stehen mit Azure Virtual Machines. Um eine vollständige Liste anzuzeigen, führen Sie den folgenden Befehl mit der Azure-CLI den Speicherort der gewünschten Position ersetzen.

```none
azure vm extension-image list westus
```

<br />

## <a name="common-vm-extensions"></a>VM-Erweiterungen

|Name der Erweiterung   |Beschreibung   |Weitere Informationen   |
|---|---|---|
|Benutzerdefiniertes Skript-Erweiterung für Linux  | Führen Sie Skripts mit einer Azure Virtual Machine  |[Benutzerdefiniertes Skript-Erweiterung für Linux](./virtual-machines-linux-extensions-customscript.md)   |
|Andockfenster Erweiterung |Den Daemon Andockfenster Remotebefehle Andockfenster Unterstützung installiert.  | [Andockfenster VM-Erweiterung](./virtual-machines-linux-dockerextension.md)  |
|VM-Erweiterung | Zugriff auf Azure Virtual Machine  |[VM-Erweiterung](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
|Erweiterung der Azure-Diagnose |Verwalten von Azure Diagnostics |[Erweiterung der Azure-Diagnose](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |

