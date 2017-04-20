<properties
   pageTitle="Optimieren Sie Ihre Struktur Abfragen zur schnelleren Ausführung in HDInsight | Microsoft Azure"
   description="Informationen Sie zum Hadoop in HDInsight Struktur Abfragen optimieren."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Hadoop in HDInsight Struktur Abfragen optimieren

Hadoop-Cluster sind standardmäßig nicht für Leistung optimiert. Dieser Artikel behandelt ein Paar der häufigsten Struktur Performance-Optimierungsmethoden, die auf unsere Abfragen angewendet.

##<a name="scale-out-worker-nodes"></a>Arbeitskraft Knoten skalieren

Erhöhen der Anzahl von workerknoten in einem Cluster profitieren immer Mapper Reduzierstück parallel auszuführenden. Zweierlei Skalierung in HDInsight zu erhöhen:

- Gleichzeitig Bestimmung soll Anzahl Worker Azure-Portal, Azure PowerShell oder plattformübergreifende Befehlszeilenschnittstelle verwenden.  Weitere Informationen finden Sie unter [Bereitstellung HDInsight-Cluster](hdinsight-provision-clusters.md). Die folgende Anzeige Arbeitskraft Knotenkonfiguration Azure-Portal:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- Die Laufzeit können Sie auch einen Cluster skalieren, ohne eine. Dies ist unten dargestellt.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Weitere Informationen zu den anderen virtuellen Computern HDInsight unterstützt finden Sie unter [HDInsight Preisgestaltung](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Tez aktivieren

[Apache Tez](http://hortonworks.com/hadoop/tez/) ist eine alternative Ausführungsmodul MapReduce-Modul:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez ist schneller, weil:

- Gesteuerte azyklische Graph (so) als Druckauftrag im Modul MapReduce führen, die ausgedrückt wird so erfordert jeder Mapper bestimmte Reduzierstück folgen. Dadurch werden mehrere MapReduce Aufträge für jede Abfrage Struktur eröffnet. Tez hat keine solche Einschränkung und komplexe so als ein Auftrag Auftrag starten Verwaltungsaufwand minimiert verarbeiten kann.
- **Vermeidet unnötigen schreibt** Durch mehrere Aufträge für dieselbe Struktur Abfrage MapReduce-Engine ein wird die Ausgabe eines Auftrags, bietet für Daten geschrieben. Da Tez minimiert die Anzahl der Aufträge für jede Abfrage Struktur kann unnötige schreiben zu vermeiden.
- **Minimiert Start verzögert** Tez kann besser durch Reduzierung der Mapper zu starten und gleichzeitig Optimierung in startverzögerung minimieren.
- **Container verwendet** Mögliche Tez ist immer wiederverwenden Container, um sicherzustellen, dass Wartezeit durch Container gestartet wird.
- **Kontinuierliche Optimierungstechniken** Traditionell wurde während der Kompilierungsphase Optimierung durchgeführt. Jedoch weitere über Eingaben Informationen zulassen, die für eine bessere Optimierung während der Laufzeit Tez verwendet kontinuierliche Optimierungstechniken, die den Plan in der Laufzeit Phase optimieren können.

Weitere Informationen zu diesen Konzepten finden Sie [hier](http://hortonworks.com/hadoop/tez/)

Sie können Fragen Struktur Tez vorangestellt Abfrage mit der folgenden Einstellung aktiviert:

    set hive.execution.engine=tez;

Für Windows-basierte HDInsight-Cluster muss Tez Bestimmung gleichzeitig aktiviert sein. Nachfolgend ein Beispiel Azure PowerShell-Skript für die Bereitstellung eines Clusters Hadoop mit Tez aktiviert:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Linux-basierte HDInsight Cluster haben Tez standardmäßig aktiviert.
    

## <a name="hive-partitioning"></a>Struktur-Partitionierung

E/a-Vorgang ist der wichtigsten Leistungsengpass Struktur Abfragen ausführen. Die Leistung kann verbessert werden, wenn der Betrag Daten gelesen werden kann. Standardmäßig Scannen Struktur Abfragen Struktur Tabellen. Dies ist für Abfragen wie Tabellenscans jedoch diese unnötigen Aufwand für Abfragen, die nur eine kleine Datenmenge (z. B. Abfragen mit Filtern) überprüfen müssen, erstellt. Struktur Partitionierung ermöglicht Struktur Abfragen nur die erforderliche Datenmenge im Hive-Tabellen zugreifen.

Struktur Partitionierung erfolgt reorganisiert die unformatierten Daten in neue Verzeichnisse mit jedes mit einem eigenen Verzeichnis -, in dem der Benutzer die Partition definiert ist. Das folgende Diagramm zeigt Partitionieren einer Tabelle Struktur nach der Spalte *Jahr*. Jährlich wird ein neues Verzeichnis erstellt.

![Partitionierung][image-hdi-optimize-hive-partitioning_1]

Einige Aspekte der Partitionierung:

- **Führen Sie nicht unter Partition** - Partitionierung Spalten nur Werte kann sehr wenige Partitionen. Z. B. werden Geschlecht partitionieren nur zwei Partitionen (männlich und weiblich) erstellt werden, damit nur die Latenz maximal die Hälfte.

- **Über keine partition** - führt andererseits Erstellen einer Partition für eine Spalte mit einem eindeutigen Wert (z. B. Userid) auf mehrere Partitionen viel Druck auf Namenode Cluster verursacht, wie es viele Verzeichnisse behandeln.

- **Daten neigen** - treffen Sie Ihre Partitionierungsschlüssel sind alle Partitionen gleicher Größe. Ein Beispiel ist das Partitionieren *Zustand* kann die Anzahl der Datensätze unter Kalifornien fast 30 x, Vermont aufgrund der Bevölkerung.

Verwenden Sie zum Erstellen einer Partitionstabelle der *Partitionierten* by:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Nachdem die partitionierte Tabelle erstellt wurde, können Sie entweder statische Partitionierung oder dynamische Partitionierung erstellen.

- **Statische Partitionierung** bedeutet, dass Sie bereits Sharding Daten in die entsprechenden Verzeichnisse und du Struktur Partitionen manuell basierend auf den Speicherort kannst. Der folgende Codeausschnitt zeigt.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dynamische Partitionierung** bedeutet, dass Struktur Partitionen automatisch erstellt. Da wir bereits in der Stagingtabelle Partitionierung Tabelle erstellt haben, müssen wir lediglich Daten in der partitionierten Tabelle wie folgt eingefügt:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Weitere Informationen finden Sie unter [Partitionierten Tabellen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

##<a name="use-the-orcfile-format"></a>Verwenden Sie das Format ORCFile

Struktur unterstützt verschiedene Dateiformate. Zum Beispiel:

- **Text**: Dies ist das Standardformat und funktioniert mit den meisten Szenarien
- **Avro**: eignet sich besonders für Szenarien
- **ORK-Parkett**: geeignet für Leistung

ORK (Zeile Einspaltig optimiert) Format ist eine sehr effiziente Möglichkeit Registrierungsstruktur gespeichert. Im Vergleich zu anderen Formaten, hat ORK folgende Vorteile:

- Unterstützung für komplexe Typen mit DateTime und komplexe und semistrukturierten
- bis zu 70 % ige Komprimierung
- Indizes jeweils 10.000 Zeilen ermöglicht Zeilen überspringen
- ein Rückgang bei der Ausführung zur Laufzeit

Um ORK Format zu aktivieren, erstellen Sie zuerst eine Tabelle mit der Klausel *gespeichert als ORK*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Legen Sie danach den Daten der ORK-Tabelle in der Stagingtabelle. Zum Beispiel:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Erfahren Sie mehr über das ORK-Format [hier](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vektorisierung

Vektorisierung kann Struktur eine Stapelverarbeitung 1024 Zeilen anstatt eine Zeile auf einmal verarbeitet. Dies bedeutet, dass einfache Vorgänge schneller durchgeführt werden, da weniger interner Code ausgeführt werden muss.

Zum Aktivieren Präfix Vektorisierung Hive-Abfrage mit der folgenden Einstellung:

    set hive.vectorized.execution.enabled = true;

Weitere Informationen finden Sie unter [Vectorized Ausführung](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Andere Methoden zur Optimierung

Es gibt weitere Methoden zur Optimierung, die Sie, beispielsweise berücksichtigen können:

- **Struktur Buckets:** eine Technik, cluster oder Segmentierung großer Datensätze Abfrageperformance optimieren.
- **Optimierung beitreten:** Optimierung der Struktur der Ausführung der Abfrage zu Effizienz Joins müssen Benutzer Hinweise planen. Weitere Informationen finden Sie in der [Join-Optimierung](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
- **Reduzierstück erhöhen**

##<a id="nextsteps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere allgemeine Struktur Optimierung Abfragemethoden. Weitere finden Sie in folgenden Artikeln:

- [Apache-Struktur in HDInsight verwenden](hdinsight-use-hive.md)
- [Analysieren Sie Verzögerung Daten mit Struktur in HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Analysieren Sie Twitter-Daten mithilfe von Struktur in HDInsight](hdinsight-analyze-twitter-data.md)
- [Analysieren Sie Hadoop in HDInsight Konsole Abfrage Struktur mit Daten](hdinsight-hive-analyze-sensor-data.md)
- [Mit der Struktur mit HDInsight Protokolle von Websites analysieren](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
