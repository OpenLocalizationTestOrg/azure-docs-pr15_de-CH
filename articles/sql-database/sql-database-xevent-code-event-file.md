<properties 
    pageTitle="XEvent Datei Code für SQL-Datenbank | Microsoft Azure" 
    description="PowerShell und Transact-SQL bietet für ein Zweiphasen Codebeispiel zur Datei Ziel in einem erweiterten Ereignis in Azure SQL-Datenbank. Azure-Speicher ist ein erforderlicher Teil dieses Szenarios." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Ereigniscode Datei Ziel für erweiterte Ereignisse in SQL-Datenbank

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Ein vollständiges Codebeispiel möchten für eine fehleranfällige Methode zum Erfassen und Bericht Informationen für eine erweiterte Ereignis.


In Microsoft SQL Server dient der [Ereignisdatei Ziel](http://msdn.microsoft.com/library/ff878115.aspx) Ereignis Ausgaben eine Festplatte speichern. Diese Dateien sind jedoch nicht zur Azure SQL-Datenbank. Stattdessen verwenden wir Azure Storage-Diensts, unterstützen die Ereignisdatei.


In diesem Thema werden zwei Phasen Codebeispiel:


- PowerShell Azure Storage-Container in der Cloud erstellen.

- Transact-SQL:
 - Azure Storage-Container auf eine Datei zuweisen.
 - Zum Erstellen und Starten der Veranstaltung, usw.


## <a name="prerequisites"></a>Erforderliche Komponenten


- Ein Azure-Konto und ein Abonnement. Sie können sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)anmelden.


- Jede Datenbank können Sie eine Tabelle in.
 - Optional können Sie [erstellen eine Datenbank **AdventureWorksLT** ](sql-database-get-started.md) in Minuten.


- SQL Server Management Studio (ssms.exe) im Idealfall die letzte monatliche Updateversion. Sie können die neueste ssms.exe aus:
 - Thema [SQL Server Management Studio herunterzuladen](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Einen Link zum Download.](http://go.microsoft.com/fwlink/?linkid=616025)


- Sie müssen die [Azure PowerShell-Module](http://go.microsoft.com/?linkid=9811175) installiert.
 - Module stellen Befehle wie - **AzureStorageAccount neu**.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Phase 1: PowerShell-Code für Azure Storage container


Diese PowerShell ist Phase 1 des zwei-Phasen-Codebeispiel.

Das Skript beginnt mit Befehlen Bereinigen nach eine vorherigen möglich, und dass.



1. Fügen Sie das PowerShell-Skript in einem einfachen Texteditor wie Notepad.exe, und speichern Sie das Skript als Datei mit der Erweiterung **. ps1**.

2. Starten Sie PowerShell ISE als Administrator.

3. Geben Sie bei der Aufforderung<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>und drücken Sie die EINGABETASTE.

4. Öffnen Sie in PowerShell ISE **. ps1** -Datei. Führen Sie das Skript.

5. Das Skript startet zunächst ein neues Fenster, in dem Sie in Azure anmelden.
 - Wenn Sie das Skript ohne Unterbrechung der Sitzung erneut ausführen, können Sie bequem den Befehl **Add-AzureAccount** kommentieren.


![PowerShell ISE mit Azure-Modul installiert, Skript ausführen.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Beachten Sie die benannte Werte, die am Ende das PowerShell-Skript ausgibt. Sie müssen diese Werte in Transact-SQL-Skript bearbeiten, die Phase 2 folgt.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Phase 2: Transact-SQL-Code, Azure Storage Container verwendet


- In Phase 1 des dieses Codebeispiels haben Sie ein PowerShell-Skript erstellen einen Azure Storage Container ausgeführt.
- In Phase 2 muss folgende Transact-SQL-Skript weiter Container verwenden.


Das Skript beginnt mit Befehlen Bereinigen nach eine vorherigen möglich, und dass.


PowerShell-Skript gedruckt einige benannte Werte, wenn es beendet. Bearbeiten Sie das Transact-SQL-Skript, um diesen Werten. **TODO** in Transact-SQL-Skript die Bearbeitungspunkte zu suchen.


1. Öffnen Sie SQL Server Management Studio (ssms.exe).

2. Verbinden Sie mit Ihrem Azure SQL-Datenbank.

3. Klicken Sie, um eine neue Abfrage zu öffnen.

4. Fügen Sie das folgende Transact-SQL-Skript im Abfragebereich.

5. Finden Sie alle **TODO** im Skript und die entsprechenden Änderungen.

6. Speichern und dann das Skript ausführen.


&nbsp;


> [AZURE.WARNING] Der SAS-Schlüsselwert von Powershellskript generierte beginnt mit einem '?' (Fragezeichen). Wenn Sie die SAS-Taste folgende T-SQL-Skript verwenden, müssen Sie *entfernen den führenden '?'*. Andernfalls könnte sich Sicherheit blockiert.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Ziel nicht anfügen, wenn Sie ausführen, müssen Sie beenden und Starten der Veranstaltung:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Ausgabe


Abschluss von Transact-SQL-Skript, klicken Sie auf eine Zelle unter der Spaltenüberschrift **Event_data_XML** . Ein **<event>** Element wird eine UPDATE-Anweisung zeigt.

Hier ist ein **<event>** -Element, das während der Tests generiert wurde:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


Transact-SQL-Skript verwendet die folgenden Systemfunktion der Event_file lesen:

- [Sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


Eine Erläuterung der erweiterte Optionen für die Anzeige von Daten aus erweiterten Ereignissen ist verfügbar unter:

- [Erweiterte Anzeige von Zieldaten von Extended Events](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Das Codebeispiel auf SQL Server konvertieren


Angenommen Sie, Sie möchten im vorangehende Beispiel Transact-SQL für Microsoft SQL Server ausgeführt.


- Der Einfachheit halber sollten Sie vollständig Azure Behälter mit einer einfachen Datei wie **C:\myeventdata.xel**ersetzen. Die Datei wird auf der lokalen Festplatte des Computers geschrieben, auf dem SQL Server gehostet.


- Sie benötigen keine Art von Transact-SQL-Anweisungen für **HAUPTSCHLÜSSEL erstellen** und **Anmeldeinformationen erstellen**.


- Ersetzen Sie in der Anweisung **CREATE EVENT SESSION** in der Klausel **Ziel hinzufügen** zugewiesenen Wert Http zur **Filename =** mit vollständiger Pfad wie **C:\myfile.xel**.
 - Kein Azure Storage-Konto muss einbezogen werden.


## <a name="more-information"></a>Weitere Informationen


Weitere Informationen zu Konten und Container in Azure Storage-Diensts finden Sie unter:

- [Wie BLOB-Speicher von .NET](../storage/storage-dotnet-how-to-use-blobs.md)
- [Benennung und verweisen auf Container, Blobs und Metadaten](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Arbeiten mit dem Stammcontainer](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Lektion 1: Erstellen einer gespeicherten Zugriffsrichtlinie und ein SAS Azure Container](http://msdn.microsoft.com/library/dn466430.aspx)
    - [Lektion 2: Erstellen einer SAS mit SQL Server-Anmeldeinformationen](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

