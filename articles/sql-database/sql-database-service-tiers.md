<properties
    pageTitle="SQL-Datenbank-Performance & Optionen: Service-Tiers | Microsoft Azure"
    description="Vergleichen Sie SQL Performance und Business Continuity Datenbankfunktionen von Dienstebenen, um Kosten und Funktionalität beim Skalieren."
    keywords="Datenbankoptionen, Datenbank-performance"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>SQL-Datenbank und Leistung: verstehen, was in jeder Dienstebene verfügbar

[Azure SQL-Datenbank](sql-database-technical-overview.md) bietet drei Service-Stufen mehrere Leistung unterschiedliche Arbeitslasten verarbeiten. Leistungsfähigen stellt eine zunehmende Ressourcen zunehmend höheren Durchsatz bietet. Sie können jede Datenbank im eigenen [Service-Tier](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) Performance Level verwalten. Sie können auch mehrere Datenbanken in einer [elastischen Pool](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) mit freigegebenen Ressourcen verwalten. Die verfügbaren Ressourcen für eigenständige Datenbanken werden für Datenbank Buchungseinheiten (DTUs) und flexible Pools elastischen DTUs oder eDTUs ausgedrückt. Weitere DTUs und eDTUs finden Sie unter [Was ist ein DTU](sql-database-what-is-a-dtu.md). 

In beiden Fällen enthalten die Dienstebenen **Basic**, **Standard**und **Premium**. Datenbankoptionen in diesen Stufen sind für eigenständige Datenbanken und elastische ähnlich, aber es gibt zusätzliche Aspekte für elastische Pools. Dieser Artikel enthält Details der Dienstebenen für eigenständige Datenbanken und elastisch.

## <a name="service-tiers-and-database-options"></a>Dienstebenen und Datenbankoptionen
Basic, Standard und Premium-Service-Ebenen haben eine Verfügbarkeit von 99,99 % SLA vorhersagbare Leistung, flexible Business Continuity-Optionen Sicherheitsfunktionen und stündlichen Abrechnung. Folgende Tabelle enthält Beispiele für Ebenen am besten geeignet für unterschiedliche Arbeitslasten.

| Service-tier | Geeignete Arbeitslasten |
|---|---|
| **Grundlegende** | Geeignet für eine kleine Datenbank unterstützen normalerweise einem einzelnen aktiven Vorgang zu einem bestimmten Zeitpunkt. Beispiele für Entwicklungs- oder Testzwecke verwendete Datenbanken oder kleine Anwendung selten verwendet. |
| **Standard** | Die Option Gehe zu den meisten Cloudanwendungen unterstützt mehrere gleichzeitige Abfragen. Arbeitsgruppe oder Web Applications gehören. |
| **Premium** | Entwickelt für hohen Transaktionsvolumen viele gleichzeitige Benutzer und Höchstmaß an Business Continuity-Funktionen erfordern. Beispiele sind Datenbanken unterstützen umwandeln. |

>[AZURE.NOTE] Web- und Business-Editionen werden deaktiviert. Lesen Sie den [Sonnenuntergang FAQ](https://azure.microsoft.com/pricing/details/sql-database/web-business/) , wenn Sie weiterhin Web und Business-Editionen verwenden.

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Eigenständige Datenbank Dienstebenen und Performance
Für eigenständige Datenbanken sind mehrere Performance innerhalb jeder Dienstebene. Sie haben die Flexibilität, die Ebene auszuwählen, die am besten Ihre Arbeitslast Anforderungen. Benötigen Sie nach oben oder unten skalieren, können Sie einfach die Ebenen der Datenbank ändern. Einzelheiten finden Sie unter [Ändern der Datenbank Dienstebenen und Performance](sql-database-scale-up.md) .

Hier aufgelistete Eigenschaften gelten für Datenbanken mit [SQL Datenbank V12](sql-database-v12-whats-new.md)erstellt. Unabhängig von der Anzahl der Datenbanken die Datenbank erhält garantierte Ressourcen und die erwarteten Leistungsmerkmale der Datenbank sind nicht betroffen.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Eine ausführliche Erläuterung der anderen Zeilen in dieser Tabelle-Ebenen finden Sie unter [Service-Tier-Funktionen und Grenzen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Elastische Pool Dienstebenen und Leistung eDTUs
Erstellen und eine eigenständige Datenbank skalieren, können Sie mehrere Datenbanken in einer [elastischen Pool](sql-database-elastic-pool.md)verwalten. Alle Datenbanken in einer elastischen Pool verfügen über gemeinsame Ressourcen. Die Leistungsmerkmale sind *Elastische Datenbank Buchungseinheiten* (eDTUs) gemessen. Wie eigenständige Datenbanken Pools in drei Dienstebenen: **Basic**, **Standard**und **Premium**. Für Pools definieren diese drei Ebenen noch die Gesamt-Performance sowie verschiedene Funktionen.

Pools können Datenbanken freigeben und DTU Ressourcen ohne eine Leistung jeder Datenbank im Pool zuweisen. Z. B. geht eine Standalone-Datenbank in einem Standard-Pool mit 0 eDTUs, maximale eDTU, die Sie beim Konfigurieren des Pools eingerichtet. Pools können mehrere Datenbanken mit unterschiedlichen Arbeitslasten gesamte Pool eDTU Ressourcen effizient nutzen. Einzelheiten Sie [und Hinweise für einen elastischen Pool](sql-database-elastic-pool-guidance.md) .

Die folgende Tabelle beschreibt die Merkmale der Pool Dienstebenen.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Jede Datenbank innerhalb eines Pools entspricht auch der eigenständigen Datenbank Merkmale für diese Ebene. Z. B. grundlegende Pool hat einen Grenzwert für die maximale Sessions pro Pool 4800 28800, aber eine einzelne Datenbank in einem grundlegenden Pool hat Datenbank maximal 300 Sessions.

## <a name="choosing-a-service-tier"></a>Wählen eine Service-Stufe

Um eine Dienstebene entscheiden zunächst feststellen, ob die Datenbank sollten eigenständige Datenbank oder Teil einer elastischen Pool. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Auswählen einer Dienstebene für eine Standalone-Datenbank

Starten Sie auf Dienstebene für eine eigenständige Datenbank entscheiden, Ermitteln der müssen Sie Ihre SQL Database Edition Datenbankfunktionen:

- Datenbankgröße (für Basic maximal 250 GB für Standard maximal 2 GB und 500 GB bis 1 TB maximum für Premium - je nach Leistung)
- Datenbank backup Aufbewahrungsdauer (7 Tage Basic 35 Tagen für Standard und 35 Tage Premium)

Nachdem Sie SQL Database Edition bestimmt haben, können Sie die Leistung der Datenbank (Anzahl DTUs) bestimmen. Erraten Sie und [Skalierung nach oben oder unten dynamisch](sql-database-scale-up.md) tatsächliche Erfahrung. [DTU Rechner](http://dtucalculator.azurewebsites.net/) können Sie ungefähre Anzahl der erforderlichen DTUs. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Auswählen einer Dienstebene für einen Datenbankpool elastische.

Starten Sie auf Dienstebene für einen Datenbankpool elastische entscheiden, durch Ermitteln der müssen Sie die Dienstebene für das Pool Datenbankfunktionen.

- Datenbankgröße (2 GB für Basic, Standard 250 GB und 500 GB für Premium)
- Datenbank backup Aufbewahrungsdauer (7 Tage Basic 35 Tagen für Standard und 35 Tage Premium)
- Anzahl der Datenbanken pro Pool (Basic 400, 400 für Standard und 50 Premium)
- Maximaler Speicher pro Pool (117 GB Basic, Standard, 1200 und 750 Premium)

Nachdem die Dienstebene für das Pool bestimmt haben, können Sie die Leistung für den Pool (eDTUs) zu ermitteln. Erraten Sie und dann [Vergrößern oder verkleinern dynamisch](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) auf Erfahrung. [DTU Rechner](http://dtucalculator.azurewebsites.net/) können Sie die ungefähre Anzahl der DTUs erforderlich, für jede Datenbank in einem Pool bestimmt die obere Grenze für den Pool.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die Preise für diese Ebenen [SQL Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/).
- Lernen Sie die Details des [elastischen Pools](sql-database-elastic-pool-guidance.md) und [Preis und Leistung für elastische Pools](sql-database-elastic-pool-guidance.md).
- Informationen zum [überwachen, verwalten und Größe elastische Pools](sql-database-elastic-pool-manage-portal.md) und [überwachen die Leistung eigenständige Datenbanken](sql-database-single-database-monitor.md).
- Jetzt wissen über SQL-Datenbankebene, probieren sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) und erstellen Sie [Ihre erste SQL-Datenbank](sql-database-get-started.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Entwurfsmuster für Multi-Tenant SaaS-Applikationen mit Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoft Virtual Academy video-Kurs auf elastische Datenbankfunktionen in Azure SQL-Datenbank](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
