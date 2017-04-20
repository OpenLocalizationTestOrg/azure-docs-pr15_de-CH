<properties
   pageTitle="Analysieren von Daten mit Apache Storm und HBase | Microsoft Azure"
   description="Erfahren Sie mehr über das Apache Storm mit einem virtuellen Netzwerk herstellen. Mit Storm HBase Sensor Daten aus einem Ereignis-Hub und mit D3.js darstellen."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analysieren von Daten mit Apache Storm Event Hub und HBase in HDInsight (Hadoop) 

Erfahren Sie, wie Apache Storm auf HDInsight Sensor Daten von Azure Event Hub verwenden, in Apache HBase auf HDInsight speichern und Visualisieren mithilfe von D3.js als Azure Web App ausgeführt.

In diesem Dokument verwendeten Azure-Ressourcen-Manager-Vorlage veranschaulicht mehrere Azure Ressourcen in einer Ressourcengruppe erstellen. Insbesondere wird ein virtuelles Netzwerk Azure, zwei HDInsight-Cluster (Sturm und HBase) und eine Azure Web App erstellt. Node.js-Implementierung des Echtzeit-Dashboards wird automatisch auf die Webanwendung bereitgestellt.

> [AZURE.NOTE] Die Informationen in diesem Dokument, das Beispiel wurden mit Linux-basierten HDInsight 3.3 und 3.4 Cluster-Versionen.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Abonnement. Finden Sie [kostenlose Testversion von Azure zu erhalten](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

    > [AZURE.IMPORTANT] Einen vorhandenen HDInsight-Cluster ist nicht erforderlich. die Schritte in diesem Dokument werden Folgendes erstellt:
    >
    > * Azure Virtual Network
    > * Storm HDInsight Cluster (Linux-basierten 2 workerknoten)
    > * HBase auf HDInsight Cluster (Linux-basierten 2 workerknoten)
    > * Ein Azure Web App, die das Cockpit hostet

* [Node.js](http://nodejs.org/): Hiermit wird das Cockpit lokal auf Umgebung Vorschau.

* [Java und JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Storm-Topologie entwickelt.

* [Maven](http://maven.apache.org/what-is-maven.html): zum Erstellen und kompilieren Sie das Projekt.

* [Git](http://git-scm.com/): verwendet, um das Projekt von GitHub herunterladen.

* __SSH__ -Client: Verbindung mit Linux-basierte HDInsight-Cluster verwendet. Weitere Informationen über SSH mit HDInsight finden Sie in folgenden Dokumenten.

    * [Verwenden von SSH mit HDInsight von einem Windows-client](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Verwenden von SSH mit HDInsight Linux, Unix oder Mac-Clients](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] Haben Sie auch Zugriff auf die `scp` Befehl zum Kopieren von Dateien zwischen lokalen Umgebung und HDInsight Cluster über SSH verwendet.

## <a name="architecture"></a>Architektur

![Architekturdiagramm](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

Dieses Beispiel besteht aus den folgenden Komponenten:

* **Azure Event Hubs**: Sensoren gesammelten Daten enthält. Für dieses Beispiel ist eine Anwendung, sofern die Daten generiert.

* **Auf HDInsight**: Event Hub Echtzeit-Verarbeitung von Daten bietet.

* **HBase auf HDInsight**: Daten nach der Verarbeitung Sturm permanenten NoSQL-Datenspeicher bereit.

* **Azure Virtual Network Service**: ermöglicht die sichere Kommunikation zwischen auf HDInsight und HBase auf HDInsight-Cluster.

    > [AZURE.NOTE] Ein virtuelles Netzwerk ist erforderlich, um die Java HBase-Client-API verwenden, nicht über das öffentliche Gateway für HBase Cluster verfügbar gemacht wird. Installation HBase und Storm-Cluster im gleichen virtuellen Netzwerk kann Cluster Sturm oder andere System im virtuellen Netzwerk direkt HBase mit Client-API.

* **Dashboard-Website**: ein Beispieldashboard, die Diagramme in Echtzeit.

    * Die Website ist in Node.js implementiert, sodass auf jedem Client-Betriebssystem für Tests ausführen oder auf Azure Websites bereitgestellt werden.

    * [Socket.IO](http://socket.io/) wird für Echtzeitkommunikation zwischen Storm-Topologie und der Website verwendet.

        > [AZURE.NOTE] Dies ist ein Implementierungsdetail. Sie können alle Kommunikationsframework SignalR oder raw WebSockets.

    * [D3.js](http://d3js.org/) dient zur Darstellung der Daten, die an die Website gesendet.

> [AZURE.IMPORTANT] Zwei Cluster müssen, gibt es keine unterstützte Methode zum Sturm und HBase einen HDInsight-Cluster erstellen.

Die Topologie Daten Ereignis Hub mithilfe der [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) -Klasse liest und schreibt Daten in HBase mithilfe der [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) -Klasse. Kommunikation mit der Website erfolgt mithilfe von [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Im folgenden finden ein Diagramm der Topologie.

![Topologiediagramm](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] Dies ist eine sehr vereinfachte Ansicht der Topologie. Zur Laufzeit wird eine Instanz jeder Komponente für jede Partition für den Event Hub erstellt, die gelesen wird. Diese Instanzen werden die Knoten im Cluster verteilt und Daten zwischen ihnen weitergeleitet werden wie folgt:
>
> * Daten aus der Schnauze Parser ist Lastenausgleich.
> * Daten vom Parser Dashboard und HBase werden von Geräte-ID gruppiert, aus demselben Gerät immer auf dieselbe Komponente übertragen.

### <a name="topology-components"></a>Topologiekomponenten

* **EventHub Schnauze**: Schnauze bereitgestellten als Teil der Apache Storm-Version 0.10.0 oder höher ist.

    > [AZURE.NOTE] In diesem Beispiel verwendete Ereignis Hubs Schnauze erfordert Sturm HDInsight Cluster Version 3.3 und 3.4. Informationen zur Verwendung von Event Hubs mit einer älteren Version auf HDInsight anzeigen Sie [Prozessereignisse von Azure Ereignis Hubs mit auf HDInsight](hdinsight-storm-develop-java-event-hub-topology.md)

* **ParserBolt.java**: die Daten, die von der Schnauze ausgegeben ist raw JSON und gelegentlich mehrere Ereignisse gleichzeitig ausgegeben. Diese Schraube veranschaulicht die Schnauze ausgegebenen Daten gelesen und als ein Tupel, mehrere Felder enthält, in einen neuen Stream auszugeben.

* **DashboardBolt.java**: Dies veranschaulicht, wie die Clientbibliothek Socket.io für Java auf Daten auf Web-Dashboard in Echtzeit senden.

In diesem Beispiel verwendet das Framework [Wärmefluss](https://storm.apache.org/releases/0.10.0/flux.html) , Topologie-Definition in YAML Dateien enthalten ist. Es gibt zwei:

* __keine hbase.yaml__ - diese Datei beim Testen der Topologie in Umgebung. Es verwenden nicht HBase Komponenten, da die HBase Java API von außerhalb des virtuellen Netzwerks zugreifen kann, die Cluster lebt.

* __mit hbase.yaml__ - diese Datei, wenn die Topologie Storm-Cluster bereitstellen. Es verwendet HBase Komponenten im gleichen virtuellen Netzwerk HBase Cluster läuft.

## <a name="prepare-your-environment"></a>Bereiten Sie Ihrer Umgebung vor

Bevor Sie dieses Beispiel verwenden möchten, legen Sie einen Hub Azure Ereignis der Storm-Topologie aus liest.

### <a name="configure-event-hub"></a>Konfigurieren von Event Hub

Event Hub ist die Datenquelle für dieses Beispiel. Gehen Sie folgendermaßen vor, einen neuen Ereignis Hub erstellen.

1. Wählen Sie aus dem [Azure-Portal](https://portal.azure.com) **+ neu** -> __Internet der Dinge__ -> __Ereignis Hubs__.

2. Führen Sie auf den __Namespace erstellen__ die folgenden Aufgaben:

    1. Geben Sie einen __Namen__ für den Namespace.
    2. Wählen Sie einen Tarif. __Grundlegende__ ist für dieses Beispiel ausreichend.
    3. Wählen Sie den Azure __Abonnement__ verwenden.
    4. Wählen Sie eine vorhandene Ressourcengruppe oder erstellen Sie eine neue.
    5. Wählen Sie den __Speicherort__ für den Ereignis-Hub.
    6. Wählen Sie __Dashboard__, und klicken Sie auf __Erstellen__.

3. Nach Abschluss des Erstellungsprozesses wird das Ereignis Hubs Blade für den Namespace. Hier wählen Sie __+ Add Event Hub__. Geben Sie auf dem Blatt __Erstellen Event Hub__ __Sensordata__ und dann __Erstellen__. Lassen Sie die anderen Felder die Standardwerte.

4. Wählen Sie Blatt Ereignis Hubs für den Namespace __Ereignis Hubs__. Wählen Sie den Eintrag __Sensordata__ .

5. Wählen Sie Blatt für Sensordata Event Hub __Shared-Richtlinien__. Mithilfe des Links __+ Hinzufügen__ fügen Sie die folgenden Richtlinien:


  	| Richtlinienname | Ansprüche |
  	| ----- | ----- |
  	| Geräte | Senden |
  	| Sturm | Überwachen |

5. Wählen Sie beide Richtlinien und __PRIMÄRSCHLÜSSEL__ -Wert. Sie benötigen den Wert für beide Richtlinien in späteren Schritten.

## <a name="download-and-configure-the-project"></a>Herunterladen Sie und konfigurieren Sie das Projekt

Anhand der folgenden GitHub Projekt herunterladen.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Nachdem der Befehl abgeschlossen ist, müssen Sie die folgende Verzeichnisstruktur:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] Dieses Dokument geht nicht auf alle Details der Code in diesem Beispiel. der Code ist jedoch vollständig kommentiert.

Öffnen Sie die Datei **hdinsight-eventhub-example/TemperatureMonitor/dev.properties** , und fügen Sie Ihr Event Hub Informationen die folgenden Zeilen hinzu:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] Es wird angenommen, dass Sie __Sturm__ als Namen für die Richtlinie verwendet eine Forderung __hören__ und Event-Hub __Sensordata__heißt.

 Speichern Sie die Datei, nachdem Sie diese Informationen hinzufügen.

## <a name="compile-and-test-locally"></a>Kompilieren und lokal testen

Vor dem Testen, müssen Sie das Dashboard, um die Ausgabe der Topologie anzeigen und Daten zum Speichern in Hub-Ereignis starten.

> [AZURE.IMPORTANT] HBase-Komponente dieser Topologie ist nicht aktiv, wenn Java API für den Cluster HBase lokal Testen von außerhalb des virtuellen Netzwerks Azure zugegriffen werden kann, die den Cluster enthält.

### <a name="start-the-web-application"></a>Starten Sie die Anwendung

1. Öffnen Sie ein neues Eingabeaufforderungsfenster oder Terminal ändern Sie Verzeichnisse **Hdinsight-Eventhub-Beispiel/Dashboard**, und verwenden Sie den folgenden Befehl von der Anwendung benötigten Abhängigkeiten installieren:

        npm install

2. Verwenden Sie den folgenden Befehl zum Starten der Anwendung:

        node server.js

    Sie sollten eine Meldung ähnlich der folgenden angezeigt:

        Server listening at port 3000

2. Öffnen Sie einen Webbrowser und geben **http://localhost: 3000 /** als Adresse. Sie sollte eine Seite ähnlich der folgenden angezeigt:

    ![Web-dashboard](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    Lassen Sie diese Befehlszeile oder Terminal geöffnet. Verwenden Sie nach dem Testen Strg-C den Webserver beendet.

### <a name="start-generating-data"></a>Starten Sie Generieren von Daten

> [AZURE.NOTE] Die Schritte in diesem Abschnitt verwenden Node.js, sodass sie auf allen Plattformen verwendet werden können. Andere Sprachbeispiele finden Sie unter **SendEvents** -Verzeichnis.

1. Öffnen Sie ein neues Eingabeaufforderungsfenster, Shell oder Terminal wechseln Sie **Hdinsight-Eventhub-Beispiel/SendEvents/Nodejs**, und verwenden Sie den folgenden Befehl von der Anwendung benötigten Abhängigkeiten installieren:

        npm install

2. Öffnen Sie **app.js** -Datei in einem Texteditor und die Ereignis-Hub Informationen, die Sie zuvor erworben:

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] Es wird angenommen, dass Sie __Sensordata__ als Namen für Ihr Ereignis Hub und __Geräte__ als Namen für die Richtlinie verwendet haben, die eine Forderung __Senden__ .

2. Verwenden Sie folgenden Befehl im Ereignis neue Einträge einfügen:

        node app.js

    Mehrere Zeilen der Ausgabe, die an Event Hub gesendeten Daten enthalten, sollte angezeigt werden. Diese werden ähnlich der folgenden angezeigt:

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>Starten der Topologie

2. Öffnen Sie ein neues Eingabeaufforderungsfenster, Shell oder Terminal und Änderung Verzeichnisse __Hdinsight-Eventhub-Beispiel/TemperatureMonitor__, und verwenden Sie den folgenden Befehl zum Starten der Topologie:

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Bei Verwendung von PowerShell stattdessen Sie Folgendes:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Wenn Sie unter Linux/Unix/OS X, und [Sturm Umgebung installiert](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html)haben, können Sie stattdessen die folgenden Befehle:
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    Die Topologie definiert in der Datei __keine hbase.yaml__ im lokalen Modus wird gestartet. Die Werte in der Datei __dev.properties__ bieten die Verbindungsinformationen für Event Hubs. Nach dem Start die Topologie liest Einträge Ereignis Hub und Dashboard auf Ihrem lokalen Computer sendet. Das Cockpit ähnlich der folgenden angezeigt Zeilen sollten angezeigt werden:

    ![mit Daten](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Und das Dashboard verwenden die `node app.js` Befehl aus den vorherigen Schritten Ereignis Hubs neue Daten an. Da die Temperaturwerte zufällig erzeugt werden, sollten im Diagramm zeigen große Temperaturschwankungen aktualisieren.

    > [AZURE.NOTE] Muss im Verzeichnis __Hdinsight-Eventhub-Beispiel/SendEvents/Nodejs__ bei Verwendung der `node app.js` Befehl.

3. Vergewissert haben, dass dies funktioniert, beenden Sie die Topologie mit STRG + C. STRG + C können auch zu den lokalen Webserver.

## <a name="create-a-storm-and-hbase-cluster"></a>Erstellen eines Clusters Sturm und HBase

Um die Topologie auf HDInsight ausgeführt und HBase Schraube aktivieren, müssen Sie eine neue Sturm und HBase Cluster erstellen. Die Schritte in diesem Abschnitt verwenden eine [Azure-Ressourcen-Manager-Vorlage](../resource-group-template-deploy.md) eine Azure Virtual Network und Sturm und HBase Cluster virtuellen Netzwerk neue. Die Vorlage erstellt eine Azure Web App und Bereitstellung eine Kopie des Dashboards.

> [AZURE.NOTE] Ein virtuelles Netzwerk wird verwendet, damit die Topologie auf Storm-Cluster HBase Cluster mit HBase Java API direkt kommunizieren kann.

In diesem Dokument verwendeten Ressourcen-Manager-Vorlage befindet sich ein Blob für den öffentlichen Container auf __https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__.

1. Klicken Sie auf die folgende Schaltfläche, um in Azure anmelden und öffnen die Vorlage Ressourcenmanager in Azure-Portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Blatt **Parameter** Geben Sie Folgendes ein:

    ![HDInsight Parameter](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: Dieser Wert wird als Basisname der Sturm verwendet und HBase-Cluster. Z. B. erstellt __Hdi__ eingeben ein Sturm namens __Storm-Hdi__ und ein HBase Cluster mit dem Namen __Hbase-Hdi__.
    * __CLUSTERLOGINUSERNAME__: den Benutzernamen Administrator für Cluster Sturm und HBase.
    * __CLUSTERLOGINPASSWORD__: Benutzer Administratorkennwort für Cluster Sturm und HBase.
    * __SSHUSERNAME__: die SSH-Benutzer für den Cluster Sturm und HBase erstellen.
    * __SSHPASSWORD__: das Kennwort für den Benutzer SSH für Cluster Sturm und HBase.
    * __Ort__: Region in Clustern erstellt werden.
    
    Klicken Sie auf __OK__ , um die Parameter zu speichern.
    
3. Verwenden Sie Bereich __Ressourcengruppe__ eine neue Ressourcengruppe erstellen oder eine vorhandene auswählen.

4. Wählen Sie im Dropdownmenü __Gruppe Ort__ am gleichen Speicherort wie den __Position__ -Parameter.

5. Wählen Sie __rechtlich__und dann __Erstellen__.

6. Abschließend überprüfen Sie __Pin Dashboard__ und dann __Erstellen__. Ungefähr 20 Minuten dauert die Cluster erstellen.

Sobald die Ressourcen erstellt haben, werden Sie Blatt für die Ressourcengruppe weitergeleitet mit den Clustern und Cockpit.

![Blatt für die Vnet und Ressourcen](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] Werden die Namen der HDInsight-Cluster __Sturm BASENAME__ und __BASENAME Hbase__, wobei BASISNAME der Name der Vorlage bereitgestellt. Diesen Namen benötigen später bei der Verbindung mit dem Cluster. Beachten Sie, dass der Name der Dashboardsite __Basename-Dashboard__. Sie werden diese später verwenden, wenn Dashboard anzeigen.

## <a name="configure-the-dashboard-bolt"></a>Konfigurieren des Bolzens Dashboard

Um das Dashboard bereitgestellt als Web-app Daten senden, müssen Sie die folgende Zeile in der Datei __dev.properties__ ändern:

    dashboard.uri: http://localhost:3000

Ändern `http://localhost:3000` , `http://BASENAME-dashboard.azurewebsites.net` und speichern Sie die Datei. Ersetzen Sie durch den im vorherigen Schritt eingegebenen Namen __BASENAME__ . Sie können auch die Ressourcengruppe zuvor erstellte Dashboard auswählen und Anzeigen der URL.

## <a name="create-the-hbase-table"></a>Erstellen Sie die HBase-Tabelle

Speichern von Daten in HBase müssen wir zunächst eine Tabelle erstellen. Im Allgemeinen soll Ressourcen vor, die Sturm schreiben, zu erstellen Ressourcen innerhalb einer Storm-Topologie, verteilte Kopien des Codes versuchen, dieselbe Ressource erstellen zu können. Ressourcen außerhalb der Topologie erstellen und das Sturm für Lesen und Schreiben und Analysen verwenden.

1. SSH Verbindung mit HBase SSH Benutzer- und zur Vorlage bei der Clustererstellung angegebene Kennwort verwenden. Beispielsweise verwenden die `ssh` Befehl, verwenden Sie die folgende Syntax:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    In diesem Befehl SSH-Benutzername beim Erstellen von Cluster und __BASENAME__ mit dem Basisnamen eingegebenen eingegebenen ersetzen Sie __Benutzernamen__ . Wenn Sie aufgefordert werden, geben Sie das Kennwort für Benutzer SSH.

2. Die SSH-Sitzung starten der Shell HBase.

        hbase shell
    
    Wenn die Shell geladen wurde, sehen Sie eine `hbase(main):001:0>` aufgefordert.

3. Geben Sie den folgenden Befehl zum Erstellen einer Tabelle zum Speichern der Daten aus der HBase-Shell:

        create 'SensorData', 'cf'

4. Stellen Sie sicher, dass die Tabelle erstellt wurde, mithilfe des folgenden Befehls:

        scan 'SensorData'
        
    Dies gibt Informationen ähnlich dem folgenden Beispiel, dass 0 Zeilen in der Tabelle vorhanden sind.
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Geben Sie Folgendes ein, um die HBase Shell beenden:

        exit

## <a name="configure-the-hbase-bolt"></a>Konfigurieren Sie die HBase Schraube

Um HBase von Storm-Cluster schreiben, müssen Sie HBase Schraube Variantendetails HBase Cluster bereitstellen. Die einfachste Möglichkeit hierzu ist zu __Hbase site.xml__ aus dem Cluster in Ihr Projekt aufnehmen. Sie müssen auch mehrere Abhängigkeiten in der Datei __pom.xml__ kommentieren die Sturm Hbase Komponente und erforderliche Abhängigkeit laden.

> [AZURE.IMPORTANT] Sie müssen auch die Datei Storm-hbase.jar auf Ihrem HDInsight Cluster 3.3 und 3.4 Cluster herunterladen; Diese Version wird mit HBase kompiliert 1.1.x, die für HBase HDInsight 3.3 und 3.4 Cluster verwendet wird. Storm-Hbase Komponente an anderer Stelle verwenden, können gegen eine ältere Version von HBase kompiliert werden.

### <a name="download-the-hbase-sitexml"></a>Hbase-site.xml herunterladen

Verwenden Sie von einer Befehlszeile aus SCP zum Herunterladen der Datei __Hbase site.xml__ aus dem Cluster. Im folgenden Beispiel SSH-Benutzer beim Erstellen von Cluster und __BASENAME__ mit diesem Namen bereits eingegebenen eingegebenen ersetzen Sie __Benutzernamen__ . Wenn Sie aufgefordert werden, geben Sie das Kennwort für Benutzer SSH. Ersetzen Sie die `/path/to/TemperatureMonitor/resources/hbase-site.xml` durch den Pfad zu dieser Datei in das Projekt TemperatureMonitor.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

Diese downloadet __Hbase site.xml__ angegebenen Pfad.

### <a name="download-and-install-the-storm-hbase-component"></a>Storm-Hbase Komponente herunterladen und installieren

1. Eine Befehlszeile verwenden Sie SCP zum Herunterladen der Datei __Storm-hbase.jar__ von Storm-Cluster. Im folgenden Beispiel SSH-Benutzer beim Erstellen von Cluster und __BASENAME__ mit diesem Namen bereits eingegebenen eingegebenen ersetzen Sie __Benutzernamen__ . Wenn Sie aufgefordert werden, geben Sie das Kennwort für Benutzer SSH.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    Diese downloadet eine Datei namens `storm-hbase-####.jar`, wobei ### ist die Versionsnummer für diesen Cluster Sturm. Notieren Sie sich diese Nummer später verwendet wird.

2. Verwenden Sie den folgenden Befehl zum Installieren dieser Komponente in lokalen Maven Repository Umgebung. Maven, die beim Kompilieren des Projekts finden können. Ersetzen Sie __####__ mit der Versionsnummer im Dateinamen enthalten.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Wenn Sie PowerShell verwenden, verwenden Sie den folgenden Befehl:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>Aktivieren Sie Storm-Hbase Komponente im Projekt

1. Öffnen Sie die Datei __TemperatureMonitor/pom.xml__ , und löschen Sie die folgenden Zeilen:

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Nur löschen Sie diese beiden Zeilen. Löschen Sie keine Zeilen zwischen ihnen.
    
    Dadurch können mehrere Komponenten, die bei der Kommunikation mit HBase mit Hbase-Schraube benötigt werden.

2. Suchen Sie die folgenden Zeilen, und Ersetzen Sie __####__ mit der Versionsnummer der Storm Hbase Datei bereits heruntergeladen.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] Die Versionsnummer muss die Version Maven dazu verwendet, die die Komponente beim Erstellen des Projekts bei der Installation der Komponente im lokalen Repository Maven verwendet.

2. Speichern Sie die Datei __pom.xml__ .

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Paket erstellen und Bereitstellen der Lösung auf HDInsight

Gehen Sie in Ihrer Entwicklungsumgebung Storm-Topologie Storm-Cluster bereitstellen.

1. Verwenden Sie aus dem Verzeichnis __TemperatureMonitor__ den folgenden Befehl zu einem neuen Build JAR-Paket aus dem Projekt erstellen:

        mvn clean compile package

    Dies erstellt eine Datei mit dem Namen **TemperatureMonitor-1.0-SNAPSHOT.jar** im **Zielverzeichnis des Projekts** .

2. Mit der __TemperatureMonitor-1.0-SNAPSHOT.jar__ -Datei zum Cluster Sturm hochladen scp. Im folgenden Beispiel SSH-Benutzer beim Erstellen von Cluster und __BASENAME__ mit diesem Namen bereits eingegebenen eingegebenen ersetzen Sie __Benutzernamen__ . Wenn Sie aufgefordert werden, geben Sie das Kennwort für Benutzer SSH.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] Die Datei hochzuladen kann dauern mehrere Megabyte groß werden.

    Mit der scp upload der Datei __dev.properties__ als Ereignis Hubs und das Dashboard verwendeten Informationen enthält.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. Sobald die Dateien hochgeladen haben, verbinden Sie mit SSH.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. Verwenden Sie über SSH-Sitzung folgenden Befehl an der Topologie.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Die Topologie mit Topology Definition in der Datei __mit hbase.yaml__ und die Werte in der Datei __dev.properties__ wird gestartet.

3. Nach dem Start der Topologie öffnen Sie einen Browser auf der Website auf Azure veröffentlicht, verwenden die `node app.js` Befehl zum Senden von Daten an Event Hub. Aktualisiert die Informationen Cockpit sollte angezeigt werden.

    ![Dashboard](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>HBase Daten

Nachdem Sie Daten mit der Topologie übermittelt haben `node app.js`, gehen Sie zu HBase und überprüfen, ob die Daten in die Tabelle zuvor erstellten geschrieben.

1. SSH Verbindung zum Cluster HBase verwenden.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. Die SSH-Sitzung starten der Shell HBase.

        hbase shell
    
    Wenn die Shell geladen wurde, sehen Sie eine `hbase(main):001:0>` aufgefordert.

2. Anzeigen von Zeilen aus der Tabelle:

        scan 'SensorData'
        
    Diese sollten Informationen ähnlich der folgenden zurück, der angibt, dass es 0 Zeilen in der Tabelle.
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] Scan-Vorgang wird nur maximal 10 Zeilen aus der Tabelle zurück.

## <a name="delete-your-clusters"></a>Cluster löschen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Um Cluster, Speicher und WebApp gleichzeitig zu löschen, löschen Sie die Ressourcengruppe, die sie enthält.

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie mit Sturm Auslesen von Ereignis in HBase speichern und Visualisieren Informationen auf externen Dashboard mit Socket.io und D3.js.

* Weitere Beispiele für Topologien mit HDInsight Sturm finden Sie unter:

    * [Topologien für auf HDInsight](hdinsight-storm-example-topology.md)

* Weitere Informationen zu Apache Storm finden Sie unter [Apache Storm](https://storm.incubator.apache.org/) -Website.

* Weitere Informationen über HBase HDInsight [HBase HDInsight Überblick](hdinsight-hbase-overview.md)anzeigen

* Weitere Informationen über Socket.io anzeigen Sie [socket.io](http://socket.io/) Website

* Weitere Informationen zu D3.js finden Sie unter [D3.js - datengesteuerte Dokumente](http://d3js.org/).

* Informationen zum Erstellen von Topologien in Java finden Sie unter [Java entwickeln Topologien für Apache Storm auf HDInsight](hdinsight-storm-develop-java-topology.md).

* Informationen zum Erstellen von Topologien in .NET finden Sie unter [Entwickeln von C# Topologien für Apache Storm auf HDInsight mit Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
