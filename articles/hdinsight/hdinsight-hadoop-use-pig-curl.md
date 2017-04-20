<properties
   pageTitle="Verwenden Sie Hadoop Schwein mit Kringel in HDInsight | Microsoft Azure"
   description="Erfahren Sie mit Kringel Pig Latin Aufträge auf einem Cluster Hadoop in Azure HDInsight ausgeführt."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Führen Sie Schwein Aufträge mit Hadoop auf HDInsight mit Kringel

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

In diesem Dokument erfahren Sie, wie Sie Aufrollen Pig Latin Aufträge in Azure HDInsight-Cluster ausgeführt. Pig Latin Programmiersprache können Sie Transformationen beschreiben, die die Eingabedaten für die gewünschte Ausgabe angewendet werden.

Aufrollen wird veranschaulicht, wie Sie interagieren mit HDInsight ausführen, überwachen und Abrufen der Ergebnisse des Schweins Aufträge mit unformatierten HTTP-Anfragen verwendet. Dies kann mithilfe der WebHCat REST API (ehemals Templeton), die von HDInsight Cluster bereitgestellt.

> [AZURE.NOTE] Wenn Sie bereits mit Hadoop Linux-basierte Server, aber neu HDInsight, anzeigen Sie [Linux-basierte HDInsight Tipps](hdinsight-hadoop-linux-information.md)

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (Linux- oder Windows-basiert)

* [Aufrollen](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Aufträgen Sie Schwein mit Kringel

> [AZURE.NOTE] WebHCat locken oder andere REST-Kommunikation mit müssen Sie Anfragen mit den Administrator-Benutzernamen und das Kennwort für den Cluster HDInsight authentifizieren. Sie müssen den Namen des Clusters als Teil von dem URI Uniform Resource Identifier () auch, die Anfragen an den Server verwendet wird.
>
> Befehle in diesem Abschnitt zum Cluster Authentifizierung des Benutzers ersetzen Sie **Benutzernamen** , und Ersetzen Sie **PASSWORD** durch das Kennwort für das Benutzerkonto. Der Name des Clusters ersetzen Sie **CLUSTERNAME** .
>
> Die REST-API wird über [grundlegende Authentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Anfragen immer, mit HTTPS (Secure HTTP) um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.

1. Verwenden Sie den folgenden Befehl um zu überprüfen, ob HDInsight Cluster hergestellt werden kann, von einer Befehlszeile aus:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Sie erhalten eine Antwort ähnlich der folgenden:

        {"status":"ok","version":"v1"}

    In diesem Befehl verwendeten Parameter werden wie folgt:

    * **-u**: Benutzername und Kennwort zur Authentifizierung der Anforderung
    * **-G**: Gibt an, dass dies eine GET-Anforderung

    Anfang der URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**werden für alle Anfragen. Path **Status**gibt an, dass die Anforderung den Status des WebHCat (auch bekannt als Templeton) zurück für den Server.

2. Verwenden Sie den folgenden Code einen Auftrag Pig Latin Cluster senden:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    In diesem Befehl verwendeten Parameter werden wie folgt:

    * **-d**: Da `-G` nicht verwendet wird, die Anforderung standardmäßig die POST-Methode. `-d`Gibt die Datenwerte gesendet werden mit der Anforderung.

        * **User.Name**: der Benutzer, der den Befehl ausführt
        * **Ausführen**: die Pig Latin-Anweisungen ausführen
        * **Statusdir**: das Verzeichnis, das der Status für diesen Auftrag geschrieben werden

    > [AZURE.NOTE] Benachrichtigung, die Leerzeichen in Pig Latin-Anweisungen durch ausgetauscht werden die `+` Zeichen mit Kringel.

    Dieser Befehl sollte eine Auftrags-ID zurück, die verwendet werden kann, überprüfen Sie den Status des Auftrags, zum Beispiel:

        {"id":"job_1415651640909_0026"}

3. Verwenden Sie den folgenden Befehl zum Überprüfen des Status des Auftrags. Der Rückgabewert im vorherigen Schritt ersetzen Sie **JOBID** . Beispielsweise der Rückgabewert war `{"id":"job_1415651640909_0026"}`, dann **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Wenn der Auftrag abgeschlossen ist, wird der Status **erfolgreich**sein.

    > [AZURE.NOTE] Diese Anforderung Curl gibt ein JavaScript Object Notation (JSON)-Dokument mit Informationen über den Auftrag und Jq ist nur den Wert abgerufen.

##<a id="results"></a>Ergebnisse anzeigen

Wenn der Status des Auftrags **erfolgreich**geändert wurde, können Sie die Ergebnisse des Auftrags von Azure BLOB-Speicher abrufen. Die `statusdir` mit der Abfrage übergebenen Parameter enthält den Speicherort der Ausgabedatei. In diesem Fall **Wasbs: / / / Beispiel/Pigcurl**. Diese Adresse speichert die Ausgabe des Einzelvorgangs im **Beispiel-Pigcurl** -Verzeichnis im Standardcontainer für die Speicherung von HDInsight-Cluster verwendet.

Sie können auch diese Dateien mithilfe der [Azure-CLI](../xplat-cli-install.md). Beispielsweise Dateien im **Beispiel-Pigcurl**Befehl:

    azure storage blob list <container-name> example/pigcurl

Um eine Datei herunterzuladen, verwenden Sie Folgendes:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Geben Sie den Speicher-Kontonamen, das Blob enthält die `-a` und `-k` Parameter oder Festlegen der **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_Zugriff\_Schlüssel** Umgebungsvariablen.

##<a id="summary"></a>Zusammenfassung

Wie in diesem Dokument, können Sie eine unformatierte HTTP-Anforderung ausführen, überwachen und Anzeigen der Ergebnisse des Schweins Aufträge im Cluster HDInsight.

Finden Sie weitere Informationen in diesem Artikel verwendeten REST-Schnittstelle [WebHCat-Verweis](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Schwein auf HDInsight:

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

Informationen zum Arbeiten Sie mit Hadoop auf HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)
