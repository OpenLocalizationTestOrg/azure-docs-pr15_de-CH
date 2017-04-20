<properties
    pageTitle="Wie führen Sie Verwaltungsaufgaben, z. B. Administratorkennwort zurücksetzen | Microsoft Azure"
    description="Beschreibt, wie allgemeine Verwaltungsaufgaben in SQL-Datenbank. Administratorkennwort gewähren und Entziehen von Access beispielsweise zurücksetzen."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="Administratorkennwort zurücksetzen"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Zum Ausführen von Verwaltungsaufgaben wie das Administratorkennwort in Azure SQL-Datenbank zurücksetzen
Verwenden Sie weitere Schritte und Entfernen einer Azure SQL-Datenbank. Weitere Informationen finden Sie unter:

- [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md)
- [Die SQL-Datenbank sichern](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbankmodul und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Administratorkennwort für einen logischen Server zurücksetzen

- Klicken Sie in [Azure-Portal](https://portal.azure.com) auf **SQL Server**, wählen Sie den Server aus der Liste und klicken Sie dann auf **Kennwort zurücksetzen**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>Dazu beitragen, dass ausschließlich autorisierte IP Adressen können auf Server zugreifen
- Siehe [wie: Konfigurieren der Firewall für SQL-Datenbank](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>In der Datenbank enthaltenen Datenbankbenutzer erstellen
- Verwenden Sie die Anweisung [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) und [Enthaltenen Benutzer - macht Ihre Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx)sehen.

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>Enthaltene Benutzer mithilfe von Azure Active Directory authentifizieren
- Siehe [Herstellen einer Verbindung mit SQL-Datenbank mithilfe von Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>Zusätzliche Logins für hoch privilegierten Benutzern in virtuellen master Datenbank erstellen
- Verwenden Sie die Anweisung [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx) und Abschnitt Logins verwalten [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md) für weitere Details.
