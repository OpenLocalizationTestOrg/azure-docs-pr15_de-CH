<properties
    pageTitle="Lernprogramm für SQL: Erste Schritte mit Sicherheit"
    description="Informationen Sie zum Erstellen von Benutzerkonten Zugriff auf und das verwalten eine Datenbank."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>Lernprogramm für SQL: Benutzerkonten verwaltet und einer SQL-Datenbank erstellen


> [AZURE.SELECTOR]
- [Gestartete Get-Lernprogramm](sql-database-get-started-security.md)
- [Zugriff gewähren](sql-database-manage-logins.md)

In diesem Lernprogramm erfahren Sie, wie SQL Server Management Studio (SSMS) zu verwenden:

- Auf Serverebene principal Anmeldung SQL-Datenbank anmelden.
- Erstellen einer SQL-Datenbank-Benutzerkonto.
- Erteilen Sie eine SQL-Datenbank [Db_owner-Berechtigungen](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0).
- Verbinden Sie mit einer SQL-Datenbank mit einem Benutzerkonto, das kein Prinzipal auf Serverebene.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Nächste Schritte
Sie haben dieses Lernprogramm SQL abgeschlossen und erstellt ein Benutzerkonto und erteilt dem Benutzer Dbo Berechtigungen, können Sie weitere Informationen zu [SQL-Datenbank-Sicherheit](sql-database-manage-logins.md).


