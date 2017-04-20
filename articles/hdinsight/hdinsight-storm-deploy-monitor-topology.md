<properties
   pageTitle="Bereitstellen und Verwalten von Apache Storm-Topologien HDInsight | Microsoft Azure"
   description="Informationen Sie zum Bereitstellen, überwachen und verwalten HDInsight Storm-Dashboard mit Apache Storm-Topologien. Verwenden Sie Hadoop Tools für Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Bereitstellen und Verwalten von Apache Storm Topologien auf Windows-basierten HDInsight

Storm-Dashboard ermöglicht die einfache Bereitstellung und Apache Storm Topologien zum HDInsight Cluster durch Webbrowser ausgeführt. Sie können auch das Dashboard überwachen und Verwalten von ausgeführten Topologien. Wenn Sie Visual Studio verwenden, bieten HDInsight Tools für Visual Studio ähnlichen Features in Visual Studio.

Storm-Dashboard und Storm-Features in HDInsight Tools basieren auf Sturm REST API dazu Ihre eigene Überwachung und Management-Lösungen.

> [AZURE.IMPORTANT] Die Schritte in diesem Dokument ist einen Windows-basierte Sturm HDInsight Cluster erforderlich. Weitere Informationen zur Verwendung von Linux-basierten Cluster [Bereitstellen und Verwalten von Apache Storm Topologien für Linux-basierte HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

##<a name="prerequisites"></a>Erforderliche Komponenten

* **Apache Storm auf HDInsight** - finden Sie unter <a href="../hdinsight-storm-getting-started/" target="_blank">Erste Schritte mit Apache auf HDInsight</a> Schritte zum Erstellen eines Clusters

* **Storm-Dashboard**: einen modernen Webbrowser, HTML5 unterstützt,

* Für **Visual Studio** - Azure SDK 2.5.1 oder höher und HDInsight Tools für Visual Studio. Finden Sie unter <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">Erste Schritte mit HDInsight-Tools für Visual Studio</a> installieren und Konfigurieren von HDInsight-Tools für Visual Studio.

    Einer der folgenden Versionen von Visual Studio:

    * Visual Studio 2012 <a href="http://www.microsoft.com/download/details.aspx?id=39305" target="_blank">Update 4</a>

    * Visual Studio 2013 <a href="http://www.microsoft.com/download/details.aspx?id=44921" target="_blank">Update 4</a> oder <a href="http://go.microsoft.com/fwlink/?LinkId=517284" target="_blank">Visual Studio 2013 Community</a>

    * <a href="http://visualstudio.com/downloads/visual-studio-2015-ctp-vs" target="_blank">Visual Studio 2015 CTP6</a>

    > [AZURE.NOTE] HDInsight-Tools für Visual Studio unterstützen derzeit nur Sturm HDInsight Cluster Version 3.2.

##<a name="storm-dashboard"></a>Storm-Dashboard

Storm-Dashboard ist eine Webseite im Sturm Cluster verfügbar. Die URL **https://&lt;Clustername >.azurehdinsight.net/**, wobei **Clustername** Name der Storm HDInsight Cluster.

Wählen Sie oben im Storm-Dashboard **Topologie übermitteln**. Gehen Sie der Seite eine Beispieltopologie ausführen zu laden und Ausführen eine Topologie, die Sie erstellt haben.

![der Topologie-Seite senden][storm-dashboard-submit]

###<a name="storm-ui"></a>Storm-Benutzeroberfläche

Storm-Dashboard klicken Sie auf **Storm-Benutzeroberfläche** . Informationen zum Cluster neben laufenden Topologien erscheint.

![Storm-Benutzeroberfläche][storm-dashboard-ui]

> [AZURE.NOTE] Mit einigen Versionen von Internet Explorer können Sie feststellen, dass die Storm-Benutzeroberfläche nicht aktualisiert wird, nachdem Sie zuerst besucht haben. Beispielsweise kann es keine neuen Topologien zeigen Sie, oder wenn Sie zuvor deaktiviert gegebenenfalls eine Topologie als aktiv angezeigt. Microsoft ist sich dieses Problems bewusst und arbeitet an einer Lösung.

####<a name="main-page"></a>Hauptseite

Die Hauptseite der Storm-Benutzeroberfläche enthält folgende Informationen:

* **Cluster-Zusammenfassung**: grundlegende Informationen zum Cluster Sturm.

* **Topologie-Zusammenfassung**: eine Liste der ausgeführten Topologien. Verwenden Sie die Links in diesem Abschnitt Weitere Informationen zu bestimmten Topologien.

* **Supervisor Zusammenfassung**: Informationen zum Vorgesetzten Sturm.

* **Nimbus-Konfiguration**: Nimbus-Konfiguration für den Cluster.

####<a name="topology-summary"></a>Topologie-Zusammenfassung

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

####<a name="spout-and-bolt-summary"></a>Auslauf und Mutterschrauben-Zusammenfassung

**Tüllen** oder **Schrauben** Abschnitte eine Schnauze auswählen zeigt die folgenden Informationen über das ausgewählte Element:

* **Komponente Zusammenfassung**: grundlegende Informationen zum Auslauf oder Bolzen.

* **Auslauf-Schraube Statistiken**: Statistiken über die Schnauze oder Bolzen. Verwenden Sie die Links in der Spalte **Fenster** Zeitrahmen für die verbleibenden Einträge auf der Seite festgelegt.

* **Eingabe-Statistiken** (nur Bolzen): Informationen der Bolzen belegten Eingabestreams.

* **Ausgabe Stats**: Informationen über Streams von diesem ausgegeben Auslauf oder Bolzen.

* **Executors**: Informationen über die Schnauze oder Bolzen. Wählen Sie den Eintrag **Port** für einen bestimmten Executor Anzeigen eines Protokolls der Diagnoseinformationen für diese Instanz hergestellt.

* **Fehler**: Fehlerinformationen für diese Auslauf oder Bolzen.

##<a name="hdinsight-tools-for-visual-studio"></a>HDInsight-Tools für Visual Studio

HDInsight-Tools lässt sich C# oder hybride Topologien Sturm Cluster senden. Die folgenden Schritte verwenden eine beispielanwendung. Informationen zum Erstellen eigener Topologies mit HDInsight-Tools finden Sie unter [Entwickeln von C# Topologien mit HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Gehen Sie folgendermaßen vor, ein Beispiel für die Sturm HDInsight Cluster bereitstellen anzeigen und Verwalten der Topologie.

1. Wenn Sie nicht die neueste Version der HDInsight-Tools für Visual Studio installiert haben, finden Sie unter <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">Erste Schritte mit HDInsight-Tools für Visual Studio</a>.

2. Öffnen Sie Visual Studio, wählen Sie **Datei** > **neu** > **Projekt**.

3. Erweitern Sie im Dialogfeld **Neues Projekt** **installiert** > **Vorlagen**, und wählen Sie dann **HDInsight**. Wählen Sie aus der Liste der Vorlagen **Storm-Beispiel**. Klicken Sie unten im Dialogfeld Namen Sie einen für die Anwendung.

    ![Bild](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

1. Im **Projektmappen-Explorer**mit der rechten Maustaste des Projekts, und wählen Sie **Absenden an auf HDInsight**.

    > [AZURE.NOTE] Wenn Sie aufgefordert werden, geben Sie die Anmeldeinformationen für Ihr Azure-Abonnement. Wenn Sie über mehrere Abonnements verfügen, melden Sie sich Ihre Sturm HDInsight Cluster enthält.

2. Wählen Sie Ihre Sturm HDInsight Cluster aus Liste **Storm-Cluster** , und wählen Sie **Senden**. Sie können überwachen, ob die Vorlage erfolgreich mit **Ausgabefenster** .

3. Wenn die Topologie erfolgreich übermittelt wurde, erscheinen **Sturm Topologien** für den Cluster. Wählen Sie aus der Liste Informationen zu laufenden Topologie die Topologie.

    ![Visual studiomonitor](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

    > [AZURE.NOTE] Auch **Sturm Topologien** im **Server-Explorer** angezeigt, **Azure**erweitern > **HDInsight**und mit der rechten Maustaste eines Sturms HDInsight Cluster und **Ansicht Sturm Topologien**auswählen.

    Symbol für Ausläufe oder Schrauben Informationen zu diesen Komponenten. Für jedes ausgewählte Element wird ein neues Fenster geöffnet.
    
    > [AZURE.NOTE] Der Name der Topologie ist der Klassenname der Topologie (in diesem Fall `HelloWord`,) mit einem Zeitstempel angehängt.

4. Wählen Sie Zusammenfassungsansicht **Topologie** die Topologie zu **Töten** .

    > [AZURE.NOTE] Storm-Topologien weiterhin ausgeführt, bis sie beendet oder Cluster gelöscht.

##<a name="rest-api"></a>REST-API

Storm-Benutzeroberfläche baut auf die REST-API wie Management und Überwachungsfunktionen mithilfe der REST-API ausführen können. Die REST-API können Sie um benutzerdefinierte Tools zur Verwaltung und Überwachung Sturm Topologien zu erstellen.

Weitere Informationen finden Sie unter [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). Folgendes ist die Verwendung von REST-API mit Apache auf HDInsight.

###<a name="base-uri"></a>Basis-URI

Der Basis-URI für die REST-API auf HDInsight-Cluster ist **https://&lt;Clustername >.azurehdinsight.net/stormui/api/v1/**, wobei **Clustername** Name der Storm HDInsight Cluster.

###<a name="authentication"></a>Authentifizierung

Die REST-API verwenden **Standardauthentifizierung**HDInsight Clusternamen und Kennwort verwenden.

> [AZURE.NOTE] Da Standardauthentifizierung unverschlüsselt gesendet wird, sollten Sie **immer** verwenden HTTPS zum Absichern der Kommunikation mit dem Cluster.

###<a name="return-values"></a>Rückgabewerte

REST-API zurückgegebenen Informationen möglicherweise nur innerhalb des Clusters oder virtuellen Computern auf demselben Azure Virtual Network-Cluster verwendet. Z. B. den vollqualifizierten Domänennamen (FQDN) für Zookeeper-Server über das Internet nicht zurückgegeben.

##<a name="next-steps"></a>Nächste Schritte

Da Sie gelernt haben, bereitstellen und überwachen Topologien Storm-Dashboard mit Informationen wie:

* [Entwickeln von C# Topologien mit HDInsight-Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Entwickeln Sie Java-basierte Topologien mit Maven](hdinsight-storm-develop-java-topology.md)

Finden Sie eine Liste mehrere Topologien [Topologien für auf HDInsight](hdinsight-storm-example-topology.md).

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
