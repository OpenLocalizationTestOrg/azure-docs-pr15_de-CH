<properties
    pageTitle="Legen Sie einen Datenträger Linux VM | Microsoft Azure"
    description="Wie Linux VM in Azure-Portal mit dem Ressourcen-Manager-Bereitstellungsmodell neuen oder vorhandenen Datenträger an."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Wie Datenträger einer Linux VM in Azure-portal

Dieser Artikel veranschaulicht die neue und vorhandene Festplatten zu einer virtuellen Linux-Maschine über Azure-Portal hinzufügen. Sie können auch [einen Datenträger, einen virtuellen Computer Windows Azure-Portal anfügen](virtual-machines-windows-attach-disk-portal.md). Bevor Sie dies tun, überprüfen Sie diese Tipps:

- Die Größe des virtuellen Computers steuert, wie viele Datenträger anfügen können. Einzelheiten finden Sie [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).
- Um Premium-Speicher zu verwenden, benötigen Sie einen virtuellen Computer DS- oder GS-Serie. Premium und Standard Speicherkonten Datenträgern können Sie mit diesen virtuellen Computern. Premium-Speicher ist in bestimmten Regionen verfügbar. Weitere Informationen finden Sie unter [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](../storage/storage-premium-storage.md).
- Datenträgern virtuellen Maschinen sind tatsächlich VHD-Dateien in einem Konto Azure-Speicher. Weitere Informationen finden Sie unter [Festplatten und Festplatten für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md).
- Für eine neue Festplatte müssen Sie zuerst erstellen, da Azure erstellt werden, wenn Sie sie anfügen.
- Für einen vorhandenen Datenträger muss die VHD-Datei in einem Konto Azure-Speicher verfügbar sein. Können Sie ein VHD bereits vorhanden, wenn es nicht an einen anderen virtuellen Computer oder Hochladen eigener VHD-Datei in das Speicherkonto.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem die Festplatte hinzugefügt wurde, müssen Sie für die Verwendung vorzubereiten. Im Betriebssystem des virtuellen Computers angezeigt: "wie: Initialisieren eines neuen Datenträgers Daten in Linux" in diesem [Artikel](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).
