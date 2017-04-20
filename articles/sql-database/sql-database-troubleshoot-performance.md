<properties
    pageTitle="SQL-Datenbank-Performance tuning Tipps | Microsoft Azure"
    description="Tipps zur Performance-Optimierung in Azure SQL-Datenbank durch Auswertung und Verbesserung."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="SQL Performance-tuning, Datenbankperformance-tuning, Sql-Optimierung Tipps Sql Datenbank Performance-tuning"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>SQL Datenbank Performance tuning-Tipps
Die [Dienstebene](sql-database-service-tiers.md) einer Datenbank ändern oder eDTUs eines Pools elastische Datenbank jederzeit Leistung erhöhen, aber Chancen zu Abfrage zuerst optimieren möchten. Fehlende Indizes und schlecht optimierte Abfragen sind häufige Ursachen für schlechte Datenbank-Performance. Dieser Artikel enthält Hinweise zur Performance-Optimierung in SQL-Datenbank.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Schritte zu optimieren Datenbank-performance
1.  In [Azure-Portal](https://portal.azure.com)auf **SQL-Datenbanken**, wählen Sie die Datenbank und verwenden Sie Monitoring-Diagramm für Ressourcen maximal nähert sich. DTU Verbrauch wird standardmäßig angezeigt. Klicken Sie auf **Bearbeiten** , um Zeit und Werte ändern.
2.  Verwenden Sie [Leistung Einblick Abfrage](sql-database-query-performance.md) , um Abfragen mit DTUs auszuwerten, und verwenden Sie [SQL Datenbank Advisor](sql-database-advisor.md) Rahmen erstellen Indizes löschen, Parametrisieren von Abfragen und Behebung von Problemen Schema anzeigen.
3.  Dynamische Verwaltungsansichten (DMVs), Extended Events (Xevents) und den Speicher Abfrage in SSMS können Sie um Parameter in Echtzeit zu erhalten. Finden Sie im [Leitfaden Thema Performance](sql-database-performance-guidance.md) detaillierte Überwachung und Optimierung Tipps.


    > [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Schritte zur Verbesserung der Datenbankperformance mit mehr Ressourcen
1.  Für einzelne Datenbanken können Sie [Dienstebenen ändern](sql-database-scale-up.md) bei Bedarf auf die Performance der Datenbank verbessern.
2.  Erwägen Sie für mehrere Datenbanken [elastische datenbankpools](sql-database-elastic-pool-guidance.md) Ressourcen automatisch skaliert.
