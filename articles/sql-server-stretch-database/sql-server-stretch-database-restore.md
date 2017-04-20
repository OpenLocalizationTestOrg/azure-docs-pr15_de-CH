<properties
    pageTitle="Stretch-fähigen Datenbanken wiederherstellen | Microsoft Azure"
    description="Informationen zum Wiederherstellen der Strecke\-Datenbanken aktiviert."
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
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Stretch-fähigen Datenbanken wiederherstellen

Stellen Sie eine gesicherte Datenbank aus vielen Arten von Fehlern, Fehler und Katastrophen wiederherstellen wieder her.

Weitere Informationen zu Backup finden Sie unter [Backup Stretch-fähigen Datenbanken](sql-server-stretch-database-backup.md).

>   [AZURE.NOTE] Sicherung wird nur ein Teil einer vollständigen hohe Verfügbarkeit und Business Continuity-Lösung. Weitere Informationen zur Verfügbarkeit finden Sie unter [Lösungen für hohe Verfügbarkeit](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>Wiederherstellen von SQL Server-Daten
Zum Wiederherstellen von Hardwarefehlern oder Beschädigung Wiederherstellen der Strecken\-SQL Server-Datenbank von einer Sicherung aktiviert. Sie können weiterhin SQL Server Wiederherstellungsmethoden verwenden, die Sie derzeit verwenden. Weitere Informationen finden Sie unter [Restore und Recovery Overview](https://msdn.microsoft.com/library/ms191253.aspx).

Nach dem Wiederherstellen der SQL Server-Datenbank müssen Sie die gespeicherte Prozedur **sys.sp_rda_reauthorize_db** zum Herstellen einer Verbindung zwischen der Stretch erneut ausführen\-SQL Server-Datenbank und der entfernten Datenbank Azure aktiviert. Weitere Informationen finden Sie unter [Wiederherstellen der Verbindung zwischen der SQL Server-Datenbank und der entfernten Datenbank Azure](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database).

## <a name="restore-your-remote-azure-data"></a>Die entfernten Azure Daten wiederherstellen

### <a name="recover-a-live-azure-database"></a>Wiederherstellen einer Livedatenbank Azure
Stretch-Datenbank von SQL Server-Dienst auf Azure Snapshots alle Livedaten alle 8 Stunden Azure Storage Snapshots. Diese Snapshots sind 7 Tage beibehalten. Dadurch können Sie Daten zu mindestens 21 Punkten innerhalb der letzten 7 Tage bis zu dem Zeitpunkt wiederherstellen beim letzte Snapshot erstellt wurde.

Gehen Sie folgendermaßen vor, um live Azure-Datenbank zu einem früheren Zeitpunkt wiederherstellen mithilfe von Azure-Portal.

1. Melden Sie sich bei Azure-Portal.
2. Wählen Sie auf der linken Seite des Bildschirms **Durchsuchen** , und wählen Sie **SQL-Datenbanken**.
3. Navigieren Sie zu der Datenbank, und wählen Sie sie aus.
4. Klicken Sie oben auf Blatt Datenbank **Wiederherstellen**.
5. Sie **Datenbankname**, wählen Sie einen **Wiederherstellungspunkt** , und klicken Sie auf **Erstellen**.
6. Datenbank-Wiederherstellungsvorgang beginnt und mit **Benachrichtigung**überwacht werden.

### <a name="recover-a-deleted-azure-database"></a>Wiederherstellen einer gelöschten Azure-Datenbank
Der Dienst SQL Server Stretch Datenbank Azure Snapshot einen Datenbank eine Datenbank gelöscht und 7 Tage beibehalten. Danach nicht mehr Snapshots von der echten Datenbank beibehalten. Dadurch können Sie so eine gelöschte Datenbank wiederherstellen, wenn sie gelöscht wurde.

Zum Wiederherstellen einer gelöschten Azure darauf beim Löschen der Azure-Portal mit Aktionen.

1. Melden Sie sich bei Azure-Portal.
2. Wählen Sie auf der linken Seite des Bildschirms **Durchsuchen** , und wählen Sie **SQL Server**.
3. Navigieren Sie zu Ihrem Server, und wählen Sie sie aus.
4. Scrollen Sie auf Ihr Server-Blade, klicken Sie auf die Kachel **Datenbanken gelöscht** .
5. Wählen Sie die gelöschte Datenbank wiederherstellen möchten.
5. Sie **Name der Datenbank** , und klicken Sie auf **Erstellen**.
6. Datenbank-Wiederherstellungsvorgang beginnt und mit **Benachrichtigung**überwacht werden.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>Wiederherstellen der Verbindung zwischen der SQL Server-Datenbank und der entfernten Datenbank Azure

1.  Wenn Sie eine wiederhergestellte Azure-Datenbank unter einem anderen Namen oder in einem anderen Bereich herstellen wollen, führen Sie die gespeicherte Prozedur [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) vorherige Azure-Datenbank trennen.  

2.  Führen Sie die gespeicherte Prozedur [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) zum Wiederherstellen der lokalen Strecken\-Datenbank in Azure-Datenbank aktiviert.  

    -   Vorhandene Datenbank begrenzt Anmeldeinformationen ein genauerer oder eine Varchar bieten\(128\) Wert. \(Verwenden Sie Varchar, nicht\(maximale\).\) In der Ansicht den Anmeldenamen Nachschlagen **sys.database\_Bereich\_Anmeldeinformationen**.  

    -   Geben Sie an, ob die entfernte Datenbank kopieren und die Kopie (empfohlen).  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Siehe auch

[Verwaltung und Problembehandlung von Stretch-Datenbank](sql-server-stretch-database-manage.md)

[Sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Sichern Sie und Wiederherstellen Sie der SQL Server-Datenbanken](https://msdn.microsoft.com/library/ms187048.aspx)
