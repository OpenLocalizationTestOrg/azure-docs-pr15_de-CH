<properties
   pageTitle="Überwachen und Verwalten von HDInsight Cluster mit Apache Ambari REST API | Microsoft Azure"
   description="Enthält Informationen zum Verwenden von Ambari zur Überwachung und Verwaltung von Linux-basierten HDInsight-Cluster. In diesem Dokument erfahren Sie, wie Sie Ambari REST API mit HDInsight enthalten."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Verwalten von HDInsight-Cluster mithilfe der REST-API Ambari

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereinfacht die Verwaltung und Überwachung von Hadoop Cluster mit leicht Webbenutzeroberfläche und REST-API verwenden. Ambari HDInsight Linux-basierten Clustern befindet und zum Überwachen des Clusters und Konfiguration ändern. In diesem Dokument lernen Sie die Grundlagen der Ambari REST API durch allgemeine Aufgaben mit cURL.

##<a name="prerequisites"></a>Erforderliche Komponenten

* [Rollen](http://curl.haxx.se/): cURL ist ein plattformübergreifendes Dienstprogramm, REST-APIs in der Befehlszeile arbeiten. In diesem Dokument dient es Ambari REST API kommunizieren.
* [Jq](https://stedolan.github.io/jq/): Jq ist eine plattformübergreifende Befehlszeilenprogramm für die Arbeit mit JSON-Dokumente. In diesem Dokument dient es Ambari REST API zurückgegebenen JSON-Dokumente analysieren.
* [Azure-CLI](../xplat-cli-install.md): eine plattformübergreifende Befehlszeilenprogramm für die Arbeit mit Azure Services.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Was ist Ambari?

[Apache Ambari](http://ambari.apache.org) macht Hadoop Management einfacher durch eine leicht zu bedienende Web UI bereitstellen, verwalten und Überwachen von Hadoop-Cluster verwendet werden kann. Entwickler können diese Funktionen mithilfe der [Ambari REST-APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)in ihre Anwendung integrieren.

Ambari wird standardmäßig mit Linux-basierten HDInsight bereitgestellt.

##<a name="rest-api"></a>REST-API

Base-URI für die Ambari-REST-API auf HDInsight ist https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, wobei __CLUSTERNAME__ den Namen des Clusters.

> [AZURE.IMPORTANT] Der Clustername qualified Domain Name (FQDN) Teil des URIS (CLUSTERNAME.azurehdinsight.net) werden Kleinschreibung anderen Vorkommnisse im URI beachtet. Beispielsweise wenn Cluster MeinCluster heißt, sind folgende gültige URIs:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Folgende URIs einen Fehler zurück, weil das zweite Vorkommen des Namens nicht die Groß-/Kleinschreibung.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Ambari auf HDInsight mit erforderlich HTTPS. Bei der Verbindung verwenden Sie Administratorkontonamen (der Standardwert ist __Admin__) und Kennwort ein, das Sie bei der Erstellung des Clusters angegeben.

Im folgende Beispiel wird cURL zu einer GET-Anforderung für die REST-API verwendet. Ersetzen Sie __PASSWORD__ durch das Administratorkennwort für den Cluster. Der Name des Clusters ersetzen Sie __CLUSTERNAME__ :

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Die Antwort ist ein JSON-Dokument mit Informationen, die der folgenden ähnelt:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Da dies JSON, ist es einfacher, mit JSON-Parser mit den Daten arbeiten. Im folgenden Beispiel wird beispielsweise Jq nur die `health_report` Element.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Beispiel: Abrufen Sie den FQDN von Clusterknoten

Beim Arbeiten mit HDInsight müssen Sie den vollqualifizierten Domänennamen (FQDN) eines Clusterknotens kennen. Den FQDN für die verschiedenen Knoten im Cluster der folgende können einfach abgerufen werden:

* __Head-Knoten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Arbeitskraft-Knoten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper-Knoten__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Beachten Sie, dass jedes dieser Beispiele demselben Muster folgen:

1. Abfrage eine Komponente, die wir wissen auf diesen Knoten ausgeführt wird.
2. Abrufen der `host_name` -Elemente, die den FQDN für diese Knoten enthalten.

Die `host_components` das Retouren-Dokument enthält mehrere Elemente. Mit `.host_components[]`, und jedes Element durchlaufen und ziehen Sie den Wert von bestimmten Pfad wird einen Pfad innerhalb des Elements anzugeben. Wenn Sie nur einen Wert wie dem ersten Eintrag FQDN können Elemente als Auflistung zurück und wählen Sie dann einen bestimmten Eintrag:

    jq '[.host_components[].HostRoles.host_name][0]'

Den ersten vollqualifizierten Domänennamen zurückgegeben aus der Auflistung.

##<a name="example-get-the-default-storage-account-and-container"></a>Beispiel: Standard-Speicherkonto und Container abrufen

Beim Erstellen eines HDInsight-Clusters müssen Sie ein Speicherkonto Azure und BLOB-Container als der Standardspeicher für den Cluster verwenden. Ambari können Sie diese Informationen abrufen, nach Erstellung des Clusters. Beispielsweise möchten Sie programmgesteuert schreiben Sie Daten direkt auf den Container.

Folgendes wird der WASB URI der Standardspeicher Cluster abrufen:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Dies gibt die erste Konfiguration auf den Server angewendet (`service_config_version=1`,) enthält diese Informationen. Wenn Sie einen Wert, der nach Erstellung des Clusters geändert wurde abrufen, müssen Sie die Konfigurationsversionen auflisten und aktuell abrufen.

Dies ergibt den Wert wie beispielsweise dem __CONTAINER__ Standardcontainer und __Kontoname__ ist der Azure Storage-Kontoname:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Anschließend können Sie diese Informationen mit der [Azure-CLI](../xplat-cli-install.md) den upload oder download von Daten aus dem Container.

1. Die Ressourcengruppe für das Speicherkonto abrufen. Konto Name Ambari entnommen ersetzen Sie __Kontoname__ :

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Ressource Gruppenname für das Konto zurückgegeben.
    
    > [AZURE.NOTE] Wenn keine dieser Befehl zurückgegeben wird, müssen Sie Azure-CLI in Azure-Ressourcen-Manager-Modus ändern, und führen den Befehl erneut. Verwenden Sie in Azure-Ressourcen-Manager-Modus zu wechseln den folgenden Befehl ein:
    >
    > `azure config mode arm`
    
2. Den Schlüssel für das Speicherkonto abrufen. Ersetzen Sie die Ressourcengruppe aus dem vorherigen Schritt __GROUPNAME__ . Ersetzen Sie __Kontoname__ Speicher berücksichtigt:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    Dieses Beispiel gibt den Primärschlüssel für das Konto.
    
3. Befehl der Upload eine Datei im Container gespeichert:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Ersetzen Sie __Kontoname__ Speicher berücksichtigt. Ersetzen Sie __ACCOUNTKEY__ durch zuvor abgerufene Schlüssel. __FILEPATH__ ist der Pfad der Datei hochladen __BLOBPATH__ ist der Pfad im Container möchten.

    Beispielsweise, wenn Sie am wasbs://example/data/filename.txt in HDInsight angezeigt werden soll, __BLOBPATH__ wäre `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Beispiel: Update Ambari Konfiguration

1. Die aktuelle Konfiguration Ambari "Konfiguration gespeichert" zu erhalten:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    Dieses Beispiel gibt ein JSON-Dokument mit der aktuellen Konfiguration (gekennzeichnet durch den Wert der _Marke_ ) für die Komponenten im Cluster installiert. Im folgende Beispiel ist ein Auszug aus einem Cluster Spark zurückgegebenen Daten.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    Aus dieser Liste müssen Sie den Namen der Komponente kopieren (z. B. __Spark\_Sparsamkeit\_Sparkconf__ und den Wert der __Marke__ .
    
2. Rufen Sie die Konfiguration für die Komponente und Tag mit dem folgenden Befehl ab. Ersetzen Sie __Spark Sparsamkeit Sparkconf__ und __ANFÄNGLICHEN__ mit der Komponente und der Tag, das die Konfiguration abgerufen werden soll.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Aufrollen Ruft das JSON-Dokument und Jq verwendet, um die Daten zum Erstellen einer Vorlage. Die Vorlage wird dann zum Hinzufügen/Ändern von Werten. Insbesondere bewirkt Folgendes:
    
    * Erstellt einen eindeutigen Wert mit der Zeichenfolge "Version" und das Datum, die in __Newtag__gespeichert.
    * Erstellt ein Stammdokument für die neue gewünschte Konfiguration.
    * Ruft den Inhalt der `.items[]` array und fügt unter dem __Desired_config__ -Element.
    * Löscht __Href__, __Version__und __Konfiguration__ Elemente dieser Elemente benötigt werden, um eine neue Konfiguration senden.
    * Fügt ein neues __Tag__ -Element und legt seinen Wert auf __Version ###__. Der numerische Teil basiert auf dem aktuellen Datum. Jede Konfiguration muss ein eindeutiges Kennzeichen.
    
    Schließlich werden die Daten an das __newconfig.json__ -Dokument gespeichert. Die Dokumentstruktur sollte ähnlich wie das folgende Beispiel aussehen:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Öffnen Sie die Werte __newconfig.json__ Dokument und bearbeiten/hinzufügen in das __Properties__ -Objekt. Im folgenden Beispiel wird den Wert __"spark.yarn.am.memory"__ von __"1"__ , __"3 g"__ und und fügt ein neues Element für __"spark.kryoserializer.buffer.max"__ mit dem Wert __"256 MB"__.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Speichern Sie die Datei anschließend zu ändern.

4. Verwenden Sie den folgenden Befehl zum Senden der aktualisierten Konfigurations Ambari.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Dieser Befehl leitet den Inhalt der Datei __newconfig.json__ die Curl-Anforderung, die dem Cluster neue Konfiguration sendet. CURL-Anforderung gibt eine JSON-Dokument. __VersionTag__ Element in diesem Dokument übereinstimmen Version eingereichten und __Konfigurationen__ -Objekt enthält die angeforderte Konfiguration ändert.

###<a name="example-restart-a-service-component"></a>Beispiel: Starten einer Service-Komponente

Nun betrachten Sie Ambari Webbenutzeroberfläche, zeigt Spark Service muss neu gestartet werden, bevor die neue Konfiguration wirksam werden. Gehen Sie folgendermaßen vor, um den Dienst neu starten.

1. Anhand der folgenden um Wartungsmodus für Spark-Dienst zu aktivieren:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Dieser Befehl sendet ein JSON-Dokument auf dem Server (enthalten der `echo` -Anweisung) der Wartungsmodus aktiviert.
    Sie können überprüfen, dass der Dienst jetzt im Wartungsmodus die folgende Anforderung:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Dies liefert den Wert `"ON"`.

3. Verwenden Sie folgende weiter, um den Dienst zu deaktivieren:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Dieser Befehl gibt eine Antwort ähnlich der folgenden.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    Die `href` Rückgabewert dieser URI verwendet die interne IP-Adresse des Clusterknotens. Um von außerhalb des Clusters verwenden, ersetzen Sie '10.0.0.18:8080'-Teil mit dem vollqualifizierten Domänennamen des Clusters. Der folgende Befehl ruft beispielsweise den Status der Anforderung.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Wenn die zurückgegebene Wert `"COMPLETED"` dann die Anforderung abgeschlossen ist.

4. Nach Abschluss die vorhergehenden Anforderung anhand der folgenden um den Dienst zu starten.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Sobald der Dienst neu gestartet wird, wird die neue Konfigurationen.

5. So deaktivieren Sie den Wartungsmodus verwenden Sie folgenden schließlich.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Nächste Schritte

Eine vollständige Übersicht über die REST-API finden Sie unter [Ambari-API-Referenz V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Ambari Funktionalität ist deaktiviert, da es Clouddienst HDInsight verwaltet; z. B. hinzufügen oder Entfernen von Hosts aus dem Cluster oder Hinzufügen neuer Dienste.
