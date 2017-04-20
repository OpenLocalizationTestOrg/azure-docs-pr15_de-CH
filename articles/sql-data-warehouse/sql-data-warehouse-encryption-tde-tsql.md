<properties
   pageTitle="Transparente Verschlüsselung im SQL Datawarehouse (T-SQL) | Microsoft Azure"
   description="Transparente Verschlüsselung (DSA) im SQL Datawarehouse (T-SQL)"
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

# <a name="get-started-with-transparent-data-encryption-tde"></a>Erste Schritte mit Transparent Data Encryption (DSA)


> [AZURE.SELECTOR]
- [Übersicht über die Sicherheit](sql-data-warehouse-overview-manage-security.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
- [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Erforderliche Berechtigungen

Um transparente Data Encryption (DSA) zu aktivieren, müssen Sie ein Administrator oder ein Mitglied der Rolle Dbmanager sein.

## <a name="enabling-encryption"></a>Aktivieren der Verschlüsselung

Gehen Sie ein SQL Data Warehouse TDE aktivieren

1. Die *master* -Datenbank auf dem Server die Datenbank Anmeldung, die ein Administrator oder ein Mitglied der Rolle **Dbmanager** in der master-Datenbank herstellen
2. Führen Sie die folgende Anweisung um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung

Gehen einer SQL Data Warehouse TDE deaktivieren

1. Verbinden Sie mit der *master* -Datenbank eine Anmeldung, die ein Administrator oder ein Mitglied der Rolle **Dbmanager** in der master-Datenbank
2. Führen Sie die folgende Anweisung um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Eine angehaltene SQL Data Warehouse muss vor der Änderung auf den TDE fortgesetzt werden.

## <a name="verifying-encryption"></a>Überprüfen der Verschlüsselung

Gehen Sie folgendermaßen vor um Verschlüsselung für ein SQL Data Warehouse zu überprüfen:

1. Verbinden Sie mit der Datenbank *master* oder Instanz eine Anmeldung, die ein Administrator oder ein Mitglied der Rolle **Dbmanager** in der master-Datenbank
2. Führen Sie die folgende Anweisung um die Datenbank zu verschlüsseln.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Aus ```1``` gibt eine verschlüsselte Datenbank ```0``` gibt eine nicht verschlüsselte Datenbank an.

## <a name="encryption-dmvs"></a>Verschlüsselung DMVs  

- [Sys.Databases][] 
- [Sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[Sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[Sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
