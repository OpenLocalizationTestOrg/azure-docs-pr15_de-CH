<properties
    pageTitle="Daten in Tabellen Azure HDInsight Struktur | Microsoft Azure"
    description="Sie Samplingdaten in Azure HDInsight (Hadopop) Struktur Tabellen"
    services="machine-learning,hdinsight"
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
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Beispieldaten in Tabellen Azure HDInsight Struktur

In diesem Artikel beschreiben wir unten-Daten in Azure HDInsight Struktur Tabellen Struktur Abfragen Sample. Drei allgemein verwendeten Probenahmeverfahren und Fragestellungen:

* Einheitliche Stichproben
* Stichproben von Gruppen
* Geschichtete Stichprobe

**Warum Beispieldaten?**
Dataset zu analysieren groß ist, ist es in der Regel sollten Sie Daten auf eine kleinere aber repräsentativ und überschaubarer Größe reduzieren Stichproben. Dies erleichtert die Daten verstehen, Untersuchung und Featureentwicklung. Seine Rolle im Team von wissenschaftlichen Daten soll schnell Prototypen Datenverarbeitung und maschinelles lernen Modelle.

Das **Menü** unten Links zu Themen, die beschreiben, wie Sie Daten aus verschiedenen Speicherausfällen Beispiel.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Diese Aufgabe Sampling ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Struktur Abfragen einreichen
Hive-Abfragen können in der Befehlszeile Hadoop Konsole auf dem Head-Knoten des Clusters Hadoop gesendet werden. Dazu melden Sie sich bei dem Head-Knoten des Clusters Hadoop Hadoop Befehlszeilenkonsole öffnen und die Struktur Anfragen aus. Hinweise Hive Abfrage in der Befehlszeile Hadoop-Konsole finden Sie unter [Struktur Abfragen absenden](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Einheitliche Stichproben
Einheitliche Stichproben bedeutet, dass jede Zeile im DataSet Chancengleichheit von aufgenommen wird. Dies kann durch ein zusätzliches Feld rand() das Dataset in die innere Abfrage "auswählen" und in der äußeren Abfrage "auswählen" die Bedingung auf zufällige implementiert werden.

Hier ist eine Beispielabfrage:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Hier `<sample rate, 0-1>` gibt den Anteil der Datensätze, die die Benutzer aufnehmen möchten.

## <a name="group"></a>Stichproben von Gruppen

Beim Sampling Kategoriedaten möchten Sie einschließen oder Ausschließen aller Instanzen einen bestimmten Wert eine kategoriale Variable. Dies ist gemeint Stichprobenkontrollen"Gruppe".
Beispielsweise haben eine kategoriale Variable "Zustand" die Werte, NY, MA, CA, NJ, PA usw., Sie möchten Datensätze dieselbe immer, ob sie oder nicht abgetastet.

Hier ist eine Beispielabfrage, Proben von Gruppe:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Geschichtete Stichprobe

Stichproben ist in Bezug auf eine kategoriale Variable geschichtet, wenn die Proben dürfen, kategorisierten in demselben Verhältnis in übergeordneten Bevölkerung sind die Proben entnommen wurden. Das gleiche Beispiel wie oben Angenommen Sie, Ihre Daten unter Populationen von Mitgliedstaaten, sagen Sie NJ 100 Messwerte, NY hat 60 Messwerte, und WA hat 300 Messwerte. Bei Angabe bei geschichtete Stichprobe 0,5 sollte die Probe ca. 50, 30 und 150 Messwerte NJ, NY und WA bzw. müssen.

Hier ist eine Beispielabfrage:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Informationen zu erweiterten Struktur verfügbaren Probenahmeverfahren finden Sie unter [LanguageManual Sampling](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
