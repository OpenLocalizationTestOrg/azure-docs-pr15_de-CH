<properties
    pageTitle="Problembehebung bei der Sicherung und Wiederherstellung mit Azure SQL-Datenbank"
    description="Informationen Sie zum Fehler und Ausfälle Backups und Replikate in Azure SQL-Datenbank mit einer Wiederherstellung."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Wiederherstellen einer Datenbank zu einem bestimmten Zeitpunkt eine gelöschte Datenbank wiederherstellen oder einen Rechenzentrums hat wiederherstellen

SQL-Datenbank wird Replikate Ihrer Datenbank, damit Sie Ausfälle und Fehler beheben können. Verfügbaren Optionen hängen von der Datenbankebene Service und gewählten Optionen ab. [Übersicht über Business Continuity](sql-database-business-continuity.md) für Informationen und Vorüberlegungen anzeigen

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Um eine Datenbank zu einem früheren Zeitpunkt wiederherstellen
1.  Klicken Sie im [Azure-Portal](https://azure.microsoft.com/)auf **SQL-Datenbanken**.
2.  Wählen Sie die Datenbank aus und klicken Sie auf **Wiederherstellen**.
3.  Geben Sie einen neuen Namen für die Datenbank, wählen Sie das Datum und die Zeit wiederherstellen aus, und klicken Sie auf **erstellen.**
4.  Nehmen Sie app vor, gegebenenfalls auf neue Datenbank. Finden Sie unter [Wiederherstellen eine Datenbank zu einem bestimmten Zeitpunkt](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="to-restore-an-accidentally-deleted-database"></a>Versehentlich gelöschte Datenbank wiederherstellen
1.  Klicken Sie im [Azure-Portal](https://azure.microsoft.com/)auf **SQL Server**.
2.  Wählen Sie den Server, der Datenbank aus der Liste gehostet.
3.  Das Server-Blade einen Bildlauf nach unten und dann auf **Datenbanken gelöscht**.
4.  Wählen Sie die Datenbank wiederherstellen, und klicken Sie auf **Erstellen**.
5.  Nehmen Sie app vor, gegebenenfalls auf neue Datenbank. [Eine gelöschte Datenbank wiederherstellen](sql-database-recovery-using-backups.md#deleted-database-restore)anzeigen

## <a name="to-recover-from-a-regional-datacenter-outage"></a>Zum Wiederherstellen nach einem Ausfall regionale Datenzentrum
Standard- und Premium-Datenbanken Wenn sekundäre Geo repliziert einrichten können Sie wiederherstellen verwenden diese sekundäre. So können Sie eine Datenbank mit weniger Datenverlust wiederherstellen. Details finden Sie unter [Wiederherstellen eine Azure SQL-Datenbank mit automatisierter Backups](sql-database-disaster-recovery.md) .

Azure bietet auch Backups jede Datenbank in einer anderen Region (Geo redundante Backup). Erstellen einer neuen Datenbank aus dieser Sicherungen Geo-Restore aufgerufen wird, aber es ist wahrscheinlich, dass Sie Datenverlust erleben, wenn Sie diese Methode nur verwenden.

**Zum Wiederherstellen einer Datenbank mit Geo-Wiederherstellung:**

- Klicken Sie in [Azure-Portal](https://azure.microsoft.com/)auf **neu**, klicken Sie auf **Daten und**klicken Sie auf **SQL-Datenbank**und wählen Sie dann **Sicherung** als Datenbankquelle. Details finden Sie unter [Wiederherstellen eine SQL Azure-Datenbank nach einem Ausfall](sql-database-disaster-recovery.md) .