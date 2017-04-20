<properties
    pageTitle="Erste Schritte mit elastische Datenbankaufträge"
    description="wie elastische Datenbankaufträge"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Erste Schritte mit elastische Datenbank Aufträge

Elastische Datenbankaufträge (Vorschau) für Azure SQL-Datenbank können Zuverlässigkeit T-SQL-Skripts ausführen, die mehrere Datenbanken umfassen automatisch wiederholen und eventuelle Abschluss Garantien. Weitere Informationen zu der Funktion elastische Datenbank finden Sie unter [Übersicht über die Seite](sql-database-elastic-jobs-overview.md).

In diesem Thema erweitert das Beispiel finden in [Erste Schritte mit elastische Datenbanktools](sql-database-elastic-scale-get-started.md). Wenn abgeschlossen, wird: Informationen zum Erstellen und Verwalten von Aufträgen, die eine Gruppe von verwandten Datenbanken verwalten. Es ist nicht erforderlich, die elastische Skalierung Tools verwenden, um die Vorteile der elastische Aufträge.

## <a name="prerequisites"></a>Erforderliche Komponenten

Herunterladen Sie und führen Sie aus, die [Erste Schritte mit elastische Datenbank Tools](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Erstellen Sie einen Map-Manager die Beispiel-app mit

Hier erstellen ein shardzuordnung Manager und mehrere Fragmente, gefolgt von Daten in der Splitter Sie. Haben Sie bereits Splitter Sharding Daten einrichten, können die folgenden Schritte überspringen und zum nächsten Abschnitt wechseln.

1. Erstellen Sie und führen Sie die **Schritte mit elastische Datenbanktools** beispielanwendung. Gehen Sie Schritt 7 im Abschnitt [herunter und führen die Beispiel-app](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Ende Schritt 7 sehen Sie die folgende Befehlszeile:

    ![Befehlszeile][1]

2.  Geben Sie im Befehlsfenster "1" und drücken Sie die **EINGABETASTE**. Dies erstellt den Splitter Map-Manager und fügt zwei Splitter an den Server. Dann geben Sie "3" und drücken Sie die **EINGABETASTE**. Wiederholen Sie diesen Vorgang vier Mal. Dies fügt Beispiel Datenzeilen in der Splitter.

3.  [Azure-Portal](https://portal.azure.com) sollten drei neue Datenbanken auf dem Server v12 anzeigen:

    ![Visual Studio-Bestätigung][2]

    Jetzt erstellen wir eine benutzerdefinierte Datenbank-Auflistung, die alle Datenbanken in der shardzuordnung widerspiegelt. So können wir erstellen und Ausführen eines Auftrags, die Splitter eine neue Tabelle hinzufügen.

Hier würden wir normalerweise ein Splitter Karte Ziel erstellen mit dem **New-AzureSqlJobTarget** -Cmdlet. Splitter Karte Manager-Datenbank als Datenbank Ziel festgelegt werden und bestimmte shardzuordnung als Ziel angegeben. Stattdessen werden wir alle Datenbanken auf dem Server auflisten und fügen die Datenbank in die neue benutzerdefinierte Auflistung mit Ausnahme der master-Datenbank.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Erstellt eine benutzerdefinierte Auflistung und fügt alle Datenbanken auf dem Server auf benutzerdefinierte Auflistung außer Master.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Erstellen Sie eine T-SQL-Skript für die Ausführung in Datenbanken

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Erstellen Sie den Auftrag zum Ausführen eines Skripts für die benutzerdefinierte Gruppe von Datenbanken

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>Führen Sie das Projekt 

PowerShell-Skript kann verwendet werden, um einen vorhandenen Auftrag ausführen:

Aktualisieren Sie die folgende Variable entsprechend der gewünschten Auftragsname ausgeführt haben:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>Den Zustand der einzelnen Auftragsausführungsergebnisse abrufen

Verwenden Sie derselbe **Get-AzureSqlJobExecution** -Cmdlet mit dem Parameter **IncludeChildren** der untergeordneten Projekt wie nämlich den genauen Zustand für die Ausführung jedes Auftrags für jede Datenbank, die das Projekt abzielt Ansichtszustand.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>Anzeigen des Status für mehrere auftragsausführungen

Das Cmdlet " **Get-AzureSqlJobExecution** " verfügt über mehrere optionale Parameter verwendet werden können mehrere auftragsausführungen gefiltert durch die angegebenen Parameter. Das folgende Beispiel veranschaulicht einige der Methoden Get-AzureSqlJobExecution verwenden:

Alle aktiven obersten Ebene auftragsausführungen abrufen:

    Get-AzureSqlJobExecution

Rufen Sie alle Top-Level Job Ausführungen einschließlich inaktive Aufträge wie ab:

    Get-AzureSqlJobExecution -IncludeInactive

Rufen Sie aller untergeordneten Projekt Ausführungen eine bereitgestellte Ausführung Einzelvorgangsnummer, einschließlich inaktive Aufträge wie ab:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Abrufen aller auftragsausführungen erstellt einen Zeitplan / job Kombination aus inaktive Aufträge:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Rufen Sie alle Aufträge, die für eine angegebene shardzuordnung einschließlich inaktive Aufträge ab:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Rufen Sie alle Aufträge, die auf eine angegebene benutzerdefinierte Auflistung auch inaktive ab:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Ruft die Liste der Aufgabe auftragsausführungen in einen bestimmten Auftrag ausführen:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Abrufen von Auftragsdetails Task-Ausführung:

PowerShell-Skript lässt sich ein Auftrag Taskausführung Einzelheiten mit Debuggen Ausführungsfehler.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="retrieve-failures-within-job-task-executions"></a>Fehler in auftragsausführungen Aufgabe abrufen

Das JobTaskExecution-Objekt enthält eine Eigenschaft für den Lebenszyklus der Aufgabe sowie eine Eigenschaft. Wenn ein Auftrag Task fehlgeschlagen, Lifecycle-Eigenschaft auf *Fehler* festgelegt und die Message-Eigenschaft auf die resultierende Fehlermeldung und einen Stapel festgelegt. Wenn ein Auftrag nicht erfolgreich war, ist es wichtig die Details der Aufgaben anzeigen, die für einen bestimmten Auftrag nicht erfolgreich war.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Warten auf Auftragsausführungsergebnisse abgeschlossen

PowerShell-Skript kann verwendet werden, um für eine Projektaufgabe abgeschlossen warten:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Erstellen einer benutzerdefinierten Ausführungsrichtlinie

Elastische Datenbankaufträge unterstützt das Erstellen von benutzerdefinierten Ausführungsrichtlinien Aufträge gestartet angewendet werden können.
  
Ausführungsrichtlinien lässt derzeit definieren:

* Name: Bezeichner Ausführungsrichtlinie.
* Zeitlimit:-Gesamtzeit, bevor ein Auftrag elastische Datenbankaufträge abgebrochen wird.
* Ersten Wiederholungsintervall: Wartezeitintervall vor dem ersten Wiederholungsversuch.
* Maximale Wiederholungsintervall: Maximal Wiederholungsintervalle verwenden.
* Intervall Backoff Koeffizient Wiederholen: Koeffizienten zur Berechnung des nächsten Intervalls zwischen Wiederholungsversuchen.  Die folgende Formel verwendet: (ersten Wiederholungsintervall) * Math.pow ((Intervall Backoff Koeffizient) (Anzahl der Wiederholungsversuche) - 2). 
* Maximale Anzahl an versuchen: Die maximale Anzahl der Wiederholungsversuche versucht, innerhalb eines Projektes durchzuführen.

Die Standardrichtlinie für die Ausführung verwendet die folgenden Werte:

* Name: Standard-Ausführungsrichtlinie
* Zeitlimit: 1 Woche
* Ersten Wiederholungsintervall: 100 Millisekunden
* Maximale Wiederholungsintervall: 30 Minuten
* Koeffizienten Intervall wiederholen: 2
* Maximale Anzahl an versuchen: 2.147.483.647

Erstellen Sie die gewünschte Ausführungsrichtlinie:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Aktualisieren einer benutzerdefinierten Ausführungsrichtlinie

Aktualisieren Sie die gewünschte Ausführungsrichtlinie aktualisieren:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Abbrechen eines Auftrags

Elastische Datenbankaufträge unterstützt Aufträge abbrechen Anfragen.  Erkennt elastische Datenbankaufträge eine Absage für einen Auftrag, der gerade ausgeführt wird, wird versucht, den Auftrag beenden.

Es gibt zwei Arten, elastische Datenbankaufträge einen Abbruch durchführen können:

1. Abbrechen von derzeit ausgeführten Aufgaben: Wenn ein Abbruch erkannt wird, während ein Vorgang ausgeführt wird, ein Abbruch versucht in der aktuell ausgeführten Aspekt der Aufgabe.  Beispiel: wird eine Abfrage mit langer aktuell bei ein Abbruch wird ausgeführt, werden versucht die Abfrage abzubrechen.
2. Abbrechen Aufgabe Wiederholungsversuche: Wenn ein Abbruch vor eine Aufgabe für die Ausführung von der Steuerthread erkannt wird, der Steuerthread wird die Aufgabe starten vermeiden und deklarieren die Anforderung abgebrochen.

Wenn für ein übergeordnetes Projekt ein Job Abbruch angefordert wird, wird die Stornierung für das übergeordnete Projekt und alle seine untergeordneten Aufträge berücksichtigt.
 
Um eine Absage zu senden, verwenden Sie das Cmdlet **Stop-AzureSqlJobExecution** und den **JobExecutionId** -Parameter.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Löschen eines Auftrags nach Name und den Auftragsverlauf

Elastische Datenbankaufträge unterstützt asynchrone Aufträge. Ein Auftrag für die Löschung markiert werden und das System löscht den Auftrag und seine Auftragsverlauf nach Abschluss aller auftragsausführungen für das Projekt. Das System wird nicht automatisch aktive Ausführung abgebrochen.  

Stattdessen muss Stop AzureSqlJobExecution aufgerufen werden, um aktive Ausführung abzubrechen.

Auftrag löschen auslösen, verwenden Sie das Cmdlet " **Remove-AzureSqlJob** " und des **JobName** -Parameters.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Erstellen einer benutzerdefinierten Datenbank-Ziel
Benutzerdefinierte Datenbank Ziele können in elastische Datenbank Aufträge definiert die Ausführung direkt oder für die Aufnahme in einer benutzerdefinierten Datenbank-Gruppe verwendet werden kann. Da **Pools für elastische Datenbanken** sind noch nicht direkt unterstützt über PowerShell APIs, erstellen Sie einfach eine benutzerdefinierte Datenbank Ziel und benutzerdefinierten Datenbank Auflistung Ziel umfasst alle Datenbanken im Pool.

Legen Sie die folgenden Variablen gewünschte Datenbank Informationen:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Erstellen einer benutzerdefinierten Datenbank Auflistung Ziel
Eine benutzerdefinierte Datenbank Auflistung Ziel kann über mehrere Datenbank-Ziele können definiert werden. Nach dem Erstellen eine Datenbankgruppe können Datenbanken auf benutzerdefinierte Auflistung verknüpft werden.

Die folgenden Variablen entsprechend die gewünschten benutzerdefinierten Auflistung Zielkonfiguration festgelegt:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Datenbanken auf Auflistung eine benutzerdefinierte Datenbank hinzufügen

Datenbank-Ziele können benutzerdefinierte Datenbank Ziele zum Erstellen einer Gruppe von Datenbanken zugeordnet werden. Wenn ein Auftrag erstellt wird, die Auflistung benutzerdefinierter Ziel ausgerichtet wird erweitert, um die zum Zeitpunkt der Ausführung der Gruppe verbundenen Datenbanken als Ziel.

Fügen Sie die gewünschte Datenbank auf eine bestimmte benutzerdefinierte:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Überprüfen Sie die Datenbanken innerhalb einer benutzerdefinierten Datenbank-Auflistung

Verwenden Sie das Cmdlet " **Get-AzureSqlJobTarget** " untergeordnete Datenbanken innerhalb einer benutzerdefinierten Datenbank Auflistung abrufen. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Erstellen Sie einen Auftrag zum Ausführen eines Skripts in einer benutzerdefinierten Datenbank Auflistung Ziel

Verwenden Sie das Cmdlet **New-AzureSqlJob** zum Erstellen eines Auftrags für eine Gruppe von Datenbanken target Auflistung benutzerdefinierter definiert. Elastische Datenbankaufträge erweitern Sie das Projekt in mehrere untergeordnete Projekte jeweils einer Auflistung mit benutzerdefinierten Datenbank verbunden und das Skript für jede Datenbank ausgeführt. Wieder ist wichtig, dass Skripts Idempotent sind gegen Versuche.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Datensammlung in Datenbanken

**Elastische Datenbank Aufträge** unterstützt eine Abfrage über eine Gruppe von Datenbanken und sendet die Ergebnisse an einen angegebenen Datenbanktabelle. Die Tabelle kann anschließend an den Abfrageergebnissen aus jeder Datenbank abgefragt werden. Dies bietet einen asynchronen Mechanismus zum Ausführen einer Abfrage über mehrere Datenbanken. Fehlern wie Datenbanken wird vorübergehend erfolgt automatisch über Versuche.

Die angegebene Zieltabelle wird Wenn es noch nicht existiert das Schema des zurückgegebenen Resultsets entsprechen, automatisch erstellt. Wenn die Ausführung eines Skripts mehrere Resultsets zurückgibt, sendet elastische Datenbank Aufträge nur den ersten in der bereitgestellten Zieltabelle.

PowerShell-Skript kann zur Ausführung eines Skripts, sammeln die Ergebnisse in einer angegebenen Tabelle verwendet werden. Dieses Skript setzt voraus, dass ein T-SQL-Skript wurde die ein einzelnes Resultset ausgegeben und Ziel Auflistung benutzerdefinierten Datenbank erstellt wurde.

Legen Sie das gewünschte Skript, Anmeldeinformationen und Ausführungsziel Folgendes:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Erstellen und Starten eines Einzelvorgangs Auflistung Szenarien
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Erstellen eines Zeitplans zum Ausführen eines Triggers Auftrag

PowerShell-Skript kann verwendet werden, um einen wiederkehrenden Zeitplan zu erstellen. Dieses Skript verwendet ein Intervall 1 Minute, neu AzureSqlJobSchedule unterstützt aber auch - DayInterval und -HourInterval, - MonthInterval, WeekInterval - Parameter. Zeitpläne, die nur einmal ausgeführt, entstehen durch Übergabe - einmalige.

Erstellen Sie einen neuen Zeitplan:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Erstellen eines Triggers Auftrag um einen Auftrag nach einem Zeitplan ausgeführt

Job-Trigger kann definiert werden, um einen Auftrag nach einem Zeitplan ausgeführt. PowerShell-Skript kann verwendet werden, einen Job-Trigger erstellen.

Legen Sie das gewünschte Projekt und Zeitplan entsprechen die folgenden Variablen:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Entfernen einer geplanten Zuordnung Auftrag planmäßig ausführen

Wiederkehrende Auftragsausführungsergebnisse durch einen Auftrag Trigger einzustellen, kann Trigger Auftrag entfernt. Entfernen eines auftragsauslösers um einen Auftrag mithilfe des Cmdlets **Entfernen AzureSqlJobTrigger** Zeitplan ausgeführt wird.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Elastische Datenbank Abfrageergebnisse in Excel importieren

 Sie können die Ergebnisse einer Abfrage in eine Exceldatei importieren.

1. Starten Sie Excel 2013.
2.  Navigieren Sie zu der Multifunktionsleiste **Daten** .
3.  Klicken Sie **Aus anderen Quellen** und **Aus SQL Server**.

    ![Excel importieren aus anderen Quellen][5]
4.  Geben Sie im **Datenverbindungs-Assistenten** die Server und der Anmeldeinformationen. Klicken Sie auf **Weiter**.
5.  Wählen Sie im Dialogfeld **Wählen Sie die Datenbank, die die Daten enthält**die **ElasticDBQuery** -Datenbank.
6.  Wählen Sie die Tabelle **Customers** in der Liste und klicken Sie auf **Weiter**. Klicken Sie auf **Fertig stellen**.
7.  Wählen Sie im Formular **Daten importieren** unter **Wählen Sie, wie Sie diese Daten in der Arbeitsmappe anzeigen möchten** **Tabelle** und klicken Sie auf **OK**.

Alle Zeilen aus verschiedenen Splitter gespeicherte Tabelle **Kunden** Auffüllen der Excel-Tabelle.

## <a name="next-steps"></a>Nächste Schritte
Jetzt können Excel Funktionen. Verwenden Sie die Verbindungszeichenfolge mit Servername, Datenbankname und Anmeldeinformationen Verbindung der Integrationstools BI- und Daten zur Abfragedatenbank elastische. Stellen Sie sicher, dass SQL Server als Datenquelle für das Tool unterstützt wird. Finden Sie die elastische Abfragedatenbank und externen Tabellen wie jede andere SQL Server-Datenbank SQL Server-Tabellen, die mit dem Tool herstellen möchten.

### <a name="cost"></a>Kosten
Es ist kostenlos mit der Abfragefunktion elastische Datenbank. Zu diesem Zeitpunkt dieses Feature steht nur für Premium-Datenbanken als Endpunkt jedoch den Splitter sind alle Dienstebene.

Preisinformationen finden Sie unter [SQL Datenbank Preisdetails](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
