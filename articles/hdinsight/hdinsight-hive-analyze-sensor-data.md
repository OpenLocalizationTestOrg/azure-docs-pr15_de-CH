<properties
    pageTitle="Analysieren von Daten mit Struktur und Hadoop | Microsoft Azure"
    description="Erfahren Sie, wie Daten mit HDInsight (Hadoop) Konsole Abfrage Struktur analysieren und Darstellen der Daten in Microsoft Excel mit PowerView."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Analysieren Sie Hadoop in HDInsight Konsole Abfrage Struktur mit Daten

Erfahren Sie, wie Daten mit HDInsight (Hadoop) Konsole Abfrage Struktur analysieren und Visualisieren von Daten in Microsoft Excel mithilfe von Power View.

> [AZURE.NOTE] Die Schritte in diesem Dokument funktionieren nur mit Windows-basierten HDInsight.

In diesem Beispiel verwenden Sie Struktur, Verlaufsdaten von Heizung, Lüftung und Klimaanlagen (HVAC) erzeugten Prozess um Systeme zu identifizieren, die nicht zu einem Satz zuverlässig Temperatur. Erfahren Sie, wie Sie:

- Erstellen von Struktur Tabellen zum Abfragen von Daten in durch Trennzeichen getrennte Wertdatei (CSV) Dateien gespeichert.
- Erstellen Sie Struktur Abfragen zum Analysieren der Daten.
- Verwenden Sie Microsoft Excel, Verbindung mit HDInsight (Verbindung (ODBC) zum Abrufen der analysierten Daten verwenden.
- Verwenden Sie Power View, der Daten.

![Ein Diagramm der Lösungsarchitektur](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Erforderliche Komponenten

* Ein Cluster HDInsight (Hadoop): Weitere Informationen zum Erstellen eines Clusters [Bereitstellung Hadoop Cluster in HDInsight](hdinsight-provision-clusters.md) .

* Microsoft Excel 2013

    > [AZURE.NOTE] Microsoft Excel wird für datenvisualisierung mit [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US)verwendet.

* [Struktur der Microsoft ODBC-Treiber](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>Zum Ausführen des Beispiels

1. Navigieren Sie in Ihrem Webbrowser zur folgenden URL. Ersetzen Sie `<clustername>` mit dem Namen HDInsight Cluster.

        https://<clustername>.azurehdinsight.net

    Bei Aufforderung mit Administrator-Benutzernamen und dasselbe Kennwort wie bei der Bereitstellung dieses Clusters authentifizieren.

2. Öffnet Webseite klicken Sie auf der Registerkarte **Getting Started Katalog** und klicken Sie im Beispiel **Datenanalyse** Kategorie **mit Beispieldaten** .

    ![Erste Schritte Galeriebild](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Anweisungen Sie auf der Webseite im Beispiel beendet.
