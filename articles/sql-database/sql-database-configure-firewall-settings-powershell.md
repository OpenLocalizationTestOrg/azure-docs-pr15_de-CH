<properties
    pageTitle="Azure SQL-Datenbank auf Serverebene Firewall-Regeln mit PowerShell konfigurieren | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die SQL Azure-Datenbanken zugreifen."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="sstein"/>


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Mithilfe von PowerShell konfigurieren Sie Azure SQL-Datenbank auf Serverebene Firewall-Regeln


> [AZURE.SELECTOR]
- [Übersicht](sql-database-firewall-configure.md)
- [Azure-portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Azure SQL-Datenbank verwendet Firewall-Regeln zwischen Servern und Datenbanken. Server und Datenbank Firewall Settings für die master-Datenbank oder eine Datenbank können in der SQL-Datenbankserver selektiv auf die Datenbank zugreifen.

> [AZURE.IMPORTANT] Um von Azure Verbindung zum Datenbankserver zu ermöglichen, müssen Azure Verbindungen aktiviert werden. Weitere Informationen zu Firewall-Regeln und ermöglicht Verbindungen von Azure finden Sie unter [Azure SQL-Datenbank-Firewall](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Azure-Cloud erstellen, müssen Sie einige zusätzliche TCP-Anschlüsse öffnen. Weitere Informationen finden Sie unter der "V12 SQL Datenbank: außerhalb Vs in" Abschnitt Ports [über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Server-Firewall-Regeln erstellen

Auf Serverebene Firewall Regeln erstellt werden, aktualisiert und mithilfe von Azure PowerShell gelöscht.

Erstellen Sie eine neue Firewallregel auf Serverebene ausgeführt [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)-Cmdlet. Im folgenden Beispiel aktiviert einen Bereich von IP-Adressen auf dem Server Contoso.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Ändern Sie eine bestehende Firewallregeln auf Serverebene ausgeführt [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)-Cmdlet. Im folgenden Beispiel wird den Bereich der zulässigen IP-Adressen für die Regel ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Führen Sie zum Löschen einer vorhandenen Serverebene Firewallregel [Remove-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)-Cmdlet. Das folgende Beispiel löscht die Regel mit dem Namen ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Verwalten Sie Firewall-Regeln mit PowerShell

PowerShell können Sie um Firewallregeln zu verwalten. Weitere Informationen finden Sie unter den folgenden Themen:

* [Neue AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [Remove-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Nächste Schritte

Informationen zur Verwendung von anzeigen Transact-SQL-Server und Datenbank Firewallregeln erstellen [mit T-SQL Azure SQL-Datenbank konfigurieren Sie Server und Datenbank Firewall-Regeln](sql-database-configure-firewall-settings-tsql.md)

Informationen zum Firewallregeln auf Serverebene mit anderen Methoden finden Sie unter:

- [Konfigurieren Sie Azure SQL-Datenbank auf Serverebene Firewall-Regeln mit der Azure-portal](sql-database-configure-firewall-settings.md)
- [Konfigurieren Sie die REST-API mit Azure SQL-Datenbank auf Serverebene Firewall-Regeln](sql-database-configure-firewall-settings-rest.md)

Ein Lernprogramm zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten Azure-Portal](sql-database-get-started.md).
Hilfe zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank offene Quelle oder Drittanbietern finden Sie unter [Client Schnellstart - Beispiele zu SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Navigieren zu Datenbanken finden Sie unter [Zugriff und Anmeldung Sicherheit verwalten](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Sichern der Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbankmodul und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->
