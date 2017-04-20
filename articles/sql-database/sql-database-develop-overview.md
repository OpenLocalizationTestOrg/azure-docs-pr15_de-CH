<properties
    pageTitle="SQL-Datenbank entwickeln Übersicht | Microsoft Azure"
    description="Informationen Sie zu verfügbaren Connectivity-Bibliotheken und best Practices für die Anwendung mit SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>SQL Datenbank-Anwendungsentwicklung, Überblick
Dieser Artikel führt über grundlegende Aspekte, denen Entwickler beim Schreiben von Code zum Azure SQL-Datenbank herstellen beachten sollten.

## <a name="language-and-platform"></a>Sprache und Plattform
Gibt Codebeispiele für verschiedene Programmiersprachen und Plattformen. Finden Sie Links zu den Codebeispielen in: 

* Weitere Informationen: [Bibliotheken für SQL-Datenbank und SQL Server-Verbindung](sql-database-libraries.md)

## <a name="resource-limitations"></a>Ressourcen
Azure SQL-Datenbank verwaltet eine Datenbank mithilfe zweier verschiedener Mechanismen zur Ressourcen: Ressourcen Governance und Durchsetzung von Grenzen.

* Weitere Informationen: [Ressourcengrenzen Azure SQL-Datenbank](sql-database-resource-limits.md)

## <a name="security"></a>Sicherheit
Azure SQL-Datenbank bietet Ressourcen zur Einschränkung des Zugriffs, Datenschutz und Überwachung von Aktivitäten in einer SQL-Datenbank.

* Weitere Informationen: [die SQL-Datenbank sichern](sql-database-security.md)

## <a name="authentication"></a>Authentifizierung
* Azure SQL-Datenbank unterstützt sowohl Benutzer der SQL Server-Authentifizierung Benutzernamen sowie [Authentifizierung Azure Active Directory](sql-database-aad-authentication.md) -Benutzer und Benutzernamen.
* Sie müssen eine bestimmte Datenbank anstatt auf der *master* -Datenbank angeben.
* Anweisung Transact-SQL **verwenden, MyDatabaseName,** können SQL-Datenbank Sie in einer anderen Datenbank wechseln.
* Weitere Informationen: [SQL Sicherheit: Datenbank Zugriff und Anmeldung Sicherheit](sql-database-manage-logins.md)

## <a name="resiliency"></a>Stabilität
Tritt ein vorübergehender Fehler beim Verbinden mit SQL-Datenbank, sollte Ihr Code den Aufruf wiederholen.  Wir empfehlen, die Logik verwenden Backoff-Logik wiederholen, damit nicht der SQL-Datenbank mit mehreren Clients gleichzeitig wiederholen überlastet ist.

* Code-Beispiele: Codebeispiele veranschaulichen Logik wiederholen, finden Sie unter Beispiele für die Sprache Ihrer Wahl an: [Verbindung Bibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)
* Weitere Informationen: [Fehlermeldungen für SQL-Datenbank-Clientprogramme](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Verwaltung
* In der Client-Verbindungslogik Überschreiben der Standard-Timeoutwert 30 Sekunden.  Der Standardwert von 15 Sekunden ist für Verbindungen über das Internet ausgeführt.
* Bei Verwendung eines [Verbindungs-Pool](http://msdn.microsoft.com/library/8xx3tyca.aspx)müssen Sie die Verbindung sofort schließen Ihr Programm nicht aktiv verwendet wird und nicht bereitet es wiederverwenden.

## <a name="network-considerations"></a>Vorüberlegungen zum Netzwerk
* Sicherzustellen Sie auf dem Computer, der die Client-Anwendung hostet, dass die Firewall ausgehenden TCP-Kommunikation über Port 1433 zulässt.  Weitere Informationen: [Konfigurieren einer Firewall Azure SQL-Datenbank](sql-database-configure-firewall-settings.md)
* Wenn Ihr Clientprogramm SQL Datenbank V12 verbindet, während der Client auf eine Azure Virtual Machine (VM) ausgeführt wird, müssen Sie bestimmte Portbereiche auf dem virtuellen Computer öffnen. Weitere Informationen: [Ports über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)
* Clientverbindungen zu Azure SQL-Datenbank V12 manchmal umgehen den Proxy und direkt mit der Datenbank interagieren. Anschlüsse als 1433 Bedeutung. Weitere Informationen: [Ports über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Daten Sharding mit elastische
Elastische Skalierung vereinfacht die Skalierung (und). 

* [Entwurfsmuster für Multi-Tenant SaaS-Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md)
* [Erste Schritte mit SQL Azure-Datenbank elastische Skalierung Vorschau](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Nächste Schritte

Entdecken Sie die [Funktionen der SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).
