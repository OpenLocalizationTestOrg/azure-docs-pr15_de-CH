<properties
    pageTitle="Verwenden Sie Azure Resource Manager Vorlagen in Azure Stapel (Tenant-Entwickler) | Microsoft Azure"
    description="Informationen Sie zum Verwenden von Azure-Ressourcen-Manager Vorlagen in Azure Stapel bereitstellen und alle Ressourcen für die Anwendung in einer einzigen koordinierten Operation."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure-Ressourcen-Manager Vorlagen in Azure Stapel verwenden

Azure Ressourcenmanager Vorlagen Bereitstellung und alle Ressourcen für die Anwendung in einer einzigen koordinierten Operation. Sie definieren die Ressourcen für die Anwendung und wie sie bereitgestellt wird.  Sie können auch Vorlagen, um die Ressourcen in der Ressourcengruppe ändern erneut bereitstellen.

Diese Vorlagen können mit Microsoft Azure Stack-Portal, PowerShell, Befehlszeile und Visual Studio bereitgestellt werden.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

Die folgenden Vorlagen sind auf [GitHub](http://aka.ms/azurestackgithub)verfügbar:

## <a name="deploy-sharepoint-non-high-availability"></a>Bereitstellen von SharePoint (keine hohe Verfügbarkeit)

Mithilfe der PowerShell DSC-Erweiterung SharePoint 2013-Farm erstellen, die Folgendes enthält:

-   Ein virtuelles Netzwerk

-   Drei Speicherkonten

-   Zwei externe Lastenausgleich

-   Ein virtueller Computer als Domänencontroller für eine neue Gesamtstruktur mit einer Domäne konfiguriert

-   Eine VM als eigenständiger Server SQL Server 2014

-   Eine VM als eine Maschine SharePoint 2013 Farm konfiguriert

## <a name="deploy-ad-non-high-availability"></a>Bereitstellen von AD (keine hohe Verfügbarkeit)

Mithilfe der PowerShell DSC-Erweiterung ein AD Domain Controller-Servers erstellen, das Folgendes enthält:

-   Ein virtuelles Netzwerk

-   Ein Speicherkonto

-   Ein externer Lastenausgleich

-   Ein virtueller Computer als Domänencontroller für eine neue Gesamtstruktur mit einer Domäne konfiguriert

## <a name="deploy-adsql-non-high-availability"></a>Bereitstellen von AD/SQL (keine hohe Verfügbarkeit)

Mithilfe der Erweiterung PowerShell DSC SQL Server 2014 eigenständigen Server erstellen, der Folgendes enthält:

-   Ein virtuelles Netzwerk

-   Zwei Storage-Konten

-   Ein externer Lastenausgleich

-   Ein virtueller Computer als Domänencontroller für eine neue Gesamtstruktur mit einer Domäne konfiguriert

-   Eine VM als eigenständiger Server SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Verwenden Sie die PowerShell DSC-Erweiterung vorhandenen virtuellen Computer lokale Configuration Manager (LCM) konfigurieren und Azure-Konto DSC ziehen Automatisierungsserver registriert.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Erstellen Sie einen virtuellen Computer von einem Bild

Erstellen Sie einen virtuellen Computer aus ein benutzerdefiniertes Bild. Diese Vorlage stellt auch ein virtuelles Netzwerk (mit DNS), öffentliche IP-Adresse und Netzwerkschnittstelle.

## <a name="simple-vm"></a>Einfache VM

Bereitstellen einer einfachen Windows VM, die ein virtuelles Netzwerk (mit DNS), öffentliche IP-Adresse und Netzwerkschnittstelle enthält.

## <a name="cancel-a-running-template-deployment"></a>Abbrechen einer laufenden vorlagenbereitstellung

Verwenden Sie zum Abbrechen einer laufenden vorlagenbereitstellung der `Stop-AzureRmResourceGroupDeployment` PowerShell-Cmdlet.


## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit dem portal](azure-stack-deploy-template-portal.md)

[Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md)

