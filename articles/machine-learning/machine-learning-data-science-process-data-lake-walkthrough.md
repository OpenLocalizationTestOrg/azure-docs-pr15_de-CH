<properties
    pageTitle="Skalierbare Datenwissenschaft Azure Data Lake: eine End-to-End Exemplarische Vorgehensweise | Microsoft Azure"
    description="Wie Sie Azure Data Lake Daten durchsuchen und binäre Klassifizierung Aufgaben auf ein Dataset."  
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
    ms.date="09/19/2016"
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Skalierbare Datenwissenschaft Azure Data Lake: eine End-to-End Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise veranschaulicht, wie mit Azure Data Lake Durchsuchen von Daten und Klassifizierungsaufgaben auf einer Reise Taxi NYC binäre und Tarif Dataset Vorhersagen, ob ein Tipp einen bezahlt wird. Es führt Sie durch die Schritte des [Team wissenschaftliche Daten](http://aka.ms/datascienceprocess)End-to-End von Datenerfassung Training Modell und dann auf die Bereitstellung eines Webdienstes, der das Modell veröffentlicht.


### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

[Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) bietet alle Funktionen erforderlichen datenwissenschaftler erleichtert Datenspeicher Größe, Form und Geschwindigkeit und Datenverarbeitung, modernste Analytik und lernen Modellierung mit hoher Skalierbarkeit kostengünstige Computer durchzuführen.   Sie Zahlen pro pro Auftrag nur Daten tatsächlich verarbeitet werden. Azure Data Lake Analytics U-SQL enthält, eine Sprache, die deklarative Natur von SQL mit Ausdruckskraft C# ermöglicht skalierbares gemischt verteilt ermöglichen. Sie können Sie verarbeiten unstrukturierte Daten anwenden Schema lesen, Einfügen benutzerdefinierter Logik und benutzerdefinierte Funktionen (UDFs) und enthält Erweiterbarkeit feinkörnige Kontrolle Ebene ausführen aktivieren. Erfahren Sie mehr über die Designphilosophie U SQL Blogbeitrag [Visual Studio](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Datenanalyse See gehört auch wichtige Cortana Analytics Suite und Azure SQL Data Warehouse Power BI und Data Factory. Dies ermöglicht eine vollständige Cloud Datenverlustvorfalls und erweiterte Analyse-Plattform.

In dieser exemplarischen Vorgehensweise beginnt die Komponenten und Ressourcen für Aufgaben mit dem Datenanalyse, die wissenschaftlichen Daten und wie Sie diese installieren. Beschreibt die Schritte, Datenverarbeitung mit U-SQL und abschließend Verwendung Python und Struktur mit Azure Machine Learning Studio erstellen und Bereitstellen der Vorhersagemodelle. 


### <a name="u-sql-and-visual-studio"></a>U-SQL und Visual Studio

In dieser exemplarischen Vorgehensweise empfiehlt Visual Studio U-SQL-Skripts verarbeitet das Dataset bearbeiten. U-SQL-Skripts beschriebenen und in einer separaten Datei. Der Prozess umfasst Einnahme und Erforschen der Samplingdaten. Außerdem wird das Ausführen eines Auftrags U-SQL-Skript von Azure-Portal veranschaulicht. Hive-Tabellen werden für die Daten in einem zugeordneten HDInsight-Cluster erstellen und Bereitstellung eines klassifizierungsmodells binäre in Azure Machine Learning Studio erstellt.  


### <a name="python"></a>Python

Diese exemplarische Vorgehensweise enthält auch einen Abschnitt, der zeigt, wie das Erstellen und Bereitstellen eines Vorhersagemodells Azure Machine Learning Studio Python mit.  Bieten wir ein Jupyter Notebook-Skripten für diese Schritte. Das Notizbuch enthält Code für einige zusätzliche Funktion engineering Schritte und Modelle multiklassenklassifizierung und zusätzlich die hier beschriebenen binären Klassifizierung modeling Regression. Regression soll an der Spitze basierend auf anderen Tip-Funktionen vorherzusagen. 


### <a name="azure-machine-learning"></a>Azure maschinelles lernen
Azure Machine Learning Studio dient zum Erstellen und Bereitstellen der Vorhersagemodelle. Dies erfolgt über zwei Herangehensweisen: zuerst mit Python-Skripten und Struktur von Tabellen in einem Cluster HDInsight (Hadoop).


### <a name="scripts"></a>Skripts

In dieser exemplarischen Vorgehensweise werden die wichtigsten Schritte beschrieben. Sie können vollständige **U-SQL-Skripts** und **Jupyter Notebook** von [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough)herunterladen.


## <a name="prerequisites"></a>Erforderliche Komponenten

Vor dieser Themen benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie nicht bereits eine haben, finden Sie unter [Get Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- [Empfohlen] Visual Studio 2013 und 2015. Wenn Sie nicht bereits eine dieser Versionen installiert haben, können Sie [hier](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)eine kostenlose Community Edition. Klicken Sie auf die Schaltfläche **Download Gemeinschaft 2015** Abschnitt Visual Studio. 

>[AZURE.NOTE] Statt Visual Studio auch können Azure-Portal Sie Azure Data Lake Abfragen absenden. Wir bieten Anleitung sowohl mit Visual Studio und das Portal im Abschnitt **Daten mit U-SQL**. 

- Anmeldung für Azure Datenvorschau See

>[AZURE.NOTE] In diesem Fall müssen Sie eine Genehmigung von Azure Data Lake Speicher (ADL) und Azure Data Lake Analytics (ADLA) sind diese Dienste in der Vorschau. Sie werden aufgefordert, sich beim Erstellen der ersten ADL oder ADLA. Seufzer klicken auf **Vorschau anmelden**, lesen Sie den Lizenzvertrag und klicken Sie auf **OK**. Hier ist z. B. ADL-Anmeldeseite:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Wissenschaft-Umgebung für Azure Data Lake vorbereiten
Vorbereitung die Umgebung Wissenschaft für diese exemplarische Vorgehensweise erstellen Sie die folgenden Ressourcen:

- Azure-See Datenspeicher (ADL) 
- Azure Data Lake Analytics (ADLA)
- Azure BLOB-Speicher-Konto
- Azure Machine Learning Studio-Konto
- Azure Data Lake-Tools für Visual Studio (empfohlen)

Dieser Abschnitt beschreibt diese Ressourcen erstellen. Hive-Tabellen mit Azure Machine Learning statt Python verwenden möchten zum Erstellen eines Modells auch müssen Sie einen HDInsight (Hadoop)-Cluster bereitstellen. Dieses alternative Verfahren im folgenden Abschnitt beschrieben.
<br/>
>AZURE. Hinweis **Azure See Datenspeicher** erstellt werden entweder separat oder beim Erstellen von **Azure Data Lake Analytics** als Standard speichern. Anleitung für die Erstellung dieser Ressourcen unten separat verwiesen, aber Daten See Speicherkonto muss nicht separat erstellt werden.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Erstellen eines Datenspeichers See Azure

Erstellen einer ADL [Azure-Portal](http://portal.azure.com). Details finden Sie unter [Erstellen eines Clusters HDInsight mit dem Datenspeicher verwenden Azure-Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Achten Sie darauf, dass Cluster AAD Identität in **DataSource** Blade **Optionale Konfiguration** Blade beschrieben einrichten. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Ein Azure Data Lake Analytics-Konto erstellen
Erstellen Sie ein Konto ADLA aus dem [Azure-Portal](http://portal.azure.com). Details finden Sie in [Lernprogramm: Erste Schritte mit Azure See Datenanalyse mithilfe von Azure-Portal](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Erstellen Sie ein Konto Azure BLOB-Speicher
Registrieren Sie Azure BLOB-Speicher aus dem [Azure-Portal](http://portal.azure.com). Details finden Sie in einen Speicher Kontoabschnitt [über Azure Storage](../storage/storage-create-storage-account.md)-Konten erstellen.
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Einrichten eines Kontos Azure Machine Learning Studio
Melden Sie an/in Azure Machine Learning Studio auf [Azure maschinelles lernen](https://azure.microsoft.com/services/machine-learning/) . Klicken Sie auf **jetzt beginnen** und dann eine "Freie Arbeitsbereich" oder "Standard-Arbeitsbereich". Danach werden Sie Versuche in Azure ML Studio erstellen.  

### <a name="install-azure-data-lake-tools-recommended"></a>Installieren von Azure See Datentools [empfohlen]
Installieren Sie Azure Data Lake Tools für Ihre Version von Visual Studio aus [Azure Data Lake-Tools für Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Nach erfolgreichem die Installation Abschluss von Visual Studio öffnen. Sie sollten die Daten im Menü oben Registerkarte see. Azure Ressourcen sollten im linken Bereich angezeigt, wenn Sie Ihre Azure-Konto anmelden.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>Das Dataset NYC Taxi Trips
Hier verwendete Datensatz ist ein öffentlich Dataset - [NYC Taxi Trips Dataset](http://www.andresmh.com/nyctaxitrips/). NYC Taxi Reise Daten besteht aus ca. 20GB komprimierte CSV-Dateien (~ 48GB nicht komprimiert), Aufzeichnung 173 Millionen einzelnen Schleifen und die Preise für jede Reise bezahlt. Jeder Reise Datensatz enthält den Abholung und Ladengeschäft und Zeiten, anonymisierten Hack (Treiber) Lizenznummer sowie Anzahl Medaille (Taxi eindeutige Id). Daten ist umfasst alle Fahrten im Jahr 2013 und in die folgenden zwei Datasets für jeden Monat:

 - Trip_data CSV enthält REISEDETAILS wie Anzahl der Fahrgäste, Abholung und Drop-Off Punkt Fahrtdauer und Reisedauer. Hier sind einige Beispieldatensätze:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - 'Trip_fare' enthält CSV Details für jede Reise wie Zahlungstyp, Fahrpreis Betrag Zuschlag und steuern, Tipps und Mautgebühren gezahlten Flugpreis und den Gesamtbetrag. Hier sind einige Beispieldatensätze:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Der eindeutige Schlüssel auf Reise\_Daten und Reise\_Tarif besteht aus drei Bereichen: Medaille Hack\_Lizenz und Abholung\_Datetime. Raw CSV-Dateien können aus Interventionsbeständen Azure Blob zugegriffen werden. U-SQL-Skript für diese Verknüpfung wird im Abschnitt [Verknüpfungstabellen Reise und Preis](#join) .

## <a name="process-data-with-u-sql"></a>Prozessdaten mit U-SQL

Die Datenverarbeitung Aufgaben in diesem Abschnitt dargestellten: Einnahme Qualität, untersuchen und die Samplingdaten Es wird das Reise und Tarif Tabellen veranschaulichen. Im letzten Abschnitt zeigt einen U-SQL-Skript von Azure-Portal Auftrag ausführen. Hier werden Links zu jedem Unterabschnitt:

- [Erfassung der Daten: Daten aus öffentlichen Blob lesen](#ingest)
- [Daten-Qualitätskontrolle](#quality)
- [Durchsuchen von Daten](#explore)
- [Reise und Tarif Tabellen verknüpfen](#join)
- [Datenstichproben](#sample)
- [U-SQL-Aufträgen](#run)

U-SQL-Skripts beschriebenen und in einer separaten Datei. Sie können die vollständige **U-SQL-Skripts** von [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough)herunterladen.

U-SQL, geöffneten Visual Studio ausführen klicken Sie auf **Datei -> Neu--> Projekt**, **U-SQL Project**Name und in einem Ordner auswählen.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] Es ist möglich, Azure-Portal mit U-SQL anstelle von Visual Studio ausführen. Sie können in Azure Data Lake Analytics Ressource im Portal navigieren und Anfragen direkt wie in der folgenden Abbildung dargestellt.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Erfassung der Daten: Daten aus öffentlichen Blob lesen

Der Speicherort der Daten in Azure Blob wird als **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** mit **Extractors.Csv()**extrahiert werden. Ersetzen Sie Ihre eigenen Container und Storage-Kontonamens in folgenden Skripts für container_name@blob_storage_account_name Wasb-Adresse. Da Dateinamen im gleichen Format sind, können wir **Reise\_Data_ {\*\}CSV** 12 Reise Dateien lesen. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Da in der ersten Zeile Header vorhanden sind, müssen wir die Header entfernen und Spaltentypen in passend. Wir können entweder die verarbeiteten Daten Azure See Datenspeicher mit **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ oder Azure BLOB-Speicher mit Konto speichern **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Ebenso können wir Fahrpreis Datensätze lesen. Klicken Sie mit der rechten Maustaste Azure See Datenspeicher, können Ihre Daten in **Azure-Portal: Daten-Explorer** oder **Datei-Explorer** in Visual Studio. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Daten-Qualitätskontrolle

Nachdem Tabellen Reise und Fahrpreis in gelesen wurden, Qualitätskontrollen Daten auf folgende Weise erfolgt. Die CSV-Dateien können in Azure BLOB-Speicher oder Azure See Datenspeicher ausgegeben werden. 

Finden Sie die Anzahl der Medaillen und eindeutige Nummer Medaillen:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Suchen Sie die Medaillen, die mehr als 100 Besuche:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Suchen Sie ungültige Datensätze in Pickup_longitude:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Suchen Sie fehlende Werte für einige Variablen:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Durchsuchen von Daten

Um ein besseres Verständnis der Daten einige Durchsuchen von Daten möglich.

Suchen Sie die Verteilung der Spitzen und nicht gekippt:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

Die Verteilung der Spitze Betrag Ausschlusswerten finden: 0,5,10 und 20 Euro.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Suchen Sie grundlegende statistische Angaben Wegstrecke:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Suchen Sie Perzentile Wegstrecke:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Reise und Tarif Tabellen verknüpfen

Reise und Tarif Tabellen können medaillon, Hack_license und Pickup_time verknüpft werden.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Berechnen Sie für jede Anzahl der Fahrgäste die Anzahl der Datensätze durchschnittliche Spitze Betrag Abweichung Tipp Menge, Prozentsatz Geneigter Reisen.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Datenstichproben

Zuerst wählen wir zufällig 0,1 % der Daten aus der verknüpften Tabelle:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Dann führen wir geschichtete Stichprobe von binären Variable Datenlecks:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>U-SQL-Aufträgen

Bei Bearbeitung U-SQL-Skripts können Sie sie über Ihr Konto Azure Data Lake Analytics Server senden. Wählen Sie **Daten dem** **Auftrag senden**Ihre **Analytics-Konto**wählen Sie **Parallelität aus**und klicken Sie auf **Absenden** .  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Wenn der Auftrag erfolgreich erfüllt, der Status des Projekts in Visual Studio für die Überwachung erscheint. Nachdem der Auftrag beendet ist, können Sie auch wiederholen den Ausführungsprozess und Engpass Schritte ausführen, um Ihre Arbeit effizienter finden. Sie können auch überprüfen Sie den Status Ihrer Aufträge U SQL Azure-Portal wechseln.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


Jetzt können Sie die Ausgabedateien in Azure BLOB-Speicher oder Azure-Portal überprüfen. Wir verwenden die geschichteten Beispieldaten für unsere Modellierung im nächsten Schritt.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Erstellen und Bereitstellen von Modellen in Azure maschinelles lernen

Wir zeigen zwei Optionen für Sie mit Daten in Azure Computer erstellen und 

- In der ersten Option verwenden die erfassten Daten in einer Azure Blob (im **Datenstichproben** Schritt oben) geschrieben und Python erstellen und Bereitstellen von Azure Machine Learning Modelle verwenden. 
- Zweite Option Abfrage Daten in Azure Data Lake direkt mit einer Struktur Abfrage. Diese Option erfordert, dass Sie einen neuen HDInsight-Cluster erstellen oder einen vorhandenen Cluster HDInsight, NY Taxi Daten in Azure See Datenspeicher Struktur Tabellen zeigen.  Diese beiden Optionen unten besprochen. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Option 1: Verwenden von Python zu erstellen und bereitzustellen Computer lernmodelle

Zum Erstellen und Bereitstellen von Learning Modelle mit Python, erstellen Sie Jupyter Notebook auf dem lokalen Computer oder in Azure Machine Learning Studio. [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) zur Jupyter-Notebook enthält den vollständigen Code untersuchen, Visualisieren von Daten, Feature Engineering, Modellierung und Bereitstellung. In diesem Artikel zeigen wir einfach die Modellierung und Bereitstellung. 

### <a name="import-python-libraries"></a>Python-Importbibliotheken

Um das Beispiel auszuführen Skriptdatei Jupyter Notebook oder Python, die folgenden Pakete benötigt werden Python. Bei Verwendung den AzureML Notebook-Service haben diese Pakete vorinstalliert.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Die Daten aus dem Blob gelesen

- Verbindungszeichenfolge   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- Als Text lesen

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Spaltennamen hinzufügen und separate Spalten

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Einige Spalten numerisch ändern

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Computer lernen Modelle

Hier erstellen wir eine binäre klassifizierungsmodell abschätzen, ob eine gekippt ist. Jupyter Notebook finden Sie andere zwei Modelle: multiklassenklassifizierung und regressionsmodelle.

- Zunächst müssen wir erstellen dummy Variablen können in Scikit-Modellen erfahren

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Rahmen Sie Daten für die Modellierung

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Schulung und 60-40 Teilen testen

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Logistische Regression Trainingsmenge

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Testdataset Ergebnis

        Y_test_pred = logit_fit.predict(X_test)

- Bewertung Metriken berechnen

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Webdienst-API erstellen und nutzen in Python

Wir möchten Computer Lernmodell nach ihrer Erstellung durchsetzen. Hier verwenden wir die Logistik Binärmodell als Beispiel. Stellen Sie sicher, dass der Scikit-Informationen auf dem lokalen Computer ist 0.15.1. Sie müssen nicht kümmern, wenn Sie Azure ML Studio nutzen.

- Finden Sie Ihre Anmeldeinformationen Workspace von Azure ML Studio Settings. **Klicken Sie in Azure Machine Learning Studio** --> **Name** --> **Autorisierungstoken**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Webdienst erstellen

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Web Anmeldeinformationen abrufen

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Web Service-API aufrufen. Sie haben 5 bis 10 Sekunden nach dem vorherigen Schritt.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Option 2: Erstellen und Bereitstellen von Modellen direkt in Azure maschinelles lernen

Azure Machine Learning Studio können Daten direkt von Azure See Datenspeicher lesen und erstellen und Bereitstellen von Modellen verwendet werden. Dieser Ansatz eine Struktur Tabelle verwendet, die im Datenspeicher See Azure zeigt. Dies erfordert, dass ein separater Azure HDInsight Cluster bereitgestellt werden, auf die Tabelle Struktur erstellt wird. Die folgenden Abschnitte zeigen dazu. 

### <a name="create-an-hdinsight-linux-cluster"></a>Erstellen Sie ein HDInsight Linux-Cluster

Erstellen Sie HDInsight-Cluster (Linux) aus dem [Azure-Portal](http://portal.azure.com). Weitere Informationen finden Sie im Abschnitt **HDInsight-Cluster mit dem Datenspeicher Azure** [Erstellen HDInsight-Cluster mit dem Datenspeicher](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)mit Azure-Portal.

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Struktur-Tabelle in HDInsight erstellen

Jetzt erstellen wir Hive-Tabellen in Azure Machine Learning Studio im HDInsight Cluster mit Daten in Azure See Datenspeicher im vorherigen Schritt verwendet werden. HDInsight Cluster einfach erstellt werden. **Klicken Sie** --> **Eigenschaften** --> **Cluster AAD Identität** --> **ADL Zugriff**sicher, dass Ihr Konto Azure See Datenspeicher wird in der Liste mit Schreiben und Ausführungsrechte. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


**Dashboard** neben **der Schaltfläche** klicken und ein Fenster wird angezeigt. Klicken Sie auf **Struktur anzeigen** in der oberen rechten Ecke der Seite und Sie sehen den **Abfrage-Editor**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


Fügen Sie in der folgenden Struktur Skripts erstellen. Die Datenquelle ist in Azure See Datenspeicher auf diese Weise: **Adl://data_lake_store_name.azuredatalakestore.net:443/Ordnername Dateiname**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Wenn die Abfrage abgeschlossen ist, sehen Sie die Ergebnisse wie folgt:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Erstellen und Bereitstellen von Modellen in Azure Machine Learning Studio

Wir können jetzt erstellen und Bereitstellen eines Modells das voraussagt, ob ein Azure Computer lernen Trinkgeld. Geschichtete Beispieldaten werden diese binären Klassifizierung verwendet werden (oder Kante) Problem. Vorhersagemodelle multiklassenklassifizierung (Datenlecks) mit Regression (Tip_amount) erstellt und können mit Azure Machine Learning Studio bereitgestellt, aber hier zeigen wir nur Behandlung binäre klassifizierungsmodell verwenden.

1. Rufen Sie Daten in Azure ML mit dem **Datenimport** -Modul im Abschnitt **Eingabe und Ausgabe ab** . Weitere Informationen finden Sie auf der Referenzseite [Importdaten Modul](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) .
2. Auswahlabfrage **Struktur** als **Datenquelle** im Bedienfeld " **Eigenschaften** ".
3. Fügen Sie das folgende Skript Struktur in **Hive Abfrage** -editor

        select * from nyc_stratified_sample;

4. Geben Sie URI des HDInsight-Cluster (Dies kann in Azure-Portal gefunden), Hadoop Anmeldeinformationen, Speicherort der Daten und Azure Storage Name/Schlüsselcontainer Kontonamen ein

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Ein Beispiel eines binären Klassifizierung Experiments Lesen von Daten aus Tabelle Struktur ist in der folgenden Abbildung.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Klicken Sie nach dem Erstellen des Experiments auf **Web-Service** --> **Vorhersehbaren Webdienst**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Ausführen automatisch erstellten Experiment bewerten, wenn es abgeschlossen ist, klicken Sie auf **Webdienst bereitstellen**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Das Web Service-Dashboard wird kurz angezeigt:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Zusammenfassung

Nach Abschluss dieser exemplarischen Vorgehensweise haben Sie eine Umgebung zum Erstellen von skalierbarer End-to-End-Lösungen in Azure Data Lake Wissenschaft erstellt. Diese Umgebung wurde verwendet, um große öffentliche Dataset kanonische Schritte des wissenschaftlichen Daten von Datenerfassung durch Modell Schulung und Bereitstellung des Modells als Webdienst unter analysieren. U-SQL wurde zu verarbeiten, die Beispieldaten verwendet. Python und Struktur wurden mit Azure Machine Learning Studio erstellen und Bereitstellen der Vorhersagemodelle verwendet.

## <a name="whats-next"></a>Was kommt als nächstes?

Learning Path für das [Team Daten Wissenschaft Prozess (TDSP)](http://aka.ms/datascienceprocess) enthält Links zu Themen, in denen jeder Schritt Erweiterte Analyse. Es gibt eine Reihe von exemplarischen Vorgehensweisen auf der Seite [Team Daten Wissenschaft Exemplarische Vorgehensweisen](data-science-process-walkthroughs.md) aufgeführt, die Verwendung von Ressourcen und Services in Szenarien Vorhersageanalysen präsentieren:

- [Team wissenschaftliche Daten in Aktion: mit SQL Data Warehouse](machine-learning-data-science-process-sqldw-walkthrough.md)
- [Team wissenschaftliche Daten in Aktion: HDInsight Hadoop Cluster](machine-learning-data-science-process-hive-walkthrough.md)
- [Das Team wissenschaftliche Daten: mithilfe von SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
- [Übersicht über Azure HDInsight Spark über Wissenschaft von Daten](machine-learning-data-science-spark-overview.md)

