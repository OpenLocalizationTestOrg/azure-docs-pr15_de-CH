<properties
    pageTitle="Skript für Aktion-Entwicklung mit Linux-basierten HDInsight | Microsoft Azure"
    description="Wie HDInsight Linux-basierten Cluster mit Skript anpassen. Skript-Aktionen sind Azure HDInsight Cluster anpassen Clusterkonfigurationseinstellungen angeben oder zusätzliche Dienste, Tools oder andere Software auf dem Cluster installieren. "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>Skript-Aktion-Entwicklung mit HDInsight

Skript-Aktionen sind Azure HDInsight Cluster anpassen Clusterkonfigurationseinstellungen angeben oder zusätzliche Dienste, Tools oder andere Software auf dem Cluster installieren. Skript-Aktionen können während der Erstellung des Clusters oder auf einem Cluster ausgeführt.

> [AZURE.NOTE] Die Informationen in diesem Dokument ist für Linux-basierte HDInsight-Cluster. Informationen mit Windows-basierten Clustern Skriptaktionen finden Sie unter [Skript Aktion Entwicklung mit HDInsight (Windows)](hdinsight-hadoop-script-actions.md).

## <a name="what-are-script-actions"></a>Was sind Skriptaktionen?

Skript-Aktionen sind Bash-Skripten, die Azure-Konfiguration ändern oder Software installieren auf Clusterknoten ausgeführt wird. Eine Skriptaktion als Root ausgeführt und bietet volle Zugriffsrechte auf den Clusterknoten.

Skript-Aktionen können mit den folgenden Methoden angewendet werden:

| Können Sie ein Skript anwenden... | Während der cluster erstellen... | In einem Cluster ausgeführt. |
| ----- |:-----:|:-----:|
| Azure-Portal | ✓ | ✓ |
| Azure PowerShell | ✓ | ✓ |
| Azure CLI | &nbsp; | ✓ |
| HDInsight .NET SDK | ✓ | ✓ |
| Azure-Ressourcen-Manager-Vorlage | ✓ | &nbsp; |

Weitere Informationen zur Verwendung dieser Methoden zum Skript Aktionen finden Sie unter [Anpassen HDInsight Cluster mit Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Best Practices für die Entwicklung von Skripten

Beim Entwickeln eines benutzerdefinierten Skripts für HDInsight-Cluster gibt es verschiedene empfohlene Vorgehensweisen zu beachten:

- [Ziel der Hadoop version](#bPS1)
- [Ziel der Betriebssystemversion](#bps10)
- [Stabile Links zu Skriptressourcen](#bPS2)
- [Vorkompilierte Ressourcen](#bPS4)
- [Wird der Cluster Benutzerskript idempotent](#bPS3)
- [Hohe Verfügbarkeit für die Cluster-Architektur](#bPS5)
- [Konfigurieren von benutzerdefinierten Komponenten um Azure BLOB-Speicher verwenden](#bPS6)
- [Schreiben von Informationen in STDOUT und STDERR](#bPS7)
- [LF-Zeilenenden ASCII-speichern](#bps8)
- [Mit der Wiederholungslogik vorübergehender Fehler beheben](#bps9)

> [AZURE.IMPORTANT] Skript-Aktionen müssen innerhalb von 60 Minuten oder Timeout wird. Während der Bereitstellung Knoten das Skript gleichzeitig mit anderen Prozessen Einrichtung und Konfiguration ausgeführt. Wettbewerb um Ressourcen wie CPU-Zeit oder der Bandbreite kann das Skript länger dauern, als bei Umgebung.

### <a name="bPS1"></a>Ziel der Hadoop version

Verschiedene Versionen von HDInsight haben verschiedene Versionen von Hadoop-Dienste und Komponenten. Wenn das Skript eine bestimmte Version eines Dienstes oder Komponente erwartet, nur verwenden das Skript mit der Version von HDInsight Sie, die erforderlichen Komponenten enthält. Komponentenversionen HDInsight enthaltenen Informationen finden Sie mithilfe der [HDInsight Komponente Versionen](hdinsight-component-versioning.md) des Dokuments.

###<a name="bps10"></a>Ziel der Betriebssystemversion

Linux-basierte HDInsight basiert auf Ubuntu Linux-Distribution. Verschiedene Versionen von HDInsight verlassen sich auf verschiedene Versionen von Ubuntu, die Ihr Skript Verhalten beeinflussen kann. Z. B. HDInsight 3.4 und früher basieren auf Ubuntu-Versionen, die Upstart verwenden. Version 3.5 basiert auf Ubuntu 16.04 Systemd verwendet. Systemd und Upstart basieren auf verschiedene Befehle, Skripts geschrieben werden soll, beide arbeiten.

Ein weiterer wichtiger Unterschied zwischen HDInsight 3.4 und 3.5 ist die `JAVA_HOME` verweist jetzt auf Java 8.

Überprüfen Sie die OS-Version mit `lsb_release`. Die folgenden Codeausschnitte vom Installationsskript Farbton zu bestimmen, ob das Skript Ubuntu 14 oder 16 veranschaulicht:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

Finden Sie das vollständige Skript, das diese Ausschnitte in https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh enthält.

Die Version von Ubuntu von HDInsight verwendet wird, finden Sie unter [Komponentenversion HDInsight](hdinsight-component-versioning.md) Dokument.

Die Unterschiede zwischen Systemd und Upstart finden Sie unter [Systemd Upstart Benutzer](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Stabile Links zu Skriptressourcen

Sie sollten sicherstellen, dass die Skripts und vom Skript verwendeten Ressourcen während der gesamten Lebensdauer des Clusters verfügbar bleiben und die Versionen dieser Dateien nicht für die Dauer ändern. Diese Ressourcen sind erforderlich, wenn neue Knoten zum Cluster hinzugefügt werden, während der Skalierung Operationen.

Am besten ist zu alle in Azure Storage-Konto für Ihr Abonnement.

> [AZURE.IMPORTANT] Verwendet Storage-Konto muss Speicher Domänenkonto für den Cluster oder einen öffentlichen schreibgeschützten Container auf andere Speicher-Konto sein.

Beispielsweise von Microsoft bereitgestellten Beispiele im Speicherkonto [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) befinden sich die öffentlichen, schreibgeschützten Container das HDInsight-Team verwaltet.

### <a name="bPS4"></a>Vorkompilierte Ressourcen

Um die Zeit reduzieren, die benötigt wird, um das Skript ausführen, vermeiden Sie Vorgänge, die Ressourcen aus dem Quellcode kompilieren. Stattdessen kompilieren Sie Ressourcen vor und speichern Sie die binäre Version in Azure BLOB-Speicher, sodass sie schnell zum Cluster aus dem Skript heruntergeladen werden kann.

### <a name="bPS3"></a>Wird der Cluster Benutzerskript idempotent

Skripts auszulegen, insofern Idempotent sein, das ist das Skript mehrmals ausgeführt, sollten sicherstellen, dass der Cluster in demselben Zustand zurückgegeben wird jedes Mal ausgeführt wird.

Beispielsweise wenn ein benutzerdefiniertes Skript installiert eine Anwendung unter usr auf erstmals ausführen und bei jeder folgenden Ausführung des Skripts sollte prüfen, ob bereits an usr vor mit anderen im Skript.

### <a name="bPS5"></a>Hohe Verfügbarkeit für die Cluster-Architektur

Linux-basierte HDInsight Cluster stellt zwei Head-Knoten, die innerhalb des Clusters und Skript Aktionen sind für beide Knoten ausgeführt wurde. Wenn die Komponenten, die Sie installieren nur eine Headknoten erwarten, müssen Sie ein Skript entwerfen, die nur die Komponente auf einem der beiden Head-Knoten im Cluster installieren.

> [AZURE.IMPORTANT] Standarddienste installiert HDInsight soll zwischen den beiden Head-Knoten ein Failover bei Bedarf jedoch dadurch nicht auf benutzerdefinierte Komponenten durch Skript Weise erweitert wird. Benötigen Sie Komponenten durch eine Skriptaktion hoch verfügbar sein, müssen Sie Failovermechanismus implementieren, der die beiden verfügbaren Hauptknoten verwendet.

### <a name="bPS6"></a>Konfigurieren von benutzerdefinierten Komponenten um Azure BLOB-Speicher verwenden

Komponenten des Clusters können eine Standardkonfiguration verfügen, die Hadoop verteilt Datei System bietet Speicher verwendet. HDInsight verwendet der Standardspeicher Azure BLOB-Speicher (WASB). Dies bietet eine bietet kompatible Dateisystem, die Daten erhalten bleibt, auch wenn Cluster gelöscht wird. Konfigurieren Sie Komponenten installieren, um WASB anstelle bietet.

Beispielsweise kopiert die folgende Giraph examples.jar Datei aus dem lokalen Dateisystem in WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>Schreiben von Informationen in STDOUT und STDERR

Während der Ausführung des Skripts an STDOUT und STDERR geschrieben wird protokolliert und kann mithilfe der Ambari Web-Benutzeroberfläche angezeigt werden.

> [AZURE.NOTE] Ambari stehen nur zur Verfügung, wenn der Cluster erstellt wird. Wenn Sie eine Skriptaktion während der Clustererstellung Erstellung fehlschlägt, im Abschnitt Problembehandlung [Anpassen HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) anderweitig protokollierten Informationen.

Die meisten Dienstprogramme und Installationspakete schreibt bereits Informationen zu STDOUT und STDERR jedoch zusätzliche Protokollierung hinzufügen möchten. Text an STDOUT mit `echo`. Zum Beispiel:

        echo "Getting ready to install Foo"

Standardmäßig `echo` senden die Zeichenfolge an STDOUT. Um direkt an STDERR hinzufügen `>&2` vor dem `echo`. Zum Beispiel:

        >&2 echo "An error occurred installing Foo"

Leitet diese Informationen an STDOUT (1 Standard daher hier nicht aufgeführt) an STDERR (2). Weitere Informationen zu e/a-Umleitung finden Sie unter [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Weitere Informationen zum Anzeigen von Skriptaktionen protokollierten Informationen finden Sie unter [Anpassen HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

###<a name="bps8"></a>LF-Zeilenenden ASCII-speichern

Bash Skripts sollten mit LF beendet im ASCII-Format gespeichert werden. Wenn Dateien als UTF-8, z. B. eine Bytereihenfolge-Marke am Anfang der Datei und Zeilenenden CRLF, die für Windows-Editoren gespeichert sind, schlägt das Skript ähnelt dem folgenden Fehler fehl:

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>Mit der Wiederholungslogik vorübergehender Fehler beheben

Beim Herunterladen, Installieren von Paketen mit apt-Get oder andere Aktionen, die Daten über das Internet kann die Aktion durch vorübergehende Netzwerkfehlern fehl. Z. B. möglicherweise die Kommunikation mit remote Ressource beim Failover auf einen.

Um Ihr Skript stabil zu vorübergehender Fehler machen, können Sie die Wiederholungslogik implementieren. Folgendes ist ein Beispiel für eine Funktion, die führen Sie einen Befehl übergeben, und (wenn der Befehl fehlschlägt,) bis zu dreimal wiederholen. Es werden zwei Sekunden zwischen jedem Neuversuch.

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

Es folgen Beispiele für die Verwendung dieser Funktion.

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>Hilfsmethoden für benutzerdefinierte Skripts

Skript Aktionshilfsmethoden sind Dienstprogramme, die Sie beim Schreiben von benutzerdefinierten Skripts verwenden können. Diese sind in [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)definiert und können in den folgenden Skripts enthalten sein:

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

Dadurch die folgenden Hilfsprogramme zur Verwendung in Ihrem Skript:

| Helper-Verwendung | Beschreibung |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | Downloadet eine Datei von der Quell-URL auf den angegebenen Dateipfad. Standardmäßig wird eine vorhandene Datei nicht überschrieben. |
| `untar_file TARFILE DESTDIR` | Tar-Datei extrahiert (mit `-xf`,) in das Zielverzeichnis. |
| `test_is_headnode` | Wenn für einen Cluster Head-Knoten gibt 1; ausgeführt andernfalls 0. |
| `test_is_datanode` | Wenn der aktuelle Knoten einen Datenknoten (Mitarbeiter) ist, gibt 1; andernfalls 0. |
| `test_is_first_datanode` | Wenn der aktuelle Knoten der erste (Arbeitskraft) Datenknoten (genannt workernode0) ist, gibt 1 zurück. andernfalls 0. |
| `get_headnodes` | Gibt den vollqualifizierten Domänennamen der Headnodes im Cluster. Namen sind durch Kommas getrennt. Fehler wird eine leere Zeichenfolge zurückgegeben. |
| `get_primary_headnode` | Ruft den vollqualifizierten Domänennamen des primären Hauptknoten. Fehler wird eine leere Zeichenfolge zurückgegeben. |
| `get_secondary_headnode` | Ruft den vollqualifizierten Domänennamen des sekundären Hauptknoten. Fehler wird eine leere Zeichenfolge zurückgegeben. |
| `get_primary_headnode_number` | Ruft das numerische Suffix der primären Hauptknoten. Fehler wird eine leere Zeichenfolge zurückgegeben. |
| `get_secondary_headnode_number` | Ruft das numerische Suffix sekundären Hauptknoten. Fehler wird eine leere Zeichenfolge zurückgegeben. |

## <a name="commonusage"></a>Häufige Verwendungsmuster

Dieser Abschnitt enthält Hinweise zur Implementierung einige häufige Verwendungsmuster, die in beim Schreiben eines eigenes benutzerdefinierten Skripts ausgeführt werden kann.

### <a name="passing-parameters-to-a-script"></a>Übergeben von Parametern an ein Skript

In einigen Fällen kann das Skript Parameter erforderlich. Z. B. das Administratorkennwort für den Cluster möglicherweise Informationen aus der Ambari-REST-API abrufen.

Parameter an das Skript übergeben werden als _Positionsparameter_bezeichnet und zugeordnet `$1` für den ersten Parameter `$2` für das zweite und so für. `$0`enthält den Namen des Skripts.

Werte als Parameter an das Skript übergeben sollten einfache Anführungszeichen (') eingeschlossen werden, sodass der übergebene Wert als Literal behandelt und besondere Behandlung nicht enthalten Zeichen gegeben '!'.

### <a name="setting-environment-variables"></a>Festlegen von Umgebungsvariablen

Festlegen von Umgebungsvariablen wie folgt durchgeführt:

    VARIABLENAME=value

Ist VARIABLENNAME den Namen der Variablen ein. Für den Zugriff auf die Variable anschließend verwenden `$VARIABLENAME`. Zum Zuweisen eines Werts durch einen positionellen Parameter als eine Umgebungsvariable Kennwort bereitgestellt würden Sie beispielsweise Folgendes verwenden:

    PASSWORD=$1

Der nachfolgende Zugriff auf die Informationen können dann `$PASSWORD`.

Im Skript festgelegten existieren nur innerhalb des Bereichs des Skripts. In einigen Fällen müssen Sie große Systemumgebungsvariablen hinzufügen, die nach Abschluss des Skripts beibehalten wird. Normalerweise ist diese Benutzer mit dem Cluster über SSH Komponenten vom Skript verwenden können. Sie erreichen dies durch Hinzufügen der Umgebungsvariable `/etc/environment`. Beispielsweise fügt die folgende __HADOOP\_ü\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Speicherorte für benutzerdefinierte Skripts werden auf

Skripts zum Anpassen eines Clusters müssen entweder im Speicher Domänenkonto für den Cluster oder auf einem anderen Speicherkonto in einem öffentlichen schreibgeschützten Container sein. Wenn Ihr Skript greift auf Ressourcen an anderen diese müssen auch zu einem (mindestens öffentliche schreibgeschützte). Beispielsweise sollten eine Datei mit dem Cluster `download_file`.

Gespeichert in ein Konto Azure-Speicher vom Cluster (wie das Standardkonto Speicher), bietet schnellen Zugriff, diesen Speicher Azure-Netzwerk ist.

### <a name="checking-the-operating-system-version"></a>Überprüfen der Version des Betriebssystems

Verschiedene Versionen von HDInsight verlassen sich auf bestimmte Versionen von Ubuntu. Möglicherweise gibt es Unterschiede zwischen Betriebssystemversionen, die Sie in Ihrem Skript suchen müssen. Beispielsweise müssen Sie Binärdateien installieren, die auf die Version von Ubuntu gebunden ist.

Verwenden Sie zum Überprüfen der Betriebssystemversion `lsb_release`. Beispielsweise veranschaulicht folgender je nach Betriebssystem eine andere Tar verweisen:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>Prüfliste für die Bereitstellung einer Skriptaktion

Hier sind die Schritte, die wir bei der Vorbereitung auf diese Skripts bereitstellen:

- Legen Sie die Dateien mit benutzerdefinierten Skripts an einem Ort, die während der Bereitstellung der Clusterknoten erreichbar ist. Dies ist Standard oder zusätzliche Speicherkonten zum Zeitpunkt der Bereitstellung oder andere öffentlich zugängliche Behälter angegeben.

- Fügen Sie Schecks in Skripts, um sicherzustellen, dass machtlos, Ausführung, damit das Skript mehrmals auf demselben Knoten ausgeführt werden kann.

- Verwenden einer temporären Datei Verzeichnis tmp heruntergeladenen Dateien von den Skripts verwendet und dann bereinigen sie nach Skripts ausgeführt wurden.

- Die Betriebssystem-Einstellungen oder Hadoop werden geändert wurden, sollten Sie HDInsight Dienste neu starten, damit alle Einstellungen auf Betriebssystemebene wie Umgebungsvariablen festgelegt in den Skripts aufnehmen kann.

## <a name="runScriptAction"></a>Eine Skriptaktion ausführen

Skript-Aktionen können Sie HDInsight-Cluster mithilfe von Azure-Portal Azure PowerShell Azure Resource Manager (ARM) Vorlagen oder HDInsight .NET SDK anpassen. Eine Anleitung finden Sie unter [Skriptaktion verwenden](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Benutzerdefiniertes Skript-Beispiele

Microsoft enthält Beispielskripts Komponenten auf einen HDInsight-Cluster installieren. Beispielskripts und eine Anleitung zur Verwendung sind unter den folgenden Links verfügbar:

- [Installieren und Verwenden von Farbton auf HDInsight-Cluster](hdinsight-hadoop-hue-linux.md)
- [Installieren und Verwenden von R HDInsight Hadoop-Cluster](hdinsight-hadoop-r-scripts-linux.md)
- [Installieren und Verwenden von Solr auf HDInsight-Cluster](hdinsight-hadoop-solr-install-linux.md)
- [Installieren und Verwenden von Giraph auf HDInsight-Cluster](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] Die Dokumente oben sind spezifisch für Linux-basierte HDInsight-Cluster. Für einen mit Windows-basierten HDInsight Skripts [Skript Aktion](hdinsight-hadoop-script-actions.md) Entwicklung mit HDInsight (Windows) oder die Links am oberen Rand jeder Artikel.

##<a name="troubleshooting"></a>Problembehandlung

Folgende sind Fehler, die auftreten können, wenn Skripts, das, die Sie entwickelt:

__Error__: `$'\r': command not found`. Manchmal gefolgt von `syntax error: unexpected end of file`.

_Ursache_: Dieser Fehler wird verursacht, wenn die Zeilen in einem Skript CRLF enden. UNIX-Systeme erwarten nur LF Zeile endet.

Dieses Problem tritt am häufigsten erstellte Skript auf einer Windows-Umgebung wie CRLF eine gemeinsame Haltung für viele Text-Editoren für Windows.

_Auflösung_: ist dies eine Option im Text-Editor wählen Sie Unix-Format oder LF für Zeile endet. Auch können die folgenden Befehle auf einem Unix-System das CRLF ein LF ändern:

> [AZURE.NOTE] Die folgenden Befehle sind entspricht, sollten sie die Zeilenenden CRLF LF ändern. Wählen Sie auf die Dienstprogramme auf Ihrem System.

| Befehl | Notizen |
| ------- | ----- |
| `unix2dos -b INFILE` | Die ursprüngliche Datei gesichert werden mit ein. BAK-Erweiterung |
| `tr -d '\r' < INFILE > OUTFILE` | OUTFILE enthält eine Version mit nur LF |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Dies wird direkt ohne ändern Erstellen einer neuen Datei |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | OUTFILE enthält eine Version mit nur LF.

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_Ursache_: Dieser Fehler tritt auf, wenn das Skript als UTF-8 mit Byte Order Mark (BOM) gespeichert wurde.

_Lösung_: Speichern Sie die Datei als ASCII- oder UTF-8 ohne eine Stückliste. Sie können auch den folgenden Befehl Linux oder Unix verwenden, zum Erstellen einer neuen Datei ohne die Stückliste:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Der obige Befehl Ersetzen Sie __EINGABEDATEI__ mit der Stückliste. __OUTFILE__ sollte das Skript ohne BOM enthalten einen neuen Dateinamen ein.

## <a name="seeAlso"></a>Nächste Schritte

* Erfahren Sie, wie [mit Skriptaktion anpassen HDInsight-Cluster](hdinsight-hadoop-customize-cluster-linux.md)

* [HDInsight .NET SDK-Referenz](https://msdn.microsoft.com/library/mt271028.aspx) verwenden Sie, um weitere Informationen zum Erstellen von .NET Applications, die HDInsight verwalten

* Verwenden Sie die [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) erfahren Sie, wie mit anderen Maßnahmen für HDInsight-Cluster ausführen.
