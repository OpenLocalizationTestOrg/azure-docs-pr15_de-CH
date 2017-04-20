<properties
    pageTitle="Windows-VMs in einem Clouddienst verbinden | Microsoft Azure"
    description="Verbinden Sie mit dem klassischen Bereitstellungsmodell ein Azure-Cloud-Dienst oder virtuelles Netzwerk erstellt Windows virtuellen Computer."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Verbinden Sie mit dem klassischen Bereitstellungsmodell mit einem virtuellen Netzwerk oder Cloud-Dienst erstellt Windows virtuellen Computer

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Virtuelle Windows-Maschinen mit klassischen Bereitstellungsmodell erstellt werden immer in einem Clouddienst abgelegt. Cloud-Dienst fungiert als Container und bietet einen eindeutigen öffentlichen DNS-Namen, öffentliche IP-Adresse und Endpunkte auf dem virtuellen Computer über das Internet. Cloud-Dienst kann in einem virtuellen Netzwerk, aber nicht erforderlich ist. Sie können auch [virtuelle Linux-Computer mit einem virtuellen Netzwerk oder Cloud-Dienst herstellen](virtual-machines-linux-classic-connect-vms.md).

Cloud-Dienst in einem virtuellen Netzwerk nicht es einen *Eigenständiges* Cloud-Dienst genannt. Virtuelle Computer in einem eigenständigen Clouddienst können nur mit den anderen virtuellen Computern die öffentlichen DNS-Namen mit anderen virtuellen Computern kommunizieren, und der Verkehr über das Internet übertragen. Wenn ein Cloud-Dienst in einem virtuellen Netzwerk, die virtuellen Computer Cloud-Dienst mit anderen virtuellen Computern in der virtuellen Netzwerk werden ohne Datenverkehr über das Internet kommunizieren kann.

Setzen Sie die virtuellen Computer im gleichen eigenständige Cloud-Dienst können Sie weiterhin Lastausgleich und Verfügbarkeit legt. Details finden Sie im [Lastenausgleich virtuellen Maschinen](virtual-machines-windows-load-balance.md) und [Verwalten der Verfügbarkeit virtueller Maschinen](virtual-machines-windows-manage-availability.md). Jedoch kann nicht den virtuellen Computern in Subnetzen organisieren oder einen eigenständige Cloud-Dienst mit dem lokalen Netzwerk herstellen. Hier ist ein Beispiel:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Nächste Schritte

Nach der Erstellung einer virtuellen Maschine ist es eine gute Idee, [einen Datenträger hinzufügen](virtual-machines-windows-classic-attach-disk.md) haben Ihre Dienste und Arbeitslasten einen Speicherort zum Speichern von Daten. 