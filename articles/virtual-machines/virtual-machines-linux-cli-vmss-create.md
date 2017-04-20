<properties
    pageTitle="Legt fest welche VM skalieren? | Microsoft Azure"
    description="Erfahren Sie mehr über VM Maßstab legt."
    keywords="virtuellen Linux-Maschine legt virtuellen skalieren" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Was sind virtuelle Computer skalieren werden?

Virtual Machine Maßstab legt können mehrere VMs als Gruppe verwalten. Auf einer hohen Ebene haben Skalierung die folgenden vor- und Nachteile:

Vorteile:

1. Hohe Verfügbarkeit. Jede Skalierungsgruppe versetzt die virtuellen in eine Verfügbarkeit Set mit 5 Fehlerdomänen (FDs) und 5 Update Domains (UDs) Verfügbarkeit (für Weitere Informationen siehe FDs und UDs, [VM Verfügbarkeit](./virtual-machines-linux-manage-availability.md)). 
2. Einfache Integration mit Lastenausgleich Azure und App-Gateway.
3. Einfache Integration in Azure skalieren.
4. Vereinfachte Bereitstellung, Verwaltung und VMs zu bereinigen.
5. Allgemeine Windows- und Linux-Versionen sowie benutzerdefinierte Bilder unterstützen.

Nachteile:

1. Datenträger kann nicht VM-Instanzen in einer Skalierung angefügt werden. Stattdessen verwenden BLOB-Speicher, Azure Dateien, Azure-Tabellen oder anderen speicherlösung.

## <a name="quick-create-using-azure-cli"></a>Erstellen Sie Quick-Azure-CLI

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Allgemeine Informationen finden Sie der [Hauptseite für Skalierung legt](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Weitere Dokumentation finden Sie die [Hauptdokumentation Seite Skalierung festgelegt](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Beispielsweise suchen Ressourcenmanager Vorlagen mit Skalierung, in [Azure Schnellstart Vorlagen Github Repo](https://github.com/Azure/azure-quickstart-templates)"Vmss".

