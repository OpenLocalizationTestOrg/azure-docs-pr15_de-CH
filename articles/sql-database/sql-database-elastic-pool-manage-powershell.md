<properties 
    pageTitle="Verwalten einer elastischen Datenbank (PowerShell) | Microsoft Azure" 
    description="Erfahren Sie mit PowerShell elastischen Datenbankpool verwalten."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Überwachen Sie und verwalten Sie einer elastischen Datenbank mit PowerShell 

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Verwalten einer [elastischen Datenbankpool](sql-database-elastic-pool.md) mithilfe von PowerShell-Cmdlets. 

Allgemeine Fehlercodes finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbank-Verbindungsfehler und andere Probleme](sql-database-develop-error-messages.md).

Werte für Pools finden Sie in [eDTU und Storage Limits](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Erforderliche Komponenten

* Azure PowerShell 1.0 oder höher. Ausführliche Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
* Elastische datenbankpools sind nur mit SQL-Datenbank V12-Servern verfügbar. Haben Sie einen SQL-Datenbank V11 Server in einem Schritt [Verwenden Sie PowerShell auf V12 und einen Pool erstellen](sql-database-upgrade-server-portal.md) .


## <a name="move-a-database-into-an-elastic-pool"></a>Eine Datenbank in einer elastischen Pool verschieben

Sie können eine Datenbank in oder aus einem Pool mit [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx)verschieben. 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Ändern der Leistung eines Pools

Wenn die Leistung leidet, können Sie die Einstellungen des Pools Wachstum ändern. Verwenden Sie das Cmdlet " [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) ". Den Dtu - Parameter auf eDTUs pro Pool festgelegt. [EDTU und Storage Limits](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) für mögliche Werte anzeigen  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>Der Status von Pool-Operationen

Erstellen eines Pools kann länger dauern. Um den Status des Pools Vorgänge erstellen und Updates zu überwachen, verwenden Sie das Cmdlet " [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) ".

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>Den Status eine elastische Datenbank in und aus einem Pool verschieben

Verschieben einer Datenbank kann länger dauern. Mit dem Cmdlet [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) Status verschieben nachverfolgen.

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Abrufen von Daten für einen pool

Metriken, die als Prozentsatz der Pool-Ressourcengrenze abgerufen werden können:   


| Metrische name | Beschreibung |
| :-- | :-- |
| CPU\_Prozent | Durchschnitt berechnen Auslastung in Prozent der Grenze des Pools. |
| physikalische\_Daten\_lesen\_Prozent | Durchschnittliche e/a-Auslastung in Prozent basierend auf der Grenze des Pools. |
| Protokoll\_schreiben\_Prozent | Durchschnittliche schreiben Ressourcenverwendung in Prozent der Grenze des Pools. | 
| DTU\_Verbrauch\_Prozent | Durchschnittliche eDTU Auslastung in Prozent der eDTU Grenzwert für den pool | 
| Storage\_Prozent | Durchschnittliche Auslastung des Speichers in Prozent der Speicherkapazität des Pools. |  
| Arbeitskräfte\_Prozent | Maximale gleichzeitige Arbeitskräfte (Anfragen) in Prozent basierend auf der Grenze des Pools. |  
| Sessions\_Prozent | Maximale gleichzeitige Sitzungen Prozentsatz der Grenze des Pools. | 
| eDTU_limit | Aktuelle max elastischen Pool DTU Einstellung für dieses flexible Pool während dieses Intervalls. |
| Storage\_Grenze | Aktuelle max elastischen Pool Speichergrenze während dieses Intervalls für diese elastische Pool in MB festlegen. |
| eDTU\_verwendet | Durchschnittliche eDTUs Pool in diesem Intervall verwendet. |
| Storage\_verwendet | Durchschnittliche Storage Pool in diesem Intervall in Byte verwendet |

**Metriken Granularität/Aufbewahrungsfristen:**

* Daten werden 5 Minuten Granularität zurückgegeben werden.  
* Data Retention ist 35 Tagen.  

Dieses Cmdlet und API begrenzt die Anzahl der Zeilen, die einen Aufruf von 1000 Zeilen (etwa 3 Tage 5 Minuten Granularität Daten) abgerufen werden können. Dieser Befehl kann mit anderen Start/Ende Zeitintervalle mehr Daten abrufen aufgerufen 

Die Metriken abrufen:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Daten für eine elastische Datenbank abrufen

Diese APIs sind identisch mit den aktuellen (V12) APIs zur Überwachung der Ressourcenverwendung einer eigenständigen Datenbank außer den folgenden semantischen unterschieden. 

Für diese API werden Metriken abgerufen ausgedrückt als Prozentsatz des die maximale eDTUs (oder entsprechende Cap für die zugrunde liegende Metrik wie CPU, e/a usw.) für den Pool festgelegt. Beispielsweise 50 % Auslastung aller dieser Metriken gibt an, dass bestimmte Ressourcenverbrauch 50 % der pro Datenbank Cap für diese Ressource in den übergeordneten Pool. 

Die Metriken abrufen:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Hinzufügen einer Benachrichtigung für eine Ressource pool

Sie können hinzufügen Warnregeln Ressourcen senden e-Mail-Benachrichtigungen oder Warnmeldungszeichenfolgen [URL](https://msdn.microsoft.com/library/mt718036.aspx) Endpunkte trifft die Ressource einen Schwellenwert Auslastung, den eingerichtet. Verwenden Sie das Cmdlet AzureRmMetricAlertRule hinzufügen. 

> [AZURE.IMPORTANT]Ressourcenverwendung für elastische Pools wurde eine Verzögerung von mindestens 20 Minuten. Definieren von Alarmen von weniger als 30 Minuten für elastische wird derzeit nicht unterstützt. Alarme für elastische mit festgelegt (Parameter namens "-WindowSize" PowerShell-API) von weniger als 30 Minuten kann nicht ausgelöst werden. Stellen Sie sicher, dass Warnungen für elastische definieren einen Zeitraum (WindowSize) von mindestens 30 Minuten.

Dieses Beispiel fügt eine Warnung für die Benachrichtigung bei einem Pool eDTU Verbrauch bestimmten Schwellenwert überschreitet.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Alle Datenbanken in einem Pool Alerts hinzufügen

Alle Datenbank in einer elastischen Pool Benachrichtigungen senden Warnregeln hinzufügen oder Warnmeldungszeichenfolgen [URL Endpunkte](https://msdn.microsoft.com/library/mt718036.aspx) einer Ressource schlägt einen Auslastung Schwellenwert von der Warnung einrichten. 

> [AZURE.IMPORTANT] Ressourcenverwendung für elastische Pools wurde eine Verzögerung von mindestens 20 Minuten. Definieren von Alarmen von weniger als 30 Minuten für elastische wird derzeit nicht unterstützt. Alarme für elastische mit festgelegt (Parameter namens "-WindowSize" PowerShell-API) von weniger als 30 Minuten kann nicht ausgelöst werden. Stellen Sie sicher, dass Warnungen für elastische definieren einen Zeitraum (WindowSize) von mindestens 30 Minuten.

In diesem Beispiel fügt eine Warnung aller Datenbanken in einem Pool immer benachrichtigt, wenn die Datenbank DTU Verbrauch bestimmten Schwellenwert überschreitet.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Erfassen und Überwachen von Daten über mehrere Pools in einem Abonnement

Wenn Sie eine große Anzahl von Datenbanken in einem Abonnement haben, ist jeder elastische Pool separat überwacht umständlich. Stattdessen können SQL Datenbank-PowerShell-Cmdlets und T-SQL-Abfragen zum Sammeln von Daten aus mehreren Pools und ihre Datenbanken für Überwachung und Analyse der Ressourcen kombiniert werden. Eine [beispielhafte](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) eine Gruppe von Powershell-Skripten werden im Repository GitHub SQL Server-Beispiele und Dokumentation für Ihre Funktion und Ihre Verwendung finden.

Zum Verwenden dieses Implementierungsbeispiel gehen unten.


1. [Skripts und der Dokumentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools)herunterladen:
2. Ändern Sie die Skripts für Ihre Umgebung. Geben Sie einen oder mehrere Server mit elastischen Pools befinden.
3. Angeben einer Telemetrie dem erfassten Metriken gespeichert werden. 
4. Passen Sie das Skript, um die Dauer der Ausführung der Skripts angeben.

Auf einer hohen Ebene folgendermaßen Skripts:

*   Listet alle Server in einer bestimmten Azure-Abonnement (oder eine angegebene Liste von Servern).
*   Führt ein Hintergrundauftrag für jeden Server. Der Auftrag regelmäßig in einer Schleife ausgeführt und sammelt Telemetriedaten für alle Pools im Server. Daraufhin lädt er die Daten in der telemetriedatenbank angegebenen.
*   Listet Datenbanken in jedem Pool die Datenbankdaten sammeln. Daraufhin lädt er die Daten in die telemetriedatenbank.

Die erfassten Metriken Telemetrie Datenbank können analysiert werden, um elastische Pools und die Datenbanken überwachen. Das Skript installiert auch einer vordefinierten Tabellenwert Funktion (TVF), in der telemetriedatenbank Aggregat zu Metriken für einen angegebenen Zeitfenster. Z. B. Ergebnisse der Tabellenwertfunktion dienen "Top N elastische Pools mit der maximalen eDTU in einem angegebenen Zeitfenster." angezeigt Verwenden Sie optional Analysetools wie Excel oder Power BI Abfragen und Analysieren der gesammelten Daten.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Beispiel: Abrufen von Ressourcen Verbrauch Metriken für einen Pool und Datenbanken

Dieses Beispiel ruft die Metriken Verbrauch für einen bestimmten elastische Pools und alle Datenbanken. Daten formatiert und in eine formatierte CSV-Datei geschrieben. Die Datei kann mit Excel durchsucht werden.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Latenz elastischen Pool Vorgänge

- Min eDTUs pro Datenbank oder Max eDTUs pro Datenbank normalerweise ändern schließt in 5 Minuten oder weniger.
- EDTUs pro Pool ändern, hängt von den Gesamtbetrag aller Datenbanken im Pool verwendete. Ändert durchschnittlich 90 Minuten oder weniger pro 100 GB. Z. B. Gesamtspeicherplatz von alle Datenbanken im Pool wird 200 GB wird die erwartete Latenz zum Ändern der Pool-eDTU pro Pool 3 Stunden oder weniger.

## <a name="migrate-from-v11-to-v12-servers"></a>Migrieren von V11 V12-Servern

PowerShell-Cmdlets sind zum Starten, beenden oder Überwachen einer Aktualisierung zu Azure SQL-Datenbank V12 V11 oder andere Pre-V12-Version verfügbar.

- [Upgrade auf SQL Datenbank V12 mit PowerShell](sql-database-upgrade-server-powershell.md)

Referenzdokumentation über PowerShell-Cmdlets finden Sie unter:


- [AzureRMSqlServerUpgrade abrufen](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [AzureRMSqlServerUpgrade starten](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [AzureRMSqlServerUpgrade beenden](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Stop-Cmdlet bedeutet, um nicht anhalten. Gibt es keine Möglichkeit, eine Aktualisierung wieder beginnend fortsetzen. Stop-Cmdlet bereinigt und gibt alle Ressourcen frei.

## <a name="next-steps"></a>Nächste Schritte

- [Elastische Aufträge erstellen](sql-database-elastic-jobs-overview.md) Elastische Aufträge können Sie T-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool ausgeführt werden.
- [Dezentrales Skalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md)finden Sie unter: elastische Datenbanktools Dezentrales Skalieren, Verschieben von Daten verwenden, oder erstellen Sie Transaktionen.
