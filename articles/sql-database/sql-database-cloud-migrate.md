<properties
   pageTitle="SQL Server-Datenbankmigration zu SQL Datenbank | Microsoft Azure"
   description="Erfahren Sie, wie lokale SQL Server-Datenbankmigration zu Azure SQL-Datenbank in der Cloud. Verwenden Sie Datenbank-Migrationstools bei Prüfung vor der Datenbankmigration."
   keywords="Datenbank-Migration, Migration zu Sql Server-Datenbank, Datenbank-Migrationstools, migrieren Sie die Datenbank, Sql-Datenbank migrieren"
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

# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>SQL Server-Datenbankmigration zu SQL-Datenbank in der cloud

In diesem Artikel lernen Sie eine lokale SQL Server 2005-Datenbank oder höher in Azure SQL-Datenbank migrieren. In dieser Datenbank Migrationsvorgang migrieren Sie das Schema und die Daten aus der SQL Server-Datenbank in der aktuellen Umgebung in SQL-Datenbank. Damit dies gelingt, muss die vorhandene Datenbank zuerst Compatibility Test. Mit [SQL Datenbank V12](sql-database-v12-whats-new.md)sind als Thema auf Serverebene und datenbankübergreifenden wenige verbleibende Kompatibilitätsprobleme. Datenbanken und Programme, die auf [teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md) benötigen einige Umstrukturierung um diese Inkompatibilität zu beheben, bevor die SQL Server-Datenbank migriert werden kann.

Um zu migrieren, sind die Schritte zu:

- **Test für Kompatibilität**: Datenbankkompatibilität mit [SQL Datenbank V12](sql-database-v12-whats-new.md)validieren. 
- **Update Kompatibilitätsprobleme, sofern vorhanden**: Wenn die Validierung fehlschlägt, müssen die Fehler beheben.  
- **Durchführen der migration** Sobald Ihre Datenbank kompatibel ist, können Sie eine oder mehrere Methoden, für die Migration. 

SQL Server bietet verschiedene Methoden, um diese Aufgaben auszuführen. Dieser Artikel bietet einen Überblick über die verfügbaren Methoden für jeden Vorgang. Das folgende Diagramm veranschaulicht die Schritte und Methoden.

  ![Diagramm zur Datenmigration VSSSDT](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
  
 > [AZURE.NOTE] Migration eine SQL Server-Datenbank, einschließlich Microsoft Access, Sybase, MySQL Oracle und DB2 Azure SQL-Datenbank finden Sie unter [SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/).

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Datenbank-Migrationstools testen SQL Server Datenbank-Kompatibilität mit SQL-Datenbank

SQL Datenbank Kompatibilitätsprobleme Testen vor dem Starten des Migrationsprozesses Datenbank, verwenden Sie eine der folgenden Methoden:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Upgrade Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [SQL Server Data Tools für Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT verwendet die neuesten Kompatibilitätsregeln SQL Datenbank V12 Inkompatibilitäten erkennen. Wenn Inkompatibilitäten erkannt werden, können Sie auf dieses Tool erkannte Probleme beheben. Diese Methode ist die empfohlene Methode zum Testen und Beheben von Kompatibilitätsproblemen SQL Datenbank V12. 
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage ist ein Befehlszeilenprogramm, das tests auf Kompatibilität Probleme und generiert einen Bericht mit den erkannten Kompatibilitätsprobleme. Verwenden Sie dieses Tool, stellen Sie sicher, dass Sie die aktuellste Version verwenden, die aktuelle Kompatibilität verwendet. Wenn Fehler erkannt werden, müssen Sie ein anderes Tool erkannten Kompatibilitätsprobleme - Update verwenden SSDT empfiehlt.  
- [Assistent der Datenebene exportieren in SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): Dieser Assistent erkennt und meldet Fehler auf dem Bildschirm. Nicht Fehler, Sie können fortfahren und die Migration zu SQL-Datenbank. Wenn Fehler erkannt werden, müssen Sie ein anderes Tool erkannten Kompatibilitätsprobleme - Update verwenden SSDT empfiehlt.
- [Die Microsoft SQL Server 2016 Upgrade Advisor Vorschau](http://www.microsoft.com/download/details.aspx?id=48119): Dieses eigenständige Programm, das zurzeit in der Vorschau, erkennt und generiert einen Bericht SQL Datenbank V12 Inkompatibilitäten. Dieses Tool verfügt noch nicht über die neuesten Kompatibilitätsregeln. Wenn keine Fehler gefunden werden, können Sie fortfahren und ausfüllen die Migration zu SQL-Datenbank. Wenn Fehler erkannt werden, müssen Sie ein anderes Tool erkannten Kompatibilitätsprobleme - Update verwenden SSDT empfiehlt. 
- [SQL Azure-Migrationsassistenten ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW ist ein Codeplex-Tool, mit denen die Kompatibilitätsregeln Azure SQL-Datenbank V11 Azure SQL-Datenbank V12 Inkompatibilitäten erkennen. Nötigenfalls Inkompatibilitäten können direkt in diesem Tool Probleme behoben werden. Dieses Tool finden Inkompatibilitäten, die nicht behoben werden. Es war der erste Azure SQL-Datenbank Migration Unterstützung verfügbar und aktiv von der SQL Server-Community unterstützt. Dieses Tool kann auch die Migration innerhalb des Tools abschließen. 

## <a name="fix-database-migration-compatibility-issues"></a>Beheben von Kompatibilitätsproblemen Datenbankmigration

Wenn Kompatibilitätsprobleme erkannt werden, müssen Sie vor der Migration SQL Server-Datenbank beheben. Es gibt eine Vielzahl von Kompatibilitätsproblemen, die beide je auftreten können, auf die Version von SQL Server in der Quelldatenbank und der Komplexität der Datenbank, die Sie migrieren. Ältere Versionen von SQL Server haben weitere Kompatibilitätsprobleme. Verwenden Sie die folgenden Ressourcen eine gezielte Internetsuche mit Ihrer Suchmaschine Auswahlmöglichkeiten:

- [SQL Server-Datenbank in Azure SQL-Datenbank nicht unterstützte features](sql-database-transact-sql-information.md)
- [Nicht mehr unterstützte Datenbankfunktionalität in SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [Nicht mehr unterstützte Datenbankfunktionalität in SQL Server 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [Nicht mehr unterstützte Datenbankfunktionalität in SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [Nicht mehr unterstützte Datenbankfunktionalität in SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [Nicht mehr unterstützte Datenbankfunktionalität in SQLServer 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Suchen im Internet und Nutzung dieser Ressourcen, verwenden Sie [MSDN SQL Server-Foren](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) oder [StackOverflow](http://stackoverflow.com/).

Verwenden Sie eine der folgenden Migrations-Tools, die festgestellten Probleme zu beheben:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [SQL Server Data Tools für Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): Wenn Sie SSDT verwenden Sie Datenbankschema importieren in SQL Server Data Tools für Visual Studio "SSDT") und das Projekt für eine SQL-Datenbank V12-Bereitstellung. Anschließend beheben Sie alle erkannten Kompatibilitätsprobleme in SSDT. Nach Abschluss des Vorgangs synchronisieren die Änderungen an der Quelldatenbank (oder eine Kopie der Datenbank. SSDT ist derzeit die empfohlene Methode zum Testen und Beheben von Kompatibilitätsproblemen SQL Datenbank V12. Den Link für eine [schrittweise SSDT verwenden](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
- Verwenden Sie [SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): Verwendung SSMS Transact-SQL-Befehle zum Beheben der Fehler mit einem anderen Tool ausführen. Diese Methode ist hauptsächlich für fortgeschrittene Benutzer das Datenbankschema direkt in der Quelldatenbank ändern. 
- Verwenden Sie [SQL Azure-Assistenten ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): um SAMW verwenden, generieren Sie ein Transact-SQL-Skript aus der Quelldatenbank. Der Assistent wandelt Skript Möglichkeit, das Schema mit SQL Datenbank V12. Nach Abschluss können SAMW auf SQL-Datenbank V12 ausführen. Dieses Tool analysiert auch Ablaufverfolgungsdateien um Kompatibilitätsprobleme zu ermitteln. Das Skript kann nur Schema generiert oder Daten im BCP-Format enthalten.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Migrieren Sie eine SQL Server-kompatible Datenbank mit SQL-Datenbank

Um eine SQL Server-kompatible Datenbank migrieren, bietet Microsoft mehrere Migrationsmethoden für verschiedene Szenarien. Die gewählte Methode hängt von der Toleranz Ausfallzeiten, Größe und Komplexität Ihrer SQL Server-Datenbank und die Verbindung mit Microsoft Azure-Cloud.  

> [AZURE.SELECTOR]
- [SSMS Migrationsassistenten](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC-Datei exportieren](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [BACPAC importieren](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Wählen Sie die Migrationsmethode ist die erste Frage leisten können Sie die Datenbank aus der Produktion während der Migration. Migrieren einer Datenbank während der aktive Transaktionen auftreten führt zu Inkonsistenzen und Datenbank möglicherweise beschädigt. Es gibt viele Methoden zum Stoppen einer Client-Konnektivität zum Erstellen eines [Datenbanksnapshots](https://msdn.microsoft.com/library/ms175876.aspx)deaktivieren.

Verwenden Sie mit minimaler Ausfallzeit migrieren [SQL Server Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md) die Datenbank die Transaktionsreplikation erfüllt. Ausfallzeiten leisten oder eine Testmigration einer Produktionsdatenbank höher Migration durchführen, sollten Sie einen der folgenden drei Methoden:

- [Assistent für die Migration SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): für kleine und mittlere Datenbanken migrieren kompatible SQL Server 2005 oder höher ist so einfach wie die [Datenbank bereitstellen, Microsoft Azure-Datenbank-Assistenten](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) in SQL Server Management Studio ausgeführt.
- [BACPAC-Datei exportieren](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) und [Importieren BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): Wenn Sie Konnektivität Probleme (keine Konnektivität, niedrige Bandbreite oder Timeoutprobleme) für mittlere bis große Datenbanken verwenden eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) . Mit dieser Methode wird SQL Server Schema und Daten in eine BACPAC exportiert. Dann importieren Sie die Datei BACPAC in SQL-Datenbank mit dem Exportieren-Tier-Anwendung-Assistenten in SQL Server Management Studio oder [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) Befehlszeilen-Dienstprogramm.
- Verwenden Sie BACPAC und BCP zusammen: ein [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) Datei- und [BCP](https://msdn.microsoft.com/library/ms162802.aspx) für größere Datenbanken zu mehr Parallelisierung erhöht Leistung mit komplexeren verwenden. Migrieren Sie mit dieser Methode das Schema und die Daten getrennt.
 - [Exportieren Sie das Schema nur für eine BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
 - [Importieren des Schemas aus der Datei BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) in SQL-Datenbank.
 - Verwenden Sie [BCP](https://msdn.microsoft.com/library/ms162802.aspx) zum Extrahieren der Daten in Flatfiles und [parallele laden](https://technet.microsoft.com/library/dd425070.aspx) dieser Dateien in Azure SQL-Datenbank.

     ![SQL Server-Datenbank-Migration - SQL-Datenbank in die Cloud migrieren.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Nächste Schritte

- [Microsoft SQL Server 2016 Upgrade Advisor-Vorschau](http://www.microsoft.com/download/details.aspx?id=48119)
- [Neueste Version des SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

##<a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank V12](sql-database-v12-whats-new.md)
[Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren Sie SQL Server-Datenbanken mithilfe von SQL Server-Migrationsassistenten](http://blogs.msdn.com/b/ssma/)
