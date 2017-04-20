<properties
   pageTitle="Erste Schritte mit Azure Batch PowerShell | Microsoft Azure"
   description="Eine schnelle Einführung in Azure PowerShell-Cmdlets, mit Azure Batch-Dienst verwalten"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Erste Schritte mit Azure Batch PowerShell-cmdlets
Azure Batch PowerShell-Cmdlets können Sie Skripts viele Aufgaben, die mit Batch-APIs, Azure-Portal und der Azure-Befehlszeilenschnittstelle (CLI) durchführen und ausführen. Dies ist eine kurze Einführung in verwendeten Batch Konten verwalten und Arbeiten mit Batch Ressourcen-Pools, Projekte und Aufgaben Cmdlets.

Eine vollständige Liste der Batch-Cmdlets und detaillierte Cmdlet-Syntax finden Sie unter [Azure Batch Cmdlet Verweis](https://msdn.microsoft.com/library/azure/mt125957.aspx).

Dieser Artikel basiert auf Cmdlets in Azure PowerShell Version 3.0.0. Wir empfehlen aktualisieren Ihre Azure PowerShell zum Service-Updates und verbesserte nutzen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die folgenden Vorgänge um Azure PowerShell Batch Ressourcen verwalten.

* [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)

* Führen Sie das Cmdlet **Login-AzureRmAccount** für die Verbindung zu Ihrem Abonnement (Azure Batch Cmdlets Schiffes in Azure-Ressourcen-Manager-Modul):

    `Login-AzureRmAccount`

* **Mit Batch Anbieternamespace registrieren**. Dieser Vorgang muss nur **einmal pro Abonnement**ausgeführt werden.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Batch-Konten und Schlüssel verwalten

### <a name="create-a-batch-account"></a>Ein Batch-Konto erstellen

**New AzureRmBatchAccount** erstellt ein Konto Batch in einer angegebenen Ressourcengruppe. Haben Sie bereits eine Ressourcengruppe erstellen, indem Sie das [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) -Cmdlet ausführen. Geben Sie eines der Azure-Parameter **Speicherort** wie "USA". Zum Beispiel:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Dann registrieren Sie einen Batch in der Ressourcengruppe einen Namen für das Konto in <*Kontoname*> sowie Pfad und Name der Ressourcengruppe. Batch-Konto erstellen kann einige Zeit dauern. Zum Beispiel:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] Batch-Kontoname muss eindeutig Azure Region für die Ressourcengruppe zwischen 3 und 24 Zeichen enthalten und Kleinbuchstaben und Zahlen nur.

### <a name="get-account-access-keys"></a>Konto Zugriffsschlüssel abrufen
**Get-AzureRmBatchAccountKeys** zeigt die Zugriffstasten eine Azure Batch-Konto zugeordnet. Führen Sie z. B. Folgendes zu primären und sekundären Schlüssel für das Konto, das Sie erstellt haben.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Erstellen Sie eine neue Tastenkombination
**Neu AzureRmBatchAccountKey** einen neuen primären oder sekundären kontoschlüssel für ein Konto Azure Batch generiert. Generiert einen neuen Primärschlüssel für Batch-Konto beispielsweise:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Generiert einen neuen sekundären Schlüssel geben Sie an "Sekundär" für den Parameter **Schlüsseltyp** . Sie müssen die primären und sekundären Schlüssel separat neu generieren.

### <a name="delete-a-batch-account"></a>Batch-Konto löschen
**Entfernen AzureRmBatchAccount** Löscht einen Batch-Konto. Zum Beispiel:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Bestätigen Sie bei Aufforderung das Konto entfernen möchten. Entfernen des Kontos kann einige Zeit dauern.

## <a name="create-a-batchaccountcontext-object"></a>Erstellen Sie ein BatchAccountContext-Objekt

Um die Batch-PowerShell-Cmdlets verwenden, beim Erstellen und Verwalten von Batch-Pools, Projekte, Aufgaben und andere Ressourcen zu authentifizieren, zunächst ein BatchAccountContext-Objekt, um Ihren Benutzernamen und Schlüssel speichern:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Sie übergeben das Objekt BatchAccountContext in Cmdlets, die den **BatchContext** -Parameter verwenden.

> [AZURE.NOTE] Standardmäßig Primärschlüssel des Kontos für die Authentifizierung verwendet, aber Sie können den Schlüssel durch Ändern der BatchAccountContext des **KeyInUse** -Objekts explizit auswählen: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Erstellen und Ändern von Batch-Ressourcen
Verwenden Sie Cmdlets **Neu AzureBatchPool** **Neu AzureBatchJob**und **Neue AzureBatchTask** Ressourcen unter einem Batch-Konto erstellen. Entsprechende **Get-** und **Set -** Cmdlets zum Aktualisieren der Eigenschaften der vorhandenen Ressourcen und **Entfernen -** Cmdlets Ressourcen unter einem Batch-Konto entfernen.

Wenn viele dieser Cmdlets zusätzlich ein BatchContext-Objekt übergeben müssen Sie erstellen oder Objekte, die detaillierte Ressourcen wieder übergeben, wie im folgenden Beispiel gezeigt. Detaillierten Informationen für jedes Cmdlet Weitere Beispiele anzeigen

### <a name="create-a-batch-pool"></a>Erstellen eines Batch-Pools

Beim Erstellen oder Aktualisieren eines Batch-Pools, wählen Sie eine Cloud-Dienstkonfiguration oder Konfiguration des virtuellen Computers für das Betriebssystem auf den Compute-Knoten (siehe [Stapelverarbeitung Funktionsübersicht](batch-api-basics.md#pool)). Ihre Auswahl bestimmt, ob die Compute-Knoten mit einem [Gastbetriebssystem Azure frei](../cloud-services/cloud-services-guestos-update-matrix.md#releases) oder mit einem unterstützten Linux- oder Windows-VM Bilder in Azure Marketplace abgebildet werden.

Ausführung **Neu AzureBatchPool**übergeben Sie die Einstellung für ein PSCloudServiceConfiguration oder PSVirtualMachineConfiguration-Objekt. Das folgende Cmdlet erstellt beispielsweise einen neuen Batch Pool mit Größe kleine Compute-Knoten in der Cloud-Dienst mit der neuesten Betriebssystemversion Familie 3 (Windows Server 2012) abgebildet. Hier gibt der Parameter **CloudServiceConfiguration** die Variable *$configuration* als PSCloudServiceConfiguration-Objekt. Der Parameter **BatchContext** gibt zuvor definierte Variable *$context* als BatchAccountContext-Objekt.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Eine Formel für die automatische Skalierung Zielnummer von Compute-Knoten im neuen Pool bestimmt. In diesem Fall ist die Formel einfach **$TargetDedicated = 4**, höchstens 4 ist die Anzahl von Compute-Knoten im Pool.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Abfrage für Pools, Projekte, Aufgaben und andere details

Verwenden Sie Cmdlets **Get-AzureBatchPool** **Get-AzureBatchJob**und **Get-AzureBatchTask** Abfrage für Entitäten erstellt unter einem Batch-Konto.

### <a name="query-for-data"></a>Abfrage für Daten

Verwenden Sie beispielsweise **Get-AzureBatchPools** Pools gefunden. Dieser Abfragen für alle Pools unter Ihrem Konto, wenn Sie bereits standardmäßig BatchAccountContext Objekt in *$context*gespeichert:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Einen OData-Filter verwenden

Sie können einen OData-Filter mit **dem Filterparameter** finden Sie nur die Objekte angeben. Beispielsweise finden Sie alle Pools mit Ids mit "MyPool":

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Diese Methode ist nicht so flexibel wie "Where-Object" in einer lokalen Pipeline verwenden. Jedoch wird die Abfrage Batch-Dienst geschickt, damit alle auf dem Server Internetbandbreite sparen geschieht.

### <a name="use-the-id-parameter"></a>Verwenden Sie den Id-parameter

Eine Alternative zum OData Filter ist den **Id** -Parameter verwenden. Bei einem bestimmten Pool mit Id "MyPool" Abfragen:

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

**ID-** Parameter unterstützt Full-Id suchen, keine Platzhalter oder Filter OData-Stil.

### <a name="use-the-maxcount-parameter"></a>Verwenden Sie den Parameter MaxCount

Jedes Cmdlet gibt standardmäßig maximal 1000 Objekte zurück. Wenn Sie dieses Limit erreichen, verfeinern Sie den Filter wieder weniger Objekte oder explizit mit dem Parameter **MaxCount** höchstens. Zum Beispiel:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Entfernen Sie die obere Grenze festgelegt **MaxCount** 0 oder kleiner.

### <a name="use-the-powershell-pipeline"></a>Verwenden Sie die PowerShell-pipeline

Batch-Cmdlets können die PowerShell-Pipeline zum Senden von Daten zwischen Cmdlets nutzen. Dies entspricht der Angabe eines Parameters jedoch erleichtert die Arbeit mit mehreren Personen.

Z. B. Suchen und alle Aufgaben unter Ihrem Konto anzeigen:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Neustart (Reboot) jeder Compute-Knoten in einem Pool:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Verwalten von Paketen

Pakete können vereinfachte Anwendung Computeknoten in Pools bereitstellen. Mit Batch-PowerShell-Cmdlets können hochladen und Anwendungspakete im Batch Konto verwalten und Bereitstellen von Paketversionen Knoten berechnen.

**Erstellen** einer Anwendung:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Hinzufügen** eines Anwendungspakets:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Festlegen Sie die **Standardversion** für die Anwendung:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Liste** der Anwendungspakete

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Löschen** eines Anwendungspakets

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Löschen** einer Anwendung

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Sie müssen vor dem Löschen der Anwendungdes aller Versionen einer Anwendung Anwendung löschen. 'Konflikt' Fehler erhalten, wenn Sie versuchen, eine Anwendung zu löschen, die derzeit Pakete.

### <a name="deploy-an-application-package"></a>Ein Anwendungspaket bereitstellen

Sie können eine oder mehrere Anwendungspakete für die Bereitstellung bei der Erstellung eines Pools angeben. Wenn Sie ein Paket zur Erstellungszeit Pool angeben, wird als Knoten Joins Pool jeden Knoten bereitgestellt. Pakete werden auch bereitgestellt, wenn ein Knoten neu gestartet oder versehen ist.

Geben Sie die `-ApplicationPackageReference` option beim Erstellen eines Pools Antrag des Pools Knoten bereitstellen, wie sie das Pool. Zunächst erstellen Sie ein **PSApplicationPackageReference** -Objekt, und mit der Anwendung-Id und Paket des Pools Compute-Knoten bereitstellen möchten konfigurieren:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Jetzt Pool zu erstellen, und das Paket Verweis als Argument für die `ApplicationPackageReferences` Option:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Weitere Informationen über Pakete finden in [Bereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

>[AZURE.IMPORTANT] [Link Azure Storage-Konto](#linked-storage-account-autostorage) müssen Sie Ihrem Konto Batch Pakete verwenden.

### <a name="update-a-pools-application-packages"></a>Aktualisieren des Pools Anwendungspakete

Aktualisieren Sie die Anwendung zu einem vorhandenen Pool zugewiesen erstellen Sie zunächst ein PSApplicationPackageReference-Objekt mit den gewünschten Eigenschaften (Id und Paket Anwendungsversion):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Anschließend erhalten Sie den Pool aus, vorhandene Pakete löschen unserer neuen Paket Verweis hinzufügen und Updatedienst Batch bei Pool:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Sie haben jetzt die Pool-Eigenschaften im Batch-Dienst aktualisiert. Um neue Anwendungspaket Knoten im Pool berechnen bereitzustellen, müssen Sie jedoch starten oder image Knoten. Sie können alle Knoten in einem Pool mit diesem Befehl neu starten:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] Sie können mehrere Anwendungspakete auf Compute-Knoten in einem Pool bereitstellen. *Hinzufügen* ein Anwendungspakets statt der aktuell bereitgestellten Pakete ersetzen möchten, lassen die `$pool.ApplicationPackageReferences.Clear()` Zeile.

## <a name="next-steps"></a>Nächste Schritte

* Detaillierte Cmdlet Syntax und Beispiele finden Sie unter [Azure Batch Cmdlet Verweis](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Weitere Informationen über Anträge und Anwendungspakete im Stapel anzeigen Sie [Bereitstellung mit Azure Batch Pakete](batch-application-packages.md)
