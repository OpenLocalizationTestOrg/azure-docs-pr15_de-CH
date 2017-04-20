<properties
   pageTitle="Wie virtuellen Linux-Maschine tag | Microsoft Azure"
   description="Enthält Informationen Sie zum tagging einer virtuellen Linux-Maschine in Azure mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Wie Sie eine virtuellen Linux-Maschine in Azure tag

Dieser Artikel beschreibt verschiedene Methoden zum virtuellen Linux-Maschine in Azure durch den Ressourcen-Manager-Bereitstellungsmodell tag. Tags sind benutzerdefinierte Schlüssel-Wert-Paare direkt auf eine Ressource oder eine Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 15 Kategorien pro Ressource und die Ressourcengruppe. Tags für eine Ressource angelegt, zum Zeitpunkt der Erstellung oder einer vorhandenen Ressource hinzugefügt. Beachten Sie, dass Tags für Ressourcen über den Ressourcen-Manager-Bereitstellungsmodell nur unterstützt werden.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Tagging von Azure CLI

Zunächst [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-azure-resource-manager.md) und Ressourcen-Manager-Modus (`azure config mode arm`).

Für einen bestimmten virtuellen Computer, einschließlich der Tags mit diesem Befehl können Sie alle Eigenschaften anzeigen:

        azure vm show -g MyResourceGroup -n MyTestVM

Fügen Sie ein neues VM-Tag in der Azure-Befehlszeilenschnittstelle können Sie die `azure vm set` mit Tag Parameter **t -**Befehl:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Entfernen Sie alle Tags können Sie den Parameter **-T** in den `azure vm set` Befehl.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Da wir unsere Ressourcen Azure CLI und Portal Tags zugewiesen haben, betrachten einer Verwendungsdetails Tags in der Abrechnung Portal anzeigen.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nächste Schritte

* Über tagging Azure Ressourcen finden Sie unter [Azure Ressourcenmanager Übersicht][] und [Tags Azure Ressourcen zu verwenden][].
* Tags Verwalten von Azure Ressourcen helfen Sie finden Sie [Ihre Azure-Rechnung Verständnis][] und [Einblick in Ihre Microsoft Azure Ressourcenverbrauch][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure-Ressourcen-Manager (Übersicht)]: ../azure-resource-manager/resource-group-overview.md
[Mithilfe von Tags Organisieren Ihrer Azure-Ressourcen]: ../resource-group-using-tags.md
[Verstehen Ihre Azure-Rechnung]: ../billing/billing-understand-your-bill.md
[Einblicke in Ihre Microsoft Azure Ressourcenverbrauch]: ../billing-usage-rate-card-overview.md
