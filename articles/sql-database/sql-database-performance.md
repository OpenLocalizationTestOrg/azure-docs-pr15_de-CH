<properties 
   pageTitle="SQL Azure-Datenbank Performance Insight | Microsoft Azure" 
   description="Azure SQL-Datenbank bietet Performance Tools können Sie die Bereiche identifizieren, die aktuellen steigern können." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="07/19/2016"
   ms.author="sstein"/>

# <a name="sql-database-performance-insight"></a>SQL-Datenbank-Performance-Sicherung

Azure SQL-Datenbank bietet Performance Tools zum Erkennen und Verbessern der Leistung von Datenbanken durch intelligente Optimierung Aktionen und Vorschläge. 

1. Suchen Sie Ihre Datenbank in [Azure-Portal](http://portal.azure.com) und auf **Alle** > **Leistung **  >  **Übersicht** auf der Seite " **Leistung** ". 


2. Auf **Empfehlung** [SQL Datenbank Advisor](#sql-database-advisor)öffnen und auf **Abfragen** [Leistung Einblick Abfrage](#query-performance-insight)öffnen.

    ![Anzeigen der Leistung](./media/sql-database-performance/entries.png)



## <a name="performance-overview"></a>Systemmonitor im Überblick

**Übersicht** oder **Leistung** Kachel auf gelangen Sie zur Performance Dashboard für die Datenbank. Diese Ansicht enthält eine Zusammenfassung der Datenbank-Performance und hilft Ihnen beim Leistungstuning und Fehlerbehebung. 

![Leistung](./media/sql-database-performance/performance.png)

- **Empfohlene** Kachel enthält eine Aufschlüsselung der Vorschläge für die Datenbank optimieren (Top 3 Recommendations werden angezeigt, wenn es mehr). Auf diese Kachel nimmt **SQL Datenbank**Berater. 
- Die Kachel **Tuning Aktivität** enthält eine Zusammenfassung der laufende und abgeschlossene Aktionen für die Datenbank optimieren, geben Ihnen einen schnellen Überblick über den Verlauf der Aktivität optimieren. Diese Kachel auf öffnet vollständige Optimierung Verlaufsansicht für die Datenbank.
- **Automatisch** nebeneinander zeigt die automatische Abstimmung für die Datenbank (die Feinabstimmung Aktionen automatisch auf die Datenbank angewendet werden konfiguriert sind). Auf diese Kachel Öffnet das Konfigurationsdialogfeld Automatisierung.
- **Datenbankabfragen** Kachel zeigt die Zusammenfassung der Abfrageperformance für die Datenbank (insgesamt DTU Verwendung und Top ressourcenintensiven Abfragen). Auf diese Kachel öffnet **Einen Einblick Abfrage-Leistung**.



## <a name="sql-database-advisor"></a>SQL Datenbank Advisor


[SQL Datenbank Advisor](sql-database-advisor.md) bietet intelligente optimierungsempfehlungen, die die Datenbank-Performance verbessern können. 

- Wann welche Indizes zu erstellen oder zu löschen (und eine Option für Index Recommendations automatisch ohne Eingreifen des Benutzers automatisch zurücksetzen Vorschläge, die sich negativ auf die Leistung gelten).
- Empfohlene Schema Probleme in der Datenbank identifiziert werden.
- Empfehlung beim Abfragen von parametrisierten Abfragen profitieren können.




## <a name="query-performance-insight"></a>Einen Einblick Abfrage-Leistung

[Leistung Einblick Abfrage](sql-database-query-performance.md) können Sie weniger Zeit mit Datenbank-Performance troubleshooting:

- Einblick in Ihre Datenbanken Ressourcenverbrauch (DTU). 
- Top CPU verbraucht Abfragen möglicherweise für verbesserte Leistung optimiert werden können. 
- Die Möglichkeit, einen Drilldown in die Details einer Abfrage. 


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Azure SQL-Datenbank Leitfaden zur Leistung für einzelne Datenbanken](sql-database-performance-guidance.md)
- [Wenn werden ein Datenbankpool elastische soll verwendet?](sql-database-elastic-pool-guidance.md)