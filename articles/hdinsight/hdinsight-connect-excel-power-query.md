<properties
    pageTitle="Verbinden Sie Excel mit Hadoop Power Abfrage | Microsoft Azure"
    description="Informationen Sie zum Nutzen der Business Intelligence-Komponenten und Power Query für Excel Datenzugriff in Hadoop auf HDInsight gespeichert."
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


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Hadoop Power Abfrage mit herstellen Sie Excel

Ein Hauptmerkmal der Microsoft big-Lösung ist die Integration von Microsoft Business Intelligence (BI) Komponenten mit Hadoop in Azure HDInsight. Primäre wird dieser Integration ist die Möglichkeit zu Excel Azure Storage-Konto verbinden, Hadoop Cluster Abfrage macht Microsoft Excel-add-Ins mit zugeordneten Daten enthält. Dieser Artikel führt Sie durch das Einrichten und Verwenden von Power Abfrage abgefragt werden, die eine verwaltete mit HDInsight Hadoop Cluster zugeordnet.

> [AZURE.NOTE] Während der Schritte in diesem Artikel mit einem Linux- oder Windows-basierten HDInsight verwendet werden können, muss Windows Client-Arbeitsstation.

### <a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein HDInsight-Cluster**. Konfigurieren einer finden Sie unter [Erste Schritte mit Azure HDInsight][hdinsight-get-started].
- **Eine Arbeitsstation** , die Windows 7, Windows Server 2008 R2 oder ein noch aktuelleres Betriebssystem ausgeführt wird.
- **Office 2013 Professional Plus, Office 365 ProPlus, Excel 2013 eigenständigen oder Office 2010 Professional Plus**.


## <a name="install-power-query"></a>Power Query installieren

Power Abfrage kann verwendet werden, um Daten aus einer Vielzahl von Quellen in Microsoft Excel importieren, BI-Tools wie PowerPivot und Power View eingeschaltet werden kann. Insbesondere importieren Power Query Daten, die Ausgabe wurde oder von einem auf einem Cluster HDInsight Hadoop Auftrag generiert wurde.

Microsoft macht Query für Excel aus dem [Microsoft Download Center] herunterladen[ powerquery-download] und installieren Sie es.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight Daten in Excel importieren

Power Query-add-in für Excel vereinfacht HDInsight Cluster in Excel eingelesen, BI-Tools wie PowerPivot und Karte verwendet werden können, um zu überprüfen, analysieren und darstellen.

**Zum Importieren von Daten aus einem Cluster HDInsight**

1. Öffnen Sie Excel.

2. Erstellen Sie eine neue leere Arbeitsmappe.

3. **Power Query** im Menü und klicken Sie dann auf **Von Microsoft Azure HDInsight** **Von Azure**auf.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Hinweis:** **Power Query** im Menü nicht angezeigt wird, wechseln Sie zur **Datei** > **Optionen** > **Add-Ins**und wählen **COM-Add-ins** aus dem Dropdownfeld **Verwalten** am unteren Rand der Seite. Wählen Sie die Schaltfläche **Gehe zu...** und stellen Sie sicher, dass das für die Stromversorgung Excel-add-in aktiviert wurde.

    **Hinweis:** Power Query können Sie Daten aus bietet auf **Aus anderen Quellen**importieren.

3. **Kontoname**geben mit Ihrem Konto Azure BLOB-Speicher und klicken Sie dann auf **OK**. Dies ist das [Standardkonto Speicher](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) oder ein verknüpftes Speicherkonto.  Das Format ist *https://<StorageAccountName>.blob.core.windows.net/*.

4. **Kontoschlüssel**den Schlüssel für das Speicherkonto Blob und klicken Sie dann auf **Speichern**. (Sie müssen dazu nur beim ersten Zugriff auf diesen Speicher.)

5. Doppelklicken Sie im **Navigator** auf der linken Seite des Abfrage-Editors auf den Containernamen BLOB-Speicher. Der Containername ist standardmäßig denselben Namen wie den Namen des Clusters.

6. Suchen Sie in der Spalte **Name** (der Pfad ist **... **HiveSampleData.txt** / Struktur/warehouse/Hivesampletable/**), und klicken Sie dann auf **binäre** links HiveSampleData.txt. HiveSampleData.txt enthält der Cluster. Optional können Sie eine eigene Datei.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Wenn Sie möchten, können Sie Spaltennamen umbenennen. Wenn Sie bereit sind, klicken Sie auf **Schließen und Laden**.  Die Daten wurde zu Ihrer Arbeitsmappe geladen:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, Power-Abfrage zum Abrufen von Daten aus HDInsight in Excel verwenden. Sie können auch Daten aus HDInsight in Azure SQL-Datenbank abrufen. Es kann auch zum Hochladen von Daten in HDInsight. Weitere finden Sie in folgenden Artikeln:

* [Schließen Sie Excel an HDInsight mit der Struktur der Microsoft ODBC-Treiber][hdinsight-ODBC]
* [Upload von Daten auf HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
