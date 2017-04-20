<properties
    pageTitle="Alle Themen für SQL-Datenbankdienst | Microsoft Azure"
    description="Alle Themen Azure Service Namen SQL-Datenbank, die auf http://azure.microsoft.com/documentation/articles/, Titel und Beschreibung."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Alle Themen für Azure SQL-Datenbank-Dienst

Dieses Thema listet jedes Thema direkt auf den Datenbankdienst **SQL** Azure. Diese Webseite Schlüsselwörter können mit **STRG + F**, die aktuellen Themen finden.




## <a name="new"></a>Neu

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [Daxko-CSI verwendet Azure der Entwicklungszyklus beschleunigen und der Kunden-Service und Leistung](sql-database-implementation-daxko.md) | Lernen Sie, wie Daxko-CSI SQL-Datenbank verwendet der Entwicklungszyklus beschleunigen und der Kunden-Service und Leistung |
| 2 | [Azure bietet weltweit GEP und Effizienz](sql-database-implementation-gep.md) | Erfahren Sie mehr über GEP SQL Datenbank Verwendung mehr Kunden und mehr Effizienz |
| 3 | [Mit Azure erweitert SnelStart schnell die Business Services mit einer Geschwindigkeit von 1.000 neue Azure SQL-Datenbanken pro Monat](sql-database-implementation-snelstart.md) | Informationen Sie darüber, wie SnelStart SQL-Datenbank schnell erweitert seine Business Services mit einer Geschwindigkeit von 1.000 neue Azure SQL-Datenbanken pro Monat |
| 4 | [Umbraco verwendet Azure SQL-Datenbank zu schnell bereitstellen und Skalierung für Tausende von Mandanten in der cloud](sql-database-implementation-umbraco.md) | Erfahren Sie mehr über wie Umbraco SQL-Datenbank verwendet, um schnell bereitzustellen und Skalierung Services für Tausende von Mandanten in der cloud |
| 5 | [Azure SQL-Datenbank mit PowerShell verwalten](sql-database-manage-powershell.md) | Azure SQL-Datenbank-Management mit PowerShell. |
| 6 | [SSMS Unterstützung für Azure AD MFA SQL-Datenbank mit SQL Data Warehouse](sql-database-ssms-mfa-authentication.md) | Verwenden Sie mehrere berücksichtigt Authentifizierung mit SSMS für SQL-Datenbank und SQL Datawarehouse. |
| 7 | [Erläutert Datenbank Transaktion (DTUs) und elastische Datenbank Transaktion (eDTUs)](sql-database-what-is-a-dtu.md) | Welche einer Azure SQL-Datenbank versteht Buchungseinheit. |


## <a name="updated-articles-sql-database"></a>Aktualisierte Artikel, SQL-Datenbank

Dieser Abschnitt enthält Artikel, die kürzlich aktualisiert wurden, wurde das Update Groß oder erheblich. Für jede aktualisierte Artikel grober Ausschnitt hinzugefügten Abzug Text angezeigt. Die Artikel wurden innerhalb des Datumsbereichs **2016-08-22** **2016 10**05 aktualisiert.

| &nbsp; | Artikel | Aktualisierter Text, Ausschnitt | Aktualisiert, wenn |
| --: | :-- | :-- | :-- |
| 8 | [Azure SQL-Datenbank mit PowerShell verwalten](sql-database-command-line-tools.md) | Erstellen Sie eine Ressourcengruppe für unsere SQL-Datenbank und zugehörige Azure Ressourcen mit dem Cmdlet New-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx). *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "Northcentralus" neu AzureRmResourceGroup-Name $resourceGroupName-Speicherort $resourceGroupLocation* Weitere Informationen finden Sie unter Verwendung von Azure PowerShell Azure Ressourcenmanager (... / Powershell Azure-Ressource manager.md). Ein Beispielskript finden Sie unter Erstellen einer SQL-Datenbank PowerShell-Skript (Sql-Datenbank-Get-gestartet-powershell.md Create-a-sql-database-powershell-script). **Erstellen einer SQL-Datenbankserver** Erstellen Sie einen SQL-Datenbankserver mit dem Cmdlet New-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx). Ersetzen Sie *server1* mit dem Namen des Servers ein. Servernamen müssen auf allen Servern Azure SQL-Datenbank eindeutig sein. Wenn der Servername bereits vergeben ist, erhalten Sie Fehler. Dieser Befehl kann einige Minuten dauern. Die resou | 2016-09-14 |
| 9 | [Kopieren einer SQL Azure-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md) |   SQL Datenbankquelle (die vorhandene Datenbank kopieren) $sourceDbName = "db1" $sourceDbServerName = "server1" $sourceDbResourceGroupName = "rg1" SQL Datenbank kopieren (die neue Datenbank erstellt werden)--$copyDbName = "db1_copy" $copyDbServerName = "server2" $copyDbResourceGroupName = "rg2" Kopie eine Datenbank auf demselben Server - neu-AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - ServerName $sourceDbServerName DatabaseName - $sourceDbName - CopyDatabaseName $copyDbName kopiert eine Datenbank auf einem anderen Server neu AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - ServerName $sourceDbServerName DatabaseName - $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName kopieren eine Datenbank in einem Datenbankpool elastische - $poolName = "pool1" neu AzureRmSqlDatabaseCopy - ResourceGroupName $ SourceDbResourceGroupName - ServerName $sourceDbServerName DatabaseName - $sourceDbName - CopyReso | 2016-09-08 |
| 10 | [Erstellen Sie einen Datenbankpool elastische mit C#](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine ("Serverfirewall...");  FirewallRuleGetResponse Fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine ("Server-Firewall:" + Fwr. FirewallRule.Id);  Console.WriteLine ("elastische Pool...");  ElasticPoolCreateOrUpdateResponse Epr = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine ("elastischen Pool:" + Epr. ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse Dbr = CreateOrUpdateDatabase (_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine ("Datenbank:" + Dbr. Database.Id);  Console.WriteLine ("drücken Sie eine beliebige Taste, um fortzufahren...");  Console.ReadKey();  } statische ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa | 2016-09-14 |
| 11 | [PowerShell-Skript zum Identifizieren einer elastischen Datenbankpool für Datenbanken](sql-database-elastic-pool-database-assessment-powershell.md) | **Für elastische Datenbankpool Kandidaten ausgeschlossen Master- und Datenbanken, die bereits im Pool. Sie können weitere Datenbanken ausgeschlossenen Liste nach Bedarf hinzufügen** $ListOfDBs = Get-AzureRmSqlDatabase - ServerName $servername. Split('.') 0 - ResourceGroupName $ResourceGroupName / Where-Object {$_. DatabaseName - Notin ("master") - und $_. CurrentServiceLevelObjectiveName - Notin ("ElasticPool")- und $_. CurrentServiceObjectiveName - Notin ("ElasticPool")} $outputConnectionString = "Data Source = $outputServerName; Integrated Security = false; ersten Katalog = $outputdatabaseName; Benutzer-Id = $outputDBUsername; Kennwort = $outputDBpassword "$destinationTableName ="Resource_stats_output" **Erstellen Sie eine Tabelle im Ausgabedatenbank Metriken Auflistung** $sql =" Wenn NOT EXISTS (Wählen Sie * aus sys.objects wo Object_id = OBJECT_ID(N'$($destinationTableName)') und geben (N'U')) BEGIN Create Table $($destinationTableName) (Datenbankname varchar(128) Slo varchar(20) End_time Datetime, Avg_cpu Float, Avg_ | 2016-09-29 |
| 12 | [Lernprogramm für SQL: mithilfe eine SQL-Datenbank in Minuten Azure-Portal](sql-database-get-started.md) |   ! Neue Sql-Datenbank 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Klicken Sie auf **SQL-Datenbank** -Blade SQL Datenbank öffnen. Der Inhalt auf diese hängt von der Anzahl von Abonnements und die vorhandenen Objekte (z. B. vorhandene Server).  ! Neue Sql-Datenbank 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. Geben Sie einen Namen für die erste Datenbank - wie "Meine Datenbank" im Feld **Datenbankname** . Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen angegeben haben.  ! Neue Sql-Datenbank 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Wenn Sie mehrere Abonnements verfügen, wählen Sie ein Abonnement. 6. **Ressourcengruppe**klicken Sie auf **neu erstellen** , und geben Sie einen Namen für die erste Ressourcengruppe - wie "Meine Ressourcen-Gruppe". Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen angegeben haben.  ! Neue Sql-Datenbank 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. Unter **Quelle auswählen* | 2016-09-08 |
| 13 | [Versuchen Sie SQL-Datenbank: Mit C# eine SQL-Datenbank mit der SQL-Datenbank-Bibliothek für .NET erstellen](sql-database-get-started-csharp.md) | **Erstellen Sie einen Service principal zugreifen** PowerShell-Skript erstellt die Anwendung Active Directory (AD) und Dienstprinzipalnamen, die wir unsere C Anwendung authentifizieren müssen. Das Skript gibt die Werte für im vorangehenden Beispiel C. Ausführliche Informationen finden Sie unter Verwenden Azure PowerShell Dienstprinzipalnamen Zugriff auf die Ressourcen erstellen (... / Resource-Gruppe-Authentifizierung-Service-principal.md).  Melden Sie sich bei Azure.  Fügen Sie AzureRmAccount Wenn Sie mehrere Abonnements, Kommentieren und Abonnement zu arbeiten.  $subscriptionId = "{Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx}" Set-AzureRmContext - SubscriptionId $subscriptionId bieten diese Werte für Ihre neue AAD.  $appName ist der Anzeigename für Ihre Anwendung, muss in Ihrem Verzeichnis.  $uri muss nicht real Uri sein.  $secret ist ein Kennwort, das Sie erstellen.  $appName = "{app-Name}" $uri = "http://{app-name}" $secret = "{app-Kennwort}" Erstellen einer app AAD $azureAdApplication = New AzureRmADApp | 2016-09-23 |
| 14 | [Verwalten von Azure SQL-Datenbanken mithilfe von Azure-portal](sql-database-manage-portal.md) | Anzeigen oder aktualisieren Sie Ihre datenbankeinstellungen, klicken Sie auf die gewünschte Einstellung auf die SQL-Datenbank:! SQL Datenbank Settings (. / media/sql-database-manage-portal/settings.png) **wie finden Sie SQL-Datenbanken vollqualifizierte Servernamen?** Zum Anzeigen der Servername Datenbanken **Übersicht** auf die **SQL-Datenbank** auf, und beachten Sie den Servernamen:! SQL Datenbank Settings (. / media/sql-database-manage-portal/server-name.png) **wie verwalten Sie Firewall-Regeln zur Steuerung des Zugriffs auf meine SQL Server und Datenbank?** Zum Anzeigen, erstellen oder Aktualisieren von Firewall-Regeln, klicken Sie auf **Server-Firewall** auf die **SQL-Datenbank** . Weitere Informationen finden Sie unter Konfigurieren einer Azure SQL-Datenbank auf Serverebene mithilfe Firewallregel Azure-Portal (Sql-Datenbank-Konfiguration-Firewall-settings.md). ! Firewall-Regeln (. / media/sql-database-manage-portal/sql-database-firewall.png) **Wie ändere ich meine SQL-Datenbank Tier oder Leistung Servicelevel?** Aktualisieren Sie die Dienstebene Tier oder Leistung einer SQL-Datenbank klicken Sie auf ** Preisstufe (s | 2016-09-20 |





## <a name="get-started"></a>Erste Schritte

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 15 | [Multi-Tenant-Apps mit SQL Azure-Datenbank mit Isolierung und Effizienz erstellt](sql-database-build-multi-tenant-apps.md) | Erfahren Sie, wie SQL-Datenbank Multi-Tenant-apps erstellt |
| 16 | [Verbinden mit SQL-Datenbank mit SQL Server Management Studio und Beispiel T-SQL-Abfragen](sql-database-connect-query-ssms.md) | Informationen Sie zum Herstellen einer Datenbank SQL Azure mit SQL Server Management Studio (SSMS). Führen Sie dann eine Beispielabfrage mithilfe von Transact-SQL (T-SQL). |
| 17 | [Verbinden Sie mit SQL-Datenbank mit .NET (C#)](sql-database-develop-dotnet-simple.md) | Verwenden Sie der Beispielcode in diesem schnell eine moderne Anwendung mit C# erstellen und eine leistungsfähige relationale Datenbank in der Cloud mit Azure SQL-Datenbank gesichert. |
| 18 | [Erstellen Sie einen neuen Datenbankpool elastische mit Azure-portal](sql-database-elastic-pool-create-portal.md) | Wie die SQL-Datenbankkonfiguration für einfachere Verwaltung und Ressourcenfreigabe über viele Datenbanken skalierbare elastische Datenbankpool hinzugefügt. |
| 19 | [Lernprogramme für SQL Azure-Datenbank durchsuchen](sql-database-explore-tutorials.md) | Erfahren Sie mehr über SQL Datenbank- Funktionen |
| 20 | [Lernprogramm für SQL: mithilfe eine SQL-Datenbank in Minuten Azure-Portal](sql-database-get-started.md) | Informationen Sie zum Einrichten einer logischen SQL-Datenbankserver, Server Firewallregel SQL-Datenbank und Beispieldaten. Außerdem Informationen Sie zum Verbinden mit Clienttools, Konfigurieren von Benutzern und eine Firewallregel Datenbank einrichten. |
| 21 | [SQL Azure-Datenbank sichert und schützt](sql-database-helps-secures-and-protects.md) | Erfahren Sie, wie SQL-Datenbank sicher und geschützt |
| 22 | [SQL Azure-Datenbank lernt &amp; passt](sql-database-learn-and-adapt.md) | Erfahren Sie, wie SQL-Datenbank lernt und passt |
| 23 | [Wählen Sie eine Wolke SQL Server Option: Azure SQL (PaaS) Datenbank oder SQL Server auf Azure VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Erfahren Sie, welche Cloud SQL Server Option die Anwendung passt: Azure SQL (PaaS) Datenbank oder SQL Server in der Cloud auf Azure Virtual Machines. |
| 24 | [Azure SQL-Datenbank skaliert dynamisch](sql-database-scale-on-the-fly.md) | Erfahren Sie, wie SQL-Datenbank dynamisch skaliert |
| 25 | [Untersuchen von Azure SQL-Datenbank Lösung schnell beginnt](sql-database-solution-quick-starts.md) | Erfahren Sie mehr über SQL Azure Database Solutions |
| 26 | [SQL Azure-Datenbank funktioniert in Ihrer Umgebung](sql-database-works-in-your-environment.md) | Erfahren Sie, wie SQL-Datenbank hilft, sichert und schützt |



## <a name="build-an-app"></a>Erstellen Sie eine Anwendung

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 27 | [Problembehandlung, diagnose und verhindern von SQL-Verbindung und vorübergehende Fehler für SQL-Datenbank](sql-database-connectivity-issues.md) | Informationen Sie zum beheben, diagnostizieren und zu verhindern, dass ein Verbindungsfehler SQL oder vorübergehender Fehler in Azure SQL-Datenbank.  |
| 28 | [Verbinden Sie mit einer SQL-Datenbank mit Visual Studio](sql-database-connect-query.md) | Schreiben Sie ein Programm in C# Abfragen und SQL-Datenbank. Informationen zu IP-Adressen, Verbindungszeichenfolgen sichere Anmeldung und kostenlose Visual Studio. |
| 29 | [Ports über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md) | Clientverbindungen von ADO.NET zu Azure SQL-Datenbank V12 manchmal umgehen den Proxy und direkt mit der Datenbank interagieren. Anschlüsse als 1433 Bedeutung. |
| 30 | [Fehlercodes für Clientanwendungen SQL Datenbank SQL: Datenbank-Verbindungsfehler und andere Probleme](sql-database-develop-error-messages.md) | Enthält Informationen Sie zum SQL-Fehlercodes für Clientanwendungen SQL Datenbank Verbindung Fehlermeldungen kopieren Datenbankproblemen und allgemeine Fehler.  |
| 31 | [Verbinden Sie mit SQL-Datenbank mit PHP auf Windows](sql-database-develop-php-simple.md) | Zeigt ein Beispiel PHP, die Verbindung Azure SQL-Datenbank von einem Windows-Client und Links, die vom Client benötigten Softwarekomponenten. |
| 32 | [Versuchen Sie SQL-Datenbank: Mit C# eine SQL-Datenbank mit der SQL-Datenbank-Bibliothek für .NET erstellen](sql-database-get-started-csharp.md) | Testen Sie SQL-Datenbank für SQL und C# apps entwickeln und Erstellen einer Azure SQL-Datenbank mit C# mit SQL-Datenbank-Bibliothek für .NET. |
| 33 | [Erste Schritte mit JSON in Azure SQL-Datenbank](sql-database-json-features.md) | Azure SQL-Datenbank können Sie analysieren, Abfrage und Formatieren von Daten in JavaScript Object Notation (JSON)-Notation. |
| 34 | [Verbindungsprobleme, Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md) | Schritte zum Erkennen und Beheben von allgemeinen Verbindungsfehler für Azure SQL-Datenbank. |
| 35 | [Fehler "Datenbank auf dem Server ist derzeit nicht verfügbar" beim Verbinden mit Sql-Datenbank](sql-database-troubleshoot-connection.md) | Problembehandlung bei der Datenbank auf Server ist nicht verfügbar Fehler, wenn eine Anwendung mit SQL-Datenbank verbindet. |



## <a name="develop"></a>Entwickeln

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 36 | [Erhalten Sie die erforderlichen Werte für die Authentifizierung einer Anwendung Code SQL-Datenbank zugreifen](sql-database-client-id-keys.md) | Erstellen Sie einen Dienstprinzipalnamen für Code SQL-Datenbank zugreifen. |
| 37 | [Verbinden Sie mit SQL-Datenbank mit Java JDBC unter Windows](sql-database-develop-java-simple.md) | Zeigt ein Java-Codebeispiel, mit Azure SQL-Datenbank herstellen. Das Beispiel verwendet JDBC und läuft auf einem Windows-Client-Computer. |
| 38 | [Verbinden Sie mit SQL-Datenbank mit Node.js](sql-database-develop-nodejs-simple.md) | Stellt ein Codebeispiel Node.js, mit Azure SQL-Datenbank herstellen. |
| 39 | [SQL Datenbank-Anwendungsentwicklung, Überblick](sql-database-develop-overview.md) | Informationen Sie zu verfügbaren Connectivity-Bibliotheken und best Practices für die Anwendung mit SQL-Datenbank. |
| 40 | [Verbinden Sie mit SQL-Datenbank mithilfe von Python](sql-database-develop-python-simple.md) | Stellt ein Codebeispiel Python, mit Azure SQL-Datenbank herstellen. |
| 41 | [Verbinden Sie mit SQL-Datenbank mit Ruby](sql-database-develop-ruby-simple.md) | Geben Sie ein Ruby Codebeispiel können Sie zum Azure SQL-Datenbank herstellen. |
| 42 | [Erste Schritte mit temporären Tabellen in SQL Azure-Datenbank](sql-database-temporal-tables.md) | Informationen Sie zum Einstieg temporäre Tabellen in Azure SQL-Datenbank. |



## <a name="performance-and-scale"></a>Performance und Skalierung

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 43 | [SQL Datenbank Advisor](sql-database-advisor.md) | Azure SQL-Datenbank Advisor Leitfaden für Ihre SQL-Datenbanken, die aktuelle abfrageleistung verbessern können. |
| 44 | [SQL Datenbank-Berater mit der Azure-portal](sql-database-advisor-portal.md) | Können Sie Azure SQL-Datenbank Advisor in Azure-Portal überprüfen und Vorschläge für die SQL-Datenbanken, die aktuelle abfrageleistung verbessern können. |
| 45 | [Azure SQL-Datenbank Benchmark-Übersicht](sql-database-benchmark-overview.md) | Azure SQL-Datenbank Maßstab zur Messung der Leistung von Azure SQL-Datenbank beschrieben. |
| 46 | [Wenn werden ein Datenbankpool elastische soll verwendet?](sql-database-elastic-pool-guidance.md) | Ein Datenbankpool elastische ist eine Auflistung der verfügbaren Ressourcen, die von einer elastischen Datenbanken gemeinsam genutzt werden. Dieses Dokument enthält dabei helfen, die Eignung einen elastischen Datenbankpool für eine Gruppe von Datenbanken verwenden. |
| 47 | [Erste Schritte mit elastische Datenbanktools](sql-database-elastic-scale-get-started.md) | Grundlegende Erklärung elastische Datenbank Tools Feature von Azure SQL-Datenbank einschließlich einfache Beispiel-app ausgeführt. |
| 48 | [SQL Datenbank häufig gestellte Fragen](sql-database-faq.md) | Antworten auf häufige Fragen von Kunden Fragen Clouddatenbanken und Azure SQL-Datenbank, Microsoft Datenbank-Managementsystem (RDBMS) und Datenbank als Dienst in der Cloud. |
| 49 | [Erste Schritte mit im Speicher (Vorschau) SQL-Datenbank](sql-database-in-memory.md) | SQL In-Memory-Technologie verbessern die Leistung von Transaktions- und Analytics Arbeitslasten. Erfahren Sie, wie diese Technologie nutzen. |
| 50 | [Verwendung im Arbeitsspeicher OLTP (Vorschau) Ihre Anwendung Leistung in SQL-Datenbank](sql-database-in-memory-oltp-migration.md) | Übernehmen im Arbeitsspeicher OLTP transaktionale Leistung in einer vorhandenen SQL-Datenbank. |
| 51 | [Monitor im Arbeitsspeicher OLTP-Speicher](sql-database-in-memory-oltp-monitoring.md) | Schätzung und Monitor XTP in-Speicher verwenden, Kapazität; Beheben Sie Fehler Kapazität 41823 |
| 52 | [Betrieb der Abfrage in Azure SQL-Datenbank speichern](sql-database-operate-query-store.md) | Enthält Informationen zum Bedienen der Abfrage in Azure SQL-Datenbank speichern |
| 53 | [SQL-Datenbank-Performance-Sicherung](sql-database-performance.md) | Azure SQL-Datenbank bietet Performance Tools können Sie die Bereiche identifizieren, die aktuellen steigern können. |
| 54 | [Azure SQL-Datenbank und Leistung für einzelne Datenbanken](sql-database-performance-guidance.md) | In diesem Artikel helfen Ihnen festzustellen welche Dienstebene für Ihre Anwendung auswählen. Außerdem wird empfohlen, Methoden die Anwendung von Azure SQL-Datenbank zu optimieren. |
| 55 | [SQL Azure-Datenbank Query Performance Insight](sql-database-query-performance.md) | Abfrage Performance monitoring bezeichnet die meisten CPU-Nutzung Abfragen einer Azure SQL-Datenbank. |
| 56 | [Ändern der Service Tier und Leistung (Tarif) eine SQL-Datenbank](sql-database-scale-up.md) | Ändern der Dienstebene und Leistung einer Azure SQL-Datenbank zeigt die SQL-Datenbank nach oben oder unten skalieren. Ändern den Tarif einer Azure SQL-Datenbank. |
| 57 | [Überwachung der Datenbank-Performance in Azure SQL-Datenbank](sql-database-single-database-monitor.md) | Erfahren Sie mehr über die Optionen für die Überwachungsdatenbank Azure Tools und dynamische Verwaltungsansichten. |
| 58 | [SQL Datenbank Performance tuning-Tipps](sql-database-troubleshoot-performance.md) | Tipps zur Performance-Optimierung in Azure SQL-Datenbank durch Auswertung und Verbesserung. |
| 59 | [Wie Batchverarbeitung SQL Datenbank Anwendung Leistung verwenden](sql-database-use-batching-to-improve-performance.md) | Das Thema belegt, Batchverarbeitung Datenbankoperationen erheblich Imroves Geschwindigkeit und Erweiterbarkeit Azure SQL-Datenbank Anwendung. Obwohl diese Batchverarbeitung Verfahren für jede SQL Server-Datenbank arbeiten, liegt der Schwerpunkt des Artikels Azure. |



## <a name="tools-for-scaling-out"></a>Tools für das dezentrale Skalieren

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 60 | [Entwerfen von Mustern mandantenfähigen SaaS-Applikationen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md) | Dieser Artikel beschreibt die Vorschriften und allgemeinen Architektur Datenmuster mandantenfähigen Datenbank benötigten Programme in einer Cloud-Umgebung zu berücksichtigen und verschiedene Nachteile dieser Muster zugeordnet. Darüber hinaus wie Azure SQL-Datenbank mit elastischen Pools elastische Tools helfen diese Anforderungen Weise ohne Kompromisse. |
| 61 | [Migrieren Sie bestehende Datenbanken nach Dezentrales Skalieren](sql-database-elastic-convert-to-use-elastic-tools.md) | Konvertieren von Sharding Datenbanken um elastische Datenbanktools erstellen einen Schema-Manager verwenden |
| 62 | [Skalierbare Cloud-Datenbanken erstellen](sql-database-elastic-database-client-library.md) | Erstellen Sie skalierbare .NET Datenbank apps Clientbibliothek elastische Datenbank |
| 63 | [Leistungsindikatoren für shardzuordnungs-manager](sql-database-elastic-database-perf-counters.md) | ShardMapManager-Klasse und Daten von Leistungsindikatoren |
| 64 | [Einen Splitter mit elastische Database Tools hinzufügen](sql-database-elastic-scale-add-a-shard.md) | Festlegen, wie elastische Skalierung APIs verwenden, um einen neuen Splitter hinzuzufügen. |
| 65 | [Split-Merge-Dienst bereitstellen](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Trennen und Zusammenführen mit elastische Datenbanktools |
| 66 | [Datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md) | Verwendung die ShardMapManager-Klasse in .NET apps für datenabhängiges routing eine elastische Datenbanken für Azure SQL-Datenbank |
| 67 | [Elastische Datenbanktools FAQ](sql-database-elastic-scale-faq.md) | Häufig gestellte Fragen zur Azure SQL-Datenbank elastische skalieren. |
| 68 | [Elastische Datenbank Tools Glossar](sql-database-elastic-scale-glossary.md) | Erläuterung der Begriffe für elastische Datenbanktools |
| 69 | [Dezentrales Skalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md) | Software als Service (SaaS)-Entwickler kann leicht flexible, skalierbare Datenbanken in der Cloud mit diesen Tools erstellen. |
| 70 | [Die Clientbibliothek elastische Datenbank Zugriff auf Anmeldeinformationen](sql-database-elastic-scale-manage-credentials.md) | Wie Sie die richtige Anmeldeinformationen Admin Apps elastische Datenbank schreibgeschützt festlegen |
| 71 | [Abfragen mit mehreren Splitter](sql-database-elastic-scale-multishard-querying.md) | Splitter mit der Clientbibliothek elastische Datenbank verlaufen Sie Abfragen. |
| 72 | [Verschieben von Daten zwischen Clouddatenbanken skalierten](sql-database-elastic-scale-overview-split-and-merge.md) | Erläutert die Splitter verändern und Verschieben von Daten über ein lokal gehosteter Dienst elastische Datenbank-APIs verwenden. |
| 73 | [Skalieren Sie mit der shardzuordnungs-manager](sql-database-elastic-scale-shard-map-management.md) | Verwendung der ShardMapManager elastische Datenbank-Clientbibliothek |
| 74 | [Split-Merge-Sicherheitskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md) | Einrichten von X409 Zertifikate für Verschlüsselung |
| 75 | [Aktualisieren einer Anwendung die neueste elastische Datenbank-Clientbibliothek verwenden](sql-database-elastic-scale-upgrade-client-library.md) | Aktualisieren Sie apps und Bibliotheken mit Nuget |
| 76 | [Elastische Datenbank-Clientbibliothek mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Verwenden Sie elastische Datenbank-Clientbibliothek und Entity Framework zum Codieren von Datenbanken |
| 77 | [Verwenden von Clientbibliothek elastische Datenbanken mit Dapper](sql-database-elastic-scale-working-with-dapper.md) | Elastische Datenbank-Clientbibliothek verwenden mit Dapper. |
| 78 | [Multi-Tenant-Applikationen elastische Datenbanktools mit Sicherheit auf Zeilenebene](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Informationen Sie zur Verwendung von elastische Datenbanktools mit Sicherheit auf Zeilenebene zum Erstellen einer Anwendung mit einer hochgradig skalierbaren Datenebene auf Azure SQL-Datenbank, die Multi-Tenant-Splitter unterstützt. |



## <a name="elastic-jobs"></a>Elastische Aufträge

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 79 | [Erstellen und Verwalten von skalierten Azure SQL-Datenbanken mit elastische (Vorschau)](sql-database-elastic-jobs-create-and-manage.md) | Erstellung und Verwaltung eines Auftrags elastische Datenbank durchgehen. |
| 80 | [Erste Schritte mit elastische Datenbank Aufträge](sql-database-elastic-jobs-getting-started.md) | wie elastische Datenbankaufträge |
| 81 | [Skalierte Clouddatenbanken verwalten](sql-database-elastic-jobs-overview.md) | Zeigt den elastische Datenbankdienst |
| 82 | [Erstellen Sie und verwalten Sie einer SQL-Datenbank elastische Datenbankaufträge mit PowerShell (Vorschau)](sql-database-elastic-jobs-powershell.md) | PowerShell zum Verwalten von Azure SQL-Datenbank-pools |
| 83 | [Installieren von elastische Datenbank Projekte – Überblick](sql-database-elastic-jobs-service-installation.md) | Installation der elastische Funktion durchlaufen. |
| 84 | [Elastische Aufträge Datenbankkomponenten deinstallieren](sql-database-elastic-jobs-uninstall.md) | Elastische Datenbank Aufträge Tool deinstallieren |



## <a name="elastic-pools"></a>Elastische pools

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 85 | [Was ist ein Azure elastischen Pool?](sql-database-elastic-pool.md) | Verwalten von Hunderten oder Tausenden von Datenbanken mit einem Pool. Der Pool kann ein Preis für einen Satz von Leistungseinheiten verteilt. Verschieben Sie Datenbanken oder werden. |
| 86 | [Erstellen Sie einen Datenbankpool elastische mit C#](sql-database-elastic-pool-create-csharp.md) | Verwenden Sie C# Datenbank Entwicklungstechniken Ressourcen über viele Datenbanken nutzen können skalierbare elastische Datenbankpool in Azure SQL-Datenbank erstellen. |
| 87 | [Erstellen eines neuen Pools elastische Datenbank mit PowerShell](sql-database-elastic-pool-create-powershell.md) | Erfahren Sie mit PowerShell Azure SQL-Datenbank skalieren Ressourcen durch Erstellen eines Pools skalierbare elastische Datenbank zur Verwaltung von Datenbanken. |
| 88 | [PowerShell-Skript zum Identifizieren einer elastischen Datenbankpool für Datenbanken](sql-database-elastic-pool-database-assessment-powershell.md) | Ein Datenbankpool elastische ist eine Auflistung der verfügbaren Ressourcen, die von einer elastischen Datenbanken gemeinsam genutzt werden. Dieses Dokument enthält ein Powershell-Skript, um die Eignung einer elastischen Datenbankpool für eine Gruppe von Datenbanken mit Hilfe. |
| 89 | [Überwachen Sie und verwalten Sie einer elastischen Datenbank mit C#](sql-database-elastic-pool-manage-csharp.md) | Verwenden Sie C#-Datenbank Entwicklungstechniken zum Verwalten eines elastischen Datenbankpool Azure SQL-Datenbank. |
| 90 | [Überwachen Sie und verwalten Sie einer elastischen Datenbank mit Azure-portal](sql-database-elastic-pool-manage-portal.md) | Erfahren Sie, wie Azure-Portal und integrierte Intelligenz SQL-Datenbank zu verwalten und passenden einen Pool skalierbare elastische Datenbank Datenbank optimieren und Verwalten der Kosten. |
| 91 | [Überwachen Sie und verwalten Sie einer elastischen Datenbank mit PowerShell](sql-database-elastic-pool-manage-powershell.md) | Erfahren Sie mit PowerShell elastischen Datenbankpool verwalten. |
| 92 | [Überwachen Sie und verwalten Sie einer elastischen Datenbank mit Transact-SQL](sql-database-elastic-pool-manage-tsql.md) | Mit T-SQL Azure SQL-Datenbank in einem elastischen Pool erstellen. Oder T-SQL und Pools die Datenbank verschoben. |
| 93 | [Elastische Datenbank Pool Abrechnung und Preisinformationen](sql-database-elastic-pool-price.md) | Preisinformationen für elastische datenbankpools. |



## <a name="elastic-query"></a>Elastische Abfrage

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 94 | [Bericht über skalierte Clouddatenbanken (Vorschau)](sql-database-elastic-query-getting-started.md) | wie Sie cross-Datenbankabfragen |
| 95 | [Erste Schritte mit datenbankübergreifenden Abfragen (vertikale Partitionierung) (Vorschau)](sql-database-elastic-query-getting-started-vertical.md) | Verwendung von elastische Abfrage mit vertikal partitionierten Datenbanken |
| 96 | [Berichte über skalierte Clouddatenbanken (Vorschau)](sql-database-elastic-query-horizontal-partitioning.md) | elastische Abfragen über horizontale Partitionen einrichten |
| 97 | [Azure SQL-Datenbank elastische Datenbank Abfrage Übersicht (Vorschau)](sql-database-elastic-query-overview.md) | Übersicht über die elastische Abfragefunktion |
| 98 | [Clouddatenbanken mit unterschiedlichen Schemas (Vorschau) Abfragen](sql-database-elastic-query-vertical-partitioning.md) | datenbankübergreifende Abfragen über vertikale Partitionen einrichten |



## <a name="elastic-transaction"></a>Elastische Transaktion

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 99 | [Verteilte Transaktionen über Cloud-Datenbanken](sql-database-elastic-transactions-overview.md) | Übersicht über elastische Datenbanktransaktionen mit SQL Azure-Datenbank |



## <a name="backup-and-recovery"></a>Backup und recovery

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 100 | [SQL Datenbank-backups](sql-database-automated-backups.md) | Erfahren Sie mehr über integrierte SQL-Datenbank Datenbank-Backups, die Rollen ermöglichen eine SQL Azure-Datenbank zu einem früheren Zeitpunkt Zeit wieder oder Kopieren einer Datenbank in eine neue Datenbank in einer geografischen Region (bis zu 35 Tage). |
| 101 | [Übersicht über Business Continuity mit Azure SQL-Datenbank](sql-database-business-continuity.md) | Erfahren Sie, wie Azure SQL-Datenbank Cloud Business Continuity und Datenbank-Recovery unterstützt und hilft, unternehmenskritische Cloudanwendungen ausgeführt. |
| 102 | [Eine einzelne Tabelle aus einer Azure SQL-Datenbank-Sicherung wiederherstellen](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Erfahren Sie, wie eine einzelne Tabelle von Azure SQL-Datenbank-Sicherung wiederherstellen. |
| 103 | [Entwerfen Sie für Cloud Disaster Recovery über aktive Geo-Replikation in SQL-Datenbank](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Erfahren Sie, wie Cloud Disaster Recovery-Lösung für Business Continuity-Planung mit Geo-Replikation für app-Daten-Backup mit Azure SQL-Datenbank. |
| 104 | [Wiederherstellen einer Azure SQL-Datenbank oder ein Failover auf die sekundäre](sql-database-disaster-recovery.md) | Informationen Sie zum Wiederherstellen einer Datenbank aus einer regionalen Datacenter Ausfall oder Azure SQL-Datenbank aktive Geo-Replikation und Geo-Wiederherstellungsfunktionen. |
| 105 | [Disaster Recovery Drilldown ausführen](sql-database-disaster-recovery-drills.md) | Erfahren Sie Leitfäden und bewährte Methoden für die Verwendung von Azure SQL-Datenbank Disaster Recovery einen Drilldown ausführen, mit dem Ihre Mission kritische Business Applications sowie zu Fehlern und Ausfällen. |
| 106 | [Disaster Recovery-Strategien für Applikationen mit SQL Datenbank elastischen Pool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Informationen Sie zum Cloud-Lösung für Disaster Recovery mit der richtigen Failover-Muster entwerfen. |
| 107 | [Splitter Karte Probleme mithilfe der RecoveryManager-Klasse](sql-database-elastic-database-recovery-manager.md) | Verwenden Sie RecoveryManager-Klasse, um Probleme mit Splitter |
| 108 | [Importieren Sie BACPAC-Datei zum Erstellen einer SQL Azure-Datenbank](sql-database-import.md) | Erstellen einer SQL Azure-Datenbank durch Importieren einer vorhandenen BACPAC-Datei. |
| 109 | [Wiederherstellen einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit der Azure-Portal](sql-database-point-in-time-restore-portal.md) | Wiederherstellen einer Azure SQL-Datenbank zu einem früheren Zeitpunkt Zeit. |
| 110 | [Wiederherstellen einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](sql-database-point-in-time-restore-powershell.md) | Wiederherstellen einer Azure SQL-Datenbank zu einem früheren Zeitpunkt in Zeit |
| 112 | [Wiederherstellen einer Azure SQL-Datenbank mit automatisierter backups](sql-database-recovery-using-backups.md) | Erfahren Sie mehr über Point-in-Time-Wiederherstellung, die Sie einer Azure SQL-Datenbank zu einem früheren Zeitpunkt in bis zu 35 Tage zurücksetzen kann. |
| 113 | [Wiederherstellen einer gelöschten Azure SQL-Datenbank mit der Azure-Portal](sql-database-restore-deleted-database-portal.md) | Wiederherstellen einer gelöschten Azure SQL-Datenbank (Azure-Portal). |
| 114 | [Wiederherstellen einer gelöschten Azure SQL-Datenbank mithilfe von PowerShell](sql-database-restore-deleted-database-powershell.md) | Wiederherstellen einer gelöschten Azure SQL-Datenbank (PowerShell). |
| 115 | [Wiederherstellen einer Datenbank zu einem bestimmten Zeitpunkt eine gelöschte Datenbank wiederherstellen oder einen Rechenzentrums hat wiederherstellen](sql-database-troubleshoot-backup-and-restore.md) | Informationen Sie zum Fehler und Ausfälle Backups und Replikate in Azure SQL-Datenbank mit einer Wiederherstellung. |



## <a name="migrate"></a>Migrieren

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 116 | [Exportieren einer SQL Server-Datenbank in eine SqlPackage mit BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank exportieren, BACPAC Datei Sqlpackage exportieren |
| 117 | [Exportieren einer SQL Server-Datenbank in eine SQL Server Management Studio mit BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank exportieren, BACPAC Datei exportieren Data-Tier Application Wizard exportieren |
| 118 | [SQL-Datenbank aus einer BACPAC Datei SqlPackage importieren](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure SQL-Datenbank Datenbankmigration importieren, importieren BACPAC Datei sqlpackage |
| 119 | [Importieren von BACPAC in SQL-Datenbank mit SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Microsoft Azure SQL-Datenbank Datenbank bereitstellen, Datenbankmigration Datenbank importieren exportieren Datenbank Migrationsassistenten |
| 120 | [Migrieren von SQL Server-Datenbank in SQL Datenbank Datenbank Microsoft Azure-Datenbank-Assistenten bereitstellen](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure SQL-Datenbank Datenbankmigration Microsoft Azure-Datenbank-Assistent |
| 121 | [Migrieren von SQL Server-Datenbank in Azure SQL-Datenbank mithilfe der Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank importieren Transaktionsreplikation |
| 122 | [Ermitteln Sie die SQL-Datenbank-Kompatibilität mit SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure SQL-Datenbank Datenbankmigration, SQL Datenbank Kompatibilität SqlPackage |
| 123 | [Verwenden Sie SQL Server Management Studio, bestimmt SQL Datenbank-Kompatibilitätsgrad vor der Migration in Azure SQL-Datenbank](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure SQL-Datenbank Datenbankmigration, SQL Datenbank-Kompatibilitätsgrad Tier Anwendung Assistent für den Export |
| 124 | [Verwenden Sie SQL Azure Assistenten beheben SQL Server Datenbank Kompatibilitätsprobleme vor der Migration in Azure SQL-Datenbank](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure SQL-Datenbank Datenbankmigration, Kompatibilität, SQL Azure-Migrationsassistenten |
| 125 | [Migrieren einer SQL Server-Datenbank in SQL Azure-Datenbank mithilfe von SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Microsoft Azure SQL-Datenbank Datenbankmigration, Kompatibilität, SQL Azure-Migrationsassistenten, SSDT |
| 126 | [SQL Server-Datenbank Kompatibilität Probleme mit SQL Server Management Studio vor der Migration zu SQL-Datenbank](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure SQL-Datenbank Datenbankmigration, Kompatibilität, SQL Azure-Migrationsassistenten |
| 127 | [Archivieren einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe von PowerShell](sql-database-export-powershell.md) | Archivieren einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe von PowerShell |
| 128 | [Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-import-powershell.md) | Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell |
| 129 | [Laden von Daten aus CSV in Azure SQL Data Warehouse (Flatfiles)](sql-database-load-from-csv-with-bcp.md) | Für eine kleine Größe verwendet Bcp zum Importieren von Daten in Azure SQL-Datenbank. |
| 130 | [Verschieben von Datenbanken zwischen Servern, Abonnements und in Azure](sql-database-troubleshoot-moving-data.md) | Schritte zum Kopieren, verlagern und Migrieren von Daten und Datenbanken in Azure SQL-Datenbank. |



## <a name="move-data-in-and-out"></a>Verschieben von Daten und

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 131 | [SQL Server-Datenbankmigration zu SQL-Datenbank in der cloud](sql-database-cloud-migrate.md) | Erfahren Sie, wie lokale SQL Server-Datenbankmigration zu Azure SQL-Datenbank in der Cloud. Verwenden Sie Datenbank-Migrationstools bei Prüfung vor der Datenbankmigration. |
| 132 | [Kopieren einer SQL Azure-Datenbank](sql-database-copy.md) | Erstellen Sie eine Kopie einer SQL Azure-Datenbank |
| 133 | [Archivieren einer Datenbank SQL Azure eine BACPAC-Datei mithilfe der Azure-Portal](sql-database-export.md) | Archivieren einer Datenbank SQL Azure eine BACPAC-Datei mithilfe der Azure-Portal |



## <a name="security"></a>Sicherheit

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 134 | [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Datawarehouse Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) | Informationen Sie zum SQL-Datenbank herstellen, von Azure Active Directory-Authentifizierung. |
| 135 | [Verschlüsselt: Schützen Sie vertrauliche Daten in SQL-Datenbank und Speichern von Verschlüsselungsschlüsseln im Windows-Zertifikatspeicher](sql-database-always-encrypted.md) | Schützen Sie vertrauliche Daten in der SQL-Datenbank in Minuten. |
| 136 | [Verschlüsselt: Schützen Sie vertrauliche Daten in SQL-Datenbank und speichern Sie Verschlüsselungsschlüssel in Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md) | Schützen Sie vertrauliche Daten in der SQL-Datenbank in Minuten. |
| 137 | [SQL-Datenbank - kompatible Clients unterstützen und IP-Endpunkt Überwachung ändert](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Informationen Sie zu SQL-Datenbank zur Unterstützung für kompatible Clients und IP-Endpunkt ändert Überwachung. |
| 138 | [Mithilfe von PowerShell konfigurieren Sie Azure SQL-Datenbank auf Serverebene Firewall-Regeln](sql-database-configure-firewall-settings-powershell.md) | Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die SQL Azure-Datenbanken zugreifen. |
| 139 | [Konfigurieren Sie die REST-API mit Azure SQL-Datenbank auf Serverebene Firewall-Regeln](sql-database-configure-firewall-settings-rest.md) | Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die SQL Azure-Datenbanken zugreifen. |
| 140 | [Mit T-SQL Azure SQL-Datenbank-Server und Datenbank Firewall-Regeln konfigurieren](sql-database-configure-firewall-settings-tsql.md) | Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die SQL Azure-Datenbanken zugreifen. |
| 141 | [Erste Schritte mit SQL-Datenbank dynamische Daten maskieren (Azure-Portal)](sql-database-dynamic-data-masking-get-started.md) | Erste Schritte mit SQL-Datenbank dynamische Daten Masking in Azure-Portal |
| 142 | [Erste Schritte mit SQL-Datenbank dynamische Daten maskieren (Azure-Verwaltungsportal)](sql-database-dynamic-data-masking-get-started-portal.md) | Erste Schritte mit SQL-Datenbank dynamische Daten Masking in Azure-Verwaltungsportal |
| 143 | [Konfigurieren von Firewallregeln Azure SQL-Datenbank \- Überblick](sql-database-firewall-configure.md) | So konfigurieren Sie die Firewall eine SQL-Datenbank mit Server und Datenbank Firewall den Zugriff verwalten. |
| 144 | [Lernprogramm für SQL: Benutzerkonten verwaltet und einer SQL-Datenbank erstellen](sql-database-get-started-security.md) | Informationen Sie zum Erstellen von Benutzerkonten Zugriff auf und das verwalten eine Datenbank. |
| 145 | [SQL Datenbank-Authentifizierung und Autorisierung: Zugriff](sql-database-manage-logins.md) | Erfahren Sie mehr über SQL Datenbank Sicherheitsmanagement insbesondere wie Datenbank Zugriff und Anmeldung über das Prinzipal auf Serverebene Konto Sicherheitsmanagement. |
| 146 | [Azure SQL-Datenbank Sicherheits Richtlinien](sql-database-security-guidelines.md) | Informationen Sie zu Microsoft Azure SQL-Datenbank-Richtlinien und Einschränkungen Sicherheit. |
| 147 | [Erste Schritte mit SQL-Datenbank Erkennung](sql-database-threat-detection-get-started.md) | Erste Schritte mit SQL-Datenbank Erkennung im Azure-Portal |
| 148 | [Zum Ausführen von Verwaltungsaufgaben wie das Administratorkennwort in Azure SQL-Datenbank zurücksetzen](sql-database-troubleshoot-permissions.md) | Beschreibt, wie allgemeine Verwaltungsaufgaben in SQL-Datenbank. Administratorkennwort gewähren und Entziehen von Access beispielsweise zurücksetzen. |



## <a name="manage-your-database"></a>Verwalten der Datenbank

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 149 | [Erste Schritte mit SQL Datenbank-auditing](sql-database-auditing-get-started.md) | Erste Schritte mit SQL Datenbank-auditing |
| 150 | [Konfigurieren einer Azure SQL-Datenbank auf Serverebene mithilfe Firewallregel Azure-Portal](sql-database-configure-firewall-settings.md) | Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die Azure SQL Server zugreifen. |
| 151 | [Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md) | Erstellen Sie eine Kopie einer SQL Azure-Datenbank |
| 152 | [Verwalten von Azure Automatisierung Azure SQL-Datenbanken](sql-database-manage-automation.md) | Erfahren Sie, wie Azure Automation Service zur Azure SQL-Datenbanken Ebene verwalten. |
| 153 | [Übersicht: Verwaltungstools für SQL-Datenbank](sql-database-manage-overview.md) | Tools und Optionen zum Verwalten von Azure SQL-Datenbank vergleicht |
| 154 | [Überwachung über dynamische Verwaltungsansichten Azure SQL-Datenbank](sql-database-monitoring-with-dmvs.md) | Informationen Sie zum Erkennen und zu diagnostizieren häufig auftretende Leistungsprobleme mit dynamische Verwaltungsansichten Microsoft Azure SQL-Datenbank überwachen. |
| 155 | [Die SQL-Datenbank sichern](sql-database-security.md) | Azure SQL-Datenbank und SQL Server-Sicherheit, einschließlich der Unterschiede zwischen der Cloud erfahren und SQL Server lokal bei Authentifizierung, Autorisierung, verbindungssicherheit, Verschlüsselung und Compliance. |



## <a name="extended-events"></a>Erweiterte Ereignisse

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 156 | [Ereigniscode Datei Ziel für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-event-file.md) | PowerShell und Transact-SQL bietet für ein Zweiphasen Codebeispiel zur Datei Ziel in einem erweiterten Ereignis in Azure SQL-Datenbank. Azure-Speicher ist ein erforderlicher Teil dieses Szenarios. |
| 157 | [Ring Puffer Zielcode für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-ring-buffer.md) | Enthält ein Codebeispiel Transact-SQL, die einfach und schnell mithilfe des Ziels Ringpuffer in Azure SQL-Datenbank besteht. |
| 158 | [Erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-db-diff-from-svr.md) | Beschreibt erweiterte Ereignisse (XEvents) in Azure SQL-Datenbank und wie Veranstaltungssessions von Veranstaltungssessions in Microsoft SQL Server abweichen. |



## <a name="geo-replication"></a>Geo-Replikation

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 159 | [Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit Azure-portal](sql-database-geo-replication-failover-portal.md) | Starten einer geplanten oder ungeplanten Failover für Azure SQL Azure-portal |
| 160 | [Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-failover-powershell.md) | Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit PowerShell |
| 161 | [Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | Starten einer geplanten oder ungeplanten Failover mit Transact-SQL Azure SQL-Datenbank |
| 162 | [Übersicht: SQL Datenbank aktive Geo-Replikation](sql-database-geo-replication-overview.md) | Aktive Geo-Replikation können Sie 4 Replikaten der Datenbank eines Azure Rechenzentren einrichten. |
| 163 | [Konfigurieren Sie Geo-Replikation für Azure SQL-Datenbank mit der Azure-portal](sql-database-geo-replication-portal.md) | Konfigurieren Sie Geo-Replikation für Azure SQL Azure-portal |
| 164 | [Konfigurieren Sie Geo-Replikation für SQL Azure-Datenbank mit PowerShell](sql-database-geo-replication-powershell.md) | Konfigurieren Sie aktive Geo-Replikation für Azure SQL-Datenbank mit PowerShell |
| 165 | [Verwalten von Sicherheit von Azure SQL-Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md) | Dieses Thema erläutert Sicherheitsaspekte zum Verwalten der Sicherheit nach der Wiederherstellung einer Datenbank oder einem Failover. |
| 166 | [Konfigurieren Sie Geo-Replikation für SQL Azure-Datenbank mit Transact-SQL](sql-database-geo-replication-transact-sql.md) | Konfigurieren Sie Geo-Replikation mit Transact-SQL Azure SQL-Datenbank |
| 167 | [Geo-Wiederherstellung einer Azure SQL-Datenbank von Geo redundante Sicherungskopie der Azure-Portal](sql-database-geo-restore-portal.md) | Geo-Wiederherstellung einer Azure SQL-Datenbank von Geo-redundantes Backup (Azure-Portal). |
| 168 | [Wiederherstellen Sie eine SQL Azure-Datenbank mithilfe von PowerShell aus einer Geo redundante Sicherung](sql-database-geo-restore-powershell.md) | Wiederherstellen einer Azure SQL-Datenbank auf einen neuen Server aus Geo-redundantes backup |



## <a name="upgrade"></a>Aktualisieren

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 169 | [Verbesserte Leistung Kompatibilität auf 130 in Azure SQL-Datenbank](sql-database-compatibility-level-query-performance-130.md) | Schritte und Tools zur Bestimmung der Kompatibilitätsgrad für die Datenbank in Azure SQL-Datenbank oder Microsoft SQL Server |
| 170 | [Verwalten von parallelen Updates Cloudanwendungen SQL Datenbank aktive Geo-Replikation](sql-database-manage-application-rolling-upgrade.md) | Informationen Sie zum Azure SQL-Datenbank Geo-Replikation verwenden, um online-Upgrades der Cloudanwendung unterstützen. |
| 171 | [Upgrade auf Azure SQL-Datenbank V12 mit Azure-Portal](sql-database-upgrade-server-portal.md) | Erläutert die Azure SQL-Datenbank V12 upgrade einschließlich Web- und Datenbanken und ein V11 Server Datenbanken direkt in einem Pool elastische Datenbanken mit Azure-Portal migrieren. |
| 172 | [Upgrade auf Azure SQL-Datenbank V12 mit PowerShell](sql-database-upgrade-server-powershell.md) | Erläutert die Azure SQL-Datenbank V12 upgrade einschließlich Web- und Datenbanken und ein V11 Server Datenbanken direkt in einem Pool elastische Datenbanken mithilfe von PowerShell migrieren. |
| 173 | [Planen Sie und bereiten Sie der SQL-Datenbank V12 aktualisieren vor](sql-database-v12-plan-prepare-upgrade.md) | Beschreibt die Vorbereitung und Grenzen bei der Aktualisierung auf Version V12 von Azure SQL-Datenbank. |
| 174 | [Neuigkeiten in SQL Datenbank V12](sql-database-v12-whats-new.md) | Business-Systeme, Azure SQL-Datenbank in der Cloud profitieren werden durch eine Aktualisierung auf Version V12 jetzt beschreibt. |
| 175 | [Web und Business Edition Sonnenuntergang FAQ](sql-database-web-business-sunset-faq.md) | Finden Sie die Datenbanken Azure SQL Web wird und erfahren Sie mehr über die Features und Funktionen der neuen Dienstebenen. |



## <a name="tools"></a>Tools

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 176 | [Azure SQL-Datenbank mit PowerShell verwalten](sql-database-command-line-tools.md) | Azure SQL-Datenbank-Management mit PowerShell. |
| 177 | [Lernprogramm für SQL: eine SQL Azure-Datenbank mit Excel und Erstellen eines Berichts](sql-database-connect-excel.md) | Erfahren Sie, wie Microsoft Excel mit SQL Azure-Datenbank in die Cloud zu verbinden. Importieren Sie Daten für Berichte und Daten in Excel. |
| 178 | [Erstellen Sie eine SQL-Datenbank und Aufgaben Sie häufige Datenbank Setup mit PowerShell-cmdlets](sql-database-get-started-powershell.md) | Lernen Sie eine SQL-Datenbank mit PowerShell erstellen. Setup Aufgaben können über PowerShell-Cmdlets verwaltet werden. |
| 179 | [Erste Schritte mit SQL Azure Data Sync (Vorschau)](sql-database-get-started-sql-data-sync.md) | Dieses Lernprogramm hilft Ihnen Einstieg in Azure SQL Data Sync (Vorschau). |
| 180 | [Verwalten von Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md) | Erfahren Sie, wie SQL Server Management Studio verwenden, um SQL-Datenbankserver und Datenbanken verwalten. |
| 181 | [Verwalten von Azure SQL-Datenbanken mithilfe von Azure-portal](sql-database-manage-portal.md) | Informationen Sie zum Azure-Portal verwenden, um eine relationale Datenbank in der Cloud Azure-Portal verwalten. |
| 182 | [Ändern Sie Tier und Leistung Dienstebene (Tarif) einer SQL-Datenbank mit PowerShell](sql-database-scale-up-powershell.md) | Ändern der Dienstebene und Leistung einer Azure SQL-Datenbank zeigt die SQL-Datenbank mit PowerShell nach oben oder unten zu skalieren. Den Tarif einer Azure SQL-Datenbank mit PowerShell ändern. |
| 183 | [Verwenden Sie SQL Server Management Studio in Azure RemoteApp Verbindung zur SQL-Datenbank](sql-database-ssms-remoteapp.md) | Verwenden Sie in diesem Lernprogramm erfahren Sie, wie mit SQL Server Management Studio in Azure RemoteApp für Sicherheit und Leistung beim Verbinden mit SQL-Datenbank |



## <a name="reference"></a>Referenz

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 184 | [Bibliotheken für SQL-Datenbank und SQL Server-Verbindung](sql-database-libraries.md) | Zeigt die minimale Versionsnummer für jeden Treiber, mit dem Programme Azure SQL-Datenbank oder Microsoft SQL Server herzustellen. Ein Link ist Version Informationen zu Treibern bereitgestellt, die von der Gemeinschaft nicht von Microsoft veröffentlicht wurden. |
| 185 | [Azure SQL-Datenbank Ressourcengrenzen](sql-database-resource-limits.md) | Diese Seite beschreibt einige allgemeine Ressourcengrenzen für Azure SQL-Datenbank. |
| 186 | [Azure SQL-Datenbank-Transact-SQL-Unterschiede](sql-database-transact-sql-information.md) | Transact-SQL-Anweisungen, die in Azure SQL-Datenbank weniger als vollständig unterstützt werden. |



## <a name="miscellaneous"></a>Sonstige

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 187 | [Kopieren einer SQL Azure-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md) | Kopie einer Azure SQL-Datenbank mit PowerShell |
| 188 | [Kopieren einer SQL Azure-Datenbank mithilfe von Transact-SQL](sql-database-copy-transact-sql.md) | Erstellen Sie Kopie einer Azure SQL-Datenbank mithilfe von Transact-SQL |
| 189 | [SQL Azure Database General Grenzen und Richtlinien](sql-database-general-limitations.md) | Diese Seite beschreibt einige allgemeinen Einschränkungen für Azure SQL-Datenbank sowie Interoperabilität und Support. |
| 190 | [SQL Datenbank Preise Tier recommendations](sql-database-service-tier-advisor.md) | Beim Ändern der Preise sind Ebenen in Azure-Portal Preise Tier Vorschläge sollten die Ebene, der am besten geeignet für eine vorhandene Azure SQL-Datenbank Arbeitslast ausgeführt. Tarifen Beschreiben der Service-Tier und Leistung einer SQL-Datenbank. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Extras

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Verwandte Artikel aus dem Azure-Dienste

- [Sichern Sie Ihre Azure App Services-Anwendung in Azure](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Dokumentationstools

- [Updates für Azure SQL-Datenbank angekündigt](http://azure.microsoft.com/updates/?service=sql-database)

- [Suchen der Dokumentation alle Microsoft Azure mit Filtern](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Learning Path Grafiken

- [Learning Path Grafiken für alle Microsoft Azure-Dienste](http://azure.microsoft.com/documentation/learning-paths/)

- [Learning Path: Lernen Sie Azure SQL-Datenbank](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Learning Path: SQL Datenbank elastische skalieren](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->


