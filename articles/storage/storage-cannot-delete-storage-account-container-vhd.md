<properties
    pageTitle="Problembehandlung bei Azure-Speicherkonten, Containern oder virtuelle Festplatten in einer klassischen Bereitstellung löschen | Microsoft Azure"
    description="Problembehandlung bei Azure-Speicherkonten, Containern oder virtuelle Festplatten in einer klassischen Bereitstellung löschen"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="tysonn"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="genli"/>

# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Problembehandlung bei Azure-Speicherkonten, Containern oder virtuelle Festplatten in einer klassischen Bereitstellung löschen

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Sie können Fehlermeldungen beim Azure Speicherkonto, Container oder VHD in [Azure-Portal](https://portal.azure.com/) oder [Azure-Verwaltungsportal](https://manage.windowsazure.com/)zu löschen. Die Probleme können durch folgende Umstände verursacht werden:

-   Wenn Sie einen virtuellen Computer löschen, werden Datenträger und VHD nicht automatisch gelöscht. Die möglicherweise die Ursache auf Storage-Konto löschen. Wir löschen nicht den Datenträger, so dass Sie den Datenträger Bereitstellen einer anderen VM verwenden können.

-   Gibt es noch eine Lease auf einem Datenträger oder Blob, das den Datenträger zugeordnet ist.

Wenn Ihre Azure Problem in diesem Artikel nicht behandelt wird, finden Sie auf [MSDN und den Stapelüberlauf](https://azure.microsoft.com/support/forums/)Azure Foren. Buchen kann das Problem auf diese Foren oder @AzureSupport auf Twitter. Auch können Sie durch Auswählen der [Azure-support](https://azure.microsoft.com/support/options/) -Website **erhalten Sie Unterstützung** Azure Supportanfrage einreichen.

## <a name="symptoms"></a>Symptome

Abschnitt enthält Fehlermeldungen, die möglicherweise bei der Azure-Speicherkonten, Container oder virtuelle Festplatten zu löschen.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Szenario 1: Kein Speicherkonto löschen

Navigieren an das Speicherkonto [Azure-Portal](https://portal.azure.com/) oder [Azure-Verwaltungsportal](https://manage.windowsazure.com/) und **Löschen**möglicherweise die folgende Fehlermeldung angezeigt:

*Konto StorageAccountName enthält VM Bilder. Sicherstellen Sie, dass diese VM vor dem Löschen dieser Speicher entfernt werden.*

Dieser Fehler kann auch angezeigt werden:

**Der Azure-Portal**:

*Fehler beim Speicherkonto < Vm-Speicher-Kontoname > löschen. Storage-Konto < Vm-Speicher-Kontoname > kann nicht gelöscht: "Storage-Konto < Vm-Speicher-Kontoname > hat einige aktive Bilder oder Datenträger. Sicherzustellen, dass diese Bilder bzw. Datenträger vor dem Löschen dieses Speicherkonto entfernt. ".*

**Der Azure-Verwaltungsportal**:

*Speicherkonto < Vm-Speicher-Kontoname > hat einige aktive Bilder Datenträger, z.B. Xxxxxxxxx-Xxxxxxxxx-O-209490240936090599. Stellen Sie sicher, diese Bilder bzw. Datenträger vor dem Löschen dieser Speicher entfernt.*

Oder

**Der Azure-Portal**:

*Storage-Konto < Vm-Speicher-Kontoname > hat 1 Container die aktiven Bild bzw. Datenträger Artefakte. Sicherzustellen, dass diese Artefakte aus dem Abbildrepository vor dem Löschen dieser Speicher entfernt sind*.

**Der Azure-Verwaltungsportal**:

*Senden konnte Speicherkonto < Vm-Speicher-Kontoname > hat 1 Container die aktiven Bild bzw. Datenträger Artefakte. Sicherstellen Sie, dass diese Artefakte aus dem Abbildrepository vor dem Löschen dieser Speicher entfernt werden. Wenn Sie versuchen, ein Speicherkonto löschen und aktiver Datenträger zugeordnet, sehen Sie eine Meldung sind aktive Laufwerke, die gelöscht werden müssen*.

### <a name="scenario-2-unable-to-delete-a-container"></a>Szenario 2: Entfernen eines Containers

Beim Löschen der Behälter wird möglicherweise die folgende Fehlermeldung angezeigt:

*Fehler beim Löschen der Behälter <container name>. Fehler: "Es gibt eine Lease für den Container und keine Lease-ID in der Anforderung angegeben wurde*.

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Szenario 3: Keine virtuelle Festplatte löschen

Nach dem Löschen einer VM und versuchen, die Blobs für die zugeordneten VHDs löschen können Sie folgende Meldung:

*Fehler beim Löschen des Blob ' Pfad/XXXXXX-XXXXXX-os-1447379084699.vhd'. Fehler: "Es gibt eine Lease für das Blob und keine Lease-ID in der Anforderung angegeben wurde.*

## <a name="solution"></a>Lösung
Um die am häufigsten auftretenden Probleme zu beheben, versuchen Sie folgende Methode:

### <a name="step-1-delete-any-os-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Schritt 1: Löschen Sie alle Betriebssystemlaufwerke, die Löschung des Speicherkonto, Container oder VHD verhindern

1. Wechseln Sie zur [klassischen Azure-Portal](https://manage.windowsazure.com/).
2. Wählen Sie **VIRTUAL MACHINE** > **Festplatten**.

    ![Image der Festplatten auf virtuellen Maschinen Azure-Verwaltungsportal.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Suchen Sie die Datenträger mit Speicherkonto, Container oder virtuelle Festplatte, die Sie löschen möchten. Wenn Sie den Speicherort des Datenträgers überprüfen, finden Sie zugeordnete Speicherkonto, Container oder virtuelle Festplatte.

    ![Bild, Standortinformationen für Festplatten auf Azure-Verwaltungsportal anzeigt](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Bestätigen Sie, dass keine VM im Feld **an der** Datenträger aufgeführt ist und löschen Sie die Datenträger.

    > [AZURE.NOTE] Wenn ein virtueller Computer ein Datenträger zugeordnet ist, werden Sie nicht gelöscht. Festplatten werden asynchron von gelöschten VM getrennt. Es dauert einige Minuten, nachdem die VM für dieses Feld zu löschen.

### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Schritt 2: Löschen Sie alle Grafiken VM, die Löschung des Storage oder Container verhindern

1. Wechseln Sie zur [klassischen Azure-Portal](https://manage.windowsazure.com/).
2. Wählen Sie **VIRTUAL MACHINE** > **Bilder**, und löschen Sie die Bilder, das Speicherkonto, Container oder VHD zugeordnet sind.

    Versuchen Sie danach, Speicherkonto, Container oder VHD wieder löschen.

> [AZURE.WARNING] Achten Sie darauf, dass Sie alles sichern zu speichern, bevor Sie das Konto löschen. Es ist nicht möglich, Wiederherstellen gelöschter Speicherkonto oder Löschvorgang enthaltenen Inhalte abrufen. Dies gilt auch für alle Ressourcen im Konto: VHD, Blob, Tabelle, Warteschlange oder Datei löschen, wird es dauerhaft gelöscht. Stellen Sie sicher, dass die Ressource nicht verwendet wird.

## <a name="about-the-stopped-deallocated-status"></a>Über den Status beendet (freigegeben)

VMs im klassischen Bereitstellungsmodell entstanden und wurden, beibehalten werden **beendet (freigegeben)** auf dem [Azure-Portal](https://portal.azure.com/) oder dem [klassischen Azure-Portal](https://manage.windowsazure.com/)haben.

**Klassische Azure-Portal**:

![Angehalten (Deallocated)-Status für VMs Azure-Portal.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Azure-Portal**:

![(Freigegeben) Status beendet für VMs auf Azure-Verwaltungsportal.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

Status "Beendet (freigegeben)" gibt die Computerressourcen wie CPU, Speicher und Netzwerk. Die Laufwerke bleiben jedoch weiterhin, damit Sie schnell die VM ggf. neu erstellen können. Diese Laufwerke sind auf VHDs erstellt der Azure-Speicher gesichert werden. Storage-Konto hat diese Festplatten und Datenträger Leases auf die VHDs haben.

## <a name="next-steps"></a>Nächste Schritte

- [Ein Speicherkonto löschen](storage-create-storage-account.md#delete-a-storage-account)
- [Wie die gesperrte Lease BLOB-Speicher in Microsoft Azure (PowerShell) aufteilen](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
