<properties
    pageTitle="Erstellen Sie ein Konto Azure Batch | Microsoft Azure"
    description="Informationen Sie zum Erstellen eines Kontos Azure Batch in Azure-Portal umfangreichen parallelen Workloads in der Cloud ausgeführt"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Erstellen Sie ein Azure-Portal mit Azure Batch-Konto

> [AZURE.SELECTOR]
- [Azure-portal](batch-account-create-portal.md)
- [Batchverwaltung .NET](batch-management-dotnet.md)

Informationen zum Erstellen eines Kontos Azure Batch in [Azure-Portal][azure_portal], und wichtige Eigenschaften wie Zugriffstasten und URLs berücksichtigt. Wir besprechen Sie Batch Preise und Azure Storage-Konto Ihrem Batch-Konto verknüpfen, sodass [Pakete](batch-application-packages.md) und [beibehalten von Projekt und Ausgabe](batch-task-output.md)verwenden können.

## <a name="create-a-batch-account"></a>Ein Batch-Konto erstellen

1. [Azure-Portal]anmelden[azure_portal].

2. **Klicken Sie auf** > **berechnen** > **Batch-Dienst**.

    ![Stapel auf dem Markt][marketplace_portal]

3. Das **Neue Stapel Konto** Blatt wird angezeigt. Siehe Artikel *eine* bis *e* unten eine Beschreibung jedes Blade-Element.

    ![Ein Batch-Konto erstellen][account_portal]

    ein. **Kontoname**: einen eindeutigen Namen für die Batch-Konto. Dieser Name muss eindeutig innerhalb der Azure-Region sein, das Konto erstellt wird (siehe unten *Position* ). Es darf nur Kleinbuchstaben, Zahlen und muss 3 24 Zeichen lang sein.

    b. **Abonnement**: ein Abonnement, das Batch-Konto erstellen. Wenn Sie nur ein Abonnement verfügen, ist es standardmäßig aktiviert.

    c. **Gruppe**: eine vorhandene Ressource für das neue Konto Batch gruppieren oder optional eine neue erstellen.

    d. **Ort**: Region eines Azure, Batch-Konto erstellen. Mit Ihrem Abonnement Ressourcengruppe unterstützten Regionen werden als Optionen angezeigt.

    e. **Konto** (optional): ein **allgemeiner** Speicherkonto verknüpfen (Link) zu Batch. Weitere Informationen finden Sie unter [verknüpfte Azure Storage-Konto](#linked-azure-storage-account) unter.

4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.

  Das Portal gibt an, dass **Bereitstellen** des Kontos, und nach Abschluss des Vorgangs eine Benachrichtigung **Bereitstellung erfolgreich** *Benachrichtigung*.

## <a name="view-batch-account-properties"></a>Batch Eigenschaften anzeigen

Sobald das Konto erstellt wurde, können Sie **Batch-Konto Blade** Zugriff auf die Systemeinstellungen und Eigenschaften öffnen. Mit dem linken Menü Blade Konto Batch können Sie alle Einstellungen und Eigenschaften zugreifen.

![Batch-Konto Blade in Azure-portal][account_blade]

* **Batch-Konto URL**: Applikationen mit der [Stapelverarbeitung APIs](batch-technical-overview.md#batch-development-apis) erstellen Konto URL Ressourcen und Aufträgen im Konto benötigen. Eine Stapelverarbeitung Konto URL hat das folgende Format:

    `https://<account_name>.<region>.batch.azure.com`

![Batch-Konto URL im portal][account_url]

* **Zugriffstasten**: Anwendung benötigen auch eine Zugriffstaste beim Arbeiten mit Ressourcen in Ihrem Konto Batch. Um anzuzeigen oder Regenerieren der Batch-Konto Zugriffstasten, geben Sie `keys` im **Suchfeld** im linken Menü auf den Stapel Konto **Schlüssel**wählen.

    ![Batch-kontoschlüssel in Azure-portal][account_keys]

## <a name="pricing"></a>Preisgestaltung

Batch-Konten werden nur in einer "freien Tier" womit belastet werden nicht für Batch-Konto selbst angeboten. Sie Zahlen zugrunde liegenden Azure computeressourcen, die Stapelverarbeitung Projektmappen nutzen und die Ressourcen von anderen Diensten Ausführung die Arbeitslasten. Angenommen, Sie Zahlen für den Compute-Knoten in Pools und Daten speichern in Azure Storage als Eingabe oder Ausgabe für Ihre Aufgaben. Bei Verwendung die Funktion [Anwendungspakete](batch-application-packages.md) Charge werden Sie auch zum Speichern der Anwendungspakete Azure Storage-Ressourcen berechnet. Siehe [Stapel Preise] [ batch_pricing] Weitere Informationen.

## <a name="linked-azure-storage-account"></a>Verknüpfte Azure Storage-Konto

Wie bereits erwähnt, können Sie (optional) ein **allgemeiner** Speicherkonto Batch-Konto verknüpfen. [Anwendungspakete](batch-application-packages.md) Feature des Stapels verwendet BLOB-Speicher in verknüpfte allgemeine Speicherkonto wie [Batch Datei Konventionen .NET](batch-task-output.md) Library. Dieser optionalen Features Sie bei der Bereitstellung die Stapelverarbeitungsaufgaben ausgeführt Applications und Beibehalten von Daten, die sie erzeugen.

Batch derzeit unterstützt *nur* **Allgemeine** Speicher-Kontotyp wie in Schritt 5 [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) [zum Azure-Speicherkonten](../storage/storage-create-storage-account.md)beschrieben. Beim Verknüpfen von Azure Storage-Konto mit Batch-Konto werden Sie sicher Link *nur* ein Speicherkonto für **allgemeine Zwecke** .

![Erstellen ein Speicherkonto "Allzweck"][storage_account]

Wir empfehlen ein Speicherkonto für die exklusive Verwendung von Batch-Konto erstellen.

>[AZURE.WARNING] Achten Sie beim Regenerieren der Zugriffstasten ein verknüpftes Speicherkonto. Regenerieren Sie nur ein Konto Speicherschlüssel und verknüpfte Speicher Konto Blade **Sync-Schlüssel** auf. Warten Sie fünf Minuten, sollen die Schlüssel an den Compute-Knoten in die Pools weitergegeben Regenerieren und Synchronisieren des Schlüssels. Beide Tasten gleichzeitig Regenerieren Compute-Knoten dürfen entweder Schlüssel synchronisiert und sie verlieren den Zugriff auf das Speicherkonto.

  ![Konto Speicherschlüssel Regenerieren][4]

## <a name="batch-service-quotas-and-limits"></a>Batch-Dienst Vorgaben und Grenzwerte

Beachten der Azure-Abonnement und anderen Azure Services bestimmte [Vorgaben und Grenzwerte](batch-quota-limit.md) Batch-Konten zuweisen. Aktuelle Kontingente für Batch-Kontos im Portal in das Konto **Eigenschaften**angezeigt.

![Batch-Konto Kontingente in Azure-portal][quotas]

Beachten Sie diese Kontingente entwerfen und Batch-Workloads skalieren. Z. B. wenn das Pool Zielnummer von Compute-Knoten erreichen ist nicht angegeben haben, können Sie Core Kontingentgrenze für Ihr Konto Batch erreicht haben.

Beachten Sie, dass Sie nicht auf ein einziges Konto Batch Azure-Abonnement beschränkt werden. Sie können mehrere Stapel Arbeitslasten in einem Batch-Konto oder verteilen die Arbeitsbelastungen von Batch-Konten in dieselbe Abonnement jedoch in Azure Regionen.

Viele dieser Kontingente können einfach mit Produkt-Support in Azure-Portal übermittelt erhöht werden. Weitere Informationen auf Kontingentsgrenzen [Kontingente und Grenzwerte für Azure Batch-Dienst](batch-quota-limit.md) zu.

## <a name="other-batch-account-management-options"></a>Andere Batch Konto Management-Optionen

Neben des Azure-Portals, können Sie auch Konten Batch mit folgenden:

* [Batch-PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [Batchverwaltung .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Nächste Schritte

* Finden Sie unter [Überblick über die Funktionen von Azure Batch](batch-api-basics.md) Batch Konzepte und Features erfahren. Der Artikel beschreibt primären Batch Ressourcen wie Pools, Compute-Knoten, Aufträgen und Aufgaben und bietet eine Übersicht über die Service-Funktionen, die umfangreiche Compute Arbeitslast können.

* Grundlagen der Entwicklung einer Anwendung Stapel aktiviert die [Clientbibliothek Batch .NET](batch-dotnet-get-started.md). [Einführende Artikel](batch-dotnet-get-started.md) führt Sie durch eine funktionierende Anwendung, die Batch-Dienst eine Arbeitslast auf mehrere Compute-Knoten ausgeführt wird und Azure Storage Arbeitslast Datei Staging und Abrufen verwenden.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Konto Speicherschlüssel Regenerieren"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
