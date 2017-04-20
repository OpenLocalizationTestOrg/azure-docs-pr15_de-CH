
<properties
   pageTitle="Exportieren eine SQL Server-Datenbank in eine SQL Server Management Studio mit BACPAC | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank exportieren, BACPAC Datei exportieren Data-Tier Application Wizard exportieren"
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
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>Exportieren einer SQL Server-Datenbank in eine SQL Server Management Studio mit BACPAC

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
Dieser Artikel beschreibt, wie eine SQL Server-Datenbank in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei mit dem Export-Tier-Anwendung-Assistenten in SQL Server Management Studio zu exportieren. 

1. Überprüfen Sie, ob Sie die neueste Version von SQL Server Management Studio. Neue Versionen von Management Studio werden monatliche Updates Azure-Portal synchronisiert bleiben.

     > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Management Studio öffnen Sie und die Datenbank im Objekt-Explorer an.

    ![Exportieren einer Anwendung auf Datenebene im Aufgaben](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Mit der rechten Maustaste im Objekt-Explorer der Quelldatenbank, **Tasks**zeigen und auf **Datenebene Anwendung exportieren**

    ![Exportieren einer Anwendung auf Datenebene im Aufgaben](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Konfigurieren Sie im Export-Assistenten exportieren, um der BACPAC Datei entweder einen lokalen Speicherort oder eine Azure blob. Exportierte BACPAC enthält immer das vollständige Datenbankschema und Daten aus den Tabellen. Verwenden Sie die Registerkarte Erweitert ggf. einige oder alle Tabellen Daten ausschließen. Sie können beispielsweise nur die Daten für Tabellen nicht aus allen Tabellen exportieren.

***Wichtig*** Beim Exportieren einer BACPAC in Azure BLOB-Speicher verwenden Sie standard Speicherung. Importieren einer BACPAC von Premium-Speicher wird nicht unterstützt.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Importieren Sie eine BACPAC in Azure SQL-Datenbank mit SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Importieren einer BACPAC in Azure SQL-Datenbank SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Importieren einer BACPAC in Azure SQL-Datenbank Azure-portal](sql-database-import.md)
- [Importieren einer BACPAC in Azure SQL-Datenbank PowerShell](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
