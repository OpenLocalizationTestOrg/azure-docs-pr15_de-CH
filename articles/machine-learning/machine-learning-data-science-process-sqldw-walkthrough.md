<properties
    pageTitle="Team wissenschaftliche Daten in Aktion: mit SQL Data Warehouse | Microsoft Azure"
    description="Modernste Analytik Prozesse und Technologie in Aktion"  
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Team wissenschaftliche Daten in Aktion: mit SQL Data Warehouse

In diesem Lernprogramm gehen wir Sie durch das Erstellen und Bereitstellen von einem Computer mit SQL Data Warehouse (SQL DW) lernen für öffentlich zugängliche Dataset - [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) Dataset. Binäre klassifizierungsmodell erstellt prognostiziert ein Trinkgeld für eine Fahrt und für multiklassenklassifizierung und Regression werden erörtert, die die Verteilung für den Tipp Beträge vorherzusagen.

Die Prozedur folgt Workflow [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) . Zum Einrichten einer Umgebung Wissenschaft erfahren Modell features wie die Daten in SQL DW laden und wie Daten und Techniker verwenden SQL DW oder ein IPython Notebook. Dann zeigen wir zum Erstellen und Bereitstellen eines Modells mit einer Azure maschinelles lernen.


## <a name="dataset"></a>Das Dataset NYC Taxi Trips

NYC Taxi Reise Daten besteht aus ca. 20GB komprimierte CSV-Dateien (~ 48GB nicht komprimiert), Aufzeichnung 173 Millionen einzelnen Schleifen und die Preise für jede Reise bezahlt. Jeder Reise Datensatz enthält den Abholung und Ladengeschäft und Zeiten, anonymisierten Hack (Treiber) Lizenznummer sowie Anzahl Medaille (Taxi eindeutige Id). Daten ist umfasst alle Fahrten im Jahr 2013 und in die folgenden zwei Datasets für jeden Monat:

1. Die Datei **trip_data.csv** enthält REISEDETAILS wie Anzahl der Fahrgäste, Abholung und Drop-Off Punkt Fahrtdauer und Reisedauer. Hier sind einige Beispieldatensätze:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Die Datei **trip_fare.csv** enthält Details für jede Reise wie Zahlungstyp, Fahrpreis Betrag Zuschlag und steuern, Tipps und Mautgebühren gezahlten Flugpreis und den Gesamtbetrag. Hier sind einige Beispieldatensätze:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Der **eindeutige Schlüssel** Reise\_Daten und Reise\_Tarif besteht aus drei Bereichen:

- medaillon
- Hack\_Lizenz und
- Abholung\_Datetime.

## <a name="mltasks"></a>Drei Adresstypen Vorhersage Aufgaben

Wir formulieren drei Vorhersage Probleme anhand der *Tipp\_Betrag* drei Arten von Modellierungsaufgaben erläutern:

1. **Binäre Klassifizierung**: vorherzusagen, ob ein Tipp entrichtet für eine Fahrt also ein *Tipp\_Betrag* größer als 0 Beispiel ist zwar ein *Tipp\_Betrag* $0 ist eine negative.

2. **Multiklassenklassifizierung**: den Bereich Trinkgeld für die Reise vorhergesagt. Wir teilen die *Tipp\_Betrag* in fünf Lagerplätze oder Klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressionsaufgabe**: Menge Trinkgeld für eine Reise vorhergesagt.  


## <a name="setup"></a>Einrichten der Umgebung für die erweiterte Analyse Azure Wissenschaft

Gehen Sie folgendermaßen vor, um Ihre Azure Data Science-Umgebung einzurichten.

**Erstellen Sie Ihr eigenes Konto Azure BLOB-Speicher**

- Wenn Sie eigene Azure BLOB-Speicher bereitstellen, wählen Sie einen Standort für Ihren Azure BLOB-Speicher in oder möglichst auf **Südlichen zentralen USA**, ist wo NYC Taxi Daten gespeichert ist. Die Daten werden mithilfe von AzCopy aus dem Blob für den öffentlichen Speicher-Container zu einem Container in Ihrem Speicherkonto kopiert. Je näher der Azure BLOB-Speicher südlichen zentralen USA, schneller dieser Vorgang (Schritt 4) abgeschlossen.
- Befolgen Sie die Schritte am [von Azure-Speicherkonten](../storage/storage-create-storage-account.md), erstellen Sie Ihr eigenes Konto Azure-Speicher. Unbedingt Notizen auf der folgenden Speicher-Anmeldeinformationen später in dieser exemplarischen Vorgehensweise erforderlich.

  - **Speicher-Kontoname**
  - **Speicherkontoschlüssel**
  - **Containername** (die Daten in Azure BLOB-Speicher gespeichert werden sollen)

**Bereitstellen Sie Ihrer Azure SQL DW-Instanz.**
Führen Sie Dokumentation [erstellen eine SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) eine SQL Data Warehouse-Instanz bereitstellen. Stellen Sie sicher, dass stellen Notationen auf folgenden SQL Data Warehouse-Daten, die in nachfolgenden Schritten verwendet werden.

  - **Servername**: <server Name>. database.windows.net
  - **Name des SQLDW (Datenbank)**
  - **Benutzername**
  - **Kennwort**

**Installieren Sie Visual Studio 2015 und SQL Server-Tools.** Eine Anleitung finden Sie unter [Visual Studio 2015 installieren bzw. SSDT (SQL Server Data Tools) für SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Verbinden Sie mit Ihrem SQL Azure DW mit Visual Studio.** Eine Anleitung finden Sie Schritte 1 und 2 in [Verbindung mit Azure SQL Data Warehouse mit Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

>[AZURE.NOTE] Führen Sie die folgende SQL-Abfrage in der Datenbank im SQL Data Warehouse (statt in Schritt 3 des Themas verbinden bereitgestellte Abfrage) erstellten erstellt **einen Hauptschlüssel**.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Erstellen Sie einen Azure Machine Learning-Arbeitsbereich unter Azure-Abonnement.** Eine Anleitung finden Sie [eine Azure Machine Learning-Arbeitsbereich erstellen](machine-learning-create-workspace.md).

## <a name="getdata"></a>Laden Sie die Daten in SQL Data Warehouse

Öffnen Sie ein Windows PowerShell-Befehl Console. Führen Sie die folgende PowerShell Befehle zum Beispiel SQL downloaden Skriptdateien, die wir für Sie auf Github in ein lokales Verzeichnis freigeben, die mit dem Parameter angeben *- DestDir*. Ändern Sie den Wert des Parameters *-DestDir* lokalen Verzeichnis. *-DestDir* nicht vorhanden ist, wird es durch das PowerShell-Skript erstellt.

>[AZURE.NOTE] Sie müssen als **Administrator** beim folgenden PowerShell-Skript ausführen, sollte das Verzeichnis *DestDir* Administratorrechte erstellen oder schreiben.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Nach der erfolgreichen Ausführung Änderungen *- DestDir*aktuellen Arbeitsverzeichnis. Sie sollten möglicherweise angezeigt wie folgt:

![][19]

Führen Sie in Ihrem *DestDir -*das folgende PowerShell-Skript im Administratormodus:

    ./SQLDW_Data_Import.ps1

Wenn das PowerShell-Skript zum ersten Mal ausgeführt wird, müssen Sie die Informationen der Azure SQL-DW und Ihr Konto Azure BLOB-Speicher. Nach Abschluss dieses PowerShell-Skript wird zum ersten Mal die Anmeldeinformationen ausgeführt Sie Eingabe in einer Konfigurationsdatei SQLDW.conf im Verzeichnis vorhanden geschrieben wurden. Zukünftige Ausführen des PowerShell-Skriptdatei kann erforderlichen Parameter aus dieser Konfigurationsdatei lesen. Benötigen Sie einige Parameter ändern, können Sie Eingabeparameter auf dem Bildschirm auf Aufforderung durch diese Datei löschen und die Parameterwerte eingeben, wie oder die Parameterwerte durch Bearbeiten der Datei SQLDW.conf im Verzeichnis *- DestDir* ändern.

>[AZURE.NOTE] Vermeidung Schemakonflikte mit Namen, die in der Azure SQL-DW vorhanden, wenn Parameter direkt aus der Datei SQLDW.conf gelesen wird eine Zufallszahl 3-stellige Schemaname aus der Datei SQLDW.conf als Standardname für alle Schemas hinzugefügt. PowerShell-Skript möglicherweise eine Aufforderung für ein Schema: der Name kann Ermessen Benutzer angegeben werden.

Diese Skriptdatei **PowerShell** führt folgende Aufgaben aus:

- **Downloadet und installiert AzCopy**AzCopy nicht installiert ist

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Kopiert Daten in Ihrem privaten Blob Speicherkonto** aus öffentlichen Blob mit AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- **Lädt Daten mithilfe der Azure SQL-DW Polybase (durch Ausführen von LoadDataToSQLDW.sql)** von Ihrem privaten BLOB-Speicherkonto mit den folgenden Befehlen.

    - Erstellen Sie ein schema

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Erstellen Sie eine Datenbank begrenzt Anmeldeinformationen

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Erstellen einer externen Datenquelle für ein Blob Azure-Speicher

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Erstellen Sie ein externes Dateiformat für eine CSV-Datei. Daten nicht komprimiert und Felder mit den senkrechten Strich getrennt.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Erstellen Sie externe Fahrpreis und Reise Tabellen für NYC Taxi Dataset in Azure BLOB-Speicher.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Laden von Daten aus externen Tabellen in Azure BLOB-Speicher in SQL Data Warehouse

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Erstellen einer Beispiel-Datentabelle (NYCTaxi_Sample) und Einfügen von Daten, SQL-Abfragen für die Reise und Tarif Tabellen auswählen. (Einige Schritte in dieser exemplarischen Vorgehensweise muss die Beispieltabelle verwenden.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Der geografische Standort des Speicherkonten betrifft Ladezeiten.

>[AZURE.NOTE] Kopieren Daten aus öffentlichen Blob zu Ihrem Konto private Lagerhaltung dauert je nach Lage Ihres Kontos private BLOB-Speicher ca. 15 Minuten oder länger, und beim Laden von Daten aus Ihrem Speicherkonto Ihre Azure SQL DW 20 Minuten oder länger.  

Sie müssen was entscheiden, ob Sie Duplikate für Quelle und Ziel.

>[AZURE.NOTE] Wenn die CSV-Dateien aus kopiert werden das Speicherkonto private Blob Blob für den öffentlichen Speicher bereits in das Speicherkonto private Blob, AzCopy fragt Sie, ob Sie diese überschreiben möchten. Möchten Sie nicht überschrieben werden, geben Sie **n** , wenn Sie dazu aufgefordert werden. Wenn Sie **Alle** von Ihnen, Eingabe **eine** Aufforderung überschreiben möchten. Sie können auch input **y** CSV-Dateien einzeln zu überschreiben.

![#21 zeichnen][21]

Sie können Ihre eigenen Daten verwenden. Wenn die Daten in Ihrem lokalen Computer in einer realen Anwendung, noch können AzCopy Sie Ihre privaten Azure BLOB-Speicher auf lokale Daten hinzufügen. Müssen nur am **Quellspeicherort** ändern `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, AzCopy Befehl der PowerShell-Skriptdatei im lokalen Verzeichnis, das Ihre Daten enthält.


>[AZURE.TIP] Wenn die Daten bereits im privaten Azure BLOB-Speicher in der realen Anwendung, Sie können die AzCopy überspringen im PowerShell-Skript direkt einholen und die Daten Azure SQL dw. Dies erfordert eine zusätzliche Bearbeitung des Skripts an das Format der Daten anpassen.


Powershell-Skript wird auch Azure SQL DW-Informationen in Datendateien durchsuchen wird SQLDW_Explorations.sql, SQLDW_Explorations.ipynb und SQLDW_Explorations_Scripts.py sind diese drei Dateien sofort getestet werden nach Abschluss des PowerShell-Skripts.

Nach der erfolgreichen Ausführung wird angezeigt wie folgt:

![][20]

## <a name="dbexplore"></a>Durchsuchen von Daten und Featureentwicklung Azure SQL Data Warehouse

In diesem Abschnitt führen wir Durchsuchen von Daten und Funktion Generieren von SQL-Abfragen für Azure SQL DW direkt mit **Visual Studio Datentools**auszuführen. Alle in diesem Abschnitt verwendeten SQL-Abfragen finden im Beispielskript mit dem Namen *SQLDW_Explorations.sql*. Diese Datei wurde bereits von PowerShell-Skript in einem lokalen Verzeichnis heruntergeladen. Sie können auch von [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql)abrufen. Jedoch die Datei in Github keinen Azure SQL DW Informationen angeschlossen.

Verbindung mit Ihre Azure SQL DW mit Visual Studio SQL DW-Benutzername und Kennwort und bestätigen, dass die Datenbank sowie Tabellen importiert wurden im **SQL Objekt-Explorer** öffnen. Rufen Sie die *SQLDW_Explorations.sql* -Datei.

>[AZURE.NOTE] Um einen Parallel Data Warehouse (PDW) Abfrage-Editor zu öffnen, den Befehl **Neue Abfrage** während der PDW im **SQL Objekt-Explorer**ausgewählt ist. Der standardmäßige SQL-Abfrage-Editor wird von PDW nicht unterstützt.

Hier sind die Daten durchsuchen und Funktion Generation Aufgaben in diesem Abschnitt:

- Durchsuchen Sie Daten-Distributionen einige Felder in unterschiedlichen Zeitfenster.
- Überprüfen Sie Datenqualität Längen- und Felder.
- Binäre und multiclass Klassifizierung Etiketten auf der Grundlage der **Tipp\_Betrag**.
- Generieren von Funktionen und Compute/vergleichen Reise Abstände.
- Verknüpfen der beiden Tabellen und Extrahieren einer Stichprobe, die zum Erstellen von Modellen verwendet werden.

### <a name="data-import-verification"></a>Überprüfung importieren

Diese Abfragen ermöglichen eine schnelle Überprüfung der Anzahl der Zeilen und Spalten in den Tabellen gefüllt über parallele Bulk Polybase importieren

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Ausgabe:** 173,179,759 Zeilen und 14 Spalten erhalten.

### <a name="exploration-trip-distribution-by-medallion"></a>Durchsuchen: Reise Verteilung von medaillon

Diese Beispielabfrage gibt, Medaillen (Taxi Zahlen), die mehr als 100 Besuche innerhalb eines bestimmten Zeitraums abgeschlossen. Die Abfrage profitiert von der partitionierten Tabellenzugriff seit dem Partitionsschema der bedingt **Abholung\_Datetime**. Das vollständige Dataset Abfragen wird der partitionierten Tabelle verwenden bzw. index Scan machen.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Ausgabe:** Die Abfrage sollte eine Tabelle mit Zeilen angeben 13,369 Medaillen (Taxis) und die Anzahl der Reise abgeschlossen werden 2013 zurück. Die letzte Spalte enthält die Anzahl der Schleifen abgeschlossen.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Durchsuchen: Reise Verteilung von medaillon und hack_license

In diesem Beispiel gibt, Medaillen (Taxi Zahlen) und Hack_license Zahlen (Treiber) abgeschlossen mehr als 100 Besuche innerhalb eines bestimmten Zeitraums.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Ausgabe:** Die Abfrage sollte eine Tabelle mit 13,369 angegeben 13,369/Fahrers IDs, die dieser 100 Reisen 2013 abgeschlossen wurden zurück. Die letzte Spalte enthält die Anzahl der Schleifen abgeschlossen.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Datenbewertung: mit falschen Längen- oder Breitengrad überprüfen

In diesem Beispiel wird untersucht, wenn keines der Felder Länge oder Breite entweder einen ungültigen Wert enthalten (Radiant Grad entsprechen zwischen-90 und 90) oder (0, 0) Koordinaten.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Ausgabe:** Die Abfrage gibt 837,467 Reisen, die ungültige Länge oder Breite Felder.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Untersuchung: Gekippt und nicht Geneigter Trips Verteilung

Dieses Beispiel ermittelt die Anzahl der Reisen, die gegen die Anzahl gekippt wurden und nicht gekippt wurden, in einem bestimmten Zeitraum (oder im vollständigen Dataset Wenn das Jahr hier eingerichtet). Diese Verteilung entspricht die binäre Bezeichnung Verteilung später für binäre Klassifizierung Modellierung verwendet werden.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Ausgabe:** Die Abfrage sollte zurückgeben, gaben die folgenden Tipp Frequenzen im Jahr 2013: 90,447,622 und 82,264,709 nicht gekippt.

### <a name="exploration-tip-classrange-distribution"></a>Durchsuchen: Tipp Bereich Klasse Verteilung

In diesem Beispiel berechnet die Verteilung der Tipp Bereiche in einem bestimmten Zeitraum (oder im vollständigen Dataset, wenn das Jahr). Dies ist die Verteilung der Label-Klassen, die später zur Modellierung von multiklassenklassifizierung verwendet werden.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Ausgabe:**

|Datenlecks  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Untersuchung: Berechnen und Vergleichen von Wegstrecke

Dieses Beispiel konvertiert die Abholung und Ladengeschäft Länge und Breite zu SQL verweist, Fahrtstrecke SQL Geography Punkte Differenz berechnet und Stichprobe der Ergebnisse für den Vergleich zurückgegeben. Das Beispiel beschränkt die Ergebnisse gültig nur mit Qualität Bewertung Datenabfrage zuvor behandelten Koordinaten.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Feature Engineering mit SQL-Funktionen

SQL-Funktionen können manchmal eine effiziente Option für Featureentwicklung sein. In dieser exemplarischen Vorgehensweise definiert eine SQL-Funktion zum Berechnen des unmittelbaren Abstands zwischen der Abholung und Dropoff. Sie können die folgenden SQL-Skripts in **Visual Studio Daten**ausführen.

Hier wird das SQL-Skript, das die Distance-Funktion definiert.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Hier ist ein Beispiel für diesen Funktionsaufruf Funktionen in einer SQL-Abfrage generiert:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Ausgabe:** Diese Abfrage generiert eine Tabelle (mit 2,803,538) mit Abholung Dropoff Breiten und Längen und entsprechenden direkten Distanzen in Meilen. Hier sind die Ergebnisse für die ersten 3 Zeilen:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Vorbereiten von Daten für Gebäude

Die folgende Abfrage Joins der **Nyctaxi\_Reise** und **Nyctaxi\_Tarif** Tabellen, generiert eine binäre Klassifizierung Bezeichnung **gekippt**, Multi-Klasse Typenschild **Tipp\_Klasse**, und ein Beispiel aus dem vollständigen verknüpfte Dataset. Die Probenahme erfolgt durch eine Teilmenge anhand pickup Trips abrufen.  Diese Abfrage kann kopiert und eingefügt in [Azure Machine Learning Studio](https://studio.azureml.net) [Daten importieren] [ import-data] Modul für direkte Erfassung von SQL-Datenbankinstanz in Azure. Die Abfrage schließt mit falsch (0, 0) Koordinaten.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Wenn Sie Azure Machine Learning fortfahren möchten, können Sie:  

1. Speichern Sie die letzte SQL-Abfrage zum Extrahieren der Daten und kopieren die Abfrage direkt in [Daten importieren] [ import-data] Modul in Azure Computerlernen oder
2. Beibehalten der aufgenommenen und technischen Daten für Modell in einer neuen SQL-DW-Tabelle erstellen und verwenden die neue Tabelle [Daten importieren] möchten[ import-data] Modul in Azure maschinelles lernen. PowerShell-Skript zuvor hat dies für Sie erledigt. Sie können direkt aus dieser Tabelle im Modul "Daten importieren" lesen.


## <a name="ipnb"></a>Durchsuchen von Daten und Featureentwicklung IPython notebook

In diesem Abschnitt führen wir Durchsuchen von Daten und Funktion, bei der beide Python und zuvor erstellte SQL-Abfragen in SQL DW. Ein Beispiel IPython Notebook namens **SQLDW_Explorations.ipynb** und eine Python-Datei **SQLDW_Explorations_Scripts.py** in einem lokalen Verzeichnis heruntergeladen wurde. Sie stehen auch auf [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Diese beiden Dateien sind identisch-Skripten. Python-Skriptdatei wird Ihnen bereitgestellt, keinen IPython Notebook-Server verfügen. Diese zwei Python-Beispieldateien sind unter **Python 2.7**entwickelt.

Die benötigte Azure SQL DW Informationen im Beispiel IPython Notebook und Python-Skript-Datei auf Ihren lokalen Computer heruntergeladen wurde von PowerShell-Skript zuvor angeschlossen. Ohne Änderung ausführbar sind.

Wenn bereits ein AzureML Arbeitsbereich eingerichtet haben, direkt im Beispiel IPython Notebook AzureML IPython Notebook-Service hochladen und führen Sie es. Hier werden die Schritte zum Hochladen auf AzureML IPython Notebook-Service:

1. Arbeitsbereich AzureML melden Sie an, klicken Sie auf "Studio" und "NOTEBOOKS" auf der linken Seite der Webseite auf.

    ![#22 zeichnen][22]

2. Klicken Sie auf der Webseite Links unten auf "Neu", und wählen Sie "Python 2". Dann geben Sie einen Namen in das Notizbuch, und klicken Sie zum Erstellen neuer leerer IPython Notebook.

    ![#23 zeichnen][23]

3. Klicken Sie auf das Symbol "Jupyter" in der linken oberen Ecke des neuen IPython Notebooks.

    ![#24 zeichnen][24]

4. Ziehen Sie das Beispiel IPython Notebook **Struktur** auf AzureML IPython Notebook-Dienst, und klicken Sie auf **Hochladen**. Anschließend werden im Beispiel IPython Notebook AzureML IPython Notebook-Service hochgeladen.

    ![#25 zeichnen][25]

Um das Beispiel auszuführen Skriptdatei IPython Notebook oder Python, die folgenden Pakete benötigt werden Python. Bei Verwendung den AzureML IPython Notebook-Service haben diese Pakete vorinstalliert.

    - Panda
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Die empfohlene Reihenfolge beim Erstellen von analytischer Lösungen auf AzureML mit großen lautet wie folgt:

- Lesen der Daten in einem kleinen in einen Rahmen mit Daten im Speicher.
- Führen Sie einige Visualisierung und aufgenommenen Daten kennen.
- Experimentieren Sie mit Feature mithilfe der gesammelten Daten.
- Verwenden Sie Durchsuchen von mehr Daten, Datenmanipulation Featureentwicklung und Python, SQL-Abfragen direkt gegen SQL DW.
- Entscheiden Sie Stichprobengröße für Gebäude Azure Machine Learning.

Im folgenden sind einige Durchsuchen von Daten Visualisierung und Funktion engineering-Beispiele. Weitere Daten kennen finden im Beispiel IPython Notebook und Python-Beispielskriptdatei.

### <a name="initialize-database-credentials"></a>Initialisieren von Anmeldeinformationen

Initialisieren Sie die Datenbankverbindungseinstellungen in die folgenden Variablen:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Verbindung erstellen

Hier ist die Verbindungszeichenfolge, die Verbindung zur Datenbank herstellt.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Bericht Anzahl von Zeilen und Spalten in der Tabelle < Nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Gesamtanzahl der Zeilen = 173179759  
- Gesamtanzahl der Spalten = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Bericht Anzahl von Zeilen und Spalten in der Tabelle < Nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Gesamtanzahl der Zeilen = 173179759  
- Gesamtanzahl der Spalten = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Lesen Sie im kleinen Beispiel SQL Data Warehouse-Datenbank

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Zeit zum Lesen die Beispieltabelle 14.096495 Sekunden.  
Die Anzahl der Zeilen und Spalten abgerufen = (1000, 21).

### <a name="descriptive-statistics"></a>Beschreibende Statistik

Jetzt können Sie die aufgenommenen Daten. Wir beginnen mit einigen Deskriptive Statistik für die **Reise\_Abstand** (oder andere Felder angegeben).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>-Visualisierung: Beispiel für plot

Als Nächstes betrachten wir Boxplot Reise Abstand die Quantile darstellen.

    df1.boxplot(column='trip_distance',return_type='dict')

![Zeichnen Sie #1][1]

### <a name="visualization-distribution-plot-example"></a>Visualisierung: Verteilung Plot-Beispiel

Grundstücke, die Verteilung und ein Histogramm für die aufgenommenen Reise Abstände darstellen.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![#2 zeichnen][2]

### <a name="visualization-bar-and-line-plots"></a>Visualisierung: Leiste und Linie gezeichnet

In diesem Beispiel werden bin Reise Abstand in fünf Lagerplätze und Visualisieren binning Ergebnisse.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Wir zeichnen oben Bin Verteilung in Bar oder mit Zeile:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 zeichnen][3]

und

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 zeichnen][4]

### <a name="visualization-scatterplot-examples"></a>Visualisierung: Streudiagramm Beispiele

Wir zeigen Punktdiagramm zwischen **Reise\_Zeit\_in\_Sekunden** und **Reise\_Abstand** zu sehen, ob eine Korrelation

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 zeichnen][6]

Entsprechend finden wir die Beziehung zwischen **Rate\_Code** und **Reise\_Abstand**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 zeichnen][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Durchsuchen von Daten im aufgenommenen Daten mithilfe von SQL-Abfragen in IPython notebook

In diesem Abschnitt untersuchen wir Daten Distributionen mit aufgenommenen Daten in die neue Tabelle weiter oben erstellten beibehalten werden. Beachten Sie, dass ähnliche kennen die ursprünglichen Tabellen durchgeführt werden können.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Durchsuchen: Nummer der Zeilen und Spalten in der Tabelle aufgenommenen

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Untersuchung: Geneigter nicht ausgelöst Verteilung

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Durchsuchen: Tipp Klasse Verteilung

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Untersuchung: Zeichnen der Spitze Verteilung von Klasse

    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 zeichnen][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Durchsuchen: Tägliche Verteilung Reisen

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Durchsuchen: Reise Verteilung pro Medaille

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Durchsuchen: Reise Verteilung Medaille und Hack-Lizenz

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Durchsuchen: Reise zeitverteilung

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Durchsuchen: Reise Abstand Verteilung

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Untersuchung: Zahlung Verteilung

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Überprüfen Sie die endgültige Form der Featurized-Tabelle

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Modelle in Azure Machine Learning

Wir sind jetzt Model und Modell Bereitstellung in [Azure Machine Learning](https://studio.azureml.net)fortfahren. Die Daten sind in Vorhersage Probleme identifiziert, nämlich verwendet werden:

1. **Binäre Klassifizierung**: vorherzusagen, ob ein Tipp bezahlt wurde eine.

2. **Multiklassenklassifizierung**: Bereich Trinkgeld bereits definierte Klassen vorherzusagen.

3. **Regressionsaufgabe**: Menge Trinkgeld für eine Reise vorhergesagt.  



Um die modellierungsübung beginnen, melden Sie sich den **Azure Machine Learning** -Arbeitsbereich. Wenn einen Computer lernen Arbeitsbereich noch nicht erstellt haben, finden Sie unter [Erstellen eines Workspace Azure ML](machine-learning-create-workspace.md).

1. Zunächst mit Azure Computer finden Sie unter [Neuigkeiten Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

2. Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net).

3. Studio-Startseite bietet eine Fülle von Informationen, Videos, Lernprogramme, Links Verweis Module und andere Ressourcen. Weitere Informationen zu Azure Machine Learning finden Sie in [Azure Arbeitsplatzes Learning Dokumentation](https://azure.microsoft.com/documentation/services/machine-learning/).

Normale trainingsexperiment besteht aus folgenden Schritten:

1. Erstellen eines Experiments **+ neu** .
2. Daten in Azure ML abrufen.
3. Vor der Verarbeitung, Transformieren und die Daten nach Bedarf ändern.
4. Generieren von Funktionen nach Bedarf.
5. Unterteilen Sie die Daten in Datasets Training, Überprüfung, testen (oder separate Datensätze für jeden).
6. Wählen Sie eine oder mehrere Algorithmen für maschinelles lernen je nach Learning zu. Z. B. binäre Klassifizierung, multiklassenklassifizierung, Regression.
7. Ein oder mehrere Modelle Trainings-Dataset zu trainieren.
8. Faktor Validierung Dataset mit qualifizierten Modelle.
9. Bewerten Sie Modelle relevanten Kennzahlen für das Lernen Problem berechnen.
10. Feinabstimmung der Modelle und wählen das geeignetste Modell bereitstellen.

In dieser Übung bereits untersucht und Daten im SQL Data Warehouse Engineering, und beschlossen, die Größe der Stichprobe in Azure ML aufnehmen. So erstellen Sie eine oder mehrere der Vorhersagemodelle lautet

1. Daten in Azure ML mit den [Importierten Daten] abrufen[ import-data] Modul im Abschnitt **Eingabe und Ausgabe** . Weitere Informationen finden Sie unter [Daten importieren] [ import-data] Modul-Referenzseite.

    ![Azure ML Importdaten][17]

2. **Azure SQL-Datenbank** als **Datenquelle** im Bedienfeld " **Eigenschaften** " wählen.

3. Geben Sie im Feld **Servername** der DNS-Datenbankname. Format:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Geben Sie den **Datenbanknamen** in das entsprechende Feld ein.

5. Geben Sie den *Benutzernamen für SQL* **Server Benutzerkontonamen**und das *Kennwort* das **Benutzerkennwort Server**.

6. Aktivieren Sie die Option **Alle Server Zertifikate übernehmen** .

7. Im Bearbeitungsbereich Text **Datenbankabfrage** Proben einfügen Abfrage extrahiert die erforderlichen Datenbankfelder (einschließlich berechnete Felder wie Etiketten) und unten die Daten der gewünschten Stichprobengröße.

Ein Beispiel eines binären Klassifizierung Experiments lesen Daten direkt aus SQL Data Warehouse-Datenbank ist in der Abbildung unten (Bitte ersetzen Tabelle Namen Nyctaxi_trip und Nyctaxi_fare durch den Schemanamen und Tabellennamen in der exemplarischen Vorgehensweise verwendet). Ähnliche Experimente können für multiklassenklassifizierung und Regressionsprobleme erstellt werden.

![Azure ML Zug][10]

> [AZURE.IMPORTANT] Die Modelldaten gemäß Extraktion und Beispiele für die Probenahme in vorherigen Abschnitten **Alle Etiketten für die drei modellierungsübungen in der Abfrage enthalten sind**. Ein (erforderlich) wichtiger aller modellierungsübungen ist **auszuschließen** unnötigen Etiketten für die beiden Probleme sowie alle anderen **Ziel verliert**. Beispielsweise bei binären Klassifizierung **Geneigter** Bezeichnung verwenden und Felder ausschließen **Tipp\_Klasse**, **Tipp\_Betrag**, und **insgesamt\_Betrag**. Sind Ziel Verluste seit sie die Spitze implizieren bezahlt.
>
> Um nicht benötigten Spalten ausschließen oder Speicherverluste abzielen, können Sie [Spalten im Dataset auswählen] [ select-columns] Modul oder [Metadaten bearbeiten][edit-metadata]. Weitere Informationen finden Sie unter [Spalten im Dataset auswählen] [ select-columns] und [Metadaten bearbeiten] [ edit-metadata] Seiten verweisen.

## <a name="mldeploy"></a>Bereitstellen von Modellen in Azure maschinelles lernen

Wenn das Modell fertig ist, können Sie problemlos als Webdienst direkt aus dem Experiment bereitstellen. Weitere Informationen zum Bereitstellen von Azure ML Webdienste finden Sie unter [Bereitstellen einer Azure Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).

Um einen neuen Webdienst bereitstellen, müssen Sie:

1. Erstellen Sie ein bewertungsexperiment.
2. Bereitstellen Sie den Web Service.

Klicken Sie ein bewertungsexperiment von einem Experiment **beendet** Training erstellen **Erstellen EXPERIMENTIEREN bewerten** in der unteren Leiste.

![Azure bewerten][18]

Azure Machine Learning versucht, ein bewertungsexperiment basierend auf den Komponenten des trainingsexperiments erstellen. Insbesondere ist:

1. Geschulte Modell speichern und Modell Schulungsmodulen entfernen.
2. Identifizieren Sie einen logischen **Eingabeanschluss** Schema erwarteten Daten darstellen.
3. Ermitteln Sie logische **Ausgabeanschluss** erwarteten Web Service-Ausgabeschema darstellen.

Erstellung Punktzahl Experiment überprüfen Sie und stellen Sie Bedarf anpassen. Standard Einstellung ist mit der Bezeichnung Felder schließt Eingabedatasets bzw. Abfrage ersetzen, wie diese nicht verfügbar sind, wenn der Dienst aufgerufen wird. Es empfiehlt sich auch die Größe des Eingabedatasets bzw. Abfrage einige Datensätze für input Schema angeben. Für den Ausgang wird alle Eingabefelder und ausschließen nur **Etiketten bewertet** und **Erzielte Wahrscheinlichkeiten** in der Ausgabe der [Spalten im Dataset auswählen] [ select-columns] Modul.

In der folgenden Abbildung wird ein Beispiel bewerten Experiment bereitgestellt. Bei Bereitstellung klicken Sie **Webdienst veröffentlichen** in der unteren Leiste.

![Azure ML veröffentlichen][11]


## <a name="summary"></a>Zusammenfassung
Zusammengefasst, was wir in dieser exemplarischen Vorgehensweise Lernprogramm, haben Sie Azure Data Science Umgebung arbeitete mit einem großen öffentlichen Dataset erstellt die durch Daten Wissenschaft Teamprozesses von Datenerfassung Training modellieren und die Bereitstellung eines Webdienstes Azure maschinelles lernen.

### <a name="license-information"></a>Lizenzinformationen

Diese exemplarische Vorgehensweise Beispiel und die zugehörigen Skripts und IPython schreibhefte() werden von Microsoft MIT Lizenz freigegeben. Bitte checken Sie die Datei LICENSE.txt im Verzeichnis des Beispielcodes auf GitHub Weitere Informationen.

## <a name="references"></a>Referenzen

• [Andrés Monroy NYC Taxi Trips-Downloadseite](http://www.andresmh.com/nyctaxitrips/)  
• [Vereiteln NYC Taxi Reisedaten von Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi Limousine Kommission Forschung und Statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
