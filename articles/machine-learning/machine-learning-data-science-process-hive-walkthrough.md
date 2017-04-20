<properties
    pageTitle="Team wissenschaftliche Daten in Aktion: Verwendung Hadoop Cluster | Microsoft Azure"
    description="Team von wissenschaftlichen Daten verwenden für eine End-to-End-Szenario mit einem HDInsight Hadoop-Cluster erstellen und Bereitstellen eines Modells mit einer öffentlich zugänglichen Datasets."
    services="machine-learning,hdinsight"
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
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>Team wissenschaftliche Daten in Aktion: HDInsight Hadoop Cluster

In dieser exemplarischen Vorgehensweise verwenden wir das [Team Daten Wissenschaft Prozess (TDSP)](data-science-process-overview.md) in einem End-to-End-Szenario mit einer [Azure HDInsight Hadoop-Cluster](https://azure.microsoft.com/services/hdinsight/) zu speichern, feature Engineering Daten aus dem öffentlich zugänglichen [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) Dataset und zum Beispiel die Daten nach unten. Modelle der Daten werden mit Azure Machine Learning Binär- und multiclass Klassifizierung und Regression vorhersehbaren Aufgaben erstellt.

Eine exemplarische Vorgehensweise veranschaulicht, die einen größeren Dataset (1 TB) für ein ähnliches Szenario mit HDInsight Hadoop-Cluster für die Datenverarbeitung zu behandeln, finden Sie unter [Team wissenschaftliche Daten - mithilfe von Azure HDInsight Hadoop-Cluster auf einem 1-TB-Dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Es ist möglich, ein IPython Notebook die Aufgaben die exemplarischen Vorgehensweise mit 1 TB-Dataset dargestellt. Benutzer möchten diese Vorgehensweise wenden die [Criteo eine Struktur ODBC-Verbindung](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) exemplarischen.


## <a name="dataset"></a>NYC Taxi Trips Dataset Beschreibung

NYC Taxi Reise werden etwa 20GB komprimierte kommagetrennten Werten (CSV) Dateien (~ 48GB nicht komprimiert), 173 Millionen einzelnen Schleifen und die Preise aus jeder Reise bezahlt. Jeder Reise Datensatz enthält die Abholung und Rückgabe an, anonymisierten Hack (Treiber) Lizenznummer und Uhrzeit Medaille (Taxi eindeutige Id). Daten ist umfasst alle Fahrten im Jahr 2013 und in die folgenden zwei Datasets für jeden Monat:

1. 'Trip_data' CSV-Dateien enthalten REISEDETAILS wie Anzahl der Fahrgäste, Abholung und Drop-Off Punkt Fahrtdauer und Reisedauer. Hier sind einige Beispieldatensätze:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 'Trip_fare' CSV-Dateien enthalten Details für jede Reise wie Zahlungstyp, Fahrpreis Betrag Zuschlag und steuern, Tipps und Mautgebühren gezahlten Flugpreis und den Gesamtbetrag. Hier sind einige Beispieldatensätze:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Der eindeutige Schlüssel auf Reise\_Daten und Reise\_Tarif besteht aus den Feldern: Medaille Hack\_Lizenz und Abholung\_Datetime.

Um alle Details für eine bestimmte, genügt mit drei Tasten: "medaillon", "hack\_Lizenz" und "Pickup\_Datetime".

Wir werden in Kürze in Hive-Tabellen speichern einige Details der Daten beschrieben.

## <a name="mltasks"></a>Beispiele für Vorhersage Aufgaben
Wenn nähert Daten bestimmen die Art der Vorhersagen zu ihrer Analyse anhand kann Aufgaben zu klären, die für die Bearbeitung benötigen.
Drei Beispiele Vorhersage-Probleme, die wir in dieser exemplarischen Vorgehensweise angehen, deren Formulierung basiert auf, der *Tipp\_Betrag*:

1. **Binäre Klassifizierung**: Vorhersagen, ob eine für eine Fahrt also Trinkgeld wurde eine *Tipp\_Betrag* größer als 0 Beispiel ist zwar ein *Tipp\_Betrag* $0 ist eine negative.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Multiklassenklassifizierung**: Bereich Tipp Beträge für die Reise vorhergesagt. Wir teilen die *Tipp\_Betrag* in fünf Lagerplätze oder Klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressionsaufgabe**: Menge Trinkgeld für eine Reise vorhergesagt.  


## <a name="setup"></a>Richten Sie eine HDInsight Hadoop Cluster für erweiterte Analyse

>[AZURE.NOTE] Dies ist normalerweise eine Aufgabe **Admin** .

Sie können eine Azure Umgebung für erweiterte Analyse einrichten, die einen HDInsight-Cluster in drei Schritten verwendet:

1. [Erstellen ein Speicherkonto](../storage/storage-create-storage-account.md): dieses Speicherkonto zum Speichern von Daten in Azure BLOB-Speicher. Die Daten in HDInsight-Cluster befindet sich auch hier.

2. [Anpassen von Azure HDInsight Hadoop Cluster für die erweiterte Analyse und Technologie](machine-learning-data-science-customize-hadoop-cluster.md). Dieser Schritt erstellt eine Azure HDInsight Hadoop Cluster mit 64-Bit-Anaconda Python 2.7 auf allen Knoten installiert. Es gibt zwei wichtige Schritte beim Anpassen von HDInsight Cluster an.

    * Denken Sie daran, verknüpfen Sie das Speicherkonto erstellt in Schritt 1 mit der HDInsight beim Erstellen. Dieses Speicherkonto zum Datenzugriff im Cluster verarbeitet wird.

    * Nach Erstellung des Clusters Remotezugriff auf dem Head-Knoten des Clusters. Navigieren Sie zu der Registerkarte **Konfiguration** und auf **Remote aktivieren**. Dieser Schritt gibt die Benutzeranmeldeinformationen für Remotebenutzernamen verwendet.

3. [Ein Azure Machine Learning-Arbeitsbereich erstellen](machine-learning-create-workspace.md): Diese Azure Machine Learning Arbeitsbereich zum Learning Modelle erstellen. Diese Aufgabe ist nach Abschluss einer Untersuchung Anfangsdaten und Sampling mit HDInsight Cluster gerichtet.

## <a name="getdata"></a>Die Daten aus einer öffentlichen Quelle abrufen

>[AZURE.NOTE] Dies ist normalerweise eine Aufgabe **Admin** .

Um [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) Dataset aus ihrem öffentlichen Speicherort, können [Verschieben Sie Daten zwischen Azure BLOB-Speicher](machine-learning-data-science-move-azure-blob.md) beschriebenen Methoden Sie die Daten auf Ihren Computer kopieren.

Hier erläutert, wie AzCopy verwenden, um Dateien mit übertragen. Downloaden und installieren AzCopy gehen Sie unter [Erste Schritte mit AzCopy Befehlszeilenprogramm](../storage/storage-use-azcopy.md).

1. Geben Sie in einem Eingabeaufforderungsfenster die folgenden AzCopy Befehle *< Path_to_data_folder >* mit dem gewünschten Ziel:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Wenn der Kopiervorgang abgeschlossen ist, werden insgesamt 24 ZIP-Dateien im ausgewählten Ordner. Extrahieren Sie die heruntergeladenen Dateien in das gleiche Verzeichnis auf dem lokalen Computer. Notieren Sie sich den Ordner, in dem nicht komprimierten Dateien befinden. Dieser Ordner wird als bezeichnet die *< Pfad\_,\_Unzipped_data\_Dateien\> * ist wie folgt.


## <a name="upload"></a>Hochladen der Daten zum Standardcontainer Azure HDInsight Hadoop-cluster

>[AZURE.NOTE] Dies ist normalerweise eine Aufgabe **Admin** .

Ersetzen Sie die folgenden Befehle AzCopy die folgenden Parameter mit den tatsächlichen Werten, die Sie beim Erstellen des Clusters Hadoop angegeben und Entpacken Sie die Dateien.

* ***& #60; Path_to_data_folder >*** Verzeichnis (mit Pfad) auf dem Computer, der die entpackten Dateien enthalten  
* ***& #60; Kontonamen Hadoop Cluster Storage >*** HDInsight Cluster zugeordnete Speicherkonto
* ***& #60; Standardcontainer Hadoop Cluster >*** Standardcontainer im Cluster verwendet. Beachten Sie, dass der Name der Standardcontainer normalerweise den gleichen Namen wie der Cluster selbst. Beispielsweise wenn Cluster "abc123.azurehdinsight.net" aufgerufen wird, ist der Standardcontainer abc123.
* ***& #60; speicherkontoschlüssel >*** Schlüssel für das Speicherkonto Cluster verwendet

Führen Sie die folgenden zwei AzCopy Befehle, Befehlszeile oder ein Windows PowerShell-Fenster auf Ihrem Computer.

Dieser Befehl lädt die Reisedaten ***Nyctaxitripraw*** Verzeichnis im Standardcontainer Hadoop Cluster.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Dieser Befehl lädt Daten der Tarif ***Nyctaxifareraw*** Verzeichnis im Standardcontainer Hadoop-Cluster.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Die Daten sollten in Azure BLOB-Speicher und im HDInsight-Cluster verbraucht werden.

## <a name="#download-hql-files"></a>Melden Sie sich bei dem Head-Knoten des Clusters Hadoop und und explorativen Datenanalyse

>[AZURE.NOTE] Dies ist normalerweise eine Aufgabe **Admin** .

Zugriff auf dem Head-Knoten des Clusters Versuchsvorhaben für die Analyse und Probenahme von Daten folgen Sie dem Verfahren in [Access die Hadoop Cluster Head Knoten](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

In dieser exemplarischen Vorgehensweise verwenden wir hauptsächlich Abfragen in [Hive](https://hive.apache.org/), einer Abfragesprache wie SQL geschriebene auszuführenden vorläufige Daten kennen. Die Hive-Abfragen werden in .hql-Dateien gespeichert. Wir probieren dann ab dieser Daten in Azure Machine Learning Modelle.

Explorative Datenanalyse Cluster Vorbereitung Dateien wir die .hql mit den relevanten Scripts Struktur von [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) in ein lokales Verzeichnis (C:\temp) auf dem Head-Knoten. Dazu öffnen Sie die **Befehlszeile** in den Head-Knoten des Clusters und folgende zwei Befehle:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Diese beiden Befehle downloadet alle .hql Dateien in dieser exemplarischen Vorgehensweise im lokalen Verzeichnis ***C:\temp & 92;*** in den Head-Knoten benötigt.

## <a name="#hive-db-tables"></a>Erstellen Sie Struktur-Datenbank und Monat partitionierte Tabellen

>[AZURE.NOTE] Dies ist normalerweise eine Aufgabe **Admin** .

Wir können jetzt Struktur Tabellen für unsere NYC Taxi Dataset erstellen.
In den Head-Knoten des Clusters Hadoop ***Hadoop Befehlszeile*** auf dem Head-Knoten aufrufen Sie und das Verzeichnis Struktur durch Eingabe des Befehls

    cd %hive_home%\bin

>[AZURE.NOTE] **Alle Struktur Befehle Papierschacht Struktur oben in dieser exemplarischen Vorgehensweise ausführen / Directory aufgefordert. Dieser kümmern Pfad Probleme automatisch. Verwenden wir die Begriffe "Struktur Verzeichnis Aufforderung" "Struktur Bin / Directory Aufforderung", und "Hadoop" Synonym in dieser exemplarischen Vorgehensweise.**

Geben Sie aus dem Verzeichnis Struktur den folgenden Befehl in Hadoop Befehlszeile des Head-Knotens zum Übergeben der Abfrage Struktur Struktur Datenbank und Tabellen erstellen:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Hier wird der Inhalt der ***C:\temp\sample\_Struktur\_erstellen\_Db\_und\_tables.hql*** Datei ***Struktur Datenbank Tabellen und ***Nyctaxidb*** ***Reise*** - und***erstellt.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Das Hive-Skript erstellt zwei Tabellen:

* die "Reise" Tabelle enthält REISEDETAILS jeder Fahrt (Treiberdetails abgeholt, Wegstrecke und Zeiten)
* die Tabelle "Preis" enthält Fahrpreis Details (Flugpreis, Spitze Betrag, Gebühren und Zuschlagsgebühren).

Wenn Sie zusätzliche Hilfe mit diesen Verfahren benötigen oder alternative zu untersuchen, finden Sie im Abschnitt [Senden Struktur Abfragen direkt aus der Hadoop-Befehlszeile ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Daten von Partitionen Struktur Tabellen laden

>[AZURE.NOTE] Dies ist normalerweise eine Aufgabe **Admin** .

NYC Taxi Dataset hat eine natürliche Partitionierung nach Monat nutzen wir schnellere Verarbeitung und Abfrage. Daten nach Monaten partitioniert die Tabellen "Reise" und "Preis" Struktur Laden der PowerShell Befehle unten (aus der **Befehlszeile Hadoop**Struktur-Verzeichnis).

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

Die *Probe\_Struktur\_laden\_Daten\_von\_partitions.hql* enthält die folgenden Befehle **Laden** .

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Beachten Sie, dass eine Anzahl Hive-Abfragen verwenden wir hier bei der Untersuchung betreffen nur eine einzelne Partition oder nur einige Partitionen auf. Aber diese Abfragen können die gesamten Daten.

### <a name="#show-db"></a>Datenbanken in HDInsight Hadoop Cluster anzeigen

Um in HDInsight Hadoop Cluster in Hadoop Befehlszeilenfenster erstellten Datenbanken anzuzeigen, führen Sie folgenden Befehl in Hadoop Befehlszeile:

    hive -e "show databases;"

### <a name="#show-tables"></a>Anzeigen der Hive-Tabellen in der Datenbank nyctaxidb

Um die Tabellen in der Datenbank Nyctaxidb anzuzeigen, führen Sie folgenden Befehl in Hadoop Befehlszeile:

    hive -e "show tables in nyctaxidb;"

Wir können bestätigen, dass die Tabellen partitioniert werden durch folgenden Befehl:

    hive -e "show partitions nyctaxidb.trip;"

Die erwartete Ausgabe ist unten dargestellt:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Ebenso kann sichergestellt werden, dass Tabelle Fahrpreis partitionierten durch folgenden Befehl:

    hive -e "show partitions nyctaxidb.fare;"

Die erwartete Ausgabe ist unten dargestellt:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Durchsuchen von Daten und Featureentwicklung Struktur

>[AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

Durchsuchen von Daten und Aufgaben für die Daten in Tabellen Struktur Funktion können Hive-Abfragen ausgeführt werden. Es folgen Beispiele für solche Aufgaben, dass wir Sie in diesem Abschnitt durchgehen:

- Die ersten 10 Datensätze in beide Tabellen anzeigen
- Durchsuchen Sie Daten-Distributionen einige Felder in unterschiedlichen Zeitfenster.
- Überprüfen Sie Datenqualität Längen- und Felder.
- Binäre und multiclass Klassifizierung Etiketten auf der Grundlage der **Tipp\_Betrag**.
- Erzeugen Sie Funktionen von computing direkt Abstände

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Untersuchung: Die ersten 10 Datensätze in Tabelle Reise anzeigen

>[AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

Um anzuzeigen, wie die Daten dargestellt, werden 10 Datensätze aus jeder Tabelle untersucht. Führen Sie die folgenden beiden Abfragen einzeln aus der Struktur Verzeichnis in der Befehlszeile Hadoop Konsole Datensätze überprüfen.

Die ersten 10 Datensätze in der Tabelle "Reise" ersten abrufen:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Die Top 10 Datensätze in der Tabelle "Preis" ersten abrufen:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Es ist häufig sinnvoll, die Datensätze in einer Datei für die einfache Anzeige speichern. Eine kleine Änderung an die obige Abfrage erreicht dies:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Untersuchung: Die Anzahl der Datensätze in alle 12 Partitionen anzeigen

>[AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

Interessant ist wie die Zahl der im Laufe des Kalenderjahrs variiert. Nach Monat gruppieren ermöglicht diese Verteilung Reisen ähnelt.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Dadurch ist die Ausgabe:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Hier ist die erste Spalte der Monat und das zweite ist die Anzahl der Besuche für diesen Monat.

Wir können auch die Gesamtzahl der Datensätze in unserem Datensatz Reise zählen, den folgenden Befehl an der Struktur Verzeichnis.

    hive -e "select count(*) from nyctaxidb.trip;"

Dies ergibt:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Befehle ähnlich für das DataSet Reise, können wir Struktur überprüft die Anzahl der Datensätze für den Tarif Datensatz aus der Struktur Verzeichnis Abfragen.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Dadurch ist die Ausgabe:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Beachten Sie, dass die gleiche Anzahl Besuche pro Monat für beide Datensätze zurückgegeben wird. Dadurch wird die erste Überprüfung, dass die Daten korrekt geladen wurde.

Zählen die Gesamtzahl der Datensätze im DataSet Fahrpreis lässt den Befehl unten aus der Struktur Verzeichnis:

    hive -e "select count(*) from nyctaxidb.fare;"

Dies ergibt:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Die Gesamtzahl der Datensätze in beide Tabellen ist identisch. Dadurch wird eine zweite Überprüfung, dass die Daten korrekt geladen wurde.

### <a name="exploration-trip-distribution-by-medallion"></a>Durchsuchen: Reise Verteilung von medaillon

>[AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

In diesem Beispiel wird der Medaille (Taxi Zahlen) mit mehr als 100 innerhalb eines bestimmten Zeitraums. Die Abfrage profitiert von partitionierten Tabellenzugriff seit Partition Variable **Monat**bedingt. Die Ergebnisse der Abfrage werden in einer lokalen Datei queryoutput.tsv in geschrieben `C:\temp` auf dem Head-Knoten.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Hier wird der Inhalt des *Beispiel\_Struktur\_Reise\_Anzahl\_von\_medallion.hql* Datei zur Überprüfung.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Der Medaille im DataSet NYC Taxi identifiziert eindeutig Cab. Wir können identifizieren, welche CAB-Dateien sind "beschäftigt" gefragt, welche mehr als eine bestimmte Anzahl von Schleifen in einem bestimmten Zeitraum erstellt. Das folgende Beispiel identifiziert CAB-Dateien, die aus mehr als 100 Besuche der ersten drei Monate und die Abfrageergebnisse in einer lokalen Datei C:\temp\queryoutput.tsv gespeichert.

Hier wird der Inhalt des *Beispiel\_Struktur\_Reise\_Anzahl\_von\_medallion.hql* Datei zur Überprüfung.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Aus der Struktur Verzeichnis den Befehl unten:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Durchsuchen: Reise Verteilung von medaillon und hack_license

>[AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

Bei einem Dataset, wir häufig die Anzahl der co-Vorkommen von Gruppen von Werten untersuchen möchten. In diesem Abschnitt enthalten ein Beispiel dazu CAB-Dateien und Treiber.

Die *Probe\_Struktur\_Reise\_Anzahl\_von\_Medaille\_license.hql* Datei gruppiert Fahrpreis Dataset auf "medaillon" und "Hack_license" und gibt zählt jede Kombination. Folgen Sie seinen Inhalt.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Diese Abfrage gibt Cab und Treiberkombinationen nach Anzahl der Reisen absteigend sortiert.

Aus der Struktur Verzeichnis ausführen:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Die Ergebnisse der Abfrage werden in einer lokalen Datei C:\temp\queryoutput.tsv geschrieben.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Untersuchung: Bewertung Qualität durch ungültige Länge-Breite Datensätze überprüfen

>[AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

Gemeinsames Ziel der explorativen Datenanalyse ist ungültig oder fehlerhafte Datensätze herausfiltern. Das Beispiel in diesem Abschnitt bestimmt, ob die Längen- oder Breitengrad Felder weit außerhalb des NYC Wert. Da es wahrscheinlich ist, dass solche Datensätze eine falsche Länge-Breite Werte soll aus beliebigen Daten eliminieren, die für die Modellierung verwendet werden.

Hier wird der Inhalt des *Beispiel\_Struktur\_Qualität\_assessment.hql* Datei zur Überprüfung.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Aus der Struktur Verzeichnis ausführen:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

*S -* Argument enthalten in diesem Befehl unterdrückt Status Bildschirm Ausdruck der Struktur Map/Reduce-Aufträge. Dies ist nützlich, da es den Bildschirm Drucken der Abfrageergebnisse Struktur besser lesbar macht.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Untersuchung: Binäre Klasse Distributionen von Tipps

> [AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

Im Abschnitt [Vorhersage Beispiele](machine-learning-data-science-process-hive-walkthrough.md#mltasks) beschriebenen Problems binäre Klassifizierung ist es nützlich zu wissen, ob eine QuickInfo angegeben wurde. Diese Tipps ist binär:

* Tipp gegeben (Klasse 1, Spitze\_Betrag > 0)  
* Kein Tipp (Klasse 0, Spitze\_Betrag = 0).

Das *Beispiel\_Struktur\_Geneigter\_frequencies.hql* Datei unten angezeigt wird.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Aus der Struktur Verzeichnis ausführen:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Untersuchung: Klasse Verteilung in der multiclass

> [AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

Für die [Vorhersage Beispiele](machine-learning-data-science-process-hive-walkthrough.md#mltasks) beschriebene multiklassenklassifizierung-Problem kann dieses Dataset auch eine natürliche Einstufung, wo wir an die Tipps vorhersagen möchten. Lagerplätze können wir um Tipp Bereiche in der Abfrage zu definieren. Zu der Klasse Distributionen für die verschiedenen Bereiche Tipp, verwenden wir die *Probe\_Struktur\_Tipp\_Bereich\_frequencies.hql* Datei. Folgen Sie seinen Inhalt.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Führen Sie den folgenden Befehl aus Hadoop Befehlszeilenkonsole:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Untersuchung: Berechnen Sie unmittelbaren Abstands zwischen zwei Länge-Breite

> [AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

Eine direkte Abstand ermöglicht die Diskrepanz zwischen ihm und den tatsächlichen Abstand zu. Wir ermutigen dazu darauf hin, dass ein Fahrgast kann voraussichtlich Tipp, wenn sie herausfinden, dass der Treiber sie absichtlich länger Weg getroffen hat.

Finden Sie den Vergleich zwischen den tatsächlichen Abstand und [Haversine Abstand](http://en.wikipedia.org/wiki/Haversine_formula) zwischen Punkten Länge-Breite (Abstand "großen Kreis") verwenden wir trigonometrische Funktionen innerhalb der Struktur so:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

In der obigen Abfrage ist R der Radius der Erde in Meilen und Pi Bogenmaß konvertiert. Beachten Sie, dass die Länge-Breite "Punkte" gefilterte sind Werte entfernen weit NYC.

In diesem Fall schreiben wir unsere Ergebnisse ein Verzeichnis namens "Queryoutputdir". Die Befehlsfolge unten zunächst erstellt dieses Verzeichnis und führt dann den Befehl Struktur.

Aus der Struktur Verzeichnis ausführen:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Die Abfrageergebnisse werden 9 Azure-Blobs geschrieben ***Queryoutputdir-000000\_0*** , ***Queryoutputdir/000008\_0*** unter den Standardcontainer Hadoop-Cluster.

Um die Größe der einzelnen Blobs anzuzeigen, führen wir Folgendes aus der Struktur Verzeichnis:

    hdfs dfs -ls wasb:///queryoutputdir

Um den Inhalt einer Datei anzuzeigen, sagen 000000\_0, verwenden wir Hadoop `copyToLocal` Befehl so.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`kann bei großen Dateien sehr langsam sein und sollte nicht für die Verwendung mit.  

Ein Hauptvorteil dieser Daten in Azure Blob befinden werden möglicherweise Daten in Azure Machine Learning [Importdaten] vorgestellt[ import-data] Modul.


## <a name="#downsample"></a>Beispiel von Daten und erstellen Modelle in Azure Machine Learning

> [AZURE.NOTE] Dies ist normalerweise ein Vorgang **Datenwissenschaftler** .

Nach der Analysephase explorativen Daten können wir nun zum Beispiel die Daten in Azure Machine Learning Modelle nach unten. In diesem Abschnitt erfahren wie Sie Hive-Abfrage zum Beispiel unten die Daten dann aus den [Importierten Daten] erfolgt[ import-data] Modul in Azure maschinelles lernen.

### <a name="down-sampling-the-data"></a>Sie die Samplingdaten

Dieses Verfahren umfasst zwei Schritte. Zunächst verknüpfen die Tabellen **nyctaxidb.trip** und **nyctaxidb.fare** über Tasten, die in allen Datensätzen vorhanden sind: "medaillon", "hack\_Lizenz", und "Pickup\_Datetime". Wir erstellen dann eine binäre Klassifizierung Bezeichnung **gekippt** und Multi-Klasse Typenschild **Tipp\_Klasse**.

Verwenden Sie den Pfeil können abgetastete Daten direkt aus den [Daten importieren] [ import-data] Modul in Azure Machine Learning sind die obige Abfrage auf eine interne Struktur Tabelle speichern. Im folgenden eine interne Tabelle Struktur erstellen und den Inhalt der verknüpften mit aufgenommenen Daten auffüllen.

Die Abfrage wendet Struktur Standardfunktionen zum Generieren der Stunde des Tages, Woche des Jahres Wochentag (1 steht für Montag und 7 steht für Sonntag) direkt aus der "Pickup\_Datetime" Feld und des unmittelbaren Abstands zwischen der Abholung und Dropoff. Benutzer können eine vollständige Liste der Funktionen [LanguageManual](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) UDF verweisen.

Die Abfrage nimmt dann unten die Daten, damit die Ergebnisse der Abfrage in Azure Machine Learning Studio passt. Nur 1 % des ursprünglichen Dataset wird in dem Studio importiert.

Im folgenden wird der Inhalt der *Beispiel\_Struktur\_vorbereiten\_für\_Aml\_full.hql* -Datei, die in Azure Machine Learning Modellbau Daten vorbereitet.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Zum Ausführen dieser Abfrage aus der Struktur Verzeichnis:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Wir haben jetzt eine interne Tabelle "nyctaxidb.nyctaxi_downsampled_dataset" die [Importdaten] zugegriffen werden kann[ import-data] Modul von Azure maschinelles lernen. Außerdem können wir dieses Dataset maschinelles lernen Modelle.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Mithilfe des Importdaten in Azure Machine Learning unten gesammelten Daten zugreifen

Als Voraussetzung für Struktur Abfragen [Daten importieren] [ import-data] Modul von Azure maschinelles lernen wir eine Azure maschinelles lernen Arbeitsbereich und Zugriff auf die Anmeldeinformationen des Clusters und die zugehörige Speicher benötigen.

Einige Details der [Importdaten] [ import-data] Modul und den Parametern eingeben:

**HCatalog Server-URI**: Wenn der Clustername abc123, ist: https://abc123.azurehdinsight.net

**Hadoop Benutzernamen** : Benutzername für den Cluster (**nicht** den Benutzernamen RAS) ausgewählt

**Hadoop Ser Kennwort** : das Kennwort für den Cluster (**nicht** das Kennwort) ausgewählt

**Speicherort der Ausgabedaten** : Dies möchten Azure.

**Azure-speicherkontoname** : Name des Standardkontos Speicher dem Cluster zugeordnet.

**Azure Containernamen** : Dies ist der Standardcontainername für den Cluster und entspricht in der Regel den Namen des Clusters. Für einen Cluster mit dem Namen "abc123" ist dies nur abc123.

> [AZURE.IMPORTANT] **Alle Tabellen zu Abfragen mit den [Importierten Daten] [ import-data] Modul in Azure Machine Learning muss eine Tabelle sein.** Ein Tipp zum bestimmen, ob eine Tabelle T in einer D.db eine interne Tabelle ist lautet wie folgt:

Befehl aus dem Verzeichnis Struktur:

    hdfs dfs -ls wasb:///D.db/T

Die Tabelle ist eine interne Tabelle es ausgefüllt und müssen hier seinen Inhalt anzeigen. Können bestimmen, ob eine Tabelle eine interne Tabelle ist ist der Azure-Speicher-Explorer verwenden. Navigieren Sie zu der Standardcontainername des Clusters und Filtern nach dem Tabellennamen verwenden. Wenn die Tabelle und des Inhalts angezeigt, bestätigt dies ist eine interne Tabelle.

Hier ist eine Momentaufnahme der Hive-Abfrage und die [Importdaten] [ import-data] Modul:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Beachten Sie, dass seit unserer gesammelten Daten befinden sich im Standardcontainer ab, die Hive-Abfrage von Azure Computer ist sehr einfach nur eine "Wählen Sie * aus nyctaxidb.nyctaxi\_aus\_Daten".

Das Dataset kann nun als Ausgangspunkt dienen maschinelles lernen Modelle.

### <a name="mlmodel"></a>Modelle in Azure Machine Learning

Wir können jetzt Model und Modell Bereitstellung in [Azure Machine Learning](https://studio.azureml.net)fortfahren. Die Daten für die Vorhersage Probleme genannten bereit:

**1. binäre Klassifizierung**: vorherzusagen, ob ein Tipp bezahlt wurde eine.

**Teilnehmern verwendet:** Zwei Klassen logistische regression

ein. Dieses Problem ist unser Ziel (oder Klasse) Bezeichnung "Geneigter". Unsere ursprünglichen Down-sampled Dataset hat einige Spalten Ziel für dieses Experiment Klassifizierung Speicherverlusten. Insbesondere: Tipp\_Klasse, Tipp\_Betrag und Summe\_Betrag zeigen Informationen über die Zielmarke nicht auf Tests verfügbar ist. Wir entfernen diese Spalten aus den [Spalten im Dataset auswählen] [ select-columns] Modul.

Das Bild unten zeigt Experiment Vorhersagen, ob für einen bestimmten Trinkgeld wurde.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Für diesen Test wurden unser Ziel Bezeichnung Distributionen ungefähr 1:1.

Das Bild unten zeigt die Verteilung der Tipp Klassenbezeichner für binäre klassifizierungsproblem.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Dadurch erhalten wir ein AUC 0,987, wie in der folgenden Abbildung dargestellt.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. multiklassenklassifizierung**: Bereich Tipp Beträge für die Reise mit den zuvor definierten Klassen vorherzusagen.

**Teilnehmern verwendet:** Logistische Regression multiclass

ein. Für dieses Problem ist unser Ziel (oder Klasse) Bezeichnung "Tipp\_Klasse" die nehmen einen von fünf Werten (0,1,2,3,4). Bei binären Klassifizierung haben wir einige Spalten Ziel für dieses Experiment Speicherverlusten. Insbesondere: Tipp gekippt,\_Betrag insgesamt\_Betrag zeigen Informationen über die Zielmarke nicht auf Tests verfügbar ist. Wir entfernen diese Spalten mit den [Spalten im Dataset auswählen] [ select-columns] Modul.

Das Bild unten zeigt Experiment vorherzusagen, in welchem Lagerplatz ein Tipp fallen (Klasse 0: Tipp = $0, Klasse 1: > $0 und Tipp Tipp < = $5, Klasse 2: Tipp > 5 und Tipp < = $10, Klasse 3: Tipp > $10 und Tipp < = $20, Klasse 4: Tipp > $20)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Wir zeigen nun sieht unsere eigentlichen Klasse Verteilung. Wir sehen, dass Klasse 0 und Klasse 1 verbreitet sind die Klassen selten.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Dieses Experiment verwenden wir eine Matrix Verwirrung sich unsere Vorhersage-Genauigkeit. Dies ist unten dargestellt.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Beachten Sie, dass unsere Klasse Genauigkeit verbreitete Klassen ist gut, das Modell nicht gut "lernen" seltener Klassen.


**3. regressionsaufgabe**: Menge Trinkgeld für eine Reise vorhergesagt.

**Teilnehmern verwendet:** Stärkere Entscheidungsstruktur

ein. Für dieses Problem ist unser Ziel (oder Klasse) Bezeichnung "Tipp\_Betrag". Unser Ziel Verluste sind: Tipp gekippt,\_Klasse insgesamt\_Betrag; Diese Variablen zeigen Informationen über Tip Betrag an Tests normalerweise nicht verfügbar. Wir entfernen diese Spalten mit den [Spalten im Dataset auswählen] [ select-columns] Modul.

Snapshot unten zeigt Experiment an bestimmten Tipp vorherzusagen.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. Für Regressionsprobleme die Genauigkeit unserer Vorhersage anhand des quadratischen Fehlers Vorhersagen, den Bestimmungskoeffizienten und dergleichen gemessen. Wir zeigen diese unten.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Wir sehen, dass das Bestimmtheitsmaß 0.709 ist, was etwa 71 % der Varianz unser Modell Koeffizienten erklärt.

> [AZURE.IMPORTANT] Erfahren Sie mehr über Azure maschinelles lernen und zu verwenden, finden Sie in [Neuigkeiten maschinelles lernen?](machine-learning-what-is-machine-learning.md). Eine sehr nützliche Ressource mit ein paar Computerlernen Experimente Azure Machine Learning ist [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/). Die Galerie umfasst Farbumfang Experimente und bietet eine umfassende Einführung in die vielen Funktionen der Azure Machine Learning.

## <a name="license-information"></a>Lizenzinformationen

In dieser exemplarischen Vorgehensweise Beispiel und die zugehörigen Skripts werden von Microsoft MIT Lizenz freigegeben. Bitte checken Sie die Datei LICENSE.txt im Verzeichnis des Beispielcodes auf GitHub Weitere Informationen.

## <a name="references"></a>Referenzen

• [Andrés Monroy NYC Taxi Trips-Downloadseite](http://www.andresmh.com/nyctaxitrips/)  
• [Vereiteln NYC Taxi Reisedaten von Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi Limousine Kommission Forschung und Statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
