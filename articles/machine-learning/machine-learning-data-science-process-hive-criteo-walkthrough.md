<properties
    pageTitle="Team wissenschaftliche Daten in Aktion: mit HDInsight Hadoop 1 TB Criteo Dataset Cluster | Microsoft Azure"
    description="Team von wissenschaftlichen Daten verwenden für ein End-to-End-Szenario mit einem HDInsight Hadoop-Cluster erstellen und Bereitstellen eines Modells mit großen (1 TB) öffentlich dataset"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Team wissenschaftliche Daten in Aktion – mithilfe von Azure HDInsight Hadoop-Cluster auf einem 1-TB-dataset

In dieser exemplarischen Vorgehensweise zeigen wir Daten Wissenschaft Team in einem End-to-End-Szenario mit einer [Azure HDInsight Hadoop-Cluster](https://azure.microsoft.com/services/hdinsight/) zu speichern, Feature Engineering unten Beispieldaten eines öffentlich zugänglichen [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) Datasets. Wir verwenden Azure Machine Learning binäre klassifizierungsmodell für diese Daten erstellen. Wir zeigen eines dieser Modelle als Webdienst veröffentlichen.

Es kann auch ein IPython Notebook verwenden, um die Aufgaben in dieser exemplarischen Vorgehensweise vorgestellt. Benutzer möchten diese Vorgehensweise wenden die [Criteo eine Struktur ODBC-Verbindung](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) exemplarischen.


## <a name="dataset"></a>Criteo Dataset Beschreibung

Criteo ist ein Dataset auf Vorhersage, die etwa 370GB Gzip komprimierte TSV-Dateien (~1.3TB nicht komprimiert), bestehend aus 4.3 Mrd. Datensätze. Stammt aus 24 Tagen klicken Sie auf Daten [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Der Einfachheit halber Datenanalysten haben wir Daten um experimentieren entpackt.

Jeder Datensatz im Dataset enthält 40 Spalten:

- die erste Spalte ist eine Bezeichnungsspalte, die angibt, ob ein eine **Hinzufügen** (Wert 1 Benutzer) oder nicht (Wert 0) klicken
- nächsten 13 Spalten sind numerische, und
- letzten 26 werden kategoriale Spalten

Die Spalten werden anonymisiert und verwenden eine Reihe von aufgelisteten Namen: "SP1" (für die Beschriftungsspalte), "Col40" (für die letzte Kategoriespalte).            

Hier ist ein Auszug der ersten 20 Spalten (Zeilen) aus diesem Dataset zweierlei:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Werte fehlen in numerischen und kategorischen Spalten im Dataset. Eine einfache Methode für die Behandlung fehlender Werte beschrieben. Wenn wir in Hive-Tabellen speichern zusätzliche Details der Daten untersucht.

**Definition:** *Klickrate (CTR):* Dies ist der Prozentsatz der Daten klickt. In diesem Dataset Criteo wird der Preis von 3,3 % oder 0,033.

## <a name="mltasks"></a>Beispiele für Vorhersage Aufgaben
In dieser exemplarischen Vorgehensweise werden zwei Beispiel Vorhersage Probleme behandelt:

1. **Binäre Klassifizierung**: voraussagt, ob ein Benutzer hinzufügen geklickt haben:
    - Klasse 0: Keine auf
    - Klasse 1: Klicken Sie auf

2. **Regression**: prognostiziert die Wahrscheinlichkeit einer Anzeigenklicks von Funktionen.


## <a name="setup"></a>Ein HDInsight Hadoop auf Cluster für wissenschaftliche Daten

**Hinweis:** Dies ist normalerweise eine Aufgabe **Admin** .

Richten Sie Ihre Azure Data Science Umgebung für Vorhersageanalysen Lösungen mit HDInsight in drei Schritten:

1. [Erstellen ein Speicherkonto](../storage/storage-create-storage-account.md): dieses Speicherkonto zum Speichern von Daten in Azure BLOB-Speicher. Die Daten in HDInsight-Cluster werden hier gespeichert.

2. [Anpassen von Azure HDInsight Hadoop-Cluster für Daten](machine-learning-data-science-customize-hadoop-cluster.md): Dieser Schritt erstellt einen Azure HDInsight Hadoop Cluster mit 64-Bit-Anaconda Python 2.7 auf allen Knoten installiert. Gibt es zwei wichtige Schritte (in diesem Thema beschrieben) beim Anpassen des HDInsight-Clusters abgeschlossen.

    * Verknüpfen Sie das Speicherkonto bei seiner Erstellung in Schritt 1 mit der HDInsight erstellt. Dieser Speicher-Konto dient für den Zugriff auf Daten innerhalb des Clusters verarbeitet werden können.

    * Sie müssen der Head-Knoten des Clusters RAS aktivieren, nachdem es erstellt wurde. Sie hier geben (anders als für den Cluster erstellen) RAS-Anmeldeinformationen speichern: Sie benötigen sie die folgenden Schritte ausführen.

3. [Ein Azure ML-Arbeitsbereich erstellen](machine-learning-create-workspace.md): Diese Azure Machine Learning Arbeitsbereich Learning Modelle erstellen, nach einer anfänglichen datenuntersuchung und Beprobung von HDInsight-Cluster verwendet.

## <a name="getdata"></a>Abrufen Sie und Daten aus einer öffentlichen Quelle

Das Dataset [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) kann durch Klicken auf den Link und die Vereinbarung akzeptieren einen Namen zugegriffen werden. Eine Momentaufnahme der hierfür lautet:

![Criteo akzeptieren](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Klicken Sie auf **Weiter, um Download** erfahren Sie mehr über das Dataset und seine Verfügbarkeit.

Die Daten befinden sich an einem öffentlichen Ort [Azure BLOB-Speicher](../storage/storage-dotnet-how-to-use-blobs.md) : wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. "Wasb" bezieht sich auf Azure BLOB-Speicher. 

1. Die Daten in diesem öffentlichen Blob-Speicher besteht aus drei Unterordner extrahiert Daten.

    1. Der Unterordner *raw/Anzahl/* enthält die ersten 21 Tage des Daten - Tag\_00 Tag\_20
    2. Der Unterordner *raw/Zug/* besteht aus einem Tag Daten Tag\_21
    3. Der Unterordner *raw/Test/* besteht aus zwei Daten Tag\_22 und Tag\_23

2. Wer mit Gzip unformatierte Daten, diese stehen auch im Hauptordner *raw /* als day_NN.gz, wobei NN von 00 bis 23 reicht.

Ein alternativer Ansatz für den Zugriff auf Durchsuchen und Modell dieser Daten, der keine lokalen Downloads später in dieser exemplarischen Vorgehensweise erstellen wir Hive-Tabellen erläutert.

## <a name="login"></a>Melden Sie sich bei dem Hauptknoten cluster

[Azure-Portal](https://ms.portal.azure.com) mit der Hauptknoten des Clusters anmelden Cluster gefunden. Klicken Sie HDInsight Elephant auf der linken Seite, und doppelklicken Sie auf den Namen des Clusters. Navigieren Sie zu der Registerkarte **Konfiguration** , doppelklicken Sie auf das Ende der Seite verbinden und Anmeldeinformationen Sie RAS Aufforderung. Dadurch gelangen Sie auf die Hauptknoten des Clusters.

Hier sieht wie eine normale erste Anmeldung Hauptknoten Cluster:

![Melden Sie sich im cluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


Auf der linken Seite sehen wir die "Hadoop Befehlszeile" unsere für das Durchsuchen von Daten Arbeitspferd. Wir sehen auch nützliche URLs – "Hadoop Garn Status" und "Hadoop Namensknoten". Garn Status URL zeigt Auftrag und Namen Knoten URL gibt Details der Clusterkonfiguration.

Jetzt wir werden eingerichtet und bereit zum ersten Teil der exemplarischen Vorgehensweise: Durchsuchen von Daten mithilfe von Struktur und Daten für Azure maschinelles lernen.

## <a name="hive-db-tables"></a>Struktur-Datenbank und Tabellen erstellen

Zum Erstellen der Struktur Tabellen für unsere Criteo Dataset ***Hadoop Befehlszeile*** auf dem Head-Knoten aufrufen Sie und Struktur Verzeichnis durch Eingabe des Befehls

    cd %hive_home%\bin

>[AZURE.NOTE] Führen Sie alle Struktur Befehle in dieser exemplarischen Vorgehensweise aus der Struktur / Directory aufgefordert. Dies übernimmt pfadprobleme automatisch. Verwenden wir die Begriffe "Struktur Verzeichnis Aufforderung" "Struktur Bin / Directory Aufforderung", und "Hadoop" austauschbar.

>[AZURE.NOTE]  Zum Ausführen einer Abfrage Struktur können eine immer die folgenden Befehle verwenden:

        cd %hive_home%\bin
        hive

Nach Struktur REPL erscheint eine "Struktur >"Melden Sie sich einfach Ausschneiden und fügen Sie die Abfrage ausführen.

Der folgende Code erstellt eine Datenbank "Criteo" und 4 Tabellen generiert:


* eine *Tabelle zum Generieren von Anzahl* Tage Tag erstellt\_00 Tag\_20,
* eine *Tabelle für die Verwendung als Zug Dataset* erstellt am Tag\_21 und
* zwei *Tabellen verwenden als Test-Datasets* erstellt am Tag\_22 und Tag\_23 bzw..

Wir unser testdataset in zwei Tabellen aufgeteilt, da einen Tag ein Feiertag ist, und prüfen Sie, ob das Modell Unterschiede zwischen Urlaub und nicht an die Klickrate erkennen soll.

Das Skript [Sample #95; Struktur & #95; erstellen & #95; Criteo & #95, Datenbank & #95; und & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) Einfachheit hier angezeigt:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Wir Beachten Sie, dass diese Tabellen externe wir einfach auf Azure BLOB-Speicher (Wasb) verweisen.

**Gibt es zwei Methoden, um beliebige Struktur Abfrage ausführen, die wir jetzt erwähnt.**

1. **Verwenden der Befehlszeile Struktur REPL**: der erste ist ein Befehl "Struktur" und kopieren und Einfügen einer Abfrage an der Befehlszeile Struktur REPL. Vorgehen Sie dieses:

        cd %hive_home%\bin
        hive

    Jetzt wird ausgeführt auf Befehlszeile REPL Ausschneiden und Einfügen der Abfrage.

2. **Abfragen in einer Datei speichern und den Befehl ausführen**: die zweite ist eine .hql Abfragen speichern ([Beispiel #95; Struktur & #95; erstellen & #95; Criteo & #95, Datenbank & #95; und & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) und führen Sie den folgenden Befehl zum Ausführen der Abfrage:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Erstellung von Datenbank und Tabelle bestätigen

Als Nächstes bestätigen wir die Erstellung der Datenbank mit den folgenden Befehl aus der Struktur / Directory aufgefordert:

        hive -e "show databases;"

Dadurch werden:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Dadurch wird die Erstellung der neuen Datenbank "Criteo" bestätigt.

Welche Tabellen finden wir haben wir einfach den Befehl hier aus der Struktur / Directory aufgefordert:

        hive -e "show tables in criteo;"

Wir sehen dann die folgende Ausgabe:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Durchsuchen von Daten in Struktur

Jetzt werden wir einige Basisdaten Untersuchung Struktur führen. Wir beginnen, indem die Anzahl der Beispiele im Zug und Tabellen testen.

### <a name="number-of-train-examples"></a>Zug-Beispiele

Der Inhalt der [Beispiel #95; Struktur #95; Count & #95, Zug & #95, Tabelle & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) sind hier dargestellt:

        SELECT COUNT(*) FROM criteo.criteo_train;

Dies ergibt:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Alternativ eine möglicherweise auch mit folgendem Befehl aus der Struktur / Directory aufgefordert:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Anzahl der Test-Beispiele in zwei Test-datasets

Wir jetzt Anzahl die der Beispiele in zwei Test-Datasets. Der Inhalt des [Beispiel & #95 Struktur & #95; Count & #95; Criteo & #95; Test & #95, Tag & #95; 22 & #95, Tabelle und #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) sind:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Dies ergibt:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Wie gewohnt können wir das Skript aus der Struktur aufrufen / Directory Aufforderung durch den Befehl:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Außerdem untersuchen wir die Anzahl der Test-Beispiele in das testdataset basierend auf Tag\_23.

Der Befehl ist ähnlich der oben gezeigte (siehe [Beispiel #95; Struktur & #95; Count & #95; Criteo & #95; Test #95, Tag & #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Dadurch werden:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Bezeichnung Verteilung im Dataset Zug

Die Bezeichnung Verteilung im Zug Dataset ist. Dazu zeigen wir Inhalt [Sample #95; Struktur & #95; Criteo & #95; Beschriftung & #95; Verteilung #95, Zug & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Dies ergibt die Bezeichnung Verteilung:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Beachten Sie, dass der Prozentsatz der positiven Etiketten 3,3 % (in Übereinstimmung mit dem ursprünglichen Dataset).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Histogramm Distributionen einige numerische Variablen im Dataset Zug

Struktur des einheitlichen können "Histogramm\_numerischen"-Funktion die Verteilung von numerischen Variablen sieht herausfinden. Hier sind die Inhalte der [Sample #95; Struktur #95; Criteo & #95; Histogramm & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Dies ergibt Folgendes:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

SEITLICHE Ansicht - explodieren Kombination Struktur dient eine SQL-ähnliche Ausgabe statt der üblichen Liste. Beachten Sie, dass diese Tabelle die erste Spalte Bin Center und der zweite Bin Frequenz entspricht.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Ungefähre Perzentile einige numerischen Variablen im Dataset Zug

Auch ist mit numerischen Variablen die Berechnung der ungefähren Perzentile. Struktur des systemeigenen "prozentuale\_ca." wird für uns. Der Inhalt des [Beispiel #95; Struktur #95; Criteo & #95; ungefähre & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) sind:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Dies ergibt:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Wir bemerken, dass die Verteilung der Perzentile Histogramm Verteilung einer numerischen Variablen in der Regel eng ist.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Suchen Sie Anzahl eindeutiger Werte für einige kategorischen Spalten im Dataset Zug

Weiterhin Daten durchsuchen, finden wir nun einige kategorische Spalten der Anzahl eindeutiger Werte nehmen sie. Dazu zeigen wir Inhalt [Beispiel #95; Struktur #95; Criteo & #95, eindeutige & #95, Werte und #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Dies ergibt:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Wir Beachten Sie, dass Col15 19 M eindeutige Werte. Naive Techniken wie "ein hot encoding" ist hoch-dimensional kategorischen Variablen codieren unmöglich. Insbesondere erläutern und zeigen leistungsstarke, stabile genanntes [Learning zählt](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) für effiziente Lösung dieses Problems.

Wir Ende dieses Unterabschnitts anhand der Anzahl eindeutiger Werte für einige andere kategorischen Spalten sowie. Der Inhalt des [Beispiel #95; Struktur #95; Criteo & #95, eindeutige & #95; Werte & #95 mehrere & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) sind:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Dies ergibt:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Wieder sehen wir, dass außer Col20, die anderen Spalten viele eindeutige Werte.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Co Vorkommen von Paaren kategorischen Variablen im Dataset Zug zählt

Interessant ist auch gemeinsam Vorkommen Anzahl Paare von kategorischen Variablen. Diese ermittelt werden mit dem Code in [Beispiel #95; Struktur #95; Criteo & #95; gepaarten & #95; Kategorieliste & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Wir reverse bestellen die Anzahl ihrer vorkommen und Top 15 bei. Dadurch:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Sie Beispiel Datasets für Azure maschinelles lernen

Die Datasets untersucht und gezeigt, wie wir diese Art der Untersuchung für alle Variablen (einschließlich Kombinationen) wir jetzt Beispiel Daten tun, damit wir in Azure Machine Learning erstellen können. Daran erinnern, dass das Problem wir konzentrieren: einer gegebenen Beispiel Attribute (featurewerte von SP2 - Col40) wir voraussagen SP1 ist 0 (kein klicken) oder 1 (klicken).

Unten Zug und Datasets auf 1 % der Originalgröße zu testen, verwenden wir die Struktur des systemeigenen Rand()-Funktion. Das nächste Skript dies [Beispiel #95; Struktur #95; Criteo & #95; verkleinern & #95, Zug & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) für das Dataset Zug:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Dies ergibt:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Skript [Beispiel #95; Struktur & #95; Criteo & #95; verkleinern & #95; Test #95, Tag & #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) Funktion für Testdaten Tag\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Dies ergibt:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Schließlich Skript [Beispiel #95; Struktur & #95; Criteo & #95; verkleinern & #95; Test #95, Tag & #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) erledigt das für Testdaten Tag\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Dies ergibt:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Damit sind wir unten aufgenommenen Zug und test-Datasets Modelle in Azure Machine Learning bereit.

Bevor wir Azure Computer lernen, betrifft Count-Tabelle besteht eine letzte wichtige Komponente. Im nächsten Unterabschnitt besprechen wir dies ausführlich.

##<a name="count"></a>Eine kurze Erläuterung der Count-Tabelle

Wie wir gesehen haben mehrere kategorische Variablen eine sehr hohe Dimensionalität. In unserer exemplarischen Vorgehensweise stellen wir ein leistungsfähiges Verfahren zum Codieren dieser Variablen effiziente und robuste [Learning zählt](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) aufgerufen. Weitere Informationen zu dieser Technik ist in den Link.

**Hinweis:** In dieser exemplarischen Vorgehensweise liegt der Schwerpunkt auf Anzahl Tabellen kompakte Darstellung High-dimensional kategorische Funktionen zu verwenden. Dies ist nicht die einzige Möglichkeit, kategorisierte Funktionen codieren. Weitere Informationen zu anderen Verfahren interessierte Benutzer [1-hot-Codierung](http://en.wikipedia.org/wiki/One-hot) und [Feature hashing](http://en.wikipedia.org/wiki/Feature_hashing)finden.

Um die Anzahl der Tabellen zu der Daten werden die Daten in der unformatierten/Anzahl verwenden. Im Abschnitt Modellierung erfahren Benutzer wie diese Anzahl Tabellen für kategorische Features völlig oder eine vordefinierte anzahltabelle für ihre Entdeckungen verwendet. Im folgenden Wenn wir auf "vordefinierte Anzahl Tabellen" verstehen wir die Anzahl Tabellen, die wir bereitstellen. Detaillierte Informationen zum Zugreifen auf diese Tabellen werden im nächsten Abschnitt bereitgestellt.

## <a name="aml"></a>Ein Modell mit Azure maschinelles lernen

Unser Prozess in Azure Machine Learning Modell die folgenden Schritte:

1. [Daten in Azure Machine Learning Struktur Tabellen abrufen](#step1)
2. [Erstellen des Experiments: Daten bereinigen, wählen Sie Teilnehmern und mit Anzahl Tabellen](#step2)
3. [Trainieren Sie das Modell](#step3)
4. [Bewerten des Modells auf Daten](#step4)
5. [Auswerten des Modells](#step5)
6. [Veröffentlichen Sie das Modell als Webdienst verbraucht werden](#step6)

Jetzt können wir in Azure Machine Learning Studio erstellen. Unsere unten gesammelten Daten werden als Struktur Tabellen im Cluster. Wir mithilfe der Azure Machine Learning **Daten importieren** dieser Daten lesen. Im folgenden werden die Anmeldeinformationen für das Speicherkonto dieses Clusters zugreifen bereitgestellt.

### <a name="step1"></a>Schritt 1: Struktur Tabellen in Azure Computer mit Importdaten Module Daten erhalten und für einen Computer lernen Experiment auswählen

Wählen einer **+ neu** -> **EXPERIMENT** -> **Leere experimentieren**. Suchen Sie dann über **das Suchfeld oben** für "Daten importieren". Drag & drop Moduls **Import** auf der Leinwand Experiment (der mittlere Teil des Bildschirms) um das Modul für den Datenzugriff verwenden.

Dies sieht die **Importdaten** beim Abrufen von Daten aus der Tabelle Struktur:

![Ruft Daten importieren](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

Für das Modul **Import Data** sind die Werte der Parameter, die in der Grafik enthaltenen Beispiele für die Art der Werte, die Sie angeben müssen. Hier ist einige allgemeine Richtlinien zum Ausfüllen des Parameters für das Modul **Import** festgelegt.

1. Wählen Sie "Struktur Query" **für**
2. Im **Struktur Datenbankabfrage** , einfache SELECT * FROM < Ihre\_Datenbank\_name.your\_Tabelle\_Name >-reicht.
3. **Hcatalog Server-URI**: Wenn Cluster "Abc", ist: https://abc.azurehdinsight.net
4. **Hadoop Benutzernamen**: Benutzername bei Inbetriebnahme des Clusters ausgewählt. (Nicht RAS Benutzername!)
5. **Hadoop Benutzerkennwort**: das Kennwort für den Benutzernamen, der zum Zeitpunkt der Inbetriebnahme des Clusters ausgewählt. (Nicht die RAS-Kennwort!)
6. **Speicherort der Ausgabedaten**: Wählen Sie "Azure"
7. **Azure-speicherkontoname**: Speicherkonto den Cluster
8. **Azure-Speicher kontoschlüssel**: der Schlüssel für das Speicherkonto Cluster zugeordnet.
9. **Azure Containernamen**: Wenn der Clustername "Abc", ist dies normalerweise einfach "Abc".


Nach Abschluss die **Importdaten** erste Daten wird (das grüne Häkchen im Modul) speichern Sie diese Daten als Dataset (mit einem Namen Ihrer Wahl). Dies sieht:

![Daten importieren speichern Daten](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Klicken Sie auf den Ausgang des Moduls **Importieren** . Dies zeigt eine Option **als Dataset speichern** und **visualisieren** Option. Die Option **visualisieren** zeigt markiert, 100 Zeilen der Daten und einen rechten Bereich für die zusammenfassenden Statistiken. Um Daten zu speichern, wählen Sie **als Dataset speichern** und befolgen.

Markieren Sie das gespeicherte Dataset für die Verwendung in einem computerlernen suchen Sie Datasets verwenden **das Suchfeld in der folgenden Abbildung dargestellt** . Geben Sie dann einfach den Namen teilweise Zugriff, und ziehen das Dataset auf den Hauptfensterbereich Dataset hat. Der Hauptbereich ablegen auswählt in Computer lernen Modellierung verwendet.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] Dies gilt für Zug und Test-Datasets. Beachten Sie außerdem, den Datenbanknamen und Tabellennamen zu diesem Zweck hat verwenden. Die Werte in der Abbildung sind ausschließlich für Abbildung purposes.* *

### <a name="step2"></a>Schritt 2: Erstellen ein einfaches Experiments in Azure Machine Learning vorherzusagen Klicks / keine Klicks

Experiment Azure ML sieht folgendermaßen aus:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Nun untersuchen wir die Hauptkomponenten dieses Experiment. Erinnerung müssen wir gespeicherte Zug und Datasets auf Leinwand Experiment zunächst testen.

#### <a name="clean-missing-data"></a>Fehlende Daten bereinigen

**Saubere fehlt** Moduls ist wie der Name bereits sagt: bereinigt fehlende Daten, die Benutzer angegeben werden können. In dieser Unterrichtseinheit sehen wir dies:

![Fehlende Daten bereinigen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Hier haben wir alle fehlenden Werte durch eine 0 ersetzen. Es gibt auch andere Optionen, die anhand der Dropdown-Liste im Modul angezeigt werden können.

#### <a name="feature-engineering-on-the-data"></a>Featureentwicklung Daten

Millionen eindeutige Werte für einige kategorische Features von großen Datasets kann. Naive Methoden wie eine hot-Codierung für hohe dimensionale kategorische Features darstellt ist völlig unmöglich. In dieser exemplarischen Vorgehensweise demonstrieren wir Zählfunktionen integrierte Azure Machine Learning-Module zu kompakten Darstellung dieser High-dimensional kategorischen Variablen verwenden. Das Endergebnis ist Modell kleiner, schneller Training und Leistungsindikatoren, die vergleichbar mit anderen Techniken.

##### <a name="building-counting-transforms"></a>Zählen von Transformationen erstellen

Erstellen Sie Zählfunktionen verwenden wir **Build Counting Transform** -Modul, das in Azure maschinelles lernen. Das Modul sieht folgendermaßen aus:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Hinweis** : im Feld **Anzahl Spalten** geben wir die Spalten, die wir zählt auf durchführen möchten. Normalerweise sind (wie erwähnt) hochdimensionalen Kategoriespalten. Beginn erwähnt Criteo Dataset 26 kategorische Spalten hat: vom Col15 zum Col40. Hier, wir haben alle und ihre Indizes (von 15 bis 40 wie durch Kommas getrennt).

Das Modul MapReduce-Modus verwenden (geeignet für große Datasets), wir benötigen ein HDInsight Hadoop-Cluster (für Feature durchsuchen kann für diesen Zweck wiederverwendet werden) und die Anmeldeinformationen. Die vorherigen Zahlen zeigen die ausgefüllten Werte aussehen (zur Veranschaulichung mit für eigene Anwendungsfall Werte ersetzen).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

In der obigen Abbildung zeigen wir, wie die Eingabe Blob eingeben. Dieser Speicherort enthält Daten zählen Tabellen Grundlage vorbehalten.


Nach Abschluss dieser Unterrichtseinheit ausführen sparen die Transformation für später wir das Modul und wählen die Option **als Transformation speichern** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

In unserem Experiment Architektur oben entspricht das Dataset "ytransform2" genau eine gespeicherte Anzahl Transformation. Für den Rest dieses Experiment wird angenommen, dass der Reader ein **Build Counting Transform** -Modul einige Daten zählt erzeugt, und kann diese Zahlen generieren Zählfunktionen im Zug und Datasets testen.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Was zählt zu trainieren und Testen Datasets einschließen features auswählen

Haben wir Anzahl Transformation bereit, kann der Benutzer auswählen, welche features im Zug und test-Datasets mithilfe des Moduls **Zählen die Parameter ändern** . Wir zeigen dieses hier der Vollständigkeit, jedoch aus Gründen der Einfachheit nicht tatsächlich verwenden im Experiment.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

In diesem Fall gesehen werden können, haben wir verwenden einfach die Protokoll-Chancen und Backoff Spalte. Wir legen Sie die Parameter wie der Garbage Bin, wie viele Pseudo vorherigen Beispiele für glätten und ob Laplace Geräusche oder nicht hinzufügen. Diese erweiterte Funktionen und ist darauf hinzuweisen, dass die Standardwerte sind ein guter Ausgangspunkt für Benutzer der dieses Typs der KE-Erzeugung.

##### <a name="data-transformation-before-generating-the-count-features"></a>Transformation der Daten vor dem Generieren der Zählfunktionen

Wir konzentrieren wichtig zum Transformieren von Zug und Testdaten vor Zählfunktionen tatsächlich generieren. Beachten Sie, dass zwei **Ausführen R** Skriptmodule verwendet, bevor wir unsere Daten zählen Transformation anwenden.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Hier ist das erste R-Skript:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


In diesem Skript R benennen wir unsere Namen "SP1", "Col40". Dies ist die Anzahl Transformation Namen dieses Formats erwartet.

Im zweiten Skript R wir die Verteilung zwischen positiven und negativen Saldo (Klassen 1 und 0) von Downsampling negative Klasse. Das R-Skript veranschaulicht dies:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

In diesem Beispielskript R verwenden wir "pos\_Neg\_Verhältnis" Gleichgewicht zwischen positiven und negativen Klassen fest. Dies ist wichtig, da Klasse Ungleichgewicht normalerweise verbessert Leistung klassifizierungsprobleme die Klasse Verteilung ist geneigt (Beachten Sie hat, dass in diesem Fall 3,3 % positive und negative Class 96,7 % haben).

##### <a name="applying-the-count-transformation-on-our-data"></a>Die Anzahl der Transformation anwenden auf Daten

Schließlich können wir **Transformation anwenden** -Modul die Anzahl der Transformationen auf Zug und Datasets testen. Dieses Modul dauert gespeicherte Anzahl Transformation als eine Eingabe und Schulen oder Test Datasets als die Eingabe und gibt Daten mit Count. Hier das ist dargestellt:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Auszug der Zählfunktionen aussehen

Es ist hilfreich, sehen die Anzahl Funktionen wie in unserem Fall. Hier zeigen wir ein Auszug aus:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

In diesem Auszug zeigen wir, dass für die Spalten, die wir auf gezählt, wir die Anzahl und Quoten zusätzlich alle relevanten Backoffs anmelden.

Wir sind jetzt ein Azure Machine Learning-Modell mit diesen umgewandelten Datasets erstellen. Im nächsten Abschnitt zeigen wir, wie dies erreicht werden kann.

#### <a name="azure-machine-learning-model-building"></a>Azure Machine Learning-Modellbau

##### <a name="choice-of-learner"></a>Wahl von Teilnehmern

Zunächst müssen wir ein Schüler auswählen. Wir werden eine Entscheidungsstruktur zwei Klasse erhöht als unsere Teilnehmern verwenden. Hier werden die Standardoptionen für diesen Teilnehmern:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Experiment werden wir die Standardwerte auswählen. Wir Beachten Sie, dass die Standardeinstellungen in der Regel aussagekräftig sind und eine gute Möglichkeit, schnell Baselines Leistung erhalten. Verbessern Leistung durch Parameter ziehen, wenn Sie haben Sie eine Baseline auswählen.

#### <a name="train-the-model"></a>Trainieren Sie das Modell

Für Schulung rufen wir einfach **Train Model** -Modul. Die beiden Eingaben sind zwei Klassen erhöht Entscheidungsstruktur Teilnehmern und unser Zug Dataset. Dies ist hier dargestellt:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>Bewerten des Modells

Wir haben geschultes Modell können wir auf das Test-Dataset und seine Leistung zu bewerten. Dazu wird mit dem **Faktor Modell** Modul in der folgenden Abbildung mit einem Modul **Modell evaluieren** angezeigt:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>Schritt 5: Auswerten des Modells

Schließlich möchten wir Modell analysieren. Normalerweise ist ein gutes Maß für zwei (binär) Einstufung Probleme, die AUC. Um dies zu veranschaulichen, verknüpfen wir Modul **Score Model** , ein Modul **Modell evaluieren** dafür. Modul **Modell evaluieren** **visualisieren** auf ergibt eine Grafik wie die folgenden:

![Modul BDT Modell auswerten](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

Binär (oder zwei) Klassifizierung, ein gutes Maß für vorhersagegenauigkeit ist der Bereich unter Kurve (AUC). Im folgenden zeigen wir unsere Ergebnisse mit diesem Modell auf unserem Test-Dataset. Um dies zu erhalten, mit der rechten Maustaste Ausgabeanschluss das Modul **Modell evaluieren** und **visualisieren**.

![Modul Modell evaluieren visualisieren](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Schritt 6: Veröffentlichen Sie das Modell als Webdienst
Die Möglichkeit, ein Azure Machine Learning-Modell als Webdienste mit minimalem Aufwand zu veröffentlichen ist eine wertvolle Funktion dafür zur Verfügung. Danach, kann jeder anrufen mit Daten, dass sie Vorhersagen für den Webdienst verwendet das Modell diese Vorhersagen zurückgegeben.

Dazu speichern wir zuerst unsere ausgebildeten Modell als geschulte Modellobjekts. Dies geschieht **Train Model** -Modul, und mithilfe der Option **ausgebildeten Modell speichern** .

Als Nächstes müssen wir erstellen und Ausgangsports für Webdienst:

* ein Eingang akzeptiert Daten aus den Daten, die wir Vorhersagen für
* Ein Ausgabeanschluss gibt die Etiketten bewertet und Wahrscheinlichkeiten zugeordneten.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Wählen Sie Zeilen von Daten für den Eingang

Es empfiehlt sich, ein Modul **SQL-Transformation anwenden** nur 10 Zeilen dienen als Eingang Daten auszuwählen. Wählen Sie diese Datenzeilen für unsere Eingangs-Port hier SQL-Abfrage:

![Eingang-Daten](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webdienst
Nun können wir ein kleines Experiment ausführen, mit dem Webdienst veröffentlicht werden können.

#### <a name="generate-input-data-for-webservice"></a>Generieren von Eingabedaten für webservice

Schritt nullte Count Tabelle groß ist wir einige Zeilen mit Daten und Ausgabedaten mit Count daraus erstellen. Dies dient als Eingabedaten Format für unsere Webservice. Dies ist hier dargestellt:

![BDT eingegebenen Daten erstellen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] Für das Format Eingabedaten verwenden wir nun die Ausgabe des Moduls **Anzahl Featurizer** . Nach diesem Versuch beendet ist, speichern Sie die Ausgabe von **Count Featurizer** Modul als Dataset. Dieses Dataset wird für die Eingabedaten in den Webdienst verwendet.

#### <a name="scoring-experiment-for-publishing-webservice"></a>Publishing Webservice Experiment bewerten

Zunächst gezeigt, wie das aussieht. Die Grundstruktur ist ein **Faktor Modell** -Modul, die akzeptiert unsere ausgebildeten Modellobjekte und einige Zeilen mit Daten, die wir in den vorherigen Schritten mit **Count Featurizer** Modul generiert. Wir verwenden "Wählen Sie Spalten im Dataset" Projekt erzielte Etiketten und Wahrscheinlichkeiten Faktor.

![Wählen Sie Spalten im Dataset](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Beachten Sie, wie das Modul **Spalten im Dataset auswählen** lässt "Filterung" Daten aus einem Dataset. Wir zeigen den Inhalt:

![Wählen Sie Spalten im Dataset Modul filtern](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

Blaue ein- und Ausgänge klicken Sie einfach unten rechts **Webservice vorbereiten** . Dieses Experiment ausführen ermöglicht uns, den Webdienst veröffentlichen: Klicken Sie auf **Webdienst veröffentlichen** Symbol unten rechts, hier:

![Webdienst veröffentlichen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Sobald der Webdienst veröffentlicht wird, erhalten wir auf eine Seite umgeleitet, die so aussieht:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Wir sehen zwei Links zu Webservices auf der linken Seite:

* **Anforderung-Antwort** -Dienst (oder Einträge) soll für einzelne Vorhersagen und ist, was wir in diesem Workshop verwenden.
* Die **BATCHAUSFÜHRUNG** Service (BES) Batch Vorhersagen verwendet und erfordert die Eingabedaten Vorhersagen in Azure BLOB-Speicher befinden.

Der Link dauert **Anforderung/Antwort** uns zu einer Seite, die uns ermöglicht Dosen Code in C#, Python und r vorab Dieser Code kann problemlos für Aufrufe an den Webdienst verwendet werden. Beachten Sie, dass der API-Schlüssel auf dieser Seite zur Authentifizierung verwendet werden.

Es empfiehlt sich, diese Python-Code in eine neue Zelle in der Arbeitsmappe IPython kopieren.

Hier zeigen wir ein Segment der Python-Code mit dem richtigen API-Schlüssel.

![Python-code](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Beachten Sie, dass wir mit unserer Webservices API-Schlüssel Standard API-Schlüssel ersetzt. **Führen Sie** in diese Zelle ein IPython Notebook ergibt Folgendes:

![IPython Antwort](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Wir sehen, dass für die Test-Beispiele, die, denen wir (in Python-Skript JSON-Framework) gefragt, wir Antworten im Formular "Erzielte Etiketten erzielte Wahrscheinlichkeiten" zurück. Beachten Sie, dass in diesem Fall haben wir die Standardwerte vorerstellte Code enthält (0 für alle numerischen Spalten und die Zeichenfolge "Value" für alle kategorisierten Spalten).

Unsere End-to-End Exemplarische Vorgehensweise zeigt, wie mit großen Datasets mithilfe von Azure Machine Learning abgeschlossen. Wir begannen mit einem Terabyte an Daten, ein Vorhersagemodell erstellt und als Web Service in der Cloud bereitgestellt.
