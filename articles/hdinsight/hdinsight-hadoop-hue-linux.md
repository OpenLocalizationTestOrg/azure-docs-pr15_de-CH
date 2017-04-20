<properties
    pageTitle="Verwenden Sie Farbton Hadoop auf HDInsight Linux-Clustern | Microsoft Azure"
    description="Informationen Sie zum Installieren und Farbton mit Hadoop auf HDInsight Linux verwenden."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von Farbton auf HDInsight Hadoop-Cluster

Informationen Sie zum Farbton auf HDInsight Linux-Cluster installieren und Verwenden von tunneling zum Weiterleiten von Anfragen an Farbton.

## <a name="what-is-hue"></a>Was ist Farbton?

Farbton ist eine Reihe von Web Applications, die Interaktion mit einem Hadoop-Cluster verwendet. Können Sie Farbton, Durchsuchen des Speichers für Hadoop Cluster (bei HDInsight-Cluster WASB) Struktur Aufträge Schwein Skripts usw. ausführen. Folgenden Komponenten sind mit Farbton in einem HDInsight Hadoop-Cluster.

* Bienenwachs Hive-Editor
* Schweine
* Metastore-manager
* Oozie
* FileBrowser (die WASB Standardcontainer spricht)
* Projekt-Browser

> [AZURE.WARNING] Komponenten mit HDInsight-Cluster vollständig unterstützt und Microsoft Support hilft isolieren und Lösen von Problemen im Zusammenhang mit diesen Komponenten.
>
> Benutzerdefinierte Komponenten erhalten angemessene Unterstützung helfen, das Problem zu beheben. Dadurch kann die Fehlerbehebung oder Aufforderung zu Kanälen für open-Source-Technologien, fundiertes Fachwissen für diese Technologie fand. Beispielsweise sind viele Community-Sites, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-Projekte verfügen Projektsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Installieren Sie Farbton mit Skriptaktionen

Die folgende Skriptaktion lässt sich Farbton auf einem Linux-basierten HDInsight-Cluster installieren.
https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.sh
    
Dieser Abschnitt enthält Hinweise können das Skript den Cluster mithilfe der Azure-Portal bereitstellen. 

> [AZURE.NOTE] Azure PowerShell, Azure-CLI, HDInsight .NET SDK oder Azure Resource Manager Vorlagen können auch verwendet werden, Skriptaktionen anwenden. Sie können auch Skriptaktionen auf Cluster bereits anwenden. Weitere Informationen finden Sie unter [Cluster mit Skriptaktionen HDInsight anpassen](hdinsight-hadoop-customize-cluster-linux.md).

1. Bereitstellen eines Clusters mithilfe der Schritte in [Bereitstellung HDInsight Cluster unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), aber nicht Bereitstellung abgeschlossen.

    > [AZURE.NOTE] Farbton für HDInsight-Cluster installieren, die empfohlene Hauptknoten ist mindestens A4 (8 Kernen, 14 GB Speicher).

2. **Optionale Konfiguration** Blade wählen Sie **Skriptaktionen**und Informationen Sie wie folgt:

    ![Stellen Skriptparameter für "Farbton"] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Stellen Skriptparameter für "Farbton"")

    * __NAME__: Geben Sie einen Anzeigenamen für die Skriptaktion.
    * __Skript-URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __Kopf__: Aktivieren Sie diese Option
    * __ARBEITSKRAFT__: Dieses Feld leer lassen.
    * __ZOOKEEPER__: Dieses Feld leer lassen.
    * __Parameter__: Dieses Feld leer lassen.

3. Am Ende der **Skriptaktionen**Schaltfläche **Wählen Sie** zum Speichern der Konfiguration. Schließlich Schaltfläche **Wählen Sie** unten die **Optionale Konfiguration** Blade optionale Konfigurationsinformationen speichern.

4. Bereitstellung des Clusters gemäß der [Vorschrift HDInsight Cluster unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal)weiter.

## <a name="use-hue-with-hdinsight-clusters"></a>Farbton mit HDInsight verwenden

SSH-Tunneling ist die einzige Möglichkeit, Farbton im Cluster zuzugreifen, nachdem er ausgeführt wird. Über SSH Tunneling zugelassen, direkt zu den Hauptknoten des Clusters dem Farbton ausgeführt. Nach der Cluster bereitstellen, gehen Sie Farbton in einem HDInsight Linux-Cluster verwendet.

1. Mithilfe der Informationen in [Verwenden Sie SSH-Tunneling Ambari Webbenutzeroberfläche ResourceManager, JobHistory, NameNode, Oozie, und andere Webbenutzeroberfläche auf](hdinsight-linux-ambari-ssh-tunnel.md) HDInsight Cluster einen SSH-Tunnel vom Client-System erstellen und konfigurieren Sie den Webbrowser SSH-Tunnel als Proxy verwenden.

2. Nach einer SSH-Tunnel erstellt und Konfiguration Browser Proxy Datenverkehr finden Sie den Hostnamen des primären Head-Knoten. Dies kann durch Verbinden mit SSH auf Port 22. Beispielsweise `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` wobei __Benutzername__ Ihr Benutzername SSH und __CLUSTERNAME__ ist der Name des Clusters.

    Weitere Informationen über SSH finden Sie in folgenden Dokumenten:

    * [SSH mit Linux-basierten HDInsight Linux, Unix und Mac OS X-Clients verwenden](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden von SSH mit Linux-basierte HDInsight von einem Windows-client](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Nachdem die Verbindung hergestellt ist, verwenden Sie folgenden Befehl zu den vollqualifizierten Domänennamen des primären Hauptknoten:

        hostname -f

    Ein erhalten ähnlich der folgenden:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Dies ist der Hostname des primären Hauptknoten Farbton-Website befindet.

2. Verwenden Sie den Browser öffnen Farbton-Portal unter Http://HOSTNAME:8888. Der Name, den Sie im vorherigen Schritt abgerufen ersetzen Sie HOSTNAMEN.

    > [AZURE.NOTE] Wenn Sie zum ersten Mal anmelden, werden Sie aufgefordert, ein Benutzerkonto Hue-Portal anmelden. Die hier angegebenen Anmeldeinformationen werden im Portal und hängen nicht Admin oder SSH-Anmeldeinformationen während der Bereitstellung des Clusters angegeben.

    ![Anmeldung zum Portal Farbton] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Geben Sie Anmeldeinformationen für Farbton portal")

### <a name="run-a-hive-query"></a>Eine Struktur Abfrage ausführen

1. Hue-Portal auf **Abfrage-Editoren**und klicken Sie auf **Struktur** , um die Hive-Editor zu öffnen.

    ![Verwenden von Struktur] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Verwenden von Struktur")

2. Auf der Registerkarte **Unterstützung** unter **Datenbank**sollte **Hivesampletable**angezeigt werden. Dies ist eine Beispieltabelle mit allen Hadoop auf HDInsight geliefert. Geben Sie im rechten Bereich eine Beispielabfrage und erhalten Sie die Ausgabe auf der Registerkarte **Ergebnisse** im Bereich, wie im Screenshot dargestellt.

    ![Abfrage ausführen-Struktur] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Abfrage ausführen-Struktur")

    Die Registerkarte **Diagramm** können Sie eine visuelle Darstellung des Ergebnisses angezeigt.

### <a name="browse-the-cluster-storage"></a>Durchsuchen des Clusterspeichers

1. Wählen Sie das Portal Farbton **Dateibrowser** oben rechts auf der Menüleiste aus.

2. Dateibrowser Öffnen standardmäßig im Verzeichnis **/user/myuser** . Klicken Sie auf den Schrägstrich vor dem Benutzerverzeichnis im Pfad zum Stamm der Azure-Speichercontainer den Cluster zu.

    ![Dateibrowser verwenden] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Dateibrowser verwenden")

3. Mit der rechten Maustaste auf eine Datei oder einen Ordner verfügbaren Operationen. Die Schaltfläche **Hochladen** rechts zum Hochladen von Dateien im aktuellen Verzeichnis. Verwenden Sie die Schaltfläche **neu** Erstellen neuer Dateien oder Verzeichnisse.

> [AZURE.NOTE] Dateibrowser Farbton zeigen nur den Inhalt der Standardcontainer HDInsight Cluster zugeordnet. Alle zusätzlichen Speicher Konten/Container, die Sie dem Cluster zugeordnet haben möglicherweise über den Dateibrowser nicht. Jedoch werden den zusätzlichen Container den Cluster für Hive-Aufträge immer. Beispielsweise geben Sie den Befehl `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` im Hive-Editor sehen den Inhalt der zusätzlichen Container. In diesem Befehl ist **Newcontainer** nicht den Standardcontainer ein Cluster zugeordnet.

## <a name="important-considerations"></a>Wichtige Hinweise

1. Das Skript zur Installation von Farbton installiert auf primäre Hauptknoten des Clusters.

2. Während der Installation werden mehrere Hadoop Dienste (bietet GARN MR2, Oozie) für die Aktualisierung der Konfigurations gestartet. Nach Abschluss des Skripts installieren Farbton dauert es einige Zeit für andere Hadoop-Dienste zu starten. Dies könnte zunächst Hue-Leistung beeinträchtigen. Alle Dienste starten, werden Farbton voll funktionsfähige.

3.  Farbton verstehen nicht Tez Aufträge die aktuelle Standardeinstellung für Struktur. Wenn MapReduce als Ausführungsmodul Struktur verwenden, aktualisieren Sie des Skripts den folgenden Befehl im Skript verwenden:

        set hive.execution.engine=mr;

4.  Mit Linux haben ein Szenario Sie, Ihre Dienste auf primäre Hauptknoten ausgeführt werden, während der Ressourcen-Manager auf dem sekundären Server ausgeführt werden konnte. Ein Szenario möglicherweise Fehler (siehe unten) Wenn Farbton Aufträge Ausführung des Clusters Einzelheiten. Jedoch können Sie die Details anzeigen, wenn der Auftrag abgeschlossen wurde.

    ![Farbton-Portal-Fehler] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Farbton-Portal-Fehler")

    Dies ist ein bekanntes Problem. Um dieses Problem zu umgehen ändern Sie Ambari aktiven Ressourcen-Manager auch auf primäre Hauptknoten ausgeführt wird.

5.  Farbton versteht WebHDFS und HDInsight-Cluster mithilfe von Azure Storage `wasbs://`. So installiert das benutzerdefinierte Skript mit Skript verwendet WebWasb, der eine kompatible WebHDFS für die Kommunikation mit WASB. Also auch wenn Farbton Portal bietet (wie beim Verschieben der Maus über den **Datei-Browser**), sollten sie als WASB interpretiert werden.


## <a name="next-steps"></a>Nächste Schritte

- [Giraph für HDInsight-Cluster installieren](hdinsight-hadoop-giraph-install-linux.md). Verwenden Sie Cluster Anpassung Giraph auf HDInsight Hadoop-Cluster installieren. Giraph können Sie graphverarbeitung Hadoop und mit Azure HDInsight verwendet werden.

- [Solr auf HDInsight-Cluster installieren](hdinsight-hadoop-solr-install-linux.md). Verwenden Sie Cluster Anpassung Solr auf HDInsight Hadoop-Cluster installieren. Solr können Sie leistungsstarke Suchvorgänge auf gespeicherte Daten.

- [R auf HDInsight-Cluster installieren](hdinsight-hadoop-r-scripts-linux.md). Verwenden Sie Cluster Anpassung R auf HDInsight Hadoop-Cluster installieren. R ist ein Open-Source-Sprache und Umgebung für statistische Datenverarbeitung. Es enthält Hunderte von integrierten statistischen Funktionen und eigene Programmiersprachen, die Aspekte der funktionalen und objektorientierte Programmierung kombiniert. Darüber hinaus umfangreiche Grafiken bereit.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
