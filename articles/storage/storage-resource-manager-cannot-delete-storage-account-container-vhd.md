<properties
    pageTitle="Fehlerbehebung beim Löschen von Azure-Speicherkonten, Containern oder VHDs Ressourcenmanager Bereitstellung | Microsoft Azure"
    description="Fehlerbehebung beim Löschen von Azure-Speicherkonten, Containern oder VHDs Ressourcenmanager Bereitstellung"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="genli"/>

# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds-in-a-resource-manager-deployment"></a>Fehlerbehebung beim Löschen von Azure-Speicherkonten, Containern oder VHDs Ressourcenmanager Bereitstellung

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Fehler erhalten, wenn Sie versuchen, ein Azure-Speicherkonto, Container oder virtuelle Festplatte (VHD) im [Azure-Portal](https://portal.azure.com)löschen. Dieser Artikel enthält Hinweise zur Problembehandlung zum Beheben des Problems in einer Bereitstellung Azure-Ressourcen-Manager.

Wenn dieser Artikel Ihr Azure Problems nicht finden Sie auf [MSDN und Stapelüberlauf](https://azure.microsoft.com/support/forums/)Azure Foren. Buchen kann das Problem auf diese Foren oder @AzureSupport auf Twitter. Auch können Sie durch Auswählen der [Azure-support](https://azure.microsoft.com/support/options/) -Website **erhalten Sie Unterstützung** Azure Supportanfrage einreichen.

## <a name="symptoms"></a>Symptome

### <a name="scenario-1"></a>Szenario 1

Beim Löschen einer virtuellen Festplatte in ein Speicherkonto in einem Ressourcenmanager Bereitstellung erhalten Sie folgende Fehlermeldung:

**Fehler beim Löschen der Blob 'vhds/BlobName.vhd'. Fehler: Es gibt eine Lease für das Blob und keine Lease-ID in der Anforderung angegeben wurde.**

Dieses Problem kann auftreten, weil eine Virtual Machine (VM) eine Lease auf der virtuellen Festplatte, die Sie löschen möchten.

### <a name="scenario-2"></a>Szenario 2

Beim Entfernen eines Containers in ein Speicherkonto in einem Ressourcenmanager Bereitstellung erhalten Sie folgende Fehlermeldung:

**Fehler beim Löschen von Storage Container "Festplatten". Fehler: Es gibt eine Lease für den Container und keine Lease-ID in der Anforderung angegeben wurde.**

Dieses Problem kann auftreten, weil Container eine virtuelle Festplatte, die Lease Status gesperrt ist.

### <a name="scenario-3"></a>Szenario 3

Beim Löschen ein Speicherkonto in einem Ressourcenmanager Bereitstellung erhalten Sie folgende Fehlermeldung:

**Fehler beim Löschen von Konto 'StorageAccountName'. Fehler: Das Speicherkonto kann durch die Verwendung der Artefakte gelöscht werden.**

Dieses Problem kann auftreten, weil das Speicherkonto eine virtuelle Festplatte enthält, die der Lease ist.

## <a name="solution"></a>Lösung

Zur Behebung dieser Probleme müssen Sie die virtuelle Festplatte, die den Fehler verursacht und die zugeordneten VM ermitteln. Trennen der virtuellen Festplatte von VM (für Datenträger) oder löschen Sie den virtuellen Computer, der die VHD-Datei (für Betriebssystemlaufwerke) verwendet. Dies entfernt der Lease von der virtuellen Festplatte und gelöscht werden.

### <a name="step-1-identify-the-problem-vhd-and-the-associated-vm"></a>Schritt 1: Ermitteln des Problems VHD-Datei und die zugeordneten VM


1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2. Wählen Sie im **Hub** auf **alle Ressourcen**. Besuchen Sie das Speicherkonto, das Sie löschen möchten, und wählen Sie **Blobs** > **Vhds**.

    ![Screenshot des Portals "Festplatten" Container hervorgehoben und das Speicherkonto](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

3. Überprüfen Sie die Eigenschaften jeder virtuellen Festplatte im Container. Suchen Sie die virtuelle Festplatte im **Leased** Zustand befindet. Anschließend bestimmen die VM verwenden der virtuellen Festplatte. Normalerweise bestimmen Sie die VM die VHD-Datei enthält, überprüfen Sie die Namen der virtuellen Festplatte:

    - Betriebssystemlaufwerke prinzipiell dieser Namenskonvention: VMNameYYYYMMDDHHMMSS.vhd
    - Datenträger im allgemeinen Namenskonvention folgen: VMName JJJJMMTT HHMMSS.vhd

    ![Screenshot der Container Informationen im Portal mit dem Namen der VM den Leasezustand "Locked" und Leasing-Zustand "Leased" markiert](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/locatevm.png)

### <a name="step-2-remove-the-lease-from-the-vhd"></a>Schritt 2: Entfernen der Lease von der virtuellen Festplatte

Um den virtuellen Computer löschen verwenden, die VHD-Datei (für Betriebssystemlaufwerke):

1.  Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2.  Wählen Sie im **Hub** auf **virtuellen Maschinen**.
3.  Wählen Sie den virtuellen Computer, die eine Lease für die virtuelle Festplatte enthält.
4.  Sicherstellen, dass keine virtuellen Computer aktiv verwendet wird und Sie nicht den virtuellen Computer.
5.  Oben **VM Details** Blade wählen Sie **Löschen**und dann auf **Ja** zu bestätigen.
6.  Die VM sollte gelöscht, aber die VHD-Datei beibehalten werden. Allerdings sollte die VHD nicht mehr eine Lease sein. Es dauert einige Minuten für den Lease freigegeben werden. Um sicherzustellen, dass die Lease freigegeben wurde, gehen **Alle**Ressourcen > **Storage Kontoname** > **Blobs** > **Vhds**. Im **BLOB-Eigenschaften** sollte der Statuswert **Lease** **frei**.

Die virtuelle Festplatte aus der VM trennen, die (für Datenträger) verwendet:

1.  Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2.  Wählen Sie im **Hub** auf **virtuellen Maschinen**.
3.  Wählen Sie den virtuellen Computer, die eine Lease für die virtuelle Festplatte enthält.
4.  Wählen Sie **Laufwerke** auf den **VM-Details** .
5.  Wählen Sie den Datenträger, der eine Lease für die virtuelle Festplatte enthält. Bestimmen Sie die virtuelle Festplatte angefügt ist Datenträger überprüfen Sie die URL der virtuellen Festplatte.
6.  Bestimmen Sie mit Sicherheit, dass nichts den Datenträger aktiv verwendet wird.
7.  Klicken Sie auf den **Datenträger** auf **Trennen** .
8.  Die Festplatte sollte jetzt von der VM getrennt und die VHD-Datei sollte nicht mehr eine Lease auf. Es dauert einige Minuten für den Lease freigegeben werden. Um sicherzustellen, dass die Lease freigegeben wurde, gehen **Alle**Ressourcen > **Storage Kontoname** > **Blobs** > **Vhds**. Im **BLOB-Eigenschaften** sollte der Statuswert **Lease** **frei**.

## <a name="what-is-a-lease"></a>Was ist eine Lease?

Ein Leasingverhältnis ist eine Sperre, die Steuerung des Zugriffs auf ein Blob (z. B. VHD) verwendet werden kann. Ein Blob vermietet, können nur der Besitzer des Lease Blob zugreifen. Eine Lease ist aus folgenden Gründen wichtig:

-   Wenn mehrere Eigentümer zum Teil Blob gleichzeitig schreiben verhindert Datenverluste.
-   Verhindert das Blob wird gelöscht, wenn etwas aktiv (z. B. VM) verwendet.
-   Verhindert das Speicherkonto gelöscht wird, wenn etwas aktiv (z. B. VM) verwendet.



## <a name="next-steps"></a>Nächste Schritte

- [Ein Speicherkonto löschen](storage-create-storage-account.md#delete-a-storage-account)
- [Wie die gesperrte Lease BLOB-Speicher in Microsoft Azure (PowerShell) aufteilen](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
