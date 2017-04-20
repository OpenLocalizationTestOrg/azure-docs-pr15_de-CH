<properties
   pageTitle="Andere Azure Services integrieren Datenspeicher See | Microsoft Azure"
   description="Integration von Datenspeicher-See mit anderen Azure verstehen"
   documentationCenter=""
   services="data-lake-store"
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

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Andere Azure Services integrieren See Datenspeicher

Azure See Datenspeicher kann zusammen mit anderen Azure Services ermöglichen einen größeren Bereich von Szenarios verwendet werden. Der folgende Artikel listet die Dienste, denen Datenspeicher See integriert werden können.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Mit See Datenspeicher mit Azure HDInsight

Sie können einen [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) Cluster bereitstellen, der See Datenspeicher als bietet konformen Speicher verwendet. Diese Version können Sie für Hadoop und Storm-Cluster unter Windows und Linux, See Datenspeicher als zusätzlicher Speicher verwenden. Solche Cluster verwenden noch Azure-Speicher (WASB) als Standardspeicher. Jedoch für HBase-Cluster unter Windows und Linux können See Datenspeicher der Standardspeicher oder zusätzlichen Speicher Sie.

Informationen HDInsight-Cluster mit See Datenspeicher Bereitstellung finden Sie unter:

* [Bereitstellen eines HDInsight Clusters mit See Datenspeicher Azure-Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Bereitstellen eines HDInsight Clusters mit See Datenspeicher Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Ziehen Sie Videos?** Hyperlinks mit HDInsight-Cluster See Datenspeicher mit Videos ansehen.

* [Erstellen Sie einen HDInsight-Cluster Zugriff auf See Datenspeicher](https://mix.office.com/watch/l93xri2yhtp2)
* Wenn Cluster, [Zugriff auf Daten in dem Datenspeicher Struktur und Schwein Skripts eingerichtet ist](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Mit See Datenspeicher mit Azure Data Lake Analytics

[Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) können Sie Big Data Cloud Ebene arbeiten. Dynamische Ressourcen Rechtsvorschriften und ermöglicht Analytics Terabyte oder sogar Exabyte, die Anzahl der unterstützten Datenquellen, darunter See Datenspeicher gespeichert werden können. Datenanalyse See ist speziell mit Azure See Datenspeicher - mit der höchsten Leistung, Durchsatz und zur Parallelisierung Datenverlustvorfalls Arbeitslasten optimiert.

Informationen See Datenspeicher See Datenanalyse mit finden Sie unter [Erste Schritte mit See Datenanalyse mit dem Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

**Ziehen Sie Videos?** Hyperlinks mit HDInsight-Cluster See Datenspeicher mit Videos ansehen.

* [Azure Data Lake Analytics Azure See Datenspeicher herstellen](https://mix.office.com/watch/qwji0dc9rx9k)
* [Zugriff auf Azure See Datenspeicher über See Datenanalyse](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Mit See Datenspeicher mit Azure Data Factory

[Azure Data Factory](https://azure.microsoft.com/services/data-factory/) können Sie Daten von Azure-Tabellen, Azure SQL-Datenbank Azure SQL Data Warehouse, Azure Storage Blobs und lokalen Datenbanken aufnehmen. Einen Bürger erster Klasse im Azure Ökosystem sondern, kann Azure Data Factory orchestrieren Erfassung von Daten aus dieser Quelle in Azure See Datenspeicher verwendet werden.

Informationen See Datenspeicher Azure Data Factory mit finden Sie unter [Verschieben von Daten in und aus dem Datenspeicher Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

**Videos wieder!** Siehe [Daten Orchestrierung Azure Data Factory für Azure See Datenspeicher](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Daten von Azure Storage Blobs in Lake Datenspeicher

Azure See Datenspeicher bietet ein Befehlszeilentool AdlCopy, das Kopieren von Daten von Azure BLOB-Speicher in einen Datenspeicher See Konto ermöglicht. Weitere Informationen finden Sie unter [Kopieren von Daten aus Azure Storage Blobs See Datenspeicher](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Kopieren von Daten zwischen Azure SQL-Datenbank und Datenspeicher See

Sie können Apache Sqoop importieren und Exportieren von Daten zwischen Azure SQL-Datenbank und See Datenspeicher. Weitere Informationen finden Sie unter [Kopieren von Daten zwischen dem Datenspeicher und Azure SQL-Datenbank mit Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

**In diesem Video** [Apache Sqoop zum Verschieben von Daten zwischen relationalen und Azure See Datenspeicher verwenden](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Mit See Datenspeicher mit Stream Analytics

See Datenspeicher können als eines der Ergebnisse Sie mit Azure Stream Analytics übertragene Daten speichern. Weitere Informationen finden Sie unter [Streaming von Daten von Azure Storage Blob in dem Datenspeicher Azure Stream Analytics verwenden](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Mit See Datenspeicher mit Power BI

Power BI können Sie Daten von einem Datenspeicher See Konto analysieren und Darstellen der Daten. Weitere Informationen finden Sie unter [analysieren Daten im Datenspeicher See Power BI verwenden](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Mit See Datenspeicher mit Data Catalog

Sie können Daten aus See Datenspeicher in Azure Data Catalog Daten unternehmensweit auffindbar registrieren. Weitere Informationen finden Sie in der [Daten aus dem Datenspeicher in Azure Data Catalog](data-lake-store-with-data-catalog.md).


## <a name="see-also"></a>Siehe auch

- [Übersicht über Azure See Datenspeicher](data-lake-store-overview.md)
- [Erste Schritte mit See Datenspeicher Portal](data-lake-store-get-started-portal.md)
- [Erste Schritte mit See Datenspeicher PowerShell](data-lake-store-get-started-powershell.md)  
