<properties
    pageTitle="XTP arbeitsspeicherinternen überwachen | Microsoft Azure"
    description="Schätzung und Monitor XTP in-Speicher verwenden, Kapazität; Beheben Sie Fehler Kapazität 41823"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Monitor im Arbeitsspeicher OLTP-Speicher

Bei [OLTP Speicher](sql-database-in-memory.md)befindet sich im im Arbeitsspeicher OLTP-Speicher Daten im Speicher optimiert Tabellen und Tabellenvariablen. Jeder Premium-Service-Tier hat im Arbeitsspeicher OLTP Speicher maximal, die [SQL-Datenbank Dienstebenen Artikel](sql-database-service-tiers.md#service-tiers-for-single-databases)dokumentiert. Dieses Limit überschritten, einfügen und Aktualisieren von Vorgängen können fehlschlagen (mit Fehler 41823). An diesem Punkt müssen Sie entweder löschen, um Speicherplatz freizugeben, oder Leistungsebene der Datenbank aktualisieren.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Bestimmen Sie, ob Daten im Speicher Gap passen

Speicher-Cap bestimmen: finden Sie im [Artikel SQL Datenbank Dienstebenen](sql-database-service-tiers.md#service-tiers-for-single-databases) Caps Speicher der verschiedenen Premium Service leisten.

Schätzen Speicherplatz für eine Speicheroptimierte Tabelle funktioniert wie für SQL Server sie in Azure SQL-Datenbank ist. Lesen Sie das Thema auf [MSDN](https://msdn.microsoft.com/library/dn282389.aspx)einige Minuten dauern.

Beachten Sie, dass die Tabelle Variablen Tabellenzeilen, sowie Indizes Datengröße Max. Benutzer zählen. Darüber hinaus benötigt ALTER TABLE ausreicht, um eine neue Version der gesamten Tabelle und Indizes erstellen.

## <a name="monitoring-and-alerting"></a>Überwachung und Alarmierung

Sie können im Speicher verwenden als Prozentangabe der [Speicher für die Leistungsebene cap](sql-database-service-tiers.md#service-tiers-for-single-databases) im Azure- [Portal](https://portal.azure.com/)überwachen: 

- Suchen Sie auf die Datenbank das Auslastung Ressource, und klicken Sie auf Bearbeiten.
- Wählen Sie die Metrik `In-Memory OLTP Storage percentage`.
- Um eine Warnung hinzuzufügen, klicken Sie auf die Ressourcenverwendung Blatt Metrik öffnen und dann auf Warnung hinzufügen.

Oder verwenden Sie die folgende Abfrage in Speicher-Auslastung anzeigen:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Situationen Sie Out of Memory - Fehler 41823

Out of Memory ausführen in einfügen, aktualisieren und erstellen schlägt fehl mit Fehlermeldung 41823.

Fehlermeldung 41823 gibt an, dass der speicheroptimierten Tabellen und Tabellenvariablen die Maximalgröße überschritten haben.

Zum Beheben dieses Fehlers entweder:


- Löschen von speicheroptimierten Tabellen möglicherweise Verschiebung zu herkömmlichen, festplattenbasierte Tabellen Daten; oder
- Aktualisieren Sie die Dienstebene mit genügend Speicher für die Daten im Speicher optimiert Tabellen müssen.

## <a name="next-steps"></a>Nächste Schritte
Zusätzliche Ressourcen über [Azure SQL-Überwachungsdatenbank dynamische Verwaltungsansichten](sql-database-monitoring-with-dmvs.md)
