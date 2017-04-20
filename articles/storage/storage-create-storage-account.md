<properties
    pageTitle="Erstellen, verwalten oder löschen Sie ein Speicherkonto in Azure-Portal | Microsoft Azure"
    description="Erstellen Sie ein neues Speicherkonto, verwalten Sie Konto Zugriff oder löschen Sie ein Speicherkonto in Azure-Portal. Standard- und Premium-Speicherkonten erfahren."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Konteninformationen Azure-Speicher

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Übersicht

Ein Azure Storage-Konto enthält einen eindeutigen Namespace zum Speichern und Datenobjekte Azure-Speicher. Alle Objekte in ein Speicherkonto werden zusammen als Gruppe berechnet. Die Daten in Ihrem Konto ist standardmäßig nur für Sie Konto verfügbar.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Storage-kontoabrechnung

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Beim Erstellen einer Azure virtuellen Computers ist ein Speicherkonto erstellt automatisch am Bereitstellungsspeicherort Wenn Sie nicht bereits ein Speicherkonto an diesem Speicherort verfügen. So ist es nicht notwendig so ein Speicherkonto für die virtuellen Datenträger erstellen. Der speicherkontoname basiert auf Name des virtuellen Computers. Finden Sie die [Dokumentation zu Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) .

## <a name="storage-account-endpoints"></a>Storage-Konto Endpunkte

Jedes Objekt, die in Azure Storage gespeichert hat eine eindeutige URL-Adresse. Speicherkontoname bildet die Unterdomäne der Adresse. Die Kombination aus Domäne und Domäne, ist für jeden Dienst bildet einen *Endpunkt* für das Speicherkonto.

Z. B. wenn das Speicherkonto *Mystorageaccount*lautet, sind Standardendpunkte für das Speicherkonto:

- BLOB-Dienst: http://*Mystorageaccount*. blob.core.windows.net

- Tabelle Service: http://*Mystorageaccount*. table.core.windows.net

- Warteschlange Service: http://*Mystorageaccount*. queue.core.windows.net

- Service-Datei: http://*Mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Ein Speicherkonto Blob macht nur den Blob-Endpunkt.

Die URL für den Zugriff auf ein Objekt in ein Speicherkonto wird durch Anhängen der Speicherort des Objekts im Speicherkonto zum Endpunkt erstellt. Beispielsweise könnte eine BLOB-Adresse Folgendes Format aufweisen: http://*Mystorageaccount*.blob.core.windows.net/*Mycontainer*/*Myblob*.

Sie können auch einen benutzerdefinierten Domänennamen Ihres Speicherkontos mit konfigurieren. Für klassische Speicherkonten Einzelheiten finden Sie unter [Konfigurieren einer benutzerdefinierten Domäne Name für den Endpunkt BLOB-Speicher](storage-custom-domain-name.md) . Für Ressourcenmanager Speicherkonten dies nicht hinzugefügt wurde, [Azure-Portal](https://portal.azure.com) noch mit PowerShell konfigurieren. Weitere Informationen finden Sie unter [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) -Cmdlet.  

## <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.

2. Wählen Sie im Hub **neu** -> **Daten + Speicher** -> **Konto**.

3. Geben Sie einen Namen für das Speicherkonto. Einzelheiten Sie [Storage Konto Endpunkte](#storage-account-endpoints) wie Storage-Kontonamen an Objekte in Azure-Speicher verwendet werden.

    > [AZURE.NOTE] Speicher-Kontonamen müssen zwischen 3 und 24 Zeichen lang sein und darf Zahlen und nur Kleinbuchstaben enthalten.
    >  
    > Speicherkontonamen muss in Azure eindeutig sein. Azure-Portal wird angegeben, wenn speicherkontonamen gewählte bereits verwendet wird.

4. Geben Sie das Bereitstellungsmodell verwendet werden: **Ressourcenmanager** oder **Classic**. **Ressourcen-Manager** ist das empfohlene Bereitstellungsmodell. Weitere Informationen finden Sie unter [Understanding Ressourcenmanager und klassischen Bereitstellung](../resource-manager-deployment-model.md).

    > [AZURE.NOTE] BLOB-Speicher-Konten können nur mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt werden.

5. Wählen Sie das Speicherkonto: **Allgemeine** oder **BLOB-Speicher**. **Allgemeine** ist die Standardeinstellung.

    **Allgemeine** ausgewählt wurde, geben Sie dann die Leistungsebene: **Standard** oder **Premium**. Der Standardwert ist **Standard**. Weitere Informationen zu Standard- und Premium-Speicherkonten finden Sie unter [Einführung in Microsoft Azure Storage](storage-introduction.md) und [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](storage-premium-storage.md).

    **BLOB-Speicher** ausgewählt wurde, geben Sie dann die Zugriffsschicht: **Hot** oder **Cool**. Der Standardwert ist **interessant**. Siehe [Azure BLOB-Speicher: abgekühlt und Hot Ebenen](storage-blob-storage-tiers.md) Weitere Informationen.

6. Die Option Replikation für das Speicherkonto: **LRS**, **g**, **G RA**oder **ZRS**. Der Standardwert ist **RA-GRS**. Weitere Informationen zu Azure Storage Replication Optionen finden Sie unter [Azure Storage Replication](storage-redundancy.md).

7. Wählen Sie das Abonnement auf das neue Storage-Konto erstellt werden soll.

8. Eine neue Ressourcengruppe oder wählen Sie eine vorhandene Ressourcengruppe. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Ressource-Manager](../azure-resource-manager/resource-group-overview.md).

9. Wählen Sie den Standort Ihres Speicherkontos. Weitere Informationen, welche Dienste verfügbar, in welcher Region sind finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/#services) .

10. Klicken Sie auf **Erstellen** , um das Speicherkonto erstellen.

## <a name="manage-your-storage-account"></a>Verwalten Sie Ihr Speicherkonto

### <a name="change-your-account-configuration"></a>Ändern Sie die Konfiguration des Kontos

Nach der Erstellung Ihres Speicherkontos können Sie die Konfiguration wie die Replikationsoption das verwendete Konto ändern oder die Zugriffsschicht für ein Speicherkonto Blob. In [Azure-Portal](https://portal.azure.com)Ihres Speicherkontos navigieren Sie und anschließend auf **Alle** anschließend **Konfiguration** anzeigen oder Ändern der Kontokonfiguration.

> [AZURE.NOTE] Je nach der Erstellung das Speicherkonto gewählte Leistungsebene einige Optionen für die Replikation möglicherweise nicht verfügbar.

Die Replikationsoption werden die Preise ändern. Einzelheiten siehe Seite [Preise Azure-Speicher](https://azure.microsoft.com/pricing/details/storage/) .

BLOB-Speicher Konten kann der Zugriffsschicht ändern Änderung neben ändern Ihre Preise anfallen. [Speicherkonten Blob - Preise und Fakturierung](storage-blob-storage-tiers.md#pricing-and-billing) Weitere Informationen anzeigen

### <a name="manage-your-storage-access-keys"></a>Zugriff auf Speicher verwalten

Wenn Sie ein Speicherkonto erstellen, generiert Azure zwei 512-Bit-Speicher Zugriffstasten für die Authentifizierung verwendet werden, wenn das Speicherkonto zugegriffen wird. Mit zwei Speicher-Zugriffstasten kann Azure Schlüssel ohne Unterbrechung der Speicherdienst oder diesen Dienst neu generieren.

> [AZURE.NOTE] Wir empfehlen, vermeiden die Zugriffstasten Speicher mit anderen Teilen. Um Zugriff auf Ressourcen zu ermöglichen, ohne Angabe der Zugriffstasten können Sie *SAS*. Eine SAS bietet Zugriff auf eine Ressource in Ihrem Konto für ein Intervall definieren und mit den Berechtigungen, die Sie angeben. Weitere Informationen finden Sie unter [Verwenden gemeinsame Zugriff Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

#### <a name="view-and-copy-storage-access-keys"></a>Anzeigen und Kopieren von Zugriffstasten Speicher

In [Azure-Portal](https://portal.azure.com)Ihres Speicherkontos navigieren Sie und anschließend auf **Alle Einstellungen** dann **Zugriffstasten** anzeigen, kopieren und Ihr Konto Zugriffstasten regenerieren. **Zugriffstasten** Blade auch vorkonfigurierte Verbindungszeichenfolgen der primären und sekundären Schlüssel, die Sie kopieren können, verwenden Sie in Ihrer Anwendung.

#### <a name="regenerate-storage-access-keys"></a>Regenerieren Sie Zugriffstasten Speicher

Wir empfehlen Zugriffstasten in das Speicherkonto in regelmäßigen Abständen zu sichern Ihre Storage-Verbindung ändern. Zwei Zugriffstasten werden zugeordnet, sodass Sie Zugriff auf das Speicherkonto mithilfe einer Tastenkombination die andere Taste Regenerieren beibehalten können.

> [AZURE.WARNING] Regenerieren die Zugriffstasten betreffen Dienstleistungen in Azure, eigene Programme das Speicherkonto abhängig sind. Alle Clients, die Zugriffstaste verwenden auf das Speicherkonto, müssen aktualisiert werden, um den neuen Schlüssel.

**Media Services** - Falls Sie Media Services das Speicherkonto abhängig sind, muss erneut synchronisieren Zugriffstasten Media-Dienst nach dem Regenerieren der Schlüssel.

**Applications** - ASP.NET-Webanwendungen oder Cloud-Dienste, mit denen das Speicherkonto Verbindung verlieren Sie Schlüssel generiert neu, wenn Sie Ihre Schlüssel wiederherstellen.

**Speicher-Explorers** - verwenden Sie [Storage Explorer Applikationen](storage-explorers.md), wahrscheinlich müssen Sie Speicherschlüssel verwendet die Anwendung aktualisieren.

Hier ist der Prozess zum Drehen der Zugriffstasten Speicher:

1. Aktualisieren Sie die Verbindungszeichenfolgen im Anwendungscode auf die sekundäre Taste des Speicherkontos.

2. Regenerieren Sie den primäre Schlüssel für das Speicherkonto. -Blade **Zugriffstasten** auf **Key1 Regenerieren**und klicken Sie auf **Ja,** um sicherzustellen, dass Sie einen neuen Schlüssel generieren möchten.

3. Aktualisieren Sie die Verbindungszeichenfolgen im Code auf die neue primäre Taste.

4. Regenerieren Sie die sekundäre Taste auf die gleiche Weise.

## <a name="delete-a-storage-account"></a>Ein Speicherkonto löschen

Um ein Speicherkonto zu entfernen, die Sie nicht mehr verwenden, an das Speicherkonto in [Azure-Portal](https://portal.azure.com)navigieren Sie, und klicken Sie auf **Löschen**. Ein Speicherkonto löschen das gesamte Konto einschließlich alle Daten in das Konto.

> [AZURE.WARNING] Es ist nicht möglich, Wiederherstellen gelöschter Speicherkonto oder Löschvorgang enthaltenen Inhalte abrufen. Achten Sie darauf, dass Sie alles sichern zu speichern, bevor Sie das Konto löschen. Dies gilt auch für alle Ressourcen im Konto – ein Blob, Tabelle, Warteschlange oder Datei löschen, wird es dauerhaft gelöscht.

Um ein Speicherkonto löschen, Azure VM zugeordnet ist, müssen Sie zunächst sicherstellen, dass alle virtuellen Datenträger gelöscht werden. Wenn Sie nicht zuerst die virtuellen Datenträger löschen, werden versuchen, das Speicherkonto löschen Sie eine ähnliche Fehlermeldung angezeigt:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Das Speicherkonto das klassischen Bereitstellungsmodell verwendet, können Sie den virtuellen Datenträger entfernen, folgendermaßen in [Azure-Portal](https://manage.windowsazure.com):

1. Navigieren Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com).
2. Navigieren Sie zu der Registerkarte virtuelle Maschinen.
3. Klicken Sie auf die Registerkarte Datenträger.
4. Wählen Sie die Datenträger und dann auf Datenträger löschen.
5. Datenträgerabbilder löschen, navigieren Sie zu der Registerkarte Bilder und löschen Sie alle Bilder, die im Feld gespeichert sind.

Weitere Informationen finden Sie in der [Dokumentation zu Azure Virtual Machine](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Nächste Schritte

- [Azure BLOB-Speicher: Cool und Hot Ebenen](storage-blob-storage-tiers.md)
- [Azure Speicherreplikation](storage-redundancy.md)
- [Konfigurieren Sie Verbindungszeichenfolgen Azure-Speicher](storage-configure-connection-string.md)
- [Daten mit AzCopy-Befehlszeilenprogramm](storage-use-azcopy.md)
- Besuchen Sie den [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/).
