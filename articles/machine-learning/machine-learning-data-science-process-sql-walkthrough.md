<properties
    pageTitle="Team wissenschaftliche Daten in Aktion: mit SQL Server | Microsoft Azure"
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
    ms.date="09/19/2016"
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Team wissenschaftliche Daten in Aktion: mit SQL Server

In diesem Lernprogramm Sie exemplarischen Vorgehensweise erstellen und Bereitstellen einer Maschine Lernmodell mit SQL Server und einem öffentlich zugänglichen Dataset - [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) Dataset. Der Workflow wissenschaftliche Daten erfolgt: Aufnahme und Durchsuchen der Daten Engineering Features Lernen erleichtern, erstellen und Bereitstellen eines Modells.


## <a name="dataset"></a>NYC Taxi Reisen Datasetbeschreibung

NYC Taxi Reise werden etwa 20GB komprimierte CSV-Dateien (~ 48GB nicht komprimiert), 173 Millionen einzelnen Schleifen und die Preise aus jeder Reise bezahlt. Jeder Reise Datensatz enthält die Abholung und Rückgabe an, anonymisierten Hack (Treiber) Lizenznummer und Uhrzeit Medaille (Taxi eindeutige Id). Daten ist umfasst alle Fahrten im Jahr 2013 und in die folgenden zwei Datasets für jeden Monat:

1. Trip_data CSV enthält REISEDETAILS wie Anzahl der Fahrgäste, Abholung und Drop-Off Punkt Fahrtdauer und Reisedauer. Hier sind einige Beispieldatensätze:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 'Trip_fare' enthält CSV Details für jede Reise wie Zahlungstyp, Fahrpreis Betrag Zuschlag und steuern, Tipps und Mautgebühren gezahlten Flugpreis und den Gesamtbetrag. Hier sind einige Beispieldatensätze:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Der eindeutige Schlüssel auf Reise\_Daten und Reise\_Tarif besteht aus den Feldern: Medaille Hack\_Lizenz und Abholung\_Datetime.

## <a name="mltasks"></a>Beispiele für Vorhersage Aufgaben

Wir formulieren drei Vorhersage Probleme anhand der *Tipp\_Betrag*, nämlich:

1. Binäre Klassifizierung: Vorhersagen, ob eine für eine Fahrt also Trinkgeld wurde eine *Tipp\_Betrag* größer als 0 Beispiel ist zwar ein *Tipp\_Betrag* $0 ist eine negative.

2. Multiklassenklassifizierung: den Bereich Trinkgeld für die Reise vorhergesagt. Wir teilen die *Tipp\_Betrag* in fünf Lagerplätze oder Klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Regressionsaufgabe: Menge Trinkgeld für eine Reise vorhergesagt.  


## <a name="setup"></a>Einstellung der Azure Data Science Umgebung für erweiterte Analyse

Siehe Handbuch [Ihrer Umgebung planen](machine-learning-data-science-plan-your-environment.md) können, sind mehrere Optionen NYC Taxi Trips Dataset in Azure arbeiten:

- Arbeiten Sie mit Daten in Azure-Blobs in Azure Machine Learning-Modell
- Laden Sie die Daten in einer SQL Server-Datenbank und Modell in Azure maschinelles lernen

In diesem Tutorial zeigen wir parallel Massenimport von Daten einer SQL Server-Daten durchsuchen, Funktion engineering unten Sampling mithilfe von SQL Server Management Studio sowie IPython Notebook verwenden. [Beispielskripts](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) und [IPython Notebooks](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) werden in GitHub verwendet. Ein Beispiel IPython Notebook arbeiten mit Daten in Azure-Blobs kostenfrei an derselben Stelle.

So richten Ihre Azure Data Science-Umgebung

1. [Ein Speicherkonto erstellen](../storage/storage-create-storage-account.md)

2. [Einen Azure Machine Learning-Arbeitsbereich erstellen](machine-learning-create-workspace.md)

3. [Bereitstellung einer Science-VM](machine-learning-data-science-setup-sql-server-virtual-machine.md), die als auch ein SQL Server-IPython Notebook-Server dient.

    > [AZURE.NOTE] Beispielskripts und IPython Notebooks werden Ihre Datenwissenschaft virtuellen Computer während der Installation heruntergeladen werden. Abschluss der Installationsskript VM werden Proben in der VM-Dokumentbibliothek:  
    > - Skripts zum Beispiel:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Beispiel IPython Notebooks:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > wo `<user_name>` der VM Windows Benutzername. Wir verweisen auf den Beispielordnern **Beispielskripts** und **Beispiel IPython Notebooks**.


Basierend auf die Größe des Datasets, Speicherort der Datenquelle und die ausgewählte(n) Azure-Umgebung dieses Szenario ähnelt [Szenario \#5: großes Dataset in lokalen Dateien Ziel SQL Server in Azure VM](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Die Daten aus öffentlichen Quelle abrufen

Um [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) Dataset aus ihrem öffentlichen Speicherort, können [Verschieben Sie Daten zwischen Azure BLOB-Speicher](machine-learning-data-science-move-azure-blob.md) beschriebenen Methoden Sie die Daten auf den neuen virtuellen Computer kopieren.

Kopieren Sie die Daten mit AzCopy

1. Melden Sie sich mit Ihrer virtuellen Maschine (VM)

2. Erstellen Sie ein neues Verzeichnis in der VM-Datenträger (Hinweis: Verwenden Sie nicht die temporäre Diskette mit VM als Datenträger).

3. Führen Sie in einem Eingabeaufforderungsfenster die folgenden Azcopy Zeile ersetzen < Path_to_data_folder > mit Ihrem Datenordner erstellt (2):

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Nach Abschluss der AzCopy ZIP insgesamt 24 CSV-Dateien (12 Reise\_Daten und 12 Reise\_Tarif) sollte im Ordner.

4. Extrahieren Sie die heruntergeladenen Dateien. Notieren Sie den Ordner, in denen die dekomprimierte Dateien befinden. Dieser Ordner wird nachstehend das < Pfad\_,\_Daten\_Dateien\>.

## <a name="dbload"></a>Massendaten in SQL Server-Datenbank importieren

Mit _partitionierten Tabellen und Ansichten_kann die Leistung beim Laden/übertragen großer Mengen von Daten in einer SQL-Datenbank und nachfolgender Abfragen verbessert werden. In diesem Abschnitt folgen wir Anweisungen in [Parallele Bulk Import verwenden SQL Partition Datentabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) eine neue Datenbank erstellen und die Daten in partitionierten Tabellen gleichzeitig geladen.

1. Während die VM angemeldet, starten Sie **SQL Server Management Studio**.

2. Verbindung mit Windows-Authentifizierung.

    ![SSMS verbinden][12]

3. Wenn Sie noch nicht den SQL Server-Authentifizierungsmodus geändert und erstellt einen neuen SQL Login Benutzer, öffnen die Skriptdatei namens **Ändern\_auth.sql** in den Ordner **Skripts** . Ändern Sie den Standardbenutzernamen und das Kennwort. Klicken Sie auf **. Führen Sie** in der Symbolleiste, um das Skript auszuführen.

    ![Skript ausführen][13]

4. Überprüfen Sie oder ändern Sie der SQL Server-Datenbank und die Protokolldateien Standardordner sicherstellen, die neu erstellten Datenbanken auf einem Datenträger. SQL Server VM-Image auf die Lasten Datawarehousing ist mit Daten- und Festplatten. VM enthielt einen Datenträger und neue virtuelle Festplatten während des Installationsvorgangs VM hinzugefügt ändern Sie Standardordner wie folgt:

    - Der SQL Server-Namen im linken Bereich, und klicken Sie auf **Eigenschaften**.

        ![SQL Server-Eigenschaften][14]

    - Einstellungen Sie **Datenbank** aus der Liste **Wählen Sie eine Seite** nach links.

    - Überprüfen Sie und/oder ändern Sie die **Standardspeicherorte für Datenbank** zu den **Datenträger** Ihrer Wahl. Dies ist, wo neue Datenbanken befinden, mit den Standardeinstellungen Speicherort erstellt.

        ![Standardwerte für SQL][15]  

5. Öffnen Sie zum Erstellen einer neuen Datenbank und einen Satz von Dateigruppen die partitionierten Tabellen enthalten das Beispielskript **Erstellen\_Db\_default.sql**. Das Skript erstellt eine neue Datenbank mit dem Namen **TaxiNYC** und 12 Dateigruppen am Speicherort für Daten. Jede Dateigruppe enthalten einen Monat Reise\_Daten und Reise\_Daten gehen. Ändern Sie bei Bedarf den Datenbanknamen. Klicken Sie auf **. Führen Sie** zum Ausführen des Skripts.

6. Als Nächstes erstellen Sie zwei Partitionstabellen, für die Reise\_und eine andere für die Reise\_Tarif. Öffnen Sie das Beispielskript **Erstellen\_partitionierten\_speichern**, die:

    - Erstellen einer Partitionsfunktion, um die Daten nach Monaten unterteilt.
    - Erstellen Sie ein Partitionsschema jeden Monat eine andere Dateigruppe zugeordnet.
    - Zwei partitionierten Tabellen Partitionsschema zugeordnet: **Nyctaxi\_Reise** halten die Reise\_Daten und **Nyctaxi\_Tarif** halten die Reise\_Daten gehen.

    Klicken Sie auf **. Führen Sie** führen Sie das Skript und die partitionierten Tabellen erstellen.

7. Im Ordner **Skripts** sind zwei PowerShell Beispielskripts zeigen, parallele Massenkopieren von Daten in SQL Server-Tabellen importiert.

    - **Bcp\_parallel\_generic.ps1** ist ein allgemeines Skript parallel Bulk Import Daten in einer Tabelle. Ändern Sie dieses Skript die Eingabe- und Ziel Variablen festlegen, wie in den Kommentarzeilen im Skript.
    - **Bcp\_parallel\_nyctaxi.ps1** ist eine vorkonfigurierte Version des generischen Skripts und wird auf beide Tabellen NYC Taxi Trips Daten laden.  

8. Mit der rechten Maustaste die **Bcp\_parallel\_nyctaxi.ps1** script Name, und klicken Sie auf **Bearbeiten** , um es in PowerShell öffnen. Die vordefinierten Variablen überprüfen und ändern Sie nach der Name der ausgewählten Datenbank, Eingabedaten Ordner Zielordner Protokoll und Pfade zum Beispiel Format Dateien **nyctaxi_trip.xml** und **Nyctaxi\_fare.xml** (im Ordner **Skripts** bereitgestellt).

    ![Importieren von Massendaten][16]

    Sie können auch den Authentifizierungsmodus auswählen, standardmäßig Windows-Authentifizierung. Klicken Sie auf den grünen Pfeil in der Symbolleiste auf ausführen. Das Skript startet 24 Massenimportvorgänge parallel, 12 für jede partitionierte Tabelle. Öffnen Sie den SQL Server-Daten Standardordner oben können Sie Daten importieren Fortschritt überwachen.

9. PowerShell-Skript meldet die Start- und Endzeiten. Wenn alle vollständigen Imports Massenkopieren wird die Endzeit gemeldet. Kontrollkästchen Protokoll Zielordner zu überprüfen, ob das gleichzeitige importiert waren erfolgreich, d. h. keine Fehler im Protokoll Zielordner.

10. Die Datenbank kann jetzt Untersuchung Featureentwicklung und andere Vorgänge wie gewünscht. Da nach die Tabellen partitioniert werden der **Abholung\_Datetime** Feld Abfragen sollen **Abholung\_Datetime** in einer **WHERE** -Klausel auf das Partitionsschema profitieren.

11. Untersuchen Sie in **SQL Server Management Studio**bereitgestellten Skript **Beispiel\_queries.sql**. Führen Sie eine der Beispielabfragen markieren Sie Abfragezeilen klicken Sie **! Führen Sie** in der Symbolleiste.

12. NYC Taxi Trips-Daten werden in zwei separaten Tabellen geladen. Um Join-Operationen zu verbessern, wird empfohlen, die Tabellen. Das Beispielskript **Erstellen\_partitionierten\_index.sql** partitionierten Indizes auf zusammengesetzte Verbindungsschlüssel erstellt **Medaille Hack\_Lizenz und Abholung\_Datetime**.

## <a name="dbexplore"></a>Durchsuchen von Daten und Engineering Feature in SQL Server

In diesem Abschnitt führen Durchsuchen von Daten und Funktion Generation wir durch Ausführen von SQL-Abfragen direkt in der zuvor erstellten SQL Server-Datenbank mit **SQL Server Management Studio** . Ein Beispielskript namens **Beispiel\_queries.sql** im Ordner **Skripts** bereitgestellt. Das Skript den Datenbanknamen ändern ändern, wenn von der Standardeinstellung abweicht: **TaxiNYC**.

In dieser Übung werden wir:

- Verbinden Sie mit **SQL Server Management Studio** entweder Windows-Authentifizierung oder SQL-Authentifizierung und SQL-Benutzername und Kennwort.
- Durchsuchen Sie Daten-Distributionen einige Felder in unterschiedlichen Zeitfenster.
- Überprüfen Sie Datenqualität Längen- und Felder.
- Binäre und multiclass Klassifizierung Etiketten auf der Grundlage der **Tipp\_Betrag**.
- Generieren von Funktionen und Compute/vergleichen Reise Abstände.
- Verknüpfen der beiden Tabellen und Extrahieren einer Stichprobe, die zum Erstellen von Modellen verwendet werden.

Wenn Sie Azure Machine Learning fortfahren möchten, können Sie:  

1. Speichern Sie die letzte SQL-Abfrage zum Extrahieren der Daten und kopieren die Abfrage direkt in [Daten importieren] [ import-data] Modul in Azure Computerlernen oder
2. Beibehalten der Daten im aufgenommenen und Engineering für Modell in eine neue Datenbanktabelle erstellen und verwenden die neue Tabelle [Daten importieren] soll[ import-data] Modul in Azure maschinelles lernen.

In diesem Abschnitt wird die letzte Abfrage zu extrahieren der Daten gespeichert. Die zweite Methode zeigt im Abschnitt [Durchsuchen von Daten und Funktion in IPython Notebook](#ipnb) .

Zur schnellen Überprüfung der Anzahl der Zeilen und Spalten in den Tabellen oben Massenimport parallel aufgefüllt

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Durchsuchen: Reise Verteilung von medaillon

In diesem Beispiel wird der Medaille (Taxi Zahlen) mit mehr als 100 innerhalb eines bestimmten Zeitraums. Die Abfrage profitiert von der partitionierten Tabellenzugriff seit dem Partitionsschema der bedingt **Abholung\_Datetime**. Das vollständige Dataset Abfragen wird der partitionierten Tabelle verwenden bzw. index Scan machen.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Durchsuchen: Reise Verteilung von medaillon und hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Beurteilung Daten: Überprüfen Sie, ob Datensätze mit falscher Länge oder Breite

In diesem Beispiel wird untersucht, wenn keines der Felder Länge oder Breite entweder einen ungültigen Wert enthalten (Radiant Grad entsprechen zwischen-90 und 90) oder (0, 0) Koordinaten.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Durchsuchen: Tipped im Vergleich zu nicht gekippt Trips Verteilung

Dieses Beispiel ermittelt die Anzahl der Schleifen oder nicht in einem bestimmten Zeitraum gekippt gekippt wurden, Periode (oder vollständigen Dataset, wenn das Jahr). Diese Verteilung entspricht die binäre Bezeichnung Verteilung später für binäre Klassifizierung Modellierung verwendet werden.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Untersuchung: Tipp Bereich Klasse Verteilung

In diesem Beispiel berechnet die Verteilung der Tipp Bereiche in einem bestimmten Zeitraum (oder im vollständigen Dataset, wenn das Jahr). Dies ist die Verteilung der Label-Klassen, die später zur Modellierung von multiklassenklassifizierung verwendet werden.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Untersuchung: Berechnen und Vergleichen von Wegstrecke

Dieses Beispiel konvertiert die Abholung und Ladengeschäft Länge und Breite zu SQL verweist, Fahrtstrecke SQL Geography Punkte Differenz berechnet und Stichprobe der Ergebnisse für den Vergleich zurückgegeben. Das Beispiel beschränkt die Ergebnisse gültig nur mit Qualität Bewertung Datenabfrage zuvor behandelten Koordinaten.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Feature Engineering in SQL-Abfragen

Die Bezeichnung Generierung und Geografie Konvertierung Untersuchung Abfragen können auch Etiketten-Funktionen durch Entfernen der Inventur generiert verwendet werden. Zusätzliche Funktion engineering SQL im Abschnitt [Durchsuchen von Daten und Funktion in IPython Notebook](#ipnb) Beispiele. Es ist effizienter, die Funktion Abfragen Generation voll Dataset oder eine große Teilmenge mit SQL-Abfragen direkt in der SQL Server-Datenbankinstanz ausgeführt. Abfragen können in **SQL Server Management Studio**, IPython Notebook oder jeder Entwicklung Tool/die Datenbank, lokal oder Remote zugreifen können ausgeführt werden.

#### <a name="preparing-data-for-model-building"></a>Vorbereiten von Daten für Gebäude

Die folgende Abfrage Joins der **Nyctaxi\_Reise** und **Nyctaxi\_Tarif** Tabellen, generiert eine binäre Klassifizierung Bezeichnung **gekippt**, Multi-Klasse Typenschild **Tipp\_Klasse**, und eine Stichprobe von %1 aus dem vollständigen verknüpfte Dataset. Diese Abfrage kann kopiert und eingefügt in [Azure Machine Learning Studio](https://studio.azureml.net) [Daten importieren] [ import-data] Modul für direkte Erfassung von SQL Server-Datenbankinstanz in Azure. Die Abfrage schließt mit falsch (0, 0) Koordinaten.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Durchsuchen von Daten und Featureentwicklung IPython Notebook

In diesem Abschnitt führen wir das Durchsuchen von Daten und Funktion Generieren von Python und SQL-Abfragen in der zuvor erstellten SQL Server-Datenbank. Ein Beispiel IPython Notizbuch mit dem Namen **machine-Learning-data-science-process-sql-story.ipynb** wird im Ordner **IPython Beispielnotizbücher** bereitgestellt. Dieses Notizbuch ist auch auf [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks)verfügbar.

Der empfohlene Ablauf beim Arbeiten mit großen Daten lautet wie folgt:

- Lesen der Daten in einem kleinen in einen Rahmen mit Daten im Speicher.
- Führen Sie einige Visualisierung und aufgenommenen Daten kennen.
- Experimentieren Sie mit Feature mithilfe der gesammelten Daten.
- Verwenden Sie Durchsuchen von mehr Daten, Datenmanipulation Featureentwicklung und Python, SQL-Abfragen direkt mit der SQL Server-Datenbank in der Azure-VM.
- Entscheiden Sie die Stichprobengröße Azure Machine Learning Gebäude mit.

Wenn Sie fertig sind, Azure maschinelles lernen, können Sie:  

1. Speichern Sie die letzte SQL-Abfrage zum Extrahieren der Daten und kopieren die Abfrage direkt in [Daten importieren] [ import-data] Modul in Azure maschinelles lernen. Diese Methode ist [In Azure Machine Learning Modelle](#mlmodel) demonstriert.    
2. Beibehalten der Daten im aufgenommenen und Engineering für Modell in eine neue Datenbanktabelle erstellen soll und anschließend die neue Tabelle [Daten importieren] [ import-data] Modul.

Es folgen einige Durchsuchen von Daten, Visualisierung und Beispiele engineering-Funktion. Weitere Beispiele finden Sie unter Beispiel SQL IPython Notebook im Ordner **Beispielnotizbücher IPython** .

#### <a name="initialize-database-credentials"></a>Initialisieren von Anmeldeinformationen

Initialisieren Sie die Datenbankverbindungseinstellungen in die folgenden Variablen:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Verbindung erstellen

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Bericht Anzahl von Zeilen und Spalten in der Tabelle nyctaxi_trip

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Gesamtanzahl der Zeilen = 173179759  
- Gesamtanzahl der Spalten = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Lesen in einem kleinen Beispiel aus der SQL Server-Datenbank

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Zeit zum Lesen die Beispieltabelle 6,492000 Sekunden  
Die Anzahl der Zeilen und Spalten abgerufen = (84952, 21)

#### <a name="descriptive-statistics"></a>Beschreibende Statistik

Nun können die gesammelten Daten zu. Wir beginnen mit aussagekräftigen Statistiken für die **Reise\_Abstand** (oder anderen) Feldern:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>-Visualisierung: Beispiel für Plot

Als Nächstes stellen wir Boxplot Reise Abstand die Quantile visualisieren

    df1.boxplot(column='trip_distance',return_type='dict')

![Zeichnen Sie #1][1]

#### <a name="visualization-distribution-plot-example"></a>Visualisierung: Verteilung Plot-Beispiel

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![#2 zeichnen][2]

#### <a name="visualization-bar-and-line-plots"></a>-Visualisierung: Balken und Liniendiagramme

In diesem Beispiel werden bin Reise Abstand in fünf Lagerplätze und Visualisieren binning Ergebnisse.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Wir zeichnen oben Bin Verteilung in Bar oder Fläche unten ausrichten

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 zeichnen][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 zeichnen][4]

#### <a name="visualization-scatterplot-example"></a>Visualisierung: Streudiagramm Beispiel

Wir zeigen Punktdiagramm zwischen **Reise\_Zeit\_in\_Sekunden** und **Reise\_Abstand** zu sehen, ob eine Korrelation

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 zeichnen][6]

Entsprechend finden wir die Beziehung zwischen **Rate\_Code** und **Reise\_Abstand**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 zeichnen][8]

### <a name="sub-sampling-the-data-in-sql"></a>Die Daten in SQL Sub-Sampling

Beim Vorbereiten für in [Azure Machine Learning Studio](https://studio.azureml.net)erstellen Datenmodell, Sie können **SQL Abfrage direkt im Modul "Daten importieren"** entscheiden oder beibehalten entwickelten und aufgenommenen Daten in eine neue Tabelle in den [Datenimport] verwenden können[ import-data] mit einer einfachen * *auswählen* von < Ihre\_neue\_Tabelle\_Name >**.

In diesem Abschnitt wird eine neue Tabelle der aufgenommenen und technische Daten erstellt. Ein Beispiel für eine direkte SQL-Abfrage für Gebäude gemäß Abschnitt [Durchsuchen von Daten und Funktion in SQL Server](#dbexplore) .

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Erstellen eines Beispiels Tabelle und 1 % der verknüpften Tabellen füllen. Löschen Sie erste Tabelle, sofern vorhanden.

In diesem Abschnitt wir Tabellen verknüpfen, **Nyctaxi\_Reise** und **Nyctaxi\_Tarif**extrahieren eine Stichprobe von 1 % und beibehalten die gesammelten Daten in eine neue Tabelle namens **Nyctaxi\_eine\_%**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Durchsuchen von Daten mithilfe von SQL-Abfragen in IPython Notebook

In diesem Abschnitt untersuchen wir Daten Distributionen mit 1 % aufgenommenen Daten in die neue Tabelle weiter oben erstellten beibehalten werden. Ähnliche Untersuchungen durchgeführt werden können die ursprünglichen Tabellen, optional mit **TABLESAMPLE** beschränken die Erforschung Probe oder durch Beschränkung der Ergebnisse auf einen bestimmten Zeitraum mithilfe der **Abholung\_Datetime** Partitionen wie in Abschnitt [Durchsuchen von Daten und Funktion in SQL Server](#dbexplore) .

#### <a name="exploration-daily-distribution-of-trips"></a>Durchsuchen: Tägliche Verteilung Reisen

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Durchsuchen: Reise Verteilung pro Medaille

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Funktion Generieren von SQL-Abfragen in IPython Notebook

In diesem Abschnitt wir neue Etiketten erstellen und Funktionen direkt mit SQL-Abfragen auf 1 % Beispieltabelle wir im vorherigen Abschnitt.

#### <a name="label-generation-generate-class-labels"></a>Label-Generation: Klassenbezeichner generieren

Im folgenden Beispiel erstellen wir zwei Sätze von Etiketten für die Modellierung verwendet:

1. Binäre Klassenbezeichner **Geneigter** (Vorhersage einer QuickInfo zugeordnet werden)
2. Multiclass Etiketten **Tipp\_Klasse** (Vorhersage Tipp Bin oder Bereich)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Featureentwicklung: Anzahl Funktionen für kategorische Spalten

In diesem Beispiel wird ein numerisches Feld ersetzt jede Kategorie die Anzahl der Vorkommen in der Kategorieliste Feld verwandelt.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Featureentwicklung: Bin Funktionen für numerische Spalten

In diesem Beispiel verwandelt vordefinierte Kategorie Bereiche, z. B. Transformation numerisches Feld in einer kategorisierten Feld ein kontinuierliches numerisches Feld.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Featureentwicklung: Decimal Breiten-und Längengrad extrahieren Sie Features Speicherort

In diesem Beispiel bricht die dezimale Darstellung eines Felds Breiten- und Längengrad in mehrere Region Felder unterschiedliche Granularität, Land, Stadt, Stadt, Block usw.. Beachten Sie, dass die neuen Geo-Felder nicht tatsächlichen Speicherorte zugeordnet sind. Informationen zum Mapping Geocode Speicherorte anzeigen Sie [Bing Maps REST-Dienste](https://msdn.microsoft.com/library/ff701710.aspx)

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Überprüfen Sie die endgültige Form der Featurized-Tabelle

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Wir sind jetzt Model und Modell Bereitstellung in [Azure Machine Learning](https://studio.azureml.net)fortfahren. Die Daten sind bereit für die Vorhersage Probleme identifiziert, nämlich:

1. Binäre Klassifizierung: vorherzusagen, ob ein Tipp bezahlt wurde eine.

2. Multiklassenklassifizierung: Bereich Trinkgeld bereits definierte Klassen vorherzusagen.

3. Regressionsaufgabe: Menge Trinkgeld für eine Reise vorhergesagt.  


## <a name="mlmodel"></a>Modelle in Azure maschinelles lernen

Um die modellierungsübung beginnen, melden Sie sich den Azure Machine Learning-Arbeitsbereich. Wenn einen Computer lernen Arbeitsbereich noch nicht erstellt haben, finden Sie unter [Erstellen einer Azure Machine Learning-Arbeitsbereich](machine-learning-create-workspace.md).

1. Zunächst mit Azure Computer finden Sie unter [Neuigkeiten Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

2. Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net).

3. Studio-Startseite bietet eine Fülle von Informationen, Videos, Lernprogramme, Links Verweis Module und andere Ressourcen. Finden Sie weitere Informationen über Azure Machine Learning in [Azure Arbeitsplatzes Learning Dokumentation](https://azure.microsoft.com/documentation/services/machine-learning/).

Normale trainingsexperiment umfasst Folgendes:

1. Erstellen eines Experiments **+ neu** .
2. Daten zu Azure Machine Learning abrufen.
3. Vor der Verarbeitung, Transformieren und die Daten nach Bedarf ändern.
4. Generieren von Funktionen nach Bedarf.
5. Unterteilen Sie die Daten in Datasets Training, Überprüfung, testen (oder separate Datensätze für jeden).
6. Wählen Sie eine oder mehrere Algorithmen für maschinelles lernen je nach Learning zu. Z. B. binäre Klassifizierung, multiklassenklassifizierung, Regression.
7. Ein oder mehrere Modelle Trainings-Dataset zu trainieren.
8. Faktor Validierung Dataset mit qualifizierten Modelle.
9. Bewerten Sie Modelle relevanten Kennzahlen für das Lernen Problem berechnen.
10. Feinabstimmung der Modelle und wählen das geeignetste Modell bereitstellen.

In dieser Übung bereits untersucht und die Daten in SQL Server entwickelt, und beschlossen, die Größe der Stichprobe in Azure Machine Learning aufnehmen. Vorhersagemodelle build beschlossen wir:

1. Daten zu Azure Computerlernen mit den [Importierten Daten] [ import-data] Modul im Abschnitt **Eingabe und Ausgabe** . Weitere Informationen finden Sie unter [Daten importieren] [ import-data] Modul-Referenzseite.

    ![Daten importieren Azure maschinelles lernen][17]

2. **Azure SQL-Datenbank** als **Datenquelle** im Bedienfeld " **Eigenschaften** " wählen.

3. Geben Sie im Feld **Servername** der DNS-Datenbankname. Format:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Geben Sie den **Datenbanknamen** in das entsprechende Feld ein.

5. Geben Sie den **SQL-Benutzernamen** in die **Server Aqccount Benutzernamen und das Kennwort in die **Server Benutzer Konto Kennwort **.

6. Überprüfen Sie Option **akzeptieren alle Serverzertifikat** .

7. Im Bearbeitungsbereich Text **Datenbankabfrage** Proben einfügen Abfrage extrahiert die erforderlichen Datenbankfelder (einschließlich berechnete Felder wie Etiketten) und unten die Daten der gewünschten Stichprobengröße.

In der folgenden Abbildung ist ein Beispiel eines binären Klassifizierung Experiments Daten direkt aus der SQL Server-Datenbank zu lesen. Ähnliche Experimente können für multiklassenklassifizierung und Regressionsprobleme erstellt werden.

![Azure Machine Learning Zug][10]

> [AZURE.IMPORTANT] Die Modelldaten gemäß Extraktion und Beispiele für die Probenahme in vorherigen Abschnitten **Alle Etiketten für die drei modellierungsübungen in der Abfrage enthalten sind**. Ein (erforderlich) wichtiger aller modellierungsübungen ist **auszuschließen** unnötigen Etiketten für die beiden Probleme sowie alle anderen **Ziel verliert**. Z. B. bei binären Klassifizierung verwenden Bezeichnung **gekippt** und Felder ausschließen **Tipp\_Klasse**, **Tipp\_Betrag**, und **insgesamt\_Betrag**. Sind Ziel Verluste seit sie die Spitze implizieren bezahlt.
>
> Um nicht benötigte Spalten ausschließen oder Speicherverluste abzielen, können Sie die [Spalten im Dataset auswählen] [ select-columns] Modul oder [Metadaten bearbeiten][edit-metadata]. Weitere Informationen finden Sie unter [Spalten im Dataset auswählen] [ select-columns] und [Metadaten bearbeiten] [ edit-metadata] Seiten verweisen.

## <a name="mldeploy"></a>Bereitstellen von Modellen in Azure maschinelles lernen

Wenn das Modell fertig ist, können Sie problemlos als Webdienst direkt aus dem Experiment bereitstellen. Weitere Informationen zum Bereitstellen von Azure Machine Learning-Webdiensten finden Sie unter [Bereitstellen einer Azure Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).

Um einen neuen Webdienst bereitstellen, müssen Sie:

1. Erstellen Sie ein bewertungsexperiment.
2. Bereitstellen Sie den Web Service.

Klicken Sie ein bewertungsexperiment von einem Experiment **beendet** Training erstellen **Erstellen EXPERIMENTIEREN bewerten** in der unteren Leiste.

![Azure bewerten][18]

Azure Machine Learning versucht, ein bewertungsexperiment basierend auf den Komponenten des trainingsexperiments erstellen. Insbesondere ist:

1. Geschulte Modell speichern und Modell Schulungsmodulen entfernen.
2. Identifizieren Sie einen logischen **Eingabeanschluss** Schema erwarteten Daten darstellen.
3. Ermitteln Sie logische **Ausgabeanschluss** erwarteten Web Service-Ausgabeschema darstellen.

Erstellung Punktzahl Experiment überprüfen und nach Bedarf anpassen. Standard Einstellung ist mit der Bezeichnung Felder schließt Eingabedatasets bzw. Abfrage ersetzen, wie diese nicht verfügbar sind, wenn der Dienst aufgerufen wird. Es empfiehlt sich auch die Größe des Eingabedatasets bzw. Abfrage einige Datensätze für input Schema angeben. Für den Ausgang wird alle Eingabefelder und ausschließen nur **Etiketten bewertet** und **Erzielte Wahrscheinlichkeiten** in der Ausgabe der [Spalten im Dataset auswählen] [ select-columns] Modul.

In der folgenden Abbildung ist ein Beispiel bewerten Experiment. Bei Bereitstellung klicken Sie **Webdienst veröffentlichen** in der unteren Leiste.

![Veröffentlichen von Azure maschinelles lernen][11]

Zur Erinnerung in diesem Lernprogramm exemplarischen Vorgehensweise haben Sie Azure Data Science Umgebung arbeitete mit einem großen öffentlichen Dataset von Datenerfassung, Schulung und Bereitstellen eines Webdiensts Azure Machine Learning Modell erstellt.

### <a name="license-information"></a>Lizenzinformationen

Diese exemplarische Vorgehensweise Beispiel und die zugehörigen Skripts und IPython schreibhefte() werden von Microsoft MIT Lizenz freigegeben. Bitte checken Sie die Datei LICENSE.txt im Verzeichnis des Beispielcodes auf GitHub Weitere Informationen.

### <a name="references"></a>Referenzen

• [Andrés Monroy NYC Taxi Trips-Downloadseite](http://www.andresmh.com/nyctaxitrips/)  
• [Vereiteln NYC Taxi Reisedaten von Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi Limousine Kommission Forschung und Statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
