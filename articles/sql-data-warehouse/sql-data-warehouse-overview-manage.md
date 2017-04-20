<properties
   pageTitle="Datenbanken in Azure SQL Data Warehouse verwalten | Microsoft Azure"
   description="Übersicht über SQL Data Warehouse-Datenbanken verwalten. Enthält Verwaltungstools, DWUs und Scale-Out-Performance troubleshooting Abfrageperformance gute Sicherheitsrichtlinien einrichten und Wiederherstellen einer Datenbank von Datenkorruption oder einen regionalen Ausfall."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse-Datenbanken verwalten

SQL Data Warehouse automatisiert viele Aspekte der Datenbanken verwalten. Zum Beispiel genügt zum Skalieren der Leistung das richtige Maß Serverressourcen bezahlen, und lassen Sie SQL Data Warehouse Arbeit und Dezentrales Skalieren zurück. 

Sicherlich möchten überwachen Workload um Ihren Bedürfnissen Leistung sowie Abfragen zu beheben. Sie müssen auch einige Sicherheitsaufgaben Berechtigungen für Benutzer und Logins verwalten.

Dieser Überblick befasst sich diese Aspekte der Verwaltung von SQL Data Warehouse.

- Management-tools
- Skalierung Compute
- Anhalten und fortsetzen
- Best Practices zur Performance
- Überwachen der Abfrage
- Sicherheit
- Sicherung und Wiederherstellung

## <a name="management-tools"></a>Management-tools

Eine Vielzahl von Tools können Sie Datenbanken im SQL Data Warehouse verwalten. Verwalten von Datenbanken entwickeln Sie Voreinstellungen Werkzeug für jede Aufgabe ausführen müssen.

### <a name="azure-portal"></a>Azure-portal
[Azure-Portal][] ist eine webbasierte Portal, können erstellen, aktualisieren und Löschen der Datenbanken und Ressourcen überwachen. Dieses Tool ist groß ist sind nur Einstieg in Azure, eine kleine Anzahl von Data Warehouse-Datenbanken verwalten oder schnell etwas tun müssen.

Zunächst mit der Azure-Portal finden Sie unter [Erstellen einer SQL Data Warehouse (Azure-Portal)][].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools in Visual Studio
[SQL Server Data Tools][] (SSDT) verwalten und Entwickeln von Datenbanken in Visual Studio können Sie eine Verbindung hergestellt werden. Wenn Sie Anwendungsentwickler mit Visual Studio oder integrated Development Environments (IDEs) vertraut sind, versuchen Sie SSDT in Visual Studio.

SSDT enthält die SQL Server-Objekt-Explorer visualisieren, verbinden und Skripts für SQL Data Warehouse-Datenbanken ausführen können. Schnelle Verbindung zu SQL Data Warehouse können Sie die Schaltfläche in der Befehlsleiste **in Visual Studio öffnen** klicken beim Anzeigen der Details der Datenbank in der Azure-Verwaltungsportal.  

Zunächst mit SSDT in Visual Studio finden Sie unter [Query Azure SQL Data Warehouse mit Visual Studio][].

### <a name="command-line-tools"></a>Befehlszeilentools
Befehlszeilen-Dienstprogramme sind ideal für Ihre Arbeitslasten automatisieren.  PowerShell und Sqlcmd sind zwei großartige Arten, Ihre Prozesse zu automatisieren.  Es wird empfohlen, diese Tools für die Verwaltung einer großen Anzahl von logischen Servern und Änderung einer Ressource erforderlichen Aufgaben können Skripts erstellt und anschließend die automatische Bereitstellung.

### <a name="dynamic-management-views"></a>Dynamische Verwaltungsansichten 

DMVs sind die Grundbestandteile SQL Data Warehouse verwalten. Fast alle Informationen, der im Portal Flächen verwendet DMVs. Eine Liste der SQL Data Warehouse DMVs finden Sie unter [SQL Data Warehouse Systemansichten][].

Zunächst [Verbinden und Abfrage mit Sqlcmd][]und [Erstellen einer Datenbank (PowerShell)][]angezeigt.

## <a name="scale-compute"></a>Skalierung compute

SQL Data Warehouse können Sie schnell Leistung oder zurück durch erhöhen oder Verringern der Serverressourcen von CPU, Speicher und e/a-Bandbreite skalieren. Zum Skalieren der Leistung müssen Sie lediglich die Anzahl der Data Warehouse-Einheiten (DWUs) anpassen, die Ihre Datenbank SQL Data Warehouse zuweist. SQL Data Warehouse schnell die Änderung und behandelt alle zugrunde liegenden Änderungen an Hardware oder Software.

Skalierung DWUs Informationen finden Sie unter [Skalierung von Performance][].

##  <a name="pause-and-resume"></a>Anhalten und fortsetzen

Um Kosten zu sparen, können Sie anhalten und Fortsetzen von Compute-Ressourcen bei Bedarf. Z. B. Wenn Sie die Datenbank nachts und an Wochenenden verwenden, können Sie während dieser Zeit anhalten und tagsüber fortsetzen. Sie werden für DWUs berechnet, während die Datenbank angehalten wird.

Weitere Informationen finden Sie unter [berechnen anhalten][]und [Fortsetzen berechnen][].

## <a name="performance-best-practices"></a>Best Practices zur Performance

Zu Beginn eine neue Technologie, sparen entdecken, Tipps und Tricks, die Arbeiten am besten rechts ab Sie viel Zeit.  Best Practices in vielen Themen finden.

Viele eine Zusammenfassung der wichtigsten Aspekte bei der Entwicklung der Arbeitslast finden Sie unter [SQL Data Warehouse Best Practices][].

## <a name="query-monitoring"></a>Überwachen der Abfrage

Manchmal eine Abfrage zu lange ausgeführt wird, aber Sie sind nicht sicher die Ursache ist. SQL Data Warehouse verfügt über dynamische Verwaltungsansichten (DMVs), mit denen Sie herausfinden, welche Abfrage zu lange dauert. 

Lang andauernde Abfragen finden Sie [Ihre Arbeitslast mit DMVs überwachen][].

## <a name="security"></a>Sicherheit

Um ein sicheres System zu erhalten, müssen Sie wachsam sein und gegen unbefugten Zugriff zu schützen. Ein Sicherheitssystem muss sicherstellen, dass die Firewall-Regeln nur berechtigt sind IP-Adressen herstellen können. Ordnungsgemäße Authentifizierung von Benutzeranmeldeinformationen erforderlich. Nachdem ein Benutzer mit der Datenbank verbunden ist, sollte der Benutzer nur eine minimale Anzahl von Aktionen ausführen berechtigt. Um Daten zu schützen, können Sie Verschlüsselung verwenden. Es ist auch wichtig, überwachen und verfolgen, damit Sie Ereignisse ist verdächtige Aktivitäten verfolgen können.

Informationen zum Sicherheitsmanagement, Head über [Sicherheit-Überblick][].

## <a name="backup-and-restore"></a>Sicherung und Wiederherstellung

Zuverlässige Backps der Daten ist ein wesentlicher Bestandteil jeder Produktionsdatenbank. SQL Data Warehouse schützt Ihre Daten durch aktiven Datenbanken in regelmäßigen Abständen automatisch sichern. Diese Sicherung können Sie Wiederherstellung Szenarien, wo Sie Ihre Daten beschädigt oder versehentlich gelöscht, die Daten oder Datenbankobjekte.  Daten-backup-Planung Aufbewahrungsrichtlinie und Wiederherstellen eine Datenbank finden Sie unter [Wiederherstellen von Snapshots][].

## <a name="next-steps"></a>Nächste Schritte
Verwenden von guten Datenbankentwurf, Grundsätze zur Verwaltung der Datenbanken im SQL Data Warehouse erleichtert. Lernen, Head über [Development Overview][].

<!--Image references-->

<!--Article references-->
[Erstellen Sie ein SQL Datawarehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Erstellen einer Datenbank (PowerShell)]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Abfrage SQL Azure Datawarehouse mit Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Verbinden und Abfrage mit sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Übersicht über die Anwendungsentwicklung]: sql-data-warehouse-overview-develop.md
[Überwachen Sie Ihre Arbeitslast mit DMVs]: sql-data-warehouse-manage-monitor.md
[Pause compute]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Wiederherstellen von Snapshots]: sql-data-warehouse-restore-database-overview.md
[Resume compute]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Skalierung von performance]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Übersicht über die Sicherheit]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse Systemansichten]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure-portal]: http://portal.azure.com/
