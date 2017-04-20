<properties
   pageTitle="Twitter Trends mit Apache auf HDInsight | Microsoft Azure"
   description="Erfahren Sie mit Trident Apache Storm-Topologie, die Trends auf Twitter basierend auf Hashtags bestimmt."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Bestimmen Sie Twitter Trends mit Apache auf HDInsight

Erfahren Sie mit Trident Storm-Topologie, die Trends (hashtags) auf Twitter bestimmt.

Trident ist eine Abstraktionsebene, die bereitgestellten Tools Joins Aggregationen, Gruppierung, Funktionen und Filter. Zusätzlich fügt Trident Primitives für statusbehaftete, inkrementelle Verarbeitung. Dieses Beispiel veranschaulicht, wie eine Topologie mit einer benutzerdefinierten Schnauze, Funktion und mehrere integrierte Funktionen Trident erstellen können.

> [AZURE.NOTE] In diesem Beispiel stark [Trident Storm](https://github.com/jalonsoramos/trident-storm) -Beispiel Juan Alonso basiert.

##<a name="requirements"></a>Vorschriften

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java, und JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Ein Twitter Entwicklerkonto

##<a name="download-the-project"></a>Laden Sie das Projekt

Verwenden Sie den folgenden Code das Projekt lokal klonen.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Topologie

Die Topologie in diesem Beispiel lautet wie folgt:

![Topologie](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] Dies ist eine vereinfachte Ansicht der Topologie. Mehrere Instanzen der Komponenten werden der Knoten im Cluster verteilt.

Trident-Code, der die Topologie implementiert, lautet wie folgt:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Dieser Code führt Folgendes aus:

1. Erstellt einen neuen Stream aus der Schnauze. Schnauze Twitter Tweets entnimmt und Filter für bestimmte Schlüsselwörter (Liebe, Musik und Kaffee in diesem Beispiel).

2. HashtagExtractor, einer benutzerdefinierten Funktion zur Extrahierung von Hash-Tags aus jeder Tweet. Diese werden in den Stream ausgegeben.

3. Der Stream ist hashtag gruppiert und Aggregator an. Dieser Aggregator erstellt Anzahl wie oft jedes hashtag aufgetreten ist. Diese Daten werden im Arbeitsspeicher beibehalten. Schließlich wird ein neuer Stream ausgegeben, die hashtag und die Anzahl enthält.

4. Da wir nur die am häufigsten verwendeten hashtags für Charge Tweets interessiert sind, wird die **FirstN** -Assembly angewendet, um nur die 10 größten Werte, basierend auf dem Anzahlfeld zurück.

> [AZURE.NOTE] Auslauf und HashtagExtractor verwenden integrierte Trident-Funktion.
>
> Informationen über integrierte Operationen finden Sie unter <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">Paket storm.trident.operation.builtin</a>.
>
> Trident-State-Implementierung als MemoryMapState finden Sie unter:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Storm Trident elastische suchen</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident-redis</a>

###<a name="the-spout"></a>Schnauze

Auslauf **TwitterSpout**wird <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> Tweets Twitter abgerufen. Erstellt ein Filter (Liebe, Musik und Kaffee in diesem Beispiel) und eingehende Tweets (Status), die dem Filter entsprechen werden in eine verknüpfte blockierte Warteschlange gespeichert. (Weitere Informationen finden Sie in der <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">Klasse LinkedBlockingQueue</a>.) Schließlich werden Elemente aus der Warteschlange abgerufen und in der Topologie ausgegeben.

###<a name="the-hashtagextractor"></a>Die HashtagExtractor

Hash-Tags zu extrahieren, wird <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">GetHashtagEntities</a> zum Abrufen von Hash-Tags in der Tweet. Diese werden dann in den Stream ausgegeben.

##<a name="enable-twitter"></a>Twitter aktivieren

Gehen Sie folgendermaßen eine neue twitteranwendung registrieren und erhalten die Verbraucher und Access token nötigen Twitter gelesen:

1. Gehe zu <a href="https://apps.twitter.com" target="_blank">Twitter Apps</a> , und klicken Sie auf **neue Anwendung erstellen** . Beim Ausfüllen des Formulars lassen Sie das **Callback-URL** -Feld leer.

2. Beim Erstellen die Anwendung klicken Sie auf der Registerkarte **Tasten und Zugriffstoken** .

3. Kopieren Sie die **Verbraucherschlüssel** und **Verbrauchergeheimnis** Informationen.

4. Wählen Sie am unteren Rand der Seite **Erstellen meiner Zugriffstoken** existiert keine Token. Wenn Token erstellt haben, kopieren Sie **Zugriffstoken** und **Access Token geheime** Informationen.

5. **TwitterSpoutTopology** Projekt vorher geklont, öffnen Sie die Datei **resources/twitter4j.properties** in den vorherigen Schritten fügen Sie gesammelten Informationen hinzu und speichern Sie die Datei.

##<a name="build-the-topology"></a>Erstellen der Topologie

Verwenden Sie den folgenden Code zum Erstellen des Projekts:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>Testen der Topologie

Verwenden Sie den folgenden Befehl die Topologie lokal testen:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Nach dem Start der Topologie sollten Debuginformationen, das den Hash enthält tags und zählt die Topologie ausgegebenen angezeigt. Die Ausgabe sollte etwa wie folgt aussehen:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Nächste Schritte

Damit die Topologie lokal getestet haben, erfahren Sie, wie die Topologie bereitgestellt: [Bereitstellen und Verwalten von Apache Storm Topologien auf HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Sie können auch folgenden Sturm Themen interessieren:

* [Entwickeln von Java-Topologien für auf HDInsight mit Maven](hdinsight-storm-develop-java-topology.md)

* [Entwickeln von C# Topologien für auf HDInsight mit Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Weitere Sturm Beispiele für HDinsight:

* [Topologien für auf HDInsight](hdinsight-storm-example-topology.md)
