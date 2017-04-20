<properties
   pageTitle="Exportieren eine SQL Server-Datenbank in eine SqlPackage mit BACPAC | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank exportieren, BACPAC Datei Sqlpackage exportieren"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Exportieren einer SQL Server-Datenbank in eine SqlPackage mit BACPAC

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

Dieser Artikel veranschaulicht, wie eine SQL Server-Datenbank in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) mit dem Befehlszeilenprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) exportieren. Dieses Dienstprogramm ist mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server-Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)oder die neueste Version von [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) können direkt aus dem Microsoft Downloadcenter herunterladen.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und ändern Sie ein Verzeichnis mit dem Befehlszeilenprogramm sqlpackage.exe: Dieses Dienstprogramm ist im Lieferumfang von Visual Studio und SQL Server. Suche auf Ihrem Computer den Pfad in Ihrer Umgebung verwenden.
2. Führen Sie den folgenden sqlpackage.exe Befehl die folgenden Argumente für Ihre Umgebung:

    "sqlpackage.exe /Action:Export /ssn: /sdn < Servername >: < Datenbankname > /tf: < Target_file >

  	| Argument  | Beschreibung  |
  	|---|---|
  	| < Servername >  | Name des Quellservers  |
  	| < Datenbankname >  | Name der Quelldatenbank  |
  	| < Target_file >  | Dateinamen und Speicherort für die Datei BACPAC  |

    ![Exportieren einer Anwendung auf Datenebene im Aufgaben](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

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
