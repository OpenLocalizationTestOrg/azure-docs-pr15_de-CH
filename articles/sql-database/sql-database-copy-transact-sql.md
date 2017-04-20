<properties 
    pageTitle="Kopieren eine SQL Azure-Datenbank mithilfe von Transact-SQL | Microsoft Azure" 
    description="Erstellen Sie Kopie einer Azure SQL-Datenbank mithilfe von Transact-SQL" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Kopieren einer SQL Azure-Datenbank mithilfe von Transact-SQL


> [AZURE.SELECTOR]
- [Übersicht](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)


Diese folgenden Schritte zeigen, wie eine SQL-Datenbank mit Transact-SQL auf demselben Server oder einen anderen Server kopieren. Des Kopiervorgangs verwendet die [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) -Anweisung.

In diesem Artikel Schritte benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie Azure-Abonnement lediglich **Testversion** am oberen Rand dieser Seite klicken Sie und dann wieder zu diesem Artikel.
- Ein Azure SQL-Datenbank. Wenn Sie keinen SQL-Datenbank erstellen einen der Schritte in diesem Artikel: [Erstellen Sie Ihre erste Azure SQL-Datenbank](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Haben Sie SSMS oder in diesem Artikel beschriebenen Features sind nicht verfügbar, [die neueste Version herunterladen](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>Kopieren der SQL-Datenbank

Melden Sie sich bei der Masterdatenbank mithilfe der Serverebene principal Benutzername oder der Benutzername, der die Datenbank erstellt, die Sie kopieren möchten. Benutzernamen, die nicht dem Prinzipal auf Serverebene muss Mitglied der Rolle Dbmanager um Datenbanken kopieren. Weitere Informationen zu Benutzernamen und Verbindung mit dem Server finden Sie unter [Manage Benutzernamen](sql-database-manage-logins.md).

Beginnen Sie kopieren die Datenbank mit der [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) -Anweisung. Diese Anweisung startet die Datenbank kopieren. Ist eine Datenbank kopieren als asynchroner Prozess implementiert, gibt die CREATE DATABASE-Anweisung vor dem kopieren die Datenbank abgeschlossen ist.


### <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf demselben server

Melden Sie sich bei der Masterdatenbank mithilfe der Serverebene principal Benutzername oder der Benutzername, der die Datenbank erstellt, die Sie kopieren möchten. Benutzernamen, die nicht dem Prinzipal auf Serverebene muss Mitglied der Rolle Dbmanager um Datenbanken kopieren.

Dieser Befehl kopiert Database1 auf eine neue Datenbank mit dem Namen Database2 auf demselben Server. Die Größe der Datenbank kann der Kopiervorgang einige Zeit dauern.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Kopieren einer SQL-Datenbank auf einen anderen server

Melden Sie sich bei der master-Datenbank des Zielservers Azure SQL-Datenbank-Server, die neue Datenbank erstellt werden. Verwenden Sie eine Anmeldung mit dem gleichen Namen und Kennwort als Datenbankbesitzer (DBO) der Quelldatenbank auf dem Quellserver Azure SQL-Datenbank. Die Anmeldung auf dem Zielserver muss auch ein Mitglied der Rolle Dbmanager oder Serverebene principal anmelden.

Dieser Befehl kopiert Database1 auf server1 in eine neue Datenbank mit dem Namen Database2 auf server2. Die Größe der Datenbank kann der Kopiervorgang einige Zeit dauern.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>Überwachen Sie den Fortschritt des Kopiervorgangs

Überwachen des Kopiervorgangs Ansichten sys.databases und sys.dm_database_copies Abfragen. Während der Kopiervorgang durchgeführt wird, wird die Spalte State_desc der Ansicht sys.databases für die neue Datenbank kopieren festgelegt.


- Wenn der Kopiervorgang fehlschlägt, wird die State_desc Spalte die sys.databases für die neue Datenbank FEHLERVERDÄCHTIG festgelegt. In diesem Fall führen Sie die DROP-Anweisung für die neue Datenbank, und versuchen Sie es erneut.
- Wenn der Kopiervorgang erfolgreich ist, wird die State_desc Spalte die sys.databases für die neue Datenbank auf ONLINE festgelegt. In diesem Fall das Kopieren abgeschlossen ist und die neue Datenbank wird eine reguläre Datenbank unabhängig von der Quelldatenbank geändert werden.

> [AZURE.NOTE] -Wenn Sie stornieren der Kopiervorgang durchgeführt wird, führen Sie die Anweisung [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) für die neue Datenbank. Ausführen der DROP DATABASE-Anweisung in der Quelldatenbank hebt alternativ auch des Kopiervorgangs.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>Lösen Sie Benutzernamen auf, nachdem der Kopiervorgang abgeschlossen ist

Nachdem die Datenbank auf dem Zielserver ist, Anweisung [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) Benutzer aus der neuen Datenbank für Benutzernamen auf dem Zielserver neu zuordnen. Zum Auflösen von verwaister Benutzern finden Sie unter [Problembehandlung bei verwaisten Benutzern](https://msdn.microsoft.com/library/ms175475.aspx). Siehe auch [SQL Azure-datenbanksicherheit nach Wiederherstellung zu verwalten](sql-database-geo-replication-security-config.md).

Alle Benutzer in der neuen Datenbank verwalten die Berechtigungen, die sie in der Quelldatenbank. Benutzer die Datenbankkopie initiiert wird der Besitzer der neuen Datenbank und wird eine neue Sicherheits-ID (SID) zugewiesen. Nachdem der Kopiervorgang erfolgreich war und bevor andere Benutzer zugeordnet sind, kann nur der Benutzername, der das Kopieren der Datenbankbesitzer (DBO) initiiert in die neue Datenbank anmelden.


## <a name="next-steps"></a>Nächste Schritte

- Finden Sie einen Überblick über das Kopieren einer Azure SQL-Datenbank [eine SQL Azure-Datenbank kopieren](sql-database-copy.md) .
- Anzeigen Sie kopieren eine Datenbank mithilfe von Azure Portal [kopieren eine SQL Azure-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md)
- Anzeigen Sie kopieren eine Datenbank mit PowerShell [Kopie einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md)
- Finden Sie unter [Verwalten von Azure SQL-datenbanksicherheit nach Wiederherstellung](sql-database-geo-replication-security-config.md) Informationen zum Verwalten von Benutzern und Benutzernamen beim Kopieren einer Datenbank auf einem anderen logischen Server.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Verwalten von Benutzernamen](sql-database-manage-logins.md)
- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine T-SQL-Abfrage](sql-database-connect-query-ssms.md)
- [Die Datenbank in ein BACPAC exportieren](sql-database-export.md)
- [Business Continuity – Überblick](sql-database-business-continuity.md)
- [Dokumentation zur SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)


