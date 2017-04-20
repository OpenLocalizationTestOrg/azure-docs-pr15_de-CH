<properties
    pageTitle="Über uns und Spark Azure Data Science | Microsoft Azure"
    description="Verwendung von Scala für überwachte Computer lernaufgaben mit dem Spark skalierbare MLlib und Spark ML in Azure HDInsight Spark-Cluster."  
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
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Über uns und Spark Azure Data Science

Dieser Artikel veranschaulicht das Scala für überwachte Computer lernaufgaben mit dem Spark skalierbare MLlib und Spark ML in Azure HDInsight Spark-Cluster verwenden. Er führt Sie durch die Aufgaben, die [Datenwissenschaft Prozess](http://aka.ms/datascienceprocess)bilden: Daten Erfassung und Untersuchung, Visualisierung Featureentwicklung, Modellierung und Modell Verbrauch. Modelle in Artikel gehören logistische und lineare Regression, zufällige Wälder und Farbverlauf verstärkt Strukturen (AGB) neben zwei überwachten Computer lernaufgaben:

- Regressionsproblem: Vorhersage des Tip-Betrag (MW) für eine Taxi
- Binäre Klassifizierung: Vorhersage Tipp oder eine Taxi kein Tipp (1/0)

Modellierungsprozesses erfordert Ausbildung und Bewertung Testdaten und entsprechende Genauigkeit Metriken. In diesem Artikel erfahren Sie, wie diese Modelle in Azure BLOB-Speicher speichern und bewerten und Bewertung ihrer vorhersehbare Leistung. Dieser Artikel behandelt auch die Themen der Modelle optimieren mit Cross-Validierung und hyper-Parameter ziehen. Die verwendeten Daten ist ein Beispiel der 2013 NYC Taxi Reise und des Fahrpreises Daten auf GitHub.

[Scala](http://www.scala-lang.org/)basiert auf der Java VM ist objektorientiert und funktionale Sprachkonzepte integriert. Es ist eine skalierbare Sprache, die auch für verteilte Verarbeitung in der Cloud auf Azure Spark-Cluster ausgeführt wird.

[Spark](http://spark.apache.org/) ist ein Open Source-parallele Verarbeitung, die Verarbeitung im Speicher zur Leistungssteigerung big Data Analytics Applikationen unterstützt. Verarbeitungsmoduls Spark ist für einfachen und komplexen Analytics erstellt. Spark verteilte Berechnung speicherresidenten Funktionen machen es eine gute Wahl für iterative Algorithmen Computer lernen und Graph berechnet. [Spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) -Paket bietet eine einheitliche Daten, die Ihnen helfen können erstellen und optimieren praktische maschinelles lernen Rohrleitungen, erstellten allgemeinen APIs. [MLlib](http://spark.apache.org/mllib/) ist Spark skalierbare maschinelles lernen bringt Modellierungsfunktionen dieser verteilten Umgebung.

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) ist der Azure gehostet Opensource-Funken. Außerdem bietet Unterstützung für Jupyter Scala Notebooks Spark-Cluster und können Abfragen Spark SQL interaktive transformieren, Filtern und Daten in Azure BLOB-Speicher gespeichert. Scala Codeausschnitte in diesem Artikel, die die Lösungen und entsprechenden Plots, die Daten anzeigen führen Jupyter Notebooks auf Spark-Cluster installiert. Modellierung Schritte in den folgenden Themen ist Code, der wie Schulen, bewerten, speichern und nutzen jeder Typ des Modells.

Die Schritte und Code in diesem Artikel sind für Azure HDInsight 3.4 Spark 1.6. Der Code in diesem Artikel und in [Scala Jupyter Notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) jedoch sind generisch und Spark-Cluster arbeiten sollten. Die Cluster-Einrichtung und Verwaltung Schritte möglicherweise geringfügig von den in diesem Artikel gezeigt, verwenden Sie nicht HDInsight Spark.

> [AZURE.NOTE] Ein Thema, das in Python als Scala verwenden, um Aufgaben für eine End-to-End-Datenwissenschaft veranschaulicht, finden Sie unter [Data Science Spark auf Azure HDInsight verwenden](machine-learning-data-science-spark-overview.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

-   Sie müssen ein Azure-Abonnement. Wenn Sie nicht bereits eine [Azure Testversion erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)haben.

-   Sie benötigen einen Azure HDInsight 3.4 Spark 1.6 Cluster die folgenden Schritte ausführen. Zum Erstellen eines Clusters finden Sie in [Erste Schritte: Erstellen von Apache Spark auf Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Legen Sie Clustertyp und Version Menü **Clustertyp auswählen** .

![HDInsight Clusterkonfiguration](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Eine NYC Taxi Daten und Informationen zum Code ein Jupyter Notebook Spark-Cluster ausgeführt werden finden Sie unter den entsprechenden Abschnitten im [Überblick der Daten mit Spark auf Azure HDInsight](machine-learning-data-science-spark-overview.md).  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Scala Code vom Jupyter Notebook Spark-Cluster auszuführen

Sie können ein Jupyter Notebook von Azure-Portal starten. Klicken Sie, um die Verwaltungsseite für den Cluster eingeben finden Sie Spark-Cluster im Dashboard Anschließend auf **Cluster-Dashboards**und **Jupyter Notebook** , um den Cluster Spark Notizbuch öffnen klicken.

![Cluster-Dashboard und Jupyter notebooks](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Https:// Jupyter Notebooks können auch zugreifen&lt;Clustername&gt;.azurehdinsight.net/jupyter. Der Name des Clusters ersetzen Sie *Clustername* . Sie benötigen das Kennwort für das Administratorkonto auf Jupyter Notebooks.

![Wechseln Sie mithilfe des Clusternamens Jupyter notebooks](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Wählen Sie **Scala** zu einem Verzeichnis mit ein paar Beispiele für vorkonfigurierte Notebooks, die PySpark-API verwenden. Untersuchung Modellierung und mit Scala.ipynb Notizbuch, Codebeispiele für diese Sammlung Spark Themen enthält, Scoring ist auf [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala)verfügbar.


Sie können das Notebook direkt von GitHub Jupyter Notebook-Server im Cluster Spark hochladen. Jupyter Startseite klicken Sie auf **Hochladen** . Fügen Sie im Dateiexplorer GitHub (unformatierte Inhalt) URL des Notizbuchs Scala, und klicken Sie auf **Öffnen**. Scala Notebook steht unter der folgenden URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Setup: Voreinstellung Spark und Struktur Kontexte Funken Magie und Spark-Bibliotheken

### <a name="preset-spark-and-hive-contexts"></a>Voreingestellte Spark und Struktur Kontexte

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Spark-Kernels, die mit Jupyter Notebooks Voreinstellung Kontexte. Sie müssen explizit den Funken oder Struktur Kontexten vor der Arbeit mit der Anwendung entwickeln. Die vordefinierten Kontexte sind:

- `sc`für SparkContext
- `sqlContext`für HiveContext


### <a name="spark-magics"></a>Spark Magie

Spark-Kernel stellt einige vordefinierte "zaubert" sind spezielle Befehle aufrufen mit `%%`. Zwei dieser Befehle sind in den folgenden Codebeispielen verwendet.

- `%%local`Gibt an, dass der Code in den folgenden Zeilen lokal ausgeführt werden. Der Code muss gültige Scala ein.
- `%%sql -o <variable name>`führt eine Abfrage Struktur in `sqlContext`. Wenn die `-o` -Parameter übergeben wird, wird das Ergebnis der Abfrage im beibehalten der `%%local` Scala Kontext als Spark Datenrahmen.

Für Weitere Informationen zu den Kernel für Jupyter Notebooks und ihre vordefinierten "die zaubert"-Aufruf mit `%%` (z. B. `%%local`), finden Sie unter [für Jupyter Notebooks für HDInsight mit HDInsight Spark Linux Kernels](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Importbibliotheken

Importieren Sie die Spark, MLlib und anderen Bibliotheken müssen Sie mithilfe des folgenden Codes.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Erfassung von Daten

Der erste Schritt im Prozess Datenwissenschaft ist die Daten aufnehmen, die Sie analysieren möchten. Die Daten einzubinden aus externen Quellen oder Systemen Speicherort in Ihrer datenumgebung, Untersuchung und Modellierung. In diesem Artikel ist die Daten, die Sie aufnehmen einer verknüpften 0,1 % Beispiel der Taxi Reise und Tarif Datei (gespeichert als TSV-Datei). Daten ist durchsuchen und Modellierung Spark. Dieser Abschnitt enthält den Code, um die folgende Reihe von Aufgaben ausführen:

1. Festlegen Sie Verzeichnispfade für Daten und Modell.
2. Lesen des Eingabedatensatzes (gespeichert als TSV-Datei).
3. Definieren Sie ein Schema für die Daten und bereinigen Sie die Daten.
4. Erstellt einen Datenrahmen bereinigten und im Arbeitsspeicher zwischengespeichert.
5. Registrieren Sie die Daten als eine temporäre Tabelle zur SQLContext.
6. Die Tabelle und die Ergebnisse in einem Datenrahmen importieren.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Festlegen Sie Verzeichnispfade für Lagerorte in Azure BLOB-Speicher

Spark lesen und Schreiben in Azure BLOB-Speicher. Sie können Spark verwenden, um vorhandene Daten verarbeiten und speichern die Ergebnisse wieder im BLOB-Speicher.

Um Modelle oder Dateien im BLOB-Speicher zu speichern, müssen Sie die Pfadangabe korrekt. Verweisen Standardcontainer zugeordnet Spark-Cluster mit einem Pfad, der mit `wasb:///`. Verweisen Sie andere Speicherorte mit `wasb://`.

Das folgende Codebeispiel gibt den Speicherort der Eingabedaten gelesen werden und den Pfad zum BLOB-Speicher, der Spark-Cluster zugeordnet ist, wo das Modell gespeichert werden.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Daten importieren und erstellen ein RDD Datenrahmen nach Schema definieren

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Ausgabe:**

Die Zelle führen: 8 Sekunden.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Die Tabelle und Ergebnisse in einem Frame Daten importieren

Dann Fragen Sie die Tabelle für Flugpreis, Personenkraftwagen und Tipp; beschädigt und ausgelagerte Daten filtern. und mehrere Zeilen drucken.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Ausgabe:**

fare_amount|passenger_count|tip_amount|Geneigter
-----------|---------------|----------|------
       13,5|            1.0|       2.9|   1.0
       16,0|            2.0|       3.4|   1.0
       10.5|            2.0|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Durchsuchen von Daten und Visualisierung

Nachdem Sie die Daten in Spark, ist der nächste Schritt im Prozess Datenwissenschaft erhalten Sie ein besseres Verständnis der Daten durch Untersuchung und Visualisierung. In diesem Abschnitt überprüfen Sie Taxi Daten mithilfe von SQL-Abfragen. Importieren Sie dann die Ergebnisse in einem Datenrahmen Zielvariablen und künftige Features für visuelle Inspektion mit der Auto-Visualisierung des Jupyter darstellen.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Mit der lokalen und Magic SQL Daten

Standardmäßig steht die Ausgabe jeder Codeausschnitt aus Jupyter Notebook im Rahmen der Sitzung, die die Arbeitskraft Knoten beibehalten wird. Wenn eine Arbeitskraft Knoten für jede Berechnung speichern möchten und wenn die Daten, die für die Berechnung benötigt lokal auf dem Jupyter-Serverknoten (die dem Head-Knoten) verfügbar ist, Sie können die `%%local` Magic Codeausschnitt auf Jupyter Server ausgeführt.

- **Magic SQL** (`%%sql`). HDInsight Spark-Kernel unterstützt SqlContext-einfach Inline-HiveQL Abfragen. Die (`-o VARIABLE_NAME`)-Argument behält die Ausgabe der SQL-Abfrage als Panda Datenrahmen auf dem Server Jupyter. Dies bedeutet, dass es im lokalen Modus verfügbar werden.
- `%%local`**Magic**. Die `%%local` Magic führt den Code lokal auf dem Head-Knoten des Clusters HDInsight ist Jupyter Server. Normalerweise verwenden Sie `%%local` Magic zusammen mit der `%%sql` mit dem `-o` Parameter. Die `-o` Parameter wird die Ausgabe der SQL-Abfrage lokal beibehalten und `%%local` Magic auslösen würde den nächsten Satz von Codeausschnitt lokal für die Ausgabe von SQL-Abfragen ausführen, die lokal gespeichert werden.

### <a name="query-the-data-by-using-sql"></a>Fragen Sie Daten mithilfe von SQL ab
Diese Abfrage ruft taxifahrten vom Preis Menge, Anzahl der Fahrgäste und Tipp.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

Im folgenden Code die `%%local` Magic erstellt einen lokalen Rahmen SqlResults. SqlResults können mit Matplotlib darstellen.

> [AZURE.TIP] Lokale Magic wird mehrmals in diesem Artikel verwendet. Wenn Ihr Dataset groß ist, probieren Sie zum Erstellen eines passen Datenrahmens im lokalen Speicher.

### <a name="plot-the-data"></a>Darstellen von Daten

Sie können mithilfe von Python-Code nach der Datenrahmen im lokalen Kontext als Panda Datenrahmen ist darstellen.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Spark-Kernel visualisiert Ausgabe (HiveQL) SQL-Abfragen automatisch, nachdem der Code ausgeführt. Sie können zwischen mehreren Visualisierung:
 
- Tabelle
- Kreis
- Zeile
- Bereich
- Leiste

Hier ist der Code zur Darstellung der Daten:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Ausgabe:**

![Tipp Betrag Histogramm](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tipp Betrag nach Anzahl der Fahrgäste](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tipp Betrag Fahrpreis Betrag](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>KEs Features transformieren und prep Daten für Eingabe modeling-Funktionen

Für strukturbasierte Funktionen Spark ML und MLlib müssen Sie Ziel und Funktionen mithilfe verschiedener Techniken wie binning, Indizierung einer hot-Codierung und Vektorisierung vorbereiten. Hier sind die Verfahren in diesem Abschnitt:

1. Erstellen Sie ein neues Feature von **binning** Stunden in Verkehr Perioden
2. **Indizierung und einer hot - Codierung** kategorische Funktionen anwenden
3. **Beispiel und teilen der** Schulung und Brüche.
4. **Geben Sie Training Variablen und Funktionen**, dann indiziert oder eine hot-codierte Schulung und erstellen testen gekennzeichneten Eingabepunkt stabil Datasets (RDDs) oder Datenrahmen verteilt.
5. Automatisch **Kategorisieren und Vektorisieren Funktionen und Ziele** als Eingaben für Learning Modelle verwenden.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Binning Stunden in Verkehr Perioden erstellen Sie neu

Dieser Code zeigt, wie binning Stunden neu in Verkehr Perioden erstellen und resultierende Datenrahmen im Arbeitsspeicher zwischengespeichert. Zwischenspeichern von führt zu verbesserter Ausführungszeiten, RDDs und Data Frames wiederholt verwendet werden. Entsprechend werden Sie in mehreren Phasen im folgenden RDDs und Datenrahmen Zwischenspeichern.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indizierung und einer hot kategorische Features Codierung

Die Modellierung und MLlib-Funktionen erfordern mit input Kategoriedaten indiziert oder vor der Verwendung codiert werden. In diesem Abschnitt wird das Indizieren oder codieren kategorische Funktionen für Eingabe der Funktionen veranschaulicht.

Index oder Modellen je nach Modell unterschiedlich codiert werden müssen. Logistik und lineare regressionsmodelle erfordern beispielsweise eine hot-Codierung. Beispielsweise kann eine Funktion mit drei Kategorien in drei Funktion Spalten erweitert werden. Jede Spalte enthält 0 oder 1, je nach einer Beobachtung. MLlib stellt die [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) -Funktion für ein hot-Codierung. Diese Encoder Spalte binären Vektoren mit höchstens einer One-Wert eine Spalte Beschriftung Indizes zugeordnet. Mit dieser Codierung können Algorithmen, die numerischen Werten Features wie logistische Regression erwarten kategorische Funktionen angewendet werden.

Transformieren Sie hier nur vier Variablen Beispielen sind Zeichenfolgen. Sie können auch andere Variablen wie Wochentags, dargestellt durch numerische Werte als kategoriale Variable index.

Indizierung verwenden `StringIndexer()`, sowie für die Codierung einer hot- `OneHotEncoder()` MLlib-Funktionen. Hier ist der Code indiziert und codieren kategorische Features:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Ausgabe:**

Die Zelle Ausführung: 4 Sekunden.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Beispiel und teilen die Datengruppe in Schulung und Brüche

Dieser Code erstellt eine zufällige Stichprobe der Daten (in diesem Beispiel 25 %). Obwohl Probenahme nicht für dieses Beispiel die Größe der Daten erforderlich ist, zeigt Artikel, wie Sie aufnehmen, damit Sie für Ihre eigenen Probleme bei Bedarf verwenden können. Wenn Proben groß sind, kann diese Zeit speichern, während Modelle trainieren. Anschließend Teilen Sie im Beispiel eine Schulung Teil (in diesem Beispiel 75 %) und testen (in diesem Beispiel 25 %), Klassifizierung und Regression Modellierung verwenden.

Jede Zeile (in der Spalte "Rand") mit Wählen Sie übergreifende Überprüfung Falten Training eine Zufallszahl (zwischen 0 und 1) hinzugefügt.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Ausgabe:**

Die Zelle führen: 2 Sekunden.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Geben Sie Schulung Variablen und Funktionen an und erstellen Sie dann indiziert oder einem hot codiert Ausbildung und Prüfung von Eingaben Punkt RDDs oder Daten Rahmen gekennzeichnet

Dieser Abschnitt enthält Code, der wie Indexdaten kategorischen Text als Datentyp bezeichneten Punkt und Verwendung es und logistische Regression MLlib und andere klassifizierungsmodelle testen codieren. Beschriftete Point-Objekte sind RDDs, die so formatiert sind, die von den meisten Computer lernen Algorithmen in MLlib als Eingabedaten erforderlich ist. Ein [Punkt bezeichnet](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) ist einer lokalen Vektor dicht oder gering gefüllten, Bezeichnung/Antwort.

In diesem Code Geben Sie Ziel (abhängigen) Variablen und Funktionen zur Modelle trainieren. Dann erstellen Sie indiziert oder eine hot-codierte Ausbildung und Prüfung von Eingaben Punkt RDDs oder Daten Rahmen gekennzeichnet.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Ausgabe:**

Die Zelle Ausführung: 4 Sekunden.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Automatisch kategorisieren Sie und Vektorisieren Sie Funktionen und Ziele als Eingaben für maschinelles lernen Modelle verwenden

Verwenden Sie Spark ML Ziel- und Funktionen für strukturbasierte Funktionen kategorisiert. Der Code führt zwei Aufgaben aus:

-   Erstellt eine binäre Ziel Klassifizierung Datenpunkte zwischen 0 und 1 mit einem Schwellenwert von 0,5 ein Wert 0 oder 1 zugewiesen.
- Funktionen kategorisiert automatisch. Die Anzahl von unterschiedlichen numerischen Werten für jede Funktion kleiner als 32 ist, wird dieses Feature kategorisiert.

Hier ist der Code für diese beiden Aufgaben.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Binäre klassifizierungsmodell: abschätzen, ob ein Tipp gezahlt werden soll

In diesem Abschnitt erstellen Sie drei Arten von binären klassifizierungsmodelle Vorhersagen, ob ein Tipp zu entrichten:

- Eine **logistische Regressionsmodell** mit Spark ML `LogisticRegression()` Funktion
- Ein **zufälliger Klassifizierung Gesamtstrukturmodell** mit Spark ML `RandomForestClassifier()` Funktion
- Ein **Farbverlauf Steigerung Klassifizierung Strukturmodell** mithilfe der MLlib `GradientBoostedTrees()` Funktion

### <a name="create-a-logistic-regression-model"></a>Logistische Regressionsmodell erstellen

Anschließend mithilfe eine logistischen Regressionsmodell Spark ML `LogisticRegression()` Funktion. Sie erstellen das Modell Code in eine Reihe von Schritten erstellen:

1. **Trainieren des Modells** mit einem Parameter des Datensatzes.
2. **Auswerten des Modells** auf einem Test mit Metriken.
3. **Speichern Sie das Modell** im BLOB-Speicher für zukünftige Verbrauch.
4. **Bewerten des Modells** mit Daten.
5. **Die Ergebnisse Plotten** mit Kurven Merkmal (ROC) ausgeführt.

Hier ist der Code für diese Prozeduren:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Laden, bewerten und die Ergebnisse speichern.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Ausgabe:**

ROC auf Testdaten = 0.9827381497557599


Verwenden Sie Python Frames von Panda-Daten, die ROC Kurve zeichnen.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**Ausgabe:**

![Tipp oder kein Tipp ROC Kurve](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Erstellen Sie eine zufällige Gesamtstrukturmodell Klassifizierung

Anschließend mithilfe einer zufälligen Klassifizierung Gesamtstrukturmodell Spark ML `RandomForestClassifier()` Funktion und Auswerten des Modells auf Daten.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Ausgabe:**

ROC auf Testdaten = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>AGB klassifizierungsmodell erstellen

AGB klassifizierungsmodell anschließend mithilfe des MLlib erstellen `GradientBoostedTrees()` Funktion und Auswerten des Modells auf Daten.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Ausgabe:**

Bereich unter ROC Kurve: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Regressionsmodell: Vorhersagen Spitze Betrag

In diesem Abschnitt erstellen Sie zwei Arten von regressionsmodelle vorherzusagen Spitze Betrag:

- Eine **lineare Regressionsmodell legalisiert** mit Spark ML `LinearRegression()` Funktion. Sie das Modell speichern und Modell Daten auszuwerten.
- Ein **Farbverlauf Förderung Struktur Regressionsmodell** mit Spark ML `GBTRegressor()` Funktion.


### <a name="create-a-regularized-linear-regression-model"></a>Regularisierte lineare Regressionsmodell erstellen

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Ausgabe:**

Die Zelle führen: 13 Sekunden.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Ausgabe:**

R-Sqr auf Testdaten = 0.5960320470835743


Als Nächstes die Testergebnisse als Datenrahmen und verwenden Sie AutoVizWidget und Matplotlib um zu visualisieren.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Der Code erstellt einen lokalen Rahmen aus der Abfrageausgabe und zeichnet die Daten. Die `%%local` Magic erstellt einen Rahmen lokale Daten `sqlResults`, mit Matplotlib darstellen.

>[AZURE.NOTE] Diese Magic Spark wird mehrmals in diesem Artikel verwendet. Wenn die Datenmenge groß ist, probieren Sie zum Erstellen eines passen Datenrahmens im lokalen Speicher.

Erstellen Sie Plots mithilfe von Python Matplotlib.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Ausgabe:**

![Tipp Betrag: Vergleich der vorhergesagten und](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>Erstellen eines Regressionsmodells AGB

Erstellen ein Regressionsmodells AGB mit Spark ML `GBTRegressor()` Funktion und Auswerten des Modells auf Daten.

[Farbverlauf verstärkt Strukturen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (AGB) sind Ensembles Entscheidungsstrukturen. AGB Schulen Entscheidungsstrukturen iterativ zu einem verlustfunktion. Sie können AGB Regression und verwenden. Sie können kategorische Features erfordern keine Funktion skalieren und Nichtlinearitäten und Feature-Aktivitäten aufzeichnen. Sie können auch in einem mehrfachklasse Klassifizierung verwenden.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Ausgabe:**

Test R Sqr ist: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Erweiterte Modellierung Dienstprogramme zur Optimierung

In diesem Abschnitt verwenden Sie Computer lernen Dienstprogramme, mit denen Entwickler häufig zur modelloptimierung. Insbesondere können Sie Parameter-Sweeping mit übergreifende Überprüfung Learning Modelle drei Arten optimieren:

-   Zug und Validierung legt die Daten aufgeteilt, Modell mit hyper-Parameter ziehen eine Trainingsmenge optimieren und auswerten auf Überprüfung (lineare Regression)
-   Optimieren Sie Modell mit Cross-Validierung und hyper-Parameter Funktion Spark ML CrossValidator (binäre Klassifizierung) kehren
-   Optimieren des Modells mit benutzerdefinierten übergreifende Überprüfung und Parameter kehren mit jedem Computer lernen-Funktion und die Parameter festgelegt (lineare Regression)


**Übergreifende Überprüfung** ist ein Verfahren, das wie ein Modell auf einem bekannten Satz von Daten trainiert generalisiert Features von Daten Vorhersagen, an dem er nicht trainiert wurde, analysiert. Der Grundgedanke hinter dieser Technik ist, dass ein Modell auf einem bekannten Daten trainiert und die Genauigkeit der Prognosen wird anhand einer unabhängigen Daten getestet. Eine gängige Implementierung wird *k*eine Datengruppe aufgeteilt-Falten, und Trainieren Sie das Modell eine Roundrobin auf alle Falten.

**Hyper-Parameter Optimierung** ist das Problem der Auswahl ein hyper-Parameter für einen Algorithmus Learning normalerweise mit dem Ziel der Optimierung der Leistung der Algorithmus auf eine unabhängige. Hyper-Parameter ist ein Wert, den außerhalb der Schulung Modell anzugeben. Annahmen über hyper-Parameterwerte können die Flexibilität und die Genauigkeit des Modells auswirken. Entscheidungsstrukturen haben hyper-Parameter, z. B. wie die gewünschte Tiefe und Anzahl der Blätter in der Struktur. Sie müssen einen falschen Klassifizierung Sanktion Begriff für einen Vektor Maschine (SVM) festlegen.

Eine gängige Methode zum hyper-Parameter Optimierung ist ein Raster Suche, auch als **Parameter Sweep**. Eine vollständige Suche erfolgt eine Suche Raster durch die Werte für eine angegebene Teilmenge von hyper-Parameterraum Learning-Algorithmus. Übergreifende Überprüfung liefern Leistungsmetrik zu sortieren, die optimale Ergebnisse Suchalgorithmus Raster. Verwenden Sie übergreifende Überprüfung hyper-Parameter kehren helfen Ihnen Limit Probleme wie überanpassung ein Modell Trainingsdaten. Auf diese Weise behält das Modell die Fähigkeit, auf die allgemeinen Daten aus dem Daten extrahiert wurde.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Optimieren von einem linearen Regressionsmodell mit hyper-Parameter ziehen

Anschließend Teilen Sie Daten in Schulen und Validierung legt mit hyper-Parameter auf eine Trainingsmenge Modell optimieren und wertet auf Überprüfung (lineare Regression) ziehen.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Ausgabe:**

Test R Sqr ist: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Optimieren Sie binäre klassifizierungsmodell mit Cross-Validierung und hyper-Parameter kehren

Dieser Abschnitt veranschaulicht die binäre klassifizierungsmodell optimieren, indem übergreifende Überprüfung und hyper-Parameter ziehen. Verwendet die Spark ML `CrossValidator` Funktion.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Ausgabe:**

Die Zelle führen: 33 Sekunden.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Optimieren Sie lineare Regressionsmodell mit benutzerdefinierten übergreifende Überprüfung und Parameter-Sweeping code

Als Nächstes optimieren Sie Modell mithilfe von benutzerdefiniertem Code und identifizieren Sie die besten Modellparameter mit der höchsten Genauigkeit. Erstellen Sie anschließend das letzte Modell Werten Sie des Modells auf Daten aus und speichern Sie das Modell im BLOB-Speicher. Schließlich das Modell laden Daten bewerten und Genauigkeit auswerten.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Ausgabe:**

Die Zelle führen: 61 Sekunden.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Nutzen Sie Spark erstellt Learning Modelle mit Scala

Eine Übersicht über die Themen, die mit Vorgehensweisen durch die Aufgaben, die den Prozess Datenwissenschaft in Azure umfassen finden Sie unter [Team wissenschaftliche Daten](http://aka.ms/datascienceprocess).

[Exemplarische Vorgehensweisen für Team wissenschaftliche Daten](data-science-process-walkthroughs.md) beschreibt andere End-to-End-Schritte im Team wissenschaftliche Daten für bestimmte Szenarien exemplarischen Vorgehensweisen für. Die exemplarischen Vorgehensweisen veranschaulichen auch Cloud und lokalen Tools und Dienste in einen Workflow oder eine Pipeline erstellt eine intelligente Anwendung kombiniert.

[Bewertung Spark erstellt maschinelles lernen Modelle](machine-learning-data-science-spark-model-consumption.md) veranschaulicht mit Scala Code automatisch laden und bewerten neue Datensätze mit computerlernen Spark integriert und in Azure BLOB-Speicher gespeichert. Sie können Anweisungen vorhanden und mit Scala Code in diesem Artikel für den automatischen Verbrauch Python-Code ersetzen.
