<properties
   pageTitle="Sichern einer Datenbank im SQL Data Warehouse | Microsoft Azure"
   description="Tipps zum Sichern einer Datenbank in Azure SQL Data Warehouse Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>Sichern einer Datenbank im SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht über die Sicherheit](sql-data-warehouse-overview-manage-security.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
- [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Dieser Artikel führt die Grundlagen der Azure SQL Data Warehouse-Datenbank sichern. Insbesondere in diesem Artikel erhalten Sie Ressourcen zur Einschränkung des Zugriffs, Datenschutz und Überwachung von Aktivitäten in einer Datenbank.

## <a name="connection-security"></a>Verbindungssicherheit

Wie einschränken und Verbindung mit der Datenbank mithilfe von Firewall-Regeln und Verbindung Verschlüsselung Connection Security gemeint.

Firewall-Regeln wird durch den Server und die Datenbank Verbindungsversuche von IP-Adressen, die nicht explizit zugelassenen ablehnen. Ermöglicht Verbindungen aus der Anwendung oder des Client-Computers öffentliche IP-Adresse muss mit der Azure-Portal, REST-API oder PowerShell eine Firewallregel auf Serverebene erstellen. Als bewährte Methode sollten Sie die IP-Adressbereiche passieren der Serverfirewall möglichst einschränken.  Zugriff auf Azure SQL Data Warehouse vom lokalen Computer sicher, dass die Firewall im Netzwerk und Rechner ausgehende Kommunikation über TCP-Port 1433 zulässt.  Weitere Informationen finden Sie unter [Firewall Azure SQL-Datenbank][], [Sp_set_firewall_rule][]und [Sp_set_database_firewall_rule][].

Verbindung mit SQL Data Warehouse sind standardmäßig verschlüsselt.  Ändern von Einstellungen Verschlüsselung deaktivieren werden ignoriert.

## <a name="authentication"></a>Authentifizierung

Authentifizierung bezeichnet, wie Ihre Identität, beim Verbinden mit der Datenbank nachzuweisen. Derzeit unterstützt SQL Data Warehouse SQL Server-Authentifizierung mit Benutzername und Kennwort sowie einem Azure Active Directory. 

Logischen Server für die Datenbank erstellt, wird "serveradmin" Anmeldung mit Benutzername und Kennwort angegeben. Diese Anmeldeinformationen verwenden, können Sie mit einer Datenbank auf diesem Server als Datenbankbesitzer oder "Dbo" über die SQL Server-Authentifizierung authentifizieren.

Jedoch empfohlen verwenden Benutzer in Ihrem Unternehmen ein anderes Konto um zu authentifizieren. Sie können die Berechtigungen für die Anwendung beschränken und Risiken eines unberechtigten Anwendungscode SQL-Injection-Angriff anfällig ist. 

Erstellen Sie einen SQL Server authentifizierte Benutzer auf die **master** -Datenbank auf dem Server mit Ihrem Server administratoranmeldung und erstellen Sie eine neue Server-Anmeldung.  Darüber hinaus ist es sollten Benutzer in der master-Datenbank Azure SQL Data Warehouse-Benutzer erstellen. Erstellen eines Benutzers in Master kann Benutzer sich mit Tools wie SSMS ohne einen Datenbanknamen.  Außerdem können sie mit Objekt-Explorer an alle Datenbanken auf einem SQL Server.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Dann mit der **SQL Data Warehouse-Datenbank** mit Ihrem Server administratoranmeldung und Erstellen eines Datenbankbenutzers basierend auf Server-Benutzername, den Sie gerade erstellt haben.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Wenn ein Benutzer zusätzliche Vorgänge wie Benutzernamen oder erstellen neue Datenbanken ausführen, müssen sie auch zugewiesen werden die `Loginmanager` und `dbmanager` Rollen in der master-Datenbank. Weitere Informationen über diese zusätzlichen Funktionen und Authentifizierung bei einer SQL-Datenbank finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank][].  Weitere Informationen zu Azure AD für SQL Data Warehouse finden Sie unter [Verbinden mit SQL Data Warehouse durch Verwendung von Azure Active Directory-Authentifizierung][].


## <a name="authorization"></a>Autorisierung

Autorisierung bezieht sich auf eine Azure SQL Data Warehouse-Datenbank möglich, und dies wird durch die Rollenmitgliedschaften Ihres Benutzerkontos und Berechtigungen gesteuert. Als bewährte Methode sollten Sie Benutzer über die mindestens erforderlichen Berechtigungen erteilen. Azure SQL Data Warehouse dadurch die Verwaltung mit T-SQL:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Das Verbinden mit Server Admin-Konto ist ein Mitglied von Db_owner die Behörde keine in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von anderen Verwaltungsvorgänge und Schema-Upgrades. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen mit den geringsten Berechtigungen Ihre Anwendung benötigt eine Verbindung aus der Anwendung an die Datenbank.

Es gibt Methoden weiter einschränken tun kann Benutzer Azure SQL-Datenbank:

- Abgestufte [Berechtigungen][] können Sie steuern, welche Vorgänge Sie auf einzelne Spalten, Tabellen, Ansichten, Prozeduren und andere Objekte in der Datenbank. Verwenden Sie abgestufte Berechtigungen haben die größte Kontrolle und die minimalen erforderlichen Berechtigungen. Die detaillierte Rechte ist etwas kompliziert und erfordert einige Informationen zu nutzen.
- [Datenbankrolle][] Db_datareader und Db_datawriter kann verwendet werden, leistungsfähiger Anwendungsbenutzerkonten oder weniger leistungsfähige Management-Konten erstellen. Integrierte festen Datenbankrollen bieten eine einfache Möglichkeit, Berechtigungen, jedoch können mehr Berechtigungen als notwendig.
- [Gespeicherte Prozeduren][] können verwendet werden, die Aktionen einschränken, die in der Datenbank ausgeführt werden können.

Verwalten von Datenbanken und logische Server über Azure-Verwaltungsportal oder mithilfe der Azure-Ressourcen-Manager-API Ihr Portal Benutzerkonto Rolle Arbeitsaufträge gesteuert. Weitere Informationen zu diesem Thema finden Sie unter [Rollenbasierte Zugriffskontrolle in Azure-Portal][].

## <a name="encryption"></a>Verschlüsselung

Azure SQL Data Warehouse Transparent Data Encryption (TDE) schützt gegen bösartige Aktivitäten in Echtzeit Verschlüsselung und Entschlüsselung der Daten.  Wenn Sie Ihre Datenbank verschlüsseln, werden zugeordneten Backups und Transaktionsprotokolldateien ohne jede Änderung der Anwendung verschlüsselt. TDE verschlüsselt die Speicherung der gesamten Datenbank einen symmetrischen Schlüssel Chiffrierschlüssel der Datenbank bezeichnet. SQL-Datenbank der Datenbankverschlüsselungsschlüssel integrierte Serverzertifikat geschützt. Das integrierte Zertifikat ist eindeutig für jeden SQL-Datenbankserver. Microsoft wird diese Zertifikate automatisch mindestens alle 90 Tage. SQL Data Warehouse verwendete Verschlüsselungsalgorithmus ist AES-256. Eine allgemeine Beschreibung der TDE finden Sie [Transparente Verschlüsselung][].

Verschlüsseln Sie Ihre Datenbank mit der [Azure-Portal] [ Encryption with Portal] oder [T-SQL-][Encryption with TSQL].

## <a name="next-steps"></a>Nächste Schritte

Informationen und Beispiele zum Verbinden mit SQL Data Warehouse mit unterschiedlichen Protokollen finden Sie unter [Verbinden mit SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Verbinden Sie mit SQL Datawarehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Herstellen einer Verbindung mit SQL Datawarehouse Azure Active Directory-Authentifizierung]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL-Datenbank-firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Datenbankrollen]: https://msdn.microsoft.com/library/ms189121.aspx
[Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank]: https://msdn.microsoft.com/library/ee336235.aspx
[Berechtigungen]: https://msdn.microsoft.com/library/ms191291.aspx
[Gespeicherte Prozeduren]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparente Verschlüsselung]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Rollenbasierte Zugriffskontrolle in Azure-Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
