<properties
    pageTitle="Mit der Solr auf Hadoop Cluster installieren Skriptaktion | Microsoft Azure"
    description="Informationen Sie zum HDInsight-Cluster mit Solr mit Skriptaktion anpassen."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von Solr auf HDInsight Hadoop-Cluster

Lernen Sie Windows basierend HDInsight Cluster mit Solr mit Skriptaktion anpassen und Solr verwenden, um Daten zu suchen. Informationen zu Linux-basierten Cluster mit Solr finden Sie unter [Installieren und verwenden Solr auf HDinsight Hadoop-Cluster (Linux)](hdinsight-hadoop-solr-install-linux.md).
 
Solr für jeden Cluster (Hadoop Sturm, HBase, Funken) können Azure HDInsight mit *Skriptaktion*. Eine Beispielskript Solr auf einen HDInsight-Cluster installieren ist ein Blob schreibgeschützt Azure-Speicher zur [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)ab. 

Das Beispielskript funktioniert nur mit HDInsight Clusterversion 3.1. Weitere Informationen über HDInsight Cluster Versionen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).

In diesem Thema verwendete Skript erstellt einen Solr Windows-basierten Cluster mit einer bestimmten Konfiguration. Ggf. die Clusterkonfiguration Solr mit anderen Sammlungen, Splitter, Schemas, Replikate müssen Sie das Skript und Solr Binärdateien entsprechend ändern.

**Verwandte Artikel**

- [Installieren und Verwenden von Solr auf HDinsight Hadoop-Cluster (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [In HDInsight Cluster Hadoop erstellen](hdinsight-provision-clusters.md): Allgemeine Informationen zum HDInsight-Cluster erstellen.
- [HDInsight Cluster mit Skriptaktion anpassen][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mit Skriptaktion.
- [Skripts für HDInsight Skriptaktion entwickeln](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Was ist Solr?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> ist eine Plattform für unternehmensweite suchen, die leistungsstarke Volltextsuche auf Daten ermöglicht. Wohingegen Hadoop speichern und Verwalten von Daten, bietet Apache Solr Suchfunktionen, Daten schnell abzurufen. 

## <a name="install-solr-using-portal"></a>Installieren Sie Solr mit portal

1. Erstellen eines Clusters mithilfe der Option **Benutzerdefinierte** wie [Hadoop erstellen Cluster in HDInsight](hdinsight-provision-clusters.md#portal)beschrieben.
2. Der **Skript-Aktionen** des Assistenten klicken Sie auf **Skriptaktion hinzufügen** , um Angaben über die Skriptaktion wie folgt:

    ![Skriptaktion für einen Cluster anpassen verwenden] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Skriptaktion für einen Cluster anpassen verwenden")

    <table border='1'>
        <tr><th>Eigenschaft</th><th>Wert</th></tr>
        <tr><td>Name</td>
            <td>Geben Sie einen Namen für die Skriptaktion. Z. B. <b>Solr installieren</b>.</td></tr>
        <tr><td>Skript URI</td>
            <td>Geben Sie den URI Uniform Resource Identifier () in das Skript aufgerufen wird, um den Cluster anpassen. Beispielsweise <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Knotentyp</td>
            <td>Geben Sie Knoten, auf denen das Skript Anpassung ausgeführt wird. Sie können <b>alle Knoten</b>, <b>Head-Knoten</b>oder <b>Worker-Knoten</b>.
        <tr><td>Parameter</td>
            <td>Geben Sie die Parameter ggf. vom Skript an. Das Skript Solr installieren erfordert keine Parameter, damit Sie dieses Feld leer lassen können.</td></tr>
    </table>

    Sie können mehrere Skriptaktion mehrere Komponenten im Cluster installieren. Nachdem die Skripts hinzugefügt haben, klicken Sie auf das Häkchen, um den Cluster erstellen.


## <a name="use-solr"></a>Solr verwenden

Sie müssen mit Solr mit einigen Dateien indizieren beginnen. Solr können Sie Suchabfragen für die indizierten Daten ausführen. Führen Sie die folgenden Schritte aus, um Solr in einen HDInsight-Cluster verwenden:

1. **Mit Remote Desktop Protocol (RDP) Remote HDInsight Cluster mit Solr installiert**. Azure-Portal aktivieren Sie den Remotedesktop für den Cluster erstellten Solr installiert und dann Remote zum Cluster. Eine Anleitung finden Sie [mit HDInsight-Cluster über RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

2. **Index Solr Dateien hochladen**. Wenn index Solr stellen Sie Dokumente, die Sie suchen möchten. Indizierung von Solr verwenden Sie RDP Remote in Cluster, wechseln Sie zum Desktop öffnen Sie Hadoop Befehlszeile und navigieren Sie zu **C:\apps\dist\solr-4.7.2\example\exampledocs**. Führen Sie den folgenden Befehl ein:

        java -jar post.jar solr.xml monitor.xml

    Sie sehen auf der Konsole die folgende Ausgabe:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Das Dienstprogramm post.jar Indizes Solr mit zwei Beispieldokumente, **solr.xml** und **monitor.xml**. Post.jar-Dienstprogramm und die Beispieldokumente stehen Solr Installation.

3. **Mit Solr Dashboard indizierten Dokumente durchsuchen**. HDInsight Cluster RDP-Sitzung, öffnen Sie Internet Explorer und starten das Dashboard Solr **Solr/Http://headnodehost:8983 / #/**. Im linken Bereich aus der Dropdownliste **Core Selektor** wählen Sie **collection1**, klicken Sie auf **Abfrage** So wählen Sie die Dokumente in Solr zurück und bieten Sie folgende Werte:

    * Geben Sie im Textfeld **Q** ** \*:**\*. Alle Dokumente, die indiziert sind wird in Solr zurückgegeben. Wenn Sie nach einer bestimmten Zeichenfolge in den Dokumenten suchen möchten, können Sie die Zeichenfolge eingeben.
    
    * Wählen Sie das Ausgabeformat im Textfeld **wt** . Standardeinstellung ist **Json**. Klicken Sie auf **Abfrage ausführen**.

    ![Skriptaktion für einen Cluster anpassen verwenden] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Abfrage Solr-Dashboard")
    
    Die Ausgabe gibt zwei Dokumente, die für die Indizierung von Solr verwendet. Die Ausgabe ähnelt der folgenden:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Empfohlen: indizierten Daten aus Solr Azure Blob-Speicher HDInsight Cluster zugeordnet**. Empfiehlt sich jedoch sollten Sie die indizierten Daten Solr Clusterknoten auf Azure BLOB-Speicher sichern. Führen Sie dazu die folgenden Schritte aus:

    1. RDP-Sitzung öffnen Sie Internet Explorer, und zeigen Sie auf den folgenden URL:

            http://localhost:8983/solr/replication?command=backup

        Eine Antwort wie folgt sollte angezeigt werden:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. Navigieren Sie in der Remotesitzung {SOLR_HOME}\{Auflistung} \data. Für den Cluster über das Beispielskript erstellt wurden sollte das **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. An dieser Stelle sollte einen Snapshot-Ordner mit einem Namen wie *erstellt angezeigt *Snapshot.* Zeitstempel***.

    3. ZIP Snapshotordner und Azure BLOB-Speicher hochladen. Befehlszeile Hadoop navigieren Sie zum Speicherort der Snapshotordner mithilfe des folgenden Befehls:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        Dieser Befehl kopiert den Snapshot /example/data und Container in Standard Speicherkonto Cluster zugeordnet.

## <a name="install-solr-using-aure-powershell"></a>Installieren Sie Solr mit Aure PowerShell

Siehe [Anpassen HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Das demonstriert Spark mit Azure PowerShell installieren. Sie müssen das Skript [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)Verwendung anpassen.

## <a name="install-solr-using-net-sdk"></a>Installieren Sie Solr mit .NET SDK

Siehe [Anpassen HDInsight Cluster mit Skriptaktion](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Das demonstriert Spark mit .NET SDK installieren. Sie müssen das Skript [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)Verwendung anpassen.



## <a name="see-also"></a>Siehe auch

- [Installieren und Verwenden von Solr auf HDinsight Hadoop-Cluster (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [In HDInsight Cluster Hadoop erstellen](hdinsight-provision-clusters.md): Allgemeine Informationen zum HDInsight-Cluster erstellen.
- [HDInsight Cluster mit Skriptaktion anpassen][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mit Skriptaktion.
- [Skripts für HDInsight Skriptaktion entwickeln](hdinsight-hadoop-script-actions.md).
- [Installieren und Verwenden von Spark auf HDInsight Cluster][hdinsight-install-spark]: Skriptaktion Beispiel Spark installieren.
- [R auf HDInsight-Cluster installieren][hdinsight-install-r]: Aktionsbeispiel Skript zum Installieren von r
- [Giraph für HDInsight-Cluster installieren](hdinsight-hadoop-giraph-install.md): Aktionsbeispiel Skript zur Installation von Giraph.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
