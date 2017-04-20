<properties
   pageTitle="Azure SQL-Datenbank Web und Business Edition Sonnenuntergang FAQ | Microsoft Azure"
   description="Finden Sie die Datenbanken Azure SQL Web wird und erfahren Sie mehr über die Features und Funktionen der neuen Dienstebenen."
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/08/2016"
   ms.author="sstein" />

# <a name="web-and-business-edition-sunset-faq"></a>Web und Business Edition Sonnenuntergang FAQ

Azure SQL Web Datenbanken werden jetzt deaktiviert. Die Stufen Basic, Standard, Premium und elastisch ersetzen die Abschaltung Web Datenbanken.

Um Unterstützung bei der Aktualisierung von Web- und Datenbanken empfiehlt die SQL-Datenbank einen entsprechenden Service Tier und Performance-Niveau (Tarif) für jede Datenbank basierend auf Verlaufsdaten Arbeitslast.

**Empfohlene Tier Preise abrufen:**

- [Upgrade auf SQL Datenbank V12 mit Azure-Portal](sql-database-upgrade-server-portal.md)
- [Upgrade auf SQL Datenbank V12 mit PowerShell](sql-database-upgrade-server-powershell.md)
- [Ändern Sie den Tarif einer Web- oder Business](sql-database-service-tier-advisor.md)



## <a name="why-does-the-azure-portal-show-my-web-and-business-edition-databases-as-retired"></a>Warum zeigt das Azure-Portal meine Web und Business Edition-Datenbanken im Ruhestand?

Da Web- und Business Edition-Datenbanken nach September 2015 nicht verfügbar ist, kennzeichnet das Portal Web- und Datenbanken zurückgezogen. Zurückgezogene Bezeichnung bietet auch daran, dass alle Datenbanken Web Standard, Basic oder Premium aktualisiert werden soll. Detaillierte Informationen zur Aktualisierung von Datenbanken Web oder Business auf neuen Dienstebenen finden Sie unter [Aktualisieren auf Azure SQL Datenbank V12](sql-database-upgrade-server-portal.md).

## <a name="which-new-service-tier-is-the-best-choice-to-upgrade-my-existing-web-or-business-database-to"></a>Der neue Service-Tier ist die beste Wahl meine vorhandene Web- oder Business Datenbank aktualisieren?

Neue Service-Tier und Leistung angemessener für die vorhandene Web- oder Business-Datenbank auswählen hängt der bestimmte Funktion und Leistung für Ihre Anwendung.

Verwenden Sie Preise Tier Empfehlung beschriebenen oben oder Informationen unterstützen Sie bei der Auswahl einer geeigneten neuen Dienstebene, finden Sie unter [Aktualisieren auf Azure SQL Datenbank V12](sql-database-upgrade-server-portal.md).

## <a name="why-is-microsoft-introducing-new-service-tiers"></a>Warum führt Microsoft neue Dienstebenen?

Aufgrund von Kundenfeedback führt Azure SQL-Datenbank neue Dienstebenen Kunden Weitere relationale Datenbanken problemlos zu unterstützen. Die neuen Ebenen sollen vorhersagbare Leistung in einem Spektrum von sieben Ebenen für leichte, schwere transaktionalen Anforderungen. Darüber hinaus Angebot neuen Stufen der Business Continuity-Funktionen eine stärkere Uptime SLA größere Datenbank für weniger Geld und eine verbesserte Abrechnung Erfahrung.

## <a name="where-can-i-learn-more-about-the-new-service-tiers"></a>Wo erfahre ich mehr über die neuen Dienstebenen?

Ausführliche Informationen zu den neuen Dienstebenen Modell finden Sie unter [Dienstebenen](sql-database-service-tiers.md). Preisinformationen für neue Dienstebenen finden Sie [Preise SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="what-features-or-functionality-will-not-be-available-in-basic-standard-and-premium"></a>Welche Merkmale oder Funktionen in Basic, Standard und Premium nicht?

Verbünden Feature wird mit Web- und Business-Editionen deaktiviert. Kunden, die ihre Datenbanken Skalieren sollten stattdessen oder Migrieren auf [elastische Datenbanktools](sql-database-elastic-scale-get-started.md) für [Azure SQL-Datenbank](sql-database-elastic-scale-get-started.md)vereinfacht das Erstellen und Verwalten einer Anwendung, die Sharding verwendet. Eine Clientbibliothek .NET kann Anwendung definieren, wie Daten auf Splitter und Routen OLTP entsprechenden Datenbanken zugeordnet werden. Um Verwaltungsvorgänge unterstützen, die Verteilung von Daten zwischen Splitter konfigurieren, ist eine Azure Cloud Servicevorlage enthalten in eigenen Azure-Abonnement gehostet werden kann. Neben [elastische Datenbanktools](sql-database-elastic-scale-get-started.md)weiterhin Microsoft erstellen und Veröffentlichen von [benutzerdefinierten Sharding Muster und Praktiken Anleitung](https://msdn.microsoft.com/library/azure/dn764977.aspx) basierend auf Erkenntnisse aus Tiefe Kunden. Neue Kunden skalieren Funktionalität sollte [elastische Datenbanktools](sql-database-elastic-scale-get-started.md) Auschecken oder wenden Sie sich an Microsoft Support, um ihre Optionen.

Microsoft wird auch die Datenbank kopieren Erfahrung mit Premium-Datenbanken ändern. Erstellen Sie Datenbank zuvor beschränkte Premium Datenbank Kontingent... Eine Kopie der in T-SQL einer Prämie ausgesetzt reservierte Ressourcen erstellt, wie einer Datenbank berechnet wurde. Premium Kontingent jetzt frei verfügbar ist, wird ausgesetzt Premium nicht mehr unterstützt. Kopien jetzt mit derselben Edition Leistungsstufe als Quelle erstellt und entsprechend berechnet. Kunden können kopierte Datenbanken auf eine andere Ebene oder Leistung Leistungsebene ihre Kosten senken, bei Bedarf heruntergestuft. Business Edition als Teil dieser Version werden angehalten Premium-Datenbanken konvertiert. Voraussichtlich Einführung von [Point-In-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore) müssen Sicherungskopien der Datenbanken zu verringern.

## <a name="how-does-basic-standard-and-premium-improve-my-billing-experience"></a>Wie verbessern Basic, Standard und Premium Meine Rechnungsadresse?

Basic, Standard und Premium Azure SQL-Datenbanken werden pro Stunde berechnet, und jede Datenbank nach oben oder unten skalieren. Sie werden Festpreis basierend auf höchster Ebene und Leistung erforderlichem Service-Level für jede Stunde berechnet. Darüber hinaus Leistung (Beispiel: Basic, S1 und P2) werden in der Stückliste zu erleichtern, die Zahl der Datenbank Tage in einem Monat für jede Leistung entstehen aufgeschlüsselt. Web Datenbanken weiterhin belastet mit Datenbank basierend auf der Größe der Datenbank. Weitere Informationen zu Preisen und Unterschiede zwischen den neuen Dienstebenen [SQL Datenbank Preisseite](https://azure.microsoft.com/pricing/details/sql-database/) besuchen.


## <a name="see-also"></a>Siehe auch

[SQL Azure-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)

[Web und Business Preise](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[Dienstebenen](sql-database-service-tiers.md)

[Aktualisieren Sie auf SQL Azure-Datenbank V12](sql-database-upgrade-server-portal.md)
