<properties
   pageTitle="Ermitteln Sie die SQL-Datenbank-Kompatibilität mit SqlPackage.exe | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration, SQL Datenbank Kompatibilität SqlPackage"
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

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>Ermitteln Sie die SQL-Datenbank-Kompatibilität mit SqlPackage.exe

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

In diesem Artikel erfahren Sie, ob eine SQL Server-Datenbank auf SQL-Datenbank mit dem Befehlszeilen-Dienstprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) migrieren kompatibel ist.

## <a name="using-sqlpackageexe"></a>SqlPackage.exe verwenden

1. Öffnen Sie ein Eingabeaufforderungsfenster, und ändern Sie ein Verzeichnis mit der neuesten Version des sqlpackage.exe. Dieses Dienstprogramm ist mit den neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server-Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)oder die neueste Version von [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) können direkt aus dem Microsoft Downloadcenter herunterladen.
2. Befehl der folgenden SqlPackage mit den folgenden Argumenten für Ihre Umgebung:

    "sqlpackage.exe /Action:Export /ssn: /sdn < Servername >: < Datenbankname > /tf: < Target_file > /p:TableData < schema_name.table_name >>< = Ausgabedatei > 2 > & 1

  	| Argument  | Beschreibung  |
  	|---|---|
  	| < Servername >  | Name des Quellservers  |
  	| < Datenbankname >  | Name der Quelldatenbank  |
  	| < Target_file >  | Dateinamen und Speicherort für die Datei BACPAC  |
  	| < schema_name.table_name >  | Tabellen für die Daten an die Zieldatei ausgegeben  |
  	| < Ausgabedatei >  | Dateinamen und Speicherort für die Ausgabedatei Fehler, falls vorhanden  |

    Der Grund für das /p:TableName-Argument ist, dass wir nur für Datenbank-Kompatibilitätsgrad für die Ausfuhr in Azure SQL DB V12 testen, anstatt die Daten aus allen Tabellen exportieren. Das Argument exportieren sqlpackage.exe unterstützt keine Tabellen extrahieren leider nicht. Sie müssen mindestens eine Tabelle wie eine kleine Tabelle angeben. < Ausgabedatei > enthält der Bericht alle Fehler. Die "> 2 > & 1" Zeichenfolge leitet die Standardausgabe und den Standardfehler durch Ausführen des Befehls angegebenen Ausgabedatei.

    ![Exportieren einer Anwendung auf Datenebene im Aufgaben](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Öffnen der Ausgabedatei und überprüfen die Kompatibilitätsfehler gegebenenfalls. 

    ![Exportieren einer Anwendung auf Datenebene im Aufgaben](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
[neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Beheben von Kompatibilitätsproblemen Datenbankmigration](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migrieren Sie eine SQL Server-kompatible Datenbank mit SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
