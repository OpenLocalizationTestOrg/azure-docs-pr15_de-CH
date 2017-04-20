<properties
    pageTitle="Stretch-Datenbank deaktivieren und wieder remote Daten | Microsoft Azure"
    description="Informationen Sie zum Deaktivieren Stretch-Datenbank eine Tabelle und optional Remotedaten wieder."
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
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Deaktivieren Sie Stretch-Datenbank und wieder entfernte Daten

Aktivieren Sie Stretch-Datenbank für eine Tabelle, um deaktivieren **Strecken** für eine Tabelle in SQL Server Management Studio. Wählen Sie eine der folgenden Optionen.

-   **Deaktivieren | Daten von Azure zurückzuholen**. Kopie die remote-Daten für die Tabelle von Azure SQL Server wieder deaktivieren Stretch-Datenbank für die Tabelle. Dadurch entstehen Kosten für die Datenübertragung und kann nicht abgebrochen werden.

-   **Deaktivieren | Lassen Sie Daten in Azure**. Stretch-Datenbank für die Tabelle zu deaktivieren.  Verlassen Sie remote-Daten für die Tabelle in Azure.

Sie können auch die Transact\-SQL Datenbank Strecken für eine Tabelle oder eine Datenbank deaktiviert.

Nach dem Deaktivieren von Stretch-Datenbank eine Tabelle enthalten Data Migration beendet und Abfrageergebnisse mehr Ergebnisse aus der entfernten Tabelle.

Wenn Sie einfach die Datenmigration anhalten möchten, finden Sie unter [Anhalten und fortsetzen Stretch-Datenbank](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Stretch-Datenbank für eine Tabelle oder eine Datenbank deaktivieren wird das remote-Objekt nicht gelöscht. Wenn Sie die entfernte Tabelle oder der entfernten Datenbank löschen möchten, müssen Sie mithilfe das Azure-Verwaltungsportal ablegen. Die entfernten Objekte weiterhin Azure Kosten, bis Sie sie löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-table"></a>Stretch-Datenbank eine Tabelle deaktivieren

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Verwenden Sie SQL Server Management Studio Stretch-Datenbank eine Tabelle deaktivieren

1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer Tabelle Stretch Datenbank deaktivieren möchten.

2.  Rechts\-auf **Strecken**wählen und wählen Sie dann eine der folgenden Optionen.

    -   **Deaktivieren | Daten von Azure zurückzuholen**. Kopie die remote-Daten für die Tabelle von Azure SQL Server wieder deaktivieren Stretch-Datenbank für die Tabelle. Dieser Befehl kann nicht abgebrochen werden.

        >   [AZURE.NOTE] Kopieren die entfernte Datenbank auf SQL Server berechnet Tabelle von Azure Übertragungskosten. Weitere Informationen finden Sie unter [Preisdetails Daten überträgt](https://azure.microsoft.com/pricing/details/data-transfers/).

        Nachdem alle entfernten Daten von Azure kopiert wurden auf SQL Server sichern, ist Strecken für die Tabelle deaktiviert.

    -   **Deaktivieren | Lassen Sie Daten in Azure**. Stretch-Datenbank für die Tabelle zu deaktivieren.  Verlassen Sie remote-Daten für die Tabelle in Azure.

    >   [AZURE.NOTE] Deaktivieren der Stretch-Datenbank eine Tabelle die Remotedaten oder nicht der entfernten Tabelle gelöscht. Wenn Sie die entfernte Tabelle löschen möchten, müssen Sie mithilfe das Azure-Verwaltungsportal ablegen. Die entfernte Tabelle weiterhin Azure Kosten, bis Sie sie löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Verwenden Sie Transact\-SQL Stretch-Datenbank eine Tabelle deaktivieren

-   Führen Sie den folgenden Befehl deaktivieren Strecken für eine Tabelle und eine Kopie die remote-Daten für die Tabelle von Azure SQL Server zurück. Nachdem alle entfernten Daten von Azure kopiert wurden auf SQL Server sichern, ist Strecken für die Tabelle deaktiviert.

    Dieser Befehl kann nicht abgebrochen werden.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] Kopieren die entfernte Datenbank auf SQL Server berechnet Tabelle von Azure Übertragungskosten. Weitere Informationen finden Sie unter [Preisdetails Daten überträgt](https://azure.microsoft.com/pricing/details/data-transfers/).

-   Deaktivieren Strecken für eine Tabelle die Remotedaten verlassen und Befehl.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Deaktivieren der Stretch-Datenbank eine Tabelle die Remotedaten oder nicht der entfernten Tabelle gelöscht. Wenn Sie die entfernte Tabelle löschen möchten, müssen Sie mithilfe das Azure-Verwaltungsportal ablegen. Die entfernte Tabelle weiterhin Azure Kosten, bis Sie sie löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-database"></a>Stretch-Datenbank für eine Datenbank deaktivieren
Vor dem Deaktivieren von Stretch-Datenbank für eine Datenbank müssen Sie deaktivieren Stretch-Datenbank auf den einzelnen\-Tabellen in der Datenbank aktiviert.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Verwenden Sie SQL Server Management Studio Stretch-Datenbank für eine Datenbank deaktivieren

1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Datenbank für die Stretch-Datenbank deaktiviert werden soll.

2.  Rechts\-auf Wählen Sie **Aufgaben**, und wählen Sie **Strecken**und wählen Sie **Deaktivieren**.

>   [AZURE.NOTE] Deaktivieren der Stretch-Datenbank für eine Datenbank wird die entfernte Datenbank nicht gelöscht. Die entfernte Datenbank gelöscht werden soll, müssen Sie mithilfe das Azure-Verwaltungsportal ablegen. Die entfernte Datenbank weiterhin Azure Kosten, bis Sie sie löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Verwenden Sie Transact\-SQL Datenbank Strecken für eine Datenbank deaktivieren
Führen Sie den folgenden Befehl ein.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Deaktivieren der Stretch-Datenbank für eine Datenbank wird die entfernte Datenbank nicht gelöscht. Die entfernte Datenbank gelöscht werden soll, müssen Sie mithilfe das Azure-Verwaltungsportal ablegen. Die entfernte Datenbank weiterhin Azure Kosten, bis Sie sie löschen. Weitere Informationen finden Sie unter [SQL Server Stretch Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="see-also"></a>Siehe auch

[ALTER DATABASE SET Options (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[Anhalten und Fortsetzen von Stretch-Datenbank](sql-server-stretch-database-pause.md)
