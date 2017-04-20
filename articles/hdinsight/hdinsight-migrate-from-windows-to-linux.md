<properties
pageTitle="Migration von Windows-basierten HDInsight auf Linux-basierten HDInsight | Azure"
description="Informationen Sie zum Migrieren von einem Windows-basierten HDInsight Cluster auf einem Linux-basierten HDInsight Cluster."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migrieren Sie aus einem Windows-basierten HDInsight-Cluster auf einem Linux-basierten cluster

Während der Windows-basierten HDInsight Hadoop in der Cloud verwenden kann, können Sie feststellen, dass Sie Linux-basierten Cluster nutzen für Ihre Lösung empfehlen. Viele Dinge im Ökosystem Hadoop auf Linux-Systeme entwickelt und einige möglicherweise nicht mit Windows-basierten HDInsight. Außerdem angenommen viele Bücher, Videos und andere Schulungsmaterialien, einem linuxsystem verwendeten Hadoop arbeiten.

Dieses Dokument enthält Informationen zu den Unterschieden zwischen HDInsight unter Windows und Linux und wie vorhandene Arbeitslasten auf Linux-basierten Cluster migrieren.

> [AZURE.NOTE] HDInsight-Cluster verwenden Ubuntu langfristigen Support (LTS) als Betriebssystem für die Knoten im Cluster. Informationen zur Version von Ubuntu mit HDInsight, zusammen mit anderen Komponenten Versionsinformationen finden Sie unter [HDInsight Versionen](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Schritte für die Migration

Der allgemeine Arbeitsablauf für Migration lautet wie folgt:

![Migration Workflowdiagramm](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Lesen Sie jeden Abschnitt dieses Dokuments, zu verstehen, die bei der Migration von vorhandenen Workflow, Projekte usw. auf Linux-basierten Cluster erforderlich sein kann.

2.  Erstellen Sie einen Linux-basierten Cluster als Test-Quality Assurance Umgebung. Weitere Informationen zum Erstellen eines Linux-basierten Clusters finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3.  Kopieren Sie Arbeitsplätze, Datenquellen und Empfängern in die neue Umgebung. Die Daten in Umgebung im Abschnitt Weitere Informationen anzeigen

4.  Führen Sie die Überprüfung, um sicherzustellen, dass die Aufträge erwartungsgemäß auf den neuen Cluster.

Nachdem Sie sichergestellt haben, dass alles erwartungsgemäß funktioniert, Planen Sie die Ausfallzeit für die Migration. Während dieser Zeit werden Sie die folgenden Aktionen.

1.  Sichern Sie vorübergehenden lokal auf den Clusterknoten gespeicherten Daten. Wenn beispielsweise Daten direkt auf Head-Knoten gespeichert sind.

2.  Löschen des Windows-basierten Clusters.

3.  Erstellen eines Linux-basierten Clusters mit denselben Standarddatenspeicher, den Windows-basierten Cluster verwendet. Dadurch wird den neuen Cluster mit Ihrer vorhandenen Daten weiterarbeiten.

4.  Importieren Sie vorübergehenden gesicherten Daten.

5.  Start Aufträge/weiter Verarbeitung mit den neuen Cluster.

### <a name="copy-data-to-the-test-environment"></a>Kopieren von Daten in der Umgebung

Es gibt viele Methoden zum Kopieren von Daten und Aufträge, Verschieben von Dateien jedoch in diesem Abschnitt erläutert am einfachsten Methoden direkt beides Test-Cluster.

#### <a name="hdfs-dfs-copy"></a>HDFS DFS-Kopie

Hadoop HDFS Befehl können direkt Daten vom Speicher für Ihre vorhandenen Produktionscluster auf den Speicher für einen neuen Testcluster mithilfe der folgenden Schritte kopieren.

1. Informationen Sie Speicher Konto und Standard-Container für den vorhandenen Cluster. Dazu können Sie folgende Azure PowerShell-Skript verwenden.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Folgen Sie den Schritten erstellen Linux-basierten Clustern im HDInsight-Dokument zum Erstellen einer neuen Umgebung. Vor dem Erstellen des Clusters beenden und **Optionale Konfiguration**gewählt.

3. Wählen Sie das Blade optionale Konfiguration **Verknüpfte Speicherkonten**.

4. Wählen Sie **Hinzufügen einer Speicherschlüssel**und Aufforderung das Speicherkonto von PowerShell-Skript in Schritt 1 zurückgegeben wurde. Klicken Sie auf jedes schließen **Wählen** . Erstellen Sie den Cluster.

5. Nach Erstellung des Clusters Verbinden mit **SSH.** Wenn Sie nicht mit SSH mit HDInsight vertraut sind, finden Sie in den folgenden Artikeln.

    * [Verwenden von SSH mit Linux-basierte HDInsight von Windows-clients](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Verwenden von SSH mit Linux-basierte HDInsight von Linux, Unix und Mac-clients](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Über SSH-Sitzung Befehl folgende Dateien vom Speicherkonto verknüpfte auf neue Standard-Speicherkonto kopieren. Ersetzen Sie CONTAINER und Konto mit Container von PowerShell-Skript in Schritt 1 zurückgegebenen Informationen. Ersetzen Sie den Pfad zu Daten durch den Pfad zu einer Datei.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Wenn die Verzeichnisstruktur mit den Daten nicht in der Umgebung vorhanden ist, können Sie mit dem folgenden Befehl erstellen.

        hdfs dfs -mkdir -p /new/path/to/create

    Die `-p` Switch ermöglicht die Erstellung aller Verzeichnisse im Pfad.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Direkte Kopie zwischen Azure Storage blobs

Sie möchten auch mithilfe der `Start-AzureStorageBlobCopy` Azure PowerShell-Cmdlet Blobs zwischen Speicherkonten außerhalb HDInsight kopieren. Weitere Informationen finden Sie im Abschnitt Azure-Blobs mithilfe von Azure PowerShell mit Azure-Speicher verwalten.

##<a name="client-side-technologies"></a>Clientseitige Technologie

Im Allgemeinen werden clientseitige Technologien wie [Azure PowerShell-Cmdlets](../powershell-install-configure.md), [Azure-CLI](../xplat-cli-install.md) oder [.NET SDK für Hadoop](https://hadoopsdk.codeplex.com/) weiterhin mit Linux-basierten Clustern wie sie zu anderen APIs, die in beiden Clustertypen OS ist.

##<a name="server-side-technologies"></a>Serverseitige Technologie

Die folgende Tabelle enthält Hinweise auf Migration von serverseitigen Komponenten bestimmte Windows.

| Wenn Sie diese Technologie verwenden. | Auszuführende Aktion |
| ----- | ----- |
| **PowerShell** (serverseitige Skripts, einschließlich Skript während der Erstellung des Clusters verwendet) | Bash-Skripten ändern. Skript-Aktionen finden Sie unter [anpassen, Linux-basierte HDInsight mit Skripts](hdinsight-hadoop-customize-cluster-linux.md) und [Skript Aktion Entwicklung für Linux-basierte HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI** (serverseitige Skripts) | Während der Azure-CLI auf Linux ist kommt es nicht vorinstallierte HDInsight Cluster-Head-Knoten. Bedarf für serverseitiges Skripting, finden Sie Informationen zur Installation auf Linux-Plattformen [der Azure-CLI installieren](../xplat-cli-install.md) . |
| **.NET Komponenten** | .NET ist für Linux-basierte HDInsight-Cluster nicht unterstützt. Linux-basierte Sturm auf HDInsight Cluster nach 10/28/2017 unterstützt C# Sturm Topologien mit SCP.NET Framework erstellt. Zusätzliche Unterstützung für .NET wird in zukünftigen Updates hinzugefügt. |
| **Win32-Komponenten oder andere Windows-Technologie** | Anleitung hängt von der Komponente oder Technologie. Möglicherweise können Sie eine Version, die mit Linux kompatibel ist oder möglicherweise eine alternative Lösung suchen oder diese Komponente schreiben. |

##<a name="cluster-creation"></a>Erstellung des Clusters

Dieser Abschnitt enthält Informationen zu Unterschieden bei der Erstellung des Clusters.

### <a name="ssh-user"></a>SSH Benutzer

Linux-basierte HDInsight Cluster verwenden das Protokoll **Secure Shell (SSH)** remote Zugriff auf den Clusterknoten. Im Gegensatz zum Remote Desktop für Windows-basierten Clustern die meisten SSH-Clients bieten keine GUI-Erfahrung, sondern bietet eine Befehlszeile, mit der Sie Befehle im Cluster ausführen können. Manche Clients (z. B. [MobaXterm](http://mobaxterm.mobatek.net/),) bieten grafisch Datei System-Browser neben einer Fernbedienung Befehlszeilen.

Während der Clustererstellung erforderlich eine SSH-Benutzer und ein **Kennwort** oder **öffentliches Schlüsselzertifikat** für die Authentifizierung.

Wir empfehlen öffentliches Schlüsselzertifikat ist sicherer als die Verwendung eines Kennworts. Zertifikatauthentifizierung wird eine signierte öffentliche/Private Schlüsselpaar generieren und den öffentlichen Schlüssel bereitstellen, wenn den Cluster erstellen. Beim Verbinden mit dem Server über SSH enthält der private Schlüssel auf dem Client für die Verbindung.

Weitere Informationen über SSH mit HDInsight finden Sie unter:

- [Verwenden von SSH mit HDInsight von Windows-clients](hdinsight-hadoop-linux-use-ssh-windows.md)

- [Verwenden von SSH mit HDInsight von Linux, Unix und Mac OS-clients](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Cluster-Anpassung

**Skript-Aktionen** mit Linux-basierten Clustern müssen in Bash-Skript geschrieben. Während bei der Clustererstellung Skriptaktionen verwendet werden kann, können für Linux-basierten Clustern sie auch zur Anpassung durchzuführen, nachdem ein Cluster ist und ausgeführt werden. Weitere Informationen finden Sie unter [Anpassen von Linux-basierten HDInsight mit Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md) und [Skript Aktion Entwicklung für Linux-basierte HDInsight](hdinsight-hadoop-script-actions-linux.md).

Anpassung ist **bootstrap**. Für Windows-Clustern können Sie den Speicherort der zusätzliche Bibliotheken mit Struktur anzugeben. Nach Erstellung des Clusters Bibliotheken stehen automatisch mit Struktur Abfragen ohne verwenden `ADD JAR`.

Bootstrap für Linux-basierten Clustern ist diese Funktionalität nicht bieten. Verwenden Sie stattdessen Skriptaktion in [Bibliotheken während der Erstellung des Clusters hinzufügen Struktur](hdinsight-hadoop-add-hive-libraries.md)dokumentiert.

### <a name="virtual-networks"></a>Virtuelle Netzwerke

Windows-basierte HDInsight Cluster funktionieren nur mit klassischen virtuelle Netzwerke HDInsight Linux-basierten Cluster Resource Manager virtuelle Netzwerke erfordern. Haben Sie Ressourcen in einem klassischen virtuellen Netzwerk, die Linux-HDInsight-Cluster herstellen muss, finden Sie unter [Verbinden einer klassischen virtuelles Netzwerk zu einem virtuellen Netzwerk Ressourcen-Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Weitere Informationen zu Konfiguration HDInsight Azure virtuelle Netzwerke mit finden Sie unter [Erweitern HDInsight Funktionen über ein virtuelles Netzwerk](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Management und Überwachung

Viele Web-Benutzeroberflächen, die Sie mit Windows-basierten HDInsight Auftragsverlauf oder Garn Benutzeroberfläche verwendet haben sind Ambari erhältlich. Darüber hinaus ermöglicht das Ambari Struktur Ansicht Abfragen Struktur Webbrowser. Ambari Web-Benutzeroberfläche ist auf Linux-basierten Clustern unter https://CLUSTERNAME.azurehdinsight.net verfügbar.

Weitere Informationen zum Arbeiten mit Ambari finden Sie in folgenden Dokumenten:

- [Ambari Web](hdinsight-hadoop-manage-ambari.md)

- [Ambari-REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari-Alarme

Ambari ist ein Warnsystem, das Sie Probleme mit dem Cluster erkennen kann. Alarme werden rote oder gelbe Ambari Web-Benutzeroberfläche jedoch Sie auch über die REST-API abgerufen werden können.

> [AZURE.IMPORTANT] Ambari Warnungen angeben, *kann* ein Problem nicht, es *ist* ein Problem. Beispielsweise erhalten Sie eine Warnung über HiveServer2 zugegriffen werden kann, obwohl Sie normalerweise zugreifen können.
>
> Zahlreiche Warnungen als Intervall-Abfragen an einen Dienst implementiert und eine Antwort innerhalb eines bestimmten Zeitraums. Damit die Warnung zwangsläufig, dass der Dienst ausgefallen ist, Ergebnissen nicht nur, dass es innerhalb des erwarteten Zeitraums.

Im Allgemeinen sollten Sie überprüfen, ob eine Warnung über einen längeren Zeitraum ausgeführt wurde oder Probleme, die zuvor mit dem Cluster gemeldet wurden vor auf spiegelt.

##<a name="file-system-locations"></a>Dateisystem-Speicherorte

Anders als Windows-basierte HDInsight-Cluster ist Linux Cluster-Dateisystem angelegt. Verwenden Sie in der folgenden Tabelle, um häufig verwendete Dateien.

| Sie müssen finden... | Es ist... |
| ----- | ----- |
| Konfiguration | `/etc`. Zum Beispiel`/etc/hadoop/conf/core-site.xml` |
| Protokolldateien | `/var/logs` |
| Hortonworks Plattform (HDP) | `/usr/hdp`. Es gibt zwei Verzeichnisse, eine aktuelle HDP Version (z. B. `2.2.9.1-1`,) und `current`. Die `current` Directory symbolische Links zu Dateien und Verzeichnisse im Verzeichnisses Version und so bequem HDP Dateizugriff seit der Version Anzahl ändert HDP Version aktualisiert wird. |
| Hadoop streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Im Allgemeinen sollten Sie den Namen der Datei können den folgenden Befehl aus einer SSH-Sitzung Sie den Pfad finden:

    find / -name FILENAME 2>/dev/null

Sie können auch Platzhalter mit dem Namen verwenden. Beispielsweise `find / -name *streaming*.jar 2>/dev/null` gibt den Pfad auf JAR-Dateien, in denen das Wort "streaming" als Teil des Dateinamens zurück.

##<a name="hive-pig-and-mapreduce"></a>Struktur und Schwein MapReduce

Schweine und MapReduce Arbeitslasten ähneln auf Linux-basierten Clustern – der Hauptunterschied ist Remotedesktop Verbindung zu einem Windows-basierten Cluster verwenden und Ausführen von Aufträgen verwenden Sie SSH mit Linux-basierten Clustern.

- [Schwein mit SSH verwenden](hdinsight-hadoop-use-pig-ssh.md)

- [Verwenden Sie MapReduce mit SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Struktur

Das folgende Diagramm enthält Richtlinien zum Migrieren der Struktur Arbeitslasten.

| Auf Windows basierende ich Benutze... | Auf Linux-basierten... |
| ----- | ----- |
| **Struktur-Editor** | [Struktur auf Ambari](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Tez aktivieren | Tez ist das Ausführungsmodul Standard für Linux-basierten Clustern, die Set-Anweisung nicht mehr benötigt wird. |
| CMD-Dateien oder Skripts auf dem Server als Bestandteil einer Struktur | Verwenden von Skripts für Bash |
| `hive`Befehl von Remotedesktop | Verwenden Sie [Beeline](hdinsight-hadoop-use-hive-beeline.md) oder [Struktur aus einer SSH-Sitzung](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Sturm

| Auf Windows basierende ich Benutze... | Auf Linux-basierten... |
| ----- | ----- |
| Storm-Dashboard | Storm-Dashboard ist nicht verfügbar. Finden Sie unter [Bereitstellen und Verwalten von Topologien für Linux-basierte HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) wegen Topologien senden |
| Storm-Benutzeroberfläche | Storm-Benutzeroberfläche steht unter https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio zum Erstellen, bereitstellen und Verwalten von C# oder Hybrid-Topologien | Visual Studio kann zum Erstellen, bereitstellen und Verwalten von C# (SCP.NET) oder Hybridtopologien auf HDInsight Cluster erstellt nach 10/28/2017 auf Linux-basierten verwendet werden. |

##<a name="hbase"></a>HBase

Auf Linux-basierten Clustern ist das übergeordnete cpmwelt.de HBase `/hbase-unsecure`. Sie müssen dies in der Konfiguration für Java-Clients festgelegt, die systemeigene HBase Java API verwenden.

Finden Sie ein Beispiel für einen Client, der diesen Wert legt [eine HBase Java-basierte Anwendung](hdinsight-hbase-build-java-maven.md) .

##<a name="spark"></a>Funken

Spark-Cluster wurden während der Vorschau auf Windows-Cluster; Version ist Spark jedoch nur mit Linux-basierten Clustern. Es ist keine Migrationspfad von einem Windows-basierten Spark Vorschau Cluster zu einem Release Spark Linux-basierten Cluster.

##<a name="known-issues"></a>Bekannte Probleme

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory benutzerdefinierte .NET Aktivitäten

Azure Data Factory benutzerdefinierte .NET Aktivitäten werden auf HDInsight Linux-basierten Clustern derzeit nicht unterstützt. Stattdessen sollten Sie eine der folgenden Methoden implementieren benutzerdefinierte Aktivitäten als Teil der ADF-Pipeline verwenden.

-   .NET Azure Batch-Pool ausführen. Abschnitt Azure Stapel verknüpft Service [benutzerdefinierte Aktivitäten in Azure Data Factory-pipeline](../data-factory/data-factory-use-custom-activities.md#AzureBatch)

-   Implementieren Sie die Aktivität als MapReduce Aktivität. Weitere Informationen finden Sie unter [Data Factory MapReduce Programme aufrufen](../data-factory/data-factory-map-reduce.md) .

### <a name="line-endings"></a>Zeilenenden

Im Allgemeinen verwenden Zeilenenden auf Windows-basierten Systemen CRLF, und Linux-Systeme LF. Erzeugen oder erwarten, Daten mit CRLF Zeilenenden müssen Sie die Erzeuger oder Verbraucher mit LF Zeile ändern.

Beispielsweise wird mithilfe von Azure PowerShell Abfrage HDInsight auf einem Windows-basierten Cluster mit CRLF zurück. Dieselbe Abfrage mit einem Linux-basierten Cluster gibt LF zurück. In vielen Fällen ist dies jedoch vor der Migration zu Linux-basierten Cluster untersucht werden sollte an den Datenconsumer unerheblich.

Haben Sie Skripts, die auf Linux-Cluster-Knoten (z. B. eine Struktur oder einen MapReduce-Auftrag verwendet Python-Skript) ausgeführt wird, verwenden Sie immer LF als Zeile endet. Verwenden Sie CRLF kann Fehler angezeigt, wenn die Skripts auf einem Linux-basierten Cluster ausgeführt.

Wenn Sie wissen, dass die Skripts enthalten keine Zeichenfolgen mit eingebetteten Zeilenumbruchzeichen, Massenkopieren ändern Sie die Zeilenenden mit einer der folgenden Methoden:

-   **Bei Skripts, die Sie zum Cluster hochladen möchten**, verwenden Sie PowerShell Aussagen die Zeilenenden von CRLF, LF ändern, bevor das Skript zum Cluster hochladen.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Haben Sie Skripts, die bereits im Speicher vom Cluster verwendet werden**, den folgenden Befehl zum Linux-basierten Cluster aus einer SSH-Sitzung können Sie das Skript ändern.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Nächste Schritte

-   [Erstellen Sie Linux-basierte HDInsight-Cluster](hdinsight-hadoop-provision-linux-clusters.md)

-   [Verbinden Sie mit einem Linux-basierten Cluster über SSH aus einem Windows-client](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Verbinden Sie mit einem Linux-basierten Cluster über SSH Linux, Unix oder Mac-Clients](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Verwalten eines Linux-basierten Clusters mit Ambari](hdinsight-hadoop-manage-ambari.md)
