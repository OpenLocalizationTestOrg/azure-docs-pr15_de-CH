<properties
    pageTitle="Access Hadoop GARN Anwendungsprotokolle programmgesteuert | Microsoft Azure"
    description="Access-Anwendung protokolliert programmgesteuert in einem Cluster Hadoop in HDInsight."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
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

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Zugriff Anwendungsprotokolle aus auf Windows-basierten HDInsight

Dieses Thema erläutert die Protokolle für Applikationen GARN (noch eine andere Ressource Vermittlung) zugreifen, die in einem Cluster Hadoop in Azure HDInsight haben

> [AZURE.NOTE] Die Informationen in diesem Dokument gilt nur für Windows-basierte HDInsight-Cluster. Protokolliert Informationen über den GARN auf HDInsight Linux-basierten Clustern finden Sie unter [Zugriff GARN Anwendungsprotokolle auf Linux-basierten Hadoop auf HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Erforderliche Komponenten

- Eine Windows-basierte HDInsight-Cluster.  Finden Sie unter [Erstellen von Windows-basierten Hadoop Cluster in HDInsight](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>AUS Zeitachse Server

<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Aus Zeitachse Server</a> bietet allgemeinen Informationen auf Bewerbungen Framework-Anwendungsinformationen über zwei verschiedene Schnittstellen. Insbesondere:

* Speicherung und Abruf von Informationen auf HDInsight Cluster generische Anwendung wurde aktiviert, Version 3.1.1.374 oder höher.
* Der Framework-Anwendung Informationen Bestandteil der Zeitachse Server ist derzeit nicht verfügbar auf HDInsight-Cluster.


Allgemeine Informationen über die Anwendung enthält die folgenden Arten von Daten:

* Die ID der Anwendung, eine eindeutige Kennung für eine Anwendung
* Der Benutzer, der die Anwendung gestartet
* Informationen in versucht das Ausfüllen
* Jegliche Anwendung verwendete Container

Auf der HDInsight-Cluster werden diese Informationen von Azure-Ressourcen-Manager einen Verlauf Shop im Standardcontainer Standardkonto Azure-Speicher gespeichert. Diese generischen Daten Bewerbungen können über eine REST-API abgerufen werden:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>GARN-Applikationen und Protokolle

GARN unterstützt mehrere Programmiermodelle (einer davon MapReduce) entkoppeln Ressourcenmanagement aus Anwendung Planung und Überwachung. Dies erfolgt durch eine globale *Ressourcen-Manager* (RM) pro Arbeitskraft Knoten *NodeManagers* (NMs) und pro Anwendung *ApplicationMasters* (AMs). AM pro Anwendung verwendet läuft mit RM Ressourcen (CPU, Arbeitsspeicher, Datenträger, Netzwerk) RM arbeitet mit NMs erteilen diese Ressourcen als *Container*gewährt werden. Die Uhr ist für Fortschritt der Container RM zugewiesen Eine Anwendung kann viele Container je nach Art der Anwendung benötigen.

Darüber hinaus jede Anwendung bestehen mehrere *Anwendung* um vor Abstürzen oder durch die Unterbrechung der Verbindung zwischen einem beenden und ein RM. Container sind daher einen bestimmten Versuch einer Anwendung gewährt. In gewisser Hinsicht Container stellt den Kontext für die Einheit der Arbeit von einer Anwendung aus und alle Arbeit innerhalb eines Containers erfolgt auf einzelnen workerknoten auf dem Container zugeordnet wurde. [GARN-Konzepte] finden Sie unter[ YARN-concepts] zu Referenzzwecken.

Anwendungsprotokolle (und die zugehörigen Container Protokolle) sind entscheidend für problematische Hadoop Debuggen. GARN ermöglicht gut erfassen, sammeln und Speichern von Anwendungsprotokollen mit [Protokoll Aggregation] [ log-aggregation] Funktion. Protokoll Aggregierungsfunktion wird Zugriff auf Anwendungsprotokolle gezielter, Protokolle über alle Container auf eine Arbeitskraft Knoten aggregiert und nach Beendigung eine Anwendung als eine aggregierte Protokolldatei pro Arbeitskraft Knoten auf das Standard-Dateisystem gespeichert. Ihre Anwendung kann Hunderte oder Tausende von Containern verwenden Protokolle für alle Container ausführen auf einem einzelnen Worker-Knoten immer in einer einzigen Datei, wodurch eine Protokolldatei pro Arbeitskraft Knoten von der Anwendung verwendeten aggregiert. Aggregation Protokoll standardmäßig auf HDInsight Cluster (Version 3.0 und höher), und aggregierte Protokolle finden Sie in den Standardcontainer des Clusters am folgenden Speicherort:

    wasbs:///app-logs/<user>/logs/<applicationId>

Ort, *Benutzer* ist der Name des Benutzers, der die Anwendung gestartet und *ApplicationId* der eindeutige Bezeichner einer Anwendung vom GARN RM ist.

Die aggregierten Protokolle sind nicht direkt lesbar, Schreiben in [TFile][T-file], [Binärformat] [ binary-format] Container indiziert. GARN enthält CLI Tools zum Sichern dieser Protokolle als Klartext für oder Container an. Nur-Text mit den folgenden GARN direkt auf den Clusterknoten (nach dem Anschließen durch RDP) Befehle können Sie diese Protokolle anzeigen:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>GARN ResourceManager-Benutzeroberfläche

GARN ResourceManager-Benutzeroberfläche auf dem Hauptknoten Cluster ausgeführt wird und Azure Portal Dashboard möglich: 

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com/). 
2. Im linken Menü auf, klicken Sie **Durchsuchen,**klicken Sie auf **HDInsight-Cluster**einen Windows-basierten Cluster, den auf die Anwendungsprotokolle aus zugreifen möchten.
3. Klicken Sie im oberen Menü auf **Dashboard**. Sie sehen eine Seite namens **HDInsight Abfragekonsole**Registerkarte in einem neuen Browserfenster geöffnet.
4. **HDInsight Abfrage-Konsole**klicken Sie auf **Benutzeroberfläche aus**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
