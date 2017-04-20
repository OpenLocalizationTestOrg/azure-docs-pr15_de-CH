<properties
   pageTitle="Transparente Verschlüsselung im SQL Datawarehouse (Portal) | Microsoft Azure"
   description="Transparente Verschlüsselung (DSA) im SQL Datawarehouse"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Erste Schritte mit Transparent Data Encryption (DSA) im SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht über die Sicherheit](sql-data-warehouse-overview-manage-security.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
- [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Erforderliche Berechtigungen

Um transparente Data Encryption (DSA) zu aktivieren, müssen Sie ein Administrator oder ein Mitglied der Rolle Dbmanager sein.

## <a name="enabling-encryption"></a>Aktivieren der Verschlüsselung

Gehen Sie folgendermaßen vor um TDE für ein SQL Data Warehouse zu aktivieren:

1. Öffnen Sie die Datenbank in [Azure-portal](https://portal.azure.com)
2. Blatt Datenbank klicken Sie auf **die Schaltfläche**
3. Wählen Sie die Option **transparente Verschlüsselung**![][1]
4. Wählen Sie die Einstellung **auf**![][2]
5. Wählen Sie **Speichern**
![][3]  

## <a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung

Gehen Sie folgendermaßen vor um TDE für ein SQL Data Warehouse zu deaktivieren:

1. Öffnen Sie die Datenbank in [Azure-portal](https://portal.azure.com)
2. Blatt Datenbank klicken Sie auf **die Schaltfläche**
3. Wählen Sie die Option **transparente Verschlüsselung**![][1]
4. Wählen Sie die Einstellung **Deaktivieren**![][4]
5. Wählen Sie **Speichern**
![][5]  

## <a name="encryption-dmvs"></a>Verschlüsselung DMVs

Verschlüsselung kann mit der folgenden DMVs bestätigt:

- [Sys.Databases]
- [Sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[Sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[Sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
