<properties
    pageTitle="Azure SQL-Datenbank-Server und Datenbank Firewall-Regeln mit T-SQL | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die SQL Azure-Datenbanken zugreifen."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Mit T-SQL Azure SQL-Datenbank-Server und Datenbank Firewall-Regeln konfigurieren


> [AZURE.SELECTOR]
- [Übersicht](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-Datenbank verwendet Firewall-Regeln zwischen Servern und Datenbanken. Server und Datenbank Firewall Settings Master oder eine Benutzerdatenbank können in der Azure SQL-Datenbankserver selektiv auf die Datenbank zugreifen.

> [AZURE.IMPORTANT] Um von Azure Verbindung zum Datenbankserver zu ermöglichen, müssen Azure Verbindungen aktiviert werden. Weitere Informationen zu Firewall-Regeln und ermöglicht Verbindungen von Azure finden Sie unter [Azure SQL-Datenbank-Firewall](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Azure-Cloud erstellen, müssen Sie einige zusätzliche TCP-Anschlüsse öffnen. Weitere Informationen finden Sie unter der **V12-SQL-Datenbank: außerhalb Vs in** Abschnitt Ports [über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="server-level-firewall-rules"></a>Auf Serverebene Firewall-Regeln

Nur auf Serverebene principal Anmeldung oder Azure Active Directory-Administrator kann mithilfe von Transact-SQL-Serverebene Firewallregel erstellen.

1. Ein Abfragefenster starten und Verbinden mit virtuellen master-Datenbank mithilfe von SQL Server Management Studio.
2. Serverebene Firewallregeln können aktiviert, erstellt, aktualisiert oder Abfragefenster gelöscht werden.
3. Führen Sie zum Erstellen oder Aktualisieren der Serverebene Firewallregeln, die `sp_set_firewall_rule` gespeicherte Prozedur. Im folgenden Beispiel aktiviert einen Bereich von IP-Adressen auf dem Server Contoso.<br/>Zunächst sehen, nach welchen Regeln vorhanden.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Fügen Sie eine Firewall-Regel.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Führen Sie zum Löschen einer Firewallregel auf Serverebene Sp_delete_firewall_rule gespeicherte Prozedur. Das folgende Beispiel löscht die Regel mit dem Namen ContosoFirewallRule.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Weitere Informationen zu diesen gespeicherten Prozeduren finden Sie unter [Sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) und [Sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Firewallregeln auf Datenbankebene

Nur ein Datenbankbenutzer mit **der Berechtigung in der Datenbank (z. B. der Datenbankbesitzer)** kann auf Datenbankebene Firewallregel erstellen.

1. Starten Sie nach dem Erstellen einer Firewalls auf Serverebene für Ihre IP-Adresse, ein Abfragefenster Classic-Portal oder über SQL Server Management Studio.
2. Verbinden Sie mit der Datenbank eine Firewallregel auf Datenbankebene erstellt werden soll.

    Erstellen Sie ein neues oder aktualisieren eine bestehende Firewallregeln auf Datenbankebene Ausführen der `sp_set_database_firewall_rule` gespeicherte Prozedur. Das folgende Beispiel erstellt eine neue Firewall-Regel mit dem Namen ContosoFirewallRule.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Um eine bestehende Firewallregeln auf Datenbankebene zu löschen, führen Sie die `sp_delete_database_firewall_rule` gespeicherte Prozedur. Das folgende Beispiel löscht die Regel mit dem Namen ContosoFirewallRule.
`
   EXEC Sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

Weitere Informationen zu diesen gespeicherten Prozeduren finden Sie unter [Sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) und [Sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Nächste Schritte

Für wie Artikel auf Serverebene Firewallregeln mit anderen Methoden finden Sie unter: 

- [Konfigurieren Sie Azure SQL-Datenbank auf Serverebene Firewall-Regeln mit der Azure-Portal](sql-database-configure-firewall-settings.md)
- [Mithilfe von PowerShell Azure SQL-Datenbank auf Serverebene Firewall-Regeln konfigurieren](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurieren Sie die REST-API mit Azure SQL-Datenbank auf Serverebene Firewall-Regeln](sql-database-configure-firewall-settings-rest.md)

Ein Lernprogramm zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten Azure-Portal](sql-database-get-started.md).
Hilfe in Verbindung mit einer Azure SQL-Datenbank von open Source oder Drittanbietern finden Sie unter [Client Schnellstart - Beispiele zu SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Navigieren zu Datenbanken finden Sie unter [Zugriff und Anmeldung Sicherheit verwalten](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Sichern der Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbankmodul und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)
