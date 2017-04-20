<properties
    pageTitle="Geplante Wartung für Windows-VMs | Microsoft Azure"
    description="Verstehen Sie, welche Azure Wartungsarbeiten und wie wirkt sich der virtuelle Windows Maschinen in Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-virtual-machines-in-azure"></a>Geplante Wartung für virtuelle Computer in Azure


Verstehen Sie, welche Azure Wartungsarbeiten ist und wie die Verfügbarkeit Ihrer virtuellen Maschinen Windows beeinträchtigen können. Dieser Artikel steht auch für [virtuelle Linux-Computer](virtual-machines-linux-planned-maintenance.md). 

Dieser Artikel erklärt, Azure Wartungsarbeiten Prozess. Wenn Sie wollen, Problembehandlung, wenn die VM neu gestartet, können Sie [dieser Blog Post detailliert anzeigen VM Protokolle starten](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="why-azure-performs-planned-maintenance"></a>Warum Azure führt geplante Wartung

Microsoft Azure führt in regelmäßigen Abständen Updates zu verbessern die Zuverlässigkeit, Leistung und Sicherheit von der Hostinfrastruktur, die virtuellen Maschinen zugrunde liegt. Viele dieser Updates erfolgt ohne Ihre virtuellen Maschinen oder Cloud-Dienste, einschließlich Updates Speicher beibehalten.

Einige Updates erfordern jedoch einen Neustart der virtuellen Maschinen mit der erforderlichen Updates der Infrastruktur. Die virtuellen Computer werden heruntergefahren, während wir die Infrastruktur patch und dann die virtuellen Computer neu gestartet.

Es sind zwei Arten von Wartung, die die Verfügbarkeit Ihrer virtuellen Maschinen auswirken können: geplante und ungeplante. Diese Seite beschreibt, wie Microsoft Azure Wartungsarbeiten durchgeführt. Weitere Informationen über nicht geplante Wartung finden Sie unter [Geplante verstehen und außerplanmäßige Instandhaltung](virtual-machines-windows-manage-availability.md).

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]
