<properties
    pageTitle="Empfohlene Mahout mit WIndows-basierten HDInsight generieren | Microsoft Azure"
    description="Erfahren Sie mit Apache Mahout Computer lernen Bibliothek Film Recommendations mit Windows-basierten HDInsight (Hadoop) generiert."
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
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Generieren Sie Film-Empfehlung mit Apache Mahout Hadoop in HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Erfahren Sie mit [Apache Mahout](http://mahout.apache.org) Computer Lernen mit Azure HDInsight Film Recommendations generiert.

> [AZURE.NOTE] Die Schritte in diesem Dokument erfordern einen Windows-Client und einem Windows-basierten HDInsight Cluster. Informationen über Mahout Linux, OS X oder Unix-Client mit einer Linux-basierten HDInsight finden Sie unter [generieren Film Recommendations mit Apache Mahout Linux-basierten Hadoop in HDInsight](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>Lernziele

Mahout ist ein [Computer lernen] [ ml] Bibliothek für Apache Hadoop. Mahout enthält Algorithmen für die Verarbeitung von Daten filtern, Klassifizierung und clustering. In diesem Artikel verwenden Sie eine Empfehlung Engine zu Filmen basieren Freunden gesehen Film Recommendations. Sie erfahren auch Klassifikationen mit Entscheidung durchführen. Dies lernen Folgendes Sie:

* Mahout Aufträge mithilfe von Windows PowerShell ausführen

* Mahout-Aufträgen über die Befehlszeile Hadoop ausführen

* Mahout HDInsight 3.0 und HDInsight 2.0 Installieren von Clustern

    > [AZURE.NOTE] Mahout wird mit der Version 3.1 HDInsight Cluster bereitgestellt. Bei Verwendung eine frühere Version von HDInsight finden Sie unter [Mahout installieren](#install) bevor Sie fortfahren.

##<a name="prerequisites"></a>Erforderliche Komponenten

- **Eine Windows-basierte Hadoop Cluster in HDInsight**. Informationen zum Erstellen einer finden Sie unter [Erste Schritte mit Hadoop in HDInsight][getstarted]
- **Eine Workstation mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Mit Windows PowerShell generieren Sie recommendations

> [AZURE.NOTE] Zwar der Auftrag in diesem Abschnitt arbeitet mit Windows PowerShell viele Klassen mit Mahout derzeit funktionieren nicht mit Windows PowerShell und muss über die Befehlszeile Hadoop ausgeführt werden. Eine Liste der Klassen, die nicht mit Windows PowerShell finden Sie im Abschnitt [Problembehandlung](#troubleshooting) .
>
> Ein Beispiel für die Befehlszeile Hadoop Mahout Auftrag ausführen finden Sie unter [Klassifizieren von Daten mithilfe der Befehlszeile Hadoop](#classify).

Eine der Funktionen, die von Mahout bereitgestellten ist eine Empfehlung Engine. Dieses Modul akzeptiert Daten im Format `userID`, `itemId`, und `prefValue` (Benutzer Vorrang für den Artikel). Mahout können Sie gemeinsames Vorkommen Analyse, ausführen: _Benutzer eine Einstellung für ein Element haben auch bevorzugen diese Elemente_. Mahout bestimmt Benutzer mit ähnlichen Artikel Voreinstellungen zur Vorschläge machen.

Nachfolgend ein sehr einfaches Beispiel, Filme:

* __Gemeinsames Vorkommen__: Joe, Alice und Bob gefällt _Star Wars_ _Das Imperium schlägt zurück_und _jedi zurück_. Mahout fest, dass Benutzer, die eine dieser Filme auch die beiden anderen wie.

* __Gemeinsames Vorkommen__: Bob und Alice auch gern _Bedrohung_ _Angriff der Klone_und _Rache der Sith_. Mahout fest, dass Benutzer auch die vorherigen drei Filme gefallen diese drei wie.

* __Vergleichbare Empfehlung__: Da Joe die ersten drei Filme gefallen, Mahout untersucht Filme, die andere mit ähnlichen gefällt, aber Joe wurde nicht überwacht (gefällt/Note). In diesem Fall empfiehlt Mahout _Bedrohung_, _Angriff der Klone_und _Rache der Sith_.

###<a name="understanding-the-data"></a>Verständnis der Daten

[GroupLens Research] ,[ movielens] Filme in einem Format, das mit Mahout Bewertungsdaten bereit. Diese Daten werden im Standardspeicher des Clusters auf `/HdiSamples/MahoutMovieData`.

Es gibt zwei Dateien, `moviedb.txt` (Informationen zu den Filmen) und `user-ratings.txt`. Benutzer-ratings.txt-Datei wird während der Analyse verwendet, während moviedb.txt verwendet wird, um benutzerfreundlichen Text Informationen bereitzustellen, wenn die Ergebnisse der Analyse anzeigen.

Die Daten in ratings.txt Benutzer hat eine `userID`, `movieID`, `userRating`, und `timestamp`, die sagt uns, wie jeder Benutzer einen Film bewertet. Hier ist ein Beispiel:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>Auftrag ausführen

Verwenden Sie folgende Windows PowerShell-Skript zum Ausführen eines Auftrags, das Mahout Recommendation Engine Films verwendet:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
            
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Temporäre Daten entfernen, die beim Verarbeiten des Auftrags erstellt Mahout Aufträge nicht. Die `--tempDir` Parameter gemäß Auftrag Beispiel temporären Dateien in einem bestimmten Verzeichnis zu isolieren.

Mahout-Auftrag kehrt die Ausgabe an STDOUT. Stattdessen gespeichert im angegebenen Ausgabeverzeichnis als __Teil R 00000__. Das Skript liest diese Datei, __aufgegeben__ im aktuellen Verzeichnis auf Ihrer Arbeitsstation.

Folgendes ist ein Beispiel für den Inhalt dieser Datei:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Die erste Spalte ist die `userID`. Die Werte ' [' und ']' sind `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>Die Ausgabe anzeigen

Zwar die generierte Ausgabe OK für die Verwendung in einer Anwendung ist es nicht schwer lesbar. Die `moviedb.txt` vom Server kann zum Auflösen der `movieId` -Films Namen, aber Sie müssen zuerst herunterladen es und der Filterdatei vom Server mithilfe des folgenden Skripts:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Nachdem die Dateien heruntergeladen haben, mit der das folgende PowerShell-Skript Recommendations Film Namen angezeigt:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Folgendes ist ein Beispiel für die Ausführung des Skripts:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

Die Ausgabe sollte etwa wie folgt aussehen:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Klassifizieren von Daten über die Befehlszeile Hadoop

Mit Mahout Klassifikationsverfahren gehört zu einer [zufälligen Gesamtstruktur][forest]. Dies ist ein mehrere Schritte umfassender Prozess, der das mit Daten zu Entscheidungsstrukturen, die anschließend verwendet werden, um Daten zu klassifizieren. Dies wird vom Mahout bereitgestellte __org.apache.mahout.classifier.df.tools.Describe__ -Klasse verwendet. Es muss derzeit über Hadoop Befehlszeile ausgeführt werden.

###<a name="load-the-data"></a>Laden Sie die Daten

1. Die folgenden Dateien aus [der NSL KDD Daten](http://nsl.cs.unb.ca/NSL-KDD/)herunterladen

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): der Trainingsdatei

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): die Testdaten

2. Jede Datei öffnen und entfernen Sie die Zeilen mit oben '@', und speichern Sie die Dateien. Wenn diese nicht entfernt werden, erhalten Sie Fehlermeldungen, Mahout Daten mit.

2. Hochladen Sie die Dateien zum __Beispiel-Daten__. Dazu können Sie das folgende Skript verwenden. Der Name des Clusters HDInsight ersetzen Sie __CLUSTERNAME__ . Dateiname mit dem Namen ersetzen fo die Datei hochgeladen werden.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>Auftrag ausführen

1. Dabei müssen die Befehlszeile Hadoop. Aktivieren von Remotedesktop für den HDInsight-Cluster und gemäß der Anleitung in [Verbindung mit HDInsight-Cluster mit RDP](hdinsight-administer-use-management-portal.md#rdp)herstellen.

3. Verwenden Sie nach dem anschließen, __Befehlszeile Hadoop__ Symbol Hadoop Befehlszeile öffnen:

    ![Hadoop cli][hadoopcli]

3. Verwenden Sie den folgenden Befehl Dateideskriptor (__KDDTrain + Info__), generieren, der Mahout verwendet.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    Die `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` beschreibt die Attribute der Daten in der Datei. L gibt z. B. eine Beschriftung.

4. Erstellen einer Gesamtstruktur Entscheidungsstrukturen mithilfe des folgenden Befehls:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    Die Ausgabe dieses Vorgangs werden im Verzeichnis __Nsl Gesamtstruktur__ im Speicher für die Cluster HDInsight _ Wasbs://user/&lt;Benutzername > / Nsl Gesamtstruktur/Nsl forest.seq. Die &lt;Username > ist der Benutzername, der für die Remotedesktopsitzung verwendet. Diese Datei ist nicht von Menschen lesbar.

5. Testen der Gesamtstruktur Dataset __KDDTest + .arff__ klassifizieren. Verwenden Sie den folgenden Befehl ein:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    Dieser Befehl gibt die Zusammenfassung Klassifizierung ähnlich der folgenden:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  Dieser Auftrag erstellt außerdem eine Datei unter __wasbs:///example/data/predictions/KDDTest+.arff.out__. Diese Datei ist jedoch nicht für Menschen lesbar.

> [AZURE.NOTE] Mahout Aufträge werden Dateien nicht überschrieben. Wenn Sie diese Projekte erneut ausführen möchten, müssen Sie Dateien löschen, die von früheren Aufträge erstellt wurden.

##<a name="troubleshooting"></a>Problembehandlung

###<a name="install"></a>Mahout installieren

Mahout auf HDInsight 3.1 Cluster installiert, und es kann mithilfe der folgenden Schritte manuell auf HDInsight 3.0 oder HDInsight 2.1 Cluster installiert werden:

1. Die Version des Mahout verwenden abhängig HDInsight Version des Clusters. Clusterversion finden Sie die Eigenschaften für den Cluster in Azure-Portal anzeigen.

  * __Für HDInsight 2.1__können Sie eine Java Archive (JAR), die [Mahout 0,9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar)enthält.

  * __Für HDInsight 3.0__müssen Sie [build Mahout von der Quelle] [ build] und Hadoop Version von HDInsight bereitgestellt. Auf der Seite erstellen aufgeführten erforderlichen Komponenten zu installieren und verwenden Sie den folgenden Befehl zum Erstellen der Mahout JAR-Dateien herunterladen der Quelle:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Bei Mahout 1.0 sollten Sie vorgefertigte Pakete mit HDInsight 3.0 verwenden können.

2. Hochladen Sie die JAR-Datei zum __Beispiel-Gläser__ im Standardspeicher für Cluster. Ersetzen Sie CLUSTERNAME im folgenden Skript durch den Namen des Clusters HDInsight, und Ersetzen Sie DATEINAMEN mit dem Pfad zur Datei __Mahout-Coure-0.9-job.jar__ ...

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Dateien können nicht überschrieben werden.

Führen Sie Mahout Aufträge nicht bereinigen von temporären Dateien, die während der Verarbeitung erstellt wurden. Darüber hinaus werden die Aufträge nicht vorhandenen Ausgabedatei überschrieben.

Zur Vermeidung von Fehlern beim Ausführen von Aufträgen Mahout löschen Sie temporärer und Ausgabe zwischen oder verwenden Sie eindeutige temporäre und Ausgabe-Verzeichnis.

###<a name="cannot-find-the-jar-file"></a>Die JAR-Datei kann nicht gefunden werden.

3.1 HDInsight Cluster enthalten Mahout. Der Pfad und der Dateiname enthalten die Versionsnummer der Mahout im Cluster installiert ist. Das Windows PowerShell-Beispielskript in diesem Lernprogramm verwendet November 2015 gültig ist, aber die Versionsnummer ändert sich in Zukunft Updates zu HDInsight. Ermitteln Sie den aktuellen Pfad zu Mahout JAR-Datei für den Cluster die folgenden Windows PowerShell-Befehl, und ändern Sie das Skript auf den Pfad, der zurückgegeben wird:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Klassen, die nicht mit Windows PowerShell

Mahout-Aufträge, die der folgenden Klassen zurückgeben verschiedener Fehlermeldungen in Windows PowerShell verwendet werden:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Auftrag ausführen, die verwenden Sie diese Klassen, die mit HDInsight Cluster verbinden und Aufträgen über die Befehlszeile Hadoop. Ein Beispiel finden Sie unter [klassifizieren Daten über die Befehlszeile Hadoop](#classify) .

##<a name="next-steps"></a>Nächste Schritte

Da Sie mit Mahout vertraut gemacht haben, erkennen Sie andere Verfahren zum Arbeiten mit Daten auf HDInsight:

* [Struktur mit HDInsight](hdinsight-use-hive.md)
* [Schwein mit HDInsight](hdinsight-use-pig.md)
* [MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
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
 
