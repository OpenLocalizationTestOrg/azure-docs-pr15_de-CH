<properties
    pageTitle="Azure SQL-Datenbank auf Serverebene Firewallregeln mithilfe der REST-API | Microsoft Azure"
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


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Konfigurieren Sie die REST-API mit Azure SQL-Datenbank auf Serverebene Firewall-Regeln


> [AZURE.SELECTOR]
- [Übersicht](sql-database-firewall-configure.md)
- [Azure-portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-Datenbank verwendet Firewall-Regeln zwischen Servern und Datenbanken. Server und Datenbank Firewall Settings Master oder eine Benutzerdatenbank können in der Azure SQL-Datenbankserver selektiv auf die Datenbank zugreifen.

> [AZURE.IMPORTANT] Um von Azure Verbindung zum Datenbankserver zu ermöglichen, müssen Azure Verbindungen aktiviert werden. Weitere Informationen zu Firewall-Regeln und ermöglicht Verbindungen von Azure finden Sie unter [Azure SQL-Datenbank-Firewall](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Azure-Cloud erstellen, müssen Sie einige zusätzliche TCP-Anschlüsse öffnen. Weitere Informationen finden Sie unter der **V12-SQL-Datenbank: außerhalb Vs in** Abschnitt Ports [über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Verwaltung auf Serverebene Firewallregeln über REST-API
1. Verwalten von Firewall-Regeln über REST-API muss authentifiziert werden. Informationen finden Sie im [Entwicklerhandbuch Bewilligung mit der Azure-Ressourcen-Manager-API](../resource-manager-api-authentication.md).
2. Auf Serverebene Regeln können, aktualisierten oder gelöschten mit REST-API erstellt werden

    Erstellen oder aktualisieren eine Firewallregel auf Serverebene, die PUT-Methode mit den folgenden:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Anfragetext

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Um eine bestehende Firewallregeln auf Serverebene zu entfernen, führen Sie die DELETE-Methode der folgende:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Verwalten von Firewall-Regeln mit der REST-API

* [Erstellen oder Aktualisieren der Firewall-Regel](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Firewall-Regel löschen](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Firewall-Regel erhalten](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Listen Sie alle Firewall-Regeln](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Nächste Schritte

Ein Artikel mit Transact-SQL-Server und Datenbank Firewall-Regeln zum Anzeigen Sie [Azure SQL-Datenbank konfigurieren Sie Server und Datenbank Firewallregeln mit T-SQL](sql-database-configure-firewall-settings-tsql.md) 

Für wie Artikel auf Serverebene Firewallregeln mit anderen Methoden finden Sie unter: 

- [Konfigurieren Sie Azure SQL-Datenbank auf Serverebene Firewall-Regeln mit der Azure-portal](sql-database-configure-firewall-settings.md)
- [Mithilfe von PowerShell Azure SQL-Datenbank auf Serverebene Firewall-Regeln konfigurieren](sql-database-configure-firewall-settings-powershell.md)

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

 
