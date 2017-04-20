<properties
   pageTitle="SQL Azure-Datenbank erstellt Multi-Tenant-Apps mit Isolierung und Effizienz"
   description="Erfahren Sie, wie SQL-Datenbank Multi-Tenant-apps erstellt"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Multi-Tenant-Apps mit SQL Azure-Datenbank mit Isolierung und Effizienz erstellt

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Elastische Pools nutzen und effizienter Multi-Tenant-apps erstellen

Bist du eine SaaS-Dev Multi-Tenant-Anwendung schreiben und viele behandeln, stellen Sie Kompromisse bei Leistung, Verwaltung und Sicherheit. Mit Azure SQL-Datenbank elastische Datenbank müssen Sie nicht mehr Kompromiss. Diese Pools können Sie verwalten, Multi-Tenant-apps überwachen und Vorteile der Isolation von einem Kunden pro Datenbank. [Entwurfsmuster für Multi-Tenant SaaS-Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)anzeigen

![Build-Multi-Tenant-apps](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automatische Skalierung steuern

Automatisches Skalieren Pools Leistung und Speicherkapazität für elastische Datenbanken dynamisch. Sie können steuern die Leistung in einem Pool zugewiesen, hinzufügen oder Entfernen von elastischen Datenbanken bei Bedarf und Leistung elastische Datenbanken ohne die Gesamtkosten des Pools definieren. Dies bedeutet, dass Sie nicht verwalten die Verwendung der einzelnen Datenbanken.

[Lesen Sie die Dokumentation](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>Intelligentes Management Ihrer Umgebung

Integrierte Größe Recommendations identifizieren proaktiv Datenbanken aus profitieren würden. Diese Vorschläge können "Was-wenn-Analyse für schnellen Optimierung der Performance-Ziele. Historisch Pool Nutzung Visualisierung Rich Performance-Überwachung und Problembehandlung Dashboards helfen.

[Lesen Sie die Dokumentation](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>Leistung und Preis für Ihre Bedürfnisse

Basic, Standard und Premium-Pools bieten ein breites Spektrum von Leistung, Speicher und Preismodelle. Pools können bis zu 400 elastische Datenbanken enthalten. Elastische Datenbanken können automatische Skalierung bis 1000 elastische Datenbank Buchungseinheiten (eDTU).

[Lesen Sie die Dokumentation](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Flexible tools

Neben elastische Pools gibt SQL Datenbank-Funktionen zur Verwaltung der betriebliche Aktivitäten in mehreren Datenbanken:

**Datenbankübergreifende Abfragen und Berichte ausführen.**  
[Elastische Abfrage](sql-database-elastic-query-overview.md) können Sie Abfragen oder Berichte über Datenbanken im elastischen Pool und entfernte Daten in vielen Datenbanken Ihres gleichzeitig zugreifen.

**Führen Sie Cross Datenbanktransaktionen.**  
[Elastische Datenbanktransaktionen](sql-database-elastic-transactions-overview.md) können Transaktionen ausführen, die mehrere Datenbanken in SQL-Datenbanken und Operationen (z. B. beim Verarbeiten von finanzieller Transaktionen in Datenbanken oder Bestand in einer Datenbank und Aufträge aktualisieren).

**Führen Sie dieselben Operationen für mehrere Datenbanken.**  
[Elastische Datenbankaufträge](sql-database-elastic-jobs-overview.md) ausführen Verwaltungsvorgänge oder Neuerstellen von Indizes für jede Datenbank im elastischen Pool aktualisieren.

Gehen Sie zur Homepage zu sehen, was andere SQL-Datenbank an.
[Probieren Sie es](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Nächste Schritte

Ein [Azure-Testversion](https://azure.microsoft.com/get-started/) und [Erstellen Sie Ihre erste Azure SQL-Datenbank](sql-database-get-started.md)zu erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Entdecken Sie die [Funktionen der SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).
 
Lesen Sie die [Technische Übersicht der SQL-Datenbank](sql-database-technical-overview.md).  
