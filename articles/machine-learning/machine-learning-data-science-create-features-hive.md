<properties
    pageTitle="Funktionen für Daten in einer Struktur Abfragen Hadoop Cluster erstellen | Microsoft Azure"
    description="Beispiele für Hive-Abfragen, die Daten in einem Cluster Azure HDInsight Hadoop Funktionen generieren."
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
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Erstellen Sie Funktionen für Daten in einer Struktur Abfragen Hadoop cluster

Dieses Dokument veranschaulicht die Features für Daten in einer Struktur Abfragen Azure HDInsight Hadoop-Cluster erstellen. Diese Struktur Abfragen verwenden eingebettete Struktur benutzerdefinierten Funktionen (UDFs) für die Skripts bereitgestellt werden.

Vorgänge zu Funktionen können speicherintensiv sein. Die Leistung der Hive-Abfragen wird in solchen Fällen wichtiger und verbessern, indem Sie bestimmte Abstimmungsparameter. Die Optimierung dieser Parameter wird im letzten Abschnitt beschrieben.

Beispiele für Abfragen, die werden [NYC Taxi Daten](http://chriswhong.com/open-data/foil_nyc_taxi/) gelten Szenarien auch im [Github Repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)bereitgestellt. Diese Abfragen bereits Datenschema angegeben haben und können bei ausführen. Im letzten Abschnitt werden Benutzer optimieren können, damit die Struktur Abfragen verbessert werden können Parameter auch erläutert.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Dieses **Menü** enthält Links zu Themen, in denen Funktionen für Daten in unterschiedlichen Umgebungen erstellen. Diese Aufgabe ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Erforderliche Komponenten
Dieser Artikel setzt voraus:

* Ein Azure Storage-Konto erstellt. Anleitung, finden Sie unter [Azure Storage-Konto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account)
* Angepassten Hadoop Cluster mit den HDInsight-Dienst bereitgestellt.  Anleitung, finden Sie unter [Anpassen von Azure HDInsight Hadoop-Cluster für erweiterte Analysen](machine-learning-data-science-customize-hadoop-cluster.md).
* Die Daten wurde Struktur Tabellen in Azure HDInsight Hadoop Cluster hochgeladen. Wenn dies nicht der Fall ist, führen Sie [Hive-Tabellen erstellen und laden Daten](machine-learning-data-science-move-hive-tables.md) Hive-Tabellen zuerst Datenupload.
* Remote-Zugriff auf den Cluster aktiviert. Anleitung, finden Sie unter [Zugriff die Hadoop Cluster Head Knoten](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="hive-featureengineering"></a>KE-Erzeugung

In diesem Abschnitt beschriebenen Beispiele der Arten in denen Features Struktur Abfragen generieren können. Generierten zusätzliche Features können Sie als Spalten in der vorhandenen Tabelle hinzufügen oder Erstellen einer neuen Tabelle mit zusätzlichen Features und Primärschlüssel mit der ursprünglichen Tabelle verknüpft werden kann. Es folgen Beispiele dargestellt:

1. [Häufigkeit basiert Funktion generieren](#hive-frequencyfeature)
2. [Risiken kategorischen Variablen in binären Klassifizierung](#hive-riskfeature)
3. [Funktionen von Datetime Field extrahieren](#hive-datefeatures)
4. [Funktionen aus Text extrahieren](#hive-textfeatures)
5. [Abstand zwischen GPS-Koordinaten zu berechnen](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Häufigkeit basiert Funktion generieren

Es ist häufig sinnvoll, die Frequenzen eine kategoriale Variable Ebenen oder Frequenzen bestimmte Kombinationen von Ebenen aus mehreren kategorischen Variablen berechnet. Das folgende Skript können Benutzer um diese Frequenzen zu berechnen:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Risiken kategorischen Variablen in binären Klassifizierung

Binäre Klassifizierung müssen numerische Funktionen nur verwendeten Modelle numerische Funktionen werden numerische kategorische Variablen umwandeln. Dies ist jeder numerische Ebene mit numerischen ersetzen. In diesem Abschnitt zeigen wir einige generischen Struktur Abfragen, die Risiken (Protokoll Quote) eine kategoriale Variable Berechnung.


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

In diesem Beispiel werden die Variablen `smooth_param1` und `smooth_param2` Glätten Risikowerte aus den Daten berechnet werden sollen. Risiken müssen zwischen -Inf und Inf. Risiken > 0 gibt an, dass die Wahrscheinlichkeit, dass das Ziel gleich 1 ist größer als 0,5 ist.

Nachdem das Risiko Tabelle berechnet wurde, Benutzer einer Tabelle Risikowerte zuweisen können, indem mit diesen Risiken. Im vorherigen Abschnitt wurde Struktur verknüpfen Abfrage bereitgestellt.

###<a name="hive-datefeatures"></a>Funktionen von Datetime Fields extrahieren

Struktur enthält eine Reihe von UDFs für die Verarbeitung von Datetime-Felder. In Struktur, das Standard-Datetime-Format ist ' Yyyy-MM-Dd 00:00:00 ' ('1970-01-01 12:21:32 "beispielsweise). In diesem Abschnitt zeigen wir, die den Tag des Monats, den Monat aus einem Datetime-Feld Extrahieren, und die anderen Beispiele, die eine Datetime-Zeichenfolge in ein Format konvertieren, anders als das Standardformat in eine Datetime-Zeichenfolge im format.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Dieser Struktur Abfrage davon aus, dass die *& #60; Datetime-Feld >* ist das Standard-Datetime-Format.

Ist ein Datetime-Feld nicht im Standardformat müssen Sie zuerst Unix Zeitstempel Datetime-Feld umwandeln und Unix-Zeitstempel in eine Datetime-Zeichenfolge in das Standardformat konvertieren. Wenn Datetime im Format ist, können Benutzer eingebettete Datetime UDFs Funktionen extrahiert anwenden.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

In dieser Abfrage Wenn die *& #60; Datetime-Feld >* hat das Muster *26/03/2015 12:04:39*, *' & #60; Muster Datetime-Feld >'* sollte `'MM/dd/yyyy HH:mm:ss'`. Zum Testen können Benutzer ausführen.

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

*Hivesampletable* in dieser Abfrage ist vorinstalliert auf alle Azure HDInsight Hadoop Cluster standardmäßig als Cluster bereitgestellt werden.


###<a name="hive-textfeatures"></a>Eigenschaften von Textfeldern extrahieren

Wenn die Tabelle Struktur ein Textfeld mit einer Reihe von Wörtern, die durch Leerzeichen getrennt sind ist, extrahiert die folgende Abfrage die Länge der Zeichenfolge und die Anzahl der Wörter in der Zeichenfolge.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>Berechnen der Entfernung zwischen GPS-Koordinaten

In diesem Abschnitt angegebenen Abfrage kann direkt auf NYC Taxi Reisedaten angewendet werden. Diese Abfrage dient wie eine eingebettete mathematische Funktionen im Hive Features generieren.

In dieser Abfrage verwendeten Felder sind GPS-Koordinaten der Abholung und Dropoff, namens *Abholung\_Länge*, *Abholung\_Latitude*, *Dropoff\_Länge*, und *Dropoff\_Latitude*. Abfragen, die direkten Abstand die Abholung und Dropoff Koordinaten berechnet werden:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Mathematischen Formeln, die den Abstand zwischen zwei Koordinaten auf der Website <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type Skripts</a> finden Peter Lapisu erstellt. In seinem Javascript die Funktion `toRad()` nur *Lat_or_lon*Pi/180*, Grad in Bogenmaß konvertiert. Hier *Lat_or_lon * ist die Breiten- oder Längenkoordinate. Da Struktur nicht die Funktion bietet `atan2`, aber die Funktion `atan`, `atan2` Funktion durchgeführten `atan` Funktion in der obigen Hive-Abfrage mit der Definition in <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Eine vollständige Liste der Struktur eingebettete UDFs im Abschnitt **Integrierte Funktionen** der <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Struktur der Apache Wiki</a>finden).  

## <a name="tuning"></a>Erweiterte Themen: Tune Struktur Parameter Geschwindigkeit von Abfragen verbessern

Parameter Standardeinstellungen der Hive-Cluster möglicherweise nicht für die Hive-Abfragen und die Daten, die Abfragen verarbeiten. In diesem Abschnitt erläutern wir einige Parameter Benutzer optimiert werden können, die Struktur Abfragen verbessern. Benutzer müssen den Parameter Optimieren von Abfragen vor dem Abfragen von Daten.

1. **Java-Heap-Speicher**: für Abfragen mit großen Datasets beitreten oder Verarbeitung lange **Heap Speicherplatz** ist eines der auftretende Fehler. Dies kann optimiert werden, indem Parameter *mapreduce.map.java.opts* und *mapreduce.task.io.sort.mb* auf die gewünschten Werte. Hier ist ein Beispiel:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Dieser Parameter weist 4GB Speicher auf Java-Heap-Speicher und auch effizienter sortieren Sie mehr Arbeitsspeicher zuordnen. In diesem Zusammenhang empfiehlt es sich, mit diesen spielen, wenn es ein Auftrag Heapspeicher bezogene Fehler.

2. **DFS-Blockgröße** : mit diesem Parameter wird die kleinste Einheit von Daten, das Dateisystem gespeichert. Beispielsweise ist die Größe des DFS-128 MB, dann Daten Größe und bis zu 128 MB in einem Block, während Daten befindet, die größer als 128MB zusätzliche Blöcken zugewiesen. Kleine Blockgröße auswählen große Gemeinkosten in Hadoop seit dem Namensknoten zu viele weitere Anfragen zu der entsprechenden Block zu der Datei. Eine empfohlene Einstellung beim Umgang mit GB (oder mehr) Daten:

        set dfs.block.size=128m;

3. **Optimieren join Operation Struktur** : während Verknüpfungsoperationen Map/Reduce-Framework normalerweise in der verringern, manchmal statt enorme Vorteile erzielt werden durch Planen von Joins in der Karte (auch "Mapjoins" genannt). Wir können direkte Struktur dazu möglichst festlegen:

        set hive.auto.convert.join=true;

4. **Angibt Mappers Struktur** : während Hadoop ermöglicht dem Benutzer die Anzahl der Reduzierstück ist die Anzahl der Mapper normalerweise nicht vom Benutzer festgelegt werden. Bestimmt ein Trick, der ist ein gewisses Maß an Kontrolle über diese Nummer Hadoop Variablen, *mapred.min.split.size* und *mapred.max.split.size* als die Größe der einzelnen Aufgaben zuordnen wählen können:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    Der Standardwert von *mapred.min.split.size* ist normalerweise 0, der *mapred.max.split.size* **Long.MAX** und der *dfs.block.size* ist 64 MB. Wie wir Größe Daten sehen diese Abstimmungsparameter "Einstellung" können wir die Anzahl der verwendeten Mapper optimieren.

5. Weniger mehr **Optionen** zum Optimieren der Leistung der Struktur sind unten aufgeführt. Diese können Sie Speichers und Aufgaben reduzieren und möglich Leistung optimiert. Beachten Sie, dass *mapreduce.reduce.memory.mb* die Größe des physischen Speichers der einzelnen Arbeitskraft Knoten im Cluster Hadoop größer werden kann.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
