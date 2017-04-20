<properties
   pageTitle="Authentifizierung in SQL Azure Datawarehouse | Microsoft Azure"
   description="Azure Active Directory (AAD) und SQL Server-Authentifizierung Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Authentifizierung in SQL Azure Datawarehouse

> [AZURE.SELECTOR]
- [Übersicht über die Sicherheit](sql-data-warehouse-overview-manage-security.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
- [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Verbindung zu SQL Data Warehouse müssen Sie Anmeldeinformationen für die Authentifizierung übergeben. Beim Herstellen einer Verbindung werden bestimmte Einstellungen als Teil der Abfrage Sitzung konfiguriert.  

Weitere Informationen zu Sicherheit und Aktivieren der Verbindung zum Datawarehouse finden Sie unter [sichere Datenbank im SQL Data Warehouse][].

## <a name="sql-authentication"></a>SQL-Authentifizierung
Die Verbindung mit SQL Data Warehouse müssen Sie Folgendes angeben:

- Vollqualifizierte servername
- Geben Sie SQL-Authentifizierung
- Benutzername
- Kennwort
- Standarddatenbank (optional)

Standardmäßig wird die Verbindung mit der *master* -Datenbank und nicht Ihre Datenbank. Um mit der Datenbank verbinden, können Sie Folgendes tun:

- Geben Sie die Standarddatenbank beim Registrieren des Servers mit SQL Server Objekt-Explorer SSDT SSMS, oder in der Verbindungszeichenfolge der Anwendung. Beispielsweise InitialCatalog-Parameter für eine ODBC-Verbindung.
- Markieren Sie die Datenbank vor dem Erstellen einer Sitzung in SSDT.

> [AZURE.NOTE] Die Transact-SQL-Anweisung **Verwenden MyDatabase;** wird zum Ändern der Datenbank für eine Verbindung nicht unterstützt. Verbinden mit SQL Data Warehouse mit SSDT Anleitung finden Sie im Artikel [Abfrage mit Visual Studio][] .

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD) Authentifizierung

[Azure Active Directory] [ What is Azure Active Directory] -Authentifizierung ist ein Mechanismus für mit Microsoft Azure SQL Data Warehouse mit Identitäten in Azure Active Directory (Azure AD). Azure Active Directory-Authentifizierung können Sie zentral die Identitäten der Benutzer und anderen Microsoft-Diensten an einem zentralen Ort verwalten. Zentrale ID-Verwaltung bietet eine zentrale Stelle zum Verwalten von Benutzern SQL Data Warehouse und Berechtigungsmanagement vereinfacht. 

### <a name="benefits"></a>Vorteile

Azure Active Directory bietet folgende Vorteile:

- Stellt eine Alternative zur SQL Server-Authentifizierung.
- Stoppt die Verbreitung von Benutzeridentitäten auf Datenbankserver.
- Ermöglicht Kennwortrotation an einem einzigen Ort
- Verwalten Sie Datenbankberechtigungen externe (AAD) Gruppen.
- Speichern von Kennwörtern wird integrierte Windows-Authentifizierung und andere Formen der Authentifizierung von Azure Active Directory unterstützt.
- Benutzer zur Authentifizierung von Identitäten auf Datenbankebene verwendet enthalten.
- Token-basierte Authentifizierung unterstützt für Applikationen mit SQL Data Warehouse.
- Unterstützt Multi-Authentifizierung durch universelle Active Directory-Authentifizierung für SQL Server Management Studio. Eine mehrstufige Authentifizierung Siehe [SSMS Unterstützung für Azure AD MFA SQL-Datenbank mit SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure Active Directory ist noch relativ neu und hat einige Nachteile. Um sicherzustellen, dass Azure Active Directory für Ihre Umgebung geeignet ist, finden Sie unter [Azure AD-Funktionen und Grenzen][], insbesondere zusätzliche.

### <a name="configuration-steps"></a>Konfigurationsschritte

Gehen Sie Azure Active Directory-Authentifizierung konfigurieren.

1. Erstellen und Auffüllen von Azure Active Directory
2. Optional: Ordnen oder Ändern von active Directory mit der Azure-Abonnement verknüpft ist
3. Erstellen Sie Administrator Azure Active Directory Azure SQL Data Warehouse.
4. Konfigurieren der Clientcomputer
5. Erstellen Sie enthaltene Benutzer in der Datenbank zugeordnet Azure AD Identitäten
6. Verbinden Sie mit Datawarehouse mithilfe von Azure AD-Identitäten

Azure Active Directory-Benutzer sind derzeit nicht im SSDT Objekt-Explorer angezeigt. Um dieses Problem zu umgehen wird zeigen Sie der Benutzer in [Kataloganzeige an](https://msdn.microsoft.com/library/ms187328.aspx).
  
### <a name="find-the-details"></a>Die Details finden
- Die detaillierten Schritte. Die einzelnen Schritte zum Konfigurieren und Verwenden von Azure Active Directory-Authentifizierung sind fast identisch für Azure SQL-Datenbank und Azure SQL Data Warehouse. Führen Sie die einzelnen Schritte im Thema [Verbinden mit SQL-Datenbank oder SQL Data Warehouse durch Verwendung von Azure Active Directory-Authentifizierung](../sql-database/sql-database-aad-authentication.md).
- Benutzerdefinierte Datenbankrollen erstellen und Hinzufügen von Benutzern zu Rollen. Weisen Sie präzise Berechtigungen für Rollen. Weitere Informationen finden Sie in [Modul Datenbankberechtigungen Einstieg](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Nächste Schritte

Starten das Datawarehouse mit Visual Studio und weitere Abfragen finden Sie unter [Abfrage mit Visual Studio][].

<!-- Article references -->
[Sichern einer Datenbank im SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Abfrage mit Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD-Funktionen und Grenzen]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
