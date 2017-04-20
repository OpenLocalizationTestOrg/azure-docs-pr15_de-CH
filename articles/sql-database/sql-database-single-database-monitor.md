<properties
    pageTitle="Datenbank-Performance in Azure SQL-Datenbank überwachen | Microsoft Azure"
    description="Erfahren Sie mehr über die Optionen für die Überwachungsdatenbank Azure Tools und dynamische Verwaltungsansichten."
    keywords="Datenbank Cloud-Datenbank-Performance überwachen"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>

# <a name="monitoring-database-performance-in-azure-sql-database"></a>Überwachung der Datenbank-Performance in Azure SQL-Datenbank
Überwachen der Leistung einer SQL Azure beginnt mit der Überwachung der Ressourcenverwendung gegenüber wählen Sie Datenbank-Leistung. Überwachung können bestimmen, ob die Datenbank Überkapazitäten oder Probleme Ressourcen überlastet sind, und dann entscheiden, ob diese Zeit Leistung und [Dienstebene](sql-database-service-tiers.md) der Datenbank. Sie können Ihre Datenbank mit Grafiktools in [Azure-Portal](https://portal.azure.com) oder mit SQL [dynamische Verwaltungsansichten](https://msdn.microsoft.com/library/ms188754.aspx)überwachen.

## <a name="monitor-databases-using-the-azure-portal"></a>Überwachen von Datenbanken mithilfe von Azure-portal

In [Azure-Portal](https://portal.azure.com/)können Sie eine einzelne Datenbank Nutzung Ihrer Datenbank und klicken Sie auf das Diagramm **Überwachung** überwachen. Dadurch wird ein **Metrik** , die Sie ändern können, auf die Schaltfläche **Diagramm bearbeiten** . Fügen Sie die folgenden Kriterien:

- CPU-Prozentsatz
- DTU in Prozent
- Daten e/a-Prozentsatz
- Datenbank Größe in Prozent

Nachdem Sie diese Metriken hinzugefügt haben, können Sie weiterhin diese **Überwachung** mit mehr Details im Fenster **Metrik** einzusehen. Alle vier Kriterien anzeigen den durchschnittliche Auslastung Prozentsatz der **DTU** der Datenbank Die [Dienstebenen](sql-database-service-tiers.md) Siehe Einzelheiten DTUs.

![Service-Tier Überwachung der Datenbank-Performance.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Sie können auch Alerts auf Leistungsdaten. Klicken Sie **Benachrichtigung hinzufügen** im Fenster **Metrik** . Führen Sie den Assistenten zum Konfigurieren der Warnung. Sie können die Warnung die Metrik einen bestimmten Schwellenwert überschreiten oder die Metrik einen bestimmten Schwellenwert unterschreitet.

Wenn Sie die Arbeitslast auf die Datenbank zu erwarten, können Sie eine e-Mail-Benachrichtigung konfigurieren, wenn die Datenbank auf Leistungsdaten 80 % erreicht. Hiermit können Sie als frühe Warnung beim Wechseln auf die nächsthöhere Leistung möglicherweise herauszufinden.

Leistungsdaten können auch Ihnen festzustellen, ob Leistung Stufen heruntergestuft werden. Angenommen Sie Standard S2-Datenbank und alle Leistungswerte zeigen, dass die Datenbank durchschnittlich nicht mehr als 10 % jederzeit verwenden. Es ist wahrscheinlich, dass die Datenbank in Standard S1 funktioniert. Beachten Sie jedoch die Arbeitslasten, die Spitze oder vor der Entscheidung für einen geringeren Leistung schwanken.

## <a name="monitor-databases-using-dmvs"></a>Monitor-Datenbanken mit DMVs

Die gleichen Metriken, die im Portal verfügbar gemacht sind auch über Systemansichten verfügbar: [resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) in der logischen **master** -Datenbank Server und [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) in der Datenbank. Verwenden Sie **resource_stats** benötigen Sie weniger detaillierte Daten über einen längeren Zeitraum überwachen. Verwenden Sie **sys.dm_db_resource_stats** benötigen Sie genauere Daten innerhalb kleiner. Weitere Informationen finden Sie im [Leitfaden zur Leistung der Azure SQL-Datenbank](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats).

>[AZURE.NOTE] **Sys.dm_db_resource_stats** gibt ein leeres Resultset bei Web- und Business Edition-Datenbanken zurückgezogen werden.

Überwachen Sie für elastische datenbankpools einzelne Datenbanken im Pool in diesem Abschnitt beschrieben. Aber Sie können auch den Pool als Ganzes überwachen. Weitere Informationen finden Sie unter [Überwachen und Verwalten einer elastischen Datenbank](sql-database-elastic-pool-manage-portal.md).
