<properties 
    pageTitle="Analyse in Echtzeit Twitter Stimmung mit HBase | Microsoft Azure" 
    description="Lernen Sie in Echtzeit stimmungsanalyse große Datenmengen von HBase in einem HDInsight (Hadoop) Cluster mit Twitter." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="jgao"/>

# <a name="analyze-real-time-twitter-sentiment-with-hbase-in-hdinsight"></a>Analyse in Echtzeit Twitter Stimmung mit HBase in HDInsight

Lernen Sie in Echtzeit [stimmungsanalyse](http://en.wikipedia.org/wiki/Sentiment_analysis) große Datenmengen von Twitter mit HBase in einem Cluster HDInsight (Hadoop).


Social-Websites sind eine der wichtigsten treibenden Kräfte Datenverlustvorfalls suchen. Von Websites wie Twitter bereitgestellten öffentlichen APIs bieten nützliche Daten für Analyse und beliebte Trends. In dieser praktischen Einführung entwickeln Sie eine Konsole streaming-Anwendung und einer ASP.NET Web-Anwendung Folgendes ausführen:

![Gefühl HDInsight HBase analysieren Twitter][img-app-arch]

- Streaming-Anwendung
    - Rufen Sie Geo-Tags Tweets mit streaming-API Twitter in Echtzeit ab
    - Bewerten Sie die Stimmung dieser TWEETS
    - Speichern Sie die Stimmung Informationen in HBase mit Microsoft HBase SDK
- Die Anwendung Azure Websites
    - darstellen Sie statistische Echtzeitergebnisse auf Bing Maps mithilfe einer ASP.NET Web-Anwendung. Eine Visualisierung der Tweets wird wie folgt aussehen:

    ![hdinsight.hbase.twitter.sentiment.Bing.Map][img-bing-map]
    
    Sie werden möglicherweise Abfrage Tweets mit bestimmten Schlüsselwörtern auf der ausgedrückte Stellungnahme Tweets ist positiv, negativ oder neutral.

Ein vollständiges Beispiel der Visual Studio-Lösung finden Sie auf GitHub: [Echtzeit sozialen Stimmung Analyse app](https://github.com/maxluk/tweet-sentiment).































### <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein HBase-Cluster in HDInsight**. Anleitung zum Erstellen von Clustern finden Sie unter [Erste Schritte mit HBase Hadoop in HDInsight][hbase-get-started]. Sie benötigen die folgenden Daten in das Tutorial:


    <table border="1">
    <tr><th>Clustereigenschaft</th><th>Beschreibung</th></tr>
    <tr><td>HBase Clustername</td><td>Der Clustername HDInsight HBase. Beispiel: https://myhbase.azurehdinsight.net/</td></tr>
    <tr><td>Cluster-Benutzername</td><td>Hadoop-Benutzerkontoname. Der Standardbenutzername Hadoop ist <strong>Admin</strong>.</td></tr>
    <tr><td>Cluster-Benutzerkennwort</td><td>Das Hadoop Benutzerkennwort.</td></tr>
    </table>

- **Eine Arbeitsstation** mit Visual Studio 2013 installiert. Eine Anleitung finden Sie in [Visual Studio installieren](http://msdn.microsoft.com/library/e2h7fzkw.aspx).





## <a name="create-a-twitter-application-id-and-secrets"></a>Erstellen einer Anwendung Twitter-ID und Schlüssel

Streaming-APIs verwenden [OAuth](http://oauth.net/) autorisieren Twitter anfordert. Zunächst verwenden Sie OAuth ist auf Twitter Developer Site eine neue Anwendung erstellen.

**ID der Twitter-Anwendung und Kennwörter erstellen**

1. Melden Sie sich bei [Twitter Apps](https://apps.twitter.com/). Klicken Sie **Jetzt anmelden** haben Sie einen Twitter-Konto.
2. Klicken Sie auf **neue Anwendung**.
3. Geben Sie einen **Namen**, eine **Beschreibung**und eine **Website**. Name der Twitter-Anwendung muss ein eindeutiger Name sein. Website-Feld wird nicht wirklich verwendet. Es muss eine gültige URL sein. 
4. Überprüfen Sie **Ja, ich stimme zu**und dann auf **der Twitter-Anwendung erstellen**.
5. Klicken Sie auf die Registerkarte **Berechtigungen** . Die Standardberechtigung ist **schreibgeschützt**. Dies reicht für dieses Lernprogramm. 
6. Klicken Sie auf der Registerkarte **Tasten und Zugriffstoken** .
7. Klicken Sie auf **Meine Zugriffstoken erstellen**.
8. Klicken Sie auf **Test OAuth** in der oberen rechten Ecke der Seite.
9. Kopieren Sie **Consumer Schlüssel**, **Verbrauchergeheimnis** **Zugriffstoken**und **Access token Schlüssel** Werte Sie benötigen diese Werte später im Lernprogramm.

    ![hdi.hbase.twitter.sentiment.twitter.app][img-twitter-app]






























## <a name="create-twitter-streaming-service"></a>Erstellen Sie Twitter-Streaming-Dienst

Sie müssen zum Erstellen einer Anwendung zu Tweets berechnen Tweet Stimmung und HBase verarbeiteten Tweet Wörter an.

**Die Streaming-Anwendung erstellen**

1. Öffnen Sie **Visual Studio**, und erstellen Sie eine Visual C# **TweetSentimentStreaming**aufgerufen. 
2. **Paket-Manager-Konsole**führen Sie die folgenden Befehle:

        Install-Package Microsoft.HBase.Client -version 0.4.2.0
        Install-Package TweetinviAPI -version 1.0.0.0

    Diese Befehle installieren [HBase.NET SDK](https://www.nuget.org/packages/Microsoft.HBase.Client/) Paket ist die Clientbibliothek HBase zugreifen und das [Tweetinvi-API](https://www.nuget.org/packages/TweetinviAPI/) -Paket auf Twitter-API verwendet wird.

    > [AZURE.NOTE] In diesem Artikel verwendeten Beispiel wurde mit der oben angegebenen Version getestet.  Entnehmen Sie der Option Version auf die neueste Version installieren.

3. Der Verweis im **Projektmappen-Explorer**fügen Sie **System.Configuration hinzu** .
4. Das Projekt **HBaseWriter.cs**eine neue Klassendatei hinzu, und Ersetzen Sie den Code durch Folgendes:

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling;
        using org.apache.hadoop.hbase.rest.protobuf.generated;
        using Microsoft.HBase.Client;
        using Tweetinvi.Models;

        namespace TweetSentimentStreaming
        {
            class HBaseWriter
            {
                // HDinsight HBase cluster and HBase table information
                const string CLUSTERNAME = "https://<Enter Your Cluster Name>.azurehdinsight.net/";
                const string HADOOPUSERNAME = "admin"; //the default name is "admin"
                const string HADOOPUSERPASSWORD = "<Enter the Hadoop User Password>";

                const string HBASETABLENAME = "tweets_by_words";
                const string COUNT_ROW_KEY = "~ROWCOUNT";
                const string COUNT_COLUMN_NAME = "d:COUNT";
                
                long rowCount = 0;

                // Sentiment dictionary file and the punctuation characters
                const string DICTIONARYFILENAME = @"..\..\dictionary.tsv";
                private static char[] _punctuationChars = new[] {
            ' ', '!', '\"', '#', '$', '%', '&', '\'', '(', ')', '*', '+', ',', '-', '.', '/',   //ascii 23--47
            ':', ';', '<', '=', '>', '?', '@', '[', ']', '^', '_', '`', '{', '|', '}', '~' };   //ascii 58--64 + misc.

                // For writting to HBase
                HBaseClient client;

                // a sentiment dictionary for estimate sentiment. It is loaded from a physical file.
                Dictionary<string, DictionaryItem> dictionary;

                // use multithread write
                Thread writerThread;
                Queue<ITweet> queue = new Queue<ITweet>();
                bool threadRunning = true;

                // This function connects to HBase, loads the sentiment dictionary, and starts the thread for writting.
                public HBaseWriter()
                {
                    ClusterCredentials credentials = new ClusterCredentials(new Uri(CLUSTERNAME), HADOOPUSERNAME, HADOOPUSERPASSWORD);
                    client = new HBaseClient(credentials);

                    // create the HBase table if it doesn't exist
                    if (!client.ListTablesAsync().Result.name.Contains(HBASETABLENAME))
                    {
                        TableSchema tableSchema = new TableSchema();
                        tableSchema.name = HBASETABLENAME;
                        tableSchema.columns.Add(new ColumnSchema { name = "d" });
                        client.CreateTableAsync(tableSchema).Wait();
                        Console.WriteLine("Table \"{0}\" is created.", HBASETABLENAME);
                    }

                    // Read current row count cell
                    rowCount = GetRowCount();

                    // Load sentiment dictionary from a file
                    LoadDictionary();

                    // Start a thread for writting to HBase
                    writerThread = new Thread(new ThreadStart(WriterThreadFunction));
                    writerThread.Start();
                }

                ~HBaseWriter()
                {
                    threadRunning = false;
                }

                private long GetRowCount()
                {
                    try
                    {
                        RequestOptions options = RequestOptions.GetDefaultOptions();
                        options.RetryPolicy = RetryPolicy.NoRetry;
                        var cellSet = client.GetCellsAsync(HBASETABLENAME, COUNT_ROW_KEY, null, null, options).Result;
                        if (cellSet.rows.Count != 0)
                        {
                            var countCol = cellSet.rows[0].values.Find(cell => Encoding.UTF8.GetString(cell.column) == COUNT_COLUMN_NAME);
                            if (countCol != null)
                            {
                                return Convert.ToInt64(Encoding.UTF8.GetString(countCol.data));
                            }
                        }
                    }
                    catch(Exception ex)
                    {
                        if (ex.InnerException.Message.Equals("The remote server returned an error: (404) Not Found.", StringComparison.OrdinalIgnoreCase))
                        {
                            return 0;
                        }
                        else
                        {
                            throw ex;
                        }
                        
                    }

                    return 0;
                }

                // Enqueue the Tweets received
                public void WriteTweet(ITweet tweet)
                {
                    lock (queue)
                    {
                        queue.Enqueue(tweet);
                    }
                }

                // Load sentiment dictionary from a file
                private void LoadDictionary()
                {
                    List<string> lines = File.ReadAllLines(DICTIONARYFILENAME).ToList();
                    var items = lines.Select(line =>
                    {
                        var fields = line.Split('\t');
                        var pos = 0;
                        return new DictionaryItem
                        {
                            Type = fields[pos++],
                            Length = Convert.ToInt32(fields[pos++]),
                            Word = fields[pos++],
                            Pos = fields[pos++],
                            Stemmed = fields[pos++],
                            Polarity = fields[pos++]
                        };
                    });

                    dictionary = new Dictionary<string, DictionaryItem>();
                    foreach (var item in items)
                    {
                        if (!dictionary.Keys.Contains(item.Word))
                        {
                            dictionary.Add(item.Word, item);
                        }
                    }
                }

                // Calculate sentiment score
                private int CalcSentimentScore(string[] words)
                {
                    Int32 total = 0;
                    foreach (string word in words)
                    {
                        if (dictionary.Keys.Contains(word))
                        {
                            switch (dictionary[word].Polarity)
                            {
                                case "negative": total -= 1; break;
                                case "positive": total += 1; break;
                            }
                        }
                    }
                    if (total > 0)
                    {
                        return 1;
                    }
                    else if (total < 0)
                    {
                        return -1;
                    }
                    else
                    {
                        return 0;
                    }
                }

                // Popular a CellSet object to be written into HBase
                private void CreateTweetByWordsCells(CellSet set, ITweet tweet)
                {
                    // Split the Tweet into words
                    string[] words = tweet.Text.ToLower().Split(_punctuationChars);

                    // Calculate sentiment score base on the words
                    int sentimentScore = CalcSentimentScore(words);
                    var word_pairs = words.Take(words.Length - 1)
                                        .Select((word, idx) => string.Format("{0} {1}", word, words[idx + 1]));
                    var all_words = words.Concat(word_pairs).ToList();

                    // For each word in the Tweet add a row to the HBase table
                    foreach (string word in all_words)
                    {
                        string time_index = (ulong.MaxValue - (ulong)tweet.CreatedAt.ToBinary()).ToString().PadLeft(20) + tweet.IdStr;
                        string key = word + "_" + time_index;

                        // Create a row
                        var row = new CellSet.Row { key = Encoding.UTF8.GetBytes(key) };

                        // Add columns to the row, including Tweet identifier, language, coordinator(if available), and sentiment 
                        var value = new Cell { column = Encoding.UTF8.GetBytes("d:id_str"), data = Encoding.UTF8.GetBytes(tweet.IdStr) };
                        row.values.Add(value);

                        value = new Cell { column = Encoding.UTF8.GetBytes("d:lang"), data = Encoding.UTF8.GetBytes(tweet.Language.ToString()) };
                        row.values.Add(value);

                        if (tweet.Coordinates != null)
                        {
                            var str = tweet.Coordinates.Longitude.ToString() + "," + tweet.Coordinates.Latitude.ToString();
                            value = new Cell { column = Encoding.UTF8.GetBytes("d:coor"), data = Encoding.UTF8.GetBytes(str) };
                            row.values.Add(value);
                        }

                        value = new Cell { column = Encoding.UTF8.GetBytes("d:sentiment"), data = Encoding.UTF8.GetBytes(sentimentScore.ToString()) };
                        row.values.Add(value);

                        set.rows.Add(row);
                    }
                }

                // Write a Tweet (CellSet) to HBase
                public void WriterThreadFunction()
                {
                    try
                    {
                        while (threadRunning)
                        {
                            if (queue.Count > 0)
                            {
                                CellSet set = new CellSet();
                                lock (queue)
                                {
                                    do
                                    {
                                        ITweet tweet = queue.Dequeue();
                                        CreateTweetByWordsCells(set, tweet);
                                    } while (queue.Count > 0);
                                }

                                // Write the Tweet by words cell set to the HBase table
                                client.StoreCellsAsync(HBASETABLENAME, set).Wait();
                                Console.WriteLine("\tRows written: {0}", set.rows.Count);
                            }
                            Thread.Sleep(100);
                        }
                    }
                    catch (Exception ex)
                    {
                        Console.WriteLine("Exception: " + ex.Message);
                    }
                }
            }
            public class DictionaryItem
            {
                public string Type { get; set; }
                public int Length { get; set; }
                public string Word { get; set; }
                public string Pos { get; set; }
                public string Stemmed { get; set; }
                public string Polarity { get; set; }
            }
        }

6. Legen Sie Konstanten im vorherigen Code einschließlich **CLUSTERNAME**, **HADOOPUSERNAME**, **HADOOPUSERPASSWORD**und DICTIONARYFILENAME. Der DICTIONARYFILENAME ist der Dateiname und der Speicherort der direction.tsv.  Die Datei kann von **https://hditutorialdata.blob.core.windows.net/twittersentiment/dictionary.tsv**heruntergeladen werden. Wenn Sie den Tabellennamen HBase ändern möchten, müssen Sie den Tabellennamen in der Anwendung entsprechend ändern.

7. Öffnen Sie **Program.cs**, und Ersetzen Sie den Code durch Folgendes:

        using System;
        using System.Diagnostics;
        using Tweetinvi;
        using Tweetinvi.Models;

        namespace TweetSentimentStreaming
        {
            class Program
            {
                const string TWITTERAPPACCESSTOKEN = "<Enter Twitter App Access Token>";
                const string TWITTERAPPACCESSTOKENSECRET = "<Enter Twitter Access Token Secret>";
                const string TWITTERAPPAPIKEY = "<Enter Twitter App API Key>";
                const string TWITTERAPPAPISECRET = "<Enter Twitter App API Secret>";

                static void Main(string[] args)
                {
                    Auth.SetUserCredentials(TWITTERAPPAPIKEY, TWITTERAPPAPISECRET, TWITTERAPPACCESSTOKEN, TWITTERAPPACCESSTOKENSECRET);

                    Stream_FilteredStreamExample();
                }

                private static void Stream_FilteredStreamExample()
                {
                    for (;;)
                    {
                        try
                        {
                            HBaseWriter hbase = new HBaseWriter();
                            var stream = Stream.CreateFilteredStream();
                            stream.AddLocation(new Coordinates(-180, -90), new Coordinates(180, 90)); 

                            var tweetCount = 0;
                            var timer = Stopwatch.StartNew();

                            stream.MatchingTweetReceived += (sender, args) =>
                            {
                                tweetCount++;
                                var tweet = args.Tweet;

                                // Write Tweets to HBase
                                hbase.WriteTweet(tweet);

                                if (timer.ElapsedMilliseconds > 1000)
                                {
                                    if (tweet.Coordinates != null)
                                    {
                                        Console.ForegroundColor = ConsoleColor.Green;
                                        Console.WriteLine("\n{0}: {1} {2}", tweet.Id, tweet.Language.ToString(), tweet.Text);
                                        Console.ForegroundColor = ConsoleColor.White;
                                        Console.WriteLine("\tLocation: {0}, {1}", tweet.Coordinates.Longitude, tweet.Coordinates.Latitude);
                                    }

                                    timer.Restart();
                                    Console.WriteLine("\tTweets/sec: {0}", tweetCount);
                                    tweetCount = 0;
                                }
                            };

                            stream.StartStreamMatchingAllConditions();
                        }
                        catch (Exception ex)
                        {
                            Console.WriteLine("Exception: {0}", ex.Message);
                        }
                    }
                }

            }
        }

8. Legen Sie die Konstanten, einschließlich **TWITTERAPPACCESSTOKEN**, **TWITTERAPPACCESSTOKENSECRET**, **TWITTERAPPAPIKEY** und **TWITTERAPPAPISECRET**. 

Drücken Sie **F5**, um streaming-Dienst ausführen. Im folgenden finden einen Screenshot der Konsolenanwendung:

![hdinsight.hbase.twitter.sentiment.Streaming.Service][img-streaming-service]
    
Laufen Sie die Streaming-Konsole Anwendung Web-Anwendung zu entwickeln, haben Sie mehr Daten zur Verfügung. Verwenden Sie zum Überprüfen der Daten in die Tabelle eingefügt HBase Shell. [Erste Schritte mit HBase in HDInsight](hdinsight-hbase-tutorial-get-started.md#create-tables-and-insert-data)anzeigen


## <a name="visualize-real-time-sentiment"></a>Visualisierung in Echtzeit Stimmung

In diesem Abschnitt erstellen Sie eine ASP.NET MVC-Webanwendungsprojekt Daten in Echtzeit Gefühl von HBase lesen und die Daten auf Bing Maps.

**Eine ASP.NET MVC Web-Anwendung erstellen**

1. Öffnen Sie Visual Studio.
2. Klicken Sie auf **Datei**, klicken Sie auf **neu**, und klicken Sie auf **Projekt**.
3. Geben Sie Folgendes ein:

    - Vorlagenkategorie: **Visual C# / Web**
    - Vorlage: **ASP.NET Web-Anwendung**
    - Name: **TweetSentimentWeb**
    - Ort: **C:\Tutorials** 
4. Klicken Sie auf **OK**.
5. Klicken Sie unter **Wählen Sie eine Vorlage**auf **MVC**. 
6. **Microsoft Azure**klicken Sie auf **Abonnements verwalten**.
7. **Microsoft Azure-Abonnements verwalten**klicken Sie auf **Anmelden**.
8. Geben Sie Ihre Azure-Anmeldeinformationen. Ihre Azure Abonnementinformationen wird auf der Registerkarte **Konten** angezeigt.
9. Klicken Sie auf **Schließen** , um das Fenster **Microsoft Azure-Abonnements verwalten** zu schließen.
10. **Neue ASP.NET Projekt - TweetSentimentWeb**klicken Sie auf **OK**.
11. Wählen Sie **Configure Microsoft Azure Site Settings**die **Region** am nächsten. Sie müssen einen Datenbankserver angeben. 
12. Klicken Sie auf **OK**.

**Installieren von NuGet-Pakete**

1. Klicken Sie im Menü **Extras** auf **Nuget Paket-Manager**und klicken Sie dann auf **Paket-Manager-Konsole**. Die Konsole wird am unteren Rand der Seite geöffnet.
2. Verwenden Sie den folgenden Befehl [HBase.NET SDK](https://www.nuget.org/packages/Microsoft.HBase.Client/) -Paket installieren die Clientbibliothek Zugriff auf HBase Cluster:

        Install-Package Microsoft.HBase.Client 

**HBaseReader-Klasse hinzufügen**

1. Erweitern Sie im **Projektmappen-Explorer** **TweetSentiment**aus.
2. Maustaste **Modelle**, klicken Sie auf **Hinzufügen**und dann auf **Klasse**.
3. Geben Sie im Feld **Name** **HBaseReader.cs**und klicken Sie dann auf **Hinzufügen**.
4. Ersetzen Sie den Code durch Folgendes:

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Web;
        
        using System.Configuration;
        using System.Threading.Tasks;
        using System.Text;
        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;
        
        namespace TweetSentimentWeb.Models
        {
            public class HBaseReader
            {
                // For reading Tweet sentiment data from HDInsight HBase
                HBaseClient client;
        
                // HDinsight HBase cluster and HBase table information
                const string CLUSTERNAME = "<HBaseClusterName>";
                const string HADOOPUSERNAME = "<HBaseClusterHadoopUserName>"
                const string HADOOPUSERPASSWORD = "<HBaseCluserUserPassword>";
                const string HBASETABLENAME = "tweets_by_words";
        
                // The constructor
                public HBaseReader()
                {
                    ClusterCredentials creds = new ClusterCredentials(
                                    new Uri(CLUSTERNAME),
                                    HADOOPUSERNAME,
                                    HADOOPUSERPASSWORD);
                    client = new HBaseClient(creds);
                }
        
                // Query Tweets sentiment data from the HBase table asynchronously 
                public async Task<IEnumerable<Tweet>> QueryTweetsByKeywordAsync(string keyword)
                {
                    List<Tweet> list = new List<Tweet>();
        
                    // Demonstrate Filtering the data from the past 6 hours the row key
                    string timeIndex = (ulong.MaxValue -
                        (ulong)DateTime.UtcNow.Subtract(new TimeSpan(6, 0, 0)).ToBinary()).ToString().PadLeft(20);
                    string startRow = keyword + "_" + timeIndex;
                    string endRow = keyword + "|";
                    Scanner scanSettings = new Scanner
                    {
                        batch = 100000,
                        startRow = Encoding.UTF8.GetBytes(startRow),
                        endRow = Encoding.UTF8.GetBytes(endRow)
                    };
        
                    // Make async scan call
                    ScannerInformation scannerInfo =
                        await client.CreateScannerAsync(HBASETABLENAME, scanSettings);
        
                    CellSet next;
        
                    while ((next = await client.ScannerGetNextAsync(scannerInfo)) != null)
                    {
                        foreach (CellSet.Row row in next.rows)
                        {
                            // find the cell with string pattern "d:coor" 
                            var coordinates =
                                row.values.Find(c => Encoding.UTF8.GetString(c.column) == "d:coor");
        
                            if (coordinates != null)
                            {
                                string[] lonlat = Encoding.UTF8.GetString(coordinates.data).Split(',');
        
                                var sentimentField =
                                    row.values.Find(c => Encoding.UTF8.GetString(c.column) == "d:sentiment");
                                Int32 sentiment = 0;
                                if (sentimentField != null)
                                {
                                    sentiment = Convert.ToInt32(Encoding.UTF8.GetString(sentimentField.data));
                                }
        
                                list.Add(new Tweet
                                {
                                    Longtitude = Convert.ToDouble(lonlat[0]),
                                    Latitude = Convert.ToDouble(lonlat[1]),
                                    Sentiment = sentiment
                                });
                            }
        
                            if (coordinates != null)
                            {
                                string[] lonlat = Encoding.UTF8.GetString(coordinates.data).Split(',');
                            }
                        }
                    }
        
                    return list;
                }
            }
        
            public class Tweet
            {
                public string IdStr { get; set; }
                public string Text { get; set; }
                public string Lang { get; set; }
                public double Longtitude { get; set; }
                public double Latitude { get; set; }
                public int Sentiment { get; set; }
            }
        }

4. Ändern Sie in der Klasse **HBaseReader** Konstante Werte wie folgt:

    - **CLUSTERNAME**: die HBase Clusternamen, z. B. *https://<HBaseClusterName>.azurehdinsight.net/*. 
    - **HADOOPUSERNAME**: die HBase Cluster Hadoop-Benutzernamen. Der Standardname lautet *Admin*.
    - **HADOOPUSERPASSWORD**: die HBase Cluster Hadoop Benutzerkennwort.
    - **HBASETABLENAME** = "Tweets_by_words";

    Der Name der HBase ist **"Tweets_by_words"**. Die Werte müssen im streaming-Dienst gesendeten Werte übereinstimmen, damit die Anwendung die Daten aus derselben Tabelle HBase liest.




**TweetsController-Controller hinzufügen**

1. Erweitern Sie im **Projektmappen-Explorer** **TweetSentimentWeb**aus.
2. **Controller**Maustaste klicken Sie auf **Hinzufügen**, und klicken Sie auf **Controller**.
3. Klicken Sie auf **Web API 2 Controller - leer**und klicken Sie auf **Hinzufügen**.
4. Geben Sie im Feld **Name des Domänencontrollers** **TweetsController**, und klicken Sie dann auf **Hinzufügen**.
5. Doppelklicken Sie im **Projektmappen-Explorer**auf TweetsController.cs um die Datei zu öffnen.
5. Ändern Sie die Datei, damit er wie folgt aussieht:

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Http;
        
        using System.Threading.Tasks;
        using TweetSentimentWeb.Models;
        
        namespace TweetSentimentWeb.Controllers
        {
            public class TweetsController : ApiController
            {
                HBaseReader hbase = new HBaseReader();
        
                public async Task<IEnumerable<Tweet>> GetTweetsByQuery(string query)
                {
                    return await hbase.QueryTweetsByKeywordAsync(query);
                }
            }
        }

**Heatmap.js hinzufügen**

1. Erweitern Sie im **Projektmappen-Explorer** **TweetSentimentWeb**aus.
2. Maustaste auf **Skripts**, klicken Sie auf **Hinzufügen**, klicken Sie auf **JavaScript-Datei**.
3. Geben Sie in das Feld **Name** **heatmap.js**.
4. Fügen Sie den folgenden Code in die Datei. Der Code wurde von Alastair Aitchison geschrieben. Weitere Informationen finden Sie unter [Bing Maps AJAX v7 HeatMap Bibliothek](http://alastaira.wordpress.com/2011/04/15/bing-maps-ajax-v7-heatmap-library/).

        /*******************************************************************************
        * Author: Alastair Aitchison
        * Website: http://alastaira.wordpress.com
        * Date: 15th April 2011
        * 
        * Description: 
        * This JavaScript file provides an algorithm that can be used to add a heatmap
        * overlay on a Bing Maps v7 control. The intensity and temperature palette
        * of the heatmap are designed to be easily customisable.
        *
        * Requirements:
        * The heatmap layer itself is created dynamically on the client-side using
        * the HTML5 <canvas> element, and therefore requires a browser that supports
        * this element. It has been tested on IE9, Firefox 3.6/4 and 
        * Chrome 10 browsers. If you can confirm whether it works on other browsers or
        * not, I'd love to hear from you!
        
        * Usage:
        * The HeatMapLayer constructor requires:
        * - A reference to a map object
        * - An array or Microsoft.Maps.Location items
        * - Optional parameters to customise the appearance of the layer
        *  (Radius,, Unit, Intensity, and ColourGradient), and a callback function
        *
        */
        
        var HeatMapLayer = function (map, locations, options) {
        
            /* Private Properties */
            var _map = map,
              _canvas,
              _temperaturemap,
              _locations = [],
              _viewchangestarthandler,
              _viewchangeendhandler;
        
            // Set default options
            var _options = {
                // Opacity at the centre of each heat point
                intensity: 0.5,
        
                // Affected radius of each heat point
                radius: 1000,
        
                // Whether the radius is an absolute pixel value or meters
                unit: 'meters',
        
                // Colour temperature gradient of the map
                colourgradient: {
                    "0.00": 'rgba(255,0,255,20)',  // Magenta
                    "0.25": 'rgba(0,0,255,40)',    // Blue
                    "0.50": 'rgba(0,255,0,80)',    // Green
                    "0.75": 'rgba(255,255,0,120)', // Yellow
                    "1.00": 'rgba(255,0,0,150)'    // Red
                },
        
                // Callback function to be fired after heatmap layer has been redrawn 
                callback: null
            };
        
            /* Private Methods */
            function _init() {
                var _mapDiv = _map.getRootElement();
        
                if (_mapDiv.childNodes.length >= 3 && _mapDiv.childNodes[2].childNodes.length >= 2) {
                    // Create the canvas element
                    _canvas = document.createElement('canvas');
                    _canvas.style.position = 'relative';
        
                    var container = document.createElement('div');
                    container.style.position = 'absolute';
                    container.style.left = '0px';
                    container.style.top = '0px';
                    container.appendChild(_canvas);
        
                    _mapDiv.childNodes[2].childNodes[1].appendChild(container);
        
                    // Override defaults with any options passed in the constructor
                    _setOptions(options);
        
                    // Load array of location data
                    _setPoints(locations);
        
                    // Create a colour gradient from the suppied colourstops
                    _temperaturemap = _createColourGradient(_options.colourgradient);
        
                    // Wire up the event handler to redraw heatmap canvas
                    _viewchangestarthandler = Microsoft.Maps.Events.addHandler(_map, 'viewchangestart', _clearHeatMap);
                    _viewchangeendhandler = Microsoft.Maps.Events.addHandler(_map, 'viewchangeend', _createHeatMap);
        
                    _createHeatMap();
        
                    delete _init;
                } else {
                    setTimeout(_init, 100);
                }
            }
        
            // Resets the heat map
            function _clearHeatMap() {
                var ctx = _canvas.getContext("2d");
                ctx.clearRect(0, 0, _canvas.width, _canvas.height);
            }
        
            // Creates a colour gradient from supplied colour stops on initialisation
            function _createColourGradient(colourstops) {
                var ctx = document.createElement('canvas').getContext('2d');
                var grd = ctx.createLinearGradient(0, 0, 256, 0);
                for (var c in colourstops) {
                    grd.addColorStop(c, colourstops[c]);
                }
                ctx.fillStyle = grd;
                ctx.fillRect(0, 0, 256, 1);
                return ctx.getImageData(0, 0, 256, 1).data;
            }
        
            // Applies a colour gradient to the intensity map
            function _colouriseHeatMap() {
                var ctx = _canvas.getContext("2d");
                var dat = ctx.getImageData(0, 0, _canvas.width, _canvas.height);
                var pix = dat.data; // pix is a CanvasPixelArray containing height x width x 4 bytes of data (RGBA)
                for (var p = 0, len = pix.length; p < len;) {
                    var a = pix[p + 3] * 4; // get the alpha of this pixel
                    if (a != 0) { // If there is any data to plot
                        pix[p] = _temperaturemap[a]; // set the red value of the gradient that corresponds to this alpha
                        pix[p + 1] = _temperaturemap[a + 1]; //set the green value based on alpha
                        pix[p + 2] = _temperaturemap[a + 2]; //set the blue value based on alpha
                    }
                    p += 4; // Move on to the next pixel
                }
                ctx.putImageData(dat, 0, 0);
            }
        
            // Sets any options passed in
            function _setOptions(options) {
                for (attrname in options) {
                    _options[attrname] = options[attrname];
                }
            }
        
            // Sets the heatmap points from an array of Microsoft.Maps.Locations  
            function _setPoints(locations) {
                _locations = locations;
            }
        
            // Main method to draw the heatmap
            function _createHeatMap() {
                // Ensure the canvas matches the current dimensions of the map
                // This also has the effect of resetting the canvas
                _canvas.height = _map.getHeight();
                _canvas.width = _map.getWidth();
        
                _canvas.style.top = -_canvas.height / 2 + 'px';
                _canvas.style.left = -_canvas.width / 2 + 'px';
        
                // Calculate the pixel radius of each heatpoint at the current map zoom
                if (_options.unit == "pixels") {
                    radiusInPixel = _options.radius;
                } else {
                    radiusInPixel = _options.radius / _map.getMetersPerPixel();
                }
        
                var ctx = _canvas.getContext("2d");
        
                // Convert lat/long to pixel location
                var pixlocs = _map.tryLocationToPixel(_locations, Microsoft.Maps.PixelReference.control);
                var shadow = 'rgba(0, 0, 0, ' + _options.intensity + ')';
                var mapWidth = 256 * Math.pow(2, _map.getZoom());
        
                // Create the Intensity Map by looping through each location
                for (var i = 0, len = pixlocs.length; i < len; i++) {
                    var x = pixlocs[i].x;
                    var y = pixlocs[i].y;
        
                    if (x < 0) {
                        x += mapWidth * Math.ceil(Math.abs(x / mapWidth));
                    }
        
                    // Create radial gradient centred on this point
                    var grd = ctx.createRadialGradient(x, y, 0, x, y, radiusInPixel);
                    grd.addColorStop(0.0, shadow);
                    grd.addColorStop(1.0, 'transparent');
        
                    // Draw the heatpoint onto the canvas
                    ctx.fillStyle = grd;
                    ctx.fillRect(x - radiusInPixel, y - radiusInPixel, 2 * radiusInPixel, 2 * radiusInPixel);
                }
        
                // Apply the specified colour gradient to the intensity map
                _colouriseHeatMap();
        
                // Call the callback function, if specified
                if (_options.callback) {
                    _options.callback();
                }
            }
        
            /* Public Methods */
        
            this.Show = function () {
                if (_canvas) {
                    _canvas.style.display = '';
                }
            };
        
            this.Hide = function () {
                if (_canvas) {
                    _canvas.style.display = 'none';
                }
            };
        
            // Sets options for intensity, radius, colourgradient etc.
            this.SetOptions = function (options) {
                _setOptions(options);
            }
        
            // Sets an array of Microsoft.Maps.Locations from which the heatmap is created
            this.SetPoints = function (locations) {
                // Reset the existing heatmap layer
                _clearHeatMap();
                // Pass in the new set of locations
                _setPoints(locations);
                // Recreate the layer
                _createHeatMap();
            }
        
            // Removes the heatmap layer from the DOM
            this.Remove = function () {
                _canvas.parentNode.parentNode.removeChild(_canvas.parentNode);
        
                if (_viewchangestarthandler) { Microsoft.Maps.Events.removeHandler(_viewchangestarthandler); }
                if (_viewchangeendhandler) { Microsoft.Maps.Events.removeHandler(_viewchangeendhandler); }
        
                _locations = null;
                _temperaturemap = null;
                _canvas = null;
                _options = null;
                _viewchangestarthandler = null;
                _viewchangeendhandler = null;
            }
        
            // Call the initialisation routine
            _init();
        };
        
        // Call the Module Loaded method
        Microsoft.Maps.moduleLoaded('HeatMapModule');


**TwitterStream.js hinzufügen**

1. Erweitern Sie im **Projektmappen-Explorer** **TweetSentimentWeb**aus.
2. Maustaste auf **Skripts**, klicken Sie auf **Hinzufügen**, klicken Sie auf **JavaScript-Datei**.
3. Geben Sie in das Feld **Name** **twitterStream.js**.
4. Kopieren Sie und fügen Sie den folgenden Code in die Datei:

        var liveTweetsPos = [];
        var liveTweets = [];
        var liveTweetsNeg = [];
        var map;
        var heatmap;
        var heatmapNeg;
        var heatmapPos;
        
        function initialize() {
            // Initialize the map
            var options = {
                credentials: "AvFJTZPZv8l3gF8VC3Y7BPBd0r7LKo8dqKG02EAlqg9WAi0M7la6zSIT-HwkMQbx",
                center: new Microsoft.Maps.Location(23.0, 8.0),
                mapTypeId: Microsoft.Maps.MapTypeId.ordnanceSurvey,
                labelOverlay: Microsoft.Maps.LabelOverlay.hidden,
                zoom: 2.5
            };
            var map = new Microsoft.Maps.Map(document.getElementById('map_canvas'), options);
        
            // Heatmap options for positive, neutral and negative layers
        
            var heatmapOptions = {
                // Opacity at the centre of each heat point
                intensity: 0.5,
        
                // Affected radius of each heat point
                radius: 15,
        
                // Whether the radius is an absolute pixel value or meters
                unit: 'pixels'
            };
        
            var heatmapPosOptions = {
                // Opacity at the centre of each heat point
                intensity: 0.5,
        
                // Affected radius of each heat point
                radius: 15,
        
                // Whether the radius is an absolute pixel value or meters
                unit: 'pixels',
        
                colourgradient: {
                    0.0: 'rgba(0, 255, 255, 0)',
                    0.1: 'rgba(0, 255, 255, 1)',
                    0.2: 'rgba(0, 255, 191, 1)',
                    0.3: 'rgba(0, 255, 127, 1)',
                    0.4: 'rgba(0, 255, 63, 1)',
                    0.5: 'rgba(0, 127, 0, 1)',
                    0.7: 'rgba(0, 159, 0, 1)',
                    0.8: 'rgba(0, 191, 0, 1)',
                    0.9: 'rgba(0, 223, 0, 1)',
                    1.0: 'rgba(0, 255, 0, 1)'
                }
            };
        
            var heatmapNegOptions = {
                // Opacity at the centre of each heat point
                intensity: 0.5,
        
                // Affected radius of each heat point
                radius: 15,
        
                // Whether the radius is an absolute pixel value or meters
                unit: 'pixels',
        
                colourgradient: {
                    0.0: 'rgba(0, 255, 255, 0)',
                    0.1: 'rgba(0, 255, 255, 1)',
                    0.2: 'rgba(0, 191, 255, 1)',
                    0.3: 'rgba(0, 127, 255, 1)',
                    0.4: 'rgba(0, 63, 255, 1)',
                    0.5: 'rgba(0, 0, 127, 1)',
                    0.7: 'rgba(0, 0, 159, 1)',
                    0.8: 'rgba(0, 0, 191, 1)',
                    0.9: 'rgba(0, 0, 223, 1)',
                    1.0: 'rgba(0, 0, 255, 1)'
                }
            };
        
            // Register and load the Client Side HeatMap Module
            Microsoft.Maps.registerModule("HeatMapModule", "scripts/heatmap.js");
            Microsoft.Maps.loadModule("HeatMapModule", {
                callback: function () {
                    // Create heatmap layers for positive, neutral and negative tweets
                    heatmapPos = new HeatMapLayer(map, liveTweetsPos, heatmapPosOptions);
                    heatmap = new HeatMapLayer(map, liveTweets, heatmapOptions);
                    heatmapNeg = new HeatMapLayer(map, liveTweetsNeg, heatmapNegOptions);
                }
            });
        
            $("#searchbox").val("xbox");
            $("#searchBtn").click(onsearch);
            $("#positiveBtn").click(onPositiveBtn);
            $("#negativeBtn").click(onNegativeBtn);
            $("#neutralBtn").click(onNeutralBtn);
            $("#neutralBtn").button("toggle");
        }
        
        function onsearch() {
            var uri = 'api/tweets?query=';
            var query = $('#searchbox').val();
            $.getJSON(uri + query)
                .done(function (data) {
                    liveTweetsPos = [];
                    liveTweets = [];
                    liveTweetsNeg = [];
        
                    // On success, 'data' contains a list of tweets.
                    $.each(data, function (key, item) {
                        addTweet(item);
                    });
        
                    if (!$("#neutralBtn").hasClass('active')) {
                        $("#neutralBtn").button("toggle");
                    }
                    onNeutralBtn();
                })
                .fail(function (jqXHR, textStatus, err) {
                    $('#statustext').text('Error: ' + err);
                });
        }
        
        function addTweet(item) {
            //Add tweet to the heat map arrays.
            var tweetLocation = new Microsoft.Maps.Location(item.Latitude, item.Longtitude);
            if (item.Sentiment > 0) {
                liveTweetsPos.push(tweetLocation);
            } else if (item.Sentiment < 0) {
                liveTweetsNeg.push(tweetLocation);
            } else {
                liveTweets.push(tweetLocation);
            }
        }
        
        function onPositiveBtn() {
            if ($("#neutralBtn").hasClass('active')) {
                $("#neutralBtn").button("toggle");
            }
            if ($("#negativeBtn").hasClass('active')) {
                $("#negativeBtn").button("toggle");
            }
        
            heatmapPos.SetPoints(liveTweetsPos);
            heatmapPos.Show();
            heatmapNeg.Hide();
            heatmap.Hide();
        
            $('#statustext').text('Tweets: ' + liveTweetsPos.length + "   " + getPosNegRatio());
        }
        
        function onNeutralBtn() {
            if ($("#positiveBtn").hasClass('active')) {
                $("#positiveBtn").button("toggle");
            }
            if ($("#negativeBtn").hasClass('active')) {
                $("#negativeBtn").button("toggle");
            }
        
            heatmap.SetPoints(liveTweets);
            heatmap.Show();
            heatmapNeg.Hide();
            heatmapPos.Hide();
        
            $('#statustext').text('Tweets: ' + liveTweets.length + "   " + getPosNegRatio());
        }
        
        function onNegativeBtn() {
            if ($("#positiveBtn").hasClass('active')) {
                $("#positiveBtn").button("toggle");
            }
            if ($("#neutralBtn").hasClass('active')) {
                $("#neutralBtn").button("toggle");
            }
        
            heatmapNeg.SetPoints(liveTweetsNeg);
            heatmapNeg.Show();
            heatmap.Hide();;
            heatmapPos.Hide();;
        
            $('#statustext').text('Tweets: ' + liveTweetsNeg.length + "\t" + getPosNegRatio());
        }
        
        function getPosNegRatio() {
            if (liveTweetsNeg.length == 0) {
                return "";
            }
            else {
                var ratio = liveTweetsPos.length / liveTweetsNeg.length;
                var str = parseFloat(Math.round(ratio * 10) / 10).toFixed(1);
                return "Positive/Negative Ratio: " + str;
            }
        }


**Ändern der layout.cshtml**

1. **Projektmappen-Explorer** **TweetSentimentWeb**erweitern erweitern Sie **Ansichten**, **Shared**und anschließend Doppelklicken Sie auf _**Layout.cshtml**.
2. Ersetzen Sie den Inhalt durch Folgendes:

        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>@ViewBag.Title</title>
            @Styles.Render("~/Content/css")
            @Scripts.Render("~/bundles/modernizr")
            <!-- Bing Maps -->
            <script type="text/javascript" src="http://ecn.dev.virtualearth.net/mapcontrol/mapcontrol.ashx?v=7.0&mkt=en-gb"></script>
            <!-- Spatial Dashboard JavaScript -->
            <script src="~/Scripts/twitterStream.js" type="text/javascript"></script>
        </head>
        <body onload="initialize()">
            <div class="navbar navbar-inverse navbar-fixed-top">
                <div class="container">
                    <div class="navbar-header">
                        <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                            <span class="icon-bar"></span>
                            <span class="icon-bar"></span>
                            <span class="icon-bar"></span>
                        </button>
                    </div>
                    <div class="navbar-collapse collapse">
                        <div class="row">
                            <ul class="nav navbar-nav col-lg-5">
                                <li class="col-lg-12">
                                    <div class="navbar-form">
                                        <input id="searchbox" type="search" class="form-control">
                                        <button type="button" id="searchBtn" class="btn btn-primary">Go</button>
                                    </div>
                                </li>
                            </ul>
                            <ul class="nav navbar-nav col-lg-7">
                                <li>
                                    <div class="navbar-form">
                                        <div class="btn-group" data-toggle="buttons-radio">
                                            <button type="button" id="positiveBtn" class="btn btn-primary">Positive</button>
                                            <button type="button" id="neutralBtn" class="btn btn-primary">Neutral</button>
                                            <button type="button" id="negativeBtn" class="btn btn-primary">Negative</button>
                                        </div>
                                    </div>
                                </li>
                                <li><span id="statustext" class="navbar-text"></span></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
            <div class="map_container">
                @RenderBody()
            </div>
            @Scripts.Render("~/bundles/jquery")
            @Scripts.Render("~/bundles/bootstrap")
            @RenderSection("scripts", required: false)
        </body>
        </html>



**Ändern der Index.cshtml**

1. **Projektmappen-Explorer**- **TweetSentimentWeb**erweitern, erweitern Sie **Ansichten** **Home**erweitern und doppelklicken Sie auf **Index.cshtml**.
2. Ersetzen Sie den Inhalt durch Folgendes:

        @{
            ViewBag.Title = "Tweet Sentiment";
        }
        
        <div class="map_container">
            <div id="map_canvas"/>
        </div>

**Die Datei "Site.CSS" ändern**

1. **Projektmappen-Explorer**erweitern Sie **TweetSentimentWeb**, erweitern Sie **Content**und doppelklicken Sie dann auf **"Site.CSS"**.
2. Fügen Sie den folgenden Code in die Datei:
        
        /* make container, and thus map, 100% width */
        .map_container {
            width: 100%;
            height: 100%;
        }
        
        #map_canvas{
          height:100%;
        }
        
        #tweets{
          position: absolute;
          top: 60px;
          left: 75px;
          z-index:1000;
          font-size: 30px;
        }

**So ändern Sie die Datei global.asax**

1. **Projektmappen-Explorer**erweitern Sie **TweetSentimentWeb**und doppelklicken Sie dann auf **"Global.asax"**.
2. Fügen Sie die folgende Anweisung **verwenden** :

        using System.Web.Http;

2. Fügen Sie folgende Zeilen in der **Application_Start()** -Funktion:

        // Register API routes
        GlobalConfiguration.Configure(WebApiConfig.Register);
  
    Ändern der Registrierung API Routen zu Web API-Controller innerhalb der MVC-Anwendung funktionieren.

**Anwendung ausführen**

1. Überprüfen Sie ob streaming Service Console Anwendung läuft in Echtzeit ändern zu sehen.
2. Drücken Sie **F5** , um die Anwendung auszuführen:

    ![hdinsight.hbase.twitter.sentiment.Bing.Map][img-bing-map]
2. Geben Sie im Textfeld einen Suchbegriff, und klicken Sie auf **OK**.  Einige Schlüsselwörter können je nach den Daten in der HBase-Tabelle nicht gefunden. Versuchen Sie, einige allgemeine Schlüsselwörter wie "Liebe", "Xbox" und "Playstation." 
3. **Positiv**, **Neutral**und **Negative** Stimmung dazu vergleichen umschalten
4. Lassen Sie streaming-Dienst für eine Stunde ausgeführt und die gleichen Schlüsselwörter suchen und vergleichen.

 
Optional können Sie die Anwendung auf Azure Websites bereitstellen. Informationen finden Sie in [Erste Schritte mit Azure Websites und ASP.NET][website-get-started].
 
## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie Tweets abgerufen, analysiert die Stimmung TWEETS, Stimmung speichern, HBase und Aufbereitung in Echtzeit Twitter Stimmung auf Bing Maps. Weitere Informationen finden Sie unter:

- [Erste Schritte mit HDInsight][hdinsight-get-started]
- [Konfigurieren der Replikation HBase in HDInsight](hdinsight-hbase-geo-replication.md) 
- [Datenanalyse Twitter Hadoop in HDInsight][hdinsight-analyze-twitter-data]
- [Datenanalyse Flug Verzögerung mit HDInsight][hdinsight-analyze-flight-delay-data]
- [Entwickeln von Java MapReduce Programme für HDInsight][hdinsight-develop-mapreduce]


[hbase-get-started]: hdinsight-hbase-tutorial-get-started-linux.md
[website-get-started]: ../app-service-web/web-sites-dotnet-get-started.md



[img-app-arch]: ./media/hdinsight-hbase-analyze-twitter-sentiment/AppArchitecture.png
[img-twitter-app]: ./media/hdinsight-hbase-analyze-twitter-sentiment/TwitterApp.png
[img-streaming-service]: ./media/hdinsight-hbase-analyze-twitter-sentiment/StreamingService.png
[img-bing-map]: ./media/hdinsight-hbase-analyze-twitter-sentiment/TwitterSentimentBingMap.png



[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-analyze-twitter-data]: hdinsight-analyze-twitter-data.md




[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
 
