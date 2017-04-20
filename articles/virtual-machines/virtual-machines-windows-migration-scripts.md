<properties
    pageTitle="Gemeinschaftsinstrumente für Azure Azure Ressourcenmanager Migration"
    description="In diesem Artikel katalogisiert die Tools, die von der Gemeinschaft zugunsten der Migration IaaS-Ressourcen von Azure Service Management Stapel Azure-Ressourcen-Manager gewährt wurden."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Gemeinschaftsinstrumente für Azure Azure Ressourcenmanager Migration

In diesem Artikel katalogisiert die Tools, die von der Gemeinschaft zugunsten der Migration IaaS-Ressourcen von Azure Service Management Stapel Azure-Ressourcen-Manager gewährt wurden.

>[AZURE.NOTE]Diese Tools werden vom Support von Microsoft nicht offiziell unterstützt. Daher sind öffnen bezogen auf Github, und wir gerne PRs Updates oder zusätzliche Szenarien. Wenn Sie ein Problem melden, Funktion Github Probleme.
>
> Mit diesen Tools migrieren verursacht Ausfallzeiten für den klassischen virtuellen Computer. Wenn Plattformmigration suchen, besuchen Sie 
>
>- [Plattform-Migration IaaS-Ressourcen von Classic Azure Ressourcenmanager Stapel](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Technische Deep Dive Plattform unterstützt die Migration von Classic an Azure-Ressourcen-Manager](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [Migrieren Sie IaaS-Ressourcen von Classic, Azure-Ressourcen-Manager mit Azure PowerShell](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

Dies ist ein Skriptmodul PowerShell für die Migration der **einzelnen** virtuellen Computer (VM) von Azure Service Management Stack Azure Ressourcenmanager Stapel. 

[Verknüpfen Sie mit der Dokumentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

MigAz ist eine weitere Option umfassende Azure Service Management IaaS-Ressourcen in Azure Ressourcenmanager IaaS-Ressourcen migrieren. Die Migration möglich innerhalb des gleichen Abonnements oder zwischen verschiedenen Subskriptionen und Abonnements (ex: CSP-Abonnements).

[Verknüpfen Sie mit der Dokumentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)