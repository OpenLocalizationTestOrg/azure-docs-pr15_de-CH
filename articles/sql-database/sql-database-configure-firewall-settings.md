<properties
    pageTitle="Konfigurieren eine SQL-Datenbank auf Serverebene Firewallregel | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die Azure SQL Server zugreifen."
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
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Konfigurieren einer Azure SQL-Datenbank auf Serverebene mithilfe Firewallregel Azure-Portal


> [AZURE.SELECTOR]
- [Übersicht](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)

Azure SQL Server verwendet Firewallregeln Verbindung zu Ihren Servern und Datenbanken. Server und Datenbank Firewall Settings Master oder eine Benutzerdatenbank können in Ihrem SQL Azure-logische Server selektiv auf die Datenbank zugreifen. Dieses Thema behandelt Serverebene Firewallregeln.

> [AZURE.IMPORTANT] Um von Azure, SQL Azure-Server verbinden zu ermöglichen, müssen Azure Verbindungen aktiviert werden. Wie von die Firewall-Regeln finden Sie unter [eine SQL Azure-Server-Firewall konfigurieren \- Übersicht über](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Azure-Cloud erstellen, müssen Sie einige zusätzliche TCP-Anschlüsse öffnen. Weitere Informationen finden Sie unter der **V12-SQL-Datenbank: außerhalb Vs in** Abschnitt Ports [über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)

**Empfehlung:** Verwenden Sie auf Serverebene Firewallregeln für Administratoren und, wenn viele Datenbanken mit den gleichen zugriffsanforderungen Zeit jede Datenbank einzeln konfigurieren möchten. Microsoft empfiehlt die Verwendung der Datenbankebene Firewallregeln möglichst Sicherheit zu erhöhen und die Datenbank leichter.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Verwalten Sie vorhandene Firewall auf Serverebene Regeln über den Azure-portal

Wiederholen Sie die Schritte zum Verwalten der Serverebene Firewallregeln.

- Um den aktuellen Computer hinzuzufügen, klicken Sie auf Add Client-IP.
- Geben Sie zum Hinzufügen weiterer IP-Adressen in der Regel IP-Startadresse und letzte IP-Adresse.
- Um eine vorhandene Regel zu ändern, klicken Sie auf eines der Felder in der Regel und ändern.
- Um eine vorhandene Regel zu löschen, bewegen Sie den Cursor über die Regel X am Ende der Zeile angezeigt. Klicken Sie auf X, um die Regel zu entfernen.

Klicken Sie auf Änderungen **Speichern** .

## <a name="next-steps"></a>Nächste Schritte

Ein Artikel mit Transact-SQL-Server und Datenbank Firewall-Regeln zum Anzeigen Sie [Azure SQL-Datenbank konfigurieren Sie Server und Datenbank Firewallregeln mit T-SQL](sql-database-configure-firewall-settings-tsql.md) 

Für wie Artikel auf Serverebene Firewallregeln mit anderen Methoden finden Sie unter: 

- [Mithilfe von PowerShell Azure SQL-Datenbank auf Serverebene Firewall-Regeln konfigurieren](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurieren Sie die REST-API mit Azure SQL-Datenbank auf Serverebene Firewall-Regeln](sql-database-configure-firewall-settings-rest.md)

Ein Lernprogramm zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten Azure-Portal](sql-database-get-started.md).
Hilfe in Verbindung mit einer Azure SQL-Datenbank von open Source oder Drittanbietern finden Sie unter [Client Schnellstart - Beispiele zu SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Navigieren zu Datenbanken finden Sie unter [Zugriff und Anmeldung Sicherheit verwalten](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Sichern der Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbankmodul und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

 
