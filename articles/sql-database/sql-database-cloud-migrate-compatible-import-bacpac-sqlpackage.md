<properties
   pageTitle="SQL-Datenbank aus einer BACPAC Datei SqlPackage importieren"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration importieren, importieren BACPAC Datei sqlpackage"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>SQL-Datenbank aus einer BACPAC Datei SqlPackage importieren

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure-portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

Dieser Artikel beschreibt, wie aus einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei mit dem Befehlszeilenprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) auf SQL-Datenbank importieren. Dieses Dienstprogramm ist mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server-Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)oder die neueste Version von [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) können direkt aus dem Microsoft Downloadcenter herunterladen.


> [AZURE.NOTE] Folgendermaßen wird davon ausgegangen, dass einen SQL Server bereits bereitgestellt haben, die Informationen zur hand haben und überprüft haben, dass die Datenbank kompatibel ist.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Azure SQL-Datenbank mit SqlPackage aus einer BACPAC-Datei importieren

Gehen Sie mit dem Befehlszeilenprogramm [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) kompatiblen SQL Server-Datenbank (oder SQL Azure-Datenbank) importieren aus einer Datei BACPAC.

> [AZURE.NOTE] Die folgenden Schritte gehen, einen Azure SQL-Datenbankserver bereits bereitgestellt haben und die Informationen auf Seite.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und ändern Sie ein Verzeichnis mit dem Befehlszeilenprogramm sqlpackage.exe: Dieses Dienstprogramm ist im Lieferumfang von Visual Studio und SQL Server.
2. Führen Sie den folgenden sqlpackage.exe Befehl die folgenden Argumente für Ihre Umgebung:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Argument  | Beschreibung  |
  	|---|---|
  	| < Servername >  | Zielservername  |
  	| < Datenbankname >  | Zieldatenbankname  |
  	| < Benutzername >  | der Benutzername auf dem Zielserver |
  	| < Kennwort >  | das Kennwort des Benutzers  |
  	| < Source_file >  | Dateinamen und Speicherort für die zu importierende BACPAC-Datei  |

    ![Exportieren einer Anwendung auf Datenebene im Aufgaben](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
