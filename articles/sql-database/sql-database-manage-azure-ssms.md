<properties 
    pageTitle="Verwalten einer SQL-Datenbank mit SSMS | Microsoft Azure" 
    description="Erfahren Sie, wie SQL Server Management Studio verwenden, um SQL-Datenbankserver und Datenbanken verwalten." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Verwalten von Azure SQL-Datenbank mit SQL Server Management Studio 


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

SQL Server Management Studio (SSMS) können Sie Azure SQL-Datenbank-Server und Datenbanken verwalten. Dieses Thema führt Sie durch häufige Aufgaben mit SSMS. Sie haben bereits einen Server und in Azure SQL-Datenbank erstellt wurden, bevor Sie beginnen. Weitere Informationen finden Sie unter [erstellen Ihre erste Azure SQL-Datenbank](sql-database-get-started.md) und [Verbinden und Abfrage SSMS](sql-database-connect-query-ssms.md) .

Es wird empfohlen, dass Sie die neueste Version von SSMS verwenden, wenn Sie Azure SQL-Datenbank arbeiten. 

> [AZURE.IMPORTANT] Verwenden Sie immer die neueste Version von SSMS ständig verbessert wird, um die neuesten Updates für Azure und SQL-Datenbank arbeiten. Die neueste Version finden Sie unter [Laden von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Erstellen und Verwalten von Azure SQL-Datenbanken

Verbindung zur **Masterdatenbank** können Datenbanken auf dem Server erstellen und bearbeiten oder Löschen von Datenbanken. Die folgenden Schritte beschreiben, wie verschiedene allgemeine Datenbank durch Management Studio durchführen. Diese Aufgaben stellen Sie sicher, dass Sie die **master** -Datenbank mit dem Prinzipal auf Serverebene Benutzernamen verbunden sind, das Sie bei Ihrem Server einrichten.

Um ein Abfragefenster in Management Studio öffnen, öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **master**und klicken Sie dann auf **Neue Abfrage**.

-   Verwenden Sie die **CREATE DATABASE** -Anweisung zum Erstellen einer Datenbank. Weitere Informationen finden Sie unter [Erstellen Datenbank (SQL)](https://msdn.microsoft.com/library/dn268335.aspx). Die folgende Anweisung erstellt eine Datenbank namens **MyTestDB** und gibt an, dass es eine Standardversion S0 Datenbank standardmäßig maximal 250 GB.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

-   Verwenden Sie die Anweisung **ALTER DATABASE** zum Ändern einer vorhandenen Datenbank beispielsweise den Namen und die Version der Datenbank ändern möchten. Weitere Informationen finden Sie unter [ALTER DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/ms174269.aspx). Die folgende Anweisung ändert die im vorherigen Schritt ändern erstellte Datenbank Edition Standard s1.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   **DROP DATABASE** -Anweisung löschen Sie eine vorhandene Datenbank. Weitere Informationen finden Sie unter [DROP DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/ms178613.aspx). Die folgende Anweisung löscht die Datenbank **MyTestDB** , aber nicht gelöscht werden, es jetzt da es mit der Benutzernamen im nächsten Schritt erstellen.

        DROP DATABASE myTestBase;

-   Der master-Datenbank hat die **sys.databases** -Ansicht, die Sie verwenden können, um Details zu allen Datenbanken. Um alle vorhandenen Datenbanken anzuzeigen, führen Sie die folgende Anweisung aus:

        SELECT * FROM sys.databases;

-   SQL-Datenbank wird die **USE** -Anweisung nicht zwischen Datenbanken unterstützt. Stattdessen müssen Sie eine Verbindung direkt mit der Datenbank herzustellen.

>[AZURE.NOTE] Viele Transact-SQL-Anweisungen, die erstellen oder Ändern einer Datenbank müssen in ihren eigenen Stapel ausgeführt werden und nicht mit anderen Transact-SQL-Anweisungen gruppiert werden. Weitere Informationen finden Sie in der Anweisung Informationen.

## <a name="create-and-manage-logins"></a>Erstellen und Verwalten von Benutzernamen

Die **master** -Datenbank enthält Benutzernamen und welche Benutzernamen berechtigt, Datenbanken oder anderen Benutzernamen erstellen. Verwalten Sie Benutzernamen mit der **master** -Datenbank mit dem Prinzipal auf Serverebene Benutzernamen, den Sie beim Einrichten der Server erstellt. **CREATE LOGIN**, **ALTER LOGIN**oder **DROP LOGIN** -Anweisung können Sie die master-Datenbank Abfragen ausführen, die Benutzernamen auf den gesamten Server verwaltet. Weitere Informationen finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in SQL-Datenbank](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   Verwenden Sie **CREATE LOGIN** -Anweisung zum Erstellen eines Benutzernamens auf Serverebene. Weitere Informationen finden Sie unter [CREATE LOGIN (SQL-Datenbank)](https://msdn.microsoft.com/library/ms189751.aspx). Die folgende Anweisung erstellt einen Benutzernamen **login1**aufgerufen. Ersetzen Sie **Kennwort1** mit dem Kennwort Ihrer Wahl.

        CREATE LOGIN login1 WITH password='password1';

-   Verwenden Sie die Anweisung **CREATE USER** Berechtigungen auf Datenbankebene. Alle Benutzernamen müssen in der **master** -Datenbank erstellt. Eine Anmeldung mit einer anderen Datenbank verbinden müssen Sie es mit der Anweisung **CREATE USER** der Datenbank Berechtigungen auf Datenbankebene gewähren. Weitere Informationen finden Sie unter [CREATE USER (SQL-Datenbank)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Geben Sie Berechtigungen für eine Datenbank namens **MyTestDB**login1 Schritte:

 1.  Zum Aktualisieren der Objekt-Explorer **MyTestDB** Datenbank anzeigen, die Sie erstellt klicken Sie den Servernamen im Objekt-Explorer und dann auf **Aktualisieren**.  

     Wenn die Verbindung können Sie erneut **Verbinden Objekt-Explorer** im Menü Datei auswählen.

 2. **MyTestDB** Datenbank Maustaste, und wählen Sie **Neue Abfrage**.

    3.  Führen Sie folgende Anweisung in der Datenbank MyTestDB erstellen Sie einen Datenbankbenutzer namens **login1User** , die auf Serverebene Login **login1**entspricht.

            CREATE USER login1User FROM LOGIN login1;

-   Verwenden der **sp\_Addrolemember** Verfahren zu dem Benutzerkonto die entsprechende Berechtigungsstufe in der Datenbank gespeichert. Weitere Informationen finden Sie unter [Sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Die folgende Anweisung liefert **login1User** schreibgeschützte Berechtigungen für die Datenbank durch Hinzufügen von **login1User** zu den **Db\_Datareader** Rolle.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Verwenden Sie die Anweisung **ALTER LOGIN** ändern einen vorhandenen Benutzernamen beispielsweise das Kennwort für die Anmeldung ändern möchten. Weitere Informationen finden Sie unter [ALTER LOGIN (SQL-Datenbank)](https://msdn.microsoft.com/library/ms189828.aspx). Die **master** -Datenbank sollte **ALTER LOGIN** -Anweisung ausgeführt. Wechseln Sie zum Abfragefenster mit der Datenbank verbunden ist. Die folgende Anweisung ändert den **login1** Benutzernamen, das Kennwort zurückzusetzen. Ersetzen Sie **NewPassword** mit dem Kennwort Ihrer Wahl und **OldPassword** mit dem aktuellen Kennwort für den Benutzernamen.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Verwenden Sie **DROP LOGIN** -Anweisung, um einen vorhandenen Benutzernamen löschen. Eine Anmeldung auf Serverebene löschen auch alle verbundenen Datenbankbenutzerkonten. Weitere Informationen finden Sie unter [DROP DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/ms178613.aspx). **DROP LOGIN** -Anweisung sollte die **master** -Datenbank ausgeführt. Die Anweisung löscht **login1** anmelden.

        DROP LOGIN login1;

-   Der master-Datenbank wurde der **sys.sql\_Benutzernamen** anzeigen, mit Benutzernamen anzuzeigen. Um alle vorhandenen Benutzernamen anzuzeigen, führen Sie die folgende Anweisung:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Überwachen Sie mit dynamischen SQL-Datenbank

SQL-Datenbank unterstützt mehrere dynamische Verwaltungsansichten mit eine einzelne Datenbank überwacht. Es folgen Beispiele des Typs des Monitor-Daten über diese Ansichten abgerufen werden können. Ausführliche Details und weitere Beispiele finden Sie unter [SQL-Überwachungsdatenbank verwenden dynamische Verwaltungsansichten](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Abfragen einer dynamischen Ansicht erfordert **VIEW DATABASE STATE** Berechtigungen. Um einem bestimmten Datenbankbenutzer die **VIEW DATABASE STATE** -Berechtigung erteilen, Verbinden mit der Datenbank und führen Sie folgende Anweisung in der Datenbank:

        GRANT VIEW DATABASE STATE TO login1User;

-   Berechnung Datenbank mit der Größe der **sys.dm\_Db\_Partition\_Statistiken** anzeigen. Die **sys.dm\_Db\_Partition\_Statistiken** Ansicht gibt Informationen für jede Partition Seite und Anzahl der Zeilen in der Datenbank mit Berechnung der Datenbankgröße. Die folgende Abfrage gibt die Größe der Datenbank in MB:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Verwenden der **sys.dm\_Exec\_Verbindung** und **sys.dm\_Exec\_Sitzungen** Ansichten zum Abrufen von Informationen zu betätigen und internen Aufgaben im Zusammenhang mit der Datenbank. Die folgende Abfrage gibt Informationen über die aktuelle Verbindung zurück:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Verwenden der **sys.dm\_Exec\_Abfrage\_Statistiken** Ansicht abrufen aggregierte Leistungsstatistiken für zwischengespeicherte Abfragepläne. Die folgende Abfrage gibt Aufschluss über die durchschnittliche CPU-Zeit Rangfolge oberen fünf Abfragen.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
