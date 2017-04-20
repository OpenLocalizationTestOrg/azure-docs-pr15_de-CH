<properties
    pageTitle="SQL Azure-Datenbank Ressourcengrenzen"
    description="Diese Seite beschreibt einige allgemeine Ressourcengrenzen für Azure SQL-Datenbank."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Azure SQL-Datenbank Ressourcengrenzen

## <a name="overview"></a>Übersicht

Azure SQL-Datenbank verwaltet eine Datenbank mithilfe zweier verschiedener Mechanismen zur Ressourcen: **Ressourcen Governance** und **Durchsetzung von Grenzen**. Dieses Thema erläutert diese zwei Hauptbereiche Ressourcenmanagement.

## <a name="resource-governance"></a>Ressource-governance
Eines der Entwicklungsziele der Dienstebenen Basic, Standard und Premium ist für Azure SQL-Datenbank die Datenbank auf einem eigenen Computer vollständig isoliert von anderen Datenbanken läuft verhält. Ressource Governance emuliert dieses Verhalten. Die aggregierten Ressourcenverwendung maximal verfügbare CPU, Speicher, e/a-Protokoll und Daten-e/a-Ressource zur Datenbank erreicht, wird Ressource Governance Abfragen bei Ausführung in die Warteschlange und Warteschlange Abfragen als freigegeben Ressourcen zuordnen.

Als auf einem dedizierten Computer führt alle verfügbaren Ressourcen zu längeren Ausführung derzeit ausgeführte Abfragen Befehl Timeouts auf dem Client führen können. Mit aggressiven Wiederholungslogik und Applikationen, die Abfragen an die Datenbank mit einer hohen Frequenz ausführen können Fehlermeldungen auftreten, wenn neue Abfragen ausführen maximal gleichzeitige Anfragen erreicht.

### <a name="recommendations"></a>Empfehlung:
Überwachen der Ressourcenverwendung sowie die durchschnittlichen Antwortzeiten von Abfragen als die maximale Auslastung einer Datenbank. Wenn höhere Abfrage Wartezeiten in der Regel haben drei Optionen:

1.  Weniger Anfragen zur Datenbank Timeout und Stapel von Anfragen.

2.  Weisen Sie eine höhere Leistung mit der Datenbank.

3.  Optimieren Sie Abfragen, um die Ressourcenverwendung jeder Abfrage verringern. Weitere Informationen finden Sie im Abschnitt Abfrage optimieren/Hinting Artikel Leitfaden zur Leistung von Azure SQL-Datenbank.

## <a name="enforcement-of-limits"></a>Durchsetzung von Grenzwerten
Ressourcen als CPU, Speicher, Protokoll-e/a und e/a-Daten werden durch neue Anfragen verweigern, wenn die Grenzwerte erreicht werden erzwungen. Kunden erhalten eine [Fehlermeldung](sql-database-develop-error-messages.md) der Grenzwert, die erreicht wurde.

Beispielsweise beschränken die Anzahl der Verbindungen mit einer SQL-Datenbank sowie die Anzahl der gleichzeitigen Anfragen verarbeitet werden können. SQL-Datenbank kann die Anzahl der Verbindungen zur Datenbank größer als die Anzahl der gleichzeitigen Anfragen Verbindungspooling unterstützen. Während die Anzahl der verfügbaren Verbindungen einfach von der Anwendung gesteuert werden kann, ist die parallele Anfragen häufig schwieriger zu schätzen und zu steuern. Besonders während der Spitzenbelastungszeiten beim entweder sendet zu viele Anfragen oder der Datenbank erreicht die Ressource begrenzt und startet Arbeitsthreads an länger ausgeführten Abfragen ansammeln können Fehler sein.

## <a name="service-tiers-and-performance-levels"></a>Dienstebenen und Performance

Gibt es für beide eigenständige Datenbank und elastisch Dienstebenen und Performance.

### <a name="standalone-databases"></a>Eigenständige Datenbanken

Für eine Standalone-Datenbank werden die Grenzen einer Datenbank von Service-Tier und Leistung Datenbankebene definiert. Die folgende Tabelle beschreibt die Merkmale der Basic, Standard und Premium Datenbanken unterschiedliche Leistungsmerkmale.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Elastische pools

[Elastische Pools](sql-database-elastic-pool.md) gemeinsam genutzter Ressourcen in Datenbanken im Pool. Die folgende Tabelle beschreibt die Merkmale der Basic, Standard und Premium elastische datenbankpools.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Eine erweiterte Definition jeder Ressource in der vorherigen Tabelle aufgeführten finden Sie unter der Beschreibung [Service-Tier-Funktionen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)und Grenzen. Überblick Dienstebenen finden Sie unter [Azure SQL Datenbank Dienstebenen und Performance](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Andere SQL-Datenbank-Grenzwerte

| Bereich | Grenzwert | Beschreibung |
|---|---|---|
| Exportieren von Datenbanken mit automatisierten pro Abonnement | 10 | Automatisierte können Sie einen benutzerdefinierten Zeitplan für die Sicherung von SQL-Datenbanken erstellen. Weitere Informationen finden Sie unter [SQL-Datenbanken: Unterstützung für automatische SQL-Datenbank exportiert](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Pro server | Bis zu 5.000 | Bis zu 5000 Datenbanken pro Server auf V12-Servern zulässig. |  
| DTUs pro server | 45000 | 45000 DTUs sind pro Server auf Servern bereitstellen Datenbanken elastische Pools und Datawarehouses V12 verfügbar. |



## <a name="resources"></a>Ressourcen

[Azure-Abonnement und Service Grenzen, Kontingente und Integritätsregeln](../azure-subscription-service-limits.md)

[SQL Azure-Datenbank Dienstebenen und Performance](sql-database-service-tiers.md)

[Fehlermeldungen für SQL-Datenbank-Clientprogramme](sql-database-develop-error-messages.md)
