<properties
   pageTitle="Transparente Verschlüsselung (DSA) für SQL Server Stretch Datenbank in Azure TSQL aktivieren | Microsoft Azure"
   description="Aktivieren Sie transparente Verschlüsselung (DSA) für SQL Server Stretch Datenbank in Azure TSQL"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Aktivieren Sie transparente Verschlüsselung (DSA) für Stretch Datenbank auf Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Azure-portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Transparent Data Encryption (DSA) schützt gegen bösartige Aktivitäten durch Echtzeit-Verschlüsselung und Entschlüsselung der Datenbank zugeordneten Backups und Transaktionsprotokolldateien ruhende ohne Änderung der Anwendung ausführen.

TDE verschlüsselt die Speicherung der gesamten Datenbank einen symmetrischen Schlüssel Chiffrierschlüssel der Datenbank bezeichnet. Integrierte Serverzertifikat Datenbank-Verschlüsselungsschlüssel geschützt. Das integrierte Zertifikat ist eindeutig für jede Azure-Server. Microsoft wird diese Zertifikate automatisch mindestens alle 90 Tage. Eine allgemeine Beschreibung der TDE finden Sie [Transparent Data Encryption (DSA)].

##<a name="enabling-encryption"></a>Aktivieren der Verschlüsselung

Um eine Azure TDE aktivieren, das Speichern von Daten migriert Stretch-fähigen SQL Server-Datenbank gehen Sie folgendermaßen vor:

1. Die *master* -Datenbank auf dem Azure Server Datenbank Anmeldung, die ein Administrator oder ein Mitglied der Rolle **Dbmanager** in der master-Datenbank herstellen
2. Führen Sie die folgende Anweisung um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung

Deaktivieren TDE für eine Azure-Datenbank, die die Daten speichert Stretch-fähigen SQL Server-Datenbank migriert, gehen Sie folgendermaßen vor:

1. Verbinden Sie mit der *master* -Datenbank eine Anmeldung, die ein Administrator oder ein Mitglied der Rolle **Dbmanager** in der master-Datenbank
2. Führen Sie die folgende Anweisung um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Überprüfen der Verschlüsselung

Um sicherzustellen, dass Verschlüsselung eine Azure-Datenbank, die Daten Speichern von Stretch-fähigen SQL Server-Datenbank migriert, gehen Sie folgendermaßen vor:

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


<!--Anchors-->
[Transparente Verschlüsselung (DSA)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
