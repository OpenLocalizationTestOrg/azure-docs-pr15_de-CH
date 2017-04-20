<properties
pageTitle="Bei der Clustererstellung HDInsight Struktur Bibliotheken hinzufügen | Azure"
description="Erfahren Sie Struktur Bibliotheken (JAR-Dateien) zu einem Cluster HDInsight während der Erstellung des Clusters."
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
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Bei der Clustererstellung HDInsight Struktur Bibliotheken hinzufügen

Haben Sie Bibliotheken, die häufig mit Struktur auf HDInsight verwenden, enthält dieses Dokument Informationen über eine Skriptaktion Bibliotheken während der Clustererstellung vorab geladen. Dadurch die Bibliotheken global verfügbaren Struktur (ohne [Hinzufügen JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) Laden verwenden.)

##<a name="how-it-works"></a>Funktionsweise

Beim Erstellen eines Clusters können Sie optional eine Skriptaktion angeben, die ein Skript auf den Clusterknoten ausgeführt wird, während sie erstellt werden. Das Skript in diesem Dokument akzeptiert einen einzigen Parameter, die sehr WASB Bibliotheken (gespeichert als JAR-Dateien) enthält, die vorab geladen werden.

Während der Clustererstellung Skript werden die Dateien aufgelistet, kopiert sie die `/usr/lib/customhivelibs/` Kopf und Arbeitskraft Knoten Verzeichnis anschließend hinzugefügt, die `hive.aux.jars.path` -Eigenschaft in der `core-site.xml` Datei. Auf Linux-basierten Clustern wird außerdem aktualisiert, die `hive-env.sh` den Speicherort der Datei.

> [AZURE.NOTE] Skript-Aktionen ermöglicht in diesem Artikel mit Bibliotheken in den folgenden Szenarien:
>
> * __Linux-basierte HDInsight__ - Verwendung der __Befehlszeile Struktur__ __WebHCat__ (Templeton) und __HiveServer2__.
> * __Windows-basierte HDInsight__ - die __Struktur Befehlszeilen__ und __WebHCat__ (Templeton).

##<a name="the-script"></a>Das Skript

__Skript__

Für __Linux - Cluster__: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Für __Windows - Cluster__: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Vorschriften__

* Die Skripts müssen auf __Head-Knoten__ und __Arbeitskraft Knoten__angewendet.

* Die Gläser installieren möchten müssen in einem __einzelnen Container__in Azure BLOB-Speicher gespeichert. 

* Mit der Bibliothek von JAR-Dateien __müssen__ Speicherkonto verknüpft werden HDInsight Cluster während der Erstellung. Dies kann auf zwei Arten erfolgen:

    * Vom Container auf Speicher Standardkonto für den Cluster.
    
    * Mit einem Container verknüpften Speichercontainer. Beispielsweise im Portal können __Optionale Konfiguration__ __verknüpfte Speicherkonten__ zusätzlichen Speicher hinzufügen Sie.

* WASB Pfad zum Container muss als Parameter für die Aktion Skript angegeben werden. Beispielsweise die Gläser in einem Container mit dem Namen __Bibliotheken__ auf ein Speicherkonto namens __Mystorage__gespeichert sind, der Parameter wäre __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] Dieses Dokument wird vorausgesetzt, dass bereits ein Speicherkonto zu erstellen, BLOB-Container und hochgeladen. 
    >
    > Wenn Sie kein Speicherkonto erstellt haben, möglich das über das [Azure-Portal](https://portal.azure.com). Anschließend können Sie ein Dienstprogramm wie [Azure Storage Explorer](http://storageexplorer.com/) Konto einen neuen Container erstellen und Hochladen von Dateien.

##<a name="create-a-cluster-using-the-script"></a>Erstellen eines Clusters mithilfe des Skripts

> [AZURE.NOTE] Die folgenden Schritte erstellen einen neuen Linux-basierte HDInsight-Cluster. Erstellen Sie einen neuen Windows-basierten Cluster __Windows__ Cluster OS wählen Sie beim Erstellen des Clusters und Skript Skript Windows (PowerShell) anstelle.
> 
> Azure PowerShell oder HDInsight .NET SDK können auch einen Cluster dieses Skript erstellen. Weitere Informationen zur Verwendung dieser Methode finden Sie unter [Cluster mit Skriptaktionen HDInsight anpassen](hdinsight-hadoop-customize-cluster-linux.md).

1. Bereitstellen eines Clusters mithilfe der Schritte in [Bereitstellung HDInsight Cluster unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), aber nicht Bereitstellung abgeschlossen.

2. **Optionale Konfiguration** Blade wählen Sie **Skriptaktionen**und Informationen Sie wie folgt:

    * __NAME__: Geben Sie einen Anzeigenamen für die Skriptaktion.
    * __Skript-URI__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __Kopf__: Aktivieren Sie diese Option
    * __ARBEITSKRAFT__: Aktivieren Sie diese Option.
    * __ZOOKEEPER__: Dieses Feld leer lassen.
    * __Parameter__: Geben Sie die Adresse WASB Container und Storage-Konto, das die Gläser enthält. Z. B. __wasbs://libs@mystorage.blob.core.windows.net/__.

3. Am Ende der **Skriptaktionen**Schaltfläche **Wählen Sie** zum Speichern der Konfiguration.

4. -Blade **Optionale Konfiguration** wählen Sie __Verknüpfte Speicherkonten__ und den Link zum __Hinzufügen einer Speicherschlüssel__ . Wählen Sie das Speicherkonto, das die Gläser enthält und verwenden Sie die Schaltflächen __Aktivieren__ zu speichern das __Optionale Konfiguration__ Blade.

5. Schaltfläche **Wählen Sie** unten die **Optionale Konfiguration** Blade optionale Konfigurationsinformationen speichern.

6. Bereitstellung des Clusters gemäß der [Vorschrift HDInsight Cluster unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal)weiter.

Nach Erstellung des Clusters abgeschlossen ist, werden durch dieses Skript Struktur ohne hinzugefügte Gläser verwenden die `ADD JAR` Anweisung.

##<a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie gelernt in JAR-Dateien an einen HDInsight-Cluster bei der Clustererstellung enthaltenen Struktur Bibliotheken hinzufügen. Weitere Informationen zum Arbeiten mit Struktur finden Sie unter [Verwenden mit HDInsight Struktur](hdinsight-use-hive.md)
