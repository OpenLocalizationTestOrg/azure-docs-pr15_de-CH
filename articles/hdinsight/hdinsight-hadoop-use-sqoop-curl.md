<properties
   pageTitle="Verwenden Sie Hadoop Sqoop mit Kringel in HDInsight | Microsoft Azure"
   description="Informationen Sie zum Sqoop Aufträge an HDInsight mit Curl Remote zu übermitteln."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Aufträgen Sie Sqoop Hadoop in HDInsight mit Kringel

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie mit Kringel Sqoop Aufträge auf einem Cluster Hadoop in HDInsight ausgeführt.

Aufrollen wird veranschaulicht, wie Sie interagieren mit HDInsight mit unformatierten HTTP-Anfragen ausführen, überwachen und Abrufen der Ergebnisse des Sqoop Einzelvorgänge verwendet. Dies funktioniert mit der WebHCat REST-API (ehemals Templeton) von HDInsight Cluster bereitgestellt.

> [AZURE.NOTE] Wenn Sie bereits mit Hadoop Linux-basierte Server, aber neu HDInsight, finden Sie unter [Informationen über HDInsight für Linux](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Hadoop auf HDInsight Cluster (Linux oder Windows)

* [Aufrollen](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Senden Sie Sqoop Aufträge mit Kringel

> [AZURE.NOTE] WebHCat locken oder andere REST-Kommunikation mit müssen Sie Anfragen durch Benutzernamen und Kennwort HDInsight Cluster-Administrator authentifizieren. Sie müssen den Clusternamen auch als Teil der der URI Uniform Resource Identifier () verwendet, um Anfragen an den Server verwenden.
>
> Befehle in diesem Abschnitt zum Cluster Authentifizierung des Benutzers ersetzen Sie **Benutzernamen** , und Ersetzen Sie **PASSWORD** durch das Kennwort für das Benutzerkonto. Der Name des Clusters ersetzen Sie **CLUSTERNAME** .
>
> Die REST-API wird über [die Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Anfragen immer, mit HTTPS (Secure HTTP) um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.

1. Verwenden Sie den folgenden Befehl um zu überprüfen, ob HDInsight Cluster hergestellt werden kann, von einer Befehlszeile aus:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Sie erhalten eine Antwort ähnlich der folgenden:

        {"status":"ok","version":"v1"}

    In diesem Befehl verwendeten Parameter werden wie folgt:

    * **-u** - Benutzernamen und ein Kennwort zur Authentifizierung der Anforderung verwendet.
    * **-G** - gibt an, dass dies eine GET-Anforderung ist.

    Anfang der URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**werden für alle Anfragen. Pfad **laufwerkszuweisungen/Status**zeigt an, dass die Anforderung Status WebHCat (auch bekannt als Templeton) zurück für den Server. 

2. Verwenden Sie folgende Sqoop Auftrag senden:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    In diesem Befehl verwendeten Parameter werden wie folgt:

    * **-d** - seit `-G` nicht verwendet wird, die Anforderung standardmäßig die POST-Methode. `-d`Gibt die Datenwerte gesendet werden mit der Anforderung.

        * **user.name** - Benutzers, der den Befehl ausführt.

        * **Befehl** - Befehl zum Ausführen der Sqoop.

        * **Statusdir** - Verzeichnis, die der Status für diesen Auftrag geschrieben werden.

    Dieser Befehl sollte eine Auftrags-ID zurück, die verwendet werden kann, überprüfen Sie den Status des Auftrags.

        {"id":"job_1415651640909_0026"}

3. Verwenden Sie den folgenden Befehl zum Überprüfen des Status des Auftrags. Der Rückgabewert im vorherigen Schritt ersetzen Sie **JOBID** . Beispielsweise der Rückgabewert war `{"id":"job_1415651640909_0026"}`, dann **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Wenn der Auftrag abgeschlossen ist, wird der Status **erfolgreich**sein.

    > [AZURE.NOTE] Diese Anforderung Curl gibt ein JavaScript Object Notation (JSON)-Dokument mit Informationen über den Auftrag; Jq wird nur den Wert abzurufen.

4. Wenn der Zustand des Auftrags **erfolgreich**geändert wurde, können Sie die Ergebnisse des Auftrags von Azure BLOB-Speicher abrufen. Die `statusdir` mit der Abfrage übergebenen Parameter enthält den Speicherort der Ausgabedatei. In diesem Fall **Wasbs: / / / Beispiel/Curl**. Diese Adresse speichert die Ausgabe des Einzelvorgangs im Verzeichnis **Beispiel-Curl** Speicher Standardcontainer HDInsight Cluster verwendet.

    Sie können auch diese Dateien mithilfe der [Azure-CLI](../xplat-cli-install.md). Beispielsweise Dateien im **Beispiel-Aufrollen**Befehl:

        azure storage blob list <container-name> example/curl

    Um eine Datei herunterzuladen, verwenden Sie Folgendes:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Geben Sie Speicher-Kontonamen, das Blob enthält entweder die `-a` und `-k` Parameter oder Festlegen der **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_Zugriff\_Schlüssel** Umgebungsvariablen. Siehe < ein Href = "Hdinsight-Upload-data.md" Target = "_blank" für Weitere Informationen.

##<a name="limitations"></a>Grenzen

* Massenexport - mit Linux-basierte HDInsight, Sqoop-Connector verwendet, um Daten in Azure SQL-Datenbank oder Microsoft SQL Server exportieren unterstützt derzeit keine Bulk INSERT.

* Batchverarbeitung von-mit Linux-basierte HDInsight bei Verwendung der `-batch` fügt beim wechseln, wird Sqoop mehrere fügt anstelle von Batchvorgängen einfügen.

##<a name="summary"></a>Zusammenfassung

Wie in diesem Dokument, können Sie eine unformatierte HTTP-Anforderung ausführen, überwachen und die Ergebnisse der Sqoop Aufträge im Cluster HDInsight anzeigen.

Weitere Informationen zu der in diesem Artikel verwendeten REST-Schnittstelle finden Sie unter <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API-Handbuch</a>.

##<a name="next-steps"></a>Nächste Schritte

Allgemeine Informationen zur Struktur mit HDInsight:

* [Verwenden Sie Sqoop mit Hadoop auf HDInsight](hdinsight-use-sqoop.md)

Weitere Informationen können Sie mit Hadoop auf HDInsight arbeiten:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


