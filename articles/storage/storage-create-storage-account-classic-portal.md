<properties
    pageTitle="Erstellen, verwalten oder löschen Sie ein Speicherkonto in Azure-Verwaltungsportal | Microsoft Azure"
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

Ein Azure Storage-Konto erhalten Sie Zugriff auf die Dienste Azure Blob, Warteschlange, Tabelle und Datei in Azure-Speicher. Das Speicherkonto bereit Datenobjekte Azure Storage eindeutigen Namespace. Die Daten in Ihrem Konto ist standardmäßig nur für Sie Konto verfügbar.

Es gibt zwei Arten von Speicherkonten:

- Ein standard Speicherkonto enthält Blob, Tabelle, Warteschlange und Datei speichern.
- Ein Premium-Speicherkonto unterstützt derzeit nur Azure virtuellen Datenträger. Siehe [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](storage-premium-storage.md) eine ausführliche Übersicht über Premium-Speicher.

## <a name="storage-account-billing"></a>Storage-kontoabrechnung

Verwendung der Azure-Speicher anhand Ihres Speicherkontos werden berechnet. Speicherkosten basieren auf vier Faktoren: Speicherkapazität, Replikationsschema Speichertransaktionen und Daten-Ausgang.

- Speicherkapazität bezieht, wie viel der Zuteilung Storage-Konto Sie verwenden zum Speichern von Daten. Die Kosten einfach zum Speichern von Daten wird bestimmt, wie viele Daten gespeichert werden und wie repliziert.
- Replikation bestimmt, wie viele Kopien Ihrer Daten gleichzeitig und an welchen Orten verwaltet werden.
- Transaktionen verweisen alle Lese- und Schreibvorgänge in den Azure-Speicher.
- Daten-Ausgang bezieht sich auf Daten aus einer Azure-Region. Wenn die Daten in das Speicherkonto erfolgt durch eine Anwendung, die nicht im selben Bereich ausgeführt wird, ob die Anwendung einen Cloud-Dienst oder eine andere Anwendung ist, werden Sie für ausgehenden Daten erhoben. (Für Azure Services können Sie Schritte zum Gruppieren der Daten und Dienste in derselben Rechenzentren zur Verringerung oder Beseitigung Ausgang Gebühren.)  

Die Seite [Azure Storage Preise](https://azure.microsoft.com/pricing/details/storage) bietet Ausführliche Preisinformationen für Speicherkapazität, Replikation und Transaktionen. Detailseite [Daten überträgt Preise](https://azure.microsoft.com/pricing/details/data-transfers/) bietet Ausführliche Preisinformationen für Daten-Ausgang.

Details zu Konto Kapazität und Performance Ziele finden Sie in [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md).

> [AZURE.NOTE] Beim Erstellen einer Azure virtuellen Computers ist ein Speicherkonto erstellt automatisch am Bereitstellungsspeicherort Wenn Sie nicht bereits ein Speicherkonto an diesem Speicherort verfügen. So ist es nicht notwendig so ein Speicherkonto für die virtuellen Datenträger erstellen. Der speicherkontoname basiert auf Name des virtuellen Computers. Finden Sie die [Dokumentation zu Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) .

## <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen

1. Das [klassische Azure-Portal](https://manage.windowsazure.com)anmelden.

2. Klicken Sie in der Taskleiste am unteren Rand der Seite auf **neu** . Wählen Sie **Data Services** | **Speicher** **Schnell erstellen**klicken.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. Geben Sie einen Namen für das Speicherkonto **URL**.

    > [AZURE.NOTE] Speicher-Kontonamen müssen zwischen 3 und 24 Zeichen lang sein und darf Zahlen und nur Kleinbuchstaben enthalten.
    >  
    > Speicherkontonamen muss in Azure eindeutig sein. Azure-Verwaltungsportal wird angezeigt, wenn bereits speicherkontonamen gewählte.

    Einzelheiten Sie [Storage Konto Endpunkte](#storage-account-endpoints) unter wie Storage-Kontonamen an Objekte in Azure-Speicher verwendet werden.

4. **Standort/Gruppe**wählen Sie einen Speicherort für das Speicherkonto, die Sie oder Ihre Kunden. Wenn Daten in das Speicherkonto anderen Azure-Dienst oder einen virtuellen Azure Cloud-Dienst erfolgt empfiehlt es sich, wählen eine Gruppe aus der Liste das Speicherkonto im gleichen Rechenzentrum mit anderen Azure gruppieren, die Sie verwenden, um Leistung und geringere Kosten.

    Beachten Sie, dass Sie eine Gruppe auswählen müssen, wenn das Speicherkonto erstellt wird. Sie können kein Konto in eine Gruppe verschieben. Weitere Informationen zu Gruppen finden Sie unter [Speicherort gemeinsam mit einer Gruppe](#service-co-location-with-an-affinity-group) unten.

    >[AZURE.IMPORTANT] Um zu bestimmen, welche Speicherorte für Ihr Abonnement verfügbar sind, können Sie die [Liste aller Ressourcenprovider](https://msdn.microsoft.com/library/azure/dn790524.aspx) Operation aufrufen. Liste der Anbieter von PowerShell, rufen Sie [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). Verwenden Sie von .NET die [Liste](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) der ProviderOperationsExtensions-Klasse.
    >
    >Weitere Informationen, welche Dienste verfügbar, in welcher Region sind sehen Sie [Azure Regionen](https://azure.microsoft.com/regions/#services) außerdem.


5. Haben mehrere Azure-Abonnements wird das **Abonnement** -Feld angezeigt. Geben Sie im **Abonnement**gewünschte Speicherkonto mit Azure-Abonnement.

6. Wählen Sie den gewünschten Grad der Replikation für das Speicherkonto **Replikation**. Die empfohlene Replikationsoption ist redundant Geo-Replikation Haltbarkeit für Ihre Daten. Weitere Informationen zu Azure Storage Replication Optionen finden Sie unter [Azure Storage Replication](storage-redundancy.md).

6. Klicken Sie auf **Konto erstellen**.

    Es dauert einige Minuten das Speicherkonto erstellen. Überwachen Sie zum Überprüfen des Status die Benachrichtigungen am unteren Rand der Azure-Verwaltungsportal. Nach dem Erstellen das Speicherkonto neue Speicherkonto hat Status **Online** und betriebsbereit ist.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Storage-Konto Endpunkte

Jedes Objekt, die in Azure Storage gespeichert hat eine eindeutige URL-Adresse. Speicherkontoname bildet die Unterdomäne der Adresse. Die Kombination aus Domäne und Domäne, ist für jeden Dienst bildet einen *Endpunkt* für das Speicherkonto.

Z. B. wenn das Speicherkonto *Mystorageaccount*lautet, sind Standardendpunkte für das Speicherkonto:

- BLOB-Dienst: http://*Mystorageaccount*. blob.core.windows.net

- Tabelle Service: http://*Mystorageaccount*. table.core.windows.net

- Warteschlange Service: http://*Mystorageaccount*. queue.core.windows.net

- Service-Datei: http://*Mystorageaccount*. file.core.windows.net

Sie sehen die Endpunkte für das Speicherkonto Armaturenbrett Speicher in [Azure-Verwaltungsportal](https://manage.windowsazure.com) sobald das Konto erstellt wurde.

Die URL für den Zugriff auf ein Objekt in ein Speicherkonto wird durch Anhängen der Speicherort des Objekts im Speicherkonto zum Endpunkt erstellt. Beispielsweise könnte eine BLOB-Adresse Folgendes Format aufweisen: http://*Mystorageaccount*.blob.core.windows.net/*Mycontainer*/*Myblob*.

Sie können auch einen benutzerdefinierten Domänennamen Ihres Speicherkontos mit konfigurieren. Einzelheiten finden Sie unter [Konfigurieren einer benutzerdefinierten Domänennamen für den BLOB-Speicher-Endpunkt](storage-custom-domain-name.md) .

### <a name="service-co-location-with-an-affinity-group"></a>Speicherort gemeinsam mit einer Gruppe

Eine *Gruppe* ist eine geografische Gruppierung Azure Services und VMs mit Ihrem Konto Azure-Speicher. Eine Gruppe kann Computer Arbeitslasten im selben Rechenzentrum oder in der Nähe der Benutzer Zielgruppe lokalisiert Leistung verbessern. Wenn Daten in einem Speicherkonto von einem anderen Dienst zugegriffen werden, Teil der gleichen Gruppe Affinität auch keine Gebühren für Ausgang fallen.

> [AZURE.NOTE]  Erstellen Sie eine Gruppe <b>Einstellungsbereich [Azure-Verwaltungsportal](https://manage.windowsazure.com)</b> zu öffnen, klicken Sie auf <b>Gruppen</b>und <b>Hinzufügen einer Gruppe</b> oder die Schaltfläche <b>Hinzufügen</b> klicken. Sie können auch Gruppen mithilfe der Azure Management API verwalten. Weitere Informationen finden Sie <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">auf Gruppen</a> .

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Zeigen Sie an, kopieren Sie und regenerieren Sie Zugriffstasten Speicher

Wenn Sie ein Speicherkonto erstellen, generiert Azure zwei 512-Bit-Speicher Zugriffstasten für die Authentifizierung verwendet werden, wenn das Speicherkonto zugegriffen wird. Mit zwei Speicher-Zugriffstasten kann Azure Schlüssel ohne Unterbrechung der Speicherdienst oder diesen Dienst neu generieren.

> [AZURE.NOTE] Wir empfehlen, vermeiden die Zugriffstasten Speicher mit anderen Teilen. Um Zugriff auf Ressourcen zu ermöglichen, ohne Angabe der Zugriffstasten können Sie *SAS*. Eine SAS bietet Zugriff auf eine Ressource in Ihrem Konto für ein Intervall definieren und mit den Berechtigungen, die Sie angeben. Weitere Informationen finden Sie unter [Verwenden gemeinsame Zugriff Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

Tasten Sie in [Azure-Verwaltungsportal](https://manage.windowsazure.com) **Verwalten** auf dem Dashboard oder **Speicher** anzeigen, kopieren und Regenerieren Speicher Zugriffstasten, die Zugriff auf die Dienste Blob, Tabelle und Warteschlange verwendet werden.

### <a name="copy-a-storage-access-key"></a>Kopieren einer Zugriffstaste Speicher  

**Schlüssel verwalten** können Sie einen Speicher Zugriffsschlüssel in einer Verbindungszeichenfolge kopieren. Die Verbindungszeichenfolge muss speicherkontonamen und einen Schlüssel für Authentifizierung verwendet. Informationen zum Konfigurieren von Verbindungszeichenfolgen für den Zugriff auf Azure-Speicherdienste finden Sie unter [Konfigurieren von Azure Storage Connection Strings](storage-configure-connection-string.md).

1. Im [Klassischen Azure-Portal](https://manage.windowsazure.com)auf **Speicher**und klicken Sie dann auf den Namen des Speicherkontos Dashboard öffnen.

2. Klicken Sie auf **Schlüssel verwalten**.

    **Zugriffstasten verwalten** wird geöffnet.

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Um eine Zugriffstaste Speicher Kopieren markieren Sie Schlüssel. Dann mit der rechten Maustaste, und klicken Sie auf **Kopieren**.

### <a name="regenerate-storage-access-keys"></a>Regenerieren Sie Zugriffstasten Speicher
Wir empfehlen Zugriffstasten in das Speicherkonto in regelmäßigen Abständen zu sichern Ihre Storage-Verbindung ändern. Zwei Zugriffstasten werden zugeordnet, sodass Sie Zugriff auf das Speicherkonto mithilfe einer Tastenkombination die andere Taste Regenerieren beibehalten können.

> [AZURE.WARNING] Regenerieren die Zugriffstasten betreffen Dienstleistungen in Azure, eigene Programme das Speicherkonto abhängig sind. Alle Clients, die Zugriffstaste verwenden auf das Speicherkonto, müssen aktualisiert werden, um den neuen Schlüssel.

**Media Services** - Falls Sie Media Services das Speicherkonto abhängig sind, muss erneut synchronisieren Zugriffstasten Media-Dienst nach dem Regenerieren der Schlüssel.

**Applications** - ASP.NET-Webanwendungen oder Cloud-Dienste, mit denen das Speicherkonto Verbindung verlieren Sie Schlüssel generiert neu, wenn Sie Ihre Schlüssel wiederherstellen. 

**Speicher-Explorers** - verwenden Sie [Storage Explorer Applikationen](storage-explorers.md), wahrscheinlich müssen Sie Speicherschlüssel verwendet die Anwendung aktualisieren.

Hier ist der Prozess zum Drehen der Zugriffstasten Speicher:

1. Aktualisieren Sie die Verbindungszeichenfolgen im Anwendungscode auf die sekundäre Taste des Speicherkontos.

2. Regenerieren Sie den primäre Schlüssel für das Speicherkonto. Klicken Sie im [Klassischen Azure-Portal](https://manage.windowsazure.com)über das Dashboard oder **Konfigurieren** auf **Schlüssel verwalten**. Der primäre Schlüssel auf **Regenerieren** und klicken Sie auf **Ja,** um sicherzustellen, dass Sie einen neuen Schlüssel generieren möchten.

3. Aktualisieren Sie die Verbindungszeichenfolgen im Code auf die neue primäre Taste.

4. Regenerieren Sie die sekundäre Taste.

## <a name="delete-a-storage-account"></a>Ein Speicherkonto löschen

Verwenden Sie auf dem Dashboard oder **Konfigurieren** **Löschen** , um ein Speicherkonto zu entfernen, die Sie nicht mehr verwenden. **Löschen** löscht das gesamte Konto einschließlich aller Blobs und Tabellen sowie Warteschlangen im Konto.

> [AZURE.WARNING] Es ist nicht möglich, Wiederherstellen gelöschter Speicherkonto oder Löschvorgang enthaltenen Inhalte abrufen. Achten Sie darauf, dass Sie alles sichern zu speichern, bevor Sie das Konto löschen. Dies gilt auch für alle Ressourcen im Konto – ein Blob, Tabelle, Warteschlange oder Datei löschen, wird es dauerhaft gelöscht.
>
> Enthält das Speicherkonto VHD-Dateien für Azure Virtual Machine müssen Sie löschen alle Bilder und Festplatten, die diese VHD-Dateien verwenden, bevor dem Speicher löschen kann. Zuerst beenden Sie den virtuellen Computer ausgeführt wird, und löschen Sie ihn. Laufwerke, navigieren Sie zu der Registerkarte **Datenträger** , und löschen alle Laufwerke. Um Bilder zu löschen, navigieren Sie zu der Registerkarte **Bilder** , und löschen Sie Bilder, die im Feld gespeichert sind.

1. Klicken Sie im [Azure-Verwaltungsportal](https://manage.windowsazure.com)auf **Speicher**.

2. In Kontoeintrag Speicher bis auf den Namen klicken Sie und dann auf **Löschen**.

     – Oder –

    Klicken Sie auf das Speicherkonto Dashboard öffnen und dann auf **Löschen**.

3. Klicken Sie auf **Ja** , dass Sie das Speicherkonto löschen möchten.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure-Speicher finden Sie unter [Azure Storage-Dokumentation](https://azure.microsoft.com/documentation/services/storage/).
- Besuchen Sie den [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/).
- [Daten mit AzCopy-Befehlszeilenprogramm](storage-use-azcopy.md)
