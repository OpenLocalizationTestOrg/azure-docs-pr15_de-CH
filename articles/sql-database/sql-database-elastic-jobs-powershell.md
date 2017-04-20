<properties 
    pageTitle="Elastische Datenbank stellen mit PowerShell erstellen und verwalten" 
    description="PowerShell zum Verwalten von Azure SQL-Datenbank-pools" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Erstellen Sie und verwalten Sie einer SQL-Datenbank elastische Datenbankaufträge mit PowerShell (Vorschau)

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)



Die PowerShell-APIs für **Aufträge elastische Datenbank** (in der Vorschau) können Sie definieren eine Gruppe von Datenbanken für die Skripts ausgeführt werden. Dieser Artikel veranschaulicht die **elastische Datenbank Aufträge** mithilfe von PowerShell-Cmdlets erstellen und verwalten. [Elastische Aufträge](sql-database-elastic-jobs-overview.md)Übersicht. 

## <a name="prerequisites"></a>Erforderliche Komponenten
* Ein Azure-Abonnement. Eine kostenlose Testversion finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/).
* Eine Gruppe von Datenbanken mit elastische Datenbanktools erstellt. [Erste Schritte mit elastische Datenbanktools](sql-database-elastic-scale-get-started.md)anzeigen
* Azure PowerShell. Ausführliche Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
* **Elastische Datenbank Aufträge** PowerShell-Paket: Stellenangebote [Elastische Datenbank installieren](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Wählen Sie Ihre Azure-Abonnement

Aktivieren des Abonnements benötigen Sie Ihre Abonnement-Id (**-SubscriptionId**) oder Abonnement Namen (**-SubscriptionName**). Haben Sie mehrere Abonnements führen Sie das Cmdlet " **Get-AzureRmSubscription** " und die gewünschte Abonnementinformationen aus der Ergebnismenge kopieren. Haben Sie Ihre Abonnementinformationen, führen Sie das folgende Cmdlet zum Festlegen dieses Abonnements standardmäßig, nämlich das Ziel erstellen und Verwalten von Aufträgen:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

[PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) wird empfohlen, für die Verwendung zu PowerShell-Skripts für Aufträge elastische Datenbank ausführen.

## <a name="elastic-database-jobs-objects"></a>Elastische Datenbankobjekte Aufträge

Die folgende Tabelle listet alle Objekttypen **Elastische Datenbank** Aufträge Beschreibung und relevante PowerShell APIs.

<table style="width:100%">
  <tr>
    <th>Objekttyp</th>
    <th>Beschreibung</th>
    <th>Verwandte PowerShell-APIs</th>
  </tr>
  <tr>
    <td>Anmeldeinformationen</td>
    <td>Benutzername und Kennwort beim Verbinden mit Datenbanken für die Ausführung von Skripts oder Anwendung von DACPACs verwenden. <p>Das Kennwort verschlüsselt senden und in der Datenbank elastische Datenbank speichern.  Das Kennwort wird vom Dienst elastische Datenbankaufträge über Anmeldeinformationen erstellt und übergeben hochgeladen entschlüsselt.</td>
    <td><p>AzureSqlJobCredential abrufen</p>
    <p>Neue AzureSqlJobCredential</p><p>AzureSqlJobCredential festlegen</p></td></td>
  </tr>

  <tr>
    <td>Skript</td>
    <td>Transact-SQL-Skript für die Ausführung in Datenbanken verwendet werden.  Das Skript sollte Idempotent sein, da der Dienst Ausführung des Skripts auf Fehler erneut erstellt werden.
    </td>
    <td>
    <p>AzureSqlJobContent abrufen</p>
    <p>AzureSqlJobContentDefinition abrufen</p>
    <p>Neue AzureSqlJobContent</p>
    <p>AzureSqlJobContentDefinition festlegen</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Anwendung auf Datenebene </a> Paket auf Datenbanken angewendet werden.

    </td>
    <td>
    <p>AzureSqlJobContent abrufen</p>
    <p>Neue AzureSqlJobContent</p>
    <p>AzureSqlJobContentDefinition festlegen</p>
    </td>
  </tr>
  <tr>
    <td>Datenbank-Ziel</td>
    <td>Datenbank und Name einer Azure SQL-Datenbank auf.

    </td>
    <td>
    <p>AzureSqlJobTarget abrufen</p>
    <p>Neue AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Splitter Karte Ziel</td>
    <td>Kombination aus Datenbank Ziel und Anmeldeinformationen in einer shardzuordnung elastische Datenbank gespeicherte Informationen zu ermitteln.
    </td>
    <td>
    <p>AzureSqlJobTarget abrufen</p>
    <p>Neue AzureSqlJobTarget</p>
    <p>AzureSqlJobTarget festlegen</p>
    </td>
  </tr>
<tr>
    <td>Benutzerdefinierte Auflistung Ziel</td>
    <td>Gruppe von Datenbanken gemeinsam für Ausführung verwendet.</td>
    <td>
    <p>AzureSqlJobTarget abrufen</p>
    <p>Neue AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Benutzerdefinierte Auflistung untergeordnetes Ziel</td>
    <td>Datenbank-Ziel, das aus einer benutzerdefinierten Auflistung verwiesen wird.</td>
    <td>
    <p>Hinzufügen AzureSqlJobChildTarget</p>
    <p>AzureSqlJobChildTarget entfernen</p>
    </td>
  </tr>

<tr>
    <td>Auftrag</td>
    <td>
    <p>Definition von Parametern für einen Auftrag, der Ausführung auslösen oder zu einem Zeitplan verwendet werden kann.</p>
    </td>
    <td>
    <p>AzureSqlJob abrufen</p>
    <p>Neue AzureSqlJob</p>
    <p>AzureSqlJob festlegen</p>
    </td>
  </tr>

<tr>
    <td>Ausführung des Auftrags</td>
    <td>
    <p>Gemäß einer behandelt Container Aufgaben erfüllen Skript ausführen oder eine DACPAC mit Anmeldeinformationen für Datenbankverbindungen mit Fehlern auf anwenden.</p>
    </td>
    <td>
    <p>AzureSqlJobExecution abrufen</p>
    <p>AzureSqlJobExecution starten</p>
    <p>AzureSqlJobExecution beenden</p>
    <p>AzureSqlJobExecution warten</p>
  </tr>

<tr>
    <td>Ausführung des Auftrags-Aufgabe</td>
    <td>
    <p>Arbeitseinheit um einen Auftrag zu erfüllen.</p>
    <p>Wenn eine Aufgabe nicht erfolgreich kann ausführen die resultierende Ausnahmemeldung protokolliert und eine entsprechende neue Projektaufgabe erstellt und gemäß den angegebenen Ausführungsrichtlinie ausgeführt.</p></p>
    </td>
    <td>
    <p>AzureSqlJobExecution abrufen</p>
    <p>AzureSqlJobExecution starten</p>
    <p>AzureSqlJobExecution beenden</p>
    <p>AzureSqlJobExecution warten</p>
  </tr>

<tr>
    <td>Job-Ausführungsrichtlinie</td>
    <td>
    <p>Steuerelemente Auftrag Ausführung Timeouts und Höchstwerte zwischen Wiederholungsversuchen.</p>
    <p>Elastische Datenbankaufträge enthält eine Auftrag Ausführung Standardrichtlinie praktisch unbegrenzte Versuche Auftragsfehler Aufgabe mit exponentielle Backoff zwischen jedem Neuversuch führen.</p>
    </td>
    <td>
    <p>AzureSqlJobExecutionPolicy abrufen</p>
    <p>Neue AzureSqlJobExecutionPolicy</p>
    <p>AzureSqlJobExecutionPolicy festlegen</p>
    </td>
  </tr>

<tr>
    <td>Zeitplan</td>
    <td>
    <p>Zeitbasierte Spezifikation für die Ausführung in wiederkehrenden Intervallen oder zu einem bestimmten Zeitpunkt stattfinden.</p>
    </td>
    <td>
    <p>AzureSqlJobSchedule abrufen</p>
    <p>Neue AzureSqlJobSchedule</p>
    <p>AzureSqlJobSchedule festlegen</p>
    </td>
  </tr>

<tr>
    <td>Job-Trigger</td>
    <td>
    <p>Eine Zuordnung zwischen einem Projekt und einen Zeitplan für die Ausführung von Triggern Auftrag planmäßig.</p>
    </td>
    <td>
    <p>Neue AzureSqlJobTrigger</p>
    <p>AzureSqlJobTrigger entfernen</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Unterstützte elastische Datenbank Aufträge gruppieren Typen
Der Auftrag führt Transact-SQL (T-SQL)-Skripts oder Anwendung von DACPACs für eine Gruppe von Datenbanken. Bei einem Projekt für eine Gruppe von Datenbanken ausgeführt werden der Auftrag "Erweitert" die in untergeordneten Aufträge, jedes angeforderte Ausführungsebene für eine Datenbank in der Gruppe führt. 
 
Es gibt zwei Arten von Gruppen, die Sie erstellen können: 

* [Shardzuordnung](sql-database-elastic-scale-shard-map-management.md) Gruppe: bei ein Auftrag ein shardzuordnung Ziel der Auftrag fragt shardzuordnung um seine aktuelle Splitter bestimmen und erstellt dann untergeordnete Aufträge für jeden Splitter Splitter zuordnen.
* Benutzerdefinierte Gruppe: eine benutzerdefinierte Datenbanken. Wenn ein Projekt eine benutzerdefinierte Auflistung abzielt, erstellt er untergeordnete Aufträge für jede Datenbank derzeit in der benutzerdefinierten Auflistung.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Um die elastische Datenbank Aufträge Verbindung

Eine Verbindung soll zu der *Datenbank* vor mit den APIs festgelegt werden. Dieses Cmdlet ausführen, wird ein Anmeldeinformationen anfordern, Benutzernamen und Kennwort erstellt, wenn Aufträge elastische Datenbank installieren angezeigt. Alle Beispiele in diesem Thema wird davon ausgegangen, dass dieser erste Schritt bereits durchgeführt wurde.

Öffnen einer Verbindung zur elastische Datenbank Aufträge:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Verschlüsselte Anmeldeinformationen in die elastische Datenbank Aufträge

Anmeldeinformationen können und das Kennwort verschlüsselt in der *Datenbank* eingefügt. Sind zum Speichern von Anmeldeinformationen, um Aufträge zu einem späteren Zeitpunkt ausgeführt werden (mit Auftragszeitpläne) aktivieren.
 
Verschlüsselung arbeitet als Teil des Installationsskripts erstellt. Das Installationsskript erstellt und lädt das Zertifikat in Azure-Cloud-Dienst für die Entschlüsselung der gespeicherten verschlüsselten Kennwörter. Azure-Cloud-Dienst speichert später den öffentlichen Schlüssel in der *Datenbank* ermöglicht die PowerShell-API oder Azure-Verwaltungsportal Schnittstelle angegebenen Kennwort verschlüsselt, ohne das Zertifikat lokal installiert werden.
 
Anmeldeinformationen Kennwörter sind verschlüsselt und Benutzer mit Lesezugriff elastische Aufträge Datenbankobjekte geschützt. Jedoch kann ein böswilliger Benutzer mit Lese-/ Schreibzugriff elastische Datenbankaufträge Objekte ein extrahieren. Anmeldeinformationen sollen über auftragsausführungen wiederverwendet werden. Anmeldeinformationen werden beim Herstellen der Verbindung zu Zieldatenbanken übergeben. Derzeit gibt es keine Einschränkung für die Zieldatenbanken für bestimmte Anmeldeinformationen verwendet, böswilliger Benutzer konnte Datenbank Ziel für eine Datenbank unter Kontrolle der böswillige Benutzer hinzufügen. Der Benutzer kann anschließend einen Auftrag für diese Datenbank die Anmeldeinformationen Kennwort zu starten.

Bewährte Sicherheitsmethoden für elastische Datenbank Projekte umfassen:

* Einschränken der APIs für vertrauenswürdige Personen.
* Anmeldeinformationen müssen die geringsten Berechtigungen zum Durchführen der Projektaufgabe.  Diese [Autorisierung und Berechtigungen](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-Artikel Weitere Informationen sehen.

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Verschlüsselte Anmeldeinformationen für die Ausführung des Auftrags auf Datenbanken erstellen

Erstellen Sie eine neue verschlüsselte Anmeldeinformationen fordert das [**Get-Credential-Cmdlet**](https://technet.microsoft.com/library/hh849815.aspx) einen Benutzernamen und ein Kennwort, das [**neue AzureSqlJobCredential-Cmdlet**](https://msdn.microsoft.com/library/mt346063.aspx)übergeben werden kann.

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Anmeldeinformationen aktualisieren

Wenn Kennwörter ändern, verwenden Sie das [**Cmdlet "Set-AzureSqlJobCredential"**](https://msdn.microsoft.com/library/mt346062.aspx) und legen Sie den Parameter **CredentialName** .

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Ein elastische Datenbank Splitter Karte Ziel definieren

Zum Ausführen eines Auftrags für alle Datenbanken in einer Splitter (erstellt mit [elastische Datenbank-Clientbibliothek](sql-database-elastic-database-client-library.md)) verwenden Sie eine shardzuordnung als Datenbank-Ziel. Dieses Beispiel erfordert eine Sharding mit der Clientbibliothek elastische Datenbank erstellte Anwendung. [Erste Schritte mit elastische Datenbank Tools](sql-database-elastic-scale-get-started.md)anzeigen

Splitter Karte Manager-Datenbank als Ziel Datenbank festgelegt werden muss und dann bestimmte shardzuordnung muss als Ziel angegeben werden.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Erstellen Sie eine T-SQL-Skript für die Ausführung in Datenbanken

Beim Erstellen von T-SQL-Skripts für die Ausführung ist das empfohlene [Idempotent](https://en.wikipedia.org/wiki/Idempotence) zu erstellen und vor Fehlern. Aufträge für die elastische Datenbank wiederholt Ausführung eines Skripts bei jeder Ausführung ein Fehlers, unabhängig von der Einstufung des Fehlers gefunden.

Verwenden Sie das [**New-AzureSqlJobContent-Cmdlet**](https://msdn.microsoft.com/library/mt346085.aspx) erstellen und speichern Sie ein Skript für die Ausführung und die **Fehlt das ContentName-** und **CommandText -** Parameter.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Erstellen Sie ein neues Skript aus einer Datei
Wenn das T-SQL-Skript in einer Datei definiert ist, verwenden Sie, um das Skript zu importieren:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Ein T-SQL-Skript für die Ausführung auf Datenbanken aktualisieren  

PowerShell-Skript aktualisiert den T-SQL-Befehlstext für ein vorhandenes Skript.

Legen Sie die folgenden Variablen entsprechend die gewünschten Skriptdefinition festgelegt werden:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Die Definition eines vorhandenen Skripts aktualisieren

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Beim Erstellen eines Auftrags zum Ausführen eines Skripts über eine Karte Splitter

PowerShell-Skript startet einen Auftrag für die Ausführung eines Skripts über jede Splitter einer elastischen Skalierung Splitter-Zuordnung.

Legen Sie das gewünschte Skript und Ziel die folgenden Variablen:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Zur Ausführung eines Auftrags 

PowerShell-Skript führt einen vorhandenen Auftrag:

Aktualisieren Sie die folgende Variable entsprechend der gewünschten Auftragsname ausgeführt haben:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Den Zustand der einzelnen Auftragsausführungsergebnisse abrufen

Verwenden Sie das [**Cmdlet "Get-AzureSqlJobExecution"**](https://msdn.microsoft.com/library/mt346058.aspx) und den **JobExecutionId** -Parameter den Status der Ausführung des Auftrags anzeigen.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Verwenden Sie derselbe **Get-AzureSqlJobExecution** -Cmdlet mit dem Parameter **IncludeChildren** der untergeordneten Projekt wie nämlich den genauen Zustand für die Ausführung jedes Auftrags für jede Datenbank, die das Projekt abzielt Ansichtszustand.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Der Ansichtszustand über mehrere auftragsausführungen

Das [**Cmdlet "Get-AzureSqlJobExecution"**](https://msdn.microsoft.com/library/mt346058.aspx) hat mehrere optionale Parameter, mit der mehrere auftragsausführungen durch den bereitgestellten Parametern gefiltert angezeigt. Das folgende Beispiel veranschaulicht einige der Methoden Get-AzureSqlJobExecution verwenden:

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

## <a name="to-retrieve-failures-within-job-task-executions"></a>Fehler in auftragsausführungen Aufgabe abrufen

Das **JobTaskExecution-Objekt** enthält eine Eigenschaft für den Lebenszyklus der Aufgabe sowie eine Eigenschaft. Wenn ein Auftrag Task fehlgeschlagen, Lifecycle-Eigenschaft auf *Fehler* festgelegt und die Message-Eigenschaft auf die resultierende Fehlermeldung und einen Stapel festgelegt. Wenn ein Auftrag nicht erfolgreich war, ist es wichtig die Details der Aufgaben anzeigen, die für einen bestimmten Auftrag nicht erfolgreich war.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Warten Auftragsausführungsergebnisse abgeschlossen

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
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
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

Elastische Datenbankaufträge unterstützt Anfragen Stornierung von Aufträgen.  Erkennt elastische Datenbankaufträge eine Absage für einen Auftrag, der gerade ausgeführt wird, wird versucht, den Auftrag beenden.

Es gibt zwei Arten, elastische Datenbankaufträge einen Abbruch durchführen können:

1. Derzeit ausgeführten Aufgaben Abbrechen: Wenn ein Abbruch erkannt wird, während ein Vorgang ausgeführt wird, ein Abbruch versucht in der aktuell ausgeführten Aspekt der Aufgabe.  Beispiel: wird eine Abfrage mit langer aktuell bei ein Abbruch wird ausgeführt, werden versucht die Abfrage abzubrechen.
2. Aufgabe Wiederholungsversuche Abbrechen: Wenn ein Abbruch vor eine Aufgabe für die Ausführung von der Steuerthread erkannt wird, der Steuerthread vermeiden die Aufgabe starten und deklarieren die Anforderung abgebrochen.

Wenn für ein übergeordnetes Projekt ein Job Abbruch angefordert wird, wird die Stornierung für das übergeordnete Projekt und alle seine untergeordneten Aufträge berücksichtigt.
 
Um eine Absage zu senden, verwenden Sie das [**Cmdlet "Stop AzureSqlJobExecution"**](https://msdn.microsoft.com/library/mt346053.aspx) und legen Sie den Parameter **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Löschen eines Auftrags und Verlauf asynchron job

Elastische Datenbankaufträge unterstützt asynchrone Aufträge. Ein Auftrag für die Löschung markiert werden und das System löscht den Auftrag und seine Auftragsverlauf nach Abschluss aller auftragsausführungen für das Projekt. Das System wird nicht automatisch aktive Ausführung abgebrochen.  

Rufen Sie [**Stop AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) um aktive Ausführung abzubrechen.

Auftrag löschen auslösen, verwenden Sie das [**Cmdlet "Remove-AzureSqlJob"**](https://msdn.microsoft.com/library/mt346083.aspx) und des **JobName** -Parameters.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Zum Erstellen einer benutzerdefinierten Datenbank

Sie können benutzerdefinierte Datenbank Ziele für die direkte Ausführung oder für die Aufnahme in einer benutzerdefinierten Datenbankgruppe definieren. Da **elastische Datenbank Pools** sind noch nicht direkt unterstützt PowerShell-APIs verwenden, können Sie z. B. Erstellen einer benutzerdefinierten Datenbank Ziel und benutzerdefinierten Datenbank Auflistung Ziel umfasst alle Datenbanken im Pool.

Legen Sie die folgenden Variablen gewünschte Datenbank Informationen:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Zum Erstellen einer benutzerdefinierten Datenbank-Auflistung

Verwenden Sie das Cmdlet [**Neu AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) eine benutzerdefinierte Datenbank Auflistung Ziel können über mehrere Datenbank-Ziele definieren. Nach dem Erstellen einer Datenbankgruppe können Datenbanken benutzerdefinierte Auflistung Ziel zugeordnet werden.

Die folgenden Variablen entsprechend die gewünschten benutzerdefinierten Auflistung Zielkonfiguration festgelegt:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Ziel-Auflistung benutzerdefinierter Datenbanken hinzufügen

Fügen Sie eine Datenbank auf eine bestimmte benutzerdefinierte [**Add-AzureSqlJobChildTarget**](https://msdn.microsoft.comlibrary/mt346064.aspx) -Cmdlet verwenden.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Überprüfen Sie die Datenbanken innerhalb einer benutzerdefinierten Datenbank-Auflistung

Verwenden Sie das Cmdlet " [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) " untergeordnete Datenbanken innerhalb einer benutzerdefinierten Datenbank Auflistung abrufen. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Erstellen Sie einen Auftrag zum Ausführen eines Skripts in einer benutzerdefinierten Datenbank Auflistung Ziel

Verwenden Sie das Cmdlet [**New-AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) zum Erstellen eines Auftrags für eine Gruppe von Datenbanken target Auflistung benutzerdefinierter definiert. Elastische Datenbankaufträge erweitern Sie das Projekt in mehrere untergeordnete Projekte jeweils einer Auflistung mit benutzerdefinierten Datenbank verbunden und das Skript für jede Datenbank ausgeführt. Wieder ist wichtig, dass Skripts Idempotent sind gegen Versuche.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Datensammlung in Datenbanken

Sie können einen Auftrag eine Abfrage über eine Gruppe von Datenbanken ausführen und die Ergebnisse für eine bestimmte Tabelle. Die Tabelle kann anschließend an den Abfrageergebnissen aus jeder Datenbank abgefragt werden. Dadurch wird eine asynchrone Methode zum Ausführen einer Abfrage über mehrere Datenbanken. Fehlgeschlagene Versuche werden automatisch über Versuche behandelt.

Die angegebenen Tabelle wird automatisch erstellt, wenn noch nicht vorhanden ist. Die neue Tabelle entspricht das Schema des zurückgegebenen Resultsets. Wenn ein Skript auf mehrere Resultsets zurückgibt, sendet Aufträge elastische Datenbank nur die erste in die Zieltabelle.

PowerShell-Skript führt ein Skript aus und sammelt die Ergebnisse in einer angegebenen Tabelle. Dieses Skript nimmt ein T-SQL-Skript die gibt eines einzelnes Resultsets erstellt wurde und eine benutzerdefinierte Datenbank Auflistung Ziel erstellt wurde.

Dieses Skript verwendet das Cmdlet " [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) ". Festlegen Sie Parameter für das Skript, Anmeldeinformationen und Ausführungsziel:

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Erstellen und Starten eines Auftrags für Auflistung Szenarien

Dieses Skript verwendet das [**Start-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) -Cmdlet.
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>So planen Sie einen Auftrag ausführen trigger

PowerShell-Skript kann zu einem wiederkehrenden Zeitplan verwendet werden. Dieses Skript verwendet Minutentakt, [**Neue AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) unterstützt aber auch - DayInterval und -HourInterval, - MonthInterval, WeekInterval - Parameter. Zeitpläne, die nur einmal ausgeführt, entstehen durch Übergabe - einmalige.

Erstellen Sie einen neuen Zeitplan:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Trigger einen Auftrag nach einem Zeitplan ausgeführt

Job-Trigger kann definiert werden, um einen Auftrag nach einem Zeitplan ausgeführt. PowerShell-Skript kann verwendet werden, einen Job-Trigger erstellen.

Verwenden Sie [Neu AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) , und legen Sie das gewünschte Projekt und Zeitplan entsprechen die folgenden Variablen:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Geplante Assoziation zu Auftrag planmäßig ausführen

Wiederkehrende Auftragsausführungsergebnisse durch einen Auftrag Trigger einzustellen, kann Trigger Auftrag entfernt. Entfernen eines Auslösers Job zu einem Auftrag planmäßig mit dem [**Cmdlet "Remove-AzureSqlJobTrigger"**](https://msdn.microsoft.com/library/mt346070.aspx)ausgeführt werden.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Rufen Sie Job-Auslöser an einen Zeitplan gebunden ab

PowerShell-Skript kann verwendet werden, und Auftrag Trigger registriert einen bestimmten Zeitplan angezeigt.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Rufen Sie Auftrag Trigger gebunden zu einem Auftrag 

Verwenden Sie [Get-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) und Zeitpläne mit registrierten Auftrag anzuzeigen.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Erstellen eine Anwendung auf Datenebene (DACPAC) für die Ausführung über Datenbanken

Erstellen Sie eine DACPAC finden Sie unter [Data-Tier Applications](https://msdn.microsoft.com/library/ee210546.aspx). Verwenden Sie zum Bereitstellen einer DACPAC [Cmdlet "New-AzureSqlJobContent"](https://msdn.microsoft.com/library/mt346085.aspx). Die DACPAC muss auf den Dienst zugreifen können. Es wird empfohlen, erstellte DACPAC Azure-Speicher hochgeladen und [SAS](../storage/storage-dotnet-shared-access-signature-part-1.md) für das DACPAC erstellen.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Aktualisieren eine Anwendung auf Datenebene (DACPAC) für die Ausführung in Datenbanken

Bestehende DACPACs innerhalb von elastische Datenbankaufträge können neue URIs zeigen aktualisiert werden. Verwenden Sie das [**Cmdlet "Set-AzureSqlJobContentDefinition"**](https://msdn.microsoft.com/library/mt346074.aspx) DACPAC URI auf eine vorhandene registrierte DACPAC aktualisieren:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Beim Erstellen eines Auftrags um eine Datenebene (DACPAC) über Datenbanken Anwendung

Nachdem eine DACPAC in Aufträge für die elastische Datenbank erstellt wurde, kann ein Auftrag anwenden der DACPAC über eine Gruppe von Datenbanken erstellt werden. PowerShell-Skript kann zum Erstellen eines Auftrags DACPAC über eine benutzerdefinierte Auflistung von Datenbanken verwendet werden:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
