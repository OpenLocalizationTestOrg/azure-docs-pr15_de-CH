<properties
    pageTitle="Bewertungspunktestand Spark erstellt Learning Modelle | Microsoft Azure"
    description="Wie erzielen Learning Modelle, die in Azure BLOB-Speicher (WASB) gespeichert wurden."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="score-spark-built-machine-learning-models"></a>Bewertungspunktestand Spark erstellt Learning Modelle 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Dieses Thema beschreibt, wie Sie lernen (ML) Modelle laden, die mit Spark MLlib erstellt wurde und in Azure BLOB-Speicher (WASB) und erzielen sie mit Datasets, die auch im WASB gespeichert wurden gespeichert. Es zeigt wie vor Verarbeitung der Eingabedaten transformieren Features über die Indizierung und Codierung in den MLlib und ein Objekt mit der Bezeichnung Punkt erstellen, die als Eingabe für mit ML-Modellen verwendet werden kann. Zur Bewertung Modelle sind lineare Regression, logistische Regression, zufällige Gesamtstruktur Modelle und Farbverlauf Förderung Struktur Modelle.


## <a name="prerequisites"></a>Erforderliche Komponenten

1. Sie benötigen ein Azure-Konto und ein HDInsight Spark Sie müssen eine HDInsight 3.4 Spark 1.6 Cluster zum Durchführen dieser exemplarischen Vorgehensweise. [Übersicht über Datenwissenschaft Spark auf Azure HDInsight mit](machine-learning-data-science-spark-overview.md) Informationen wie diese Anforderungen anzeigen Das Thema enthält auch eine Beschreibung der NYC 2013 Taxi Daten hier verwendet und wie Sie Code in ein Jupyter Notebook Spark-Cluster ausführen. **PySpark-machine-learning-data-science-spark-model-consumption.ipynb** Notizbuch, das die Codebeispiele in diesem Thema enthält, ist in [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)verfügbar.

2. Sie müssen auch Computer lernen Modelle hier arbeitet das [Durchsuchen von Daten und mit Spark](machine-learning-data-science-spark-data-exploration-modeling.md) Thema erzielt erstellen.   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Setup: Speicherorte, Bibliotheken und den voreingestellten Spark-Kontext

Spark kann zum Lesen und Schreiben auf ein Azure Storage Blob (WASB). Vorhandene Daten gespeichert kann es Spark mit erneut in WASB gespeicherten Ergebnisse verarbeitet werden.

Um Modelle oder Dateien in WASB zu speichern, muss der Pfad korrekt angegeben werden. Standardcontainer an der Spark-Cluster kann mit einem Pfad, beginnend mit verwiesen: *"Wasb / /"*. Das folgende Codebeispiel gibt den Speicherort der zu lesenden Daten und den Pfad für das Modell Speicherverzeichnis Modell Ausgabe gespeichert wird. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Verzeichnispfade für Lagerorte in WASB festgelegt

Modelle werden: "Wasb: / / / Benutzer/Remoteuser/NYCTaxi/Modelle". Wenn dieser Pfad nicht richtig eingestellt ist, werden Modelle zur Bewertung nicht geladen.

Erzielte Ergebnisse wurden gespeichert: "Wasb: / / / Benutzer/Remoteuser/NYCTaxi/ScoredResults". Wenn der Pfad zum Ordner falsch ist, werden Ergebnisse nicht in diesem Ordner gespeichert.   


>[AZURE.NOTE] Speicherorte der Pfad können kopiert und Platzhalter in diesen Code aus der Ausgabe der letzten Zelle der **machine-learning-data-science-spark-data-exploration-modeling.ipynb** Notizbuch eingefügt werden.   


Hier ist der Code Verzeichnispfaden: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**AUSGABE:**

DateTime.DateTime (2016 4 25 23, 56, 19, 229403)


### <a name="import-libraries"></a>Importbibliotheken

Spark-Kontext festlegen und erforderlichen Bibliotheken mit dem folgenden Code importieren

    #IMPORT LIBRARIES
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

PySpark-Kernels, die mit Jupyter Notebooks haben vordefinierte. So müssen nicht den Funken festgelegt oder Struktur Kontexte explizit vor dem Starten der Anwendung arbeiten Sie entwickeln. Diese sind standardmäßig verfügbar. Diese Kontexte sind:

- SC - Spark 
- zur SqlContext - für Struktur

PySpark Kernel stellt einige vordefinierte "zaubert" sind spezielle Befehle aufrufen mit %%. Es gibt zwei Befehle, die in den Codebeispielen verwendet werden.

- **%% lokale** Angegebene Code in den folgenden Zeilen wird lokal ausgeführt. Code muss gültige Python-Code.
- **% Sql -o<variable name>** 
- Führt die SqlContext-Struktur Abfragen. -O-Parameter übergeben wird, wird das Ergebnis der Abfrage im beibehalten der %% lokalen Python Kontext als Panda Datensatz.
 

Für Weitere Informationen-Kernels für Jupyter Notebooks und vordefinierten "die zaubert" diese bereitgestellt wird, finden Sie unter [für Jupyter Notebooks für HDInsight mit HDInsight Spark Linux Kernels](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Daten einlesen und bereinigten Daten Rahmen

Dieser Abschnitt enthält den Code für eine Reihe von Aufgaben zur Aufnahme der Daten erzielt werden. In einem verknüpften 0,1 % der Taxi Reise und Tarif Datei (als TSV-Datei gespeichert) Format die Daten lesen und erstellt dann einen saubere Datenrahmen.

Taxi Reise und Tarif Dateien traten auf das Verfahren anhand der: [The Team wissenschaftliche Daten in Aktion: HDInsight Hadoop Cluster](machine-learning-data-science-process-hive-walkthrough.md) Thema.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
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
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE:**

Ausführung über Zelle: 46.37 Sekunden


## <a name="prepare-data-for-scoring-in-spark"></a>Vorbereiten von Daten für Spark bewerten 

Dieser Abschnitt veranschaulicht indizieren, codieren und Skalierung kategorische Funktionen zur Verwendung in MLlib überwacht Learning Algorithmen für die Einstufung und Regression Vorbereitung.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Funktion Transformation: index und codieren kategorische Funktionen für Eingabe Modelle zur Bewertung 

Dieser Abschnitt veranschaulicht die kategorische Daten mit index einen `StringIndexer` und Codieren mit `OneHotEncoder` in den Modellen eingeben.

[StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) codiert eine Zeichenfolgenspalte Etiketten in Spalte Beschriftung Indizes. Die Indizes werden nach Bezeichnung Frequenzen sortiert. 

[OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) Spalte binären Vektoren mit höchstens einer One-Wert eine Spalte Beschriftung Indizes zugeordnet. Diese Codierung kann Algorithmen, die kontinuierliche bewertet Features wie logistische Regression kategorische Features anzuwendenden erwarten.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
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
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE:**

Ausführung über Zelle: 5,37 Sekunden


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Erstellen Sie RDD Objekte mit Funktion für Eingabe Modelle

Dieser Abschnitt enthält Code, der veranschaulicht Indexdaten kategorischen Text als RDD-Objekt und ein hot codieren sie zu trainieren und logistische Regression MLlib und Struktur-Modellen verwendet werden kann. Die indizierten Daten werden [Stabil verteilt Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) Objekte gespeichert. Dies sind die grundlegende Abstraktion in Spark. Ein RDD-Objekt repräsentiert eine unveränderliche partitionierte Auflistung Elemente Spark parallel betrieben werden kann.

Enthält auch Code, der zeigt, wie Daten mit der `StandardScalar` von MLlib für die lineare Regression mit Stochastic Farbverlauf Abstieg (EUR), beliebte Algorithmus für eine Vielzahl von Learning Modelle Schulung bereitgestellt. [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) wird verwendet, um die Features Einheit Abweichung skalieren. Featureskalierung, auch bekannt als datennormalisierung von sichergestellt, dass Funktionen mit weit verstreuten sind nicht angegebene übermäßig wiegen in der Zielfunktion. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**AUSGABE:**

Ausführung über Zelle: 11.72 Sekunden


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Mit logistischen Regressionsmodells bewerten und Ausgabe BLOB speichern

Der Code in diesem Abschnitt veranschaulicht die Laden eines logistischen Regressionsmodells in Azure BLOB-Speicher gespeichert und Vorhersagen, ob ein Trinkgeld unterwegs Taxi, mit standardisierten Klassifizierung Kennzahlen Ergebnis speichern und zeichnen die Ergebnisse auf BLOB-Speicher verwenden. Erzielte Ergebnisse werden RDD Objekte gespeichert. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**AUSGABE:**

Ausführung über Zelle: 19.22 Sekunden


## <a name="score-a-linear-regression-model"></a>Lineare Regressionsmodell Punktzahl

[LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) verwendet, um eine lineare Regression Modell mit Stochastic Farbverlauf Abstieg (EUR) zur Optimierung an Trinkgeld vorherzusagen. 

Der Code in diesem Abschnitt veranschaulicht die lineare Regressionsmodell von Azure BLOB-Speicher laden, mit skalierten Variablen bewerten und speichern Sie die Ergebnisse zurück in den Blob.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**AUSGABE:**

Ausführung über Zelle: 16.63 Sekunden


## <a name="score-classification-and-regression-random-forest-models"></a>Klassifizierung und Regression zufällige Gesamtstruktur Modelle Punktzahl

Der Code in diesem Abschnitt veranschaulicht die gespeicherte Klassifizierung laden und Regression zufällige Gesamtstruktur Modelle in Azure BLOB-Speicher gespeichert mit standard Klassifizierung und Regression Maßnahmen bewerten und speichern Sie die Ergebnisse auf BLOB-Speicher.

[Random](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) sind Ensembles Entscheidungsstrukturen.  Verbinden sie viele Entscheidungsstrukturen, um das Risiko von überanpassung. Zufällige Gesamtstrukturen können kategorische Features Erweitern der multiklassenklassifizierung Einstellung erfordern keine Funktion skalieren und können Nichtlinearitäten erfassen und Aktivitäten. Zufällige Gesamtstrukturen sind einer der erfolgreichsten Modelle für die Klassifikation und Regression lernen.

[Spark.mllib](http://spark.apache.org/mllib/) unterstützt zufällige Gesamtstrukturen für binäre und multiclass Klassifizierung Regression mit kontinuierlichen und kategorisierten. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**AUSGABE:**

Ausführung über Zelle: 31.07 Sekunden


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Klassifizierung und Regression Farbverlauf Förderung Struktur Modelle Punktzahl

Der Code in diesem Abschnitt veranschaulicht die Klassifizierung und Regression Farbverlauf Förderung Struktur Modelle von Azure BLOB-Speicher geladen, mit standard Klassifizierung und Regression Maßnahmen bewerten und speichern Sie die Ergebnisse auf BLOB-Speicher. 

**Spark.mllib** unterstützt AGB binäre Klassifizierung Regression mit kontinuierlichen und kategorisierten. 

[Farbverlauf Steigerung Strukturen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (AGB) sind Ensembles Entscheidungsstrukturen. AGB Schulen Entscheidungsstrukturen iterativ zu einem verlustfunktion. AGB können kategorische Features, erfordern keine Funktion skalieren und können Nichtlinearitäten erfassen und Aktivitäten. Sie können auch in einem mehrfachklasse Klassifizierung verwendet werden.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**AUSGABE:**

Ausführung über Zelle: 14,6 Sekunden


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Bereinigen von Objekten aus dem Speicher und erzielte Dateispeicherorte drucken

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**AUSGABE:**

LogisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

LinearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

RandomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

RandomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Spark-Modelle über eine Webschnittstelle nutzen

Spark bietet einen Mechanismus zum Remote Stapelverarbeitungsaufträge oder interaktive Abfragen über eine REST-Schnittstelle mit einer Komponente Livius senden. Livius ist standardmäßig auf HDInsight Spark Cluster aktiviert. Finden Sie weitere Informationen zu Livius: [Spark senden Aufträge über Livius](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Livius können Remote senden einen Auftrag, der batch-Ergebnisse eine Datei, die in Azure Blob gespeichert und schreibt die Ergebnisse in einem anderen Blob. Dazu Laden Sie Python-Skript aus  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) BLOB Spark-Cluster. Ein Tool wie **Microsoft Azure Storage Explorer** oder **AzCopy** können Sie das Skript in der Cluster-Blob kopiert. In unserem Fall haben wir das Skript ***wasb:///example/python/ConsumeGBNYCReg.py***hochgeladen.   


>[AZURE.NOTE] Die Zugriffstasten, die Sie im Portal für den Cluster Spark Speicherkonto finden müssen. 


Nachdem in dieses Verzeichnis hochgeladen wird dieses Skript Spark-Cluster in einem verteilten Kontext ausgeführt. Lädt das Modell und Vorhersagen auf Basis des Modells Dateien ausgeführt.  

Sie können dieses Skript Livius eine HTTPS-REST-Anfrage zu Remote aufrufen.  Hier ist der Befehl Curl Python-Skript Remote Aufrufen die HTTP-Anforderung erstellt. Ersetzen Sie CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAMEN mit den entsprechenden Werten für den Cluster Spark.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Jede Sprache können auf dem Remotesystem Spark Auftrag über Livius aufrufen, indem eine HTTPS-Anruf mit Standardauthentifizierung.   


>[AZURE.NOTE] Wäre mit der Bibliothek Python Anfragen bei diesen HTTP-Aufruf, aber es ist zurzeit nicht installiert standardmäßig in Azure-Funktionen. Stattdessen werden so ältere HTTP-Bibliotheken verwendet.   


Hier werden Python-Code für HTTP-Aufruf:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Sie können auch diese Python-Code hinzufügen, [Azure Funktionen](https://azure.microsoft.com/documentation/services/functions/) eine Bewerbung Spark ausgelöst, dass Ergebnisse ein Blob auf verschiedene Ereignisse wie Timer, Erstellung oder Aktualisierung eines Blob. 

Verwenden Sie eine kostenlose Clientumgebung Code natürlich [Azure Logik Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) Spark Batch Bewertung durch eine HTTP-Aktion **Logik Apps Designer** definieren und dessen Parameter aufrufen. 

- Neue Logik App von Azure-Portal auswählen **+ neu**erstellen -> **Web + Mobile** -> **Logik App**. 
- Geben Sie den Namen der Logik App Service-Plan so **Logik Apps Designer**.
- Wählen Sie HTTP-Aktion aus, und geben Sie die Parameter in der folgenden Abbildung dargestellt:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Was kommt als nächstes? 

**Übergreifende Überprüfung und Hyperparameter ziehen**: finden Sie unter [Erweiterte Daten durchsuchen und mit Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) auf Modelle wie möglich mit Cross-Validierung und hyper-Parameter kehren geschult.
