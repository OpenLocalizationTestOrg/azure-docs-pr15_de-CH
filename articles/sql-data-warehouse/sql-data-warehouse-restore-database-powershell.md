<properties
   pageTitle="Wiederherstellen einer Azure SQL Datawarehouse (PowerShell) | Microsoft Azure"
   description="PowerShell-Aufgaben für die Wiederherstellung einer Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Wiederherstellen einer Azure SQL Datawarehouse (PowerShell)

> [AZURE.SELECTOR]
- [Übersicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In diesem Artikel erfahren Sie, wie einer Azure SQL Data Warehouse mit PowerShell wiederhergestellt.

## <a name="before-you-begin"></a>Bevor Sie beginnen

**Überprüfen Sie die Kapazität Ihres DTU.** Jede SQL Data Warehouse wird von SQL Server (z.B. myserver.database.windows.net) gehostet hat ein Kontingent DTU.  Überprüfen Sie vor dem Wiederherstellen einer SQL Data Warehouse, die Ihren SQL Server hat genügend verbleibenden DTU Kontingent für die Datenbank wiederhergestellt wird. DTU Bedarf berechnen und weitere DTU Informationen finden Sie unter [DTU Kontingent Änderung anfordern][].

### <a name="install-powershell"></a>Installieren Sie PowerShell

Für die Nutzung der Azure PowerShell mit SQL Data Warehouse müssen Sie Azure PowerShell Version 1.0 oder höher installieren.  Überprüfen Sie die Version mit **Get-Modul - ListAvailable-Namen-AzureRM**.  Die neueste Version kann vom [Microsoft-Webplattform-Installer][]installiert werden.  Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

## <a name="restore-an-active-or-paused-database"></a>Wiederherstellen einer Datenbank aktiven oder angehalten

Zum Wiederherstellen eine Datenbank von einem Snapshot [Wiederherstellen AzureRmSqlDatabase][] PowerShell-Cmdlets verwenden.

1. Öffnen Sie Windows PowerShell.
2. Azure-Konto verbinden, und alle Abonnements, die mit Ihrem Konto verknüpfte Liste.
3. Wählen Sie das Abonnement mit der Datenbank wiederhergestellt werden.
4. Liste der Wiederherstellungspunkte für die Datenbank.
5. Wählen Sie den gewünschten Wiederherstellungspunkt mithilfe der RestorePointCreationDate.
6. Wiederherstellen der Datenbank auf den gewünschten Wiederherstellungspunkt.
7. Stellen Sie sicher, dass die wiederhergestellte Datenbank online ist.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Nach Abschluss die Wiederherstellung können Sie die wiederhergestellte Datenbank folgende [Datenbank nach Wiederherstellung konfigurieren][].


## <a name="restore-a-deleted-database"></a>Eine gelöschte Datenbank wiederherstellen

Verwenden Sie zum Wiederherstellen einer gelöschten Datenbank [Wiederherstellen-AzureRmSqlDatabase][] -Cmdlet.

1. Öffnen Sie Windows PowerShell.
2. Azure-Konto verbinden, und alle Abonnements, die mit Ihrem Konto verknüpfte Liste.
3. Wählen Sie das Abonnement mit der gelöschten Datenbank wiederhergestellt werden.
4. Gelöschte datenbankspezifische zu erhalten.
5. Wiederherstellen Sie gelöschte Datenbank.
6. Stellen Sie sicher, dass die wiederhergestellte Datenbank online ist.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Nach Abschluss die Wiederherstellung können Sie die wiederhergestellte Datenbank folgende [Datenbank nach Wiederherstellung konfigurieren][].


## <a name="restore-from-an-azure-geographical-region"></a>Wiederherstellen von Azure geografischen region

Verwenden Sie zum Wiederherstellen einer Datenbank [Wiederherstellen-AzureRmSqlDatabase][] -Cmdlet.

1. Öffnen Sie Windows PowerShell.
2. Azure-Konto verbinden, und alle Abonnements, die mit Ihrem Konto verknüpfte Liste.
3. Wählen Sie das Abonnement mit der Datenbank wiederhergestellt werden.
4. Rufen Sie die Datenbank wiederherstellen möchten.
5. Erstellen Sie die Anfrage zur Wiederherstellung der Datenbank.
6. Überprüfen Sie den Status der Datenbank Geo wiederhergestellt.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Konfigurieren die Datenbank nach der Wiederherstellung finden Sie in der [Datenbank nach Wiederherstellung konfigurieren][]. 


Die wiederhergestellte Datenbank werden TDE aktiviert, wenn die Quelldatenbank TDE aktiviert ist.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Business Continuity-Funktionen von Azure SQL Database Edition lesen Sie [Azure SQL-Datenbank Business Continuity – Überblick][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank Business Continuity – Überblick]: sql-database-business-continuity.md
[DTU Kontingent Änderung anfordern]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Datenbank nach Wiederherstellung konfigurieren]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Zum Installieren und Konfigurieren von Azure PowerShell]: powershell-install-configure.md
[Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Datenbank nach Wiederherstellung konfigurieren]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Wiederherstellung AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft-Webplattform-Installer]: https://aka.ms/webpi-azps
