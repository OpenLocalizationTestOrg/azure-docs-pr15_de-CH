<properties
   pageTitle="Migrieren Sie vorhandene Azure SQL Data Warehouse Premium Speicher | Microsoft Azure"
   description="Anleitung für die Migration ein vorhandenes SQL Data Warehouse zu Premium-Speicher"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Migration zu Premium Speicherdetails
SQL Data Warehouse stellte kürzlich [Premium-Speicher für mehr Leistung Vorhersagbarkeit][].  Wir können nun vorhandene Data Warehouses auf Standardspeicher Premium Speicher migrieren.  Lesen Sie weitere Details wie und wann automatische Migration auftreten und selbst migrieren möchten Sie steuern die Ausfallzeit tritt.

Haben Sie mehr als ein Datawarehouse verwenden Sie unten [Automatische Migration planen][] , wann es ebenfalls migriert werden.

## <a name="determine-storage-type"></a>Speicher ermitteln
Erstellt eine DW vor dem Datum unten verwenden Sie derzeit Standardspeicher.  Jedes Data Warehouse auf automatische Migration unterliegt Standardspeicher hat eine Benachrichtigung am oberen Rand der Data Warehouse-Blade in [Azure-Portal][] mit der Aufschrift "*ein späteres Upgrade auf Premium-Speicher einen Ausfall benötigen.  Erfahren Sie mehr ->*. "

| **Region**          | **Vor diesem Datum erstellten DW**   |
| :------------------ | :-------------------------------- |
| Australien OST      | Premium-Speicher noch nicht verfügbar |
| Australien Südost | 5. August 2016                    |
| Brasilien Süd        | 5. August 2016                    |
| Kanada zentral      | 25 Mai 2016                      |
| Kanada Osten         | 26 Mai 2016                      |
| USA          | 26 Mai 2016                      |
| China-OST          | Premium-Speicher noch nicht verfügbar |
| China North         | Premium-Speicher noch nicht verfügbar |
| Ostasien           | 25 Mai 2016                      |
| Osten der USA             | 26 Mai 2016                      |
| Ost US2            | 27. Mai 2016                      |
| Indien Central       | 27. Mai 2016                      |
| Indien Süd         | 26 Mai 2016                      |
| Indien West          | Premium-Speicher noch nicht verfügbar |
| Japan OST          | 5. August 2016                    |
| Japan West          | Premium-Speicher noch nicht verfügbar |
| Norden der USA – zentral    | Premium-Speicher noch nicht verfügbar |
| Nordeuropa        | 5. August 2016                    |
| Südlichen zentralen USA    | 27. Mai 2016                      |
| Südostasien      | 24 Mai 2016                      |
| Westeuropa         | 25 Mai 2016                      |
| Westen der USA – zentral     | 26 August 2016                   |
| Westen der USA             | 26 Mai 2016                      |
| West US2            | 26 August 2016                   |

## <a name="automatic-migration-details"></a>Details zur automatischen migration
Standardmäßig migrieren wir Ihre Datenbank für Sie bei 6 Uhr und 6.00 Uhr Ortszeit des Bereichs während des [automatischen Migrationszeitplan][] unter.  Vorhandene Data Warehouse werden während der Migration nicht verwendet werden.  Wir schätzen, dass die Migration etwa eine Stunde pro TB Speicherkapazität pro Data Warehouse dauert.  Es wird außerdem sichergestellt, dass Sie während eines Teils der automatische Migration nicht belastet werden.

> [AZURE.NOTE] Nicht werden vorhandene Data Warehouse während der Migration verwenden.  Sobald die Migration abgeschlossen ist, wird das Data Warehouse wieder online sein.

Nachfolgend sind die Schritte, dass Microsoft in Ihrem Auftrag für die Migration und Beteiligung Ihrerseits erfordert.  Angenommen Sie in diesem Beispiel wird, Ihre vorhandenen DW Standardspeicher derzeit "MyDW" lautet

1.  Microsoft wird "MyDW", "MyDW_DO_NOT_USE_ [Timestamp]"
2.  Microsoft hält "MyDW_DO_NOT_USE_ [Timestamp]."  Während dieser Zeit ist eine Sicherung erstellt.  Mehrere Pause-setzt möglicherweise angezeigt werden, wenn es dabei Probleme auftreten.
3.  Erstellt eine neue DW mit dem Namen "MyDW" auf Premium-Speicher aus der Sicherung in Schritt 2 Microsoft.  "MyDW" wird erst angezeigt, nachdem die Wiederherstellung abgeschlossen ist.
4.  Sobald die Wiederherstellung abgeschlossen ist, gibt "MyDW" dem gleichen DWUs und angehalten oder aktiven Zustand vor der Migration.
5.  Sobald die Migration abgeschlossen ist, löscht Microsoft "MyDW_DO_NOT_USE_ [Timestamp]"
    
> [AZURE.NOTE] Diese Werte werden als Teil der Migration nicht übertragen:
> 
>   -  Überwachung auf Datenbankebene muss erneut aktiviert werden
>   -  Firewall-Regeln auf **der Datenbankebene** wieder hinzugefügt werden müssen.  Firewall-Regeln auf **Serverebene** sind nicht betroffen.

### <a name="automatic-migration-schedule"></a>Automatische Migration planen
Automatische Migration auftreten von 6 – 6 Uhr (lokale Zeit pro Region) bei folgenden Ausfall Zeitplan.

| **Region**          | **Vorkalkuliertes Startdatum**     | **Vorkalkuliertes Enddatum**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Australien OST      | Noch festgelegt nicht           | Noch festgelegt nicht           |
| Australien Südost | 10. August 2016              | 24 August 2016              |
| Brasilien Süd        | 10. August 2016              | 24 August 2016              |
| Kanada zentral      | 23. Juni 2016                | 1. Juli 2016                 |
| Kanada Osten         | 23. Juni 2016                | 1. Juli 2016                 |
| USA          | 23. Juni 2016                | 4. Juli 2016                 |
| China-OST          | Noch festgelegt nicht           | Noch festgelegt nicht           |
| China North         | Noch festgelegt nicht           | Noch festgelegt nicht           |
| Ostasien           | 23. Juni 2016                | 1. Juli 2016                 |
| Osten der USA             | 23. Juni 2016                | 11 Juli 2016                |
| Ost US2            | 23. Juni 2016                | 8. Juli 2016                 |
| Indien Central       | 23. Juni 2016                | 1. Juli 2016                 |
| Indien Süd         | 23. Juni 2016                | 1. Juli 2016                 |
| Indien West          | Noch festgelegt nicht           | Noch festgelegt nicht           |
| Japan OST          | 10. August 2016              | 24 August 2016              |
| Japan West          | Noch festgelegt nicht           | Noch festgelegt nicht           |
| Norden der USA – zentral    | Noch festgelegt nicht           | Noch festgelegt nicht           |
| Nordeuropa        | 10. August 2016              | 31. August 2016              |
| Südlichen zentralen USA    | 23. Juni 2016                | 2. Juli 2016                 |
| Südostasien      | 23. Juni 2016                | 1. Juli 2016                 |
| Westeuropa         | 23. Juni 2016                | 8. Juli 2016                 |
| Westen der USA – zentral     | 14 August 2016              | 31. August 2016              |
| Westen der USA             | 23. Juni 2016                | 7 Juli 2016                 |
| West US2            | 14 August 2016              | 31. August 2016              |

## <a name="self-migration-to-premium-storage"></a>Automatische Migration auf Premium-Speicher
Möchten Sie steuern, wann Ihre Ausfallzeiten auftreten, können die folgenden Schritte Sie ein vorhandenes Data Warehouse Standardspeicher Premium Speicher migrieren.  Wenn Sie selbst migrieren, Sie müssen selbst Migration vor Beginn der automatische Migration in diesem Bereich die automatische Migration verursacht einen Konflikt zu vermeiden (siehe [Automatische Migration planen][]).

### <a name="self-migration-instructions"></a>Automatische Migration Anleitung
Möchten Sie Ihre Ausfallzeiten steuern, können Sie das Data Warehouse mit Backup und Wiederherstellung selbst migrieren.  Wiederherstellungsteil der Migration wird etwa eine Stunde pro TB Speicherkapazität pro DW zu erwarten.  Möchten Sie nach Abschluss der Migration den Namen beizubehalten, führen Sie die Schritte für [Schritte bei der Migration umbenennen][]. 

1.  [Anhalten][] der DW führt eine automatische Sicherung
2.  [Wiederherstellen][] von der letzten snapshot
3.  Löschen Sie die vorhandene DW Standardspeicher. **Wenn Sie diesen Schritt nicht, Sie für beide DWs berechnet.**

> [AZURE.NOTE] Diese Werte werden als Teil der Migration nicht übertragen:
> 
>   -  Überwachung auf Datenbankebene muss erneut aktiviert werden
>   -  Firewall-Regeln auf **der Datenbankebene** wieder hinzugefügt werden müssen.  Firewall-Regeln auf **Serverebene** sind nicht betroffen.

#### <a name="optional-steps-to-rename-during-migration"></a>Optional: Schritte bei der Migration umbenennen 
Zwei Datenbanken auf demselben logischen Server können nicht denselben Namen haben. SQL Data Warehouse unterstützt jetzt eine DW umbenennen.

Angenommen Sie in diesem Beispiel wird, Ihre vorhandenen DW Standardspeicher derzeit "MyDW" lautet

1.  Umbenennen Sie "MyDW" des Befehls ALTER DATABASE folgt etwas wie "MyDW_BeforeMigration"  Dieser Befehl bricht alle vorhandene Transaktionen und in der master-Datenbank erfolgreich vorgenommen werden.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Anhalten][] "MyDW_BeforeMigration" führt eine automatische Sicherung
3.  [Wiederherstellen][] von der letzten snapshot eine neue Datenbank mit dem Namen verwendet haben (ex: "MyDW")
4.  "MyDW_BeforeMigration" löschen.  **Wenn Sie diesen Schritt nicht, Sie für beide DWs berechnet.**

> [AZURE.NOTE] Diese Werte werden als Teil der Migration nicht übertragen:
> 
>   -  Überwachung auf Datenbankebene muss erneut aktiviert werden
>   -  Firewall-Regeln auf **der Datenbankebene** wieder hinzugefügt werden müssen.  Firewall-Regeln auf **Serverebene** sind nicht betroffen.

## <a name="next-steps"></a>Nächste Schritte
Sich Premium-Speicher haben wir auch die Anzahl der BLOB-Dateien in die zugrunde liegende Architektur des Data Warehouse erhöht.  Maximieren Sie die Leistungsvorteile der Änderung empfiehlt die Indizes Columnstore Cluster mit dem folgenden Skript neu zu erstellen.  Das folgende Skript wird die vorhandenen Daten zusätzliche Blobs erzwingen.  Wenn keine Aktion werden die Daten als weitere Daten in die Datawarehouse-Tabellen zu laden natürlich langfristig verteilen.

**Erforderliche Komponenten:**

1.  Datawarehouse läuft mit 1.000 DWUs oder höher (siehe [Skalieren Compute Stromversorgung][])
2.  Benutzer das Skript sollte [Mediumrc Rolle][] oder höher sein.
    1.  Um dieser Rolle einen Benutzer hinzufügen möchten, führen Sie Folgendes aus: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Wenn Probleme mit Data Warehouse, [supportticket erstellen][] und Verweis "Migration zu Premium Storage" als mögliche Ursache auftreten.

<!--Image references-->

<!--Article references-->
[Automatische Migration planen]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[supportticket erstellen]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Anhalten]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Wiederherstellen]: sql-data-warehouse-restore-database-portal.md
[Schritte bei der Migration umbenennen]: #optional-steps-to-rename-during-migration
[rechenleistung skalieren]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[Mediumrc-Rolle]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Premium-Speicher für mehr Leistung Vorhersagbarkeit]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure-Portal]: https://portal.azure.com
