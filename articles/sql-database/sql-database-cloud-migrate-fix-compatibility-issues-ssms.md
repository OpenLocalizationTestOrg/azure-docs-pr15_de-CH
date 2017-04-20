<properties
   pageTitle="SQL Server-Datenbank Kompatibilitätsprobleme mit SQL Server Management Studio vor der Migration zu SQL Datenbank beheben | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration, Kompatibilität, SQL Azure-Migrationsassistenten"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>SQL Server-Datenbank Kompatibilität Probleme mit SQL Server Management Studio vor der Migration zu SQL-Datenbank

> [AZURE.SELECTOR]
- Verwenden Sie [SQL Azure-Assistenten](sql-database-cloud-migrate-fix-compatibility-issues.md)
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) verwenden
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) verwenden

Fortgeschrittene Benutzer können SQL Server Datenbank-Kompatibilitätsproblemen mit SQL Server Management Studio vor der Migration in Azure SQL-Datenbank beheben.


> [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>Verwenden SQL Server Management Studio

Verwenden Sie SQL Server Management Studio zum Beheben von Kompatibilitätsproblemen mit Transact-SQL-Befehlen, wie **ALTER DATABASE**. Diese Methode ist hauptsächlich für fortgeschrittene Benutzer fühlen Transact-SQL einer gearbeitet. Andernfalls wird empfohlen, dass Sie SSDT verwenden. 



## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migrieren Sie eine SQL Server-kompatible Datenbank mit SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
