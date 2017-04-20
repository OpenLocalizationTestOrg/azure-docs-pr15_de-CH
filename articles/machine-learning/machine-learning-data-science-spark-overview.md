<properties
    pageTitle="Übersicht der Daten über Spark Azure HDInsight | Microsoft Azure"
    description="Spark MLlib Toolkit bringt erhebliche maschinelles lernen Modellierungsfunktionen verteilten HDInsight Umgebung."
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

# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Übersicht der Azure HDInsight Spark über Daten

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Diese Suite Themen gezeigt, wie HDInsight Spark Daten wissenschaftliche Aufgaben wie Daten Aufnahme, Feature Engineering, Modellierung und auswerten abgeschlossen. Die verwendeten Daten ist ein Beispiel für 2013 NYC Taxi Reise und Tarif Dataset. Die Modelle umfassen Logistik und lineare Regression, zufällige Wälder und Farbverlauf stärkere Strukturen. Die Themen zeigen auch wie diese Modelle in Azure BLOB-Speicher (WASB) und wie und Bewertung ihrer vorhersehbare Leistung. Erweiterte Themen wie Modelle sind mit Cross-Validierung und hyper-Parameter kehren geschult. Diese Übersicht beschreibt auch Spark-Cluster einrichten, die Schritte in den drei exemplarischen Vorgehensweisen werden müssen. 

[Spark](http://spark.apache.org/) ist ein Open Source-parallele Verarbeitung Rahmen, die Verarbeitung im Speicher zur Leistungssteigerung big Data analytische Applikationen unterstützt. Spark Verarbeitungsmodul ist für einfachen und komplexen Analytics erstellt. Spark verteilte Berechnung speicherresidenten Funktionen machen es eine gute Wahl für iterative Algorithmen Computer lernen und Graph berechnet. [MLlib](http://spark.apache.org/mllib/) ist Spark skalierbare Computer lernen, die diese verteilten Umgebung Modellierungsfunktionen bringt. 

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) ist der Azure gehosteten Opensource-Funken. Sie umfasst auch Unterstützung für **Jupyter PySpark Notebooks** Spark-Cluster, die interaktive Abfragen transformieren, Filtern und Visualisieren von Daten in Azure-Blobs (WASB) gespeichert Spark SQL ausführen können. PySpark ist Python Spark. Die Codeausschnitte, die Lösungen anzeigen relevanten Flächen die Daten hier Jupyter Notebooks installiert Spark-Cluster ausführen. Die Modellierung Schritte in diesen Themen enthalten Code, der zeigt, wie Schulen, bewerten, speichern und jede Art von Modell verwenden. 

Die Schritte in dieser exemplarischen Vorgehensweise bereitgestellt ist für HDInsight 3.4 Spark 1.6. Jedoch der Code hier die Notebooks ist generisch und Spark-Cluster arbeiten sollten. Wenn HDInsight Spark, dem Cluster-Setup nicht verwenden und Schritte möglicherweise geringfügig von den hier gezeigten.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Sie müssen Azure-Abonnement. Wenn Sie nicht bereits eine haben, finden Sie unter [Get Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. Sie benötigen einen HDInsight 3.4 Spark 1.6 Cluster zum Durchführen dieser exemplarischen Vorgehensweise. Eine finden Sie die Anleitung [Erste Schritte: Apache Spark Azure HDInsight erstellen](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Clustertyp und Version ist Menü **Clustertyp auswählen** angegeben. 


![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [AZURE.NOTE] Ein Thema, das Scala anstelle von Python verwenden, um Aufgaben für die End-to-End wissenschaftliche Daten veranschaulicht, finden Sie unter [wissenschaftliche Daten mithilfe von Scala mit Spark Azure](machine-learning-data-science-process-scala-walkthrough.md).

<!-- -->

>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="the-nyc-2013-taxi-data"></a>NYC 2013 Taxi Daten

NYC Taxi Reise werden etwa 20 GB komprimierte kommagetrennten Werten (CSV) Dateien (~ 48 GB nicht komprimiert), 173 Millionen einzelnen Schleifen und die Preise aus jeder Reise bezahlt. Jeder Reise Datensatz enthält die Abholung und Ladengeschäft, anonymisierten Hack (Treiber) Lizenznummer und Uhrzeit Medaille (Taxi eindeutige Id). Daten ist umfasst alle Fahrten im Jahr 2013 und in die folgenden zwei Datasets für jeden Monat:

1. 'Trip_data' CSV-Dateien enthalten REISEDETAILS wie Anzahl der Fahrgäste, Abholen und Dauer und Reisedauer Reise Dropoff verweist. Hier sind einige Beispieldatensätze:

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


Wir haben ein Beispiel 0,1 % dieser Dateien und verknüpft die Reise\_Daten und Reise\_Tarif CVS-Dateien in einem einzelnen Dataset als Eingabedatasets für diese exemplarische Vorgehensweise verwenden. Der eindeutige Schlüssel auf Reise\_Daten und Reise\_Tarif besteht aus den Feldern: Medaille Hack\_Lizenz und Abholung\_Datetime. Jeder Datensatz des Datasets enthält die folgenden Attribute, die eine NYC Taxi darstellt:


|Feld| Kurze Beschreibung
|------|---------------------------------
| medaillon |Anonymes Taxi Medaille (Taxi eindeutige Id)
| hack_license |    Anonymes Hackney Beförderung Lizenznummer
| vendor_id |   Taxi Lieferanten id
| rate_code | NYC Taxi Rate Flugpreis
| store_and_fwd_flag | Speichern und Weiterleiten Flagge
| pickup_datetime | Wählen Sie Datum und Uhrzeit
| dropoff_datetime | Dropoff Datum & Uhrzeit
| pickup_hour | Stunde auswählen
| pickup_week | Woche des Jahres abholen
| Wochentag | Wochentag (Bereich 1 bis 7)
| passenger_count | Anzahl von Passagieren in einem Taxi Reise
| trip_time_in_secs | Reise-Zeit in Sekunden
| trip_distance | Reise Entfernung in Meilen
| pickup_longitude | Längengrad abholen
| pickup_latitude | Latitude abholen
| dropoff_longitude | Dropoff Länge
| dropoff_latitude | Dropoff latitude
| direct_distance | Direkte Abstand Kommissionierung einrichten und Dropoff
| payment_type | Zahlungsart (cas, Kreditkarten usw.)
| fare_amount | Fahrpreis Betrag
| Zuschlag | Zuschlag
| mta_tax | MTA-Steuer
| tip_amount | Tipp Betrag
| tolls_amount | Maut-Betrag
| total_amount | Gesamtbetrag
| Geneigter | Geneigter (0/1 für Nein oder Ja)
| Datenlecks | Tipp Klasse (0: 0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > $20)


## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Ausführen von Code aus einem Notizbuch Jupyter Spark-cluster 

Sie können Jupyter Notebook von Azure-Portal starten. Spark Cluster auf dem Dashboard und klicken Sie auf, um die Verwaltungsseite für den Cluster eingeben. Öffnen Sie den Cluster Spark Notizbuch klicken Sie auf **Cluster Dashboards** -> **Jupyter Notebook** .

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Sie können auch auf ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** Jupyter Notebooks auf Durchsuchen. Der Name des eigenen Cluster ersetzen Sie CLUSTERNAME Teil dieser URL. Sie benötigen das Kennwort für Ihr Administratorkonto auf den Notebooks.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Wählen Sie PySpark zu einem Verzeichnis mit ein paar Beispiele für vorkonfigurierte Notebooks, die PySpark-API verwenden. Notebooks, die Codebeispiele für diese Sammlung Spark Thema enthalten sind bei [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)


Sie können Notebooks direkt von Github Jupyter Notebook-Server im Cluster Spark hochladen. Klicken Sie auf der Homepage der Jupyter **Hochladen** auf der rechten Seite des Bildschirms. Datei-Explorer geöffnet. Sie können hier die URL Github (unformatierte Inhalt) des Notizbuchs einfügen, und klicken Sie auf **Öffnen**. PySpark-Notebooks sind unter den folgenden URLs:

1.  [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.  [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-model-consumption.ipynb)
3.  [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Sie sehen den Dateinamen auf Jupyter Datei eine Schaltfläche **Laden** wieder. Klicken Sie auf diese Schaltfläche **Hochladen** . Jetzt haben Sie das Notizbuch importiert. Wiederholen Sie diese Schritte zum Hochladen der Notebooks aus dieser exemplarischen Vorgehensweise.

> [AZURE.TIP] Sie können mit der rechten Maustaste Links in Ihrem Browser und wählen **Link kopieren** zu den unformatierten Inhalt Github-URL. Sie können diese URL Jupyter hochladen Datei Explorer im Dialogfeld einfügen.

Jetzt können Sie:

- Einsehen des Codes das Notizbuch.
- Führen Sie jede Zelle durch Drücken von **UMSCHALT + EINGABETASTE**.
- Führen Sie das Notizbuch auf **Zelle** -> **Ausführen**.
- Verwenden Sie die automatische Visualisierung von Abfragen.

> [AZURE.TIP] PySpark Kernel visualisiert automatisch die Ausgabe (HiveQL) SQL-Abfragen. Sie erhalten die Möglichkeit, verschiedene Visualisierung (Tabelle, Kreis-, Linien-, oder Balken) mithilfe **der Schaltflächen im Menü** im Notizbuch auswählen:

![Logistische Regression ROC Kurve für generische Ansatz](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Was kommt als nächstes?

Mit einem HDInsight Spark-Cluster einrichten und hochgeladen Jupyter Notebooks können Sie Themen arbeiten, die drei PySpark Notebooks entsprechen. Sie zeigen die Daten zu und dann zum Erstellen und Verwenden von Modellen. Erweiterte Untersuchung und Modellierung Notebook der Cross-hyper-Parameter ziehen, Validierung und Bewertung Modell veranschaulicht. 

**Durchsuchen von Daten und mit Spark:** Untersuchen Sie das Dataset und erstellen Faktor lernen Modelle arbeitet das Thema [Erstellen binäre Klassifizierung und regressionsmodelle für Daten mit dem Toolkit Spark MLlib](machine-learning-data-science-spark-data-exploration-modeling.md) Computer ausgewertet.

**Verbrauch Modell:** Das Ergebnis der Klassifizierung und Regression Modelle in diesem Thema finden Sie unter [Bewertung und bewerten Spark erstellt Learning Modelle](machine-learning-data-science-spark-model-consumption.md).

**Übergreifende Überprüfung und Hyperparameter ziehen**: Modelle wie möglich finden Sie [Erweiterte Daten durchsuchen und mit Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) geschult mit Cross-Validierung und hyper-Parameter ziehen

