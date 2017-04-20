<properties
   pageTitle="Migration einer SQL Server-Datenbank zu Azure SQL-Datenbank | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbank bereitstellen, Datenbankmigration Datenbank importieren exportieren Datenbank Migrationsassistenten"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>Importieren von BACPAC in SQL-Datenbank mit SSMS

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure-portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

Dieser Artikel beschreibt wie Importieren aus einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei in SQL-Datenbank mit dem Export-Tier-Anwendung-Assistenten in SQL Server Management Studio.

> [AZURE.NOTE] Die folgenden Schritte gehen die logische SQL Azure-Instanz bereits bereitgestellt haben und die Informationen zur hand haben.

1. Überprüfen Sie, ob Sie die neueste Version von SQL Server Management Studio. Neue Versionen von Management Studio werden monatliche Updates Azure-Portal synchronisiert bleiben.

     > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Verbindung zum Azure SQL-Datenbank-Server den Ordner **Datenbanken** und **Datenebene Anwendung importieren**

    ![Datenebene Anwendung Menüelement importieren](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  Zum Erstellen der Datenbank in Azure SQL-Datenbank importieren Sie vom lokalen Datenträger BACPAC oder der Azure-Speicherkonto und Container, die BACPAC-Datei hochgeladen.

    ![Importieren](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Beim Importieren einer BACPAC von Azure BLOB-Speicher verwenden Sie standard Speicherung. Importieren einer BACPAC von Premium-Speicher wird nicht unterstützt.

4.  Die **neuen Datenbanknamen** für die Datenbank auf Azure SQL DB, **Edition von Microsoft Azure SQL-Datenbank** (Service-Ebene), **Maximale Datenbankgröße**und **Service-Ziel** (Performance Level) festgelegt.

    ![Datenbank-Einstellung](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Klicken Sie auf **Weiter** , und klicken Sie dann auf **Fertig stellen** , um BACPAC-Datei in eine neue Datenbank in Azure SQL-Datenbankserver importieren.

6. Schließen Sie in Objekt-Explorer an die migrierte Datenbank in der Azure SQL-Datenbankserver.

6.  Zeigen Sie Azure-Portal Ihre Datenbank und deren Eigenschaften.

## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
