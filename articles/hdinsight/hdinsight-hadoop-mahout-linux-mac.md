<properties
    pageTitle="Mahout mit HDInsight Linux-basierten Vorschläge generieren | Microsoft Azure"
    description="Erfahren Sie mit Apache Mahout Computer lernen Bibliothek Film Recommendations mit Linux-basierte HDInsight (Hadoop) generiert."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Generieren Sie Film-Empfehlung mit Apache Mahout Linux-basierten Hadoop in HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Erfahren Sie mit [Apache Mahout](http://mahout.apache.org) Computer Lernen mit Azure HDInsight Film Recommendations generiert.

Mahout ist ein [Computer lernen] [ ml] Bibliothek für Apache Hadoop. Mahout enthält Algorithmen für die Verarbeitung von Daten filtern, Klassifizierung und clustering. In diesem Artikel verwenden Sie eine Empfehlung Engine zu Filmen basieren Freunden gesehen Film Recommendations.

> [AZURE.NOTE] Die Schritte in diesem Dokument erfordern eine Linux-basierte Hadoop auf HDInsight Cluster. Informationen zu einem Windows-basierten Cluster mit Mahout finden Sie unter [generieren Film Recommendations mit Apache Mahout mit Windows-basierten Hadoop in HDInsight](hdinsight-mahout.md)

##<a name="prerequisites"></a>Erforderliche Komponenten

* Ein Linux-basiertes Hadoop auf HDInsight Cluster. Weitere Informationen zum Erstellen einer [Einsatz von Linux-basierten Hadoop in HDInsight][getstarted]

##<a name="mahout-versioning"></a>Mahout Versionen

Finden Sie weitere Informationen zur Version des Mahout HDInsight Cluster enthaltene [HDInsight Versionen und Hadoop-Komponenten](hdinsight-component-versioning.md).

> [AZURE.WARNING] Eine andere Version von Mahout HDInsight Cluster hochladen kann, nur Komponenten mit dem HDInsight werden unterstützt und Microsoft Support isolieren und Lösen von Problemen im Zusammenhang mit diesen Komponenten unterstützt.
>
> Benutzerdefinierte Komponenten erhalten angemessene Unterstützung helfen, das Problem zu beheben. Dadurch kann die Fehlerbehebung oder Aufforderung zu Kanälen für open-Source-Technologien, fundiertes Fachwissen für diese Technologie fand. Beispielsweise sind viele Community-Sites, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-Projekte verfügen Projektsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/), [Funken](http://spark.apache.org/).

##<a name="recommendations"></a>Empfohlene Kenntnisse

Eine der Funktionen, die von Mahout bereitgestellten ist eine Empfehlung Engine. Dieses Modul akzeptiert Daten im Format `userID`, `itemId`, und `prefValue` (Benutzer Vorrang für den Artikel). Mahout können Sie gemeinsames Vorkommen Analyse, ausführen: _Benutzer eine Einstellung für ein Element haben auch bevorzugen diese Elemente_. Mahout bestimmt Benutzer mit ähnlichen Artikel Voreinstellungen zur Vorschläge machen.

Nachfolgend ein sehr einfaches Beispiel, Filme:

* __Gemeinsames Vorkommen__: Joe, Alice und Bob gefällt _Star Wars_ _Das Imperium schlägt zurück_und _jedi zurück_. Mahout fest, dass Benutzer, die eine dieser Filme auch die beiden anderen wie.

* __Gemeinsames Vorkommen__: Bob und Alice auch gern _Bedrohung_ _Angriff der Klone_und _Rache der Sith_. Mahout fest, dass Benutzer auch die vorherigen drei Filme gefallen diese drei wie.

* __Vergleichbare Empfehlung__: Da Joe die ersten drei Filme gefallen, Mahout untersucht Filme, die andere mit ähnlichen gefällt, aber Joe wurde nicht überwacht (gefällt/Note). In diesem Fall empfiehlt Mahout _Bedrohung_, _Angriff der Klone_und _Rache der Sith_.

###<a name="understanding-the-data"></a>Verständnis der Daten

[GroupLens Research] ,[ movielens] Filme in einem Format, das mit Mahout Bewertungsdaten bereit. Diese Daten werden im Standardspeicher des Clusters auf `/HdiSamples/HdiSamples/MahoutMovieData`.

Es gibt zwei Dateien, `moviedb.txt` (Informationen zu den Filmen) und `user-ratings.txt`. Benutzer-ratings.txt-Datei wird während der Analyse verwendet, während moviedb.txt verwendet wird, um benutzerfreundlichen Text Informationen bereitzustellen, wenn die Ergebnisse der Analyse anzeigen.

Die Daten in ratings.txt Benutzer hat eine `userID`, `movieID`, `userRating`, und `timestamp`, die sagt uns, wie jeder Benutzer einen Film bewertet. Hier ist ein Beispiel:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>Führen Sie die Analyse

Verwenden Sie den folgenden Befehl zum Ausführen des Auftrags Empfehlung:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] Der Auftrag kann mehrere Minuten dauern und kann mehrere MapReduce-Jobs ausgeführt.

##<a name="view-the-output"></a>Die Ausgabe anzeigen

1. Sobald der Auftrag abgeschlossen ist, verwenden Sie folgenden Befehl zum Anzeigen der generierten Ausgabe:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    Die Ausgabe erscheint wie folgt:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    Die erste Spalte ist die `userID`. Die Werte ' [' und ']' sind `movieId`:`recommendationScore`.

2. Die Ausgabe mit der moviedb.txt können Sie weitere benutzerfreundliche Informationen anzuzeigen. Zunächst müssen wir die Dateien lokal mit den folgenden Befehlen:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Dadurch wird die Ausgabedaten in einer Datei namens **recommendations.txt** im aktuellen Verzeichnis mit Film-Datendateien kopiert.

3. Verwenden Sie folgenden Befehl ein neues Python-Skript zu erstellen, das Film-Namen für die Daten in der Empfehlung Ausgabe aussieht:

        nano show_recommendations.py

    Wenn der Editor geöffnet wird, verwenden Sie folgende als Inhalt der Datei:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Drücken Sie **STRG + X**, **Y**und schließlich **Geben Sie** zum Speichern der Daten.

3. Verwenden Sie den folgenden Befehl, um die Datei ausführbare:

        chmod +x show_recommendations.py

4. Das Python-Skript ausführen. Folgendes wird angenommen, dass in dem Verzeichnis befinden, in dem die Dateien heruntergeladen wurden:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    Dies sehen Vorschläge für Benutzer ID 4 generiert.

    * **Benutzer-ratings.txt** -Datei verwendet, um Filme abzurufen, die der Benutzer bewertet
    * Die **moviedb.txt** -Datei zum Abrufen der Namen der Filme
    * **Recommendations.txt** zum Film Vorschläge für diesen Benutzer abrufen

    Die Ausgabe dieses Befehls werden ähnlich der folgenden:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Temporäre Daten löschen

Temporäre Daten entfernen, die beim Verarbeiten des Auftrags erstellt Mahout Aufträge nicht. Die `--tempDir` Parameter des Auftrags wird die temporären Dateien in einem bestimmten Pfad zum einfachen löschen isolieren angegeben. Um die temporären Dateien zu entfernen, verwenden Sie den folgenden Befehl ein:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Wenn Sie den Befehl erneut ausführen möchten, müssen Sie das Verzeichnis löschen. Verwenden Sie folgende, um dieses Verzeichnis zu löschen:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Nächste Schritte

Da Sie mit Mahout vertraut gemacht haben, erkennen Sie andere Verfahren zum Arbeiten mit Daten auf HDInsight:

* [Struktur mit HDInsight](hdinsight-use-hive.md)
* [Schwein mit HDInsight](hdinsight-use-pig.md)
* [MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
