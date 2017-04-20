<properties
    pageTitle="Anhalten und Fortsetzen der Datenmigration (Stretch Datenbank) | Microsoft Azure"
    description="Informationen Sie zum Anhalten oder Fortsetzen der Datenmigration in Azure."
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
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Halten Sie an und fortsetzen Sie der Datenmigration (Stretch-Datenbank)

Zum Anhalten oder fortsetzen Datenmigration in Azure, wählen Sie **Strecken** für eine Tabelle in SQL Server Management Studio und dann Datenmigration anhalten **Anhalten** oder Datenmigration wieder **Fortsetzen** . Sie können auch die Transact\-SQL zum Anhalten oder Fortsetzen der Datenmigration.

Pause Datenmigration auf einzelne Tabellen bei Problemen auf dem lokalen Server oder die verfügbare Netzwerkbandbreite optimieren möchten.

## <a name="pause-data-migration"></a>Pause-Datenmigration

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Verwenden Sie SQL Server Management Studio Datenmigration anhalten

1.  Wählen Sie SQL Server Management Studio im Objekt-Explorer die Strecke\-aktiviert Tabelle Datenmigration angehalten werden soll.

2.  Rechts\-auf **Strecken**wählen und wählen Sie **Anhalten**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Verwenden Sie Transact\-SQL Datenmigration anhalten
Führen Sie den folgenden Befehl ein.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Resume-Datenmigration

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Verwenden Sie SQL Server Management Studio wieder Datenmigration

1.  Wählen Sie SQL Server Management Studio im Objekt-Explorer die Strecke\-aktiviert Tabelle Datenmigration fortgesetzt werden soll.

2.  Rechts\-auf **Strecken**wählen und wählen Sie dann **Fortsetzen**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Verwenden Sie Transact\-SQL Data Migration fortsetzen
Führen Sie den folgenden Befehl ein.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Überprüfen Sie, ob die Migration aktiv oder angehalten ist.

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Verwenden Sie SQL Server Management Studio überprüfen, ob die Migration aktiv oder angehalten ist
Öffnen Sie in SQL Server Management Studio **Stretch Datenbankmonitor** und überprüfen Sie den Wert der Spalte **Status der Migration** . Weitere Informationen finden Sie unter [Überwachung und Problembehandlung bei der Datenmigration](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Verwenden Sie Transact-SQL, ob Migration aktiv oder angehalten ist
Katalog Ansicht **sys.remote_data_archive_tables** Abfragen und den Wert der **Is_migration_paused** -Spalte. Weitere Informationen finden Sie unter [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="see-also"></a>Siehe auch

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[Überwachung und Problembehandlung bei der Datenmigration](sql-server-stretch-database-monitor.md)
