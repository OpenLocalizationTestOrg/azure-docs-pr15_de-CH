<properties
    pageTitle="Sichern und Wiederherstellen von App Service apps mithilfe von PowerShell"
    description="Informationen Sie zum Sichern und Wiederherstellen einer Anwendung in Azure App Service mithilfe von PowerShell"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Sichern und Wiederherstellen von App Service apps mithilfe von PowerShell

> [AZURE.SELECTOR]
- [PowerShell](app-service-powershell-backup.md)
- [REST-API](../app-service-web/websites-csm-backup.md)

Informationen Sie zum Sichern und Wiederherstellen von [App Service apps](https://azure.microsoft.com/services/app-service/web/)mithilfe von Azure PowerShell. Weitere Informationen zu Web app Backups, einschließlich Vorschriften und Einschränkungen finden Sie unter [Sichern einer Web-app in Azure App Service](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Erforderliche Komponenten
Verwendung von PowerShell Backups app verwalten, benötigen Sie Folgendes:

- **Ein SAS-URL** , der Lese- und Schreibzugriff auf eine Azure Storage Container ermöglicht. Eine Erläuterung der SAS-URLs finden Sie unter [Understanding SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md) . Azure Speichermanagement mit PowerShell-Beispiele finden Sie unter [Verwendung von Azure PowerShell mit Azure-Speicher](../storage/storage-powershell-guide-full.md) .
- **Verbindungszeichenfolge für eine Datenbank** wollen Sie Sichern einer Datenbank mit Ihrer Anwendung.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Wie SAS-URL mit Web app backup cmdlets
SAS-URL kann mit PowerShell generiert werden. Hier ist ein Beispiel zu generieren, die in diesem Artikel beschriebenen Cmdlets verwendet werden können.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Installieren Sie Azure PowerShell 1.3.2 oder größer

Installation und Verwendung von Azure PowerShell finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-install-configure.md) .

## <a name="create-a-backup"></a>Erstellen einer Sicherung

Verwenden Sie das Cmdlet neu AzureRmWebAppBackup zum Sichern von Web app.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Dies erstellt eine Sicherung mit einem automatisch generierten Namen. Wenn Sie einen Namen für die Sicherung angeben möchten, verwenden Sie BackupName optionalen Parameter.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Um eine Datenbank in der Sicherung enthalten, erstellen Sie eine Datenbank backup-Einstellung mit dem New-AzureRmWebAppDatabaseBackupSetting-Cmdlet zunächst Geben Sie dieser Einstellung in Datenbanken Parameter des Cmdlets New-AzureRmWebAppBackup an. Datenbanken-Parameter akzeptiert ein Array von datenbankeinstellungen können Sie mehrere Datenbanken sichern.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Abrufen von backups

Das Cmdlet "Get-AzureRmWebAppBackupList" gibt ein Array aller Backups für eine Webanwendung. Geben Sie den Namen des Web app und einer Ressourcengruppe.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Um eine bestimmte Sicherung abzurufen, verwenden Sie das Cmdlet "Get-AzureRmWebAppBackup".

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

In backup-Management-Cmdlets zur Vereinfachung können Sie auch ein Web app-Objekt übergeben.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Automatisch Sicherungskopien

Sie können Backups automatisch in angegebenen Intervallen planen. Konfigurieren Sie einen Sicherungszeitplan entsprechend dem Cmdlet AzureRmWebAppBackupConfiguration bearbeiten. Dieses Cmdlet hat mehrere Parameter:

- **Name** - der Name der Web-app.
- **ResourceGroupName** - der Name der Ressourcengruppe, die Web-app.
- **Steckplatz** - Optional. Der Name des Web app Steckplatz.
- **StorageAccountUrl** - die SAS-URL für den Azure-Speicher Container zum Speichern von Sicherungen.
- **FrequencyInterval** - numerischen Wert für die Häufigkeit der Sicherung erfolgen soll. Eine positive ganze Zahl muss sein.
- **FrequencyUnit** - Zeiteinheit für die Häufigkeit der Sicherung erfolgen soll. Optionen sind Stunden und Tage.
- **RetentionPeriodInDays** - Anzahl von Tagen automatischen Sicherungskopien gespeichert werden sollen, bevor Sie automatisch gelöscht werden.
- **StartTime** - Optional. Die Zeit, wenn die automatische Sicherung beginnen soll. Backups sofort bei null. Muss ein DateTime-Wert.
- **Datenbanken** - Optional. Ein Array von DatabaseBackupSettings für die Datenbanken zu sichern.
- **KeepAtLeastOneBackup** - Optional gewechselt Parameter. Geben Sie diesen ein Backup immer sollte im Speicherkonto, unabhängig davon, wie alt ist.

Es folgt ein Beispiel zur Verwendung dieses Cmdlets.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Um den aktuellen Sicherungszeitplan abzurufen, verwenden Sie das Cmdlet "Get-AzureRmWebAppBackupConfiguration". Dies ist nützlich zum Ändern eines Zeitplans, der bereits konfiguriert.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Eine Webanwendung aus einer Sicherung wiederherstellen

Um eine Webanwendung aus einer Sicherung wiederherzustellen, verwenden Sie Restore-AzureRmWebAppBackup-Cmdlet. Die einfachsten mithilfe dieses Cmdlet Pipe in einem backup-Objekt aus dem Cmdlet Get-AzureRmWebAppBackup oder Get-AzureRmWebAppBackupList-Cmdlet abgerufen.

Haben Sie ein backup-Objekt können Sie in der Restore-AzureRmWebAppBackup-Cmdlet übergeben werden. Geben Sie überschreiben Switch-Parameter um anzugeben, dass der Inhalt Ihrer Anwendung mit dem Inhalt der Sicherung überschrieben werden soll. Das Backup-Datenbanken enthält, werden diese Datenbanken sowie wiederhergestellt.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Es folgt ein Beispiel zum Wiederherstellen-AzureRmWebAppBackup verwenden, die Parameter.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Löschen einer Sicherung

Um eine Sicherung zu löschen, verwenden Sie das Cmdlet "Remove-AzureRmWebAppBackup". Die Sicherung entfernt aus das Speicherkonto. Geben Sie Namen, die Ressourcengruppe und die ID der Sicherung, die Sie löschen möchten.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Ein backup-Objekt leiten Sie in Remove-AzureRmWebAppBackup-Cmdlet zu löschen.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
