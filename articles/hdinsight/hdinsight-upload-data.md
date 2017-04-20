<properties
    pageTitle="Daten für Hadoop Aufträge in HDInsight | Microsoft Azure"
    description="Informationen Sie zum Hochladen und Datenzugriff Hadoop Aufträge in Azure CLI, Azure Storage Explorer, Azure PowerShell, Hadoop Befehlszeile oder Sqoop mit HDInsight."
    services="hdinsight,storage"
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
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Daten Sie für Projekte in HDInsight Hadoop

Azure HDInsight bietet eine umfassende Hadoop verteilten Dateisystem (bietet) über Azure BLOB-Speicher. Diese Erweiterung bietet soll problemlos Kunden anbieten. Es ermöglicht den vollständigen Satz von Komponenten im Hadoop-Ökosystem direkt mit Daten arbeiten, die er verwaltet. Azure BLOB-Speicher und bietet sind unterschiedliche Dateisysteme, die für die Speicherung von Daten und Berechnungen auf Daten optimiert werden. Informationen zu den Vorteilen der Verwendung von Azure BLOB-Speicher finden Sie unter [verwenden Azure BLOB-Speicher mit HDInsight][hdinsight-storage].

**Erforderliche Komponenten**

Beachten Sie die folgende Bedingung vor:

* Ein Azure HDInsight-Cluster. Informationen finden Sie unter [Erste Schritte mit Azure HDInsight] [ hdinsight-get-started] oder [Bereitstellung HDInsight Cluster][hdinsight-provision].

##<a name="why-blob-storage"></a>Warum BLOB-Speicher?

Azure HDInsight-Cluster sind typischerweise MapReduce-Jobs ausführen und Cluster werden gelöscht, nachdem diese Aufgaben ausführen. Schutz der Daten in der bietet wäre Cluster nach Berechnungen abgeschlossen sind eine teure für diese Daten. Azure BLOB-Speicher ist eine hochverfügbare, hochgradig skalierbare, hohe Kapazität, kostengünstige und gemeinsam nutzbare Speicheroption für Daten, die verarbeitet HDInsight. Speichern von Daten in ein Blob kann HDInsight-Cluster, die problemlos freigegeben werden, ohne Daten für die Berechnung verwendet werden.

###<a name="directories"></a>Verzeichnisse

Azure BLOB-Speicher Containern speichern als Schlüssel-Wert-Paare und es gibt keine Verzeichnishierarchie. Jedoch kann das Zeichen "/" zu erscheinen in einer Verzeichnisstruktur gespeichert wird innerhalb des Schlüssels verwendet werden. HDInsight sieht diese als wären sie tatsächliche Verzeichnisse.

Beispielsweise kann ein Blob-Schlüssel *input/log1.txt*sein. Das Verzeichnis "input" vorhanden, aber aufgrund der Zeichen "/" der Name des Schlüssels wird die Darstellung des Dateipfades.

Daher verwenden Sie Azure Extras Sie möglicherweise einige Dateien mit 0 Bytes fest. Diese Dateien dienen zwei unterschiedlichen Zwecken:

- Sind leere Ordner, markieren sie die Existenz des Ordners. Azure BLOB-Speicher ist klug genug zu wissen, dass ein Blob namens Foo-Leiste vorhanden ist, ein Ordner mit dem Namen **Foo wird**. Jedoch können nur einen leeren Ordner namens **"Foo"** bedeuten, dass diese speziellen 0 Byte-Datei an.

- Sie enthalten spezielle Metadaten vom Dateisystem Hadoop, insbesondere die Berechtigungen und Besitzer für Ordner erforderlich ist.

##<a name="command-line-utilities"></a>Befehlszeilen-Dienstprogramme

Microsoft bietet die folgenden Dienstprogramme Azure BLOB-Speicher arbeiten:

| Tool | Linux | OS X | Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure Befehlszeilenschnittstelle][azurecli] | ✔ | ✔ | ✔ |
| [Azure PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Hadoop-Befehl](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Während die CLI Azure Azure PowerShell und AzCopy können von externen Azure Hadoop Befehl steht nur auf dem HDInsight-Cluster und lässt nur das Laden von Daten aus dem lokalen Dateisystem in Azure BLOB-Speicher verwendet werden.

###<a id="xplatcli"></a>Azure CLI

Azure-CLI ist eine plattformübergreifende Tool Azure Services verwalten. Gehen Sie zum Hochladen von Daten in Azure BLOB-Speicher:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Installieren und Konfigurieren der Azure-CLI für Mac, Linux und Windows](../xplat-cli-install.md).

2. Öffnen Sie ein Eingabeaufforderungsfenster, Bash oder anderen Shell, und verwenden Sie die folgenden-Abonnements Azure authentifizieren.

        azure login

    Bei Aufforderung geben Sie den Benutzernamen und das Kennwort für Ihr Abonnement.

3. Geben Sie den folgenden Befehl Listen Sie Speicherkonten für Ihr Abonnement:

        azure storage account list

4. Wählen Sie das Speicherkonto, das Blob enthält arbeiten und verwenden Sie den folgenden Befehl, um den Schlüssel für dieses Konto abzurufen:

        azure storage account keys list <storage-account-name>

    Dies sollte **primären** und **sekundären** Schlüssel zurückgeben. Kopieren Sie **den Primärschlüsselwert** , da in den nächsten Schritten Hiermit.

5. Verwenden Sie den folgenden Befehl zum Abrufen einer Liste der BLOB-Container das Speicherkonto:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. Verwenden Sie die folgenden Befehle zum Uploaden und Downloaden von Dateien in den Blob:

    * Hochladen eine Datei:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Herunterladen eine Datei:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Wenn Sie immer mit demselben Speicherkonto arbeiten, können die folgenden Umgebungsvariablen anstatt das Konto und Schlüssel für jeden Befehl:
>
> * **AZURE\_Speicher\_Konto**: Speicher-Kontoname
>
> * **AZURE\_Speicher\_Zugang\_KEY**: Speicherschlüssel Konto

###<a id="powershell"></a>Azure PowerShell

Azure PowerShell ist eine Skriptingumgebung, mit denen Sie steuern und Automatisieren der Bereitstellung und Verwaltung der Arbeitslasten in Azure. Weitere Informationen zum Konfigurieren Ihrer Arbeitsstation ausführen Azure PowerShell [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Eine lokale Datei Azure BLOB-Speicher hochladen**

1. Azure PowerShell-Konsole öffnen, wie in [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
2. Legen Sie Werte für die ersten fünf Variablen, das folgende Skript:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Fügen Sie das Skript in der Azure PowerShell-Konsole ausführen, um die Datei zu kopieren.

Z. B. PowerShell-Skripts erstellt mit HDInsight, finden Sie Sie [HDInsight](https://github.com/blackmist/hdinsight-tools).

###<a id="azcopy"></a>AzCopy

AzCopy ist ein Befehlszeilenprogramm, das die Übertragung von Daten aus einem Azure Storage-Konto vereinfachen soll. Sie können als eigenständiges Tool verwenden oder dieses Tool in einer vorhandenen Anwendung integrieren. [Downloaden Sie AzCopy][azure-azcopy-download].

AzCopy-Syntax lautet:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Weitere Informationen finden Sie unter [AzCopy - Dateien für Azure-Blobs Hochladen/Herunterladen][azure-azcopy].


###<a id="commandline"></a>Hadoop-Befehlszeile

Hadoop Befehlszeile eignet sich nur zum Speichern von Daten in den BLOB-Speicher, wenn die Daten bereits auf dem Head-Knoten des Clusters.

Um den Hadoop-Befehl verwenden, müssen Sie zunächst den Hauptknoten mithilfe einer der folgenden Methoden verbinden:

* **Windows-basierte HDInsight**: [Verwenden von Remotedesktop](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **Linux-basierte HDInsight**: Verbindung über SSH ([Befehl SSH](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) oder [kitten](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Nachdem die Verbindung hergestellt ist, können Sie folgende Syntax, zum Hochladen einer Datei in den Speicher.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Zum Beispiel`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Ist das Standarddateisystem für HDInsight in Azure BLOB-Speicher ist /example/data.txt in Azure BLOB-Speicher. Sie können auch als Datei verweisen:

    wasbs:///example/data/data.txt

oder

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Eine Liste der anderen Hadoop Befehle für Dateien finden Sie unter [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] Auf HBase Cluster standardmäßig verwendet, wenn Daten schreiben 256 KB Größe blockieren. Während dies funktioniert bei HBase APIs oder REST-APIs verwenden, die `hadoop` oder `hdfs dfs` Befehle schreiben größer als 12GB Daten führt zu einem Fehler. Abschnitt [Speicher Ausnahme schreiben BLOB](#storageexception) unter Weitere Informationen.

##<a name="graphical-clients"></a>Grafik-clients

Es gibt auch mehrere Programme, die eine Benutzeroberfläche für die Arbeit mit Azure Storage bereitstellen. Folgendes ist eine Liste mit ein Paar der Anwendung:

| Client | Linux | OS X | Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio-Tools für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Azure-Speicher-Explorer](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Cloud-Speicher Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio-Tools für HDInsight

Weitere Informationen finden Sie unter [verknüpften Ressourcen navigieren](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

###<a id="storageexplorer"></a>Azure-Speicher-Explorer

*Azure-Speicher-Explorer* ist ein nützliches Tool zum Überprüfen und Ändern von Daten in Blobs. Es ist eine kostenlose open Source-Tool, die von [http://storageexplorer.com/](http://storageexplorer.com/)heruntergeladen werden kann. Der Quellcode ist auch hier ab.

Bevor Sie das Tool verwenden, müssen Sie Ihre Azure-Konto und Speicherschlüssel kennen. Informationen zum Abrufen dieser Informationen finden Sie unter der "wie: anzeigen, kopieren und Regenerieren Speicher Zugriffstasten" Abschnitt [erstellen, verwalten oder löschen Sie ein Speicherkonto][azure-create-storage-account].  

1. Azure-Speicher-Explorer ausführen. Wenn dies das erste Mal ist der Speicher-Explorer, Sie werden aufgefordert, ___speicherkontonamen__ und __speicherkontoschlüssel__. Haben Sie haben es vor Verwendung __Hinzufügen__ , um einen neuen Speicher-Kontonamen und Schlüssel hinzuzufügen.

    Geben Sie den Namen und Schlüssel für das Speicherkonto HDinsight Cluster verwendet und wählen Sie dann __Speichern und öffnen__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Klicken Sie in der Liste der Container auf der linken Seite der Schnittstelle des Containers HDInsight Cluster zugeordnet ist. Standardmäßig ist der Name des Clusters HDInsight dies abweichen, wenn Sie einen bestimmten Namen beim Erstellen des Clusters eingegeben.

6. Wählen Sie der Tool-Leiste das Symbol hochladen.

    ![Symbolleiste mit Upload-Symbol markiert](./media/hdinsight-upload-data/toolbar.png)

7. Angeben einer hochzuladenden Datei, und klicken Sie auf **Öffnen**. Bei Aufforderung wählen Sie hochladen in das Stammverzeichnis der Speichercontainer __Hochladen__ . Möchten Sie upload der Datei auf einen bestimmten Pfad, geben Sie den Pfad im Feld __Ziel__ und wählen Sie dann __Hochladen__.

    ![Datei-Upload-Dialogfeld](./media/hdinsight-upload-data/fileupload.png)
    
    Nach die Datei hochladen, können Sie es aus auf HDInsight Cluster.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Bereitstellen von Azure BLOB-Speicher als lokales Laufwerk

[Mount Azure BLOB-Speicher als lokales Laufwerk](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx)anzeigen

##<a name="services"></a>Dienste

###<a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory-Dienst ist ein vollständig verwalteter Dienst für Speicherung, Verarbeitung und Daten Datentransfer-Services in optimierte, skalierbare und zuverlässige Produktion Datenpipelines verfassen.

Azure Data Factory kann zum Verschieben von Daten in Azure BLOB-Speicher oder Datenpipelines erstellen, die direkt HDInsight Struktur wie Schwein Funktionen verwendet werden.

Weitere Informationen finden Sie in der [Dokumentation zu Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

###<a id="sqoop"></a>Apache Sqoop

Sqoop ist ein Tool zum Übertragen von Daten zwischen Hadoop und relationalen Datenbanken. Sie können Daten aus einer relationalen Datenbank-Managementsystem (RDBMS) importieren, z. B. SQL Server, MySQL und Oracle Hadoop verteiltes Dateisystem (bietet) Transformieren der Daten in Hadoop MapReduce oder Struktur und exportieren Sie die Daten in einem RDBMS.

Weitere Informationen finden Sie unter [Verwenden Sqoop mit HDInsight][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Entwicklung SDKs

Azure BLOB-Speicher kann auch mit einer Azure SDK aus folgenden Programmiersprachen zugegriffen werden:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Weitere Informationen zum Installieren der Azure-SDKs finden Sie unter [Azure-downloads](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Problembehandlung

### <a id="storageexception"></a>Ausnahme für Schreibzugriff BLOB Speicher

__Symptome__: bei Verwendung der `hadoop` oder `hdfs dfs` Befehle Dateien schreiben 12 GB oder größer auf einem Cluster HBase folgenden Fehler: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Ursache__: HBase auf HDInsight Cluster Standard auf einer Blockgröße von 256 KB, beim Schreiben in Azure-Speicher. Dies funktioniert für HBase-APIs oder anderen APIs führt ein Fehler bei Verwendung der `hadoop` oder `hdfs dfs` Befehlszeilenprogramme.

__Auflösung__: mit `fs.azure.write.request.size` eine größere Größe angeben. Sie können dazu pro Nutzung der `-D` Parameter. Im folgenden werden ein Beispiel zur Verwendung dieser Parameter mit dem `hadoop` Befehl:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Sie können auch den Wert der `fs.azure.write.request.size` Global mithilfe von Ambari. Den Wert Ambari Web-Benutzeroberfläche ändern können folgendermaßen verwendet werden:

1. Gehen Sie in Ihrem Browser die Webbenutzeroberfläche Ambari für Cluster. Dies ist https://CLUSTERNAME.azurehdinsight.net, wobei __CLUSTERNAME__ den Namen des Clusters.

    Wenn Sie aufgefordert werden, geben Sie den Administratornamen und das Kennwort für den Cluster.

2. Wählen Sie von der linken Seite des Bildschirms __bietet__, und wählen Sie dann die Registerkarte __Konfigurationen__ .

3. Geben Sie im Feld __Filter...__ `fs.azure.write.request.size`. Das Feld und den aktuellen Wert in der Mitte der Seite erscheint.

4. Ändern Sie den Wert auf den neuen Wert von 262144 (256KB). Z. B. 4194304 (4MB).

![Bild ändern Sie den Wert über Ambari Web-Benutzeroberfläche](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Weitere Informationen über Ambari finden Sie unter [Verwalten HDInsight Cluster mit Ambari Web-Benutzeroberfläche](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Nächste Schritte

Nun wissen, wie Daten zu HDInsight lesen Sie folgenden Artikel Informationen zum Analysieren:

* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [Hadoop Aufträge programmgesteuert übermitteln][hdinsight-submit-jobs]
* [Struktur mit HDInsight verwenden][hdinsight-use-hive]
* [Verwenden Sie Schwein mit HDInsight][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
