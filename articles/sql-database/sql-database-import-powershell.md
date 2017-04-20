<properties
    pageTitle="Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell | Microsoft Azure"
    description="Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell

**Datenbank**

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Dieser Artikel beschreibt, wie Sie für SQL Azure-Datenbank durch Importieren einer Datei [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) mit PowerShell erstellen.

Die Datenbank wird aus einer BACPAC-Datei (.bacpac) von Azure BLOB-Speichercontainer importiert erstellt. Haben Sie eine BACPAC in Azure-Speicher, anzeigen Sie [Archiv einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe von PowerShell](sql-database-export-powershell.md) Haben Sie bereits eine BACPAC, die nicht in Azure Storage [mit AzCopy problemlos Azure Storage-Konto hochladen](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Azure SQL-Datenbank automatisch erstellt und verwaltet Sicherungskopien für jede Datenbank, die Sie wiederherstellen können. Details finden Sie in der [SQL-Datenbank automatisierte Backups](sql-database-automated-backups.md).


Um eine SQL-Datenbank zu importieren, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie Azure-Abonnement lediglich **Testversion** am oberen Rand dieser Seite klicken Sie und dann wieder zu diesem Artikel.
- Eine BACPAC der Datenbank, die Sie importieren möchten. Die BACPAC muss in einem [Speicherkonto Azure](../storage/storage-create-storage-account.md) BLOB-Container.



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>Variablen Sie die für Ihre Umgebung

Es gibt einige Variablen sollten bestimmte Werte für die Datenbank und das Speicherkonto Beispielwerte ersetzen.

Der Servername sollte ein Server, der im vorherigen Schritt ausgewählte Abonnement existiert. Es sollte der Server die Datenbank erstellt werden soll. Importieren einer Datenbank direkt in einer elastischen Pool wird nicht unterstützt. Aber zunächst in eine Datenbank importieren und die Datenbank in einem Pool verschieben.

Der Datenbankname ist der Name für die neue Datenbank ein.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Die folgenden Variablen werden von Storage-Konto Ihre BACPAC befindet. Suchen Sie in [Azure-Portal](https://portal.azure.com)das Speicherkonto, diese Werte abzurufen. Den primäre Schlüssel finden Sie auf **Alle Einstellungen** , **Schlüssel** in das Speicherkonto Blade.

Der BLOB-Name ist der Name einer vorhandenen BACPAC-Datei, die die Datenbank erstellen möchten. Sie müssen die Erweiterung .bacpac.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Ausgeführt [Get-Credential] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx)-Cmdlet öffnet ein Fenster für Ihren Benutzernamen und Ihr Kennwort. Geben Sie Admin-Benutzername und Kennwort für die SQL-Datenbankserver ($ServerName von oben) und nicht den Benutzernamen und Passwort Azure.

    $credential = Get-Credential


## <a name="import-the-database"></a>Importieren der Datenbank

Dieser Befehl sendet eine Import Datenbank-Anforderung an den Dienst. Je nach Größe der Datenbank dauert der Importvorgang einige Zeit in Anspruch.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>Überwachen Sie den Fortschritt des Vorgangs

Nach dem Ausführen von [New AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), Sie können den Status Überprüfen der Anforderung mit [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>SQL Datenbank PowerShell Skript


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Nächste Schritte

- Und eine importierte SQL Datenbank finden Sie unter [mit SQL-Datenbank mit SQL Server Management Studio und eine T-SQL-Abfrage](sql-database-connect-query-ssms.md)
