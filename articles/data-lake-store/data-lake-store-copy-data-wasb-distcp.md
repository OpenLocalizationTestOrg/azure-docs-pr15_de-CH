<properties
   pageTitle="Daten zu und von WASB in Lake Datenspeicher Distcp | Microsoft Azure"
   description="Verwenden Sie Distcp Tool, um Daten zu und von Azure Storage Blobs See Datenspeicher kopieren"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Verwenden Sie Distcp Kopieren von Daten zwischen Azure Storage Blobs und See Datenspeicher

> [AZURE.SELECTOR]
- [DistCp verwenden](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy verwenden](data-lake-store-copy-data-azure-storage-blob.md)


Nachdem Sie einen HDInsight-Cluster Zugriff auf ein Konto See Datenspeicher erstellt haben, können Hadoop Ökosystem Tools wie Distcp Sie Daten **in und aus** einem HDInsight Cluster (WASB) Rechnung See Datenspeicher kopieren. Dieser Artikel enthält Hinweise dazu.

##<a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).
- Für Datenspeicher See public Preview **Azure Abonnement aktivieren** . [Siehe.](data-lake-store-get-started-portal.md#signup)
- **Azure HDInsight Cluster** Zugriff auf ein Konto See Datenspeicher. Siehe [Erstellen eines Clusters HDInsight mit dem Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md). Stellen Sie sicher, dass Remotedesktop für den Cluster zu aktivieren.

## <a name="do-you-learn-fast-with-videos"></a>Lernen Sie mit Videos?

[In diesem Video](https://mix.office.com/watch/1liuojvdx6sie) zum Kopieren von Daten zwischen Azure Storage Blobs und Datenspeicher Lake DistCp.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Verwenden von Remote Desktop (Windows Cluster) oder SSH (Linux Cluster) Distcp

Ein HDInsight-Cluster enthält das Distcp-Dienstprogramm, das Kopieren von Daten aus verschiedenen Quellen in einen HDInsight-Cluster verwendet werden kann. Wenn HDInsight Cluster See Datenspeicher als zusätzlicher Speicher Verwendung konfiguriert haben, kann das Dienstprogramm Distcp verwendeten Out-of-the-Box Kopieren von Daten und von einem Datenspeicher See-Konto. In diesem Abschnitt betrachten wir die Verwendung der Distcp ein.

1. Haben Sie einen Windows-Cluster hat remote in einen HDInsight-Cluster, die Zugriff auf ein Konto See Datenspeicher. Informationen finden Sie unter [Verbinden zu Clustern mit RDP](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). Öffnen Sie vom Desktop Cluster Hadoop-Befehlszeile.

    Haben Sie einen Linux-Cluster SSH Verbindung zum Cluster verwenden. [Mit einem Linux-basierten HDInsight Cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)anzeigen Führen Sie die Befehle aus der SSH.

3. Überprüfen Sie, ob Sie Azure Storage Blobs (WASB) zugreifen können. Führen Sie den folgenden Befehl ein:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Damit soll ein Inhaltsverzeichnis im Speicher-Blob.

4. Auf ähnliche Weise überprüfen Sie, ob Cluster See Datenspeicher-Konto zugreifen können. Führen Sie den folgenden Befehl ein:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Eine Liste der Dateien und Ordner im Datenspeicher See Konto beinhalten.

5. Mit Distcp können Daten aus WASB auf See Datenspeicher Konto kopieren.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Dadurch wird der Inhalt des **Ordners/Beispiel** Daten/Gutenberg/WASB **/myfolder** im Datenspeicher See Konto kopiert.

6. In ähnlicher Weise, Distcp mit Daten in WASB von Datenspeicher See Konto kopieren.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Dies wird der Inhalt des **/myfolder** im Datenspeicher See **Konto/Beispiel** Daten/Gutenberg/Ordner WASB kopiert.

## <a name="see-also"></a>Siehe auch

- [Daten von Azure Storage Blobs See Datenspeicher](data-lake-store-copy-data-azure-storage-blob.md)
- [Sichere Daten im Datenspeicher See](data-lake-store-secure-data.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
