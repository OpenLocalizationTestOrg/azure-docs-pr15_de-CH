<properties
    pageTitle="Wiederherstellen einer Azure SQL-Datenbank aus einer Sicherung Geo-redundant (PowerShell) | Microsoft Azure"
    description="Wiederherstellen einer Azure SQL-Datenbank auf einen neuen Server aus Geo-redundantes backup"
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
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Wiederherstellen Sie eine SQL Azure-Datenbank mithilfe von PowerShell aus einer Geo redundante Sicherung


> [AZURE.SELECTOR]
- [Übersicht](sql-database-recovery-using-backups.md)
- [Geo-Wiederherstellung: Azure-Portal](sql-database-geo-restore-portal.md)

Dieser Artikel veranschaulicht das Wiederherstellen der Datenbank auf einen neuen Server mit Geo-Wiederherstellung. Dies kann über PowerShell erfolgen.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>Geo-Wiederherstellung der Datenbank in einer eigenständigen Datenbank

1. Abrufen der Geo redundante Sicherung der Datenbank mithilfe der [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx)-Cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie die Wiederherstellung von Geo redundantes Backup mit [Restore-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx)-Cmdlet.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Geo-Wiederherstellung der Datenbank in einem Datenbankpool elastische

1. Abrufen der Geo redundante Sicherung der Datenbank mithilfe der [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx)-Cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten Sie die Wiederherstellung von Geo redundantes Backup mit [Restore-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx)-Cmdlet. Angeben der Name in die Datenbank wiederherstellen möchten.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md).
- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte Backups](sql-database-automated-backups.md).
- Automatisierte Backups Recovery mit finden Sie unter [Wiederherstellen einer Datenbank aus den Sicherungskopien Dienst initiiert](sql-database-recovery-using-backups.md).
- Schnellere Recovery-Optionen finden Sie unter [Aktive geografische Replikation](sql-database-geo-replication-overview.md).  
- Archivierung mit automatischen Sicherungen finden Sie unter [Datenbank kopieren](sql-database-copy.md).
