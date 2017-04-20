<properties
   pageTitle="SQL Data Warehouse Backups | Microsoft Azure"
   description="Erfahren Sie mehr über SQL Data Warehouse integrierte Datenbank-Backups, die Sie einer Azure SQL Data Warehouse einer anderen geographischen Region zu einem Wiederherstellungspunkt wiederherstellen können."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>SQL Data Warehouse backups

SQL Data Warehouse bietet lokalen und geografische Backups als Teil seiner backup Data Warehouse-Funktionen. Dazu gehören Azure BLOB Storage-Snapshots und Geo-redundanten Speicher. Verwenden Sie Data Warehouse-Backups wiederherstellen Datawarehouse auf einen Wiederherstellungspunkt in der primären oder Wiederherstellen einer anderen geographischen Region. Dieser Artikel erläutert die Besonderheiten des Backups im SQL Data Warehouse.

## <a name="what-is-a-data-warehouse-backup"></a>Was ist ein Data Warehouse Backup?

Lagerort Sicherungskopien der Daten sind Daten, die Sie verwenden können, um ein Datawarehouse zu einem bestimmten Zeitpunkt wiederherzustellen.  Da SQL Data Warehouse ein verteiltes System, besteht ein Data Warehouse Backup viele Dateien, die in Azure-Blobs gespeichert sind. 

Datenbank-Backups sind Bestandteil jeder Business Continuity und Disaster Recovery-Strategie, da sie Ihre Daten vor versehentlichen Beschädigung oder Löschung schützen. Weitere Informationen finden Sie unter [Übersicht über Business Continuity](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Datenredundanz

SQL Data Warehouse schützt Ihre Daten durch Speichern von Daten in lokal redundant (LRS) Azure Premium-Speicher. Dieses Feature Azure Storage speichert synchrone Kopien der Daten im lokalen Rechenzentrum transparenten Datenschutz sicherzustellen, wenn lokalisierte Fehler auftreten. Datenredundanz wird sichergestellt, dass Daten infrastrukturthemen Festplattenfehler überstehen. Datenredundanz gewährleistet Business Continuity mit einer Fehlertoleranz und hochverfügbare Infrastruktur.

Weitere Informationen zu:

- Azure Premium-Speicher finden Sie unter [Einführung in Azure Premium-Speicher](../storage/storage-premium-storage.md).
- Lokal redundanter Speicher finden Sie unter [Azure Storage Replication](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Azure BLOB-Speicher-snapshots

Azure Premium Speicher nutzen verwendet SQL Data Warehouse Azure Storage Blob Snapshots lokal Datawarehouse sichern. Sie können ein Datawarehouse zu einem Snapshot Wiederherstellungspunkt wiederherstellen. Snapshots starten mindestens alle acht Stunden und 7 Tage zur Verfügung.  

Weitere Informationen zu:

- Azure BLOB-Snapshots finden Sie unter [Erstellen einer BLOB-Snapshot](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>Geo-redundanter backups

24 Stunden SQL Data Warehouse vollständiges Datawarehouse im Standardspeicher gespeichert. Vollständiges Datawarehouse wird entsprechend die Zeit des letzten Snapshots erstellt. Der Standardspeicher gehört Geo redundante Speicherkonto mit Lesezugriff (RA-GRS). Azure Storage RA-GRS-Funktion repliziert die Sicherungsdateien [gepaarten Rechenzentrum](../best-practices-availability-paired-regions.md). Diese Geo-Replikation stellt sicher, dass Sie Datawarehouse wiederherstellen können, falls Snapshots Ihre primäre Region zugreifen kann. 

Diese Funktion ist standardmäßig aktiviert. Wenn Sie geometrische redundanter Backups verwenden möchten, können Sie sich abmelden. 

>[AZURE.NOTE] In Azure-Speicher bezieht sich der Begriff *Replikation* zum Kopieren von Dateien von einem Speicherort zu einem anderen. SQL *Replikation* bezieht sich auf sekundäre Datenbanken mit einer primären Datenbank synchronisiert. 

Weitere Informationen zu:
- Geo-redundanten Speicher finden Sie unter [Azure Storage Replication](../storage/storage-redundancy.md).
- RA-GRS Speicher finden Sie unter [Geo-redundant Storage Lesezugriff](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Data warehouse-Sicherungszeitplan und Aufbewahrungszeitraum

SQL Data Warehouse erstellt Snapshots auf Ihre Online-Datawarehouses alle vier bis acht Stunden und hält jeder Snapshot für sieben Tage. Sie können Ihre online-Datenbank eines Wiederherstellungspunkte in den letzten sieben Tagen wiederherstellen. 

Beginn der letzte Snapshot finden diese Abfrage auf online SQL Data Warehouse. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Benötigen Sie eine Momentaufnahme länger als sieben Tage beibehalten, können Sie ein neues Datawarehouse Wiederherstellungspunkt wiederherstellen. Nachdem die Wiederherstellung abgeschlossen ist, startet SQL Data Warehouse Snapshots auf neuen Datawarehouse erstellen. Wenn Sie das neue Datawarehouse ändern nicht, Snapshots leer bleiben und daher die Snapshot-Kosten minimal. Um SQL Data Warehouse Erstellen von Snapshots die Datenbank konnte auch angehalten werden.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Was geschieht mit meiner backup Aufbewahrung Meine Datawarehouse anhalten?

SQL Data Warehouse erstellt keine Snapshots und Snapshots ein Datawarehouses anhalten nicht abläuft. Alter Snapshot anhalten Datawarehouse nicht geändert. Snapshot Aufbewahrung basiert auf der Anzahl der Tage ist das Datawarehouse online nicht Kalendertage.

Ein Snapshot beginnt um 16 Uhr 1. Oktober und Datawarehouse angehalten Oktober-3: 00 Uhr ist der Snapshot zwei Tage alt. Wenn das Datawarehouse wieder online kommt ist der Snapshot zwei Tage alt. Kommt das Datawarehouse online Oktober 5: 00 Uhr, der Snapshot ist zwei Tage und für fünf Tage.

Das Datawarehouse Onlinemodus SQL Data Warehouse setzt neue Snapshots und Snapshots läuft ab, wenn sie Daten von mehr als sieben Tage sind.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Wie lange dauert die Aufbewahrungsdauer für verworfene Datawarehouse?
Beim Ablegen eines Datawarehouses das Datawarehouse und die Snapshots sieben Tage lang gespeichert und anschließend entfernt. Sie können das Datawarehouse auf die Wiederherstellungspunkte wiederherstellen.

> [AZURE.IMPORTANT] Löschen eine logische SQL Server-Instanz alle Datenbanken in der Instanz gehören ebenfalls gelöscht und können nicht wiederhergestellt werden. Sie können keinen gelöschten Server wiederherstellen.

## <a name="data-warehouse-backup-costs"></a>Data Warehouse-backup-Kosten

Die Gesamtkosten für die primären Datawarehouse und sieben Tage Azure BLOB-Snapshots wird auf die nächste TB gerundet. Beispielsweise werden Wenn das Datawarehouse 1,5 TB ist und Snapshots 100 GB, für 2 TB bei Azure Premium Speicher berechnet. 

>[AZURE.NOTE] Jeder Snapshot ist zunächst leer und wächst mit dem primären Datawarehouse ändern. Alle Snapshots Zunahme der Größe als Data Warehouse ändern. Daher wächst Speicherkosten für Snapshots entsprechend ändern.

Bei Verwendung von Geo-redundanten Speicher erhalten eine separaten Gebühr. Geo-redundanten Speicher wird der Standardsatz Lesezugriff geografisch redundanten Speicher (RA-GRS) abgerechnet.

Weitere Informationen zu Preisen SQL Data Warehouse finden Sie unter [SQL Data Warehouse Preise](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Mithilfe von Datenbank-backups

Der primäre SQL Data Warehouse Backups wird Datawarehouse innerhalb der Aufbewahrungsdauer eines Wiederherstellungspunkte wiederherstellen.  

- Um ein SQL Datawarehouse wiederherzustellen, finden Sie unter [Wiederherstellen einer SQL Datawarehouse](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Verwandte Themen

### <a name="scenarios"></a>Szenarien

- Eine Übersicht über Business Continuity finden Sie unter [Business Continuity – Überblick](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

- Um ein Datawarehouse wiederherzustellen, finden Sie unter [Wiederherstellen einer SQL Datawarehouse](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

