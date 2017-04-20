<properties
    pageTitle="SQL Datenbank elastischen Pool Preis und Leistung"
    description="Preisinformationen für elastische datenbankpools."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Elastische Datenbank Pool Abrechnung und Preisinformationen

Elastische datenbankpools erfolgt die Abrechnung pro folgende Merkmale:

- Elastischer Pool wird bei seiner Erstellung abgerechnet, auch wenn keine Datenbanken im Pool.
- Elastischer Pool wird stündlich abgerechnet. Dies ist dieselbe Dosierung für Leistungsfähigkeit der einzelnen Datenbanken.
- Wenn elastischer Pool zu neuen eDTUs geändert wird, wird dann Pool nicht abgerechnet nach dem neuen Betrag eDTUS bis Größe abgeschlossen. Folgt das gleiche Muster wie die Leistung von eigenständigen Datenbanken ändern.


- Der Preis eines elastischen Pools anhand der Anzahl der eDTUs des Pools. Der Preis elastischen Pool ist unabhängig von der Anzahl und Nutzung der elastischen Datenbanken innerhalb.
- Preis berechnet (Anzahl der Pool-eDTUs) x (Einzelpreis pro eDTU).

EDTU Verkaufspreis für elastischen Pool ist höher als der DTU Verkaufspreis für eine Standalone-Datenbank in der gleichen Service. Details finden Sie in der [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/). 


EDTUs und Service-Ebenen finden Sie unter [SQL-Datenbank und Leistung](sql-database-service-tiers.md).

## <a name="next-steps"></a>Nächste Schritte

- [Elastische Datenbankpool erstellen](sql-database-elastic-pool-create-portal.md)
- [Überwachen und verwalten sowie einen elastischen Datenbankpool Größe](sql-database-elastic-pool-manage-portal.md)
- [SQL-Datenbank und Leistung: verstehen, was in jeder Dienstebene verfügbar](sql-database-service-tiers.md)
- [PowerShell-Skript zum Identifizieren einer elastischen Datenbankpool für Datenbanken](sql-database-elastic-pool-database-assessment-powershell.md)
