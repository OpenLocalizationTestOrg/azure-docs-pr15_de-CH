<properties 
    pageTitle="Verschieben von Daten auf SQL Server auf einem virtuellen Computer Azure | Azure" 
    description="Bewegen Sie Daten aus Flatfiles oder aus einer lokalen SQL Server, SQL Server auf Azure VM" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Verschieben der Daten auf SQL Server auf einem virtuellen Computer Azure

In diesem Thema werden die Optionen zum Verschieben von Daten aus Flatfiles (CSV- oder TSV-Format) oder aus einer lokalen SQL Server auf SQL Server auf einem virtuellen Computer Azure. Diese Vorgänge zum Verschieben von Daten in der Cloud gehören Daten Wissenschaft Team.

Ein Thema, das Optionen zum Verschieben von Daten mit einer Azure SQL-Datenbank für maschinelles lernen werden, finden Sie unter [Verschieben einer Azure SQL-Datenbank für Azure maschinelles lernen](machine-learning-data-science-move-sql-azure.md).

Klicken Sie im **Menü** unten Links zu Themen, die beschreiben, wie Sie Daten in anderen zielumgebungen aufnehmen, wo die Daten gespeichert und verarbeitet während Team Daten Wissenschaft Prozess (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


In der folgenden Tabelle werden die Optionen zum Verschieben von Daten in SQL Server auf einem virtuellen Computer Azure zusammengefasst.

<b>QUELLE</b> |<b>Ziel: SQL Server auf Azure VM</b> |
------------------ |-------------------- |
<b>Flatfile</b> |1. <a href="#insert-tables-bcp">Befehlszeilen Massenkopierprogramm (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">bulk Insert SQL-Abfrage</a><br> 3. <a href="#sql-builtin-utilities">grafisch Dienstprogramme in SQL Server</a>
<b>Lokale SQL Server</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Bereitstellen einer SQL Server-Datenbank Microsoft Azure VM-Assistent</a><br> 2. <a href="#export-flat-file">Flatfile exportieren</a><br> 3. <a href="#sql-migration">SQL Datenbank-Migrationsassistenten</a> <br> 4. <a href="#sql-backup">Datenbank zurück und Wiederherstellen</a><br>

Beachten Sie, dass dieses Dokument setzt voraus, dass SQL-Befehlen von SQL Server Management Studio oder Visual Studio Database Explorer ausgeführt werden.

> [AZURE.TIP] Als Alternative können Sie [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) zum Erstellen und planen eine Pipeline, die Daten in einer SQL Server-VM auf Azure verschoben werden. Weitere Informationen finden Sie unter [Daten mit Azure Data Factory (Copy-Aktivität)](../data-factory/data-factory-data-movement-activities.md).


## <a name="prereqs"></a>Erforderliche Komponenten
In diesem Lernprogramm vorausgesetzt:

* Ein **Azure-Abonnement**. Wenn Sie nicht über ein Abonnement verfügen, können Sie eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)anmelden.
* Ein **Azure-Speicherkonto**. Ein Azure Storage-Konto verwendet zum Speichern der Daten in diesem Lernprogramm. Wenn ein Azure Storage-Konto haben, finden Sie im Artikel [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) . Nachdem das Speicherkonto erstellt haben, müssen Sie Zugriff auf den Speicher verwendet Konto abrufen. [Anzeigen, kopieren und Zugriffstasten Regenerieren Speicher](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)anzeigen
* **SQL Server auf Azure VM**bereitgestellt. Informationen finden Sie unter [Einrichten einer Azure SQL Server virtuellen Computer als Server für erweiterte Analyse IPython Notebook](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Installierte und konfigurierte **Azure PowerShell** lokal. Eine Anleitung hierzu finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Verschieben von Daten aus einer Flatfile in SQL Server auf Azure-VM

Wenn die Daten in einem Flatfileformat (in einem Zeilen-/Spaltenformat angeordnet), können sie SQL Server-VM auf Azure über die folgenden Methoden verschoben werden:

1. [Befehlszeile Massenkopierprogramm (BCP)](#insert-tables-bcp) 
2. [Bulk Insert SQL-Abfrage](#insert-tables-bulkquery)
3. [Grafisch Dienstprogramme in SQL Server (Import/Export, SSIS)](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Befehlszeile Massenkopierprogramm (BCP)

BCP ist ein Befehlszeilendienstprogramm mit SQL Server installiert und ist eines der schnellsten zum Verschieben von Daten. Es eignet sich für alle drei SQL Server-Varianten (lokale SQL Server, SQL Azure und SQL Server-VM auf Azure). 

> [AZURE.NOTE]**Daten sein sollte, für BCP?**  
> Es ist, zwar nicht erforderlich ermöglicht Dateien befindet sich auf dem gleichen Computer wie der Zielserver SQL Daten schneller übertragen (Netzwerk Geschwindigkeit Vs lokalen Datenträger e/a-Geschwindigkeit). Flatfiles mit Daten auf den Computer verschieben SQL Server installiert ist, mit verschiedenen Tools wie [AZCopy](../storage/storage-use-azcopy.md)kopieren Dateien [Azure Storage Explorer](http://storageexplorer.com/) oder Windows kopieren über Remote Desktop Protocol (RDP).

1. Stellen Sie sicher, dass die Datenbank und die Tabellen in der SQL Server-Datenbank erstellt werden. Hier ist ein Beispiel für die Verwendung der `Create Database` und `Create Table` Befehle:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. Generieren der Formatdatei, die das Schema für die Tabelle die folgenden Befehl in der Befehlszeile des Computers beschreibt, Bcp installiert ist.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Einfügen der Daten in der Datenbank mit Bcp-Befehl wie folgt. Dies sollte in der Befehlszeile arbeiten, vorausgesetzt, dass SQL Server auf demselben Computer installiert ist:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Optimieren von BCP fügt** Finden Sie im folgenden Artikel ["Richtlinien für Massenimport optimieren"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) , so fügt optimieren.


### <a name="insert-tables-bulkquery-parallel"></a>Fügt für schnelleren Datentransfer Parallelisierung

Wenn die zu verschiebenden Daten groß ist, können Sie beschleunigen möchten durch mehrere BCP-Befehle gleichzeitig parallel in einem PowerShell-Skript ausführen.

> [AZURE.NOTE]**Big Data Einnahme** Optimierung für große und sehr große Datasets laden Daten Partitionieren der logischen und physischen Tabellen mit mehreren Dateigruppen und Partition Tabellen. Weitere Informationen zum Erstellen und Laden von Daten in Partitionstabellen finden Sie unter [Paralleles Laden SQL-Partitionstabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Powershellskript unten zeigen parallel mit Bcp eingefügt:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Bulk Insert SQL-Abfrage

[Legen Sie SQL Massenabfrage](https://msdn.microsoft.com/library/ms188365) verwendet werden, um Daten in die Datenbank von Zeile/Spalte basiert Dateien importieren (die unterstützten Datentypen finden Sie unter[Vorbereiten von Bulk exportieren oder importieren (SQL Server)](https://msdn.microsoft.com/library/ms188609)) Thema. 

Hier sind einige Beispiele für Befehle für Bulk Insert sind wie folgt:  

1. Analysieren Sie Daten und stellen Sie benutzerdefinierten Optionen vor um sicherzustellen, dass die SQL Server-Datenbank das gleiche Format für alle besonderen Bereichen Daten übernimmt importieren. Hier ist ein Beispiel wie das Datumsformat als Jahr-Monat-Tag (wenn die Daten das Datum im Format Jahr-Monat-Tag enthält):

        SET DATEFORMAT ymd; 
    
2. Importieren von Daten mit Bulk Import-Anweisung:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>In SQL Server-Dienstprogramme

SQL Server Integration Services (SSIS) können Sie Daten in SQL Server-VM auf Azure aus einer Textdatei importieren. SSIS ist in zwei Studio-Umgebung verfügbar. Weitere Informationen finden Sie unter [Integration Services (SSIS) und Studio](https://technet.microsoft.com/library/ms140028.aspx):

- Informationen zu SQL Server Data Tools finden Sie in [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Informationen zum Import/Export-Assistenten finden Sie unter [SQL Server-Import / Export-Assistenten](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Verschieben von Daten von lokalen SQL Server, SQL Server auf Azure VM

Sie können auch die folgenden Migrationsstrategien verwenden:

1. [Bereitstellen einer SQL Server-Datenbank eine Microsoft Azure VM-Assistenten](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Exportieren in Flatfile](#export-flat-file) 
3. [SQL Datenbank-Migrationsassistenten](#sql-migration)
4. [Datenbank zurück und Wiederherstellen](#sql-backup)

Wir werden diese unten:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Bereitstellen einer SQL Server-Datenbank eine Microsoft Azure VM-Assistenten

**Bereitstellen einer SQL Server-Datenbank Microsoft Azure VM-Assistent** ist eine einfache und empfohlene Möglichkeit, Daten aus einer lokalen SQL Server-Instanz auf SQL Server auf Azure-VM verschieben. Ausführliche Schritte sowie eine Beschreibung der alternativen finden Sie unter [Migrieren einer Datenbank in SQL Server auf eine Azure-VM](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Exportieren in Flatfile

Exportieren von Daten aus einer auf lokalen SQL Server-Massenkopieren im Thema [Bulk Import und Export von Daten (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) dokumentiert können verschiedene Methoden verwendet werden. Dieses Dokument behandelt (Bulk Copy Program, BCP) als Beispiel. Sobald Daten in eine Flatfile exportiert werden, kann es auf einen anderen SQL Server Massenimport importiert werden. 

1. Exportieren Sie die Daten aus lokalen SQL Server in eine Datei wie folgt mit dem Dienstprogramm bcp

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. Erstellen Sie die Datenbank und die Tabelle auf SQL Server-VM auf Azure mit den `create database` und `create table` für das Tabellenschema in Schritt 1 exportiert.

3. Erstellen einer Formatdatei für beschreibt das Schema der Daten exportiert bzw. importiert werden. Details der Formatdatei werden im [Erstellen einer Formatdatei (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx)beschrieben.

    Format der Datei beim BCP von SQL Server-Computer ausführen 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Format der Datei bei Remoteausführung BCP für SQL Server 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Verwenden der Methoden im Abschnitt [Daten aus Datei verschieben](#filesource_to_sqlonazurevm) um Daten in Flatfiles in SQL Server zu verschieben.

### <a name="sql-migration"></a>SQL Datenbank-Migrationsassistenten

[SQL Server Datenbank-Migrationsassistent](http://sqlazuremw.codeplex.com/) bietet benutzerfreundliche zum Verschieben von Daten zwischen zwei SQL Server-Instanzen. Es ermöglicht dem Benutzer, Zuordnung Datenschemas zwischen Quellen und Zieltabellen Spaltentypen und verschiedene andere Funktionen auswählen. Massenkopieren (BCP) im Hintergrund verwendet. Nachfolgend finden Sie ein Screenshot des Begrüßungsbildschirms für SQL-Datenbank-Migrationsassistenten.  

![SQL Server-Migrationsassistenten][2]

### <a name="sql-backup"></a>Datenbank zurück und Wiederherstellen

SQL Server unterstützt: 

1. [Datenbank Back up und Funktionalität wiederherstellen](https://msdn.microsoft.com/library/ms187048.aspx) (auf einer lokalen Datei oder Bacpac exportieren Blob) und [Data-Tier Applications](https://msdn.microsoft.com/library/ee210546.aspx) (Bacpac). 
2. Möglichkeit, SQL Server VMs auf Azure direkt mit einem kopierten Datenbank oder eine Kopie einer vorhandenen SQL Azure-Datenbank erstellen. Weitere Informationen finden Sie unter [Verwenden der Datenbank kopieren](https://msdn.microsoft.com/library/ms188664.aspx). 

Einen Screenshot des Datenbank-Back/Wiederherstellung von SQL Server Management Studio Optionen sind unten aufgeführt.

![SQL Server-Importtool][1]

## <a name="resources"></a>Ressourcen

[Migrieren einer Datenbank in SQL Server in Azure VM](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server auf Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
