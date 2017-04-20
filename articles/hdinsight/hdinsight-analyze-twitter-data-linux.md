<properties
    pageTitle="Datenanalyse Twitter mit Apache Struktur auf HDInsight | Microsoft Azure"
    description="Erfahren Sie mit Python Tweets gespeichert, die bestimmte Schlüsselwörter enthalten, mit der Struktur und Hadoop auf HDInsight TWitter-Rohdaten in eine durchsuchbare Tabelle Struktur transformieren."
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
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analysieren Sie Twitter-Daten mithilfe von Struktur in HDInsight

In diesem Dokument erhalten Sie mithilfe einer streaming-API Twitter Tweets und dann mit Apache Struktur auf einem Linux-basierten HDInsight Cluster Prozess JSON formatierte Daten. Das Ergebnis werden eine Liste der Twitter-Benutzer geschickt die meisten Tweets, die ein bestimmtes Wort enthalten.

> [AZURE.NOTE] Zwar einzelnen Teile dieses Dokuments können mit Windows-basierten HDInsight (z. B. Python) viele Schritte basieren auf Linux basierende HDInsight ein Cluster. Bestimmte Windows-basierten Cluster finden Sie unter [Twitter analysieren Daten mit Struktur in HDInsight](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- Ein __Linux-basiertes Azure HDInsight-Cluster__. Informationen zum Erstellen eines Clusters finden Sie unter [Erste Schritte mit Linux-basierten HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) Schritte zum Erstellen eines Clusters.

- Ein __SSH-Client__. Weitere Informationen über SSH mit Linux-basierten HDInsight finden Sie in folgenden Artikeln:

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ und [pip](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>Abrufen Sie eine Twitter-feed

Twitter ermöglicht das Abrufen der [Daten für jeden Tweet](https://dev.twitter.com/docs/platform-objects/tweets) als JavaScript Object Notation (JSON)-Dokument über eine REST-API. [OAuth](http://oauth.net) ist für die Authentifizierung an die API erforderlich. Sie müssen auch _Twitter-Anwendung_ erstellen, die Einstellungen enthält auf die API zugreifen.

###<a name="create-a-twitter-application"></a>Erstellen Sie eine twitteranwendung

1. Melden Sie sich über einen Webbrowser auf [https://apps.twitter.com/](https://apps.twitter.com/)an. Klicken Sie **Jetzt anmelden** haben Sie einen Twitter-Konto.
2. Klicken Sie auf **neue Anwendung**.
3. Geben Sie **Name**, **Beschreibung**, **Website**. Sie können einen URL für die **Website** -Feld. Die folgende Tabelle zeigt einige Werte verwenden:

  	| Feld | Wert |
  	|:----- |:----- |
  	| Name  | MyHDInsightApp |
  	| Beschreibung | MyHDInsightApp |
  	| Website | http://www.myhdinsightapp.com |
    
4. Überprüfen Sie **Ja, ich stimme zu**und dann auf **der Twitter-Anwendung erstellen**.
5. Klicken Sie auf die Registerkarte **Berechtigungen** . Die Standardberechtigung ist **schreibgeschützt**. Dies reicht für dieses Lernprogramm.
6. Klicken Sie auf der Registerkarte **Tasten und Zugriffstoken** .
7. Klicken Sie auf **Meine Zugriffstoken erstellen**.
8. Klicken Sie auf **Test OAuth** in der oberen rechten Ecke der Seite.
9. Notieren Sie **Verbraucherschlüssel**, **Verbrauchergeheimnis** **Zugriffstoken**und **Access token geheim**. Die Werte werden später benötigen werden.

>[AZURE.NOTE] Bei Verwendung von Curl-Befehl in Windows verwenden Sie doppelte Anführungszeichen statt Anführungszeichen für die Werte.

###<a name="download-tweets"></a>Tweets herunterladen

Python-Code Twitter 10.000 Tweets herunterladen und speichern in einer Datei namens __tweets.txt__.

> [AZURE.NOTE] Die folgenden Schritte erfolgen im Cluster HDInsight da Python bereits installiert ist.

1. Verbinden Sie mit HDInsight SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Wenn Sie ein Kennwort zum schützen Ihr Benutzerkonto SSH verwendet, werden Sie aufgefordert, eingeben. Wenn Sie einen öffentlichen Schlüssel verwendet, müssen Sie möglicherweise verwenden die `-i` Parameter, um den passenden privaten Schlüssel. Z. B. `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Weitere Informationen über SSH mit Linux-basierten HDInsight finden Sie in folgenden Artikeln:
    
    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Standardmäßig ist das __Pip__ -Dienstprogramm auf dem Headknoten HDInsight nicht installiert. Verwenden Sie die folgenden installieren und aktualisieren Sie dieses Dienstprogramm:

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. Der Code herunterladen Tweets basiert auf [Tweepy](http://www.tweepy.org/) und [Progressbar](https://pypi.python.org/pypi/progressbar/2.2). Um diese zu installieren, verwenden Sie den folgenden Befehl ein:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] Die Bits zum Entfernen von Python-Openssl installieren Python-Entwickler, Libffi-Dev Dev Libssl, PyOpenSSL und Anfragen [Sicherheit] ist eine InsecurePlatform Warnung zu vermeiden, bei der Verbindung mit Twitter über SSL aus Python.
    >
    > Tweepy v3.2.0 wird verwendet, um [Fehler](https://github.com/tweepy/tweepy/issues/576) zu vermeiden, die auftreten können, wenn Tweets verarbeitet.

4. Verwenden Sie folgenden Befehl, um eine neue Datei namens __gettweets.py__:

        nano gettweets.py

5. Verwenden Sie die folgenden als Inhalt der Datei __gettweets.py__ . Ersetzen Sie die Platzhalterinformationen für __Verbraucher\_Schlüssel__, __Consumer\_Schlüssel__, __Zugriff /\_token__, und __Zugriff\_token\_Schlüssel__ mit den Informationen aus der Twitter-Anwendung.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Verwenden Sie __STRG + X__und __Y__ , um die Datei zu speichern.

7. Verwenden Sie folgenden Befehl zu der Datei Tweets herunterladen:

        python gettweets.py

    Eine Statusanzeige soll angezeigt werden, und bis zu 100 % zählen Tweets heruntergeladen und in Datei gespeichert.

    > [AZURE.NOTE] Dauert sehr lange für die Statusanzeige wechseln, sollten Sie den Filter, um Trends zu verfolgen ändern; Wenn es viele TWEETS zum Thema, dem Sie filtern möchten, erhalten Sie schnell 10000 Tweets erforderlich.

###<a name="upload-the-data"></a>Die Daten

Verwenden Sie zum Hochladen von Daten auf WASB (distributed File System von HDInsight verwendet) die folgenden Befehle ein:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

Dies speichert die Daten an, die alle Knoten im Cluster zugreifen können.

##<a name="run-the-hiveql-job"></a>Die Stapelverarbeitung HiveQL


1. Verwenden Sie den folgenden Befehl zum Erstellen einer Datei mit HiveQL-Anweisungen:

        nano twitter.hql
    
    Verwenden Sie folgende als Inhalt der Datei:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        
3. Drücken Sie __STRG + X__und dann __Y__ , die Datei zu speichern.

4. Verwenden Sie den folgenden Befehl in der Datei enthaltenen HiveQL ausgeführt:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    Dies wird Shell Struktur Laden der HiveQL in der Datei __twitter.hql__ ausführen und schließlich ein `jdbc:hive2//localhost:10001/>` aufgefordert.
    
5. Verwenden Sie aus der Luftlinie folgende um sicherzustellen, dass Daten aus der __Tweets__ Tabelle HiveQL in der Datei __twitter.hql__ erstellt auswählen können:
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    Maximal 10 Tweets wird zurückgegeben, die das Wort __Azure__ im Nachrichtentext enthalten.

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir gesehen, wie Sie machen eine unstrukturierte JSON Dataset strukturierte Struktur Abfragen, untersuchen und analysieren Daten aus Twitter Azure HDInsight mit. Weitere Informationen finden Sie unter:

- [Erste Schritte mit HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Datenanalyse Flug Verzögerung mit HDInsight](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
