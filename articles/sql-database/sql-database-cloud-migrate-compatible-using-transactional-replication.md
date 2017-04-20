<properties
   pageTitle="Migrieren zu SQL-Datenbank mithilfe der Transaktionsreplikation | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank importieren Transaktionsreplikation"
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
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>Migrieren von SQL Server-Datenbank in Azure SQL-Datenbank mithilfe der Transaktionsreplikation

> [AZURE.SELECTOR]
- [SSMS Migrationsassistenten](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC-Datei exportieren](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [BACPAC importieren](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

In diesem Artikel lernen Sie eine SQL Server-kompatible Datenbank in Azure SQL-Datenbank mit minimaler Ausfallzeit mithilfe der Transaktionsreplikation SQL Server migrieren.

## <a name="understanding-the-transactional-replication-architecture"></a>Grundlegendes zur Architektur der Transaktionsreplikation

Wenn ausnahmslos Produktion während die Migration wird die SQL Server-Datenbank entfernen, können Sie SQL Server Transaktionsreplikation als migrationslösung. Um diese Lösung zu verwenden, konfigurieren Sie Ihre Azure SQL-Datenbank als Abonnent für die lokale SQL Server-Instanz, die Sie migrieren möchten. Lokalen Transaktionsreplikation Verteiler Daten aus der lokalen Datenbank synchronisiert (Herausgeber) synchronisiert werden, während neue Transaktionen weiterhin auftreten. 

Transaktionsreplikation können Sie eine Teilmenge der lokalen Datenbank migrieren. Die Publikation, die Sie in Azure SQL-Datenbank replizieren kann auf eine Teilmenge der Tabellen in der Datenbank repliziert. Für jede Tabelle repliziert wird können die Daten auf eine Teilmenge der Zeilen oder eine Teilmenge der Spalten einschränken.

Für die Transaktionsreplikation werden alle Änderungen auf Daten oder Schema in Ihrer Azure SQL-Datenbank. Wenn die Synchronisierung abgeschlossen ist und Sie migrieren, ändern Sie die Verbindungszeichenfolge der Anwendung sie Ihre Azure SQL-Datenbank auf. Sobald Transaktionsreplikation Änderungen in der lokalen Datenbank und alle Azure DB Anwendung darauf verlassen verbraucht, können Sie die Transaktionsreplikation deinstallieren. Der SQL Azure-Datenbank wird jetzt das Produktionssystem.

 ![SeedCloudTR Diagramm](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Bedarf nach Transaktions Replikation

Transaktionsreplikation ist eine Technologie, integrierte und SQL Server integriert, seit SQL Server 6.5. Es ist eine ausgereifte und bewährte Technologie, die meisten Datenbankadministratoren wissen, mit denen sie Erfahrung. [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)kann jetzt als [Transaktionsreplikation Abonnenten](https://msdn.microsoft.com/library/mt589530.aspx) der Publikation vor Ort Ihrer Azure SQL-Datenbank konfigurieren. Die Erfahrung, die es von Management Studio eingerichtet erhalten entspricht der Transaktionsreplikation Abonnent auf einem lokalen Server einrichten. Unterstützung für dieses Szenario wird unterstützt, wenn der Verleger und der Verteiler mindestens die folgenden SQL Server-Versionen sind:

 - SQL Server 2016 und höher 
 - SQL Server 2014 SP1 CU3 und höher
 - SQL Server 2014 RTM CU10 und höher
 - SQL Server 2012 SP2 CU8 und höher
 - SQL Server 2012 SP3 und höher


> [AZURE.IMPORTANT] Verwenden Sie die neueste Version von SQL Server Management Studio mit Updates für Microsoft Azure und SQL Datenbank synchronisiert bleiben. Ältere Versionen von SQL Server Management Studio können nicht als Abonnent SQL-Datenbank einrichten. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Transaktionsreplikation](https://msdn.microsoft.com/library/mt589530.aspx)
- [SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
