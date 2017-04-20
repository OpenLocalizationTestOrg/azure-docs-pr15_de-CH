<properties
    pageTitle="Verwenden Sie Python mit Struktur und Schweine in HDInsight | Microsoft Azure"
    description="Informationen Sie zum Python benutzerdefinierte Funktion (UDF) von Hive und Pig HDInsight Technologie Hadoop auf Azure verwenden."
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
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/07/2016" 
    ms.author="larryfr"/>

#<a name="use-python-with-hive-and-pig-in-hdinsight"></a>Python mit Struktur und Schweine in HDInsight verwenden

Struktur und Schwein eignen sich hervorragend zum Arbeiten mit Daten in HDInsight, aber manchmal brauchen Sie allgemeinen Zweck Sprache. Struktur und Schweine können Sie erstellen benutzerdefinierte Funktion (UDF) mit verschiedenen Programmiersprachen. In diesem Artikel erfahren Sie, wie eine UDF Python Struktur und Schweine.

##<a name="requirements"></a>Vorschriften

* Ein HDInsight-Cluster (Windows oder Linux-basierten)

* Ein Text-editor

    > [AZURE.IMPORTANT] Verwenden Sie einen HDInsight Linux-basierte Server jedoch die Python-Dateien auf einem Windows-Client erstellen, müssen Sie einen Editor verwenden verwendet, LF als ein Zeilenendetyp. Wenn Sie nicht sicher sind, ob der Editor LF oder CRLF verwendet, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) Schritte zum Entfernen von HDInsight-Cluster über Dienstprogramme CR-Zeichen.
    
##<a name="python"></a>Python auf HDInsight

Python2.7 wird standardmäßig auf HDInsight 3.0 und höher Cluster installiert. Struktur kann mit dieser Version von Python für die Verarbeitung verwendet werden (Daten zwischen Struktur und Python mit STDOUT-STDIN übergeben werden).

HDInsight umfasst auch Jython, Python-Implementierung in Java geschrieben ist. Schwein versteht mit Jython ohne Streaming, so ist es empfehlenswert beim Schwein mit. Allerdings können Sie auch normale Python (C Python) sowie Schwein.

##<a name="hivepython"></a>Struktur und Python

Python dienen als UDF aus Struktur durch **TRANSFORMATION** HiveQL Anweisung. Beispielsweise ruft der folgende HiveQL Python-Skript in der Datei **streaming.py** gespeichert.

**Linux-basierte HDInsight**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'python streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

**Windows-basierte HDInsight**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'D:\Python27\python.exe streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

> [AZURE.NOTE] Auf Windows-basierte HDInsight-Cluster muss die **USING** -Klausel den vollständigen Pfad zum python.exe angeben. Dies ist immer `D:\Python27\python.exe`.

Hier macht dieses Beispiel:

1. Die **Datei** -Anweisung am Anfang der Datei hinzugefügt verteilten Cache Datei **streaming.py** , sodass alle Knoten im Cluster erreichen.

2. Die TRANSFORMATION **wählen... MIT** Anweisung wählt Daten aus **Hivesampletable**und Clientid, Devicemake und Devicemodel an das **streaming.py** -Skript.

3. Die **AS** -Klausel beschrieben, die von **streaming.py** zurückgegebenen Felder

<a name="streamingpy"></a>Hier ist die Datei **streaming.py** im HiveQL-Beispiel.

    #!/usr/bin/env python

    import sys
    import string
    import hashlib

    while True:
      line = sys.stdin.readline()
      if not line:
        break

      line = string.strip(line, "\n ")
      clientid, devicemake, devicemodel = string.split(line, "\t")
      phone_label = devicemake + ' ' + devicemodel
      print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])

Da wir streaming, hat dieses Skript Folgendes:

1. Lesen Sie Daten von STDIN. Dies geschieht mit `sys.stdin.readline()` in diesem Beispiel.

2. Ein Zeichen für den Zeilenvorschub entfernt mit `string.strip(line, "\n ")`, da wir die Textdaten und kein Ende Zeile möchte.

2. Bei Verarbeitung Stream enthält eine einzelne Zeile alle Werte mit einem Tabulatorzeichen zwischen jedem Wert. So `string.split(line, "\t")` kann zum Teilen der Eingabe jeder Registerkarte nur die Felder zurückgibt.

3. Nach Abschluss der Verarbeitung muss die Ausgabe an STDOUT als einzelne Zeile mit zwischen jedem Feld geschrieben. Dies geschieht mithilfe von `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.

4. Alles innerhalb einer `while` Schleife, die Richtung, bis keine wiederholt wird `line` lesen, bei `break` beendet die Schleife und das Skript beendet.

Darüber hinaus verkettet das Skript nur die Eingabewerte für `devicemake` und `devicemodel`, und berechnet einen Hashwert verketteten Wert. Ziemlich einfach, aber es beschreibt die Grundlagen der Funktionsweise Python Skripts aus Struktur aufgerufen: Schleife, Read input bis keine weiteren Teilen jede Zeile der Eingaben auf den Registerkarten Prozess ist, Schreiben einer einzelnen Ausgabezeile Tabulator getrennt.

Anzeigen Sie zum Ausführen dieses Beispiels auf HDInsight Cluster [Ausführen der Beispiele](#running)

##<a name="pigpython"></a>Schweine und Python

Python-Skript kann durch die Anweisung **generieren** als UDF aus verwendet werden. Es gibt zwei Arten dazu; Verwenden von Jython (Python implementiert die Java Virtual Machine) und C Python (reguläre Python). 

Der Hauptunterschied zwischen diesen werden Jython auf JVM ausgeführt und kann direkt aufgerufen werden aus (auch auf die JVM ausgeführt.) C Python ist ein externer Prozess (in c geschrieben) Damit Daten aus auf die JVM gesendet an das Skript in einem Python-Prozess ausgeführt und die Ausgabe, in Schwein gesendet.

Um festzustellen, ob Schwein Jython oder C Python verwendet, um das Skript auszuführen, verwenden Sie __Registrieren__ verweisen auf Python-Skript von Pig Latin. Dies weist Schwein, welche Interpreter verwendet und welche Alias für das Skript erstellen. Die folgenden registrieren Skripts Schwein als __Myfuncs__:

* __Jython verwenden__:`register '/path/to/pig_python.py' using jython as myfuncs;`
* __C Python verwenden__:`register '/path/to/pig_python.py' using streaming_python as myfuncs;`

> [AZURE.IMPORTANT] Wenn Sie Jython verwenden, kann der Pfad zur Datei Pig_jython einen lokalen Pfad oder einem WASB: / / Pfad. Jedoch müssen bei C Python Sie eine Datei im lokalen Dateisystem des Knotens verweisen, mit dem Sie Schwein Auftrag senden.

Einmal wird nach Eintragung Pig Latin beispielsweise sowohl für:

    LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
    LOG = FILTER LOGS by LINE is not null;
    DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
    DUMP DETAILS;

Hier macht dieses Beispiel:

1. Die erste Zeile lädt die Beispieldatendatei **sample.log** in **PROTOKOLLEN**. Da diese Protokolldatei ein konsistentes Schema besitzt, werden außerdem jeden Datensatz (**Zeile** in diesem Fall) als **Chararray**definiert. Chararray ist im Wesentlichen eine Zeichenfolge.

2. Die nächste Zeile filtert alle null-Werte speichern das Ergebnis der Operation **an**.

3. Anschließend durchläuft die Datensätze im **Protokoll** und **generieren** in Python-Jython Skript als **Myfuncs**enthaltenen **Create_structure** -Methode aufgerufen wird.  **Zeile** wird zum aktuellen Datensatz an die Funktion übergeben.

4. Schließlich werden die Ausgaben **Sichern** Befehls STDOUT ausgegeben. Es wird nur die Ergebnisse sofort anzeigen, nach Abschluss des Vorgangs. in einem wirklichen Skript würden Sie normalerweise **Speicher** die Daten in eine neue Datei.

Die tatsächliche Python-Skriptdatei entspricht auch zwischen C Python und Jython, der einzige Unterschied, dass Sie importieren müssen __Schwein\_Util__ Verwendung C Python. Hier wird die __Schwein\_python.py__ Skript:

<a name="streamingpy"></a>

    # Uncomment the following if using C Python
    #from pig_util import outputSchema

    @outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
    def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail

> [AZURE.NOTE] 'Pig_util' nicht installieren müssen. Es ist das Skript automatisch zur Verfügung.

Beachten Sie, dass wir bisher nur, die **Zeile** als ein Chararray eingeben definiert, da war kein konsistentes Schema für die Eingabe? Das Python-Skript wird zum Transformieren der Daten in ein konsistentes Schema für die Ausgabe. Sie funktioniert folgendermaßen:

1. Die **@outputSchema** Anweisung definiert das Format der Daten, die wieder Schweine. In diesem Fall ist eine **Sammlung von Daten**einen Schwein handelt. Die Sammlung enthält die folgenden Felder, die alle Chararray (Zeichenfolgen) sind:

    * Datum - das Datum der Protokolleintrag erstellt wurde
    * -die Zeit, die der Protokolleintrag erstellt wurde
    * ClassName - wurde für der Klassennamen der Eintrag erstellt.
    * Level - die Ebene
    * Detail - ausführliche Details für den Protokolleintrag

2. Als Nächstes definiert **Def create_structure(input)** Funktion, der Schwein Posten übergeben wird.

3. Beispieldaten **sample.log**entspricht größtenteils Datum, Uhrzeit, Classname, Ebene und Detail Schema zurückgegeben werden soll. Enthält aber auch einige Zeilen beginnen mit der Zeichenfolge "*java.lang.Exception*', die das Schema geändert werden müssen. Die **if** -Anweisung überprüft, ob die und Massage Eingabedaten um '*java.lang.Exception*' Zeichenfolge, bringen die Daten in Zeile mit unseren erwartete Ausgabeschema verschieben.

4. Anschließend wird der Befehl **aufteilen** der Daten in den ersten vier Leerzeichen verwendet. Dies führt fünf Werte, die **Datum**, **Uhrzeit**, **Classname**, **Ebene**und **Details**zugewiesen werden.

5. Schließlich werden die Werte Schwein zurückgegeben.

Wenn Daten Schwein zurückgegeben werden, sie haben ein konsistentes Schema im Sinne der **@outputSchema** Anweisung.

##<a name="running"></a>Die Beispiele ausführen

Bei Verwendung von Linux-basierten HDInsight Cluster gehen Sie **SSH** unten. Wenn Sie eine Windows-basierte HDInsight-Cluster und einem Windows-Client verwenden, gehen Sie **PowerShell** .

###<a name="ssh"></a>SSH

Weitere Informationen über SSH finden Sie unter <a href="../hdinsight-hadoop-linux-use-ssh-unix/" target="_blank">Verwenden SSH Linux-basierten Hadoop auf Linux, Unix oder OS X HDInsight</a> oder <a href="../hdinsight-hadoop-linux-use-ssh-windows/" target="_blank">Mit SSH mit Linux-basierten Hadoop auf HDInsight von Windows</a>.

1. Erstellen Sie lokale Kopien der Dateien auf dem Entwicklungscomputer mit Python Beispiele für [streaming.py](#streamingpy) und [pig_python.py](#jythonpy).

2. Mit `scp` HDInsight Cluster die Dateien kopieren. Beispielsweise würde die folgenden Dateien zu einem Cluster mit dem Namen **MeinCluster**kopieren.

        scp streaming.py pig_python.py myuser@mycluster-ssh.azurehdinsight.net:

3. SSH Verbindung zum Cluster verwenden. Beispielsweise würde die folgenden zu einem Cluster mit dem Namen **MeinCluster** als Benutzer **Benutzername**verbinden.

        ssh myuser@mycluster-ssh.azurehdinsight.net

4. Fügen Sie über SSH-Sitzung zuvor WASB Speicher für den Cluster hochgeladen Python-Dateien hinzu.

        hdfs dfs -put streaming.py /streaming.py
        hdfs dfs -put pig_python.py /pig_python.py

Gehen Sie nach dem Upload Struktur und Schwein Auftrag ausführen.

####<a name="hive"></a>Struktur

1. Verwenden der `hive` Befehl Struktur Shell starten. Sollte ein `hive>` aufgefordert, wenn die Shell geladen wurde.

2. Geben Sie Folgendes an der `hive>` aufgefordert.

        add file wasbs:///streaming.py;
        SELECT TRANSFORM (clientid, devicemake, devicemodel)
          USING 'python streaming.py' AS
          (clientid string, phoneLabel string, phoneHash string)
        FROM hivesampletable
        ORDER BY clientid LIMIT 50;

3. Geben Sie die letzte Zeile, sollte das Projekt beginnen. Schließlich wird Ausgabe ähnelt der folgenden zurückgegeben.

        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####<a name="pig"></a>Schweine

1. Verwenden der `pig` Befehl zum Starten der Shell. Sollte ein `grunt>` aufgefordert, wenn die Shell geladen wurde.

2. Geben Sie Folgendes an der `grunt>` Fragen mit dem Jython-Interpreter Python-Skript ausführen.

        Register wasbs:///pig_python.py using jython as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

3. Geben Sie die folgende Zeile, sollte das Projekt beginnen. Schließlich wird Ausgabe ähnelt der folgenden zurückgegeben.

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Verwenden Sie `quit` Beenden der Shell schwierige und verwenden Sie die folgende pig_python.py Datei im lokalen Dateisystem:

    Nano pig_python.py

5. Einmal im Editor Auskommentieren der nächsten Zeile durch Entfernen der `#` aus dem Anfang der Zeile:

        #from pig_util import outputSchema

    Nach der Änderung verwenden Sie STRG + X, um den Editor zu verlassen. Wählen Sie Y, und geben Sie zum Speichern der Änderung.

6. Verwenden der `pig` um die Shell erneut starten. Sobald Sie sind die `grunt>` dazu aufgefordert werden, verwenden Sie die folgenden mit dem C-Python-Interpreter Python-Skript ausführen.

        Register 'pig_python.py' using streaming_python as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

    Nach Abschluss dieses Auftrags erhalten Sie das gleiche Ergebnis als wenn Sie das Skript Jython ausgeführt haben.

###<a name="powershell"></a>PowerShell

Diese Schritte verwenden Azure PowerShell. Wenn dies nicht bereits installiert und auf Ihrem Entwicklungscomputer konfiguriert, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) vor folgt.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Erstellen Sie lokale Kopien der Dateien auf dem Entwicklungscomputer mit Python Beispiele für [streaming.py](#streamingpy) und [pig_python.py](#jythonpy).

2. Verwenden Sie das folgende PowerShell-Skript **streaming.py** hochladen und **Schwein\_python.py** Dateien auf den Server. Namen von Azure HDInsight Cluster und den Pfad der **streaming.py** und **Schwein\_python.py** Dateien in den ersten drei Zeilen des Skripts.

        $clusterName = YourHDIClusterName
        $pathToStreamingFile = "C:\path\to\streaming.py"
        $pathToJythonFile = "C:\path\to\pig_python.py"

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
            -File $pathToStreamingFile `
            -Blob "streaming.py" `
            -Container $container `
            -Context $context
        
        Set-AzureStorageBlobContent `
            -File $pathToJythonFile `
            -Blob "pig_python.py" `
            -Container $container `
            -Context $context

    Dieses Skript ruft Informationen für die Cluster HDInsight und extrahiert das Konto und den Schlüssel für das standardspeicherkonto und lädt die Dateien in das Stammverzeichnis des Containers.

    > [AZURE.NOTE] Andere Methoden Hochladen von Skripts finden im Dokument [Daten Hadoop Aufträge in HDInsight](hdinsight-upload-data.md) .

Verwenden Sie nach dem Upload die folgenden PowerShell Skripts Einzelvorgänge gestartet. Bei Beendigung des Auftrags sollte die Ausgabe in die PowerShell-Konsole geschrieben.

####<a name="hive"></a>Struktur

Das folgende Skript wird __streaming.py__ ausgeführt werden. Vor dem Ausführen, werden sie der HTTPs-Admin-Kontoinformationen für den HDInsight-Cluster aufgefordert.

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName
    $creds=Get-Credential
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
    
    # If using a Windows-based HDInsight cluster, change the USING statement to:
    # "USING 'D:\Python27\python.exe streaming.py' AS " +
    $HiveQuery = "add file wasbs:///streaming.py;" +
                 "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                   "USING 'python streaming.py' AS " +
                   "(clientid string, phoneLabel string, phoneHash string) " +
                 "FROM hivesampletable " +
                 "ORDER BY clientid LIMIT 50;"

    $jobDefinition = New-AzureRmHDInsightHiveJobDefinition `
        -Query $HiveQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -JobId $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
    #   -Clustername $clusterName `
    #   -JobId $job.JobId `
    #   -DefaultContainer $container `
    #   -DefaultStorageAccountName $storageAccountName `
    #   -DefaultStorageAccountKey $storageAccountKey `
    #   -HttpCredential $creds `
    #   -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

Die Ausgabe für das Projekt **Struktur** sollte ähnlich der folgenden angezeigt:

    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####<a name="pig-jython"></a>Schwein (Jython)

Folgendes wird das __pig_python.py__ -Skript mit dem Jython-Interpreter verwenden. Vor dem Ausführen, werden sie HTTPs-Admin-Informationen für den HDInsight-Cluster aufgefordert.

> [AZURE.NOTE] Bei Remote einen Druckauftrag PowerShell kann nicht C Python als der Interpreter verwendet werden.

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName

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
            
    $PigQuery = "Register wasbs:///jython.py using jython as myfuncs;" +
                "LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);" +
                "LOG = FILTER LOGS by LINE is not null;" +
                "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
                "DUMP DETAILS;"

    $jobDefinition = New-AzureRmHDInsightPigJobDefinition -Query $PigQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
        
    Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -Job $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

Die Ausgabe für den Auftrag **Schwein** sollte ähnlich der folgenden angezeigt:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

##<a name="troubleshooting"></a>Problembehandlung

###<a name="errors-when-running-jobs"></a>Fehler beim Ausführen von Aufträgen

Ausführung den Struktur Auftrag können Fehler wie die folgenden auftreten:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.
    
Dieses Problem kann durch die Zeilenenden in der Datei streaming.py verursacht. Viele Windows-Editoren standardmäßig CRLF Zeilenendetyp aber Linux Applications normalerweise erwarten LF.

Wenn Sie einen Editor, der Zeilenenden LF kann nicht erstellt oder nicht sicher sind verwenden, welche Zeilenenden verwendet werden, verwenden Sie PowerShell Aussagen CR-Zeichen vor dem Hochladen der Datei auf HDInsight entfernen:

    $original_file ='c:\path\to\streaming.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)

###<a name="powershell-scripts"></a>PowerShell-Skripts

Die PowerShell-Beispielskripts, die zum Ausführen der Beispiele enthalten eine Kommentarzeile, die Fehlerausgabe für den Einzelvorgang angezeigt. Wenn nicht die erwartete Ausgabe für das Projekt angezeigt wird, kommentieren Sie die folgende Zeile und die Fehlerinformationen auf ein Problem hinweist.

    # Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Die Fehlerinformationen (STDERR) und das Ergebnis des Auftrags (STDOUT) werden ebenfalls zum Standardcontainer Blob für den Cluster an folgenden Speicherorten protokolliert.

Für diesen Auftrag...|Sehen Sie sich diese Dateien in den Blob-container
---|---
Struktur|/ HivePython/stderr<p>/ HivePython/stdout
Schweine|/ PigPython/stderr<p>/ PigPython/stdout

##<a name="next"></a>Nächste Schritte

Python-Module geladen, die standardmäßig bereitgestellt werden, finden Sie unter [ein Modul Azure HDInsight Bereitstellung](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx) ein Beispiel dazu.

Andere Methoden mit Schwein, Struktur, und über die Verwendung von MapReduce finden Sie im folgenden.

* [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)

* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce mit HDInsight](hdinsight-use-mapreduce.md)
