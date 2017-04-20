<properties
   pageTitle="SQL Azure-Datenbank Lösung schnell starten | Microsoft Azure"
   description="Erfahren Sie mehr über SQL Azure Database Solutions"
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
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>Untersuchen von Azure SQL-Datenbank Lösung schnell beginnt

Dieser Artikel enthält eine Übersicht über die Azure SQL-Datenbank Lösung schnell beginnt. Diese schnelle beginnt im Repository Beispiele GitHub SQL Server befinden und veranschaulichen die Verwendung der SQL-Datenbank in eine vollständige Lösung basierend auf realistischen Szenarien. Einfache Lernprogramme, die SQL-Datenbank besondere demonstrieren, finden Sie unter [Lernprogramme Azure SQL-Datenbank durchsuchen](sql-database-explore-tutorials.md).

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Versuchen Sie WingTipTickets Demo und praktische Übungseinheit

[Azure SQL-Datenbank WingTipTickets](https://github.com/microsoft/wingtiptickets) Demo und praktische Übungseinheit wird Azure SQL-Datenbank als Azure Search-basierte beispielanwendung, Karten verkaufen.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Erfassen und Überwachen von Daten über mehrere pools

[Lösung Schnellstart: elastischen Pool Telemetrie mit PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) bietet eine Lösung für das Sammeln und Überwachen der Ressourcenverwendung SQL-Datenbank über mehrere Pools in einem Abonnement. Wenn Sie eine große Anzahl von Datenbanken in einem Abonnement haben, ist jeder elastische Pool separat überwacht umständlich.

Um dieses Problem zu beheben, können Sie SQL-Datenbank-PowerShell-Cmdlets und T-SQL-Abfragen zum Sammeln von Daten aus mehreren Pools und ihre Datenbanken kombinieren. Dadurch können Sie überwachen und analysieren die Ressourcenverwendung effizienter.

Dieser Schnellstart bietet eine Reihe von PowerShell-Skripts und T-SQL-Abfragen mit Dokumentation macht die Lösung und Implementierung.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>Erste Schritte mit elastische Datenbanken in einem SaaS-Szenario

 [Lösung Schnellstart: benutzerdefiniertes Dashboard SaaS elastischen Pool](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) bietet eine Lösung für ein Software-als-a-Lösung (SaaS)-Szenario, das Feature elastische Datenbank SQL-Datenbank für eine SaaS-Anwendung zu einer kostengünstigen und skalierbaren Datenbank-Backend verwendet.

In dieser Lösung führt Sie durch die Implementierung einer Web App. Diese Webanwendung können Sie die Last visualisieren, die elastische Datenbank laden-Generator erstellt wird, die ein benutzerdefiniertes Dashboard verwendet, das Azure-Portal ergänzt.

Dieser Schnellstart bietet eine Auslastungsgenerator und Überwachung WebApp zusammen mit der Dokumentation zur Funktionsweise der Anwendung und Ihre Verwendung.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Erstellen einer Azure SQL-Datenbank mithilfe von Code First-Entwicklung und Entity Framework

Video und Beispiel in [Code First in eine neue Datenbank](https://msdn.microsoft.com/data/jj193542.aspx) enthält eine Einführung in Code First-Entwicklung, die eine neue Datenbank abzielt. Dabei zielt auf eine Datenbank, die nicht vorhanden sind, aber die wird von Code First erstellt. Das Szenario erstellt auch eine leere Datenbank, Code First neue Tabellen fügt.

Code First können Sie Ihr Modell mithilfe von C# oder Visual Basic .NET Klassen definieren. Sie können Attribute auf Ihre Klassen und Eigenschaften oder mithilfe der fluent-API optional zusätzliche Konfigurationsschritte ausführen.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Integrieren Sie elastische Datenbanktools in einer Entity Framework-Anwendung

[Elastische Datenbank-Clientbibliothek mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) gezeigt geändert, die Entity Framework-Anwendung mit [elastische Datenbanktools](sql-database-elastic-scale-get-started.md)integrieren zu müssen. Der Schwerpunkt liegt auf [Splitter Management zugeordnet](sql-database-elastic-scale-shard-map-management.md) und mit dem Entity Framework Code First [datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md) verfassen.

[Code First, eine neue Beispieldatenbank für EF](http://msdn.microsoft.com/data/jj193542.aspx) dient als Beispiel in diesem Beispiel. Der Beispielcode dieses Dokument ist Teil der elastische Datenbank Tools Proben in den Visual Studio.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Sicherheit auf Zeilenebene elastische Datenbanktools integriert

[Mandantenfähigen Applikationen elastische Datenbanktools mit Sicherheit auf Zeilenebene](sql-database-elastic-tools-multi-tenant-row-level-security.md) zufolge die Änderung einer Entity Framework-Anwendung zum [zeilenbasierter Sicherheit](https://msdn.microsoft.com/library/dn765131) [elastische Datenbanktools](sql-database-elastic-scale-get-started.md) integrieren müssen. Dieses Beispiel veranschaulicht, wie diese Technologie zusammen verwenden, um eine Anwendung mit einer hochgradig skalierbaren Datenebene erstellen, mandantenfähigen Splitter unterstützt.

Dazu verwenden ADO.NET SqlClient oder Entity Framework. In diesem Beispiel erweitert die [Clientbibliothek elastische Datenbank mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) um Unterstützung mandantenfähigen Splitter Datenbanken.
Erstellung eine einfachen Konsolenanwendung für Blogs und Beiträge mit vier Mieter und zwei mandantenfähigen Splitter-Datenbanken.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>Online-Umfragen mit Anwendung Tailspin Umfragen erstellen

Dieses [Beispiel Tailspin Umfragen](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) ist eine mandantenfähigen Anwendung, Umfragen, aufgerufen, die Benutzer online-Umfragen erstellen kann. Das Beispiel Bedenken einige wichtige Informationen zur Verwaltung von Benutzeridentitäten in einer mehrinstanzenfähigen Anwendung, einschließlich Anmeldung, Authentifizierung, Autorisierung und app-Rollen.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>Erfahren Sie mehr über die neuesten Sicherheitsfeatures der SQL-Datenbank mit Contoso Technologieüberblick Demo-Anwendung

[Contoso Technologieüberblick Demo-Anwendung](https://github.com/Microsoft/azure-sql-security-sample) zeigt die neuesten Sicherheitsfunktionen der SQL-Datenbank.

## <a name="next-steps"></a>Nächste Schritte

[Lernprogramme Azure SQL-Datenbank durchsuchen](sql-database-explore-tutorials.md)
