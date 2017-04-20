<properties 
    pageTitle="Mit der Apache Spark Machine Learning Anwendung auf HDInsight | Microsoft Azure" 
    description="Anleitung zur Verwendung von Notebooks mit Apache Spark Machine Learning Applikationen erstellen" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>Maschinelles lernen: prognostische Analyse auf Food Inspection Apache Spark MLlib mit Daten HDInsight Linux cluster

> [AZURE.TIP] Dieses Lernprogramm ist auch als ein Jupyter Notebook in einem Cluster Spark (Linux), den im HDInsight erstellt. Notebook-Erfahrung können Sie die Python-Ausschnitte aus dem Notizbuch selbst ausführen. Gehen Sie die Einführung in ein Notizbuch Spark-Cluster erstellen, Jupyter Notebook starten (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), und führen Sie dann unter **Python** Ordner Notebook **Spark Machine Learning - Vorhersageanalysen Speisen Daten mit MLLib.ipynb** .


Dieser Artikel veranschaulicht, wie **MLLib**, Funken der integrierten Computer lernen Bibliotheken öffnen Datasets einfache Vorhersageanalysen durchzuführen. MLLib ist eine zentrale Spark-Bibliothek, die eine Reihe von Dienstprogrammen, die für Computer lernaufgaben für Dienstprogramme bereitstellt:

* Klassifizierung

* Regression

* Clustering

* Thema Modellierung

* Wert Zerlegung (SVD) und hauptkomponentenanalyse (PCA)

* Hypothese testen und Beispiel Statistiken berechnen

Dieser Artikel stellt einen einfachen Ansatz *Klassifizierung* durch logistische Regression.

## <a name="what-are-classification-and-logistic-regression"></a>Was sind Klassifizierung und logistische Regression?

*Klassifizierung*, eine sehr allgemeine maschinelles lernen Aufgabe versteht man das Sortieren von Daten in Kategorien. Es ist die Aufgabe eines klassifizierungsalgorithmus herauszufinden, wie "Labels", Daten einzugeben, die Sie bereitstellen. Beispielsweise könnte ein Computer Learning-Algorithmus, die Börsendaten als Eingabe akzeptiert und die Aktie in zwei Kategorien unterteilt: die Sie verkaufen und der beibehalten werden soll.

Logistische Regression wird der Algorithmus, den für die Klassifizierung verwenden. Logistische Regression Spark API ist nützlich für *binäre*oder Klassifizierung der Daten in zwei Gruppen. Weitere Informationen über logistische Regressionen anzeigen Sie [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression)

Zusammenfassend erzeugt der Prozess logistische Regression *Logistik* , mit der die Wahrscheinlichkeit Vorhersagen, dass ein Eingabe Vektor in einer Gruppe oder anderen gehört.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Was wir in diesem Artikel ausführen möchten?

Spark verwendet einige Vorhersageanalysen auf Lebensmittel Daten (**Food_Inspections1.csv**) ausführen, die über das [Portal zum von Chicago](https://data.cityofchicago.org/)erworben wurde. Dieses Dataset enthält Informationen über Lebensmittel Inspektionen, die in Chicago, einschließlich Informationen zu jeder Lebensmittel Betrieb, der überprüft wurde (falls vorhanden) gefundenen Verstöße und die Ergebnisse der Überprüfung durchgeführt wurden. Die CSV-Datei ist bereits in den Cluster auf **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**Speicherkonto verfügbar.

In den folgenden Schritten entwickeln Sie ein Modell sehen, was übergeben oder Food Inspection. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Erstellen einer Maschine Learning Anwendung Spark MLlib

1. [Azure-Portal](https://portal.azure.com/)aus dem Startmenü klicken Sie auf die Kachel Spark Cluster (Wenn Sie es an das Startmenü angeheftet). Sie können auch zum Cluster unter **Alle durchsuchen** > **HDInsight-Cluster**.   

2. Blade Cluster Spark auf **Cluster-Dashboard**und klicken Sie dann auf **Jupyter Notebook**. Wenn Sie aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster.

    > [AZURE.NOTE] Sie können Jupyter Notebook für den Cluster erreichen, durch die folgende URL in Ihrem Browser öffnen. Der Name des Clusters ersetzen Sie __CLUSTERNAME__ :
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen Sie ein neues Notizbuch. Klicken Sie auf **neu**, und klicken Sie auf **PySpark**.

    ![Erstellen einer neuen Jupyter notebook] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Erstellen einer neuen Jupyter notebook")

3. Ein neues Notizbuch erstellt und mit dem Namen Untitled.pynb geöffnet. Klicken Sie oben auf Notebook, und geben Sie einen Anzeigenamen.

    ![Geben Sie einen Namen für das Notizbuch] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Geben Sie einen Namen für das Notizbuch")

3. Da Sie ein Notebook mit dem PySpark Kernel erstellt, müssen Sie keine explizit erstellen. Kontexte Spark und Struktur werden zur Ausführung von Code Zelle automatisch erstellt. Beginnen Sie Ihre Maschine lernen Anwendung für dieses Szenario erforderlichen Typen importieren. Dazu platzieren Sie den Cursor in die Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Input Datensatz erstellen

Wir können `sqlContext` zum Ausführen von Transformationen für strukturierte Daten. Zunächst ist die Beispieldaten ((**Food_Inspections1.csv**)) in Spark SQL- *Datensatz*zu laden. 

1. Ist die unformatierten Daten im CSV-Format, müssen wir Spark-Kontext verwenden, um jede Zeile der Datei in den Speicher unstrukturierten Text ziehen. Anschließend verwenden Sie Python CSV-Bibliothek, jede Zeile einzeln analysieren. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. Wir haben jetzt die CSV-Datei als ein RDD. Wir rufen Sie eine Zeile von RDD Schema der Daten zu verstehen.


        inspections.take(1)


    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. Die obige Ausgabe gibt uns eine Vorstellung des Schemas der Eingabedatei; die Datei enthält den Namen des jede Niederlassung, den Betrieb, die Adresse, die Daten der Kontrollen und Position unter anderem. Wählen Sie einige Spalten, die für unsere Vorhersageanalysen und gruppiert die Ergebnisse als ein Datensatz, der es verwendet, um eine temporäre Tabelle erstellen.


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. Wir haben einen *Datensatz* `df` auf dem wir unsere Analyse durchführen. Wir haben auch eine temporäre Tabelle **CountResults**aufrufen. Wir haben 4 Spalten in dem Datensatz enthalten: **Id**, **Name**, **Ergebnisse**und **Verstöße**. 
    
    Wir erhalten eine kleine Auswahl der Daten:

        df.show(5)

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Verstehen der Daten

1. Wir beginnen, einen Eindruck davon, was unser Dataset enthält. Was sind beispielsweise verschiedene Werte in der Spalte **Ergebnisse** ?


        df.select('results').distinct().show()

    
    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Eine schnelle Visualisierung helfen uns Grund zur Verteilung dieser Ergebnisse. Wir haben bereits die Daten in einer temporären Tabelle **CountResults**. Ausführen die folgende SQL-Abfrage für die Tabelle besser zu verstehen, wie die Ergebnisse verteilt werden.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    Die `%%sql` Magic gefolgt von `-o countResultsdf` wird sichergestellt, dass die Ausgabe der Abfrage lokal auf dem Server Jupyter (in der Regel den Hauptknoten des Clusters) beibehalten wird. Die Ausgabe wird als ein [Panda](http://pandas.pydata.org/) Datensatz mit den angegebenen Namen **CountResultsdf**beibehalten.
    
    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:
    
    ![Ausgabe der SQL-Abfrage] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "Ausgabe der SQL-Abfrage")

    Weitere Informationen zu den `%%sql` Magic sowie andere mit Kernel PySpark Magie finden Sie [auf Jupyter Notebooks mit Spark HDInsight Kernel](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. Matplotlib einer Bibliothek erstellt Visualisierung von Daten können Sie eine Zeichnung erstellen. Da die Handlung von lokal gespeicherten **CountResultsdf** Datensatz erstellt werden muss, muss der Codeausschnitt mit beginnen die `%%local` Magic. Dies stellt sicher, dass der Code lokal auf dem Server Jupyter ausgeführt wird.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

    ![Ergebnisausgabe](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. Sie können sehen, dass es 5 unterschiedliche Ergebnisse eine haben:
    
    * Unternehmen nicht gefunden 
    * Fehler
    * Übergeben
    * PSS mit Auflagen und
    * Geschäftstätigkeit 

    Wir entwickeln Sie ein Modell, das das Ergebnis einer Überprüfung Lebensmittel erraten kann Verstöße angegeben. Logistische Regression binäre Klassifizierungsmethode ist, ist es sinnvoll, unsere Daten in zwei Kategorien gruppiert: **fehlschlagen** und **übergeben**. Ein "übergeben mit" ist noch eine Bahn, wenn wir des Modells trainieren wir zwei Ergebnissen entsprechende berücksichtigt. Daten mit anderen Ergebnissen ("Business nicht gefunden", "Out of Business") sind nicht nützlich, damit wir sie von unserem Trainingsmenge entfernen. Dies sollte kein Problem, da ein sehr kleiner Prozentsatz der Ergebnisse dieser beiden Kategorien trotzdem bilden.

5. Lassen Sie uns nun und unsere vorhandenen Datensatz konvertieren (`df`) in einen neuen Datensatz, wo jeder Kontrolle ein Label-Verstöße als wird. In diesem Fall ein `0.0` entspricht einem Ausfall ein `1.0` stellt einen Erfolg und ein `-1.0` einige Ergebnisse neben den beiden darstellt. Wir werden diese Ergebnisse herausfiltern, bei neuen Datenrahmen.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Wir rufen eine Zeile aus bezeichneten Daten, wie es aussieht.


        labeledData.take(1)


    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Erstellen Sie logistische Regressionsmodell aus der eingegebenen Datensatz

Unsere letzte Aufgabe ist die gekennzeichneten Daten in ein Format konvertieren, die logistische Regression analysiert werden können. Eingabe logistic Regression-Algorithmus sollte eine Reihe von *Label-Funktion Vector-Paare*ist "Feature Vektor" ein Vektor von Zahlen, die die Eingabe in irgendeiner Weise darstellt. Daher benötigen wir die Spalte "Verstöße" Konvertierung halbstrukturierten und enthält viele Kommentare in Freitext-, auf ein Array reeller Zahlen, die ein Computer ohne weiteres verstehen kann. 

Eine standard-Maschine für die Verarbeitung natürlicher Sprache lernen soll jedes distinct-Wort "Index" zuweisen und dann einen Vektor, den Algorithmus für maschinelles lernen jeden Indexwert die relative Häufigkeit des Worts in der Zeichenfolge enthält. 

MLLib bietet eine einfache Möglichkeit, diesen Vorgang auszuführen. Erstens wir werden "Token" Strings Verstöße zu einzelnen Wörtern in jeder Zeichenfolge und dann verwenden wir ein `HashingTF` jeden Satz von Token in einen Vektor Funktion konvertieren die logistische Regression-Algorithmus erstellt ein Modell übergeben werden können. Wir werden diese Schritte nacheinander mit "Pipeline" führen.


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Auswerten des Modells auf einem separaten dataset

Das Modell zuvor *vorherzusagen* welche Ergebnisse erstellte können neue Inspektionen werden basierend auf Verstöße festgestellt wurden. Wir geschult dieses DataSet **Food_Inspections1.csv**. Wir verwenden Sie ein zweites Dataset **Food_Inspections2.csv**die Stärke dieses Modells auf neue Daten *auszuwerten* . Diese zweite Datengruppe (**Food_Inspections2.csv**) sollte bereits im Speicher Standardcontainer Cluster zugeordnet.

1. Der folgende Codeausschnitt erstellt neue Datensatz **PredictionsDf** , die vom Modell generierte Vorhersage enthält. Der Codeausschnitt erstellt auch eine temporäre Tabelle **Vorhersagen** basierend auf den Datensatz.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Sehen Sie sich eine der Vorhersagen. Dieser Ausschnitt ausführen:

        predictionsDf.take(1)

    Sie sehen die Vorhersage für den ersten Eintrag in das Test-DataSet.

3. Die `model.transform()` -Methode wenden Sie dieselbe auf neuen Daten mit dem Schema und Vorhersage der Klassifizierung der Daten eintreffen. Können wir einige einfachen Statistiken zu einen Eindruck davon, wie genau Vorhersagen waren:


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Die Ausgabe sieht folgendermaßen aus:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Spark logistische Regression mit gibt uns ein genaues Modell der Beziehung zwischen Verstöße Beschreibung in Englisch und ob ein Unternehmen erfolgreich oder Fehler Food Inspection würde. 

## <a name="create-a-visual-representation-of-the-prediction"></a>Erstellen einer visuellen Darstellung der Vorhersage

Wir können nun eine endgültige Visualisierung dazu Grund über die Ergebnisse dieser Tests erstellen. 

1. Zunächst extrahiert die verschiedenen Vorhersagen und Ergebnisse aus der zuvor erstellten temporären **Vorhersagen** -Tabelle. Die folgenden Abfragen trennen die Ausgabe als *True_positive*, *False_positive*, *True_negative*und *False_negative*. In den folgenden Abfragen wir deaktivieren Visualisierung mit `-q` und speichern die Ausgabe (mit `-o`) als Dataframes mit verwendet werden kann die `%%local` Magic. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Schließlich verwenden Sie im folgenden Codeausschnitt generiert den Plot mit **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Die folgende Ausgabe sollte angezeigt werden.
    
    ![Vorhersage-Ausgabe](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    In diesem Diagramm bezieht sich "positiv" Überprüfung fehlgeschlagen Lebensmittel, während eine übergebene Überprüfung negativ bezieht.

## <a name="shut-down-the-notebook"></a>Fahren Sie das notebook

Nach der Ausführung der Anwendung sollten Sie Herunterfahren des Notebooks um die Ressourcen freizugeben. Klicken Sie dazu im Menü **Datei** auf das Notizbuch **Schließen und Anhalten**. Diese wird geschlossen und das Notizbuch schließen.


## <a name="seealso"></a>Siehe auch


* [Übersicht: Apache Spark auf Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Spark BI: Datenanalyse interaktive BI-Tools Spark in HDInsight mit](hdinsight-apache-spark-use-bi-tools.md)

* [Spark mit Computer: Funken im HDInsight für die Analyse erstellen Temperatur HKL-Daten verwenden](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark Streaming: Verwendung Funken im HDInsight zum Erstellen von Echtzeit-streaming](hdinsight-apache-spark-eventhub-streaming.md)

* [Websiteanalyse mit Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen der Anwendung

* [Erstellen Sie eine eigenständige Anwendung Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Führen Sie Aufträge auf einem Spark-Cluster mit Livius Remote aus](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterung

* [Verwenden Sie HDInsight Tools Plugin für IntelliJ IDEA erstellen und übermitteln Spark Scala Programme](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Mit HDInsight Tools Plugin IntelliJ Idee Remotedebugging Spark-Applikationen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Verwenden Sie Zeppelin Notebooks mit einem Cluster Spark HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Cluster-Kernels für Jupyter Notebook Spark für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Verwenden Sie externe Pakete mit Jupyter notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter auf dem Computer installieren und Verbinden mit einem HDInsight Spark-cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Ressourcen Sie für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debug Aufträge in einem Apache Spark-Cluster HDInsight](hdinsight-apache-spark-job-debugging.md)
