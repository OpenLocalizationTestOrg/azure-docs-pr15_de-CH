<properties
   pageTitle="SQL Azure-Datenbank Sicherheits Richtlinien | Microsoft Azure"
   description="Informationen Sie zu Microsoft Azure SQL-Datenbank-Richtlinien und Einschränkungen Sicherheit."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Azure SQL-Datenbank Sicherheits Richtlinien

Dieses Thema beschreibt die Microsoft Azure SQL-Datenbank-Richtlinien und Einschränkungen Sicherheit. Beachten Sie die folgenden Punkte, wenn die Sicherheit der SQL Azure-Datenbanken verwalten.

## <a name="access-to-the-virtual-master-database"></a>Zugriff auf virtuelle master-Datenbank

Normalerweise benötigen nur Administratoren Zugriff auf die master-Datenbank. Routine auf jede Benutzerdatenbank sollte über Administratorkonten enthaltenen Benutzer in jeder Datenbank erstellt. Verwendung enthaltenen Benutzer brauchen Sie Benutzernamen in der master-Datenbank erstellen. Weitere Informationen finden Sie unter [Enthaltenen Benutzer - macht Ihre Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Firewall

Die SQL Server-Firewall für die gesamte Azure SQL Server ausgelegt wird normalerweise über das Portal konfiguriert und sollten nur von Administratoren verwendet IP-Adressen zulassen. Um eine Datenbank begrenzt Firewallregel erstellen, die erforderlichen IP-Adressen für jede Datenbank öffnet, Verbinden mit einer Datenbank und [Sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) Transact-SQL-Anweisung verwenden.

Der Dienst Azure SQL-Datenbank ist nur über TCP-Port 1433. Zugriff auf eine SQL-Datenbank von Ihrem Computer sicher, dass die Firewall Client Computer ausgehenden TCP-Kommunikation über TCP-Port 1433 zulässt. Ggf. nicht für andere Programme blockiert eingehende Verbindungen auf TCP-Port 1433. 

Als Teil des Verbindungsprozesses werden Verbindungen von Azure virtuelle Computer, andere IP-Adresse für jede Arbeitskraft Rolle eindeutiger Port umgeleitet. Die Portnummer ist zwischen 11000 und 11999. Weitere Informationen zu TCP-Ports finden Sie unter [Ports über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Authentifizierung

Active Directory-Authentifizierung verwenden (integrierte Sicherheit) Möglichkeit. Informationen zum Konfigurieren von AD-Authentifizierung finden Sie unter [Herstellen einer Verbindung mit SQL Datenbank durch Verwendung von Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md)und [Auswählen eines Authentifizierungsmodus](https://msdn.microsoft.com/library/ms144284.aspx) in SQL Server-Onlinedokumentation. 

Enthaltene Benutzer skalierbar zu verwenden. Weitere Informationen finden Sie unter [Enthaltenen Benutzer - macht Ihre Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx)und [Datenbanken enthalten](https://technet.microsoft.com/library/ff929071.aspx).

Das Datenbankmodul schließt Verbindungen, die mehr als 30 Minuten im Leerlauf. Die Verbindung wieder müssen sich anmelden, bevor sie verwendet werden kann.

Ständig aktive Verbindungen zu SQL-Datenbank Reauthorization (durchgeführt vom Datenbankmodul) erfordern alle 10 Stunden. Das Datenbankmodul versucht Reauthorization das ursprünglich Kennwort verwenden und keine Benutzereingaben erforderlich. Aus Gründen der Leistung beim Zurücksetzen des Kennworts in SQL-Datenbank die Verbindung ist nicht erneut authentifiziert werden, auch wenn die Verbindung durch Verbindungspooling zurückgesetzt wird. Dies unterscheidet sich vom Verhalten der lokalen SQL Server. Wenn das Kennwort geändert wurde, seit die Verbindung zunächst autorisiert wurde die Verbindung muss beendet und eine neue Verbindung hergestellt mit dem neuen Kennwort. Ein Benutzer mit der Berechtigung KILL-Verbindung kann eine Verbindung zur SQL-Datenbank mithilfe des Befehls [KILL](https://msdn.microsoft.com/library/ms173730.aspx) explizit beenden.

## <a name="logins-and-users"></a>Benutzernamen und Benutzern

Beim Verwalten von Benutzernamen und Benutzern in SQL-Datenbank sind eingeschränkt.


- Sie müssen mit der **master** -Datenbank verbunden sein, beim Ausführen der ``CREATE/ALTER/DROP DATABASE`` Aussagen. -Datenbankbenutzer in der master-Datenbank entspricht der Serverebene principal Benutzername kann nicht geändert oder gelöscht werden. 
- US-Englisch ist die Standardsprache der Serverebene principal anmelden.
- Nur Administratoren (Prinzipal auf Serverebene Anmeldung oder Azure AD Administrator) und das **Dbmanager** -Datenbankrolle in **der Masterdatenbank** die Berechtigung zum Ausführen der ``CREATE DATABASE`` und ``DROP DATABASE`` Aussagen.
- Sie müssen mit der master-Datenbank verbunden sein, beim Ausführen der ``CREATE/ALTER/DROP LOGIN`` Aussagen. Mithilfe von Benutzernamen wird jedoch abgeraten. Verwenden Sie enthaltene Benutzer.
- Zum Verbinden mit einer Datenbank müssen Sie den Namen der Datenbank in der Verbindungszeichenfolge angeben.
- Nur auf Serverebene principal Login und Mitglieder der Rolle der **Loginmanager** in der **master** -Datenbank die Berechtigung zum Ausführen der ``CREATE LOGIN``, ``ALTER LOGIN``, und ``DROP LOGIN`` Aussagen.
- Beim Ausführen der ``CREATE/ALTER/DROP LOGIN`` und ``CREATE/ALTER/DROP DATABASE`` Anweisungen in einer Anwendung mit ADO.NET parametrisierte Befehle ist nicht zulässig. Weitere Informationen finden Sie unter [Befehle und Parameter](https://msdn.microsoft.com/library/ms254953.aspx).
- Beim Ausführen der ``CREATE/ALTER/DROP DATABASE`` und ``CREATE/ALTER/DROP LOGIN`` Anweisungen dieser Aussagen muss die einzige Anweisung in einem Batch Transact-SQL. Andernfalls tritt ein Fehler auf. Die folgenden Transact-SQL überprüft z. B., ob die Datenbank vorhanden ist. Es existiert ein ``DROP DATABASE`` -Anweisung aufgerufen, um die Datenbank zu entfernen. Da die ``DROP DATABASE`` ist nicht die einzige Anweisung im Batch, die Ausführung der folgenden Transact-SQL-Anweisung führt zu einem Fehler.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Beim Ausführen der ``CREATE USER`` mit dem ``FOR/FROM LOGIN`` Option muss die einzige Anweisung in einem Batch Transact-SQL.
- Beim Ausführen der ``ALTER USER`` mit dem ``WITH LOGIN`` Option muss die einzige Anweisung in einem Batch Transact-SQL.
- Auf ``CREATE/ALTER/DROP`` Benutzer die ``ALTER ANY USER`` Berechtigungen für die Datenbank.
- Der folgende Fehler kann auftreten, wenn der Besitzer einer Datenbankrolle hinzufügen oder Entfernen eines anderen Datenbankbenutzers zu oder von dieser Datenbankrolle versucht,: **Benutzer oder die Rolle 'Name' ist in dieser Datenbank nicht vorhanden.** Dieser Fehler tritt auf, weil der Benutzer nicht der Besitzer angezeigt wird. Um dieses Problem zu beheben, gewähren Sie den Besitzer der ``VIEW DEFINITION`` Berechtigung für die Benutzer. 

Weitere Informationen zu Benutzernamen und Benutzer finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md).


## <a name="see-also"></a>Siehe auch

[SQL Azure-Datenbank Firewall](sql-database-firewall-configure.md)

[Gewusst wie: Konfigurieren der Firewall (Azure SQL-Datenbank)](sql-database-configure-firewall-settings.md)

[Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md)

[Sicherheitscenter für SQL Server-Datenbankmodul und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)
