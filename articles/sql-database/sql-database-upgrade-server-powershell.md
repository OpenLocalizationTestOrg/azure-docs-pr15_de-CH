<properties
    pageTitle="Upgrade auf Azure SQL-Datenbank V12 mit PowerShell | Microsoft Azure"
    description="Erläutert die Azure SQL-Datenbank V12 upgrade einschließlich Web- und Datenbanken und ein V11 Server Datenbanken direkt in einem Pool elastische Datenbanken mithilfe von PowerShell migrieren."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Upgrade auf Azure SQL-Datenbank V12 mit PowerShell


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


SQL Datenbank V12 ist die neueste Version aktualisieren auf SQL Datenbank V12 empfiehlt.
SQL Datenbank V12 hat viele [Vorteile gegenüber der vorherigen Version](sql-database-v12-whats-new.md) einschließlich:

- Verbesserte Kompatibilität mit SQL Server.
- Verbesserte Leistung und neue Leistungsmerkmale.
- [Elastische datenbankpools](sql-database-elastic-pool.md).

Dieser Artikel beschreibt, wie Sie für das Aktualisieren von vorhandenen Datenbank V11 SQL Server und Datenbanken auf SQL-Datenbank V12.

Bei der Aktualisierung auf V12 aktualisieren Sie Web- und Datenbanken auf einen neuen Service-Tier erfahren Sie, wie Web Datenbanken aktualisieren enthalten sind.

Zusätzlich möglich zu einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md) migrieren kostengünstiger als die Aktualisierung einzelner Leistung (Preisgestaltung Ebenen) für einzelne Datenbanken. Pools vereinfachen auch Datenbank brauchen Sie nur die Leistung Einstellungen für den Pool nicht separat verwalten die Leistungsfähigkeit der einzelnen Datenbanken. Haben Sie Datenbanken auf mehreren Servern sollten Sie zu demselben Server und Inbetriebnahme eines Pools nutzen.

Sie können die Schritte in diesem Artikel Datenbanken problemlos von V11 Server direkt in Pools für elastische Datenbanken migrieren folgen.

Beachten Sie, dass die Datenbanken online bleiben und weiterhin in den Aktualisierungsvorgang. Zum Zeitpunkt der tatsächlichen Umstellung auf die neue temporäre Verbindung zur Datenbank ablegen Leistungsfähigkeit kann kann eine sehr kleine Dauer, die in der Regel etwa 90 Sekunden passieren jedoch 5 Minuten. Wenn die Anwendung [für die Verbindung Beendigungen vorübergehender Fehler](sql-database-connectivity-issues.md) ist ausreichend Schutz Verbindungen am Ende der Aktualisierung unterbrochen.

SQL Datenbank V12 aktualisieren kann nicht rückgängig gemacht werden. Nach einer Aktualisierung kann der Server V11 wiederhergestellt werden.

Nach der Aktualisierung auf V12 werden [Service-Tier-Empfehlung](sql-database-service-tier-advisor.md) und [elastischen Pool Recommendations](sql-database-elastic-pool-create-portal.md) nicht sofort verfügbar bis der Dienst Zeit zum Auswerten der Arbeitslasten auf dem neuen Server. V11 Server Empfehlung Geschichte gilt nicht für V12-Servern, nicht beibehalten.  

## <a name="prepare-to-upgrade"></a>Vorbereiten der Aktualisierung

- **Alle Web- und Datenbanken**: Verwenden Sie das Portal oder [PowerShell Datenbanken und Server](sql-database-upgrade-server-powershell.md).
- **Überprüfen und Geo-Replikation**: Wenn Ihre Azure SQL-Datenbank für Geo-Replikation konfiguriert ist sollten Sie Dokumentieren der aktuellen Konfiguration und [Geo-Replikation beenden](sql-database-geo-replication-portal.md#remove-secondary-database). Nach die Aktualisierung neu Geo-Replikation konfigurieren Sie die Datenbank.
- **Öffnen Sie diese Ports, wenn Clients auf Azure-VM**: Wenn Ihr Clientprogramm SQL Datenbank V12 verbindet, während der Client auf eine Azure Virtual Machine (VM) ausgeführt wird, müssen Sie Anschlussbereiche 11000 11999 und 14000-14999 auf dem virtuellen Computer öffnen. Einzelheiten finden Sie [Ports für SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

Um V12 mit PowerShell einen Server aktualisieren, müssen Sie die neuesten Azure PowerShell installiert und ausgeführt. Ausführliche Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Ihre Anmeldeinformationen konfigurieren und Ihr Abonnement auswählen

Um Abonnements Azure PowerShell-Cmdlets ausführen, müssen Sie zunächst Access Azure-Konto herstellen. Führen Sie die folgenden und Sie werden mit einem Bildschirm Ihre Anmeldeinformationen eingeben. Verwenden Sie dieselbe e-Mail und Kennwort für die Anmeldung bei Azure-Portal.

    Add-AzureRmAccount

Nach der erfolgreichen Anmeldung erhalten Sie einige Informationen auf dem Bildschirm, die die Id Sie angemeldet und Azure-Abonnements haben Sie Zugriff auf.

Wählen Sie das Abonnement Sie arbeiten möchten muss Ihre Abonnement-Id (**SubscriptionId-**) oder ein Abonnement (**-SubscriptionName**) nennen. Aus dem vorherigen Schritt kopieren, oder haben Sie mehrere Abonnements können Sie das Cmdlet " **Get-AzureRmSubscription** " ausführen und die gewünschte Abonnementinformationen aus dem Resultset.

Führen Sie das folgende Cmdlet mit Informationen zu Ihrem Abonnement Ihres aktuellen Abonnements festlegen:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Die folgenden Befehle werden für das Abonnement nur oben ausgewählten ausgeführt werden.

## <a name="get-recommendations"></a>Lassen Sie sich

Um die Empfehlung für den Server aktualisieren das folgende Cmdlet:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Weitere Informationen finden Sie unter [Erstellen einer elastischen Datenbankpool](sql-database-elastic-pool-create-portal.md) und [Azure SQL-Datenbank Preise Tier Recommendations](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>Starten des Upgrades

So starten Sie die Aktualisierung des Servers das folgende Cmdlet ausgeführt

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Beim Ausführen wird dieser Befehl Aktualisierung beginnen. Sie können Anpassen der Ausgabe der Empfehlung und die bearbeitete Empfehlung dieses Cmdlet.


## <a name="upgrade-a-server"></a>Aktualisieren eines Servers


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Benutzerdefinierte Zuordnung aktualisieren

Wenn die Empfehlung nicht für Ihre Server und Geschäftsszenario geeignet sind, können Sie auswählen, wie die Datenbanken werden aktualisiert und können für einzelne oder elastische Datenbanken zuordnen.

ElasticPoolCollection und DatabaseCollection wurde sind optional:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Überwachen Sie nach der Aktualisierung auf SQL Datenbank V12 Datenbanken


Nach der Aktualisierung wird empfohlen, überwachen die Datenbank aktiv dafür laufen die gewünschte Leistung und Auslastung zu optimieren, Bedarf.

Darüber hinaus überwachen Sie elastische Datenbank Pools [über das Portal](sql-database-elastic-pool-manage-portal.md) überwachen können einzelne Datenbanken oder mit [PowerShell](sql-database-elastic-pool-manage-powershell.md)


**Ressourcendaten:** Basic, Standard und Premium-Datenbanken ist Ressourcendaten über die [sys.dm_ Db_ Resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV in der Datenbank. Diese DMV bietet nahezu in Echtzeit Verbrauch Ressourceninformationen 15 zweite Granularität für die letzte Stunde des Vorgangs. DTU Prozentsatz Verbrauch für ein Intervall wird als Höchstprozentsatz Verbrauch von CPU und EA Protokoll Dimensionen berechnet. Dies ist eine Abfrage zum Berechnen des durchschnittlichen DTU Prozentsatz Verbrauchs in der letzten Stunde:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Überwachung Zusatzinformationen:

- [Leitfaden zur Azure SQL-Datenbank-Leistung für einzelne Datenbanken](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Preis- und Hinweise für einen Datenbankpool elastische](sql-database-elastic-pool-guidance.md).
- [Überwachung über dynamische Verwaltungsansichten Azure SQL-Datenbank](sql-database-monitoring-with-dmvs.md)



**Warnung:** Soll "Alarme" in Azure-Portal benachrichtigen DTU Verbrauch für eine aktualisierte Datenbank bestimmte hohe nähert. Datenbank-Alerts können im Azure-Portal für verschiedene Leistungsmetriken wie DTU, CPU, e/a und Protokoll einrichten. Die Datenbank durchsuchen Sie, und wählen Sie **Warnregeln** **Einstellungen** -Blades.

Beispielsweise können Sie Einrichten einer e-Mail-Warnung "DTU Prozentsatz" übersteigt die durchschnittliche DTU Prozentsatz 75 % in den letzten 5 Minuten. Finden Sie weitere Informationen zum Konfigurieren von Benachrichtigungen [Benachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) erhalten.



## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer elastischen Datenbankpool](sql-database-elastic-pool-create-portal.md) und einige oder alle Datenbanken im Pool.
- [Die Dienstebene Tier und Leistung Ihrer Datenbank ändern](sql-database-scale-up.md).



## <a name="related-information"></a>Informationen

- [AzureRmSqlServerUpgrade abrufen](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [AzureRmSqlServerUpgrade starten](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [AzureRmSqlServerUpgrade beenden](https://msdn.microsoft.com/library/azure/mt603589.aspx)
