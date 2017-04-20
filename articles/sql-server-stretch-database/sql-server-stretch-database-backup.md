<properties
    pageTitle="Stretch-fähigen Datenbanken sichern | Microsoft Azure"
    description="So strecken Sichern\-Datenbanken aktiviert."
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
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Sicherungsdatenbanken Strecken aktiviert

Datenbank-Backups können aus vielen Arten von Fehlern, Fehler und Katastrophen wiederherstellen.  

-   Sichern der Strecke haben\-SQL Server-Datenbanken aktiviert.  

-   Microsoft Azure sichert automatisch die entfernte Datenbank, die Stretch-Datenbank von SQL Server in Azure migriert wurde.  

>    [AZURE.NOTE] Sicherung wird nur ein Teil einer vollständigen hohe Verfügbarkeit und Business Continuity-Lösung. Weitere Informationen zur Verfügbarkeit finden Sie unter [Lösungen für hohe Verfügbarkeit](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>Sichern Sie die SQL Server-Daten  

Sichern der Strecke\-ermöglicht SQL Server-Datenbanken können weiterhin SQL Server backup-Methoden verwenden, die Sie derzeit verwenden. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen der SQL Server-Datenbanken](https://msdn.microsoft.com/library/ms187048.aspx).

Nur lokale Daten und geeignete Daten für die Migration an dem Punkt in der Ausführung bei die Sicherung enthalten Backups Stretch-fähigen SQL Server-Datenbank. \(Berechtigte Daten sind, die noch nicht migriert, aber auf der Basis Migration Tabellen Azure migriert wird.\) Dieser als **flache** Sicherung bekannt ist und die Daten bereits in Azure migriert.  

## <a name="back-up-your-remote-azure-data"></a>Sichern Sie Ihre Azure Remotedaten   

Microsoft Azure sichert automatisch die entfernte Datenbank, die Stretch-Datenbank von SQL Server in Azure migriert wurde.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure reduziert das Risiko von Datenverlust mit automatischen Sicherung  
Der Dienst SQL Server Stretch Datenbank Azure schützt Ihre entfernten Datenbanken mit automatischen Snapshots alle 8 Stunden. Jeder Snapshot 7 Tage bieten zahlreiche mögliche Wiederherstellungspunkte behält.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure reduziert das Risiko von Datenverlust mit Geo\-Redundanz  
Azure-Datenbank-Backups auf Geo gespeichert\-redundante Azure Storage (RA\-g) und daher geometrische\-standardmäßig redundant. Geometrische\-redundante Speicherung Daten auf einen sekundären Region Hunderte von Meilen von der primären Region repliziert. Daten werden in primäre und sekundäre jeweils dreimal auf separaten Fehlertoleranz und Upgrade Domänen repliziert. Dadurch wird sichergestellt, dass Ihre Daten auch bei einer vollständigen regionalen Ausfall oder, die eines der Azure nicht verfügbar macht dauerhaften.

### <a name="stretchRPO"></a>Dehnen Datenbank reduziert das Risiko von Datenverlust für Azure Daten vorübergehend migrierte Zeilen beibehalten
Nachdem Stretch Datenbank können Zeilen aus SQL Server in Azure migriert, behält sie die Zeilen in der Stagingtabelle für mindestens 8 Stunden. Wiederherstellen eine Sicherung Ihrer Azure-Datenbank verwendet Stretch Datenbank gespeicherten in der Stagingtabelle Zeilen zu SQL Server und der Azure-Datenbanken.

Nach dem Wiederherstellen einer Sicherung von Azure Daten haben, führen Sie die gespeicherte Prozedur [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) um die Strecke schließen\-SQL Server-Datenbank an die entfernte Datenbank Azure aktiviert. Beim Ausführen von **sys.sp_rda_reauthorize_db**gleicht Stretch-Datenbank automatisch Azure Datenbanken und SQL Server.

Um die Anzahl der Stunden der migrierten Daten erhöhen, die Stretch-Datenbank vorübergehend in der Stagingtabelle behält, führen Sie die gespeicherte Prozedur [sys.sp_rda_set_rpo_duration](https://msdn.microsoft.com/library/mt707766.aspx) und geben Sie eine Zahl größer als 8 Stunden. Entscheidung, Daten beibehalten, Faktoren Sie die folgenden:
-   Frequenz der automatischen Azure Backups (alle 8 Stunden).
-   Nachdem ein Problem, das Problem zu erkennen und Wiederherstellen einer Sicherung erforderliche Zeit.
-   Die Dauer des Wiederherstellungsvorgangs Azure.

> [AZURE.NOTE] Die Stretch-Datenbank vorübergehend in der Stagingtabelle behält Datenmenge erhöht den Speicherplatz auf dem SQL Server.

Um Stretch Datenbank vorübergehend aktuell die Stagingtabelle behält die Anzahl der Stunden von Daten zu überprüfen, führen Sie die gespeicherte Prozedur [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx).

## <a name="see-also"></a>Siehe auch

[Verwaltung und Problembehandlung von Stretch-Datenbank](sql-server-stretch-database-manage.md)

[Sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Sichern Sie und Wiederherstellen Sie der SQL Server-Datenbanken](https://msdn.microsoft.com/library/ms187048.aspx)
