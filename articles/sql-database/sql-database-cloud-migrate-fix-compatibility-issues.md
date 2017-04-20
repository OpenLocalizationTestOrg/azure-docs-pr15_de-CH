<properties
   pageTitle="SQL Server-Datenbank Kompatibilität Probleme vor der Migration zu SQL Datenbank | Microsoft Azure"
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

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>Verwenden Sie SQL Azure Assistenten beheben SQL Server Datenbank Kompatibilitätsprobleme vor der Migration in Azure SQL-Datenbank

> [AZURE.SELECTOR]
- Verwenden Sie [SQL Azure-Assistenten](sql-database-cloud-migrate-fix-compatibility-issues.md)
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) verwenden
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) verwenden

In diesem Artikel lernen Sie erkennen und Beheben von SQL Server-Datenbank Kompatibilitätsproblemen mit SQL Azure Migrations-Assistenten vor der Migration in Azure SQL-Datenbank.

## <a name="using-sql-azure-migration-wizard"></a>SQL Azure Assistenten für die Migration

Mit [SQL Azure-Migrationsassistenten](http://sqlazuremw.codeplex.com/) CodePlex Tool können aus einem inkompatiblen Quelldatenbank ein T-SQL-Skript generieren. Dieses Skript wird dann vom Assistenten kompatibel mit der SQL-Datenbank transformiert. Sie verbinden dann Azure SQL-Datenbank ausführen. Dieses Tool analysiert auch Ablaufverfolgungsdateien um Kompatibilitätsprobleme zu ermitteln. Das Skript kann nur Schema generiert oder Daten im BCP-Format enthalten. Zusätzliche Dokumentation steht auch schrittweise CodePlex im [Assistenten für die Migration von SQL Azure](http://sqlazuremw.codeplex.com/).  

 ![Diagramm zur Datenmigration SAMW](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Nicht alle inkompatible Schema vom Assistenten erkannt werden kann die integrierte Transformationen behoben werden. Inkompatible Skript adressiert werden kann werden als Fehler gemeldet, Kommentare in das generierte Skript eingefügt. Verwenden Sie viele Fehler Visual Studio oder SQL Server Management Studio durchgehen und beheben Fehler, die mit dem SQL Server-Migrationsassistenten nicht behoben werden konnte.

## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migrieren Sie eine SQL Server-kompatible Datenbank mit SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
