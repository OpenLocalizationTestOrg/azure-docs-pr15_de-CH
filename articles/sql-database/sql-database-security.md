<properties
   pageTitle="Übersicht über die Sicherheit von SQL-Datenbank"
   description="Azure SQL-Datenbank und SQL Server-Sicherheit, einschließlich der Unterschiede zwischen der Cloud erfahren und SQL Server lokal bei Authentifizierung, Autorisierung, verbindungssicherheit, Verschlüsselung und Compliance."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>Die SQL-Datenbank sichern

## <a name="overview"></a>Übersicht

Dieser Artikel führt die Grundlagen der Datenebene einer Anwendung mithilfe von Azure SQL-Datenbank sichern. In diesem Artikel erhalten Sie Ressourcen zur Einschränkung des Zugriffs, Datenschutz, insbesondere in [Erste Schritte mit SQL Lernprogramm](sql-database-get-started.md)erstellt Aktivitäten in einer Datenbank überwacht. Eine vollständige Übersicht über Sicherheitsfunktionen für alle Arten von SQL finden Sie im [Security Center für SQL Server-Datenbankmodul und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589). Weitere Informationen sind auch in [Sicherheits- und Azure SQL-Datenbank Whitepaper](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF) verfügbar.

## <a name="connection-security"></a>Verbindungssicherheit

Wie einschränken und Verbindung mit der Datenbank mithilfe von Firewall-Regeln und Verbindung Verschlüsselung Connection Security gemeint.

Firewall-Regeln wird durch den Server und die Datenbank Verbindungsversuche von IP-Adressen, die nicht explizit zugelassenen ablehnen. Damit Ihre Anwendung oder öffentliche IP-Adresse des Client-Computers versucht, eine neue Datenbank verbinden, müssen Sie zuerst mithilfe der Azure-Verwaltungsportal REST-API oder PowerShell eine Firewallregel auf Serverebene erstellen. Als bewährte Methode sollten Sie die IP-Adressbereiche passieren der Serverfirewall möglichst einschränken. Weitere Informationen finden Sie in [Azure SQL-Datenbank-Firewall](https://msdn.microsoft.com/library/ee621782).

Alle Verbindungen mit Azure SQL-Datenbank Verschlüsselung (SSL/TLS) am jederzeit Daten in und aus der Datenbank "in Transit" ist. In der Verbindungszeichenfolge Ihrer Anwendung geben Sie Parameter, um die Verbindung verschlüsseln und *nicht* vertrauenswürdig dem Serverzertifikat (Dies erfolgt zur Verbindungszeichenfolge aus der Azure-Verwaltungsportal kopieren), andernfalls die Verbindung wird nicht die Identität des Servers anfällig für "Man-in-the-Middle"-Angriffe. ADO.NET-Treiber beispielsweise diese Parameter für Verbindungszeichenfolgen werden **Encrypt = True** und **TrustServerCertificate = False**. Weitere Informationen finden Sie unter [Azure SQL Datenbank Verbindung Verschlüsselung und Überprüfung des Zertifikats](https://msdn.microsoft.com/library/azure/ff394108#encryption).


## <a name="authentication"></a>Authentifizierung

Authentifizierung bezeichnet, wie Ihre Identität, beim Verbinden mit der Datenbank nachzuweisen. SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

 - **SQL-Authentifizierung**, die einen Benutzernamen und ein Kennwort
 - **Azure Active Directory-Authentifizierung**von Azure Active Directory verwaltet Identitäten und unterstützte für verwalteten und integrierte Domänen

Logischen Server für die Datenbank erstellt, wird "serveradmin" Anmeldung mit Benutzername und Kennwort angegeben. Diese Anmeldeinformationen verwenden, können Sie mit einer Datenbank auf diesem Server als Datenbankbesitzer oder "Dbo" authentifizieren Wenn Sie Azure Active Directory-Authentifizierung verwenden möchten, legen Sie einen anderen serveradmin bezeichnet die "Azure AD Admin" die Azure Active Directory-Benutzer und Gruppen verwalten. Dieser Administrator kann auch alle Vorgänge ausführen, die einen normalen serveradmin. Eine exemplarische Vorgehensweise zum Erstellen von Azure AD Administrator Azure Active Directory-Authentifizierung aktivieren finden Sie unter [Herstellen einer Verbindung mit SQL Datenbank durch Verwendung von Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) .

Als bewährte Methode sollte die Anwendung ein anderes Konto Authentifizierung so beschränken die Berechtigungen für die Anwendung und bösartige Aktivitäten bei der Anwendungscode SQL-Injection-Angriff anfällig Risiken – verwenden. Empfohlen wird eine [Datenbank enthalten](https://msdn.microsoft.com/library/ff929188), wodurch Ihre Anwendung eine einzelne Datenbank authentifizieren erstellen. Sie können einen eigenständigen Datenbankbenutzer erstellen mit SQL-Authentifizierung mit dem folgenden T-SQL-Befehl während der Datenbank als Server administratoranmeldung an:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Wenn Sie Azure AD Administrator erstellt, können Sie einen eigenständigen Datenbankbenutzer erstellen, der Azure Active Directory-Authentifizierung mit dem folgenden T-SQL-Befehl während der Datenbank als Azure AD Administrator an verwendet:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

In beiden Fällen sollten Verbindungszeichenfolge Ihrer Anwendung diese Anmeldeinformationen als Server Admin-Benutzernamen für die Verbindung zur Datenbank angeben.

Weitere Informationen zu Authentifizierung bei einer SQL-Datenbank finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md).


## <a name="authorization"></a>Autorisierung
Autorisierung bezieht sich auf eine Azure SQL-Datenbank möglich, und dies wird durch die Rollenmitgliedschaften Ihres Benutzerkontos und Berechtigungen gesteuert. Als bewährte Methode sollten Sie Benutzer über die mindestens erforderlichen Berechtigungen erteilen. Azure SQL-Datenbank erleichtert dies mit Rollen in T-SQL:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Das Verbinden mit Server Admin-Konto ist ein Mitglied von Db_owner die Behörde keine in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von anderen Verwaltungsvorgänge und Schema-Upgrades. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen mit den geringsten Berechtigungen Ihre Anwendung benötigt eine Verbindung aus der Anwendung an die Datenbank.

Es gibt Methoden weiter einschränken tun kann Benutzer Azure SQL-Datenbank:

* [Datenbankrolle](https://msdn.microsoft.com/library/ms189121) Db_datareader und Db_datawriter kann verwendet werden, leistungsfähiger Anwendungsbenutzerkonten oder weniger leistungsfähige Management-Konten erstellen.
* Abgestufte [Berechtigungen](https://msdn.microsoft.com/library/ms191291) können Sie steuern, welche Vorgänge Sie auf einzelne Spalten, Tabellen, Ansichten, Prozeduren und andere Objekte in der Datenbank.
* [Identitätswechsel](https://msdn.microsoft.com/library/vstudio/bb669087) und [Modul-Signatur](https://msdn.microsoft.com/library/bb669102) dienen, sichere Berechtigungen vorübergehend zu erhöhen.
* [Zeilenbasierter Sicherheit](https://msdn.microsoft.com/library/dn765131) dienen Limit einen Benutzer Zeilen zugreifen kann.
* [Maskierung der Daten](sql-database-dynamic-data-masking-get-started.md) kann verwendet werden, um vertrauliche Daten zu beschränken.
* [Gespeicherte Prozeduren](https://msdn.microsoft.com/library/ms190782) können verwendet werden, die Aktionen einschränken, die in der Datenbank ausgeführt werden können.

Verwalten von Datenbanken und logische Server über Azure-Verwaltungsportal oder mithilfe der Azure-Ressourcen-Manager-API Ihr Portal Benutzerkonto Rolle Arbeitsaufträge gesteuert. Weitere Informationen zu diesem Thema finden Sie unter [Rollenbasierte Zugriffskontrolle in Azure-Portal](../active-directory./role-based-access-control-configure.md).


## <a name="encryption"></a>Verschlüsselung

Azure SQL-Datenbank kann schützen Ihre Daten durch Verschlüsselung Ihrer Daten wird "at Rest" oder in Datenbank- und sichern, [Transparente Verschlüsselung](http://go.microsoft.com/fwlink/?LinkId=526242)gespeichert. Ihre Datenbank verschlüsselt, als Datenbankbesitzer und ausführen:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Dazu Ihre Geheimnisse Daten verschlüsseln sollten Sie:

* [Verschlüsselung Zelle auf](https://msdn.microsoft.com/library/ms179331.aspx) bestimmte Spalten oder Zellen Daten auch mit anderen Schlüssel verschlüsselt.
* Benötigen Sie ein Hardwaresicherheitsmodul oder die zentrale Verwaltung der Hierarchie für Verschlüsselungsschlüssel, erwägen Sie [Azure Key Vault mit SQL Server in eine Azure-VM](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Verschlüsselt](https://msdn.microsoft.com/library/mt163865.aspx) (in der Vorschau) ist Verschlüsselung Anwendung transparent und ermöglicht Clients beim Entschlüsseln sensibler Daten in Clientanwendungen ohne die Verschlüsselungsschlüssel mit SQL-Datenbank.

## <a name="auditing"></a>Überwachung

Überwachung und Verfolgung Datenbankereignisse können Sie Einhaltung behördlicher Auflagen und identifiziert verdächtigen Aktivitäten. SQL Datenbank Überwachung können Sie zum Aufzeichnen von Ereignissen in der Datenbank zu Azure Storage-Konto anmelden. SQL Datenbank Überwachung integriert auch Microsoft Power BI detaillierte Berichte und Analysen. Weitere Informationen finden Sie unter [Erste Schritte mit SQL Datenbank überwachen](sql-database-auditing-get-started.md).

SQL Datenbank Erkennung bietet eine zusätzliche Sicherheitsebene auf Überwachung. Sie können Risiken reagiert mit Sicherheitshinweisen auf ungewöhnliche Aktivitäten an. Weitere Informationen finden Sie unter [Erste Schritte mit SQL Datenbank Erkennung](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Compliance

Neben den Features und Funktionen, die die Anwendung verschiedener Security Compliance Azure SQL-Datenbank auch erfüllen, unterstützen reguläre Prüfungen beteiligt ist und mit einer Reihe von Sicherheitsstandards zertifiziert. Weitere Informationen finden Sie im [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/)die aktuelle Liste der [SQL-Datenbank Compliance-Zertifizierungen finden](https://azure.microsoft.com/support/trust-center/services/).
