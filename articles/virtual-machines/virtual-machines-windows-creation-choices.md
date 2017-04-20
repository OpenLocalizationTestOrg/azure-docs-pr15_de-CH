<properties
    pageTitle="Verschiedene Verfahren zum Erstellen einer Windows-VM | Microsoft Azure"
    description="Listet die verschiedenen Methoden zu einem Windows-Computer mit Ressourcen-Manager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Verschiedene Methoden zum Erstellen von virtuellen Windows-Maschine mit Ressourcen-Manager

Azure bietet verschiedene Arten, einen virtuellen Computer erstellen, da virtuelle Computer für verschiedene Benutzer und Zwecke geeignet sind. Daher müssen Sie einige Optionen für die virtuelle Maschine und wie es erstellt. Dieser Artikel enthält eine Übersicht über diese Optionen und Links Anweisungen.

## <a name="azure-portal"></a>Azure-portal

Azure-Portal mit ist eine einfache Möglichkeit, einen virtuellen Computer zu testen, besonders wenn Sie nur Azure beginnen mit. 

[Erstellen einer virtuellen Maschine unter Windows über das portal](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Vorlage

Virtuelle Computer erfordern eine Kombination an Ressourcen (wie ein Sets und Speicherkonten). Bereitstellen, und jede Ressource einzeln verwalten, können Sie eine Vorlage Azure-Ressourcen-Manager erstellen, die bereitgestellt und Vorschriften alle Ressourcen in einer einzigen koordinierten Operation.

- [Erstellen einer virtuellen Windows-Maschine mit einer Ressourcen-Manager-Vorlage](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure PowerShell

Auf Wunsch an eine Befehls-Shell können Sie Azure PowerShell.

- [Erstellen einer Windows-VM mit PowerShell](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Verwenden Sie Visual Studio zum Erstellen, verwalten und Bereitstellen von VMs Azure Tools für Visual Studio und Azure SDK.

[Azure Tools für Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

