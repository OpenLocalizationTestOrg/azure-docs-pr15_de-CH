<properties
   pageTitle="Eine Übersicht über SQL Server-Firewall konfigurieren | Microsoft Azure"
   description="So konfigurieren Sie die Firewall eine SQL-Datenbank mit Server und Datenbank Firewall den Zugriff verwalten."
   keywords="Datenbank-firewall"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Konfigurieren von Firewallregeln Azure SQL-Datenbank \- Überblick


> [AZURE.SELECTOR]
- [Übersicht](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-Datenbank bietet eine relationale Datenbank Azure und andere Internet-basierte Programme. Zum Schutz Ihrer Daten zu verhindern, dass Firewalls Zugriff auf dem Datenbankserver bis Sie angeben, welche Computer berechtigt. Die Firewall gewährt Zugriff auf Datenbanken basierend auf die ursprüngliche IP-Adresse jeder Anforderung.

Konfigurieren Sie Ihre Firewall erstellen Sie Firewallregeln, die zulässigen IP-Adressbereichen angeben. Firewall-Regeln können Server und Datenbank.

- **Firewall-Regeln auf Serverebene:** Diese Regeln können Kunden den gesamten SQL Azure-Server zugreifen, also alle Datenbanken in der gleichen logischen Server. Diese Regeln werden in der **master** -Datenbank gespeichert. Auf Serverebene Firewallregeln können über das Portal oder Transact-SQL-Anweisungen konfiguriert werden.
- **Auf Datenbankebene Firewallregeln:** Diese Regeln können Clients Zugriff auf einzelne Datenbanken innerhalb Ihrer Azure SQL-Datenbankserver. Diese Regeln für jede Datenbank erstellen und sie in die einzelnen Datenbanken gespeichert werden. (Sie können auf Datenbankebene Firewallregeln für die **master** -Datenbank erstellen.) Diese Regeln können beim Einschränken des Zugriffs auf bestimmte (sicheren) Datenbanken auf demselben logischen Server sein. Auf Datenbankebene Firewallregeln können nur mithilfe von Transact-SQL-Anweisungen konfiguriert werden.

**Empfehlung:** Microsoft empfiehlt die Verwendung der Datenbankebene Firewallregeln möglichst Sicherheit zu erhöhen und die Datenbank leichter. Verwenden Sie auf Serverebene Firewallregeln für Administratoren und viele Datenbanken mit den gleichen zugriffsanforderungen als Zeit jede Datenbank einzeln konfigurieren möchten.


## <a name="firewall-overview"></a>Firewall (Übersicht)

Zunächst wird alle Transact-SQL auf Ihrem SQL Azure-Server durch die Firewall blockiert. Zunächst mit Ihrem SQL Azure-Server Sie zum Azure-Portal und mindestens auf Serverebene Firewallregeln, die Zugang zu Ihrem SQL Azure-Server angeben. Verwenden Sie Firewall-Regeln an die IP-Adressbereiche aus dem Internet zulässig sind und ob Azure Applications versuchen die Verbindung zu Ihrem SQL Azure-Server.

Um Zugriff auf eine der Datenbanken in Ihrem SQL Azure-Server selektiv gewähren, legen Sie eine Regel auf Datenbankebene für die Datenbank erforderlich. Geben Sie einen IP-Adressbereich für die Datenbank Firewallregel, die über den IP-Adressbereich der Serverebene Firewallregel angegeben ist, und sicherstellen Sie, dass die IP-Adresse des Clients in der Regel auf Datenbankebene angegebenen Bereich fällt.

Verbindungsversuche von Internet und Azure müssen zuerst den Firewall passieren, bevor sie Ihre Azure SQL Server oder SQL-Datenbank zugreifen können wie im folgenden Diagramm dargestellt.

   ![Diagramm zur Beschreibung der Firewallkonfiguration.][1]

## <a name="connecting-from-the-internet"></a>Verbindung über das Internet

Wenn ein Computer versucht, die Verbindung zum Datenbankserver über das Internet, prüft die Firewall zuerst die ursprüngliche IP-Adresse der Anforderung mit den vollständigen Satz von Firewall-Regeln:

- Die IP-Adresse der Anforderung innerhalb eines der Serverebene Firewallregeln angegeben ist, erhält die Verbindung mit dem Azure SQL-Datenbankserver.

- Wenn die IP-Adresse der Anforderung nicht innerhalb eines der Serverebene Firewallregel angegeben ist, werden auf Datenbankebene Firewall-Regeln überprüft. Ist die IP-Adresse der Anforderung innerhalb eines der in der Datenbankebene Firewallregeln angegeben, erhält die Verbindung zur Datenbank, die auf Datenbankebene Übereinstimmungsregel.

- Wenn die IP-Adresse der Anforderung nicht innerhalb der Bereiche in der Server- oder Datenbank Firewallregeln Verbindung Anforderung wird angegeben.

> [AZURE.NOTE] Zugriff auf Azure SQL-Datenbank vom lokalen Computer sicher, dass die Firewall im Netzwerk und Rechner ausgehende Kommunikation über TCP-Port 1433 zulässt.


## <a name="connecting-from-azure"></a>Verbinden von Azure

Wenn eine Anwendung von Azure versucht die Verbindung zum Datenbankserver, die Firewall prüft, ob Azure Verbindungen zulässig sind. Eine Firewall mit Start- und Endadresse gleich 0.0.0.0 gibt an, dass diese Verbindungen zulässig sind. Wenn der Verbindungsversuch nicht zulässig ist, erreicht die Anforderung nicht Azure SQL-Datenbankserver.

> [AZURE.IMPORTANT] Diese Option konfiguriert den Firewall für alle Verbindungen von Azure einschließlich Verbindungen von Abonnements für andere Kunden. Bei Auswahl dieser Option stellen Sie sicher, dass Ihre Anmeldung und Berechtigungen Zugriff auf autorisierte Benutzer beschränken.

Sie können Verbindungen von Azure auf zwei Arten:

- Aktivieren Sie das Kontrollkästchen **Zulassen Azure Services Access-Server** [Microsoft Azure-Portal](https://portal.azure.com/)beim Erstellen eines neuen Servers.

- Klicken Sie auf **Ja** für **Microsoft Azure Services**, im [klassischen Portal](http://go.microsoft.com/fwlink/p/?LinkID=161793)aus der Registerkarte **Konfigurieren** auf einem Server unter dem Abschnitt **-Dienste zulässig** .

## <a name="creating-the-first-server-level-firewall-rule"></a>Die erste Serverebene Firewallregel erstellen

Die erste Firewall auf Serverebene Einstellung kann mithilfe der [Azure-Portal](https://portal.azure.com/) oder programmgesteuert mithilfe von REST-API oder Azure PowerShell erstellt werden. Nachfolgende Serverebene Firewallregeln können erstellt und verwaltet diese Methoden durch Transact-SQL. Zur Leistungssteigerung werden auf Serverebene Firewallregeln auf Datenbankebene vorübergehend zwischengespeichert. Zum Aktualisieren des Caches finden Sie unter [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Informationen auf Serverebene Firewall-Regeln finden Sie unter [wie: konfigurieren eine Azure SQL Serverfirewall mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Erstellen von Firewallregeln auf Datenbankebene

Nachdem Sie die erste Firewall auf Serverebene konfiguriert haben, sollten Sie den Zugriff auf bestimmte Datenbanken. Wenn Sie einen IP-Adressbereich der Datenbankebene Firewallregel, die außerhalb des Bereichs der Serverebene Firewallregel angegeben ist angeben, können nur Clients, die IP-Adressen im Bereich Datenbank auf die Datenbank zugreifen. Sie können maximal 128 auf Datenbankebene Firewall-Regeln für eine Datenbank. Auf Datenbankebene Firewallregeln für Master- und Datenbanken können erstellt und verwaltet über Transact-SQL. Weitere Informationen zum Konfigurieren der Datenbankebene Firewall-Regeln finden Sie unter [Sp_set_database_firewall_rule (Azure SQL-Datenbanken)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Programmgesteuertes Verwalten von Firewall-Regeln

Neben der Azure-Portal können Firewallregeln programmgesteuert mithilfe von Transact-SQL, REST-API und Azure PowerShell verwaltet werden. Die folgenden Tabellen beschreiben die Befehle für jede Methode.


### <a name="transact-sql"></a>Transact-SQL

| Katalogansicht oder gespeicherte Prozedur                                                           | Ebene     | Beschreibung                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [Sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Server    | Zeigt die aktuellen Serverebene Firewallregeln     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Server    | Erstellt oder aktualisiert auf Serverebene Firewall-Regeln       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Server    | Entfernt auf Serverebene Firewall-Regeln                  |
| [Sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Datenbank  | Zeigt die aktuellen Datenbankebene Firewallregeln   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Datenbank  | Erstellt oder aktualisiert die Datenbankebene Firewallregeln |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Datenbanken | Entfernt auf Datenbankebene Firewall-Regeln                |

### <a name="rest-api"></a>REST-API

| API                  | Ebene  | Beschreibung                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Firewall-Regeln](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Server | Zeigt die aktuellen Serverebene Firewallregeln                 |
| [Firewall-Regel erstellen](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Server | Erstellt oder aktualisiert auf Serverebene Firewall-Regeln                   |
| [Firewall-Regel festlegen](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Server | Aktualisiert die Eigenschaften einer vorhandenen Ebene Server Firewall-Regel |
| [Firewall-Regel löschen](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Server | Entfernt auf Serverebene Firewall-Regeln                              |


### <a name="azure-powershell"></a>Azure PowerShell

| Cmdlets                                                                                              | Ebene  | Beschreibung                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [AzureSqlDatabaseServerFirewallRule abrufen](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Server | Gibt die aktuelle Serverebene Firewallregeln                  |
| [Neue AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Server | Erstellt eine neue Firewallregel auf Serverebene                         |
| [AzureSqlDatabaseServerFirewallRule festlegen](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Server | Aktualisiert die Eigenschaften einer vorhandenen Ebene Server Firewall-Regel |
| [AzureSqlDatabaseServerFirewallRule entfernen](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Server | Entfernt auf Serverebene Firewall-Regeln                              |

> [AZURE.NOTE] Möglich wie fünf Minuten Verzögerung Firewall Einstellungen wirksam.

## <a name="troubleshooting-the-database-firewall"></a>Problembehandlung für Datenbank-firewall

Berücksichtigen Sie Folgendes beim Zugriff auf den Dienst Microsoft Azure SQL-Datenbank nicht so verhält wie erwartet:

- **Lokale Firewall-Konfiguration:** Vor der Computers Azure SQL-Datenbank zugreifen kann, müssen Sie eine Firewallausnahme auf Ihrem Computer für TCP-Port 1433 erstellen. Wenn Sie Verbindungen innerhalb der Azure-Cloud erstellen, müssen Sie zusätzliche Ports öffnen. Weitere Informationen finden Sie unter der **V12-SQL-Datenbank: außerhalb Vs in** Abschnitt Ports [über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Netzwerkadressübersetzung (NAT):** Durch NAT möglicherweise die IP-Adresse von Ihrem Computer zum Azure SQL-Datenbank herstellen anders als die IP-Adresse in Ihrem Computer IP-Konfiguration angezeigt. Um die IP-Adresse des Computers mit Azure herstellen anzuzeigen, melden Sie sich beim Portal und navigieren Sie zur Registerkarte " **Konfigurieren** " auf dem Server, der die Datenbank hostet. Im Abschnitt **IP-Adressen zulässig** ist die **Aktuelle IP-Adresse** angezeigt. Klicken Sie auf **Hinzufügen** in **Zulässige IP-Adressen** zu diesem Computer Zugriff auf den Server.

- **Ändert die Zulassungsliste nicht übernommen wurden noch:** Möglicherweise als fünf Minuten Verzögerung für die Firewallkonfiguration Azure SQL-Datenbank zu ändern.

- **Darf nicht der Benutzernamen oder ein falsches Kennwort verwendet wurde:** Eine Anmeldung ist nicht berechtigt auf Azure SQL-Datenbankserver oder das Kennwort ist falsch, wird die Verbindung zum Azure SQL-Datenbankserver verweigert. Erstellen einer firewalleinstellung gibt nur Kunden versuchen, mit dem Server verbinden. Jeder Client muss die erforderlichen Anmeldeinformationen bereitstellen. Weitere Informationen zum Vorbereiten der Benutzernamen finden Sie unter Verwalten von Datenbanken, Benutzernamen und Benutzer in Azure SQL-Datenbank.

- **Dynamische IP-Adresse:** Wenn Sie eine Internet-Verbindung mit dynamischen IP-Adressen und Probleme werden durch die Firewall, könnten Sie eine Folgendes versuchen:

 - Bitten Sie Internetdienstanbieter (ISP) für den IP-Adressbereich zugewiesen an die Clientcomputer, die Azure SQL-Datenbankserver zugreifen, und fügen Sie den IP-Adressbereich als eine Firewall-Regel.

 - Erhalten Sie statische IP-Adressen sich stattdessen für Client-Computer zu, und fügen Sie die IP-Adressen als Firewall-Regeln.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu Artikeln zum Erstellen von Server und Datenbank Firewall-Regeln finden Sie unter:

- [Konfigurieren Sie Azure SQL-Datenbank auf Serverebene Firewall-Regeln mit der Azure-portal](sql-database-configure-firewall-settings.md)
- [Mit T-SQL Azure SQL-Datenbank-Server und Datenbank Firewall-Regeln konfigurieren](sql-database-configure-firewall-settings-tsql.md)
- [Mithilfe von PowerShell Azure SQL-Datenbank auf Serverebene Firewall-Regeln konfigurieren](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurieren Sie die REST-API mit Azure SQL-Datenbank auf Serverebene Firewall-Regeln](sql-database-configure-firewall-settings-rest.md)

Ein Lernprogramm zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten Azure-Portal](sql-database-get-started.md).
Hilfe in Verbindung mit einer Azure SQL-Datenbank von open Source oder Drittanbietern finden Sie unter [Client Schnellstart - Beispiele zu SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Navigieren zu Datenbanken finden Sie unter [Zugriff und Anmeldung Sicherheit verwalten](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Sichern der Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbankmodul und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
