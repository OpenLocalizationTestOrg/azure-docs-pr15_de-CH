<properties
   pageTitle="Verwenden Hadoop Struktur mit Kringel in HDInsight | Microsoft Azure"
   description="Informationen Sie zum Remote mit Curl HDInsight Schwein Aufträge senden."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>Abfragen Sie Struktur Hadoop in HDInsight mit Kringel

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Dokument erfahren Sie, wie Sie Aufrollen Struktur Abfragen auf Hadoop in Azure HDInsight Cluster.

Aufrollen wird veranschaulicht, wie Sie interagieren mit HDInsight mit unformatierten HTTP-Anfragen ausführen, überwachen und Abrufen der Ergebnisse der Hive-Abfragen verwendet. Dies funktioniert mit der WebHCat REST-API (ehemals Templeton) von HDInsight Cluster bereitgestellt.

> [AZURE.NOTE] Wenn Sie bereits mit Hadoop Linux-basierte Server, aber neu HDInsight, finden Sie unter [brauchen Hadoop auf Linux-basierten HDInsight kennen](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

* Hadoop auf HDInsight Cluster (Linux oder Windows)

* [Aufrollen](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Abfragen Sie Struktur mit Kringel

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

    Anfang der URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**werden für alle Anfragen. Pfad **laufwerkszuweisungen/Status**zeigt an, dass die Anforderung Status WebHCat (auch bekannt als Templeton) zurück für den Server. Mit dem folgenden Befehl können Sie auch die Version Struktur anfordern:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    Dies sollte eine Antwort ähnlich der folgenden zurück:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Erstellen Sie eine neue Tabelle namens **log4jLogs**anhand der folgenden:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    In diesem Befehl verwendeten Parameter werden wie folgt:

    * **-d** - seit `-G` nicht verwendet wird, die Anforderung standardmäßig die POST-Methode. `-d`Gibt die Datenwerte gesendet werden mit der Anforderung.

        * **user.name** - Benutzers, der den Befehl ausführt.

        * **Ausführen** - der HiveQL-Anweisungen ausgeführt.

        * **Statusdir** - Verzeichnis, die der Status für diesen Auftrag geschrieben werden.

    Diese Aussagen werden die folgenden Aktionen durchführen:

    * **DROP TABLE** - löscht die Tabelle und die Datendatei, wenn die Tabelle bereits vorhanden ist.

    * **Externe Tabelle erstellen** - erstellt eine neue 'externe' Tabelle Struktur. Externe Tabellen enthalten nur die Tabellendefinition in Struktur. Die Daten verbleiben am ursprünglichen Speicherort.

        > [AZURE.NOTE] Externe Tabellen sollte verwendet werden, die zugrunde liegenden Daten von einer externen Quelle wie ein automatisches Uploadprozess oder anderen MapReduce Vorgang aktualisiert aber immer Hive-Abfragen mit den neuesten Daten erwartet.
        >
        > Löschen einer externen Tabelle ist **nicht** löschen die Daten nur die Tabellendefinition.

    * **FORMAT Zeile** - weist Struktur wie die Daten formatiert werden. In diesem Fall werden die Felder in jedem Protokoll durch ein Leerzeichen getrennt.

    * **Gespeichert als Textdatei Speicherort** - weist Struktur, in der Daten, gespeichert (Beispiel-Daten-Verzeichnis) und davon wird als Text gespeichert.

    * **Auswählen** - wählt alle Zeilen Spalte **t4** Wert **[Fehler]**enthält. Drei Zeilen, die diesen Wert enthalten sollte dadurch der Wert **3** zurückgegeben.

    > [AZURE.NOTE] Benachrichtigung, die Leerzeichen zwischen HiveQL durch ausgetauscht werden die `+` Zeichen mit Kringel. Werte in Anführungszeichen, die, wie das Trennzeichen Leerzeichen, nicht ausgetauscht werden durch `+`.

    * **INPUT__FILE__NAME wie "% 25.log"** - diese Grenzen die Suchfunktion nur enden. Log. Wenn dies nicht vorhanden ist, versucht Struktur durchsucht alle Dateien in diesem Verzeichnis und seine Unterverzeichnisse, einschließlich Dateien, die nicht für diese Tabelle definierte Spaltenschema entsprechen.

    > [AZURE.NOTE] Beachten Sie, dass 25 % ist der URL codiert Form %, also der tatsächliche Zustand `like '%.log'`. Die muss URL-codiert, als Sonderzeichen in URLs behandelt wird.

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

6. Mit der neue 'interne' Tabelle **Fehlerprotokolle von.**erstellt Folgendes:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Diese Aussagen werden die folgenden Aktionen durchführen:

    * **Erstellen der Tabelle IF NOT EXISTS** - erstellt eine Tabelle, wenn es nicht bereits vorhanden ist. Da **externe** Schlüsselwort nicht verwendet wird, ist eine interne Tabelle Struktur Data Warehouse gespeichert und wird vollständig von Struktur verwaltet.

        > [AZURE.NOTE] Im Gegensatz zu externen Tabellen löscht Ablegen einer internen Tabelle zugrunde liegenden Daten.

    * **Gespeichert als ORK** - speichert die Daten optimiert Zeile Einspaltig (ORK)-Format. Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten der Struktur.
    * ÜBERSCHREIBEN von **Einfügen... Wählen Sie** - wählt Zeilen aus der **log4jLogs** -Tabelle, die enthalten **[Fehler]**und fügt die Daten in der Tabelle **ErrorLogs** .
    * **Auswählen** - wählt alle Zeilen aus der neuen **ErrorLogs** -Tabelle.

7. Verwenden Sie die Auftrags-ID überprüfen Sie den Status des Auftrags zurückgegeben. Nach erfolgreich abgeschlossen ist, verwenden Sie Azure CLI für Mac, Linux und Windows wie zuvor herunterladen und Anzeigen der Ergebnisse. Die Ausgabe sollte drei Zeilen die **[Fehler]**enthalten.


##<a id="summary"></a>Zusammenfassung

Wie in diesem Dokument, können Sie eine unformatierte HTTP-Anforderung ausführen, überwachen und die Ergebnisse der Hive-Aufträge im Cluster HDInsight anzeigen.

Informationen in diesem Artikel verwendeten REST-Schnittstelle finden Sie unter <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">WebHCat-Verweis</a>.

##<a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zur Struktur mit HDInsight:

* [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

Weitere Informationen können Sie mit Hadoop auf HDInsight arbeiten:

* [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

Bei Verwendung von Tez mit Struktur finden Sie in den folgenden Dokumenten Debuginformationen:

* [Mithilfe der Tez-Benutzeroberfläche auf Windows-basierten HDInsight](hdinsight-debug-tez-ui.md)

* [Ambari Tez Ansicht auf Linux-basierten HDInsight](hdinsight-debug-ambari-tez-view.md)

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


