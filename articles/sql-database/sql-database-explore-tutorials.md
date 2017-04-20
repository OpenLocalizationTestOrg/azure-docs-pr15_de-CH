<properties
   pageTitle="Lernprogramme für SQL Azure-Datenbank durchsuchen | Microsoft Azure"
   description="Erfahren Sie mehr über SQL Datenbank- Funktionen"
   keywords=""
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
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>Lernprogramme für SQL Azure-Datenbank durchsuchen

Die unten stehenden Links gelangen Sie zu Übersicht jeder aufgelisteten Funktionsbereich und einfachen Schritt Start-Lernprogramm für jeden Bereich. Lösung begrenzt quick Start, die die Verwendung des SQL-Datenbank in realen Szenarien eine vollständige Lösung finden Sie unter [Azure SQL Datenbank Lösung schnell startet](sql-database-solution-quick-starts.md).

## <a name="using-sql-server-management-studio"></a>Verwenden SQL Server Management Studio

Die folgenden Lernprogramme lernen Sie mit SQL Server Management Studio verwalten und Azure SQL-Datenbank Abfragen.


> [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Auf Serverebene principal Anmeldung Azure SQL-Datenbank herstellen](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| In diesem Lernprogramm lernen Sie Serverebene principal Anmeldung Azure SQL-Datenbank herstellen.|
| [Verbinden Sie mit Azure SQL-Datenbank als Benutzer](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | In diesem Lernprogramm erfahren Sie, wie eine Verbindung mit einer Azure SQL-Datenbank mit einem Benutzerkonto auf Datenbankebene.|
||||

## <a name="elastic-pools"></a>Elastische pools

Die folgenden Lernprogramme lernen Sie Leistungsziele für mehrere Datenbanken verwalten, die häufig verschiedene und unvorhersehbaren Verwendungsmuster mit [elastischen Pools](sql-database-elastic-pool.md) .

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Elastischen Pool erstellen](sql-database-elastic-pool-create-portal.md) | In diesem Lernprogramm erfahren Sie, wie einen skalierbaren Pool von SQL Azure-Datenbanken erstellen. |
| [Überwachen einer elastischen Datenbank](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | In diesem Lernprogramm erfahren Sie, wie eine einzelne elastische Datenbank für potenzielle Probleme zu überwachen. |
| [Hinzufügen einer Benachrichtigung für eine Ressource pool](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | In diesem Lernprogramm erfahren Sie, wie Ressourcen Regeln hinzu, die e-Mail oder Warnmeldungszeichenfolgen URL Endpunkte trifft die Ressource einen Schwellenwert Auslastung, den Sie haben eingerichtet. |
| [Eine Datenbank in einer elastischen Pool verschieben](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | In diesem Lernprogramm erfahren Sie, wie eine Datenbank in einer elastischen Pool verschieben. |
| [Verschieben einer Datenbank aus einer elastischen](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | In diesem Lernprogramm erfahren Sie, wie eine Datenbank aus einer elastischen Pool verschieben. |
| [Ändern der Leistung eines Pools](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | In diesem Lernprogramm erfahren Sie, wie die Leistung und Grenzwerte für einen Pool. |
||||

## <a name="elastic-database-jobs"></a>Elastische Datenbankaufträge

Die folgenden Lernprogramme lernen Sie mit [elastische Datenbank](sql-database-elastic-jobs-overview.md).

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Erste Schritte mit elastische Datenbanktools](sql-database-elastic-scale-get-started.md) | In diesem Lernprogramm lernen Sie die Funktionen elastische Datenbanktools einer einfachen Sharding-Anwendung verwenden. |
| [Erste Schritte mit Azure SQL-Datenbank elastische Aufträge](sql-database-elastic-jobs-getting-started.md)  | In diesem Lernprogramm erfahren Sie wie das Erstellen und verwalten, die Arbeitsplätze Datenbanken verwalten.  | 
||||

## <a name="elastic-queries"></a>Flexible Abfragen

Die folgenden Lernprogramme lernen Sie [elastische Abfragen](sql-database-elastic-query-overview.md)ausführen. 

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Abfragen von horizontal partitionierten Datenbank (Sharding))](sql-database-elastic-query-getting-started.md) | In diesem Lernprogramm erfahren Sie, wie Berichte aus allen Datenbanken horizontal partitionierten (Sharding) einer [elastischen](sql-database-elastic-query-overview.md) Abfrage erstellen |
| [Abfragen einer vertikal partitioniert)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | In diesem Lernprogramm enthält Informationen zum Erstellen von Berichten über alle Datenbanken in einer vertikal Datenbank [elastische Abfrage](sql-database-elastic-query-overview.md) |
| [Migrieren einer vorhandenen Datenbank zu skalieren](sql-database-elastic-convert-to-use-elastic-tools.md)| In diesem Lernprogramm lernen Sie horizontal skalieren (Splitter) eine SQL Azure-Datenbank. |
||||

## <a name="performance-optimization"></a>Performance-Optimierung

Die folgenden Lernprogramme erfahren Sie zum Optimieren der [Leistung der einzelnen Datenbanken](sql-database-performance-guidance.md). Optimieren der Leistung von Datenbanken finden Sie unter [elastische Pools](#elastic-pools).

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Ändern Sie die Dienstebene Tier und Leistung der Datenbank](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | In diesem Lernprogramm lernen Sie vergrößern oder verkleinern die Leistung einer Azure SQL-Datenbank mit Dienstebenen. |
| [SQL Datenbank Advisor Query Performance Insight](sql-database-performance.md#performance-overview) | In diesem Lernprogramm lernen Sie öffnen und SQL Datenbank Advisor Query Performance-Sicherung verwenden.|
| [SQL Datenbank Advisor Leistung empfohlen](sql-database-advisor.md#viewing-recommendations) | In diesem Lernprogramm lernen Sie zu SQL Datenbank Advisor Performance Vorschläge gelten. |
| [Überprüfen Sie obere CPU verbraucht Abfragen](sql-database-query-performance.md#review-top-cpu-consuming-queries)| In diesem Lernprogramm erfahren Sie, wie Sie SQL Datenbank Advisor Query Performance Insight Top CPU verbraucht Abfragen überprüfen.|
| [Abfrage-Details anzeigen](sql-database-query-performance.md#viewing-individual-query-details)| In diesem Lernprogramm erfahren Sie, wie mit SQL Datenbank Advisor Abfrage Leistung Einblick Abfrage Leistungsdetails anzeigen.|
||||

## <a name="sql-database-migration-and-archive"></a>SQL Datenbankmigration und Archivierung 

Die folgenden Lernprogramme erfahren Sie mehr über die [Migration einer vorhandenen SQL Server-Datenbank auf Azure SQL-Datenbank](sql-database-cloud-migrate.md).

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Erkennung von Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | In diesem Lernprogramm erfahren Sie, wie Sie SQL Server Data Tools für Visual Studio Azure SQL-Datenbank-Kompatibilität. |
| [Beheben von Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | In diesem Lernprogramm erfahren Sie, wie SQL Server Datentools für Visual Studio zum Beheben von Kompatibilitätsproblemen Azure SQL-Datenbank verwenden. |
| [Ermitteln Sie die SQL-Datenbank-Kompatibilität mit SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | In diesem Lernprogramm erfahren Sie, wie mit dem Befehlszeilenprogramm SQLPackage.exe Azure SQL-Datenbank-Kompatibilität.|
| [Ermitteln Sie die SQL-Datenbank-Kompatibilität mit SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |In diesem Lernprogramm erfahren Sie, wie Sie SQL Server Management Studio Azure SQL-Datenbank-Kompatibilität.|
| [Migrieren von SQL Server-Datenbank in SQL Datenbank Datenbank Microsoft Azure-Datenbank-Assistenten bereitstellen](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | In diesem Lernprogramm lernen Sie eine SQL Server-kompatible Datenbank Datenbank bereitstellen, Microsoft Azure-Datenbank-Assistenten in SQL Server Management Studio mit Azure SQL-Datenbank migrieren.
| [Exportieren Sie eine SQL Server-Datenbank in eine BACPAC Datei SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | In diesem Lernprogramm erfahren Sie, wie eine BACPAC-Datei mit dem Export-Tier-Anwendung-Assistenten in SQL Server Management Studio eine SQL Server-kompatible Datenbank exportieren.|
| [Exportieren einer SQL Server-Datenbank in eine SqlPackage mit BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | In diesem Lernprogramm erfahren Sie, wie eine SQL Server-kompatible Datenbank in eine mit dem Befehlszeilenprogramm SQLPackage.exe BACPAC exportieren.|
| [Importieren Sie in Azure SQL-Datenbank mit SSMS BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | In diesem Lernprogramm erfahren Sie, wie eine Datenbank in SQL Azure-Datenbank aus einer BACPAC-Datei mit dem Export-Tier-Anwendung-Assistenten in SQL Server Management Studio importiert. |
| [Importieren Sie in Azure SQL-Datenbank mit SqlPackage BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | In diesem Lernprogramm erfahren Sie, wie eine Datenbank in Azure SQL-Datenbank aus einer BACPAC-Datei mit dem Befehlszeilenprogramm SQLPackage importiert. |
| [Importieren einer Datei BACPAC in Azure SQL-Datenbank über das Azure-portal](sql-database-import.md) | In diesem Lernprogramm erfahren Sie, wie ein Azure Blob mit der Azure-Portal auf Azure SQL-Datenbank aus einer BACPAC-Datei eine Datenbank importieren.|
| [Importieren Sie in Azure SQL-Datenbank mit PowerShell BACPAC](sql-database-import-powershell.md) | In diesem Lernprogramm erfahren Sie, wie eine Datenbank in Azure SQL-Datenbank aus einer BACPAC-Datei mit PowerShell importiert.|
| [Archivieren einer Azure SQL-Datenbank mithilfe von Azure-portal](sql-database-export.md#export-your-database) | In diesem Lernprogramm erfahren Sie, wie eine SQL Azure-Datenbank in eine BACPAC-Datei mit der Azure-Portal archivieren. |
| [Archivieren einer Azure SQL-Datenbank mit PowerShell](sql-database-export-powershell.md) | In diesem Lernprogramm erfahren Sie, wie eine SQL Azure-Datenbank eine BACPAC mithilfe von PowerShell archivieren. |
| [Kopieren einer SQL Azure-Datenbank mithilfe des Azure-Portals](sql-database-copy.md#copy-your-sql-database) | In diesem Lernprogramm lernen Sie eine Azure-Portal mit SQL Azure-Datenbank kopieren. |
| [Kopieren einer SQL Azure-Datenbank mithilfe von PowerShell](sql-database-copy-powershell#copy-your-sql-database) | In diesem Lernprogramm erfahren Sie, wie eine SQL Azure-Datenbank mithilfe von PowerShell kopieren. |
| [Kopieren einer SQL Azure-Datenbank mithilfe von Transact-SQL](sql-database-copy-transact-sql.md#copy-your-sql-database) | In diesem Lernprogramm erfahren Sie, wie eine mit Transact-SQL Azure SQL-Datenbank kopieren. |
||||

##<a name="develop"></a>Entwickeln

Die folgenden Lernprogramme erfahren Sie über [SQL-Datenbankentwicklung](sql-database-develop-overview.md) und [Connectivity-Bibliotheken](sql-database-libraries.md)verwenden.

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Verbinden Sie mit SQL-Datenbank mit .NET (C#)](sql-database-develop-dotnet-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer Azure SQL-Datenbank über C#. |
| [Verbinden Sie mit SQL-Datenbank mithilfe von Java](sql-database-develop-java-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer Azure SQL-Datenbank mithilfe von Java. |
| [Verbinden Sie mit SQL-Datenbank mit Node.js](sql-database-develop-nodejs-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer Azure SQL-Datenbank mithilfe von Node.js. |
| [Verbinden Sie mit SQL-Datenbank mit PHP](sql-database-develop-php-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer Azure SQL-Datenbank mit PHP. |
| [Verbinden Sie mit SQL-Datenbank mithilfe von Python](sql-database-develop-python-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer Azure SQL-Datenbank mithilfe von Python. |
| [Verbinden Sie mit SQL-Datenbank mit Ruby](sql-database-develop-ruby-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer Azure SQL-Datenbank über Ruby. |
||||
 
## <a name="database-access"></a>Datenbankzugriff

Die folgenden Lernprogramme erfahren Sie mehr über das [Erstellen und Verwalten von Benutzernamen und Benutzern](sql-database-manage-logins.md).

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Erstellen einer Azure SQL-Datenbank auf Serverebene mithilfe Firewallregel Azure-portal](sql-database-configure-firewall-settings.md)  | In diesem Lernprogramm erfahren Sie, wie eine SQL-Datenbank auf Serverebene Firewall mithilfe von Azure-Portal konfigurieren.  |
| [Mit Transact-SQL eine Datenbankebene Firewallregel erstellen](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | In diesem Lernprogramm erfahren Sie, wie eine mit Transact-SQL-Datenbankebene Firewallregel erstellen.|
| [Mit Transact-SQL-Serverebene Firewall-Regeln verwalten](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | In diesem Lernprogramm erfahren Sie, wie eine mit Transact-SQL Azure SQL-Datenbank auf Serverebene Firewall verwaltet.|
| [Verwalten Sie auf Serverebene Firewallregeln mit PowerShell](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | In diesem Lernprogramm erfahren Sie, wie eine Firewall Azure SQL-Datenbank auf Serverebene mit PowerShell verwaltet.|
| [Mithilfe der REST-API auf Serverebene Firewall-Regeln verwalten](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | In diesem Lernprogramm erfahren Sie, wie eine Azure SQL-Datenbank auf Serverebene Firewall mit der RESET-API verwaltet.|
| [Auf Serverebene principal Anmeldung Azure SQL-Datenbank herstellen](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| In diesem Lernprogramm lernen Sie Serverebene principal Anmeldung Azure SQL-Datenbank herstellen.|
| [Datenbankzugriff einer Anmeldung] (sql-database-manage-logins.md#granting-database-access-to-a-login() | In diesem Lernprogramm erfahren Sie, wie sich auf Serverebene Datenbankzugriff erteilen.|
| [Erstellen Sie Neuer Datenbankbenutzer mit SSMS](sql-database-get-started-security.md#create-new-database-user-using-ssms) | In diesem Lernprogramm erfahren Sie, wie in einer vorhandenen Datenbank mit SSMS Neuer Datenbankbenutzer erstellen.|
| [Neue Datenbank Db_owner Benutzerberechtigungen gewähren](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | In diesem Lernprogramm erfahren Sie, wie eine vorhandene Datenbank Db_owner-Berechtigungen erteilen.|
| [Verbinden Sie mit Azure SQL-Datenbank als Benutzer](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer Azure SQL-Datenbank mit einem Benutzerkonto auf Datenbankebene.|
||||


## <a name="data-security"></a>Sicherheit

Die folgenden Lernprogramme erfahren Sie zum [Sichern von Daten in Azure SQL-Datenbank](sql-database-security.md). 

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Aktivieren der Erkennung Datenbank Azure-portal](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | In diesem Lernprogramm lernen Sie Erkennung im Azure-Portal für die Datenbank einrichten.|
| [Schützen Sie vertrauliche Daten Einzelhandelsabonnements verschlüsselt](sql-database-always-encrypted-azure-key-vault.md) | In diesem Lernprogramm verwenden Sie den Assistenten verschlüsselt Daten in einer Azure SQL-Datenbank sichern.|
| [Daten Sie Senstive über transparente Verschlüsselung](https://msdn.microsoft.com/library/dn948096.aspx)| In diesem Lernprogramm erfahren Sie, wie Sie transparente Verschlüsselung Senstive Daten.|
| [Eine Spalte mit Daten verschlüsseln](https://msdn.microsoft.com/library/ms179331.aspx)| In diesem Lernprogramm erfahren Sie, wie eine Spalte mit Daten mithilfe von Transact-SQL zu verschlüsseln.|
| [Dynamische Daten Masking einrichten](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | In diesem Lernprogramm erfahren Sie, wie dynamische Daten Masking für SQL Azure-Datenbank einrichten. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Business Continuity und Abfrage Dezentrales Skalieren

Die folgenden Lernprogramme erfahren Sie über [geografische Wiederherstellung und aktive Geo-Replikation](sql-database-business-continuity.md) auf Reccover von Fehlern für Business Continuity und Abfrage Dezentrales Skalieren.

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Wiederherstellen einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit der Azure-Portal](sql-database-point-in-time-restore-portal.md)| In diesem Lernprogramm erfahren Sie, wie Ihre Datenbank zu einem früheren Zeitpunkt mit der Azure-Portal wiederherstellen.|
| [Wiederherstellen einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](sql-database-point-in-time-restore-powershell.md) | In diesem Lernprogramm erfahren Sie, wie Ihre Datenbank zu einem früheren Zeitpunkt mit PowerShell wiederherstellen|
| [Wiederherstellen einer gelöschten Azure SQL-Datenbank mit der Azure-Portal](sql-database-restore-deleted-database-portal.md) | In diesem Lernprogramm erfahren Sie, wie eine gelöschte Datenbank mit Azure-Portal wiederherstellen.|
| [Wiederherstellen einer gelöschten Azure SQL-Datenbank mit der PowerShell](sql-database-restore-deleted-database-powershell.md) | In diesem Lernprogramm erfahren Sie, wie eine gelöschte Datenbank mit PowerShell wiederherzustellen.|
| [Konfigurieren Sie Geo-Replikation für Azure SQL Azure-portal](sql-database-geo-replication-portal.md)| In diesem Lernprogramm lernen Sie aktive Geo-Replikation über das Azure-Portal zu konfigurieren.|
| [Konfigurieren Sie Geo-Replikation für Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-powershell.md)| In diesem Lernprogramm lernen Sie aktive Geo-Replikation konfigurieren mithilfe von PowerShell.|
| [Konfigurieren Sie Geo-Replikation mit Transact-SQL Azure SQL-Datenbank](sql-database-geo-replication-transact-sql.md)| In diesem Lernprogramm lernen Sie aktive Geo-Replikation konfigurieren mit Transact-SQL.|
| [Starten einer geplanten oder ungeplanten Failover für Azure SQL Azure-portal](sql-database-geo-replication-failover-portal.md) | In diesem Lernprogramm lernen Sie das Failover auf ein geografisch repliziert sekundären Replikat mit Azure-Portal.|
| [Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-failover-powershell.md) | In diesem Lernprogramm lernen Sie das Failover auf einen sekundären Geo repliziert Replikat mit PowerShell.|
| [Starten einer geplanten oder ungeplanten Failover mit Transact-SQL Azure SQL-Datenbank](sql-database-geo-replication-failover-transact-sql.md) | In diesem Lernprogramm lernen Sie das Failover auf ein geografisch repliziert sekundären Replikat mit Transact-SQL.|
||||

## <a name="data-sync"></a>Daten synchronisieren

In diesem Lernprogramm lernen Sie [Daten synchronisieren](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Lernprogramm  | Beschreibung  |
|---|---|---| 
| [Erste Schritte mit SQL Azure Data Sync (Vorschau)](sql-database-get-started-sql-data-sync.md)  | In diesem Lernprogramm lernen Sie die Grundlagen von Azure SQL Daten synchronisieren mithilfe der Azure-Verwaltungsportal. |
||||

## <a name="next-steps"></a>Nächste Schritte

[Untersuchen von Azure SQL-Datenbank Lösung schnell beginnt](sql-database-solution-quick-starts.md)
