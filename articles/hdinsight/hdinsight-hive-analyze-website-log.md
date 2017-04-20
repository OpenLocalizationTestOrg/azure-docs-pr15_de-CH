<properties 
    pageTitle="Struktur für Website-Protokollanalyse Hadoop verwenden | Microsoft Azure" 
    description="Erfahren Sie, wie HDInsight Struktur mit Website-Protokolle zu analysieren. Sie verwenden eine Log-Datei als Eingabe in eine HDInsight-Tabelle, und HiveQL verwenden, um die Daten abzufragen." 
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
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Mit der Struktur mit HDInsight Protokolle von Websites analysieren

Erfahren Sie, wie HDInsight HiveQL mit Protokolle von einer Website zu analysieren. Websiteanalyse dienen, segment Zielgruppe aufgrund von ähnlichen Aktivitäten Sitebesucher nach Demographie kategorisieren und zu den Inhalt sie Anzeigen der Websites sie stammen, und.

In diesem Beispiel verwenden Sie einen HDInsight-Cluster Website Protokolldateien Einblick in die Häufigkeit der Besuche auf der Website von externen Websites täglich analysieren. Sie werden auch eine Website Fehler generieren, die der Benutzer. Erfahren Sie, wie Sie:

- Verbinden Sie mit einer Azure Blob-Speicher, Website-Protokolldateien enthält.
- Erstellen Sie Struktur Tabellen abgefragt werden diese Protokolle.
- Erstellen Sie Struktur Abfragen zum Analysieren der Daten.
- Verwenden Sie Microsoft Excel, Verbindung mit HDInsight (mit Verbindung (ODBC) zum Abrufen der analysierten Daten.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Erforderliche Komponenten

- Sie müssen einen Cluster Hadoop auf Azure HDInsight bereitgestellt. Informationen finden Sie unter [Bereitstellung HDInsight Cluster][hdinsight-provision]. 
- Sie benötigen Microsoft Excel 2013 oder Excel 2010.
- Sie benötigen [Microsoft Struktur ODBC-Treiber](http://www.microsoft.com/download/details.aspx?id=40886) Struktur in Excel eingelesen.


##<a name="to-run-the-sample"></a>Zum Ausführen des Beispiels

1. [Azure-Portal](https://portal.azure.com/)aus Startmenü (Wenn Sie den Cluster fixiert) klicken Sie auf den Cluster auf dem das Beispiel ausgeführt werden soll.

2. Blatt Cluster unter **Quicklinks**klicken Sie auf **Cluster-Dashboard**und dann Blatt **Cluster Dashboard** auf **HDInsight Cluster Dashboard**. Alternativ können Sie das Dashboard direkt öffnen, mithilfe der folgenden URL:

        https://<clustername>.azurehdinsight.net
    
    Wenn Sie aufgefordert werden, mit den Administrator-Benutzernamen und das Kennwort beim Bereitstellen des Clusters verwendeten authentifizieren.
  
2. Klicken Sie von der Webseite, die geöffnet wird auf der Registerkarte **Getting Started Katalog** und klicken Sie **Websiteanalyse** Beispiel Kategorie **mit Beispieldaten** .

3. Anweisungen Sie auf der Webseite im Beispiel beendet.

##<a name="next-steps"></a>Nächste Schritte
Im folgende Beispiel versuchen: [Analysieren von Daten mit Struktur mit HDInsight](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
