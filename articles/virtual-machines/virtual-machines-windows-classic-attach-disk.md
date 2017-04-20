<properties
    pageTitle="Fügen Sie einen Datenträger einer VM | Microsoft Azure"
    description="Legen Sie einen Datenträger auf einem Windows-Computer mit dem klassischen Bereitstellungsmodell erstellt und initialisiert."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Legen Sie einen Datenträger auf einem Windows-Computer mit dem klassischen Bereitstellungsmodell erstellt

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Wenn Sie das neue Portal verwenden möchten, finden Sie unter [Legen Sie einen Datenträger, einen virtuellen Computer Windows Azure-Portal](virtual-machines-windows-attach-disk-portal.md).

Benötigen Sie einen zusätzlichen Datenträger können Sie eine leere Diskette oder einen vorhandenen Datenträger mit Daten einer VM anfügen. In beiden Fällen sind die Laufwerke VHD-Dateien in einem Konto Azure-Speicher. Bei einem neuen Datenträger nach der Zuordnung der Festplatte müssen auch Sie bereit für die Verwendung von Windows-VM initialisieren.

Weitere Informationen zu Datenträgern finden Sie unter [über Laufwerke und Festplatten für virtuelle Computer](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Initialisieren Sie den Datenträger

1. Verbinden Sie mit dem virtuellen Computer. Informationen finden Sie unter [Anmelden bei einem virtuellen Computer unter Windows Server][logon].

2. Nachdem Sie den virtuellen Computer anmelden, öffnen Sie **Server-Manager**. Wählen Sie im linken Fensterbereich **Datei und**.

    ![Öffnen des Server-Managers](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Erweitern Sie das Menü, und wählen Sie **Festplatten**.

4. Die **Festplatten** werden Datenträger aufgeführt. In den meisten Fällen müssen sie Datenträger 0, Datenträger 1 und Datenträger 2. Festplatte 0 ist die Betriebssystem-Datenträger, Datenträger 1 ist die temporäre Diskette und Datenträger 2 Datenträger, nur die VM zugeordnet, ist. Neue Datenträger werden die Partition als **unbekannt**aufgeführt. Maustaste auf die Festplatte und wählen Sie **Initialisieren**.

5.  Sie werden benachrichtigt, dass alle Daten gelöscht werden, wenn das Laufwerk initialisiert ist. Klicken Sie auf **Ja,** um die Warnung bestätigen und initialisieren Sie den Datenträger. Danach wird die Partition als **GPT**aufgeführt. Mit der rechten Maustaste erneut auf des Datenträgers, und wählen Sie **Neues Volume**.

6.  Führen Sie den Assistenten unter Verwendung der Standardwerte. Wenn der Assistent abgeschlossen ist, listet Abschnitt **Volumes** das neue Volume. Der Datenträger ist jetzt online und bereit zum Speichern von Daten.

    ![Volume erfolgreich initialisiert](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Die Größe der VM bestimmt, wie viele Laufwerke Sie anfügen können. Einzelheiten finden Sie [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Wie Sie einen Datenträger aus einem Windows-Computer trennen](virtual-machines-windows-classic-detach-disk.md)

[Virtueller Festplatten für virtuelle Maschinen zu Datenträgern](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md
