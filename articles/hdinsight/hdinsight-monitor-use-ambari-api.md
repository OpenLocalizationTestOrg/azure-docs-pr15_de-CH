<properties
    pageTitle="In HDInsight mit der Ambari-API Hadoop Cluster überwachen | Microsoft Azure"
    description="Verwenden Sie die Apache Ambari APIs zum Erstellen, verwalten und überwachen Hadoop-Clustern. Intuitive Operator Tools und APIs ausblenden Komplexität Hadoop"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Überwachen von Clustern Hadoop in HDInsight mit der Ambari-API

Erfahren Sie mit Ambari APIs HDInsight Cluster überwachen.

> [AZURE.NOTE] Die Informationen in diesem Artikel ist in erster Linie für Windows-basierte HDInsight Cluster schreibgeschützt Ambari REST-API zur Verfügung. Linux-basierten Clustern finden Sie unter [Verwalten Hadoop Cluster mit Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Was ist Ambari?

[Apache Ambari] [ ambari-home] für die Bereitstellung, Verwaltung und Überwachung Apache Hadoop Cluster verwendet. Sie enthält eine intuitive Tools Operator und einen robusten Satz von APIs, die die Komplexität von Hadoop vereinfacht den Betrieb des Clusters. Weitere Informationen zu den APIs finden Sie unter [Ambari-API-Referenz][ambari-api-reference]. 

HDInsight unterstützt zurzeit nur die Ambari überwachen. HDInsight 3.0 und 2.1 Cluster Ambari-API 1.0 unterstützt. Dieser Artikel behandelt den Zugriff auf Ambari-APIs in HDInsight Version 3.1 und 2.1-Cluster. Der wesentliche Unterschied zwischen beiden ist, dass einige der Komponenten mit der Einführung neuer Funktionen (wie Geschichte Auftragsserver) geändert haben. 

**Erforderliche Komponenten**

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Eine Workstation mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Optional) [Aufrollen] [curl]. Sie finden Sie unter [Rollen Downloads und frei][curl-download].

    >[AZURE.NOTE] Wenn Befehl cURL in Windows verwenden doppelte Anführungszeichen statt einfache Anführungszeichen für die Werte.

- **Ein Azure HDInsight-Cluster**. Informationen zur Bereitstellung von Cluster finden Sie unter [Erste Schritte mit HDInsight] [ hdinsight-get-started] oder [Bereitstellung HDInsight Cluster][hdinsight-provision]. Sie benötigen die folgenden Daten in das Tutorial:

    Clustereigenschaft|Azure PowerShell Variablennamen|Wert|Beschreibung
    ---|---|---|---
    HDInsight Clustername|$clusterName||Der Name des Clusters HDInsight.
    Cluster-Benutzername|$clusterUsername||Cluster-Benutzername angegeben, bei der Erstellung des Clusters.
    Cluster-Kennwort|$clusterPassword||Cluster-Benutzerkennwort.

    >[AZURE.NOTE] Eingeben der Werte in der Tabelle. Für dieses Lernprogramm durchlaufen werden.

## <a name="jump-start"></a>Schnellstart

Es gibt verschiedene mit Ambari HDInsight-Cluster überwachen.

**Azure PowerShell verwenden**

Im folgenden ist ein Skript Azure PowerShell MapReduce Job Tracker Informationen zu *in einem Cluster 3.1 HDInsight.*  Der Hauptunterschied ist, dass wir diese Informationen vom Dienst aus (statt MapReduce) ziehen.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Im folgenden finden eine Azure PowerShell-Skript zum Abrufen der MapReduce Job Tracker Informationen *in einem Cluster HDInsight 2.1*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Die Ausgabe lautet:

![Jobtracker-Ausgabe][img-jobtracker-output]

**Mit Kringel**

Nachfolgend ein Beispiel Clusterinformationen mit Kringel abrufen:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Die Ausgabe lautet:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**10/8/2014 veröffentlicht**:

Verwendung des Ambari-Endpunkts "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" *Host_name* -Feld gibt den vollqualifizierten Domänennamen (FQDN) des Knotens anstelle des Hostnamens. Vor der Version 8/10/2014 zurückgegeben dabei einfach "**headnode0**". Nach der Veröffentlichung 8/10/2014 erhalten den FQDN "**headnode0. {} ClusterDNS} .azurehdinsight .net**", wie im vorherigen Beispiel gezeigt. Diese Änderung war erforderlich, vereinfacht Szenarios, in denen mehrere Clustertypen (wie HBase und Hadoop) in einem virtuellen Netzwerk (VNET) bereitgestellt werden können. Dies geschieht beispielsweise bei Verwendung von HBase als Back-End-Plattform für Hadoop.

## <a name="ambari-monitoring-apis"></a>Ambari Überwachung APIs

Die folgende Tabelle enthält einige der häufigsten Ambari Überwachung API-Aufrufe. Weitere Informationen zur API finden Sie unter [Ambari-API-Referenz][ambari-api-reference].

Monitor-API-Aufruf|URI|Beschreibung
---|---|---
Abrufen von Clustern|`/api/v1/clusters`|
Clusterinformationen abrufen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|Cluster, Dienste, Server
Dienste|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Dienste: bietet Mapreduce
Erfahren Sie Services.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Abrufen von Komponenten|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|BIETET: Namenode datanode<br/>MapReduce: Jobtracker; tasktracker
Erfahren Sie Komponente.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo Host-Komponenten, Metriken
Hosts abrufen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0 workernode0
Host-Informationen abrufen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
Erhalten von Host-Komponenten|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|Namenode resourcemanager
Infos Host Komponente.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles-Komponente, Host, Metriken
Abrufen von Konfigurationen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Config-Typen: Core-Website bietet Website Mapred Website, Struktur-Website
Konfigurationsinformationen zu erhalten.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Config-Typen: Core-Website bietet Website Mapred Website, Struktur-Website


##<a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie zum Ambari Überwachung API-Aufrufe verwenden. Weitere Informationen finden Sie unter:

- [Verwalten Sie HDInsight-Cluster mithilfe der Azure-Portal][hdinsight-admin-portal]
- [Verwalten Sie HDInsight-Cluster mithilfe von Azure PowerShell][hdinsight-admin-powershell]
- [Verwalten von HDInsight Cluster mit Befehlszeilen-Schnittstelle][hdinsight-admin-cli]
- [HDInsight-Dokumentation][hdinsight-documentation]
- [Erste Schritte mit HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
