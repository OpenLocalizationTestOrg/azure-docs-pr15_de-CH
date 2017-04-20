<properties
    pageTitle="Skript für Aktion-Entwicklung mit HDInsight | Microsoft Azure"
    description="Weitere Informationen zum Anpassen von Hadoop Cluster mit Skript. Skriptaktion kann zusätzlichen Software auf einem Hadoop Cluster installieren oder zum Ändern der Konfiguration auf einem Cluster verwendet werden."
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

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Entwickeln von Skripts für HDInsight Windows-basierten Clustern Skriptaktion

Informationen Sie zum Skriptaktion Skripten für HDInsight. Informationen über Skriptaktion Skripts finden Sie unter [Anpassen HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster.md). Der gleiche Artikel HDInsight Linux-basierten Clustern finden Sie unter [entwickeln Skriptaktion Skripts für HDInsight](hdinsight-hadoop-script-actions-linux.md).

> [AZURE.NOTE] Die Informationen in diesem Dokument ist spezifisch für Windows-basierte HDInsight-Cluster. Informationen mit Windows-basierten Clustern Skriptaktionen finden Sie unter [Skript Aktion Entwicklung mit HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).


Skriptaktion kann zusätzlichen Software auf einem Hadoop Cluster installieren oder zum Ändern der Konfiguration auf einem Cluster verwendet werden. Skript-Aktionen sind Skripts, die auf den Clusterknoten ausführen, wenn HDInsight-Cluster bereitgestellt werden, und sie nach Knoten im Cluster HDInsight Konfiguration ausgeführt. Eine Skriptaktion unter System Konto Administratorrechte ausgeführt und bietet volle Zugriffsrechte auf den Clusterknoten. Jeder Cluster kann bereitgestellt werden, mit einer Liste von Skriptaktionen in der Reihenfolge in der sie angegeben werden.

> [AZURE.NOTE] Wenn die folgende Fehlermeldung auftreten:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> Deshalb Hilfsmethoden nicht enthalten.  [Hilfsmethoden für benutzerdefinierte Skripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts)anzeigen

## <a name="sample-scripts"></a>Beispielskripts

Die Aktion Skript ist für das Erstellen HDInsight-Cluster unter Windows Azure PowerShell-Skript. Im folgenden finden Sie ein Beispiel Skript für Website-Konfigurationsdateien konfigurieren:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Das Skript akzeptiert vier Parameter, die Konfigurationsdatei und eine Eigenschaft zu, ändern Sie den Wert festlegen möchten. Zum Beispiel:

    hive-site.xml hive.metastore.client.socket.timeout 90

Dieser Parameter werden in der Struktur site.xml Datei hive.metastore.client.socket.timeout Wert auf 90 festgelegt.  Der Standardwert ist 60 Sekunden.

Dieses Beispielskript kann auch bei [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1)gefunden werden.

HDInsight bietet mehrere Skripts auf HDInsight Cluster zusätzliche Komponenten installieren:

Name | Skript
----- | -----
**Spark installieren** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. [Installieren und verwenden Funken auf HDInsight-Cluster]finden Sie unter[hdinsight-install-spark].
**Installieren von R** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Siehe [Installieren und R mit HDInsight-Cluster][hdinsight-r-scripts].
**Solr installieren** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. [Installieren und verwenden Solr auf HDInsight Cluster](hdinsight-hadoop-solr-install.md)anzeigen
- **Giraph installieren** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. [Installieren und verwenden Giraph auf HDInsight Cluster](hdinsight-hadoop-giraph-install.md)anzeigen

Skriptaktion kann von Azure Azure PowerShell-Portal oder über HDInsight .NET SDK bereitgestellt werden.  Weitere Informationen finden Sie unter [Anpassen HDInsight Cluster Skriptaktion mit][hdinsight-cluster-customize].

> [AZURE.NOTE] Die Beispielskripts arbeiten nur mit HDInsight Clusterversion 3.1 oder höher. Weitere Informationen über HDInsight Cluster Versionen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).





## <a name="helper-methods-for-custom-scripts"></a>Hilfsmethoden für benutzerdefinierte Skripts

Skript Aktionshilfsmethoden sind Dienstprogramme, die Sie beim Schreiben von benutzerdefinierten Skripts verwenden können. Diese sind in [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)definiert und können in den folgenden Skripts enthalten sein:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Hier werden die Hilfsmethoden, die von diesem Skript bereitgestellt werden:

Hilfsmethode | Beschreibung
-------------- | -----------
**HDIFile speichern** | Herunterladen einer Datei aus der angegebenen URI Uniform Resource Identifier () auf dem lokalen Datenträger des Clusters zugewiesen Azure VM-Knoten zugeordnet ist
**Erweitern Sie HDIZippedFile** | Entpacken Sie eine ZIP-Datei.
**Rufen Sie HDICmdScript** | Führen Sie ein Skript von cmd.exe.
**Schreiben HDILog** | Die Ausgabe des benutzerdefinierten Skripts für eine Skriptaktion verwendet.
**Get-Services** | Enthält eine Liste der Dienste auf dem Computer ausgeführt werden, führt das Skript aus.
**Get-Service** | Mit bestimmten Service-Namen als Eingabe erhalten Sie detaillierte Informationen für einen bestimmten Dienst (Dienstnamen, Prozess-ID, Status usw.) auf dem Computer, in dem das Skript ausgeführt wird.
**HDIServices abrufen** | Enthält eine Liste von HDInsight Services auf dem Computer ausgeführt werden, führt das Skript aus.
**HDIService abrufen** | Mit der spezifischen HDInsight Dienstname als Eingabe erhalten Sie detaillierte Informationen für einen bestimmten Dienst (Dienstnamen, Prozess-ID, Status usw.) auf dem Computer, in dem das Skript ausgeführt wird.
**Get-Dienstemehrere betreiben** | Enthält eine Liste der Dienste auf dem Computer ausgeführt werden, führt das Skript aus.
**Get-Dienst wird ausgeführt** | Überprüfen Sie, ob ein bestimmter Dienst (mit Namen) auf dem Computer ausgeführt wird, führt das Skript aus.
**HDIServicesRunning abrufen** | Enthält eine Liste von HDInsight Services auf dem Computer ausgeführt werden, führt das Skript aus.
**HDIServiceRunning abrufen** | Überprüfen Sie, ob ein bestimmter HDInsight-Dienst (mit Namen) auf dem Computer ausgeführt wird, führt das Skript aus.
**HDIHadoopVersion abrufen** | Abrufen der Version Hadoop installiert auf dem Computer, in dem das Skript ausgeführt wird.
**Test-IsHDIHeadNode** | Überprüfen Sie der Computer, in dem das Skript ausgeführt wird, Head-Knoten.
**Test-IsActiveHDIHeadNode** | Überprüfen Sie der Computer, in dem das Skript ausgeführt wird, active Head-Knoten.
**Test-IsHDIDataNode** | Überprüfen Sie der Computer, in dem das Skript ausgeführt wird, ein Datenknoten.
**HDIConfigFile bearbeiten** | Bearbeiten Sie Config-Dateien Hive-site.xml, Core site.xml, bietet site.xml, Mapred site.xml oder Garn site.xml


## <a name="best-practices-for-script-development"></a>Best Practices für die Entwicklung von Skripten

Beim Entwickeln eines benutzerdefinierten Skripts für HDInsight-Cluster gibt es verschiedene empfohlene Vorgehensweisen zu beachten:

- Hadoop Version prüfen

    Nur HDInsight Version 3.1 (Hadoop 2.4) und Support Skriptaktion für angepasste Komponenten in einem Cluster verwenden. In ein benutzerdefiniertes Skript verwenden Sie **Get-HDIHadoopVersion** -Hilfsmethode Hadoop Version vor anderen Aufgaben im Skript überprüfen.


- Stabile Links zu Skriptressourcen

    Benutzer sollten sicherstellen, dass alle Skripts und andere Artefakte in der Anpassung eines Clusters verwendet während der gesamten Lebensdauer des Clusters verfügbar bleiben und die Versionen dieser Dateien nicht für die Dauer ändern. Diese Ressourcen sind erforderlich, wenn die erneute Abbildung der Knoten im Cluster erforderlich ist. Am besten ist zu alle in ein Speicherkonto, das der Benutzer steuert. Dies ist das Standardkonto Speicher oder einer zusätzlichen Speicher Konten zum Zeitpunkt der Bereitstellung für einen angepassten Cluster.
    Spark und R angepasst Cluster Beispiele in der Dokumentation zum Beispiel wir eine lokale Kopie der Ressourcen in diesem Speicherkonto gemacht haben: https://hdiconfigactions.blob.core.windows.net/.


- Wird der Cluster Benutzerskript idempotent

    Sie müssen erwarten, dass die Knoten eines Clusters HDInsight neu abgebildeten Lebensdauer Cluster. Cluster Anpassung Skript wird ausgeführt, wenn ein Cluster neu abgebildeten. Dieses Skript muss auszulegen, idempotente insofern auf neuer Abbilder Skript achten Cluster gleich wieder Zustand angepasst, die unmittelbar nachdem das Skript zum ersten Mal ausgeführt wurde als Cluster ursprünglich erstellt wurde. Z. B. wenn ein benutzerdefiniertes Skript eine Anwendung unter D:\AppLocation installiert erstmals ausführen und bei jeder nachfolgenden Ausführung auf neuer Abbilder Skript sollte prüfen, ob die Anwendung an D:\AppLocation vor mit anderen im Skript.


- Benutzerdefinierte Komponenten in die optimale Position installieren

    Wenn Clusterknoten neu abgebildeten, kann Ressource Laufwerk C:\ und Laufwerk D:\ neu formatiert, Verlust von Daten und Programmen, die auf diesen Laufwerken installiert wurde. Das kann auch geschehen, wenn ein Knoten Azure Virtual Machine (VM), der Teil des Clusters ausfällt und durch einen neuen Knoten ersetzt. Sie können Komponenten auf D:\ Laufwerk oder an der C:\Apps im Cluster installieren. Alle anderen Speicherorten auf Laufwerk C:\ vorbehalten. Geben Sie den Speicherort für Applikationen oder Bibliotheken Benutzerskript Cluster installiert werden.


- Hohe Verfügbarkeit für die Cluster-Architektur

    HDInsight hat eine Aktiv / Passiv-Architektur für hohe Verfügbarkeit in dem Head-Knoten im aktiven Modus (wo die HDInsight Dienste ausgeführt werden) und andere Head-Knoten wird im standby-Modus werden (die HDInsight Dienste nicht ausgeführt werden). Die Knoten umschalten aktiven und passiven HDInsight Dienste unterbrochen werden. Beachten Sie eine Skriptaktion zur Installation von Diensten auf beide Hauptknoten für hohe Verfügbarkeit, dass HDInsight Failover-Mechanismus für diesen Benutzer installierte Dienste automatisch Failover nicht. So Benutzer installierte Dienste auf HDInsight Head-Knoten, die voraussichtlich hochverfügbare müssen entweder eigene Failover-Mechanismus in Aktiv-Passiv-Modus oder im Aktiv / Aktiv-Modus.

    Ein HDInsight Skript Aktionsbefehl der beider Hauptknoten ausgeführt wird, wenn die Head-Knoten-Rolle als Wert in der *ClusterRoleCollection* -Parameter angegeben ist. So entwerfen ein benutzerdefiniertes Skript Achten Sie, Ihr Skript Setup kennt. Sie sollten nicht ausführen Probleme, dieselben Dienste installiert und auf dem Head-Knoten gestartet und sie miteinander konkurrieren. Auch beachten, dass Daten während der erneuten imaging verloren, Clientsoftware über Skriptaktion gegen solche Ereignisse muss. Anwendung soll mit hoher Verfügbarkeit arbeiten, die mehrere Knoten verteilt werden. Beachten Sie, dass als 1/5 der Knoten in einem Cluster gleichzeitig neu aufgespielt werden kann.


- Konfigurieren von benutzerdefinierten Komponenten um Azure BLOB-Speicher verwenden

    Benutzerdefinierten Komponenten, die auf den Clusterknoten installieren möglicherweise eine Standardkonfiguration Hadoop verteilt Datei System bietet Speicher verwendet. Ändern Sie die Konfiguration zum Azure BLOB-Speicher verwenden. Ein Cluster image bietet Dateisystem formatiert wird und alle dort gespeicherten Daten verliert. Azure BLOB-Speicher wird stattdessen sichergestellt, dass die Daten aufbewahrt werden sollen.

## <a name="common-usage-patterns"></a>Häufige Verwendungsmuster

Dieser Abschnitt enthält Hinweise zur Implementierung einige häufige Verwendungsmuster, die in beim Schreiben eines eigenes benutzerdefinierten Skripts ausgeführt werden kann.

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Häufig in Skript Aktion Entwicklung werden müssen Umgebungsvariablen fühlen. Wahrscheinlichste Szenario ist beispielsweise eine Binärdatei von einer externen Site herunterladen, auf dem Cluster installieren und den Speicherort der Installationsort zu Ihrer Umgebungsvariable 'PATH' hinzufügen. Der folgende Codeausschnitt veranschaulicht die Umgebungsvariablen in das benutzerdefinierte Skript.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Dies legt die Umgebungsvariable **MDS_RUNNER_CUSTOM_CLUSTER** auf den Wert "True" und auch den Gültigkeitsbereich dieser Variablen Computer zu. Manchmal muss im entsprechenden Bereich – Computer- oder Umgebungsvariablen festgelegt sind. Finden Sie [hier] [ 1] Weitere Informationen zum Festlegen von Umgebungsvariablen.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Speicherorte für benutzerdefinierte Skripts werden auf

Skripts zum Anpassen eines Clusters müssen entweder im Speicher Standardkonto für den Cluster oder in einem öffentlichen schreibgeschützten Container auf andere Speicher-Konto sein. Wenn Ihr Skript an anderen Ressourcen zugreift müssen öffentlich zugänglich sein (mindestens öffentliche schreibgeschützte). Sie möchten beispielsweise auf eine Datei zugreifen und SaveFile-HDI Befehl speichern.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

In diesem Beispiel müssen Sie sicherstellen, dass der Container 'Somecontainer' in 'Somestorageaccount' Konto zugänglich ist. Andernfalls wird das Skript 'Nicht gefunden' Ausnahme auslösen und fehl.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Übergeben von Parametern an das Cmdlet hinzufügen AzureRmHDInsightScriptAction

Um mehrere Parameter hinzufügen-AzureRmHDInsightScriptAction-Cmdlet übergeben, müssen Sie den Zeichenfolgenwert enthält alle Parameter für das Skript formatieren. Zum Beispiel:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

oder

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Ausnahme bei fehlgeschlagenen Bereitstellung

Möchten Sie genau der benachrichtigt daß die cluster-Anpassung konnte nicht wie erwartet, wird eine Ausnahme und nicht die Erstellung des Clusters. Sie möchten z. B. Datei, falls vorhanden, und der Fehler Fall, in dem die Datei nicht vorhanden ist. Dadurch wird sichergestellt, dass das Skript ordnungsgemäß beendet und der Status des Clusters ordnungsgemäß bekannt ist. Der folgende Codeausschnitt zeigt ein Beispiel hierzu:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

Dieser Ausschnitt Wenn die Datei nicht existiert, führt es zu einem Zustand, in dem das Skript tatsächlich beendet ordnungsgemäß nach dem Drucken der Fehlermeldung und Cluster erreicht Ausführungszustand vorausgesetzt es "erfolgreich" Cluster Anpassung abgeschlossen. Möchten Sie genau darauf, die Anpassung im Wesentlichen cluster benachrichtigt erwartungsgemäß aufgrund einer fehlenden Datei fehlgeschlagen ist es angebracht, eine Ausnahme auslösen und der Cluster Anpassung Schritt fehl. Dazu müssen Sie den folgenden Beispielcode-Ausschnitt verwenden.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Prüfliste für die Bereitstellung einer Skriptaktion
Hier sind die Schritte, die wir bei der Vorbereitung auf diese Skripts bereitstellen:

1. Legen Sie die Dateien mit benutzerdefinierten Skripts an einem Ort, die während der Bereitstellung der Clusterknoten erreichbar ist. Dies ist Standard oder zusätzliche Speicherkonten zum Zeitpunkt der Bereitstellung oder andere öffentlich zugängliche Behälter angegeben.
2. Fügen Sie Skripts sicher, dass sie Idempotently, ausführen, sodass das Skript mehrmals auf demselben Knoten ausgeführt werden, kann überprüft.
3. Verwenden des **Write-Output** Azure PowerShell-Cmdlets zu STDOUT und STDERR. Verwenden Sie **Write-Host**nicht.
4. Verwenden Sie einen Ordner für temporäre Dateien, wie $env: TEMP von Skripts verwendete Datei und dann bereinigen sie nach Skripts ausgeführt wurden.
5. Installieren Sie benutzerdefiniertes Software nur auf D:\ oder C:\Apps. Orte auf Laufwerk C: sollte nicht verwendet werden, sind reserviert. Beachten Sie, dass Dateien auf Laufwerk C: außerhalb des Ordners C:\Apps Setupfehler während des Knotens Abbild führen.
6. Die Betriebssystem-Einstellungen oder Hadoop werden geändert wurden, sollten Sie HDInsight Dienste neu starten, damit alle Einstellungen auf Betriebssystemebene wie Umgebungsvariablen festgelegt in den Skripts aufnehmen kann.

## <a name="debug-custom-scripts"></a>Debuggen von benutzerdefinierten Skripts

Skript-Fehlerprotokolle befinden und andere Ausgaben in das Standardkonto Speicher, die Sie für den Cluster bei seiner Erstellung angegeben. Die Protokolle werden in einer Tabelle mit dem Namen *u < \cluster-name-fragment >< \time-stamp > Setuplog*. Dies sind aggregierte Protokolle, die Datensätze aller Knoten (Head-Knoten und workerknoten) auf dem das Skript im Cluster ausgeführt wird.
So überprüfen Sie die Protokolle werden HDInsight Tools für Visual Studio verwendet. Installieren der Tools finden Sie unter [Erste Schritte mit Visual Studio Hadoop Tools für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio)

**Überprüfen Sie das Protokoll mit Visual Studio**

1. Öffnen Sie Visual Studio.
2. Klicken Sie auf **Ansicht**, und klicken Sie dann auf **Server-Explorer**.
3. Maustaste auf "Azure", klicken Sie auf mit **Microsoft Azure-Abonnements**und geben Sie Ihre Anmeldeinformationen.
4. **Erweitern Sie**das Azure-Speicherkonto als Standard-Dateisystem verwendet, **Tabellen**erweitern und anschließend Doppelklicken Sie auf den Tabellennamen.


Sie können auch remote in den Clusterknoten an die STDOUT und STDERR für benutzerdefinierte Skripts. Die Protokolle auf den einzelnen Knoten werden nur auf diesen Knoten und **C:\HDInsightLogs\DeploymentAgent.log**angemeldet. Diese Protokolldateien aufzeichnen alle Ausgaben das benutzerdefinierte Skript Ein Beispiel-Protokoll Ausschnitt für eine Skriptaktion Spark sieht folgendermaßen aus:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


In diesem Protokoll ergibt Skriptaktion Spark auf dem virtuellen Computer mit dem Namen HEADNODE0 ausgeführt wurde, die während der Ausführung keine Ausnahmen ausgelöst wurden.

Ein Ausführungsfehler auftritt, wird die Ausgabe beschreiben auch in dieser Protokolldatei enthalten. Die Informationen in diesen Protokollen sollte beim Debuggen von Skripts Probleme.


## <a name="see-also"></a>Siehe auch

- [Passen Sie HDInsight Cluster mit Skriptaktion an][hdinsight-cluster-customize]
- [Installieren und Verwenden von Spark auf HDInsight-Cluster][hdinsight-install-spark]
- [Installieren und Verwenden von R auf HDInsight-Cluster][hdinsight-r-scripts]
- [Installieren und verwenden Solr auf HDInsight Cluster](hdinsight-hadoop-solr-install.md).
- [Installieren und verwenden Giraph auf HDInsight Cluster](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
