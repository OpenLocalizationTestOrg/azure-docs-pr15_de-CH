<properties
    pageTitle="Daten in Tabellen mit Struktur Abfragen Struktur | Microsoft Azure"
    description="Struktur Tabellen Struktur Abfragen untersuchen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Daten Sie in Tabellen mit Struktur Abfragen Struktur

Dieses Dokument enthält Skripts, mit denen Daten in Tabellen in einem Cluster HDInsight Hadoop Struktur Struktur.

Das folgende **Menü** enthält Links zu Themen über die Verwendung von Tools zu Daten aus verschiedenen Speicher.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten
Dieser Artikel setzt voraus:

* Ein Azure Storage-Konto erstellt. Anleitung, finden Sie unter [Azure Storage-Konto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account)
* Angepassten Hadoop Cluster mit den HDInsight-Dienst bereitgestellt. Anleitung, finden Sie unter [Anpassen von Azure HDInsight Hadoop-Cluster für erweiterte Analysen](machine-learning-data-science-customize-hadoop-cluster.md).
* Die Daten wurde Struktur Tabellen in Azure HDInsight Hadoop Cluster hochgeladen. Wenn dies nicht der Fall ist, führen Sie die Schritte [Erstellen und Laden der Daten Struktur Tabellen](machine-learning-data-science-move-hive-tables.md) Hive-Tabellen Daten zunächst hochladen.
* Remote-Zugriff auf den Cluster aktiviert. Anleitung, finden Sie unter [Zugriff die Hadoop Cluster Head Knoten](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Benötigen Sie Informationen zur Struktur Abfragen senden, finden Sie unter [Struktur Abfragen senden](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Beispielskripts Hive Abfrage für das Durchsuchen von Daten

1. Anzahl der Messwerte pro Partition abrufen `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Rufen Sie die Anzahl der Messwerte pro Tag `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Die Ebenen in eine Kategoriespalte abrufen  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Die Anzahl der Ebenen in Kombination zweier kategorische Spalten abrufen `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. Die Verteilung für numerische Spalten abrufen  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Extrahieren Sie Datensätze aus zwei Tabellen verknüpfen

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Zusätzliche Abfrage Skripts für Taxi Reise Szenarien

Beispiele für Abfragen, die spezifischen [Daten NYC Taxi](http://chriswhong.com/open-data/foil_nyc_taxi/) Szenarien werden im [Github Repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)bereitgestellt. Diese Abfragen bereits Datenschema angegeben haben und können bei ausführen.
