<properties
   pageTitle="Business Continuity Cloud - eine gelöschte Datenbank - SQL-Datenbank wiederherstellen | Microsoft Azure"
   description="Erfahren Sie mehr über Point-in-Time-Wiederherstellung, die Sie einer Azure SQL-Datenbank zu einem früheren Zeitpunkt in bis zu 35 Tage zurücksetzen kann."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Wiederherstellen einer Azure SQL-Datenbank mit automatisierter backups

SQL-Datenbank bietet drei Optionen für die Recovery mit [SQL Datenbank Backups automatisiert](sql-database-automated-backups.md). Sie können eine Datenbank aus den Sicherungskopien Dienst initiiert die [Aufbewahrungsdauer](sql-database-service-tiers.md) auf Wiederherstellen:

- Eine neue Datenbank auf dem gleichen logischen Server an einem bestimmten Punkt innerhalb der Aufbewahrungsdauer wiederhergestellt. 
- Eine Datenbank auf dem gleichen logischen Server die Löschzeit für eine gelöschte Datenbank wiederhergestellt.
- Eine neue Datenbank auf einem logischen Server in jeder Region der letzten täglichen Backups in geografisch repliziert BLOB-Speicher (RA-GRS) wiederhergestellt.

[SQL Datenbank automatisierte Backups](sql-database-automated-backups.md) können auch eine [Datenbank kopieren](sql-database-copy.md) auf einem logischen Server in jeder Region erstellen, die mit der aktuellen SQL-Datenbank transaktionell konsistent ist. Sie können Datenbank kopieren und [in ein BACPAC exportieren](sql-database-export.md) transaktionell konsistente Kopie einer Datenbank für die langfristige Lagerung über die Aufbewahrungsdauer archivieren oder einer lokalen Kopie der Datenbank übertragen oder Azure VM-Instanz von SQL Server.

## <a name="recovery-time"></a>Recovery-Zeit

Die Recovery-Zeit zum Wiederherstellen einer Datenbank mit automatisierter Backups ist eine Reihe von Faktoren beeinflusst: 
 - Die Größe der Datenbank
 - Die Leistungsfähigkeit der Datenbank
 - Die Anzahl der beteiligten Transaktionsprotokolle
 - Der Aufwand, die wiedergegeben werden, um den Wiederherstellungspunkt wiederherstellen
 - Die Netzwerkbandbreite ist die Wiederherstellung in eine andere region 
 - Die Anzahl gleichzeitiger Wiederherstellung in den Zielbereich verarbeitet. 
 
 Die Wiederherstellung kann für eine sehr große oder aktive Datenbank mehrere Stunden dauern. Ist bei längeren Ausfällen in einem Bereich ist möglich, dass viele geografische Wiederherstellung Anfragen von anderen Regionen verarbeitet werden. Gibt eine große Anzahl von Anfragen erhöht dadurch die Wiederherstellungszeit für Datenbanken in diesem Bereich. Die meisten Datenbank-Wiederherstellung innerhalb von 12 Stunden abgeschlossen.

 Es gibt keine integrierte Funktionalität wiederherstellen Massenkopieren. Die [Azure SQL-Datenbank: vollständige Server Recovery](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) Skript zeigt eine Möglichkeit, diese Aufgabe durchzuführen.

> [AZURE.IMPORTANT] Wiederherstellen mit automatischen Backups müssen Sie Mitglied der SQL Server-Beitragendenrolle im Abonnement oder der Abonnementbesitzer sein. Sie können mithilfe der Azure-Portal PowerShell oder die REST-API wiederherstellen. Sie können keine Transact-SQL. 

## <a name="point-in-time-restore"></a>Point-In-Time-Wiederherstellung

Point-In-Time wiederherstellen können Sie eine vorhandene Datenbank auf demselben logischen Server mit [SQL Datenbank automatisierte Backups](sql-database-automated-backups.md)eine neue Datenbank zu einem früheren Zeitpunkt wiederherstellen. Die Datenbank kann nicht überschrieben werden. Sie können mit der [Azure-Portal](sql-database-point-in-time-restore-portal.md), [PowerShell](sql-database-point-in-time-restore-powershell.md) oder die [REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx)zu einem früheren Zeitpunkt wiederherstellen.

> [AZURE.SELECTOR]
- [Point-In-Time-Wiederherstellung: Azure-portal](sql-database-point-in-time-restore-portal.md)
- [Point-In-Time-Wiederherstellung: PowerShell](sql-database-point-in-time-restore-powershell.md)

Die Datenbank Leistung oder elastischen wiederhergestellt werden Pool. Sie müssen sicherstellen, dass Sie ausreichende DTU Kontingent auf dem logischen Server oder elastischen Pool. Denken Sie daran, die die Wiederherstellung eine neue Datenbank erstellt und Tier und Leistung Dienstebene der wiederhergestellten Datenbank möglicherweise nicht den aktuellen Zustand der live-Datenbank. Danach ist die wiederhergestellte Datenbank eine normale barrierefrei Onlinedatenbank seine Service-Tier und Performance-Niveau normalen Gebühren berechnet. Sie keine Kosten erst nach Abschluss die Wiederherstellung der Datenbank.

Im Allgemeinen wird eine Datenbank auf einen weiter vorn Punkt für die Wiederherstellung wiederherstellen. Dabei können die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank behandelt oder Daten abrufen und aktualisieren Sie dann die ursprüngliche Datenbank verwenden. 

- ***Datenbank ersetzt:*** Soll die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank, prüfen die Leistungsfähigkeit oder Dienstebene sind und die Datenbank bei Bedarf zu skalieren. Benennen Sie die ursprüngliche Datenbank, und geben der wiederhergestellten Datenbank der ursprüngliche Name in T-SQL mit dem Befehl ALTER DATABASE. 
- ***Daten-Recovery:*** Wenn Sie Daten aus der wiederhergestellten Datenbank wiederherstellen nach einem Benutzer oder einer Anwendung Fehler abrufen möchten, müssen Sie separat schreiben und Ausführen von beliebigen Daten Recovery-Skripts Sie Daten aus der wiederhergestellten Datenbank in die ursprüngliche Datenbank extrahieren müssen. Wiederherstellung eine lange dauern, ist in der Liste in die Datenbank wiederherstellende sichtbar. Wenn Sie während der Wiederherstellung die Datenbank löschen, bricht den Vorgang und Sie nicht berechnet werden für die Datenbank, die die Wiederherstellung nicht abgeschlossen wurde. 

Ausführliche Informationen zur Verwendung von Point-in-Time-Wiederherstellung zur Wiederherstellung Benutzer und Anwendungsfehler finden Sie unter [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>Gelöschte wiederherstellen

Gelöschte wiederherstellen können Sie die Löschzeit für eine gelöschte Datenbank auf demselben logischen Server mit [SQL Datenbank automatisierte Backups](sql-database-automated-backups.md)eine gelöschte Datenbank wiederherstellen. 

> [AZURE.IMPORTANT] Löschen eine Azure SQL-Datenbank-Server-Instanz aller Datenbanken werden ebenfalls gelöscht und kann nicht wiederhergestellt werden. Gibt keine Unterstützung für das Wiederherstellen von gelöschten Server zu diesem Zeitpunkt.

Sie können den gleichen oder einen neuen Datenbanknamen für die wiederhergestellte Datenbank. [Azure-Portal](sql-database-restore-deleted-database-portal.md) [PowerShell](sql-database-restore-deleted-database-powershell.md) verwenden oder [REST (CreateMode = wiederherstellen)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Wiederherstellung der Datenbank gelöscht: Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [Wiederherstellung der Datenbank gelöscht: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>Geo-Wiederherstellung

Geo-Wiederherstellung können Sie eine SQL-Datenbank auf einem beliebigen Server in Azure Regionen aus der letzten Geo repliziert [Automatische tägliche Sicherung](sql-database-automated-backups.md)wiederherstellen. Geo-Wiederherstellung Geo-redundantes Backup als Quelle verwendet und kann zum Wiederherstellen einer Datenbank wird die Datenbank oder Datacenter aufgrund eines Ausfalls nicht zugegriffen werden. [Azure-Portal](sql-database-geo-restore-portal.md) [PowerShell](sql-database-geo-restore-powershell.md)verwenden oder [REST (CreateMode = Wiederherstellung)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [Geo-Wiederherstellung: Azure-portal](sql-database-geo-restore-portal.md)
- [Geo-Wiederherstellung: PowerShell](sql-database-geo-restore-powershell.md)

Geo-Wiederherstellung wird die standardmäßige Wiederherstellungsoption Ihrer Datenbank wegen eines Vorfalls in der Region nicht verfügbar ist, die Datenbank gehostet wird. Wenn eine umfangreiche Vorfall in einem Bereich Ausfall der datenbankanwendung können Geo wiederherstellen Sie eine Datenbank auf einem Server in eine andere Region aus der letzten Sicherung wiederherstellen. Alle Backups Geo repliziert und können eine Verzögerung zwischen die Sicherung aufgenommen und auf den Azure Geo repliziert wird Blob in einer anderen Region. Diese Verzögerung können bis zu einer Stunde im Katastrophenfall möglich von 1 Stunde von Datenverlust, d. h. RPO von 1 Stunde. Im folgenden wird die Wiederherstellung der Datenbank aus der letzten täglichen Sicherung.

![Geo-Wiederherstellung](./media/sql-database-geo-restore/geo-restore-2.png)

Ausführliche Informationen über geografische Wiederherstellung nach einem Ausfall wiederherstellen Siehe [Wiederherstellung nach einem Ausfall](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Geo-Wiederherstellung mit allen Dienstebenen ist, ist die einfachste der Disaster-Recovery-Lösung in SQL-Datenbank mit der längsten RPO Schätzung Recovery Time (Einfügen). Für einfache Datenbanken mit maximal 2 GB Geo-Wiederherstellung bietet eine angemessene DR-Lösung mit Einfügen von 12 Stunden. Bei größeren Standard oder Premium Datenbanken erheblich kürzere Recovery-Zeiten sind, oder um die Wahrscheinlichkeit eines Datenverlusts reduziert erwägen Sie aktive Geo-Replikation. Aktive Geo-Replikation bietet eine viel niedriger RPO und konvertieren Sie benötigt nur ein Failover auf die ständig replizierten sekundäre initiiert. Weitere Informationen finden Sie unter [Aktive Geo-Replikation](sql-database-geo-replication-overview.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Wiederherstellung mit automatischen Backups ausführen programmgesteuert

Wie erwähnt, im Addiition der Azure-Portal kann Datenbank programmgesteuert mit Azure PowerShell die REST-API durchgeführt werden. Die Tabellen unten beschreiben den Satz von Befehlen zur Verfügung.

### <a name="powershell"></a>PowerShell

|Cmdlets|Beschreibung|
|------|-----------|
|[AzureRmSqlDatabase abrufen](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Ruft eine oder mehrere Datenbanken.|
|[AzureRMSqlDeletedDatabaseBackup abrufen](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Ruft eine gelöschte Datenbank wiederherstellen können.|
|[AzureRmSqlDatabaseGeoBackup abrufen](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Ruft ein geometrische redundantes Backup einer Datenbank.|
|[Wiederherstellung AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Eine SQL-Datenbank wiederhergestellt.|
||||

### <a name="rest-api"></a>REST-API

|API|Beschreibung|
|---|-----------|
|[REST (CreateMode = Wiederherstellung)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Stellt eine Datenbank wieder her|
|[Erstellen oder Aktualisieren des Datenbankstatus](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Gibt den Status während einer Wiederherstellung|
||||



## <a name="summary"></a>Zusammenfassung

Automatische Backups schützen Sie Ihre Datenbanken Benutzer, Anwendungsfehler, versehentliche Datenbank löschen und längere Ausfälle. Diese Kosten NULL NULL-Admin-Lösung ist mit allen SQL-Datenbanken. 

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte backups](sql-database-automated-backups.md)
- Schnellere Recovery-Optionen finden Sie unter [Aktive geografische Replikation](sql-database-geo-replication-overview.md)  
- Archivierung mit automatischen Sicherungen finden Sie unter [Datenbank kopieren](sql-database-copy.md)
