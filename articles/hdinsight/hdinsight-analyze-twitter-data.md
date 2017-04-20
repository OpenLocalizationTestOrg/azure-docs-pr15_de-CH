<properties
    pageTitle="Datenanalyse Twitter Hadoop in HDInsight | Microsoft Azure"
    description="Erfahren Sie mit Struktur Hadoop in HDInsight die Verwendung Frequenz eines bestimmten Worts Twitter Daten analysieren."
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
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analysieren Sie Twitter-Daten mithilfe von Struktur in HDInsight

Social-Websites sind eine der wichtigsten treibenden Kräfte für big Data Annahme. Von Websites wie Twitter bereitgestellten öffentlichen APIs bieten nützliche Daten für Analyse und beliebte Trends. In diesem Lernprogramm wird Tweets mit Twitter streaming-API abrufen und dann mit der Apache Struktur auf Azure HDInsight twitterbenutzer aufzulisten, die meisten Tweets gesendet hat, die ein bestimmtes Wort enthalten.

> [AZURE.NOTE] Die Schritte in diesem Dokument ist einen Windows-basierte HDInsight-Cluster erforderlich. Bestimmte Linux-basierten Cluster finden Sie unter [Analysieren Twitter Daten mit Struktur in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Eine Arbeitsstation** mit Azure PowerShell installiert und konfiguriert. 

    Zum Ausführen von Windows PowerShell-Skripts müssen Sie Azure PowerShell als Administrator ausführen und die Ausführungsrichtlinie auf *RemoteSigned*festgelegt. [Führen Sie Windows PowerShell-Skripts]finden Sie unter[powershell-script].

    Vor dem Ausführen von Windows PowerShell-Skripts, stellen Sie sicher, dass Sie Ihre Azure-Abonnements sind mit dem folgenden Cmdlet:

        Login-AzureRmAccount

    Haben mehrere Azure-Abonnements mit dem folgenden Cmdlet das aktuelle Abonnement fest:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Ein Azure HDInsight-Cluster**. Hinweise auf Cluster-Bereitstellung finden Sie unter [Erste Schritte mit HDInsight] [ hdinsight-get-started] oder [Bereitstellung HDInsight Cluster] [hdinsight-provision]. Sie benötigen den Namen des Clusters später im Lernprogramm.

In der folgenden Tabelle sind die in diesem Lernprogramm verwendeten Dateien aufgeführt:

Dateien|Beschreibung
---|---
/Tutorials/Twitter/Data/TWEETS.txt|Die Quelldaten für den Hive-Auftrag.
/Tutorials/Twitter/Output|Der Ausgabeordner für die Struktur. Die Struktur Auftrag Standardausgabedateinamen ist **000000_0**.
Tutorials/Twitter/Twitter.hql|Die HiveQL-Skriptdatei.
/Tutorials/Twitter/Jobstatus|Auftragsstatus Hadoop.


##<a name="get-twitter-feed"></a>Twitter-feed abrufen

In diesem Lernprogramm verwenden Sie [Streaming-APIs Twitter][twitter-streaming-api]. Die spezifische Twitter streaming-API Sie verwenden [Status-Filter][twitter-statuses-filter].

>[AZURE.NOTE] Eine Datei mit 10.000 Tweets und die Hive-Skriptdatei (im nächsten Abschnitt behandelt) in einem öffentlichen BLOB-Container hochgeladen. Wenn die hochgeladenen Dateien verwenden möchten, können Sie diesen Abschnitt überspringen.

[TWEETS Daten](https://dev.twitter.com/docs/platform-objects/tweets) ist in JavaScript Object Notation (JSON) Format gespeichert, komplexe geschachtelte Struktur enthält. Statt viele Codezeilen mit einer konventionellen Programmiersprache, Sie können machen geschachtelte Struktur Struktur, sodass es durch eine strukturierte Abfragesprache (SQL) abgefragt werden kann – wie Sprache namens HiveQL.

Twitter verwendet OAuth den autorisierten Zugriff auf die API. OAuth ist ein Authentifizierungsprotokoll, das Benutzern erlaubt, Programme ohne ihr Kennwort handeln genehmigen. Weitere Informationen zur [oauth.net](http://oauth.net/) oder hervorragende [Einführung in OAuth](http://hueniverse.com/oauth/) aus Hueniverse finden.

Zunächst verwenden Sie OAuth ist auf Twitter Developer Site eine neue Anwendung erstellen.

**Erstellen eine twitteranwendung**

1. [Https://apps.twitter.com/](https://apps.twitter.com/)anmelden. Klicken Sie **Jetzt anmelden** haben Sie einen Twitter-Konto.
2. Klicken Sie auf **neue Anwendung**.
3. Geben Sie **Name**, **Beschreibung**, **Website**. Sie können einen URL für die **Website** -Feld. Die folgende Tabelle zeigt einige Werte verwenden:

    Feld|Wert
    ---|---
    Name|MyHDInsightApp
    Beschreibung|MyHDInsightApp
    Website|http://www.myhdinsightapp.com

4. Überprüfen Sie **Ja, ich stimme zu**und dann auf **der Twitter-Anwendung erstellen**.
5. Klicken Sie auf die Registerkarte **Berechtigungen** . Die Standardberechtigung ist **schreibgeschützt**. Dies reicht für dieses Lernprogramm.
6. Klicken Sie auf der Registerkarte **Tasten und Zugriffstoken** .
7. Klicken Sie auf **Meine Zugriffstoken erstellen**.
8. Klicken Sie auf **Test OAuth** in der oberen rechten Ecke der Seite.
9. Notieren Sie **Verbraucherschlüssel**, **Verbrauchergeheimnis** **Zugriffstoken**und **Access token geheim**. Sie benötigen die Werte später im Lernprogramm.

In diesem Lernprogramm verwenden Sie Windows PowerShell zu den Webdienst aufrufen. Eine .NET C#, finden Sie unter [Analyse in Echtzeit Twitter Stimmung mit HBase in HDInsight][hdinsight-hbase-twitter-sentiment]. Die andere beliebte Web Service-Aufrufe ist [*Aufrollen*][curl]. Aufrollen kann [hier]heruntergeladen werden[curl-download].

>[AZURE.NOTE] Bei Verwendung von Curl-Befehl in Windows verwenden Sie doppelte Anführungszeichen statt Anführungszeichen für die Werte.

**Tweets zu**

1. Öffnen Sie die Windows PowerShell Integrated Scripting Environment (ISE). (Auf dem Bildschirm Windows 8 Starten Geben Sie **PowerShell_ISE** und klicken Sie dann auf **Windows PowerShell ISE**. Siehe [Starten von Windows PowerShell auf Windows 8 und Windows][powershell-start].)

2. Kopieren Sie das folgende Skript in das Skriptfenster:

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Die ersten fünf bis acht Variablen im Skript festgelegt:


    Variable|Beschreibung
    ---|---
    $clusterName|Dies ist der Name des Clusters HDInsight die Anwendung ausgeführt werden soll.
    $oauth_consumer_key|Dies ist der Twitter-Anwendung **Consumer Schlüssel** , die Sie zuvor beim Erstellen der Twitter-Anwendung notiert.
    $oauth_consumer_secret|Dies ist der Twitter-Anwendung **Verbrauchergeheimnis** zuvor notiert.
    $oauth_token|Dies ist der Twitter-Anwendung **Zugriffstoken** zuvor notiert.
    $oauth_token_secret|Das ist Twitter Anwendung **token Schlüssel zugreifen** , die Sie zuvor notiert.
    $destBlobName|Dies entspricht der Ausgabename Blob. Der Standardwert ist **tutorials/twitter/data/tweets.txt**. Wenn Sie den Standardwert ändern, müssen Sie die Windows PowerShell-Skripts entsprechend aktualisieren.
    $trackString|Der Webdienst gibt Tweets Zusammenhang mit diesen Schlüsselwörtern zurück. Der Standardwert ist **Azure Cloud, HDInsight**. Wenn Sie den Standardwert ändern, aktualisieren Sie die Windows PowerShell-Skripts entsprechend.
    $lineMax|Der Wert bestimmt, wie viele Tweets Skript lesen. Dauert ca. drei Minuten 100 Tweets lesen. Eine größere Zahl festgelegt, aber es dauert download mehr Zeit.

5. Drücken Sie **F5** , um das Skript auszuführen. Probleme, um dieses Problem zu umgehen, führen wählen Sie alle Positionen aus und drücken Sie dann **F8**.
6. Sie sehen "Vollständig" am Ende der Ausgabe. Fehlermeldungen werden in Rot angezeigt.

Validation-Prozedur können Sie die Ausgabedatei **/tutorials/twitter/data/tweets.txt**im Azure BLOB-Speicher mit einer Azure-Speicher-Explorer oder Azure PowerShell suchen. Ein Windows PowerShell-Beispielskript für Dateien finden Sie unter [mit BLOB-Speicher mit HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>HiveQL-Skript erstellen

Azure PowerShell verwenden, können Sie mehrere HiveQL-Anweisungen eine gleichzeitig oder package-HiveQL-Anweisung in einer Skriptdatei. In diesem Lernprogramm erstellen Sie ein HiveQL Skript. Die Skriptdatei muss Azure BLOB-Speicher hochgeladen werden. Im nächsten Abschnitt führen Sie die Datei mithilfe von Azure PowerShell.

>[AZURE.NOTE] Die Skriptdatei Struktur und eine Datei mit 10.000 Tweets in einem öffentlichen BLOB-Container hochgeladen. Wenn die hochgeladenen Dateien verwenden möchten, können Sie diesen Abschnitt überspringen.

Das HiveQL-Skript führt die folgenden Vorgänge:

1. Bei der Tabelle **der Tweets_raw Tabelle** ist bereits vorhanden.
2. **Erstellen der Struktur Tweets_raw**. Dieser temporäre Struktur hält strukturierte Daten für weitere extrahieren, Transformieren und laden (ETL) verarbeitet. Informationen über Partitionen finden Sie unter [Lernprogramm Struktur][apache-hive-tutorial].  
3. **Laden von Daten** aus dem Quellordner /tutorials/twitter/data. Große Tweets Dataset in geschachtelten JSON-Format wurde nun in eine temporäre Tabellenstruktur Struktur transformiert.
3. Bei der Tabelle **der Tweets Tabelle** ist bereits vorhanden.
4. **Tweets Tabelle erstellen**. Bevor Sie das Dataset Tweets mit Struktur Abfragen können, müssen Sie ein anderes ETL-Prozess ausgeführt. Diese ETL-Prozess definiert eine detailliertere Tabellenschemas für Daten, die in der Tabelle "Twitter_raw" gespeichert.  
5. **Tabelle überschreiben**. Dieser komplexe Struktur Auftakt mehrere lange MapReduce Aufträge Hadoop Cluster. Dataset und der Größe der Cluster konnte dies ungefähr 10 Minuten dauern.
6. **Einsatz Directory überschreiben**. Ausführen einer Abfrage und das Dataset in eine Datei ausgegeben. Dieser Suchanfrage werden einer Liste der Twitter-Benutzer die meisten Tweets mit versendet das Wort "Azure".

**Erstellen Sie ein Skript Struktur und Azure hochladen**

1. Öffnen Sie Windows PowerShell ISE.
2. Kopieren Sie das folgende Skript in das Skriptfenster:

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
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
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Die ersten beiden Variablen im Skript festgelegt:

    Variable|Beschreibung
    ---|---
    $clusterName|Geben Sie den Clusternamen HDInsight soll die Anwendung auszuführen.
    $subscriptionID|Geben Sie Ihre Azure-Abonnement.
    $sourceDataPath|Der Azure BLOB-Speicherort, in dem die Struktur Abfragen die Daten gelesen. Sie müssen diese Variable zu ändern.
    $outputPath|Der Azure BLOB-Speicherort, in dem die Struktur Abfragen der Ergebnisse Ausgabe. Sie müssen diese Variable zu ändern.
    $hqlScriptFile|Den Speicherort und den Namen der Skriptdatei HiveQL. Sie müssen diese Variable zu ändern.

5. Drücken Sie **F5** , um das Skript auszuführen. Probleme, um dieses Problem zu umgehen, führen wählen Sie alle Positionen aus und drücken Sie dann **F8**.
6. Sie sehen "Vollständig" am Ende der Ausgabe. Fehlermeldungen werden in Rot angezeigt.

Validation-Prozedur können Sie die Ausgabedatei **/tutorials/twitter/twitter.hql**im Azure BLOB-Speicher mit einer Azure-Speicher-Explorer oder Azure PowerShell suchen. Ein Windows PowerShell-Beispielskript für Dateien finden Sie unter [mit BLOB-Speicher mit HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Twitter Daten mit Struktur

Sie haben die vorbereitenden. Sie können Hive-Skript aufrufen und die Ergebnisse überprüfen.

### <a name="submit-a-hive-job"></a>Senden eines Auftrags Struktur

Verwenden Sie das folgende Windows PowerShell-Skript Hive-Skript ausgeführt. Sie müssen die erste Variable festlegen.

>[AZURE.NOTE] Um die Tweets und HiveQL Skript Hochladen in den letzten beiden Abschnitten verwenden, legen Sie $hqlScriptFile auf "/ tutorials/twitter/twitter.hql". Um die verwenden, die Sie in einem öffentlichen Blob hochgeladen wurden, legen Sie $hqlScriptFile auf "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>Überprüfen Sie die Ergebnisse

Verwenden Sie das folgende Windows PowerShell-Skript Auftragsausgabe Struktur überprüfen. Sie müssen die ersten beiden Variablen festlegen.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] Die Tabelle Struktur verwendet \001 als Feldtrennzeichen. Das Trennzeichen wird nicht in der Ausgabe angezeigt.

Nachdem die Analyseergebnisse in Azure BLOB-Speicher gesetzt haben, können Sie Exportieren der Daten nach einem SQL Azure-Datenbank-SQL Server mit Power-Abfrage die Daten nach Excel exportieren oder Ihre Anwendung mit Daten verbinden Struktur ODBC-Treiber verwenden. Weitere Informationen finden Sie unter [Verwenden Sqoop mit HDInsight][hdinsight-use-sqoop], [Analyze Verzögerung Flugdaten mit HDInsight][hdinsight-analyze-flight-delay-data], [Verbinden Excel HDInsight Power Abfrage][hdinsight-power-query], und [HDInsight mit der Struktur ODBC-Treiber von Microsoft Excel verbinden][hdinsight-hive-odbc].

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir gesehen, wie Sie machen eine unstrukturierte JSON Dataset strukturierte Struktur Abfragen, untersuchen und analysieren Daten aus Twitter Azure HDInsight mit. Weitere Informationen finden Sie unter:

- [Erste Schritte mit HDInsight][hdinsight-get-started]
- [Analyse in Echtzeit Twitter Stimmung mit HBase in HDInsight][hdinsight-hbase-twitter-sentiment]
- [Datenanalyse Flug Verzögerung mit HDInsight][hdinsight-analyze-flight-delay-data]
- [Schließen Sie Excel an HDInsight mit Power-Abfrage][hdinsight-power-query]
- [Schließen Sie Excel an HDInsight mit der Struktur der Microsoft ODBC-Treiber][hdinsight-hive-odbc]
- [Verwenden Sie Sqoop mit HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
