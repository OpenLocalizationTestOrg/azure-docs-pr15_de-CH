<properties
   pageTitle="Tipps zur Verwendung von Hadoop auf Linux-basierten HDInsight | Microsoft Azure"
   description="Implementierung Tipps für die Verwendung von Linux-basierten HDInsight (Hadoop)-Cluster auf einer vertrauten Linux Umgebung in der Azure-Cloud ausgeführt."
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Informationen über HDInsight unter Linux

Linux-basierte Azure HDInsight Cluster bieten Hadoop auf einer vertrauten Linux-Umgebung in der Azure-Cloud ausgeführt. Für die meisten Aktionen sollten genauso wie andere Hadoop auf Linux-Installation funktioniert. Dieses Dokument ruft Unterschiede, denen Sie beachten sollten.

##<a name="prerequisites"></a>Erforderliche Komponenten

Viele der Schritte in diesem Dokument verwenden Sie die folgenden Dienstprogramme, die auf Ihrem System installiert sein.

* [Rollen](https://curl.haxx.se/) - Kommunikation mit webbasierten Diensten
* [Jq](https://stedolan.github.io/jq/) - JSON-Dokumente analysiert
* [Azure-CLI](../xplat-cli-install.md) - Remoteverwaltung Azure Services verwendet

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Domänennamen

Der vollqualifizierten Domänennamen (FQDN) des Clusters aus dem Internet herstellen können ist ** &lt;Clustername >. azurehdinsight.net** oder (SSH) ** &lt;Clustername-ssh >. azurehdinsight.net**.

Intern hat jeder Knoten im Cluster ein, die während der Konfiguration zugewiesen. Zu den Clusternamen können auf der Seite __Hosts__ auf die Webbenutzeroberfläche Ambari oder anhand der folgenden Liste von Hosts Ambari REST API zurückgeben:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Ersetzen Sie __PASSWORD__ durch das Kennwort des Administratorkontos und __CLUSTERNAME__ mit dem Namen des Clusters. Ein JSON Dokument mit einer Liste der Hosts im Cluster erhalten, dann zieht Jq der `host_name` Elementwert für jeden Host im Cluster.

Möchten Sie den Namen des Knotens für einen bestimmten Dienst finden, können Sie Ambari für diese Komponente Abfragen. Knoten Name bietet Hosts suchen, z. B. die folgenden.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Die zurückgegebene JSON Beschreibung des Dienstes, und dann Jq zieht nur die `host_name` Wert für die Hosts.

## <a name="remote-access-to-services"></a>Remote-Zugriff auf Dienste

* **Ambari (Web)** - https://&lt;Clustername >. azurehdinsight.net

    Mithilfe von Cluster Administratorbenutzer und Kennwort authentifizieren, und melden Sie sich Ambari. Dies wird auch Cluster Administratorbenutzer und Kennwort.

    Authentifizierung im Klartext - immer HTTPS verwenden, um sicherzustellen, dass die Verbindung sicher ist.

    > [AZURE.IMPORTANT] Ambari für den Cluster direkt über das Internet zugänglich ist, setzt einige Funktionen zum Zugriff auf Knoten durch den internen Domänennamen vom Cluster verwendet. Da diese internen Domänennamen und nicht öffentlich "Server nicht gefunden" beim Versuch Fehlermeldungen wird, einige Features zugreifen.
    >
    > Um die volle Funktionalität der Ambari Web-Benutzeroberfläche verwenden, verwenden Sie SSH-Tunnel Proxy Web-Datenverkehr an die Cluster-Head-Knoten. Siehe [Verwenden SSH Tunneling auf Ambari Webbenutzeroberfläche ResourceManager, JobHistory, NameNode, Oozie, und andere Webbenutzeroberfläche](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** - https://&lt;Clustername >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Mithilfe von Cluster Administratorbenutzer und Kennwort authentifizieren.
    >
    > Authentifizierung im Klartext - immer HTTPS verwenden, um sicherzustellen, dass die Verbindung sicher ist.

* **WebHCat (Templeton)** - https://&lt;Clustername >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Mithilfe von Cluster Administratorbenutzer und Kennwort authentifizieren.
    >
    > Authentifizierung im Klartext - immer HTTPS verwenden, um sicherzustellen, dass die Verbindung sicher ist.

* **SSH** - &lt;Clustername >--SSH.azurehdinsight.NET auf Port 22 oder 23. Anschluss 22 zum primären Hauptknoten herstellen, während die sekundäre Verbindung 23 verwendet wird. Weitere Informationen zu den Hauptknoten finden Sie unter [Verfügbarkeit und Zuverlässigkeit von Hadoop Cluster in HDInsight](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] Sie können nur die Kopf Clusterknoten über SSH von einem Clientcomputer zugreifen. Sobald verbunden, dann möglich die Arbeitskraft Knoten mithilfe von SSH aus einem Hauptknoten.

## <a name="file-locations"></a>Speicherorte

Hadoop-Dateien finden Sie auf den Clusterknoten unter `/usr/hdp`. Dieses Verzeichnis enthält die folgenden Unterverzeichnisse:

* __2.2.4.9-1__: Dieses Verzeichnis für die Version von Hortonworks Daten von HDInsight, verwendet, die Anzahl der Cluster möglicherweise andere als die hier aufgeführten Namen.
* __aktuelle__: Dieses Verzeichnis enthält Links auf Verzeichnisse unter dem Verzeichnis __2.2.4.9-1__ und vorhanden ist, müssen Sie eine Versionsnummer eingeben (die ändern,) jedes Mal, wenn Sie auf eine Datei zugreifen möchten.

Beispieldaten und JAR-Dateien befinden sich Hadoop verteilt Datei System bietet oder Azure BLOB-Speicher mit "/ Beispiel ' oder ' Wasbs: / / Beispiel".

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>BIETET Azure BLOB-Speicher und bewährte Methoden zum Speichern

In den meisten Hadoop stützt bietet vom lokalen Speicher auf den Computern im Cluster. Effizient ist, kann es teuer für Cloud-basierte Lösung, wo Sie Stunde oder Minute Datenverarbeitungsressourcen Zahlen.

HDInsight verwendet Azure BLOB-Speicher als Standardspeicher, bietet die folgenden Vorteile:

* Günstige langfristige Speicherung

* Zugriff über externe Dienste wie Websites, Datei hochladen/herunterladen Dienstprogramme, SDKs für verschiedene Sprachen und Webbrowser

Ist der Standardspeicher für HDInsight müssen Sie normalerweise nichts Unternehmen, um es zu verwenden. Beispielsweise wird der folgende Befehl Dateien im Ordner **/example/data** aufgelistet, bei denen auf Azure BLOB-Speicher gespeichert:

    hdfs dfs -ls /example/data

Einige Befehle erfordern soll mit BLOB-Speicher. Dazu können Sie den Befehl mit Präfix **Wasb: / /**, oder **Wasbs: / /**.

HDInsight können Sie einem Cluster mehrere BLOB-Speicher-Konten zugeordnet. Datenzugriff auf eine nicht-BLOB-Speicher Standardkonto verwenden Sie das Format **Wasbs: / /&lt;container-name>@&lt;Namen->.blob.core.windows.net/**. Beispielsweise werden folgende den Inhalt des Verzeichnisses **/example/data** für den angegebenen Container und BLOB-Konto aufgelistet:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Welche BLOB-Speicher ist der Cluster verwenden?

Während der Clustererstellung ausgewählten Konto Azure Storage als Container verwenden oder eine neue erstellen. Dann vergessen Sie wahrscheinlich. Standard-Speicherkonto und Container finden Sie mithilfe der Ambari-REST-API.

1. Verwenden Sie den folgenden Befehl mit Curl bietet Konfigurationsinformationen abgerufen und Filtern Sie [Jq](https://stedolan.github.io/jq/):

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Die erste Konfiguration auf den Server angewendet wird zurückgegeben (`service_config_version=1`,) diese Informationen enthalten. Wenn Sie einen Wert abrufen, der nach Erstellung des Clusters geändert wurde, müssen Sie die Konfigurationsversionen auflisten und aktuell abrufen.

    Dies gibt den Wert zurück ähnlich dem folgenden, __CONTAINER__ Standardcontainer und __Kontoname__ ist der Azure Storage-Kontoname:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Die Ressourcengruppe für das Speicherkonto, [Azure CLI](../xplat-cli-install.md)verwenden. Ersetzen Sie im folgenden Befehl __Kontoname__ Speicherkonto namens Ambari entnommen:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Der Ressourcengruppenname für das Konto wird zurückgegeben.
    
    > [AZURE.NOTE] Wenn keine dieser Befehl zurückgegeben wird, müssen Sie Azure-CLI in Azure-Ressourcen-Manager-Modus ändern, und führen den Befehl erneut. In Azure-Ressourcen-Manager-Modus wechseln, Befehl.
    >
    > `azure config mode arm`
    
2. Den Schlüssel für das Speicherkonto abrufen. Ersetzen Sie die Ressourcengruppe aus dem vorherigen Schritt __GROUPNAME__ . Ersetzen Sie __Kontoname__ Speicher berücksichtigt:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Den primären Schlüssel für das Konto wird zurückgegeben.

Sie finden auch Speicherinformationen über das Azure-Portal:

1. Wählen Sie in [Azure-Portal](https://portal.azure.com/)HDInsight Cluster ein.

2. Wählen Sie im Abschnitt __Essentials__ __Alle__.

3. Wählen Sie __Einstellungen__ __Azure Storage Schlüssel__.

4. __Azure Storage Schlüssel__wählen Speicherkonten aufgeführt. Informationen über das Speicherkonto erscheint.

5. Wählen Sie das Schlüsselsymbol. Schlüssel für dieses Speicherkonto wird angezeigt.

### <a name="how-do-i-access-blob-storage"></a>Zugriffs-BLOB-Speicher

Über den Befehl Hadoop aus dem Cluster werden als verschiedene Arten Blobs auf:

* [CLI für Mac, Linux und Windows Azure](../xplat-cli-install.md): Befehlszeilenschnittstelle für die Arbeit mit Azure Befehle. Verwenden Sie nach der Installation der `azure storage` Befehl Hilfe zur Verwendung von Speicher oder `azure blob` für Blob-Befehle.

* [blobxfer.py](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): ein Python-Skript für die Arbeit mit Blobs in Azure-Speicher.

* Eine Vielzahl von SDKs:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Storage REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>Skalierung des Clusters

Skalierung Feature Cluster können Sie die Anzahl der Datenknoten von einem Cluster wird in Azure HDInsight ohne löschen und neu erstellen des Clusters verwendet.

Skalieren von Operationen während andere ausführen oder Prozesse in einem Cluster ausgeführt werden.

Unterschiedliche Arten betroffen wie folgt skalieren:

* __Hadoop__: Skalierung die Anzahl der Knoten in einem Cluster werden einige Dienste im Cluster neu gestartet. Diese kann Aufträge ausgeführt wird oder aussteht am Ende der Skalierung nicht. Sie können die Aufträge erneut, nachdem der Vorgang abgeschlossen ist.

* __HBase__: regionalen Server innerhalb weniger Minuten nach Abschluss der Skalierung automatisch ausgeglichen sind. Um regionale Server zu verteilen, gehen Sie folgendermaßen vor:

    1. Verbinden Sie mit HDInsight SSH. Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten:

        * [Verwenden von SSH mit HDInsight von Linux, Unix und Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [Verwenden von SSH mit HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. Anhand der folgenden HBase-Shell starten:

            hbase shell

    2. Nachdem die HBase Shell geladen wurde, anhand der folgenden regionalen Server manuell ausgleichen:

            balancer

* __Storm__: Sie sollten alle laufenden Sturm Topologien ausgleichen, nachdem eine Skalierung ausgeführt wurde. Dadurch wird die Topologie korrigieren Parallelität Einstellung basiert auf der neuen Anzahl von Knoten im Cluster. Um laufende Topologien auszugleichen, verwenden Sie eine der folgenden Optionen:

    * __SSH__: Verbindung zum Server und verwenden Sie den folgenden Befehl an eine Topologie auszugleichen:

            storm rebalance TOPOLOGYNAME

        Sie können auch Parameter, um die Parallelität Hinweise ursprünglich von der Topologie überschreiben. Beispielsweise `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` konfigurieren die Topologie 5 Arbeitsprozesse 3 Executors für die Komponente Blau Schnauze und 10 Executors für Gelb Bolzenkomponente.

    * __Storm-Benutzeroberfläche__: Gehen Sie eine Topologie mit Storm-Benutzeroberfläche neu.

        1. Öffnen Sie __https://CLUSTERNAME.azurehdinsight.net/stormui__ im Webbrowser ist CLUSTERNAME auf den Namen des Clusters Sturm. Wenn Sie aufgefordert werden, geben Sie HDInsight Cluster Administrator (Admin) Name und Kennwort, die Sie beim Erstellen des Clusters angegeben.

        3. Wählen Sie die Topologie, die Sie ausgleichen möchten, und wählen Sie __neu__ . Geben Sie die Verzögerung vor Ausführung auszugleichen.

Spezifische Informationen zur Skalierung des HDInsight-Clusters finden Sie unter:

* [Verwalten Sie in HDInsight Hadoop Cluster mithilfe von Azure-Portal](hdinsight-administer-use-portal-linux.md#scaling)

* [Verwalten Sie in HDinsight Hadoop Cluster mithilfe von Azure PowerShell](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Wie installiere ich die Farbe (oder eine andere Komponente Hadoop)?

HDInsight ist ein verwalteter Dienst, wodurch Knoten in einem Cluster zerstört und ausgebessert automatisch von Azure, wenn ein Problem erkannt wird. Aus diesem Grund empfiehlt es nicht manuell Dinge direkt auf den Clusterknoten installieren. Verwenden Sie stattdessen [HDInsight Skriptaktionen](hdinsight-hadoop-customize-cluster.md) müssen Sie Folgendes installieren:

* Einem Dienst oder einer Website Funken oder Farbton.
* Eine Komponente, die Neukonfiguration auf mehreren Knoten im Cluster benötigt. Z. B. erforderlichen Umgebungsvariablen, erstellen ein Protokollierungsverzeichnis oder Erstellen einer Konfigurationsdatei.

Skriptaktionen Bash Skripts während der Bereitstellung Cluster ausgeführt werden, und installieren und konfigurieren zusätzliche Komponenten im Cluster verwendet werden. Beispielskripts sind zur Installation der folgenden Komponenten:

* [Farbton](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Informationen zur Entwicklung eigener Skriptaktionen anzeigen Sie [Skriptaktion Entwicklung mit HDInsight](hdinsight-hadoop-script-actions-linux.md)

###<a name="jar-files"></a>JAR-Dateien

Einige Hadoop Technologies gemäß eigenständige JAR-Dateien, die als Teil eines Auftrags MapReduce oder von Funktionen enthalten Schwein oder Struktur. Diese mit Skript installiert sein, sie häufig Setup erfordert nicht nur für den Cluster nach der Bereitstellung hochgeladen und direkt. Wenn Makle Sie sicher, dass die Komponente überlebt Reinstallation des Clusters möchten, können Sie die JAR-Datei in WASB speichern.

Möchten Sie die neueste Version des [DataFu](http://datafu.incubator.apache.org/)verwenden, können Sie z. B. JAR-Datei mit dem Projekt herunterladen und Hochladen der HDInsight-Cluster. Gehen Sie dann nach der Dokumentation DataFu Schwein oder Struktur verwenden.

> [AZURE.IMPORTANT] Einige Komponenten eigenständige JAR-Dateien sind mit HDInsight bereitgestellt werden, jedoch nicht im Pfad. Wenn Sie eine bestimmte Komponente suchen, können die folgenden Sie im Cluster suchen:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Den Pfad der entsprechenden JAR-Dateien gibt zurück.

Wenn Cluster bereits eine Version einer Komponente als eigenständige JAR-Datei enthält, aber eine andere Version verwenden möchten, können eine neue Version der Komponente zum Cluster hochladen und verwenden sie Ihre Aufträge.

> [AZURE.WARNING] Komponenten mit HDInsight-Cluster vollständig unterstützt und Microsoft Support hilft isolieren und Lösen von Problemen im Zusammenhang mit diesen Komponenten.
>
> Benutzerdefinierte Komponenten erhalten angemessene Unterstützung helfen, das Problem zu beheben. Dadurch kann die Fehlerbehebung oder Aufforderung zu Kanälen für open-Source-Technologien, fundiertes Fachwissen für diese Technologie fand. Beispielsweise sind viele Community-Sites, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-Projekte verfügen Projektsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/), [Funken](http://spark.apache.org/).

## <a name="next-steps"></a>Nächste Schritte

* [Migration von Windows-basierten HDInsight auf Linux](hdinsight-migrate-from-windows-to-linux.md)
* [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)
* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)
* [Verwenden Sie MapReduce Aufträge mit HDInsight](hdinsight-use-mapreduce.md)
