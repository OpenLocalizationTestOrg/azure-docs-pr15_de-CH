<properties
   pageTitle="Bereitstellen und Verwalten von Apache Storm Topologien für Linux-basierte HDInsight | Microsoft Azure"
   description="Informationen Sie zum Bereitstellen, überwachen und Verwalten von Apache Storm Topologien Storm-Dashboard auf Linux-basierten HDInsight verwenden. Verwenden Sie Hadoop Tools für Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Bereitstellen und Verwalten von Apache Storm Topologien auf Linux-basierten HDInsight

Erfahren Sie in diesem Dokument die Grundlagen der Verwaltung und Überwachung der Storm-Topologien für HDInsight-Cluster auf Linux-basierten ausgeführt.

> [AZURE.IMPORTANT] Die Schritte in diesem Artikel erfordern einen Linux-basierte Sturm HDInsight Cluster. Informationen zu Bereitstellung und Überwachung von Topologien auf Windows-basierten HDInsight finden Sie unter [Bereitstellen und Verwalten von Apache Storm Topologien auf Windows basierenden HDInsight](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Erforderliche Komponenten

* **Ein Linux-basiertes Sturm HDInsight Cluster**: finden Sie unter [Erste Schritte mit Apache auf HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) Schritte zum Erstellen eines Clusters

* **Vertrautheit mit SSH und SCP**: Weitere Informationen zur Verwendung von SSH und SCP mit HDInsight finden Sie im folgenden:

    * **Linux, Unix oder OS X Clients**: Siehe [Verwendet SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-Clients**: Siehe [Verwendet SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

* **Ein SCP-Client**: wird in allen Linux, Unix und Mac OS-Systemen bereitgestellt. Für Windows-Clients empfehlen wir PSCP, [kitten Downloadseite](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)verfügbar ist.

## <a name="start-a-storm-topology"></a>Starten einer Storm-Topologie

### <a name="using-ssh-and-the-storm-command"></a>Verwenden von SSH und Sturm-Befehl

1. SSH Verbindung HDInsight Cluster verwenden. **Benutzernamen** ersetzen die die SSH-Benutzername. Der Clustername HDInsight ersetzen Sie **CLUSTERNAME** :

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Weitere Informationen über SSH Verbindung zum Cluster HDInsight finden Sie in folgenden Dokumenten:
    
    * **Linux, Unix oder OS X Clients**: Siehe [Verwendet SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windows-Clients**: Siehe [Verwendet SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie folgenden Befehl ein Beispieltopologie starten:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    WordCount Beispieltopologie startet im Cluster. Es zufällig generiert Sätze und zählen der Vorkommen eines Worts Sätze.

    > [AZURE.NOTE] Bei Topologie des Clusters müssen Sie die JAR-Datei mit den Cluster vor Kopieren der `storm` Befehl. Dies erfolgt mit der `scp` Befehl vom Client, in dem die Datei vorhanden ist. Zum Beispiel`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > WordCount-Beispiel und andere Sturm Starter Beispiele sind bereits auf Ihrem Cluster enthalten `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Programmgesteuert

Sie können eine Topologie programmgesteuert auf HDInsight bereitstellen, mit der Nimbus-Dienst im Cluster. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) bietet ein Beispiel Java-Anwendung, die zum Bereitstellen und Topologie über den Nimbus-Dienst zu starten.

## <a name="monitor-and-manage-using-the-storm-command"></a>Überwachen und Verwalten mit dem Befehl Sturm

Die `storm` -Dienstprogramm können Sie mit Topologien über die Befehlszeile ausführen. Folgendes ist eine Liste der häufig verwendeten Befehle. Mit `storm -h` für eine vollständige Liste von Befehlen.

### <a name="list-topologies"></a>Liste Topologien

Mit dem folgenden Befehl alle ausführen Topologien:

    storm list
    
Dies gibt Informationen ähnlich der folgenden zurück:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deaktivieren und reaktivieren

Deaktivieren einer Topologie hält es bis getötet oder reaktiviert. Anhand der folgenden deaktivieren und reaktivieren:

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Abbrechen einer laufenden Topologie

Storm-Topologien gestartet, werden weiterhin ausgeführt, bis. Verwenden Sie den folgenden Befehl, um eine Topologie zu beenden:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Ausgleich

Ausgleich einer Topologie kann das System die Parallelität der Topologie zu überarbeiten. Beispielsweise, wenn den Cluster weitere Notizen angepasst haben, Lastausgleich ermöglicht ausgeführte Topologie zu neuen Knoten verwenden.

> [AZURE.WARNING] Ausgleich einer Topologie zunächst deaktiviert die Topologie Arbeitskräfte gleichmäßig im Cluster verteilt dann schließlich gibt die Topologie in den Zustand vor dem Ausgleich ist aufgetreten. So wurde die Topologie aktiv, wird er wieder aktiv. Wenn es deaktiviert wurde, bleibt es deaktiviert.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Überwachen Sie und verwalten Sie mithilfe der Storm-Benutzeroberfläche

Storm-Benutzeroberfläche bietet eine Web-Oberfläche für die Arbeit mit Topologien und auf HDInsight Cluster enthalten. Zum Anzeigen der Storm-Benutzeroberfläche verwenden Sie einen Webbrowser __https://CLUSTERNAME.azurehdinsight.net/stormui__geöffnet, wobei __CLUSTERNAME__ den Namen des Clusters.

> [AZURE.NOTE] Wenn aufgefordert, einen Benutzernamen und ein Kennwort einzugeben, geben Sie den Cluster-Administrator (Admin) und Kennwort, das Sie verwendet, wenn des Clusters.


### <a name="main-page"></a>Hauptseite

Die Hauptseite der Storm-Benutzeroberfläche enthält folgende Informationen:

* **Cluster-Zusammenfassung**: grundlegende Informationen zum Cluster Sturm.

* **Topologie-Zusammenfassung**: eine Liste der ausgeführten Topologien. Verwenden Sie die Links in diesem Abschnitt Weitere Informationen zu bestimmten Topologien.

* **Supervisor Zusammenfassung**: Informationen zum Vorgesetzten Sturm.

* **Nimbus-Konfiguration**: Nimbus-Konfiguration für den Cluster.

### <a name="topology-summary"></a>Topologie-Zusammenfassung

Im Abschnitt **Zusammenfassung Topologie** eine Verknüpfung auswählen zeigt die folgenden Informationen zur Topologie:

* **Topologie-Zusammenfassung**: grundlegende Informationen zur Topologie.

* **Topologie Maßnahmen**: Maßnahmen, die Sie für die Topologie ausführen können.

  * **Aktivieren**: Lebensläufe Verarbeitung einer deaktivierten Topologie.

  * **Deaktivieren**: unterbricht eine laufende Topologie.

  * **Neu**: passt die Parallelität der Topologie. Sie sollten ausgeführte Topologien ausgleichen, nachdem Sie die Anzahl der Knoten im Cluster geändert haben. Dadurch wird die Topologie Parallelität Ausgleich für die erhöht oder verringert die Anzahl der Knoten im Cluster anzupassen.

    Weitere Informationen finden Sie unter <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding Parallelität Storm-Topologie</a>.

  * **Beenden**: beendet eine Storm-Topologie angegebenen Timeout.

* **Topologie-Statistiken**: Statistiken zur Topologie. Verwenden Sie die Links in der Spalte **Fenster** Zeitrahmen für die verbleibenden Einträge auf der Seite festgelegt.

* **Tüllen**: die Ausläufe der Topologie verwendet. Verwenden Sie die Links in diesem Abschnitt Weitere Informationen zu bestimmten Ausläufe.

* **Schrauben**: Schrauben von der Topologie verwendet. Verwenden Sie die Links in diesem Abschnitt Weitere Informationen zu bestimmten Schrauben.

* **Topologie-Konfiguration**: die Konfiguration der ausgewählten Topologie.

### <a name="spout-and-bolt-summary"></a>Auslauf und Mutterschrauben-Zusammenfassung

**Tüllen** oder **Schrauben** Abschnitte eine Schnauze auswählen zeigt die folgenden Informationen über das ausgewählte Element:

* **Komponente Zusammenfassung**: grundlegende Informationen zum Auslauf oder Bolzen.

* **Auslauf-Schraube Statistiken**: Statistiken über die Schnauze oder Bolzen. Verwenden Sie die Links in der Spalte **Fenster** Zeitrahmen für die verbleibenden Einträge auf der Seite festgelegt.

* **Eingabe-Statistiken** (nur Bolzen): Informationen der Bolzen belegten Eingabestreams.

* **Ausgabe Stats**: Informationen über Streams von diesem ausgegeben Auslauf oder Bolzen.

* **Executors**: Informationen über die Schnauze oder Bolzen. Wählen Sie den Eintrag **Port** für einen bestimmten Executor Anzeigen eines Protokolls der Diagnoseinformationen für diese Instanz hergestellt.

* **Fehler**: Fehlerinformationen für diese Auslauf oder Bolzen.

## <a name="rest-api"></a>REST-API

Storm-Benutzeroberfläche baut auf die REST-API wie Management und Überwachungsfunktionen mithilfe der REST-API ausführen können. Die REST-API können Sie um benutzerdefinierte Tools zur Verwaltung und Überwachung Sturm Topologien zu erstellen.

Weitere Informationen finden Sie unter [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Folgendes ist die Verwendung von REST-API mit Apache auf HDInsight.

> [AZURE.IMPORTANT] Storm REST API ist nicht über das Internet öffentlich verfügbar und muss mit einem SSH Tunnel HDInsight Cluster-Head-Knoten zugegriffen werden. Informationen zum Erstellen und Verwenden eines SSH-Tunnels finden Sie unter [Verwenden SSH Tunneling auf Ambari Webbenutzeroberfläche ResourceManager, JobHistory, NameNode, Oozie, und andere Webbenutzeroberfläche](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Basis-URI

Der base-URI für die REST-API auf Linux-basierten HDInsight Cluster steht auf dem Head-Knoten unter **Https://HEADNODEFQDN:8744/api/v1/**; der Domänenname des Head-Knotens jedoch während der Clustererstellung generiert und ist nicht statisch.

Der vollqualifizierte Domänenname (FQDN) finden für die Cluster-Head-Knoten auf verschiedene Weise Sie:

* __Aus einer SSH-Sitzung__: Verwenden Sie den Befehl `headnode -f` aus einer SSH-Sitzung zum Cluster.

* __Aus Ambari Web__: Wählen Sie __Dienste__ vom oberen Rand der Seite und dann __Sturm__. Wählen Sie auf der Registerkarte __Übersicht__ __Storm UI Server__. Der FQDN des Knotens Storm-Benutzeroberfläche und REST API läuft werden am oberen Rand der Seite.

* __Aus Ambari REST API__: Verwenden Sie den Befehl `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` zum Abrufen von Informationen über den Knoten, die die Storm-Benutzeroberfläche und die REST-API ausgeführt werden. Ersetzen Sie __PASSWORD__ durch das Administratorkennwort für den Cluster. Der Clustername ersetzen Sie __CLUSTERNAME__ . Die Antwort enthält den Eintrag "Hostname" den FQDN des Knotens.

### <a name="authentication"></a>Authentifizierung

Die REST-API verwenden **Standardauthentifizierung**HDInsight Clusternamen und Kennwort verwenden.

> [AZURE.NOTE] Da Standardauthentifizierung unverschlüsselt gesendet wird, sollten Sie **immer** verwenden HTTPS zum Absichern der Kommunikation mit dem Cluster.

### <a name="return-values"></a>Rückgabewerte

REST-API zurückgegebenen Informationen möglicherweise nur innerhalb des Clusters oder virtuellen Computern auf demselben Azure Virtual Network-Cluster verwendet. Z. B. den vollqualifizierten Domänennamen (FQDN) für Zookeeper-Server über das Internet nicht zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

Da Sie gelernt haben, bereitstellen und überwachen Topologien Storm-Dashboard mit Informationen wie [entwickeln Java-basierte Topologien mit Maven](hdinsight-storm-develop-java-topology.md).

Finden Sie eine Liste mehrere Topologien [Topologien für auf HDInsight](hdinsight-storm-example-topology.md).
