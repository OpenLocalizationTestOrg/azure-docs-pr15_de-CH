<properties 
    pageTitle="Analysieren der Verzögerung Daten mit Struktur auf Linux-basierten HDInsight | Microsoft Azure" 
    description="Erfahren Sie mit Struktur Flugdaten auf Linux-basierten HDInsight analysieren und Exportieren der Daten in SQL-Datenbank mit Sqoop." 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analysieren Sie Verzögerung Daten mit Struktur in HDInsight

Erfahren Sie Flight Verzögerung Daten Struktur auf Linux-basierten HDInsight analysieren und Exportieren der Daten in Azure SQL-Datenbank mit Sqoop.

> [AZURE.NOTE] Zwar einzelnen Teile dieses Dokuments können viele Schritte sind spezifisch für Linux-Cluster mit Windows-basierten HDInsight (Python und Struktur beispielsweise). Die mit einem Windows-basierten Cluster finden Sie unter [Analysieren Verzögerung Flugdaten Struktur in HDInsight verwenden](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Ein HDInsight-Cluster__. Schritte zum Erstellen eines neuen Linux-basierten HDInsight Clusters finden Sie unter [Erste Schritte mit Hadoop mit Struktur in HDInsight unter Linux](hdinsight-hadoop-linux-tutorial-get-started.md) .

- __Azure SQL-Datenbank__. Sie verwendet eine SQL Azure-Datenbank als Zieldatenspeicher. Wenn Sie nicht bereits eine SQL-Datenbank haben, finden Sie unter [SQL Lernprogramm: erstellen eine SQL-Datenbank in Minuten](../sql-database/sql-database-get-started.md).

- __Azure CLI__. Wenn der Azure-CLI nicht installiert haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) Schritte.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>Flight-Daten herunterladen

1. Wechseln Sie zu [Forschung und Technologie Administration Bureau of Transportation Statistics][rita-website].
2. Wählen Sie auf der Seite die folgenden Werte ein:

  	| Name | Wert |
  	| ---- | ---- |
  	| Jahr filtern | 2013 |
  	| Zeitraum filtern | Januar |
  	| Felder | Jahr, FlightDate, UniqueCarrier, Netzbetreiber, FlightNum, OriginAirportID, Ursprung, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Deaktivieren Sie alle anderen Felder |

3. Klicken Sie auf **Download**. 

##<a name="upload-the-data"></a>Die Daten

1. Verwenden Sie den folgenden Befehl HDInsight Cluster-Head-Knoten die Zip-Datei hochladen:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    __Dateiname__ mit dem Namen der Zip-Datei zu ersetzen. SSH-Login für den HDInsight-Cluster ersetzen Sie __Benutzernamen__ . Der Name des Clusters HDInsight ersetzen Sie CLUSTERNAME.
    
    > [AZURE.NOTE] Wenn Sie ein Kennwort verwenden, die SSH-Login authentifizieren, werden Sie aufgefordert, das Kennwort anzugeben. Bei Verwendung ein öffentlichen Schlüssels müssen mit der `-i` Parameter, und geben Sie den Pfad zu den passenden privaten Schlüssel. Beispielsweise `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Nach Abschluss des Uploads mit SSH verbinden:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Weitere Informationen über SSH mit Linux-basierten HDInsight finden Sie in folgenden Artikeln:
    
    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Nachdem die Verbindung hergestellt ist, anhand der folgenden ZIP-Datei entpacken:

        unzip FILENAME.zip
    
    Dadurch wird eine CSV-Datei extrahiert, die ungefähr 60 MB groß ist.
    
4. Verwenden Sie die folgenden WASB (verteilte Datenspeicher von HDInsight verwendet) ein neues Verzeichnis erstellen und kopieren Sie die Datei:

    bietet Dfs - Mkdir -p /tutorials/flightdelays/data bietet Dfs-setzen FILENAME.csv/Lernprogramme Flightdelays/Daten /
    
##<a name="create-and-run-the-hiveql"></a>Erstellen und Ausführen der HiveQL

Gehen Sie Struktur Tabelle __Verzögerung__Daten aus der CSV-Datei importieren.

1. Anhand der folgenden erstellen und bearbeiten eine neue Datei namens __flightdelays.hql__:

        nano flightdelays.hql
        
    Verwenden Sie die folgenden Inhalt dieser Datei:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Verwenden Sie __STRG + X__und __Y__ , um die Datei zu speichern.

3. Verwenden Sie folgende Struktur starten und führen Sie die Datei __flightdelays.hql__ :

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] In diesem Beispiel `localhost` wird verwendet, da Sie auf dem Head-Knoten des Clusters HDInsight verbunden ist dem HiveServer2 ausgeführt wird.

4. Verwenden Sie folgenden Befehl eine interaktive Sitzung Abstecher zu öffnen:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Beim Empfang der `jdbc:hive2://localhost:10001/>` aufgefordert, folgende Daten aus importierten Flugdaten Verzögerung ab.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    Dies wird eine Liste der Städte, die Wetter, Zeit Durchschnittliche Verzögerung Verzögerungen abrufen und speichern `/tutorials/flightdelays/output`. Später Sqoop Daten von diesem Speicherort lesen und Azure SQL-Datenbank exportieren.

6. Geben Sie zum Beenden der Luftlinie `!quit` aufgefordert.

## <a name="create-a-sql-database"></a>Erstellen Sie eine SQL-Datenbank

Wenn bereits eine SQL-Datenbank müssen Sie den Servernamen abrufen. Finden Sie diese in [Azure-Portal](https://portal.azure.com) von __SQL-Datenbanken__auswählen und Filtern Sie Sie anschließend auf den Namen der Datenbank, die Sie verwenden möchten. Der Servername wird in der Spalte __SERVER__ aufgelistet.

Wenn Sie nicht bereits eine SQL-Datenbank haben, verwenden Sie die Informationen in [SQL Lernprogramm: erstellen eine SQL-Datenbank in Minuten](../sql-database/sql-database-get-started.md) erstellen. Sie müssen den Servernamen für die Datenbank verwendete speichern.

##<a name="create-a-sql-database-table"></a>Erstellen einer SQL-Datenbanktabelle

> [AZURE.NOTE] Es gibt vielfältige Verbindung zu SQL-Datenbank erstellen. Die folgenden Schritte verwenden [FreeTDS](http://www.freetds.org/) aus dem HDInsight-Cluster.

1. Verwenden Sie SSH Verbindung mit Linux-basierte HDInsight-Cluster, und führen Sie die folgenden Schritte über SSH-Sitzung.

3. Verwenden Sie den folgenden Befehl FreeTDS installiert:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Installierte FreeTDS verwenden Sie folgenden Befehl für die Verbindung mit dem SQL-Datenbankserver. Der SQL Server-Datenbankname ersetzen Sie __ServerName__ . Ersetzen Sie __AdminLogin__ und __AdminPassword__ mit dem Benutzernamen für die SQL-Datenbank. Der Datenbankname ersetzen Sie __DatabaseName__ .

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Sie erhalten eine Ausgabe ähnlich der folgenden:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Bei der `1>` dazu aufgefordert werden, geben Sie die folgenden Zeilen:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Wenn die `GO` Anweisung eingegeben wird, wird die vorherige Anweisung ausgewertet. Dadurch entsteht eine neue Tabelle __Verzögerung__mit einem gruppierten Index (SQL-Datenbank erforderlich.)

    Stellen Sie sicher, dass die Tabelle erstellt wurde anhand der folgenden:

        SELECT * FROM information_schema.tables
        GO

    Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Geben Sie `exit` an der `1>` Fragen Tsql-Dienstprogramm zu beenden.
    
##<a name="export-data-with-sqoop"></a>Exportieren von Daten mit Sqoop

2. Verwenden Sie den folgenden Befehl, um sicherzustellen, dass Sqoop die SQL-Datenbank sehen kann:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Dies sollte eine Liste der Datenbanken, einschließlich der Tabelle Verzögerung in zuvor erstellte Datenbank zurückgeben.

3. Befehl die folgenden Daten aus Hivesampletable in der Mobiledata Tabelle exportieren:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    Dies weist Sqoop SQL-Datenbank auf die Datenbank, die die Tabelle Verzögerung und Exportieren von Daten aus der Wasbs: / / / Tutorials/Flightdelays/Output (wir Speicherort die Ausgabe der Abfrage Struktur,) Tabelle verzögert.

4. Nach Abschluss des Befehls verwenden Sie folgende Verbindung zu der Datenbank TSQL:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Sobald verbunden, verwenden Sie die folgenden Aussagen um zu überprüfen, ob die Daten der Mobiledata-Tabelle wurde:
    
        SELECT * FROM delays
        GO

    Sie sollten eine Liste der Daten in der Tabelle angezeigt. Typ `exit` Tsql-Dienstprogramm beenden.

##<a id="nextsteps"></a>Nächste Schritte

Sie verstehen jetzt uploaden eine Datei auf Azure BLOB-Speicher, wie eine Tabelle Struktur aufgefüllt mit den Daten aus dem Azure BLOB-Speicher Hive-Abfragen ausführen und wie mit Sqoop um Daten aus bietet mit einer Azure SQL-Datenbank zu exportieren. Weitere finden Sie in folgenden Artikeln:

* [Erste Schritte mit HDInsight][hdinsight-get-started]
* [Struktur mit HDInsight verwenden][hdinsight-use-hive]
* [Verwenden Sie Oozie mit HDInsight][hdinsight-use-oozie]
* [Verwenden Sie Sqoop mit HDInsight][hdinsight-use-sqoop]
* [Verwenden Sie Schwein mit HDInsight][hdinsight-use-pig]
* [Entwickeln von Java MapReduce Programme für HDInsight][hdinsight-develop-mapreduce]
* [Entwickeln von Python Hadoop streaming-Programme für HDInsight][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
