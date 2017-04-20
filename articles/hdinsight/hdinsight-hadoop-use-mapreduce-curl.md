<properties
   pageTitle="Mit MapReduce und Aufrollen Hadoop in HDInsight | Microsoft Azure"
   description="Erfahren Sie, MapReduce Aufträge Hadoop auf HDInsight mit Curl auszuführen."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>Führen Sie MapReduce Aufträge mit Hadoop auf HDInsight mit Kringel

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Dokument erfahren Sie, wie Sie Aufrollen MapReduce Aufträge auf Hadoop auf HDInsight Cluster ausgeführt.

Aufrollen dient zum Veranschaulichen Sie mit HDInsight Interaktion mit unformatierten HTTP-Anfragen MapReduce-Jobs ausgeführt. Dies funktioniert mit der WebHCat REST-API (ehemals Templeton) von HDInsight Cluster bereitgestellt.

> [AZURE.NOTE] Wenn Sie bereits mit Hadoop Linux-basierte Server, aber sind neue HDInsight, finden [Sie unter erforderliche Informationen zu Linux-basierten Hadoop auf HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Hadoop auf HDInsight Cluster (Linux oder Windows)

* [Aufrollen](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Aufträgen Sie MapReduce mit Kringel

> [AZURE.NOTE] Wenn Sie locken oder andere REST-Kommunikation mit WebHCat verwenden, müssen Sie die Anfragen HDInsight Cluster Administrator-Benutzernamen und Kennwort authentifizieren. Sie müssen den Clusternamen auch als Teil des URIS verwenden, mit der die Anfragen an den Server senden.
>
> Befehle in diesem Abschnitt ersetzen Sie **Benutzernamen** der Benutzer mit dem Kennwort für das Benutzerkonto auf den Cluster und **Kennwort** authentifizieren. Der Name des Clusters ersetzen Sie **CLUSTERNAME** .
>
> Die REST-API wird durch [grundlegende](http://en.wikipedia.org/wiki/Basic_access_authentication)Authentifizierung gesichert. Stellen Sie Anfragen immer, mithilfe von HTTPS sicherstellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.

1. Verwenden Sie den folgenden Befehl um zu überprüfen, ob HDInsight Cluster hergestellt werden kann, von einer Befehlszeile aus:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Sie erhalten eine Antwort ähnlich der folgenden:

        {"status":"ok","version":"v1"}

    In diesem Befehl verwendeten Parameter werden wie folgt:

    * **-u**: Gibt den Benutzernamen und das Kennwort zur Authentifizierung der Anforderung
    * **-G**: Gibt an, dass dies eine GET-Anforderung

    Anfang des URI **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**gilt für alle Anfragen.

2. Um einen MapReduce-Auftrag senden, verwenden Sie den folgenden Befehl ein:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    Ende des URI (/ Mapreduce Jar) teilt WebHCat mit, dass diese Anforderung einen MapReduce-Auftrag aus einer Klasse in einer JAR-Datei gestartet wird. In diesem Befehl verwendeten Parameter werden wie folgt:

    * **-d**: `-G` nicht verwendet wird, damit die Anforderung standardmäßig die POST-Methode verwendet. `-d`Gibt die Datenwerte gesendet werden mit der Anforderung.

        * **User.Name**: der Benutzer, der den Befehl ausführt
        * **Glas**: Speicherort der JAR-Datei mit Klasse werden ausgeführt
        * **Klasse**: die Klasse enthält die Logik MapReduce
        * **Arg**: Argumente übergeben MapReduce-Auftrags; In diesem Fall der Eingabetextdatei und das Verzeichnis für die Ausgabe

    Dieser Befehl sollte eine Auftrags-ID zurück, die verwendet werden kann, überprüfen Sie den Status des Auftrags:

        {"id":"job_1415651640909_0026"}

3. Verwenden Sie den folgenden Befehl zum Überprüfen des Status des Auftrags. Der Rückgabewert im vorherigen Schritt **JOBID** ersetzen. Beispielsweise der Rückgabewert war `{"id":"job_1415651640909_0026"}`, dann die JOBID `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Wenn das Projekt abgeschlossen ist, wird der Status "erfolgreich sein".

    > [AZURE.NOTE] Diese Anforderung Curl gibt ein JSON-Dokument mit Informationen über den Auftrag; Jq wird nur den Wert abzurufen.

4. Wenn der Status des Auftrags **erfolgreich**geändert wurde, können Sie die Ergebnisse des Auftrags von Azure BLOB-Speicher abrufen. Die `statusdir` mit der Abfrage übergeben Parameter enthält den Speicherort der Ausgabedatei. In diesem Fall **Wasbs: / / / Beispiel/Curl**. Diese Adresse speichert die Ausgabe des Einzelvorgangs im **Beispiel-Curl** Verzeichnis im Standardcontainer für die Speicherung von HDInsight-Cluster verwendet.

Sie können auch diese Dateien mithilfe der [Azure-CLI](../xplat-cli-install.md). Beispielsweise Dateien im **Beispiel-Curl**Befehl:

    azure storage blob list <container-name> example/curl

Um eine Datei herunterzuladen, verwenden Sie Folgendes:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Geben Sie den Speicher-Kontonamen, das Blob enthält die `-a` und `-k` Parameter oder Festlegen der **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_Zugriff\_Schlüssel** Umgebungsvariablen. Finden Sie weitere Informationen [zu HDInsight Daten uploaden](hdinsight-upload-data.md) .

##<a id="summary"></a>Zusammenfassung

In diesem Dokument gezeigt, können unformatierte HTTP-Anforderung ausführen, überwachen und die Ergebnisse der Hive-Aufträge im Cluster HDInsight anzeigen.

Weitere Informationen zu der in diesem Artikel verwendeten REST-Schnittstelle finden Sie unter [WebHCat Verweis](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce Aufträge in HDInsight:

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)
