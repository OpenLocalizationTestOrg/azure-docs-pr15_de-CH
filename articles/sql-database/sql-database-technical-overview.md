<properties
    pageTitle="Was ist SQL-Datenbank? Einführung in SQL Datenbank | Microsoft Azure"
    description="Einführung in SQL-Datenbank: technische Details und Funktionen von Microsoft relationalen Datenbank-Managementsystem (RDBMS) in der Cloud."
    keywords="Einführung in Sql, Einführung in Sql, was ist Sql-Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Was ist SQL-Datenbank? Einführung in SQL-Datenbank

SQL-Datenbank ist eine relationale Datenbank-Service in der Cloud basierend auf der führenden Microsoft SQL Server Engine mit wichtigen Funktionen. SQL-Datenbank bietet eine berechenbare Performance, Skalierbarkeit ohne Ausfallzeiten, Business Continuity und Datenschutz – mit nahezu null-Verwaltung. Sie können auf schnelle Anwendungsentwicklung und Verkürzung der Markteinführungszeiten statt Verwalten von virtuellen Maschinen und Infrastruktur konzentrieren. Da der [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) -Engine basiert, unterstützt SQL Datenbank vorhandenen SQL Server-Tools, Bibliotheken und APIs verschieben und in die Cloud Erweitern einfacher.

Dieser Artikel ist eine Einführung in die Grundbegriffe der SQL-Datenbank und Funktionen auf Performance, Skalierung und Verwaltung mit Links zu Informationen. Wenn Sie springen möchten, können Sie [die erste SQL-Datenbank erstellen](sql-database-get-started.md) oder [erstellen einen Datenbankpool elastische](sql-database-elastic-pool-create-portal.md) in Minuten. Eingehender gegebenenfalls dieses 30 Minuten Video.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>Anpassen von Leistung und ohne Ausfallzeiten

SQL-Datenbanken steht in Basic, Standard und Premium- *Service-Tiers hinweg*. Jedes Service-Tier bietet [verschiedene Arbeitslasten](sql-database-service-tiers.md) Leichtgewicht Heavyweight Datenbank Arbeitslasten zu unterstützen. Sie können Ihre erste Anwendung auf eine kleine Datenbank für einige Dollar pro Monat, dann [Ändern Sie die Dienstebene](sql-database-scale-up.md) manuell oder programmgesteuert jederzeit erstellen wie Ihre app virale weltweit ohne Ausfallzeiten für Ihre Anwendung oder Ihre Kunden.

Für viele Unternehmen und apps ist Datenbanken erstellen und wählen Datenbank-Performance bei Bedarf nach oben oder unten, besonders wenn Verwendungsmuster vorhersehbaren. Aber haben Sie unvorhersehbare Verwendungsmuster, es kann machen es schwer zu Kosten und Ihr Geschäftsmodell.

[Elastische Pools](sql-database-elastic-pool.md) in SQL Datenbank löst dieses Problem. Das Konzept ist einfach. Sie zu einem Pool zuweisen und Zahlen für die kollektive Leistung des Pools als einzelne Datenbank-Performance. Sie müssen wählen Sie Datenbank-Performance nach oben oder unten. Datenbanken im Pool aufgerufen *elastische Datenbanken*skalieren automatisch und auf Anforderung erfüllen. Elastische Datenbanken nutzen aber nicht überschreiten den Pool Kosten vorhersehbar bleibt, auch wenn Datenbankverwendung nicht. Können Sie [Hinzufügen und Entfernen von Datenbanken zum Pool](sql-database-elastic-pool-manage-portal.md), skalieren Ihre app von wenigen Datenbanken Tausendertrennzeichen in einem Sie steuern. Erfahren Sie mehr über Entwurfsmuster für SaaS-Applikationen mit elastischen Pools finden Sie unter [Entwurfsmuster für Multi-Tenant SaaS mit Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Oder Sie gehen so – einzelne oder elastischen – Sie sind nicht gesperrt. Sie können einzelne Datenbanken mit elastische Datenbank angeglichen und Dienstebenen Pools innovative Design zu erstellen und einzelne Datenbanken ändern. Außerdem können mit der Leistung und Reichweite von Azure-Sortimente Azure Services mit SQL-Datenbank Ihre Bedürfnisse eindeutig moderne Anwendung entwerfen, Kosten und Ressourcen Effizienz und neue Geschäftschancen zu entsperren.

Aber wie können Sie die relative Leistung von Datenbanken und datenbankpools? Woher wissen Sie den Rechtsklick beenden, wenn oben und unten wählen? Die Antwort lautet Datenbank Transaktion Einheit (DTU) für einzelne Datenbanken und elastischen DTU (eDTU) für elastische Datenbanken und Datenbank. Siehe [SQL-Datenbank und Leistung: verstehen, was jeder Dienstebene für](sql-database-service-tiers.md) Informationen.

## <a name="keep-your-app-and-business-running"></a>Ihre app und Betrieb

Azure branchenweit führenden 99,99 % Verfügbarkeit Vereinbarung zum Servicelevel [(SLA)](http://azure.microsoft.com/support/legal/sla/)ein globales Netzwerk von Microsoft verwaltete Rechenzentren angetrieben trägt Ihre app 24/7 ausgeführt. Mit jeder Datenbank SQL profitieren Sie von integrierten Datenschutz, Fehlertoleranz und Schutz von Daten, die Sie sonst entwerfen, kaufen, erstellen und verwalten. Je nach Anforderung Ihres Unternehmens verlangen Sie, zusätzliche Schutzschichten um sicherzustellen, dass Ihre Anwendung und Ihr Unternehmen bei einem Notfall, Fehler oder etwas schnell wiederherstellen können. SQL-Datenbank bietet jeder Dienstebene ein anderes Menü um verwendbare ausgeführt und bleiben so. Point-in-Time-Wiederherstellung können Sie eine Datenbank in einem früheren Zustand bereits 35 Tagen zurück. Wenn das Rechenzentrum hosten die Datenbanken ein Ausfall auftritt, können Sie außerdem Failover Datenbankreplikate in einer anderen Region. Oder Sie können Replikate für Upgrades oder Verlagerung auf die verschiedenen Bereiche.

![SQL Datenbank Geo-Replikation](./media/sql-database-technical-overview/azure_sqldb_map.png)


Details zu den verschiedenen Business-Continuity-Funktionen für verschiedene Dienstebenen finden Sie unter [Business Continuity](sql-database-business-continuity.md) .

## <a name="secure-your-data"></a>Sichern Sie Ihre Daten
SQL Server hat eine Tradition solide Sicherheit, SQL-Datenbank mit Funktionen, die Zugriff unterstützt, Datenschutz, und überwachen. Ein kurzer Überblick Sicherheitsoptionen haben in SQL-Datenbank finden Sie unter [Sichern der SQL-Datenbank](sql-database-security.md) . Finden Sie im [Security Center für SQL Server-Datenbankmodul und der SQL-Datenbank](https://msdn.microsoft.com/library/bb510589) für eine umfassendere Ansicht Sicherheitsfunktionen. Und [Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/) Informationen zum Azure Plattform.

## <a name="next-steps"></a>Nächste Schritte
Haben Sie eine Einführung in SQL-Datenbank und die Frage "Was ist SQL-Datenbank?": Sie

- Die [Preisseite](https://azure.microsoft.com/pricing/details/sql-database/) Datenbank elastische Datenbank kostenvergleichen und Rechner anzeigen
- Erfahren Sie [elastische Pools](sql-database-elastic-pool.md).
- Beginnen Sie [Ihre erste Datenbank](sql-database-get-started.md)erstellen.
- [Verbinden und mit SSMS Abfragen](sql-database-connect-query-ssms.md)
- Erstellen Sie Ihre erste Anwendung in C#, Java, Node.js, PHP, Python und Ruby: [Bibliotheken für SQL-Datenbank und SQL Server-Verbindung](sql-database-libraries.md)
- Einen Index der Titel und [Alle Themen für Azure Sql-Datenbank-Dienst](sql-database-index-all-articles.md)angezeigt.
