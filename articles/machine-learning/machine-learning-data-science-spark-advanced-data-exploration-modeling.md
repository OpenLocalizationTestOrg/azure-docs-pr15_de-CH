<properties
    pageTitle="Erweiterte Daten durchsuchen und mit Spark | Microsoft Azure"
    description="Verwenden Sie HDInsight Spark Durchsuchen von Daten und binäre Klassifizierung und Regression Modelle Optimierung übergreifende Überprüfung und Hyperparameter Schulen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Erweitertes Durchsuchen von Daten und Modellierung mit Funken 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Diese exemplarische Vorgehensweise verwendet HDInsight Spark Durchsuchen von Daten und Schulen binäre Klassifizierung und regressionsmodelle übergreifende Überprüfung und Hyperparameter einer Stichprobe der NYC taxi Reise und Tarif 2013 Dataset. Es führt Sie durch die Schritte der [Wissenschaftlichen Daten](http://aka.ms/datascienceprocess), End-to-End mit einer HDInsight Spark cluster-Verarbeitung und Azure-blobs zum Speichern der Daten und die Modelle. Untersucht und Visualisierung von Daten ein BLOB Azure Storage gebracht und bereitet die Daten Vorhersagemodelle erstellen. Python wurde die Projektmappe code und die entsprechenden Plots verwendet. Diese Modelle sind mit dem Spark MLlib Toolkit für binäre Klassifizierung und Regression Modellierungsaufgaben. 

- **Binäre Klassifizierung** soll oder ob ein für die Reise Trinkgeld vorhergesagt. 
- **Regression** soll an der Spitze basierend auf anderen Tip-Funktionen vorherzusagen. 

Die Modellierung Schritte enthalten auch Code zum Trainieren, auswerten und speichern Sie jeden Modell. Das Thema umfasst einige Masse wie das [Durchsuchen von Daten und mit Spark](machine-learning-data-science-spark-data-exploration-modeling.md) Thema. Aber es ist "Erweiterte", auch übergreifende Überprüfung in Verbindung mit Hyperparameter kehren zum Trainieren optimal Klassifizierung und Regression Modelle verwendet. 

**Übergreifende Überprüfung (CV)** ist ein Verfahren, das wie ein Modell auf einem bekannten Satz von Daten trainiert verallgemeinert Vorhersage der Funktionen von Datasets auf denen es nicht trainiert wurde analysiert. Der Grundgedanke hinter dieser Technik ist, dass ein Modell auf ein Dataset bekannten Daten trainiert und dann die Genauigkeit der Prognosen mit einem unabhängigen Dataset getestet. Eine allgemeine Implementierung wird ein Dataset in K Falten und Trainieren Sie das Modell eine Roundrobin auf alle Falten. 

**Hyperparameter Optimierung** ist das Problem der einen Satz von hyperparameter für Learning-Algorithmus normalerweise mit dem Ziel der Optimierung der Leistung der Algorithmus auf eine unabhängige auswählen. **Hyperparameter** sind Werte, die außerhalb der Prozedur Modell Schulung müssen angegeben werden. Annahmen über diese Werte können die Flexibilität und die Genauigkeit des Modells auswirken. Entscheidungsstrukturen haben hyperparameter, z. B. wie die gewünschte Tiefe und Anzahl der Blätter in der Struktur. Vektor Maschinen (SVMs) erfordern einen falschen Klassifizierung Sanktion Begriff festlegen. 

Hyperparameter Optimierung hier ausführen wird eine Raster, oder einem **Parameter-Sweep**. Dies umfasst das Durchführen einer umfassenden Suche durch die Werte einer angegebenen Teilmenge von Hyperparameter Platz für Learning-Algorithmus. Übergreifende Überprüfung liefern Leistungsmetrik zu sortieren, die optimale Ergebnisse Suchalgorithmus Raster. KA mit weitreichenden hilft Limit Probleme wie überanpassung ein Modell Daten, damit das Modell behält die Fähigkeit, auf die allgemeinen Daten aus dem Daten extrahiert wurde Hyperparameter verwendet.

Die Modelle verwenden wir sind Logistik und lineare Regression, zufällige Wälder und Farbverlauf stärkere Strukturen:

- [Lineare Regression mit EUR](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) ist eine lineare Regressionsmodell, das eine Methode Stochastic Farbverlauf Abstieg (EUR) verwendet und für Optimierung und Skalierung Vorhersagen Tipp Beträge gezahlt. 
- [Logistische Regression mit LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) oder "Logit" Regression ist ein Regressionsmodell verwendet werden kann, wird die abhängige Variable kategorischen Daten führen. LBFGS ist ein quasi-Newton Optimierungsalgorithmus nahe kommt des Broyden – Fletcher – Goldfarb – Shanno (BFGS)-Algorithmus mit einer begrenzten Menge von Arbeitsspeicher und in maschinelles lernen weit verbreitet ist.
- [Random](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) sind Ensembles Entscheidungsstrukturen.  Verbinden sie viele Entscheidungsstrukturen, um das Risiko von überanpassung. Zufällige Wälder Regression und verwendet können kategorische Features und multiklassenklassifizierung Einstellung erweitert werden. Sie benötigen keine Funktion skalieren und können Nichtlinearitäten erfassen und Aktivitäten. Zufällige Gesamtstrukturen sind einer der erfolgreichsten Modelle für die Klassifikation und Regression lernen.
- [Farbverlauf verstärkt Strukturen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (AGB) sind Ensembles Entscheidungsstrukturen. AGB Schulen Entscheidungsstrukturen iterativ zu einem verlustfunktion. AGB Regression und verwendet kategorische Features können, erfordern keine Funktion skalieren und können Nichtlinearitäten erfassen und Aktivitäten. Sie können auch in einem mehrfachklasse Klassifizierung verwendet werden.

Beispiele CV und Hyperparameter Modeling Sweep erscheinen für binäre klassifizierungsproblem. Einfacher sind (ohne Parameter abtastet) im Thema Regression Aufgaben Beispiele. Sondern in der Anlage Verwendung elastische für lineare Regression und Lebenslauf mit Parameter Sweep für zufällige Gesamtstruktur Regression Validierung auch. **Elastische Net** ist eine Regression regularisierte Methode für lineare regressionsmodelle Formstück, die Metriken L1 und L2 linear Sanktionen [Lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) und [3D-Linie](https://en.wikipedia.org/wiki/Tikhonov_regularization) kombiniert.   



>[AZURE.NOTE] Zwar Spark MLlib Toolkit bei großen Datasets arbeiten, ist hier ein Beispiel relativ kleines (ca. 30 Mb mit 0,1 % des ursprünglichen NYC Dataset 170 K Zeilen) zur Vereinfachung verwendet. Übung hier führt ein HDInsight Cluster mit 2 Knoten Arbeitskraft effizient (in etwa 10 Minuten). Der gleiche Code mit geringfügigen Änderungen kann zu größeren Datasets mit entsprechenden Daten im Arbeitsspeicher Zwischenspeichern und ändern die Clustergröße verwendet werden.


## <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen ein Azure-Konto und ein HDInsight Spark Sie müssen eine HDInsight 3.4 Spark 1.6 Cluster zum Durchführen dieser exemplarischen Vorgehensweise. [Übersicht über Datenwissenschaft Spark auf Azure HDInsight mit](machine-learning-data-science-spark-overview.md) Informationen wie diese Anforderungen anzeigen Das Thema enthält auch eine Beschreibung der NYC 2013 Taxi Daten hier verwendet und wie Sie Code in ein Jupyter Notebook Spark-Cluster ausführen. **PySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb** Notizbuch, das die Codebeispiele in diesem Thema enthält, ist in [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)verfügbar.


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Setup: Speicherorte, Bibliotheken und den voreingestellten Spark-Kontext

Spark kann zum Lesen und Schreiben in Azure Storage Blob (auch bekannt als WASB). Vorhandene Daten gespeichert kann es Spark mit erneut in WASB gespeicherten Ergebnisse verarbeitet werden.

Um Modelle oder Dateien in WASB zu speichern, muss der Pfad korrekt angegeben werden. Standardcontainer an der Spark-Cluster kann mit einem Pfad, beginnend mit verwiesen werden: "Wasb: / /". Orte verweist "Wasb: / /".

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Verzeichnispfade für Lagerorte in WASB festgelegt

Das folgende Codebeispiel gibt den Speicherort der zu lesenden Daten und den Pfad für das Modell Speicherverzeichnis Modell Ausgabe gespeichert:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**AUSGABE**

DateTime.DateTime (2016 4, 18, 17, 36, 27, 832799)


### <a name="import-libraries"></a>Importbibliotheken

Importieren Sie erforderlichen Bibliotheken durch folgenden Code:

    # LOAD PYSPARK LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime
    

### <a name="preset-spark-context-and-pyspark-magics"></a>Voreingestellte Spark Kontext und PySpark Magie

PySpark-Kernels, die mit Jupyter Notebooks haben vordefinierte. So müssen nicht den Funken festgelegt oder Struktur Kontexte explizit vor dem Starten der Anwendung arbeiten Sie entwickeln. Diese Kontexte sind standardmäßig verfügbar. Diese Kontexte sind:

- SC - Spark 
- zur SqlContext - für Struktur

PySpark Kernel stellt einige vordefinierte "zaubert" sind spezielle Befehle aufrufen mit %%. Es gibt zwei Befehle, die in den Codebeispielen verwendet werden.

- **%% lokale** Gibt an, dass der Code in den folgenden Zeilen lokal ausgeführt werden. Code muss gültige Python-Code.
- **%%sql -o <variable name>** Führt die SqlContext-Struktur Abfragen. -O-Parameter übergeben wird, wird das Ergebnis der Abfrage im beibehalten der %% lokalen Python Kontext als Panda Datensatz.
 

Für Weitere Informationen-Kernels für Jupyter Notebooks und vordefinierten "die zaubert" diese bereitgestellt wird, finden Sie unter [für Jupyter Notebooks für HDInsight mit HDInsight Spark Linux Kernels](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="data-ingestion-from-public-blob"></a>Einnahme von BLOBs für Öffentliche Daten: 

Zunächst wissenschaftliche Daten werden aus Quellen Analyse in Ihrem Durchsuchen von Daten und Modell-Umgebung Speicherort Umwandlung. Diese Umgebung ist Spark in dieser exemplarischen Vorgehensweise. Dieser Abschnitt enthält den Code, um eine Reihe von Aufgaben:

- Aufnahme Datenbeispiel modelliert werden
- Lesen des Eingabe-Datasets (gespeichert als TSV-Datei)
- Formatieren und die Daten bereinigen
- Erstellen und Objekte (RDDs oder Daten-Frames) im Arbeitsspeicher Zwischenspeichern
- Registrieren Sie als Temp-Tabelle in SQL-Kontext.

Hier ist der Code für die Aufnahme von Daten.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
        
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Ausführung über Zelle: 276.62 Sekunden


## <a name="data-exploration--visualization"></a>Durchsuchen von Daten und Visualisierung 

Sobald Daten in Spark gebracht wurde, besteht der nächste Schritt in der wissenschaftlichen Daten zu Verständnis der Daten durch Untersuchung und Visualisierung. In diesem Abschnitt werden Taxi Daten mithilfe von SQL-Abfragen überprüfen und Zielvariablen und künftige Features für visuelle Inspektion zu zeichnen. Insbesondere zeichnen wir die Häufigkeit der Fluggast zählt Taxi Trips Häufigkeit Tipp Beträge und Tipps wie Zahlungsbetrag und Typ variieren.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Zeichnen Sie ein Histogramm der Fluggast Anzahl Frequenzen im Taxi Trips-Beispiel

Dieser Code und die nachfolgenden Codeausschnitt verwenden SQL Magic zum Beispiel und lokalen Magic zur Darstellung der Daten.

- **SQL-Magic (`%%sql`)** HDInsight PySpark Kernel unterstützt der SqlContext-einfach Inline-HiveQL Abfragen. Die (-o VARIABLE_NAME)-Argument behält die Ausgabe der SQL-Abfrage als eine Panda-Datensatz auf dem Server Jupyter. Dies bedeutet, dass es im lokalen Modus verfügbar ist.
- Die ** `%%local` Magic** auszuführenden Code lokal auf dem Server Jupyter die Hauptknoten HDInsight Cluster verwendet. Normalerweise verwenden Sie `%%local` Magic zusammen mit den `%%sql` mit o - Parameter. Der Parameter-o würde die Ausgabe der SQL-Abfrage lokal beibehalten und %% lokalen Magic auslösen würde den nächsten Satz von Codeausschnitt lokal für die Ausgabe von SQL-Abfragen ausführen, die lokal gespeichert werden

Ausgabe wird automatisch angezeigt, nachdem der Code ausgeführt.

Diese Abfrage ruft Reisen nach Anzahl der Personenkraftwagen. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Dieser Code erstellt einen lokalen Datenrahmen aus der Abfrageausgabe und zeichnet die Daten. Die `%%local` Magic erstellt einen lokalen Daten-Frame, `sqlResults`, zum Plotten mit Matplotlib verwendet werden kann. 

>[AZURE.NOTE] Das Geheimnis PySpark ist mehrfach in dieser exemplarischen Vorgehensweise verwendet. Wenn die Datenmenge groß ist, probieren Sie zum Erstellen eines Datenrahmens, der passen im lokalen Speicher.


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Hier ist der Code zum Schleifen von Personenkraftwagen zählt zeichnen

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**AUSGABE**

![Häufigkeit der Besuche nach Anzahl der Fahrgäste](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

Sie können mithilfe **der Schaltflächen im Menü** im Notizbuch unter verschiedene Visualisierung (Tabelle, Kreis-, Linien-, oder Bar) auswählen. Bar-Plot wird hier angezeigt.


### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Zeichnen Sie ein Histogramm Tipp und wie Tipp anhand Fluggast Anzahl und Preis variiert.

Verwenden Sie eine SQL-Abfrage an Beispieldaten...
    
    # SQL SQUERY
    %%sql -q -o sqlResults
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train 
        WHERE passenger_count > 0 
        AND passenger_count < 7
        AND fare_amount > 0 
        AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 
        AND tip_amount < 25
    

Dieser Code Zelle mithilfe die SQL-Abfrage drei Grundstücke Daten erstellt.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    
    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()
    

**AUSGABE:** 

![Tipp Verteilung](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tipp Betrag nach Anzahl der Fahrgäste](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tipp Betrag Tarif Betrag](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Feature Engineering, Transformations- und Vorbereitung für die Modellierung

Dieser Abschnitt beschreibt und enthält den Code für das Verfahren zur Vorbereitung von Daten für die Verwendung in ML Modellierung. Es veranschaulicht die folgenden Aufgaben ausführen:

- Binning Stunden in Verkehr Perioden erstellen Sie neu
- Indizieren und auf hot kategorische Funktionen codieren
- Erstellen Sie beschriftete Point-Objekte für die Eingabe in ML Funktionen
- Erstellen Sie einer untergeordneten Stichproben der Daten und Ausbildung und Prüfung legt Teilen
- Featureskalierung
- Cacheobjekte im Arbeitsspeicher


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Binning Stunden in Verkehr Perioden erstellen Sie neu

Dieser Code veranschaulicht neu binning Stunden in Verkehr Perioden erstellen und den resultierenden Datenrahmen im Arbeitsspeicher zwischengespeichert. Stabil verteilt Datasets (RDDs) und Data Frames wiederholt verwendet werden, führt das Zwischenspeichern, verbesserte Ausführungszeiten. Daher zwischengespeichert RDDs und Daten-Frames in verschiedenen Phasen in der exemplarischen Vorgehensweise.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**AUSGABE**

126050


### <a name="index-and-one-hot-encode-categorical-features"></a>Index und eine hot kategorische Funktionen codieren

Dieser Abschnitt veranschaulicht das Indizieren oder codieren kategorische Funktionen für Eingabe der Funktionen. Die Modellierung und MLlib-Funktionen erfordern mit input Kategoriedaten indiziert oder vor der Verwendung codiert werden. 

Je nach müssen index oder auf verschiedene Weise codieren. Logistik und lineare Regression Modelle erfordern beispielsweise eine hot-Codierung, z. B. eine Funktion mit drei Kategorien in drei Funktion Spalten jeder mit 0 oder 1, je nach einer Beobachtung erweitert werden; MLlib bietet [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) um eine hot-Codierung. Diese Encoder Spalte binären Vektoren mit höchstens einer One-Wert eine Spalte Beschriftung Indizes zugeordnet. Diese Codierung kann Algorithmen, die erwarten numerische Werte Funktionen wie logistische Regression kategorische Funktionen angewendet werden.

Hier ist der Code indiziert und codieren kategorische Features:


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Ausführung über Zelle: 3.14 Sekunden


### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Erstellen Sie beschriftete Point-Objekte für die Eingabe in ML Funktionen

Dieser Abschnitt enthält Code, der zum index kategorischen Textdaten als Datentyp bezeichneten Punkt und codiert wird, sodass verwendet und logistische Regression MLlib und andere klassifizierungsmodelle testen können. Beschriftete Point-Objekte sind stabil verteilt Datasets (RDD) auf eine Weise, die von den meisten MLlib ML Algorithmen als Eingabedaten benötigt wird formatiert. Ein [Punkt bezeichnet](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) ist einer lokalen Vektor dicht oder gering gefüllten, Bezeichnung/Antwort.

Hier ist der Code zum Indizieren und Texteigenschaften für binäre Klassifizierung codieren.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Hier ist der Code codieren und kategorisierten Textfeatures für lineare Regression.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Erstellen Sie einer untergeordneten Stichproben der Daten und Ausbildung und Prüfung legt Teilen

Dieser Code erstellt eine Stichproben der Daten (25 % wird hier verwendet). Obwohl es nicht erforderlich, beispielsweise aufgrund der Größe des Datasets ist, zeigen wir, wie Sie hier aufnehmen können also Verwendung für eigene Problem bei Bedarf. Wenn Beispiele groß sind, kann dies viel Zeit beim trainingsmodelle speichern. Weiter teilen wir das Beispiel einer Schulung Teil (75 %) und testen (25 %) Einstufung und Regression Modellierung.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand
    
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

Ausführung über Zelle: 0,31 Sekunden


### <a name="feature-scaling"></a>Featureskalierung

Featureskalierung, auch bekannt als datennormalisierung von sichergestellt, dass Funktionen mit weit verstreuten sind nicht angegebene übermäßig wiegen in der Zielfunktion. Der Code für die Funktion Skalierung mithilfe [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) Features Einheit Varianz skaliert. Es ist von MLlib für lineare Regression mit Stochastic Farbverlauf Abstieg (EUR), beliebte Algorithmus zum Trainieren einer Vielzahl von anderen Computer lernen wie regularisierte Regressionen oder Vektor Maschinen (SVM) bereitgestellt.   

>[AZURE.TIP] LinearRegressionWithSGD Algorithmus auf Skalierung Funktion gefunden.   

Hier ist der Code zum Skalieren Variablen mit den regularisierte linear EUR.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

Ausführung über Zelle: 11.67 Sekunden


### <a name="cache-objects-in-memory"></a>Cacheobjekte im Arbeitsspeicher

Die Zeit für Ausbildung und Prüfung von ML Algorithmen werden durch Zwischenspeichern Eingabedaten Frame Objekte zur Klassifizierung, Regression und skaliert features

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE** 

Ausführung über Zelle: 0,13 Sekunden


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Vorhersagen Sie, ob ein Trinkgeld mit binären Klassifizierung

Dieser Abschnitt zeigt die Aufgabe binäre Klassifizierung Vorhersage oder ob ein für eine Taxi Trinkgeld wie mit drei Modelle. Modelle sind:

- Logistische regression 
- Zufällige Gesamtstruktur
- Farbverlauf Steigerung Strukturen

Jedes Modell erstellen Codeabschnitt ist in Schritte unterteilt: 

1. **Modell** mit einem Parameter Daten
2. **Auswerten des** auf einem Test mit Metriken
3. Im Blob für zukünftige Verbrauch **Modell speichern**

Wir zeigen wie übergreifende Überprüfung (CV) mit Parameter kehren auf zwei Arten:

1. Ein Algorithmus legt mit **generischen** benutzerdefinierten die jeder Algorithmus in MLlib und Parameter angewendet werden können. 
1. **PySpark CrossValidator Pipeline-Funktion**verwenden. Beachten Sie, dass zwar bequem, unsere Erfahrung CrossValidator Spark 1.5.0 einige Nachteile: 

    - Pipelinemodelle für zukünftige Verbrauch gespeichert/beibehalten nicht möglich.
    - Kann für jeden Parameter in einem Modell verwendet werden.
    - Kann jede MLlib-Algorithmus verwendet werden.


### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Generische Überprüfung und Hyperparameter kehren mit logistic Regression-Algorithmus für binäre Klassifizierung verwendet

Der Code in diesem Abschnitt veranschaulicht die Schulen, auswerten und logistische Regressionsmodell mit [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) das voraussagt, ob ein für eine Fahrt in NYC Taxi Reise und Tarif Dataset Trinkgeld speichern. Modell wird trainiert mit Validierung (CV) und Hyperparameter kehren implementiert mit benutzerdefiniertem Code zu lernalgorithmen in MLlib angewendet werden kann.   

>[AZURE.NOTE] Die Ausführung dieses benutzerdefinierten Codes KA dauert einige Minuten.

**Trainieren Sie das logistische Regressionsmodell Ka mit Hyperparameter ziehen**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    
    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)
    
    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric
    
    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
        
    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)
    
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))
    
    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Koeffizienten: [0.0082065285375,-0.0223675576104,-0.0183812028036, 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Abfangen:-0.0111216486893

Ausführung über Zelle: 14.43 Sekunden


**Auswerten des klassifizierungsmodells binäre mit standard-Metriken**

Der Code in diesem Abschnitt veranschaulicht eine logistische Regression gegen eine Test-Datensatz, einschließlich Zeichnen der Kurve ROC Auswertung.


    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Bereich unter PR = 0.985336538462

Bereich unter ROC = 0.983383274312

Zusammenfassende Statistiken

Genauigkeit = 0.984174341679

Zurückrufen = 0.984174341679

F1 Gäste = 0.984174341679

Ausführung über Zelle: 2,67 Sekunden


**ROC Kurve zu zeichnen.**

*PredictionAndLabelsDF* wird als *Tmp_results*in die vorherige Zelle einer Tabelle erfasst. *Tmp_results* kann verwendet werden, führen Sie Abfragen und Ergebnisse in Datenrahmen SqlResults zum Zeichnen. Hier ist der Code.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Hier ist der Code Vorhersagen und ROC-Kurve.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc
    
    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    # PLOT ROC CURVES
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
    

**AUSGABE**

![Logistische Regression ROC Kurve für generische Ansatz](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)


**Modell in ein Blob für zukünftige Verbrauch beibehalten**

Der Code in diesem Abschnitt veranschaulicht die logistische Regressionsmodell für Verbrauch zu speichern.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    
    logitBest.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**AUSGABE**

Ausführung über Zelle: 34.57 Sekunden


### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>Funktion des MLlib CrossValidator Rohrleitung mit logistische Regression (elastische Regression)

Der Code in diesem Abschnitt veranschaulicht die Schulen, auswerten und logistische Regressionsmodell mit [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) das voraussagt, ob ein für eine Fahrt in NYC Taxi Reise und Tarif Dataset Trinkgeld speichern. Das Modell ist mit Validierung (CV) und implementiert die Pipeline mit MLlib CrossValidator für Ka mit Parameter Hyperparameter kehren trainiert.   


>[AZURE.NOTE] Die Ausführung dieses Codes MLlib Lebenslauf kann mehrere Minuten dauern.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc
    
    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**AUSGABE**

Ausführung über Zelle: 107.98 Sekunden


**ROC Kurve zu zeichnen.**

*PredictionAndLabelsDF* wird als *Tmp_results*in die vorherige Zelle einer Tabelle erfasst. *Tmp_results* kann verwendet werden, führen Sie Abfragen und Ergebnisse in Datenrahmen SqlResults zum Zeichnen. Hier ist der Code.


    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Hier ist der Code, die ROC Kurve zeichnen.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc
    
    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    #PLOT
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


**AUSGABE**

![Logistische Regression ROC Kurve mit Hilfe MLlibs CrossValidator](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)



### <a name="random-forest-classification"></a>Zufällige Gesamtstruktur Klassifizierung

Der Code in diesem Abschnitt veranschaulicht die Schulen, auswerten und eine zufällige Gesamtstruktur Regression, die voraussagt, ob ein für eine Fahrt in NYC Taxi Reise und Tarif Dataset Trinkgeld speichern.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Bereich unter ROC = 0.985336538462

Ausführung über Zelle: 26.72 Sekunden


### <a name="gradient-boosting-trees-classification"></a>Farbverlauf Steigerung Strukturen Klassifizierung

Der Code in diesem Abschnitt veranschaulicht die Schulen, bewerten, einen Farbverlauf Steigerung Strukturen Modell das voraussagt, ob ein für eine Fahrt in NYC Taxi Reise Trinkgeld und Tarif Dataset.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # Area under ROC curve
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

Bereich unter ROC = 0.985336538462

Ausführung über Zelle: 28.13 Sekunden


## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Tipp Betrag (nicht mit CV) mit Regression vorhergesagt

Dieser Abschnitt zeigt eine Taxi basierend auf anderen Tip-Funktionen wie verwenden drei Modelle für die regressionsaufgabe an der Spitze Vorhersage bezahlt. Modelle sind:

- Lineare Regression regularisierte
- Zufällige Gesamtstruktur
- Farbverlauf Steigerung Strukturen

Diese Modelle wurden in der Einführung beschrieben. Jedes Modell erstellen Codeabschnitt ist in Schritte unterteilt: 

1. **Modell** mit einem Parameter Daten
2. **Auswerten des** auf einem Test mit Metriken
3. Im Blob für zukünftige Verbrauch **Modell speichern**   


>AZURE Hinweis: Übergreifende Überprüfung wird nicht verwendet drei Regression Modelle in diesem Abschnitt, da dies für die logistische regressionsmodelle detailliert dargestellt wurde. Ein Beispiel KA elastische Net für lineare Regression mit gemäß Anhang dieses Themas.


>AZURE Hinweis: Unserer Erfahrung können werden Probleme mit LinearRegressionWithSGD Modelle und Parameter muss auf ein gültiges Modell erhalten geändert/optimiert werden. Skalierung der Variablen erheblich erleichtert Konvergenz. Elastische net Regression, angezeigt in der Anlage zu diesem Thema kann auch anstelle von LinearRegressionWithSGD verwendet werden.


### <a name="linear-regression-with-sgd"></a>Lineare Regression mit EUR

Der Code in diesem Abschnitt werden mit skalierten lineare Regression Schulen, die stochastische Farbverlauf Abstieg (EUR) für die Optimierung verwendet, und zum Bewerten, auswerten und speichern Sie das Modell in Azure BLOB-Speicher (WASB)

>[AZURE.TIP] Erfahrungsgemäß kann Probleme mit der LinearRegressionWithSGD-Modelle und Parameter muss auf ein gültiges Modell erhalten geändert/optimiert werden. Skalierung der Variablen erheblich erleichtert Konvergenz.


    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

Koeffizienten: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

Abfangen: 0.854507624459

RMSE = 1.23485131376

Sqr R = 0.597963951127

Ausführung über Zelle: 38,62 Sekunden


### <a name="random-forest-regression"></a>Random-Gesamtstruktur regression

Der Code in diesem Abschnitt zeigt, wie Schulen, auswerten und Speichern einer zufälligen Gesamtstrukturmodell, die Spitze Betrag NYC Taxi Reise Daten vorhersagt.   

>[AZURE.NOTE] Übergreifende Überprüfung mit weitreichenden mit benutzerdefinierten Parameter gemäß Anhang.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE**

RMSE = 0.931981967875

Sqr R = 0.733445485802

Ausführung über Zelle: 25.98 Sekunden


### <a name="gradient-boosting-trees-regression"></a>Farbverlauf Steigerung Strukturen regression

Der Code in diesem Abschnitt veranschaulicht die Schulen, auswerten und ein Farbverlauf Steigerung Strukturen Modell, die Spitze Betrag NYC Taxi Reise Daten vorhersagt.

**Schulen und bewerten**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

RMSE = 0.928172197114

Sqr R = 0.732680354389

Ausführung über Zelle: 20,9 Sekunden


**Zeichnen**
    
*Tmp_results* wird als Tabelle Struktur in die vorherige Zelle registriert. Ergebnisse aus der Tabelle werden in den *SqlResults* Datenrahmen zum Plotten ausgegeben. Hier ist der code

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Hier ist der Code zur Darstellung der Daten mit dem Server Jupyter.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np
    
    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Tatsächliche-Vs-vorhergesagt-Tipp-Beträge](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)


## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Anhang: Zusätzliche Regression Aufgaben Parameter Zugkörper mit Cross-Validierung

Dieser Anhang enthält Code wie elastische für lineare Regression Verwendung KA und wie KA mithilfe von benutzerdefiniertem Code für zufällige Gesamtstruktur Regression mit Parameter angezeigt.


### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Cross-Validierung mit elastischem Netz für lineare regression

Der Code in diesem Abschnitt zeigt wie elastische Net für lineare Regression mit Validierung überschreiten und Auswertung mit Testdaten.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    
    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 
    
    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])
    
    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)
    
    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

Ausführung über Zelle: 161.21 Sekunden

**Mit R SQR auswerten**

*Tmp_results* wird als Tabelle Struktur in die vorherige Zelle registriert. Ergebnisse aus der Tabelle werden in den *SqlResults* Datenrahmen zum Plotten ausgegeben. Hier ist der code

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Hier ist der Code R Sqr berechnet.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats
    
    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**AUSGABE**

Sqr R = 0.619184907088


### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Cross-Validierung mithilfe von benutzerdefiniertem Code für zufällige Gesamtstruktur Regression mit parameter

Der Code in diesem Abschnitt zeigt wie Validierung mithilfe von benutzerdefiniertem Code für zufällige Gesamtstruktur Regression mit Parameter überschreiten und Auswertung mit Testdaten.


    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid
    
    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))
    
    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric
    
    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
            
    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE**

RMSE = 0.906972198262

Sqr R = 0.740751197012

Ausführung über Zelle: 69.17 Sekunden


### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Bereinigen von Objekten aus dem Speicher und Druckmodell Speicherorte

Mit `unpersist()` im Arbeitsspeicher zwischengespeicherten Objekte löschen.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


**AUSGABE**

PythonRDD [122] bei RDD am PythonRDD.scala: 43


**Ausdruck Pfad zu Dateien im Notizbuch Verbrauch verwendet werden.** Verwenden und einen unabhängigen Datensatz erhalten, müssen Sie kopieren und fügen Sie diese Namen in der Arbeitsmappe"Verbrauch".


    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**AUSGABE**

LogisticRegFileLoc = ModelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

LinearRegFileLoc = ModelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

RandomForestClassificationFileLoc = ModelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

RandomForestRegFileLoc = ModelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = ModelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = ModelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>Was kommt als nächstes?

Erstellung Regression und Klassifizierung Modelle mit Spark-MlLib können Sie Informationen und Auswerten dieser Modelle.

**Verbrauch Modell:** Bewerten und bewerten die Klassifizierung und Regression Modelle in diesem Thema finden Sie unter [Bewertung und Spark erstellt Learning Modelle auswerten](machine-learning-data-science-spark-model-consumption.md).
