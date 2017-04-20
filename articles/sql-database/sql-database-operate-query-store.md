<properties
   pageTitle="Abfrage in SQL Azure-Datenbank speichern Betrieb"
   description="Enthält Informationen zum Bedienen der Abfrage in Azure SQL-Datenbank speichern"
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
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>Betrieb der Abfrage in Azure SQL-Datenbank speichern 

Abfrage-Speicher in Azure ist eine vollständig verwaltete Datenbank, die kontinuierlich sammelt und präsentiert Informationen alle Abfragen historisch. Sie können Query Store als ein Flugzeug Flugschreiber Daten denken, die erheblich vereinfacht die Abfrageperformance Problembehandlung für Cloud und lokalen Kunden. Dieser Artikel beschreibt bestimmte Aspekte von Betriebssystemen in Azure Abfrage speichern. Mithilfe dieser bereits gesammelten Abfragedaten können schnell diagnostizieren und Beheben von Performance-Problemen und so mehr Zeit auf ihr Geschäft konzentrieren. 

Abfrage-Speicher ist seit November 2015 [Global verfügbar](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) in Azure SQL-Datenbank. Abfrage-Informationsspeicher ist die Grundlage für Performance-Analyse und Optimierung Funktionen wie [SQL Datenbank und Performance Dashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Zum Zeitpunkt der Veröffentlichung dieses Artikels läuft Query Store 200.000 Benutzerdatenbanken in Azure im Abfrage-bezogene Informationen für mehrere Monate ohne Unterbrechung.

> [AZURE.IMPORTANT] Microsoft führt eine Abfrage Speicher für alle SQL Azure-Datenbanken (bestehende und neue) aktivieren. 

## <a name="optimal-query-store-configuration"></a>Optimale Speicherkonfiguration Abfrage

Dieser Abschnitt beschreibt die optimale Konfiguration Standardwerte, die Abfrage speichern und abhängige Funktionen wie [SQL Datenbank und Leistungsdashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/)zuverlässigen Betrieb zu gewährleisten. Standardkonfiguration ist optimiert für kontinuierliche Datensammlung, die minimale Zeit OFF-READ_ONLY Zustände.

| Konfiguration | Beschreibung | Standard | Kommentar |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Gibt die Grenze für den Datenspeicher, die Abfrage Speicher in Z-Kundendatenbank können | 100 | Für neue Datenbanken erzwungen |
| INTERVAL_LENGTH_MINUTES | Definiert die Größe des Zeitfensters der gesammelten Laufzeitstatistik für Abfragepläne aggregiert und beibehalten werden. Alle aktiven Abfrageplan ist höchstens eine Zeile für einen Zeitraum mit dieser Konfiguration definiert | 60   | Für neue Datenbanken erzwungen |
| STALE_QUERY_THRESHOLD_DAYS | Zeitbasierte Cleanup-Richtlinie, die die Aufbewahrungsdauer von beibehaltenen Laufzeitstatistik und inaktive Abfragen | 30 | Für neue Datenbanken und Datenbanken mit früheren (367) erzwungen |
| SIZE_BASED_CLEANUP_MODE | Gibt an, ob automatische Bereinigung stattfindet, wenn Abfrage Datengröße das Limit erreicht | Auto | Für alle Datenbanken erzwungen |
| QUERY_CAPTURE_MODE | Gibt an, ob alle Abfragen oder nur eine Teilmenge der Abfragen überwacht | Auto | Für alle Datenbanken erzwungen |
| FLUSH_INTERVAL_SECONDS | Gibt an, dass die Höchstdauer Runtime erfasst Statistiken im Speicher aufbewahrt werden, vor dem leeren Datenträger | 900 | Für neue Datenbanken erzwungen |
||||||

> [AZURE.IMPORTANT] Diese Standardwerte werden automatisch in der letzten Phase der Abfrage Speicher Aktivierung in allen SQL Azure-Datenbanken (siehe Hinweis oben) angewendet. Nach diesem Licht, wird nicht Azure SQL-Datenbank Kunden, Werte ändern, wenn sie primäre Arbeitslast oder zuverlässigen Betrieb des Informationsspeichers Abfrage beeinträchtigen.

Mit benutzerdefinierten Einstellungen, verwenden Sie [ALTER DATABASE mit Query Store](https://msdn.microsoft.com/library/bb522682.aspx) Konfiguration auf den vorherigen Zustand wiederherzustellen. Checken Sie [Best Practices mit der Abfrage](https://msdn.microsoft.com/library/mt604821.aspx) um Informationen wie oben optimale Parameter haben.

## <a name="next-steps"></a>Nächste Schritte

[SQL-Datenbank-Performance-Sicherung](sql-database-performance.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie die folgenden Artikel:

- [Flugdatenschreiber für die Datenbank](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Überwachen der Leistung mithilfe von Abfrage laden](https://msdn.microsoft.com/library/dn817826.aspx)

- [Verwendungsszenarien Abfrage laden](https://msdn.microsoft.com/library/mt614796.aspx)

- [Überwachen der Leistung mithilfe von Abfrage laden](https://msdn.microsoft.com/library/dn817826.aspx) 
